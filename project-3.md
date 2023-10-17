# Project 3 - Sorting
Notes on spec as of 2023-10-16  
Updated **Java Data Structures Classes** on 2023-10-17  
Updated **Design Considerations** on 2023-10-17  
Added **Milestones** on 2023-10-17  
Added **Reference Tests** on 2023-10-17  

## Assignment
<details>
  <summary><strong>Paragraph 1</strong></summary>
  
  This project sorts a file. Yes, a file.
  The input data file is several 4-byte records.
  Each record is two `short` values in the range 1 - 30,000.
  The first `short` is the key used for sorting.
  The second `short` is a data value.
  The input file will always be a multiple of 4096 bytes.
  All I/O ops will be done on 4096 byte blocks.
  This means 1024 records in total per file.
  **Data files are _binary_, not text!**
</details>

- Sorting **binary** files, length is multiple of 4096
- Records are 4 bytes long
- Records consist of two short values
  - First value is key to sort
  - Second value is a data value
- I/O operations are to be done on 4096 byte blocks

<details>
  <summary><strong>Paragraph 2</strong></summary>
  
  Sort the file in ascending (low-to-high) order using a modified Quicksort.
  The modification is the interaction between Quicksort and the file.
  The array being sorted will be the file itself, rather than an array.
  Accesses to the file will be mediated by a buffer pool.
  The buffer pool stores 4096-byte blocks, totalling 1024 records.
  The buffer pool is organized using the Last Recently Used (LRU) replacement scheme.
  OpenDSA Module 9.4 has further details.
</details>

- Quicksort the file in ascending order
- A buffer pool is used to access the file
- The buffer pool stores 4096 bytes, or 1024 records
- The buffer pool is organized by Last Recently Used
- Read up on OpenDSA Module 9.4 for buffer pool details

<details>
  <summary><strong>Paragraph 3</strong></summary>
  
  This is not an external sorting algorithm. (See OpenDSA Module 9.6)
  This is instead a Quicksort on "virtual memory" in the form of a large array on disk.
  The biggest modification to Quicksort will be changing in-memory array accesses to
  disk file accesses, going through the buffer pool.
</details>

- Read up on OpenDSA Module 9.6
- This Quicksort is done directly on disk, rather than in memory
- The buffer pool acts as the middle-man between Quicksort and file access

## Design Considerations
<details>
  <summary><strong>Paragraph 1</strong></summary>
  
  The biggest concern is the interaction between Quicksort and the file itself.
  Pay careful attention to the buffer pool,
  this will be re-used in Project 4.
  The disk file is effectively the array.
  Your buffer pool will use "message passing" shown in OpenDSA Module 9.4.1.3.
  The buffer pool should pass byte arrays back and forth, not "records".
  Quicksort will be sorting records, not byte arrays.
</details>

- Buffer pool is priority 1, especially since it will be used again in Project 4
- Think of the file as a byte array
- The buffer pool will use the "message passing" method in OpenDSA Module 9.4.1.3
- The buffer pool should be thinking about byte arrays
- The Quicksort should be thinking about records

