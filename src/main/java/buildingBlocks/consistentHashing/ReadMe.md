# Consistent Hashing
In the world of cloud computing, distributed systems have gained popularity and relevance.

One such type of system, <b><i>distributed caches</i></b> that power many high-traffic dynamic websites and web applications, typically consists of a particular case of distributed hashing. These take advantage an algorithm known as <b><i>consistent hashing</i></b>.

<br><br>
## What is Hashing?
Hashing is a concept which uses a hash function that maps 
- one piece of data - typically describing some kind of object, often of arbitrary size.
- to another piece of data - typically an integer in finite range, known as <b><i>hash code or hash</b></i>.

Since there are way more possible inputs than the finite range of output hash codes, different inputs can be mapped to same hash code, a phenomenon called <b><i>Hashing collisions</b></i>.

<br><br>
## Introduction to Hash Tables?
A hash table uses hash codes to store data efficiently. Instead of searching through a long list of data rows, we can directly access the position n the dataset where our data is stored using its hash code. This makes data retrieval faster.

<br><br>
## The Need for Distribution
When an application grows, the number of users and the data grows. The data servers have a limited capacity to store the data, thus might need to split the data across multiple servers or nodes. By splitting data across multiple servers we can overcome the limitations of a single computer.

### Distributed hashing
- In distributed systems, it may be desirable to split hash table into several parts hosted by different servers. One of the motivations for this is to bypass the memory limitations of using a single computer. In such scenarios the object(and their keys) are distributed among several servers.
- A typical use case for this is the implementation of in-memory caches, such as <b><i>Memcached</b></i>.
- Such setups consist of a pool of caching servers that host many key/value pairs and are used to provide fast access to data originally stored (or computed) elsewhere.
- For example, to reduce the load on a database server and at the same time improve performance, an application can be designed to first fetch data from the cache servers, and only if it’s not present there—a situation known as <b><i>cache miss</b></i>—resort to the database, running the relevant query and caching the results with an appropriate key, so that it can be found next time it’s needed.


<br><br>
## The Rehashing Problem
This distribution scheme is simple (`hash modulo N`), intuitive, and works fine. That is, until the number of servers changes.

| KEY   | HASH       | HASH mod 3 |
|-------|------------|------------|
| john  | 1633428562 | 2          |
| bill  | 7594634739 | 0          |
| jane  | 5000799124 | 1          |
| steve | 9787173343 | 0          |
| kate  | 3421657995 | 2          |

After using `Hash modulo N` to map data to servers

| Server A | Server B | Server C |
|----------|----------|----------|
| bill     | jane     | john     |
| steve    |          | kate     |

- If one of the servers crashes or becomes unavailable, it needs to be removed or another server is added then keys need to be redistributed to account for the removed/added server.
- The problem with our simple modulo distribution is that when the number of servers changes, most `hashes modulo N` will change, so most keys will need to be moved to a different server.
- So, even if a single server is removed or added, all keys will likely need to be rehashed into a different server.

<br><br>
## The Solution: Consistent Hashing
We need a distribution scheme that does not depend directly on the number of servers, so that, when adding or removing servers, the number of keys that need to be relocated is minimized. One such scheme—a clever, yet surprisingly simple one—is called consistent hashing.

### How does Consistent Hashing Work?
- <b><i>Virtual Hash Ring:</i></b> Imagine a circle where both servers and keys(data) are placed on the edge of the circular hash ring. 
  - <i><u>minimum possible hash value</u></i> would correspond to angle of zero.
  - <i><u>The maximum possible hash value</u></i> would correspond to an angle of 2𝝅 radians. 
  - <i><u>All other values</u></i> would linearly fit somewhere between.
- <b><i>Server Placement:</i></b> Servers are hashed using their IP addresses and placed at positions on circular hash ring.
- <b><i>Key Placement:</i></b> Similarly keys are hashed and placed on the circular hash ring.
- <b><i>Key Assignment:</i></b> Each key is assigned to the server closest to it on the circular hash ring in counter-clockwise (or clockwise direction as per the implementation).

**For our example we’ll assume all three servers -> A,B,C:**

<div align="center">
  <img src="https://github.com/vipuldhurve/system-design/blob/main/assets/hash-ring-key-assignment-consistent-hashing.jpg" alt="Image" style="display:block; margin:auto;">
</div> 

