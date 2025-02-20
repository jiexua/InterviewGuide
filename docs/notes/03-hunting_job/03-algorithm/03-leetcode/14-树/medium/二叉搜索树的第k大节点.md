---
layout:  post
category:  hunting_job
title: 二叉搜索树的第k大节点
tagline:  by 阿秀
tags:
    - 原创
    - LeetCode
    - 校招
    - 社招
    - 阿秀
excerpt: 二叉搜索树的第k大节点
comment: false
---





<div style="border-color: #24C6DC;
            background-color: #e9f9f3;         
            margin: 1rem 0;
        padding: .25rem 1rem;
        border-left-width: .3rem;
        border-left-style: solid;
        border-radius: .5rem;
        color: inherit;">
  <p>这是四则或许对你有些许帮助的信息:</p>
  <p>1、👉 最近我发现了一个每日都会推送最新校招资讯的《校招日程》文档，其中包括<a style="text-decoration: underline" href="https://flowus.cn/share/ee50d5eb-3cd5-4f74-880e-95b215dd4ff2" target="_blank">往届补录</a>、<a style="text-decoration: underline" href="https://flowus.cn/share/5f327c98-1e31-46c8-b86b-5ac6105e021f" target="_blank">应届实习校招</a>信息，比如各种大厂、国企、银行、事业编等信息都会定期更新，帮忙扩散一下。</p>  
  <p>2、😍
    免费分享阿秀个人学习计算机以来收集到的免费学习资源，<a style="text-decoration: underline" href="/notes/07-resources/01-free/01-introduce.html" target="_blank">点此白嫖</a>；也记录一下自己以前买过的<a style="text-decoration: underline" href="/notes/07-resources/02-precious.html" target="_blank">不错的计算机书籍、网络专栏和垃圾付费专栏</a>。
  </p>
  <p>3、🚀如果你想在校招中顺利拿到更好的offer，阿秀建议你多看看前人<a style="text-decoration: underline" href="https://www.yuque.com/tuobaaxiu/httmmc/npg1k81zeq4wfpyz" target="_blank">踩过的坑</a>和<a style="text-decoration: underline"  target="_blank" href="https://www.yuque.com/tuobaaxiu/httmmc/gge9ppd0mbu2d3dp">留下的经验</a>，事实上你现在遇到的大多数问题你的学长学姐师兄师姐基本都已经遇到过了。
  </p>
  <p>4、🔥 欢迎准备计算机校招的小伙伴加入我的<a  style="text-decoration: underline" href="https://www.yuque.com/tuobaaxiu/httmmc/xg0otqvc17wfx4u9" target="_blank">学习圈子</a>，一个人踽踽独行不如一群人报团取暖，圈子里沉淀了很多过去21/22/23届学长学姐的<a  style="text-decoration: underline" href="https://www.yuque.com/tuobaaxiu/httmmc/gge9ppd0mbu2d3dp" target="_blank">经验和总结</a>，好好跟着走下去的，最后基本都可以拿到不错的offer！此外，每周都会进行<a  style="text-decoration: underline" href="https://www.yuque.com/tuobaaxiu/httmmc/npg1k81zeq4wfpyz" target="_blank">精华总结和分享！</a>如果你需要《阿秀的学习笔记》网站中📚︎校招八股文相关知识点的PDF版本的话，可以<a style="text-decoration: underline" href="https://www.yuque.com/tuobaaxiu/httmmc/qs0yn66apvkzw0ps" target="_blank">点此下载</a> 。</p>   </div>




## [二叉搜索树的第k大节点](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-di-kda-jie-dian-lcof/)

给定一棵二叉搜索树，请找出其中第k大的节点。

 

**示例 1:**

```
输入: root = [3,1,4,null,2], k = 1
   3
  / \
 1   4
  \
   2
输出: 4
```

**示例 2:**

```
输入: root = [5,3,6,2,4,null,null,1], k = 3
       5
      / \
     3   6
    / \
   2   4
  /
 1
输出: 4
```

 

**限制：**

1 ≤ k ≤ 二叉搜索树元素个数

### 1、后序遍历递归写法

执行用时：32 ms, 在所有 C++ 提交中击败了62.54%的用户

内存消耗：24.3 MB, 在所有 C++ 提交中击败了100.00%的用户

~~~C++
class Solution {
public:

	int count=0,result = 0;
	void kthLargestCore(TreeNode* root, int k) {
		if (root == nullptr) return ;
		if (root->right) kthLargestCore(root->right, k);
		count++;
		if (count == k) { result = root->val; return; }
		if (root->left) kthLargestCore(root->left, k);
		return;
	}
	int kthLargest(TreeNode* root, int k) {
		
		kthLargestCore(root, k);		
		return result;;
	}
};
~~~



### 2、后序遍历递归写法，改进一点

~~~C++
class Solution {
public:
	int result = 0;
	void kthLargestCore(TreeNode* root, int &k) {
		if (root == nullptr) return ;
		if (root->right) kthLargestCore(root->right, k);
		k--;
		if (k == 0) { result = root->val; return; }
		if (root->left) kthLargestCore(root->left, k);
		return;
	}
	int kthLargest(TreeNode* root, int k) {
		
		kthLargestCore(root, k);		
		return result;;
	}
};
~~~









### 3、后序遍历迭代模板写法，这种方法还是比较快

执行用时：28 ms, 在所有 C++ 提交中击败了80.90%的用户

内存消耗：24.1 MB, 在所有 C++ 提交中击败了100.00%的用户

~~~C++
 int kthLargest(TreeNode* root, int k) {

        if (root == nullptr) return 0;
		int count = 0;
		stack<TreeNode*>s;
		while (!s.empty() || root != nullptr) {

			if (root != nullptr) {
				s.push(root);
				root = root->right;
			}
			else {
				root = s.top();
				s.pop();
				k--;
				if (k == 0) return root->val;
				root = root->left;

			}
		}
		return 0;

    }
~~~



