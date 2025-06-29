# Leetcode---1498
Number of Subsequences That Satisfy the Given Sum Condition
//code in java 
import java.util.Arrays;

class Solution {
    public int numSubseq(int[] nums, int target) {
        final int kMod = 1_000_000_007; // Modulo constant
        final int n = nums.length;
        int ans = 0;

        // Precompute powers of 2 modulo kMod
        // pows[i] will store 2^i % kMod
        int[] pows = new int[n];
        pows[0] = 1;
        for (int i = 1; i < n; ++i) {
            pows[i] = (pows[i - 1] * 2) % kMod;
        }

        // Sort the array to efficiently find min and max elements for subsequences
        Arrays.sort(nums);

        // Use a two-pointer approach
        int left = 0;
        int right = n - 1;

        while (left <= right) {
            // If the sum of the current minimum (nums[left]) and maximum (nums[right])
            // is less than or equal to the target, then all subsequences formed
            // by nums[left] and any element between left and right (inclusive of nums[right])
            // will satisfy the condition with nums[left] as the minimum and
            // some element in the range [left, right] as the maximum.
            // The number of such subsequences is 2^(right - left).
            if (nums[left] + nums[right] <= target) {
                ans = (ans + pows[right - left]) % kMod;
                left++; // Move left pointer to consider a new minimum
            } else {
                right--; // Move right pointer to find a smaller maximum
            }
        }

        return ans;
    }
}
