# CourierX — Smart Courier Management System

**C++ DSA Sessional Project Proposal**  
**Project type:** Advanced, group-based, console application  
**Language:** C++17  
**Storage:** In-memory only  
**Primary focus:** Data Structures and Algorithms (DSA)  
**Supporting approach:** Object-Oriented Programming (OOP)

> **Status:** Draft proposal for team discussion. Features, scope, and member assignments can be changed after the group reviews the plan and confirms course requirements.

---

## 1. Project Overview

### Project name

**CourierX — Smart Courier Management System**

### Problem statement

A courier company needs to manage parcels, customers, delivery requests, urgent shipments, delivery personnel, and routes between service locations. A basic system can store records, but searching, sorting, prioritizing, and finding routes can become inefficient as the number of parcels and locations increases.

CourierX will demonstrate how suitable data structures and algorithms can support these operations and how their computational costs compare.

### Objectives

- Implement important data structures in C++.
- Apply searching and sorting algorithms to realistic courier operations.
- Use a balanced tree for efficient parcel lookup.
- Use graphs to model delivery locations and find routes.
- Demonstrate FIFO queues, priority queues, and stack-based undo.
- Compare algorithms using Big O notation and operation counts.
- Explain every selected data structure and algorithm in the report and presentation.

### Scope and constraints

- The application will run in a terminal/console.
- All records will be kept in memory and will be lost when the program exits.
- No database, network service, or GUI is required in the proposed version.
- C++ standard library utilities such as `string` and basic I/O are acceptable.
- Core DSA structures should be implemented by the team rather than hidden behind STL containers. For example, implement our own AVL tree, circular queue, heap-based priority queue, stack, and graph algorithms.
- OOP may organize the modules, but DSA must remain the main academic focus.

---

## 2. Proposed Application Architecture

```text
+-----------------------------------------+
|             Console Interface           |
|       Menus, inputs, reports, results   |
+---------------------+-------------------+
                      |
                      v
+-----------------------------------------+
|       Application / Business Logic      |
| Parcel creation, tracking, dispatch,    |
| status updates, undo, route planning    |
+---------------------+-------------------+
                      |
                      v
+-----------------------------------------+
|                 DSA Engine              |
| Dynamic Array / Linked List (if needed) |
| AVL Tree | Circular Queue | Heap        |
| Stack | Graph | Search | Sorting        |
+---------------------+-------------------+
                      |
                      v
+-----------------------------------------+
|              In-Memory Data             |
| Primary parcel records and indexes      |
| All data disappears when program exits  |
+-----------------------------------------+
```

This separation prevents the project from becoming one very large `main()` function and lets group members develop modules independently.

---

## 3. Main Features

### 3.1 Parcel management

Users can:
- Add a parcel with a unique tracking ID.
- View all parcel records.
- Search for a parcel.
- Update allowed parcel details or status.
- Cancel a parcel if it is still eligible for cancellation.
- Display parcel statistics.

**DSA:** Dynamic array as the primary store; a linked list may be included as a separate demonstration if it adds a clear learning benefit.

### 3.2 Smart tracking and search

Users can search for a parcel by tracking ID using:
- Linear search.
- Binary search on sorted data.
- AVL tree lookup.

The project can compare the logic and performance of these approaches.

**Important:** Binary search requires sorted data. If the array must be sorted before a search, the sorting cost must be counted separately. An AVL tree instead maintains a searchable index as records change.

### 3.3 Parcel sorting and analytics

Sort parcels by:
- Tracking ID.
- Delivery fee.
- Priority or another clearly defined field.

Implement:
- Merge sort.
- Quick sort.

Show summaries such as pending, dispatched, delivered, and cancelled parcel counts.

### 3.4 Dispatch management

Ordinary parcels enter a **first-in, first-out (FIFO)** dispatch queue. The earliest eligible parcel is processed first.

**DSA:** Circular queue.

The system should prevent duplicate queue entries and disallow invalid status transitions.

