# Report: Distributed Minimum Spanning Tree (MST) Algorithm

## 1. Introduction

A Minimum Spanning Tree (MST) is the cheapest possible subgraph that connects every node in a weighted, undirected graph without forming any cycles. In distributed systems, where no single node has a complete view of the network, a distributed algorithm is required to construct the MST collaboratively.

This report details a distributed MST algorithm based on the principles of the Gallager-Humblet-Spira (GHS) algorithm. It enables a set of connected nodes to find a global MST using only local communication.

## 2. Core Concepts

The algorithm builds the MST by starting with individual nodes and progressively merging them into larger sub-trees, called **fragments**. This process continues until a single fragment containing all nodes is formed.

- **Nodes and Edges**: Each computer or device is a node, and the communication links between them are edges. Each edge has a weight, representing a cost like latency or distance.
- **Fragments**: A fragment is a sub-tree of the final MST. Initially, every node is its own fragment.
- **Minimum Outgoing Edge (MOE)**: For any given fragment, its MOE is the lowest-weight edge that connects a node inside the fragment to a node outside of it. The algorithm is based on the principle that the MOE of any fragment is always part of the global MST.

## 3. The Algorithm: Phases and Steps

The algorithm operates in rounds. In each round, every fragment finds its MOE and uses it to merge with a neighboring fragment.

### Phase 1: Initialization

Every node begins as a separate fragment. It is unaware of the larger network structure beyond its immediate, adjacent edges.

```
// Executed by each node at startup
PROCEDURE Initialize():
    FragmentID := MyNodeID
    Level := 0
    State := FINDING
    BestEdge := NULL
    TestEdge := NULL
```

### Phase 2: Fragment Discovery and Merging

This phase is the core of the algorithm and repeats until the MST is complete.

**1. Find the Minimum Outgoing Edge (MOE)**

Each node in a fragment cooperates to find the fragment's MOE. This is done by testing edges to see if they lead to a different fragment.

```
// Each node finds and tests its cheapest edge
PROCEDURE Find_MOE():
    // Select the adjacent edge with the lowest weight
    MinWeightEdge := FindMinWeightEdge(AdjacentEdges)
    
    // Send a TEST message to see if it connects to another fragment
    SEND Test(Level, FragmentID) TO MinWeightEdge.Destination
```

**2. Respond to Test Messages**

When a node receives a `Test` message, it checks if the sender belongs to the same fragment.

- If not, it accepts, allowing a merge to be proposed.
- If so, it rejects the edge to prevent cycles.

```
// Logic for a node receiving a TEST message
ON RECEIVE Test(SenderLevel, SenderFragmentID) FROM Edge:
    IF SenderFragmentID = Self.FragmentID THEN
        SEND Reject TO Edge.Source
    ELSE
        SEND Accept TO Edge.Source
    END IF
```

**3. Merge Fragments**

Once a fragment's MOE is confirmed, its "core" or "leader" node sends a `Connect` request along that edge to initiate a merge with the neighboring fragment. The two fragments combine, a new `FragmentID` is established, the `Level` is increased, and the process repeats.

```
// Core node initiates a merge along the confirmed MOE
PROCEDURE Merge(MOE):
    SEND Connect(Level) TO MOE.Destination
    
    // On successful connection, the two fragments will form a new,
    // larger fragment with an incremented level and a new ID.
    // This new information is then broadcast to all nodes
    // in the newly formed fragment.
```

### Phase 3: Termination

The algorithm terminates when only one fragment remains, which signifies that all nodes in the network are connected. This final fragment is the Minimum Spanning Tree. The edges marked as `Connect` edges during the merge processes form the branches of the MST.

## 4. Algorithm Complexity

- **Message Complexity**: `O(V log V + E)`
    
    - The total number of messages sent is proportional to the number of vertices (V), edges (E), and the logarithm of V.
- **Time Complexity**: `O(V log V)`
    - The time until the final MST is formed, assuming parallel communication.

## 5. Conclusion

This distributed algorithm provides a robust and efficient solution for finding a Minimum Spanning Tree in a decentralized network. By organizing nodes into fragments that iteratively merge using their cheapest connecting edges, the system builds the global MST without requiring any central coordination. This process is fundamental for optimizing network routing, data aggregation, and other distributed tasks.