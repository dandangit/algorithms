# DFS & BFS
## 490. The Maze
There is a ball in a maze with empty spaces and walls. The ball can go through empty spaces by rolling up, down, left or right, but it won't stop rolling until hitting a wall. When the ball stops, it could choose the next direction.

Given the ball's start position, the destination and the maze, determine whether the ball could stop at the destination.

The maze is represented by a binary 2D array. 1 means the wall and 0 means the empty space. You may assume that the borders of the maze are all walls. The start and destination coordinates are represented by row and column indexes.

#### Solution
1. DFS
2. BFS
**Use Arrays.equals()**

DFS recursion <br>
Attempt: 1 <br>
~~~
public class Solution {
    public boolean hasPath(int[][] maze, int[] start, int[] destination) {
        if (maze == null || maze.length == 0 || maze[0].length == 0) return false;
        boolean[][] visited = new boolean[maze.length][maze[0].length];
        return dfs(maze, start, destination, visited);
    }

    private boolean dfs(int[][] maze, int[] start, int[] destination, boolean[][] visited) {
        // base case
        if (Arrays.equals(start, destination)) {
            return true;
        }

        int m = maze.length; // the height
        int n = maze[0].length; // the width

        visited[start[0]][start[1]] = true;
        int[][] dirs = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
        for (int[] dir : dirs) {
            int x = start[0];
            int y = start[1];
            while (x + dir[0] >= 0 && x + dir[0] < m && y + dir[1] >= 0 && y + dir[1] < n
                && maze[x + dir[0]][y + dir[1]] != 1) {
                x += dir[0];
                y += dir[1];
            }
            int[] point = {x, y};
            if (!visited[x][y] && dfs(maze, point, destination, visited)) return true;
        }

        return false;
    }
}
~~~

BFS queue <br>
~~~
public class Solution {
    public boolean hasPath(int[][] maze, int[] start, int[] destination) {
        if (maze == null || maze.length == 0 || maze[0].length == 0) return false;

        int m = maze.length;
        int n = maze[0].length;

        int[][] dirs = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};
        boolean[][] visited = new boolean[m][n];
        Queue<int[]> queue = new LinkedList<int[]>();
        queue.offer(start);
        visited[start[0]][start[1]] = true;

        while (!queue.isEmpty()) {
            int[] p = queue.poll();

            if (Arrays.equals(p, destination)) return true;

            for (int[] dir : dirs) {
                int x = p[0];
                int y = p[1];
                while (x + dir[0] >= 0 && x + dir[0] < m && y + dir[1] >= 0 && y + dir[1] < n
                    && maze[x + dir[0]][y + dir[1]] != 1) {
                    x += dir[0];
                    y += dir[1];
                }
                if (!visited[x][y]) {
                    int[] next = {x, y};
                    queue.offer(next);
                    visited[x][y] = true;
                }
            }
        }

        return false;
    }
}
~~~

## 505. The Maze II
There is a ball in a maze with empty spaces and walls. The ball can go through empty spaces by rolling up, down, left or right, but it won't stop rolling until hitting a wall. When the ball stops, it could choose the next direction.

Given the ball's start position, the destination and the maze, find the shortest distance for the ball to stop at the destination. The distance is defined by the number of empty spaces traveled by the ball from the start position (excluded) to the destination (included). If the ball cannot stop at the destination, return -1.

The maze is represented by a binary 2D array. 1 means the wall and 0 means the empty space. You may assume that the borders of the maze are all walls. The start and destination coordinates are represented by row and column indexes.

1. BFS use PriorityQueue
2. DFS

BFS <br>
Attempt: 5 (bug should return -1 if the ball cannot stop at the destination)
~~~
public class Solution {
    class Point {
        int x;
        int y;
        int d;

        Point(int x, int y, int d) {
            this.x = x;
            this.y = y;
            this.d = d;
        }
    }

    public int shortestDistance(int[][] maze, int[] start, int[] destination) {
        int m = maze.length;
        int n = maze[0].length;
        int[][] dirs = {{-1, 0}, {1, 0}, {0, 1}, {0, -1}};

        int[][] dist = new int[m][n];
        for (int i = 0; i < m; i++) {
            Arrays.fill(dist[i], -1);
        }

        PriorityQueue<Point> pq = new PriorityQueue<Point>(new Comparator<Point>() {
            public int compare(Point x, Point y) {
                return x.d - y.d;   
            }
        });
        pq.offer(new Point(start[0], start[1], 0));

        while (!pq.isEmpty()) {
            Point p = pq.poll();
            for (int[] dir : dirs) {
                int x = p.x;
                int y = p.y;
                int d = p.d;
                while (x + dir[0] >= 0 && x + dir[0] < m && y + dir[1] >= 0 && y + dir[1] < n
                    && maze[x + dir[0]][y + dir[1]] != 1) {
                        x += dir[0];
                        y += dir[1];
                        d++;
                }

                if (dist[x][y] == -1 || d < dist[x][y]) {
                    dist[x][y] = d;
                    pq.offer(new Point(x, y, d));
                }
            }
        }

        return dist[destination[0]][destination[1]];
    }
}
~~~

