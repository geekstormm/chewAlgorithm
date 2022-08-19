# 04 DFS BFS 二叉堆 二叉搜索树

## DFS

```C++
//leetcode 17 电话号码的数字组合
//写法一，将str作为类成员变量，注意搜索完要还原现场
class Solution {
public:
    vector<string> letterCombinations(string digits) {
        this->digits = digits;
        alphabet['2'] = "abc";
        alphabet['3'] = "def";
        alphabet['4'] = "ghi";
        alphabet['5'] = "jkl";
        alphabet['6'] = "mno";
        alphabet['7'] = "pqrs";
        alphabet['8'] = "tuv";
        alphabet['9'] = "wxyz";
        if(digits.empty())  return {};
        dfs(0);
        return res;
    }

private:
    void dfs(int index) {
        if(index == digits.size()) {
            res.push_back(str);
            return;
        }

        for(auto ch : alphabet[digits[index]]) {
            str.push_back(ch);
            dfs(index + 1);
            str.pop_back();
        }
    }

    string digits;
    string str;
    vector<string> res;
    unordered_map<char, string> alphabet;
};

//写法二，将str作为dfs的状态传入

class Solution {
public:
    vector<string> letterCombinations(string digits) {
        this->digits = digits;
        alphabet['2'] = "abc";
        alphabet['3'] = "def";
        alphabet['4'] = "ghi";
        alphabet['5'] = "jkl";
        alphabet['6'] = "mno";
        alphabet['7'] = "pqrs";
        alphabet['8'] = "tuv";
        alphabet['9'] = "wxyz";
        if(digits.empty())  return {};
        dfs(0, "");
        return res;
    }

private:
    void dfs(int index, string str) {
        if(index == digits.size()) {
            res.push_back(str);
            return;
        }

        for(auto ch : alphabet[digits[index]]) {
            //str.push_back(ch);
            dfs(index + 1, str + ch);
            //str.pop_back();
        }
    }

    string digits;
    //string str;
    vector<string> res;
    unordered_map<char, string> alphabet;
};
```



```C++
//leetcode 51 n皇后
class Solution {
public:
  vector<vector<string>> solveNQueens(int n) {
    this->n = n;
    used = vector<bool>(n, false);
    dfs(0);

    vector<vector<string>> ans;
    for (auto p: res) {
      vector<string> pattern(n, string(n, '.'));
      for (int row = 0; row < n; ++row) {
        pattern[row][p[row]] = 'Q';
      }
      ans.push_back(pattern);
    };

    return ans;
  }

private:
  void dfs(int row) {
    if (row == n) {
      res.push_back(p);
      return;
    }

    for (int col = 0; col < n; ++col) {
      if (!used[col] && !used_plus[row + col] && !used_minus[row - col]) {
        used[col] = true;
        used_plus[row + col] = true;
        used_minus[row - col] = true;
        p.push_back(col);
        dfs(row + 1);
        p.pop_back();
        used_minus[row - col] = false;
        used_plus[row + col] = false;
        used[col] = false;
      }
    }
  }

  int n;
  vector<bool> used;
  vector<int> p;//排列
  vector<vector<int>> res;
  unordered_map<int, bool> used_plus;
  unordered_map<int, bool> used_minus;
};
```



```C++
//leetcode 200 岛屿数量
class Solution {
public:
  int numIslands(vector<vector<char>> &grid) {
    this->grid = grid;
    m = grid.size();
    n = grid[0].size();

    int res = 0;
    for (int i = 0; i < m; ++i)
      for (int j = 0; j < n; ++j)
        if (grid[i][j] == '1') {
          ++res;
          dfs(i, j, grid);
        }
    return res;
  }

private:
  void dfs(int i, int j, vector<vector<char>> &grid) {
    grid[i][j] = '0';

    for (int k = 0; k < 4; ++k) {
      int nx = i + dir[k], ny = j + dir[k + 1];
      if (nx >= 0 && nx < m && ny >= 0 && ny < n && grid[nx][ny] == '1')
        dfs(nx, ny, grid);
    }
  }

  vector<vector<char>> grid;
  int m;
  int n;
  const vector<int> dir{-1, 0, 1, 0, -1};
};
```



```C++
//leetcode 130 被围绕的区域
class Solution {
public:
  void solve(vector<vector<char>> &board) {
    this->board = board;
    m = board.size();
    n = board[0].size();

    for (int i = 0; i < m; ++i)
      for (int j = 0; j < n; ++j) {
        if (is_edge(i, j) && board[i][j] == 'O')
          dfs(i, j, board);
      }

    for(int i = 0; i <m; ++i)
      for(int j = 0; j < n; ++j) {
        if(board[i][j] == '#')
          board[i][j] = 'O';
        else if(board[i][j] == 'O')
          board[i][j] = 'X';
      }

  }

private:
  void dfs(int x, int y, vector<vector<char>> &board) {
    board[x][y] = '#';

    for (int i = 0; i < 4; ++i) {
      int nx = x + dir[i], ny = y + dir[i + 1];
      if (valid(nx, ny) && board[nx][ny] == 'O')
        dfs(nx, ny, board);
    }
  }

  bool valid(int x, int y) {
    if (x >= 0 && x < m && y >= 0 && y < n)
      return true;
    return false;
  }

  bool is_edge(int i, int j) {
    if (i == 0 || j == 0 || i == m - 1 || j == n - 1)
      return true;
    return false;
  }

  int m, n;
  vector<vector<char>> board;
  const vector<int> dir{-1, 0, 1, 0, -1};
};
```



