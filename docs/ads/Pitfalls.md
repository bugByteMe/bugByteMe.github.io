* 如果时间复杂度，在O下正确，但是不紧，可以考虑该选项正确
* P 类问题一定是 NP问题
* 仔细阅读题目描述
* 不要揪住一个选项漏洞不放，选最好的
* Full binary tree vs complete binary tree
* $\sum logn = O(nlogn)$ 
* 不要忘记维护NPL，在合并完leftist heap
* 不会算的递归，不会算的概率，不要死算，可以尝试带入特殊值，取极限，或substitution method
* BST 删节点时，先检查是否只有一个儿子，如果是，直接拿上来
* Splay删除，先把要删除的节点splay到根，再删（L or R都行），选择替换者后，再把替换者转上来
* not FPTAS: $O(n^{2/\epsilon})$
* $(1+\epsilon) - approximation$ may not tight!! May only needs to deny it.
* 关注题目变化，可能参数会有改变，不要无脑套
* 注意Binomial queue的carry是哪个