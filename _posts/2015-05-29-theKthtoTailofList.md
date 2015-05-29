---
layout: post
title: 找到链表中的倒数第K个节点
category:  coding
---
 剑指offer 中的面试编程题，求链表中的倒数第K个结点。

 >其中一个思路就是，使用两个指针，第一个指针先走k-1步，然后再两个指针一起走，当头一个指针走到队尾时，那么后一个指针此刻就是倒数第K个。

 需要考虑的情况，输入空链表；K等于0；K大于链表长度。

 程序如下：


 	#include <iostream>
	using namespace std;
	template <typename T>
	struct ListNode
	{
	public:
	    T value;
	    ListNode *m_pNext;
 	   ListNode(int k):value(k),m_pNext(NULL){}
	};
	template <typename T>
	ListNode<T>* FindtheKthToTail(ListNode<T>* pListhead, unsigned int k)
	{
	    if(pListhead==NULL || k==0)
	        return NULL;
	    ListNode<T> *pAhead=pListhead;
	    ListNode<T> *pBehind=NULL;
	    for (unsigned int i=0; i<k-1;++i)
	    {
	        if(pAhead->m_pNext != NULL)
	            pAhead = pAhead->m_pNext;
	        else
	        {
	            return NULL;
	        }
	    }
	    pBehind = pListhead;
	    while(pAhead->m_pNext != NULL)
	    {
	        pAhead = pAhead->m_pNext;
	        pBehind = pBehind->m_pNext;
	    }
	    return pBehind;
	}
	int main()
	{
	ListNode<int> *numlist=new ListNode<int>(0);
	unsigned int k=1;
	ListNode<int> *theone,*temp,*next;
	temp=numlist;
	for(int i=1;i<10;)
	{
	    next=new ListNode<int>(i++);
	    temp->m_pNext=next;
	    temp=next;
	}
	theone=FindtheKthToTail(numlist,k);
	cout<<theone->value;
	}