<details>
  <summary><strong>Paragraph 2</strong></summary>
  
  Performance will be a grade-killer!
  Test cases will be timed, so efficiency is important here.
  If you make a bad buffer pool or 
  do a lot of byte copying in your Quicksort implementation, you will suffer.

  Note that WebCAT is faster than our computers.
  It will tell you how long your method took if it takes too long.
  You can use this information and then time the method yourself
  to get an idea of the time ratios between your computer and WebCAT's.
  [Source](https://piazza.com/class/lldsd37jppe6qs/post/773)
</details>

- **Performance is important for this project!**
- Test cases are timed
- Your computer is slower than WebCAT's. If you're failing runtime tests, compare your runtime to WebCAT's to get an idea of how much you actually need to decrease the time. [Source](https://piazza.com/class/lldsd37jppe6qs/post/773)
- Don't do a lot of byte copying in your Quicksort

## Invocation and I/O Files
<details>
  <summary><strong>Paragraph 1</strong></summary>
  
  The program is invoked like so:
  ```
  java Quicksort <data-file-name> <buffer-count> <statistic-file-name>
  ```
  `<data-file-name>` is the binary file to be sorted.
  _The input data file will be modified!_
  So keep a copy of the original file when testing.
  Maybe copy the file in-code when testing,
  but remove that part when submitting.

  `<buffer-count>` is the number of buffers for the buffer pool.
  This will be in the inclusive range 1 - 20.

  `<statistic-file-name>` is the name of a file
  that your program will make to store runtime statistics.
  It should **NOT** overwrite the statistic file,
  but instead append new statistics to the file.

  Information to write will be in paragraph 3 summary.
</details>

- Program is invoked with three arguments:
- `<data-file-name>`
  - The name of the binary file to be sorted
  - _This file will be modified!_
  - Keep a copy of the original when testing
- `<buffer-count>`
 - Number of buffers for the buffer pool
 - 1 <= `<buffer-count>` <= 20
- `<statistic-file-name>`
  - The name of the statistic file
  - This file is used to store runtime stats
  - **APPEND** to this file, do **NOT** overwrite it!

<details>
<summary><strong>Paragraph 2</strong></summary>
  
  After the program exits, your data file should be in a sorted state.
  Don't forget to flush buffers from your buffer pool,
  or the file won't be updated.
</details>

- The data file will be sorted after the program runs
- Flush all buffers in your buffer pool to ensure the file gets updated

<details>
  <summary><strong>Paragraph 3</strong></summary>
  
  Write statistics to the `<statistic-file-name>` parameter.
  _Do not overwrite the statistic file!_
  Instead, **append** to it new statistics.
</details>

- _Do not overwrite the statistic file!_ Append to it instead.
- Statistics:
 - The name of the data file being sorted
 - The number of times your program found the data it needed in a buffer and did not have to go to disk
 - The number of times your program had to read a block of data from disk into a buffer
 - The number of times your program had to write a block of data from a buffer to disk
 - The time your program took to execute the Quicksort.
   - Use `System.currentTimeMillis()` at the beginning and end of the Quicksort
   - Take the difference of these two to get the run time of your Quicksort
   - This difference is in milliseconds
   - This should only be the time to sort, not the time to print out.

## Programming Standards

<details>
  <summary><strong>Paragraph 1</strong></summary>
  
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
  <summary><strong>Paragraph 2</strong></summary>
  
  Professors and TAs want to help you (except Cao),
  so make sure your code is readable before coming to a professor or TA.
  Write documentation from the start!
</details>

- Professors and TAs are helpful
- Make your code readable before coming to a professor or TA
- Write your docs as you write your program

<details>
  <summary><strong>Paragraph 3</strong></summary>
  
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
- Built-in classes for string processing, byte arrays, parsing, are fine
  - `StringBuilder`, `Scanner`
- You may use the built-in `ByteBuffer` class. [Source](https://piazza.com/class/lldsd37jppe6qs/post/lnqb34g7cru5e6)

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

# Milestones
There are three milestones over the course of the project. There are no points explicitly allocated to milestones, but if you MISS 2 milestones, there is a 5 point penalty. If you MISS all three milestones, there is a 10 point penalty.

## Milestone 1: Friday, 10/20
- Pass 2 Web-CAT instructor reference tests
- Have NONE of your own tests failed

## Milestone 2: Wednesday, 10/25
- Pass 3 Web-CAT instructor reference tests
- Have NONE of your own tests failed
- Have at least 1 point for Style/Coding

## Milestone 3: Monday, 10/30
- Pass 5 Web-CAT instructor reference tests.
- Have NONE of your own tests failed
- Have at least 5 points for Style/Coding

# Reference Tests
All Reference tests, courtesy of lofiwifi from the 3114 Discord:
![reference tests](https://cdn.discordapp.com/attachments/1103131079965163524/1163891438640189440/image.png?ex=6541395f&is=652ec45f&hm=4a0d55af89369374513bd023c2afec13a3b8f629542004a108a567d771595e0c&)
