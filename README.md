**Using-Djikstras-and-Floyd-Warshall-to-establish-a-connection-between-2-pair-of-nodes**

The main aim of this project is to learn the concepts of virtual circuit switching and given a topology file and a connection request file, we try to establish a connection between the source and destination of the connection request keeping in mind the propagation between the nodes and the link capacity of the pair of nodes.

Virtual Circuit is the computer network providing connection-oriented service. It is a connection-oriented network. In virtual circuit resource are reserve for the time interval of data transmission between two nodes. This network is a highly reliable medium of transfer. Virtual circuits are costly to implement.


 
Working of Virtual Circuit:

•	In the first step a medium is set up between the two end nodes.
•	Resources are reserved for the transmission of packets.
•	Then a signal is sent to sender to tell the medium is set up and transmission can be started.
•	It ensures the transmission of all packets.
•	A global header is used in the first packet of the connection.
•	Whenever data is to be transmitted a new connection is set up.
Congestion Control in Virtual Circuit:

Once the congestion is detected in virtual circuit network, closed-loop techniques is used. There are different approaches in this technique:

•	No new connection –
No new connections are established when the congestion is detected. This approach is used in telephone networks where no new calls are established when the exchange is overloaded.

•	Participation of congested router invalid –
Another approach to control congestion is allow all new connections but route these new connections in such a way that congested router is not part of this route.

•	Negotiation –
To negotiate different parameters between sender and receiver of the network, when the connection is established. During the set up time, host specifies the shape and volume of the traffic, quality of service and other parameters.

Advantages of Virtual Circuit:

	Packets are delivered to the receiver in the same order sent by the sender.
	Virtual circuit is a reliable network circuit.
	There is no need for overhead in each packet.
	Single global packet overhead is used in virtual circuit.

Disadvantages of Virtual Circuit:

	Virtual circuit is costly to implement.
	It provides only connection-oriented service.
	Always a new connection set up is required for transmission.

**Chapter 2: Algorithms Used**

We have used the following algorithms in order to find the shortest path and second shortest path between every pair of nodes.

1. Floyd Warshall Algorithm (all pair shortest path).
2. Dijkstra’s Algorithm

**1. Floyd Warshall Algorithm 

We initialize the solution matrix same as the input graph matrix as a first step. Then we update the solution matrix by considering all vertices as an intermediate vertex. The idea is to one by one pick all vertices and updates all shortest paths which include the picked vertex as an intermediate vertex in the shortest path. When we pick vertex number k as an intermediate vertex, we already have considered vertices {0, 1, 2, .. k-1} as intermediate vertices. For every pair (i, j) of the source and destination vertices respectively, there are two possible cases. 
1)  k is not an intermediate vertex in shortest path from i to j. We keep the value of dist[i][j] as it is. 
2)  k is an intermediate vertex in shortest path from i to j. We update the value of            dist[i][j] as dist[i][k] + dist[k][j] if dist[i][j] > dist[i][k] + dist[k][j]

 
                                  


**2. Dijkstra’s Algorithm

Dijkstra’s algorithm is very similar to Prim’s algorithm for minimum spanning tree. Like Prim’s MST, we generate a SPT (shortest path tree) with given source as root. We maintain two sets, one set contains vertices included in shortest path tree, and other set includes vertices not yet included in shortest path tree. At every step of the algorithm, we find a vertex which is in the other set (set of not yet included) and has a minimum distance from the source.
Below are the detailed steps used in Dijkstra’s algorithm to find the shortest path from a single source vertex to all other vertices in the given graph. 
Algorithm 

