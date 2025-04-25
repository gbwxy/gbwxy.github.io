# PriorityQueue
Java 中的 PriorityQueue 是基于一个 最小堆（Min-Heap） 实现的。

## 1. 核心数据结构：最小堆

最小堆 是一种特殊的树形数据结构，通常是 完全二叉树。
它的核心特性是：堆中任意父节点的值总是小于或等于其子节点的值。因此，堆的根节点（位于树的最顶端）总是整个堆中的最小元素。

## 2. 底层实现：数组
尽管堆是一个逻辑上的树形结构，但在 Java 的 PriorityQueue 中，它实际上是通过一个 数组（Array） 来存储元素的。这种存储方式非常高效，因为完全二叉树的特性使得树的结构可以直接映射到数组的连续空间中，而不需要额外的指针来维护父子关系。

对于数组中的任意一个索引 i (从 0 开始)，其左子节点的索引是 2 * i + 1，右子节点的索引是 2 * i + 2。
对于任意一个索引 i (除了根节点)，其父节点的索引是 (i - 1) / 2。

## 3. 主要操作及原理：

PriorityQueue 的主要操作包括添加元素 (offer / add) 和取出最小元素 (poll / remove)，它们都依赖于维护堆的特性（最小堆特性）。

### 添加元素 offer(E e)：

将新元素添加到数组的末尾（即找到第一个可用的位置）。
然后进行一个称为 "上浮" (sift up) 的操作：比较新添加的元素与其父节点的值。如果新元素小于父节点，就交换它们的位置。
重复这个过程，直到新元素大于或等于其父节点，或者到达堆的根部。
这个过程保证了在添加新元素后，整个堆仍然保持最小堆的特性。

### 取出最小元素 poll()：

由于最小元素总是在根节点（数组的第一个元素），所以先取出根节点的值。
为了填补根节点的位置，将数组的最后一个元素移到根节点。
然后进行一个称为 "下沉" (sift down) 的操作：比较新的根节点与其子节点的值。选择最小的那个子节点，如果根节点大于这个最小的子节点，就交换它们的位置。
重复这个过程，直到当前节点小于或等于其所有子节点，或者到达叶子节点。
这个过程保证了在移除最小元素后，整个堆仍然保持最小堆的特性。

### 查看堆顶元素 peek()：

这个操作很简单，直接返回数组的第一个元素（根节点）即可，不需要进行任何上浮或下沉操作。

## 总结：

Java 的 PriorityQueue 通过基于数组实现的最小堆来高效地管理元素的优先级。添加和删除元素的操作都需要进行 O(log n) 的时间复杂度来维护堆的特性（上浮或下沉），而查看堆顶元素只需要 O(1) 的时间复杂度。

你提供的片段中，ScheduledThreadPoolExecutor 使用了 DelayedWorkQueue，并提到它是一个“小根堆”，这正是 PriorityQueue 底层数据结构的体现，用于按照任务的延迟时间来排序执行。

