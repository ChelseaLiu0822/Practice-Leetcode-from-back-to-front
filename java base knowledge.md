# Java内置对象的自定义比较方法

### **`Comparator` 的主要方法**

- **`compare(T o1, T o2)`**：用于比较两个对象。
  - 返回负值：如果 `o1` 应排在 `o2` 前面。
  - 返回零：如果 `o1` 和 `o2` 排序上等同。
  - 返回正值：如果 `o1` 应排在 `o2` 后面。
- **`reversed()`**：返回一个与当前比较器结果相反的比较器。
- **`thenComparing(Comparator<? super T> other)`**：允许在第一个排序规则相等时使用其他规则排序。

```java
import java.util.*;

class Person {
    String name;
    int age;

    Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
    @Override
    public String toString() {
        return name + " (" + age + ")";
    }
}

public class CustomComparatorExample {
    public static void main(String[] args) {
        List<Person> people = Arrays.asList(
            new Person("Alice", 30),
            new Person("Bob", 25),
            new Person("Charlie", 35)
        );

        // 按年龄升序排序
        people.sort(Comparator.comparingInt(person -> person.age));

        System.out.println(people); // 输出: [Bob (25), Alice (30), Charlie (35)]

        // 按年龄降序排序
        people.sort((p1, p2) -> p2.age - p1.age);

        System.out.println(people); // 输出: [Charlie (35), Alice (30), Bob (25)]
        
        // 按年龄升序排序
        people.sort(new Comparator<Person>() {
            @Override
            public int compare(Person o1, Person o2) {
                return o1.age - o2.age; // 升序排序
            }
        });
        
        // 按年龄降序排序
        people.sort(Comparator.comparingInt(person -> person.age).reversed());

        System.out.println(people); // 输出: [Charlie (35), Alice (30), Bob (25)]

    }
}

```

```java
import java.util.*;

public class MultiLevelSorting {
    public static void main(String[] args) {
        List<Person> people = Arrays.asList(
            new Person("Alice", 30),
            new Person("Bob", 30),
            new Person("Charlie", 25)
        );

        // 按年龄升序排序，再按名字字母顺序排序
        people.sort(Comparator.comparingInt((Person p) -> p.age)
                              .thenComparing(p -> p.name));

        System.out.println(people);
        // 输出: [Charlie (25), Alice (30), Bob (30)]
    }
}

```



# 小顶堆（Min-Heap)

在 Java 中，小顶堆（Min-Heap）是一种基于完全二叉树的数据结构，其中每个节点的值都小于或等于其子节点的值。因此，堆顶元素始终是堆中的最小值。

在 Java 中，小顶堆通常使用 **PriorityQueue** 来实现。`PriorityQueue` 默认是小顶堆，按照自然顺序对元素进行排序。

## 常用方法

- **`add(E e)` 或 `offer(E e)`**: 向堆中添加元素。
- **`poll()`**: 删除并返回堆顶元素（最小值）。
- **`peek()`**: 查看堆顶元素，但不删除。
- **`isEmpty()`**: 检查堆是否为空。

```java
import java.util.PriorityQueue;
import java.util.Comparator;

public class CustomMinHeap {
    public static void main(String[] args) {
        // 使用 Comparator 自定义排序规则（从小到大）
        PriorityQueue<Integer> minHeap = new PriorityQueue<>(Comparator.naturalOrder());

        // 添加元素
        minHeap.offer(8);
        minHeap.offer(1);
        minHeap.offer(6);

        // 输出堆顶
        System.out.println("Top element: " + minHeap.peek()); // 输出: 1

        // 删除并打印元素
        while (!minHeap.isEmpty()) {
            System.out.println("Removed: " + minHeap.poll());
        }
        // 输出: 1, 6, 8
    }
}
```

| 构造函数和说明                                               |
| :----------------------------------------------------------- |
| `PriorityQueue()`创建一个`PriorityQueue`具有默认初始容量（11）的，并根据 [自然顺序](https://docs.oracle.com/javase/8/docs/api/java/lang/Comparable.html)对其元素进行排序。 |
| `PriorityQueue(Collection<? extends E> c)`创建一个`PriorityQueue`包含指定集合中的元素。 |
| `PriorityQueue(Comparator<? super E> comparator)`创建一个`PriorityQueue`具有默认初始容量且其元素根据指定的比较器排序的。 |
| `PriorityQueue(int initialCapacity)`创建一个`PriorityQueue`具有指定初始容量的 ，并根据其 [自然顺序](https://docs.oracle.com/javase/8/docs/api/java/lang/Comparable.html)对其元素进行排序。 |
| `PriorityQueue(int initialCapacity, Comparator<? super E> comparator)`创建一个`PriorityQueue`具有指定初始容量的，并根据指定的比较器对其元素进行排序。 |
| `PriorityQueue(PriorityQueue<? extends E> c)`创建一个`PriorityQueue`包含指定优先级队列中的元素。 |
| `PriorityQueue(SortedSet<? extends E> c)`创建一个`PriorityQueue`包含指定排序集合中的元素。 |