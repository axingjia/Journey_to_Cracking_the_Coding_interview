# Journey to Cracking the Coding Interview

Again, it's a notetaking repo. 

If I have the book I can probably take notes on the book, but it wouldn't beat taking notes on a notebook.

And taking notes on github means I can check it anywhere, don't know the usefulness of this, but I can at least try.

# Chapter 6: Big O

#### Space Complexity

		int sum(int n){
			if (n<=0){
				return 0;
			}
			return n+sum(n-1);
		}

* this takes O(n) time and O(n) space because

	sum(4)
		->sum(3)
			->sum(2)
				->sum(1)
					->sum(0)
					
* However, just because you have n calls total doesn't mean it takes O(n) space. Consider the following:

int pairSumSequence(int n){
	int sum=0;
	for (int i=0;i<n;i++){
		sum+=pairSum(i,i+1);
	}
	return sum;
}
int pairSum(int a, int b){
	return a+b;
}

* There will be roughly O(n) calls to pairSum. However, those calls do not exist simultaneously on the call stack, so you only need *O(1)* space

Page 41

###### When do you add and when to multiply

		for (int a: arrA){
			print(a)
		}
		for (int b: arrB){
			print(b)
		}

* O(A+B)

		for (int a: arrA){
			for (int b: arrB){
				print(a+","+b);
			}
		}

* O(A*B)

###### Amortized Time 
* Dynamically resizing array 
* how do you describe the runtime of adding elements. This is a tricky question 
* You know when you add at N element, it take O(N) time, because you will have to create a new array of size 2N and then copy elements over. This adding will take O(N) time
* However, we also know that this doesn't happen very often. The vast majority of the time adding will be in O(1) time.
* We need a concept that takes both into account. This is what amortized time does. it allows us to describe that, yes, this worst case happens every once in a while. But once it happens, it won't happen again for so long that the cost is "amortized"
* In this case, what is the amortized time?
* As we adding elements, we double the capacity when the size of the array is a power of 2. So, after X elements, we double the capacity at array sizes 1,2,4,8,16...,X. That doubling takes, respectively, 1,2,4,8,16,32,64...,X copies.
* What is the sume of 1+2+4+8+16...+X. If you read right to left, it starts with X and halves until it gets to 1.
* What then is the sume of X+X/2+X/4+X/8...+1. This is roughly 2X
* Therefore, X adds take O(X) time. The amortized time for each adding is O(1)

###### Log N Runtimes
* Example: binary search
* 2^4=16 ->log(2)16=4
* log(2)N=k -> 2^k=N

###### Recursive Runtimes

	int f(int n){
		if (n<=1){
			return 1;
		}
		return f(n-1)+f(n-1);
	}
	
* A lot of people will, for some reason, see the two calls to f and jump to O(N^2). This is completely incorrect.
* Rather than making assumptions, let's derive the runtime by walking through the code. Suppose we call f(4). This call f(3) twice. Each of those calls to f(3) calls f(2), until we get down to f(1)
* The tree will have depth N. Each node has two children. Therefore, each level will have twice as many calls as the one above it. The number of node on each level is:

N |  Level  | # Node  | also expressed as | Or
4 | 0 | 1 | |2^0
3 | 1 | 2 | 2*previous level=2 |2^1
2 | 2 | 4 | | 2^2
1 | 3 | 8 | | 2^3

* More generically, this can be pexressed as 260+2^1+2^2+2^3+2^4...+2^(N-1)=2^N-1
* See sum of powers of 2 at [All the math]
* Try to remember this pattern. When you have a recursive function that makes multiple calls, the runtime will often (but not always) look like O(branches^depth), where branches is the number of times each recursive call branches. In this case, this gives us O(2^N)
* The space complexity will be O(N)

#### Example 1
SKIP: general 2 for loop takes O(N)

#### Example 2
SKIP: General one loop inside another, takes O(N^2)

#### Example 3
* this is very similar code to the above example, but the inner for loop starts at i+1

		void printUnorderPairs(int[] array){
			for (int i=0;I,array.length;i++){
				for (int j=i+1;j<array.length;j++){
					system.out.println(array[i]+","+array[j]);
				}
			}
		}

