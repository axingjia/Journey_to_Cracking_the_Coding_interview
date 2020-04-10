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

page 49

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

(And the rest like Object-oriented and recursion and dynamic programming is not at the last end of the to-do queue here)


