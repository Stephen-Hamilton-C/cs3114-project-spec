# Project 4 - Graphing
Notes on spec as of 11-6-23

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

- Use the Adjacency List data structure from OpenDSA Chapter 14
- Each node will contain either an artist or a song
- Song nodes will be linked to their respective artist nodes
- Insertion should perform these operations:
  - If new artist or song, add new nodes to the graph
  - Add a link in the graph between nodes for the artist and song
- The graph should be able to hold any number of nodes
- Remember that the graph can have nodes removed from it

<details>
  <summary><strong>Linking HashTable to Graph</strong></summary>

  The key is a string (artist name or song title) and the data is the corresponding node in the graph.
  The hash table gives access to graph nodes.
  
</details>

- HashTable keys will be Strings
  - Artist name/Song title
- HashTable data will be graph nodes

## Program Invocation

<details>
  <summary><strong>Arguments</strong></summary>

  `java GraphProject {initHashSize} {commandFile}`
  `initHashSize` is the initial size for both artist and song hash tables.
  This number must be greater than, or equal to 0.
  `commandFile` is the name of the file the program will read commands from.
</details>

- Two arguments
  - The initial hash table size for both tables
  - the name of the command file
- Read commands line-by-line from the command file

## Input and Output
<details>
  <summary><strong>Command Formatting</strong></summary>

  All commands have no unnecessary spaces or other white space or blank lines.
  Each command should have some sort of output message.
  Some output has a specific format, as is written further below.
  Only use `stdout` for output, never `stderr`.
</details>

- Commands won't have any funny whitespace, but you should still trim the entire line before processing it
- Never print to `stderr`
- Each command should output *something*.
  - Some commands have specific output requirements. See below.

<details>
  <summary><strong>Insert</strong></summary>

  `insert {artist-name}<SEP>{song-name}`  
  `<SEP>` is literal, this is not meant to be replaced by anything.
  An example insert command looks like this:
  ```
  insert Daft Punk<SEP>Give Life Back to Music
  ```
  Check if `artist-name` appears in the artist hash table.
  If it doesn't, create a graph node for that artist and add it to the table.
  Then, check if `song-name` appears in the song hash table.
  If it doesn't, create a graph node for that song and add it to the table.
  Print a message if an artist and/or song is added to the hash table.
  The output for the example above should look like this:
  ```
  |Daft Punk| is added to the Artist database.
  |Give Life Back to Music| is added to the Song database.
  ```
  Also, make sure to print a message if either hash table expands in size.
</details>

- `insert {artist-name}<SEP>{song-name}`
  - `<SEP>` is literal, and is not replaced by anything
  - Example insert command: `insert Daft Punk<SEP>Give Life Back to Music`
- First, check if `artist-name` appears in the artist hash table
  - If it doesn't, create a graph node for that artist and add it to the artist hash table.
  - Then print `|ARTIST| is added to the Artist database.`, replacing `ARTIST` with the artist name.
- Next, check if `song-name` appears in the song hash table
  - If it doesn't, create a graph node for that song and add it to the song hash table.
  - Then print `|SONG| is added to the Song database`, replacing `SONG` with the song name.
- If either hash table expands, a message should be printed indicated which hash table expanded.

<details>
  <summary><strong>Remove</strong></summary>

  ```
  remove {artist|song} {name}
  ```
  Removes a specified artist or song name from its respective hash table, and removes the node from the graph.
  A message should be printed whether it was deleted or not.
  
  If the first argument is `artist`, then it should remove the appropriate artist from the artist hash table.
  Otherwise, if the first argument is `song`, then remove from the song hash table.

  ```
  |ARTIST| is removed from the Artist database.
  |SONG| is removed from the Song database.
  ```

  ```
  FAILED -- |ARTIST| was not found in the Artist database.
  FAILED -- |SONG| was not found in the Song database.
  ```
</details>

- `remove {artist|song} {name}`
- First check if the artist/song is in its respective hash table.
- If it is not, print `FAILED -- |ARTIST/SONG| was not found in the Artist/Song database.`
- Otherwise, remove the item from the hash table and delete the graph node
- Print `|ARTIST/SONG| is removed from the Artist/Song database.`

<details>
  <summary><strong>Print</strong></summary>

  ```
  print {artist|song|graph}
  ```
  If the parameter is `artist` or `song`, print the artist/song hash table.
  The format should be the following:
  ```
  3: |Name 1|
  7: TOMBSTONE
  9: |Name 2|
  total songs: 2
  ```
  The numbers are the index that the artist/song was hashed to.

  If the parameter is `graph`, it should compute connected components on the graph.
  Do this using the Union/FIND algorithm depicted in OpenDSA Module 13.2.
  Print the number of connected components, and the number of nodes in the largest connected component.

  Then, compute and print the diameter for the largest connected component using Floyd's algorithm,
  depicted in OpenDSA Module 14.8.
  The format should be the following:
  ```
  There are # connected components
  The largest connected component has # elements
  The diameter of the largest component is #
  ```
  The diameter of the largest component is the longest path.
</details>

- `print {artist|song|graph}`
- If the parameter is `artist` or `song`...
  - Print out the respective hash table similar to project 1
  - See details above for formatting
- If the parameter is graph...
  - Find the connected components on the graph using the Union/FIND algorithm in OpenDSA Module 13.2
  - Find the diameter for the largest connected component using Floyd's algorithm in OpenDSA Module 14.8
  - See details above for formatting
 
