---
# permalink: /about/
layout: single
title: "Data Structure & Algorithm"
classes: wide
header:
  image: /assets/images/teaser/teaser.png
  caption: "Image credit: [**Yun**](http://yun-vis.net)"
last_modified_at: 2023-03-15
---

# Data Structure

## Singly Linked List

```csharp
using System;

namespace CRC_CSD_09
{
    class Program
    {
        // Where the application begins
        static void Main(string[] args) 
        {
            // Single linked list
            SinglyLinkedList list = new SinglyLinkedList();
            list.InsertLast(6);
            list.InsertLast(4);
            list.InsertFront(2);
            list.printList();
            list.DeleteNodebyKey(2);
            list.printList();
            list.InsertAfter(list.FindbyKey(4), 5);
            list.printList();
            list.Sort();
            list.printList();
        }
    }

    public class Node
    {
        // Fields
        public int Data;
        public Node? Next;

        // Constructors
        public Node(int d)
        {
            Data = d;
            Next = null;
        }
        
        // Finalizers
        ~Node() { }
    }

    public class SinglyLinkedList
    {
        // Fields
        private Node? _head;

        // Constructors
        public SinglyLinkedList()
        {
            _head = null;
        }

        // Finalizer
        ~SinglyLinkedList() { }

        // Methods
        // Private Methods

        // Public Methods
        public void InsertFront(int newData)
        {
            Node new_node = new Node(newData);
            new_node.Next = _head;
            _head = new_node;
        }

        public void InsertLast(int newData)
        {
            Node new_node = new Node(newData);
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

        public void Sort()
        {
            Node? current = _head;

            if (_head == null)
            {
                return;
            }
            else
            {
                while (current != null)
                {
                    Node? index = current.Next;

                    while (index != null)
                    {
                        if (current.Data < index.Data)
                        {
                            int temp = current.Data;
                            current.Data = index.Data;
                            index.Data = temp;
                        }
                        index = index.Next;
                    }
                    current = current.Next;
                }
            }
        }
        
        public void printList()
        {
            Node? temp = _head;
            Console.Write("The singlyLinkedList: ");
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

namespace CRC_CSD_09
{
    class Program
    {
        // Where the application begins
        static void Main(string[] args) 
        {
            // Single linked list
            // SinglyLinkedList list = new SinglyLinkedList();
            // Doubly linked list
            DoublyLinkedList list = new DoublyLinkedList();
            list.InsertLast(6);
            list.InsertLast(4);
            list.InsertFront(2);
            list.printList();
            list.DeleteNodebyKey(2);
            list.printList();
            list.InsertAfter(list.FindbyKey(4), 5);
            list.printList();
        }
    }

    public class DNode
    {
        // Fields
        public int Data;
        public DNode? Prev;
        public DNode? Next;

        // Constructors
        public DNode(int d)
        {
            Data = d;
            Prev = null;
            Next = null;
        }

        // Finalizers
        ~DNode() { }
    }

    public class DoublyLinkedList
    {
        // Fields
        private DNode? _head;

        // Constructors
        public DoublyLinkedList()
        {
            _head = null;
        }

        // Finalizer
        ~DoublyLinkedList() { }

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
            Console.Write("The doublyLinkedList: ");
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

## Graph (Node/Edge List)

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
            Graph graph = new Graph();
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
The total number of edges is 1
==============================
V(1) = Tony
V(2) = Yun
V(3) = Tim
==============================
E(2) = V(1) -- V(2)
==============================
```

In GraphLibrary/Graph.cs
```csharp
namespace GraphLibrary;

public class Graph
{
    // Fields
    // The list of vertices in the graph
    private LinkedList<Vertex> _vertices;
    private LinkedList<Edge> _edges;

    // The number of vertices
    private uint _nVertices;
    private uint _nEdges;

    // Constructors
    public Graph()
    {
        _vertices = new LinkedList<Vertex>();
        _edges = new LinkedList<Edge>();
        _nVertices = 0;
        _nEdges = 0;
    }

    // Getters and Setters

    // Finalizer
    ~Graph()
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
                        _nEdges--;
                        allChecked = false;
                    }
                    // Equal to target id
                    if (_edges.ElementAt(i).Property.TargetId == v.Property.Id)
                    {
                        _edges.Remove(_edges.ElementAt(i));
                        _nEdges--;
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
            if ((_edges.ElementAt(i).Property.SourceId == sourcdId) &&
                (_edges.ElementAt(i).Property.TargetId == targetId))
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

public struct VertexProperty
{
    // Fields
    public uint Id;
    public string Name;
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

## Graph (Node/Edge List Generic)

In SocialNet/Program.cs
```csharp
using GraphLibrary;

// namespace
namespace SocialNet
{
    public class VertexProperty : BasicVertexProperty
    {
    }

    public class EdgeProperty : BasicEdgeProperty
    {
        public float Weight;
    }

