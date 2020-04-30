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

#### Optimize & Solve Technique #3: Simplify and Generalize
* SKIP

#### Optmize & Solve Technique #4: Base Case and Build
* With Base Case and Build, we solve the problem first for a base case and then try to build up from there. When we get to more complex/interesting cases, we try to build those using the prior solutions.
* Example: Design an algorithm to print all permutations of a string. For simplicity, assume all characters are unique.
* consider a test string abcdefg

		case "a" -> {"a"}
		case "ab" ->{"ab","ba"}
		case "abc" -> ?

		P("abc") =insert "c" into all location of all strings in P("ab")
		P("abc") = insert "c" into all locations of all strings in {"ab","ba"}
		P("abc") ...

* Now that we understand the pattern, we can develop a general recursive algorithm. We generate all permutations of string s(1)...s(n) by "chopping off" the last character and generating all permutations of s(1)...s(n-1). Once we have the list of all permutations of S(1)...s(n-1), we iterate through this list. For each string in it, we insert Sn of the string.

#### Optimize & Solve Technique #5: Data Structure Brainstorm
* This approach is certainly hacky, but it often works. We can simply run through a list of data structures and try to apply one. This approach is useful because solving a problem may be trivial once it occurs to use to use, say, a tree
* Example: numbers are randomly generated and stored into an expanding array. how would you keep track of the median.
* Our data structure brainstorm might look like the following:
* Linked list? probably not. Linked lists tend not to do very well with accessing and sorting numbers
* Array? Maybe, but already have an array. Could you somehow keep the elements sorted? That's probably expensive.
* Binary tree? This is possible, since binary trees do fairly well with ordering. in fact, if the binary search tree is perfectly balance, the top might be the median. But, be careful--if there's an even number of elements, the median is actually the average of the middle two elements. The middle two elements can't both be at the top. This is probably a workable algorithm, but let's come back to it
* Heap. A heap is really good at basic ordering and keep track of max and min. This is actually interesting--if you have two heaps, you could keep track of the bigger half and the smaller half the elements. The bigger half is kept in a min heap, such that the smallest element in the bigger half is at the root. The smaller half is kept in a max help, such the the biggest element of the smaller half is at the root. Now, with these data structure, you have the potential median elements at the roots. If the heaps are not longer the same size, you can quickly "rebalance" the heap by popping an element off the one heap and push it onto the other.

### Best Conceivable Runtime
* One way BCR can be useful: we can use the runtimes to get a "hint" for what we need to reduce.
* Example: Find common numbers in two arrays
* BCR is 2N. So the best conceivable runtimes is O(N)
* Any precomputation that's O(N) or less is "free"
* In this case, we can just throw everything in B into a hash table. This will take O(N) time. Then we just go through A and look up each element in the hash table. This loop up (or search) is O(1), so our runtime is O(N)
* Suppose our interviewer hits us with a question that makes us cringe: Can we do better
* No, not in terms of runtime. We have achiebved the fastest possible runtime, therefore we can not optimize the big O time. We could potentially optimiz the space complexity.
* *This is another place where BCR is useful. It tells us that we are "done" in terms of optimizing the runtime, and we should therefore turn our efforts to the space complexity*
* in fact, even without the interviewer prompting us, we should have a question mark with respect to our algorithm. We would have achieved the exact same runtime if the data wasn't sorted. So why did the interviewer give us sorted arrays? That's not unheard of, but it is a bit strange
* We are now looking for an algorithm that: Operating in O(1) space (probably). We already have an O(N) space algorithm with optimal runtime. If we want to use less additional space, that probably means no additional space. Therefore, we need to drop the hash table
* Operates in O(N) time(probably). We'll probably want to at least match the current best runtime, and we know we can't beat it
* use the fact that the arrays are sorted.
* Our best algorithm that doesn't use extra space was the binary search one. Let's think about optimizing that. We can try walking through the algorithm.
* Think about BUD. The bottleneck is the searching. Is there anything unnecessary or duplicated?
* It is unnecessary that A[3]=40 searched over all of B. We know that we just found 35 at B[1], so 40 certainly won't be before 35.
* Each binary search should start where the last one left off
* in fact, we don't need to do a binary search at all now. We can just do a linear search. As long as the linear search in B is just picking up where the last one left off, we know that we're going to be operating in linear time
* This algoritm is very similar to merging two sorted arrays. It operates in O(N) time and O(1) space
* We have now reached the BCR and have minimal space. We know that we can not do better
* This is another way we can use BCR. If you ever reach the BCR and have O(1) additional space, then you know that you can't optimize the big O time or space
* Best Conceivable Runtime is not a "real" algorithm concept, in that you won't find it in algorithm textbooks. But I have found it personally very useful, when solving problem myself, as well as while coaching people through problems.

