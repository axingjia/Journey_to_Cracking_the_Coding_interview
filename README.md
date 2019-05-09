# Journey to Cracking the Coding Interview

Again, it's a notetaking repo. 

If I have the book I can probably take notes on the book, but it wouldn't beat taking notes on a notebook.

And taking notes on github means I can check it anywhere, don't know the usefulness of this, but I can at least try.


### chapter 7: Technical Question
* Don't read question and answer, try to do it
* Advanced: write the code on paper.
* Test your code 
* Type your apper code as-is into a computer
* try to do as many mock interviews as possible

As for Data Structure, you are usually only expected to basics. Here are teh absolute, must have knowledge:

* Linked Lists
* Tree, Tries, Graphs
* Stacks, Queues
* Heaps
* Vector/ ArrayLists
* Hash Tables
* Breath-First Search
* Depth First Search
* Binary Search
* Merge Sort
* Quick Sort 
* Bit Manipulation 
* Memory (Stack vs. Heap)
* Recursion
* Dynamic Programming
* Big O TIme & space

Make sure you know how to use them and implement them

Practice implementing the data structures (on paper, then on computer)

In particular, hash tables are an extremely important topic.

**A good way to find things quickly is using a hashtable**

## Chapter 11 Interview Questions

go to www.CrackingTheCodingInterview.com to download the complete solution.

* StringBuilder avoid the problem of iterative string concatanation

(A bit of digression here. Need to dive deeper into linkedList and other data structure. By just reading the code isn't not getting into my brain.I think the reason why it's not getting into me is I don't fully understand the steps, even though I read it and understand it. Well, I just need to implement it with psuedo code myself and then check it. Probably will give it half an hour for this purpose)

(Also need to watch videos on Bit Manipulation, need to watch some videos about it)

(And of course need to figure out Tree and Graph, go through textbook to figure it out)

(And the rest like Object-oriented and recursion and dynamic programming is at the last end of the to-do queue here)

(Also, need to wait for the book to deliver to me)

## recursion
I checked the chapter but it's not really informative. So I check geekForGeek and [here](https://www.geeksforgeeks.org/recursion/) it is.

Certain problems can be solved quite easily, such as [Towers of Hanoi(TOH)](https://www.geeksforgeeks.org/c-program-for-tower-of-hanoi/),[inorder/Preorder/Postorder Tree Traversals](https://www.geeksforgeeks.org/tree-traversals-inorder-preorder-and-postorder/), [DFS of Graph](https://www.geeksforgeeks.org/depth-first-search-or-dfs-for-a-graph/)

**What is base condition in recursion?**    
In the recursive program, the solution to the base case is provided and the solution of the bigger problem is expressed in terms of smaller problems 

        int fact(int n)
        {
            if (n < = 1) // base case
                return 1;
            else    
                return n*fact(n-1);    
        }
        
In the above example, base case for n < = 1 is defined and larger value of number can be solved by converting to smaller one till base case is reached.

**How a particular problem is solved using recursion?**
1. represent a problem in terns of one or more smaller problems
2. add one or more base conditions that stop the recursion 
3. ex: when compute factorial n if we know factorial of (n-1). the base case for factorial would be n=0. We return 1 when n=0 

**Why Stack Overflow error occurs in recursion?**
when the base case is never reached or not defined     
for example, if (n==100) but n can never become 100 

**What is difference between tailed and non-tailed recursion?**    
A recursive function is tail recursive when recursive call is the last thing executed by the function.

        // A Java program to demonstrate working of 
        // recursion 
        class GFG{ 
        static void printFun(int test) 
        { 
            if (test < 1) 
                return; 
            else
            { 
                System.out.printf("%d ",test); 
                printFun(test-1); // statement 2 
                System.out.printf("%d ",test); 
                return; 
            } 
        } 
          
        public static void main(String[] args) 
        { 
            int test = 3; 
            printFun(test); 
        } 
        } 
          
        // This code is contributed by  
        // Smitha Dinesh Semwal 

Output :    
3 2 1 1 2 3

A different way to view it
![recursion](/images/recursion.jpg)

**Every recursive program can be written iteratively and vice versa is also true**

**Advantage: recursion provides a clean and simple way to write code. Some problems are inherently recurisve like tree traversals. We can write such codes also iteratively with the help of a stack data structure. For example refer [inorder tree traversal without recursion](https://www.geeksforgeeks.org/inorder-tree-traversal-without-recursion/), [iterative Tower of Hanoi](https://www.geeksforgeeks.org/iterative-tower-of-hanoi/)

## Practice Questions for Recursion | Set 1

        int fun1(int x, int y)  
        { 
          if(x == 0) 
            return y; 
          else
            return fun1(x - 1,  x + y); 
        } 

The function fun() calculates and returns ((1 + 2 … + x-1 + x) +y) which is x(x+1)/2 + y. For example if x is 5 and y is 2, then fun should return 15 + 2 = 17.

MY: 
fun1(9,9)
fun1(8,18)
fun1(7,25)
fun(1,?)
fun1(0) return 1+?

        void fun2(int arr[], int start_index, int end_index) 
        { 
          if(start_index >= end_index)    
             return; 
          int min_index;  
          int temp;  
          
          /* Assume that minIndex() returns index of minimum value in  
            array arr[start_index...end_index] */
          min_index = minIndex(arr, start_index, end_index); 
          
          temp = arr[start_index]; 
          arr[start_index] = arr[min_index]; 
          arr[min_index] = temp;         
          
          fun2(arr, start_index + 1, end_index); 
        } 
        
Answer: The function fun2() is a recursive implementation of Selection Sort.

MY: Will get back to it a bit later

## Practice Questions for Recursion | Set 2

    /* Assume that n is greater than or equal to 1 */
    int fun1(int n) 
    { 
      if(n == 1) 
         return 0; 
      else
         return 1 + fun1(n/2); 
    }  
    
Answer: The function calculates and returns floor(log2(n)). For example, if n is between 8 and 15 then fun1() returns 3. If n is between 16 to 31 then fun1() returns 4.

MY: (I did the staircase) fun1(4)=1+fun1(2)=1+1+fun(1)=1+1+0=2

Question 2  

    /* Assume that n is greater than or equal to 0 */
    void fun2(int n) 
    { 
      if(n == 0) 
        return; 
      
      fun2(n/2); 
      printf("%d", n%2); 
    }   
    
MY: I did the staircase, n==0 should be n<=1; fun2(8) print out 100;

MY: Okay, reading and intepretating seems to be no problem, but writing one that is a lot of problem. Where can I practice it?

Okay I find this link. This is perfect. [Link](https://www.geeksforgeeks.org/recursion-practice-problems-solutions/)

### Recursive Functions 
SKIM, nothing useful

### Tail Recursion 
Nothing useful

Dive straight to the problem: [recursive palindrome](https://www.youtube.com/watch?v=7uu8WfDdsRc)

[recursion countdown](https://www.youtube.com/watch?v=FCGF7YB6g-4&list=PLGSxjw5t3yJs4i3RONXvShSnXflLp5fXS&index=2)

Sometimes geekforgeek's question is too hard and unrelated, so I just do a loop from ctci,leetcode->DS->geekforgeek->ctci,leetcode; thats why I need all three to be able to study it effectively, geekforgeek should always be supplimentary








