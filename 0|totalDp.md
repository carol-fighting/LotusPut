```
import java.util.Arrays;

class Solution {

    public static void main(String[] args) {
        int[][] A = {{1, 2, 3}, {4, 5, 6}, {7, 8, 9}};
        int value = new Solution().minFallingPathSum(A);
        System.out.println(value);

    }

    public int minFallingPathSum(int[][] A) {
        int n = A.length;
        int[][] dp = new int[n][n];

        for (int[] arr : dp) {
            Arrays.fill(arr, Integer.MAX_VALUE);
        }

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if (i == 0) {
                    dp[i][j] = A[i][j];
                    continue;
                }
                dp[i][j] = dp[i - 1][j];
                if (j - 1 >= 0) {
                    dp[i][j] = Math.min(dp[i][j], dp[i - 1][j - 1]);
                }
                if (j + 1 < A.length) {
                    dp[i][j] = Math.min(dp[i][j], dp[i - 1][j + 1]);
                }
                dp[i][j] += A[i][j];
            }
        }

        int minVal = Integer.MAX_VALUE;
        for (int curr : dp[n - 1]) {
            minVal = Math.min(minVal, curr);
        }

        return minVal;
    }
}
```
