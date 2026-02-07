# Container With Most Water

## Initial brute-force idea

Since the goal is to find the maximum possible area, my first approach was to
check all possible pairs of lines.

For each index i, I paired it with every index j > i and calculated the area
using the minimum of the two heights and the distance between them.
This guarantees the correct answer because all possible containers are evaluated.

## Why brute force is not efficient

This approach uses two nested loops.
For each element in the array, it scans all elements after it, which results in
O(n²) time complexity.

While the solution is correct, it performs a large amount of unnecessary work,
making it too slow for large input sizes.

## Why two pointers works here

The area formed by two lines depends on the distance between them and the smaller
of the two heights. Even if one side is very tall, the shorter side limits the
amount of water that can be contained.

Starting with one pointer at the beginning and one at the end gives the maximum
possible width. As the pointers move inward, the width always decreases, so the
only way to possibly increase the area is by finding a taller limiting height.

If the pointer at the larger height is moved, the limiting height does not improve
and the width becomes smaller, which cannot lead to a better result. Therefore,
moving the larger height is useless.

Only moving the pointer at the smaller height gives a chance to increase the
minimum height and potentially form a larger area. Since each pointer moves at
most once, this approach runs in O(n) time.

## Key condition or invariant

At any point, the area formed by two pointers is limited by the smaller height.
If the pointer at the larger height is moved, the width decreases while the
limiting height does not increase, so the area cannot improve.

Therefore, it is always safe to move the pointer at the smaller height, as this
is the only move that can potentially increase the area.


## Edge cases I had to be careful about

When both pointers have the same height, moving either pointer is safe since the
limiting height remains the same. In this case, moving both pointers still
preserves correctness.

Very small inputs do not cause issues because the problem guarantees at least
two lines, and the algorithm naturally handles them.

## Time and space reasoning

Each pointer starts at one end of the array and moves inward.
Since each pointer moves at most once per index, the total number of operations
is linear, resulting in O(n) time complexity.

The algorithm uses only a constant amount of extra space for variables, so the
space complexity is O(1).

## Brute-force solution (initial approach)

```cpp
class Solution {
public:
    int maxArea(vector<int>& height) {
        int maxHeight = 0;
        for (int i = 0; i < height.size(); i++) {
            for (int j = i + 1; j < height.size(); j++) {
                int length = min(height[i], height[j]);
                int breadth = j - i;
                maxHeight = max(maxHeight, length * breadth);
            }
        }
        return maxHeight;
    }
};

---

### 2️⃣ Optimized solution (two pointers — final)

class Solution {
public:
    int maxArea(vector<int>& height) {
        int maxHeight = 0;
        int left = 0;
        int right = height.size() - 1;

        while (left < right) {
            int length = min(height[left], height[right]);
            int breadth = right - left;
            maxHeight = max(maxHeight, length * breadth);

            if (height[left] < height[right]) {
                left++;
            } else if (height[left] > height[right]) {
                right--;
            } else {
                left++;
                right--;
            }
        }
        return maxHeight;
    }
};

