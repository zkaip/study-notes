Tree 树
===
### 1. 树
根、子树、结点、孩子、双亲、兄弟、堂兄弟、祖先、子孙
结点的度：结点拥有的子树数
树的度：树内各结点度的最大值
树的深度(高度)：结点的最大层次
叶子(终端结点)：度为0的节点
分支节点(非终端结点)：度不为0的节点

### 2. 二叉树
特点：每个结点至多有两个子树，且子树有左右之分。
性质1：二叉树的第i层上至多有2i-1个结点(i>=1)。
性质2：深度为k的二叉树至多有2k-1个结点(k>=1)。
性质3：任一二叉树，如果叶子结点数为n0，度为2的节点数为n2，则n0=n2+1。

### 3. 完全二叉树
满二叉树：每一层上的结点数都是最大结点数。
完全二叉树：只从满二叉树的最后剥离结点的二叉树。
特点：叶子结点只可能在层次最大的两层上出现；任一结点，若其右分支的深度为L，则其左分支的深度必为L或L+1。
性质4：具有n个结点的完全二叉树的深度为[log2n]+1。
性质5：对有n个结点的完全二叉树编号，从上到下，从左到右，则对节点i(1<=i<=n)，有：
1) 如果i=1，结点为根，无双亲；如果i>1，双亲为结点(i/2)。
2) 如果2i>n，则结点i无左孩子；否则左孩子为2i。
3) 如果2i+1>n，则结点i无右孩子；否则右孩子为2i+1。