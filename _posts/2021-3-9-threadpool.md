---
title: "Naive c++11 implmentation of thread pool"
date: 2021-03-09T15:34:30-04:00
categories:
  - C++
tags:
  - C++
---
A naive c++11 implementation of thread pool

- [1. implementation](#1-implementation)
- [2. usage](#2-usage)

# 1. implementation
- a std::vector of std::thread represented workers.
- a std::queue of std::function represented tasks.
- the workers try to obtain and then invoke a task from the task queue by binding a std::function to a std::thread after creating the thread pool.
- use std::mutex and std::condition_variable to control the synchronization and mutex for the race resources (task queue). 

ThreadPool.hpp
```cpp
#ifndef _THREAD_POOL
#define _THREAD_POOL

#include <vector>
#include <thread>
#include <mutex>
#include <condition_variable>
#include <queue>

namespace Fang
{
    class noncopyable{
    public:
        noncopyable(noncopyable&) = delete;
        noncopyable& operator=(noncopyable&) = delete;
    protected:
        noncopyable() = default;
        ~noncopyable() = default;
    };

    class ThreadPool : noncopyable {
    public:
        explicit ThreadPool(std::size_t n);
        ~ThreadPool();
        template<typename FUNC, typename... ARGS> void insert_task(FUNC&& func, ARGS&&... args);
        void join();
    private:
        std::vector<std::thread> workers_;
        std::queue<std::function<void()>> tasks_;
        std::mutex qmutex_;
        std::condition_variable cv_;
        bool stop_;
    };
} // namespace Fang

namespace Fang
{
    ThreadPool::ThreadPool(size_t n): stop_(false) {
        for (int i=0; i<n; ++i) {
            workers_.emplace_back([this]{
                    while(true) {
                        std::function<void()> task;
                        {
                            std::unique_lock<std::mutex> lock(qmutex_);
                            cv_.wait(lock, [this]{return this->stop_ || !this->tasks_.empty();});
                            if (this->stop_ && this->tasks_.empty()) {
                                return;
                            }
                            task = std::move(this->tasks_.front());
                            this->tasks_.pop();
                        }
                        task();
                    }
                });
        }
    }

    template<typename FUNC, typename... ARGS>
    void ThreadPool::insert_task(FUNC &&func, ARGS&&... args) {
        auto f = std::bind(std::forward<FUNC>(func), std::forward<ARGS>(args)...);
        {
            std::unique_lock<std::mutex> lock(qmutex_);
            if (stop_) {
                throw std::runtime_error("insert task into a stoped threadpool.");
            }
            tasks_.emplace([f]{f();});
            cv_.notify_one();
        }
    }

    void ThreadPool::join(){
        {
            std::unique_lock<std::mutex> lock;
            stop_ = true;
        }
        cv_.notify_all();
        for (auto& w: workers_)
            w.join();
    }

    ThreadPool::~ThreadPool() {
        join();
    }
} // namespace Fang

#endif  // #ifndef _THREAD_POOL

```

# 2. usage
```cpp
#include <iostream>
#include <vector>
#include <chrono>
#include <string>
#include "ThreadPool.hpp"

void test(int val) {
    std::cout << val << std::endl;
    std::this_thread::sleep_for(std::chrono::seconds(1));
}

int main() {
    std::vector<int> vec(10);
    {
        Fang::ThreadPool pool(4);

        // example: 1) std::function. 2) no return value.
        for (int i=0; i<10; ++i) {
            pool.insert_task(test, i);
        }

        // example: 1) lambda. 2) return by reference.
        for (int i=0; i<10; ++i) {
            pool.insert_task([&vec, i](){
                vec[i] = i * 10000;
                std::this_thread::sleep_for(std::chrono::seconds(1));
                });
        }
    }
    for (auto x: vec) {
        std::cout << x << std::endl;
    }
    return 0;
}
```