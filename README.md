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

Start with pivot at n-1, go through list 

```python
def quick_sort(nums, low, high):
    if low < high:
        p = partition(nums, low, high)
        quick_sort(nums, low, p - 1)
        quick_sort(nums, p + 1, high)


def partition(nums, low, high):
    pivot = nums[high]
    i = low - 1

    for j in range(low, high):
        if nums[j] < pivot:
            i += 1
            nums[i], nums[j] = nums[j], nums[i]
    nums[i + 1], nums[high] = nums[high], nums[i + 1]
    return i + 1
```

## Selection Sort (iterative)
Time complexity: O(n<sub>2</sub>)/
Space complexity: 0(1)

Loop through the entire list, have inner loop find index of smallest number, then swap that smallest number with i. Continue through entire list.

```python
def selection_sort(nums):
    for i in range(0, len(nums)):
        smallestIdx = i
        for j in range(i + 1, len(nums)):
            if nums[j] < nums[smallestIdx]:
                smallestIdx = j
        nums[i], nums[smallestIdx] = nums[smallestIdx], nums[i]
    return nums
```

## Binary Search Tree

Insertion is super easy:
- if no root, create or set val in below example
- if value exists, return
- traverse recursively until we find the place to insert and uhh... insert it!!!

Deletion is a little more
- basically, we want to recurse and "solve" the subtree of our current node
- if there's no root, we just return None, nothing to delete
- then we recurse to find the node to delete. So we save the "solved" subtrees, we need to save the result from recursing and return ourselves.
- after traversing, what cases do we need to check?
- if 1 child, just return the other up the call stack
- this can also handle if 0 children because we'll return self.left or self.right which will be None
- inorder successor: pointer to right subtree, iterate to left most child in that subtree
- set our value to the successor value, call delete on that subtree, save the "solved" subtree and return self
```python
class BSTNode:
    def delete(self, val):
        if self.val is None:
            return None

        if val < self.val:
            if self.left:
                self.left = self.left.delete(val)
            return self
        if val > self.val:
            if self.right:
                self.right = self.right.delete(val)
            return self

        # val == self.val
        if not self.left:
            return self.right
        if not self.right:
            return self.left
        #inorder successor
        succ = self.right
        while succ.left:
            succ = succ.left
        self.val = succ.val
        self.right = self.right.delete(succ.val)
        return self

    def __init__(self, val=None):
        self.left = None
        self.right = None
        self.val = val

    def insert(self, val):
        if not self.val:
            self.val = val
            return

        if self.val == val:
            return

        if val < self.val:
            if self.left:
                self.left.insert(val)
                return
            self.left = BSTNode(val)
            return

        if self.right:
            self.right.insert(val)
            return
        self.right = BSTNode(val)

    def get_min(self):
        current = self
        while current.left is not None:
            current = current.left
        return current.val

    def get_max(self):
        current = self
        while current.right is not None:
            current = current.right
        return current.val

    def exists(self, val):
        if self.val == val:
            return True

        res = False
        if val < self.val:
            if self.left:
                res = self.left.exists(val)
            return res
        else:
            if self.right:
                res = self.right.exists(val)
            return res

    def height(self):
        if self.val is None:
            return 0

        left = 0
        right = 0
        if self.left:
            left = self.left.height()
        if self.right:
            right = self.right.height()
        return max(right, left) + 1
```

## Red Black Trees
BSTs have an issue. Namely that if data is inserted in sorted order, lookups will be O(n) rather than O(logn). The time complexity is entirely dependent upon the depth.

Red Black Trees help keep the tree relatively balanced as it inserts and deletes nodes.
- Each node is either red or black
- The root is black
- All Nil leaf nodes are black
- If a node is red, then both its children are black
- All paths from a single node go through the same number of black nodes to reach any of its descendant NIL (black) nodes.

Rotations are O(1) operations

Left Rotation
- pivot node's initial parent becomes its left child
- pivot node's old left child becomes its initial parent's new right child

Left Rotation Process
- Start left rotation
- old left child replaced by parent
- old left child becomes new left child's new right child
