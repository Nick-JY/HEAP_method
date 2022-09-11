# 堆（heap）：
### 一、满二叉树的定义介绍：
[![jyICSs.png](https://s1.ax1x.com/2022/07/11/jyICSs.png)](https://imgtu.com/i/jyICSs)__如上图所示，即为一个满二叉树。__ 
- 满二叉树指的是，对于一个二叉树的任意一个节点，左右子树的高度相同；除了二叉树中的叶结点之外，其他的每一个节点的子节点数量都为2。
### 二、完全二叉树的定义介绍：
[![jyohPe.png](https://s1.ax1x.com/2022/07/11/jyohPe.png)](https://imgtu.com/i/jyohPe)__上图表示一个完全二叉树__ 
- 这里我们可以很明显的发现，完全二叉树的定义相比于满二叉树要更加的宽泛。
- 对于一个二叉树，除去最后一层之后，是一个满二叉树，最后一层节点的顺序是从左往右依次连接的，那么我们就称这个树为完全二叉树。
__完全二叉树节点的添加：__ 
针对上面这棵树，要添加一个新的节点，只能在最后一层按照从左向右的顺序添加。
[![j63ajK.png](https://s1.ax1x.com/2022/07/11/j63ajK.png)](https://imgtu.com/i/j63ajK)
##### 完全二叉树的存储结构：
- 对于完全二叉树的存储结构，我们抛弃了树形结构，取而代之的是线性结构，原因在于对于完全二叉树来讲，我们可以从顶部按照从上到下、从左到右的顺序进行编号，由于每个编号的位置都不可能为空，因此可以用一维数组来存储一个完全二叉树。
[![j6thkD.png](https://s1.ax1x.com/2022/07/11/j6thkD.png)](https://imgtu.com/i/j6thkD)
- 使用一维数组来存储完全二叉树最大的好处是便于查找，对于某一个节点，我们可以很快的查找到该节点的子节点和父节点。
- 对于8这个节点，其存储的下标是2，其父节点的下标是：（2-1）/ 2，其左子节点的下标是：2 * 2 + 1，右子节点的下标是：2 * 2 + 2。
### 三、堆的定义：
___堆本质上是一个完全二叉树，我们之前接触过队列，队列是用一个一维数组进行存储，遵循时间顺序的先进先出；由于堆也是一个一维数组，因此堆也可以叫做一个“优先队列”，按照权重大小来出队列，最大堆每次出队列的是整个堆中的最大元素，最小堆每次出队列的是整个堆中的最小元素。___ 
- 最大堆指的是：完全二叉树中的每一个节点的值都大于其子节点的值。
- 最小堆指的是：完全二叉树中的每一个节点的值都小于其子节点的值。
___接下来我们重点讨论最大堆：___ 
### 四、堆的创建：
由于堆是建立在完全二叉树的基础上的一种数据结构，因此创建一个堆包括两步，首先创建一个完全二叉树，其次将完全二叉树转换成堆。
[![j50DeK.png](https://s1.ax1x.com/2022/07/16/j50DeK.png)](https://imgtu.com/i/j50DeK)
- 第一步：我们首先需要创建出上图所示的完全二叉树。
- 第二步，我们将上面的完全二叉树转换成一个最大堆，也就是对于堆中的任意节点，其值都要大于其子节点的值。
___如何进行第二步是问题的关键，我们的做法是从最后一个不是叶节点的节点开始(上图中的3)，以该节点为根节点，依次构造堆，最终会将整个完全二叉树构建成堆。___ 
###### 具体步骤：
- 对于每个根节点来讲，我们在对其进行调整的时候都要要求它的左右子树是一个最大堆。
- 如果该根节点是叶结点，那么其本身就是一个最大堆，因此我们调整的时候应该从最后一个不是叶结点的节点开始调整，这样才能确保每次进行调整的时候都符合其左右子树是一个最大堆。
- 首先，对以3为根节点的树进行调整，以3为根节点的树是一个堆。
- 其次，对以10为根节点的树进行调整，以10为根节点的树是一个堆。
- 其次，对以4为根节点的树进行调整，以4为根节点的树不是一个堆，因此找到4的子节点中最大的一个（4的子节点只有10和3），将4和10交换位置。
[![j5hO5n.png](https://s1.ax1x.com/2022/07/17/j5hO5n.png)](https://imgtu.com/i/j5hO5n)
- 由于交换节点导致原来以10为根节点的堆被破坏，因此我们现在需要调整以4为根节点的树，将4和5交换位置，最终形成了一个正确的最大堆。
[![j549rF.png](https://s1.ax1x.com/2022/07/17/j549rF.png)](https://imgtu.com/i/j549rF)
___重点：我们把每次调整的过程叫做下沉，因为如果根节点的值小于其子节点的值，那么我们都会把根节点和子节点的值进行交换，也就是根节点做了下沉操作。并且下沉操作是从最后一个非叶结点的节点开始，按照5-4-3-2-1-0的顺序一直到第0个节点。___ 
###### 堆的表示方法：
- 使用一个结构体来表示一个堆；
- 这个结构体中包含一个指向数组的指针，用来存储堆中的元素：elem；
- 其次，包含数组的大小，也就是整个堆的最大容量：Capacity；
- 最后，要包含一个堆中元素的数量，即当前堆的大小：Size。
```C
struct heap{
    int* elem;
    int Capacity;
    int Size;
};
typedef struct heap* MaxHeap;
```

```C
MaxHeap Build_Heap(void)
{
    MaxHeap heap = (MaxHeap)malloc(sizeof(struct heap));
    if (!heap)
    {
        fprintf(stdout,"malloc apply failed!\n");
        exit(1);
    }
    printf("Please input the heap capacity:\n");
    scanf("%d" , &heap->Capacity);
    printf("Please input the heap size:\n");
    scanf("%d" , &heap->Size);
    heap->elem = (int*)malloc(sizeof(int) * heap->Size);
    if (!heap->elem)
    {
        fprintf(stdout,"malloc apply failed!\n");
        exit(1);
    }
    printf("Please input the elem:\n");
    for (int i = 0 ; i < heap->Size ; i++)
        scanf("%d" , &heap->elem[i]);
	//上面的操作创建了一个完全二叉树。
 
    int last = ((heap->Size - 1) - 1) / 2;
    //这句可以让我们得到整个完全二叉树的最后一个非叶结点的节点。
    for (int i = last ; i >= 0 ; i--)
        Shift_Down(heap , i);
    //随后从最后一个非叶结点的节点开始，从后向前，对以每一个节点为根节点的完全二叉树进行下沉操作。为何从最后一个非叶结点的节点开始？这样做可以保障我们每一次进行下沉操作的时候，根节点的左树和根节点的右树都是一个正确的最大堆。
    return heap;
}

void Shift_Down(MaxHeap heap , int start)
{
//下沉操作，start表示的是要进行下沉操作的完全二叉树的根节点的下标。
    int left = 2 * start + 1;
    int right = 2 * start + 2;
    int max = start;
    if (left < heap->Size && heap->elem[left] > heap->elem[max])
        max = left;
    if (right < heap->Size && heap->elem[right] > heap->elem[max])
        max = right;
    //上述操作找到根节点及其子节点中最大的那个节点的下标。
    if (max != start)
    {
        Swap(heap , max , start);
        Shift_Down(heap , max);
    //如果根节点的值就是最大的，那么说明该根节点形成的完全二叉树就是一个堆，不需要进行下沉操作，根本还是取决于start是从最后一个非叶结点的节点开始的。如果根节点的值不是最大的，那么就需要将根节点和其最大子节点进行位置的交换，并且由于可能对交换位置的堆造成破坏，因此还需要对交换位置进行下沉操作。
    }
//递归的退出条件，当max = start，也就是根节点的值是最大的时候，开始回溯就可以了。
    return;
}

void Swap(MaxHeap heap , int a , int b)
{
    heap->elem[a] = heap->elem[a] ^ heap->elem[b];
    heap->elem[b] = heap->elem[a] ^ heap->elem[b];
    heap->elem[a] = heap->elem[a] ^ heap->elem[b];
    
    return;
}
```
### 五、堆顶元素的删除与堆排序：
[![j549rF.png](https://s1.ax1x.com/2022/07/17/j549rF.png)](https://imgtu.com/i/j549rF)
- 我们根据整个堆的最大元素就是堆顶元素这一结论来进行堆排序。
- 对于上面所示的堆，我们首先取出10这个元素，然后把堆中最后一个元素2放到堆顶，此时我们需要注意，把2放到堆顶破坏了整个堆的结构，但是以5为根节点的堆和以3为根节点的堆并没有被破坏，因此我们只需要对新的根节点进行一次下沉操作即可。这也是为什么我们要把最后一个元素放到堆顶。（此时就已经成功的完成了堆顶元素的删除）
- 注意，我们把堆顶拿走之后，需要将节点数减一。
- 当节点数为0的时候说明堆排序完成。
###### 删除堆顶元素：
```C
int Delete(MaxHeap heap)
{
    if (!heap->Size)
    {
        fprintf(stdout,"The heap is empty\n");
        exit(1);
    }
    else
    {
        int max = heap->elem[0];
        heap->elem[0] = heap->elem[--heap->Size];
	    //由于删除了堆顶元素，因此元素的个数要减一，并且把最后一个元素放到堆顶。
        Shift_Down(heap , 0);
        //由于更改后的堆的结构大概率是被破坏的，因此我们需要对新的堆顶做下沉操作，来形成一个新的正确的堆。
        return max;
    }
}
```
###### 堆排序：
```C
void Heap_Sort(MaxHeap heap)
{
    while (heap->Size > 0)
    {
        int max = Delete(heap);
        //不断的对堆顶元素进行删除操作，并将删除的元素进行打印，当堆中所有元素都被删除之后，堆排序完成。
        printf("%d  " , max);
    }
    
    return;
}
```
##### 特别注意：
对于上面创建的堆排序代码，排序完成后会破坏原来堆的结构（存在值的交换），如果想避免这种情况，我们可以传递堆的副本，而不是传递指向堆的指针。
### 六、向堆中插入元素：
###### 重点注意：
我们在做堆的插入的时候，堆需要足够大才能进行插入操作，即一开始创建堆的时候就要有足够大的空间。
[![j549rF.png](https://s1.ax1x.com/2022/07/17/j549rF.png)](https://imgtu.com/i/j549rF)我们向这个堆中插入元素：15
[![jIFzDO.png](https://s1.ax1x.com/2022/07/17/jIFzDO.png)](https://imgtu.com/i/jIFzDO)我们可以发现，15按顺序放到堆的最后，但是这个堆的结构被破坏了，我们通过将15不断的”上浮“，也就是如果比其父节点大，那么我们就将它和它的父节点更换位置,最终重新形成一个稳定的堆。
```C
void Insert(MaxHeap heap , int x)
{
    if (heap->Size == heap->Capacity)
    {
        fprintf(stdout,"The heap is full!\n");
        exit(1);
    }
    else
    {
        heap->elem[heap->Size++] = x;
        //向堆中插入元素之后，当前堆中的元素个数要加一。
        Shift_Up(heap);
        //由于插入新的元素之后，堆的结构很可能被破坏，因此我们需要对新插入的元素进行上浮操作。
        
        return;
    }
}
void Shift_Up(MaxHeap heap)
{
    int thisnode = heap->Size - 1;
    int parent = (thisnode - 1) / 2;
    while (thisnode > 0 && heap->elem[thisnode] > heap->elem[parent])
    {
    //循环条件可以是当前节点的坐标，也可以是当前节点的父节点的坐标。
        Swap(heap , thisnode , parent);
        thisnode = parent;
        parent = (thisnode - 1) / 2;
    }

    return;
}
```