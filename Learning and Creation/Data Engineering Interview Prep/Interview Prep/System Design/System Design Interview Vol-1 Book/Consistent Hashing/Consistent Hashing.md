To achieve horizontal scaling, it is important to distribute requests/data efficiently and evenly across servers. Consistent hashing is a commonly used technique to achieve this goal.

### Rehashing problem

If you have n cache servers, a common way to balance the load is to use the following hash method:

**serverIndex = hash(key) % N, where N is the size of the server pool.**

![[/Untitled 50.png|Untitled 50.png]]

To fetch the server where a key is stored, we perform the modular operation f(key) % 4.

![[/Untitled 1 23.png|Untitled 1 23.png]]

This approach works well when the size of the server pool is fixed, and the data distribution is even. However, problems arise when new servers are added, or existing servers are removed.

For example, if server 1 goes offline, the size of the server pool becomes 3. Now, applying modular operation gives us different server indexes because the number of servers is reduced by 1. We get the results applying hash % 3:

![[/Untitled 2 22.png|Untitled 2 22.png]]

Most keys are redistributed, not just the ones originally stored in the offline server (server 1). This means that when server 1 goes offline, most cache clients will connect to the wrong servers to fetch data. This causes a storm of cache misses.

![[/Untitled 3 19.png|Untitled 3 19.png]]

### Consistent hashing

Consistent hashing is a special kind of hashing such that when a hash table is re-sized and consistent hashing is used, only k/n keys need to be remapped on average, where k is the number of keys, and n is the number of slots. In contrast, in most traditional hash tables, a change in the number of array slots causes nearly all keys to be remapped

### Hash space and hash ring

Assume SHA-1 is used as the hash function f, and the output range of the hash function is: x0, x1, x2, x3, …, xn. In cryptography, SHA-1’s hash space goes from 0 to 2^160 - 1.

That means x0 corresponds to 0, xn corresponds to 2^160 – 1, and all the other hash values in the middle fall between 0 and 2^160 - 1.

![[/Untitled 4 15.png|Untitled 4 15.png]]

By collecting both ends, we get a hash ring

![[/Untitled 5 14.png|Untitled 5 14.png]]

### Hash servers

Using the same hash function f, we map servers based on server IP or name onto the ring.

![[/Untitled 6 14.png|Untitled 6 14.png]]

### Hash keys

cache keys (key0, key1, key2, and key3) are hashed onto the hash ring

![[/Untitled 7 13.png|Untitled 7 13.png]]

  

### Server lookup

To determine which server a key is stored on, we go clockwise from the key position on the ring until a server is found

![[/Untitled 8 10.png|Untitled 8 10.png]]

  

### Add a server

Adding a new server will only require redistribution of a fraction of keys.

After a new server 4 is added, only key0 needs to be redistributed. k1, k2, and k3 remain on the same servers.

Before server 4 is added, key0 is stored on server 0. Now, key0 will be stored on server 4 because server 4 is the first server it encounters by going clockwise from key0’s position on the ring

![[/Untitled 9 9.png|Untitled 9 9.png]]

### Remove a server

When a server is removed, only a small fraction of keys require redistribution with consistent hashing. When server 1 is removed, only key1 must be remapped to server 2. The rest of the keys are unaffected.

![[/Untitled 10 8.png|Untitled 10 8.png]]

  

### Two issues in the basic approach

The basic steps for the consistent hashing are:

- Map servers and keys on to the ring using a uniformly distributed hash function.
- To find out which server a key is mapped to, go clockwise from the key position until the  
    first server on the ring is found.  
    

**Problem 1:**

it is impossible to keep the same size of partitions on the ring for all servers considering a server can be added or removed.

A partition is the hash space between adjacent servers. It is possible that the size of the partitions on the ring assigned to each server is very small or fairly large

if s1 is removed, s2’s partition (highlighted with the bidirectional arrows) is twice as large as s0 and s3’s partition.

![[/Untitled 11 8.png|Untitled 11 8.png]]

**Problem 2:**

It is possible to have a non-uniform key distribution on the ring. For instance, if servers are mapped to positions listed below, most of the keys are stored on server 2. However, server 1 and server 3 have no data.

![[/Untitled 12 8.png|Untitled 12 8.png]]

### Virtual nodes

A virtual node refers to the real node, and each server is represented by multiple virtual nodes on the ring.

In the below figure, both server 0 and server 1 have 3 virtual nodes. The 3 is arbitrarily chosen; and in real-world systems, the number of virtual nodes is much larger.

Instead of using s0, we have s0_0, s0_1, and s0_2 to represent server 0 on the ring. Similarly, s1_0, s1_1, and s1_2 represent server 1 on the ring.

With virtual nodes, each server is responsible for multiple partitions. Partitions (edges) with label s0 are managed by server 0. On the other hand, partitions with label s1 are managed by server 1.

![[/Untitled 13 6.png|Untitled 13 6.png]]

To find which server a key is stored on, we go clockwise from the key’s location and find the first virtual node encountered on the ring.

For example, to find out which server k0 is stored on, we go clockwise from k0’s location and find virtual node s1_1, which refers to server 1.

![[/Untitled 14 6.png|Untitled 14 6.png]]

**As the number of virtual nodes increases, the distribution of keys becomes more balanced. This is because the standard deviation gets smaller with more virtual nodes, leading to balanced data distribution.**