### Handling Incorrect Answers
* SKIM

### When You've Heard a Question Before
* SKIM

### The "Perfect" Language for Interviews
* Some of the verbosity of Java can be reduced by abbreviating

### What Good Codinng Looks Like
* Correct
* Efficient
* Simple
* Readable
* Maintainable

### Use Data Structure Generously
* Suppose you were asked to write a function to add two simple mathematical expressions which are of the form Ax^a+Bx^b+...

		Class ExprTerm{
			double coeffient;
			double exponent;
		}

		ExprTerm[] sum(ExprTerm[] expr1, ExprTerm[] expr2){
			...
		}

### Appropriate Code Reuse
* Sidetacked, cool code:

		int convertFromBase(String number, int base){
			if ( base<2 || (base>10 && base!=16)) return -1;
			int value=0;
			for (int i=number.length()-1;i>=0;i--){
				int digit=digitToValue(num.charAt(i));
				if (digit<0 || digit >=base){
					return -1;
				}
				int exp=number.length-1-i;
				value+=digit*Math.pow(base,exp);
			}
			return value;
		}

### Modular
* SKIP

### Flexible
* Just because your interviewer only asks you to write code to check if a normal tic-tac-toe board has a winner, doesn't mean you msut assume that it's a 3x3 board. Why not write the code in a more general way that implements it for an NxN board?

### Error checking
* IMPORTANT

# Chapter 8: The offer and Beyond
page 75

# Chapter 11 Interview Questions

## 1. Array and String
* Hashtable implementation: SKIM
* ArrayList & Resizable Array: In some languages, array are automatically resizable. The array or list will grow as you append items. In other languages, like Java, the array's size can't change after its creation.
* StringBuilder: when you concatenate string, it creates new string. Instead, use StringBuilder

		//from
		String joinWords(String[] words){
			String sentence= "";
			for (String w: words){
				sentence=sentence+w;
			}
			return sentence;
		}

		//to
		String joiWords(String[] words){
			StringBuilder sentence=new StringBuilder();
			for(String w: words){
				sentence.append(w);
			}
			return sentence.toString();
		}

* Addtional Reading: Hash Table Collision Resolution, Rabin-karp Substring Search

## Interview Question

### 1.1. Is Unique:

		public static void main(String[] args) {
	        if(check("abcdeg")){
	            System.out.println("unique");
	        }else{
	            System.out.println("not unique");
	        }
	    }
	    static boolean check(String s){
	        ArrayList<Character> array=new ArrayList<>();
	        for (int i=0;i<s.length();i++){
	            for(int j=0;j<array.size();j++){
	                if(array.get(j)==s.charAt(i)){
	                    return false;
	                }

	            }
	            array.add(s.charAt(i));
	        }
	        return true;
	    }

* MY runtime O(N^2) where N is the length, but amortized, CORRECTED: THIS IS NOT AMORTIZED. its just 2 loop
* Tip 1: use a hashtable; so the searching can go from O(N) to O(1); so the result will be O(N)
* best conceivable is O(N)
* https://www.geeksforgeeks.org/determine-string-unique-characters/
* O(nLogn), sort is nlogn, check is n

		boolean uniqueCharacters(String str)
	    {
	        char[] chArray = str.toCharArray();

	        // Using sorting
	        // Arrays.sort() uses binarySort in the background
	        // for non-primitives which is of O(nlogn) time complexity
	        Arrays.sort(chArray);

	        for (int i = 0; i < chArray.length - 1; i++) {
	            // if the adjacent elements are not
	            // equal, move to next element
	            if (chArray[i] != chArray[i + 1])
	                continue;

	            // if at any time, 2 adjacent elements
	            // become equal, return false
	            else
	                return false;
	        }
	        return true;
	    }


