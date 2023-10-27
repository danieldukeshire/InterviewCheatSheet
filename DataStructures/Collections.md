# Collections

## Lists
* Lists are implemented as dynamic arrays under the hood.

	```python
	hobbies = ['soccer', 'hockey', 'basketball']
	```
* A list can hold many values under a single name, you can access the values by referring to an index number.
* Lists are ordered.
* Lists are mutable - add/remove items after a list's creation.
* Lists can have different types.
* The largest costs come from growing pbeyond the current allocation size (because everything must move).
* If you need to add/remove at both ends, consider using a `collections.deque` instead. 

### Operations
| Operation 			| Average Cost 	| Worst Case 	| Syntax 										|
| :---         			| :---    		| :---          | :---											|
| Copy			   		| O(n)			| O(n)  		| `copy_list = original_list.copy()`		 	|
| Append[1]         	| O(1)     		| O(1)  		| `original_list.append(item`	 				|
| Pop Last         		| O(1)      	| O(1) 			| `popped_element = original_list.pop()`		|
| Pop Itermediate[2]    | O(n)      	| O(n) 			| `popped_element = original_list.pop(index)`	|
| Insert         		| O(n)      	| O(n) 			| `original_list.insert(index, item)`		 	|
| Get Item         		| O(1)      	| O(1) 			| `item = original_list[index]`		 			|
| Set Item         		| O(1)      	| O(1) 			| `original_list[index] = new_item `		 	|
| Delete Item         	| O(n)      	| O(n) 			| `del original_list[index]`		 			|
| Iteration         	| O(n)      	| O(n) 			| `for item in original_list:`		 			|
| Get Slice        		| O(k)      	| O(k) 			| `sublist = original_list[start:end]`		 	|
| Del Slice         	| O(n)      	| O(n) 			| `del original_list[start:end]`		 		|
| Set Slice         	| O(k+n)      	| O(k+n) 		| `original_list[start:end] = new_sublist`		|
| Extend[1]         	| O(k)      	| O(k) 			| `original_list.extend(other_list)`		 	|
| Sort         			| O(nlogn)     	| O(nlogn) 		| `original_list.sort()`		 				|
| Multiply         		| O(nk)      	| O(nk) 		| `multiplied_list = original_list * n`		 	|
| x in s         		| O(n)      	| O() 			| `item_in_list = item in original_list`		|
| Min/Max         		| O(n)      	| O() 			| `min_element = min(original_list)`		 	|
| Get Length         	| O(1)      	| O(1) 			| `length = len(original_list)`		 	|

## Arrays (NumPy)
```python
import numpy as np
```
* Arrays need to be declared, as Lists do not.
* Arrays store data very compactly, and are more efficient for storing large amounts of data.
* In Python, NumPy is a widely used library for arrays. NumPy arrays are more efficient for numerical operations and often used for scientific computing.


## Linked List / Deque




## Array vs List vs Deque
* In Python, both arrays and lists can be used to store collections of elements, but there is a significant difference in their insertion complexity.
  * Lists (Python Lists):
    * Lists are implemented as dynamic arrays in Python.
    * Inserting an element at the end of a list takes, on average, constant time (O(1)).
    * However, inserting an element at the beginning or in the middle of a list involves shifting elements, which takes linear time (O(n)), where n is the number of elements.
    * Lists are not the most efficient data structure for frequent insertions at arbitrary positions, especially if the list is large.
  * Arrays (NumPy Arrays):  
    * In Python, NumPy is a widely used library for arrays. NumPy arrays are more efficient for numerical operations and often used for scientific computing.  
    * NumPy arrays are not part of the standard library; you need to install NumPy to use them.  
    * NumPy arrays are implemented in C and are more memory-efficient and faster for numerical operations.  
    * Inserting an element at the end of a NumPy array is typically efficient and can be close to O(1).  
    * Inserting elements at arbitrary positions or in the middle of a NumPy array is generally not efficient, and it involves copying and reallocating memory, which is also closer to O(n) complexity.

| Operation               | Python Lists  | NumPy Arrays  | Linked Lists (Deque) |
|-------------------------|---------------|---------------|-----------------------|
| Copy                    | O(n)          | -             | O(n)                   |
| Append (at the end)     | O(1)          | O(1) or O(n)* | O(1)                |
| Pop Last                | O(1)          | O(n)*         | O(1)                |
| Pop (at a position)     | O(n)          | O(n)          | O(1)                |
| Insert (at a position)  | O(n)          | O(n)          | O(1)                |
| Get Item                | O(1)          | O(1)          | O(1)                |
| Set Item                | O(1)          | O(1)          | O(1)                |
| Delete Item             | O(n)          | O(n)          | O(1)                |
| Iteration               | O(n)          | O(n)          | O(n)                |
| Get Slice               | O(k)          | O(k)          | -                |
| Del Slice               | O(n)          | O(n)          | -                |
| Set Slice               | O(k + n)      | O(k + n)      | -            |
| Extend                  | O(k)          | O(k)          | O(k)                |
| Sort                    | O(n * log(n)) | O(n * log(n)) | -                   |
| Multiply                | O(n)          | O(n)          | -                   |
| `x` in `s`              | O(n)          | -             | -                   |
| Min/Max                 | O(n)          | O(n)          | -                   |
| Get Length              | O(1)          | O(1)          | O(1)                |



## Hash Table / Dictionary

### Operations
| Operation 			| Average Cost 	| Worst Case 		|
| :---         			| :---    		| :---          	|
| k in d		   		| O(1)			| O(n)  			|
| Copy[3]         		| O(n)     		| O(n)  		   	|
| Get Item        		| O(1)      	| O(n) 				|
| Set Item[1]    		| O(1)      	| O(n) 				|
| Iteration        		| O(n)      	| O(n) 				|

## Set

## Stack

## Queue


## Heap


