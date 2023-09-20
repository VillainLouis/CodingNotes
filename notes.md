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

