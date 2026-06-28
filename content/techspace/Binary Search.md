
## What is Binary Search?

Binary search finds a target value in a sorted array by repeatedly halving the search space.

## Time Complexity

- Best: O(1)
- Average: O(log n)
- Worst: O(log n)

## Code

```
func BinarySearch(nums []int, target int) int {
    left, right := 0, len(nums)-1
    for left <= right {
        mid := left + (right-left)/2
        if nums[mid] == target {
            return mid
        } else if nums[mid] < target {
            left = mid + 1
        } else {
            right = mid - 1
        }
    }
    return -1
}
```

