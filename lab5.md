### Part 1: Debugging Scenario

#### 1. Original Post

#### Grading Script or Test file issue? - Strange Test Results
by Anonymous Comp

Hey everyone, 

I am encountering an issue with my `grade.sh` file. I’ve tried to best follow the instructions from week 6’s lab, but I cannot seem to find what I might be missing. I’ve attached a screenshot to running bash for the sh file, and my `TestListExamples.java` with the provided git link to a correct version of `ListExamples.java`.

The script is telling me that the file does not exist, and it seems that I can’t figure out why? Any suggestions on how to fix this bug?

Thanks!

#### 2. Response from TA

Response from TA:

Hi there [Anonymous Comp],

Thanks for reaching out. To help you debug, let’s take a look at your `grade.sh` file line by line. It looks like you removed the previous directories, `student-submission` and `grading-area` before you started grading. This seems right if you are trying to grade an individual student once at a time. You then clone the git link and its contents into a `student-submission directory`. In the line after, it seems that you are checking for the `ListExamples.java` file, but where is that file exactly? (Hint: you should acknowledge which directory you are in and which directory you should check in). The following code should run as you expect based on the in-line comments provided, but you should still make sure it all follows the instructions given in week 6’s lab.

Regarding your tests, all of the ones that you have provided should pass against the correct code. If you still have questions, please don’t hesitate to ask. 

#### 3. Follow-up from Original Poster

Thanks for the prompt response. I instead provided a relative path to the `ListExamples.java` realizing that I had to check within the `student-submission` directory that I had created for the file. Here’s my revised code with the grade.sh and running it in the terminal. It seems that now the tests have passed for the correct implementation of the methods in `ListExamples.java`.

#### 4. Scenario Recap


### Part 2: Reflection

 