* *This pattern of for loop is very common. It is important that you know the runtime and that you deeply understand it. You can't reply on just memorizing common runtimes. Dep comprehension is important.*
* The first time through runs for N-1 steps. The second time, its N-2 steps. Then N-3 steps. And so on
* (N-1)+(N-2)+(N-3)+(N-4),,,+2+1
* = 1+2+3+...+N-1
* = Sum of 1 through N-1
* = N*(N-1)/2-N=(N*(N-1)-2N)/2=(N*(N-3))/2=O(N^2)

#### Example 4
* SKIP, just two array with different length, one loop inside the other, O(ab)

#### Example 5
* SKIP, just 3 loop, with the third loop from 1 to 10000. still O(ab)

#### Example 6 

void reverse(int[] array){
	for (int i=0; i<array.length/2;i++){
		int other=array.length-i-1;
		int temp=array[i];
		int[i]=array[other];
		array[other]=temp;
	}
}
* Is O(N)

#### Example 7
* SKIP, just dropping trailing big O

#### Example 8
* sort string, and sort the array of string 
* MY:  slogs+nlogn=N^2*2logN
* Incorrect 
* assume s be the length of the longest string, and a be the length of the array
* sort each string is O(sLogs)
* do this for each string, so O(a*sLogs)
* Now we have to sort all the strings. There are a string, so you may be inclined to say that this takes O(aloga). This is what most candiate would say. You should also take into account that you need to compare the strings. Each string comparison takes O(s) time. There are O(aLoga) comparisons, therefore this will take O(s*aLoga)
* So, in conclusion, O(asLogs+asLoga)

#### Example 9
* This is a balanced binary search tree 

	int sum(Node node){
		if (node==null){
			return 0;
		}
		return sum(node.left)+node.value+sum(node.right)
	}
	
* Just because it's a binary search tree doesn't mean that there is a log in it 
* The most straightforward way is to think about what this means. This code touches each node in the tree once and does a constant time amount of work with each "touch"
* Therefore, the runtime will be linear in terms of number of nodes. So O(N)
* Recursive pattern 
* On page 44, we discuss a pattern for the runtime of recursive functions that have multiple branches. There are two branches at each call, so we're looking at O(2^depth)
* At this point many people might assume that something went wrong since we have an exponential algorithm--that something in our logic is flawed or that we've inadventently created an exponential time algorithm (yikes!)
* The second statement is correct. We do have an exponential time algorithm, but it's not as bad as one might think.
* What is depth. The tree is a balanced binary search tree. Therefore, if there are N total nodes, then dept is roughly logN.
By equation above, we get O(2^(logN))
* recall that 2^p=Q  ->  log(2)Q=P
* What is 2^(logN)???
* let P=2^(logN). By definition, we can write this as Log(2)P=Log(2)N. This means that P=N
* Let P=2^(logN)
* log(2)P=Log(2)N
* P=N
* 2^(logN)=N
* Therefore, it is O(N)

#### Example 10

	boolean isPrime(int n){
		for (int x=2;x*x<=n;x++){
			if(n%x==0){
				return false;
			}
		}
		return true
	}

* result is O(sqrt(N)), easy

#### Example 11

	int factorial(int n){
		if (n<0){
			return -1;
		}else if (n==0){
			return 1;
		}else {
			return n*factorial (n-1);
		}
	}
	
* This is a straight recursion from n to n-1 to n-2 down to 1. it will take O(n) time

#### Example 12 

	void permutation(String str){
		permutation(str,"");
	}
	void permutation(String str, String prefix){
		if (str.length()==0){
			System.out.println(prefix);
		}else{
			for (int i=0;i<str.length();i++){
				String rem=str.substring(0,i)+str.substring(i+1);
				permutation(rem,prefix+str.charAt(i))
			}
		}
	}

* MY: This algorithm finds out how many permutation there is
* This is a (very) tricky one. We can think about this by looking at how long each call takes (excluding the recursion piece) and how many times permutation gets called. We'll aim for getting as tight of an upper bound as possible
* Executing line 7 (System.out.println(prefix);)takes O(n) time since each character needs to be printed
* line 10 and line 11 (String rem=str.substring(0,i)+str.substring(i+1);permutation(rem,prefix+str.charAt(i)))
 will also take O(n) time combined, due to the string concatenation. Observe that the sum of the lengths of rem, prefix, and str.charAt(i) will always be n
