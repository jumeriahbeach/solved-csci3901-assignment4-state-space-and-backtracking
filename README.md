Download Link: https://assignmentchef.com/product/solved-csci3901-assignment4-state-space-and-backtracking
<br>
Work with exploring state space and backtracking.

Background

Some games and puzzles like sudoku can be solved with a computer by systematically exploring all possible solutions, called the <em>state space</em>, until a correct solution is found. They do this by <em>backtracking </em>to a previous state whenever an incorrect solution is found, and then continuing from that point by exploring different possible solutions. In this assignment you will write a program to solve a mathematical puzzle which is closely related to sudoku by backtracking and state space exploration.

You will be solving a puzzle called <em>mathdoku</em><a href="#_ftn1" name="_ftnref1"><sup>[1]</sup></a> (see <a href="http://www.mathdoku.com/">http://www.mathdoku.com)</a>. Figure 1 shows a sample puzzle. In a mathdoku puzzle, you are given a square <em>n </em>× <em>n </em>grid. Within the grid, each cell is identified as part of some <strong>grouping</strong>. Each grouping is a <strong>connected </strong>set of 1 or more cells and each grouping is given

<ol>

 <li>a mathematical operator like + or <em>/</em>, and</li>

 <li>an integer which is the result of applying the operator to the cells.</li>

</ol>

The task is to put the integers 1 to <em>n </em>into the cells of the grid so that:

<ul>

 <li>No integer appears twice in any row</li>

 <li>No integer appears twice in any column</li>

 <li>Applying the operator to the values in a grouping gives the integer assigned to that grouping.</li>

</ul>

Figure 2 shows a solution to the puzzle in Figure 1.

The possible operators for a cell are + for addition, − for subtraction, ∗ for multiplication, <em>/ </em>for division, and = to specify that a grouping (of one cell) has a given value. For the − and <em>/ </em>operations, the groupings must have <strong>exactly 2 cells</strong>, and the <strong>larger value always comes first </strong>in the computation. For example, if the operation is <em>/ </em>and the values are 2 and 6, the result of that grouping would be

6<em>/</em>2 = 3<em>.</em>

The results of operations on groupings must always be integers, so 4<em>/</em>3 is <strong>not </strong>a valid assignment.

<table width="487">

 <tbody>

  <tr>

   <td width="38">4+</td>

   <td width="38">2<em>/</em></td>

   <td width="38">75×</td>

   <td width="38"> </td>

   <td width="38">2=</td>

   <td rowspan="5" width="109"> </td>

   <td width="38">4+1</td>

   <td width="38">2<em>/</em>4</td>

   <td width="38">75∗3</td>

   <td width="38">5</td>

   <td width="38">2=2</td>

  </tr>

  <tr>

   <td width="38"> </td>

   <td width="38"> </td>

   <td width="38"> </td>

   <td width="38">2×</td>

   <td width="38">4=</td>

   <td width="38">3</td>

   <td width="38">2</td>

   <td width="38">5</td>

   <td width="38">2∗1</td>

   <td width="38">4=4</td>

  </tr>

  <tr>

   <td width="38">5=</td>

   <td width="38">60×</td>

   <td width="38"> </td>

   <td width="38"> </td>

   <td width="38">1=</td>

   <td width="38">5=5</td>

   <td width="38">60∗3</td>

   <td width="38">4</td>

   <td width="38">2</td>

   <td width="38">1=1</td>

  </tr>

  <tr>

   <td width="38">8×</td>

   <td width="38"> </td>

   <td width="38">1−</td>

   <td width="38">1−</td>

   <td width="38"> </td>

   <td width="38">8∗2</td>

   <td width="38">5</td>

   <td width="38">1−1</td>

   <td width="38">1−4</td>

   <td width="38">3</td>

  </tr>

  <tr>

   <td width="38"> </td>

   <td width="38"> </td>

   <td width="38"> </td>

   <td width="38">8+</td>

   <td width="38"> </td>

   <td width="38"> </td>

   <td width="38"> </td>

   <td width="38"> </td>

   <td width="38">8+</td>

   <td width="38"> </td>

  </tr>

  <tr>

   <td width="38"> </td>

   <td width="38"> </td>

   <td width="38"> </td>

   <td width="38"> </td>

   <td width="38"> </td>

   <td width="109"> </td>

   <td width="38">4</td>

   <td width="38">1</td>

   <td width="38">2</td>

   <td width="38">3</td>

   <td width="38">5</td>

  </tr>

 </tbody>

</table>

Figure 1: A mathdoku puzzle.                              Figure 2: The solution to the puzzle in Figure 1.

<h2>Problem</h2>

Your task is to create a class called Mathdoku that accepts a puzzle and ultimately solves it. The class has at least 5 public methods with the following signatures:

public boolean loadPuzzle(BufferedReader stream) Read a puzzle in from a given stream of data. Further description on the puzzle input structure appears below. Return true if the puzzle is read successfully. Return false if some other error happened.

public boolean validate() Determine whether or not the loaded puzzle is valid. Return true if the puzzle is valid and can be solved, and return false otherwise. Further information on this method appears below.

public boolean solve() Do whatever you need to do to find a solution to the puzzle. The method should store the solution in the object so that it can be retrieved later. Return true if you solved the puzzle and false if you could not solve the puzzle. This method should also keep track of the number of <strong>guesses </strong>you make in the process of solving the puzzle, and then store this in the object to be retrieved later by the choices method below.

public String print() Return the current puzzle state as a String object.

public int choices() Return the number of guesses that your program had to make and later undo/backtrack while solving the puzzle.

You get to choose how you will represent the puzzle in your program and how you will proceed to solve the puzzle. The only constraint on your solution is that it should be somewhat <strong>efficient</strong>. In this case efficiency means that you explore <strong>as few solutions as possible </strong>before arriving at a correct one or discovering that there is no solution to the puzzle.

<h2>Inputs</h2>

The loadPuzzle method will accept a description of the puzzle as an input stream. The input stream will be formatted in three sections as follows:

<ul>

 <li>Section 1: Consists of an integer <em>n </em>which gives the size of the puzzle (an <em>n </em>× <em>n </em>grid)</li>

 <li>Section 2: Consists of the puzzle square itself. For an <em>n</em>×<em>n </em>puzzle square, the input will have <em>n </em>lines of input, each being a string of <em>n </em>characters (excluding the end of the line character). For one line, the <em>n </em>characters are the <em>n </em>cells in the line; each cell is a letter that represents the cell grouping to which the cell belongs.</li>

 <li>Section 3: Consists of the constraints for each cell grouping in the puzzle. There will be one line for each cell grouping. That line will have 3 values: the letter representing the grouping, the operation outcome for the grouping, and the operator for the grouping (one of +, -, *, /, or =). The values are separated from one another by <strong>at least </strong>one space.</li>

</ul>

<strong>Example:             </strong>The puzzle in Figure 1 would be represented as the following input. The constraints are given alphabetically by the grouping name here, but that’s not a guarantee in all input.

5 abccd abcef ghhei jhkll jjkmm a 4 + b 2 / c 75 * d 2 = e 2 * f 4 = g 5 = h 60 * i 1 = j 8 * k 1 l 1 m 8


<h2>Input validation</h2>

In many programs, such as compilers, there are multiple stages of input validation to check more complex properties of the input. The validate method performs additional input validation to check whether the input loaded by the loadPuzzle method specifies a valid mathdoku puzzle.

Some conditions you will need to check in your validate method (if you already check some of these in your loadPuzzle, that is fine) are:

<ul>

 <li>The cells form an <em>n </em>× <em>n </em>grid</li>

 <li>Every grouping is a <em>connected </em>set of cells — that is, there are no “gaps” between cells in a grouping.</li>

 <li>Every grouping has a value and an operator</li>

 <li>Every grouping with the = operator has exactly one cell</li>

 <li>Every grouping with the − or <em>/ </em>operator has exactly two cells</li>

 <li>Every grouping with the + or ∗ operator must have at least two cells</li>

</ul>

<h2>Outputs</h2>

The print method produces a String that represents the current state of the puzzle. The output provides each row of the current puzzle state; listed from top-to-bottom and rows separated by a carriage return (
) character. No space is printed between the columns of the rows. If the cell has a value from 1 to <em>n </em>assigned to it then print that value for the cell. Otherwise, print the grouping letter for the cell.

The following is the returned string for the puzzle in Figure 2, noting that 
 should be seen as just one character (newline character) in the text below:

14352
32514
53621
25143
41235


<strong>Assumptions</strong>

None.

<h2>Constraints</h2>

<ul>

 <li>The character for each cell grouping is <strong>case sensitive</strong>. So, a cell entered as a and another as A are two different groupings.</li>

 <li>You <strong>may </strong>use any data structures from the Java Collection Framework.</li>

 <li>You <strong>may not </strong>use an existing library that already solves this problem.</li>

 <li>If in doubt for testing, I will be running your program on cs.dal.ca. Correct operation of your program shouldn’t rely on any packages that aren’t available on that system.</li>

</ul>

<h2>Notes</h2>

<ul>

 <li>Develop a strategy on how you will solve the puzzle before you start coding your data structure(s) for the puzzle</li>

 <li>Work incrementally. First write the code to read in a puzzle. Test that. Next, write the code to print the current state of the puzzle. Test that. Then, write the code to validate the puzzle. Test that. Last, write your code to solve the puzzle.</li>

 <li>It might be helpful to start by <strong>brute-forcing </strong>the puzzle — specifically, try every possible number in each cell and check to see if it’s a solution. You probably won’t be able to solve large puzzles this way, so start with something small like a 4×4 grid. Check <a href="http://www.mathdoku.com/">http://www.mathdoku.com/</a> for sample puzzles.</li>

 <li>Once you have a working solution, think about ways in which you can make your solution more efficient. For example, if a grouping contains only one cell, do you need to try all possible values in that cell or can you simply <strong>infer </strong>the correct value for that cell? Note that there is no one solution to making your code efficient.</li>

 <li><strong>Do not change any of the given class names, method names or method signatures.</strong></li>

 <li><strong>Do not mark any of variables or methods in your class as static.</strong></li>

</ul>

<a href="#_ftnref1" name="_ftn1">[1]</a> These puzzles appear under the trade name of KenKen <a href="https://www.kenkenpuzzle.com/">(https://www.kenkenpuzzle.com)</a>.