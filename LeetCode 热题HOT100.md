# LeetCode 热题HOT100

## 简单级别



[1. 两数之和](https://leetcode.cn/problems/two-sum/)

给定一个整数数组 `nums` 和一个整数目标值 `target`，请你在该数组中找出 **和为目标值** *`target`* 的那 **两个** 整数，并返回它们的数组下标。

你可以假设每种输入只会对应一个答案，并且你不能使用两次相同的元素。

你可以按任意顺序返回答案。

**示例 1：**

```
输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。
```

**示例 2：**

```
输入：nums = [3,2,4], target = 6
输出：[1,2]
```

**示例 3：**

```
输入：nums = [3,3], target = 6
输出：[0,1]
```

**提示：**

- `2 <= nums.length <= 104`
- `-109 <= nums[i] <= 109`
- `-109 <= target <= 109`
- **只会存在一个有效答案**

***

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> map = new HashMap<>();
        for(int i=0;i<nums.length;i++){
            if(map.containsKey(target-nums[i])) {
                return new int[]{map.get(target-nums[i]), i};
            }
            map.put(nums[i], i);
        }
        throw new IllegalArgumentException("No two sum solution");
    }
}
```



[20. 有效的括号](https://leetcode.cn/problems/valid-parentheses/)

给定一个只包括 `'('`，`')'`，`'{'`，`'}'`，`'['`，`']'` 的字符串 `s` ，判断字符串是否有效。

有效字符串需满足：

1. 左括号必须用相同类型的右括号闭合。
2. 左括号必须以正确的顺序闭合。
3. 每个右括号都有一个对应的相同类型的左括号。

 

**示例 1：**

**输入：**s = "()"

**输出：**true

**示例 2：**

**输入：**s = "()[]{}"

**输出：**true

**示例 3：**

**输入：**s = "(]"

**输出：**false

**示例 4：**

**输入：**s = "([])"

**输出：**true

**示例 5：**

**输入：**s = "([)]"

**输出：**false

 

**提示：**

- `1 <= s.length <= 104`
- `s` 仅由括号 `'()[]{}'` 组成

***

```java
class Solution {
    public boolean isValid(String s) {
        if(s==null||s.length()%2!=0){
            return false;
        }
        Stack<Character> stack=new Stack<>();
        Map<Character,Character> map=new HashMap<>(){{
            put(')','(');
            put(']','[');
            put('}','{');
        }};
        for(int i=0;i<s.length();i++){
            char c=s.charAt(i);
            if(map.containsKey(c)){
                if(!stack.isEmpty()&&stack.peek()==map.get(c)){
                    stack.pop();
                }else{
                    return false;
                }
            }else{
                stack.push(c);
            }
        }
        return stack.isEmpty();
    }
}
```



[21. 合并两个有序链表](https://leetcode.cn/problems/merge-two-sorted-lists/)

将两个升序链表合并为一个新的 **升序** 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/10/03/merge_ex1.jpg)

```
输入：l1 = [1,2,4], l2 = [1,3,4]
输出：[1,1,2,3,4,4]
```

**示例 2：**

```
输入：l1 = [], l2 = []
输出：[]
```

**示例 3：**

```
输入：l1 = [], l2 = [0]
输出：[0]
```

 

**提示：**

- 两个链表的节点数目范围是 `[0, 50]`
- `-100 <= Node.val <= 100`
- `l1` 和 `l2` 均按 **非递减顺序** 排列

***

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        if(l1==null){
            return l2;
        }
        if(l2==null){
            return l1;
        }
        ListNode head=new ListNode(0);
        ListNode tail=head;
        ListNode p1=l1;
        ListNode p2=l2;
        while(p1!=null&&p2!=null){
            if(p1.val<=p2.val){
                tail.next=p1;
                p1=p1.next;
            }else{
                tail.next=p2;
                p2=p2.next;
            }
            tail=tail.next;
        }
        if(p1!=null){
            tail.next=p1;
        }
        if(p2!=null){
            tail.next=p2;
        }
        return head.next;
    }

}
```



[70. 爬楼梯](https://leetcode.cn/problems/climbing-stairs/)

假设你正在爬楼梯。需要 `n` 阶你才能到达楼顶。

每次你可以爬 `1` 或 `2` 个台阶。你有多少种不同的方法可以爬到楼顶呢？

 

**示例 1：**

```
输入：n = 2
输出：2
解释：有两种方法可以爬到楼顶。
1. 1 阶 + 1 阶
2. 2 阶
```

**示例 2：**

```
输入：n = 3
输出：3
解释：有三种方法可以爬到楼顶。
1. 1 阶 + 1 阶 + 1 阶
2. 1 阶 + 2 阶
3. 2 阶 + 1 阶
```

 

**提示：**

- `1 <= n <= 45`

***

```java
class Solution {
    public int climbStairs(int n) {
       if(n<=2){
           return n;
       }
       int[] dp=new int[n+1];
       dp[1]=1;
       dp[2]=2;
       for(int i=3;i<=n;i++){
           dp[i]=dp[i-1]+dp[i-2];
       }
       return dp[n];
    }
}
```

```java
class Solution {
    public int climbStairs(int n) {
       if(n<=2){
           return n;
       }
       int a=1;
       int b=2;
       for(int i=3;i<=n;i++){
           int c=a+b;
           a=b;
           b=c;
       }
       return b;
    }
}
```



[94. 二叉树的中序遍历](https://leetcode.cn/problems/binary-tree-inorder-traversal/)

给定一个二叉树的根节点 `root` ，返回 *它的 **中序** 遍历* 。

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/09/15/inorder_1.jpg)

```
输入：root = [1,null,2,3]
输出：[1,3,2]
```

**示例 2：**

```
输入：root = []
输出：[]
```

**示例 3：**

```
输入：root = [1]
输出：[1]
```

 

**提示：**

- 树中节点数目在范围 `[0, 100]` 内
- `-100 <= Node.val <= 100`

***

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    private List<Integer> resultList=new ArrayList<>();

    public List<Integer> inorderTraversal(TreeNode root) {
        if(root==null){
            return new ArrayList<>();
        }
        processInorderTraversal(root);
        return resultList;
    }

    public void processInorderTraversal(TreeNode root){
        if(root==null){
            return;
        }
        processInorderTraversal(root.left);
        resultList.add(root.val);
        processInorderTraversal(root.right);
    }
}
```



[101. 对称二叉树](https://leetcode.cn/problems/symmetric-tree/)

给你一个二叉树的根节点 `root` ， 检查它是否轴对称。

 

**示例 1：**

![img](https://pic.leetcode.cn/1698026966-JDYPDU-image.png)

```
输入：root = [1,2,2,3,4,4,3]
输出：true
```

**示例 2：**

![img](https://pic.leetcode.cn/1698027008-nPFLbM-image.png)

```
输入：root = [1,2,2,null,3,null,3]
输出：false
```

 

**提示：**

- 树中节点数目在范围 `[1, 1000]` 内
- `-100 <= Node.val <= 100`

***

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public boolean isSymmetric(TreeNode root) {
        if(root==null){
            return true;
        }
        return processIsSymmertric(root.left,root.right);
    }

    private boolean processIsSymmertric(TreeNode node1,TreeNode node2){
        if(node1==null&&node2==null){
            return true;
        }
        if(node1==null||node2==null||node1.val!=node2.val){
            return false;
        }
        return processIsSymmertric(node1.left,node2.right)&&processIsSymmertric(node1.right,node2.left);
    }

    
}
```



[104. 二叉树的最大深度](https://leetcode.cn/problems/maximum-depth-of-binary-tree/)

给定一个二叉树 `root` ，返回其最大深度。

二叉树的 **最大深度** 是指从根节点到最远叶子节点的最长路径上的节点数。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/11/26/tmp-tree.jpg)

 

```
输入：root = [3,9,20,null,null,15,7]
输出：3
```

**示例 2：**

```
输入：root = [1,null,2]
输出：2
```

 

**提示：**

- 树中节点的数量在 `[0, 104]` 区间内。
- `-100 <= Node.val <= 100`

***

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public int maxDepth(TreeNode root) {
        if(root==null){
            return 0;
        }
        return Math.max(maxDepth(root.left),maxDepth(root.right))+1;
    }
}
```



[121. 买卖股票的最佳时机](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock/)

给定一个数组 `prices` ，它的第 `i` 个元素 `prices[i]` 表示一支给定股票第 `i` 天的价格。

你只能选择 **某一天** 买入这只股票，并选择在 **未来的某一个不同的日子** 卖出该股票。设计一个算法来计算你所能获取的最大利润。

返回你可以从这笔交易中获取的最大利润。如果你不能获取任何利润，返回 `0` 。

 

**示例 1：**

```
输入：[7,1,5,3,6,4]
输出：5
解释：在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
     注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格；同时，你不能在买入前卖出股票。
```

**示例 2：**

```
输入：prices = [7,6,4,3,1]
输出：0
解释：在这种情况下, 没有交易完成, 所以最大利润为 0。
```

 

**提示：**

- `1 <= prices.length <= 105`
- `0 <= prices[i] <= 104`

***

```java
class Solution {
    public int maxProfit(int[] prices) {
        if(prices==null||prices.length==0){
            return 0;
        }
        int minValue=Integer.MAX_VALUE;
        int result=0;
        for(int i=0;i<prices.length;i++){
            if(prices[i]<minValue){
                minValue=prices[i];
            }else{
                result=Math.max(result,prices[i]-minValue);
            }
        }
        return result;
    }
}
```



[136. 只出现一次的数字](https://leetcode.cn/problems/single-number/)

给你一个 **非空** 整数数组 `nums` ，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。

你必须设计并实现线性时间复杂度的算法来解决此问题，且该算法只使用常量额外空间。

 

**示例 1 ：**

**输入：**nums = [2,2,1]

**输出：**1

**示例 2 ：**

**输入：**nums = [4,1,2,1,2]

**输出：**4

**示例 3 ：**

**输入：**nums = [1]

**输出：**1

 

**提示：**

- `1 <= nums.length <= 3 * 104`
- `-3 * 104 <= nums[i] <= 3 * 104`
- 除了某个元素只出现一次以外，其余每个元素均出现两次。

***

```java
class Solution {
    public int singleNumber(int[] nums) {
        int result=0;
        for(int num:nums){
            result^=num;
        }
        return result;
    }
}
```



[141. 环形链表](https://leetcode.cn/problems/linked-list-cycle/)
给你一个链表的头节点 `head` ，判断链表中是否有环。

如果链表中有某个节点，可以通过连续跟踪 `next` 指针再次到达，则链表中存在环。 为了表示给定链表中的环，评测系统内部使用整数 `pos` 来表示链表尾连接到链表中的位置（索引从 0 开始）。**注意：`pos` 不作为参数进行传递** 。仅仅是为了标识链表的实际情况。

*如果链表中存在环* ，则返回 `true` 。 否则，返回 `false` 。

 

**示例 1：**

![img](https://assets.leetcode.cn/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist.png)

```
输入：head = [3,2,0,-4], pos = 1
输出：true
解释：链表中有一个环，其尾部连接到第二个节点。
```

**示例 2：**

![img](https://assets.leetcode.cn/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist_test2.png)

```
输入：head = [1,2], pos = 0
输出：true
解释：链表中有一个环，其尾部连接到第一个节点。
```

**示例 3：**

![img](https://assets.leetcode.cn/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist_test3.png)

```
输入：head = [1], pos = -1
输出：false
解释：链表中没有环。
```

 

**提示：**

- 链表中节点的数目范围是 `[0, 104]`
- `-105 <= Node.val <= 105`
- `pos` 为 `-1` 或者链表中的一个 **有效索引** 。

***

```java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public boolean hasCycle(ListNode head) {
        if(head==null||head.next==null||head.next.next==null){
            return false;
        }
        ListNode fast=head.next.next;
        ListNode slow=head.next;
        while(fast!=slow){
            if(fast.next==null||fast.next.next==null){
                return false;
            }
            fast=fast.next.next;
            slow=slow.next;
        }
        return true;
    }
}
```



[160. 相交链表](https://leetcode.cn/problems/intersection-of-two-linked-lists/)
给你两个单链表的头节点 `headA` 和 `headB` ，请你找出并返回两个单链表相交的起始节点。如果两个链表不存在相交节点，返回 `null` 。

图示两个链表在节点 `c1` 开始相交**：**

[![img](https://assets.leetcode.cn/aliyun-lc-upload/uploads/2018/12/14/160_statement.png)](https://assets.leetcode.cn/aliyun-lc-upload/uploads/2018/12/14/160_statement.png)

题目数据 **保证** 整个链式结构中不存在环。

**注意**，函数返回结果后，链表必须 **保持其原始结构** 。

**自定义评测：**

**评测系统** 的输入如下（你设计的程序 **不适用** 此输入）：

- `intersectVal` - 相交的起始节点的值。如果不存在相交节点，这一值为 `0`
- `listA` - 第一个链表
- `listB` - 第二个链表
- `skipA` - 在 `listA` 中（从头节点开始）跳到交叉节点的节点数
- `skipB` - 在 `listB` 中（从头节点开始）跳到交叉节点的节点数

评测系统将根据这些输入创建链式数据结构，并将两个头节点 `headA` 和 `headB` 传递给你的程序。如果程序能够正确返回相交节点，那么你的解决方案将被 **视作正确答案** 。

 

**示例 1：**

[![img](https://assets.leetcode.com/uploads/2021/03/05/160_example_1_1.png)](https://assets.leetcode.com/uploads/2018/12/13/160_example_1.png)

```
输入：intersectVal = 8, listA = [4,1,8,4,5], listB = [5,6,1,8,4,5], skipA = 2, skipB = 3
输出：Intersected at '8'
解释：相交节点的值为 8 （注意，如果两个链表相交则不能为 0）。
从各自的表头开始算起，链表 A 为 [4,1,8,4,5]，链表 B 为 [5,6,1,8,4,5]。
在 A 中，相交节点前有 2 个节点；在 B 中，相交节点前有 3 个节点。
— 请注意相交节点的值不为 1，因为在链表 A 和链表 B 之中值为 1 的节点 (A 中第二个节点和 B 中第三个节点) 是不同的节点。换句话说，它们在内存中指向两个不同的位置，而链表 A 和链表 B 中值为 8 的节点 (A 中第三个节点，B 中第四个节点) 在内存中指向相同的位置。
```

 

**示例 2：**

[![img](https://assets.leetcode.com/uploads/2021/03/05/160_example_2.png)](https://assets.leetcode.com/uploads/2018/12/13/160_example_2.png)

```
输入：intersectVal = 2, listA = [1,9,1,2,4], listB = [3,2,4], skipA = 3, skipB = 1
输出：Intersected at '2'
解释：相交节点的值为 2 （注意，如果两个链表相交则不能为 0）。
从各自的表头开始算起，链表 A 为 [1,9,1,2,4]，链表 B 为 [3,2,4]。
在 A 中，相交节点前有 3 个节点；在 B 中，相交节点前有 1 个节点。
```

**示例 3：**

[![img](https://assets.leetcode.cn/aliyun-lc-upload/uploads/2018/12/14/160_example_3.png)](https://assets.leetcode.com/uploads/2018/12/13/160_example_3.png)

```
输入：intersectVal = 0, listA = [2,6,4], listB = [1,5], skipA = 3, skipB = 2
输出：No intersection
解释：从各自的表头开始算起，链表 A 为 [2,6,4]，链表 B 为 [1,5]。
由于这两个链表不相交，所以 intersectVal 必须为 0，而 skipA 和 skipB 可以是任意值。
这两个链表不相交，因此返回 null 。
```

 

**提示：**

- `listA` 中节点数目为 `m`
- `listB` 中节点数目为 `n`
- `1 <= m, n <= 3 * 104`
- `1 <= Node.val <= 105`
- `0 <= skipA <= m`
- `0 <= skipB <= n`
- 如果 `listA` 和 `listB` 没有交点，`intersectVal` 为 `0`
- 如果 `listA` 和 `listB` 有交点，`intersectVal == listA[skipA] == listB[skipB]`

***

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        if(headA==null||headB==null){
            return null;
        }
        int d=0;
        ListNode p1=headA;
        while(p1!=null){
            d++;
            p1=p1.next;
        }
        ListNode p2=headB;
        while(p2!=null){
            d--;
            p2=p2.next;
        }
        p1=d>0?headA:headB;
        p2=d>0?headB:headA;
        d=Math.abs(d);
        while(d-->0){
            p1=p1.next;
        }
        while(p1!=p2){
            p1=p1.next;
            p2=p2.next;
        }
        return p1;
    }
}
```