* Each node in our call tree therefore corresponds to O(n) work
* How many times does the function get called?
* How many leafs nodes are there. This is simply the number of permutation. We branch 4 times at the root, then 3, then 2, then 1. This gives us 4*3*2*1 leaf nodes. Or, expressed more generically, n! permutation.
* How many total nodes are there? Each leaf node is attached to a path with n nodes. So at most, there are n*n! total nodes in the tree.
* The total runtime therefore is (at worst) O(n*n*n!). 
* Advanced: O(e*n!), SKIM

#### Example 13

	int fib(int n){
		if (n<=0) return 0;
		else if (n==1) return 1;
		return fib(n-1)+fib(n-2)
	}
	
* We can use the earlier pattern we'd established for recursive call: O(branch^depth)
* There are 2 branches per call, and we go as deep as N, therefore the runtime is O(2^N).
* MY: What??
fib(4)    
	->fib(3)+fib(2)
	->fib(2)+fib(1)+(fib(1)+0)
	->fib(1)+fib(0)+0
	->1+0+0+1+0
	
* Refer to [Recursive Runtime], O(2^0+2^1+2^2+2^3)=O(2^n-1)

#### Example 14
	void allFib(int n){
		for( int i=0;i<n;i++){
			System.out.println(i+":"+fib(i));
		}
	}
	
	int fib(int n){
		if (n<=0) return 0;
		else if (n==1) return 1;
		return fib(n-1)+fib(n-2);
	}
* many people rush to O(n*2^n)
* Not so fast. The error is that the n is changing. yes fib(n) tkaes O(2^n), but it matters what the value of n is.

	fib(1)->2^1 steps
	fib(2)-> 2^2 steps
	fib(3)-> 2^3 steps
	fib(4)-> 2^4 steps
	...
	fib(n)->2^n steps
* therefore, 2^1+2^2+2^3+2^4+...+2^n
* based on what we showed on page 45, this is 2^(n+1)-2. Therefore, the runtime to compute the first n fibonacci numbers is still O(2^n)

#### Example 15
* The following code prints all Fibonacci numbers from 0 to n. However, this time, it stores(ie. caches) previously computed values in an integer array. If it has already been computed, it just returns the caches. What is its runtime?

	void allFib(int n){
		int[] memo=new int[n+1];
		for (int i=0; i<n;i++){
			System.out.println(i+": "+fib(i,memo));
		}
	}
	
	int fib(int n, int[] memo){
		int (n<=0) return 0;
		else if(n==1) return 1;
		else if (memo[n]>0) return memo[n];
		memo[n]=fib(n-1,memo)+fib(n-2,memo);
		return memo[n];
	}
	
	fib(0)-> return 0
	fib(1) -> return 1
	fib(2)
		fib(1) ->return 1
		fib(0) -> return 0
		store 1 at memo[2]
	fib(3)
		fib(2) ->lookup memo[2]->return 1
		fib(1) ->return 1
		store 2 at memo[3]
	fib(4)
		fib(3)->lookup memo[3]->return 2
		fib(2)->lookup memo[2] ->return 1
		store 3 at memo[4]
	fib(5)
		fib(4)->lookup memo[4]->return 3
		fib(3)->lookup memo[3]->return 2
		store 5 at memo[5]
	...

* At each call to fib(i), we have alrady computed and stored the values for fib(i-1) and fib(i-2). We just look up those values, sum them, store the new result, and return. This takes a constant amount of time.
* We are doing a constant amount of work n times, so this is O(N) time
* *This technique, called memorization, is a very common one to optimize exponential time recursive algorithm*

#### Example 16

	int powerOf2(int n){
		if (n<1){
			return 0;
		}else if(n==1){
			System.out.println(1);
			return 1;
		}else{
			int prev=powerOf2(n/2);
			int curr=prev*2;
			System.out.println(curr);
			return curr;
		}
	}

	MY: power(4)
	->power(2)
	->power(1)
	->return
	so it is O(logN) correct!

* Rate of increase: SKIP

## Additional problem
* VI.1: MY: O(b) CORRECT
* VI.2: MY: 

	power(4,3)
	-> a* power(a,2)
	-> a* a* power(a,1)
	-> a*a*a*power(a,0)
	O(b)  CORRECT 
