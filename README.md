# Walls and Gates

You are given a m x n 2D grid initialized with these three possible values.

-1 - A wall or an obstacle.
0 - A gate.
INF - Infinity means an empty room. We use the value 2^31 - 1 = 2147483647 to represent INF as you may assume that the distance to a gate is less than 2147483647.

Fill each empty room with the distance to its nearest gate. If it is impossible to reach a gate, it should be filled with INF.

```
For example, given the 2D grid:

INF  -1  0  INF
INF INF INF  -1
INF  -1 INF  -1
  0  -1 INF INF
  
After running your function, the 2D grid should be:
  3  -1   0   1
  2   2   1  -1
  1  -1   2  -1
  0  -1   3   4
  
```

## Solution 1 : BFS
```java
class Solution {
    public void wallsAndGates(int[][] rooms) {
        if(rooms == null || rooms.length == 0)
            return;
        int rows = rooms.length;
        int columns = rooms[0].length;
        
        Queue<int[]> q = new LinkedList<>();
        for(int i = 0; i < rows; i++) {
            for(int j = 0; j < columns; j++) {
                if(rooms[i][j] == 0) {
                    q.add(new int[]{i,j});
                }
            }
        }
        
        int step = 0;
        int[][] directions = {{0, 1}, {0, -1}, {-1, 0}, {1, 0}};
        while(!q.isEmpty()) {
            step++;
            int size = q.size();
            for(int i = 0; i < size; i++) {
                int[] pos = q.remove();
                for(int[] direction : directions) {
                    int x = pos[0] + direction[0];
                    int y = pos[1] + direction[1];
                    if(x < 0 || x >= rows || y < 0 || y >= columns || rooms[x][y] == -1)
                        continue;
                    if(rooms[x][y] == Integer.MAX_VALUE) {
                        q.add(new int[]{x,y});
                        rooms[x][y] = step;
                    }
                }
            }
        }
    }
}
```

### Approach
Iterate through the grid running a dfs on every single gate we encounter. During our dfs we stop if we have, gone out of the bounds of the grid, or if the cell we are currently on has a smaller distance than our current distance (this means there is another gate that is closer to it than the gate we are currently running a dfs on). Once we have finished iterating through the board we will have updated all reachable cells with their respective shortest distances to a gate.


### Implementation - DFS
```java
public static void wallsAndGates(int[][] rooms) {
		if (rooms == null || rooms.length == 0)
			return;

		int m = rooms.length;
		int n = rooms[0].length;

		for (int i = 0; i < m; i++) {
			for (int j = 0; j < n; j++) {
				if (rooms[i][j] == 0) {
					fill(rooms, i, j, 0);
				}
			}
		}
}

public static void fill(int[][] rooms, int i, int j, int distance) {
		int m = rooms.length;
		int n = rooms[0].length;

		if (i < 0 || i >= m || j < 0 || j >= n || rooms[i][j] < distance) {
			return;
		}

		rooms[i][j] = distance;

		fill(rooms, i - 1, j, distance + 1);
		fill(rooms, i, j + 1, distance + 1);
		fill(rooms, i + 1, j, distance + 1);
		fill(rooms, i, j - 1, distance + 1);
}
```