1) Create a set sptSet (shortest path tree set) that keeps track of vertices included in shortest path tree, i.e., whose minimum distance from source is calculated and finalized. Initially, this set is empty. 
2) Assign a distance value to all vertices in the input graph. Initialize all distance values as INFINITE. Assign distance value as 0 for the source vertex so that it is picked first. 
3) While sptSet doesn’t include all vertices 
a) Pick a vertex u which is not there in sptSet and has minimum distance value. 
b) Include u to sptSet. 
c) Update distance value of all adjacent vertices of u. To update the distance values, iterate through all adjacent vertices. For every adjacent vertex v, if sum of distance value of u (from source) and weight of edge u-v, is less than the distance value of v, then update the distance value of v. 


**CHAPTER 3: IMPLEMENTAION

**3.1 Inputs

The command line will specify the following:

	topology of the network
	connections between nodes
	routing tables
	forwarding table
	path taken from one node to another

For e.g., the input can be given as below:
% ./routing −top topology file −conn connectionsfile −rt routingtablefile − ft forwardingtablefile − path pathsfile − flag hop|dist − p 0|1
The topology file contains on the first line: NodeCount (N), Edgecount (E) of the network. Nodes are numbered from 0 through N−1. On each subsequent line, there are four numbers – the first two integers represent the two endpoints of a bi-directional link, the third integer denotes the link’s propagation delay (in ms), and the fourth integer denotes the link’s capacity (in Mbps).
The connections file contains on the first line: Number of Connection Requests (R). Connections are numbered from 0 through R − 1. On each subsequent line, there are 5 numbers – the first two integers represent the source and destination node of a unidirectional connection, the remaining 3 integers denote the connection’s stated capacity (in Mbps). The stated capacity of a connection request i specifies the requested bandwidth using three integers: ( ), which specify the minimum, average, and maximum bandwidth needed, respectively. Please note that there may be several connections between the same source-destination pair.

**3.2 Routing:

The program will first determine two shortest cost (not necessarily link-disjoint) paths for all node-pairs. You may use either hop or distance metric. The command line parameter will specify the choice.

**3.3 Connections

The program will then process the specified set of connection requests.

Optimistic Approach: This is used if −p command-line argument has value 0. Let Cl  denote the total capacity of a link. 
Let biequiv = min[bimax ,bavei + 0.25 ∗ (bmaxi − bmini )]. A connection is admitted if the following condition is met, along each link of the path selected for connection i:  
                                                  
where n denotes the number of existing connections sharing a given link (along the path selected for the given connection) and j denotes the index of a connection assigned on this link.
Next, for each source-destination pair, select the two link-disjoint paths. If adequate bandwidth is not available along the first shortest-cost path, then the network attempts to set up the connection on the second shortest cost path. If this also fails, then the connection is NOT admitted, i.e. it fails to meet the admission control test. Once an available path is identified, the connection will be set up along the path. This primarily involves setting up link-unique VCIDs for a given connection request along all links of the path, and updating the corresponding forwarding tables at each intermediate node along the path.
Pessimistic Approach: If −p value in command-line argument is 1 then use this approach. Let Cl denote the total capacity of link l. A connection is admitted if the following condition is met, along each link of the path selected for connection i:
	                                      
where j denotes the index of a connection assigned on a given link along the path selected for the given connection. This gets repeated for the pessimistic approach.

**3.4 Outputs

The routingtablefile will contain the routing table information for all the nodes. For each network node, the corresponding routing table (i.e. two link-disjoint paths from the given node to all other nodes) displayed with the following fields:
Destination Node	Path	Path Delay	Path Cost

Table 1: routingtablefile format
The forwardingtablefile will contain the forwarding table information for all the nodes. For each network node, the corresponding forwarding table (for all established connections) will have following format:
             **Router’s ID  |	ID of Incoming Port |	VCID	 |  ID of Outgoing Port  |	VCID

Table 2: forwardingtablefile format
pathsfile will contain only one line consisting of two integers which representthe total number of requested connections and the total number of admitted connections, respectively.
For each connection that is admitted into the network, the output format is as follows:
                **Connection ID  |	Source	 |  Destination |	Path | 	VCID | List	Path Cost
    




 
