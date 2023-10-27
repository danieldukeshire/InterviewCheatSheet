# Sorting

## Bubble Sort

## Merge Sort
* Uses a "divide and conquer" approach\
  1). Repeatedly divide the input into smaller subarrays, until a base case of a single element is reached (this single element subarray is considered sorted)

  2). Repeatedly merge the smaller sorted subarrays into bigger sorted subarrays, until the entire input is merged back together
* Runtime complexity: O(N log N)
* Space complexity: O(N log N)

```python
	def mergeSort(arr):
		if len(arr) > 1:
			
			# Find the mid of the array
			mid = len(arr) // 2

			# Divide the array elements into left and right
			L = arr[:mid]
			R = arr[mid:]

			# Sort left half
			mergeSort(L)

			# Sort right half
			mergeSort(R)

			i = j = k = 0	

			# Begin sorting by comparing left and right halves
			while i < len(L) and j < len(R):
				if L[i] <= R[j]:
					arr[k] = L[i]
					i += 1
				else:
					arr[k] = R[j]
					j += 1
				k += 1
			
			# Check for leftover elements
			while i < len(L):
				arr[k] = L[i]
				i += 1
				k += 1

			while j < len(R):
				arr[k] = R[j]
				j += 1
				k += 1
```

## Quick Sort
* Selects an arbitrary pivot element and puts all the elements smaller than the pivot to the left of the pivot, and large elements to the right of the pivot
* Exchanging of elements occurs through swaps
* The process of selecting a pivot and swapping the left and right elements is recursive and continues until the base case is hit: when just two elements are swapped into their correct relative order
* Worst-case runtime: O(n<sup>2</sup>)
* Expected runtime: O(N log N)

```python
	# Helper function to partition the array
	def helper(a, low, high):
		# Choose rightmore element as pivot
		pivot = a[high]

		# Pointer for greater element
		i = low - 1

		# Traverse through elements and compare with the pivot
		for j in range(low, high):

			# If the element smaller than pivot if found, swap it with the greater element
			if a[j] <= pivot:
				i = i + 1

				# Swapping elements
				(a[i], a[j]) = (a[j], a[i])
		
		# Swap the pivot element with the greater element 
		(a[i+1], a[high]) = (a[high], a[i+1])

		# Return position from where the partition is done
		return i + 1
	
	# Function to perform quicksort
	def quicksort(a, low, high):
		if len(a) == 1:
			return a

		if low < high:

			# Find the pivot such that elements smaller than pivot are on the left and elements greater are on the right
			pivot = helper(a, low, high)

			# Recurse left of pivot
			quicksort(a, low, pivot - 1)

			# Recurse right of pivot
			quicksort(a, pivot + 1, high)

		return a
```