### 3.5 Priority deliveries

Urgent parcels should be selected ahead of lower-priority parcels.

**DSA:** Binary heap-based priority queue.

Define the priority rule clearly. For example, a smaller numeric value could mean a more urgent parcel. Add a tie-break rule, such as earlier enqueue time or lower tracking ID, so results are predictable.

### 3.6 Delivery route planner

Represent service locations as graph vertices and road connections as edges. Edge weights represent simulated distance or cost.

Implement:
- BFS to find a path with the fewest edges in an unweighted graph and to identify reachable locations.
- DFS to explore the connected network.
- Dijkstra's algorithm to find a minimum-total-weight route when all edge weights are non-negative.

These are simulated routes, not live navigation or traffic predictions.

### 3.7 Undo the last supported operation

A user can reverse selected actions, such as parcel creation or a parcel status change.

**DSA:** Stack (last-in, first-out).

Each undo record must contain enough information to restore the previous state. If an operation changes several structures, undo must restore all of them consistently.

**Scope simplification:** Initially, allow cancellation only before dispatch. This avoids the need to remove arbitrary items from the middle of a dispatch queue.

### 3.8 DSA performance analysis

Include a menu option or report to explain:
- Which algorithm was used.
- Why the data structure fits the operation.
- Time and space complexity.
- Number of comparisons or other operation counts where practical.
- Results from tests on the same input data.

Do not claim an algorithm is always faster based on one timing run. For fair comparisons, use identical data, repeat tests, and distinguish measured time from theoretical complexity.

---

## 4. Data Structures: Definitions and Complexity

Let `n` be the number of records/elements, `V` the number of graph vertices, and `E` the number of graph edges.

| Data structure | Definition and purpose | Typical time complexity |
|---|---|---|
| Dynamic array | Contiguous, resizable storage for parcel records | Indexed access `O(1)`; append amortized `O(1)`; middle insertion/deletion `O(n)` |
| Singly linked list | Nodes connected by pointers; useful for sequential record management | Search `O(n)`; insert at head `O(1)` |
| Circular queue | FIFO queue that reuses array positions after dequeue | Enqueue/dequeue `O(1)` |
| Binary heap | Complete binary tree used to keep the highest-priority item at the root | Peek `O(1)`; insert/remove top `O(log n)` |
| Stack | LIFO structure suitable for undo history | Push/pop `O(1)` |
| AVL tree | Self-balancing binary search tree used as a tracking-ID index | Search/insert/delete `O(log n)` |
| Graph | Vertices represent locations; edges represent connections | BFS/DFS `O(V + E)` with adjacency lists |

### Important implementation decision

Use a dynamic array as the primary parcel store and an AVL tree as a secondary tracking index. The AVL tree should store tracking IDs and a stable reference to the relevant parcel record, or use an index strategy that is kept correct after array changes.

Do not assume that the entire system has `O(log n)` insertion/deletion just because AVL operations are logarithmic. If deleting a record requires shifting the primary array, that part still costs `O(n)`. Index consistency must be addressed in the design.

A plain BST can be added as a comparison exercise, but an ordinary BST can degrade to `O(n)` when skewed. An AVL tree maintains logarithmic height.

### Avoid unnecessary duplication

Do not use both a dynamic array and linked list to store the same records unless they have clearly different roles. A linked list can instead be a small independent demonstration or used where its operations provide a real benefit.

---

## 5. Searching Algorithms

| Algorithm | Best case | Average case | Worst case | Requirement |
|---|---:|---:|---:|---|
| Linear search | `O(1)` | `O(n)` | `O(n)` | No sorting needed |
| Binary search | `O(1)` | `O(log n)` | `O(log n)` | Data must be sorted |
| AVL tree search | `O(1)` if root matches | `O(log n)` | `O(log n)` | AVL index must be maintained |