## Programming Standards
<details>
  <summary><strong>Good Practice</strong></summary>
  
  if (reader instanceof Male) {  
  &emsp;Be a good boy programmer 😇  
  } else if (reader instanceof Female) {  
  &emsp;Be a good girl programmer 😇  
  }  

  WebCAT provides feedback on coding style.
</details>

- Include comments explaining the purpose of every variable or named constant in your program
- Use meaningful class/variable/method names
- Use camelCasing!
- Always make constant variables (`public static final int PI = 3.14159`) rather than literal constants
- Source files should be under 600 lines long
- Only one class per source file. Only exception is small inner classes (Such as a LinkedList's Node class)


<details>
  <summary><strong>Getting Help</strong></summary>
  
  Professors and TAs want to help you (except Cao),
  so make sure your code is readable before coming to a professor or TA.
  Write documentation from the start!
</details>

- Professors and TAs are helpful
- Make your code readable before coming to a professor or TA
- Write your docs as you write your program

<details>
  <summary><strong>Plagarism</strong></summary>
  
  Only use code you have written, so no ChatGPT!
  You may use code you have written for other CS classes, such as CS2114.
  You may also use code provided by the instructor.
  You can use OpenDSA code, but will require lots of modification.
</details>

- Only use your own code
- You may use code from prior classes or projects
- Code provided by instructors and OpenDSA are great starting points, and may be used

## Java Data Structures Classes
<details>
  <summary><strong>Details</strong></summary>
  
  You cannot use Java classes that implement complex data structures.
  This means no `ArrayList`, `HashMap`, `Vector`, or anything else that
  implements lists, hash tables, or extensible arrays.
  You can use standard arrays.
  You can use classes for string processing, byte array manipulation, parsing, etc.
  Ask a TA or professor if you aren't sure if you can use a certain Java class.
</details>

- Don't use the built-in Java `ArrayList`, `HashMap`, or `Vector`
- Don't use any built-in Java class that implements lists, hash tables, or extensible arrays
- Standard arrays (`Object[]`, `int[]`, etc.) are fine

## Deliverables
<details>
  <summary><strong>Details</strong></summary>
  
  Use the Eclipse WebCAT plugin for submission.
  Don't forget to add your partner's PID to the submission.
  You may only have 1 partner.
  Only one of you will submit the project.
  Be sure you have `@author` tags for each of you.
  e.g.
  ```java
  /**
   * This is a cool class my partner and I worked on
   * @author John Appleseed <john.appleseed@icloud.com>
   * @author Jane Doe <jane.doe@gmail.com>
   */
  ```
  You will be graded on your **final** submission.

  Also, don't make any other packages or store files in a directory tree.
  Everything should be flat under the `src` directory and in the default package.
  Using the starter project from the project files given in the Piazza post
  should already have this setup.
</details>

- Use Eclipse WebCAT plugin to submit
- Add your partner's PID to the submission
- You may only have 1 partner
- Only one of you should submit the project
- Ensure you have two `@author` tags in your JavaDocs
- You will be graded on your **final** submission
- Keep the directory structure flat and under `src` in the default package

## Pledge
Place this at the top of your main class:
```java
// On my honor:
//
// - I have not used source code obtained from another current or
//   former student, or any other unauthorized source, either
//   modified or unmodified.
//
// - All source code and documentation used in my program is
//   either my original work, or was derived by me from the
//   source code published in the textbook for this course.
//
// - I have not discussed coding details about this project with
//   anyone other than my partner (in the case of a joint
//   submission), instructor, ACM/UPE tutors or the TAs assigned
//   to this course. I understand that I may discuss the concepts
//   of this program with other students, and that another student
//   may help me debug my program so long as neither of us writes
//   anything during the discussion or modifies any computer file
//   during the discussion. I have violated neither the spirit nor
//   letter of this restriction.
```
If you don't, you get a zero :(

# Project Files
https://docs.google.com/document/d/1HRFXDswAfyYkjom0x0TR9LnM-7JbQZqAorXOlsEQ5P8/edit?usp=sharing

# Milestones
There are three milestones over the course of the project. There are no points explicitly allocated to milestones, but if you MISS 2 milestones, there is a 5 point penalty. If you MISS all three milestones, there is a 10 point penalty.

## Milestone 1: `TBD`
- Pass `TBD` Web-CAT instructor reference tests
- Have NONE of your own tests failed
- Have 75% mutation coverage

## Milestone 2: `TBD`
- Pass `TBD` Web-CAT instructor reference tests
- Have NONE of your own tests failed
- Have 75% mutation coverage
- Have at least 1 point for Style/Coding

## Milestone 3: `TBD`
- Pass `TBD` Web-CAT instructor reference tests.
- Have NONE of your own tests failed
- Have 90% mutation coverage
- Have at least 5 points for Style/Coding

## Final Due Date
Early Bonus Date (15 point bonus): Tuesday, December 5 at 11:00 AM  
Early Bonus Date (10 point bonus): Tuesday, December 5 at 11:00 PM  
Early Bonus Date (5 point bonus): Wednesday, December 6 at 11:00 AM  
Final Due Date: Wednesday, December 6 at 11:00 PM  
This project is worth 150 points (15% of the semester total).

# Reference Tests
No reference tests are known at this time.
When the list is found, this section will be updated.
