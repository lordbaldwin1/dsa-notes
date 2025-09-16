# Date Structures & Algorithms

## Bubble Sort (iterative)
Time complexity: O(n<sup>2</sup>)\
Space complexity: O(1)

Loop through entire input list, and sort each element with it's previous element. Keep looping until you get through the list without swapping

```python
def bubble_sort(nums):
  swapping = True
  
  while swapping:
    swapping = False
    for i in range(1, len(nums)):
      if nums[i] < nums[i-1]:
        temp = nums[i]
        nums[i] = nums[i-1]
        nums[i-1] = temp
        swapping = True
  
  return nums
```

## Merge Sort (recursive)
Time complexity: O(n * log(n))\
Space complexity: O(n)

### Pros
- Fast
- Stable: duplicate keys will be in same order in the sorted list
### Cons
- Memory usage
- Recursive

Recursively split array in half until it is a single element. Then call merge function on those split arrays. Merge will build a new sorted array from those arrays as it moves back up the call stack.

```python
def merge_sort(nums):
    if len(nums) < 2:
        return nums

    halfway = len(nums) // 2
    sorted_left = merge_sort(nums[halfway:])
    sorted_right = merge_sort(nums[:halfway])
    return merge(sorted_left, sorted_right)


def merge(first, second):
    final = []
    i, j = 0, 0

    while i < len(first) and j < len(second):
        if first[i] < second[j]:
            final.append(first[i])
            i += 1
        else:
            final.append(second[j])
            j += 1
    while i < len(first):
        final.append(first[i])
        i += 1
    while j < len(second):
        final.append(second[j])
        j += 1
    return final
```

## Insertion Sort (iterative)
Time complexity: O(n<sub>2</sub>)\
Space complexity: O(1)

### Pros
- Fast for small data sets/partially sorted data
- Constant memory
- Stable: doesn't change relative order of equal key elements
- Can sort a list as it receives it

Insertion sort is great for small lists or lists that are already almost sorted.

Loop through the entire list with i starting at i = 1. With j, loop backward through the list and swap while j > 0 and while the number at j-1 is greater than j.

Basically, loop through the entire array, then loop backwards with j and put the number at i into the correct location.

```python
def insertion_sort(nums):
    for i in range(len(nums)):
        j = i
        while j > 0 and nums[j-1] > nums[j]:
            nums[j], nums[j-1] = nums[j-1], nums[j]
            j -= 1
    return nums
```

## Quick Sort (recursive)
Time complexity: O(nlogn)\
Space complexity: O(1)

```python
def quick_sort(nums, low, high):
    if low < high:
        middle = partition(nums, low, high)
        quick_sort(nums, low, middle-1)
        quick_sort(nums, middle+1, high)


def partition(nums, low, high):
    pivot = nums[high]
    i = low - 1

    for j in range(low, high):
        if nums[j] < pivot:
            i += 1
            nums[i], nums[j] = nums[j], nums[i]
    nums[i+1], nums[high] = nums[high], nums[i+1]
    return i+1
```
