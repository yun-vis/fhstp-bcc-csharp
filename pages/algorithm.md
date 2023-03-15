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

# Data Structure

## Singly Linked List

```csharp
using System;

namespace CRC_CSD_04
{
    class Program
    {
        // Where the application begins
        static void Main(string[] args) 
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
        // Where the application begins
        static void Main(string[] args) 
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
        // Fields
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

## Graph (Adjacency List)

In SocialNet/SocialNet.csproj
```csharp
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>net6.0</TargetFramework>
    <RootNamespace>SocialNet</RootNamespace>
    <ImplicitUsings>enable</ImplicitUsings>
    <Nullable>enable</Nullable>
  </PropertyGroup>

  <!-- Set environment variable -->
  <ItemGroup>
    <ProjectReference
      Include="../GraphLibrary/GraphLibrary.csproj" />
  </ItemGroup>

</Project>
```

In SocialNet/Program.cs
```csharp
using GraphLibrary;

// namespace
namespace SocialNet
{
    // main program
    public class Program
    {
        static void Main(string[] args)
        {
            AdjacencyListGraph graph = new AdjacencyListGraph();
            uint v1 = graph.AddVertex("Helen");
            uint v2 = graph.AddVertex("Tony");
            uint v3 = graph.AddVertex("Yun");
            uint v4 = graph.AddVertex("Tim");
            // graph.RemoveVertex("Yun");
            graph.AddEdge(v1, v2);
            graph.AddEdge(v1, v3);
            graph.AddEdge(v2, v3);
            graph.PrintGraph();
            graph.RemoveVertex("Helen");
            // graph.RemoveEdge(v1, v3);
            graph.PrintGraph();
        }
    }
}
```
```bash
The total number of vertices is 4
The total number of edges is 3
==============================
V(0) = Helen
V(1) = Tony
V(2) = Yun
V(3) = Tim
==============================
E(0) = V(0) -- V(1)
E(1) = V(0) -- V(2)
E(2) = V(1) -- V(2)
==============================
The total number of vertices is 3
The total number of edges is 3
==============================
V(1) = Tony
V(2) = Yun
V(3) = Tim
==============================
E(2) = V(1) -- V(2)
==============================
```

In GraphLibrary/AdjacencyListGraph.cs
```csharp
namespace GraphLibrary;

public class AdjacencyListGraph
{
    // Fields
    // The list of vertices in the graph
    private LinkedList<Vertex> _vertices;
    private LinkedList<Edge> _edges;

    // The number of vertices
    private uint _nVertices;
    private uint _nEdges;


    // Constructors
    public AdjacencyListGraph()
    {
        _vertices = new LinkedList<Vertex>();
        _edges = new LinkedList<Edge>();
        _nVertices = 0;
        _nEdges = 0;
    }

    // Getters and Setters

    // Finalizer
    ~AdjacencyListGraph()
    {
    }

    // Methods
    // Vertex
    public uint AddVertex(string name)
    {
        Vertex v = new Vertex(_nVertices, name);
        _vertices.AddLast(v);
        _nVertices++;
        return _nVertices - 1;
    }

    public void RemoveVertex(string name)
    {
        Vertex? v = HasVertex(name);

        if (v != null)
        {
            // Remove the adjacent edges
            bool allChecked = false;
            while (!allChecked)
            {
                allChecked = true;
                for (int i = 0; i < _edges.Count; i++)
                {
                    // Equal to source id
                    if (_edges.ElementAt(i).Property.SourceId == v.Property.Id)
                    {
                        _edges.Remove(_edges.ElementAt(i));
                        allChecked = false;
                    }
                    // Equal to target id
                    if (_edges.ElementAt(i).Property.TargetId == v.Property.Id)
                    {
                        _edges.Remove(_edges.ElementAt(i));
                        allChecked = false;
                    }
                }
            }

            // Remove the vertex
            _vertices.Remove(v);
            _nVertices--;
        }
    }

