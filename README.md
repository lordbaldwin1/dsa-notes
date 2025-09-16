# Date Structures & Algorithms

## Bubble Sort
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

## 