    // main program
    public class Program
    {
        static void Main(string[] args)
        {
            Graph<VertexProperty, EdgeProperty> graph = new Graph<VertexProperty, EdgeProperty>();
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
$ V(0) = Helen
$ V(1) = Tony
$ V(2) = Yun
$ V(3) = Tim
$ ==============================
$ E(0) = V(0) -- V(1)
$ E(1) = V(0) -- V(2)
$ E(2) = V(1) -- V(2)
$ ==============================
$ The total number of vertices is 3
$ The total number of edges is 3
$ ==============================
$ V(1) = Tony
$ V(2) = Yun
$ V(3) = Tim
$ ==============================
$ E(2) = V(1) -- V(2)
$ ==============================
```

In GraphLibrary/Graph.cs
```csharp
namespace GraphLibrary;

public class Graph<T1, T2>
where T1 : BasicVertexProperty, new()
where T2 : BasicEdgeProperty, new()
{
    // Fields
    // The list of vertices in the graph
    private Dictionary<uint, Vertex<T1>> _vertices;
    private Dictionary<uint, Edge<T2>> _edges;

    // The number of vertices
    private uint _vIndex;
    private uint _eIndex;


    // Constructors
    public Graph()
    {
        _vertices = new Dictionary<uint, Vertex<T1>>();
        _edges = new Dictionary<uint, Edge<T2>>();
        _vIndex = 0;
        _eIndex = 0;
    }

    // Getters and Setters

    // Finalizer
    ~Graph()
    {
    }

    // Methods
    // Vertex
    public uint AddVertex(string name)
    {
        Vertex<T1> v = new Vertex<T1>();
        // Add attributes
        v.Property.Id = _vIndex;
        v.Property.Name = name;

        _vertices.Add(_vIndex, v);
        _vIndex++;
        return _vIndex - 1;
    }

    public void RemoveVertex(string name)
    {
        uint? vKey = HasVertex(name);

        if (vKey != null)
        {
            Vertex<T1> v = _vertices[(uint)vKey];

            // Remove the adjacent edges
            bool allChecked = false;
            while (!allChecked)
            {
                allChecked = true;
                for (int i = 0; i < _edges.Count; i++)
                {
                    KeyValuePair<uint, Edge<T2>> eItem = _edges.ElementAt(i);

                    // Equal to source id
                    if (eItem.Value.Property.SourceId == v.Property.Id)
                    {
                        _edges.Remove(eItem.Key);
                        allChecked = false;
                    }
                    // Equal to target id
                    if (eItem.Value.Property.TargetId == v.Property.Id)
                    {
                        _edges.Remove(eItem.Key);
                        allChecked = false;
                    }
                }
            }

            // Remove the vertex
            _vertices.Remove((uint)vKey);
            _vIndex--;
        }
    }

    public uint? HasVertex(string name)
    {
        for (int i = 0; i < _vertices.Count; i++)
        {
            if (_vertices.ElementAt(i).Value.Property.Name == name)
                return _vertices.ElementAt(i).Key;
        }
        return null;
    }

    // Function overloading
    public uint? HasVertex(uint id)
    {
        for (int i = 0; i < _vertices.Count; i++)
        {
            if (_vertices.ElementAt(i).Value.Property.Id == id)
                return _vertices.ElementAt(i).Key;
        }
        return null;
    }

    // Edge
    public void AddEdge(uint sourceId, uint targetId)
    {
        uint? eKey = HasEdge(sourceId, targetId);

        if (eKey == null)
        {
            uint? sourceKey = HasVertex(sourceId);
            uint? targetKey = HasVertex(targetId);
            if (sourceKey == null || targetKey == null)
            {
                Console.WriteLine("Source or Target Vertex could not be found. Please add vertices first");
                return;
            }
            else
            {
                Edge<T2> newE = new Edge<T2>();
                // Add attributes
                newE.Property.Id = _eIndex;
                newE.Property.SourceId = sourceId;
                newE.Property.TargetId = targetId;

                _edges.Add(_eIndex, newE);
                _eIndex++;
            }
        }
    }

    public void RemoveEdge(uint sourcdId, uint targetId)
    {
        uint? eKey = HasEdge(sourcdId, targetId);

        if (eKey != null)
        {
            _edges.Remove((uint)eKey);
            _eIndex--;
        }
    }

    public uint? HasEdge(uint sourcdId, uint targetId)
    {
        for (int i = 0; i < _edges.Count; i++)
        {
            if ((_edges.ElementAt(i).Value.Property.SourceId == sourcdId) &&
                (_edges.ElementAt(i).Value.Property.TargetId == targetId))
                return _edges.ElementAt(i).Key;
        }
        return null;
    }

    // Graph
    public void PrintGraph()
    {
        Console.WriteLine("The total number of vertices is " + _vIndex);
        Console.WriteLine("The total number of edges is " + _eIndex);
        Console.WriteLine("==============================");

        // Vertex list
        for (int i = 0; i < _vertices.Count; i++)
        {
            Console.WriteLine($"V({_vertices.ElementAt(i).Value.Property.Id}) = {_vertices.ElementAt(i).Value.Property.Name}");
        }
        Console.WriteLine("==============================");

        // Edge list
        for (int i = 0; i < _edges.Count; i++)
        {
            Console.WriteLine($"E({_edges.ElementAt(i).Value.Property.Id}) = V({_edges.ElementAt(i).Value.Property.SourceId}) -- V({_edges.ElementAt(i).Value.Property.TargetId})");
        }
        Console.WriteLine("==============================");
    }
}
```

In GraphLibrary/Vertex.cs
```csharp
namespace GraphLibrary;

public abstract class BasicVertexProperty
{
    // Fields
    public uint Id;
    public string Name = "Unknown_Name";
}

// BasicVertexProperty, new() are generic type constraints
public class Vertex<T> where T : BasicVertexProperty, new()
{
    // Fields
    private T _property;

    // Constructors
    public Vertex()
    {
        _property = new T();
    }

    // Getters and Setters
    public T Property
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

public abstract class BasicEdgeProperty
{
    // Fields
    public uint Id;
    public uint SourceId;
    public uint TargetId;
}

// BasicEdgeProperty, new() are generic type constraints
public class Edge<T> where T : BasicEdgeProperty, new()
{
    // Fields
    private T _property;

    // Constructors
    public Edge()
    {
        _property = new T();
    }

    // Getters and Setters
    public T Property
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

---
# External Resources