### Check Permutation

		//check the whole array, if there is, remove it, if there isn't, then its not permutation
	    //check the whole array b with first character of array a, go through the whole array b, if there is, remove it, and
	    //go to the second character of array b, if there isn't then its not a permutation
	    //when the whole array b is check, and nothing happen, then its true
	    //if there isn't: if checking the the first element of array b, and check the last element of array a, and can't find it
	    //
	    /* abc
	    bca
	    for i

	     * /
	    static boolean check(String a, String b){
	        if(a.length()!=b.length()) return false;
	        char[] aArray=a.toCharArray();
	        ArrayList<Character> array= new ArrayList<>();
	        for(int aa=0;aa<aArray.length;aa++){
	            array.add(aArray[aa]);
	        }
	        for(int i=0;i<b.length();i++){
	            for (int j=0;j<array.size();j++){
	                System.out.println(array.size());
	                if(array.get(j)==b.charAt(i)){
	                    array.remove(j);
	                    break;
	                }
	                if((j==array.size()-1)&&(array.get(j)!=b.charAt(i))){
	                    return false;
	                }
	            }

	        }
	        return true;
	    }

* This is brute force again
* It is O(N^2)
* Checked the second hint, I can use a hash table, it will become O(N)
* Check solution: sorting is the simplest. Will take O(NlogN+N)=O(NlogN)

### URLify

		public static String convert(String s){
	        String t=s.trim();
	        char[] charArray=t.toCharArray();
	        ArrayList<String> array=new ArrayList<>();
	        for (int j=0;j<charArray.length;j++){
	            System.out.println(String.valueOf(charArray[j]));
	            array.add(String.valueOf(charArray[j]));
	        }
	        for(int i=0; i<array.size();i++){
	            if (array.get(i).equals(" ")){
	                array.set(i,"%20");
	                System.out.println("this is in");
	            }
	        }
	        StringBuilder sb=new StringBuilder();
	        for (int k=0;k<array.size();k++){
	            sb.append(array.get(k));
	        }
	        return sb.toString();
	    }

* It is O(N)
* checking hint now
* Check solution now. The solution use only char array and do manipulate the char array directly.

		static char[] replaceSpace(char[] str, int trueLength){
			int numberOfSpace=countOfChar(str, 0, trueLength, ' ');
			int newIndex=trueLength-1+numberOfSpace*2;
			if(newIndex+1<str.length) str[newIndex+1]='\0';
			for (int oldIndex=trueLength-1;oldIndex>=0;oldIndex--){
				if(str[oldIndex]==' '){
					str[newIndex]='0';
					str[newIndex-1]='2';
					str[newIndex-2]='%';
					newIndex-=3;
				}else{
					str[newIndex]=str[oldIndex];
					newIndex--;
				}
			}
			return str;

		}
		static int countOfChar(char[] str, int start, int end, int target){
			int count=0;
			for (int i=start;i<end;i++){
				if(str[i]==target){
					count++;
				}
			}
			return count;
		}

* Solution: every line is essential. So the logic basically find out the new Index, and then copy the old text to the new index one by one, and then do arithmetic on newIndex-- and newIndex-=3
* this is true O(N)

## 1.4. Palindrome permutation
* So there is a shortcut. Just find out if there is 2 character for each words
* I also need to delete the space and lowercase them

		public static boolean check(String s){
			String t=s;
			t=t.replaceAll("\\s+","");
			t=t.toLowerCase();
		//        System.out.println(t);
			int[] letters=new int[128];
			for (int i=0; i<t.length();i++){

				letters[t.charAt(i)]++;
			}
			int single=0;
			for (int j=0;j<letters.length;j++){
				if (letters[j]%2!=0){
					if(letters[j]==1){
						single++;
					}
					System.out.println(letters[j]);
					if(single>1){
						return false;
					}
				}
			}
			return true;
		}

* realize I allow one single
* I got basically the same approach as the solution
* one alternative solution is bit vector, SKIP

MY: Did 3 questions in 2 hours

## 1.5 One Away

* MY: My solution

		public static void main(String[] args) {
		   if(totalCheck("abc","qbc")){
			   System.out.println("this is true!");
		   }else{
			   System.out.println("This is false");
		   }
	   }
	   public static boolean totalCheck(String a, String b){
		   if(check1MoreThanB(a,b)||check1LessThanB(a,b)||oneDifference(a,b)){
			   return true;
		   }else{
			   return false;
		   }
	   }
	   public static boolean check1MoreThanB(String a,String b){
		   if(a.length()-1==b.length()){

			   int countMax1=0;
			   int i=0;
			   int j=0;
			   //check all element of a, if there is one difference, that is okay, if there is two difference,
			   //return false
			   //if run to the last element, and everything before is the same, then okay
			   while(i<a.length()){
				   if(a.charAt(i)==b.charAt(j)){
					   i++;
					   j++;
					   if((i==a.length()-1)&&(countMax1==0)){
						   return true;
					   }
					   continue;
				   }else{
					   countMax1++;
					   i++;
					   if(countMax1>1){
						   return false;
					   }
				   }
			   }
			   return true;
		   }else{
			   return false;
		   }

	   }

	   public static boolean check1LessThanB(String a,String b){
		   if(a.length()==b.length()-1){

			   int countMax1=0;
			   int i=0;
			   int j=0;
			   //check all element of a, if there is one difference, that is okay, if there is two difference,
			   //return false
			   //if run to the last element, and everything before is the same, then okay
			   while(j<b.length()){
				   if(a.charAt(i)==b.charAt(j)){
					   i++;
					   j++;
					   if((i==b.length()-1)&&(countMax1==0)){
						   return true;
					   }
					   continue;
				   }else{
					   countMax1++;
					   j++;
					   if(countMax1>1){
						   return false;
					   }
				   }
			   }
			   return true;
		   }else{
			   return false;
		   }

	   }

	   public static boolean oneDifference(String a, String b){
		   if(a.length()==b.length()){
			   int countMax1=0;
			   for(int i=0;i<a.length();i++){
				   if(a.charAt(i)==b.charAt(i)){
					   continue;
				   }else{
					   countMax1++;
					   if(countMax1>1){
						   return false;
					   }
				   }
			   }
			   return true;
		   }else{
			   return false;
		   }

	   }

* Big O is O(max(aLength,bLength))
* Checking hints now
* Checking answer now. The first solution is basically my solution.

## String Compression

		public static String compress(String s){
	        StringBuilder sb=new StringBuilder();
	        char prev=s.charAt(0);
	        int count=1;
	        for(int i=1;i<s.length();i++){
	            //if its the same as previous, then add 1 to count
	            //if its different, change prev, and reset count
	            //add to sb only once when changing

	            if(s.charAt(i)==prev){
	                count++;
	            }else{
	                sb.append(prev);
	                sb.append(count);
	                prev=s.charAt(i);
	                count=1;
	            }
	            if(i>=s.length()-1){
	                sb.append(prev);
	                sb.append(count);

	            }
	        }
	        return sb.length()<s.length()?sb.toString():s;
	    }

* Simple, but will need debugger to find out where the bugs are
* It is O(N)
* Check solution, It is pretty similar too. Other alternative is too complicated

## Rotate Matrix
* MY: I did it, I know how to manipulate matrix now

		public static void main(String[] args) {
	        int[][] matrix= {
	                {1, 2, 3, 4},
	                {5, 6, 7, 8},
	                {9, 10, 11, 12},
	                {13, 14, 15, 16}

	                };
	        int[][] newMat=rotate(matrix);
	        for(int tall=0;tall<newMat.length;tall++){
	            for(int wide=0;wide<newMat.length;wide++){
	                System.out.print(newMat[tall][wide]);
	                System.out.print(",");
	            }
	            System.out.println();
	        }

	    }
	    public static int[][] rotate(int[][] matrix){
	        int layer=matrix.length/2;
	        //go through the first array, and move it to the corresponding
	        //go through the second array, move
	        //layer, there are 2 layers
	        int[][] newMat=new int[matrix.length][matrix.length];
	        for(int tall=0;tall<matrix.length;tall++){
	            for(int wide=0;wide<matrix.length;wide++){
					//if(tall==0&&wide==0){
	                    newMat[wide][matrix.length-1-tall]=matrix[tall][wide];
					// }
					//                if(tall==0&&wide==1){
					//                    newMat[1][matrix.length]=matrix[tall][wide];
					//                }
					//                if(tall==0&&wide==2){
					//                    newMat[2][matrix.length]=matrix[tall][wide];
					//                }
					//                if(tall==0&&wide==3){
					//                    newMat[3][matrix.length]=matrix[tall][wide];
					//                }
					//                if(tall==1&&wide==0){
					//                    newMat[0][matrix.length-1]=matrix[tall][wide];
					//                }

	            }
	        }

	        return newMat;
	    }

* Checking solution now
* Oh, I need to do it in place

		boolean rotate(int[][] matrix){
			if (matrix.length==0 || matrix.length!=matrix[0].length) return false;
			int n=matrix.length;
			for (int layer=0;layer<n/2;layer++){
				int first=layer;
				int last=n-1-layer;
				for (int i=first;i<last;i++){
					int offset=i-first;
					int top=matrix[first][i];
					//left->top
					matrix[first][i]=matrix[last-offset][first];
					//bottom->left
					matrix[last-offset][first]=matrix[last][last-offset];
					//right->bottom
					matrix[last][last-offset]=matrix[i][last];
					//top->right
					matrix[i][last]=top;
				}
			}
			return true;
		}

* MY: Did 3 questions today

## Chapter 2. Linked List
* The disadvantage is you don't have constant time retrieving Kth element
* The advantage is you can add and remove item from the beginning of the list in constant time

		class Node{
			Node next=null;
			int data;

			public Node(int d){
				data=d;
			}
			void appendToTail(int d){
				Node end=new Node(d);
				Node n=this;
				while(n.next!=null){
					n=n.next;
				}
				n.next=end;
			}
		}

* In this implementation, we don't have a LinkedList data structure. We access the linked list through a reference to the head Node of the linked list. When you implement the linked list this way, you need to be a bit careful. What if multiple objects need a reference to the linked list, and then the head of the linked list changes? Some object might still be pointing to the old head.
* We could, if we chose, implement a LinkedList class that wraps the Node class. This would essentailly just have a single mmeber variable the head Node. This would largely resolve the earlier issue.

### Deleting a Node from a Singly Linked List

		Node deleteNode(Node head, int d){
			if(head==null) return null;
			Node n=head;
			if(n.data==d){
				return head.next;
			}
			while(n.next !=null){
				if(n.next.data==d){
					n.next=n.next.next;
					return head;
				}
				n=n.next;
			}
			return head;
		}

### If you want to make a1->a2->an->b1->b2->...->bn to a1->b1->a2->b2->a3->b3->...
* you can have one pointer p1 (the fast pointer) move every two elements for every one move that p2 makes. When p1 hits the end of the linked list, p2 will be at the midpoint. Then move p1 back to the front and begin weaving the lement. On each iteration, p2 selects an element and inserts it after p1.


### 2.1 Remove Dups:

		public class RemoveDupLinkedList {



		    public static void main(String[] args) {
		        Random random=new Random();
		        LinkList ll=new LinkList();
		        for(int i=0;i<10;i++){
		            ll.insertFirst(random.nextInt(10));
		        }
		        ll.displayList();

		        //check dupliate. if the number has not appeared before, store it into a array,
		        //and check the next with the array
		        // go through a linked list, check the first element, if it isn't not in checklist
		        //add it, and then move one
		        // if its in the checklist, jump 2,
		        ArrayList<Integer> checklist=new ArrayList<>();
		        Node current=ll.first;
		        Node previous=ll.first;
		        //if contain, remove, if not contain, move to next
		        while(true)
		        {
		//            System.out.println("current.data is "+current.data);
		            if (!checklist.contains(current.data)) {
		                if(current.next == null){

		                    System.out.println("this is the end");
		                    break;
		                }

		//
		                else
		                {
		                    checklist.add(current.data);
		//                    System.out.println("add "+current.data);
		                    previous = current; // go to next link
		                    current = current.next;

		                }

		            }else{
		                if(current == ll.first) // if first link,
		                    ll.first = ll.first.next; // change first
		                else { // otherwise,
		                    //previous next go to current next, jump the current, current needs to be at previous again,
		                    //refinding the next duplicate
		                    previous.next = current.next; // bypass it
		                    current = previous.next;
		                    if(current==null){
		                        break;
		                    }
		//                    System.out.println("jump once");
		                }

		            }

		        }
		        ll.displayList();


		    }
		}
		class Node{
		    Node next=null;
		    int data;

		    public Node(int d){
		        data=d;
		    }
		    void appendTail(int d){
		        Node end=new Node(d);
		        Node n=this;
		        while(n.next!=null){
		            n=n.next;
		        }
		        n.next=end;
		    }
		}

		class LinkList
		{
		    public Node first; // ref to first link on list
		    // -------------------------------------------------------------
		    public LinkList() // constructor
		    {
		        first = null; // no items on list yet
		    }
		    // -------------------------------------------------------------
		    public boolean isEmpty() // true if list is empty
		    {
		        return (first==null);
		    }
		    // -------------------------------------------------------------
		// insert at start of list
		    public void insertFirst(int d)
		    { // make new link
		        Node newLink = new Node(d);
		        newLink.next = first; // newLink --> old first
		        first = newLink; // first --> newLink
		    }
		    // -------------------------------------------------------------
		    public Node deleteFirst() // delete first item
		    { // (assumes list not empty)
		        Node temp = first; // save reference to link
		        first = first.next; // delete it: first-->old next
		        return temp; // return deleted link
		    }

		    // -------------------------------------------------------------
		    public void displayList()
		    {
		        System.out.print("List (first-->last): ");
		        Node current = first; // start at beginning of list
		        while(current != null) // until end of list,
		        {
		//                current.displayLink(); // print data
		            System.out.print(current.data+" ");
		            current = current.next; // move to next link
		        }
		        System.out.println("");
		    }
		    // -------------------------------------------------------------
		    public Node find(int key) // find link with given key
		    { // (assumes non-empty list)
		        Node current = first; // start at 'first'
		        while(current.data != key) // while no match,
		        {
		            if(current.next == null) // if end of list,
		                return null; // didn’t find it
		            else // not end of list,
		                current = current.next; // go to next link
		        }
		        return current; // found it
		    }
		    // -------------------------------------------------------------
		    public Node delete(int key) // delete link with given key
		    { // (assumes non-empty list)
		        Node current = first; // search for link
		        Node previous = first;
		        while(current.data != key)
		        {
		            if(current.next == null)
		                return null; // didn’t find it
		            else
		            {
		                previous = current; // go to next link
		                current = current.next;
		            }
		        } // found it
		        if(current == first) // if first link,
		            first = first.next; // change first
		        else // otherwise,
		            previous.next = current.next; // bypass it
		        return current;
		    }
		    // -------------------------------------------------------------

		} // end class LinkList

* delete an element essentially is previous.next=current.next
* move one is essentially, previous=current; current=current.next;
* check answer:

		void deleteDups(LinkedListNode n){
			HashSet<Integer> set=new HashSet<Integer>();
			LinkedListNode previous=null;
			while (n!=null){
				if(set.contains(n.data)){
					previous.next=n.next;
				}else{
					set.add(n.data);
					previous=n;
				}
				n=n.next;
			}

		}

* if temporary buffer is not allowed:

		void deleteDups(LinkedListNode head){
			LinkedListNode current=head;
			while(current!=null){
				//remote all future nodes that have the same value
				LinkedListNode runner=current;
				while(runner.next !=null){
					if(runner.next.data=current.data){
						runner.next=runner.next.next;
					}else{
						runner-runner.next;
					}
				}
				current=current.next;
			}
		}

## Return Kth to Last

* I already make count wrong
* this counts 10. Use this one:

		int count=0;
        while(n!=null){
            count++;
            n=n.next;
        }

* this counts 9

		int count=0;
		while(n.next!=null){
			count++;
			n=n.next;
		}

* My code:

		public static void main(String[] args) {
		        int k=2;
		        Random random=new Random();
		        LinkList ll=new LinkList();
		        for(int i=0;i<10;i++) {
		            ll.insertFirst(random.nextInt(10));
		        }
		        ll.displayList();
		        Node n=ll.first;
		        int count=0;
		        while(n!=null){
		            count++;
		            n=n.next;
		        }
		        System.out.println("count is "+count);
		        int ktolast=count-k;
		        Node m=ll.first;
		        int i=0;
		        while(m.next!=null&&i<count-k-1){
					//check if m is the last node, if it is the last node, stop traversing
		            System.out.println(m.data);
		            m=m.next;
		            i++;

		        }
		        System.out.println("last k is "+m.data);
		    }

* I didn't do && I did ||, because I did ||, the first condition is still true, so it still goes on,
* second, 10-2 is 8, this is no the position, I need it to be 10-2-1



## Chapter 8: recursion
* By CS Dojo: https://www.youtube.com/watch?v=B0NtAFf4bvU
* I need the definition writing
* I need to know how to do recursion in tree and linked list
* reverse a linked list: https://www.youtube.com/watch?v=KYH83T4q6Vs
* Pattern: ReversePrint:

		void ReversePrint(Node p){
			if (p==null){
				return;
			}
			ReversePrint(p.next);
			printf("%d",p.data);
		}

* Pattern: factorial:

		int fact(int n)
		{
			if (n < = 1) // base case
				return 1;
			else    
				return n*fact(n-1);    
		}

* Pattern: Reverse a Linked list
		//https://www.youtube.com/watch?v=KYH83T4q6Vs
		//head->100->200->150->250->null
		void Reverse(Node p){
			if(p.next==null){
				//when p is 250
				head=p;
				return;
			}
			Revser(p.next);
			//do 2 things: reverse the link (delete and relinked to null and relink)
			//and point the second node to null
			Node q=p.next; //p will be 150 so p.next is 250
			q.next=p; //250's next is 150
			p.next=null;
			//this can be shorted as p.next.next=p
		}

* pattern: a=b; b=a will make a,b equals to
* pattern: q=p.next; q.next=p; will make reverse a link of q, which is p.next
* in other word, will reverse what p is pointing to
* MY: pattern: every statement after the recursion will execute at comingback
* search a linked list recursively: https://www.youtube.com/watch?v=7sikRsNcqgM
* if head is null, return false; 2. if head's key is same as x, return true; else return search(head.next,x)

### GeekforGeek
I checked the chapter but it's not really informative. So I check geekForGeek and [here](https://www.geeksforgeeks.org/recursion/) it is.

Certain problems can be solved quite easily, such as [Towers of Hanoi(TOH)](https://www.geeksforgeeks.org/c-program-for-tower-of-hanoi/),[inorder/Preorder/Postorder Tree Traversals](https://www.geeksforgeeks.org/tree-traversals-inorder-preorder-and-postorder/), [DFS of Graph](https://www.geeksforgeeks.org/depth-first-search-or-dfs-for-a-graph/)

##What is base condition in recursion?
In the recursive program, the solution to the base case is provided and the solution of the bigger problem is expressed in terms of smaller problems

        int fact(int n)
        {
            if (n < = 1) // base case
                return 1;
            else    
                return n*fact(n-1);    
        }

In the above example, base case for n < = 1 is defined and larger value of number can be solved by converting to smaller one till base case is reached.

###How a particular problem is solved using recursion?
1. represent a problem in terms of one or more smaller problems
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

**Advantage: recursion provides a clean and simple way to write code. Some problems are inherently recurisve like tree traversals. We can write such codes also iteratively with the help of a stack data structure. For example refer [inorder tree traversal without recursion](https://www.geeksforgeeks.org/inorder-tree-traversal-without-recursion/), [iterative Tower of Hanoi](https://www.geeksforgeeks.org/iterative-tower-of-hanoi/)**

### Practice Questions for Recursion | Set 1

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

          // Assume that minIndex() returns index of minimum value in  
            array arr[start_index...end_index]
          min_index = minIndex(arr, start_index, end_index);

          temp = arr[start_index];
          arr[start_index] = arr[min_index];
          arr[min_index] = temp;         

          fun2(arr, start_index + 1, end_index);
        }

Answer: The function fun2() is a recursive implementation of Selection Sort.

MY: Will get back to it a bit later

## Practice Questions for Recursion | Set 2

    /* Assume that n is greater than or equal to 1 * /
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

    /* Assume that n is greater than or equal to 0 * /
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
