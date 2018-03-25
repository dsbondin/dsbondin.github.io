---
layout: post
title:      "How I Failed The Codility Test "
date:       2018-03-25 23:54:28 +0000
permalink:  how_i_failed_the_codility_test
---


Today I took a coding test at [codility.com](https://codility.com). For those of you who haven't heard about Codility it is a platform for employers that allows them to automatically evaluate potential employees. You get a few algorithmic problems that you need to solve on their IDE during a specified time limit. I got 2 problems and 95 minutes to solve them. 

The benefit of using such service is that everything is automatic, the employer doesn't need to spend its engineer's time evaluating your coding abilities, they can look briefly at your final code and the history (your typing is recorded). But as you might have guessed already this benefit is also a problem: the potential candidate might cheat and google this problem and you don't get to see their thought process.

I'm not going to share the problems here as they might still be used for other evaluations, but I can say that one problem was pretty simple, you just needed to manipulate arrays and strings to get the correct result. The other was a bit tough: I knew how to solve it easily by brute force with O(n2) time complexity but the requirement for a perfect solution was O(n) and honestly, I still don't know how to do it. 

You get the result almost instantly once you submit your solutions. To my surprise I totally failed the easy problem with 12% result. As for the second I knew I wouldn't get 100% and my score was 60%. As I later realized I could have done much better. 

So if you ever get a test at Codility these tips might be helpful. I'll go in the order of importance. 

1. Test your solution. Try to use all possible inputs to see if you have a bug. I had one in my first problem and because of it I got 12%. I was rushing to get to the second problem that I didn't care to do more test for the first one. Codility platform allows you to enter 10 test inputs at a time. That should be enought but if it's not, do the first 10 and try again with more. 

2. There are time and space complexity requirements at the end of the problem description. If your first solution doesn't as efficient, do it anyway. Your not-so-perfect solution is still better than nothing. Once you've finished testing and you still don't know a way, try to optimize your imperfect solution. Even though my second problem was solved with O(n2) time complexity it could still be optimized. For example, my function was going through an array of strings calling another function to count its unique character. Some of the strings had only 1 or 2 characters and that second function call could have been avoided with a simple `if` statement. 

Hope you find this helpful. I'm still upset, I could have gotten 80% instead of 36%. But it's still an experience, I know I'll do better next time. 

Happy coding!
