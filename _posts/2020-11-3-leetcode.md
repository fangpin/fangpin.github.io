---
title: "Leetcode两月计划"
date: 2019-04-18T15:34:30-04:00
categories:
  - Algorithm
tags:
  - Leetcode
---

Leetcode两月计划

- [week 1](#week-1)
  - [Tree](#tree)
    - [94](#94)
    - [100](#100)
    - [102](#102)
    - [814](#814)
    - [112](#112)
    - [129](#129)
    - [236](#236)
    - [297](#297)
    - [508](#508)
    - [124](#124)
- [week 2](#week-2)
  - [divide and conquer](#divide-and-conquer)
    - [315](#315)
  - [list](#list)
    - [2](#2)
    - [24](#24)
    - [141](#141)
    - [23](#23)
    - [147](#147)
    - [148](#148)
    - [707](#707)
- [week 3](#week-3)
  - [binary search tree](#binary-search-tree)
    - [98](#98)
    - [700](#700)
    - [230](#230)
    - [99](#99)
    - [108](#108)
    - [501](#501)
    - [450](#450)
  - [binary search](#binary-search)
    - [35](#35)
    - [33](#33)
    - [69](#69)
    - [74](#74)
    - [875](#875)
    - [4](#4)
    - [378](#378)
    - [719](#719)
- [week 4](#week-4)
  - [two pointers](#two-pointers)
    - [11](#11)
    - [125](#125)
    - [917](#917)
    - [167](#167)
    - [977](#977)
    - [992](#992)
  - [Advanced](#advanced)
    - [208](#208)
    - [307](#307)
    - [901](#901)
    - [239](#239)
- [week 5](#week-5)
  - [Graph](#graph)
    - [133](#133)
    - [200](#200)
    - [841](#841)
    - [207](#207)
    - [399](#399)
    - [785](#785)
    - [997](#997)
    - [433](#433)
    - [684](#684)
    - [743](#743)
    - [847](#847)
- [week 6](#week-6)
  - [graph](#graph-1)
    - [332](#332)
    - [1192](#1192)
    - [959](#959)
  - [search (bfs/dfs)](#search-bfsdfs)
    - [17](#17)
    - [46](#46)
    - [22](#22)
    - [37](#37)
    - [79](#79)
    - [127](#127)
    - [542](#542)
    - [698](#698)
- [week 7](#week-7)
  - [dp](#dp)
    - [70](#70)
    - [303](#303)
    - [53](#53)
    - [62](#62)
    - [85](#85)
    - [198](#198)
    - [279](#279)
    - [139](#139)
    - [300](#300)
    - [96](#96)
    - [221](#221)
    - [213](#213)
    - [140](#140)
    - [673](#673)
    - [89](#89)
    - [10](#10)
- [week 8](#week-8)
  - [dp](#dp-1)
    - [1105](#1105)
    - [131](#131)
    - [72](#72)
    - [1139](#1139)
    - [688](#688)
    - [322](#322)
    - [813](#813)
    - [1223](#1223)
    - [312](#312)
    - [741](#741)
    - [546](#546)
    - [943](#943)
    - [712](#712)
    - [576](#576)
    - [377](#377)
    - [1220](#1220)
    - [1278](#1278)
    - [664](#664)
    - [980](#980)

# week 1
## Tree
### 94

```cpp
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> ans;
        stack<TreeNode*> st;
        if (root) {
            travleALeft(st, root);
            while (!st.empty()) {
                root = st.top();
                ans.push_back(root->val);
                st.pop();
                if (root->right)
                    travleALeft(st, root->right);
            }
        }
        return ans;
    }

    void travleALeft(stack<TreeNode*>& st, TreeNode* root)
    {
        do {
            st.push(root);
            root = root->left;
        } while (root);
    }
};
```
O(1) space solution: morris traverse。Find node x's inorder predecessorr，通过判断是否穿针引线来判定否访问过一次了。Visting it at the second time indicates that the left sub###tree has been traversed.
### 100
```cpp
class Solution {
public:
    bool isSameTree(TreeNode* p, TreeNode* q) {
        if (!p && !q) 
            return true;
        if (p && q && p->val == q->val) {
            return isSameTree(p->left, q->left) &&
                isSameTree(p->right, q->right);
        }
        else
            return false;
    }
};
```

### 102
```cpp
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> ans;
        vector<int> tmp;
        queue<TreeNode*> q;
        if (root)
            q.push(root);
        int sum = 1, outQ = 0, inQ = 0;
        while (!q.empty()) {
            TreeNode* now = q.front();
            q.pop();
            tmp.push_back(now->val);
            if (now->left) {
                q.push(now->left);
                ++inQ;
            }
            if (now->right) {
                q.push(now->right);
                ++inQ;
            }
            ++outQ;
            if (outQ == sum)
            {
                ans.push_back(tmp);
                tmp.clear();
                sum = inQ;
                outQ = inQ = 0;
            }
        }
        return ans; 
    }
};
```
### 814
```cpp
class Solution {
public:
    TreeNode* pruneTree(TreeNode* root) {
        root = change(root);
        return root; 
    }

    TreeNode* change(TreeNode* root) {
        if (!root)
            return nullptr;
        root->left = change(root->left);
        root->right = change(root->right);
        if (!root->left && !root->right && root->val == 0)
            return nullptr;
        return root;
    }
};
```

### 112

```cpp
class Solution {
    bool ok = false;
public:
    bool hasPathSum(TreeNode* root, int sum) {
        ok = false;
        if (root)
            f(root, sum, root->val);
        return ok; 
    }

    void f(TreeNode* root, const int& sum, const int& tmp) {
        if (ok)
            return;
        if (!root->left && !root->right) {
            ok = sum == tmp;
            return;
        }
        if (root->left) {
            f(root->left, sum, tmp + root->left->val);
        }
        if (root->right) {
            f(root->right, sum, tmp + root->right->val);
        }
    }
};
```

### 129

```cpp
class Solution {
    int ans = 0;
public:
    int sumNumbers(TreeNode* root) {
        if (root)
            dfs(root, root->val);
        return ans;
    }

    void dfs(TreeNode* root, int sum) {
        if (!root->left && !root->right) {
            ans += sum;
            return;
        }
        if (root->left)
            dfs(root->left, sum * 10 + root->left->val);
        if (root->right)
            dfs(root->right, sum * 10 + root->right->val);
    }
};
```

### 236

```cpp
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if (!root || root == p || root == q)
            return root;
        TreeNode* left = lowestCommonAncestor(root->left, p, q);
        TreeNode* right = lowestCommonAncestor(root->right, p, q);
        if (!left) return right;
        if (!right) return left;
        return root;
    }
};
```

### 297

```cpp
class Codec {
public:

    // Encodes a tree to a single string.
    string serialize(TreeNode* root) {
        ostringstream out;
        serialize(root, out);
        return out.str();
    }

    // Decodes your encoded data to tree.
    TreeNode* deserialize(string data) {
        istringstream in(data);
        return deserialize(in);
    }

private:
    TreeNode* deserialize(istringstream& in) {
        string str;
        in >> str;
        if (str == "#")
            return nullptr;
        TreeNode* node = new TreeNode(stoi(str));
        node->left = deserialize(in);
        node->right = deserialize(in);
        return node;
    }
    void serialize(TreeNode* root, ostringstream& out) {
        if (!root) {
            out << "# ";
            return;
        }
        out << root->val << " ";
        serialize(root->left, out);
        serialize(root->right, out);
    }
};
```

### 508

```cpp
class Solution {
    map<int, int> cnt;
public:
    vector<int> findFrequentTreeSum(TreeNode* root) {
        dfs(root);
        vector<int> ans;
        int maxn = 0;
        for (const auto& x: cnt) {
            if (x.second > maxn) {
                ans = vector<int> {x.first};
                maxn = x.second;
            } else if (x.second == maxn) {
                ans.push_back(x.first);
            }
        }
        return ans;
    }

    int dfs(TreeNode* root) {
        if (!root)
            return 0;
        int left = dfs(root->left);
        int right = dfs(root->right);
        int sum = left + right + root->val;
        cnt[sum]++;
        return sum;
    }
};
```

### 124

```cpp
class Solution {
    int ans = INT_MIN;
public:
    int maxPathSum(TreeNode* root) {
        dfs(root);
        return ans;
    }

    void dfs(TreeNode* root) {
        int sum = root->val;
        int maxSon = ###1;
        if (root->left) {
            dfs(root->left);
            maxSon = max(maxSon, root->left->val);
            sum += max(root->left->val, 0);
        }
        if (root->right) {
            dfs(root->right);
            maxSon = max(maxSon, root->right->val);
            sum += max(root->right->val, 0);
        }
        root->val += max(maxSon, 0);
        ans = max(ans, sum);
    }
}
```

# week 2
## divide and conquer
### 315

```cpp
class SegmentTree {
public:
    SegmentTree(int n) {
        tree_.resize((n << 2) + 5);
    }
    void AddValue(int pos, int root, int l, int r, int delta) {
        if (l == r) {
            tree_[root] += delta;
            return;
        }
        int m = (l + r) >> 1;
        if (pos <= m) 
            AddValue(pos, root << 1, l, m, delta);
        else
            AddValue(pos, root << 1 | 1, m + 1, r, delta);
        UpdateNode(root);
    }
    void UpdateNode(int root) {
        tree_[root] = tree_[root << 1] + tree_[root << 1 | 1];
    }
    int GetSum(int a, int b, int root, int l, int r) const {
        if (a <= l && b >= r)
            return tree_[root];
        int sum = 0;
        int m = (l + r) >> 1;
        if (a <= m)
            sum += GetSum(a, b, root << 1, l, m);
        if (b > m)
            sum += GetSum(a, b, root << 1 | 1, m + 1, r);
        return sum;
    }
private:
    vector<int> tree_;
};

class Solution {
public:
    vector<int> countSmaller(vector<int>& nums) {
        if (nums.size() == 0)
            return vector<int>();
        set<int> data(nums.begin(), nums.end());
        unordered_map<int, int> mapping;
        int rank = 0;
        for (const auto& x: data)
            mapping[x] = ++rank;
        vector<int> cnt(nums.size(), 0);
        SegmentTree tree(data.size());
        tree.AddValue(mapping[nums.back()], 1, 1, data.size(), 1);
        for (int i=int(nums.size())###2; i>=0; ######i) {
            int rank = mapping[nums[i]] ### 1;
            cnt[i] = rank ? tree.GetSum(1, rank, 1, 1, data.size()) : 0;
            tree.AddValue(rank + 1, 1, 1, data.size(), 1);
        }
        return cnt;
    }
};
```

## list
### 2

```cpp
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        int carry = 0;
        ListNode* ret = new ListNode();
        ListNode* p = l1; ListNode* q = l2; ListNode* r = ret;
        while (p && q) {
            int value = p->val + q->val + carry;
            carry = value / 10;
            r->next = new ListNode(value % 10);
            r = r->next;
            p = p->next;
            q = q->next;
        }
        p = p ? p : q;
        while (p) {
            int value = p->val + carry;
            carry = value / 10;
            r->next = new ListNode(value % 10);
            p = p->next;
            r = r->next;
        }
        
        if (carry)
            r->next = new ListNode(carry);

        return ret->next;
    }
};
```

### 24

```cpp
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        ListNode* tmpHead = new ListNode();
        tmpHead->next = head;
        ListNode* p = head, *prev = tmpHead, *q;
        while (p) {
            q = p->next;
            if (q) {
                p->next = q->next;
                q->next = p;
                if (prev) {
                    prev->next = q;
                }
            }
            prev = p;
            p = p->next;
        }

        return tmpHead->next; 
    }
};
```

### 141

```cpp
class Solution {
public:
    bool hasCycle(ListNode *head) {
        ListNode *fast = head, *slow = head;
        while(fast) {
            fast = fast->next;
            if (fast == slow)
                return true;
            if (fast) {
                fast = fast->next;
                if (fast == slow)
                    return true;
            }
            slow = slow->next;
        }
        return false;
    }
};
```

### 23

```cpp
class Solution {
public:
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        auto cmp = [](ListNode* lhs, ListNode* rhs) {
            return lhs->val > rhs->val;
        };
        priority_queue<ListNode*, vector<ListNode*>, decltype(cmp)> q(cmp);
        for (ListNode* it : lists) 
            if (it)
                q.push(it);
        ListNode* vHead = new ListNode(), *p = vHead;
        while (!q.empty()) {
            auto now = q.top();
            p->next = now;
            p = p->next;
            q.pop();
            if (now->next)
                q.push({now->next});
        }
        return vHead->next;
    }
};
```

### 147

```cpp
class Solution {
public:
    ListNode* insertionSortList(ListNode* head) {
        ListNode* vHead = new ListNode(), *tail = vHead, *p = head;
        vHead->next = head;
        while (p) {
            tail->next = p->next;
            ListNode* q, *innerPrev;
            for (q = vHead->next, innerPrev = vHead; q != tail->next; q = q->next, innerPrev = innerPrev->next) {
                if (p->val <= q->val) {
                    p->next = q;
                    innerPrev->next = p;
                    break;
                }
            }
            if (q == tail->next) {
                p->next = tail->next;
                tail->next = p;
                tail = p;
                p = p->next;
            }
            else {
                p = tail->next;
            }
        }
        return vHead->next;
    }
```

### 148

```cpp
// ugly but passed anyway.
class Solution {
public:
    ListNode* sortList(ListNode* head) {
        ListNode* lHead, *rHead;
        quickSort(head, &lHead, &rHead);
        return lHead;
    }

    ListNode* take10(ListNode* head) {
        if (head->next) {
            ListNode *p = head, *prev = nullptr;
            for (int i=0; i<10 && p; ++i, prev=p, p=p->next);
            if (p) 
            {
                prev->next = p->next;
                p->next = head;
                head = p;
            }
        }
        return head;
    }

    void quickSort(ListNode* head, ListNode** lHead, ListNode** rHead) {
        if (head != nullptr) {
            head = take10(head);
            shared_ptr<ListNode> left = make_shared<ListNode>(), right = make_shared<ListNode>();
            ListNode* lTail = left.get(), *rTail = right.get();
            for (auto p = head->next; p != nullptr; p = p->next) {
                if (p->val < head->val) {
                    lTail->next = p;
                    lTail = p;
                } else {
                    rTail->next = p;
                    rTail = p;
                }
            }
            lTail->next = rTail->next = nullptr;
            ListNode* leftLHead, *leftRHead, *rightLHead, *rightRHead;
            quickSort(left->next, &leftLHead, &leftRHead);
            quickSort(right->next, &rightLHead, &rightRHead);
            if (leftRHead) {
                leftRHead->next = head;
            }
            head->next = rightLHead;
            *lHead = leftLHead ? leftLHead : head;
            *rHead = rightRHead ? rightRHead : head;
        } else {
            *lHead = *rHead = nullptr;
        }
    }
};
```

### 707

```cpp
class MyLinkedList {
private:
    struct Node {
        int val;
        Node* prev;
        Node* next;
        Node(int val_=0, Node* prev_=nullptr, Node* next_=nullptr) : val(val_), prev(prev_), next(next_) { }
    } head_;

    Node* head;
    int len;

    void insert(Node *prev, Node *curr, Node *next) {
        next->prev = curr;
        curr->next = next;
        prev->next = curr;
        curr->prev = prev;
    }

    Node* getAtIndex(int index) {
        auto p = head->next;
        for (int i=0; i<index; ++i, p=p->next) ;
        return p;
    }

public:
    /** Initialize your data structure here. */
    MyLinkedList() {
        head = &head_;
        head->next = head;
        head->prev = head;
        len = 0;
    }
    
    /** Get the value of the index###th node in the linked list. If the index is invalid, return ###1. */
    int get(int index) {
        if (index < len && index >= 0) {
            auto p = getAtIndex(index);
            return p->val;
        }
        return ###1;
    }
    
    /** Add a node of value val before the first element of the linked list. After the insertion, the new node will be the first node of the linked list. */
    void addAtHead(int val) {
        Node *curr = new Node(val);
        insert(head, curr, head->next);
        ++len;
    }
    
    /** Append a node of value val to the last element of the linked list. */
    void addAtTail(int val) {
        Node *curr = new Node(val);
        insert(head->prev, curr, head);
        ++len;
    }
    
    /** Add a node of value val before the index###th node in the linked list. If index equals to the length of linked list, the node will be appended to the end of linked list. If index is greater than the length, the node will not be inserted. */
    void addAtIndex(int index, int val) {
        if (index <= len && index >= 0) {
            Node* curr = new Node(val);
            Node* dest = getAtIndex(index);
            insert(dest->prev, curr, dest);
            ++len;
        }
    }
    
    /** Delete the index###th node in the linked list, if the index is valid. */
    void deleteAtIndex(int index) {
        if (index < len && index >=0) {
            Node* dest = getAtIndex(index);
            dest->next->prev = dest->prev;
            dest->prev->next = dest->next;
            delete dest;
            ######len;
        }
    }
};
```

# week 3
## binary search tree
### 98

```cpp
class Solution {
    bool ok = true;
    long long prev;
public:
    bool isValidBST(TreeNode* root) {
        ok = true;
        prev = INT_MIN ### 2LL;
        if (root)
            traverse(root);
        return ok;
    }

    void traverse(TreeNode* root) {
        if (root->left) 
            traverse(root->left);
        if (root->val <= prev)
            ok = false;
        prev = root->val;
        if (root->right)
            traverse(root->right);
    }
};
```

### 700

```cpp
class Solution {
public:
    TreeNode* searchBST(TreeNode* root, int val) {
        if (!root)
            return nullptr;
        if (root->val < val)
            return searchBST(root->right, val);
        else if (root->val > val)
            return searchBST(root->left, val);
        return root;
    }
```

### 230

```cpp
class Solution {
    int cnt = 0;
    int ans = ###1;
public:
    int kthSmallest(TreeNode* root, int k) {
        cnt = k;
        f(root);
        return ans;
    }

    void f(TreeNode* root) {
        if (cnt <= 0)
            return;
        if (root->left)
            f(root->left);
        ######cnt;
        if (!cnt) {
            ans = root->val;
            return ;
        }
        if (root->right)
            f(root->right);
    }
};
```

### 99

### 108

```cpp
class Solution {
public:
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        TreeNode* root = nullptr;
        dfs(root, nums, 0, nums.size());
        return root;
    }

    void dfs(TreeNode*& root, vector<int>& nums, int l, int r) {
        if (l < r) {
            int m = l + ((r ### l) >> 1);
            root = new TreeNode(nums[m]);
            dfs(root->left, nums, l, m);
            dfs(root->right, nums, m + 1, r);
        }
    }
};
```

### 501

```cpp
class Solution {
    int maxn = 0;
    int prev = 0;
    int dump = 0;
public:
    vector<int> findMode(TreeNode* root) {
        vector<int> ans;
        traverse(root, ans);
        return ans;
    }

    void traverse(TreeNode* root, vector<int>& ans) {
        if (root) {
            traverse(root->left, ans);
            if (dump) {
                if (root->val == prev) {
                    ++dump;
                } else {
                    dump = 1;
                }
                if (dump == maxn) {
                    ans.push_back(root->val);
                } else if (dump > maxn) {
                    ans.clear();
                    ans.push_back(root->val);
                }
                maxn = max(maxn, dump);
            } else {
                maxn = dump = 1;
                ans = {root->val};
            }
            prev = root->val;
            traverse(root->right, ans);
        }
    }
};
```

### 450

```cpp
class Solution {
public:
    TreeNode* deleteNode(TreeNode* root, int key) {
        // return deleteNode_(root, key);
        if (root) {
            if (root->val == key) {
                if (root->left) {
                    TreeNode* tmp = mostRight(root->left);
                    root->val = tmp->val;
                    root->left = deleteNode(root->left, tmp->val);
                } else if (root->right) {
                    TreeNode* tmp = mostLeft(root->right);
                    root->val = tmp->val;
                    root->right = deleteNode(root->right, tmp->val);
                } else {
                    delete root;
                    root = nullptr;
                }
            } else if (root->val > key) {
                root->left = deleteNode(root->left, key);
            } else {
                root->right = deleteNode(root->right, key);
            }
        }
        return root;
    }

    TreeNode* mostRight(TreeNode* root) {
        TreeNode* p = root;
        while (p->right)
            p = p->right;
        return p;
    }
    
    TreeNode* mostLeft(TreeNode* root) {
        TreeNode* p = root;
        while (p->left)
            p = p->left;
        return p;
    }
};
```

## binary search
### 35

```cpp
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        return myLower_bound(nums, target);
    }

    inline int myLower_bound(const vector<int>& nums, int target) const {
        int l = 0, r = nums.size();
        while (l < r) {
            int m = l + (r ### l) / 2;
            if (nums[m] < target)
                l = m + 1;
            else if (nums[m] == target)
                l = r =m;
            else
                r = m;
        }
        return l;
    }
};
```

### 33

```cpp
using namespace std;
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int l = 0, r = nums.size();
        while (l < r) {
            int m = l + (r ### l) / 2;
            if (nums[m] == target)
                return m;
            if (nums[l] > nums[r ### 1]) { // two range
                if (target > nums[r ### 1]) { // left part 
                    if (nums[m] <= nums[r ### 1]) {
                        r = m;
                    } else {
                        normal(l, r, m, nums[m], target);
                    }
                } else { // right part
                    if (nums[m] > nums[r ### 1]) {
                        l = m + 1;
                    } else {
                        normal(l, r, m, nums[m], target);
                    }
                }
            } else { // one range
                normal(l, r, m, nums[m], target);
            }
        }
        return ###1;
    }

    void normal(int &l, int &r, const int& m, const int& numsm, const int& target) {
        if (numsm < target)
            l = m + 1;
        else r = m;
    }
};
```

### 69

```cpp
class Solution {
public:
    int mySqrt(int x) {
        double l = 0, r = INT_MAX;
        while (!doubleEqual(l, r)) {
            double m = (l + r) / 2.0;
            // cout << m << endl;
            if (m * m < x)
                l = m;
            else r = m;
        }
        return l + 0.000002;
    }
    bool doubleEqual(const double& x, const double& y) {
        return fabs(x ### y) < 0.000001;
    }
};
```

### 74

```cpp
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        const int r = matrix.size();
        const int c = matrix[0].size();
        int total = r * c;
        int begin = 0, end = total;
        while (begin < end) {
            int m = begin + (end ### begin) / 2;
            const int& num = getNum(matrix, m, c);
            if (num == target)
                return true;
            if (num < target)
                begin = m + 1;
            else
                end = m;
        }
        return false;
    }

    inline const int& getNum(const vector<vector<int>>& matrix, const int& index, const int& cols) {
        return matrix[index / cols][index % cols];
    }
};
```

### 875

```cpp
class Solution {
public:
    int minEatingSpeed(vector<int>& piles, int H) {
        int k = INT_MAX;
        int l = 1, r = INT_MAX ### 10000;
        while (l <= r) {
            int m = (l + r) >> 1;
            if (canEat(piles, m, H)) {
                k = min(k, m);
                r = m ### 1;
            } else {
                l = m + 1;
            }
        }
        return k;    
    }

    bool canEat(const vector<int>& piles, const int& m, const int& h) {
        return accumulate(piles.begin(), piles.end(), 
        0, [=](int init, int it){
            return init + ((it % m) ? it / m + 1 : it / m);
        }) <= h;
    }
};
```

### 4
### 378

```cpp
class Solution {
public:
    int kthSmallest(vector<vector<int>>& matrix, int k) {
        int left = matrix[0][0], right = matrix.back().back();
        while (left < right) {
            int mid = (left + right) / 2;
            if (count(matrix, mid) < k) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }
        return left;
    }

    int count(const vector<vector<int>>& matrix, int target) {
        int cnt = 0;
        for (const auto& vec : matrix) {
            cnt += upper_bound(vec.begin(), vec.end(), target) ### vec.begin();
        }
        return cnt;
    }
};
```

### 719

```cpp
class Solution {
public:
    int smallestDistancePair(vector<int>& nums, int k) {
        sort(nums.begin(), nums.end());
        int left = 0, right = nums.back() ### nums[0];
        while (left < right) {
            int mid = (left + right) / 2;
            int num = numOfLessThanTarget(nums, mid);
            if (num < k)
                left = mid + 1;
            else
                right = mid;
        }
        return left;
    }

    static int numOfLessThanTarget(const vector<int>& nums, int target) {
        int cnt = 0;
        for (int i = 0, j = 0; i < nums.size(); ++i) {
            for (; nums[i] ### nums[j] > target; ++j);
            cnt += i ### j;
        }
        return cnt;
    }
```

# week 4
## two pointers
### 11

```cpp
class Solution {
public:
    int maxArea(vector<int>& height) {
        int l = 0, r = height.size() ### 1;
        int ans = 0; 
        while (l < r) {
            ans = max(ans, (r ### l) * min(height[l], height[r]));
            if (height[l] >= height[r]) {
                int base = height[r];
                for (; l < r && height[r] <= base && height[r] <= height[l]; ######r);
            } else {
                int base = height[l];
                for (; l < r && height[l] <= base && height[l] <= height[r]; ++l);
            }
        }
        return ans;
    }
};
```

### 125
```cpp
class Solution {
public:
    bool isPalindrome(string s) {
        int l = 0, r = (int)s.size() - 1;
        while (l < r) {
            for (; l < r && !isDigitOrNumber(s[l]); ++l);
            for (; l < r && !isDigitOrNumber(s[r]); --r);
            if (l < r && tolower(s[l]) != tolower(s[r])) {
                return false;
            }
            ++l;
            --r;
        }
        return true;
    }

    bool isDigitOrNumber(char c) {
        return ('0' <= c && c <= '9') ||
        ('a' <= c && c <= 'z') ||
        ('A' <= c && c <= 'Z');
    }
};
```
### 917
```cpp
class Solution {
public:
    string reverseOnlyLetters(string S) {
        int l = 0, r = int(S.size()) - 1;
        while (l < r) {
            for (; l < r && !isalpha(S[l]); ++l);
            for (; l < r && !isalpha(S[r]); --r);
            if (l < r)
                swap(S[l], S[r]);
            ++l;
            --r;
        }
        return S;
    }
};
```
### 167
```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& numbers, int target) {
        vector<int> ans;
        int l = 0, r = int(numbers.size()) - 1;
        while (l < r) {
            if (numbers[l] + numbers[r] == target) {
                ans.push_back(l + 1);
                ans.push_back(r + 1);
                ++l;
            }
            else if (numbers[l] + numbers[r] < target) {
                ++l;
            } else {
                --r;
            }
        }
        return ans;
    }
};
```
### 977
```cpp
class Solution {
public:
    vector<int> sortedSquares(vector<int>& nums) {
        int l = 0, r = nums.size() - 1, pos = r + 1;
        vector<int> ans(nums.size());
        while (l <= r) {
            if (abs(nums[l]) < abs(nums[r])) {
                ans[--pos] = nums[r] * nums[r];
                --r;
            } else {
                ans[--pos] = nums[l] * nums[l];
                ++l;
            }
        }
        return ans;
    }
};
```
### 992
  
## Advanced
### 208
```cpp
class Trie {
    struct Node {
        vector<shared_ptr<Node>> next;
        bool isEnd;
        Node() : isEnd(false) {
            next.resize(26);
        }
    };

    Node root;
public:
    /** Initialize your data structure here. */
    Trie() {
        root.isEnd = true;
    }
    
    /** Inserts a word into the trie. */
    void insert(string word) {
        Node *p = &root;
        for (const auto& c : word) {
            if (!p->next[c - 'a']) {
                p->next[c - 'a'] = make_shared<Node>();
            }
            p = p->next[c - 'a'].get();
        }
        p->isEnd = true;
    }
    
    /** Returns if the word is in the trie. */
    bool search(string word) {
        Node *p = &root;
        return _search(word, &p) && p->isEnd;
    }
    
    /** Returns if there is any word in the trie that starts with the given prefix. */
    bool startsWith(string prefix) {
        Node *p = &root;
        return _search(prefix, &p);
    }

private:
    bool _search(const string& word, Node** p) const {
        for (const auto& c: word) {
            if (!(*p)->next[c - 'a'])
                return false;
            *p = (*p)->next[c - 'a'].get();
        }
        return true;
    }
};
```
### 307
```cpp
class NumArray {
    struct SegmentTree {
        const vector<int>& rawData_;
        vector<int> num_;
        int index;
        
        SegmentTree(const vector<int>& rawData) : rawData_(rawData) {
            num_.resize(rawData.size() * 4 + 5);
            index = 0;
            build(1, 1, rawData.size());
        }

        void build(int root, int l, int r) {
            if (l == r) {
                num_[root] = rawData_[index++];
                return ;
            }
            int m = (l + r) / 2;
            build(root << 1, l, m);
            build(root << 1 | 1, m + 1, r);
            num_[root] = num_[root << 1] + num_[root << 1 | 1];
        }

        void update(int pos, int val) {
            update_(1, 1, rawData_.size(), pos + 1, val);
        }

        void update_(int root, int l, int r, int pos, int val) {
            if (l == r) {
                num_[root] = val;
                return ;
            }
            int m = (l + r) / 2;
            if (pos <= m) {
                update_(root << 1, l, m, pos, val);
            } else {
                update_(root << 1 | 1, m + 1, r, pos, val);
            }
            num_[root] = num_[root << 1] + num_[root << 1 | 1];
        }

        int query(int left, int right) {
            return query_(1, 1, rawData_.size(), left  + 1, right + 1);
        }

        int query_(int root, int l, int r, int a, int b) {
            if (a <= l && r <= b) {
                return num_[root];
            }
            int m = (l + r) / 2;
            int sum = 0;
            if (a <= m) {
                sum += query_(root << 1, l, m, a, b);
            }
            if (b > m) {
                sum += query_(root << 1 | 1, m + 1, r, a, b);
            }
            return sum;
        }
    };

    SegmentTree seg;

public:
    NumArray(vector<int>& nums) : seg(nums) {

    }
    
    void update(int index, int val) {
        seg.update(index, val);
    }
    
    int sumRange(int left, int right) {
        return seg.query(left, right);
    }
};
```
### 901
```cpp
class StockSpanner {
    vector<int> data_;
    vector<int> f_;
    int index;
public:
    StockSpanner() : index(-1) {
    }
    
    int next(int price) {
        ++index;
        data_.push_back(price);
        f_.push_back(index);
        int parent = index;
        while (parent > 0 && data_[index] >= data_[parent - 1]) {
            f_[index] = parent - 1;
            parent = find(index);
        }
        return index - f_[index] + 1;
    }

    int find(int x) {
        return f_[x] == x ? x : f_[x] = find(f_[x]);
    }
};
```
### 239
```cpp
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        list<int> l;
        vector<int> ans;
        for (int i = 0; i < k; ++i) {
            for (; !l.empty() && nums[l.back()] <= nums[i]; l.pop_back()) ;
            l.push_back(i);
        }
        ans.push_back(nums[l.front()]);
        for (int i = 1; i + k - 1 < nums.size(); ++i) {
            for (; !l.empty() && l.front() < i; l.pop_front());
            for (; !l.empty() && nums[l.back()] <= nums[i + k - 1 ]; l.pop_back());
            l.push_back(i + k - 1);
            ans.push_back(nums[l.front()]);
        }
        return ans;
    }
};
```

# week 5
## Graph
### 133
```cpp
class Solution {
    map<Node*, Node*> vis;
public:
    Node* cloneGraph(Node* node) {
        if (!node) {
            return nullptr;
        }
        queue<Node*> q;
        q.push(node);
        vis[node] = new Node(node->val);
        while (!q.empty()) {
            auto now = q.front();
            q.pop();
            for (auto& neighbor : now->neighbors) {
                if (vis.find(neighbor) == vis.end()) {
                    vis[neighbor] = new Node(neighbor->val);
                    q.push(neighbor);
                }
                vis[now]->neighbors.emplace_back(vis[neighbor]);
            }
        }
        return vis[node];
    }
};
```
### 200
```cpp
class Solution {
public:
    int numIslands(vector<vector<char>>& grid) {
        int cnt = 0;
        for (int i = 0; i < grid.size(); ++i) {
            for (int j = 0; j < grid[0].size(); ++j) {
                if (grid[i][j] == '1') {
                    ++cnt;
                    dfs(grid, i, j);
                }
            }
        }
        return cnt;
    }

    void dfs(vector<vector<char>>& grid, int i, int j) {
        static int r[] = {-1, 1, 0, 0}, c[] = {0, 0, -1, 1};
        grid[i][j] = '0';
        for (int k = 0; k < 4; ++k) {
            int ii = i + r[k], jj = j + c[k];
            if (0 <= ii && ii < grid.size() && 0 <= jj && jj < grid[0].size() && grid[ii][jj]== '1') {
                dfs(grid, ii, jj);
            }
        }
    }
};
```
### 841
```cpp
class Solution {
public:
    bool canVisitAllRooms(vector<vector<int>>& rooms) {
        int roomCanAccess = 0;
        vector<bool> vis(rooms.size(), false);
        queue<int> q;
        q.push(0);
        vis[0] = true;
        while (!q.empty()) {
            auto now = q.front();
            q.pop();
            ++roomCanAccess;
            for (const auto& room : rooms[now]) {
                if (!vis[room]) {
                    q.push(room);
                    vis[room] = true;
                }
            }
        }
        return roomCanAccess == rooms.size();
    }
};
```
### 207
```cpp
class Solution {
    enum State {
        unsearch,
        searching,
        searched,
    };
public:
    bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
        vector<vector<int>> g(numCourses);
        for (const auto& courses: prerequisites) {
            g[courses[1]].push_back(courses[0]);
        }
        vector<State> states(numCourses);
        for (int i=0; i<numCourses; ++i) {
            if(states[i] == State::unsearch && has_loop(i, g, states))
                return false;
        }
        return true;
    }
    bool has_loop(int root, const vector<vector<int>>& g, vector<State>& states) {
        states[root] = State::searching;
        for (const auto& node: g[root]) {
            if (states[node] == State::searching)
                return true;
            if (states[node] == State::unsearch &&
                has_loop(node, g, states))
                return true;
                
        }
        states[root] = State::searched;
        return false;
    }
};
```
### 399
```cpp
class Solution {
    vector<pair<int, double>> data;
    map<string, int> kv;
public:
    vector<double> calcEquation(vector<vector<string>>& equations, vector<double>& values, vector<vector<string>>& queries) {
        vector<double> ans;
        for (const auto& equation: equations) {
            for (int i=0; i<2; ++i) {
                const string& str = equation[i];
                if (kv.find(str) == kv.end()) {
                    int len = data.size();
                    data.push_back({len, 1.0});
                    kv[str] = len;
                }
            }
        }
        for (int i=0; i<equations.size(); ++i) {
            myunion(equations[i][0], equations[i][1], values[i]);
        }
        for (int i=0; i<queries.size(); ++i) {
            if (kv.find(queries[i][0]) == kv.end() || kv.find(queries[i][1]) == kv.end()){
                ans.push_back(-1.0);
                continue;
            }
            auto [f0, v0] = find(kv[queries[i][0]]);
            auto [f1, v1] = find(kv[queries[i][1]]);
            if (f0 == f1)
            {
                ans.push_back(v0 / v1);
            } else {
                ans.push_back(-1.0);
            }
        }
        return ans;
    }

    pair<int, double> find(int const& id) {
        if (data[id].first == id)
            return {id, 1.0};
        auto [root, v] = find(data[id].first);
        data[id] = make_pair(root, v * data[id].second);
        return data[id];
    }

    void myunion(string const& a, string const& b, double const& val) {
        int idb = kv[b], ida = kv[a];
        auto [pa, va] = find(ida);
        auto [pb, vb] = find(idb);
        data[pb] = make_pair(pa, 1.0 / val * va / vb);
    }
};
```
### 785
```cpp
class Solution {
public:
    bool isBipartite(vector<vector<int>>& graph) {
        int n = graph.size();
        vector<char> vis(n, -1);
        for (int i=0; i<n; ++i)
        {
            if (vis[i] == -1) {
                if (dfs(-1, i, 0, vis, graph)) {
                    return false;
                }
            }
        }
        return true;
    }

    bool dfs(int u, int v, int color, vector<char>& vis, vector<vector<int>> const& graph) {
        vis[v] = color;
        for (const auto next: graph[v]) {
            if (next == u) {
                continue;
            }
            if (vis[next] == -1) {
                if (dfs(v, next, color ^ 1, vis, graph))
                    return true;
            } else if (vis[next] == color) {
                return true;
            }
        }
        return false;
    }
};
```
### 997
```cpp
class Solution {
public:
    int findJudge(int n, vector<vector<int>>& trust) {
        vector<int> in(n + 1, 0), out(n + 1, 0);
        for (int i=0; i<trust.size(); ++i) {
            ++out[trust[i][0]];
            ++in[trust[i][1]];
        }
        for (int i=1; i<=n; ++i) {
            if (in[i] == n - 1 && out[i] == 0)
                return i;
        }
        return -1;
    }
};
```
### 433
```cpp
class Solution {
public:
    int minMutation(string start, string end, vector<string>& bank) {
        set<string> st(bank.begin(), bank.end());
        queue<pair<string, int>> q;
        q.push({start, 0});
        while (!q.empty()) {
            auto now = q.front();
            if (now.first == end)
                return now.second;
            q.pop();
            vector<string> toErase;
            for (const auto& tmp : st) {
                if (distOne(tmp, now.first)) {
                    q.push({tmp, now.second + 1});
                    toErase.push_back(tmp);
                }
            }
            for (const auto& s : toErase)
                st.erase(s);
        }
        return -1;
    }

    bool distOne(string const& a, string const& b) {
        int cnt = 0;
        for (int i=0; i<a.size(); ++i) {
            if (a[i] != b[i])
                ++cnt;
            if (cnt > 1)
                return false;
        }
        return cnt == 1;
    }
};
```
### 684
```cpp
class Solution {
public:
    vector<int> findRedundantConnection(vector<vector<int>>& edges) {
        int n = edges.size();
        vector<int> f(n + 1);
        for (int i = 0; i < n + 1 ; ++i)
            f[i] = i;
        for(const auto& edge : edges) {
            if (find(f, edge[0]) == find(f, edge[1]))
                return edge;
            Union(f, edge[0], edge[1]);
        }
        return vector<int> {-1, -1};
    }
    int find(vector<int>& f, int id) {
        return f[id] = id != f[id] ? find(f, f[id]) : id;
    }
    void Union(vector<int>& f, int a, int b) {
        int fa = find(f, a), fb = find(f, b);
        f[fa] = fb;
    }
};
```
### 743
```cpp
class Solution {
public:
    int networkDelayTime(vector<vector<int>>& times, int n, int k) {
        vector<vector<pair<int, int>>> g(n + 1);
        for (const auto & vec : times) {
            g[vec[0]].push_back({vec[1], vec[2]});
        }
        vector<int> dis(n + 1, INT_MAX);
        dis[k] = 0;
        queue<int> q;
        q.push(k);
        while (!q.empty()) {
           auto now = q.front();
           q.pop();
           for (const auto& pr: g[now]) {
               if (dis[pr.first] > dis[now] + pr.second) {
                   dis[pr.first] = dis[now] + pr.second;
                   q.push(pr.first);
               }
           }
        }
        int maxn;
        for (int i=1; i<=n; ++i)
            maxn = max(maxn, dis[i]);
        return maxn == INT_MAX ? -1 : maxn;
    }
};
```
### 847
```cpp
class Solution {
    vector<vector<int>> dp;
    int maxn = 99999;
public:
    int shortestPathLength(vector<vector<int>>& graph) {
        int n = graph.size();
        int targetState = 1 << n;
        dp.resize(n);
        for_each(dp.begin(), dp.end(), [=](auto & it){it.resize(targetState);});
        for (int i=0; i<n; i++)
            for (int j=0; j< targetState; ++j)
                dp[i][j] = -1;
        int ans = maxn;
        for (int i=0; i<n; ++i) {
            dp[i][1 << i] = 0;
        }
        for (int i=0; i<n; ++i) {
            ans = min(ans, dfs(graph, i, targetState - 1));
        }
        return ans;
    }

    int dfs(vector<vector<int>> const& g, int start, int state) {
        if (dp[start][state] != -1)
            return dp[start][state];
        dp[start][state] = maxn;
        for (const int child : g[start])  {
            dp[start][state] = min(dp[start][state], 1 + dfs(g, child, state & ~(1 << start)));
            dp[start][state] = min(dp[start][state], 1 + dfs(g, child, state));
        }
        return dp[start][state];
    }
};
```


# week 6
## graph
### 332
```cpp
class Solution {
    vector<string> ans;
    map<string, map<string, int>> g;
    bool ok = false;
    int n_tickets;
public:
    vector<string> findItinerary(vector<vector<string>>& tickets) {
        n_tickets = tickets.size();
        for (auto & vec : tickets) {
            g[vec[0]][vec[1]]++;
        }
        vector<string> vec{"JFK"};
        dfs(vec);
        return ans;
    }

    void dfs(vector<string> & vec) {
        if (ok)
            return;
        if (vec.size() == n_tickets + 1) {
            ans = vec;
            ok = true;
        }
        string const& now = vec.back();
        for (auto & p : g[now]) {
            if (p.second > 0) {
                vec.push_back(p.first);
                --p.second;
                dfs(vec);
                ++p.second;
                vec.pop_back();
            }
        }

    }
};
```
### 1192
```cpp
class Solution {
    vector<vector<int>> g;
    vector<int> low;
    vector<int> dfn;
    int num = 0;
public:
    vector<vector<int>> criticalConnections(int n, vector<vector<int>>& connections) {
        vector<vector<int>> ans;
        g.resize(n);
        for (int i=0; i<connections.size(); ++i) {
            g[connections[i][0]].push_back(connections[i][1]);
            g[connections[i][1]].push_back(connections[i][0]);
        }

        low.resize(n);
        dfn.resize(n);
        fill(low.begin(), low.end(), INT_MAX);
        fill(dfn.begin(), dfn.end(), INT_MAX);
        dfs(-1, 0, ans);
        return ans;
    }

    void dfs(int root, int now, vector<vector<int>>& ans) {
        dfn[now] = low[now] = ++num;
        for (int i=0; i<g[now].size(); ++i) {
            int u = g[now][i];
            if (u == root)
                continue;
            if (dfn[u] == INT_MAX) {
                dfs(now, u, ans);
                low[now] = min(low[now], low[u]);
            }
            else {
                low[now] = min(low[now], dfn[u]);
            }
        }
        if (low[now] >= dfn[now] && root != -1)
            ans.push_back({root, now});
    }
};
### 943
```cpp
class Solution {
public:
    bool isCousins(TreeNode* root, int x, int y) {
        if (!root)
            return false;
        int dx =0, dy = 0;
        TreeNode* px = nullptr;
        TreeNode* py = nullptr;
        dfs(nullptr, root, x, 0, dx, px);
        dfs(nullptr, root, y, 0, dy, py);
        return dx == dy && px != py;
    }
    void dfs(TreeNode* parent, TreeNode* root, int target, int cur, int& depth, TreeNode*& ansParent) {
        if (root->val == target) {
            depth = cur;
            ansParent = parent;
        }
        if (root->left) {
            dfs(root, root->left, target, cur + 1, depth, ansParent);
        }
        if (root->right) {
            dfs(root, root->right, target, cur + 1, depth, ansParent);
        }
    }
};
```
### 959
```cpp
class Solution {
    int dr[4] = {-1, 1, 0, 0};
    int dc[4] = {0, 0, -1, 1};
public:
    int regionsBySlashes(vector<string>& grid) {
        int n = grid.size();
        vector<vector<char>> newGrid(n * 3);
        for_each(newGrid.begin(), newGrid.end(), [=](auto& it) {it.resize(n * 3);});
        for (int i=0; i<n; ++i) {
            for (int j=0; j<n; ++j) {
                int ii = i*3, jj = j*3;
                if (grid[i][j] == '/') {
                    newGrid[ii][jj+2] = newGrid[ii+1][jj+1] = newGrid[ii+2][jj] = 1;
                } else if (grid[i][j] == '\\') {
                    newGrid[ii][jj] = newGrid[ii+1][jj+1] = newGrid[ii+2][jj+2] = 1;
                }
            }
        }
        int ans = 0;
        for (int i=0; i<n*3; ++i) {
            for (int j=0; j<n*3; ++j) {
                if (newGrid[i][j] != 1) {
                    ++ans;
                    dfs(i, j, n*3, newGrid);
                }
            }
        }
        return ans;
    }
    void dfs(int r, int c, int len, vector<vector<char>>& grid) {
        grid[r][c] = 1;
        for (int i=0; i<4; ++i) {
            int rr = r + dr[i], cc = c + dc[i];
            if (rr >= 0 && rr < len && 0 <= cc && cc < len && grid[rr][cc] != 1) {
                dfs(rr, cc, len, grid);
            }
        }
    }
};
```

## search (bfs/dfs)
### 17
```cpp
class Solution {
    vector<string> phone {"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
public:
    vector<string> letterCombinations(string digits) {
        vector<string> ans;
        if (!digits.empty())
            dfs(digits, 0, ans, "");
        return ans;
    }

    void dfs(const string& digits, int pos, vector<string>& ans, string tmp) {
        if (pos == digits.size()) {
            ans.push_back(tmp);
        } else {
            for(int i=0; i<phone[digits[pos]-'0'].size(); ++i) {
                dfs(digits, pos + 1, ans, tmp + phone[digits[pos] - '0'][i]);
            }
        }
    }
};
```
### 46
### 22
### 37
### 79
### 127
### 542
### 698

# week 7
## dp
### 70
### 303
### 53
### 62
### 85
### 198
### 279
### 139
### 300
### 96
### 221
### 213
### 140
### 673
### 89
### 10


# week 8
## dp
### 1105
### 131
### 72
### 1139
### 688
### 322
### 813
### 1223
### 312
### 741
### 546
### 943
### 712
### 576
### 377
### 1220
### 1278
### 664
### 980
