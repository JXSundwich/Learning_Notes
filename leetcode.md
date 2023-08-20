### 数组
#### 二分查找数组
一开始一定要定义好区间的定义
一般是[left,right]或者[left,righ) 我更喜欢前者

以[left,right]为例子：
- while(left<=right)要使用<=因为left==right是有意义的
- if(nums[mid]>target) right要赋值为mid-1因为当前这个nums[mid]一定不是target

#### 原地移除指定元素
快慢指针
- 快指针遍历数组
- 慢指针指向新数组末尾
- 当快指针指向数组末尾时 返回慢指针

#### 长度最小的子数组
- 滑动窗口（双指针）

总结：
![img.png](img.png)

#### 哈希
可以用数组的时候就用数组（规定了数据范围且不是过大）
用数组比Set快 因为使用Set每次都需要计算hash值
<font color=red>几数之和这种问题，先给数组排序，前n-1个指针依次往后走，最后一个指针从最后往前走</font>

#### 字符串
- 可以使用trim函数去除掉字符串的前后的空格

KMP算法
- next数组
```java
  public int[] next(String s) {
        int[] res = new int[s.length()];
        res[0] = 0;
        int j = 0;
        for (int i = 1; i < s.length(); i++) {
            while (j > 0 && s.charAt(i) != s.charAt(j)) {
                j = res[j - 1];
            }
            if (s.charAt(i) == s.charAt(j)) {
                j++;
            }
            res[i] = j;
        }
        return res;
    }
```

- 使用next回退
```java
 public int strStr(String haystack, String needle) {
        int[] next = next(needle);
        int j = 0;
        for (int i = 0; i < haystack.length(); i++) {
            while (j > 0 && haystack.charAt(i) != needle.charAt(j)) {
                j = next[j - 1];
            }
            if(haystack.charAt(i)==needle.charAt(j)){
                j++;
            }
            if(j==needle.length()){
                return i-j+1;
            }
        }
        return -1;
    }
```

#### 树
- 递归遍历三要素
1. 确定递归函数的参数和返回值
2. 确定终止条件
3. 确定单层递归的逻辑

迭代写法：
都需要使用到栈
- 前序遍历
```java
// 前序遍历顺序：中-左-右，入栈顺序：中-右-左
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        if (root == null){
            return result;
        }
        Stack<TreeNode> stack = new Stack<>();
        stack.push(root);
        while (!stack.isEmpty()){
            TreeNode node = stack.pop();
            result.add(node.val);
            if (node.right != null){
                stack.push(node.right);
            }
            if (node.left != null){
                stack.push(node.left);
            }
        }
        return result;
    }
}
```
- 中序遍历
```java
// 中序遍历顺序: 左-中-右 入栈顺序： 左-右
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        if (root == null){
            return result;
        }
        Stack<TreeNode> stack = new Stack<>();
        TreeNode cur = root;
        while (cur != null || !stack.isEmpty()){
           if (cur != null){
               stack.push(cur);
               cur = cur.left;
           }else{
               cur = stack.pop();
               result.add(cur.val);
               cur = cur.right;
           }
        }
        return result;
    }
}
```
- 后序遍历
```java
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        if (root == null){
            return result;
        }
        Stack<TreeNode> stack = new Stack<>();
        stack.push(root);
        while (!stack.isEmpty()){
            TreeNode node = stack.pop();
            result.add(node.val);
            if (node.left != null){
                stack.push(node.left);
            }
            if (node.right != null){
                stack.push(node.right);
            }
        }
        Collections.reverse(result);
        return result;
    }
}
```

