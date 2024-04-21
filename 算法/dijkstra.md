# 最短路径（从源点）

```java
// 100276. 最短路径中的边
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.PriorityQueue;

class Solution {

    public boolean[] findAnswer(int n, int[][] edges) {
        List<int[]>[] g = new ArrayList[n];

        Arrays.setAll(g, i -> new ArrayList<>());

        for (int i = 0; i < edges.length; i++) {
            int[] e = edges[i];
            int x = e[0], y = e[1], w = e[2];
            g[x].add(new int[]{y, w, i});
            g[y].add(new int[]{x, w, i});
        }

        long[] dis = new long[n];

        Arrays.fill(dis, Integer.MAX_VALUE);
        dis[0] = 0;

        PriorityQueue<long[]> pq = new PriorityQueue<>((a, b) -> Long.compare(a[0], b[0])); // 保存最短的dis
        pq.offer(new long[]{0, 0});

        while (!pq.isEmpty()) {
            long[] dxPair = pq.poll();
            long dx = dxPair[0];
            int x = (int) dxPair[1];
            if (dx > dis[x]) { //已经被更新了 这条路径更远已经没用了
                continue;
            }
            for (int[] t : g[x]) {
                int y = t[0];
                int w = t[1];
                long newDis = dx + w;
                if (newDis < dis[y]) {
                    dis[y] = newDis;
                    pq.offer(new long[]{newDis, y});
                }
            }
        }

        boolean[] ans = new boolean[edges.length];
        if (dis[n - 1] == Integer.MAX_VALUE) {
            return ans;
        }

        boolean[] vis = new boolean[n];
        dfs(n - 1, g, dis, ans, vis);
        return ans;
    }

    private void dfs(int y, List<int[]>[] g, long[] dis, boolean[] ans, boolean[] vis) {
        vis[y] = true;
        for (int[] t : g[y]) {
            int x = t[0];
            int w = t[1];
            int indexEdge = t[2];
            if (dis[x] + w != dis[y]) continue;
            ans[indexEdge] = true;
            if (!vis[x]) {
                dfs(x, g, dis, ans, vis);
            }
        }
    }
}
```