DFS slow <br>
~~~
public class Solution {

    public int shortestDistance(int[][] maze, int[] start, int[] destination) {
        int m = maze.length; // the height
        int n = maze[0].length; // the width
        int[][] dist = new int[m][n];
        for (int i = 0; i < m; i++) {
            Arrays.fill(dist[i], -1);
        }
        dist[start[0]][start[1]] = 0;
        dfs(maze, start, destination, dist);
        // return dist[m - 1][n -1]; // --!
        return dist[destination[0]][destination[1]];
    }

    private void dfs(int[][] maze, int[] start, int[] destination, int[][] dist) {
        // base case
        if (Arrays.equals(start, destination)) {
            return;
        }

        int m = maze.length; // the height
        int n = maze[0].length; // the width

        int[][] dirs = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
        for (int[] dir : dirs) {
            int x = start[0];
            int y = start[1];
            int d = dist[start[0]][start[1]];
            while (x + dir[0] >= 0 && x + dir[0] < m && y + dir[1] >= 0 && y + dir[1] < n
                && maze[x + dir[0]][y + dir[1]] != 1) {
                x += dir[0];
                y += dir[1];
                d++;
            }
            if (dist[x][y] == -1 || d < dist[x][y]) {
                dist[x][y] = d;
                int[] point = {x, y};
                dfs(maze, point, destination, dist);
            }
        }
    }
}
~~~

## 499. The Maze III (Hard)*
There is a ball in a maze with empty spaces and walls. The ball can go through empty spaces by rolling up (u), down (d), left (l) or right (r), but it won't stop rolling until hitting a wall. When the ball stops, it could choose the next direction. There is also a hole in this maze. The ball will drop into the hole if it rolls on to the hole.

Given the ball position, the hole position and the maze, find out how the ball could drop into the hole by moving the shortest distance. The distance is defined by the number of empty spaces traveled by the ball from the start position (excluded) to the hole (included). Output the moving directions by using 'u', 'd', 'l' and 'r'. Since there could be several different shortest ways, you should output the lexicographically smallest way. If the ball cannot reach the hole, output "impossible".

The maze is represented by a binary 2D array. 1 means the wall and 0 means the empty space. You may assume that the borders of the maze are all walls. The ball and the hole coordinates are represented by row and column indexes.

#### Solution
由于题目要求输出the lexicographically smallest way，所以在II的基础上修改Point类，实现compareTo函数，在比较两个Point大小的时候，同时对比distance和str两个属性。

**注意Java的一些常用语法，比如implements Comparable, public int compareTo()等**

~~~
public class Solution {
    class Point implements Comparable<Point> {
        int x;
        int y;
        int d;
        String str;

        Point(int x, int y, int d, String str) {
            this.x = x;
            this.y = y;
            this.d = d;
            this.str = str;
        }

        public int compareTo(Point p) {
            if (this.d == p.d) return this.str.compareTo(p.str);
            return this.d - p.d;
        }
    }

    public String findShortestWay(int[][] maze, int[] ball, int[] hole) {
        List<String> ans = new ArrayList<String>();

        int m = maze.length; // height
        int n = maze[0].length; //width

        Point[][] points = new Point[m][n];
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                points[i][j] = new Point(i, j, Integer.MAX_VALUE, "");
            }
        }
        points[ball[0]][ball[1]].d = 0;

        PriorityQueue<Point> pq = new PriorityQueue<Point>();
        pq.offer(new Point(ball[0], ball[1], 0, ""));

        int[][] dirs = {{1, 0}, {0, -1}, {0, 1}, {-1, 0}}; // down, left, right, up
        char[] paths = {'d', 'l', 'r', 'u'};
        while (!pq.isEmpty()) {
            Point p = pq.poll();

            for (int i = 0; i < dirs.length; i++) {
                int[] dir = dirs[i];
                char ch = paths[i];

                int x = p.x;
                int y = p.y;
                int d = p.d;
                StringBuilder sb = new StringBuilder(p.str);

                while (x + dir[0] >= 0 && x + dir[0] < m && y + dir[1] >= 0 && y + dir[1] < n
                    && maze[x + dir[0]][y + dir[1]] != 1 && !(x == hole[0] && y == hole[1])) {
                    x += dir[0];
                    y += dir[1];
                    d++;
                }

                sb.append(ch);
                Point next = new Point(x, y, d, sb.toString());
                if (points[x][y].compareTo(next) > 0) {
                    points[x][y] = next;
                    pq.offer(next);
                }
            }
        }

        return points[hole[0]][hole[1]].str.equals("") ? "impossible" : points[hole[0]][hole[1]].str;
    }
}
~~~
