# CS 1501 – Lab #2 (Binary Tree)

Due: Tuesday February 7th @ 11:59pm on Gradescope

Late submission deadline: ???

Submit your completed lab Github repository via Gradescope.

## Table of Contents

- [Overview](#overview)
- [@TODO #1 - Interactive SubTree Builder](####-@TODO-#1---Interactive-SubTree-Builder)
- [@TODO #2 & #3 - Read/Write Binary Tree File](####-@TODO-#2-&-#3---Read/Write-Binary-Tree-File)
- [Testing](#testing)
- [Important Notes](#important-notes)

## Overview

* __Purpose__:  In this lab you will complete a interactive binary tree
implementation in Java, supporting file IO and a command-line prompt for various
 binary tree operations.

A starter project has been provided in this repository. You will fill in the missing
code implementation of a binary tree inside the class file __`TreeFile.java`__.

There are a total of __three__ code implementation tasks that are denoted inside __`@TODO`__
objective comments.

The __`TreeFile`__ class provides a constructor that reads a numeric option from stdin
and executes the feature that matches the given menu option number. Our __`TreeFile`__
class will have a user menu prompt with 6 options to choose from. The menu is
provided upon calling the private __`menu()`__ method, which reads and returns user
input ranging from `1` to `6`, as shown below:

```java
	/**
	 * Displays the program menu and read user selection.
	 * Input values must be within the range of 1 ~ 6 (inclusive).
 	 * @return A numeric choice value
	 */
	private int menu () {
		System.out.println("*********************************");
		System.out.println("Welcome to CS 1501 Persistent Tree Program!");
		System.out.println("1. Read tree from a file");
		System.out.println("2. Display inorder traversal of the tree");
		System.out.println("3. Attach tree as left child to a new root");
		System.out.println("4. Attach tree as right child to a new root");
		System.out.println("5. Write tree to a file");
		System.out.println("6. Exit.");
		System.out.println("*********************************");
		System.out.print("Please choose a menu option (1-6): ");

		int choice = Integer.parseInt(inScan.nextLine());
		return choice;
	}
```

---

#### @TODO #1 - Interactive SubTree Builder

The menu prompt allows you to build an in-order traversing binary tree from scratch
using options `2.` `3.` and `4.`, of which you will have to complete the feature
for option `4.` by implementing the __`@TODO`__ objective inside the constructor code:

```java
	/* TreeFile.java */

	// Inside TreeFile() Constructor
	case 4:
	System.out.println("Enter new root's character data:");
	userInput = inScan.nextLine();
	/**
	 * @TODO #1
	 *
	 * Create a new node that has the existing tree as its right child
	 * and assign in to the private bTree property.
	 */
	break;
```

---

#### @TODO #2 & #3 - Read/Write Binary Tree File

In addition to the user interactive binary tree operations, __`TreeFile`__ also supports
loading in saved binary trees from file (option `1.`) and vice versa (option `5.`).
Reading and writing binary trees are provided via __`readTree()`__ and __`writeTree()`__ methods respectively,
and you must complete each feature by implementing the missing __`@TODO`__ objectives.


```java
	/* TreeFile.java */

	private BinaryNode<Character> readTree (Scanner file) throws IOException {
		BinaryNode<Character> result = null;

		if (file.hasNext()) {
			String line = file.nextLine();

			if (line.charAt(0) == 'I') { // Internal Node
				/**
				 * @TODO #2
				 *
				 * Replace null below with a new node and recursively call
				 * readTree() on its left and right children
				 */
				result = null;
			} else if (line.charAt(0) == 'L') { // Leaf Node
				result = new BinaryNode<>(line.charAt(2));
			} else { // NULL child
				result = null;
			}
		}
		return result;
	}
```

```java
	/* TreeFile.java */

	private void writeTree (FileWriter file, BinaryNode<Character> root) throws IOException {
		if (root != null) {
			if (root.left == null && root.right == null) { // Write Leaf Node
				/**
				 * @TODO #3
				 *
				 * Write the tree node text line for a Leaf Node and return.
				 * Ex) "L %root.data%"
				 */
			} else { // Write Internal Node
				file.write("I " + root.data + "\n");
			}
			writeTree(file, root.left);
			writeTree(file, root.right);
		} else {
			file.write("N " + "\n"); // Write NULL Child (as "N")
		}
	}
```

## Testing

Once all __`@TODO`__ objectives have been implemented, test the __`TreeFile`__
Java program after compiling our source code into bytecode class files.

```sh
$ cd #YOUR_LAB2_DIRECTORY
$ javac ./TreeFile.java
$ java TreeFile

*********************************
Welcome to CS 1501 Persistent Tree Program!
1. Read tree from a file
2. Display inorder traversal of the tree
3. Attach tree as left child to a new root
4. Attach tree as right child to a new root
5. Write tree to a file
6. Exit.
*********************************
Please choose a menu option (1-6):

```

From here you can test out all the features of our program, continuously returning
back to the menu prompt until you exit the program with option `6`.

Also included in this repository are three text files of a sample test run:

__`sample.txt`__: Initial tree file to read data from.

__`expectedoutput.txt`__: Shell output of the sample test run.

__`tree1.txt`__: Tree file written during sample test run.

You can follow the exact steps as the sample test run to see if
it produces the same __`tree1.txt`__ and output as shown in __`expectedoutput.txt`__.


## Important Notes

- A __`Scanner`__ object keeps track of its read line internally, so keep that in mind when using recursion.
- In __`writeTree()`__, although the order in which the data is written to the file may
	seem like we're writing a pre-order traversal binary tree, the program reads
	in tree information in the same order as written from the file and parses it
	recursively into a in-order binary tree as shown in __`readTree()`__.
