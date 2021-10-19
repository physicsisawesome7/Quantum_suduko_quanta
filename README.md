# Quantum_suduko_quanta              Quantum Sudoku Solver


Grover’s algorithm

In an unstructured search problem, the best classical strategy is to check all the elements which eventually would result in O(N) evaluations, on average we could say it would require N/2 comparisons.
However, on a quantum machine, this type of problem can be solved with a small probability error in O(√N).
The general idea of Grover’s algorithm is to amplify the amplitude of solutions in order to make the system collapse to those values when it is measured. At first, we have to initialize the system to uniform distribution so as to have the same amplitude in every N state. This can be achieved by taking an n qubit register initialized in the state  |0〉⊗n   and applying a Hadamard gate over all qubits with the operator  H⊗n. The corresponding state is then a superposition.
If we measure the system, the probability of getting the correct answer would be 1/N. To increase the amplitude of a certain state, we have to design a diffuser. The global effect of the diffuser is “inversion about average”. In short, the oracle marks the solution states with a negative sign, encodes the information about the specific problem translating it into a phase shift over the system state, while the diffuser amplifies this marked state amplitude over all the others.

To summarize, the steps of the algorithm are:

Initialization
Application of H⊗n to first n qubit and HX to last ancilla.
The oracle inverts the amplitude of |x〉  that are solutions.
The diffuser amplifies those solution’s amplitudes by inverting them over the average of all the amplitudes.
Finally the measurement yields the solution, probability of solution is more than others.

Sudoku Solver
Sudoku is a combinatorial number-placement puzzle. Algorithms such as backtracking can solve most of 9 x 9 puzzles. However for large value of n, a combinatorial explosion occurs which limits the properties of Sudoku that can be constructed, analysed and solved.
There are 2 main aspects of problem difficulty:
First is complexity of individual steps involved in solving the problem 
Second is structure of dependency among individual steps, whether steps are independent  (can be applied in parallel) or they are dependent(must be applied sequentially).

We are implementing a sudoku solver using Grover’s algorithm for 2 x 2 sudoku.
A  2 x 2  sudoku is a grid of four blocks which contains either 1 or 0, where no two values in a row or column can be same.
                                         

First, of all we have to encode the problem into quantum representation using a quantum register of 4 qubit and assign to every cell a qubit in this register that we will call state register.
Next, we should configure the constraints on values obtained in row and column. We will use a Quantum Bit String Comparator (QBSC) to check if values on same row or column are different or not. Initially, the comparison between the first bit of each string is dominant, that is, if they are different, then the outputs will be outputs, O1 = a0 and O2 = b0. If they are equal the comparison between the second bit each string will be dominant and so on. Quite evidently, least significant bit does not have the dominion transfer that is realized with a Toffoli gate with 0 activation and two standard CCNOT.
The next step is construction of Oracle that is made up of four qbit-to-qbit comparators that check the logical constraints: A !=B, A!=C,C!=D and B!=D.
Using the Oracle we are able to mark the correct states and with the application of the Diffuser operator to amplify their amplitude. The second oracle application is used for uncomputing the ancillas, so that we can have a repeatable iteration of same gates over and over that does not need new states every time. The last ancilla is a key element to the Oracle functioning since it checks that all the states, amplified subsequently by diffuser are only the correct ones. Finally the measurement gives us a single possible solution to the problem, thus multiple measurements are needed in order to find all the possible solutions.
