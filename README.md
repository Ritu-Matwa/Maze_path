# Maze_path
#include <iostream>
#include <vector>
#include <queue>
#include<algorithm>
#include<cstdlib>
using namespace std;

const vector<vector<int>> dir = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};

vector<vector<int>> findPath(int n, int m, vector<vector<char>>& maze, vector<int>& start, vector<int>& end) {
    queue<vector<int>> Q;
    Q.push(start);
    vector<vector<bool>> visited(n, vector<bool>(m, false));

    vector<vector<vector<int>>> parent(n, vector<vector<int>>(m));
    visited[start[0]][start[1]] = true;

    while (!Q.empty()) {
        vector<int> u = Q.front();
        Q.pop();

        for (const auto& d : dir) {
            int i = u[0] + d[0];
            int j = u[1] + d[1];
            if (i >= 0 && i < n && j >= 0 && j < m && !visited[i][j] && maze[i][j] == '.') {
                Q.push({i, j});
                visited[i][j] = true;
                parent[i][j] = u;
            }
        }
    }

    if (!visited[end[0]][end[1]]) return {};
    vector<vector<int>> path;
    vector<int> cur = end;
    while (parent[cur[0]][cur[1]].size() > 0) {
        path.push_back(cur);
        cur = parent[cur[0]][cur[1]];
    }
    reverse(path.begin(), path.end());
    return path;
}

int main() {
    int n = 5;
    int m = 5;
    vector<vector<char>> maze = {
            {'.', '#', '.', '.', '.'},
            {'.', '#', '.', '#', '.'},
            {'.', '.', '.', '.', '.'},
            {'.', '#', '#', '#', '.'},
            {'.', '.', '.', '.', '.'}
    };
    vector<int> start = {0, 0};
    vector<int> end = {4, 4};
    vector<vector<int>> path = findPath(n, m, maze, start, end);
    if (path.empty()) {
        cout << "No path found!" << endl;
    } else {
        cout << "Path found:" << endl;
        for (const auto& pos : path) {
            cout << "(" << pos[0] << "," << pos[1] << ")" << endl;
        }
    }
    return 0;
}