**Explanation:**
- **Linear search:** checks records one by one until the target is found or the list ends.
- **Binary search:** compares with the middle of a sorted sequence and repeatedly discards half the remaining search range.
- **AVL search:** follows left/right child links according to the tracking ID while the tree remains balanced.

For a fair comparison, explain whether sorting time is included in the binary-search cost.

---

## 6. Sorting Algorithms

| Algorithm | Average time | Worst-case time | Typical extra space |
|---|---:|---:|---:|
| Merge sort | `O(n log n)` | `O(n log n)` | `O(n)` |
| Quick sort | `O(n log n)` | `O(n²)` | Typically `O(log n)` stack space on average; worst-case recursion can be `O(n)` |

### Merge sort

1. Divide the parcel list into smaller halves.
2. Recursively sort each half.
3. Merge the sorted halves.

Merge sort offers predictable `O(n log n)` time but needs extra memory for the usual array implementation.

### Quick sort

1. Choose a pivot.
2. Partition records around the pivot.
3. Recursively sort the partitions.

Quick sort is often efficient in practice, but poor pivot choices can lead to `O(n²)` worst-case time. A sensible pivot strategy and recursion handling should be selected and explained.

Both algorithms should sort copies of the same test dataset when being compared, so the first algorithm does not receive an already-sorted input unintentionally.

---

## 7. Graph and Route Planning

Create a small illustrative network of service locations:

- A — Hub
- B — Banasree
- C — Rampura
- D — Aftabnagar
- E — Delivery point

Connections between locations become graph edges. Weights represent assumed distance or cost, not real-time road data.

Example:

```text
A --4-- B --5-- E
 \       \
  7       2
   \       \
    C --3-- D
```

For a route from A to E:
- Direct connection cost, if one exists, can be compared with alternative routes.
- If `A → B → E` costs `4 + 5 = 9` and a direct route costs `12`, the shorter weighted route is `A → B → E`.

### Algorithms

**BFS (Breadth-First Search)**
- Explores vertices level by level.
- Finds a path with the fewest edges in an unweighted graph.
- Time: `O(V + E)` using an adjacency list.

**DFS (Depth-First Search)**
- Explores as far as possible along a branch before backtracking.
- Useful for exploring reachability and connected areas.
- Time: `O(V + E)` using an adjacency list.

**Dijkstra's algorithm**
- Finds a minimum-total-weight route when all edge weights are non-negative.
- With an adjacency list and a suitable min-heap, typical time is `O((V + E) log V)`.
- A simpler array-based implementation takes `O(V²)`.

Do not describe this as live route optimization. Traffic, road closures, and actual travel times are outside the proposed scope.

---

## 8. Proposed C++ Module and Class Design

Use C++17. Classes organize responsibilities; data structures and algorithms remain explicit and testable.

| Module/class | Responsibility |
|---|---|
| `Parcel` | Store tracking ID, sender, receiver, destination, fee, priority, and status |
| `ParcelStore` | Primary dynamic-array storage and basic record operations |
| `AVLTree` | Index parcel tracking IDs and implement rotations/search/insert/delete |
| `CircularQueue` | Ordinary FIFO dispatch |
| `PriorityQueue` | Urgent parcel dispatch using a binary heap |
| `UndoStack` | Store reversible actions |
| `DeliveryGraph` | Store locations and implement BFS, DFS, and Dijkstra |
| `SortingAlgorithms` | Implement merge sort and quick sort |
| `SearchAlgorithms` | Implement linear and binary search |
| `CourierSystem` | Connect modules to menus and coordinate business rules |

### Illustrative `Parcel` and `ParcelStore` interface

This is a starting interface, not a complete application:

```cpp
#include <iostream>
#include <string>
using namespace std;

enum class ParcelStatus {
    Pending,
    Dispatched,
    Delivered,
    Cancelled
};

struct Parcel {
    int trackingId;
    string sender;
    string receiver;
    string destination;
    double deliveryFee;
    int priority;
    ParcelStatus status;
};

class ParcelStore {
private:
    Parcel* data;
    int capacity;
    int count;

    void resize();

public:
    ParcelStore();
    ~ParcelStore();

    ParcelStore(const ParcelStore&) = delete;
    ParcelStore& operator=(const ParcelStore&) = delete;

    bool add(const Parcel& parcel);
    bool remove(int trackingId);
    Parcel* findLinear(int trackingId);
    void displayAll() const;
    int size() const;
};
```

