```
1|931|Minimum Falling Path Sum
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

```
//2|799|champagne-tower
class Solution {

    public static void main(String[] args) {
        double value = new Solution().champagneTower(3, 100, 100);
        System.out.println(value);
    }

    public double champagneTower(int poured, int query_row, int query_glass) {
        double[][] dp = new double[101][101];
        dp[0][0] = poured;
        for (int i = 0; i <= query_row; i++) {
            for (int j = 0; j <= i; j++) {
                double q = (dp[i][j] - 1.0) / 2.0;
                if (q > 0) {
                    dp[i + 1][j] += q;
                    dp[i + 1][j + 1] += q;
                }
            }
        }
        return Math.min(1, dp[query_row][query_glass]);
    }
}
```
