# Mage-Hithe-Videos-BE

The backend of the opensource project, "Mage Hithe Videos" youtube video downloader.

# Hacktoberfest 2022

import java.io.BufferedReader;
import java.io.FileOutputStream;
import java.io.FileReader;
import java.io.IOException;
import java.io.PrintStream;

/**
* Public class for Assignment#3
* Description: <Some details regarding the pipeline of the program ..>
* @author <Your Name, Student ID>
* @version <Current version>
*
*/
public class Assign3 {

/**
* Reads from text file and return strings as nodes of Binary Search tree structure
* @param inputFile - the File to read from
* @return BST - binary search tree structure
* @throws IOException
*/
public static BST<Student> readText(String inputFile) throws IOException {

BufferedReader br = null;

String operationCode; //Operation code: 1 character ('I' for insert, 'D' for delete)
String lastName; // Student last name: 25 characters (index starts from 8 and ends at 8 + 25)
String stdData; // other info of student excluding operation code (index starts from 1 and ends at the end)

BST<Student> BStree = new BST<Student>();

try {

br = new BufferedReader(new FileReader(inputFile));

String str; // temporary String to manipulate each line

while ((str = br.readLine()) != null) {

operationCode = str.substring(0, 1);
lastName = str.substring(8, 33);
stdData = str.substring(1);

if (operationCode.equals("I")) {

Student newStudent = new Student(lastName, stdData); // create a new Student object
BStree.insert(newStudent); // Insert the new Student as a node in BST

} else if (operationCode.equals("D")) {

// Delete the node

}
}
} catch (IOException err) {
System.err.println("An I/O error occurred.");
} finally {
if (br != null)
br.close();
else
System.err.println("BufferedReader could not be opened.");
}
return BStree;
}
/**
* Main function
*/
public static void main(String[] args) {

// Check for the invalid number of arguments
if (args.length != 3) {
//print out the "usage" message and abort
System.err.println("Usage: java Assign3 [inputfile].txt [outputfile1].txt [outputfile2].txt"); // you can add details in the usage message
System.exit(1); //EXIT_FAILURE
} else {

String input = args[0];
String output1 = args[1];
String output2 = args[2];

PrintStream printStream = null; // for writing into output files

try {

BST<Student> BStree = readText(input); //read from file

printStream = new PrintStream(System.out);

System.setOut(new PrintStream(new FileOutputStream(output1))); //direct to output1

//Call your method to print depth-first in-order traversal

System.setOut(printStream); // reset

System.setOut(new PrintStream(new FileOutputStream(output2))); //direct to output2

//Call your method to print Breadth-First-Search Traversal

System.setOut(printStream); // reset

} catch (Exception e) {
// Customize error-handling according to your wish ..
e.printStackTrace();
} finally {
if (printStream != null)
printStream.close();
else
System.err.println("PrintStream could not be created.");
}
}
}

}
/******** Student **********
* Student class to manage student records
*/
class Student implements Comparable<Student>{

private String lastName; // Student last name
private String stdData; // other info of student excluding operation code

// public constructor
public Student(String lastName, String stdData){

this.lastName = lastName;
this.stdData = stdData;
}

// setter-getters ..
public String getLastName() {
return lastName;
}

public void setLastName(String lastName) {
this.lastName = lastName;
}

public String getStdData() {
return stdData;
}

public void setStdData(String stdData) {
this.stdData = stdData;
}

/**
* compareTo according to student last name
* @param obj - the reference object with which to compare.
* @return - a negative integer, zero, or a positive integer according to student info
*/
public int compareTo(Student obj) {

if (lastName.equals(obj.lastName)) { // if the last names are same then check entire student info
return stdData.compareTo(obj.stdData);
} // otherwise, use last name as logical comparison
else
return lastName.compareTo(obj.lastName);
}

/**
* Returns the string - Student data
*/
public String toString() {
return stdData;
}

}

/******** BSTNode **********
* Node for Generic type binary search tree
* Adapted from Drozdek - Chapter: 6, page: 220
*/
class BSTNode<T> {
protected T key;
protected BSTNode<T> left, right;

public BSTNode() {
left = right = null;
}

public BSTNode(T el) {
this(el, null, null);
}

public BSTNode(T el, BSTNode<T> lt, BSTNode<T> rt) {
key = el;
left = lt;
right = rt;
}
}

/******** BST **********
* Generic type implementation of binary search tree
* Adapted from Drozdek - Chapter: 6, page: 220
*/
class BST<T extends Comparable<T>> {

protected BSTNode<T> root;

public BST() {
root = null;
}

/**
* Inserts new element into the BST
* Adapted from Drozdek - Chapter: 6, page: 240, Fig: 6.23
* @param el - new element to be added
*
*/
public void insert(T el) {
BSTNode<T> p = root;
BSTNode<T> prev = null;
while (p != null) { // find a place for inserting new node;
prev = p;
if ((el.compareTo(p.key)) > 0)
p = p.right;
else
p = p.left;
}
if (root == null) // tree is empty;
root = new BSTNode<T>(el);
else if ((el.compareTo(prev.key)) > 0)
prev.right = new BSTNode<T>(el);
else
prev.left = new BSTNode<T>(el);
}

}
