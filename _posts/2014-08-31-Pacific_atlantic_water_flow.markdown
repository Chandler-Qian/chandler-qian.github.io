---
layout: post
title:  "Pacific Atlantic Water Flow"
date:   2020-01-07 14:36:23
categories: problems
---
<span class="image featured"><img src="/images/atlantic_ocean.png" alt=""></span>
This is a [leetcode problem](https://leetcode.com/problems/pacific-atlantic-water-flow/).

A better displayed version can be found [here](https://github.com/Chandler-Qian/chandler-qian.github.io/blob/master/_posts/2014-08-31-Pacific_atlantic_water_flow.markdown).

## Problem Discription


Given an m x n matrix of non-negative integers representing the height of each unit cell in a continent, the "Pacific ocean" touches the left and top edges of the matrix and the "Atlantic ocean" touches the right and bottom edges.

Water can only flow in four directions (up, down, left, or right) from a cell to another one with height equal or lower.

Find the list of grid coordinates where water can flow to both the Pacific and Atlantic ocean.

Note: The order of returned grid coordinates does not matter. Both m and n are less than 150.

## My Solution
View my posted solution [here]([https://leetcode.com/problems/pacific-atlantic-water-flow/discuss/510000/java-dfs-simple-solution](https://leetcode.com/problems/pacific-atlantic-water-flow/discuss/510000/java-dfs-simple-solution)).

```
class Solution {
    private boolean[][] atlantic, pacific, visited;
    private int M, N;
    public List<List<Integer>> pacificAtlantic(int[][] matrix) {
        List<List<Integer>> res = new ArrayList<>();
        M = matrix.length;
        if (M == 0) return res;
        N = matrix[0].length;
        atlantic = new boolean[M][N];
        pacific = new boolean[M][N];
        visited = new boolean[M][N];
        dfs(pacific, matrix, 0, 0, 0, 0);
        visited = new boolean[M][N];
        dfs(atlantic, matrix, M - 1, N - 1, M - 1, N - 1);
        for (int i = 0; i < M; i++) {
            for (int j = 0; j < N; j++) {
                if (atlantic[i][j] && pacific[i][j]) res.add(Arrays.asList(new Integer[]{i, j}));
            }
        }
        return res;
    }
    
    private void dfs(boolean[][] ocean, int[][] matrix, int i, int j, int i_border, int j_border) {
        ocean[i][j] = true;
        visited[i][j] = true;
        if (i - 1 >= 0 && !visited[i - 1][j] && (i - 1 == i_border || j == j_border || matrix[i - 1][j] >= matrix[i][j]))
            dfs(ocean, matrix, i - 1, j, i_border, j_border);
        if (j - 1 >= 0 && !visited[i][j - 1] && (j - 1 == j_border || i == i_border || matrix[i][j - 1] >= matrix[i][j]))
            dfs(ocean, matrix, i, j - 1, i_border, j_border);
        if (i + 1 < M && !visited[i + 1][j] && (j == j_border || i + 1 == i_border || matrix[i + 1][j] >= matrix[i][j]))
            dfs(ocean, matrix, i + 1, j, i_border, j_border);
        if (j + 1 < N && !visited[i][j + 1] && (i == i_border || j + 1 == j_border || matrix[i][j + 1] >= matrix[i][j]))
            dfs(ocean, matrix, i, j + 1, i_border, j_border);
    }
    
}
```
 