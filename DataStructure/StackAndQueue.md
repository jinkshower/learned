# Stack

## 개념

스택은 한쪽 끝에서만 자료를 넣고 뺄 수 있는 LIFO (Last In First Out) 형식의 자료 구조.

## 연산

- push(item) : 스택의 가장 위에 항목을 추가합니다.
- pop() : 스택에서 가장 위에 있는 항목을 제거합니다.
- peek() : 스택의 가장 위에 있는 항목을 반환합니다.
- isEmpty() : 스택이 비어 있을 때에 true를 반환합니다.

## 구현

### 배열을 이용한 스택 구현
```java
public class ArrayStack {

    private int[] stack;
    private int capacity;
    private int pointer;

    public ArrayStack(int capacity) {
        this.pointer = 0;
        this.capacity = capacity;
        try {
            stack = new int[capacity];
        } catch (OutOfMemoryError e) {
            this.capacity = 0;
        }
    }

    public int push(int x) {
        if (pointer >= capacity) {
            throw new IllegalArgumentException();
        }
        return stack[pointer++] = x;
    }

    public int pop() {
        if (pointer <= 0) {
            throw new IllegalArgumentException();
        }
        return stack[--pointer];
    }

    public int peek() {
        if (pointer <= 0) {
            throw new IllegalArgumentException();
        }
        return stack[pointer - 1];
    }

    public boolean isEmpty() {
        return pointer == 0;
    }

    public boolean isFull() {
        return pointer == capacity;
    }
}
```

### 연결리스트를 이용한 스택 구현
```java
class Node<T> {

    T data;
    Node<T> next;

    public Node(T data) {
        this.data = data;
        this.next = null;
    }
}

public class LinkedListStack<T> {

    private Node<T> top;
    private int size;

    public LinkedListStack() {
        this.top = null;
        this.size = 0;
    }

    public void push(T value) {
        Node<T> newNode = new Node<>(value);
        newNode.next = top;
        top = newNode;
        size++;
    }

    public T pop() {
        if (isEmpty()) {
            throw new IllegalArgumentException();
        }
        T value = top.data;
        top = top.next;
        size--;
        return value;
    }

    public T peek() {
        if (isEmpty()) {
            throw new IllegalArgumentException();
        }
        return top.data;
    }

    public boolean isEmpty() {
        return top == null;
    }
}
```

# Queue

## 개념

큐는 한쪽 끝에서 자료를 넣고, 반대쪽 끝에서 자료를 뺄 수 있는 FIFO (First In First Out) 형식의 자료 구조.

## 연산

- offer(item) : 큐의 끝에 항목을 추가합니다.
- poll() : 큐의 첫 번째 항목을 제거합니다.
- peek() : 큐에서 가장 위에 있는 항목을 반환합니다.
- isEmpty() : 큐가 비어 있을 때에 true를 반환합니다.

## 구현

### 배열을 이용한 원형 큐 구현
```java
public class ArrayQueue {

    private int[] queue;
    private int capacity;
    private int front;
    private int rear;
    private int num;

    public ArrayQueue(int capacity) {
        num = front = rear = 0;
        this.capacity = capacity;
        queue = new int[capacity];
    }

    public int enqueue(int x) {
        if (num >= capacity) {
            throw new IllegalArgumentException();
        }
        queue[rear++] = x;
        num++;
        if (rear == capacity) {
            rear = 0;
        }
        return x;
    }

    public int deque() {
        if (num <= 0) {
            throw new IllegalArgumentException();
        }
        int value = queue[front++];
        num--;
        if (front == capacity) {
            front = 0;
        }
        return value;
    }

    public int peek() {
        if (num <= 0) {
            throw new IllegalArgumentException();
        }
        return queue[front];
    }
}
```

### 배열을 이용한 Deque 구현
```java
public class IntDeque {

    private int[] deque;
    private int capacity;
    private int front;
    private int rear;
    private int num;

    public IntDeque(int capacity) {
        num = front = rear = 0;
        this.capacity = capacity;
        deque = new int[capacity];
    }

    public int addFirst(int x) {
        if (num >= capacity) {
            throw new IllegalArgumentException();
        }
        num++;
        if (--front < 0) {
            front = capacity - 1;
        }
        deque[front] = x;
        return x;
    }

    public int addLast(int x) {
        if (num >= capacity) {
            throw new IllegalArgumentException();
        }
        deque[rear++] = x;
        num++;
        if (rear == capacity) {
            rear = 0;
        }
        return x;
    }

    public int removeFirst() {
        if (num <= 0) {
            throw new IllegalArgumentException();
        }
        int x = deque[front++];
        num--;
        if (front == capacity) {
            front = 0;
        }
        return x;
    }

    public int removeLast() {
        if (num <= 0) {
            throw new IllegalArgumentException();
        }
        num--;
        if (--rear < 0) {
            rear = capacity - 1;
        }
        return deque[rear];
    }
}
```
