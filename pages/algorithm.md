---
# permalink: /about/
layout: single
title: "Data Structure & Algorithm"
classes: wide
header:
  image: /assets/images/teaser/teaser.png
  caption: "Image credit: [**Yun**](http://yun-vis.net)"
last_modified_at: 2025-03-20
---

# Data Structure

## Create the MyBusiness console project

```bash
$ dotnet new console --use-program-main --name MyBusiness
```

## Create the DataStructureLibrary classlib project

```bash
$ dotnet new classlib --name DataStructureLibrary
```

## Singly Linked List

In MyBusiness/Program.cs
```csharp
namespace MyBusiness;

using DataStructureLibrary.SinglyLinkedList;
// using DataStructureLibrary.DoublyLinkedList;

class Program
{
    static void Main(string[] args)
    {
        // Single linked list
        SinglyLinkedList list = new SinglyLinkedList();
        // Doubly linked list
        // DoublyLinkedList list = new DoublyLinkedList();
        list.InsertLast(6);
        list.InsertLast(4);
        list.InsertFront(2);
        list.PrintList();
        list.DeleteNodebyKey(2);
        list.PrintList();
        list.InsertAfter(list.FindbyKey(4), 5);
        list.PrintList();
        list.Sort();
        list.PrintList();
    }
}
```
```bash
The singlyLinkedList: 2 6 4 
The singlyLinkedList: 6 4   
The singlyLinkedList: 6 4 5
The singlyLinkedList: 4 5 6
```

In DataStructureLibrary/SinglyLinkedList.cs
```csharp
namespace DataStructureLibrary.SinglyLinkedList;

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
    // Public Methods
    public void InsertFront(int newData)
    {
        Node newNode = new Node(newData);

        newNode.Next = _head;
        _head = newNode;
    }

    public void InsertLast(int newData)
    {
        Node newNode = new Node(newData);

        if (_head == null)
        {
            _head = newNode;
        }
        else
        {
            Node? lastNode = GetLastNode();
            // Exclamation mark ! tells C# compiler explicitly that lastNode 
            // at this moment is not a null object. This is a way to bypass the 
            // warning.
            lastNode!.Next = newNode;
        }
    }

    public Node? GetLastNode()
    {
        Node? temp = _head;

        if (temp != null)
        {
            while (temp.Next != null)
            {
                temp = temp.Next;
            }
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
            {
                return temp;
            }
            temp = temp.Next;
        }

        // The target data is not found
        return null;
    }

    public void DeleteNodebyKey(int key)
    {
        Node? temp = _head;
        Node? prev = null;

        if (temp != null)
        {
            // 1st node is the key
            if (temp.Data == key)
            {
                _head = temp.Next;
                return;
            }
            else
            {
                // Navigate the list to find the key
                while (temp!.Data != key)
                {
                    prev = temp;
                    temp = temp.Next;

                    // The key is not found when reaching to the end of the list
                    if (temp == null) return;
                }

                // The node that has the target key is found
                prev!.Next = temp.Next;
            }
        }
    }

    public void Sort()
    {
        Node? temp = _head;

        if (_head == null)
        {
            return;
        }
        else
        {
            while (temp != null)
            {
                Node? index = temp.Next;

                while (index != null)
                {
                    if (temp.Data > index.Data)
                    {
                        int tempData = temp.Data;
                        temp.Data = index.Data;
                        index.Data = tempData;
                    }
                    index = index.Next;
                }
                temp = temp.Next;
            }
        }
    }

    public void PrintList()
    {
        Node? temp = _head;
        Console.Write("The singlyLinkedList: ");
        while (temp != null)
        {
            Console.Write(temp.Data + " ");
            temp = temp.Next;
        }
        Console.WriteLine("");
    }
}
```

## Doubly Linked List

