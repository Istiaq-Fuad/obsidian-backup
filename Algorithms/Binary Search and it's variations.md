## Core binary search

We repeatedly **halve the search space** until we find the target or exhaust the array.
```python
def binary_search(nums, target):
    left, right = 0, len(nums) - 1
    while left <= right:
        mid = (left + right) // 2
        if nums[mid] == target:
            return mid
        elif nums[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    return -1
```


## Finding the first and last occurrence (for duplicates)

Imagine `nums = [1,2,2,2,3]`, and you want the **first 2** or **last 2**.  
In these problems, we don’t stop at `nums[mid] == target` — we **move toward boundaries**.

**for first occurrence:** when `nums[mid] >= target` we update the right pointer to `mid - 1` because we want to move to the left
```python
def first_occurrence(nums, target):
    left, right = 0, len(nums) - 1
    res = -1
    while left <= right:
        mid = (left + right) // 2
        if nums[mid] >= target:
            right = mid - 1
        else:
            left = mid + 1
        if nums[mid] == target:
            res = mid
    return res
```

**for last occurrence:** when `nums[mid] <= target` we update the left pointer to `mid + 1` because we want to move to the right
```python
def last_occurrence(nums, target):
    left, right = 0, len(nums) - 1
    res = -1
    while left <= right:
        mid = (left + right) // 2
        if nums[mid] <= target:
            left = mid + 1
        else:
            right = mid - 1
        if nums[mid] == target:
            res = mid
    return res
```

and in both cases we track the `mid` point when `nums[mid] == target`. But the loop isn't terminated as the same number can be found in both sides. 


## Search boundaries (lower/upper bound)

**`First_element ≥ target` (Lower Bound):**  For finding the lower bound we track the `mid` pointer when `nums[mid] >= target`. 
```python
def lower_bound(nums, target):
    left, right = 0, len(nums) - 1
    ans = len(nums)  # if not found, insert at end
    while left <= right:
        mid = (left + right) // 2
        if nums[mid] >= target:
            ans = mid
            right = mid - 1
        else:
            left = mid + 1
    return ans
```


**`First_element > target` (Upper bound):** For finding the upper bound we track the mid pointer when `nums[mid] > target`.
```python
def upper_bound(nums, target):
    left, right = 0, len(nums) - 1
    ans = len(nums)
    while left <= right:
        mid = (left + right) // 2
        if nums[mid] > target:
            ans = mid
            right = mid - 1
        else:
            left = mid + 1
    return ans
```



## Binary search on the answer

In this type of binary search problem there are no sorted array given, instead we have a `monotonic condition` (something that flips from false to true or vice versa) as a variable increases. So binary search is performed over all possible answers until a minimum or maximum number is found that satisfies the condition. Classic example: `Koko Eating Bananas`.


## Find the Minimum in a Rotated Sorted Array

Compare `nums[mid]` with `nums[right]` 
### Case 1 - `nums[mid] > nums[right]`
-  Rotation point is **to the right** of mid  
-  Because mid is bigger than the rightmost element → we’re in the "left" (big) part.
```python
[4, 5, 6, 1, 2, 3]
         ^
         
mid = 6
right = 3
6 > 3 # minimum is to the right

# So we do
left = mid + 1
```

### Case 2 — `nums[mid] < nums[right]`

- Rotation point is **left or at mid**.  
- Because mid is smaller than right → we’re in the right sorted segment.
```python
[4, 5, 6, 1, 2, 3]
            ^

mid = 1
right = 3
1 < 3 # minimum is left or mid itself

# So we do
right = mid
```

### Stop condition: `left == right`
When they meet → that position is the minimum.
```python
def find_min(nums):
    left, right = 0, len(nums) - 1
    
    while left < right:
        mid = (left + right) // 2
        
        if nums[mid] > nums[right]:
            # minimum is to the right
            left = mid + 1
        else:
            # minimum is at mid or left of mid
            right = mid
    
    return nums[left]  # or index left
```



## Find the Minimum in a Rotated Sorted Array (with duplicates)

If duplicates exist: 
- The logic `nums[mid] > nums[right]` still works  
- But when `nums[mid] == nums[right]`, you cannot tell which direction is sorted.

```python
def find_min_with_duplicates(nums):
    left, right = 0, len(nums) - 1

    while left < right:
        mid = (left + right) // 2

        # Case 1: mid element is greater than right element
        # minimum is to the RIGHT of mid
        if nums[mid] > nums[right]:
            left = mid + 1

        # Case 2: mid element is less than right element
        # minimum is at mid or to the LEFT of mid
        elif nums[mid] < nums[right]:
            right = mid

        # Case 3: nums[mid] == nums[right]
        # not sure where the min is → shrink right to skip duplicate
        else:
            right -= 1

    # left == right → the minimum
    return nums[left]
```



### Binary Search on **Continuous / Real Values**

This is used when the answer is **not an integer**, but a **real number** (float).
Examples include:

- computing square roots
- computing cube roots / k-th roots
- optimizing a function over a continuous interval
- physics/engineering problems
- `minimum time`/`minimum distance` when the function is continuous
- geometry problems (circle radius, rope cutting, etc.)

Here binary search works because the function is **monotonic**.

Even when the domain is real (infinite possibilities), the logic remains the same:  
- Repeatedly cut the search interval in half until it’s “small enough.”
- You stop when the interval size becomes smaller than a chosen precision `eps`.

**Example - 1:** Let’s compute the square root of a number **with high precision**.

**Step-1:** For √x -
- if x ≥ 1 → answer is between **1 and x**
- if 0 < x < 1 → answer is between **x and 1**

**Step - 2:** Check if mid² is too big or too small

**Step - 3:** Stop when interval is small enough

```python
def sqrt_real(x, eps=1e-7):
    if x < 0:
        raise ValueError("Negative number has no real sqrt")

    left, right = 0.0, max(1.0, x)

    while right - left > eps:
        mid = (left + right) / 2
        if mid * mid <= x:
            left = mid
        else:
            right = mid

    return (left + right) / 2
```

**Example -  2:** Given n ropes, cut them into at least k pieces with **maximum possible equal length**. Find that maximum length with precision 1e-6.

If we choose a candidate length `L`:
- Number of pieces = `sum(int(rope / L) for rope in ropes)`
- If total pieces ≥ k → length L is **possible**
- If total pieces < k → L is **too large**

```python
def max_rope_length(ropes, k, eps=1e-6):
    left, right = 0.0, max(ropes)  # possible range for L
    
    # helper function: can we cut at least k pieces of length L?
    def can_cut(L):
        pieces = 0
        for r in ropes:
            pieces += int(r // L)
        return pieces >= k

    # binary search on a real number
    while right - left > eps:
        mid = (left + right) / 2
        if can_cut(mid):
            left = mid     # try a larger length
        else:
            right = mid    # try a smaller length

    return left   # maximum length L
```

**Another version:** Given integer rope lengths, cut them into at least `k` pieces, each piece having an **integer** length `L`.

If we choose an integer `L`:
- If `pieces >= k` → L is possible  
- If `pieces < k` → L is too large

```python
def max_rope_length_int(ropes, k):
    left, right = 1, max(ropes)     # L must be at least 1
    
    def can_cut(L):
        return sum(r // L for r in ropes) >= k
    
    best = 0

    while left <= right:
        mid = (left + right) // 2
        if can_cut(mid):
            best = mid      # mid is valid → try to increase L
            left = mid + 1
        else:
            right = mid - 1

    return best
```