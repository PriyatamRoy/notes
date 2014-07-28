## Reverse Words in a String

算法：先反转整个string，然后反转每个word。
难点：根据要求，注意预处理string(去除前后space, word之间多余一个的space等等)

## Evaluate Reverse Polish Notation

算法：基本的stack应用，遇到操作符就将stack元素拿出，进行计算后再把结果压栈。
难点：基本没有，注意代码简洁即可。


## Max Points on a Line

算法：枚举每条线段(n * (n-1)条)，计算斜率(slope)，然后用一个map，key是斜率，value是这条线上点的个数。最后找到value最大的就是结果
难点：1. 计算斜率的时候要考虑两点y相同斜率无限大的情况(可以用Double.MAX_VALUE)。
      2. 输入中可能有相同的点，比如(0, 0), (1, 1), (0, 0)这种情况，输出应该为3。可以用一个小技巧，在枚举时，使用双循环
         for (int i=0; i < points.length; ++i) {
           map = new map
           int numOfSamePoints = 1; // 这里用一个变量表示多少点和points[i]相同，因为这个点在外层循环只会被处理一遍
           int max = 0;
           for (int j=i+1; j < points.length; ++j) {
             if (points[i] == points[j]) {
               numOfSamePoints++;
             } else {
               map.put(slope, map.get(slope, 0) + 1);
             }
           }

           for (val : map.values()) {
             if (max < val + numOfSamePoints) {
               max = val + numOfSamePoints;
             }
           }
         }
         return max;

## Sort List

算法：先找list中点(用fast, slow point解法)，这样把list分成两部分，然后就可以做mergesort
难点：1. 找list中点的时候要考虑清楚哪里是中点。
      2. 做归并的时候代码要写对，因为是list，所以注意正确设置next(递归比较好写)。

## Insertion Sort List

算法：基本的插入排序
难点：注意因为是list，所以要从前往后搜索，并且因为是list，所以插入代码要考虑更新next指针。

## LRU Cache

算法：使用HashMap + LinkedList(Double)。HashMap的value是(Node, value), 其中Node是对应LinkedList里面的Node。
难点：1. 理解问题，get和set都要更新key在LinkedList里面的位置。
      2. HashMap的value是(Node, value)，这是为了要更新最近访问的key在LinkedList中的位置。
      3. LinkedList最好是双向链表，这样可以以O(1)的操作将最近访问的key更新
      4. 在set时，如果key已在HashMap里，只用更新key在LinkedList的顺序就行；否则则必须删除呆的最久的key
      5. DoubleLinkedList和Pair结构要自己实现。。。

## Binary Tree Postorder Traversal

算法：基本的后序遍历实现，先左再右然后当前节点。
难点：无

## Binary Tree Preorder Traversal

算法：基本的先序遍历实现，先当前节点再左然后右。
难点：无

## Reorder List

## Linked List Cycle II

算法：先通过快慢指针法确定链表是否有环。若有环，令一指针指向head，然后慢指针和该指针每次同时移动一个，直到他们指向同一个节点，该节点即为环入口。
难点：1. 快慢指针的退出条件(fast != null && fast != slow) 以及一开始要确保fast和slow已经移动(不指向head)
      2. 如何找出环入口。假设慢指针在遇到快指针时走了k个节点(所以快指针走了2k个节点)，环入口是第r个节点，环总共有i个节点。
         那么必然有：(k - r) % i == (2k - r) % i
         根据同余性质: a mod d = b mod d => (a - b) 整除 d
         所以这里 ((2k - r) - (k - r)) = k, k 整除 i
         所以设k = n * i
         那么慢指针离环入口有k - r = n*i - r个node
         所以如果令一指针从head开始走r个节点，并且慢指针也走r个节点，它们会在环入口相遇。
         换言之：如果它们相遇，从链表头开始走的指针指向环入口。

## Linked List Cycle

算法：快慢指针。
难点：快慢指针的退出条件(fast != null && fast != slow) 以及一开始要确保fast和slow已经移动(不指向head)

## Word Break II

## Word Break

## Copy List with Random Pointer

算法：三次循环。
      第一次：对于原链表每一个节点，复制一份添加到后后面。
      第二次：将复制节点的random赋予正确的值。
      第三次：分裂成两个链表，返回复制的链表。
      比如：如果原来链表是 A -> B -> C -> D
        第一遍之后的链表变为: A -> A' -> B -> B' -> C -> C' -> D -> D' (A'是A的拷贝，但是random还未赋值)
        第二遍便利链表，将A'.random设为A.random.next (A.random是原链表的某节点，根据算法，下一个必为其拷贝)
        第三遍分裂链表, 得到 A -> B -> C -> D 和 A' -> B' -> C' -> D'
难点：1. 注意random可能为null, 所以第二遍时，要在random不为null时才用X'.random = X.random.next
      2. 第三遍分裂链表，在设置拷贝节点的next时要注意，只有当该节点不是最后一个节点时，才用X'.next = X.next.next
         if (X.next != null) {
            X'.next = X.next.next;
         }