* VI.3: MY: O(1). [CORRECT], but the code is interesting. Record:

	int mod(int a, int b){
		if(b<=0){
			return -1;
		}
		int div=a/b;
		return a-div*b;
	}
	
* VI.4: O(a/b)  CORRECT
* VI.5: SKIP


### chapter 7: Technical Question
* Don't read question and answer, try to do it
* Advanced: write the code on paper.
* Test your code 
* Type your paper code as-is into a computer
* try to do as many mock interviews as possible

As for Data Structure, you are usually only expected to basics. Here are teh absolute, must-have knowledge:

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

* Make sure you know how to use them and implement them
* Practice implementing the data structures (on paper, then on computer)
* In particular, hash tables are an extremely important topic.
* **A good way to find things quickly is using a hashtable**

### Step 1: Listen carefully
* make sure you hear the problem correctly
* You do want to ask questions about anything you're unsure about.
* For example, "Given two array that are sorted, find"... you probably need to know that the data is sorted. The optimal algorithm for the sorted situation is probably different than the optimal algorithm for the unsorted situation.
* Example 2, "Design an algorithm to be run repeatedly on a server that". The server/to-be-run-repeatedly situation is different from the run-once situation. Perhaps this means that you *cache data*? Or perhaps it justifies some reasonable precomputation on the initial dataset?
* many candidates will hear the problem correctly. But ten minutes into developing an algorithm, some of the key details of the problem have been forgotten. Now they are in a situation where they actually can't solve the problem optimally.
* You might even find it useful to write the pertinent information on the whiteboard

### Draw an Example
* An example can dramatically improve your ability to solve an interview question, and yet so many candidates just try to solve the questino in their head 
* For example, when drawing an binary search tree, don't just try 2 levels, and don't leave the value empty. And don't try a perfect tree(the last level is all filled up)

### State a brute force algorithm 
* State a brute force. Some candidate don't state the brute force because they think it's both obvious and terrible. But state it anyways, it might be obvious for you, but it is not necessarily obvious for all candidates. You don't want your interviewer to think that you're struggling to see even the easy solution

### 4. Optimize 
* Once you have a brute force algorithm, you should work on optimizing it. A few techniques that work well are: 
* 1. Look for unused information. Did you interviewer tell you that the array was sorted? How can you leverage that information 
* 2. Use a fresh example. Sometimes, just seeing a different example will unclog your mind or help you see a pattern in the problem
* 3. Solve it "incoorectly". SKIP
* 4. Make time vs space tradeoff.
* 5. Precompute information
* 6. Use a hash table.
* Think about the best conceivable runtime.

### 5. Walk Through 
* Test and fix bugs 

### 6. Implement 
* Write them down. 
* Modularized code
* error checks
* use other classes/structs where appropriate.
* Good variable names 

### 7. Test 
* Start with a "conceptual" test
* weird looking code like x=length-2
* hot spot. Ex: base case in recursive code, integer division, null node in binary tree, the start and end of iteratino through a linked list
* small test cases
* special cases. Test your code against null or single element values, the extreme cases, and other speical cases.

### Optimize & Solve Technique #1: Look for BUD 
* Bottleneck, unnecessary work, duplicate work
* if we sort an array, then find element will be O(logN)

##### Unnecessary Work
* from: 
	
	n=1000
	for a from 1 to n 
		for b from 1 to n
			for c from 1 to n
				for d from 1 to n
					if a^3+a^3==c^3+d^3
						print a,b,c,d

* To:
	
	n=1000
	for a from 1 to n 
		for b from 1 to n
			for c from 1 to n
				d=pow(a^3+b^3-c^3,1/3);//will rount to int
				if a^3+a^3==c^3+d^3 && 0<d && d<=n
					print a,b,c,d
	
* This goes from O(n^4) to O(N^3)

#### Duplicate Work 
* Why do we keep on computing all (c,d) for each (a,b) pair? We should just create the list of (c,d) pair once 

	n=1000
	for c from 1 to n
		for d from 1 to n
			result = c^3+d^3
			append (c,d) to list at value map[result]
			
	for each result, list in map 
		for each pair1 in list 
			for each pair2 in list 
				print pair1, pair2

#### Optimize & Solve Techniques #2: DIY
* SKIP


page 70

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








