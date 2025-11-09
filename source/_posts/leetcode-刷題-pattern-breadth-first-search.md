---
title: Leetcode 刷題 pattern - Breadth-First Search
date: 2019-12-21 19:32:59
tags:
author: Po-jen
---

## 前言

身在大 CS 時代，有越來越多人投入刷題的行列，在眼花撩亂的題海中，要想有效率地刷題，就需要掌握題目解法中，可以在許多地方應用的觀念，才能以簡禦繁。 Huli 寫的 [程式解題新手入門注意事項](https://blog.techbridge.cc/2019/11/02/before-start-leetcode/) 講得很好，寫題目是為了學會解題的思考方法，確保自己掌握重要的資料結構跟演算法。這也是為什麼我想要寫這系列的文章，把多個散落在各處的題目銜接起來，以後看到相似的問題就可以舉一反三，而不是去背各題目的解法。

舉例來說，之前遇過一題電話面試，問到的題目是：

```
Input: vector<bool> holidays, int pto
holidays 表示平日或假日，例如 0000011 表示前面 5 天是平日，後面 2 天是假日。
pto 表示最多可以放幾天假。
Output: 計算在可以用完 pto 的情況下，最久可以放多長的假。

範例：holidays = {0,0,0,0,0,1,1}, pto = 2, output = 4
     因為可以放 {0,0,0,1,1,1,1}
```

基本上因為之前有寫過 Sliding Window 的 pattern，所以這題很快就寫出來了，也順利進到下一關，所以大家不需要追求把題目都刷完，而是掌握好重要的基礎，接下來就是應用這些基礎就可以面對很多變化題（當然還是會有一些解法很巧妙的題目，但其實大部分公司不會硬出巧妙題）。

放上程式碼給大家參考：

```cpp
#include <iostream>
#include <vector>

using namespace std;

int longest_holidays(const vector<bool>& holidays, int pto) {
    int longestHoliday = -1, windowStart = 0;

    for(int windowEnd = 0; windowEnd < holidays.size(); ++windowEnd) {
        if(holidays[windowEnd] == 0) {
            -- pto;
        }

        // Shrink window size
        while(pto < 0) {
            if(holidays[windowStart++] == 0) {
                ++ pto;
            }
        }

        // Compare valid window size and longestHoliday
        longestHoliday = max(longestHoliday, windowEnd - windowStart + 1);
    }

    return longestHoliday;
}

int main(){
    // 000001100000110000111
    vector<bool> holidays = {0,0,0,0,0,1,1,0,0,0,0,0,1,1,0,0,0,0,1,1,1};
    int longest_holiday = longest_holidays(holidays, 9);

    cout << "My longest holiday: " << longest_holiday << ", hooray!\n";

    return 0;
}
```

上面這題還有一個 follow-up 問題，可以看我的 [面經分享](https://www.1point3acres.com/bbs/thread-558279-1-1.html)。

## Breadth-First Search 簡介

言歸正傳，今天要來跟大家介紹相當基礎的演算法 pattern - Breadth-First Search，雖然 BFS 是基礎中的基礎，大家多少都會，但你真的確定自己除了在 tree 跟 graph 這些顯而易見的題型以外，都能活用 BFS 嗎？

我們先看一個題目 - Leetcode #279 - Perfect Squares：

![img](https://i.imgur.com/iCbSTEd.png)

如果沒什麼刷題經驗，可能會想要用暴力的枚舉方法來解這個問題；如果有一些刷題經驗，可能會想到用 Dynamic Programming 來解。但其實，這題也可以用 BFS 解！而且實作非常簡單，舉這個例子是想讓大家看看 BFS 也可以應用在沒有明顯 graph 結構的問題上，我們會在第四個範例中解釋怎麼用 BFS 來解這題。

在進到題目之前，先給大家 BFS 實作的模板，這樣只要寫過幾題，之後在實作 BFS 就不容易出錯，因為基本的模板相通，只要將注意力集中在各題目不同的細節上就好。

### BFS template 1

BFS 跟 queue 這個資料結構是好夥伴，使用 queue 讓 BFS 的實作變得很簡單，比如說下面這個嘗試計算 root node 跟 target node 距離的 pseudo code：

```cpp
/**
 * Return the length of the shortest path between root and target node.
 */
int BFS(Node root, Node target) {
    queue<Node> queue; // store all nodes which are waiting to be processed
    step = 0;      // number of steps neeeded from root to current node

    // initialize
    add root to queue;

    // BFS
    while (queue is not empty) {
        step = step + 1;

        // iterate the nodes which are already in the queue
        int size = q.size();
        for (int i = 0; i < size; ++i) {
            Node cur = the first node in queue;

            return step if cur is target;
            for (Node next : the neighbors of cur) {
                add next to queue;
            }

            remove the first node from queue;
        }
    }

    return -1;  // there is no path from root to target
}
```

### BFS template 2

有時候，我們必須要避免走到重複的 node（例如在 cyclic graph 中，就可能又走回起點），才不會讓 BFS 無法結束，所以需要紀錄哪些 node 已經走過，模板如下：

```cpp
/**
 * Return the length of the shortest path between root and target node.
 */
int BFS(Node root, Node target) {
    queue<Node> queue;  // store all nodes which are waiting to be processed
    Set<Node> visited;  // store all the nodes that we've visited
    step = 0;       // number of steps neeeded from root to current node

    // initialize
    add root to queue;
    add root to visited;

    // BFS
    while (queue is not empty) {
        step = step + 1;

        // iterate the nodes which are already in the queue
        int size = queue.size();
        for (int i = 0; i < size; ++i) {
            Node cur = the first node in queue;
            return step if cur is target;

            for (Node next : the neighbors of cur) {
                if (next is not in visited) {
                    add next to queue;
                    add next to visited;
                }
            }

            remove the first node from queue;
        }
    }

    return -1;  // there is no path from root to target
}
```

## Breadth-First Search 的第一個範例 - Leetcode #286 - Walls and Gates

### 題目

![img](https://i.imgur.com/7y3XvVN.png)

這個題目比較明顯有 graph 的樣子，因為每一步都只能走到上/下/左/右的其中一個鄰居，而且是要找 "距離最近" 的門，所以比較直覺會想到可以用 BFS，我們就直接來看看解法。

### Breadth-First Search 解法

要做 BFS，我們有兩種選擇，第一種是從每個房間開始，尋找離這個房間最近的門；第二種則是先把每個門都放進 queue 裡面，每次都只走一步，如果遇到的是房間，就更新房間到門口的步數。

雖然這題的想法很簡單，可是如果你對 BFS 的模板還不是很熟，還是會需要花一些時間想一想邏輯要怎麼寫的，所以不用緊張，慢慢地想過一遍，盡力自己寫寫看，寫不出來就看看答案，看完自己想一遍再寫，反覆幾次就會越來越熟悉。提供一份 C++ 版本的實作如下：

```cpp
class Solution {
public:
    void wallsAndGates(vector<vector<int>>& rooms) {
        // Check corner cases
        int m = rooms.size();
        if(m < 1) { return; }
        int n = rooms[0].size();
        if(n < 1) { return; }

        // Prepare for BFS
        int GATE = 0, WALL = -1, ROOM = numeric_limits<int>::max();
        queue<pair<int,int>> q;
        vector<pair<int,int>> dirs{{0, -1}, {-1, 0}, {0, 1}, {1, 0}};

        // Put all gates into queue
        for(int i = 0; i < m; ++i) {
            for(int j = 0; j < n; ++j) {
                if(rooms[i][j] == GATE) q.push({i,j});
            }
        }

        // Start BFS
        int steps = 0;
        while(!q.empty()) {
            for(int sz = q.size()-1; sz >= 0; --sz) {
                pair<int, int> cur = q.front();
                q.pop();

                if(rooms[cur.first][cur.second] == ROOM) {
                    rooms[cur.first][cur.second] = steps;
                }

                if(rooms[cur.first][cur.second] != WALL) {
                    // Traverse neighbor
                    for(int i = 0; i < dirs.size(); ++i) {
                        cur.first += dirs[i].first;
                        cur.second += dirs[i].second;

                        if(isValid(cur, m, n) && rooms[cur.first][cur.second] == ROOM) {
                            q.push(cur);
                        }

                        cur.first -= dirs[i].first;
                        cur.second -= dirs[i].second;
                    }
                }
            }

            ++steps;
        }
    }

private:
    bool isValid(const pair<int, int>& cur, const int& m, const int& n) {
        return cur.first >= 0 && cur.first < m && cur.second >= 0 && cur.second < n;
    }
};
```

## Breadth-First Search 的第二個範例 - Leetcode #200 - Number of Islands

### 題目

![img](https://i.imgur.com/uIVF6nc.png)

### Depth-First Search 解法

這題應該算是非常經典的問題，目前在面試中出現的頻率也還是很高，以寫程式的簡潔度來說，DFS 是比較容易寫的，概念上就是，每次我們遇到一個島的邊界（也就是 '1' 的地方），我們就呼叫 DFS function 把跟這個 '1' 相鄰的 '1' 都變成 '0'，實作如下：

```cpp
class Solution {
public:
    int numIslands(vector<vector<char>>& grid) {
        int m = grid.size(), n = m ? grid[0].size() : 0, islands = 0;

        // 走過矩陣的所有 element，只要遇到島，就把島都消滅(即 '1' -> '0')
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == '1') {
                    islands++;
                    dfs(grid, m, n, i, j);
                }
            }
        }

        return islands;
    }

private:
    void dfs(vector<vector<char>>& grid, const int& m, const int& n, const int& i, const int& j) {
        if(grid[i][j] == '0') return;

        grid[i][j] = '0';

        if(i - 1 >= 0) { dfs(grid, m, n, i-1, j); }
        if(i + 1 < m)  { dfs(grid, m, n, i+1, j); }
        if(j - 1 >= 0) { dfs(grid, m, n, i, j-1); }
        if(j + 1 < n)  { dfs(grid, m, n, i, j+1); }
    }
};
```

### Breadth-First Search 解法

DFS 的實作雖然簡潔，可是，如果輸入的矩陣非常大，並且包含著很大的島，那隨著 DFS 的遞迴呼叫層數增加，是有可能產生 stack overflow 的，所以如果能用 BFS 解決，就會是比較安全的解法。

一樣，雖然想法上很簡單，但實作上還是需要多寫幾次才會熟悉，附上一份 BFS 的實作給大家參考：

```cpp
class Solution {
public:
    int numIslands(vector<vector<char>>& grid) {
        int m = grid.size(), n = m ? grid[0].size() : 0, islands = 0, offsets[] = {0, 1, 0, -1, 0};

        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == '1') {
                    islands++;
                    grid[i][j] = '0';

                    queue<pair<int, int>> todo;
                    todo.push({i, j});

                    while (!todo.empty()) {
                        pair<int, int> p = todo.front();
                        todo.pop();

                        for (int k = 0; k < 4; k++) {
                            int r = p.first + offsets[k], c = p.second + offsets[k + 1];
                            if (r >= 0 && r < m && c >= 0 && c < n && grid[r][c] == '1') {
                                grid[r][c] = '0';
                                todo.push({r, c});
                            }
                        }
                    }
                }
            }
        }

        return islands;
    }
};
```

## Breadth-First Search 的第三個範例 - Leetcode #752 - Open the Lock

### 題目

![img](https://i.imgur.com/W3vNo1V.png)
![img](https://i.imgur.com/dfJsEUy.png)
![img](https://i.imgur.com/RzPSMal.png)

這一題開始變得比較有趣，因為一看到這個題目的時候，未必會想到要用 BFS，所以接下來，讓我先分享一下思考的方法。

### 思考方法

這題雖然乍看之下不會直接想到要用 BFS，但其實還是有很多蛛絲馬跡，比如說：

1. 我們從 0000 開始，所以有一個起點。
2. 希望尋找的是 0000 到 target 之間的 "最短距離"。

看到這兩個提示，腦海中就會開始浮現一個從 0000 為 root，開始探索直到走到 target 的一個搜尋過程：

0000 -> 1000, 9000, 0100, 0900, 0010, 0090, 0001, 0009 -> ... -> ... -> target

所以就開始有可以使用 BFS 的影子出現。換句話說，因為看到有起點、要求最短距離，所以想了 "走一步" 是什麼意思？然後想到就是把其中一個數字 +1 或 -1。接著就可以再延伸，發現這就是一個從 0000 開始，然後假設 deadends 是已經 visit 過所以不能再走，的一個 BFS 問題。

### Breadth-First Search 解法

```cpp
class Solution {
public:
    int openLock(vector<string>& deadends, string target) {
        // Prepare for BFS
        // 因為不能走到 deadends 裡面的排列，所以我們直接放進 visited，這樣做 BFS 時就不會考慮這些路線
        unordered_set<string> visited(deadends.begin(), deadends.end());
        array<char, 10> digit= {'0', '1', '2', '3', '4', '5', '6', '7', '8', '9'};

        queue<string> todo;
        // 假設 0000 沒有在 visited(也就是 deadends 裡沒有) 中，我們才需要開始尋找
        if(!visited.count("0000")) {
            todo.push("0000");
            visited.insert("0000");
        }

        // Start BFS
        int steps = -1;
        while(!todo.empty()) {
            ++steps;

            for(int sz = todo.size() - 1; sz >= 0; --sz) {
                string cur = todo.front();
                todo.pop();

                // Check if it's target
                if(cur == target) {
                    return steps;
                }

                // Go through neighbors
                // 如何找到對的 neighbor 也是這題比較 tricky 的地方之一
                for(int i = 0; i < 4; ++i) {
                    for(int j = -1; j <= 1; j += 2) {
                        string neighbor = cur;
                        // Wrap around index to prevent out-of-bound error
                        neighbor[i] = digit[(neighbor[i]-'0'+j+10) % 10];

                        if(!visited.count(neighbor)) {
                            todo.push(neighbor);
                            visited.insert(neighbor);
                        }
                    }
                }
            }
        }

        return -1;
    }
};
```

寫完這題，如果你是 BFS 新手，應該會覺得頗好玩，因為這題的鄰居找法跟之前有明顯 matrix 結構的情況不太一樣，而是變成在另一個抽象的搜索空間中做 BFS。

## Breadth-First Search 的第四個範例 - Leetcode #279 - Perfect Squares

### 題目

![img](https://i.imgur.com/iCbSTEd.png)

### 暴力解

暴力法很直覺，反正就每種組合都試試看就對了，例如若要看 12 至少要由幾個 perfect suqare number 組成，那就看 "1 至少要幾個 + 11 至少要幾個"、"2 至少要幾個 + 10 至少要幾個" ... 依此類推，直到比較過所有可能的拆分方法，就知道最少要幾個，實作如下：

```cpp
class Solution {
public:
    int numSquares(int n) {
        if(isPerfectSquare(n)) {
            return 1;
        }

        int res = n;
        for(int i = 1; i <= n / 2; ++i) {
            int comb = numSquares(i) + numSquares(n - i);

            if(comb < res) {
                res = comb;
            }
        }

        return res;
    }

private:
    bool isPerfectSquare(double x) {
      // Find floating point value of square root of x.
      double sr = sqrt(x);

      // If square root is an integer
      return ((sr - floor(sr)) == 0);
    }
};
```

很顯然，這種方法有夠慢，在 leetcode 上面跑也確實會超時。雖然可以用 memoization、Dynamic Programming，不過今天的重點是 BFS，所以我們來探討一下 BFS 解。

### Breadth-First Search 解法

如果你有辦法想到 BFS，應該會覺得滿好玩的。這題的思路如下：

1. 如果 n 是 perfect square，答案就是 1。
2. 如果 n 可以由兩個 perfect square 組成，答案就是 2。

所以我們如果要使用 BFS，起點就是所有的 perfect square！

看一個範例：

```
n = 12
起點：1, 4, 9
走一步：1+1 = 2, 1+4 = 5, 1 + 9 = 10, 4+4 = 8
走兩步：2+1 = 3, 2+4 = 6, 2+9 = 11...
```

所以，每次要拜訪的鄰居就是 queue 裡面的數字再加上一個 perfect square！

想懂觀念之後，實作出來就只是時間問題：

```cpp
class Solution {
public:
    int numSquares(int n) {
        // Declare data structure for BFS
        unordered_set<int> visited;
        queue<int> todo;

        todo.push(0);
        visited.insert(0);

        // Start BFS
        int steps = 0;
        while(!todo.empty()) {
            ++steps;

            for(int sz = todo.size() - 1; sz >= 0; --sz) {
                // Retrieve current int
                int cur = todo.front();
                todo.pop();

                for(int j = 1; j < n; ++j) {
                    int sum = cur + j * j;

                    if(sum == n) { return steps; }
                    else if(sum > n) { break; }
                    else{
                        if(!visited.count(sum)) {
                            todo.push(sum);
                            visited.insert(sum);
                        }
                    }
                }
            }
        }

        return steps;
    }
};
```

## Breadth-First Search 的第五個範例 - Leetcode #1284 - Minimum Number of Flips to Convert Binary Matrix to Zero Matrix

### 題目

![img](https://i.imgur.com/DpIil2u.png)

![img](https://i.imgur.com/ILEBcZi.png)

這一題是我在某一週的 Leetcode contest 寫到的，因為當時我正好寫完上面四個 BFS 的範例題，對 BFS 的 pattern 很有感覺，所以這題很快就寫出來了。

### Breadth-First Search 解法

因為概念上跟第三個範例有點像，都是在自己想像中的搜索空間中進行 BFS，所以這題的想法我就不贅述了，基本上就是把整個 matrix 的狀態當作 node，每翻一次就是走一步，就可以用 BFS 解決了，附上程式碼：

```cpp
class Solution {
public:
    int minFlips(vector<vector<int>>& mat) {
        // Check corner cases
        int m = mat.size();
        int n = m ? mat[0].size() : 0;
        if(m < 1 or n < 1) { return -1; }

        // Prepare for BFS
        set<vector<vector<int>>> visited;
        queue<vector<vector<int>>> q;
        q.push(mat);
        visited.insert(mat);

        int steps = 0;
        while(!q.empty()) {
            for(int sz = q.size() - 1; sz >= 0; --sz) {
                vector<vector<int>> cur = q.front();
                q.pop();

                // 已經成功翻成全部都是 0
                if(allZero(cur)) {
                    return steps;
                }

                for(int r = 0; r < m; ++r) {
                    for(int c = 0; c < n; ++c) {
                        // 以 (r,c) 為中心翻一次 matrix
                        flip(cur, m, n, r, c);

                        // 如果還沒走過這個盤面，放入 queue
                        if(!visited.count(cur)) {
                            q.push(cur);
                            visited.insert(cur);
                        }

                        // 把盤面翻回來
                        flip(cur, m, n, r, c);
                    }
                }
            }

            ++steps;
        }

        return -1;
    }

private:
    void flip(vector<vector<int>>& mat, int& m, int& n, int& r, int& c) {
        // 翻鄰居
        if(r - 1 >= 0) { mat[r-1][c] = (mat[r-1][c] == 1) ? 0 : 1; }
        if(r + 1 < m)  { mat[r+1][c] = (mat[r+1][c] == 1) ? 0 : 1; }
        if(c - 1 >= 0) { mat[r][c-1] = (mat[r][c-1] == 1) ? 0 : 1; }
        if(c + 1 < n)  { mat[r][c+1] = (mat[r][c+1] == 1) ? 0 : 1; }

        // 翻(r,c)
        mat[r][c] = (mat[r][c] == 1) ? 0 : 1;
    }

    bool allZero(const vector<vector<int>>& mat) {
        for(int i = 0; i < mat.size(); ++i) {
            for(int j = 0; j < mat[0].size(); ++j) {
                if(mat[i][j] != 0) {
                    return false;
                }
            }
        }

        return true;
    }
};
```

## 總結

今天跟大家介紹了一個新的 pattern - Breadth-First Search，上面提供的五題是讓大家體驗一下，BFS 看似簡單的，卻可以應用在很多乍看之下不會聯想到 BFS 的題目。能夠體驗到這一層，就是你可以開始靈活運用 BFS 的起點。刷題不只是為了通過面試，而是加強自己對資結和演算法的洞見和體驗，進而優雅有效率地解決工程問題，共勉之。

## 延伸閱讀

1. [Leetcode 刷題 pattern - Two Pointer](https://blog.techbridge.cc/2019/08/30/leetcode-pattern-two-pointer/)
2. [Leetcode 刷題 pattern - Sliding Window](https://blog.techbridge.cc/2019/09/28/leetcode-pattern-sliding-window/)
3. [Leetcode 刷題 pattern - Next Greater Element](https://blog.techbridge.cc/2019/10/26/leetcode-pattern-next-greater-element/)
4. [Leetcode 刷題 pattern - Fast & Slow Pointer](https://blog.techbridge.cc/2019/11/22/leetcode-pattern-fast-and-slow-pointer/)

關於作者：
[@pojenlai](https://pojenlai.wordpress.com/) 演算法工程師，對機器人、電腦視覺和人工智慧有少許研究，正致力改善 [自己的習慣](https://www.books.com.tw/products/0010822522)。
