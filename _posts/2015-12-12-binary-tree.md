---
layout: post
title: Binary Tree 初始函数
description: "Sample post with a background image CSS override."
tags: [Binary Tree]
image:
   feature: abstract-10.jpg
---

首先我们所使用的是一个基于链表的二叉树,处理二叉树的问题,需要借助队列
{% highlight c++ %}
typedef struct node
{
	char data;
	struct node*lchild, *rchild;
}bitreeNodeCreate;

class bitree
{
public:
	bitreeNodeCreate *root;
	bitree(){ root = NULL; };
	~bitree(){};
	void create();
	void show();
};
{% endhighlight %}

这是二叉树的创建函数,创建二叉树
{% highlight c++ %}
void bitree::create()
{
	char tempChar;
	bitreeNodeCreate *nodeLine[MAXSIZETREE];//作为节点储存与处理的队列
	int parent = 1, enter = 0;//作为nodeLine队列的队首和队尾 
	//parent表示双亲节点 enter表示接受当前输入字符的元素在nodeLine队列的位置
	bitreeNodeCreate *tempNode;

	cout << "请输入想要创建的二叉树的元素 以#结尾 以@表空节点"<<endl;

	while (cin >> tempChar&&tempChar != '#')
	{
		tempNode = NULL;
		if (tempChar != '@')
		{
			tempNode = (bitreeNodeCreate*)malloc(sizeof(bitreeNodeCreate));
			tempNode->data = tempChar;
			tempNode->lchild = NULL;
			tempNode->rchild = NULL;//一定要对左右子节点进行该处理 否则内存会报错
		}
		//将输入的字符赋值给临时节点
		enter++;
		nodeLine[enter] = tempNode;
		if (enter == 1)
		{
			root = tempNode;
		}//将值赋给根节点
		else
		{
			if (tempNode&&nodeLine[parent])//确保输入节点和双亲节点不是NULL
			{
				if (enter % 2 == 0)
				{
					nodeLine[parent]->lchild = nodeLine[enter];//因为是指针,所以等价于nodeLine[parent]->lchild = tempNode;
				}
				else
				{
					nodeLine[parent]->rchild = nodeLine[enter]; 
				}//利用了二叉树的双亲节点与子节点序号上的2倍关系
			}
			if (enter % 2 == 1)
			{
				parent++;//左右子节点输入完毕 切换到下一个双亲节点
			}
		}
	}
}
{% endhighlight %}

使用举例
{% highlight c++ %}
bitree t;
t.create();
{% endhighlight %}
这是一个广度优先的遍历函数,同时也将二叉树的节点输出到屏幕上
{% highlight c++ %}
void bitree::show()
{
	bitreeNodeCreate *nodeLine[MAXSIZETREE];
	memset(nodeLine, 0, sizeof(nodeLine));
	bitreeNodeCreate *nodeNone;
	int parent = 0, son = 0;
	nodeLine[parent] = root;
	while (nodeLine[parent] != NULL)
	{
		if (nodeLine[parent]->data != '@')
		{
			cout << nodeLine[parent]->data<<" ";

			if (nodeLine[parent]->lchild != NULL)
			{
				son++;
				nodeLine[son] = nodeLine[parent]->lchild;
			}
			else
			{
				nodeNone = (bitreeNodeCreate*)malloc(sizeof(bitreeNodeCreate));
				nodeNone->data = '@';
				son++;
				nodeLine[son] = nodeNone;
			}

			if (nodeLine[parent]->rchild != NULL)
			{
				son++;
				nodeLine[son] = nodeLine[parent]->rchild;
			}
			else
			{
				nodeNone = (bitreeNodeCreate*)malloc(sizeof(bitreeNodeCreate));
				nodeNone->data = '@';
				son++;
				nodeLine[son] = nodeNone;
			}
		}
		else
		{
			cout << "@" << " ";
		}

		parent++;
	}
}
{% endhighlight %}

使用举例
{% highlight css %}
bitree t;
t.create();
t.show();
{% endhighlight %}

<div xmlns:cc="http://creativecommons.org/ns#" xmlns:dct="http://purl.org/dc/terms/" about="http://subtlepatterns.com" class="notice">Background images from <span property="dct:title">Subtle Patterns</span> (<a rel="cc:attributionURL" property="cc:attributionName" href="http://subtlepatterns.com">Subtle Patterns</a>) / <a rel="license" href="http://creativecommons.org/licenses/by-sa/3.0/">CC BY-SA 3.0</a></div>