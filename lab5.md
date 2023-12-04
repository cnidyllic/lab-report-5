### Part 1: Debugging Scenario

#### 1. Original Post

#### Grading Script or Test file issue? - Strange Test Results
by Anonymous Comp

Hey everyone, 

I am encountering an issue with my `grade.sh` file. I’ve tried to best follow the instructions from week 6’s lab, but I cannot seem to find what I might be missing. I’ve attached a screenshot to the contents of my `grade.sh` file and my `TestListExamples.java`. I also provided the output of running bash with the the provided git link to a correct version of `ListExamples.java` as an argument.

![image](https://github.com/cnidyllic/lab-report-5/assets/146884284/b036d959-3bac-4a25-ab20-f3ae56aad42f)

![image](https://github.com/cnidyllic/lab-report-5/assets/146884284/cceeceec-c9a3-475c-9225-c67e2a41d767)


The script is telling me that the file does not exist, and it seems that I can’t figure out why? Any suggestions on how to fix this bug?

Thanks!

#### 2. Response from TA

Response from TA:

Hi there [Anonymous Comp],

Thanks for reaching out. To help you debug, let’s take a look at your `grade.sh` file line by line. It looks like you removed the previous directories, `student-submission` and `grading-area` before you started grading. This seems right if you are trying to grade an individual student once at a time. You then clone the git link and its contents into a `student-submission directory`. In the if statement after, it seems that you are checking for the `ListExamples.java` file, but where is that file exactly? (Hint: you should acknowledge which directory you are in and which directory you should check in). Additionally, you should make sure that is changed whenever you are referring to the `ListExamples.java` file. The following code should run as you expect based on the in-line comments provided, but you should still make sure it all follows the instructions given in week 6’s lab.

Regarding your tests, all of the ones that you have provided should pass against the correct code. If you still have questions, please don’t hesitate to ask. 

#### 3. Follow-up from Original Poster

Thanks for the prompt response. I instead provided a relative path to the `ListExamples.java` realizing that I had to check within the `student-submission` directory that I had created for the file. Here’s my revised code with the `grade.sh` and running it in the terminal. It seems that now the tests have passed for the correct implementation of the methods in `ListExamples.java`.

![image](https://github.com/cnidyllic/lab-report-5/assets/146884284/e4617535-56f1-4d3a-bed9-5436a8532760)

![image](https://github.com/cnidyllic/lab-report-5/assets/146884284/4228111d-499f-4d23-a989-85599c586256)

#### 4. Scenario Recap
The file and directory structure is: 

![image](https://github.com/cnidyllic/lab-report-5/assets/146884284/27595abf-4af3-4bcc-bb11-56251ee2279a)

The code for the TestListExamples.java was:

```
import org.junit.*;
import static org.junit.Assert.*;
import java.util.Arrays;
import java.util.List;

class IsMoon implements StringChecker {
  public boolean checkString(String s) {
    return s.equalsIgnoreCase("moon");
  }
}


public class TestListExamples {

    @Test(timeout = 500)
    public void testMergeRightEnd() {
        List<String> left = Arrays.asList("a", "b", "c");
        List<String> right = Arrays.asList("a", "d");
        List<String> merged = ListExamples.merge(left, right);
        List<String> expected = Arrays.asList("a", "a", "b", "c", "d");
        assertEquals(expected, merged);
    }

    @Test
    public void FilterEmptyList() {
        List<String> emptyList = Arrays.asList();
        StringChecker alwaysTrue = s -> s.isEmpty(); 
        Assert.assertTrue("Empty list should return empty", ListExamples.filter(emptyList, alwaysTrue).isEmpty());
    }

    @Test
    public void FilterNoMatch() {
        List<String> list = Arrays.asList("banana", "cherry");
        StringChecker alwaysFalse = s -> s.isEmpty();
        Assert.assertTrue("No match should return empty", ListExamples.filter(list, alwaysFalse).isEmpty());
    }

    @Test
    public void FilterAllMatch() {
        List<String> list = Arrays.asList("apple", "apricot");
        StringChecker alwaysTrue = s -> !s.isEmpty();
        Assert.assertEquals("All match should return all", list, ListExamples.filter(list, alwaysTrue));
    }

    @Test
    public void FilterMixedMatch() {
        List<String> list = Arrays.asList("apple", "banana", "apricot", "cherry");
        StringChecker startsWithA = s -> s.startsWith("a");
        Assert.assertEquals("Mixed match should return matching", Arrays.asList("apple", "apricot"), ListExamples.filter(list, startsWithA));
    }

    @Test
    public void MergeEmptyLists() {
        List<String> emptyList = Arrays.asList();
        Assert.assertTrue("Merging two empty lists should return empty", ListExamples.merge(emptyList, emptyList).isEmpty());
    }

    @Test
    public void MergeEmptyAndNonEmptyList() {
        List<String> emptyList = Arrays.asList();
        List<String> nonEmptyList = Arrays.asList("apple", "banana");
        Assert.assertEquals("Merging empty with non-empty should return non-empty", nonEmptyList, ListExamples.merge(emptyList, nonEmptyList));
        Assert.assertEquals("Merging non-empty with empty should return non-empty", nonEmptyList, ListExamples.merge(nonEmptyList, emptyList));
    }

    @Test
    public void MergeSortedLists() {
        List<String> list1 = Arrays.asList("apple", "banana");
        List<String> list2 = Arrays.asList("apricot", "cherry");
        List<String> expectedResult = Arrays.asList("apple", "apricot", "banana", "cherry");
        Assert.assertEquals("Merging two sorted lists should return sorted result", expectedResult, ListExamples.merge(list1, list2));
    }

    @Test
    public void MergeListsWithDuplicates() {
        List<String> list1 = Arrays.asList("apple", "banana");
        List<String> list2 = Arrays.asList("apple", "banana", "cherry");
        List<String> expectedResult = Arrays.asList("apple", "apple", "banana", "banana", "cherry");
        Assert.assertEquals("Merging lists with duplicates should return correct result", expectedResult, ListExamples.merge(list1, list2));
    }

    @Test
    public void MergeDisjointLists() {
        List<String> list1 = Arrays.asList("apple", "banana");
        List<String> list2 = Arrays.asList("cherry", "date");
        List<String> expectedResult = Arrays.asList("apple", "banana", "cherry", "date");
        Assert.assertEquals("Merging disjoint lists should return correct result", expectedResult, ListExamples.merge(list1, list2));
    }
}

```

The code for the `grade.sh` file before fixing the bug was:

```
CPATH='.;lib/hamcrest-core-1.3.jar;lib/junit-4.13.2.jar'
EXPECTED_FILENAME="ListExamples.java"

# Cleanup before starting the grading

rm -rf student-submission
rm -rf grading-area

mkdir grading-area

git clone $1 student-submission
echo 'Finished cloning'

# Check if the expected file is present
if [ ! -f "$EXPECTED_FILENAME" ]; then
    echo "Error: Expected file $EXPECTED_FILENAME not found in the submission."
    exit 1
fi

# Copy the necessary files into the grading area
cp $EXPECTED_FILENAME grading-area
cp TestListExamples.java grading-area
cp -r lib grading-area

# Try to compile the student's submission and the test file. The previous code should exit code of 0 to continue

javac -cp $CPATH grading-area/$EXPECTED_FILENAME grading-area/TestListExamples.java
if [ $? -ne 0 ]; then
    echo "Compilation failed. Please check your code for errors."
    exit 1
fi

# Run the tests
cd grading-area
java -cp $CPATH org.junit.runner.JUnitCore TestListExamples &> results.txt
if [ $? -ne 0 ]; then
    echo "Tests failed. Please see the results below for more information."
    cat results.txt
    exit 1
fi

# If all tests pass, print success message
echo "All tests passed"

grep -E 'Tests run: [0-9]+, Failures: [0-9]+, Errors: [0-9]+, Skipped: [0-9]+' results.txt

rm -rf student-submission
rm -rf grading-area

```

I ran in the command line:

```
bash grade.sh https://github.com/ucsd-cse15l-f22/list-methods-corrected.git
```

Which produced a symptom: 

```
Cloning into 'student-submission'...
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
Receiving objects: 100% (3/3), done./2)
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 3 (delta 0), pack-reused 0
Finished cloning
Error: Expected file ListExamples.java not found in the submission.
```
The intended error message is not actually true because the file structure of the submission for the student's `ListExamples.java` file is correct, but we have cloned it in a different directory, which leads us to not checking for the file properly. Instead we should change the instancesof `$EXPECTED_FILENAME` in the lines

```
if [ ! -f "$EXPECTED_FILENAME" ]; then
```
and 
```
cp $EXPECTED_FILENAME grading-area
```
to `student-submission/$EXPECTED_FILENAME` because this correctly checks for the existence of the java file in the directory we had clone it into initially.


### Part 2: Reflection

Throughout the second half of this quarter, I hardly new what different commands like `grep` and `find` did, not to mention the various options there were that allowed us to expand the uses of the commands even further. I also learned how to write if-else code blocks, which were not as I learned in a typical java file. This helped me realize the various ways to write code and the languages that programming ultimately has.

 
