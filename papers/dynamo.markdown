---
title: Paper reading
---

## Dynamo: Amazon’s Highly Available Key-value Store
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