The eventual implementation must:
- Allocate an initial dynamic array.
- Increase capacity when full.
- Handle duplicate tracking IDs.
- Shift or otherwise manage elements on deletion.
- Release allocated memory in the destructor.
- Keep the AVL index and other structures consistent.

The copy constructor and copy assignment are deleted because this class owns a raw dynamically allocated array. A different design could implement proper deep copying or use RAII/smart pointers, but ownership must be handled safely.

---

## 9. Proposed Console Menu

```text
========================================
             COURIERX SYSTEM
========================================
1. Parcel Management
2. Search and Track Parcel
3. Sort Parcels
4. Dispatch Management
5. Priority Deliveries
6. Delivery Route Planner
7. Undo Last Operation
8. DSA Performance Analysis
9. System Statistics
0. Exit
========================================
Enter your choice:
```

The menu is only the interface. The main academic value is the implementation behind each option, the reason for selecting each structure, and the complexity analysis.

---

## 10. Expected Time Complexity of Major Operations

These are expected costs for the proposed design. The exact total depends on how the primary store, AVL index, queues, and undo records are coordinated.

| Operation | Expected complexity | Reason |
|---|---:|---|
| Display all parcels | `O(n)` | Visit every record |
| Append parcel to dynamic array | Amortized `O(1)` | Occasional resizing |
| Insert parcel into AVL index | `O(log n)` | Balanced-tree insertion |
| Add parcel to store and index | Amortized `O(1) + O(log n)` | Array append plus AVL insertion, excluding validation/other work |
| Remove from primary array | `O(n)` | Search and/or shift elements |
| Search by AVL tracking index | `O(log n)` | Balanced tree traversal |
| Linear search | `O(n)` | May inspect every record |
| Binary search | `O(log n)` | Halves sorted search range |
| Merge sort | `O(n log n)` | Divide and merge |
| Queue enqueue/dequeue | `O(1)` | Circular-buffer operations |
| Priority queue insert/remove top | `O(log n)` | Heap adjustment |
| AVL deletion | `O(log n)` | Search and rebalance |
| BFS/DFS | `O(V + E)` | Visit vertices and edges |
| Dijkstra with min-heap | `O((V + E) log V)` | Weighted graph exploration |

### Space complexity

- Dynamic array: `O(n)` capacity.
- Linked list: `O(n)` nodes.
- AVL tree: `O(n)` nodes.
- Queue/heap/stack: `O(n)` in the worst case.
- Adjacency-list graph: `O(V + E)`.
- Merge sort: typically `O(n)` auxiliary array space.
- Recursive quick sort: average `O(log n)` stack space; worst-case `O(n)`.

These estimates should be updated if the final implementation differs from the proposed design.

---

## 11. Data Consistency and Business Rules

Several structures will refer to the same parcel. The system must preserve consistency.

1. Every parcel has a unique tracking ID.
2. The primary parcel store is the source of truth.
3. The AVL index is updated whenever a parcel is inserted or removed.
4. Dispatch queues contain only eligible parcels.
5. Duplicate queue entries are prevented.
6. Invalid status transitions are rejected.
7. A delivered or cancelled parcel cannot be dispatched again.
8. Undo restores all affected structures and fields, not only the visible status.
9. The priority rule and tie-break rule are documented.
10. The program handles invalid input and missing tracking IDs safely.

**Recommended initial rule:** a parcel can be cancelled only before dispatch. More complex rollback after dispatch can be added only if the group can implement and test it correctly.

---

## 12. Group Development Plan