| KEY   | HASH       | ANGLE (DEG) |        
|-------|------------|-------------|
| john  | 1633428562 | 58.7        |
| C     | 2269549488 | 81.7        |
| kate  | 3421657995 | 123.1       |
| jane  | 5000799124 | 180         |
| A     | 5572014557 | 200.5       |
| bill  | 7594634739 | 273.4       |
| B     | 8077113361 | 290.7       |
| steve | 787173343  | 352.3       |

After mapping requests to server:

| KEY   | HASH       | ANGLE (DEG) | LABEL | SERVER |
|-------|------------|-------------|-------|--------|
| john  | 1632929716 | 58.7        | C     | C      |
| kate  | 3421831276 | 123.1       | A     | A      |
| jane  | 5000648311 | 180         | A     | A      |
| bill  | 7594873884 | 273.4       | B     | B      |
| steve | 9786437450 | 352.3       | C     | C      |

### Virtual Nodes
To further balance the distribution and reduce the impact of adding or removing servers:

- <b><i>Virtual Nodes:</b></i> Each physical server is represented by multiple virtual nodes on the hash ring. These virtual nodes are distributed evenly across the ring.
- <b><i>Adding/Removing Servers:</b></i> When adding or removing a server, it is done by adding or removing its corresponding virtual nodes, which smooths out the distribution and minimizes disruption even more.

**For our example we’ll assume all three servers(A,B,C) have an equal weight of 10: i.e. nodes 0-9 for each**

<div align="center">
  <img src="https://github.com/vipuldhurve/system-design/blob/main/assets/hash-ring-virtual-nodes-consistent-hashing.jpg" alt="Image" style="display:block; margin:auto;">
</div> 

| KEY   | HASH       | ANGLE (DEG) |
|-------|------------|-------------|
| C6    | 408965526  | 14.7        |
| A1    | 473914830  | 17          |
| A2    | 548798874  | 19.7        |
| A3    | 1466730567 | 52.8        |
| C4    | 1493080938 | 53.7        |
| john  | 1633428562 | 58.7        |
| B2    | 1808009038 | 65          |
| C0    | 1982701318 | 71.3        |
| B3    | 2058758486 | 74.1        |
| A7    | 2162578920 | 77.8        |
| B4    | 2660265921 | 95.7        |
| C9    | 3359725419 | 120.9       |
| kate  | 3421657995 | 123.1       |
| A5    | 3434972143 | 123.6       |
| C1    | 3672205973 | 132.1       |
| C8    | 3750588567 | 135         |
| B0    | 4049028775 | 145.7       |
| B8    | 4755525684 | 171.1       |
| A9    | 4769549830 | 171.7       |
| jane  | 5000799124 | 180         |
| C7    | 5014097839 | 180.5       |
| B1    | 5444659173 | 196         |
| A6    | 6210502707 | 223.5       |
| A0    | 6511384141 | 234.4       |
| B9    | 7292819872 | 262.5       |
| C3    | 7330467663 | 263.8       |
| C5    | 7502566333 | 270         |
| bill  | 7594634739 | 273.4       |
| A4    | 8047401090 | 289.7       |
| C2    | 8605012288 | 309.7       |
| A8    | 8997397092 | 323.9       |
| B7    | 9038880553 | 325.3       |
| B5    | 9368225254 | 337.2       |
| B6    | 9379713761 | 337.6       |
| steve | 9787173343 | 352.3       |

| KEY   | HASH       | ANGLE (DEG) | LABEL | SERVER |
|-------|------------|-------------|-------|--------|
| john  | 1632929716 | 58.7        | B2    | B      |
| kate  | 3421831276 | 123.1       | A5    | A      |
| jane  | 5000648311 | 180         | C7    | C      |
| bill  | 7594873884 | 273.4       | A4    | A      |
| steve | 9786437450 | 352.3       | C6    | C      |

<br><br>
## 1. Removing a server
When a server is removed from the system:
- <b><i>Identify Affected Keys:</i></b> The keys that were assigned to the removed server needs to be redistributed.
- <b><i>Redistribute Data:</i></b>
  - The keys that were mapped to the removed server will now be mapped to the next server in the counter-clockwise direction on the hash ring.
  - Other keys are unaffected and remain on their current servers.
- <b><i>Impact:</i></b>  Removing a server similarly causes minimal reallocation of keys, roughly 1/N of the total keys.

**Removing server C with nodes C0...C9:**