## 源码
```
public class PriorityQueue<E> extends AbstractQueue<E>
implements java.io.Serializable {

    ...

    /**
     * Priority queue represented as a balanced binary heap: the two
     * children of queue[n] are queue[2*n+1] and queue[2*(n+1)].  The
     * priority queue is ordered by comparator, or by the elements'
     * natural ordering, if comparator is null: For each node n in the
     * heap and each descendant d of n, n <= d.  The element with the
     * lowest value is in queue[0], assuming the queue is nonempty.
     */
    transient Object[] queue; // non-private to simplify nested class access

    /**
     * The number of elements in the priority queue.
     */
    private int size = 0;

    / * The comparator, or null if priority queue uses elements'
     * natural ordering.
     */
    private final Comparator<? super E> comparator;

     /**
     * Creates a {@code PriorityQueue} with the specified initial capacity
     * that orders its elements according to the specified comparator.
     *
     * @param  initialCapacity the initial capacity for this priority queue
     * @param  comparator the comparator that will be used to order this
     *         priority queue.  If {@code null}, the {@linkplain Comparable
     *         natural ordering} of the elements will be used.
     * @throws IllegalArgumentException if {@code initialCapacity} is
     *         less than 1
     */
    public PriorityQueue(int initialCapacity,
                         Comparator<? super E> comparator) {
        // Note: This restriction of at least one is not actually needed,
        // but continues for 1.5 compatibility
        if (initialCapacity < 1)
            throw new IllegalArgumentException();
        this.queue = new Object[initialCapacity];
        this.comparator = comparator;
    }

    /**
     * Inserts the specified element into this priority queue.
     *
     * @return {@code true} (as specified by {@link Collection#add})
     * @throws ClassCastException if the specified element cannot be
     *         compared with elements currently in this priority queue
     *         according to the priority queue's ordering
     * @throws NullPointerException if the specified element is null
     */
    public boolean add(E e) {
        return offer(e);
    }

    /**
     * Inserts the specified element into this priority queue.
     *
     * @return {@code true} (as specified by {@link Queue#offer})
     * @throws ClassCastException if the specified element cannot be
     *         compared with elements currently in this priority queue
     *         according to the priority queue's ordering
     * @throws NullPointerException if the specified element is null
     */
    public boolean offer(E e) {
        if (e == null)
            throw new NullPointerException();
        modCount++;
        int i = size;
        if (i >= queue.length)
            grow(i + 1);
        size = i + 1;
        if (i == 0)
            queue[0] = e;
        else
            siftUp(i, e);
        return true;
    }

    @SuppressWarnings("unchecked")
    public E peek() {
        return (size == 0) ? null : (E) queue[0];
    }

    private int indexOf(Object o) {
        if (o != null) {
            for (int i = 0; i < size; i++)
                if (o.equals(queue[i]))
                    return i;
        }
        return -1;
    }

    @SuppressWarnings("unchecked")
    public E poll() {
        if (size == 0)
            return null;
        int s = --size;
        modCount++;
        E result = (E) queue[0];
        E x = (E) queue[s];
        queue[s] = null;
        if (s != 0)
            siftDown(0, x);
        return result;
    }


    /**
     * Inserts item x at position k, maintaining heap invariant by
     * promoting x up the tree until it is greater than or equal to
     * its parent, or is the root.
     *
     * To simplify and speed up coercions and comparisons. the
     * Comparable and Comparator versions are separated into different
     * methods that are otherwise identical. (Similarly for siftDown.)
     *
     * @param k the position to fill
     * @param x the item to insert
     */
    private void siftUp(int k, E x) {
        if (comparator != null)
            siftUpUsingComparator(k, x);
        else
            siftUpComparable(k, x);
    }

    @SuppressWarnings("unchecked")
    private void siftUpComparable(int k, E x) {
        Comparable<? super E> key = (Comparable<? super E>) x;
        while (k > 0) {
            int parent = (k - 1) >>> 1;
            Object e = queue[parent];
            if (key.compareTo((E) e) >= 0)
                break;
            queue[k] = e;
            k = parent;
        }
        queue[k] = key;
    }

    @SuppressWarnings("unchecked")
    private void siftUpUsingComparator(int k, E x) {
        while (k > 0) {
            int parent = (k - 1) >>> 1;
            Object e = queue[parent];
            if (comparator.compare(x, (E) e) >= 0)
                break;
            queue[k] = e;
            k = parent;
        }
        queue[k] = x;
    }

    /**
     * Inserts item x at position k, maintaining heap invariant by
     * demoting x down the tree repeatedly until it is less than or
     * equal to its children or is a leaf.
     *
     * @param k the position to fill
     * @param x the item to insert
     */
    private void siftDown(int k, E x) {
        if (comparator != null)
            siftDownUsingComparator(k, x);
        else
            siftDownComparable(k, x);
    }

    @SuppressWarnings("unchecked")
    private void siftDownComparable(int k, E x) {
        Comparable<? super E> key = (Comparable<? super E>)x;
        int half = size >>> 1;        // loop while a non-leaf
        while (k < half) {
            int child = (k << 1) + 1; // assume left child is least
            Object c = queue[child];
            int right = child + 1;
            if (right < size &&
                ((Comparable<? super E>) c).compareTo((E) queue[right]) > 0)
                c = queue[child = right];
            if (key.compareTo((E) c) <= 0)
                break;
            queue[k] = c;
            k = child;
        }
        queue[k] = key;
    }

    @SuppressWarnings("unchecked")
    private void siftDownUsingComparator(int k, E x) {
        int half = size >>> 1;
        while (k < half) {
            int child = (k << 1) + 1;
            Object c = queue[child];
            int right = child + 1;
            if (right < size &&
                comparator.compare((E) c, (E) queue[right]) > 0)
                c = queue[child = right];
            if (comparator.compare(x, (E) c) <= 0)
                break;
            queue[k] = c;
            k = child;
        }
        queue[k] = x;
    }

    /**
     * Establishes the heap invariant (described above) in the entire tree,
     * assuming nothing about the order of the elements prior to the call.
     */
    @SuppressWarnings("unchecked")
    private void heapify() {
        for (int i = (size >>> 1) - 1; i >= 0; i--)
            siftDown(i, (E) queue[i]);
    }


    ...

}
```