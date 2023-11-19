# Creating a Flowchart from a Dependency List
By: Tyler Watson and Brad Bowman

For this project, the goal is simple: given a list of courses (through a text file input) output a possible course sequence given a speficied about of credits a student wants to take per quarter and a specific quarter they wish to begin their studies. To implement a solution for this problem, we applied our knowledge of graph theory, algorithms, and third party libraries to create an efficient solution. Also, to help visualize the results of this computation, we implemented a frontend GUI which animates the sequence of courses a student should take. 

## Description
For this project, we first take in an input file of course information for a given major is given. Then, using Java, the file is read and the data is used to construct a directed graph (through the utilization of the GraphStream - a graph library). Using this graph, a topological ordering is performed in order to determine the proper order for the classes to be taken, based on Pre-requisites and what quarter the course is offered in. Two schedules are created: one with constraints and one without. The constrained schedule considers a minimum and maximum value for credits taken per quarter and the quarter that the student will begin taking classes. For the non-constrained schedule, there is no limit on credits taken per-quarter; essentially, all available courses for a given quarter are scheduled. It should also be noted that the non-constrained schedule begins during fall quarter by default. 

## Requirements
In order to run this program you will need a runtime enviroinment of at least Java 11. If you choose to run the full version of the project with the graphical (GUI) representation of the results, you will need to run the program in a JVM Environment (Java Virtual Machine) which has support for the X11 Graphics Environment (for instance, we recommend running it in a linux machine as portrayed in the full version demonstration video below).

You can find all libraries in the libs folder of this repository; however, to run the program you will only need to run the appropriate jar file (as discussed below in the user manual).

## User Manual

**Video demonstrations of the program execution:**

Text output and program execution demo: https://youtu.be/3l-OO7ZRI5U

Graph GUI and Animation Demo: https://youtu.be/dSvCYHazpdM

**The following are the steps to run the program:**
1. Clone the project from the GitHub Repository
    A. Note: If you wish to run the program with the GUI you will need to have a JVM Environment available (as described in the section titled 'Requirements').
3. Naviagate to the main directory of the project where the jar files are stored within your terminal.
![image](https://user-images.githubusercontent.com/59585662/159064992-7c0f247b-bae7-4f87-bedc-8c6db5cfce15.png)

4. Navigate to your terminal and input one of the following commands:
    A. To run the text only version (without the GUI): java -jar flowchart-textonly.jar
![image](https://user-images.githubusercontent.com/59585662/159065147-ebda3cd3-0c1e-4d08-bfda-b0f69dac51e1.png)

    B. To run the full version (with the GUI): java -jar flowchart-fullversion.jar
![image](https://user-images.githubusercontent.com/59585662/159065408-774bc9a1-0153-47f3-b7a8-2fbec6d9994d.png)

6. With this project, we have included two main test files which contain a list of classes for either a Computer Science BS (major1.txt) or Exercise Science BS (major2.txt)
![image](https://user-images.githubusercontent.com/59585662/157993971-f0643ef0-9afa-4c04-b897-09526ad46cd8.png)
 
7. Next, you will be asked to input the minimum and maximum amount of credits that you wish to take each quarter (note you will need to take at least 12 credits per quarter to be a fulltime student and there is a maximum of taking 22 credits per quarter).
![image](https://user-images.githubusercontent.com/59585662/157994158-5a638c03-48fa-453d-8911-57f29090dabf.png)

8. After inputting your credit selection, you will need to include which quarter you intend to begin taking your courses. For this selection you will need to input the numerical value that matches the quarter you wish to start. For the purpose of this example, we will choose to start the course sequence in the fall so we will input a '1'.
![image](https://user-images.githubusercontent.com/59585662/157994316-5fb46a25-36fd-4097-b1d1-dfeef027ccd6.png)

8. After inputting the given constraints (outlined in steps 7 & 8), you will recieve a text representation of the output in your terminal. The following image depicts a suggested course sequence for a Computer Science BS and it also includes some fun HHP electives as well.
![image](https://user-images.githubusercontent.com/59585662/157995846-6e95c494-c766-40a4-9675-6cbd4b518d8c.png)

9. If you are using the full version of the project with the GUI (and animation), after completeing step 8 you will also get a GUI popup which will look similar to the image below. As you will notice, the edges and nodes slowly animate the sequence of courses that you are recommended to take. Note that the red coloring stands represents the animation for the no constraints sequence and the neon green coloring standing for the constrained sequence of courses. For a demo of this make sure to checkout the GUI and animation video included above.
![image](https://user-images.githubusercontent.com/59585662/159094619-e1696108-c8d5-48c1-a6fc-aa9167899fc0.png)


## Reflection
We decided very quickly that a graph would be created given the course information, with any incoming edges representing pre-requisites for the course. It quickly became clear that any node of the graph with no incoming edges would be a course that could be taken. Then, it was a matter of receiving the data and properly constructing the graph and all edges. After the graph was created, it was necessary to determine the appropriate order of courses (or schedule) to be created from the given graph. Any course that had pre-requisites also had an incoming edge on the graph. To efficiently determine the proper order of courses, each node of the graph must be observed to check if it has any incoming edges. If it does not have any incoming edges, then the course is available to be scheduled. Using a modified version of a topological ordering of the graph is a great solution to this issue. For each node of the graph, the node is checked to see if there are any incoming edges. If not, the course is added to a list of available courses. After the list of available courses is created, a merge-sort algorithm (with a time complexity of O(nlogn)) is ran for the list by the numerical course code so classes are taken in sequence. For each available course found, the program checks if it can be added based on whether it is offered for the given quarter and the constraints of the schedule. If the course can be scheduled, it is added to a list representing the quarter’s schedule, and the node is removed from the graph. The runtime for an ordinary topological ordering is O(m + n)—where m is the number of vertices and n is the number of edges—but this program’s time complexity is slightly different due to the modifications. As the function loops through all the graph’s nodes (O(n)), a function to get the proper course is called. Since the list of courses is not scheduled, the runtime for this function is O(m) to iterate through the list. Next, merge-sort is called for the created list of available courses, with a runtime of O(mlog(m)). After this, another loop is run to iterate through the created list of available courses, which will have a runtime of O(m). The appropriate nodes are removed from the graph and then the function is continued recursively, ending when there are no more nodes remaining. Overall, the runtime of the function to schedule courses is O(nm + mlog(m) + m), which simplifies to O(nm + mlog(m)). 

Once it was realized that a topological ordering was a natural solution to scheduling courses with pre-requisites, implementing the non-constraint scheduler was straight forward. When implementing the constrained scheduler, however, things became a bit more difficult. Determining when courses should be added was not too difficult, but one thing that was overlooked was the fact that a node of the graph should only be removed when a class is scheduled. Luckily, the difficulty with solving the scheduling algorithm was mainly in the form of adjusting the topological ordering algorithm as opposed to having difficulty with the latter. There were also difficulties when incorporating the GraphStream library into our program. Using and learning the GraphStream documentation was straightforward, but there were issues to be solved when running the program. A primary motivating factor for this project was the fact that we felt confident in our ideas and understanding of the issue and respective solution, it more became a matter of properly implementing the code. 

 





