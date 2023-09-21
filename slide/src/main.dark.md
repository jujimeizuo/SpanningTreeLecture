---
title: 最小生成树 - 算法设计与分析
separator: <!--s-->
verticalSeparator: <!--v-->
theme: simple
highlightTheme: monokai-sublime
css: 
    - custom.css
    - dark.css
revealOptions:
    transition: 'slide'
    transitionSpeed: fast
    center: false
    slideNumber: "c/t"
    width: 1000
---

<div class="middle center">
<div style="width: 100%">

<img src="images/jnu_cs_logo.png" style="margin-bottom: 1em">

# 最小生成树

<hr/>

2023 年算法设计与分析课程

By [@冯则涛（jujimeizuo)](http://www.jujimeizuo.cn)

<!-- ←/→ Space Home End 翻页 -->


<div style="text-align: right; margin-top: 2em;">
<p>2023.9.19&emsp;&emsp;&emsp;</p>
</div>

</div>
</div>

<!--v-->

## 本节内容

- 什么是最小生成树
- 最小生成树的唯一性证明
- 图的存储
- Prim 算法
- Kruskal 算法
- 扩展


<!--s-->

<div class="middle center">
<div style="width: 100%">

# Part.1 什么是最小生成树?

</div>
</div>

<!--v-->

## 什么是最小生成树
<div class="fragment">

- 子图:对一张图$G = (V,E)$，若存在另一张图$H=(H^\prime, E^\prime)$满足$V^\prime \in V$且$E^\prime \in E$，则称$H$是$G$的子图（subgraph），记作$H \in G$;
</div>

<div class="fragment">

- 生成子图:若$H \in G$满足$V^\prime = V$，则称$H$为$G$的生成子图/支撑子图;

</div>

<div class="fragment">

- (无根)树:有$n$个结点，$n-1$条边的连通无向图;

</div>

<div class="fragment">

- 生成树:一个连通无向图的生成子图，同时要求是树，即在图的边集中选择$n-1$条，将所有顶点连通;

</div>

<div class="fragment">

- **只有连通图才有生成树，而对于非连通图，只存在生成树林**

</div>

<div class="fragment">

- 定义无向连通图的最小生成树（Minimum Spanning Tree，MST）为边权和最小的生成树。

</div>

<!--s-->


<div class="middle center">
<div style="width: 100%">

# Part.2 最小生成树的唯一性证明

</div>
</div>

<!--v-->

## 最小生成树的唯一性证明

<!-- 如果一条边不在最小生成树的边集中，并且可以替换与其权值相同、并且在最小生成树边集的另一条边。那么，这个最小生成树就是不唯一的。 -->

<!--s-->


<div class="middle center">
<div style="width: 100%">

# Part.3 图的存储

</div>
</div>

<!--v-->
## 图的存储

- 一张图需要存储什么信息？
    - 越丰富越好，当然不能存储过多无用信息，导致空间消耗大
    - 点的信息、边的信息、边的连接情况
    - 无向图？有向图？树？


<!--v-->

## 基类 graph

<div class="fragment">

```C++
template <typename T>
class graph {
public:
    struct edge {   // 边
        int from;   // 边的起始点
        int to;     // 边的终点
        T cost;     // 边权
    };

    const int n;
    std::vector<edge> edges;
    std::vector<std::vector<int>> g;

    graph(int _n) : n(_n), g(_n) {}

    virtual int add(int from, int to, T cost) = 0;
};
```

</div>

<!--v-->

## 有向图 digraph

<div class="fragment">

```C++ {.digraph}
template <typename T>
class digraph : public graph<T> {
public:
    using graph<T>::edges;
    using graph<T>::g;
    using graph<T>::n;

    digraph(int _n) : graph<T>(_n) {}
 
    int add(int from, int to, T cost = 1) {
        assert(0 <= from && from < n && 0 <= to && to < n);
        int id = (int) edges.size();
        g[from].push_back(id);
        edges.push_back({from, to, cost});
        return id;
    }
 
    digraph<T> reverse() const {
        digraph<T> rev(n);
        for (auto &e : edges) {
            rev.add(e.to, e.from, e.cost);
        }
        return rev;
    }
};
```

</div>

<!--v-->

## 无向图 undigraph

<div class="fragment">

```C++
template <typename T>
class undigraph : public graph<T> {
public:
    using graph<T>::edges;
    using graph<T>::g;
    using graph<T>::n;
 
    undigraph(int _n) : graph<T>(_n) {}
 
    int add(int from, int to, T cost = 1) {
        assert(0 <= from && from < n && 0 <= to && to < n);
        int id = (int) edges.size();
        g[from].push_back(id);
        g[to].push_back(id);
        edges.push_back({from, to, cost});
        return id;
    }
};
```

</div>

<!--v-->

## 树 forest

<div class="fragment">

```C++
template <typename T>
class forest : public graph<T> {
public:
    using graph<T>::edges;
    using graph<T>::g;
    using graph<T>::n;

    forest(int _n) : graph<T>(_n) {}

    int add (int from, int to, T cost = 1) {
        assert(0 <= from && from < n && 0 <= to && to < n);
        int id = (int) edges.size();
        assert(id < n - 1);
        g[from].push_back(id);
        g[to].push_back(id);
        edges.push_back({from, to, cost});
        return id;
    }
};
```

</div>

<!--s-->

<div class="middle center">
<div style="width: 100%">

# Part.4 Prim

</div>
</div>

<!--s-->

<div class="middle center">
<div style="width: 100%">

# Part.5 Kruskal

</div>
</div>

<!--s-->

<div class="middle center">
<div style="width: 100%">

# Part.6 扩展

</div>
</div>

<!--s-->

<div class="middle center">
<div style="width: 100%">

# 谢谢大家

<hr/>

**Questions?**

</div>
</div>