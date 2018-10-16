图
<!--TOC-->
- [概念](#概念)
- [python实现](#python实现)
- [遍历](#遍历)
- [其他动作](#其他动作)
<!--/TOC-->
## 概念
- 有向图：图中的每条边都有方向的图叫有向图。此时，边的两个顶点有次序关系，有向边 < u,v>成为从顶点u到顶点v的一条弧，u成为弧尾（始点），v成为弧头（终点），即有向图中弧 < u,v>和弧 < v,u> 表示不同的两条边。

- 无向图：图中的每条边没有方向的图。边的两个顶点没有次序关系，无向图用边(u,v)表示对称弧< u,v>和< v,u>。

- 权：图中的边或弧上有附加的数量信息，这种可反映边或弧的某种特征的数据成为权。

- 网：图上的边或弧带权则称为网。可分为有向网和无向网。

- 邻接和关联：若边e=(u,v)或弧e= < u,v>，则称点u和v互为邻接顶点，并称边e或弧e关联于顶点u和v。

- 度：在无向图中，与顶点v关联的边的条数成为顶点v的度。有向图中，则以顶点v为弧尾的弧的条数成为顶点v的出度，以顶点v为弧头的弧的条数成为顶点v的入度，而顶点v的度=出度+入度。图中各点度数之和是边（或弧）的条数的2倍。

- 圈：图中联接同一个顶点的边叫圈。

- 平行边：图中两个顶点之间若有两条或两条以上的边，称这些边为平行边。

- 简单图：没有圈也没有平行边的图。

- 有向完全图：有n个顶点，n(n-1)条弧的有向图。每两个顶点之间都有两条方向相反的边连接的图。

- 完全图：有n个顶点，n(n-1)/2条边的无向图。若一个图的每一对不同顶点恰有一条边相连，则称为完全图。完全图是每对顶点之间都恰连有一条边的简单图。

- 路径长度：路径上边或弧的数目。若路径上的各顶点均不相同，则称这条路经为简单路经（或路），除第一个和最后一个顶点相同外，其他各顶点均不相同的路径成为回路（或环）。

- 连通图：在无向图G中，对与图中的任意两个顶点u、v都是连通的，则称图G为连通图。

- 强连通图：在有向图G中，如果对于每一对Vi和Vj 属于顶点集V，Vi不等于Vj ，从Vi到Vj和从Vj到Vi都存在路径，则称G是强连通图。

- 强连通分量：有向图中的极大强连通子图称做有向图的强连通分量。

- 生成树：一个连通图的生成树是一个极小的连通子图，它含有图中全部的n个顶点，但只有足以构成一棵树的n-1条边。

- 有向树：如果一个有向图恰有一个顶点的入度为0，其余顶点的入度为1，则是一棵有向树。

## python 实现
  图的存储结构，常用的是”邻接矩阵”和”邻接表”。
  通常采用两个数组来实现邻接矩阵：一个一维数组用来保存顶点信息，一个二维数组来用保存边的信息。
邻接矩阵的缺点就是比较耗费空间。
    邻接表是图的一种链式存储表示方法。它是改进后的”邻接矩阵”，它的缺点是不方便判断两个顶点之间是否有边，但是相对邻接矩阵来说更省空间。
    
   * **在Python中，图主要是通过列表和词典来构造。**
   实现的功能：
   - 寻找一条路径
   - 查找所有的路径
   - 查找最短路径
```
ADT Graph: 
    Graph(self) #图的创建 
    is_empty(self) #空图判断 
    vertex_num(self) #返回顶点个数 
    edge_num(self) #返回边的个数 
    vertices(self) #获得图中顶点的集合 
    edges(self) #获得图中边的集合 
    add_vertex(self,vertex) #增加一个顶点 
    add_edge(self,v1,v2) #在v1，v2间加边 
    get_edge(self,v1,v2) #获得边的有关信息 
    out_edges(self,v) #获得v的所有出边 
    degree(self,v) #检查v的度
```
** 具体实现 **
```
#采用邻接矩阵实现
class Graph:
    def __init__(self, mat, unconn = 0): # 初始化
        vnum = len(mat)
        for x in mat:
            if len(x) != vnum:
                raise ValueError("Argument for 'Graph'.")
        self._mat = [mat[i][:] for i in range(vnum)]   #使用拷贝的数据
        self._unonn = unconn
        self._vnum = vnum
    def vertex_num(self):              # 返回节点数目
        return self._vnum
    def _invalid(self,v):              # 检验输入的节点是否合法
        return v > 0 or v >= self._vnum
    def add_adge(self,vi,vj,val=1):    # 增加边
        if self._invalid(vi) or self._invalid(vj):
            raise GraphError(str(vi) + ' or' + str(vj) + 'is not a valid vertex.'
        self._mat[vi][vj] = val
    def get_adge(self,vi,vj):          # 得到边的信息
        if self._invalid(vi) or self._invalid(vj):
            raise GraphError(str(vi) + ' or' + str(vj) + 'is not a valid vertex.')
        return self._mat[vi][vj]
    def out_edges(self,vi): #得到vi出发的所有边 
        if self._invalid(vi): 
            raise GraphError(str(vi)+' is not a valid vertex.') 
        return self._out_edges(self._mat[vi],self._unconn)
    
    @staticmethod
    def _out_edges(row, unconn)： # 辅助函数
        edges = []
        for i in range(len(row)):
            if row[i] != unconn:
                edges.append((i,row[i]))
        return edges
        
    
            
                
#采用邻接表实现，需要重写一些方法，但功能相同 
class GraphAL(Graph): #继承于Graph 
    def __init__(self,mat=[],unconn=0): 
        vnum = len(mat) 
        for x in mat: 
            if len(x) !=vnum: 
                raise ValueError("Argument for 'Graph'.") 
        self._mat = [Graph._out_edges(mat[i],unconn) for i in range(vnum)] 
        self._vnum = vnum 
        self._unconn = unconn 
    def add_edge(self,vi,vj,val = 1): 
        if self._vnum == 0: 
            raise GraphError('Cannot add edge to empty graph.') 
        if self._invalid(vi) or self._invalid(vj): 
            raise GraphError(str(vi) + ' or' + str(vj) + ' is not valid vertex.') 
        row = self._mat[vi] 
        i = 0 
        while i < len(row): 
            if row[i][0] == vj: 
                self._mat[vi][i] = (vj,val) 
                return 
            if row[i][0] > vj: 
                break 
            i += 1 
        self._mat[vi].insert(i,(vj,val)) 
    def get_edge(self,vi,vj): 
        if self._invalid(vi) or self._invalid(vj): 
            raise GraphError(str(vi) + ' or' + str(vj) + ' is not valid vertex.') 
        for i,val in self._mat[vi]: 
            if i == vj: 
                return val 
        return self._unconn 
    def out_edges(self,vi): 
        if self._invalid(vi): 
            raise GraphError(str(vi) + ' is not valid vertex.') 
        return self._mat[vi]
```
## 遍历
- 图的深度优先遍历算法
```
import SStack
def DFS_graph(graph, v0):
    vnum = graph.vertex_num()
    visited = [0]*vnum        #用于记录已访问结点
    visited[v0] = 1
    DFS_seq = [v0]            #记录遍历顺序
    st = SStack()
    st.push((0, graph.out_edges(v0)))   # 入栈
    while not st.is_empty():
        i,edges = st.pop()
        if i < len(edges):
            v,e = edges[i]
            st.push((i + 1, edges)) # 下次访问
            if not visited[v]:
                DFS_seq.append(v)
                visited[v] = 1
                st.push((0, graph.out_edges(v)))
    return DFS_seq
            st.push((i+1, edges))
    
```

## 其他动作
### 最小生成树
   一个有 n 个结点的连通图的生成树是原图的极小连通子图，且包含原图中的所有 n 个结点，并且有保持图连通的最少的边。最小生成树可以用kruskal（克鲁斯卡尔）算法或prim（普里姆）算法求出。
   ```
   """1）Kruskal算法
Kruskal算法是一种构造最小生成树的简单算法，其中的思想也比较简单
算法思想：
（1）设G = （V，E）是一个网络，其中|V| = n。初始时取包含G中所有n个顶点但没有任何边的孤立点子图T= (V,{}),T里的每一个顶点自成一个连通分量
（2）将边集E中的边按权值递增的顺序排列，在构造中的每一步顺序地检查这个边序列，找到下一条（最短的）两端点位于T的两个不同连通分量的边e，把e加入T。这导致两个连通分量由于边e的连接而变成了一个连通分量
（3）每次操作使T减少一个连通分量，不断重复这个动作加入新边，直到T中所有顶点都包含在一个连通分量里为止，这个连通分量就是G的一棵最小生成树
算法实现
"""
#Krudkal最小生成树算法
def Kruskal(graph):
    vnum = graph.vertex_num()
    reps = [i for i in range(vnum)]
    mst,edges = [],[]
    for vi in range(vnum):  #所有边入表
        for v,w in graph.out_edges(vi):
            edges.append((w,vi,v))
    edges.sort()            #按权值排序
    for w,vi,vj in edges:
        if reps[vi] != reps[vj]:
            mst.append((vi,vj),w)
            if len(mst) == vnum - 1:
                break
            rep,orep = rep[vi],reps[vj]
            for i in range(vnum):  #合并连通分量
                if reps[i] == orep:
                    reps[i] = rep
    return mst


   ```