[169. 多数元素](https://leetcode.cn/problems/majority-element/)
给定一个大小为 `n` 的数组 `nums` ，返回其中的多数元素。多数元素是指在数组中出现次数 **大于** `⌊ n/2 ⌋` 的元素。

你可以假设数组是非空的，并且给定的数组总是存在多数元素。

 

**示例 1：**

```
输入：nums = [3,2,3]
输出：3
```

**示例 2：**

```
输入：nums = [2,2,1,1,1,2,2]
输出：2
```

 

**提示：**

- `n == nums.length`
- `1 <= n <= 5 * 104`
- `-109 <= nums[i] <= 109`
- 输入保证数组中一定有一个多数元素。

***

```java
class Solution {
    public int majorityElement(int[] nums) {
        Arrays.sort(nums);
        return nums[nums.length/2];
    }
}
```



[206. 反转链表](https://leetcode.cn/problems/reverse-linked-list/)

给你单链表的头节点 `head` ，请你反转链表，并返回反转后的链表。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/02/19/rev1ex1.jpg)

```
输入：head = [1,2,3,4,5]
输出：[5,4,3,2,1]
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2021/02/19/rev1ex2.jpg)

```
输入：head = [1,2]
输出：[2,1]
```

**示例 3：**

```
输入：head = []
输出：[]
```

***

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode reverseList(ListNode head) {
        if(head==null){
            return null;
        }
        ListNode pre=null;
        ListNode cur=head;
        ListNode next=null;
        while(cur!=null){
            next=cur.next;
            cur.next=pre;
            pre=cur;
            cur=next;
        }
        return pre;
    }
}
```



[226. 翻转二叉树](https://leetcode.cn/problems/invert-binary-tree/)
给你一棵二叉树的根节点 `root` ，翻转这棵二叉树，并返回其根节点。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/03/14/invert1-tree.jpg)

```
输入：root = [4,2,7,1,3,6,9]
输出：[4,7,2,9,6,3,1]
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2021/03/14/invert2-tree.jpg)

```
输入：root = [2,1,3]
输出：[2,3,1]
```

**示例 3：**

```
输入：root = []
输出：[]
```

 

**提示：**

- 树中节点数目范围在 `[0, 100]` 内
- `-100 <= Node.val <= 100`

***

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public TreeNode invertTree(TreeNode root) {
        if(root==null){
            return null;
        }
        TreeNode leftNode=invertTree(root.left);
        TreeNode rightNode=invertTree(root.right);
        root.left=rightNode;
        root.right=leftNode;
        return root;
    }
}
```



[234. 回文链表](https://leetcode.cn/problems/palindrome-linked-list/)
给你一个单链表的头节点 `head` ，请你判断该链表是否为回文链表。如果是，返回 `true` ；否则，返回 `false` 。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/03/03/pal1linked-list.jpg)

```
输入：head = [1,2,2,1]
输出：true
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2021/03/03/pal2linked-list.jpg)

```
输入：head = [1,2]
输出：false
```

 

**提示：**

- 链表中节点数目在范围`[1, 105]` 内
- `0 <= Node.val <= 9`

***

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public boolean isPalindrome(ListNode head) {
        if(head==null){
            return true;
        }
        ListNode fast=head;
        ListNode slow=head;
        while(fast.next!=null&&fast.next.next!=null){
            fast=fast.next.next;
            slow=slow.next;
        }
        ListNode preNode=null;
        ListNode curNode=slow;
        ListNode nextNode=null;
        while(curNode!=null){
            nextNode=curNode.next;
            curNode.next=preNode;
            preNode=curNode;
            curNode=nextNode;
        }
        ListNode p1=head;
        ListNode p2=preNode;
        while(p1!=null&&p2!=null){
            if(p1.val==p2.val){
                p1=p1.next;
                p2=p2.next;
            }else{
                return false;
            }
        }
        return true;
    }
}
```



[283. 移动零](https://leetcode.cn/problems/move-zeroes/)
给定一个数组 `nums`，编写一个函数将所有 `0` 移动到数组的末尾，同时保持非零元素的相对顺序。

**请注意** ，必须在不复制数组的情况下原地对数组进行操作。

 

**示例 1:**

```
输入: nums = [0,1,0,3,12]
输出: [1,3,12,0,0]
```

**示例 2:**

```
输入: nums = [0]
输出: [0]
```

 

**提示**:

- `1 <= nums.length <= 104`
- `-231 <= nums[i] <= 231 - 1`

***

```java
class Solution {
    public void moveZeroes(int[] nums) {
        if(nums==null||nums.length==0){
            return;
        }
        int leftBorder=-1;
        for(int i=0;i<nums.length;i++){
            if(nums[i]!=0){
                leftBorder++;
                int temp=nums[i];
                nums[i]=nums[leftBorder];
                nums[leftBorder]=temp;
            }
        }
    }
}
```



[543. 二叉树的直径](https://leetcode.cn/problems/diameter-of-binary-tree/)
给你一棵二叉树的根节点，返回该树的 **直径** 。

二叉树的 **直径** 是指树中任意两个节点之间最长路径的 **长度** 。这条路径可能经过也可能不经过根节点 `root` 。

两节点之间路径的 **长度** 由它们之间边数表示。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/03/06/diamtree.jpg)

```
输入：root = [1,2,3,4,5]
输出：3
解释：3 ，取路径 [4,2,1,3] 或 [5,2,1,3] 的长度。
```

**示例 2：**

```
输入：root = [1,2]
输出：1
```

 

**提示：**

- 树中节点数目在范围 `[1, 104]` 内
- `-100 <= Node.val <= 100`

***

```java
class Solution {
    public static class CustomType{
        int maxHeight;
        int maxLength;

        public CustomType(int maxHeight,int maxLength){
            this.maxHeight=maxHeight;
            this.maxLength=maxLength;
        }
    }

    public int diameterOfBinaryTree(TreeNode root) {
        if(root==null){
            return 0;
        }
        return processDiameterOfBinaryTree(root).maxLength;
    }

    private CustomType processDiameterOfBinaryTree(TreeNode root){
        if(root==null){
            return new CustomType(0,0);
        }
        CustomType t1=processDiameterOfBinaryTree(root.left);
        CustomType t2=processDiameterOfBinaryTree(root.right);
        int maxHeight=Math.max(t1.maxHeight,t2.maxHeight)+1;
        int maxLength=Math.max(t1.maxHeight+t2.maxHeight,
							Math.max(t1.maxLength,t2.maxLength));
        return new CustomType(maxHeight,maxLength);
    }
}
```

## 中等级别



[2. 两数相加](https://leetcode.cn/problems/add-two-numbers/)
给你两个 **非空** 的链表，表示两个非负的整数。它们每位数字都是按照 **逆序** 的方式存储的，并且每个节点只能存储 **一位** 数字。

请你将两个数相加，并以相同形式返回一个表示和的链表。

你可以假设除了数字 0 之外，这两个数都不会以 0 开头。

 

**示例 1：**

![img](https://assets.leetcode.cn/aliyun-lc-upload/uploads/2021/01/02/addtwonumber1.jpg)

```
输入：l1 = [2,4,3], l2 = [5,6,4]
输出：[7,0,8]
解释：342 + 465 = 807.
```

**示例 2：**

```
输入：l1 = [0], l2 = [0]
输出：[0]
```

**示例 3：**

```
输入：l1 = [9,9,9,9,9,9,9], l2 = [9,9,9,9]
输出：[8,9,9,9,0,0,0,1]
```

 

**提示：**

- 每个链表中的节点数在范围 `[1, 100]` 内
- `0 <= Node.val <= 9`
- 题目数据保证列表表示的数字不含前导零

***

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode addTwoNumbers(ListNode head1, ListNode head2) {
        if(head1==null){
            return head2;
        }
        if(head2==null){
            return head1;
        }
        ListNode head=new ListNode(0);
        ListNode tail=head;
        ListNode p1=head1;
        ListNode p2=head2;
        int carry=0;
        while(p1!=null||p2!=null){
            int v1=p1!=null?p1.val:0;
            int v2=p2!=null?p2.val:0;
            tail.next=new ListNode((v1+v2+carry)%10);
            carry=(v1+v2+carry)/10;
            tail=tail.next;
            p1=p1!=null?p1.next:null;
            p2=p2!=null?p2.next:null;
        }
        if(carry==1){
            tail.next=new ListNode(1);
        }
        return head.next;
    }
}
```



[3. 无重复字符的最长子串](https://leetcode.cn/problems/longest-substring-without-repeating-characters/)
给定一个字符串 `s` ，请你找出其中不含有重复字符的 **最长 子串** 的长度。

 

**示例 1:**

```
输入: s = "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。注意 "bca" 和 "cab" 也是正确答案。
```

**示例 2:**

```
输入: s = "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
```

**示例 3:**

```
输入: s = "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
```

 

**提示：**

- `0 <= s.length <= 105`
- `s` 由英文字母、数字、符号和空格组成

***

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        if(s==null||s.length()==0){
            return 0;
        }
        Map<Character,Integer> map=new HashMap<>();
        int left=0;
        int max=0;
        for(int i=0;i<s.length();i++){
            if(map.containsKey(s.charAt(i))){
                left=Math.max(left,map.get(s.charAt(i))+1);
            }
            map.put(s.charAt(i),i);
            max=Math.max(max,i-left+1);
        }
        return max;
    }
}
```



[5. 最长回文子串](https://leetcode.cn/problems/longest-palindromic-substring/)
给你一个字符串 `s`，找到 `s` 中最长的 回文 子串。

 

**示例 1：**

```
输入：s = "babad"
输出："bab"
解释："aba" 同样是符合题意的答案。
```

**示例 2：**

```
输入：s = "cbbd"
输出："bb"
```

 

**提示：**

- `1 <= s.length <= 1000`
- `s` 仅由数字和英文字母组成

***

```java
class Solution {

    private char[] manacherString(String str){
        char[] result=new char[2*str.length()+1];
        int index=0;
        for(int i=0;i<result.length;i++){
            result[i]=(i%2==0)?'#':str.charAt(index++);
        }
        return result;
    }
    
    public String longestPalindrome(String str) {
        if(str==null||str.length()==0){
            return str;
        }
        char[] chs=manacherString(str);
        int[] dp=new int[chs.length];
        int C=-1;
        int R=-1;
        int max=0;
        int index=-1;
        for(int i=0;i<chs.length;i++){
            dp[i]=i<R?Math.min(dp[2*C-i],R-i):1;
            while(i-dp[i]>=0&&i+dp[i]<chs.length&&chs[i-dp[i]]==chs[i+dp[i]]){
                dp[i]++;
            }
            if(i+dp[i]>R){
                R=i+dp[i];
                C=i;
            }
            if(dp[i]>max){
                max=dp[i];
                index=i;
            }
        }
        int start=(index-max+1)/2;
        return str.substring(start,start+max-1);
    }
}
```



[11. 盛最多水的容器](https://leetcode.cn/problems/container-with-most-water/)
给定一个长度为 `n` 的整数数组 `height` 。有 `n` 条垂线，第 `i` 条线的两个端点是 `(i, 0)` 和 `(i, height[i])` 。

找出其中的两条线，使得它们与 `x` 轴共同构成的容器可以容纳最多的水。

返回容器可以储存的最大水量。

**说明：**你不能倾斜容器。

 

**示例 1：**

![img](https://aliyun-lc-upload.oss-cn-hangzhou.aliyuncs.com/aliyun-lc-upload/uploads/2018/07/25/question_11.jpg)

```
输入：[1,8,6,2,5,4,8,3,7]
输出：49 
解释：图中垂直线代表输入数组 [1,8,6,2,5,4,8,3,7]。在此情况下，容器能够容纳水（表示为蓝色部分）的最大值为 49。
```

**示例 2：**

```
输入：height = [1,1]
输出：1
```

 

**提示：**

- `n == height.length`
- `2 <= n <= 105`
- `0 <= height[i] <= 104`

***

```java
class Solution {
    public int maxArea(int[] height) {
        if(height==null||height.length==0){
            return 0;
        }
        int low=0;
        int high=height.length-1;
        int result=0;
        while(low<high){
            result=Math.max(result,Math.min(height[low],height[high])*(high-low));
            if(height[low]<=height[high]){
                low++;
            }else{
                high--;
            }
        }
        return result;
    }
}
```



[15. 三数之和](https://leetcode.cn/problems/3sum/)

给你一个整数数组 `nums` ，判断是否存在三元组 `[nums[i], nums[j], nums[k]]` 满足 `i != j`、`i != k` 且 `j != k` ，同时还满足 `nums[i] + nums[j] + nums[k] == 0` 。请你返回所有和为 `0` 且不重复的三元组。

**注意：**答案中不可以包含重复的三元组。

 

 

**示例 1：**

```
输入：nums = [-1,0,1,2,-1,-4]
输出：[[-1,-1,2],[-1,0,1]]
解释：
nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0 。
nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0 。
nums[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0 。
不同的三元组是 [-1,0,1] 和 [-1,-1,2] 。
注意，输出的顺序和三元组的顺序并不重要。
```

**示例 2：**

```
输入：nums = [0,1,1]
输出：[]
解释：唯一可能的三元组和不为 0 。
```

**示例 3：**

```
输入：nums = [0,0,0]
输出：[[0,0,0]]
解释：唯一可能的三元组和为 0 。
```

 

**提示：**

- `3 <= nums.length <= 3000`
- `-105 <= nums[i] <= 105`

***

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        if(nums==null||nums.length<3){
            return new ArrayList<>();
        }
        List<List<Integer>> list=new ArrayList<>();
        Arrays.sort(nums);
        for(int k=0;k<nums.length-2;k++){
            if(nums[k]>0){
                break;
            }
            if(k>0&&nums[k]==nums[k-1]){
                continue;
            }
            int i=k+1;
            int j=nums.length-1;
            while(i<j){
                int sum=nums[k]+nums[i]+nums[j];
                if(sum<0){
                    while(i<j&&nums[i]==nums[++i]);
                }else if(sum>0){
                    while(i<j&&nums[j]==nums[--j]);
                }else{
                    list.add(Arrays.asList(nums[k],nums[i],nums[j]));
                    while(i<j&&nums[i]==nums[++i]);
                    while(i<j&&nums[j]==nums[--j]);
                }
            }
        }
        return list;
    }
}
```



[17. 电话号码的字母组合](https://leetcode.cn/problems/letter-combinations-of-a-phone-number/)
给定一个仅包含数字 `2-9` 的字符串，返回所有它能表示的字母组合。答案可以按 **任意顺序** 返回。

给出数字到字母的映射如下（与电话按键相同）。注意 1 不对应任何字母。

![img](https://pic.leetcode.cn/1752723054-mfIHZs-image.png)

 

**示例 1：**

```
输入：digits = "23"
输出：["ad","ae","af","bd","be","bf","cd","ce","cf"]
```

**示例 2：**

```
输入：digits = "2"
输出：["a","b","c"]
```

 

**提示：**

- `1 <= digits.length <= 4`
- `digits[i]` 是范围 `['2', '9']` 的一个数字。

***

```java
class Solution {
    private List<String> resultList=new ArrayList<>();
    private StringBuilder sb=new StringBuilder();
    private Map<Character,List<Character>> map=new HashMap<>(){{
        put('2',Arrays.asList('a','b','c'));
        put('3',Arrays.asList('d','e','f'));
        put('4',Arrays.asList('g','h','i'));
        put('5',Arrays.asList('j','k','l'));
        put('6',Arrays.asList('m','n','o'));
        put('7',Arrays.asList('p','q','r','s'));
        put('8',Arrays.asList('t','u','v'));
        put('9',Arrays.asList('w','x','y','z'));     
    }};

    public List<String> letterCombinations(String digits) {
        if(digits==null||digits.length()==0){
            return new ArrayList<>();
        }
        processLetterCombinations(digits.toCharArray(),0);
        return resultList;
    }
    
    private void processLetterCombinations(char[] chs,int index){
        if(index==chs.length){
            resultList.add(sb.toString());
            return;
        }
        for(Character c:map.get(chs[index])){
            sb.append(c);
            processLetterCombinations(chs,index+1);
            sb.deleteCharAt(sb.length()-1);
        }
    }

    
}
```



[19. 删除链表的倒数第 N 个结点](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/)

给你一个链表，删除链表的倒数第 `n` 个结点，并且返回链表的头结点。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/10/03/remove_ex1.jpg)

```
输入：head = [1,2,3,4,5], n = 2
输出：[1,2,3,5]
```

**示例 2：**

```
输入：head = [1], n = 1
输出：[]
```

**示例 3：**

```
输入：head = [1,2], n = 1
输出：[1]
```

 

**提示：**

- 链表中结点的数目为 `sz`
- `1 <= sz <= 30`
- `0 <= Node.val <= 100`
- `1 <= n <= sz`

***

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        if(head==null){
            return null;
        }
        ListNode newHead=new ListNode(0);
        newHead.next=head;
        ListNode p1=newHead;
        ListNode p2=newHead;
        while(n-->=0){
            p1=p1.next;
        }
        while(p1!=null){
            p1=p1.next;
            p2=p2.next;
        }
        p2.next=p2.next.next;
        return newHead.next;
    }
}
```



[22. 括号生成](https://leetcode.cn/problems/generate-parentheses/)
数字 `n` 代表生成括号的对数，请你设计一个函数，用于能够生成所有可能的并且 **有效的** 括号组合。

 

**示例 1：**

```
输入：n = 3
输出：["((()))","(()())","(())()","()(())","()()()"]
```

**示例 2：**

```
输入：n = 1
输出：["()"]
```

 

**提示：**

- `1 <= n <= 8`

***

```java
class Solution {
    private List<String> resultList=new ArrayList<>();
    private StringBuilder sb=new StringBuilder();

    public List<String> generateParenthesis(int n) {
        if(n==0){
            return new ArrayList<>();
        }
        processGenerateParenthesis(n,0);
        return resultList;
    }

    private void processGenerateParenthesis(int rest, int leftMore){
        if(rest==0){
            resultList.add(sb.toString());
            return;
        }
        if(leftMore<rest){
            sb.append("(");
            processGenerateParenthesis(rest,leftMore+1);
            sb.deleteCharAt(sb.length()-1);
        }
        if(leftMore>0){
            sb.append(")");
            processGenerateParenthesis(rest-1,leftMore-1);
            sb.deleteCharAt(sb.length()-1);
        }
    }
}
```



[24. 两两交换链表中的节点](https://leetcode.cn/problems/swap-nodes-in-pairs/)
给你一个链表，两两交换其中相邻的节点，并返回交换后链表的头节点。你必须在不修改节点内部的值的情况下完成本题（即，只能进行节点交换）。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/10/03/swap_ex1.jpg)

```
输入：head = [1,2,3,4]
输出：[2,1,4,3]
```

**示例 2：**

```
输入：head = []
输出：[]
```

**示例 3：**

```
输入：head = [1]
输出：[1]
```

 

**提示：**

- 链表中节点的数目在范围 `[0, 100]` 内
- `0 <= Node.val <= 100`

***

```java
class Solution {
    public ListNode swapPairs(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        ListNode newHead = head.next;
        head.next = swapPairs(newHead.next);
        newHead.next = head;
        return newHead;
    }
}
```



[31. 下一个排列](https://leetcode.cn/problems/next-permutation/)
整数数组的一个 **排列** 就是将其所有成员以序列或线性顺序排列。

- 例如，`arr = [1,2,3]` ，以下这些都可以视作 `arr` 的排列：`[1,2,3]`、`[1,3,2]`、`[3,1,2]`、`[2,3,1]` 。

整数数组的 **下一个排列** 是指其整数的下一个字典序更大的排列。更正式地，如果数组的所有排列根据其字典顺序从小到大排列在一个容器中，那么数组的 **下一个排列** 就是在这个有序容器中排在它后面的那个排列。如果不存在下一个更大的排列，那么这个数组必须重排为字典序最小的排列（即，其元素按升序排列）。

- 例如，`arr = [1,2,3]` 的下一个排列是 `[1,3,2]` 。
- 类似地，`arr = [2,3,1]` 的下一个排列是 `[3,1,2]` 。
- 而 `arr = [3,2,1]` 的下一个排列是 `[1,2,3]` ，因为 `[3,2,1]` 不存在一个字典序更大的排列。

给你一个整数数组 `nums` ，找出 `nums` 的下一个排列。

必须**[ 原地 ](https://baike.baidu.com/item/原地算法)**修改，只允许使用额外常数空间。

 

**示例 1：**

```
输入：nums = [1,2,3]
输出：[1,3,2]
```

**示例 2：**

```
输入：nums = [3,2,1]
输出：[1,2,3]
```

**示例 3：**

```
输入：nums = [1,1,5]
输出：[1,5,1]
```

 

**提示：**

- `1 <= nums.length <= 100`
- `0 <= nums[i] <= 100`

***

```java
class Solution {
    public void nextPermutation(int[] nums) {
        if(nums==null||nums.length<2){
            return;
        }
        int i=nums.length-2;
        while(i>=0&&nums[i]>=nums[i+1]){
            i--;
        }
        if(i>=0){
            int j=nums.length-1;
            while(nums[j]<=nums[i]){
                j--;
            }
            swap(nums,i,j);
        }
        reverse(nums,i+1);
    }

    private void swap(int[] nums,int i,int j){
        int temp=nums[i];
        nums[i]=nums[j];
        nums[j]=temp;
    }

    private void reverse(int[] nums,int index){
        int start=index;
        int end=nums.length-1;
        while(start<end){
            swap(nums,start++,end--);
        }
    }
}
```



[33. 搜索旋转排序数组](https://leetcode.cn/problems/search-in-rotated-sorted-array/)
整数数组 `nums` 按升序排列，数组中的值 **互不相同** 。

在传递给函数之前，`nums` 在预先未知的某个下标 `k`（`0 <= k < nums.length`）上进行了 **向左旋转**，使数组变为 `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]`（下标 **从 0 开始** 计数）。例如， `[0,1,2,4,5,6,7]` 下标 `3` 上向左旋转后可能变为 `[4,5,6,7,0,1,2]` 。

给你 **旋转后** 的数组 `nums` 和一个整数 `target` ，如果 `nums` 中存在这个目标值 `target` ，则返回它的下标，否则返回 `-1` 。

你必须设计一个时间复杂度为 `O(log n)` 的算法解决此问题。

 

**示例 1：**

```
输入：nums = [4,5,6,7,0,1,2], target = 0
输出：4
```

**示例 2：**

```
输入：nums = [4,5,6,7,0,1,2], target = 3
输出：-1
```

**示例 3：**

```
输入：nums = [1], target = 0
输出：-1
```

 

**提示：**

- `1 <= nums.length <= 5000`
- `-104 <= nums[i] <= 104`
- `nums` 中的每个值都 **独一无二**
- 题目数据保证 `nums` 在预先未知的某个下标上进行了旋转
- `-104 <= target <= 104`

***

```java
class Solution {
    public int search(int[] nums, int target) {
        if(nums==null||nums.length==0){
            return -1;
        }
        int low=0;
        int high=nums.length-1;
        while(low<=high){
            int mid=(low+high)>>1;
            if(nums[mid]<target){
                if(target>nums[high]&&nums[mid]<=nums[high]){
                    high=mid-1;
                }else{
                    low=mid+1;
                }
            }else if(nums[mid]>target){
                if(target<=nums[high]&&nums[mid]>nums[high]){
                    low=mid+1;
                }else{
                    high=mid-1;
                }
            }else{
                return mid;
            }
        }
        return -1;
    }
}
```



[34. 在排序数组中查找元素的第一个和最后一个位置](https://leetcode.cn/problems/find-first-and-last-position-of-element-in-sorted-array/)
给你一个按照非递减顺序排列的整数数组 `nums`，和一个目标值 `target`。请你找出给定目标值在数组中的开始位置和结束位置。

如果数组中不存在目标值 `target`，返回 `[-1, -1]`。

你必须设计并实现时间复杂度为 `O(log n)` 的算法解决此问题。

 

**示例 1：**

```
输入：nums = [5,7,7,8,8,10], target = 8
输出：[3,4]
```

**示例 2：**

```
输入：nums = [5,7,7,8,8,10], target = 6
输出：[-1,-1]
```

**示例 3：**

```
输入：nums = [], target = 0
输出：[-1,-1]
```

 

**提示：**

- `0 <= nums.length <= 105`
- `-109 <= nums[i] <= 109`
- `nums` 是一个非递减数组
- `-109 <= target <= 109`

***

```java
class Solution {
    public int[] searchRange(int[] nums, int target) {
        if(nums==null||nums.length==0){
            return new int[]{-1,-1};
        }
        int low=0;
        int high=nums.length-1;
        int leftBorder=-1;
        while(low<=high){
            int mid=(low+high)>>1;
            if(nums[mid]>target){
                high=mid-1;
            }else if(nums[mid]<target){
                low=mid+1;
            }else{
                leftBorder=mid;
                high=mid-1;
            }
        }
        if(leftBorder==-1){
            return new int[]{-1,-1};
        }
        low=0;
        high=nums.length-1;
        int rightBorder=-1;
        while(low<=high){
            int mid=(low+high)>>1;
            if(nums[mid]>target){
                high=mid-1;
            }else if(nums[mid]<target){
                low=mid+1;
            }else{
                rightBorder=mid;
                low=mid+1;
            }
        }
        return new int[]{leftBorder,rightBorder};
    }
}
```



[39. 组合总和](https://leetcode.cn/problems/combination-sum/)
给你一个 **无重复元素** 的整数数组 `candidates` 和一个目标整数 `target` ，找出 `candidates` 中可以使数字和为目标数 `target` 的 所有 **不同组合** ，并以列表形式返回。你可以按 **任意顺序** 返回这些组合。

`candidates` 中的 **同一个** 数字可以 **无限制重复被选取** 。如果至少一个数字的被选数量不同，则两种组合是不同的。 

对于给定的输入，保证和为 `target` 的不同组合数少于 `150` 个。

 

**示例 1：**

```
输入：candidates = [2,3,6,7], target = 7
输出：[[2,2,3],[7]]
解释：
2 和 3 可以形成一组候选，2 + 2 + 3 = 7 。注意 2 可以使用多次。
7 也是一个候选， 7 = 7 。
仅有这两种组合。
```

**示例 2：**

```
输入: candidates = [2,3,5], target = 8
输出: [[2,2,2,2],[2,3,3],[3,5]]
```

**示例 3：**

```
输入: candidates = [2], target = 1
输出: []
```

 

**提示：**

- `1 <= candidates.length <= 30`
- `2 <= candidates[i] <= 40`
- `candidates` 的所有元素 **互不相同**
- `1 <= target <= 40`

***

```java
class Solution {
    private List<List<Integer>> resultList=new ArrayList<>();
    private List<Integer> tempList=new ArrayList<>();

    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        if(candidates==null||candidates.length==0){
            return new ArrayList<>();
        }
        processCombinationSum(candidates,0,target);
        return resultList;
    }

    private void processCombinationSum(int[] candidates,int index,int rest){
         if(rest==0){
            resultList.add(new ArrayList<>(tempList));
            return;
        }
        if(index==candidates.length){
            return;
        }
        processCombinationSum(candidates,index+1,rest);
        for(int i=1;candidates[index]*i<=rest;i++){
            tempList.add(candidates[index]);
            processCombinationSum(candidates,index+1,rest-candidates[index]*i);
        }
        tempList=tempList.stream().filter(x->x!=candidates[index]).collect(Collectors.toList());
    }
}
```



[45. 跳跃游戏 II](https://leetcode.cn/problems/jump-game-ii/)
给定一个长度为 `n` 的 **0 索引**整数数组 `nums`。初始位置在下标 0。

每个元素 `nums[i]` 表示从索引 `i` 向后跳转的最大长度。换句话说，如果你在索引 `i` 处，你可以跳转到任意 `(i + j)` 处：

- `0 <= j <= nums[i]` 且
- `i + j < n`

返回到达 `n - 1` 的最小跳跃次数。测试用例保证可以到达 `n - 1`。

 

**示例 1:**

```
输入: nums = [2,3,1,1,4]
输出: 2
解释: 跳到最后一个位置的最小跳跃数是 2。
     从下标为 0 跳到下标为 1 的位置，跳 1 步，然后跳 3 步到达数组的最后一个位置。
```

**示例 2:**

```
输入: nums = [2,3,0,1,4]
输出: 2
```

 

**提示:**

- `1 <= nums.length <= 104`
- `0 <= nums[i] <= 1000`
- 题目保证可以到达 `n - 1`

***

```java
class Solution {
    public int jump(int[] nums) {
        if(nums==null||nums.length<=1){
            return 0;
        }
        int[] dp=new int[nums.length];
        for(int i=nums.length-2;i>=0;i--){
            int min=Integer.MAX_VALUE;
            for(int j=1;j<=Math.min(nums[i],nums.length-i-1);j++){
                if(dp[i+j]==-1){
                    continue;
                }
                min=Math.min(min,dp[i+j]);
            }
            dp[i]=min==Integer.MAX_VALUE?-1:min+1;
        }
        return dp[0];
    }
}
```



[46. 全排列](https://leetcode.cn/problems/permutations/)
给定一个不含重复数字的数组 `nums` ，返回其 *所有可能的全排列* 。你可以 **按任意顺序** 返回答案。

 

**示例 1：**

```
输入：nums = [1,2,3]
输出：[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```

**示例 2：**

```
输入：nums = [0,1]
输出：[[0,1],[1,0]]
```

**示例 3：**

```
输入：nums = [1]
输出：[[1]]
```

 

**提示：**

- `1 <= nums.length <= 6`
- `-10 <= nums[i] <= 10`
- `nums` 中的所有整数 **互不相同**

***

```java
class Solution {

    List<List<Integer>> resultList=new ArrayList<>();
    List<Integer> tempList=new ArrayList<>();

    public List<List<Integer>> permute(int[] nums) {
        if(nums==null||nums.length==0){
            return resultList;
        }
        processPermute(nums,0);
        return resultList;
    }

    private void processPermute(int[] nums, int index){
        if(index==nums.length){
            resultList.add(new ArrayList<>(tempList));
            return;
        }
        for(int i=index;i<nums.length;i++){
            tempList.add(nums[i]);
            swap(nums,index,i);
            processPermute(nums,index+1);
            tempList.remove(tempList.size()-1);
            swap(nums,index,i);
        }
    }

    private void swap(int[] nums,int i,int j){
        int temp=nums[i];
        nums[i]=nums[j];
        nums[j]=temp;
    }
}
```



[48. 旋转图像](https://leetcode.cn/problems/rotate-image/)
给定一个 *n* × *n* 的二维矩阵 `matrix` 表示一个图像。请你将图像顺时针旋转 90 度。

你必须在**[ 原地](https://baike.baidu.com/item/原地算法)** 旋转图像，这意味着你需要直接修改输入的二维矩阵。**请不要** 使用另一个矩阵来旋转图像。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/08/28/mat1.jpg)

```
输入：matrix = [[1,2,3],[4,5,6],[7,8,9]]
输出：[[7,4,1],[8,5,2],[9,6,3]]
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2020/08/28/mat2.jpg)

```
输入：matrix = [[5,1,9,11],[2,4,8,10],[13,3,6,7],[15,14,12,16]]
输出：[[15,13,2,5],[14,3,4,1],[12,6,8,9],[16,7,10,11]]
```

 

**提示：**

- `n == matrix.length == matrix[i].length`
- `1 <= n <= 20`
- `-1000 <= matrix[i][j] <= 1000`

***

```java
class Solution {
    public void rotate(int[][] matrix) {
        if(matrix==null||matrix.length==0||matrix[0].length==0){
            return;
        }
        int oRow=0;
        int oCol=0;
        int dRow=matrix.length-1;
        int dCol=matrix[0].length-1;
        while(oRow<dRow){
            processRotate(matrix,oRow++,oCol++,dRow--,dCol--);
        }
    }

    private void processRotate(int[][] matrix,int oRow, int oCol, int dRow, int dCol){
        int distance=dRow-oRow;
        for(int i=0;i<distance;i++){
            int temp=matrix[oRow][oCol+i];
            matrix[oRow][oCol+i]=matrix[dRow-i][oCol];
            matrix[dRow-i][oCol]=matrix[dRow][dCol-i];
            matrix[dRow][dCol-i]=matrix[oRow+i][dCol];
            matrix[oRow+i][dCol]=temp;
        }
    }
}
```



[49. 字母异位词分组](https://leetcode.cn/problems/group-anagrams/)
给你一个字符串数组，请你将 字母异位词 组合在一起。可以按任意顺序返回结果列表。

 

**示例 1:**

**输入:** strs = ["eat", "tea", "tan", "ate", "nat", "bat"]

**输出:** [["bat"],["nat","tan"],["ate","eat","tea"]]

**解释：**

- 在 strs 中没有字符串可以通过重新排列来形成 `"bat"`。
- 字符串 `"nat"` 和 `"tan"` 是字母异位词，因为它们可以重新排列以形成彼此。
- 字符串 `"ate"` ，`"eat"` 和 `"tea"` 是字母异位词，因为它们可以重新排列以形成彼此。

**示例 2:**

**输入:** strs = [""]

**输出:** [[""]]

**示例 3:**

**输入:** strs = ["a"]

**输出:** [["a"]]

 

**提示：**

- `1 <= strs.length <= 104`
- `0 <= strs[i].length <= 100`
- `strs[i]` 仅包含小写字母

***

```java
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        Map<String,List<String>> map = Arrays.stream(strs).collect(Collectors.groupingBy(s->{
            char[] chs=s.toCharArray();
            Arrays.sort(chs);
            return new String(chs);
        }));
        return new ArrayList<>(map.values());
    }
}
```



[53. 最大子数组和](https://leetcode.cn/problems/maximum-subarray/)
给你一个整数数组 `nums` ，请你找出一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

**子数组**是数组中的一个连续部分。

 

**示例 1：**

```
输入：nums = [-2,1,-3,4,-1,2,1,-5,4]
输出：6
解释：连续子数组 [4,-1,2,1] 的和最大，为 6 。
```

**示例 2：**

```
输入：nums = [1]
输出：1
```

**示例 3：**

```
输入：nums = [5,4,-1,7,8]
输出：23
```

 

**提示：**

- `1 <= nums.length <= 105`
- `-104 <= nums[i] <= 104`

***

```java
class Solution {
    public int maxSubArray(int[] nums) {
        if(nums==null||nums.length==0){
            return 0;
        }
        int[] dp=new int[nums.length];
        dp[0]=nums[0];
        int max=nums[0];
        for(int i=1;i<nums.length;i++){
            dp[i]=Math.max(dp[i-1]+nums[i],nums[i]);
            max=Math.max(max,dp[i]);
        }
        return max;
    }
}
```



[54. 螺旋矩阵](https://leetcode.cn/problems/spiral-matrix/)
给你一个 `m` 行 `n` 列的矩阵 `matrix` ，请按照 **顺时针螺旋顺序** ，返回矩阵中的所有元素。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/11/13/spiral1.jpg)

```
输入：matrix = [[1,2,3],[4,5,6],[7,8,9]]
输出：[1,2,3,6,9,8,7,4,5]
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2020/11/13/spiral.jpg)

```
输入：matrix = [[1,2,3,4],[5,6,7,8],[9,10,11,12]]
输出：[1,2,3,4,8,12,11,10,9,5,6,7]
```

 

**提示：**

- `m == matrix.length`
- `n == matrix[i].length`
- `1 <= m, n <= 10`
- `-100 <= matrix[i][j] <= 100`

***

```java
class Solution {

    private List<Integer> resultList=new ArrayList<>();
    public List<Integer> spiralOrder(int[][] matrix) {
        if(matrix==null||matrix.length==0||matrix[0].length==0){
            return resultList;
        }
        int m=matrix.length;
        int n=matrix[0].length;
        int x1=0;
        int y1=0;
        int x2=m-1;
        int y2=n-1;
        while(x1<=x2&&y1<=y2){
            processSpiralOrder(matrix,x1++,y1++,x2--,y2--);
        }
        return resultList;
    }

    private void processSpiralOrder(int[][] matrix,int x1,int y1,int x2,int y2){
        if(x1==x2){
            for(int i=y1;i<=y2;i++){
                resultList.add(matrix[x1][i]);
            }
        }else if(y1==y2){
            for(int i=x1;i<=x2;i++){
                resultList.add(matrix[i][y1]);
            }
        }else{
            for(int i=y1;i<y2;i++){
                resultList.add(matrix[x1][i]);
            }
            for(int i=x1;i<x2;i++){
                resultList.add(matrix[i][y2]);
            }
            for(int i=y2;i>y1;i--){
                resultList.add(matrix[x2][i]);
            }
            for(int i=x2;i>x1;i--){
                resultList.add(matrix[i][y1]);
            }
        }
    }
}
```



[55. 跳跃游戏](https://leetcode.cn/problems/jump-game/)
给你一个非负整数数组 `nums` ，你最初位于数组的 **第一个下标** 。数组中的每个元素代表你在该位置可以跳跃的最大长度。

判断你是否能够到达最后一个下标，如果可以，返回 `true` ；否则，返回 `false` 。

 

**示例 1：**

```
输入：nums = [2,3,1,1,4]
输出：true
解释：可以先跳 1 步，从下标 0 到达下标 1, 然后再从下标 1 跳 3 步到达最后一个下标。
```

**示例 2：**

```
输入：nums = [3,2,1,0,4]
输出：false
解释：无论怎样，总会到达下标为 3 的位置。但该下标的最大跳跃长度是 0 ， 所以永远不可能到达最后一个下标。
```

 

**提示：**

- `1 <= nums.length <= 104`
- `0 <= nums[i] <= 105`

***

```java
class Solution {
    public boolean canJump(int[] nums) {
        if(nums==null||nums.length==0){
            return false;
        }
        int n=nums.length;
        if(n==1){
            return true;
        }
        boolean[] dp=new boolean[n];
        dp[n-1]=true;
        for(int i=n-2;i>=0;i--){
            boolean result=false;
            for(int j=1;j<=Math.min(n-i-1,nums[i]);j++){
                if(dp[i+j]){
                    result=true;
                    break;
                }
            }
            dp[i]=result;
        }
        return dp[0];
    }
}
```



[56. 合并区间](https://leetcode.cn/problems/merge-intervals/)
以数组 `intervals` 表示若干个区间的集合，其中单个区间为 `intervals[i] = [starti, endi]` 。请你合并所有重叠的区间，并返回 *一个不重叠的区间数组，该数组需恰好覆盖输入中的所有区间* 。

 

**示例 1：**

```
输入：intervals = [[1,3],[2,6],[8,10],[15,18]]
输出：[[1,6],[8,10],[15,18]]
解释：区间 [1,3] 和 [2,6] 重叠, 将它们合并为 [1,6].
```

**示例 2：**

```
输入：intervals = [[1,4],[4,5]]
输出：[[1,5]]
解释：区间 [1,4] 和 [4,5] 可被视为重叠区间。
```

**示例 3：**

```
输入：intervals = [[4,7],[1,4]]
输出：[[1,7]]
解释：区间 [1,4] 和 [4,7] 可被视为重叠区间。
```

 

**提示：**

- `1 <= intervals.length <= 104`
- `intervals[i].length == 2`
- `0 <= starti <= endi <= 104`

***

```java
class Solution {
    public int[][] merge(int[][] nums) {
        if(nums==null||nums.length==0||nums[0].length==0){
            return new int[][]{};
        }
        int M=nums.length;
        int N=nums[0].length;
        List<int[]> resultList=new ArrayList<>();
        Arrays.sort(nums,(o1,o2)->{return o1[0]-o2[0];});
        int start=nums[0][0];
        int end=nums[0][1];
        for(int i=1;i<M;i++){
            int n1=nums[i][0];
            int n2=nums[i][1];
            if(n1<=end){
                end=Math.max(end,n2);
            }else{
                resultList.add(new int[]{start,end});
                start=n1;
                end=n2;
            }
        }
        resultList.add(new int[]{start,end});
        int[][] result=new int[resultList.size()][2];
        for(int i=0;i<result.length;i++){
            result[i]=resultList.get(i);
        }

        return result;
    }
}
```



[62. 不同路径](https://leetcode.cn/problems/unique-paths/)
一个机器人位于一个 `m x n` 网格的左上角 （起始点在下图中标记为 “Start” ）。

机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为 “Finish” ）。

问总共有多少条不同的路径？

 

**示例 1：**

![img](https://pic.leetcode.cn/1697422740-adxmsI-image.png)

```
输入：m = 3, n = 7
输出：28
```

**示例 2：**

```
输入：m = 3, n = 2
输出：3
解释：
从左上角开始，总共有 3 条路径可以到达右下角。
1. 向右 -> 向下 -> 向下
2. 向下 -> 向下 -> 向右
3. 向下 -> 向右 -> 向下
```

**示例 3：**

```
输入：m = 7, n = 3
输出：28
```

**示例 4：**

```
输入：m = 3, n = 3
输出：6
```

 

**提示：**

- `1 <= m, n <= 100`
- 题目数据保证答案小于等于 `2 * 109`

***

```java
class Solution {
    public int uniquePaths(int m, int n) {
        if(m<=0||n<=0){
            return 0;
        }
        int[][] dp=new int[m][n];
        for(int i=0;i<n;i++){
            dp[m-1][i]=1;
        }
        for(int i=0;i<m;i++){
            dp[i][n-1]=1;
        }
        for(int i=m-2;i>=0;i--){
            for(int j=n-2;j>=0;j--){
                dp[i][j]=dp[i+1][j]+dp[i][j+1];
            }
        }
        return dp[0][0];
    }
}
```



[64. 最小路径和](https://leetcode.cn/problems/minimum-path-sum/)
给定一个包含非负整数的 `*m* x *n*` 网格 `grid` ，请找出一条从左上角到右下角的路径，使得路径上的数字总和为最小。

**说明：**每次只能向下或者向右移动一步。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/11/05/minpath.jpg)

```
输入：grid = [[1,3,1],[1,5,1],[4,2,1]]
输出：7
解释：因为路径 1→3→1→1→1 的总和最小。
```

**示例 2：**

```
输入：grid = [[1,2,3],[4,5,6]]
输出：12
```

 

**提示：**

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 200`
- `0 <= grid[i][j] <= 200`

***

```java
class Solution {
    public int minPathSum(int[][] grid) {
        if(grid==null||grid.length==0||grid[0].length==0){
            return 0;
        }
        int M=grid.length;
        int N=grid[0].length;
        int[][] dp=new int[M][N];
        dp[M-1][N-1]=grid[M-1][N-1];   
        for(int i=N-2;i>=0;i--){
            dp[M-1][i]=dp[M-1][i+1]+grid[M-1][i];
        }
        for(int i=M-2;i>=0;i--){
            dp[i][N-1]=dp[i+1][N-1]+grid[i][N-1];
        }
        for(int i=M-2;i>=0;i--){
            for(int j=N-2;j>=0;j--){
                dp[i][j]=Math.min(dp[i+1][j],dp[i][j+1])+grid[i][j];
            }
        }
        return dp[0][0];
    }
}
```



[72. 编辑距离](https://leetcode.cn/problems/edit-distance/)
给你两个单词 `word1` 和 `word2`， *请返回将 `word1` 转换成 `word2` 所使用的最少操作数* 。

你可以对一个单词进行如下三种操作：

- 插入一个字符
- 删除一个字符
- 替换一个字符

 

**示例 1：**

```
输入：word1 = "horse", word2 = "ros"
输出：3
解释：
horse -> rorse (将 'h' 替换为 'r')
rorse -> rose (删除 'r')
rose -> ros (删除 'e')
```

**示例 2：**

```
输入：word1 = "intention", word2 = "execution"
输出：5
解释：
intention -> inention (删除 't')
inention -> enention (将 'i' 替换为 'e')
enention -> exention (将 'n' 替换为 'x')
exention -> exection (将 'n' 替换为 'c')
exection -> execution (插入 'u')
```

***

```java
class Solution {
    public int minDistance(String word1, String word2) {
        int len1=word1.length();
        int len2=word2.length();
        int[][] dp=new int[len1+1][len2+1];
        for(int i=0;i<=len1;i++){
            for(int j=0;j<=len2;j++){
                if(i==0||j==0){
                    dp[i][j]=Math.max(i,j);
                }else{
                    if(word1.charAt(i-1)==word2.charAt(j-1)){
                        dp[i][j]=dp[i-1][j-1];
                    }else{
                        dp[i][j]=Math.min(dp[i-1][j-1],Math.min(dp[i][j-1],dp[i-1][j]))+1;
                    }
                }
            }
        }
        return dp[len1][len2];
    }
}
```



[75. 颜色分类](https://leetcode.cn/problems/sort-colors/)

给定一个包含红色、白色和蓝色、共 `n` 个元素的数组 `nums` ，**[原地](https://baike.baidu.com/item/原地算法)** 对它们进行排序，使得相同颜色的元素相邻，并按照红色、白色、蓝色顺序排列。

我们使用整数 0、 1 和 2 分别表示红色、白色和蓝色。

必须在不使用库内置的 sort 函数的情况下解决这个问题。

 

**示例 1：**

**输入：**nums = [2,0,2,1,1,0]

**输出：**[0,0,1,1,2,2]

**解释：**

该数组包含两个 0、两个 1 和两个 2。将它们原地排序后，所有 0 排在最前面，接着是所有 1，最后是所有 2。

**示例 2：**

**输入：**nums = [2,0,1]

**输出：**[0,1,2]

**解释：**

数组中有且仅有一个 0、一个 1 和一个 2，按 0、1、2 的顺序原地排列。

 

**提示：**

- `n == nums.length`
- `1 <= n <= 300`
- `nums[i]` 为 0、1 或 2

 

**进阶：**

- 你能想出一个仅使用常数空间的一趟扫描算法吗？

***

```java
class Solution {
    public void sortColors(int[] nums) {
        if(nums==null||nums.length==0){
            return;
        }
        int leftBorder=-1;
        int rightBorder=nums.length;
        int index=0;
        while(index<rightBorder){
            if(nums[index]==0){
                swap(nums,index++,++leftBorder);
            }else if(nums[index]==1){
                index++;
            }else{
                swap(nums,index,--rightBorder);
            }
        }
    }

    private void swap(int[] nums,int i,int j){
        int temp=nums[i];
        nums[i]=nums[j];
        nums[j]=temp;
    }
}
```



[78. 子集](https://leetcode.cn/problems/subsets/)
给你一个整数数组 `nums` ，数组中的元素 **互不相同** 。返回该数组所有可能的子集（幂集）。

解集 **不能** 包含重复的子集。你可以按 **任意顺序** 返回解集。

 

**示例 1：**

```
输入：nums = [1,2,3]
输出：[[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]
```

**示例 2：**

```
输入：nums = [0]
输出：[[],[0]]
```

 

**提示：**

- `1 <= nums.length <= 10`
- `-10 <= nums[i] <= 10`
- `nums` 中的所有元素 **互不相同**

***

```java
class Solution {
    private List<List<Integer>> resultList=new ArrayList<>();
    private List<Integer> tempList=new ArrayList();
    public List<List<Integer>> subsets(int[] nums) {
        if(nums==null||nums.length==0){
            return new ArrayList<>();
        }
        processSubsets(nums,0);
        return resultList;
    }

    private void  processSubsets(int[] nums,int index){
        if(index==nums.length){
            resultList.add(new ArrayList<>(tempList));
            return;
        }
        processSubsets(nums,index+1);
        tempList.add(nums[index]);
        processSubsets(nums,index+1);
        tempList.remove(tempList.size()-1);
    }

}
```



[79. 单词搜索](https://leetcode.cn/problems/word-search/)
给定一个 `m x n` 二维字符网格 `board` 和一个字符串单词 `word` 。如果 `word` 存在于网格中，返回 `true` ；否则，返回 `false` 。

单词必须按照字母顺序，通过相邻的单元格内的字母构成，其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母不允许被重复使用。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/11/04/word2.jpg)

```
输入：board = [['A','B','C','E'],['S','F','C','S'],['A','D','E','E']], word = "ABCCED"
输出：true
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2020/11/04/word-1.jpg)

```
输入：board = [['A','B','C','E'],['S','F','C','S'],['A','D','E','E']], word = "SEE"
输出：true
```

**示例 3：**

![img](https://assets.leetcode.com/uploads/2020/10/15/word3.jpg)

```
输入：board = [['A','B','C','E'],['S','F','C','S'],['A','D','E','E']], word = "ABCB"
输出：false
```

 

**提示：**

- `m == board.length`
- `n = board[i].length`
- `1 <= m, n <= 6`
- `1 <= word.length <= 15`
- `board` 和 `word` 仅由大小写英文字母组成

***

```java
class Solution {

    private boolean[][] visit;

    public boolean exist(char[][] board, String word) {
        if(board==null||board.length==0||board[0].length==0||word==null){
            return false;
        }
        int M=board.length;
        int N=board[0].length;
        visit=new boolean[M][N];
        boolean result=false;
        for(int i=0;i<M;i++){
            for(int j=0;j<N;j++){
                result|=processExist(board,word,i,j,0);
            }
        }
        return result;
    }

    private boolean processExist(char[][] board, String word, int x, int y, int index){
         if(x<0||x>=board.length||y<0||y>=board[0].length
            ||visit[x][y]||word.charAt(index)!=board[x][y]){
            return false;
        }
        if(index==word.length()-1){
            return true;
        }
        visit[x][y]=true;
        boolean result=false;
        result|=processExist(board,word,x+1,y,index+1);
        result|=processExist(board,word,x-1,y,index+1);
        result|=processExist(board,word,x,y+1,index+1);
        result|=processExist(board,word,x,y-1,index+1);
        visit[x][y]=false;
        return result;
    }
}
```



[98. 验证二叉搜索树](https://leetcode.cn/problems/validate-binary-search-tree/)
给你一个二叉树的根节点 `root` ，判断其是否是一个有效的二叉搜索树。

**有效** 二叉搜索树定义如下：

- 节点的左子树只包含 **严格小于** 当前节点的数。
- 节点的右子树只包含 **严格大于** 当前节点的数。
- 所有左子树和右子树自身必须也是二叉搜索树。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/12/01/tree1.jpg)

```
输入：root = [2,1,3]
输出：true
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2020/12/01/tree2.jpg)

```
输入：root = [5,1,4,null,null,3,6]
输出：false
解释：根节点的值是 5 ，但是右子节点的值是 4 。
```

 

**提示：**

- 树中节点数目范围在`[1, 104]` 内
- `-231 <= Node.val <= 231 - 1`

***

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {

    public static class BstType{
        boolean bst;
        long minValue;
        long maxValue;

        public BstType(boolean bst, long minValue, long maxValue) {
            this.bst = bst;
            this.minValue = minValue;
            this.maxValue = maxValue;
        }
    }

    public BstType processIsBST(TreeNode head){
        if(head==null){
            return new BstType(true,Long.MAX_VALUE,Long.MIN_VALUE);
        }
        BstType r1 = processIsBST(head.left);
        BstType r2 = processIsBST(head.right);
        long minValue=Math.min(head.val,Math.min(r1.minValue,r2.minValue));
        long maxValue=Math.max(head.val,Math.max(r1.maxValue,r2.maxValue));
        boolean bst= r1.bst && r2.bst && head.val > r1.maxValue && head.val < r2.minValue;
        return new BstType(bst,minValue,maxValue);
    }
    public boolean isValidBST(TreeNode head) {
        if(head==null){
            return true;
        }
        return processIsBST(head).bst;
    }
}
```



[102. 二叉树的层序遍历](https://leetcode.cn/problems/binary-tree-level-order-traversal/)

给你二叉树的根节点 `root` ，返回其节点值的 **层序遍历** 。 （即逐层地，从左到右访问所有节点）。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/02/19/tree1.jpg)

```
输入：root = [3,9,20,null,null,15,7]
输出：[[3],[9,20],[15,7]]
```

**示例 2：**

```
输入：root = [1]
输出：[[1]]
```

**示例 3：**

```
输入：root = []
输出：[]
```

 

**提示：**

- 树中节点数目在范围 `[0, 2000]` 内
- `-1000 <= Node.val <= 1000`

***

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        if(root==null){
            return new ArrayList<>();
        }
        List<List<Integer>> resultList=new ArrayList<>();
        List<Integer> tempList=new ArrayList<>();
        Queue<TreeNode> queue=new LinkedList<>();
        queue.add(root);
        TreeNode curLevelEndNode=root;
        TreeNode nextLevelEndNode=null;
        while(!queue.isEmpty()){
            TreeNode node=queue.poll();
            tempList.add(node.val);
            if(node.left!=null){
                queue.add(node.left);
                nextLevelEndNode=node.left;
            }
            if(node.right!=null){
                queue.add(node.right);
                nextLevelEndNode=node.right;
            }
            if(curLevelEndNode==node){
                resultList.add(new ArrayList<>(tempList));
                tempList=new ArrayList<>();
                curLevelEndNode=nextLevelEndNode;
                nextLevelEndNode=null;
            }
        }
        return resultList;
    }
}
```



[105. 从前序与中序遍历序列构造二叉树](https://leetcode.cn/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)
给定两个整数数组 `preorder` 和 `inorder` ，其中 `preorder` 是二叉树的**先序遍历**， `inorder` 是同一棵树的**中序遍历**，请构造二叉树并返回其根节点。

 

**示例 1:**

![img](https://assets.leetcode.com/uploads/2021/02/19/tree.jpg)

```
输入: preorder = [3,9,20,15,7], inorder = [9,3,15,20,7]
输出: [3,9,20,null,null,15,7]
```

**示例 2:**

```
输入: preorder = [-1], inorder = [-1]
输出: [-1]
```

 

**提示:**

- `1 <= preorder.length <= 3000`
- `inorder.length == preorder.length`
- `-3000 <= preorder[i], inorder[i] <= 3000`
- `preorder` 和 `inorder` 均 **无重复** 元素
- `inorder` 均出现在 `preorder`
- `preorder` **保证** 为二叉树的前序遍历序列
- `inorder` **保证** 为二叉树的中序遍历序列

***

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        if(preorder==null||inorder==null||preorder.length!=inorder.length){
            return null;
        }
        return processBuildTree(preorder,inorder,0,preorder.length-1,0,inorder.length-1);
    }

    private TreeNode processBuildTree(int[] preorder,int[] inorder,int low1,int high1,int low2,int high2){
        if(low1>high1){
            return null;
        }
        int rootValue=preorder[low1];
        int index=findIndexByValue(inorder,low2,high2,rootValue);
        TreeNode root=new TreeNode(rootValue);
        root.left=processBuildTree(preorder,inorder,low1+1,low1+index-low2,low2,index-1);
        root.right=processBuildTree(preorder,inorder,low1+index-low2+1,high1,index+1,high2);
        return root;
    }

    private int findIndexByValue(int[] arr, int low,int high,int target){
        for(int i=low;i<=high;i++){
            if(arr[i]==target){
                return i;
            }
        }
        return -1;
    }

   
}
```



[114. 二叉树展开为链表](https://leetcode.cn/problems/flatten-binary-tree-to-linked-list/)
给你二叉树的根结点 `root` ，请你将它展开为一个单链表：

- 展开后的单链表应该同样使用 `TreeNode` ，其中 `right` 子指针指向链表中下一个结点，而左子指针始终为 `null` 。
- 展开后的单链表应该与二叉树 [**先序遍历**](https://baike.baidu.com/item/先序遍历/6442839?fr=aladdin) 顺序相同。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/01/14/flaten.jpg)

```
输入：root = [1,2,5,3,4,null,6]
输出：[1,null,2,null,3,null,4,null,5,null,6]
```

**示例 2：**

```
输入：root = []
输出：[]
```

**示例 3：**

```
输入：root = [0]
输出：[0]
```

 

**提示：**

- 树中结点数在范围 `[0, 2000]` 内
- `-100 <= Node.val <= 100`

***

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
 /*
    解题思路: 定义TreeNode记录前一个节点.
              如果按照先序遍历的序列, 左子树比右子树先访问,会导致在遍历左子树时会改变尚未遍历的右子树的指针.
              因此右子树需要比左子树先遍历, 为了保持先序遍历的特性(倒序) 处理节点在最后进行.
 */
class Solution {

    private TreeNode nextNode;

    public void flatten(TreeNode root) {
        if(root==null){
            return;
        }
        flatten(root.right);
        flatten(root.left);
        root.right=nextNode;
        root.left=null;
        nextNode=root;
    }

}
```



[128. 最长连续序列](https://leetcode.cn/problems/longest-consecutive-sequence/)
给定一个未排序的整数数组 `nums` ，找出数字连续的最长序列（不要求序列元素在原数组中连续）的长度。

请你设计并实现时间复杂度为 `O(n)` 的算法解决此问题。

 

**示例 1：**

```
输入：nums = [100,4,200,1,3,2]
输出：4
解释：最长数字连续序列是 [1, 2, 3, 4]。它的长度为 4。
```

**示例 2：**

```
输入：nums = [0,3,7,2,5,8,4,6,0,1]
输出：9
```

**示例 3：**

```
输入：nums = [1,0,1,2]
输出：3
```

 

**提示：**

- `0 <= nums.length <= 105`
- `-109 <= nums[i] <= 109`

***

```java
class Solution {
    public int longestConsecutive(int[] nums) {
        if(nums==null||nums.length==0){
            return 0;
        }
        Set<Integer> set=new HashSet<>();
        for(int num:nums){
            set.add(num);
        }
        int result=0;
        for(int num:nums){
            if(!set.contains(num-1)){
                int val=num;
                int length=0;
                while(set.contains(val++)){
                    length++;
                }
                result=Math.max(result,length);
            }
        }
        return result;
    }
}
```



[138. 随机链表的复制](https://leetcode.cn/problems/copy-list-with-random-pointer/)
给你一个长度为 `n` 的链表，每个节点包含一个额外增加的随机指针 `random` ，该指针可以指向链表中的任何节点或空节点。

构造这个链表的 **[深拷贝](https://baike.baidu.com/item/深拷贝/22785317?fr=aladdin)**。 深拷贝应该正好由 `n` 个 **全新** 节点组成，其中每个新节点的值都设为其对应的原节点的值。新节点的 `next` 指针和 `random` 指针也都应指向复制链表中的新节点，并使原链表和复制链表中的这些指针能够表示相同的链表状态。**复制链表中的指针都不应指向原链表中的节点** 。

例如，如果原链表中有 `X` 和 `Y` 两个节点，其中 `X.random --> Y` 。那么在复制链表中对应的两个节点 `x` 和 `y` ，同样有 `x.random --> y` 。

返回复制链表的头节点。

用一个由 `n` 个节点组成的链表来表示输入/输出中的链表。每个节点用一个 `[val, random_index]` 表示：

- `val`：一个表示 `Node.val` 的整数。
- `random_index`：随机指针指向的节点索引（范围从 `0` 到 `n-1`）；如果不指向任何节点，则为 `null` 。

你的代码 **只** 接受原链表的头节点 `head` 作为传入参数。

 

**示例 1：**

![img](https://assets.leetcode.cn/aliyun-lc-upload/uploads/2020/01/09/e1.png)

```
输入：head = [[7,null],[13,0],[11,4],[10,2],[1,0]]
输出：[[7,null],[13,0],[11,4],[10,2],[1,0]]
```

**示例 2：**

![img](https://assets.leetcode.cn/aliyun-lc-upload/uploads/2020/01/09/e2.png)

```
输入：head = [[1,1],[2,1]]
输出：[[1,1],[2,1]]
```

**示例 3：**

**![img](https://assets.leetcode.cn/aliyun-lc-upload/uploads/2020/01/09/e3.png)**

```
输入：head = [[3,null],[3,0],[3,null]]
输出：[[3,null],[3,0],[3,null]]
```

 

**提示：**

- `0 <= n <= 1000`
- `-104 <= Node.val <= 104`
- `Node.random` 为 `null` 或指向链表中的节点。

***

```java
/*
// Definition for a Node.
class Node {
    int val;
    Node next;
    Node random;

    public Node(int val) {
        this.val = val;
        this.next = null;
        this.random = null;
    }
}
*/

class Solution {
    public Node copyRandomList(Node head) {
         if(head==null){
            return null;
        }
        Node cur=head;
        while(cur!=null){
            Node nextNode=cur.next;
            Node newNode=new Node(cur.val);
            cur.next=newNode;
            newNode.next=nextNode;
            cur=nextNode;
        }
        cur=head;
        while(cur!=null){
            Node copyNode=cur.next;
            copyNode.random=cur.random!=null?cur.random.next:null;
            cur=cur.next.next;
        }
        cur=head;
        Node copyHead=head.next;
        while(cur!=null){
            Node copyNode=cur.next;
            Node nextNode=cur.next.next;
            cur.next=nextNode;
            copyNode.next=nextNode!=null?nextNode.next:null;
            cur=nextNode;
        }
        return copyHead;
    }
}
```



[139. 单词拆分](https://leetcode.cn/problems/word-break/)
给你一个字符串 `s` 和一个字符串列表 `wordDict` 作为字典。如果可以利用字典中出现的一个或多个单词拼接出 `s` 则返回 `true`。

**注意：**不要求字典中出现的单词全部都使用，并且字典中的单词可以重复使用。

 

**示例 1：**

```
输入: s = "leetcode", wordDict = ["leet", "code"]
输出: true
解释: 返回 true 因为 "leetcode" 可以由 "leet" 和 "code" 拼接成。
```

**示例 2：**

```
输入: s = "applepenapple", wordDict = ["apple", "pen"]
输出: true
解释: 返回 true 因为 "applepenapple" 可以由 "apple" "pen" "apple" 拼接成。
     注意，你可以重复使用字典中的单词。
```

**示例 3：**

```
输入: s = "catsandog", wordDict = ["cats", "dog", "sand", "and", "cat"]
输出: false
```

 

**提示：**

- `1 <= s.length <= 300`
- `1 <= wordDict.length <= 1000`
- `1 <= wordDict[i].length <= 20`
- `s` 和 `wordDict[i]` 仅由小写英文字母组成
- `wordDict` 中的所有字符串 **互不相同**

***

```java
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        if(s==null||wordDict==null||wordDict.isEmpty()){
            return false;
        }
        int n=s.length();
        boolean[] dp=new boolean[n];
        for(int i=0;i<n;i++){
            for(String word:wordDict){
                if(s.substring(0,i+1).endsWith(word)&&(i-word.length()>=0?dp[i-word.length()]:true)){
                    dp[i]=true;
                    break;
                }
            }
        }
        return dp[n-1];
    }

    
}
```



[142. 环形链表 II](https://leetcode.cn/problems/linked-list-cycle-ii/)
给定一个链表的头节点  `head` ，返回链表开始入环的第一个节点。 *如果链表无环，则返回 `null`。*

如果链表中有某个节点，可以通过连续跟踪 `next` 指针再次到达，则链表中存在环。 为了表示给定链表中的环，评测系统内部使用整数 `pos` 来表示链表尾连接到链表中的位置（**索引从 0 开始**）。如果 `pos` 是 `-1`，则在该链表中没有环。**注意：`pos` 不作为参数进行传递**，仅仅是为了标识链表的实际情况。

**不允许修改** 链表。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist.png)

```
输入：head = [3,2,0,-4], pos = 1
输出：返回索引为 1 的链表节点
解释：链表中有一个环，其尾部连接到第二个节点。
```

**示例 2：**

![img](https://assets.leetcode.cn/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist_test2.png)

```
输入：head = [1,2], pos = 0
输出：返回索引为 0 的链表节点
解释：链表中有一个环，其尾部连接到第一个节点。
```

**示例 3：**

![img](https://assets.leetcode.cn/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist_test3.png)

```
输入：head = [1], pos = -1
输出：返回 null
解释：链表中没有环。
```

 

**提示：**

- 链表中节点的数目范围在范围 `[0, 104]` 内
- `-105 <= Node.val <= 105`
- `pos` 的值为 `-1` 或者链表中的一个有效索引

***

```java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode detectCycle(ListNode head) {
        if(head==null||head.next==null||head.next.next==null){
            return null;
        }
        ListNode fast=head.next.next;
        ListNode slow=head.next;
        while(fast!=slow){
            if(fast.next==null||fast.next.next==null){
                return null;
            }
            fast=fast.next.next;
            slow=slow.next;
        }
        slow=head;
        while(fast!=slow){
            fast=fast.next;
            slow=slow.next;
        }
        return slow;
    }
}
```



[146. LRU 缓存](https://leetcode.cn/problems/lru-cache/)
请你设计并实现一个满足 [LRU (最近最少使用) 缓存](https://baike.baidu.com/item/LRU) 约束的数据结构。

实现 `LRUCache` 类：

- `LRUCache(int capacity)` 以 **正整数** 作为容量 `capacity` 初始化 LRU 缓存
- `int get(int key)` 如果关键字 `key` 存在于缓存中，则返回关键字的值，否则返回 `-1` 。
- `void put(int key, int value)` 如果关键字 `key` 已经存在，则变更其数据值 `value` ；如果不存在，则向缓存中插入该组 `key-value` 。如果插入操作导致关键字数量超过 `capacity` ，则应该 **逐出** 最久未使用的关键字。

函数 `get` 和 `put` 必须以 `O(1)` 的平均时间复杂度运行。

 

**示例：**

```
输入
["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]
输出
[null, null, null, 1, null, -1, null, -1, 3, 4]

解释
LRUCache lRUCache = new LRUCache(2);
lRUCache.put(1, 1); // 缓存是 {1=1}
lRUCache.put(2, 2); // 缓存是 {1=1, 2=2}
lRUCache.get(1);    // 返回 1
lRUCache.put(3, 3); // 该操作会使得关键字 2 作废，缓存是 {1=1, 3=3}
lRUCache.get(2);    // 返回 -1 (未找到)
lRUCache.put(4, 4); // 该操作会使得关键字 1 作废，缓存是 {4=4, 3=3}
lRUCache.get(1);    // 返回 -1 (未找到)
lRUCache.get(3);    // 返回 3
lRUCache.get(4);    // 返回 4
```

 

**提示：**

- `1 <= capacity <= 3000`
- `0 <= key <= 10000`
- `0 <= value <= 105`
- 最多调用 `2 * 105` 次 `get` 和 `put`

***

```java
class LRUCache extends LinkedHashMap<Integer,Integer>{
    int capacity;

    public LRUCache(int capacity) {
        super(capacity,0.75f,true);
        this.capacity=capacity;
    }

    public int get(int key) {
        return super.getOrDefault(key,-1);
    }

    public void put(int key, int value) {
        super.put(key,value);
    }

    public boolean removeEldestEntry(Map.Entry eldest){
        return this.size()>capacity;
    }
}
```



[148. 排序链表](https://leetcode.cn/problems/sort-list/)
给你链表的头结点 `head` ，请将其按 **升序** 排列并返回 **排序后的链表** 。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/09/14/sort_list_1.jpg)

```
输入：head = [4,2,1,3]
输出：[1,2,3,4]
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2020/09/14/sort_list_2.jpg)

```
输入：head = [-1,5,3,4,0]
输出：[-1,0,3,4,5]
```

**示例 3：**

```
输入：head = []
输出：[]
```

 

**提示：**

- 链表中节点的数目在范围 `[0, 5 * 104]` 内
- `-105 <= Node.val <= 105`

***

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode sortList(ListNode head) {
        if(head==null||head.next==null){
            return head;
        }
        ListNode fast=head;
        ListNode slow=head;
        while(fast.next!=null&&fast.next.next!=null){
            fast=fast.next.next;
            slow=slow.next;
        }
        ListNode head2=slow.next;
        slow.next=null;
        head=sortList(head);
        head2=sortList(head2);
        ListNode newHead=new ListNode(0);
        ListNode tail=newHead;
        ListNode p1=head;
        ListNode p2=head2;
        while(p1!=null&&p2!=null){
            if(p1.val<=p2.val){
                tail.next=p1;
                p1=p1.next;
            }else{
                tail.next=p2;
                p2=p2.next;
            }
            tail=tail.next;
        }
        if(p1!=null){
            tail.next=p1;
        }
        if(p2!=null){
            tail.next=p2;
        }
        return newHead.next;
    }
}
```



[152. 乘积最大子数组](https://leetcode.cn/problems/maximum-product-subarray/)
给你一个整数数组 `nums` ，请你找出数组中乘积最大的非空连续 子数组（该子数组中至少包含一个数字），并返回该子数组所对应的乘积。

测试用例的答案是一个 **32-位** 整数。

**请注意**，一个只包含一个元素的数组的乘积是这个元素的值。

 

**示例 1:**

```
输入: nums = [2,3,-2,4]
输出: 6
解释: 子数组 [2,3] 有最大乘积 6。
```

**示例 2:**

```
输入: nums = [-2,0,-1]
输出: 0
解释: 结果不能为 2, 因为 [-2,-1] 不是子数组。
```

 

**提示:**

- `1 <= nums.length <= 2 * 104`
- `-10 <= nums[i] <= 10`
- `nums` 的任何子数组的乘积都 **保证** 是一个 **32-位** 整数

***

```java
class Solution {
    public int maxProduct(int[] nums) {
        if(nums==null||nums.length==0){
            return 0;
        }
        int[] maxDP=new int[nums.length];
        int[] minDP=new int[nums.length];
        maxDP[0]=nums[0];
        minDP[0]=nums[0];
        int result=nums[0];
        for(int i=1;i<nums.length;i++){
            maxDP[i]=Math.max(nums[i],Math.max(maxDP[i-1]*nums[i],minDP[i-1]*nums[i]));
            minDP[i]=Math.min(nums[i],Math.min(maxDP[i-1]*nums[i],minDP[i-1]*nums[i]));
            result=Math.max(result,maxDP[i]);
        }
        return result;
    }
}
```



[153. 寻找旋转排序数组中的最小值](https://leetcode.cn/problems/find-minimum-in-rotated-sorted-array/)

已知一个长度为 `n` 的数组，预先按照升序排列，经由 `1` 到 `n` 次 **旋转** 后，得到输入数组。例如，原数组 `nums = [0,1,2,4,5,6,7]` 在变化后可能得到：

- 若旋转 `4` 次，则可以得到 `[4,5,6,7,0,1,2]`
- 若旋转 `7` 次，则可以得到 `[0,1,2,4,5,6,7]`

注意，数组 `[a[0], a[1], a[2], ..., a[n-1]]` **旋转一次** 的结果为数组 `[a[n-1], a[0], a[1], a[2], ..., a[n-2]]` 。

给你一个元素值 **互不相同** 的数组 `nums` ，它原来是一个升序排列的数组，并按上述情形进行了多次旋转。请你找出并返回数组中的 **最小元素** 。

你必须设计一个时间复杂度为 `O(log n)` 的算法解决此问题。

 

**示例 1：**

```
输入：nums = [3,4,5,1,2]
输出：1
解释：原数组为 [1,2,3,4,5] ，旋转 3 次得到输入数组。
```

**示例 2：**

```
输入：nums = [4,5,6,7,0,1,2]
输出：0
解释：原数组为 [0,1,2,4,5,6,7] ，旋转 4 次得到输入数组。
```

**示例 3：**

```
输入：nums = [11,13,15,17]
输出：11
解释：原数组为 [11,13,15,17] ，旋转 4 次得到输入数组。
```

 

**提示：**

- `n == nums.length`
- `1 <= n <= 5000`
- `-5000 <= nums[i] <= 5000`
- `nums` 中的所有整数 **互不相同**
- `nums` 原来是一个升序排序的数组，并进行了 `1` 至 `n` 次旋转

***

```java
class Solution {
    public int findMin(int[] nums) {
        int len=nums.length;
        int low=0;
        int high=len-1;
        while(low<=high){
            int mid=(low+high)/2;
            if(nums[mid]>nums[len-1]){
                low=mid+1;
            }else{
                high=mid-1;
            }         
        }
        return nums[low];
    }
}
```



[155. 最小栈](https://leetcode.cn/problems/min-stack/)
设计一个支持 `push` ，`pop` ，`top` 操作，并能在常数时间内检索到最小元素的栈。

实现 `MinStack` 类:

- `MinStack()` 初始化堆栈对象。
- `void push(int value)` 将元素 `value` 推入堆栈。
- `void pop()` 删除堆栈顶部的元素。
- `int top()` 获取堆栈顶部的元素。
- `int getMin()` 获取堆栈中的最小元素。

 

**示例 1:**

```
输入：
["MinStack","push","push","push","getMin","pop","top","getMin"]
[[],[-2],[0],[-3],[],[],[],[]]

输出：
[null,null,null,null,-3,null,0,-2]

解释：
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin();   --> 返回 -3.
minStack.pop();
minStack.top();      --> 返回 0.
minStack.getMin();   --> 返回 -2.
```

 

**提示：**

- `-231 <= val <= 231 - 1`
- `pop`、`top` 和 `getMin` 操作总是在 **非空栈** 上调用
- `push`, `pop`, `top`, and `getMin`最多被调用 `3 * 104` 次

***

```JAVA
class MinStack {

    private Stack<Integer> valueStack;
    private Stack<Integer> minStack;

    public MinStack() {
        valueStack=new Stack<>();
        minStack=new Stack<>();
    }
    
    public void push(int val) {
        valueStack.push(val);
        if(minStack.isEmpty()||minStack.peek()>=val){
            minStack.push(val);
        }
    }
    
    public void pop() {
        int val=valueStack.pop();
        if(minStack.peek()==val){
            minStack.pop();
        }
    }
    
    public int top() {
        return valueStack.peek();
    }
    
    public int getMin() {
        return minStack.peek();
    }
}

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack obj = new MinStack();
 * obj.push(val);
 * obj.pop();
 * int param_3 = obj.top();
 * int param_4 = obj.getMin();
 */
```



[198. 打家劫舍](https://leetcode.cn/problems/house-robber/)
你是一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响你偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统，**如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警**。

给定一个代表每个房屋存放金额的非负整数数组，计算你 **不触动警报装置的情况下** ，一夜之内能够偷窃到的最高金额。

 

**示例 1：**

```
输入：[1,2,3,1]
输出：4
解释：偷窃 1 号房屋 (金额 = 1) ，然后偷窃 3 号房屋 (金额 = 3)。
     偷窃到的最高金额 = 1 + 3 = 4 。
```

**示例 2：**

```
输入：[2,7,9,3,1]
输出：12
解释：偷窃 1 号房屋 (金额 = 2), 偷窃 3 号房屋 (金额 = 9)，接着偷窃 5 号房屋 (金额 = 1)。
     偷窃到的最高金额 = 2 + 9 + 1 = 12 。
```

 

**提示：**

- `1 <= nums.length <= 100`
- `0 <= nums[i] <= 400`

***

```JAVA
class Solution {
    public int rob(int[] nums) {
        if(nums==null||nums.length==0){
            return 0;
        }
        int n=nums.length;
        if(n==1){
            return nums[0];
        }
        int[] dp=new int[n];
        dp[n-1]=nums[n-1];
        dp[n-2]=Math.max(nums[n-1],nums[n-2]);
        for(int i=n-3;i>=0;i--){
            dp[i]=Math.max(nums[i]+dp[i+2],dp[i+1]);
        }
        return dp[0];
    }
}
```



[200. 岛屿数量](https://leetcode.cn/problems/number-of-islands/)
给你一个由 `'1'`（陆地）和 `'0'`（水）组成的的二维网格，请你计算网格中岛屿的数量。

岛屿总是被水包围，并且每座岛屿只能由水平方向和/或竖直方向上相邻的陆地连接形成。

此外，你可以假设该网格的四条边均被水包围。

 

**示例 1：**

```
输入：grid = [
  ['1','1','1','1','0'],
  ['1','1','0','1','0'],
  ['1','1','0','0','0'],
  ['0','0','0','0','0']
]
输出：1
```

**示例 2：**

```
输入：grid = [
  ['1','1','0','0','0'],
  ['1','1','0','0','0'],
  ['0','0','1','0','0'],
  ['0','0','0','1','1']
]
输出：3
```

 

**提示：**

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 300`
- `grid[i][j]` 的值为 `'0'` 或 `'1'`

***

```java
class Solution {
    public int numIslands(char[][] grid) {
        if(grid==null||grid.length==0||grid[0].length==0){
            return 0;
        }
        int result=0;
        for(int i=0;i<grid.length;i++){
            for(int j=0;j<grid[0].length;j++){
                if(grid[i][j]=='1'){
                    result++;
                    spreadIslands(grid,i,j);
                }
            }
        }
        return result;
    }
    
    private void spreadIslands(char[][] grid,int i,int j){
        if(i<0||i>=grid.length||j<0||j>=grid[0].length||grid[i][j]=='0'){
            return;
        }
        grid[i][j]='0';
        spreadIslands(grid,i-1,j);
        spreadIslands(grid,i+1,j);
        spreadIslands(grid,i,j-1);
        spreadIslands(grid,i,j+1);
    }
}
```



[207. 课程表](https://leetcode.cn/problems/course-schedule/)

你这个学期必须选修 `numCourses` 门课程，记为 `0` 到 `numCourses - 1` 。

在选修某些课程之前需要一些先修课程。 先修课程按数组 `prerequisites` 给出，其中 `prerequisites[i] = [ai, bi]` ，表示如果要学习课程 `ai` 则 **必须** 先学习课程 `bi` 。

- 例如，先修课程对 `[0, 1]` 表示：想要学习课程 `0` ，你需要先完成课程 `1` 。

请你判断是否可能完成所有课程的学习？如果可以，返回 `true` ；否则，返回 `false` 。

 

**示例 1：**

```
输入：numCourses = 2, prerequisites = [[1,0]]
输出：true
解释：总共有 2 门课程。学习课程 1 之前，你需要完成课程 0 。这是可能的。
```

**示例 2：**

```
输入：numCourses = 2, prerequisites = [[1,0],[0,1]]
输出：false
解释：总共有 2 门课程。学习课程 1 之前，你需要先完成课程 0 ；并且学习课程 0 之前，你还应先完成课程 1 。这是不可能的。
```

 

**提示：**

- `1 <= numCourses <= 2000`
- `0 <= prerequisites.length <= 5000`
- `prerequisites[i].length == 2`
- `0 <= ai, bi < numCourses`
- `prerequisites[i]` 中的所有课程对 **互不相同**

***

```java
class Solution {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        List<List<Integer>> graph=new ArrayList<>();
        for(int i=0;i<numCourses;i++){
            graph.add(new ArrayList<>());
        }
        for(int i=0;i<prerequisites.length;i++){
            graph.get(prerequisites[i][1]).add(prerequisites[i][0]);
        }
        int[] visited=new int[numCourses];
        for(int i=0;i<numCourses;i++){
            if(findCircle(graph,i,visited)){
                return false;
            }
        }
        return true;
    }
    
    private boolean findCircle(List<List<Integer>> graph,int node,int[] visited){
        if(visited[node]==1){
            return true;
        }
        if(visited[node]==2){
            return false;
        }
        visited[node]=1;
        for(Integer next:graph.get(node)){
            if(findCircle(graph,next,visited)){
                return true;
            }
        }
        visited[node]=2;
        return false;
    }
}
```



[208. 实现 Trie (前缀树)](https://leetcode.cn/problems/implement-trie-prefix-tree/)
**[Trie](https://baike.baidu.com/item/字典树/9825209?fr=aladdin)**（发音类似 "try"）或者说 **前缀树** 是一种树形数据结构，用于高效地存储和检索字符串数据集中的键。这一数据结构有相当多的应用情景，例如自动补全和拼写检查。

请你实现 Trie 类：

- `Trie()` 初始化前缀树对象。
- `void insert(String word)` 向前缀树中插入字符串 `word` 。
- `boolean search(String word)` 如果字符串 `word` 在前缀树中，返回 `true`（即，在检索之前已经插入）；否则，返回 `false` 。
- `boolean startsWith(String prefix)` 如果之前已经插入的字符串 `word` 的前缀之一为 `prefix` ，返回 `true` ；否则，返回 `false` 。

 

**示例：**

```
输入
["Trie", "insert", "search", "search", "startsWith", "insert", "search"]
[[], ["apple"], ["apple"], ["app"], ["app"], ["app"], ["app"]]
输出
[null, null, true, false, true, null, true]

解释
Trie trie = new Trie();
trie.insert("apple");
trie.search("apple");   // 返回 True
trie.search("app");     // 返回 False
trie.startsWith("app"); // 返回 True
trie.insert("app");
trie.search("app");     // 返回 True
```

 

**提示：**

- `1 <= word.length, prefix.length <= 2000`
- `word` 和 `prefix` 仅由小写英文字母组成
- `insert`、`search` 和 `startsWith` 调用次数 **总计** 不超过 `3 * 104` 次

***

```java
class Trie {
    
    private TrieNode root;

    public Trie() {
        root=new TrieNode();
    }
    
    public void insert(String word) {
        if(word==null){
            return;
        }
        TrieNode cur=root;
        root.pass++;
        for(int i=0;i<word.length();i++){
            int index=word.charAt(i)-'a';
            if(cur.next[index]==null){
                cur.next[index]=new TrieNode();
            }
            cur=cur.next[index];
            cur.pass++;
        }
        cur.end++;
    }
    
    public boolean search(String word) {
        if(word==null){
            return false;
        }
        TrieNode cur=root;
        for(int i=0;i<word.length();i++){
            int index=word.charAt(i)-'a';
            if(cur.next[index]==null){
                return false;
            }
            cur=cur.next[index];
        }
        return cur.end!=0;
    }
    
    public boolean startsWith(String prefix) {
        if(prefix==null){
            return false;
        }
        TrieNode cur=root;
        for(int i=0;i<prefix.length();i++){
            int index=prefix.charAt(i)-'a';
            if(cur.next[index]==null){
                return false;
            }
            cur=cur.next[index];
        }
        return cur.pass!=0;
    }

    public static class TrieNode{
        int pass;
        int end;
        TrieNode[] next;
        public TrieNode(){
            next=new TrieNode[26];
        }
    }
}

/**
 * Your Trie object will be instantiated and called as such:
 * Trie obj = new Trie();
 * obj.insert(word);
 * boolean param_2 = obj.search(word);
 * boolean param_3 = obj.startsWith(prefix);
 */
```



[215. 数组中的第K个最大元素](https://leetcode.cn/problems/kth-largest-element-in-an-array/)
给定整数数组 `nums` 和整数 `k`，请返回数组中第 `**k**` 个最大的元素。

请注意，你需要找的是数组排序后的第 `k` 个最大的元素，而不是第 `k` 个不同的元素。

你必须设计并实现时间复杂度为 `O(n)` 的算法解决此问题。

 

**示例 1:**

```
输入: [3,2,1,5,6,4], k = 2
输出: 5
```

**示例 2:**

```
输入: [3,2,3,1,2,4,5,5,6], k = 4
输出: 4
```

 

**提示：**

- `1 <= k <= nums.length <= 105`
- `-104 <= nums[i] <= 104`

***

```java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        int destIndex=nums.length-k;
        int low=0;
        int high=nums.length-1;
        while(low<=high){
            int index=partition(nums,low,high);
            if(index<destIndex){
                low=index+1;
            }else if(index>destIndex){
                high=index-1;
            }else{
                return nums[index];
            }
        }
        return -1;
    }

    private int partition(int[] nums,int left,int right){
        int target=nums[left];
        int border=left;
        for(int i=left+1;i<=right;i++){
            if(nums[i]<target){
                border++;
                swap(nums,i,border);
            }
        }
        swap(nums,left,border);
        return border;
    }

    private void swap(int[] nums,int i,int j){
        int temp=nums[i];
        nums[i]=nums[j];
        nums[j]=temp;
    }
}
```



[230. 二叉搜索树中第 K 小的元素](https://leetcode.cn/problems/kth-smallest-element-in-a-bst/)

给定一个二叉搜索树的根节点 `root` ，和一个整数 `k` ，请你设计一个算法查找其中第 `k` 小的元素（`k` 从 1 开始计数）。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/01/28/kthtree1.jpg)

```
输入：root = [3,1,4,null,2], k = 1
输出：1
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2021/01/28/kthtree2.jpg)

```
输入：root = [5,3,6,2,4,null,null,1], k = 3
输出：3
```

 

 

**提示：**

- 树中的节点数为 `n` 。
- `1 <= k <= n <= 104`
- `0 <= Node.val <= 104`

***

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    int index;
    int result;

    public int kthSmallest(TreeNode root, int k) {
        processKthSmallest(root,k);
        return result;
    }

    private void processKthSmallest(TreeNode root,int k){
        if(root==null){
            return;
        }
        processKthSmallest(root.left,k);
        index++;
        if(index==k){
            result=root.val;
        }
        processKthSmallest(root.right,k);
    }
}
```



[236. 二叉树的最近公共祖先](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-tree/)
给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。

[百度百科](https://baike.baidu.com/item/最近公共祖先/8918834?fr=aladdin)中最近公共祖先的定义为：“对于有根树 T 的两个节点 p、q，最近公共祖先表示为一个节点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（**一个节点也可以是它自己的祖先**）。”

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2018/12/14/binarytree.png)

```
输入：root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
输出：3
解释：节点 5 和节点 1 的最近公共祖先是节点 3 。
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2018/12/14/binarytree.png)

```
输入：root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
输出：5
解释：节点 5 和节点 4 的最近公共祖先是节点 5 。因为根据定义最近公共祖先节点可以为节点本身。
```

**示例 3：**

```
输入：root = [1,2], p = 1, q = 2
输出：1
```

 

**提示：**

- 树中节点数目在范围 `[2, 105]` 内。
- `-109 <= Node.val <= 109`
- 所有 `Node.val` `互不相同` 。
- `p != q`
- `p` 和 `q` 均存在于给定的二叉树中。

***

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if(root==null||root==p||root==q){
            return root;
        }
        TreeNode left=lowestCommonAncestor(root.left,p,q);
        TreeNode right=lowestCommonAncestor(root.right,p,q);
        if(left!=null&&right!=null){
            return root;
        }
        return left!=null?left:right;
    }
}
```



[238. 除了自身以外数组的乘积](https://leetcode.cn/problems/product-of-array-except-self/)
给你一个整数数组 `nums`，返回 数组 `answer` ，其中 `answer[i]` 等于 `nums` 中除了 `nums[i]` 之外其余各元素的乘积 。

题目数据 **保证** 数组 `nums`之中任意元素的全部前缀元素和后缀的乘积都在 **32 位** 整数范围内。

请 **不要使用除法，**且在 `O(n)` 时间复杂度内完成此题。

 

**示例 1:**

```
输入: nums = [1,2,3,4]
输出: [24,12,8,6]
```

**示例 2:**

```
输入: nums = [-1,1,0,-3,3]
输出: [0,0,9,0,0]
```

 

**提示：**

- `2 <= nums.length <= 105`
- `-30 <= nums[i] <= 30`
- 输入 **保证** 数组 `answer[i]` 在 **32 位** 整数范围内

***

```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        if(nums==null||nums.length==0){
            return new int[]{};
        }
        int len=nums.length;
        int[] result=new int[len];
        int[] leftMulArr=new int[len];
        int[] rightMulArr=new int[len];
        leftMulArr[0]=nums[0];
        rightMulArr[len-1]=nums[len-1];
        for(int i=1;i<len;i++){
            leftMulArr[i]=leftMulArr[i-1]*nums[i];
        }
        for(int i=len-2;i>=0;i--){
            rightMulArr[i]=rightMulArr[i+1]*nums[i];
        }
        for(int i=0;i<len;i++){
            result[i]=(i==0?1:leftMulArr[i-1])*(i==len-1?1:rightMulArr[i+1]);
        }
        return result;
    }
}
```



[240. 搜索二维矩阵 II](https://leetcode.cn/problems/search-a-2d-matrix-ii/)
编写一个高效的算法来搜索 `*m* x *n*` 矩阵 `matrix` 中的一个目标值 `target` 。该矩阵具有以下特性：

- 每行的元素从左到右升序排列。
- 每列的元素从上到下升序排列。

 

**示例 1：**

![img](https://assets.leetcode.cn/aliyun-lc-upload/uploads/2020/11/25/searchgrid2.jpg)

```
输入：matrix = [[1,4,7,11,15],[2,5,8,12,19],[3,6,9,16,22],[10,13,14,17,24],[18,21,23,26,30]], target = 5
输出：true
```

**示例 2：**

![img](https://assets.leetcode.cn/aliyun-lc-upload/uploads/2020/11/25/searchgrid.jpg)

```
输入：matrix = [[1,4,7,11,15],[2,5,8,12,19],[3,6,9,16,22],[10,13,14,17,24],[18,21,23,26,30]], target = 20
输出：false
```

 

**提示：**

- `m == matrix.length`
- `n == matrix[i].length`
- `1 <= n, m <= 300`
- `-109 <= matrix[i][j] <= 109`
- 每行的所有元素从左到右升序排列
- 每列的所有元素从上到下升序排列
- `-109 <= target <= 109`

***

```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        if(matrix==null||matrix.length==0||matrix[0].length==0){
            return false;
        }
        int M=matrix.length;
        int N=matrix[0].length;
        int row=0;
        int col=N-1;
        while(row<M&&col>=0){
            if(matrix[row][col]<target){
                row++;
            }else if(matrix[row][col]>target){
                col--;
            }else{
                return true;
            }
        }
        return false;
    }
}
```



[279. 完全平方数](https://leetcode.cn/problems/perfect-squares/)
给你一个整数 `n` ，返回 *和为 `n` 的完全平方数的最少数量* 。

**完全平方数** 是一个整数，其值等于另一个整数的平方；换句话说，其值等于一个整数自乘的积。例如，`1`、`4`、`9` 和 `16` 都是完全平方数，而 `3` 和 `11` 不是。

 

**示例 1：**

```
输入：n = 12
输出：3 
解释：12 = 4 + 4 + 4
```

**示例 2：**

```
输入：n = 13
输出：2
解释：13 = 4 + 9
```

 

**提示：**

- `1 <= n <= 104`

***

```java
class Solution {
    public int numSquares(int n) {
        int[] dp=new int[n+1];
        dp[1]=1;
        int target=1;
        for(int i=2;i<=n;i++){
            int min=Integer.MAX_VALUE;
            for(int j=1;j*j<=i;j++){
                min=Math.min(min,dp[i-j*j]);
            }
            dp[i]=min+1;
        } 
        return dp[n];
    }
}
```



[287. 寻找重复数](https://leetcode.cn/problems/find-the-duplicate-number/)
给定一个包含 `n + 1` 个整数的数组 `nums` ，其数字都在 `[1, n]` 范围内（包括 `1` 和 `n`），可知至少存在一个重复的整数。

假设 `nums` 只有 **一个重复的整数** ，返回 **这个重复的数** 。

你设计的解决方案必须 **不修改** 数组 `nums` 且只用常量级 `O(1)` 的额外空间。

 

**示例 1：**

```
输入：nums = [1,3,4,2,2]
输出：2
```

**示例 2：**

```
输入：nums = [3,1,3,4,2]
输出：3
```

**示例 3 :**

```
输入：nums = [3,3,3,3,3]
输出：3
```

 

 

**提示：**

- `1 <= n <= 105`
- `nums.length == n + 1`
- `1 <= nums[i] <= n`
- `nums` 中 **只有一个整数** 出现 **两次或多次** ，其余整数均只出现 **一次**

 

**进阶：**

- 如何证明 `nums` 中至少存在一个重复的数字?
- 你可以设计一个线性级时间复杂度 `O(n)` 的解决方案吗？

***

```java
class Solution {
    public int findDuplicate(int[] nums) {
        int n=nums.length;
        int max_bits=31;
        int result=0;
        while(((n-1)>>max_bits)==0){
            max_bits--;
        }
        for(int bit=0;bit<=max_bits;bit++){
            int x=0;
            int y=0;
            for(int i=0;i<n;i++){
                if((nums[i]&(1<<bit))!=0){
                    x++;
                }
                if(i>=1&&(i&(1<<bit))!=0){
                    y++;
                }
            }
            if(x>y){
                result|=1<<bit;
            }
        }
        return result;
    }
}
```



[300. 最长递增子序列](https://leetcode.cn/problems/longest-increasing-subsequence/)
给你一个整数数组 `nums` ，找到其中最长严格递增子序列的长度。

**子序列** 是由数组派生而来的序列，删除（或不删除）数组中的元素而不改变其余元素的顺序。例如，`[3,6,2,7]` 是数组 `[0,3,1,6,2,2,7]` 的子序列。

 

**示例 1：**

```
输入：nums = [10,9,2,5,3,7,101,18]
输出：4
解释：最长递增子序列是 [2,3,7,101]，因此长度为 4 。
```

**示例 2：**

```
输入：nums = [0,1,0,3,2,3]
输出：4
```

**示例 3：**

```
输入：nums = [7,7,7,7,7,7,7]
输出：1
```

 

**提示：**

- `1 <= nums.length <= 2500`
- `-104 <= nums[i] <= 104`

***

```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        if(nums==null||nums.length==0){
            return 0;
        }
        int[] dp=new int[nums.length+1];
        dp[1]=nums[0];
        int max=1;
        int index=1;
        for(int i=1;i<nums.length;i++){
            if(nums[i]>dp[index]){
                dp[++index]=nums[i];
            }else{
                int low=1;
                int high=index;
                int target=0;
                while(low<=high){
                    int mid=(high+low)>>1;
                    if(dp[mid]<nums[i]){
                        low=mid+1;
                        target=mid;
                    }else{
                        high=mid-1;
                    }
                }
                dp[target+1]=nums[i];
            }
        }
        return index;
    }
}
```



[322. 零钱兑换](https://leetcode.cn/problems/coin-change/)
给你一个整数数组 `coins` ，表示不同面额的硬币；以及一个整数 `amount` ，表示总金额。

计算并返回可以凑成总金额所需的 **最少的硬币个数** 。如果没有任何一种硬币组合能组成总金额，返回 `-1` 。

你可以认为每种硬币的数量是无限的。

 

**示例 1：**

```
输入：coins = [1, 2, 5], amount = 11
输出：3 
解释：11 = 5 + 5 + 1
```

**示例 2：**

```
输入：coins = [2], amount = 3
输出：-1
```

**示例 3：**

```
输入：coins = [1], amount = 0
输出：0
```

 

**提示：**

- `1 <= coins.length <= 12`
- `1 <= coins[i] <= 231 - 1`
- `0 <= amount <= 104`

***

```java
class Solution {
    public int coinChange(int[] coins, int amount) {
        if(coins==null||coins.length==0){
            return -1;
        }
        int n=coins.length;
        int[][] dp=new int[n+1][amount+1];
        for(int i=1;i<=amount;i++){
            dp[n][i]=-1;
        }
        for(int i=n-1;i>=0;i--){
            for(int j=1;j<=amount;j++){
                int result=Integer.MAX_VALUE;
                for(int k=0;coins[i]*k<=j;k++){
                    int count=dp[i+1][j-coins[i]*k];
                    if(count!=-1){
                        result=Math.min(result,k+count);
                    }
                }
                dp[i][j]=result==Integer.MAX_VALUE?-1:result;
            }
        }
        return dp[0][amount];
    }

    private int processCoinChange(int[] coins,int index,int rest){
        int result=Integer.MAX_VALUE;
        for(int i=0;coins[index]*i<=rest;i++){
            int count=processCoinChange(coins,index+1,rest-coins[index]*i);
            if(count!=-1){
                result=Math.min(result,i+count);
            }
        }
        return result==Integer.MAX_VALUE?-1:result;
    }
}
```



[347. 前 K 个高频元素](https://leetcode.cn/problems/top-k-frequent-elements/)

给你一个整数数组 `nums` 和一个整数 `k` ，请你返回其中出现频率前 `k` 高的元素。你可以按 **任意顺序** 返回答案。

 

**示例 1：**

**输入：**nums = [1,1,1,2,2,3], k = 2

**输出：**[1,2]

**示例 2：**

**输入：**nums = [1], k = 1

**输出：**[1]

**示例 3：**

**输入：**nums = [1,2,1,2,1,2,3,1,3,2], k = 2

**输出：**[1,2]

 

**提示：**

- `1 <= nums.length <= 105`
- `-104 <= nums[i] <= 104`
- `k` 的取值范围是 `[1, 数组中不相同的元素的个数]`
- 题目数据保证答案唯一，换句话说，数组中前 `k` 个高频元素的集合是唯一的

***

```java
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        if(nums==null||nums.length==0){
            return new int[]{};
        }
        Map<Integer,Integer> map=new HashMap<>();
        for(int num:nums){
            map.put(num,map.getOrDefault(num,0)+1);
        }
        PriorityQueue<Type> queue=new PriorityQueue<>((o1,o2)->o2.count-o1.count);
        for(Map.Entry<Integer,Integer> entry:map.entrySet()){
            queue.add(new Type(entry.getKey(),entry.getValue()));
        }
        int[] result=new int[k];
        for(int i=0;i<k;i++){
            result[i]=queue.poll().num;
        }
        return result;
    }

    public static class Type{
        int num;
        int count;
        public Type(int num,int count){
            this.num=num;
            this.count=count;
        }
    }
}
```



[394. 字符串解码](https://leetcode.cn/problems/decode-string/)

给定一个经过编码的字符串，返回它解码后的字符串。

编码规则为: `k[encoded_string]`，表示其中方括号内部的 `encoded_string` 正好重复 `k` 次。注意 `k` 保证为正整数。

你可以认为输入字符串总是有效的；输入字符串中没有额外的空格，且输入的方括号总是符合格式要求的。

此外，你可以认为原始数据不包含数字，所有的数字只表示重复的次数 `k` ，例如不会出现像 `3a` 或 `2[4]` 的输入。

测试用例保证输出的长度不会超过 `105`。

 

**示例 1：**

```
输入：s = "3[a]2[bc]"
输出："aaabcbc"
```

**示例 2：**

```
输入：s = "3[a2[c]]"
输出："accaccacc"
```

**示例 3：**

```
输入：s = "2[abc]3[cd]ef"
输出："abcabccdcdcdef"
```

**示例 4：**

```
输入：s = "abc3[cd]xyz"
输出："abccdcdcdxyz"
```

 

**提示：**

- `1 <= s.length <= 30`
- `s` 由小写英文字母、数字和方括号 `'[]'` 组成
- `s` 保证是一个 **有效** 的输入。
- `s` 中所有整数的取值范围为 `[1, 300]` 

***

```java
class Solution {
    public String decodeString(String s) {
        if(s==null||s.length()==0){
            return s;
        }
        char[] chs=s.toCharArray();
        return processDecodeString(chs,0,chs.length-1);
    }

    private String processDecodeString(char[] chs,int start,int end){
        StringBuilder sb=new StringBuilder();
        int index=start;
        
        while(index<=end){
            if(chs[index]>='a'&&chs[index]<='z'){
                sb.append(chs[index]);
            }else if(chs[index]>='0'&&chs[index]<='9'){
                int repeatIndex=-1;
                int repeatCount=0;
                int repeatDepth=0;
                while(index<=end&&chs[index]>='0'&&chs[index]<='9'){
                    repeatCount=repeatCount*10+(chs[index]-'0');
                    index++;
                }
                repeatIndex=index+1;
                while(index<=end){
                    if(chs[index]=='['){
                        repeatDepth++;
                    }else if(chs[index]==']'){
                        repeatDepth--;
                        if(repeatDepth==0){
                           break;
                        }
                    }
                    index++;
                }
                String repeatValue=processDecodeString(chs,repeatIndex,index-1);
                for(int i=0;i<repeatCount;i++){
                    sb.append(repeatValue);
                }
            }
            index++;
        }
        return sb.toString();
    }
}
```



[416. 分割等和子集](https://leetcode.cn/problems/partition-equal-subset-sum/)
给你一个 **只包含正整数** 的 **非空** 数组 `nums` 。请你判断是否可以将这个数组分割成两个子集，使得两个子集的元素和相等。

 

**示例 1：**

```
输入：nums = [1,5,11,5]
输出：true
解释：数组可以分割成 [1, 5, 5] 和 [11] 。
```

**示例 2：**

```
输入：nums = [1,2,3,5]
输出：false
解释：数组不能分割成两个元素和相等的子集。
```

 

**提示：**

- `1 <= nums.length <= 200`
- `1 <= nums[i] <= 100`

***

```java
class Solution {
    public boolean canPartition(int[] nums) {
        if(nums==null||nums.length==0){
            return false;
        }
        int n=nums.length;
        int sum=0;
        for(int num:nums){
            sum+=num;
        }
        if((sum&1)!=0){
            return false;
        }
        int target=sum/2;
        boolean[][] dp=new boolean[n+1][target+1];
        for(int i=0;i<=n;i++){
            dp[i][0]=true;
        }
        for(int i=n-1;i>=0;i--){
            for(int j=1;j<=target;j++){
                dp[i][j]=dp[i+1][j]||(j-nums[i]>=0?dp[i+1][j-nums[i]]:false);
            }
        }
        return dp[0][target];
    }
}

```



[437. 路径总和 III](https://leetcode.cn/problems/path-sum-iii/)

给定一个二叉树的根节点 `root` ，和一个整数 `targetSum` ，求该二叉树里节点值之和等于 `targetSum` 的 **路径** 的数目。

**路径** 不需要从根节点开始，也不需要在叶子节点结束，但是路径方向必须是向下的（只能从父节点到子节点）。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/04/09/pathsum3-1-tree.jpg)

```
输入：root = [10,5,-3,3,2,null,11,3,-2,null,1], targetSum = 8
输出：3
解释：和等于 8 的路径有 3 条，如图所示。
```

**示例 2：**

```
输入：root = [5,4,8,11,null,13,4,7,2,null,null,5,1], targetSum = 22
输出：3
```

 

**提示:**

- 二叉树的节点个数的范围是 `[0,1000]`
- `-109 <= Node.val <= 109` 
- `-1000 <= targetSum <= 1000` 

***

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public int pathSum(TreeNode root, long targetSum) {
        if(root==null){
            return 0;
        }
        int result=rootSum(root,targetSum);
        result+=pathSum(root.left,targetSum);
        result+=pathSum(root.right,targetSum);
        return result;
    }

    private int rootSum(TreeNode root, long rest){
        if(root==null){
            return 0;
        }
        int result=0;
        if(root.val==rest){
            result++;
        }
        result+=rootSum(root.left,rest-root.val);
        result+=rootSum(root.right,rest-root.val);
        return result;
    }
}
```



[438. 找到字符串中所有字母异位词](https://leetcode.cn/problems/find-all-anagrams-in-a-string/)

给定两个字符串 `s` 和 `p`，找到 `s` 中所有 `p` 的 **异位词** 的子串，返回这些子串的起始索引。不考虑答案输出的顺序。

 

**示例 1:**

```
输入: s = "cbaebabacd", p = "abc"
输出: [0,6]
解释:
起始索引等于 0 的子串是 "cba", 它是 "abc" 的异位词。
起始索引等于 6 的子串是 "bac", 它是 "abc" 的异位词。
```

 **示例 2:**

```
输入: s = "abab", p = "ab"
输出: [0,1,2]
解释:
起始索引等于 0 的子串是 "ab", 它是 "ab" 的异位词。
起始索引等于 1 的子串是 "ba", 它是 "ab" 的异位词。
起始索引等于 2 的子串是 "ab", 它是 "ab" 的异位词。
```

 

**提示:**

- `1 <= s.length, p.length <= 3 * 104`
- `s` 和 `p` 仅包含小写字母

***

```java
class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        if(s==null||p==null||s.length()<p.length()){
            return new ArrayList<>();
        }
        char[] chs1=s.toCharArray();
        char[] chs2=p.toCharArray();
        List<Integer> resultList=new ArrayList<>();
        HashMap<Character,Integer> map=new HashMap<>();
        for(char c:chs2){
            map.put(c,map.getOrDefault(c,0)+1);
        }
        for(int i=0;i<chs1.length;i++){
            if(map.containsKey(chs1[i])){
                map.put(chs1[i],map.getOrDefault(chs1[i],0)-1);
            }
            boolean flag=true;
            for(int value:map.values()){
                if(value!=0){
                    flag=false;
                }
            }
            int eldestIndex=i-chs2.length+1;
            if(flag){
                resultList.add(eldestIndex);
            }
            if(eldestIndex>=0&&map.containsKey(chs1[eldestIndex])){
                map.put(chs1[eldestIndex],map.getOrDefault(chs1[eldestIndex],0)+1);
            }
        }
        return resultList;
    }
}
```



[560. 和为 K 的子数组](https://leetcode.cn/problems/subarray-sum-equals-k/)

给你一个整数数组 `nums` 和一个整数 `k` ，请你统计并返回 *该数组中和为 `k` 的子数组的个数* 。

子数组是数组中元素的连续非空序列。

 

**示例 1：**

```
输入：nums = [1,1,1], k = 2
输出：2
```

**示例 2：**

```
输入：nums = [1,2,3], k = 3
输出：2
```

 

**提示：**

- `1 <= nums.length <= 2 * 104`
- `-1000 <= nums[i] <= 1000`
- `-107 <= k <= 107`

***

```java
class Solution {
    public int subarraySum(int[] nums, int target) {
        if(nums==null||nums.length==0){
            return 0;
        }
        Map<Integer,Integer> map=new HashMap<>();
        map.put(0,1);
        int count=0;
        int sum=0;
        for(int num:nums){
            sum+=num;
            if(map.containsKey(sum-target)){
                count+=map.get(sum-target);
            }
            map.put(sum,map.getOrDefault(sum,0)+1);
        }
        return count;
    }
}
```



[739. 每日温度](https://leetcode.cn/problems/daily-temperatures/)
给定一个整数数组 `temperatures` ，表示每天的温度，返回一个数组 `answer` ，其中 `answer[i]` 是指对于第 `i` 天，下一个更高温度出现在几天后。如果气温在这之后都不会升高，请在该位置用 `0` 来代替。

 

**示例 1:**

```
输入: temperatures = [73,74,75,71,69,72,76,73]
输出: [1,1,4,2,1,1,0,0]
```

**示例 2:**

```
输入: temperatures = [30,40,50,60]
输出: [1,1,1,0]
```

**示例 3:**

```
输入: temperatures = [30,60,90]
输出: [1,1,0]
```

 

**提示：**

- `1 <= temperatures.length <= 105`
- `30 <= temperatures[i] <= 100`

***

```java
class Solution {
    public int[] dailyTemperatures(int[] temperatures) {
        if(temperatures==null||temperatures.length==0){
            return new int[]{};
        }
        int[] result=new int[temperatures.length];
        Stack<Integer> stack=new Stack<>();
        for(int i=0;i<temperatures.length;i++){
            while(!stack.isEmpty()&&temperatures[i]>temperatures[stack.peek()]){
                int index=stack.pop();
                result[index]=i-index;
            }
            stack.push(i);
        }
        return result;

    }
}
```

## 困难级别



[4. 寻找两个正序数组的中位数](https://leetcode.cn/problems/median-of-two-sorted-arrays/)
给定两个大小分别为 `m` 和 `n` 的正序（从小到大）数组 `nums1` 和 `nums2`。请你找出并返回这两个正序数组的 **中位数** 。

算法的时间复杂度应该为 `O(log (m+n))` 。

 

**示例 1：**

```
输入：nums1 = [1,3], nums2 = [2]
输出：2.00000
解释：合并数组 = [1,2,3] ，中位数 2
```

**示例 2：**

```
输入：nums1 = [1,2], nums2 = [3,4]
输出：2.50000
解释：合并数组 = [1,2,3,4] ，中位数 (2 + 3) / 2 = 2.5
```

 

 

**提示：**

- `nums1.length == m`
- `nums2.length == n`
- `0 <= m <= 1000`
- `0 <= n <= 1000`
- `1 <= m + n <= 2000`
- `-106 <= nums1[i], nums2[i] <= 106`

***

```java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int m=nums1.length;
        int n=nums2.length;
        if(m>n){
            return findMedianSortedArrays(nums2,nums1);
        }
        int low=0;
        int high=m;
        while(low<=high){
            int i=(low+high)/2;
            int j=(m+n+1)/2-i;
            if(i!=0&&j!=n&&nums1[i-1]>nums2[j]){
                high=i-1;
            }else if(i!=m&&j!=0&&nums1[i]<nums2[j-1]){
                low=i+1;
            }else{
                int leftMax;
                if(i==0){
                    leftMax=nums2[j-1];
                }else if(j==0){
                    leftMax=nums1[i-1];
                }else{
                    leftMax=Math.max(nums1[i-1],nums2[j-1]);
                }
                if((m+n)%2==1){
                    return leftMax;
                }
                int rightMin;
                if(i==m){
                    rightMin=nums2[j];
                }else if(j==n){
                    rightMin=nums1[i];
                }else{
                    rightMin=Math.min(nums1[i],nums2[j]);
                }
                return (leftMax+rightMin)/2.0;
            }
        }
        return 0.0;
    }
}
```



[23. 合并 K 个升序链表](https://leetcode.cn/problems/merge-k-sorted-lists/)
给你一个链表数组，每个链表都已经按升序排列。

请你将所有链表合并到一个升序链表中，返回合并后的链表。

 

**示例 1：**

```
输入：lists = [[1,4,5],[1,3,4],[2,6]]
输出：[1,1,2,3,4,4,5,6]
解释：链表数组如下：
[
  1->4->5,
  1->3->4,
  2->6
]
将它们合并到一个有序链表中得到。
1->1->2->3->4->4->5->6
```

**示例 2：**

```
输入：lists = []
输出：[]
```

**示例 3：**

```
输入：lists = [[]]
输出：[]
```

 

**提示：**

- `k == lists.length`
- `0 <= k <= 10^4`
- `0 <= lists[i].length <= 500`
- `-10^4 <= lists[i][j] <= 10^4`
- `lists[i]` 按 **升序** 排列
- `lists[i].length` 的总和不超过 `10^4`

***

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        if(lists==null||lists.length==0){
            return null;
        }
        //ListNode[] pointerArr=new ListNode[lists.length];
        PriorityQueue<ListNode> queue=new PriorityQueue<>((o1,o2)->o1.val-o2.val);
        for(int i=0;i<lists.length;i++){
            //pointerArr[i]=lists[i];
            if(lists[i]!=null){
                queue.add(lists[i]);
            }
        }
        ListNode dummy=new ListNode(0);
        ListNode tail=dummy;
        while(!queue.isEmpty()){
            ListNode node=queue.poll();
            if(node.next!=null){
                queue.add(node.next);
            }
            tail.next=node;
            tail=tail.next;

        }
        return dummy.next;
    }
}
```



[32. 最长有效括号](https://leetcode.cn/problems/longest-valid-parentheses/)
给你一个只包含 `'('` 和 `')'` 的字符串，找出最长有效（格式正确且连续）括号 子串 的长度。

左右括号匹配，即每个左括号都有对应的右括号将其闭合的字符串是格式正确的，比如 `"(()())"`。

 

**示例 1：**

```
输入：s = "(()"
输出：2
解释：最长有效括号子串是 "()"
```

**示例 2：**

```
输入：s = ")()())"
输出：4
解释：最长有效括号子串是 "()()"
```

**示例 3：**

```
输入：s = ""
输出：0
```

 

**提示：**

- `0 <= s.length <= 3 * 104`
- `s[i]` 为 `'('` 或 `')'`

***

```java
class Solution {
    public int longestValidParentheses(String s) {
        if(s==null||s.length()==0){
            return 0;
        }
        int[] dp=new int[s.length()];
        int max=0;
        for(int i=1;i<s.length();i++){
            if(s.charAt(i)==')'){
                int matchIndex=i-dp[i-1]-1;
                if(matchIndex>=0&&s.charAt(matchIndex)=='('){
                    dp[i]=i-matchIndex+1+(matchIndex>0?dp[matchIndex-1]:0);
                    max=Math.max(max,dp[i]);
                }
            }
        }
        return max;
    }
}
```



[42. 接雨水](https://leetcode.cn/problems/trapping-rain-water/)
给定 `n` 个非负整数表示每个宽度为 `1` 的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。

 

**示例 1：**

![img](https://assets.leetcode.cn/aliyun-lc-upload/uploads/2018/10/22/rainwatertrap.png)

```
输入：height = [0,1,0,2,1,0,1,3,2,1,2,1]
输出：6
解释：上面是由数组 [0,1,0,2,1,0,1,3,2,1,2,1] 表示的高度图，在这种情况下，可以接 6 个单位的雨水（蓝色部分表示雨水）。 
```

**示例 2：**

```
输入：height = [4,2,0,3,2,5]
输出：9
```

 

**提示：**

- `n == height.length`
- `1 <= n <= 2 * 104`
- `0 <= height[i] <= 105`

***

```java
class Solution {
    public int trap(int[] height) {
        if(height==null||height.length<3){
            return 0;
        }
        int n=height.length;
        int leftMax=height[0];
        int rightMax=height[n-1];
        int low=1;
        int high=n-2;
        int result=0;
        while(low<=high){
            if(leftMax<=rightMax){
                if(height[low]<leftMax){
                    result+=leftMax-height[low];
                }else{
                    leftMax=height[low];
                }
                low++;
            }else{
                if(height[high]<rightMax){
                    result+=rightMax-height[high];
                }else{
                    rightMax=height[high];
                }
                high--;
            }
        }
        return result;
    }
}
```



[76. 最小覆盖子串](https://leetcode.cn/problems/minimum-window-substring/)
给定两个字符串 `s` 和 `t`，长度分别是 `m` 和 `n`，返回 s 中的 **最短窗口 子串**，使得该子串包含 `t` 中的每一个字符（**包括重复字符**）。如果没有这样的子串，返回空字符串 `""`。

测试用例保证答案唯一。

 

**示例 1：**

```
输入：s = "ADOBECODEBANC", t = "ABC"
输出："BANC"
解释：最小覆盖子串 "BANC" 包含来自字符串 t 的 'A'、'B' 和 'C'。
```

**示例 2：**

```
输入：s = "a", t = "a"
输出："a"
解释：整个字符串 s 是最小覆盖子串。
```

**示例 3:**

```
输入: s = "a", t = "aa"
输出: ""
解释: t 中两个字符 'a' 均应包含在 s 的子串中，
因此没有符合条件的子字符串，返回空字符串。
```

 

**提示：**

- `m == s.length`
- `n == t.length`
- `1 <= m, n <= 105`
- `s` 和 `t` 由英文字母组成

***

```java
class Solution {

    private Map<Character,Integer> destMap=new HashMap<>();
    private Map<Character,Integer> curMap=new HashMap<>();

    public String minWindow(String s, String t) {
        if(s==null||t==null||s.length()<t.length()){
            return "";
        }
        for(int i=0;i<t.length();i++){
            destMap.put(t.charAt(i),destMap.getOrDefault(t.charAt(i),0)+1);
        }
        int sLen=s.length();
        int left=0;
        int right=0;
        int start=-1;
        int end=-1;
        int min=Integer.MAX_VALUE;
        while(right<sLen){
            if(destMap.containsKey(s.charAt(right))){
                curMap.put(s.charAt(right),curMap.getOrDefault(s.charAt(right),0)+1);
            }
            while(isMatch()&&left<=right){
                if(right-left+1<min){
                    min=right-left+1;
                    start=left;
                    end=right;
                }
                if(destMap.containsKey(s.charAt(left))){
                    curMap.put(s.charAt(left),curMap.get(s.charAt(left))-1);
                }
                left++;
            }
            right++;
        }
        return min==Integer.MAX_VALUE?"":s.substring(start,end+1);
    }

    private boolean isMatch(){
        for(Map.Entry<Character,Integer> entry:destMap.entrySet()){
            char c=entry.getKey();
            int value=entry.getValue();
            if(curMap.getOrDefault(c,0)<value){
                return false;
            }
        }
        return true;
    }
}
```



[84. 柱状图中最大的矩形](https://leetcode.cn/problems/largest-rectangle-in-histogram/)

给定 *n* 个非负整数，用来表示柱状图中各个柱子的高度。每个柱子彼此相邻，且宽度为 1 。

求在该柱状图中，能够勾勒出来的矩形的最大面积。

 

**示例 1:**

![img](https://assets.leetcode.com/uploads/2021/01/04/histogram.jpg)

```
输入：heights = [2,1,5,6,2,3]
输出：10
解释：最大的矩形为图中红色区域，面积为 10
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2021/01/04/histogram-1.jpg)

```
输入： heights = [2,4]
输出： 4
```

 

**提示：**

- `1 <= heights.length <=105`
- `0 <= heights[i] <= 104`

***

```java
class Solution {
    public int largestRectangleArea(int[] heights) {
        if(heights==null||heights.length==0){
            return 0;
        }
        Stack<Integer> stack=new Stack<>();
        int max=0;
        for(int i=0;i<heights.length;i++){
            while(!stack.isEmpty()&&heights[stack.peek()]>heights[i]){
                int height=heights[stack.pop()];
                int width=i-(!stack.isEmpty()?stack.peek()+1:0);
                max=Math.max(max,width*height);
            }
            stack.push(i);
        }
        while(!stack.isEmpty()){
            int height=heights[stack.pop()];
            int width=heights.length-(!stack.isEmpty()?stack.peek()+1:0);
            max=Math.max(max,width*height);
        }
        return max;
    }
}
```



[124. 二叉树中的最大路径和](https://leetcode.cn/problems/binary-tree-maximum-path-sum/)
二叉树中的 **路径** 被定义为一条节点序列，序列中每对相邻节点之间都存在一条边。同一个节点在一条路径序列中 **至多出现一次** 。该路径 **至少包含一个** 节点，且不一定经过根节点。

**路径和** 是路径中各节点值的总和。

给你一个二叉树的根节点 `root` ，返回其 **最大路径和** 。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/10/13/exx1.jpg)

```
输入：root = [1,2,3]
输出：6
解释：最优路径是 2 -> 1 -> 3 ，路径和为 2 + 1 + 3 = 6
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2020/10/13/exx2.jpg)

```
输入：root = [-10,9,20,null,null,15,7]
输出：42
解释：最优路径是 15 -> 20 -> 7 ，路径和为 15 + 20 + 7 = 42
```

 

**提示：**

- 树中节点数目范围是 `[1, 3 * 104]`
- `-1000 <= Node.val <= 1000`

***

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {

    int result=Integer.MIN_VALUE;

    public int maxPathSum(TreeNode root) {
        if(root==null){
            return 0;
        }
        processMaxPathSum(root);
        return result;
    }

    private int processMaxPathSum(TreeNode root){
        if(root==null){
            return 0;
        }
        int r1=Math.max(processMaxPathSum(root.left),0);
        int r2=Math.max(processMaxPathSum(root.right),0);
        result=Math.max(result,root.val+r1+r2);
        return root.val+Math.max(r1,r2);

    }
}
```



[239. 滑动窗口最大值](https://leetcode.cn/problems/sliding-window-maximum/)

给你一个整数数组 `nums`，有一个大小为 `k` 的滑动窗口从数组的最左侧移动到数组的最右侧。你只可以看到在滑动窗口内的 `k` 个数字。滑动窗口每次只向右移动一位。

返回 *滑动窗口中的最大值* 。

 

**示例 1：**

```
输入：nums = [1,3,-1,-3,5,3,6,7], k = 3
输出：[3,3,5,5,6,7]
解释：
滑动窗口的位置                最大值
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
```

**示例 2：**

```
输入：nums = [1], k = 1
输出：[1]
```

 

**提示：**

- `1 <= nums.length <= 105`
- `-104 <= nums[i] <= 104`
- `1 <= k <= nums.length`

***

```java
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        if(nums==null||nums.length<k){
            return new int[]{};
        }
        Deque<Integer> queue=new LinkedList<>();
        int[] result=new int[nums.length-k+1];
        int index=0;
        for(int i=0;i<nums.length;i++){
            while(!queue.isEmpty()&&nums[queue.peekLast()]<=nums[i]){
                queue.pollLast();
            }
            queue.addLast(i);
            if(queue.peekFirst()==i-k){
                queue.pollFirst();
            }
            if(i>=k-1){
                result[index++]=nums[queue.peekFirst()];
            }
        }
        return result;
    }
}
```

