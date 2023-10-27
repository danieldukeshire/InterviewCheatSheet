# Sorting

## Bubble Sort

## Merge Sort
* Uses a "divide and conquer" approach
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

