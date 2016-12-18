---
layout: post
title: 字符串系列3--AC自动机和Trie图
categories: Algorithms
tags: HihoCoder
---

# 原题描述  

### 输入

每个输入文件有且仅有一组测试数据。

每个测试数据的第一行为一个整数N，表示河蟹词典的大小。

接下来的N行，每一行为一个由小写英文字母组成的河蟹词语。

接下来的一行，为一篇长度不超过M，由小写英文字母组成的文章。

对于60%的数据，所有河蟹词语的长度总和小于10, M<=10

对于80%的数据，所有河蟹词语的长度总和小于10^3, M<=10^3

对于100%的数据，所有河蟹词语的长度总和小于10^6, M<=10^6, N<=1000
输出

对于每组测试数据，输出一行"YES"或者"NO"，表示文章中是否含有河蟹词语。

### 样例输入

```
    6
    aaabc
    aaac
    abcc
    ac
    bcd
    cd
    aaaaaaaaaaabaaadaaac
```

### 样例输出

```
    YES
```

# 算法描述使用AC自动机或者进一步Trie图

具体多模式串匹配算法可以参考[CSDN博客](http://blog.csdn.net/mobius_strip/article/details/22549517)

实现思路：    
1. 实现Trie树，并用给定的模式串建立好相应的Trie树  
2. 建立Trie树的Fail指针，并建立Trie图  
3. 对给定的字符串，在Trie图中进行搜索  

建立Fail指针和Trie图的步骤：  

1. 使用BFS从根部遍历Trie树（使用队列），根节点的Fail指针为NULL，将root节点加入遍历的队列      
2. 遍历每个节点的时候判断其26个分支，若为空，则把相应的子分支，赋值成fail所指节点的相应子分支（如果当前节点是root，由于root节点的fail为NULL，所以该子分支赋值成root）。  
3. 若不为空，则顺着fail节点一直找到相应子分支不为空的节点，并将当前节点的子分支节点的fail指向刚才顺着fail找到的节点；若fail指向的节点是合法单词，那么当前节点也需要置成合法单词，为了防止漏数单词；将子分支节点加入队列，便于下一次BFS遍历  

> PS: 在步骤3当中，由于fail指向的一定是层数较低的节点，由于使用BFS和Trie图，所以层数较低的节点其子分支一定不为空，所以在该步骤中不需要顺着fail节点一直往上爬，只需要顺着fail指针走一次就可以了。  

搜索的步骤：  

1. 依次遍历要查询的字符串，并从字典树根部开始搜索  
2. 若找到某个节点是合法单词的时候，直接返回true
3. 若整个字符串都遍历完了，则返回false

## C++代码  

```c++
#include <iostream>
#include <cstring>
#include <queue>

using namespace std;

typedef struct trieNode {
    struct trieNode* next[26];
    bool valid;
    struct trieNode* fail;
}TrieNode, *Trie;

TrieNode* createTrieNode() {
    TrieNode* node = new TrieNode;
    memset(node->next, NULL, sizeof(node->next));
    node->fail = NULL;
    node->valid = false;
    return node;
}

void insertWord(Trie root, string word) {
    if (word.length() <= 0) {
        return;
    }
    int len = word.length();

    for (int i = 0; i < len; i++) {
        if (root->next[word[i] - 'a'] == NULL) {
            root->next[word[i] - 'a'] = createTrieNode();
        }
        root = root->next[word[i] - 'a'];
    }
    root->valid = true;
}

void buildTrieGraph(Trie root) {
    queue<TrieNode*> q;
    q.push(root);
    TrieNode *node, *p;
    while (!q.empty()) {
         node = q.front();
         q.pop();

         for (int i = 0; i < 26; i++) {
             if (node->next[i]) {
                 p = node->fail;
                 // 可以不加这个循环，由于是Trie图
                 while (p && p->next[i] == NULL) {
                     p = p->fail;
                 }
                 //----------------------------------------
                 node->next[i]->fail = p?p->next[i]:root;
                 if (p && p->next[i]->valid) {
                     node->next[i]->valid = true;
                 }
                 q.push(node->next[i]);
             }
             else {
                 // 建立Trie图，此为和AC自动机的区别
                 node->next[i] = (node==root)?root:node->fail->next[i];
             }
         }
    }
}

bool search(Trie root, string s) {
    if (s.length() <= 0) {
        return false;
    }
    int len = s.length();
    TrieNode *p = root;
    for (int i = 0; i < len; i++) {
        if (p->next[s[i] - 'a']->valid) {
            return true;
        }
        p = p->next[s[i] - 'a'];
    }
    return false;
}

int main(void) {
    int nums;
    Trie root = createTrieNode();
    string s;
    cin >> nums;
    while (nums--) {
        cin >> s;
        insertWord(root, s);
    }
    buildTrieGraph(root);
    cin >> s;
    if (search(root, s)) {
        cout << "YES" << endl;
    }
    else {
        cout << "NO" << endl;
    }

    return 0;
}
```

-------------------------

## Bug Free Procedure  从记事本直接写代码  

