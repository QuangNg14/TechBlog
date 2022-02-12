---
title: "SWE Internship: Nailing the technical interviews"
catalog: true
date: 2022-02-12 10:13:46
subtitle:
header-img: "image.png"
tags:
readtime:
---

# SWE Internship: Nailing the technical interviews

There are a lot of posts and videos that you may have gone through when preparing for a technical interview but I will you a detailed and insightful post on my journey of making it through the interviews. Remember even if you know a lot about CS and even get a CS college degree, you will still need to study and practice A LOT to be able to cover all the possible topics in an interview. When I first started practicing coding interviews, I cannot even do Leetcode easy problems. It would take me 1-2 hours just to figure out an easy problem. It was frustrating and scary but you would be surprised that this is the case for 90% of the people out there. We all start at point 0 and work our way up gradually.

## Motivation

It took me 3 months to crack the technical interview and my biggest lesson I learned was consistency. There was no single day in those 3 months that I did not complete at least one LeetCode problem or learn at least one HackerRank exercise. I am not crazy smart, daily practice was what got me through all the DS & Algorithm questions in my interviews.

The only strategy here is **CONSISTENCY**. DON’T cram 100 questions on the last couple of days. It takes time for you to be familiar with all the concepts and the techniques asked on interviews so it is definitely not an overnight thing that you can cram.

My biggest motivation is that technical interviews are essentially the last stage in the whole process. If you think of recruiting as a funnel, with thousands of candidates out there submitting resumes to firms, those being offered a technical interview have already stepped one foot into the door. While resume screening and behavioral interviews are subjective, technical interviews aren’t. They only depend on hard work, so all the extra hours of practice will all be worth it.

{% asset_img motivation.jpg %}

## Computer Science courses

No doubt, university courses offer the most crucial resources. Take classes on data structures and algorithms. These courses were crucial to my understanding of the questions asked in technical interviews. I always paid close attention to these classes and I often did some extra practice after the actual hours at school. The topics and concepts from CS classes are closely aligned with the coding interview problems so don’t underestimate the value of these classes.

One piece of advice that I have for taking these courses is to try to implement all the algorithms and data structures you are learning. For example, if you have just learned about dynamic programming in class through some examples, try to code out those examples on your own and figure out a set of steps needed to set up the problem. Determine the overlapping subproblems that can be memorized, choose between using a bottom-up (table), top-down, or by length, write out a recursive formula and lastly convert that into a memorized method.

I have attached some helpful university course links that you may want to check out:

- CS50 Harvard: [https://cs50.harvard.edu/law/2019/weeks/3/](https://cs50.harvard.edu/law/2019/weeks/3/)
- MIT Intro to Algorithm: [https://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-006-introduction-to-algorithms-spring-2008/](https://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-006-introduction-to-algorithms-spring-2008/)
- University of Birmingham: [https://www.cs.bham.ac.uk/~jxb/DSA/dsa.pdf](https://www.cs.bham.ac.uk/~jxb/DSA/dsa.pdf)

## LeetCode and HackerRank

Let’s get straight to the point. These are definitely 2 most widely used website for practicing technical interviews. Since there are thousands of questions on these platforms but not everyone knows how to approach these problems. Some pieces of advice that I gathered over time:

1. Go through all the major topics first and then go deep into the topics commonly asked by the company you are interviewing for. What is bread first search? What is depth first search?
2. Create a board (on Notion or any note-taking platform you use) to keep track of what you have been studying on LeetCode and HackerRank everyday. I had the habit of attaching links to important problems for each topic, as well as additional relevant problems.

   {% asset_img board.png %}

   Fun fact: Notion is my favorite note-taking app! Inside each card, I noted down carefully the problems I had studied for each day and some quick notes to guide my revision process later.

   {% asset_img tree.png %}

3. The tag functionality on LeetCode is really useful. Let’s say you want to practice Tree problems. You can easily search the problems using the tree tag and sort the problems based on the frequency. It is always good to learn the most frequently asked questions first.

   {% asset_img leetcode.png %}

4. **ALWAYS** try to code the problem on your own first. DO NOT be tempted to look at the solution. It will make you dependent on the solution and impair your brainstorming process when you see a new problem in your technical interview. If you have no clue how to solve the problem after 1-2 hours of thinking, you should take a look at the hint section. If after that you still don’t know what to do, then look at the solution and take some notes about the problems and make sure you come back to tackle problems with similar patterns.
5. There are always some patterns to the problems. It could be implementing a queue, stack or using prefix sum or tree traversal techniques. Make sure you understand and note down the **PATTERNS** of the problems. This would be really helpful because later when you have practiced a lot, you can identify a problem’s pattern and devise a suitable solution.
6. Set out a goal for your learning and test your skills periodically. Once you have understood all the topics you want to learn at a fundamental level, go deeper in each of the topics. There are some useful lists of problems that were compiled by the LeetCode users:
   1. LeetCode problems by company tag: https://github.com/xizhengszhang/Leetcode_company_frequency
   2. Two-pointers: [https://leetcode.com/discuss/study-guide/1688903/Solved-all-two-pointers-problems-in-100-days](https://leetcode.com/discuss/study-guide/1688903/Solved-all-two-pointers-problems-in-100-days)
   3. DP: [https://leetcode.com/discuss/study-guide/458695/dynamic-programming-patterns](https://leetcode.com/discuss/study-guide/458695/dynamic-programming-patterns)
   4. Binary Tree: [https://leetcode.com/discuss/study-guide/1212004/binary-trees-study-guide](https://leetcode.com/discuss/study-guide/1212004/binary-trees-study-guide)
   5. Blind 75: [https://leetcode.com/discuss/general-discussion/460599/blind-75-leetcode-questions](https://leetcode.com/discuss/general-discussion/460599/blind-75-leetcode-questions)
7. You can subscribe to LeetCode premium if you want to access the explore all sections of LeetCode and unlock some extra problems. You can take advantage of the Explore tab when you have unlocked Premium. Personally, I think this is a really good investment but you definitely don’t have to.
8. The discussion section in LeetCode is much more valuable than you may assume. Understanding how to do a problem in multiple ways (for example, implementing stack, queue or array for a question) would enable you to be more flexible in devising a correct approach to a problem later on.

## The actual interview

This is where all your hard work and all of your rigorous training will sum up to. I was really stressed out during both of my interviews but I have some tips that may help you on the interview day:

1. Be prepared. As I have mentioned above, the skills used during the interview day cannot be acquired overnight. It takes time to understand and to be able to implement those techniques. So plan ahead!
2. Ask clarifying questions before starting to code. It is super important that you ask the interviewer some meaningful questions about the problem to make sure you are on the right track.
3. Identify some edge cases or base cases. Always start with the base cases and work your way up to more complex cases. Then make sure to analyze some edge cases.
4. Remember to explain your code constantly with your interviewer. You need to keep them updated on what you are trying to do and what your approach is. You don’t have to produce a clean and perfectly efficient solution right away but you should try to provide a good approach to the problem. Communicating your code and ideas clearly to the interviewer has a bigger impact than silently writing the solution.
5. It’s very likely that you are not going in the right direction or you are getting stuck and that’s completely normal. You can ask the interviewer for some time to think more about it and once you have a direction make sure to explain that to the interviewer. Sometimes you can also ask for help - it gets a lot worse if you just communicate nothing.
6. Make sure to go into some detail about your approach instead of just mentioning it. “I will use BFS, DFS, or Dijkstra algorithm for this problem” is not a good answer. Try to expand a bit on what you are trying to do. How will you represent the graph? What do the edges and the nodes represent? What does the output from BFS, DFS mean?

{% asset_img interview.jpeg %}

## List of common topics

Here is a very detailed list of the common topics on algorithms and data structures and good resources on how to study them:

- **Data Structures:**
  - Array and String
  - Linked List (Single and Double)
  - Stack and Queue
  - Priority Queue (Heap)
  - Binary Search Tree: one of the most common topics in technical interview. MUST LEARN
  - Graph
  - HashMap
  - Disjoint Set
- **Algorithms**
  - Tree traversal: inorder, preorder, postorder (both recursive and iterative), level order traversal
  - Graph algorithms: BFS, DFS, Dijkstra, Kruskal and Prim
  - Binary Search and its implication
  - Sorting algorithms: quicksort, mergesort, topological sort in graph
  - Recursion: this is one of the core concept of CS.
  - Dynamic Programming
  - Bit Manipulation (AND, NOT, OR, XOR)
  - Greedy Algorithms
  - Divide and Conquer
- **Some good techniques and patterns:**
  - Two pointers
  - Hashing
  - Prefix Sum
  - Backtracking
  - Sliding Window
  - Randomized
  - Monotonic Stack/Queue
- **Math topics:**
  - Combinations
  - Permutations
  - Factorial
  - Set and Functions
  - Algebra and Arithmetic
  - Discrete mathematics
    There are a lot of good resources for you to review all of these concepts. The data structures are related and learning all the basic concepts will help you come up with the solution much easier.

## Some of the good links on algorithm and data structures\*\*

- freeCodeCamp: [https://www.youtube.com/channel/UC8butISFwT-Wl7EV0hUK0BQ](https://www.youtube.com/channel/UC8butISFwT-Wl7EV0hUK0BQ)
- Kevin Naughton Jr. [https://www.youtube.com/channel/UCKvwPt6BifPP54yzH99ff1g](https://www.youtube.com/channel/UCKvwPt6BifPP54yzH99ff1g)
- Hackerrank: [https://www.youtube.com/c/HackerrankOfficial](https://www.youtube.com/c/HackerrankOfficial)
- CS Dojo: [https://www.youtube.com/channel/UCxX9wt5FWQUAAz4UrysqK9A](https://www.youtube.com/channel/UCxX9wt5FWQUAAz4UrysqK9A)
- GeeksforGeeks: this is one of the most helpful sources I find on the internet. It has detailed explanation to all the algorithms and data structures
- Introduction to Algorithms CLRS
- Cracking the Coding Interview: this is the “Bible” of coding interview. You should always review this book before any interview
- Glassdoor and Reddit: [https://www.reddit.com/r/csMajors/](https://www.reddit.com/r/csMajors/) and [https://www.reddit.com/r/cscareerquestions/](https://www.reddit.com/r/cscareerquestions/)
- Discord: [https://discord.com/invite/cscareers](https://discord.com/invite/cscareers)
- Technical Interview Handbook: [https://www.techinterviewhandbook.org/introduction/](https://www.techinterviewhandbook.org/introduction/)

## Time Complexity and Space Complexity

This is an essential part of any coding interview. You must understand how to analyze the time and space complexity of the algorithm piece that you have just written. This question was asked in all of my 7 coding interviews, typically after I had finished solving a question or after I had explained my approach to the interviewer

{% asset_img bigo.jpeg %}

These are 2 posts that I would recommend checking out for the time complexity and Big-O notation:

- [https://www.geeksforgeeks.org/understanding-time-complexity-simple-examples/](https://www.geeksforgeeks.org/understanding-time-complexity-simple-examples/)
- [https://www.geeksforgeeks.org/analysis-of-algorithems-little-o-and-little-omega-notations/](https://www.geeksforgeeks.org/analysis-of-algorithems-little-o-and-little-omega-notations/)

## Mock interviews

Let’s face it. Talking while you are coding is really hard and counterintuitive. Everything requires practice and mock interviewing is one of the most efficient ways for you to familiarize yourself with the interview style. Some of my tips for familiarizing yourself with the technical interview style: First, while practicing the problems on LeetCode or HackerRank, try speaking out the problems and what your approach is. As you are writing the code, just practice speaking out loud and try to explain your code and your solution ideas along the way. The second thing you can do is to have a mock interview with your peers and friends. They are also practicing for the interviews so this will be a good way for both of you to get practice and feedback for each other. The last thing you can do is do an online mock interview with a stranger on the Internet. This would also help out a lot because you will get to interact with a new person. You can find online mock interviews on platforms like LeetCode.

## Understand your current progress

I have to admit that I never felt completely ready for an interview no matter how long I studied and how much I practiced. There are always more topics to dive deeper into or weak areas that need to be reviewed. Keeping track of your progress and getting feedback from your peers contribute a lot to improving performance on the real technical interview.
What I usually do to check if I am ready for the interview or not is to keep track of my completion times when I do practice questions. I am definitely not the best example, as a reference I would complete problem labeled Medium on LeetCode or HackerRank in 30-40 minutes and 45-60 minutes if it the question was labeled Hard. Also, try to pass all the edge cases to get AC (accepted) on LeetCode or HackerRank. In the real interview, the number of test cases is much smaller. Interviewers do expect you to cover some edge cases to see whether you understand the problem and your solution, but you don’t need to pass every edge case for it to count. Still, it is good practice to make sure you provide an exhaustive solution.

There is definitely an element of luck in the whole process. You may get all questions on topics you’re really comfortable with or you have practiced rigorously. On the other hand, you may get a question on a topic you haven’t touched. You cannot learn everything or prepare for every possible outcome, but more practice means higher chances of success.

If you follow the methods above, you can easily keep track of your readiness level. If you are hitting your completion times and have passed many mock interviews, be confident because the interview will be very much like what you have been practicing all this time.

## Ending

This is the end of a super long post. Above are all my important skills and experiences I want to share with you guys as I went through technical interviews and got a job offer at FAANG as a sophomore. Good luck to all of you in your job search process. Thank you for reading and see you guys in the next post.