### Phase 1 — Requirements and design
- Confirm features and parcel fields.
- Define parcel statuses and legal transitions.
- Agree on module interfaces and ownership rules.
- Draw the menu flow and basic diagrams.

### Phase 2 — Core records
- Implement the dynamic array.
- Add input validation, unique IDs, insertion, deletion, traversal, and statistics.
- Test empty and full-capacity conditions.

### Phase 3 — Searching and sorting
- Implement linear and binary search.
- Implement merge sort and quick sort.
- Test on the same datasets and record operation counts.

### Phase 4 — Advanced data structures
- Implement AVL tree and test rotations.
- Implement circular queue and binary heap.
- Implement stack-based undo records.
- Test each module independently before integration.

### Phase 5 — Graph algorithms and integration
- Build the adjacency-list graph.
- Implement BFS, DFS, and Dijkstra.
- Connect the modules to the console interface.
- Check consistency between the primary store and indexes/queues.

### Phase 6 — Testing and presentation
Test at least:
- Empty data structures.
- Duplicate tracking IDs.
- Missing parcel IDs.
- Invalid input and invalid status transitions.
- Queue empty/full conditions if bounded.
- Equal-priority parcels and tie-breaking.
- Disconnected graph locations.
- Graph routes with different total weights.
- AVL rotations and deletion.
- Undo after each supported operation.
- Large enough datasets to show complexity differences.

Prepare the report, diagrams, Big O tables, test evidence, and a live demonstration sequence.

---

## 13. Suggested Group Responsibilities

For a four-member team, divide work by technical module rather than assigning all difficult algorithms to one person.

| Member | Main responsibility | Deliverables |
|---|---|---|
| Member 1 | Parcel records and primary storage | Dynamic array, validation, basic CRUD |
| Member 2 | Searching and sorting | Linear/binary search, merge/quick sort, benchmarks |
| Member 3 | Trees and dispatch structures | AVL tree, circular queue, heap, undo stack |
| Member 4 | Graphs and integration | Graph, BFS/DFS, Dijkstra, menu integration |

Adjust this division to match the actual number of members and their skill levels. Everyone should understand the overall architecture and be able to explain their own code, correctness, and complexity during the presentation.

---

## 14. Final Deliverables

The completed project should contain:

- Working C++17 console application.
- Clearly organized source code.
- Project report defining every selected data structure and algorithm.
- Diagrams for AVL rotations, heap operations, queue processing, and graph routing.
- Big O time and space complexity analysis.
- Test cases with expected and actual results.
- Performance comparison using operation counts and, optionally, repeated timing measurements.
- Presentation slides and a live demo plan.
- Limitations section explaining in-memory storage, simulated route weights, and simplified business rules.

---

## 15. Scope Priorities

### Core requirements
- Dynamic-array parcel storage.
- Linear and binary search.
- Merge sort and quick sort.
- AVL tracking index.
- Circular dispatch queue.
- Binary heap priority queue.
- Stack-based undo for selected operations.
- Graph representation with BFS, DFS, and Dijkstra.
- Complexity analysis and testing.

### Optional extensions
- Linked-list demonstration if it serves a clear purpose.
- Plain BST versus AVL comparison.
- More sorting algorithms.
- More realistic dispatch constraints.
- Additional route algorithms, if the core is already correct and tested.

Do not add a GUI, database, network service, or complicated login system unless the teacher requests it. Those features can consume time without demonstrating more DSA.

---

## 16. Questions for the Team Discussion

Before implementation, agree on these points:

1. Is the proposed scope realistic for our semester and team size?
2. Which features are essential for the first working version?
3. Will we implement both a dynamic array and a linked list, or keep the linked list optional?
4. How will the AVL tree refer to parcels when the dynamic array changes?
5. Which operations can be undone safely?
6. What exact priority rule will the heap use?
7. How many locations and edges will our demonstration graph contain?
8. Which datasets will we use to compare searching and sorting?
9. How will we test correctness before combining modules?
10. Who will own integration, testing, report writing, and presentation?

