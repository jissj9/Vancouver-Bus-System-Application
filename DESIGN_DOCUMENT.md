
# Design Document
### Vancouver Bus App

This program provides a simple-to-use GUI application which lets the user import TransLink Open API
bus stop and times data from CSV input files. They can then query the data using three different
methods: Find the least costly path between any two stops, search for stops by name and list every
route that has a specific arrival time in order of trip ID.

The program can also handle invalid user inputs and faulty CSV data. User input is sanitized using
text field formatters and limiting the types of available input before being passed to any
error-prone methods. For example, stop IDs must be valid positive integers. The imported files are
parsed line-by-line. Whitespace is trimmed and the line is split up. If there are not enough input
fields or a required value is missing, the line is simply thrown out. Invalid formats will raise
exceptions which will also cause the line to be ignored.

## Functionality 1. Shortest path between two stops

**Description:** The user enters both a start bus stop ID and the
destination bus stop ID. The program returns the total cost of the shortest
path taken as well as the list of bus stops on that path.

### Analysis

**Data Structures:**
In terms of data structures, a HashMap was used to create a map between BusStop objects and the
total cost to get there from the starting bus stop. A second HashMap was used to map BusStop
objects to other BusStop objects. This was used as a path leading back to the starting bus stop.
A PriorityQueue (space complexity of `O(n)` for `n` number of elements) was used in the
implementation of the Dijkstra's algorithm. Unevaluated stops were queued in order of total
cost from the starting bus stop.

**Algorithms:**
When implementing the shortest path, the input data type was considered, in order to
decide on the searching algorithm. While Dijkstra's algorithm does not work with negative
weights, the costs of traversing to stops are never negative. The main advantage of using
Dijkstra's algorithm is that it is better when it comes to reducing time complexity `O(ELogV)`
and the worst case being `O(V^2logV)`, in comparison to Bellman-Ford where the time complexity
is `O(V.E)`.

A* was also considered, which is generally a better algorithm for single-pair shortest path
finding. However, A* needs a heuristic to judge if a path taken leads closer or further from the
goal. Unfortunately, there doesn't appear to be a heuristic available with the input data.
As such, it is not possible to use an informed algorithm like A*.

Another consideration we made was calculating all the shortest paths from every stop to every
other stop when the data was imported. We could have used Floyd-Warshall to perform this
calculation. However, we reasoned that it was unnecessary to force the user to wait for every
shortest path to be calculated if they are probably only interested in a tiny fraction of those
results. Therefore, it was concluded that we would use Dijkstra's algorithm after every request
from the user.

## Functionality 2. Searching by bus stop names using a TST

**Description:** The user enters a string search query. The program
returns all bus stop objects with names that begin with the searched string.

### Analysis

**Data Structures:**
In order to contain all of the Bus Stop objects, which hold all information of a single
bus stop, we have a hash map that connects the Bus Stop id to the corresponding Bus Stop
object. This allows for accessibility of Bus Stop objects as used in all other aspects of
the program. As this part of the project needs to return the information of the correct
Bus Stops, this hash map was accessed. In addition to the original hash map, another hash
map was created, which had keys of the names (with all keywords moved to the end) and values
of the ids. Therefore, once the names were found using the searching algorithm we could then
use a get method on the new hash map to get the corresponding id and then use a get on the
original hash map with the retrieved id as a key to get the correct Bus Stop object.

Additionally, a Ternary Search Tree was implemented. In order to accomplish this, a Node
object was created with the fields data, which contains the character of the tree node,
left, mid, and right nodes for accessing characters less than, equal to, or greater than
the current character value, and a termination boolean, which notes whether the node is
the final character in a string. The root node is stored in the overarching Bus Stops object.

**Algorithms:**
When searching for all results, the first step was to search the tree for the query as a
prefix. If the prefix is found in the tree, it is then traversed for all words that begin
with that prefix. This was done using breadth first search which looked for all nodes that
had a termination value of true.

**Considerations**
By implementing insert and search recursively, there was a space cost trade off to avoid
time increase. In order to avoid excessive checks and an `O(n^2)`, the recursive calls create
smaller arrays with each function call, which means that during the function there is
significant memory usage. However, since the strings are not very long and the tree becomes
massive quickly, this memory usage is not consequential at the end and less efficient tree traversal
would make a significant impact.

## Functionality 3. Searching by trip arrival time

**Description:** The user enters a time in hh:mm:ss format. The program returns a list
of all the trips which have an arrival time equal to the users input. The returned list
is sorted by the ID of each trip.

### Analysis

**Algorithms:**
We defined a trip as a type of route between two bus stops. We placed all of this data
into an object called BusTrip. Each BusStop has a list of BusTrip objects which leave that
stop and go to another stop. For other parts of the project to get a list of all bus trips
with a specific value, we found that it was efficient to use Java 8's new streams. This
allowed us to use `.flatMap()` to extract the BusTrip objects from the stream of BusStop
objects. We then used `.filter()` to filter out trips with the wrong arrival time. Both
of these stream operations are simply iterative. Finally, we use `.sorted()` on the stream
to order the collected output by trip ID. Overall, this operation has a worst case time
complexity of `O(n log n)`.

**Considerations**
Originally, we had used a Java TreeMap to associate trip IDs to BusTrip objects. Once all
the trips had been loaded in from `stop_times.txt`, the trips would be sorted by ID into a
TreeMap with time complexity `O(n log n)`. This would make it trivial to return a list of trips
with a specific arrival time sorted by ID, as you could simply iterate over the TreeMap values
and add anything matching the criteria to a list, this with an amortized time complexity of `O(n)`.
However, as the project grew, it became apparent that it was not reasonable to store
BusTrip objects in their own data structure separate from the rest. This was especially noticable
when trying to unite this part of the project with the shortest path graph. As such, all BusTrip
objects were instead added to lists inside BusStop objects, which in turn were stored in
the BusStops HashMap.

