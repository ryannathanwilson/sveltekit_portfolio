---
title: "DAILY CODING PROBLEM: #315"
date: "2021-08-15"
description: "Problem: 
This problem was asked by Google.
In linear algebra, a Toeplitz matrix is one in which the elements on any given diagonal from top left to bottom right are identical.
Here is an example:"
categories:
    - Python
    - Daily coding problem
---
### Problem: 

"This problem was asked by Google.

In linear algebra, a Toeplitz matrix is one in which the elements on any given diagonal from top left to bottom right are identical.

Here is an example:

```
1 2 3 4 8
5 1 2 3 4
4 5 1 2 3
7 4 5 1 2
```
Write a program to determine whether a given input is a Toeplitz matrix."

### SOLUTION:

Firstly I'll check that we have a valid matrix represented as a list where each item reprents a row. Each row is also a list (which I'll refer to as a sub-list since we'll only have these two tiers in this problem). If every sub-list must have the same number of items. I'll set up a few matrixes to test. For each I'll check the length of the first row, and then check that all other rows are the same length.

```python
def isItValid(matrix):
    lengthOfRows = len(matrix[0])
    numberOfRows = len(matrix)
    for i in range(1, numberOfRows):
        if len(matrix[i]) != lengthOfRows:
            return False
    return True

def main():
    validMatrix = [
        [1, 2, 3],
        [1, 2, 3],
        [1, 2, 3]
    ]
    invalidMatrix = [
        [1, 2, 3],
        [1, 2, 3],
        [1, 2, 3, 4]
    ]
    toeplitzMatrix = [
        [1, 2, 3, 4, 8],
        [5, 1, 2, 3, 4],
        [4, 5, 1, 2, 3],
        [7, 4, 5, 1, 2]
    ]
    print(isItValid(validMatrix))
    print(isItValid(invalidMatrix))
    print(isItValid(toeplitzMatrix))

if __name__ == "__main__":
    main()
```

This python script when run returns:
```
True
False
True
```
The first matrix is valid because it is 3x3. The second matrix is invalid because it the firstg two rows are of length 3 while the third row is of length 4. The last matrix is the example matrix, which is a valid matrix of form 4x5.

Now to the heart of the problem. There are two ways at least that we could iterate over this problem. We could look at every item in the matrix and compare it against the value diagnally down and to the right of it and check that they match. Alternatively, we could start with each number in the top row, and the first item in every other row and compare them against their entire diaganol. Both solutions are big-O of n generally, but the first solution is technically closer to 2n because most numbers have to be accessed twice, comparing it against the item diaganol up and left of it, and diaganol down and right of it. We'll start at the edges and compare once all the way across so every item is accessed only once.

Here is my solution using this method. Note when printing errors I refer to coordinates where the top left of the matrix is coordinate (1,1). The first number references the column, increasing as you go across the row, and the second number increases as you go down a column.

```python
def isItToeplitz(matrix):
    # This is our previously named isItValid function
    lengthOfRows = len(matrix[0])
    numberOfRows = len(matrix)
    for i in range(1, numberOfRows):
        if len(matrix[i]) != lengthOfRows:
            print("No, it's not a Toeplitz matrix. It's not even a valid matrix")
            return False

    # Iterate across the first row. We don't need to check the last item
    for i in range(0, lengthOfRows - 2):
        referenceNumber = matrix[0][i]
        for j in range(1, numberOfRows - 1):
            if referenceNumber != matrix[j][i + j]:
                print(
                    f"matrix coordinate 1,{i+1} does not match coordinate {j+1},{i+2}.")
                return False

    # Iterate down the first column, starting at the second row. We don't need to check the bottom row.
    for i in range(1, numberOfRows - 1):
        referenceNumber = matrix[i][0]
        for j in range(1, numberOfRows - i):
            if referenceNumber != matrix[i + j][j]:
                print(
                    f"matrix coordinate {i + 1},1 does not match coordinate {i + j + 1},{j + 1}.")
                return False

    print("It's a toeplitz matrix")
    return True


def main():
    validMatrix = [
        [1, 2, 3],
        [1, 2, 3],
        [1, 2, 3]
    ]
    invalidMatrix = [
        [1, 2, 3],
        [1, 2, 3],
        [1, 2, 3, 4]
    ]
    toeplitzMatrix = [
        [1, 2, 3, 4, 8],
        [5, 1, 2, 3, 4],
        [4, 5, 1, 2, 3],
        [7, 4, 5, 1, 2]
    ]
    isItToeplitz(validMatrix)
    isItToeplitz(invalidMatrix)
    isItToeplitz(toeplitzMatrix)


if __name__ == "__main__":
    main()
```

As expected this returns the following:

matrix coordinate 1,1 does not match coordinate 2,2.
No, it's not a Toeplitz matrix. It's not even a valid matrix
It's a toeplitz matrix