    public Vertex? HasVertex(string name)
    {
        for (int i = 0; i < _vertices.Count; i++)
        {
            if (_vertices.ElementAt(i).Property.Name == name)
                return _vertices.ElementAt(i);
        }
        return null;
    }

    // Function overloading
    public Vertex? HasVertex(uint id)
    {
        for (int i = 0; i < _vertices.Count; i++)
        {
            if (_vertices.ElementAt(i).Property.Id == id)
                return _vertices.ElementAt(i);
        }
        return null;
    }

    // Edge
    public void AddEdge(uint sourceId, uint targetId)
    {
        Edge? e = HasEdge(sourceId, targetId);
        if (e == null)
        {
            Vertex? sourceV = HasVertex(sourceId);
            Vertex? targetV = HasVertex(targetId);
            if (sourceV == null || targetV == null)
            {
                Console.WriteLine("Source or Target Vertex could not be found. Please add vertices first");
                return;
            }
            else
            {
                Edge newE = new Edge(_nEdges, sourceId, targetId);
                _edges.AddLast(newE);
                _nEdges++;
            }
        }
    }

    public void RemoveEdge(uint sourcdId, uint targetId)
    {
        Edge? e = HasEdge(sourcdId, targetId);
        if (e != null)
        {
            _edges.Remove(e);
            _nEdges--;
        }
    }

    public Edge? HasEdge(uint sourcdId, uint targetId)
    {
        for (int i = 0; i < _edges.Count; i++)
        {
            if ((_edges.ElementAt(i).Property.SourceId == sourcdId) && (_edges.ElementAt(i).Property.TargetId == targetId))
                return _edges.ElementAt(i);
        }
        return null;
    }

    // Graph
    public void PrintGraph()
    {
        Console.WriteLine("The total number of vertices is " + _nVertices);
        Console.WriteLine("The total number of edges is " + _nEdges);
        Console.WriteLine("==============================");

        // Vertex list
        for (int i = 0; i < _vertices.Count; i++)
        {
            Console.WriteLine($"V({_vertices.ElementAt(i).Property.Id}) = {_vertices.ElementAt(i).Property.Name}");
        }
        Console.WriteLine("==============================");

        // Edge list
        for (int i = 0; i < _edges.Count; i++)
        {
            Console.WriteLine($"E({_edges.ElementAt(i).Property.Id}) = V({_edges.ElementAt(i).Property.SourceId}) -- V({_edges.ElementAt(i).Property.TargetId})");
        }
        Console.WriteLine("==============================");
    }
}
```

In GraphLibrary/Vertex.cs
```csharp
namespace GraphLibrary;

public class VertexProperty
{
    // Fields
    public uint Id;
    public string Name = "Unknown_Name";
}

public class Vertex
{
    // Fields
    private VertexProperty _property;

    // Constructors
    public Vertex(uint id, string name)
    {
        _property = new VertexProperty();
        _property.Id = id;
        _property.Name = name;
    }

    // Getters and Setters
    public VertexProperty Property
    {
        get { return _property; }
        set { _property = value; }
    }


    // Finalizer
    ~Vertex()
    {
    }

    // Methods
}
```

In GraphLibrary/Edge.cs
```csharp
namespace GraphLibrary;

public struct EdgeProperty
{
    // Fields
    public uint Id;
    public uint SourceId;
    public uint TargetId;
}

public class Edge
{
    // Fields
    private EdgeProperty _property;

    // Constructors
    public Edge(uint id, uint source, uint target)
    {
        _property = new EdgeProperty();
        _property.Id = id;
        _property.SourceId = source;
        _property.TargetId = target;
    }

    // Getters and Setters
    public EdgeProperty Property
    {
        get { return _property; }
        set { _property = value; }
    }


    // Finalizer
    ~Edge()
    {
    }

    // Methods
}
```

## Graph (Adjacency List Generic)

```csharp
```

```bash
```

---
# External Resources