| KEY   | HASH       | ANGLE (DEG) |
|-------|------------|-------------|
| A1    | 473914830  | 17          |
| A2    | 548798874  | 19.7        |
| A3    | 1466730567 | 52.8        |
| john  | 1633428562 | 58.7        |
| B2    | 1808009038 | 65          |
| B3    | 2058758486 | 74.1        |
| A7    | 2162578920 | 77.8        |
| B4    | 2660265921 | 95.7        |
| kate  | 3421657995 | 123.1       |
| A5    | 3434972143 | 123.6       |
| B0    | 4049028775 | 145.7       |
| B8    | 4755525684 | 171.1       |
| A9    | 4769549830 | 171.7       |
| jane  | 5000799124 | 180         |
| B1    | 5444659173 | 196         |
| A6    | 6210502707 | 223.5       |
| A0    | 6511384141 | 234.4       |
| B9    | 7292819872 | 262.5       |
| bill  | 7594634739 | 273.4       |
| A4    | 8047401090 | 289.7       |
| A8    | 8997397092 | 323.9       |
| B7    | 9038880553 | 325.3       |
| B5    | 9368225254 | 337.2       |
| B6    | 9379713761 | 337.6       |
| steve | 9787173343 | 352.3       |

**Request Mappings after removing server C:**

| KEY   | HASH       | ANGLE (DEG) | LABEL | SERVER |
|-------|------------|-------------|-------|--------|
| john  | 1632929716 | 58.7        | B2    | B      |
| kate  | 3421831276 | 123.1       | A5    | A      |
| jane  | 5000648311 | 180         | B1    | B      |
| bill  | 7594873884 | 273.4       | A4    | A      |
| steve | 9786437450 | 352.3       | A1    | A      |

<br><br>
## 2. Adding a server
When a new server is added to the system:
- <b><i>Hash the New Server:</i></b> The new server ip address is hashed and added to the hash ring.
- <b><i>Redistribute Data:</i></b> 
  - The new server will be responsible for the keys in the hash ring between - previous server(clockwise direction in our case) and the new server itself.
  - Only the keys that fall between the previous server and the new server in the hash ring will be moved to the new server.
  - Other keys remain on their existing servers.
- <b><i>Impact:</i></b> Adding a new server typically causes minimal reallocation of keys, about 1/N of the total keys, where N is the number of servers.

**Adding server D with nodes D0...D9**

| KEY   | HASH       | ANGLE (DEG) |
|-------|------------|-------------|
| D2    | 439890723  | 15.8        |
| A1    | 473914830  | 17          |
| A2    | 548798874  | 19.7        |
| D8    | 796709216  | 28.6        |
| D1    | 1008580939 | 36.3        |
| A3    | 1466730567 | 52.8        |
| D5    | 1587548309 | 57.1        |
| john  | 1633428562 | 58.7        |
| B2    | 1808009038 | 65          |
| B3    | 2058758486 | 74.1        |
| A7    | 2162578920 | 77.8        |
| B4    | 2660265921 | 95.7        |
| D4    | 2909395217 | 104.7       |
| kate  | 3421657995 | 123.1       |
| A5    | 3434972143 | 123.6       |
| D7    | 3567129743 | 128.4       |
| B0    | 4049028775 | 145.7       |
| B8    | 4755525684 | 171.1       |
| A9    | 4769549830 | 171.7       |
| jane  | 5000799124 | 180         |
| B1    | 5444659173 | 196         |
| D6    | 5703092354 | 205.3       |
| A6    | 6210502707 | 223.5       |
| A0    | 6511384141 | 234.4       |
| B9    | 7292819872 | 262.5       |
| bill  | 7594634739 | 273.4       |
| A4    | 8047401090 | 289.7       |
| D0    | 8272587142 | 297.8       |
| A8    | 8997397092 | 323.9       |
| B7    | 9038880553 | 325.3       |
| D3    | 9048608874 | 325.7       |
| D9    | 9314459653 | 335.3       |
| B5    | 9368225254 | 337.2       |
| B6    | 9379713761 | 337.6       |
| steve | 9787173343 | 352.3       |

**Request Mappings after adding server D:**

| KEY   | HASH       | ANGLE (DEG) | LABEL | SERVER |
|-------|------------|-------------|-------|--------|
| john  | 1632929716 | 58.7        | B2    | B      |
| kate  | 3421831276 | 123.1       | A5    | A      |
| jane  | 5000648311 | 180         | B1    | B      |
| bill  | 7594873884 | 273.4       | A4    | A      |
| steve | 9786437450 | 352.3       | D2    | D      |

<br><br>
#### #Resource: [www.toptal.com/consistent-hashing](https://www.toptal.com/big-data/consistent-hashing)