In DataStructureLibrary/DoublyLinkedList.cs
```csharp
namespace DataStructureLibrary.DoublyLinkedList;

public class Node
{
    // Fields
    public int Data;
    public Node? Prev;
    public Node? Next;

    // Constructors
    public Node(int d)
    {
        Data = d;
        Prev = null;
        Next = null;
    }

    // Finalizers
    ~Node() { }
}

public class DoublyLinkedList
{
    // Fields
    private Node? _head;

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
        Node newNode = new Node(data);
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
        Node newNode = new Node(data);

        if (_head == null)
        {
            newNode.Prev = null;
            _head = newNode;
            return;
        }
        else
        {
            Node? lastNode = GetLastNode();

            lastNode!.Next = newNode;
            newNode.Prev = lastNode;
        }
    }

    public Node? GetLastNode()
    {
        Node? temp = _head;

        if (temp != null)
        {
            while (temp.Next != null)
            {
                temp = temp.Next;
            }
        }
        return temp;
    }

    public void InsertAfter(Node? prevNode, int newData)
    {
        if (prevNode == null)
        {
            Console.WriteLine("The method does nothing becase the node is not found.");
        }
        else
        {
            Node newNode = new Node(newData);
            newNode.Next = prevNode.Next;
            prevNode.Next = newNode;
            newNode.Prev = prevNode;
            if (newNode.Next != null)
            {
                newNode.Next.Prev = newNode;
            }
        }
    }

    public Node? FindbyKey(int data)
    {
        Node? temp = _head;

        while (temp != null)
        {
            if (temp.Data == data)
            {
                return temp;
            }
            temp = temp.Next;
        }
        return null;
    }

    public void DeleteNodebyKey(int key)
    {
        Node? temp = _head;

        // The list is not empty
        if (temp != null)
        {
            // 1st node is the key
            if (temp.Data == key)
            {
                _head = temp.Next;
                if (_head != null) _head.Prev = null;
                return;
            }
            else
            {
                while (temp!.Data != key)
                {
                    temp = temp.Next;

                    // The key is not found when reaching to the end of the list
                    if (temp == null) return;
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
        }
    }

    public void PrintList()
    {
        Node? temp = _head;
        Console.Write("The doublyLinkedList: ");
        while (temp != null)
        {
            Console.Write(temp.Data + " ");
            temp = temp.Next;
        }
        Console.WriteLine("");
    }
}
```

## Graph (Node/Edge List)

In MyBusiness/Program.csproj
```csharp
namespace MyBusiness;

using DataStructureLibrary.Graph;

class Program
{
    static void Main(string[] args)
    {
        Graph graph = new Graph();
        int v1 = graph.AddVertex("Victor");
        int v2 = graph.AddVertex("Markus");
        int v3 = graph.AddVertex("Yun");
        int v4 = graph.AddVertex("Anna");
        // graph.RemoveVertex("Yun");
        graph.AddEdge(v1, v2);
        graph.AddEdge(v1, v3);
        graph.AddEdge(v2, v3);
        graph.AddEdge(v3, v4);
        graph.PrintGraph();
        graph.RemoveVertex("Victor");
        // graph.RemoveEdge(v1, v3);
        graph.PrintGraph();
    }
}
```
```bash
The total number of vertices is 4
The total number of edges is 4
==============================
V(0) = Victor
V(1) = Markus
V(2) = Yun
V(3) = Anna
==============================
E(0) = V(0) -- V(1)
E(1) = V(0) -- V(2)
E(2) = V(1) -- V(2)
E(3) = V(2) -- V(3)
==============================
The total number of vertices is 3
The total number of edges is 3
==============================
V(1) = Markus
V(2) = Yun
V(3) = Anna
==============================
E(1) = V(0) -- V(2)
E(2) = V(1) -- V(2)
E(3) = V(2) -- V(3)
==============================
```