## BFS

```C++
//leetcode 200 岛屿数量
class Solution {
public:
  int numIslands(vector<vector<char>> &grid) {
    this->grid = grid;
    m = grid.size();
    n = grid[0].size();
    visited = vector<vector<bool>>(m, vector<bool>(n, false));

    int res = 0;
    for (int i = 0; i < m; ++i)
      for (int j = 0; j < n; ++j)
        if (grid[i][j] == '1' && !visited[i][j]) {
          ++res;
          bfs(i, j, visited);
        }
    return res;
  }

private:
  void bfs(int sx, int sy, vector<vector<bool>> &visited) {
    queue<pair<int, int>> q;
    q.push({sx, sy});
    visited[sx][sy] = true;

    while (!q.empty()) {
      int x = q.front().first;
      int y = q.front().second;
      q.pop();

      for (int i = 0; i < 4; ++i) {
        int nx = x + dir[i];
        int ny = y + dir[i + 1];
        if(nx >= 0 && nx < m && ny >= 0 && ny < n && 
           grid[nx][ny] == '1' && !visited[nx][ny]) {
          q.push({nx, ny});
          visited[nx][ny] = true;
        }
      }
    }
  }

  int m, n;
  vector<vector<bool>> visited;
  vector<vector<char>> grid;
  const vector<int> dir{-1, 0, 1, 0, -1};
};

```





## 二叉堆

```C++
//leetcode 23 合并k个升序链表
struct Node {
  int key;
  ListNode *listNode;

  Node(int key, ListNode *listNode) : key(key), listNode(listNode) {}
};

//priority_queue默认是大根堆
//重载 < 使得变成大根堆
bool operator<(const Node &a, const Node &b) {
  return a.key > b.key;
}

//堆的解法
class Solution {
public:
  ListNode *mergeKLists(vector<ListNode *> &lists) {
    for (auto node: lists) {
      if (node == nullptr) continue;
      q.push(Node(node->val, node));
    }

    ListNode head;
    ListNode *tail = &head;
    while(!q.empty()) {
      Node node = q.top();
      q.pop();
      tail->next = node.listNode;
      tail = tail->next;

      ListNode* next = node.listNode->next;
      if(next != nullptr) {
        q.push(Node{next->val, next});
      }
    }
    return head.next;
  }

private:
  priority_queue<Node> q;
};
```



```
struct Node {

}
```



## 二叉搜索树

```C++
//leetcode 701 二叉搜索树的插入操作
class Solution {
public:
  TreeNode* insertIntoBST(TreeNode* root, int val) {
    if(root == nullptr) {
      return new TreeNode(val);
    }

    if(val < root->val) {
      root->left = insertIntoBST(root->left, val);
    } else {
      root->right = insertIntoBST(root->right, val);
    }

    return root;
  }
};
```



```C++
//leetcode 面试题0406 找二叉搜索树的后继
class Solution {
public:
  TreeNode *inorderSuccessor(TreeNode *root, TreeNode *p) {
    return getNext(root, p->val);
  }

private:
  TreeNode *getNext(TreeNode *root, int val) {
    TreeNode *res = nullptr;
    TreeNode *cur = root;
    while (cur != nullptr) {
      if (cur->val == val) {
        //右子树不为空
        if (cur->right != nullptr) {
          res = cur->right;
          while (res->left != nullptr)
            res = res->left;
        }
        break;
      }

      if (val < cur->val) {
        //右子树为空时，后继必然在之前的路径里，找到路径里最小的
        if (res == nullptr || cur->val < res->val) res = cur;
        cur = cur->left;
      } else
        cur = cur->right;
    }
    return res;
  }

};
```



```C++
//leetcode 450 删除二叉搜索树中的节点
class Solution {
public:
  TreeNode *deleteNode(TreeNode *root, int key) {
    if (root == nullptr) return nullptr;

    if (root->val == key) {
      //此节点只有一侧有值，直接接上即可
      if (root->left == nullptr) return root->right;
      if (root->right == nullptr) return root->left;

      //两侧都有值，用后继（即右子树中最左边的节点）来代替
      TreeNode *next = root->right;
      while (next->left != nullptr) next = next->left;
      root->right = deleteNode(root->right, next->val);
      root->val = next->val;
      return root;
    }

    if (key < root->val) {
      root->left = deleteNode(root->left, key);
    } else {
      root->right = deleteNode(root->right, key);
    }
    return root;
  }
};

```

