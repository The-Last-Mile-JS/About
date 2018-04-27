# A coding tutor designed for prisons  

## Motivation

For all of the free “learn to code” programs out there, and all of the open-source educational platforms, workbooks, tools, and so on, few work well inside prison. For one, we’re teaching full-stack web programming without internet access: that alone breaks most existing tools. Here are the main ways that our classroom setup (and hence software requirements) are unique: 


- No internet access for students. Our staff has limited access. 
- No live teachers. We’re now at 5 prisons and seeking to expand; some of these prisons are in remote locations with no software industry. Instead, we have hundreds of books, hundreds of hours of video lectures, and a flexible curriculum that aims to guide students through it all. 
- Lockdowns are frequent. Class can be cancelled with little notice, sometimes for as long as two weeks. Our program tends to be **self-paced** for this reason. 
- On a given week, our students are only given ~35 hours of computer access.  The rest of their day is spent outside of the computer lab, with several hours per day locked in a cell or dorm. For this reason, **printing out coding problems and practicing them on paper** is common. 
- Cheating is a major concern. Through initiatives such as [Proposition 57 milestones](https://www.cdcr.ca.gov/proposition57/docs/FAQ-Prop-57-Creadits-Earning.pdf), doing well in our program can even help students reduce their sentence and potentially go home early. As a result, camaraderie between inmates can sometimes turn into cheating.    

What follows is a proposal outline for a coding tutor that works offline, disincentivizes cheating, supports print-based practice workflows, and can adapt flexibly to unpredictable delays. Please see the [background and overview](/doc/Background-and-Overview-TLM-Oz3gCFN3FC1WYGqw9kq4U?_tk=share_copylink) doc to learn more about our classroom setup. 


## Proposal: build a system to help students learn and practice JavaScript

**Goals**: learn JavaScript, learn foundational programming skills, learn the basics of unit testing, learn git. 
**Out of scope**: HTML, CSS, jQuery, web frameworks such as React. 

This tutoring system has these parts:

- **Coding Problems**. At its core, this system is a big collection of practice problems. These problems live in a git repo. Students make progress by making git commits to their local repo, and submit work by pushing code to a submissions repo. Each problem has these parts:
  - **Starter code**. What code do you want to give students to start with? At minimum, this would be an empty function stub and some code comments. On the other hand, if the problem is designed to teach unit testing, the “starter code” might be a buggy function that is fully implemented, with the challenge being to write good unit tests and then find and correct the bugs.  
  - **Unit tests**. Do you want to give students unit tests, such that they know whether they got the right answer? It would be nice to give at least one unit test per problem, and make it possible for students to add their own unit tests too. 
  - **Private unit tests**. We want to train students to write correct code. To that end, we should not always give them all of the unit tests used to automatically grade assignments. We should encourage them to write their own unit tests, to verify their own code. 
  - **Difficulty** (integer, 1 to 5). Is this an easy or hard coding challenge? 
  - **Category**. Which group of problems is this problem associated with? Example categories might include: 
    - Booleans / Logic
    - Numbers / Math
    - Strings
    - Conditionals
    - Iteration
    - Recursion
    - Regular Expressions
    - Arrays
    - Objects
    - Timers
    - Unit Testing Practice
    - Functional Programming
    - Algorithms
    - etc…
- **Code Runner**. Via the command line, or via the browser, build a tool that runs unit tests for students and shows them the result. This would be a light visualization built on top of a preexisting unit test framework such as [tape](https://github.com/substack/tape). 
- **Leaderboard**. Students compete with each other on three metrics, per coding problem: submission time (earlier is better), correctness (% of unit test success, higher is better), and simplicity (code golf score, lower is better, see below). The leaderboard is a website they can go to that visualizes information stored in git. A leaderboard is one way to disincentivize cheating. 
- **Progress map.** This visualization shows students how many problems they have completed, and what they have left. See, for example, the Khan Academy [progress map UI](http://www.khanacademy.org/images/athena-intro-screens/view-progress.png). 
- **Print support**. It should be easy for students to print out groups of problems so that they can practice on paper, during the part of the day when they don’t have computer access. This practice is meant to be ungraded.
- **Submission system**. This is a git repo owned by admins, where students have push access only. Instructors will set deadlines with students on when problems need to be submitted; deadlines do not need to be managed by this system. This submission system includes automated grading (via running unit tests). Students cannot see their grade until after the deadline passes. 
- **Peer submission view**. After enough time has passed after a problem due date, students should be able to learn from each other by looking at each others’ code. Posting each others’ submissions is another good way to disincentivize cheating. Students should be able to look at each peer’s submission for a single problem, showing name / code / unit tests, statistics (submission time, unit test correctness %, code golf score). **Admin feature**: if cheating is ever suspected — one student is copying another, let’s say — it would be nice for an admin to be able to view pairs of submissions back-to-back and see for themselves.  

Because `git` is core to this system, perhaps this is how the `git` repository structure might look: 

On a server that students do not have access to, three `git` repositories exist: 

- A **blank repo** containing all of the starter problem code and starter unit tests. Students have **read-only clone access** only. This is the first thing a new student would do: clone the blank repo into their own local repo. 
- A **submission repo** for submitting work. Perhaps each student has their own branch on this repo, with **push/pull access** for one student per branch.  `git push` to this repo is how students back up their work. They cannot see other submissions because they do not have pull access to other branches. If they lose their local repo or jump to a new computer, they could make a new clone from this branch to regain all of their old progress. The submission repo might also be the best place to put **private unit tests** meant for automatic grading (on a branch students can’t pull from, of course). 
- A **view repo**. After enough time after the submission deadline has passed for a given problem, an admin can use git to push from the submission repo to a view repo. Students have read access to all branches in the view repo. The **peer submission view** feature is simply a visualization layer on this repo. For now, TLM admins can manually use git to push from the submission repo to the view repo (or use a script you provide); no need to automate this process. 

How do students see their progress map? The progress map should be a web app inside their repo, that they can run from the command line. They would then view the progress map in a browser, say, at http://localhost:8080. Alternatively, they could view their progress map in text form by running a script. 

The leaderboard and peer submission viewer should similarly be a web app that lives inside these git repos. Students can run the leaderboard and submission viewer locally (or play with the code); they will be the only participant in the leaderboard locally. Admins will keep the leaderboard running on a server where the view repo is stored — that will show all student submissions, since that git repo stores everything.  

[git hooks](https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks) can be used to automatically re-derive the progress map / leaderboard / code runner results etc upon submission changes. 


## What does success look like? 

A successful project will be put into real practice! If TLM uses this tutor long-term across all of our prisons, I would consider this project to be a great success. I also recommend publishing your work on github openly, such that you can add this experience to your portfolio, and perhaps even recruit other volunteers to help you maintain it.  


## Leaderboard

The view repo will contain a bunch of useful metadata that can be used to make a leaderboard visualization: 

- Submission dates. Who submits code first? 
- Correctness: Who writes the least buggy code, based on private unit tests that students didn’t have access to before the assignment deadline? A percentage would work well here: correctness == the percent of private unit tests that pass.  
- Simplicity (see the “code golf” section). 
- Bonus: who writes the best unit tests? (See the end of the doc for an outline on how this could work.)


## Print support

This feature allows students to easily print groups of problems to study in their cells. It should add plenty of whitespace per problem to let them pencil in their solutions. The next day, they’ll be able to quickly type in their hand-written code to see if it runs, and save time in lab. This practice is already very common. Your job is to reduce friction and make it easier. 


## Code Golf

“Good” code isn’t just *correct*, but also *minimizes complexity* — good code is succinct and as easy as possible for others to read and maintain. How can we incentivize our students to strive for simplicity? 

One possible idea: include a `golf score` metric, per coding problem, in the progress map and leaderboard. [*Code golf*](https://en.wikipedia.org/wiki/Code_golf) aka *golf scripting* is a game to see who can write the shortest code that solves a given problem. But shortest doesn’t necessarily mean simplest — we do **not** want to incentivize our students to use shorter variable names or avoid whitespace. Similarly, we definitely don’t want to incentivize our students to avoid code comments! 

Proposal: to calculate a `golf score`, first minify student code with a library like [babel-minify](https://github.com/babel/minify) before taking the length. Including instructors’ golf scores along with their solutions, and perhaps even including a [*par*](https://en.wikipedia.org/wiki/Par_(score)) for each problem, could help students avoid overly optimizing this metric.

An example might help explain. Consider the following code challenge: 


    /*
    Write a function is_alphabetic_palindrome() that returns true if and only if the text is a palindrome (the same forwards and backwards). Specification details:
    1. Your logic should ignore capitalization. 'Abba' and 'ABCcba' are palindromes.
    2. Your logic should ignore non-alphabetic characters (a-z). 
       'Never odd or even!' and 'Lonely #$&* Tylenol' are both palindromes.
    
    function is_alphabetic_palindrome(text) {
      // your code here
    }
    */

The following is a reasonable and correct way to implement this challenge. I’ve seen more than one student write code that looks like this. But maybe you’ll notice some ways to make it simpler? How can we naturally encourage our students to do the same?  


    function is_alphabetic_palindrome(text) {
      // Filter by ASCII character code to build an array 
      // of alphabetic characters. 
      const alphas = [];
      for (let i = 0; i < text.length; i++) {
        const [char, charCode] = [text.charAt(i), text.charCodeAt(i)];
        if ((65 <= charCode && charCode <= 90) ||  // A-Z
            (97 <= charCode && charCode <= 122)) { // a-z
          alphas.push(char.toLowerCase());
        }
      }
      // A palindrome will have matching characters when 
      // walking from the start vs. walking from the end.   
      // For example, for arr = ['a', 'b', 'c', 'b', 'a'], 
      // arr is palindromic because:
      // arr[0] === arr[4] && arr[1] === arr[3]
      const n = alphas.length;
      for (let i = 0; i <= Math.floor(n / 2) - 1; i++) {
        if (alphas[i] !== alphas[(n-1) - i]) {
          return false;
        }
      }
      return true;
    }

While correct, the student that wrote this could have lowercased the input string from the beginning. That way, they would only need to check one character code range, between 97 and 122. Or they could have used a regular expression to avoid ASCII codes (yuk) entirely. Instead of comparing pairs of starting/ending characters, they could have used a standard library function like `reverse()`. This second version is so simple and self-documenting that code comments aren’t necessary: 


    function is_alphabetic_palindrome_v2(text) {
      text = text.toLowerCase().replace(/[^a-z]/g, '');
      return text === text.split('').reverse().join('');
    }

Both of these versions are equally correct. Let’s compare golf scores. Minified via [babel-minify](https://github.com/babel/minify), the first version looks like:


    function is_alphabetic_palindrome(a){const b=[];for(let d=0;d<a.length;d++){const[e,f]=[a.charAt(d),a.charCodeAt(d)];(65<=f&&90>=f||97<=f&&122>=f)&&b.push(e.toLowerCase())}const c=b.length;for(let d=0;d<=Math.floor(c/2)-1;d++)if(b[d]!==b[c-1-d])return!1;return!0}

With a length of **263 characters**. The golf score is **263**. ****

Minified, the second version looks like:


    function is_alphabetic_palindrome_v2(a){return a=a.toLowerCase().replace(/[a-z]/g,''),a===a.split('').reverse().join('')}

With a length of **121 characters**. The golf score is **121**. Well played! 🎯 

Metrics like `golf score`, if done well, could help students learn not just how to find correct ways, but also how to find simple ways to solve their problems. 
  

## Bonus features
- Cheat detection. Writing a good plagiarism detector is hard! We would want something that automatically diffs between each pair of student submissions, look for smallest diff, and figures out if one student is consistently copying from the other. 
- Automatic grading for student-submitted unit tests. Automatically grading a problem submission is easy — just run some unit tests. But if we’re asking students to submit their own unit tests as part of their work, how might that be automatically graded? Two ideas, neither of which are mutually exclusive: 
  - [simpler] use a code coverage library (eg. [Istanbul](http://gotwarlost.github.io/istanbul/) or [Blanket.js](http://blanketjs.org/)). Do the submitted unit tests “cover” the code being tested? What percent of coverage? 
  - [more complex, but interesting] Run the code against every other students’ code submission. Count test failures. The best tests will uncover the most real bugs! 


## Coding problems

These are just some ideas on where to find good practice problems. There are many other great sources out there. 

- [toy-problems](https://github.com/monicagrandy/Toy-Problems)
- [exercism.io](http://exercism.io/)
- [free code camp](https://www.freecodecamp.org/)
- [learn python the hard way](https://learncodethehardway.org/python/) (adapting problems to javascript)
- [khan academy](https://www.khanacademy.org/computing/computer-programming)