In DataStructureLibrary/Graph.cs
```csharp
namespace DataStructureLibrary.Graph;

public class Graph
{
    // Fields
    // The list of vertices in the graph
    // LinkedList is the singly linked list implementation in C#
    private LinkedList<Vertex> _vertices;
    private LinkedList<Edge> _edges;

    // Constructors
    public Graph()
    {
        _vertices = new LinkedList<Vertex>();
        _edges = new LinkedList<Edge>();
    }

    // Methods
    // Manipulate vertices
    public int AddVertex(string name)
    {
        Vertex v = new Vertex(_vertices.Count, name);
        _vertices.AddLast(v);

        return v.Id;
    }

    public void RemoveVertex(string name)
    {
        Vertex? v = HasVertex(name);

        if (v != null)
        {
            // Remove the adjacent edges
            for (int i = 0; i < _edges.Count; i++)
            {
                // Equal to source id
                if (_edges.ElementAt(i).SourceId == v.Id)
                {
                    _edges.Remove(_edges.ElementAt(i));
                }
                // Equal to target id
                if (_edges.ElementAt(i).TargetId == v.Id)
                {
                    _edges.Remove(_edges.ElementAt(i));
                }
            }

            // Remove the vertex from the list
            _vertices.Remove(v);
        }
    }

    public Vertex? HasVertex(string name)
    {
        foreach (Vertex v in _vertices)
        {
            if (v.Name == name)
                return v;
        }
        return null;
    }

    // Function overloading
    public Vertex? HasVertex(int id)
    {
        foreach (Vertex v in _vertices)
        {
            if (v.Id == id)
                return v;
        }
        return null;
    }

    // Manipulate edges
    public void AddEdge(int sourceId, int targetId)
    {
        Edge? e = HasEdge(sourceId, targetId);

        if (e == null)
        {
            Vertex? sourceV = HasVertex(sourceId);
            Vertex? targetV = HasVertex(targetId);

            // Check if the source and target vertices exist
            if (sourceV == null || targetV == null)
            {
                Console.WriteLine("Source or Target Vertex could not be found. Please add vertices first");
                return;
            }
            else
            {
                Edge newE = new Edge(_edges.Count, sourceId, targetId);
                _edges.AddLast(newE);
            }
        }
    }

    public void RemoveEdge(int sourcdId, int targetId)
    {
        Edge? e = HasEdge(sourcdId, targetId);
        if (e != null)
        {
            _edges.Remove(e);
        }
    }

    public Edge? HasEdge(int sourcdId, int targetId)
    {
        foreach (Edge e in _edges)
        {
            if ((e.SourceId == sourcdId) &&
                (e.TargetId == targetId))
                return e;
        }
        return null;
    }

    // Graph
    public void PrintGraph()
    {
        Console.WriteLine("The total number of vertices is " + _vertices.Count);
        Console.WriteLine("The total number of edges is " + _edges.Count);
        Console.WriteLine("==============================");

        // Vertex list
        foreach (Vertex v in _vertices)
        {
            Console.WriteLine($"V({v.Id}) = {v.Name}");
        }
        Console.WriteLine("==============================");

        // Edge list
        foreach (Edge e in _edges)
        {
            Console.WriteLine($"E({e.Id}) = V({e.SourceId}) -- V({e.TargetId})");
        }
        Console.WriteLine("==============================");
    }
}
```

In DataStructureLibrary/Vertex.cs
```csharp
namespace DataStructureLibrary.Graph;

public class Vertex
{
    // Fields
    public int Id;
    public string Name;

    // Constructors
    public Vertex(int id, string name)
    {
        Id = id;
        Name = name;
    }
}
```

In DataStructureLibrary/Edge.cs
```csharp
namespace DataStructureLibrary.Graph;

public class Edge
{
    // Fields
    public int Id;
    public int SourceId;
    public int TargetId;

    // Constructors
    public Edge(int id, int source, int target)
    {
        Id = id;
        SourceId = source;
        TargetId = target;
    }
}
```

## Graph (Node/Edge List Refactored)

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
