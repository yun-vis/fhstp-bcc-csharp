---
# permalink: /about/
layout: single
title: "Data Structure & Algorithm"
classes: wide
header:
  image: /assets/images/teaser/teaser.png
  caption: "Image credit: [**Yun**](http://yun-vis.net)"
last_modified_at: 2023-03-08
---

![C# Logo](/assets/images/c-sharp.png)

# Data Structure

## Singly Linked List

```csharp
using System;

namespace CRC_CSD_04
{
    class Program
    {
        static void Main(string[] args) // Where the application begins
        {
            // Single linked list
            SingleLinkedList singleList = new SingleLinkedList();
            singleList.InsertLast(4);
            singleList.InsertLast(6);
            singleList.InsertFront(2);
            singleList.printList();
            singleList.DeleteNodebyKey(2);
            singleList.printList();
            singleList.InsertAfter(singleList.FindbyKey(4),5);
            singleList.printList();
        }
    }

    public class Node
    {
        // Fields
        private int _data;
        private Node? _next;

        // Constructors
        public Node(int d)
        {
            _data = d;
            _next = null;
        }

        // Getters and Setters
        public int Data
        {
            get { return _data; }
            set { _data = value; }
        }

        public Node? Next
        {
            get { return _next; }
            set { _next = value; }
        }

        // Finalizers
        ~Node() { }
    }

    public class SingleLinkedList
    {
        // Fields
        private Node? _head;

        // Constructors
        public SingleLinkedList()
        {
            _head = null;
        }

        // Getters and Setters
        public Node? Head
        {
            get { return _head; }
            set { _head = value; }
        }

        // Finalizer
        ~SingleLinkedList() { }

        // Methods
        public void InsertFront(int new_data)
        {
            Node new_node = new Node(new_data);
            new_node.Next = _head;
            _head = new_node;
        }

        public void InsertLast(int new_data)
        {
            Node new_node = new Node(new_data);
            if (_head == null)
            {
                _head = new_node;
            }
            else
            {
                Node? lastNode = GetLastNode();

                if (lastNode != null)
                {
                    lastNode.Next = new_node;
                }
            }
        }

        public Node? GetLastNode()
        {
            Node? temp = _head;
            while ((temp != null) && (temp.Next != null))
            {
                temp = temp.Next;
            }
            return temp;
        }

        public void InsertAfter(Node? prevNode, int newData)
        {
            if (prevNode == null)
            {
                Console.WriteLine("The given previous node Cannot be null!");
            }
            else
            {
                Node newNode = new Node(newData);
                newNode.Next = prevNode.Next;
                prevNode.Next = newNode;
            }
        }

        public Node? FindbyKey(int data)
        {
            Node? temp = _head;

            while (temp != null)
            {
                if (temp.Data == data)
                    return temp;
                temp = temp.Next;
            }
            return null;
        }

        public void DeleteNodebyKey(int key)
        {
            Node? temp = _head;
            Node? prev = null;

            // The key is equal to head
            if (temp != null && temp.Data == key)
            {
                _head = temp.Next;
                return;
            }
            while (temp != null && temp.Data != key)
            {
                prev = temp;
                temp = temp.Next;
            }
            // Do not find the key in the list
            if (temp == null)
            {
                return;
            }

            if (prev != null)
                prev.Next = temp.Next;
        }

        public void printList()
        {
            Node? temp = _head;
            Console.Write("The singleLinkedList: ");
            while ((temp != null))
            {
                Console.Write(temp.Data + " ");
                temp = temp.Next;
            }
            Console.WriteLine("");
        }
    }
}
```

```bash
The singleLinkedList: 2 4 6 
The singleLinkedList: 4 6   
The singleLinkedList: 4 5 6 
```

## Doubly Linked List


```csharp
using System;

namespace CRC_CSD_04
{
    class Program
    {
        static void Main(string[] args) // Where the application begins
        {
            // double linked list
            DoubleLinkedList doubleList = new DoubleLinkedList();
            doubleList.InsertLast(4);
            doubleList.InsertLast(6);
            doubleList.InsertFront(2);
            doubleList.printList();
            doubleList.DeleteNodebyKey(2);
            doubleList.printList();
            doubleList.InsertAfter(doubleList.FindbyKey(4),5);
            doubleList.printList();
        }
    }

    public class DNode
    {
        // Fields
        private int _data;
        private DNode? _prev;
        private DNode? _next;

        // Constructors
        public DNode(int d)
        {
            _data = d;
            _prev = null;
            _next = null;
        }

        // Getters and Setters
        public int Data
        {
            get { return _data; }
            set { _data = value; }
        }

        public DNode? Prev
        {
            get { return _prev; }
            set { _prev = value; }
        }

        public DNode? Next
        {
            get { return _next; }
            set { _next = value; }
        }

        // Finalizers
        ~DNode() { }
    }

    public class DoubleLinkedList
    {
        // Field
        private DNode? _head;

        // Constructors
        public DoubleLinkedList()
        {
            _head = null;
        }

        // Getters abd Setters
        public DNode? Head
        {
            get { return _head; }
            set { _head = value; }
        }

        // Finalizer
        ~DoubleLinkedList() { }

        // Methods
        public void InsertFront(int data)
        {
            DNode newNode = new DNode(data);
            newNode.Next = _head;
            newNode.Prev = null;
            if (_head != null)
            {
                _head.Prev = newNode;
            }
            _head = newNode;
        }

        public void InsertLast(int data)
        {
            DNode newNode = new DNode(data);
            if (_head == null)
            {
                newNode.Prev = null;
                _head = newNode;
                return;
            }
            else
            {
                DNode? lastNode = GetLastNode();
                if (lastNode != null)
                {
                    lastNode.Next = newNode;
                    newNode.Prev = lastNode;
                }
            }
        }

        public DNode? GetLastNode()
        {
            DNode? temp = _head;
            while ((temp != null) && (temp.Next != null))
            {
                temp = temp.Next;
            }
            return temp;
        }

        public void InsertAfter(DNode? prevNode, int newData)
        {
            if (prevNode == null)
            {
                Console.WriteLine("The given prevoius node cannot be null!");
            }
            else
            {
                DNode newNode = new DNode(newData);
                newNode.Next = prevNode.Next;
                prevNode.Next = newNode;
                newNode.Prev = prevNode;
                if (newNode.Next != null)
                {
                    newNode.Next.Prev = newNode;
                }
            }
        }

        public DNode? FindbyKey(int data)
        {
            DNode? temp = _head;

            while (temp != null)
            {
                if (temp.Data == data)
                    return temp;
                temp = temp.Next;
            }
            return null;
        }

        public void DeleteNodebyKey(int key)
        {
            DNode? temp = _head;

            // The key is equal to head
            if (temp != null && temp.Data == key)
            {
                _head = temp.Next;
                if (_head != null) _head.Prev = null;
                return;
            }
            while (temp != null && temp.Data != key)
            {
                temp = temp.Next;
            }
            // Do not find the key in the list
            if (temp == null)
            {
                return;
            }

            if (temp.Next != null)
            {
                temp.Next.Prev = temp.Prev;
            }
            if (temp.Prev != null)
            {
                temp.Prev.Next = temp.Next;
            }
        }

        public void printList()
        {
            DNode? temp = _head;
            Console.Write("The doubleLinkedList: ");
            while ((temp != null))
            {
                Console.Write(temp.Data + " ");
                temp = temp.Next;
            }
            Console.WriteLine("");
        }
    }
}
```

```bash
The doubleLinkedList: 2 4 6 
The doubleLinkedList: 4 6   
The doubleLinkedList: 4 5 6 
```


---
# External Resources
