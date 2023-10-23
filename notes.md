[BM2 链表内指定区间反转](https://www.nowcoder.com/practice/b58434e200a648c589ca2063f1faf58c?tpId=295&tqId=654&ru=/exam/oj&qru=/ta/format-top101/question-ranking&sourceUrl=%2Fexam%2Foj)
```C++
/**
 * struct ListNode {
 *	int val;
 *	struct ListNode *next;
 *	ListNode(int x) : val(x), next(nullptr) {}
 * };
 */
#include <climits>
class Solution {
public:
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param head ListNode类 
     * @param m int整型 
     * @param n int整型 
     * @return ListNode类
     */
    ListNode* reverseBetween(ListNode* head, int m, int n) {
        // write code here
        // 为了统一操作，加入一个空的头节点
        ListNode dummy(-1);
        dummy.next = head;
        ListNode *p = &dummy;
        for (int i = 0; i < m - 1; ++i) {
            p = p->next;
        }
        // 定位到m - 1节点位置，对之后的n-m+1个阶段做反转
        ListNode *q = p->next;
        for (int i = 0; i < n - m; ++i) {
            // 反转的思路就是q指向当前待反转的节点，p指向饭庄之后的第m个节点
            ListNode *tmp = q->next;
            q->next = tmp->next;
            tmp->next = p->next;
            p->next = tmp;
        }
        return dummy.next;
    }
};
```


[链表中环的入口](https://www.nowcoder.com/practice/253d2c59ec3e4bc68da16833f79a38e4?tpId=295&tqId=23449&ru=/exam/oj&qru=/ta/format-top101/question-ranking&sourceUrl=%2Fexam%2Foj)
```C++

/*
struct ListNode {
    int val;
    struct ListNode *next;
    ListNode(int x) :
        val(x), next(NULL) {
    }
};
*/
class Solution {
public:
    ListNode* EntryNodeOfLoop(ListNode* pHead) {
        if (!pHead) {
            return nullptr;
        }
        ListNode *fast = pHead;
        ListNode *slow = pHead;
        while (fast && fast->next) {
            fast = fast->next->next;
            slow = slow->next;
            if (fast == slow) { // 有环，相遇的时候终止循环
                break;
            }
        }
        if (!fast || !fast->next) { // 无环的情况
            return nullptr;
        }
        slow = pHead;
        while (slow != fast) {
            slow = slow->next;
            fast = fast->next;
        }
        return slow;
    }
};
```

[两个链表的第一个公共节点](https://www.nowcoder.com/practice/6ab1d9a29e88450685099d45c9e31e46?tpId=295&tqId=23257&ru=%2Fpractice%2F253d2c59ec3e4bc68da16833f79a38e4&qru=%2Fta%2Fformat-top101%2Fquestion-ranking&sourceUrl=%2Fexam%2Foj)
```C++
/*
struct ListNode {
	int val;
	struct ListNode *next;
	ListNode(int x) :
			val(x), next(NULL) {
	}
};*/
#include <type_traits>
class Solution {
public:
    ListNode* FindFirstCommonNode( ListNode* pHead1, ListNode* pHead2) {
        int n1 = 0;
		ListNode *p1 = pHead1;
		while (p1) { ++n1; p1 = p1->next; }
		int n2 = 0;
		ListNode *p2 = pHead2;
		while (p2) { ++n2; p2 = p2->next; }
		p1= pHead1;
		p2 = pHead2;
		if (n1 < n2) {
			swap(n1, n2);
			swap(p1, p2);
		}
		for (int i = 0; i < n1 - n2; ++i) {
			p1 = p1->next;
		}
		while (p1 != p2) {
			p1 = p1->next;
			p2 = p2->next;
		}
		return p1;
    }
};

```

```C++
/*
struct ListNode {
	int val;
	struct ListNode *next;
	ListNode(int x) :
			val(x), next(NULL) {
	}
};*/
class Solution {
public:
    ListNode* FindFirstCommonNode( ListNode* pHead1, ListNode* pHead2) {
        if (pHead1 == nullptr || pHead2 == nullptr) {
			return nullptr;
		}
		// 获取长度
		int n1 = 0, n2 = 0;
		ListNode *p = pHead1;
		while(p) {
			p = p->next;
			++n1;
		}
		p = pHead2;
		while(p) {
			p = p->next;
			++n2;
		}

		// 长的先移动，然后然后同步移动
		ListNode *fast, *slow;
		if (n1 > n2) {
			fast = pHead1;
			slow = pHead2;
			for (int i = 0; i < n1 - n2; ++i) {
				fast = fast->next;
			}
			while(fast != slow) {
				fast = fast->next;
				slow = slow->next;
			}
			return slow;
		} 
		fast = pHead2;
		slow = pHead1;
		for (int i = 0; i < n2 - n1; ++i) {
			fast = fast->next;
		}
		while(fast != slow) {
			fast = fast->next;
			slow = slow->next;
		}
		return slow;
    }
};
```

[无重复升序数组的查找](https://www.nowcoder.com/practice/d3df40bd23594118b57554129cadf47b?tpId=295&tqId=1499549&ru=%2Fpractice%2F6ab1d9a29e88450685099d45c9e31e46&qru=%2Fta%2Fformat-top101%2Fquestion-ranking&sourceUrl=%2Fexam%2Foj)
```C++
class Solution {
public:
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param nums int整型vector 
     * @param target int整型 
     * @return int整型
     */
    int search(vector<int>& nums, int target) {
        // write code here
        int lo = 0;
        int hi = nums.size();
        while (lo < hi) {
            int mid = (lo + hi) / 2;
            if (nums[mid] == target) {
                return mid;
            } else if (nums[mid] < target) {
                lo = mid + 1;
            } else {
                hi = mid;
            }
        }
        return -1;
    }
};
```

[二维数组中的查找](https://www.nowcoder.com/practice/abc3fe2ce8e146608e868a70efebf62e?tpId=295&tqId=23256&ru=%2Fpractice%2F6ab1d9a29e88450685099d45c9e31e46&qru=%2Fta%2Fformat-top101%2Fquestion-ranking&sourceUrl=%2Fexam%2Foj)
```C++
#include <vector>
class Solution {
public:
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param target int整型 
     * @param array int整型vector<vector<>> 
     * @return bool布尔型
     */
    bool Find(int target, vector<vector<int> >& array) {
        // write code here
        // 排除空数组
        if (array.empty() || array[0].empty()) {
            return false;
        }
        const int m = array.size();
        const int n = array[0].size();
        // 从中间位置开始查找，不断调整
        int x = 0;
        int y = n - 1;
        while (x >= 0 && x < m && y >= 0 && y < n) {
            if (array[x][y] == target) {
                return true;
            } else if (array[x][y] < target) {
                ++x;
            } else {
                --y;
            }
        }
        return false;
    }
};
```

[寻找峰值](https://www.nowcoder.com/practice/fcf87540c4f347bcb4cf720b5b350c76?tpId=295&tqId=2227748&ru=/exam/intelligent&qru=/ta/format-top101/question-ranking&sourceUrl=%2Fexam%2Fintelligent)
```C++
class Solution {
public:
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param nums int整型vector 
     * @return int整型
     */
    int findPeakElement(vector<int>& nums) {
        // write code here
        int lo = 0;
        int hi = nums.size() - 1;
        while (lo < hi) {
            int mid = lo + (hi - lo) / 2;
            cout << mid << endl;
            if (nums[mid] < nums[mid + 1]) {
                lo = mid + 1;
            } else {
                hi = mid;
            }
        }
        return lo;
    }
};

#include <algorithm>
#include <iterator>
class Solution {
public:
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param nums int整型vector 
     * @return int整型
     */
    int findPeakElement(vector<int>& nums) {
        // write code here
        return distance(begin(nums), max_element(begin(nums), end(nums)));
    }
};
```

[二叉树的层序遍历](https://www.nowcoder.com/practice/04a5560e43e24e9db4595865dc9c63a3?tpId=295&tqId=644&ru=/exam/company&qru=/ta/format-top101/question-ranking&sourceUrl=%2Fexam%2Fcompany)
```C++
/**
 * struct TreeNode {
 *	int val;
 *	struct TreeNode *left;
 *	struct TreeNode *right;
 *	TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 * };
 */
#include <vector>
class Solution {
public:
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param root TreeNode类 
     * @return int整型vector<vector<>>
     */
    vector<vector<int> > levelOrder(TreeNode* root) {
        // write code here
        if (!root) {
            return {};
        }
        queue<TreeNode *> q;
        q.push(root);
        vector<vector<int>> result;
        while(!q.empty()) {
            int sz = q.size();
            vector<int> cur;
            for (int i = 0; i < sz; ++i) {
                TreeNode *tmp = q.front();
                q.pop();
                if (tmp->left) {
                    q.push(tmp->left);
                }
                if (tmp->right) {
                    q.push(tmp->right);
                }
                cur.push_back(tmp->val);
            }
            result.push_back(cur);
        }
        return result;
    }
};
```

[二叉树的最大深度](https://www.nowcoder.com/practice/8a2b2bf6c19b4f23a9bdb9b233eefa73?tpId=295&tqId=642&ru=/exam/company&qru=/ta/format-top101/question-ranking&sourceUrl=%2Fexam%2Fcompany)
```C++
/**
 * struct TreeNode {
 *	int val;
 *	struct TreeNode *left;
 *	struct TreeNode *right;
 *	TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 * };
 */
class Solution {
public:
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param root TreeNode类 
     * @return int整型
     */
    int maxDepth(TreeNode* root) {
        // write code here
        if (root == nullptr) {
            return 0;
        }
        return 1 + max(maxDepth(root->left), maxDepth(root->right));
    }
};
```

[三数之和](https://leetcode.cn/problems/3sum/)
```C++
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        const int target = 0;
        sort(begin(nums), end(nums));
        vector<vector<int>> result;
        auto end_iter = end(nums);
        for (auto i = begin(nums); i < end(nums) - 2; ++i) {
            if (i > begin(nums) && *i == *(i - 1)) {
                continue;
            }
            auto j = i + 1;
            auto k = end_iter - 1;
            while(j < k) {
                int cur_sum = *i + *j + *k;
                if (cur_sum < target) {
                    ++j;
                    while(j < k && *j == *(j - 1)) ++j;
                } else if (cur_sum > target) {
                    --k;
                    while(j < k && *k == *(k + 1)) --k;
                } else {
                    result.push_back({*i,*j,*k});
                    ++j;
                    --k;
                    while(j < k && *j == *(j - 1) && *k == *(k + 1)) {
                        ++j;
                        --k;
                    }
                }
            }
        }
        return result;
    }
};
```

[寻找旋转排序数组中的最小值](https://leetcode.cn/problems/find-minimum-in-rotated-sorted-array/solutions/698479/xun-zhao-xuan-zhuan-pai-xu-shu-zu-zhong-5irwp/?envType=study-plan-v2&envId=top-100-liked)
```C++
class Solution {
public:
    int findMin(vector<int>& nums) {
        int lo = 0;
        int hi = nums.size() - 1;
        while(lo < hi) {
            int mid = lo + (hi - lo) / 2;
            if (nums[mid] < nums[hi]) {
                hi = mid;
            } else {
                lo = mid + 1;
            }
        }
        return nums[lo];
    }
};
```

[k个一组链表反转](https://leetcode.cn/problems/reverse-nodes-in-k-group/submissions/)
```C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* reverseKGroup(ListNode* head, int k) {
        ListNode dummy(-1, head);
        ListNode *pre = &dummy;
        while(head) {
            ListNode *tail = pre;
            for (int i = 0; i < k; ++i) {
                tail = tail->next;
                if (!tail) {
                    return dummy.next;
                }
            }
            ListNode *cur = pre->next;
            for (int i = 0; i < k - 1; ++i) {
                ListNode *nx = cur->next;
                cur->next = nx->next;
                nx->next = pre->next;
                pre->next = nx;
            }
            pre = cur;
        }
        return dummy.next;
    }
};
```


[合并K个有序链表](https://leetcode.cn/problems/vvXgSW/)
```C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        auto merge2Lists = [](ListNode *a, ListNode *b) ->ListNode* {
            if (!a || !b) {
                return a ? a : b;
            }
            ListNode dummy(-1);
            ListNode *tail = &dummy;
            while(a && b) {
                if (a->val < b->val) {
                    tail->next = a;
                    a = a->next;
                } else {
                    tail->next = b;
                    b = b->next;
                }
                tail = tail->next;
            }
            tail->next = a ? a : b;
            return dummy.next;
        };
        ListNode *result = nullptr;
        for (int i = 0; i < lists.size(); ++i) {
            result = merge2Lists(result, lists[i]);
        }
        return result;
    }
};
```

[删除链表倒数第n个节点](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/submissions/)
```C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode dummy(-1, head);
        ListNode *fast = &dummy;
        ListNode *slow = &dummy;
        for (int i = 0; i < n; ++i) {
            fast = fast->next;
        }
        while(fast->next) {
            fast = fast->next;
            slow = slow->next;
        }
        slow->next = slow->next->next;
        return dummy.next;
    }
};
```

[LRU cache](https://leetcode.cn/problems/lru-cache/submissions/)
```C++
class LRUCache {
    struct KV {
        int key;
        int value;
        KV(int k, int v) : key(k), value(v) {}
    };
    list<KV> cache_;
    unordered_map<int, decltype(begin(cache_))> map_;
    int capacity_;
public:
    LRUCache(int capacity) 
        : capacity_(capacity){}
    
    int get(int key) {
        if (map_.find(key) == map_.end()) {
            return -1;
        }
        cache_.splice(begin(cache_), cache_, map_[key]);
        map_[key] = begin(cache_);
        return map_[key]->value;
    }
    
    void put(int key, int value) {
        if (map_.find(key) != map_.end()) {
            cache_.splice(begin(cache_), cache_, map_[key]);
            map_[key] = begin(cache_);
            map_[key]->value = value;
        } else {
            if (cache_.size() == capacity_) {
                map_.erase(cache_.back().key);
                cache_.pop_back();
            }
            cache_.push_front(KV(key, value));
            map_[key] = begin(cache_);
        }
    }
};

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache* obj = new LRUCache(capacity);
 * int param_1 = obj->get(key);
 * obj->put(key,value);
 */
```

[滑动窗口的最大值](https://leetcode.cn/problems/sliding-window-maximum/solutions/543426/hua-dong-chuang-kou-zui-da-zhi-by-leetco-ki6m/)
```C++
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        multiset<int> window;
        for (int i = 0; i < k; ++i) {
            window.insert(nums[i]);
        }
        vector<int> result;
        result.push_back(*prev(window.end()));
        for (int i = 1; i < nums.size() - k + 1; ++i) {
            auto tmp = window.find(nums[i - 1]);
            window.erase(tmp);
            window.insert(nums[i + k - 1]);
            result.push_back(*prev(window.end()));
        }
        return result;
    }
};
```

[岛屿数量](https://leetcode.cn/problems/number-of-islands/description/)
```C++
class Solution {
public:
    int numIslands(vector<vector<char>>& grid) {
        const int m = grid.size();
        const int n = grid[0].size();
        vector<vector<bool>> visited(m, vector<bool>(n, false));
        int result = 0;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (!visited[i][j] && grid[i][j] == '1') {
                    bfs(i,j,grid,visited);
                    ++result;
                }
            }
        }
        return result;
    }
    void bfs(int i, int j, vector<vector<char>>& grid, vector<vector<bool>>& visited) {
        visited[i][j] = true;
        if (i - 1 >= 0 && grid[i - 1][j] == '1' && !visited[i - 1][j]) {
            bfs(i-1,j,grid,visited);
        }
        const int m = grid.size();
        const int n = grid[0].size();
        if (i + 1 < m && grid[i + 1][j] == '1' && !visited[i + 1][j]) {
            bfs(i+1,j,grid,visited);
        }
        if (j - 1 >= 0 && grid[i][j - 1] == '1' && !visited[i][j - 1]) {
            bfs(i,j-1,grid,visited);
        }
        if (j + 1 < n && grid[i][j + 1] == '1' && !visited[i][j + 1]) {
            bfs(i,j+1,grid,visited);
        }
    }
};
```

[从前序和中序序列构造二叉树](https://leetcode.cn/problems/construct-binary-tree-from-preorder-and-inorder-traversal/submissions/)
```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        return buildTree(begin(preorder), end(preorder), begin(inorder), end(inorder));
    }
    template<typename Iter>
    TreeNode* buildTree(Iter ps, Iter pe, Iter is, Iter ie) {
        if (ps == pe || is == ie) {
            return nullptr;
        }
        auto root_inorder = find(is, ie, *ps);
        TreeNode *root = new TreeNode(*ps);
        int left_sz = distance(is, root_inorder);
        root->left = buildTree(next(ps), next(ps, left_sz + 1), is, root_inorder);
        root->right = buildTree(next(ps, left_sz + 1), pe, next(root_inorder), ie);
        return root;
    }
};
```

[接雨水](https://leetcode.cn/problems/trapping-rain-water/description/)
```C++
class Solution {
public:
    int trap(vector<int>& height) {
        auto max_iter = max_element(begin(height), end(height));
        
        int result = 0;
        int peak = 0;
        for (auto iter = begin(height); iter != max_iter; ++iter) {
            if (*iter > peak) peak = *iter;
            else result += peak - *iter;
        }
        peak = 0;
        for (auto iter = prev(end(height)); iter != max_iter; --iter) {
            if (*iter > peak) peak = *iter;
            else result += peak - *iter;
        }
        return result;
    }
};
```

[打家劫舍](https://leetcode.cn/problems/Gu0c2T/submissions/)
```C++
class Solution {
public:
    int rob(vector<int>& nums) {
        int prepre = 0;
        int pre = 0;
        int result = 0;
        for (int i = 0; i < nums.size(); ++i) {
            result = max(nums[i] + prepre, pre);
            prepre = pre;
            pre = result;
        }
        return result;
    }
};
```

[跳跃游戏](https://leetcode.cn/problems/jump-game/description/)
```C++
class Solution {
public:
    bool canJump(vector<int>& nums) {
        int max_idx = 0 + nums[0];
        for (int i = 1; i < nums.size() && i <= max_idx; ++i) {
            max_idx = max(max_idx, i + nums[i]);
        }
        return max_idx >= nums.size() - 1;
    }
};
```

[二叉树的右视图](https://leetcode.cn/problems/binary-tree-right-side-view/submissions/)
```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    vector<int> rightSideView(TreeNode* root) {
        if (!root) {
            return {};
        }
        queue<TreeNode *> q;
        q.push(root);
        vector<int> result;
        while(!q.empty()) {
            int sz = q.size();
            for (int i = 0; i < sz; ++i) {
                TreeNode *cur = q.front();
                q.pop();
                if (cur->left) q.push(cur->left);
                if (cur->right) q.push(cur->right);
                if (i == sz - 1) {
                    result.push_back(cur->val);
                }
            }
        }
        return result;
    }
};
```

[圆圈中最后剩下的数（约瑟夫环问题）](https://leetcode.cn/problems/yuan-quan-zhong-zui-hou-sheng-xia-de-shu-zi-lcof/description/)
```C++
class Solution {
public:
    int iceBreakingGame(int num, int target) {
        if (num == 1) {
            return 0;
        }
        int x = iceBreakingGame(num - 1, target);
        return (target + x) % num;
    }
};
```

[乘积最大子数组](https://leetcode.cn/problems/maximum-product-subarray/description/)
```C++
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        int result = nums[0];
        int pre_max = nums[0];
        int pre_min = nums[0];
        for (int i = 1; i < nums.size(); ++i) {
            int cur_max = max({pre_max * nums[i], pre_min * nums[i], nums[i]});
            int cur_min = min({pre_max * nums[i], pre_min * nums[i], nums[i]});
            result = max(result, cur_max);
            pre_max = cur_max;
            pre_min = cur_min;
        }
        return result;
    }
};
```

[N皇后](https://leetcode.cn/problems/n-queens/submissions/)
```C++
class Solution {
public:
    vector<vector<string>> solveNQueens(int n) {
        vector<vector<string>> result;
        vector<int> path(n);
        dfs(path, 0, result);
        return result;
    }
    void dfs(vector<int>& path, int row, vector<vector<string>>& result) {
        const int n = path.size();
        if (row == n) {
            vector<string> cur;
            for (int r = 0; r < n; ++r) {
                string row(n, '.');
                row[path[r]] = 'Q';
                cur.push_back(row);
            }
            result.push_back(cur);
        }
        for (int col = 0; col < n; ++col) {
            if (check(path, row, col)) {
                path[row] = col;
                dfs(path, row + 1, result);
            }
        }
    }
    bool check(vector<int>& path, int row, int col) {
        for (int i = 0; i < row; ++i) {
            if (path[i] == col) {
                return false;
            }
            if (abs(i - row) == abs(path[i] - col)) {
                return false;
            }
        }
        return true;
    }
};
```

[编辑距离](https://leetcode.cn/problems/edit-distance/description/)
```C++
class Solution {
public:
    int minDistance(string word1, string word2) {
        const int n1 = word1.size();
        const int n2 = word2.size();
        vector<vector<int>> f(n1 + 1, vector<int>(n2 + 1));
        f[0][0] = 0;
        for(int r = 1; r <= n1; ++r) {
            f[r][0] = r;
        }
        for (int c = 1; c <= n2; ++c) {
            f[0][c] = c;
        }
        for (int r = 1; r <= n1; ++r) {
            for (int c = 1; c <= n2; ++c) {
                if (word1[r - 1] == word2[c - 1]) {
                    f[r][c] = f[r - 1][c - 1];
                } else {
                    f[r][c] = min({f[r - 1][c], f[r][c - 1], f[r - 1][c - 1]}) + 1;
                }
            }
        }
        return f[n1][n2];
    }
};
```

[二叉树中和为某一值的路径一](https://www.nowcoder.com/practice/508378c0823c423baa723ce448cbfd0c)
```C++
/**
 * struct TreeNode {
 *	int val;
 *	struct TreeNode *left;
 *	struct TreeNode *right;
 *	TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 * };
 */
class Solution {
public:
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param root TreeNode类 
     * @param sum int整型 
     * @return bool布尔型
     */
    bool hasPathSum(TreeNode* root, int sum) {
        // write code here
        if (!root) {
            return false;
        }
        if (!root->left && !root->right) {
            return root->val == sum;
        }
        return hasPathSum(root->left, sum - root->val) || hasPathSum(root->right, sum - root->val);
    }
};
```

[二叉树中和为某一值的路径二](https://www.nowcoder.com/practice/b736e784e3e34731af99065031301bca?tpId=13&tqId=11177&ru=/exam/oj)
```C++
/**
 * struct TreeNode {
 *	int val;
 *	struct TreeNode *left;
 *	struct TreeNode *right;
 *	TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 * };
 */
#include <vector>
class Solution {
public:
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param root TreeNode类 
     * @param target int整型 
     * @return int整型vector<vector<>>
     */
    vector<vector<int> > FindPath(TreeNode* root, int target) {
        // write code here
        if (!root) {
            return {};
        }
        vector<int> path;
        vector<vector<int>> result;
        dfs(root, target, path, result);  
        return result;  
    }
    void dfs(TreeNode *root, int target, vector<int>& path, vector<vector<int>>& result) {
        path.push_back(root->val);
        if (!root->left && !root->right) {
            if (root->val == target) {
                result.push_back(path);
            }
        }
        if (root->left) {
            dfs(root->left, target - root->val, path, result);
        }
        if (root->right) {
            dfs(root->right, target - root->val, path, result);
        }
        path.pop_back();
    }

};
```

[二叉树中和为某一值的路径三](https://www.nowcoder.com/practice/965fef32cae14a17a8e86c76ffe3131f?tpId=196&tqId=39283&ru=/exam/oj)
```C++
/**
 * struct TreeNode {
 *	int val;
 *	struct TreeNode *left;
 *	struct TreeNode *right;
 *	TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 * };
 */
class Solution {
public:
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param root TreeNode类 
     * @param sum int整型 
     * @return int整型
     */
    int result = 0;
    int FindPath(TreeNode* root, int sum) {
        // write code here
        if (!root) {
            return 0;
        }
        find(root, sum);
        FindPath(root->left, sum);
        FindPath(root->right, sum);
        return result;    
    }
    void find(TreeNode *root, int sum) {
        if (!root) {
            return;
        }
        sum -= root->val;
        if (sum == 0) {
            ++result;
        }
        find(root->left, sum);
        find(root->right, sum);
    }
};
```
