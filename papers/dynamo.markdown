---
title: Paper reading
---

## Dynamo: Amazon's Highly Available Key-value Store
Giuseppe DeCandia, Deniz Hastorun, Madan Jampani, Gunavardhan Kakulapati,Avinash Lakshman, Alex Pilchin, Swaminathan Sivasubramanian, Peter Vosshalland Werner Vogels  
Amazon.com  

SOSP'07 

Design requirements:  
- reliability  
- availbility (scaling needs)  
- weak consistency (eventual consistency)  

There're many services on Amazon's platform that only need primary-key access to a data store. So thet design and implement dynamo.

Key techniques:  
- consistent hashing  
- object version  
- consistenct model between replicas  
- gossip based distribute failure detection and membership protocol  

Design objections:  
- simple key/value interface  
- highly available with a clearly defined consistency window  
- efficient resource usage  
- a simple scale out scheme to address growth in data set size or request rates  

Each service that uses Dynamo runs its own Dynamo instances. So every dynamo system would not be too big, most of them will consist of hundreds or thousands of machines.

Design considerations:  
* eventual consistency,   
    * so when to perform the process of resolving the update conflict, read or write? Dynamo chooses writing moment for good cunstomer experience when 	shopping like adding or removing his shopping cart service.    
    * and who performs the process of conflict resoluti    on?	data store or application itself ? Dynamo chooses application because of its better semantic information.  
* Incremental scalability - add one machine each time  
* Symmetry - all nodes are equal on responsbility  
* Decentralization - no centralized or master node like GFS  
* Heterogeneity - can add new nodes with high capacity

##### System architecture  
- simple interface  
	get(key)
	put(key, context, object): context includes the version information  
- partition algorithm
	consistent hashing: output range of the hash function is a fixed circular space or "ring". Each node contains a range in the hash space and in charge of the store work for the key that hashes into this range. Using virtual node instead of normal node for load balancing and heterogeneiry of whole system. Problem, how to handle new node creation and exit, how to dispatch virtual node to physical machine dynamically and automatically?  
- Replication  
	Each data item is replicated at N hosts, where N can be configured per application. The coordinator node who store the key will be in charge of the replication of the data items. Dynamo replicates these keys at the N-1 clockwise successor nodes of coordinator node in the ring. However, it has some problems: 1. this replication schema is not aware of actul network topology of local network and will waste the bandwidth of whole system; 2. this continuous solution will dispatch many replication virtual nodes into same physical machine which reduce the failure tolerance.  
- Data versioning
	vector clock: (node counter), maybe too long and the truncation scheme is not theoretically perfect but practical efficient.   
- Execution of get () and put () operations  
	request routing: 1. route its request through a generic load balancer that will select a node based on load information; 2. use a partition-aware client library that routes requests directly to the appropriate coordinator nodes  
	consistency protocol: quorum systems, R+W>N  
- Handling Failures  
	how does others know that the data item is ransferred to node D when node A fail to work? The coordinator ?   
	So, it's still unclear how dynamo store its object's replications in whole system. It use virtual nodes, it wants to tolerate the failure of machine in preference list and it want the replicate data across multiple data centers. The missing point is how dynamo dispatch its virtual node into physical node?  
	anti-entropy (replica synchronization) protocol to keep the replicas synchronized; merkle tree; Each node maintains a merkle tree for each key range it hosts  
	

##### Implementation  

Three main software components:  
- request coordination  
	built on top of an event-driven messaging substrate where the message processing pipeline is split into multiple stages similar to the SEDA architecture. All communications are implemented using Java NIO channels  
	read repairs  
- membership and failure detection  
- local persistance engine, very flexible  
	BDB Transactional Data Store / BDB Java Edition / MySql / in-memory buffer with persistent back store

