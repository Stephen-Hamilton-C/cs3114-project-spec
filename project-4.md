# Project 4 - Graphing
Notes on spec as of 11-3-23

## Assignment
<details>
  <summary><strong>Overview</strong></summary>
  We will be doing analysis on a subset of the Million Song database.
  Records will store artist and track names.
  We will primarily be using HashTables and Graphs in this project.
</details>
- Analyzing a subset of the Million Song database
- Records store artist and track names
- We will use HashTables and Graphs
  - Copy your HashTable from Project 1! If it isn't generic, now's a good time to make it so.

<details>
  <summary><strong>Closed Hash Table</strong></summary>
  We will use a **closed hash table**. Two instances will be created.
  One instance will access artists names and the other will access song titles.
  Read over Chapter 10 in OpenDSA for more details on hashing.
  Use the `sfold` string hash function described in OpenDSA.
  This is already given in the project starter kit.
  Use quadratic probing for collision resolution.
  In other words, the i'th probe step will be i^2 slots from the initial slot.
  The hash table should be **extensible**.
  So it will grow as it fills up.
  If it exceeds 50% full from an insert, expand the array and rehash all items.
</details>
- Two closed hash tables, one for artist names, another for song titles
- Read OpenDSA Chapter 10
- Use `sfold` string hashing as described in OpenDSA
- Use quadratic probing for HashTable collision resolution
  - Meaning the i'th probe step will be i^2 slots from the initial slot, where i starts at 1
- HashTable should be extensible
  - Expands as it reaches half capacity
  - If an insert would cause the HashTable to be more than half full, it should expand

<details>
  <summary><strong>Graph</strong></summary>
  We will also make a Graph data structure.
  This will be implemented with the adjacency list data structure as depicted in OpenDSA Chapter 14
  It has nodes for each artist and each song,
  linking songs with their respective artists.
  When inserting, do the following:
  1. If this is a new artist or song, add new nodes to the graph
  2. Add a link in the graph between nodes for the artist and the song
  You have to make a good way for expanding nodes in the graph as necessary.
  Remember that nodes can be removed as well.
</details>
