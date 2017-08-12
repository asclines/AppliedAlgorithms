# Shortest Path Tree in a Graph

This problem is introduced in [Dr. Gelfond Applied Algorithms](http://redwood.cs.ttu.edu/~mgelfond/FALL-2012/slides.pdf) and is further discussed in [Shortest Path Problem Wikipedia](https://en.wikipedia.org/wiki/Shortest_path_problem)

The algorithm used to solve this problem is [Dijsktra's Algorithm](https://en.wikipedia.org/wiki/Dijkstra%27s_algorithm).

Category: Greedy

Difficulty: Hard to implement, medium to understand.

## Problem
Given a directed graph G = [V, E] where each edge _e_ has a length _l<sub>e</sub>_, and a starting node _s_ in that graph, determine the length
of the shortest path from _s_ to every other node.

![Dijkstra's Visualization](https://github.com/CodeSpaceHQ/AppliedAlgorithms/blob/shortest-path/Guide/Greedy/Shortest%20Path/assets/DijstraDemo.gif "Visualization of Dijkstra's Algorithm")

### Overview
For this problem we will need to utilize two data structures:
* [Min Priority Queue](https://en.wikipedia.org/wiki/Priority_queue)
* [Graph](https://en.wikipedia.org/wiki/Graph_(discrete_mathematics))

The implementation of priority queue we will use is the [Fibonacci Heap](https://en.wikipedia.org/wiki/Fibonacci_heap). We will not need to implement all of the functionality
of a fibonacci heap, just those that will allow us to:
1. Add a node consisting of an _id_ and a _key_ to the min heap,
2. Extract the node with the minimum key (the key in our case is the distance from the starting node),
3. Decrease the key of a specified node and re-arrange the min heap accordingly.

The Fibonacci Heap data structure is a task to understand by itself and is not the purpose of this guide. Readers only need to know what the above three functions do on a
high level to understand Dijkstra's algorithm for shortest path. The Fibonacci Heap implementation, as well as the link to the Wikipedia page, are included in this guide for supplemental reading.

The graph data structure will just be a simple representation of a directed or undirected graph using key-value pairs in dictionaries. A graph is represented by a single dictionary
with entries in the form:
    v : [(u, d), (u<sub>1</sub>, d),..., (u<sub>n</sub>, d)]
where the key _v_ is the id of a vertex in the graph (i.e., 'A'), and the value (a list of tuples) contain _u_: a connected node and _d_: the distance from _v_ to _u_.

### Input Format

Dijkstra's algorithm to find the shortest path between a starting node (source node) and all other vertices on a graph will take two inputs:
1. The graph
2. The starting node

In our implementation, the input will be:
1. Our Graph object _g_ discussed in the propblem overview above, directed or undirected.
2. The id of a starting node, i.e. 'A' or 'E', which must be in our input graph _g_

### Constraints
The start node must be a vertex included in the graph _g_.

### Output Format
Our output format will consist of two parts:
1. A dict of distances to each node from the start node, i.e. {'A': 0, 'B': 3, 'C':2,...}
2. A dict representing the path taken from the start node to all other nodes, i.e. {'A':None, 'B': 'A', 'C': 'B',...} where A is the starting node.

## Algorithm
### Overview
Dijkstra's algorithm has these main steps:
1. Initialize two dictionaries - one for distance between a node and a source node, and one for keeping track of a path between nodes.
2. Initialize a priority queue
3. Loop through while the queue is not empty, and extract the node with the minimum key (representing distance)
4. Calculate all distances from the extracted node's corresponding vertex to all of the vertex's neighbors, and put the smallest one in the distance dictionary for that vertex
5. Decrease the key of the node in the queue by the new distance of the vertex
6. continue

The priority queue will eventually empty, because extracting the minimum node will also delete it from the queue.

One other thing to mention in our implementation is the use of one more dictionary to keep track of the nodes we put in our queue. This is so we can get the corresponding
node in the heap in O(1) time by just the id from our vertex. This functionality could be implemented instead in the FibHeap class if desired.

### Pseudo Code

```Python

    def dijsktra(Graph g, String start_id):
        dist, prev = key value dictionaries  # dictionaries to keep track of path and distances
        dist[start_id] = 0  # distance to starting node is 0
        prev[start_id] = None  # starting node is the start, no previous path

        nodes = key value dictionary()  # we need to keep track of the nodes in the fibonacci heap to be able to relate them to the vertices in g in O(1) time
        queue = Priority Queue (Fibonacci Heap Data Structure)

        for each vertex v in g:
            if v is not the starting node:
                dist[v] = infinity  # unknown distance to this node
                prev[v] = None  # unknown path to this node
            queue.add(v, dist[v])  # add to fibonacci heap with key = distance to this node from source

        while queue is not empty:
            u = queue.extract_min()  # get the next minimum node
            for each neighbor v of u in g:
                alternate_distance = dist[u] + distance between vertices u and v in g
                if the alternate_distance < dist[v]:  # if alternate distance is shorter than the current distance we have form source to v
                    dist[v] = alt
                    prev[v] = u  # get to vertex v through vertex u
                    queue.decrease_key(nodes[v], alt)  # change the key of the node representing vertex v to alt

        return dist, prev

```

## Analysis
From Wikipedia:

>Dijkstra's original algorithm does not use a min-priority queue and runs in time O(|V|^{2}) (where |V| is the number of nodes). The idea of this algorithm is also given in Leyzorek et al. 1957. The implementation based on a min-priority queue implemented by a Fibonacci heap and running in O(|E|+|V|\log |V|) (where |E| is the number of edges) is due to Fredman & Tarjan 1984. This is asymptotically the fastest known single-source shortest-path algorithm for arbitrary directed graphs with unbounded non-negative weights.

Our algorithm implements the min-priority queue using the Fibonacci Haep data structure and thus follows the run time of O(|E|+|V|\log |V|).

The difference when using a min-priority queue, specifically the in the form of a Fibonacci Heap, is due to the time O(1) time complexity for inserting a node, finding the minimum node in the heap,
and decreasing the key of a node. Our extract min also deletes the minimum node from the heap, so it's time complexity is really O(n log n).

Another interesting discovery discussed in the Wikipedia page of Fibonacci Heaps is that although they look ver efficient, Fibonacci heaps have two drawbacks:
1. They are complicated when it comes to coding them
2. Also they are not as efficient in practice when compared with the theoretically less efficient forms of heaps, since in their simplest version they require storage and manipulation of four pointers per node, compared to the two or three pointers per node needed for other structures

For a more detailed discussion on the Fibonacci Heap and it's time complexities, see [here](https://en.wikipedia.org/wiki/Fibonacci_heap#Summary_of_running_times).

## Example


## Conclusion
For this conclusion I will use a quote from the Wikipedia page on Dijkstra's Algorithm in the section _Practical optimizations and infinite graphs_.

>In common presentations of Dijkstra's algorithm, initially all nodes are entered into the priority queue. This is, however, not necessary: the algorithm can start with a priority queue that contains only one item, and insert new items as they are discovered (instead of doing a decrease-key, check whether the key is in the queue; if it is, decrease its key, otherwise insert it). This variant has the same worst-case bounds as the common variant, but maintains a smaller priority queue in practice, speeding up the queue operations.
Moreover, not inserting all nodes in a graph makes it possible to extend the algorithm to find the shortest path from a single source to the closest of a set of target nodes on infinite graphs or those too large to represent in memory. The resulting algorithm is called uniform-cost search (UCS) in the artificial intelligence literature...
