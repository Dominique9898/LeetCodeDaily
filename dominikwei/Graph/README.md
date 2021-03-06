### 图的遍历

多叉树的遍历

```c++
/* 多叉树遍历框架 */
void traverse(TreeNode* root) {
    if (root == nullptr) return;

    for (TreeNode* child : root->children)
        traverse(child);
}

```

图的遍历中,可能包含环.需要利用visited数组辅助

```c++
vector<bool> visited

/* 图遍历框架 */
void traverse(Graph graph, int s) {
    if (visited[s]) return;
    // 经过节点 s
    visited[s] = true;
    for (int neighbor : graph->neighbors(s))
        traverse(graph, neighbor);
    // 离开节点 s
    visited[s] = false;   
}

```









### 并查集 (UnionFind)

并查集代码基本结构

```c++
class UnionFind {
  public:
  	vector<int> root;
  	UnionFind(int size){}
  	void union(int x, int y){}
  	boolean connected(int x, int y){}
}
```



##### 1. Quick Find

时间复杂度O(1).  a[i] = rootNode. 保存根节点. 因此需要在Union的时候对比根节点.

```c++
class UnionFind {
  public:
  	vector<int> root;
  public:
    root.resize(size, 0);
    UnionFind(int size) {
      for (int i = 0; i < size; i++) {
        root[i] = i;
      }
    }
  
    int find(int x) {
      return root[x];
    }
  
  	void union(int x, int y) {
      // 如果没有根节点,默认x为根节点
      int rootX = find(x);
      int rootY = find(y);
      if (rootX != root) {
        for (int i = 0; i < root.size();i++){
          if (root[i] == rootY) {
            root[i] = rootX;
          }
        }
      }
      bool connented(x, y) {
        return root[x] == root[y];
      }
}
```

##### 2. Quick Union

union(x, y)

a[y] = a[x] => parentNode

```c++
class UnionFind {
	public:
  	vector<int> root;
  public:
  	UnionFind(size) {
      root.resize(size, 0);
      for (int i = 0; i < size; i++) {
        root[i] = i;
      }
    }
  	
  	int find(x) {
      if (x == root[x]) return x;
      else return find(root[x]);
    }
  
  	int union(x, y) {
      int rootX = find(x);
      int rootY = find(y);
      if (rootX != rootY) {
        // 即x，y的父节点不相等的情况下，将y的父节点对应的根节点置为x的根节点值
        root[rootY] = rootX;
    }
     
     bool connented(x, y) {
        return root[x] == root[y];
     }
}
```

#### 3. 按秩合并优化

选用树层最高的为根节点.将低的树,合并到高的树之下. 对union() 优化.

```c++
int root[];
int rank[];

UnionFind(int size) {
    root = new int[size];
    rank = new int[size];
    for (int i = 0; i < size; i++) {
        root[i] = i;
        rank[i] = 1; 
    }
}
void union(int x, int y) {
        int rootX = find(x);
        int rootY = find(y);
        if (rootX != rootY) {
          // 比较秩
            if (rank[rootX] > rank[rootY]) {
                root[rootY] = rootX;
            } else if (rank[rootX] < rank[rootY]) {
                root[rootX] = rootY;
            } else {
                root[rootY] = rootX;
                rank[rootX] += 1;
            }
        }
    };

优化
====>
// 以x作为为节点
void union(int x, int y) {
        int rootX = find(x);
        int rootY = find(y);
        if (rootX != rootY) {
          // 比较秩
          if (rank[rootX] < rank[rootY) {
            swap(rootX, rootY);
          }
          root[rootY] = rootX;
          if (rank[rootX] == rank[rootY]) rank[rootX] += 1;
        }
    };
```



#### 4. 压缩路径

找到根节点之后，将所有遍历过的元素的父节点都改成根节点. 对find() 优化. 通过递归. 就root[x] = find(root[x])

```c++
    int find(int x) {
        if (x == root[x]) {
            return x;
        }
        return root[x] = find(root[x]); // 压缩路径
    }
```



### 图论的DFS和BFS算法

### 最小生成树

### 最短路径

### 拓扑排序
