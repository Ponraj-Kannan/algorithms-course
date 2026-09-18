<script setup>
import { ref, reactive, computed, onMounted, onBeforeUnmount, watch, nextTick } from 'vue';

defineProps({
  topic: { type: String, default: 'Backtracking Algorithms' },
  subTopic: { type: String, default: 'Rat in a Maze' }
});

const CODES = {
  java: [
    ['',                   'import java.util.Scanner;'],
    ['',                   'import java.util.List;'],
    ['',                   'import java.util.ArrayList;'],
    ['',                   'import java.util.Collections;'],
    ['',                   ''],
    ['',                   'public class Main {'],
    ['c_solve_entry',      '    static void solve(int r, int c, int[][] maze, int rows, int cols, boolean[][] visited, String path, List<String> paths) {'],
    ['c_base_check',       '        if (r == rows - 1 && c == cols - 1) {'],
    ['c_save_sol',         '            paths.add(path);'],
    ['c_base_ret',         '            return;'],
    ['',                   '        }'],
    ['c_mark_vis',         '        visited[r][c] = true;'],
    ['c_dir_init',         '        int[] dRow = {1, 0, 0, -1};'],
    ['c_dir_col',          '        int[] dCol = {0, -1, 1, 0};'],
    ['c_dir_char',         '        char[] dChar = {\'D\', \'L\', \'R\', \'U\'};'],
    ['c_loop_dirs',        '        for (int i = 0; i < 4; i++) {'],
    ['c_calc_next',        '            int nextR = r + dRow[i];'],
    ['c_calc_col',         '            int nextC = c + dCol[i];'],
    ['c_check_safe',       '            if (isSafe(nextR, nextC, maze, rows, cols, visited)) {'],
    ['c_recurse',          '                solve(nextR, nextC, maze, rows, cols, visited, path + dChar[i], paths);'],
    ['',                   '            }'],
    ['',                   '        }'],
    ['c_backtrack',        '        visited[r][c] = false;'],
    ['',                   '    }'],
    ['',                   ''],
    ['c_safe_fn',          '    static boolean isSafe(int r, int c, int[][] maze, int rows, int cols, boolean[][] visited) {'],
    ['c_safe_bounds',      '        if (r < 0 || r >= rows || c < 0 || c >= cols) {'],
    ['c_safe_bounds_ret',  '            return false;'],
    ['',                   '        }'],
    ['c_safe_wall',        '        if (maze[r][c] == 0) {'],
    ['c_safe_wall_ret',    '            return false;'],
    ['',                   '        }'],
    ['c_safe_visited',     '        if (visited[r][c]) {'],
    ['c_safe_visited_ret', '            return false;'],
    ['',                   '        }'],
    ['c_safe_ret_true',    '        return true;'],
    ['',                   '    }'],
    ['',                   ''],
    ['c_entry',            '    static List<String> findPaths(int[][] maze, int rows, int cols) {'],
    ['c_init_paths',       '        List<String> paths = new ArrayList<>();'],
    ['c_chk_start',        '        if (maze[0][0] == 0 || maze[rows - 1][cols - 1] == 0) {'],
    ['c_chk_start_ret',    '            return paths;'],
    ['',                   '        }'],
    ['c_init_visited',     '        boolean[][] visited = new boolean[rows][cols];'],
    ['c_call_solve',       '        solve(0, 0, maze, rows, cols, visited, "", paths);'],
    ['c_sort_paths',       '        Collections.sort(paths);'],
    ['c_return_paths',     '        return paths;'],
    ['',                   '    }'],
    ['',                   ''],
    ['',                   '    public static void main(String[] args) {'],
    ['m_scanner',          '        Scanner sc = new Scanner(System.in);'],
    ['m_read_dims',        '        int rows = sc.nextInt();'],
    ['m_read_cols',         '        int cols = sc.nextInt();'],
    ['m_alloc_maze',       '        int[][] maze = new int[rows][cols];'],
    ['m_read_maze',        '        for (int i = 0; i < rows; i++) {'],
    ['m_read_maze_inner',  '            for (int j = 0; j < cols; j++) {'],
    ['m_read_maze_body',   '                maze[i][j] = sc.nextInt();'],
    ['',                   '            }'],
    ['',                   '        }'],
    ['m_call_solver',      '        List<String> paths = findPaths(maze, rows, cols);'],
    ['m_print_count',      '        System.out.println(paths.size());'],
    ['m_print_paths',      '        for (int i = 0; i < paths.size(); i++) {'],
    ['m_print_paths_body', '            System.out.println(paths.get(i));'],
    ['',                   '        }'],
    ['m_done',             '    }'],
    ['',                   '}']
  ],
  c: [
    ['',                   '#include <stdio.h>'],
    ['',                   '#include <string.h>'],
    ['',                   ''],
    ['',                   'int isSafe(int r, int c, int maze[10][10], int rows, int cols, int visited[10][10]);'],
    ['',                   'void solve(int r, int c, int maze[10][10], int rows, int cols, int visited[10][10], char path[100], int len, char paths[100][100], int* count);'],
    ['',                   'int findPaths(int maze[10][10], int rows, int cols, char paths[100][100]);'],
    ['',                   ''],
    ['c_solve_entry',      'void solve(int r, int c, int maze[10][10], int rows, int cols, int visited[10][10], char path[100], int len, char paths[100][100], int* count) {'],
    ['c_base_check',       '    if (r == rows - 1 && c == cols - 1) {'],
    ['c_save_sol',         '        path[len] = \'\\0\';'],
    ['',                   '        strcpy(paths[*count], path);'],
    ['',                   '        (*count)++;'],
    ['c_base_ret',         '        return;'],
    ['',                   '    }'],
    ['c_mark_vis',         '    visited[r][c] = 1;'],
    ['c_dir_init',         '    int dRow[4] = {1, 0, 0, -1};'],
    ['c_dir_col',          '    int dCol[4] = {0, -1, 1, 0};'],
    ['c_dir_char',         '    char dChar[4] = {\'D\', \'L\', \'R\', \'U\'};'],
    ['c_loop_dirs',        '    for (int i = 0; i < 4; i++) {'],
    ['c_calc_next',        '        int nextR = r + dRow[i];'],
    ['c_calc_col',         '        int nextC = c + dCol[i];'],
    ['c_check_safe',       '        if (isSafe(nextR, nextC, maze, rows, cols, visited)) {'],
    ['c_recurse',          '            path[len] = dChar[i];'],
    ['',                   '            solve(nextR, nextC, maze, rows, cols, visited, path, len + 1, paths, count);'],
    ['',                   '        }'],
    ['',                   '    }'],
    ['c_backtrack',        '    visited[r][c] = 0;'],
    ['',                   '}'],
    ['',                   ''],
    ['c_safe_fn',          'int isSafe(int r, int c, int maze[10][10], int rows, int cols, int visited[10][10]) {'],
    ['c_safe_bounds',      '    if (r < 0 || r >= rows || c < 0 || c >= cols) {'],
    ['c_safe_bounds_ret',  '        return 0;'],
    ['',                   '    }'],
    ['c_safe_wall',        '    if (maze[r][c] == 0) {'],
    ['c_safe_wall_ret',    '        return 0;'],
    ['',                   '    }'],
    ['c_safe_visited',     '    if (visited[r][c]) {'],
    ['c_safe_visited_ret', '        return 0;'],
    ['',                   '    }'],
    ['c_safe_ret_true',    '    return 1;'],
    ['',                   '}'],
    ['',                   ''],
    ['c_entry',            'int findPaths(int maze[10][10], int rows, int cols, char paths[100][100]) {'],
    ['c_init_paths',       '    int pathCount = 0;'],
    ['c_chk_start',        '    if (maze[0][0] == 0 || maze[rows - 1][cols - 1] == 0) {'],
    ['c_chk_start_ret',    '        return 0;'],
    ['',                   '    }'],
    ['c_init_visited',     '    int visited[10][10] = {0};'],
    ['',                   '    char path[100];'],
    ['c_call_solve',       '    solve(0, 0, maze, rows, cols, visited, path, 0, paths, &pathCount);'],
    ['c_sort_paths',       '    return pathCount;'],
    ['c_return_paths',     '    return pathCount;'],
    ['',                   '}'],
    ['',                   ''],
    ['',                   'int main() {'],
    ['m_scanner',          '    int rows;'],
    ['',                   '    int cols;'],
    ['m_read_dims',        '    scanf("%d %d", &rows, &cols);'],
    ['m_alloc_maze',       '    int maze[10][10];'],
    ['m_read_maze',        '    for (int i = 0; i < rows; i++) {'],
    ['m_read_maze_inner',  '        for (int j = 0; j < cols; j++) {'],
    ['m_read_maze_body',   '            scanf("%d", &maze[i][j]);'],
    ['',                   '        }'],
    ['',                   '    }'],
    ['m_call_solver',      '    char paths[100][100];'],
    ['',                   '    int count = findPaths(maze, rows, cols, paths);'],
    ['m_print_count',      '    printf("%d\\n", count);'],
    ['m_print_paths',      '    for (int i = 0; i < count; i++) {'],
    ['m_print_paths_body', '        printf("%s\\n", paths[i]);'],
    ['',                   '    }'],
    ['m_done',             '    return 0;'],
    ['',                   '}']
  ],
  cpp: [
    ['',                   '#include <iostream>'],
    ['',                   '#include <vector>'],
    ['',                   '#include <string>'],
    ['',                   '#include <algorithm>'],
    ['',                   'using namespace std;'],
    ['',                   ''],
    ['',                   'bool isSafe(int r, int c, const vector<vector<int>>& maze, int rows, int cols, const vector<vector<bool>>& visited);'],
    ['',                   'void solve(int r, int c, const vector<vector<int>>& maze, int rows, int cols, vector<vector<bool>>& visited, string path, vector<string>& paths);'],
    ['',                   'vector<string> findPaths(const vector<vector<int>>& maze, int rows, int cols);'],
    ['',                   ''],
    ['c_solve_entry',      'void solve(int r, int c, const vector<vector<int>>& maze, int rows, int cols, vector<vector<bool>>& visited, string path, vector<string>& paths) {'],
    ['c_base_check',       '    if (r == rows - 1 && c == cols - 1) {'],
    ['c_save_sol',         '        paths.push_back(path);'],
    ['c_base_ret',         '        return;'],
    ['',                   '    }'],
    ['c_mark_vis',         '    visited[r][c] = true;'],
    ['c_dir_init',         '    int dRow[4] = {1, 0, 0, -1};'],
    ['c_dir_col',          '    int dCol[4] = {0, -1, 1, 0};'],
    ['c_dir_char',         '    char dChar[4] = {\'D\', \'L\', \'R\', \'U\'};'],
    ['c_loop_dirs',        '    for (int i = 0; i < 4; i++) {'],
    ['c_calc_next',        '        int nextR = r + dRow[i];'],
    ['c_calc_col',         '        int nextC = c + dCol[i];'],
    ['c_check_safe',       '        if (isSafe(nextR, nextC, maze, rows, cols, visited)) {'],
    ['c_recurse',          '            solve(nextR, nextC, maze, rows, cols, visited, path + dChar[i], paths);'],
    ['',                   '        }'],
    ['',                   '        }'],
    ['c_backtrack',        '    visited[r][c] = false;'],
    ['',                   '}'],
    ['',                   ''],
    ['c_safe_fn',          'bool isSafe(int r, int c, const vector<vector<int>>& maze, int rows, int cols, const vector<vector<bool>>& visited) {'],
    ['c_safe_bounds',      '    if (r < 0 || r >= rows || c < 0 || c >= cols) {'],
    ['c_safe_bounds_ret',  '        return false;'],
    ['',                   '    }'],
    ['c_safe_wall',        '    if (maze[r][c] == 0) {'],
    ['c_safe_wall_ret',    '        return false;'],
    ['',                   '    }'],
    ['c_safe_visited',     '    if (visited[r][c]) {'],
    ['c_safe_visited_ret', '        return false;'],
    ['',                   '    }'],
    ['c_safe_ret_true',    '    return true;'],
    ['',                   '}'],
    ['',                   ''],
    ['c_entry',            'vector<string> findPaths(const vector<vector<int>>& maze, int rows, int cols) {'],
    ['c_init_paths',       '    vector<string> paths;'],
    ['c_chk_start',        '    if (maze[0][0] == 0 || maze[rows - 1][cols - 1] == 0) {'],
    ['c_chk_start_ret',    '        return paths;'],
    ['',                   '    }'],
    ['c_init_visited',     '    vector<vector<bool>> visited(rows, vector<bool>(cols, false));'],
    ['c_call_solve',       '    solve(0, 0, maze, rows, cols, visited, "", paths);'],
    ['c_sort_paths',       '    sort(paths.begin(), paths.end());'],
    ['c_return_paths',     '    return paths;'],
    ['',                   '}'],
    ['',                   ''],
    ['',                   'int main() {'],
    ['m_scanner',          '    int rows;'],
    ['',                   '    int cols;'],
    ['m_read_dims',        '    cin >> rows >> cols;'],
    ['m_alloc_maze',       '    vector<vector<int>> maze(rows, vector<int>(cols));'],
    ['m_read_maze',        '    for (int i = 0; i < rows; i++) {'],
    ['m_read_maze_inner',  '        for (int j = 0; j < cols; j++) {'],
    ['m_read_maze_body',   '            cin >> maze[i][j];'],
    ['',                   '        }'],
    ['',                   '    }'],
    ['m_call_solver',      '    vector<string> paths = findPaths(maze, rows, cols);'],
    ['m_print_count',      '    cout << paths.size() << "\\n";'],
    ['m_print_paths',      '    for (int i = 0; i < (int)paths.size(); i++) {'],
    ['m_print_paths_body', '        cout << paths[i] << "\\n";'],
    ['',                   '    }'],
    ['m_done',             '    return 0;'],
    ['',                   '}']
  ],
  python: [
    ['',                   'import sys'],
    ['',                   ''],
    ['c_safe_fn',          'def is_safe(r, c, maze, rows, cols, visited):'],
    ['c_safe_bounds',      '    if r < 0 or r >= rows or c < 0 or c >= cols:'],
    ['c_safe_bounds_ret',  '        return False'],
    ['c_safe_wall',        '    if maze[r][c] == 0:'],
    ['c_safe_wall_ret',    '        return False'],
    ['c_safe_visited',     '    if visited[r][c]:'],
    ['c_safe_visited_ret', '        return False'],
    ['c_safe_ret_true',    '    return True'],
    ['',                   ''],
    ['c_solve_entry',      'def solve(r, c, maze, rows, cols, visited, path, paths):'],
    ['c_base_check',       '    if r == rows - 1 and c == cols - 1:'],
    ['c_save_sol',         '        paths.append(path)'],
    ['c_base_ret',         '        return'],
    ['c_mark_vis',         '    visited[r][c] = True'],
    ['c_dir_init',         '    d_row = [1, 0, 0, -1]'],
    ['c_dir_col',          '    d_col = [0, -1, 1, 0]'],
    ['c_dir_char',         '    d_char = [\'D\', \'L\', \'R\', \'U\']'],
    ['c_loop_dirs',        '    for i in range(4):'],
    ['c_calc_next',        '        next_r = r + d_row[i]'],
    ['c_calc_col',         '        next_c = c + d_col[i]'],
    ['c_check_safe',       '        if is_safe(next_r, next_c, maze, rows, cols, visited):'],
    ['c_recurse',          '            solve(next_r, next_c, maze, rows, cols, visited, path + d_char[i], paths)'],
    ['c_backtrack',        '    visited[r][c] = False'],
    ['',                   ''],
    ['c_entry',            'def find_paths(maze, rows, cols):'],
    ['c_init_paths',       '    paths = []'],
    ['c_chk_start',        '    if maze[0][0] == 0 or maze[rows - 1][cols - 1] == 0:'],
    ['c_chk_start_ret',    '        return paths'],
    ['c_init_visited',     '    visited = [[False for _ in range(cols)] for _ in range(rows)]'],
    ['c_call_solve',       '    solve(0, 0, maze, rows, cols, visited, "", paths)'],
    ['c_sort_paths',       '    paths.sort()'],
    ['c_return_paths',     '    return paths'],
    ['',                   ''],
    ['',                   'def main():'],
    ['m_scanner',          '    input_data = sys.stdin.read().split()'],
    ['',                   '    if not input_data:'],
    ['',                   '        return'],
    ['m_read_dims',        '    rows = int(input_data[0])'],
    ['m_read_cols',         '    cols = int(input_data[1])'],
    ['m_alloc_maze',       '    maze = []'],
    ['m_read_maze',        '    idx = 2'],
    ['m_read_maze_inner',  '    for i in range(rows):'],
    ['m_read_maze_body',   '        row_vals = []'],
    ['',                   '        for j in range(cols):'],
    ['',                   '            row_vals.append(int(input_data[idx]))'],
    ['',                   '            idx += 1'],
    ['',                   '        maze.append(row_vals)'],
    ['m_call_solver',      '    paths = find_paths(maze, rows, cols)'],
    ['m_print_count',      '    print(len(paths))'],
    ['m_print_paths',      '    for p in paths:'],
    ['m_print_paths_body', '        print(p)'],
    ['m_done',             ''],
    ['',                   'if __name__ == "__main__":'],
    ['',                   '    main()']
  ],
  javascript: [
    ['c_solve_entry',      'function solve(r, c, maze, rows, cols, visited, path, paths) {'],
    ['c_base_check',       '    if (r === rows - 1 && c === cols - 1) {'],
    ['c_save_sol',         '        paths.push(path);'],
    ['c_base_ret',         '        return;'],
    ['',                   '    }'],
    ['c_mark_vis',         '    visited[r][c] = true;'],
    ['c_dir_init',         '    const dRow = [1, 0, 0, -1];'],
    ['c_dir_col',          '    const dCol = [0, -1, 1, 0];'],
    ['c_dir_char',         '    const dChar = [\'D\', \'L\', \'R\', \'U\'];'],
    ['c_loop_dirs',        '    for (let i = 0; i < 4; i++) {'],
    ['c_calc_next',        '        const nextR = r + dRow[i];'],
    ['c_calc_col',         '        const nextC = c + dCol[i];'],
    ['c_check_safe',       '        if (isSafe(nextR, nextC, maze, rows, cols, visited)) {'],
    ['c_recurse',          '            solve(nextR, nextC, maze, rows, cols, visited, path + dChar[i], paths);'],
    ['',                   '        }'],
    ['',                   '    }'],
    ['c_backtrack',        '    visited[r][c] = false;'],
    ['',                   '}'],
    ['',                   ''],
    ['c_safe_fn',          'function isSafe(r, c, maze, rows, cols, visited) {'],
    ['c_safe_bounds',      '    if (r < 0 || r >= rows || c < 0 || c >= cols) {'],
    ['c_safe_bounds_ret',  '        return false;'],
    ['',                   '    }'],
    ['c_safe_wall',        '    if (maze[r][c] === 0) {'],
    ['c_safe_wall_ret',    '        return false;'],
    ['',                   '    }'],
    ['c_safe_visited',     '    if (visited[r][c]) {'],
    ['c_safe_visited_ret', '        return false;'],
    ['',                   '    }'],
    ['c_safe_ret_true',    '    return true;'],
    ['',                   '}'],
    ['',                   ''],
    ['c_entry',            'function findPaths(maze, rows, cols) {'],
    ['c_init_paths',       '    const paths = [];'],
    ['c_chk_start',        '    if (maze[0][0] === 0 || maze[rows - 1][cols - 1] === 0) {'],
    ['c_chk_start_ret',    '        return paths;'],
    ['',                   '    }'],
    ['c_init_visited',     '    const visited = Array.from({ length: rows }, () => Array(cols).fill(false));'],
    ['c_call_solve',       '    solve(0, 0, maze, rows, cols, visited, "", paths);'],
    ['c_sort_paths',       '    paths.sort();'],
    ['c_return_paths',     '    return paths;'],
    ['',                   '}'],
    ['',                   ''],
    ['m_scanner',          'const rows = 4;'],
    ['m_read_dims',        'const cols = 4;'],
    ['m_alloc_maze',       'const maze = ['],
    ['m_read_maze',        '    [1, 0, 0, 0], [1, 1, 0, 1], [1, 1, 0, 0], [0, 1, 1, 1]'],
    ['',                   '];'],
    ['m_call_solver',      'const paths = findPaths(maze, rows, cols);'],
    ['m_print_count',      'console.log(paths.length);'],
    ['m_print_paths',      'for (let i = 0; i < paths.length; i++) {'],
    ['m_print_paths_body', '    console.log(paths[i]);'],
    ['',                   '}'],
    ['m_done',             '']
  ]
};

const PSEUDOCODE = [
  'function findPaths(maze, rows, cols):',
  '    if maze[0][0] == 0 or maze[rows - 1][cols - 1] == 0:',
  '        return []',
  '    visited = 2D boolean array of size rows x cols initialized to false',
  '    paths = []',
  '    solve(0, 0, maze, rows, cols, visited, "", paths)',
  '    sort(paths)',
  '    return paths',
  '',
  'function solve(r, c, maze, rows, cols, visited, path, paths):',
  '    if r == rows - 1 and c == cols - 1:',
  '        paths.append(path)',
  '        return',
  '    visited[r][c] = true',
  '    dRow = [1, 0, 0, -1]    // D, L, R, U movements',
  '    dCol = [0, -1, 1, 0]',
  '    dChar = ["D", "L", "R", "U"]',
  '    for i from 0 to 3:',
  '        nextR = r + dRow[i]',
  '        nextC = c + dCol[i]',
  '        if isSafe(nextR, nextC, maze, rows, cols, visited):',
  '            solve(nextR, nextC, maze, rows, cols, visited, path + dChar[i], paths)',
  '    visited[r][c] = false   // Backtrack',
  '',
  'function isSafe(r, c, maze, rows, cols, visited):',
  '    if r < 0 or r >= rows or c < 0 or c >= cols: return false',
  '    if maze[r][c] == 0: return false',
  '    if visited[r][c]: return false',
  '    return true'
];

function buildSteps(rows, cols, maze) {
  const steps = [];

  const frame = (title, rowsArr) => ({ title, rows: rowsArr });

  function cloneGrid(curR = -1, curC = -1, visitedMatrix = null, testR = -1, testC = -1, testStatus = 'normal', solPathCells = null) {
    const res = [];
    for (let i = 0; i < rows; i++) {
      const rowList = [];
      for (let j = 0; j < cols; j++) {
        const isWall = (maze[i][j] === 0);
        const isStart = (i === 0 && j === 0);
        const isGoal = (i === rows - 1 && j === cols - 1);
        const hasRat = (i === curR && j === curC);
        const isVisited = visitedMatrix ? visitedMatrix[i][j] : false;
        const isTesting = (i === testR && j === testC);
        const isSol = solPathCells ? solPathCells.some(pt => pt.r === i && pt.c === j) : false;

        let state = 'open';
        if (isWall) state = 'wall';
        else if (isSol) state = 'solution';
        else if (hasRat) state = 'rat';
        else if (isTesting) state = (testStatus === 'conflict' ? 'conflict' : 'testing');
        else if (isVisited) state = 'visited';

        rowList.push({
          r: i,
          c: j,
          val: maze[i][j],
          isWall,
          isStart,
          isGoal,
          hasRat,
          isVisited,
          state
        });
      }
      res.push(rowList);
    }
    return res;
  }

  // Initial visited matrix
  const visited = Array.from({ length: rows }, () => Array(cols).fill(false));
  const solutions = [];

  // ─── Main: Scanner setup ──────────────────────────────────────────────────
  steps.push({
    badge: `Scanner sc = new Scanner(System.in); → Preparing standard input reader.`,
    code: 'm_scanner',
    vars: [frame('main()', [['rows', '?'], ['cols', '?']])],
    grid: cloneGrid(),
    rows, cols, loopI: null, loopJ: null, r: -1, c: -1, dir: '', path: '', solCount: 0, solutions: []
  });

  // ─── Main: Read rows ───────────────────────────────────────────────────────
  steps.push({
    badge: `int rows = sc.nextInt(); → Reading number of rows = ${rows}.`,
    code: 'm_read_dims',
    vars: [frame('main()', [['rows', String(rows)], ['cols', '?']])],
    grid: cloneGrid(),
    rows, cols, loopI: null, loopJ: null, r: -1, c: -1, dir: '', path: '', solCount: 0, solutions: []
  });

  // ─── Main: Read cols ───────────────────────────────────────────────────────
  steps.push({
    badge: `int cols = sc.nextInt(); → Reading number of columns = ${cols}.`,
    code: 'm_read_cols',
    vars: [frame('main()', [['rows', String(rows)], ['cols', String(cols)]])],
    grid: cloneGrid(),
    rows, cols, loopI: null, loopJ: null, r: -1, c: -1, dir: '', path: '', solCount: 0, solutions: []
  });

  // ─── Main: Alloc maze ─────────────────────────────────────────────────────
  steps.push({
    badge: `int[][] maze = new int[${rows}][${cols}]; → Allocating ${rows}×${cols} grid memory.`,
    code: 'm_alloc_maze',
    vars: [frame('main()', [['rows', String(rows)], ['cols', String(cols)]])],
    grid: cloneGrid(),
    rows, cols, loopI: null, loopJ: null, r: -1, c: -1, dir: '', path: '', solCount: 0, solutions: []
  });

  // ─── Main: Read maze – per-iteration nested for loop ─────────────────────
  for (let _ri = 0; _ri < rows; _ri++) {
    // Outer loop condition: i < rows → TRUE
    steps.push({
      badge: `for (int i = ${_ri}; i < ${rows}; i++) → (${_ri} < ${rows}) is TRUE. Processing row ${_ri}.`,
      code: 'm_read_maze',
      vars: [frame('main()', [['i', String(_ri)], ['j', '?']])],
      grid: cloneGrid(),
      rows, cols, loopI: _ri, loopJ: null, r: -1, c: -1, dir: '', path: '', solCount: 0, solutions: []
    });
    for (let _ci = 0; _ci < cols; _ci++) {
      // Inner loop condition: j < cols → TRUE
      steps.push({
        badge: `for (int j = ${_ci}; j < ${cols}; j++) → (${_ci} < ${cols}) is TRUE. Will read maze[${_ri}][${_ci}].`,
        code: 'm_read_maze_inner',
        vars: [frame('main()', [['i', String(_ri)], ['j', String(_ci)]])],
        grid: cloneGrid(),
        rows, cols, loopI: _ri, loopJ: _ci, r: -1, c: -1, dir: '', path: '', solCount: 0, solutions: []
      });
      // Body: maze[i][j] = sc.nextInt()
      steps.push({
        badge: `maze[${_ri}][${_ci}] = sc.nextInt(); → Reading value ${maze[_ri][_ci]} into cell [${_ri}][${_ci}].`,
        code: 'm_read_maze_body',
        vars: [frame('main()', [['i', String(_ri)], ['j', String(_ci)], [`maze[${_ri}][${_ci}]`, String(maze[_ri][_ci])]])],
        grid: cloneGrid(),
        rows, cols, loopI: _ri, loopJ: _ci, r: -1, c: -1, dir: '', path: '', solCount: 0, solutions: []
      });
    }
    // Inner loop condition: j = cols → FALSE
    steps.push({
      badge: `for (int j = ${cols}; j < ${cols}; j++) → (${cols} < ${cols}) is FALSE. Row ${_ri} fully read.`,
      code: 'm_read_maze_inner',
      vars: [frame('main()', [['i', String(_ri)], ['j', String(cols)], ['inner_loop', 'EXIT']])],
      grid: cloneGrid(),
      rows, cols, loopI: _ri, loopJ: cols, r: -1, c: -1, dir: '', path: '', solCount: 0, solutions: []
    });
  }
  // Outer loop condition: i = rows → FALSE
  steps.push({
    badge: `for (int i = ${rows}; i < ${rows}; i++) → (${rows} < ${rows}) is FALSE. Entire maze read successfully.`,
    code: 'm_read_maze',
    vars: [frame('main()', [['i', String(rows)], ['outer_loop', 'EXIT']])],
    grid: cloneGrid(),
    rows, cols, loopI: rows, loopJ: null, r: -1, c: -1, dir: '', path: '', solCount: 0, solutions: []
  });

  steps.push({
    badge: `List<String> paths = findPaths(maze, ${rows}, ${cols}); → Invoking path finder.`,
    code: 'm_call_solver',
    vars: [frame('main()', [['rows', String(rows)], ['cols', String(cols)]])],
    grid: cloneGrid(),
    rows, cols, loopI: null, loopJ: null, r: -1, c: -1, dir: '', path: '', solCount: 0, solutions: []
  });

  steps.push({
    badge: `findPaths(maze, rows=${rows}, cols=${cols}) entry → Initializing path collection container.`,
    code: 'c_entry',
    vars: [frame('main()', [['rows', String(rows)], ['cols', String(cols)]]), frame('findPaths()', [['rows', String(rows)], ['cols', String(cols)]])],
    grid: cloneGrid(),
    rows, cols, loopI: null, loopJ: null, r: -1, c: -1, dir: '', path: '', solCount: 0, solutions: []
  });

  steps.push({
    badge: `List<String> paths = new ArrayList<>(); → Container created.`,
    code: 'c_init_paths',
    vars: [frame('main()', [['rows', String(rows)], ['cols', String(cols)]]), frame('findPaths()', [['paths_found', '0']])],
    grid: cloneGrid(),
    rows, cols, loopI: null, loopJ: null, r: -1, c: -1, dir: '', path: '', solCount: 0, solutions: []
  });

  // Start & Goal validation
  const startBlocked = (maze[0][0] === 0 || maze[rows - 1][cols - 1] === 0);
  steps.push({
    badge: `Checking start/goal validity: if (maze[0][0] == 0 || maze[${rows - 1}][${cols - 1}] == 0) → ${startBlocked ? 'TRUE (Blocked!)' : 'FALSE (Start & Goal are open!)'}`,
    code: 'c_chk_start',
    vars: [frame('main()', [['rows', String(rows)], ['cols', String(cols)]]), frame('findPaths()', [['start_val', String(maze[0][0])], ['goal_val', String(maze[rows - 1][cols - 1])]])],
    grid: cloneGrid(),
    rows, cols, loopI: null, loopJ: null, r: -1, c: -1, dir: '', path: '', solCount: 0, solutions: []
  });

  if (startBlocked) {
    steps.push({
      badge: `❌ Start (0, 0) or Goal (${rows - 1}, ${cols - 1}) is blocked by a wall! Returning empty path list.`,
      code: 'c_chk_start_ret',
      vars: [frame('main()', [['rows', String(rows)], ['cols', String(cols)]]), frame('findPaths()', [['result', 'empty']])],
      grid: cloneGrid(),
      rows, cols, loopI: null, loopJ: null, r: -1, c: -1, dir: '', path: '', solCount: 0, solutions: []
    });
    return steps;
  }

  steps.push({
    badge: `boolean[][] visited = new boolean[${rows}][${cols}]; → Initialized visited state tracker.`,
    code: 'c_init_visited',
    vars: [frame('main()', [['rows', String(rows)], ['cols', String(cols)]]), frame('findPaths()', [['visited', `${rows}x${cols}`]])],
    grid: cloneGrid(),
    rows, cols, loopI: null, loopJ: null, r: -1, c: -1, dir: '', path: '', solCount: 0, solutions: []
  });

  steps.push({
    badge: `solve(0, 0, maze, rows, cols, visited, "", paths); → Starting backtracking search from cell (0, 0).`,
    code: 'c_call_solve',
    vars: [frame('main()', [['rows', String(rows)], ['cols', String(cols)]]), frame('findPaths()', [['r', '0'], ['c', '0']])],
    grid: cloneGrid(0, 0, visited),
    rows, cols, loopI: null, loopJ: null, r: 0, c: 0, dir: '', path: '', solCount: 0, solutions: []
  });

  const callStackFrames = [];
  const currentPathCoords = [];

  function getStackSnapshot(extraRows = []) {
    const list = [frame('main()', [['rows', String(rows)], ['cols', String(cols)]])];
    for (const f of callStackFrames) {
      list.push(frame(f.name, f.args));
    }
    if (extraRows.length) {
      const top = list[list.length - 1];
      if (top) {
        top.rows = [...top.rows, ...extraRows];
      }
    }
    return list;
  }

  const dRow = [1, 0, 0, -1];
  const dCol = [0, -1, 1, 0];
  const dChar = ['D', 'L', 'R', 'U'];
  const dNames = ['Down', 'Left', 'Right', 'Up'];

  function simulateSolve(r, c, path) {
    callStackFrames.push({
      name: `solve(r=${r}, c=${c})`,
      args: [['r', String(r)], ['c', String(c)], ['path', `"${path}"`]]
    });
    currentPathCoords.push({ r, c });

    // Entry step
    steps.push({
      badge: `solve(r=${r}, c=${c}) → Rat is now at square (${r}, ${c}). Current path: "${path}".`,
      code: 'c_solve_entry',
      vars: getStackSnapshot(),
      grid: cloneGrid(r, c, visited),
      rows, cols, loopI: null, loopJ: null, r, c, dir: '', path, solCount: solutions.length, solutions: [...solutions]
    });

    // Base condition check
    const isGoal = (r === rows - 1 && c === cols - 1);
    steps.push({
      badge: `Checking destination: if (r == rows - 1 && c == cols - 1) → (${r} == ${rows - 1} && ${c} == ${cols - 1}) → ${isGoal ? 'TRUE! DESTINATION REACHED!' : 'FALSE (continue path search)'}`,
      code: 'c_base_check',
      vars: getStackSnapshot(),
      grid: cloneGrid(r, c, visited),
      rows, cols, loopI: null, loopJ: null, r, c, dir: '', path, solCount: solutions.length, solutions: [...solutions]
    });

    if (isGoal) {
      solutions.push({ path, cells: [...currentPathCoords] });
      steps.push({
        badge: `🎉 PATH FOUND! Path #${solutions.length}: "${path}". Adding to path list.`,
        code: 'c_save_sol',
        vars: getStackSnapshot([['status', 'FOUND_PATH']]),
        grid: cloneGrid(r, c, visited, -1, -1, 'normal', [...currentPathCoords]),
        rows, cols, loopI: null, loopJ: null, r, c, dir: '', path, solCount: solutions.length, solutions: [...solutions]
      });

      steps.push({
        badge: `return; → Returning from destination square (${r}, ${c}) to explore alternate branches.`,
        code: 'c_base_ret',
        vars: getStackSnapshot(),
        grid: cloneGrid(r, c, visited),
        rows, cols, loopI: null, loopJ: null, r, c, dir: '', path, solCount: solutions.length, solutions: [...solutions]
      });

      callStackFrames.pop();
      currentPathCoords.pop();
      return;
    }

    // Mark visited
    visited[r][c] = true;
    steps.push({
      badge: `visited[${r}][${c}] = true; → Marking cell (${r}, ${c}) as visited to avoid cycles.`,
      code: 'c_mark_vis',
      vars: getStackSnapshot(),
      grid: cloneGrid(r, c, visited),
      rows, cols, loopI: null, loopJ: null, r, c, dir: '', path, solCount: solutions.length, solutions: [...solutions]
    });

    // ── dRow initialization ────────────────────────────────────────────────
    steps.push({
      badge: `int[] dRow = {1, 0, 0, -1}; → Row offsets for Down(+1), Left(0), Right(0), Up(-1).`,
      code: 'c_dir_init',
      vars: getStackSnapshot([['dRow', '{1, 0, 0, -1}']]),
      grid: cloneGrid(r, c, visited),
      rows, cols, loopI: null, loopJ: null, r, c, dir: '', path, solCount: solutions.length, solutions: [...solutions]
    });

    // ── dCol initialization ────────────────────────────────────────────────
    steps.push({
      badge: `int[] dCol = {0, -1, 1, 0}; → Column offsets for Down(0), Left(-1), Right(+1), Up(0).`,
      code: 'c_dir_col',
      vars: getStackSnapshot([['dCol', '{0, -1, 1, 0}']]),
      grid: cloneGrid(r, c, visited),
      rows, cols, loopI: null, loopJ: null, r, c, dir: '', path, solCount: solutions.length, solutions: [...solutions]
    });

    // ── dChar initialization ───────────────────────────────────────────────
    steps.push({
      badge: `char[] dChar = {'D','L','R','U'}; → Direction labels: Down, Left, Right, Up.`,
      code: 'c_dir_char',
      vars: getStackSnapshot([['dChar', "{'D','L','R','U'}"]]),
      grid: cloneGrid(r, c, visited),
      rows, cols, loopI: null, loopJ: null, r, c, dir: '', path, solCount: solutions.length, solutions: [...solutions]
    });

    // Try all 4 directions
    for (let i = 0; i < 4; i++) {
      const dirName = dNames[i];
      const dirChar = dChar[i];
      const nextR = r + dRow[i];
      const nextC = c + dCol[i];

      // Loop header check: condition check should NOT be skipped!
      steps.push({
        badge: `Direction loop: for (int i = ${i}; i < 4; i++) → (${i} < 4) is TRUE. Exploring direction '${dirChar}' (${dirName}).`,
        code: 'c_loop_dirs',
        vars: getStackSnapshot([['i', String(i)], ['dir', `'${dirChar}'`]]),
        grid: cloneGrid(r, c, visited),
        rows, cols, loopI: i, loopJ: null, r, c, dir: dirChar, path, solCount: solutions.length, solutions: [...solutions]
      });

      // ── nextR = r + dRow[i] ───────────────────────────────────────────────
      steps.push({
        badge: `int nextR = ${r} + dRow[${i}] = ${r} + (${dRow[i]}) = ${nextR};`,
        code: 'c_calc_next',
        vars: getStackSnapshot([['i', String(i)], ['nextR', String(nextR)]]),
        grid: cloneGrid(r, c, visited),
        rows, cols, loopI: i, loopJ: null, r, c, dir: dirChar, path, solCount: solutions.length, solutions: [...solutions]
      });

      // ── nextC = c + dCol[i] ───────────────────────────────────────────────
      steps.push({
        badge: `int nextC = ${c} + dCol[${i}] = ${c} + (${dCol[i]}) = ${nextC}; → Target square is (${nextR}, ${nextC}).`,
        code: 'c_calc_col',
        vars: getStackSnapshot([['i', String(i)], ['nextR', String(nextR)], ['nextC', String(nextC)]]),
        grid: cloneGrid(r, c, visited, nextR >= 0 && nextR < rows && nextC >= 0 && nextC < cols ? nextR : -1, nextR >= 0 && nextR < rows && nextC >= 0 && nextC < cols ? nextC : -1, 'testing'),
        rows, cols, loopI: i, loopJ: null, r, c, dir: dirChar, path, solCount: solutions.length, solutions: [...solutions]
      });

      steps.push({
        badge: `if (isSafe(nextR=${nextR}, nextC=${nextC}, maze, rows, cols, visited)) → Calling isSafe() to validate target square.`,
        code: 'c_check_safe',
        vars: getStackSnapshot([['nextR', String(nextR)], ['nextC', String(nextC)]]),
        grid: cloneGrid(r, c, visited, nextR, nextC, 'testing'),
        rows, cols, loopI: i, loopJ: null, r, c, dir: dirChar, path, solCount: solutions.length, solutions: [...solutions]
      });

      // Execute isSafe step-by-step
      steps.push({
        badge: `isSafe(r=${nextR}, c=${nextC}) entry → Validating boundaries, walls, and visited status.`,
        code: 'c_safe_fn',
        vars: getStackSnapshot([['safe_r', String(nextR)], ['safe_c', String(nextC)]]),
        grid: cloneGrid(r, c, visited, nextR, nextC, 'testing'),
        rows, cols, loopI: i, loopJ: null, r, c, dir: dirChar, path, solCount: solutions.length, solutions: [...solutions]
      });

      // 1. Boundary check
      const outOfBounds = (nextR < 0 || nextR >= rows || nextC < 0 || nextC >= cols);
      steps.push({
        badge: `Boundary check: if (${nextR} < 0 || ${nextR} >= ${rows} || ${nextC} < 0 || ${nextC} >= ${cols}) → ${outOfBounds ? 'TRUE (OutOfBounds!)' : 'FALSE (Inside maze boundaries)'}`,
        code: 'c_safe_bounds',
        vars: getStackSnapshot([['bounds_check', outOfBounds ? 'INVALID' : 'VALID']]),
        grid: cloneGrid(r, c, visited, nextR, nextC, outOfBounds ? 'conflict' : 'testing'),
        rows, cols, loopI: i, loopJ: null, r, c, dir: dirChar, path, solCount: solutions.length, solutions: [...solutions]
      });

      if (outOfBounds) {
        steps.push({
          badge: `❌ Target square (${nextR}, ${nextC}) is outside the maze! Executing return false;`,
          code: 'c_safe_bounds_ret',
          vars: getStackSnapshot([['result', 'false (OutOfBounds)']]),
          grid: cloneGrid(r, c, visited, nextR, nextC, 'conflict'),
          rows, cols, loopI: i, loopJ: null, r, c, dir: dirChar, path, solCount: solutions.length, solutions: [...solutions]
        });
        continue;
      }

      // 2. Wall check
      const isWall = (maze[nextR][nextC] === 0);
      steps.push({
        badge: `Wall check: if (maze[${nextR}][${nextC}] == 0) → Cell is ${isWall ? 'a Wall (0) → TRUE' : 'an Open path (1) → FALSE'}`,
        code: 'c_safe_wall',
        vars: getStackSnapshot([['wall_val', String(maze[nextR][nextC])]]),
        grid: cloneGrid(r, c, visited, nextR, nextC, isWall ? 'conflict' : 'testing'),
        rows, cols, loopI: i, loopJ: null, r, c, dir: dirChar, path, solCount: solutions.length, solutions: [...solutions]
      });

      if (isWall) {
        steps.push({
          badge: `❌ Target square (${nextR}, ${nextC}) is a blocked wall (0)! Executing return false;`,
          code: 'c_safe_wall_ret',
          vars: getStackSnapshot([['result', 'false (Wall)']]),
          grid: cloneGrid(r, c, visited, nextR, nextC, 'conflict'),
          rows, cols, loopI: i, loopJ: null, r, c, dir: dirChar, path, solCount: solutions.length, solutions: [...solutions]
        });
        continue;
      }

      // 3. Visited check
      const isAlreadyVisited = visited[nextR][nextC];
      steps.push({
        badge: `Visited check: if (visited[${nextR}][${nextC}]) → ${isAlreadyVisited ? 'TRUE (Already visited in current path!)' : 'FALSE (Unvisited square)'}`,
        code: 'c_safe_visited',
        vars: getStackSnapshot([['visited_val', String(isAlreadyVisited)]]),
        grid: cloneGrid(r, c, visited, nextR, nextC, isAlreadyVisited ? 'conflict' : 'testing'),
        rows, cols, loopI: i, loopJ: null, r, c, dir: dirChar, path, solCount: solutions.length, solutions: [...solutions]
      });

      if (isAlreadyVisited) {
        steps.push({
          badge: `❌ Square (${nextR}, ${nextC}) is already in current path! Executing return false;`,
          code: 'c_safe_visited_ret',
          vars: getStackSnapshot([['result', 'false (AlreadyVisited)']]),
          grid: cloneGrid(r, c, visited, nextR, nextC, 'conflict'),
          rows, cols, loopI: i, loopJ: null, r, c, dir: dirChar, path, solCount: solutions.length, solutions: [...solutions]
        });
        continue;
      }

      // All checks passed
      steps.push({
        badge: `✅ All safety checks passed! Square (${nextR}, ${nextC}) is an open, unvisited cell. Executing return true;`,
        code: 'c_safe_ret_true',
        vars: getStackSnapshot([['result', 'true (Safe)']]),
        grid: cloneGrid(r, c, visited, nextR, nextC, 'testing'),
        rows, cols, loopI: i, loopJ: null, r, c, dir: dirChar, path, solCount: solutions.length, solutions: [...solutions]
      });

      // Recurse forward
      steps.push({
        badge: `Stepping forward: solve(nextR=${nextR}, nextC=${nextC}, path="${path + dirChar}");`,
        code: 'c_recurse',
        vars: getStackSnapshot([['next_step', `'${dirChar}'`]]),
        grid: cloneGrid(r, c, visited),
        rows, cols, loopI: i, loopJ: null, r, c, dir: dirChar, path: path + dirChar, solCount: solutions.length, solutions: [...solutions]
      });

      simulateSolve(nextR, nextC, path + dirChar);
    }

    // Direction loop exit check (i = 4 is not < 4)
    steps.push({
      badge: `Direction loop finished: for (int i = 4; i < 4; i++) → (4 < 4) is FALSE. All 4 directions explored from (${r}, ${c}). Exiting loop.`,
      code: 'c_loop_dirs',
      vars: getStackSnapshot([['i', '4'], ['loop', 'TERMINATED']]),
      grid: cloneGrid(r, c, visited),
      rows, cols, loopI: 4, loopJ: null, r, c, dir: '', path, solCount: solutions.length, solutions: [...solutions]
    });

    // Backtrack
    visited[r][c] = false;
    steps.push({
      badge: `visited[${r}][${c}] = false; → Backtracking: Unmarking cell (${r}, ${c}) to allow other paths.`,
      code: 'c_backtrack',
      vars: getStackSnapshot(),
      grid: cloneGrid(r, c, visited),
      rows, cols, loopI: null, loopJ: null, r, c, dir: '', path, solCount: solutions.length, solutions: [...solutions]
    });

    callStackFrames.pop();
    currentPathCoords.pop();
  }

  simulateSolve(0, 0, "");

  steps.push({
    badge: `Collections.sort(paths); → Sorting all ${solutions.length} discovered paths lexicographically.`,
    code: 'c_sort_paths',
    vars: [frame('main()', [['rows', String(rows)], ['cols', String(cols)]]), frame('findPaths()', [['total_paths', String(solutions.length)]])],
    grid: cloneGrid(),
    rows, cols, loopI: null, loopJ: null, r: -1, c: -1, dir: '', path: '', solCount: solutions.length, solutions: [...solutions]
  });

  steps.push({
    badge: `return paths; → Returning ${solutions.length} valid paths to main().`,
    code: 'c_return_paths',
    vars: [frame('main()', [['rows', String(rows)], ['cols', String(cols)]]), frame('findPaths()', [['total_paths', String(solutions.length)]])],
    grid: cloneGrid(),
    rows, cols, loopI: null, loopJ: null, r: -1, c: -1, dir: '', path: '', solCount: solutions.length, solutions: [...solutions]
  });

  steps.push({
    badge: `System.out.println(paths.size()); → Printing total path count: ${solutions.length}.`,
    code: 'm_print_count',
    vars: [frame('main()', [['total_paths', String(solutions.length)]])],
    grid: cloneGrid(),
    rows, cols, loopI: null, loopJ: null, r: -1, c: -1, dir: '', path: '', solCount: solutions.length, solutions: [...solutions]
  });

  // ─── Print each path – per-iteration for loop ────────────────────────────
  for (let _pi = 0; _pi < solutions.length; _pi++) {
    // Loop condition: i < paths.size() → TRUE
    steps.push({
      badge: `for (int i = ${_pi}; i < ${solutions.length}; i++) → (${_pi} < ${solutions.length}) is TRUE. Will print paths.get(${_pi}).`,
      code: 'm_print_paths',
      vars: [frame('main()', [['i', String(_pi)]])],
      grid: cloneGrid(),
      rows, cols, loopI: _pi, loopJ: null, r: -1, c: -1, dir: '', path: solutions[_pi].path, solCount: solutions.length, solutions: [...solutions]
    });
    // Body: System.out.println(paths.get(i))
    steps.push({
      badge: `System.out.println(paths.get(${_pi})); → Printing: "${solutions[_pi].path}"`,
      code: 'm_print_paths_body',
      vars: [frame('main()', [['i', String(_pi)], ['output', `"${solutions[_pi].path}"`]])],
      grid: cloneGrid(),
      rows, cols, loopI: _pi, loopJ: null, r: -1, c: -1, dir: '', path: solutions[_pi].path, solCount: solutions.length, solutions: [...solutions]
    });
  }
  if (solutions.length > 0) {
    steps.push({
      badge: `for (int i = ${solutions.length}; i < ${solutions.length}; i++) → (${solutions.length} < ${solutions.length}) is FALSE. All paths printed.`,
      code: 'm_print_paths',
      vars: [frame('main()', [['i', String(solutions.length)], ['loop_status', 'DONE']])],
      grid: cloneGrid(),
      rows, cols, loopI: solutions.length, loopJ: null, r: -1, c: -1, dir: '', path: '', solCount: solutions.length, solutions: [...solutions]
    });
  }

  steps.push({
    badge: `Program execution complete. Rat in a Maze backtracking found ${solutions.length} unique path(s).`,
    code: 'm_done',
    vars: [frame('main()', [['status', 'finished'], ['paths_found', String(solutions.length)]])],
    grid: cloneGrid(),
    rows, cols, loopI: null, loopJ: null, r: -1, c: -1, dir: '', path: '', solCount: solutions.length, solutions: [...solutions]
  });

  return steps;
}

const inputRows = ref(4);
const inputCols = ref(4);
const mazeInputStr = ref('[[1, 0, 0, 0], [1, 1, 0, 1], [1, 1, 0, 0], [0, 1, 1, 1]]');
const activeMaze = ref([
  [1, 0, 0, 0],
  [1, 1, 0, 1],
  [1, 1, 0, 0],
  [0, 1, 1, 1]
]);

const lang = ref('java');
const speed = ref(650);
const si = ref(0);
const playing = ref(false);
const vizHeight = ref(290);
const tableHeight = ref(60);
const leftWidth = ref(50);
const rightTab = ref('code');
const selectedSolutionIdx = ref(-1);

const stepsData = reactive({ steps: buildSteps(4, 4, activeMaze.value) });
const steps = computed(() => stepsData.steps);
const s = computed(() => steps.value[Math.max(0, Math.min(si.value, steps.value.length - 1))] || {});
const codeLines = computed(() => CODES[lang.value] || []);

let playTimer = null;

function onChipsWheel(e) {
  if (e.currentTarget) {
    e.currentTarget.scrollLeft += e.deltaY;
  }
}

function parseMazeInput(str, expectedR, expectedC) {
  try {
    const parsed = JSON.parse(str);
    if (!Array.isArray(parsed) || parsed.length !== expectedR) return null;
    for (let i = 0; i < expectedR; i++) {
      if (!Array.isArray(parsed[i]) || parsed[i].length !== expectedC) return null;
      for (let j = 0; j < expectedC; j++) {
        if (parsed[i][j] !== 0 && parsed[i][j] !== 1) return null;
      }
    }
    return parsed;
  } catch (e) {
    return null;
  }
}

function applyInput() {
  const r = parseInt(inputRows.value, 10);
  const c = parseInt(inputCols.value, 10);
  if (isNaN(r) || r < 2 || r > 6 || isNaN(c) || c < 2 || c > 6) {
    alert('Please enter rows and columns between 2 and 6 for optimal visualization.');
    inputRows.value = 4;
    inputCols.value = 4;
    return;
  }

  let matrix = parseMazeInput(mazeInputStr.value, r, c);
  if (!matrix) {
    // If mismatch or invalid JSON, construct default open grid with some walls
    matrix = Array.from({ length: r }, () => Array(c).fill(1));
    matrix[0][0] = 1;
    matrix[r - 1][c - 1] = 1;
    mazeInputStr.value = JSON.stringify(matrix);
  }

  activeMaze.value = matrix;
  playing.value = false;
  selectedSolutionIdx.value = -1;
  stepsData.steps = buildSteps(r, c, activeMaze.value);
  si.value = 0;
  if (typeof window !== 'undefined') window.scrollTo(0, 0);
}

function toggleCell(r, c) {
  if (playing.value) return;
  const current = activeMaze.value[r][c];
  activeMaze.value[r][c] = (current === 1 ? 0 : 1);
  mazeInputStr.value = JSON.stringify(activeMaze.value);
  applyInput();
}

function loadPreset(presetType) {
  if (presetType === 'default') {
    inputRows.value = 4;
    inputCols.value = 4;
    mazeInputStr.value = '[[1, 0, 0, 0], [1, 1, 0, 1], [1, 1, 0, 0], [0, 1, 1, 1]]';
  } else if (presetType === '3x3') {
    inputRows.value = 3;
    inputCols.value = 3;
    mazeInputStr.value = '[[1, 0, 0], [1, 1, 0], [0, 1, 1]]';
  } else if (presetType === 'open') {
    inputRows.value = 3;
    inputCols.value = 3;
    mazeInputStr.value = '[[1, 1, 1], [1, 1, 1], [1, 1, 1]]';
  } else if (presetType === 'blocked') {
    inputRows.value = 3;
    inputCols.value = 3;
    mazeInputStr.value = '[[1, 0, 1], [0, 0, 1], [1, 1, 1]]';
  }
  applyInput();
}

function stepBy(d) {
  selectedSolutionIdx.value = -1;
  si.value = Math.max(0, Math.min(steps.value.length - 1, si.value + d));
}

function togglePlay() {
  selectedSolutionIdx.value = -1;
  const next = !playing.value;
  if (next && si.value >= steps.value.length - 1) si.value = 0;
  playing.value = next;
}

function tick() {
  clearTimeout(playTimer);
  if (!playing.value) return;
  if (si.value >= steps.value.length - 1) {
    playing.value = false;
    return;
  }
  playTimer = setTimeout(() => {
    si.value = Math.min(steps.value.length - 1, si.value + 1);
    tick();
  }, 2100 - speed.value);
}

watch(playing, v => {
  if (v) tick();
  else clearTimeout(playTimer);
});

const codeScrollRef = ref(null);

function scrollActiveCodeLine() {
  nextTick(() => {
    const container = codeScrollRef.value;
    if (!container) return;
    const activeEl = container.querySelector('.ll-hl');
    if (!activeEl) return;

    const contRect = container.getBoundingClientRect();
    const activeRect = activeEl.getBoundingClientRect();

    if (activeRect.top < contRect.top) {
      const diff = contRect.top - activeRect.top;
      container.scrollTop = Math.max(0, container.scrollTop - diff - 24);
    } else if (activeRect.bottom > contRect.bottom) {
      const diff = activeRect.bottom - contRect.bottom;
      container.scrollTop = container.scrollTop + diff + 24;
    }
  });
}

watch(() => s.value.code, scrollActiveCodeLine);
watch(lang, scrollActiveCodeLine);
watch(rightTab, v => {
  if (v === 'code') scrollActiveCodeLine();
});

function onKeydown(e) {
  const tag = e.target.tagName;
  if (tag === 'INPUT' || tag === 'SELECT' || tag === 'TEXTAREA') return;
  if (e.key === 'ArrowRight') stepBy(1);
  if (e.key === 'ArrowLeft') stepBy(-1);
  if (e.key === ' ') { e.preventDefault(); togglePlay(); }
}

function viewSolution(idx) {
  selectedSolutionIdx.value = idx;
  playing.value = false;
}

const displayGrid = computed(() => {
  if (selectedSolutionIdx.value >= 0 && s.value.solutions && s.value.solutions[selectedSolutionIdx.value]) {
    const sol = s.value.solutions[selectedSolutionIdx.value];
    const rCount = s.value.rows || inputRows.value;
    const cCount = s.value.cols || inputCols.value;
    const g = [];
    for (let i = 0; i < rCount; i++) {
      const rowList = [];
      for (let j = 0; j < cCount; j++) {
        const isWall = (activeMaze.value[i] && activeMaze.value[i][j] === 0);
        const inSol = sol.cells && sol.cells.some(pt => pt.r === i && pt.c === j);
        rowList.push({
          r: i,
          c: j,
          val: isWall ? 0 : 1,
          isWall,
          isStart: (i === 0 && j === 0),
          isGoal: (i === rCount - 1 && j === cCount - 1),
          hasRat: (i === rCount - 1 && j === cCount - 1),
          state: isWall ? 'wall' : (inSol ? 'solution' : 'open')
        });
      }
      g.push(rowList);
    }
    return g;
  }
  return s.value.grid || [];
});

const mainRef = ref(null);
const leftColRef = ref(null);
const hResizerRef = ref(null);
const vizResizerRef = ref(null);
const tableResizerRef = ref(null);

function initHResizer() {
  const rsz = hResizerRef.value, main = mainRef.value;
  if (!rsz || !main) return;
  let dragging = false, startX = 0, startW = 0;
  const onDown = e => { dragging = true; startX = e.clientX; startW = leftColRef.value.offsetWidth; rsz.classList.add('drag'); document.body.style.userSelect = 'none'; };
  const onMove = e => { if (!dragging) return; const mainW = main.offsetWidth; leftWidth.value = (Math.max(200, Math.min(mainW - 200, startW + e.clientX - startX)) / mainW) * 100; };
  const onUp = () => { if (!dragging) return; dragging = false; rsz.classList.remove('drag'); document.body.style.userSelect = ''; };
  rsz.addEventListener('mousedown', onDown);
  document.addEventListener('mousemove', onMove);
  document.addEventListener('mouseup', onUp);
  return () => { rsz.removeEventListener('mousedown', onDown); document.removeEventListener('mousemove', onMove); document.removeEventListener('mouseup', onUp); };
}

function initVResizer(elRef, valueRef, minH, maxH) {
  const rsz = elRef.value;
  if (!rsz) return;
  let dragging = false, startY = 0, startH = 0;
  const onDown = e => { dragging = true; startY = e.clientY; startH = valueRef.value; rsz.classList.add('drag'); document.body.style.userSelect = 'none'; e.preventDefault(); };
  const onMove = e => { if (!dragging) return; valueRef.value = Math.max(minH, Math.min(maxH, startH + (e.clientY - startY))); };
  const onUp = () => { if (!dragging) return; dragging = false; rsz.classList.remove('drag'); document.body.style.userSelect = ''; };
  rsz.addEventListener('mousedown', onDown);
  document.addEventListener('mousemove', onMove);
  document.addEventListener('mouseup', onUp);
  return () => { rsz.removeEventListener('mousedown', onDown); document.removeEventListener('mousemove', onMove); document.removeEventListener('mouseup', onUp); };
}

let cleanupFns = [];
onMounted(() => {
  document.addEventListener('keydown', onKeydown);
  cleanupFns.push(initHResizer());
  cleanupFns.push(initVResizer(vizResizerRef, vizHeight, 180, 520));
  cleanupFns.push(initVResizer(tableResizerRef, tableHeight, 50, 200));
  if (typeof window !== 'undefined') window.scrollTo(0, 0);
});

onBeforeUnmount(() => {
  document.removeEventListener('keydown', onKeydown);
  clearTimeout(playTimer);
  cleanupFns.forEach(fn => fn && fn());
});
</script>

<template>
  <div class="slide-wrapper">
    <!-- Navbar Header -->
    <div class="navbar">
      <h2 class="navbar-title">{{ topic }} &mdash; {{ subTopic }}</h2>
      <img src="../../assets/logo.png" alt="Logo" />
    </div>

    <div class="slide-body">
      <div class="row-main">
        <div class="ll-root">
          <!-- Control Panel Toolbar -->
          <div class="ll-toolbar">
            <div class="ll-input-group">
              <label>Rows:</label>
              <input
                type="number"
                min="2"
                max="6"
                v-model.number="inputRows"
                class="ll-text-input"
                @keyup.enter="applyInput"
                style="width: 42px;"
              />
            </div>

            <div class="ll-input-group">
              <label>Cols:</label>
              <input
                type="number"
                min="2"
                max="6"
                v-model.number="inputCols"
                class="ll-text-input"
                @keyup.enter="applyInput"
                style="width: 42px;"
              />
            </div>

            <div class="ll-input-group">
              <label>maze[][]:</label>
              <input
                type="text"
                v-model="mazeInputStr"
                class="ll-text-input"
                @keyup.enter="applyInput"
                title="Input 2D array e.g. [[1,0,0,0],[1,1,0,1],[1,1,0,0],[0,1,1,1]]"
                style="width: 240px; font-size: 10.5px;"
              />
            </div>

            <div class="ll-preset-group">
              <!-- <button class="ll-preset-btn" @click="loadPreset('default')" title="Standard 4x4 maze">4x4</button>
              <button class="ll-preset-btn" @click="loadPreset('3x3')" title="Classic 3x3 maze">3x3</button> -->
              <button class="ll-preset-btn" @click="loadPreset('open')" title="Fully open 3x3 grid">Open</button>
            </div>

            <button class="ll-viz-btn" @click="applyInput">&#9654; Visualize</button>

            <!-- Playback Navigation Controls -->
            <div class="ll-nav-controls">
              <button class="ll-nav-btn" title="First step" @click="stepBy(-steps.length)">&#171;</button>
              <button class="ll-nav-btn" title="Previous step" @click="stepBy(-1)">&#8249; Prev</button>
              <button class="ll-play-btn" @click="togglePlay">
                {{ playing ? '\u23F8 Pause' : '\u25B6 Play' }}
              </button>
              <button class="ll-nav-btn" title="Next step" @click="stepBy(1)">Next &#8250;</button>
              <button class="ll-nav-btn" title="Last step" @click="stepBy(steps.length)">&#187;</button>
            </div>
          </div>

          <div class="ll-main" ref="mainRef">
            <!-- Left Visualization Column -->
            <div class="ll-left-col" ref="leftColRef" :style="{ width: leftWidth + '%' }">
              <div class="ll-viz-wrap" :style="{ height: vizHeight + 'px' }">
                <div class="ll-perm-area">
                  <!-- Real-time Stats Chips -->
                  <div class="ll-ptrs ll-ptrs-compact" @wheel.passive="onChipsWheel">
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">rows</span><b class="ll-c-blue">{{ s.rows || inputRows }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">cols</span><b class="ll-c-blue">{{ s.cols || inputCols }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">i</span><b class="ll-c-purple">{{ s.loopI !== null && s.loopI !== undefined ? s.loopI : '—' }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">j</span><b class="ll-c-purple">{{ s.loopJ !== null && s.loopJ !== undefined ? s.loopJ : '—' }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">r</span><b class="ll-c-orange">{{ s.r !== undefined && s.r >= 0 ? s.r : '—' }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">c</span><b class="ll-c-orange">{{ s.c !== undefined && s.c >= 0 ? s.c : '—' }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">path</span><b class="ll-c-green">"{{ s.path || '' }}"</b></span>
                  </div>

                  <!-- Maze Board Container -->
                  <div class="ll-board-container">
                    <div class="ll-board-frame">
                      <!-- Column Headers -->
                      <div class="ll-col-indices">
                        <div class="ll-axis-spacer"></div>
                        <div
                          v-for="c in (s.cols || inputCols)"
                          :key="'col-h-' + c"
                          class="ll-axis-label"
                          :class="{ 'll-axis-cur': s.c === c - 1 }"
                        >
                          {{ c - 1 }}
                        </div>
                      </div>

                      <!-- Maze Rows -->
                      <div class="ll-board-rows">
                        <div
                          v-for="(row, rIdx) in displayGrid"
                          :key="'row-' + rIdx"
                          class="ll-board-row"
                        >
                          <!-- Row Label -->
                          <div class="ll-axis-label ll-row-label" :class="{ 'll-axis-cur': s.r === rIdx }">
                            {{ rIdx }}
                          </div>

                          <!-- Maze Cells -->
                          <div
                            v-for="(cell, cIdx) in row"
                            :key="'cell-' + rIdx + '-' + cIdx"
                            class="ll-chess-cell ll-maze-cell"
                            :class="{
                              'll-cell-wall': cell.state === 'wall',
                              'll-cell-open': cell.state === 'open',
                              'll-cell-rat': cell.state === 'rat',
                              'll-cell-visited': cell.state === 'visited',
                              'll-cell-testing': cell.state === 'testing',
                              'll-cell-conflict': cell.state === 'conflict',
                              'll-cell-solution': cell.state === 'solution',
                              'll-cell-start': cell.isStart,
                              'll-cell-goal': cell.isGoal
                            }"
                            @click="toggleCell(rIdx, cIdx)"
                            :title="`Cell (${rIdx}, ${cIdx}): ${cell.isWall ? 'Wall (0)' : 'Open (1)'}. Click to toggle.`"
                          >
                            <!-- Start Marker -->
                            <div v-if="cell.isStart && !cell.hasRat" class="ll-start-marker">
                            </div>

                            <!-- Goal Marker -->
                            <div v-if="cell.isGoal && !cell.hasRat" class="ll-goal-marker">
                              🧀
                            </div>

                            <!-- Rat Icon -->
                            <transition name="queen-pop">
                              <div v-if="cell.hasRat" class="ll-rat-piece">
                                🐭
                              </div>
                            </transition>

                            <!-- Testing Marker -->
                            <div v-if="cell.state === 'testing' && !cell.hasRat" class="ll-test-marker">
                              ?
                            </div>

                            <!-- Conflict Marker -->
                            <div v-if="cell.state === 'conflict'" class="ll-conflict-marker">
                              ✕
                            </div>

                            <!-- Visited Breadcrumb -->
                            <div v-if="cell.state === 'visited' && !cell.hasRat" class="ll-visited-marker">
                              •
                            </div>

                            <!-- Cell value indicator -->
                            <span class="ll-cell-subval">{{ cell.val }}</span>
                          </div>
                        </div>
                      </div>
                    </div>

                    <!-- Discovered Solutions Gallery -->
                    <div class="ll-solutions-strip" v-if="s.solutions && s.solutions.length">
                      <div class="ll-sol-title">Discovered Paths ({{ s.solutions.length }}):</div>
                      <div class="ll-sol-cards">
                        <button
                          v-for="(sol, idx) in s.solutions"
                          :key="'sol-btn-' + idx"
                          class="ll-sol-badge-btn"
                          :class="{ active: selectedSolutionIdx === idx }"
                          @click="viewSolution(idx)"
                          :title="`Click to inspect Path #${idx + 1}: ${sol.path}`"
                        >
                          Path #{{ idx + 1 }}: {{ sol.path }}
                        </button>
                      </div>
                    </div>
                  </div>
                </div>
              </div>

              <!-- Resizer -->
              <div class="ll-vresizer" ref="vizResizerRef"></div>

              <!-- Color Legend -->
              <div class="ll-legend">
                <!-- <span class="ll-leg"><span class="ll-legdot" style="background:#ffffff; border:1px solid #cbd5e1;"></span>Open (1)</span> -->
                <span class="ll-leg"><span class="ll-legdot" style="background:#334155;"></span>Wall (0)</span>
                <!-- <span class="ll-leg"><span class="ll-legdot" style="background:var(--blue);"></span>Rat 🐭</span> -->
                <span class="ll-leg"><span class="ll-legdot ll-legdot-safe"></span>Active Path</span>
                <span class="ll-leg"><span class="ll-legdot ll-legdot-active"></span>Testing ?</span>
                <span class="ll-leg"><span class="ll-legdot ll-legdot-mismatch"></span>Rejected ✕</span>
                <span class="ll-leg"><span class="ll-legdot ll-legdot-max"></span>Goal 🧀</span>
              </div>

              <!-- Variable Frames & Call Stack -->
              <div class="ll-table-area" :style="{ height: tableHeight + 'px' }">
                <div class="ll-table-title">Recursion Call Stack &mdash; innermost = current depth</div>
                <div class="ll-stack-line">
                  <template v-if="s.vars && s.vars.length">
                    <div
                      v-for="(f, depth) in s.vars"
                      :key="depth"
                      class="ll-frame"
                      :class="{ 'll-frame-cur': depth === s.vars.length - 1 }"
                      :style="{ marginLeft: depth * 14 + 'px' }"
                    >
                      {{ f.title }}(<span v-for="(r, idx) in f.rows" :key="idx">
                        <span v-if="idx > 0">, </span>
                        <span class="ll-fname">{{ r[0] }}</span>=<span :class="depth === s.vars.length - 1 ? 'll-c-orange' : 'll-c-blue'" style="font-weight:700">{{ r[1] }}</span>
                      </span>)<span v-if="depth === s.vars.length - 1" class="ll-now"> &#9668; active</span>
                    </div>
                  </template>
                  <template v-else>&mdash;</template>
                </div>
              </div>

              <!-- Resizer -->
              <div class="ll-vresizer" ref="tableResizerRef"></div>

              <!-- Step Badge -->
              <div class="ll-badge-wrap">
                <div
                  class="ll-badge"
                  :class="{
                    'll-badge-error': s.badge && (s.badge.includes('OutOfBounds') || s.badge.includes('Wall') || s.badge.includes('Already') || s.badge.includes('FALSE')),
                    'll-badge-success': s.badge && (s.badge.includes('PATH FOUND') || s.badge.includes('DESTINATION') || s.badge.includes('passed') || s.badge.includes('Complete') || s.badge.includes('finished'))
                  }"
                >
                  {{ s.badge || 'Ready to run Rat in a Maze Backtracking algorithm.' }}
                </div>
              </div>
            </div>

            <!-- Horizontal Resizer -->
            <div class="ll-resizer" ref="hResizerRef"></div>

            <!-- Right Column: Code & Theory -->
            <div class="ll-right-col">
              <div class="ll-code-panel">
                <div class="ll-code-header">
                  <div class="ll-tabbar">
                    <button class="ll-tab-btn" :class="{ active: rightTab === 'code' }" @click="rightTab = 'code'">Code</button>
                    <button class="ll-tab-btn" :class="{ active: rightTab === 'pseudo' }" @click="rightTab = 'pseudo'">Pseudocode</button>
                    <button class="ll-tab-btn" :class="{ active: rightTab === 'complexity' }" @click="rightTab = 'complexity'">Complexity</button>
                  </div>
                  <select v-if="rightTab === 'code'" v-model="lang" class="ll-lang-select">
                    <option value="java">Java</option>
                    <option value="c">C</option>
                    <option value="cpp">C++</option>
                    <option value="python">Python</option>
                    <option value="javascript">JavaScript</option>
                  </select>
                </div>

                <!-- Code Scroll with 1-to-1 Line Highlighting -->
                <div v-if="rightTab === 'code'" class="ll-code-scroll" ref="codeScrollRef">
                  <pre class="ll-pre"><span
                    v-for="(line, idx) in codeLines"
                    :key="idx"
                    class="ll-codeline"
                    :class="{ 'll-hl': line[0] && line[0] === s.code }"
                  >{{ line[1] === '' ? ' ' : line[1] }}</span></pre>
                </div>

                <!-- Pseudocode Scroll -->
                <div v-else-if="rightTab === 'pseudo'" class="ll-code-scroll">
                  <pre class="ll-pre"><span
                    v-for="(line, idx) in PSEUDOCODE"
                    :key="idx"
                    class="ll-codeline"
                  >{{ line }}</span></pre>
                </div>

                <!-- Time and Space Complexity Tab -->
                <div v-else class="ll-info-scroll">
                  <h3 class="ll-cx-heading">Rat in a Maze &mdash; Complexity Analysis</h3>
                  <p class="ll-cx-intro">
                    Finds all paths for a rat from <code>(0, 0)</code> to destination <code>(rows - 1, cols - 1)</code> moving in 4 directions:
                    <strong>Down ('D')</strong>, <strong>Left ('L')</strong>, <strong>Right ('R')</strong>, and <strong>Up ('U')</strong>.
                    Branches into walls (0), out-of-bound cells, and already visited squares are pruned immediately.
                  </p>

                  <h4 class="ll-cx-sub">Step-by-step Breakdown</h4>
                  <table class="ll-complexity-table">
                    <thead>
                      <tr><th>Operation / Phase</th><th>Time</th><th>Space</th><th>Reason</th></tr>
                    </thead>
                    <tbody>
                      <tr>
                        <td>Safe check: <code>isSafe(r, c)</code></td>
                        <td class="ll-cx-good">O(1)</td>
                        <td class="ll-cx-good">O(1)</td>
                        <td>Validates boundary, wall (<code>maze[r][c] == 1</code>), and visited status in constant time</td>
                      </tr>
                      <tr>
                        <td>Backtracking search (Pruned)</td>
                        <td class="ll-cx-mid">O(4<sup>R &times; C</sup>)</td>
                        <td class="ll-cx-good">O(R &times; C)</td>
                        <td>At each cell up to 4 direction branches are explored; recursion stack depth &le; R &times; C</td>
                      </tr>
                      <tr>
                        <td>Grid state representation</td>
                        <td class="ll-cx-good">O(R &times; C)</td>
                        <td class="ll-cx-good">O(R &times; C)</td>
                        <td>Storage for input maze and boolean visited tracking matrix</td>
                      </tr>
                    </tbody>
                  </table>

                  <h4 class="ll-cx-sub">Overall Complexity</h4>
                  <div class="ll-cx-summary-grid">
                    <div class="ll-cx-card ll-cx-card-mid">
                      <div class="ll-cx-card-label">Time Complexity</div>
                      <div class="ll-cx-card-val">O(4<sup>R &times; C</sup>)</div>
                      <div class="ll-cx-card-note">Upper bound for exhaustive path search</div>
                    </div>
                    <div class="ll-cx-card ll-cx-card-good">
                      <div class="ll-cx-card-label">Auxiliary Space</div>
                      <div class="ll-cx-card-val">O(R &times; C)</div>
                      <div class="ll-cx-card-note">Recursion call stack &amp; visited array</div>
                    </div>
                    <div class="ll-cx-card ll-cx-card-good">
                      <div class="ll-cx-card-label">Discovered Paths</div>
                      <div class="ll-cx-card-val">Depends on Maze</div>
                      <div class="ll-cx-card-note">Pruning cuts non-viable branches early</div>
                    </div>
                  </div>

                  <div class="ll-note">
                    <strong>Backtracking Insight:</strong> The rat marks its current cell as <code>visited[r][c] = true</code> before attempting moves in the order 'D', 'L', 'R', 'U'.
                    When returning from all recursive direction explorations, it <strong>backtracks</strong> by unmarking <code>visited[r][c] = false</code>, permitting alternate viable paths to navigate through that cell.
                  </div>
                </div>
              </div>
            </div>
          </div>

          <!-- Footer -->
          <div class="ll-footer">
            Step {{ si + 1 }} / {{ steps.length }}
            <span class="ll-speed-wrap">Speed <input type="range" min="100" max="2000" step="100" v-model.number="speed" /></span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.ll-root * { box-sizing: border-box; }
.ll-root *, .ll-root, .row-main { scrollbar-width: none !important; -ms-overflow-style: none !important; }
.ll-root *::-webkit-scrollbar, .ll-root::-webkit-scrollbar, .row-main::-webkit-scrollbar { display: none !important; width: 0 !important; height: 0 !important; }

.ll-root {
  --coral: #F04D4D;
  --coral-dark: #d93e3e;
  --coral-light: #fff0f0;
  --bg: #f5f6fa;
  --surface: #ffffff;
  --surface2: #f1f4f9;
  --border: #e2e8f0;
  --border2: #cbd5e1;
  --text: #1e293b;
  --text2: #475569;
  --muted: #94a3b8;
  --blue: #3b82f6;
  --blue-light: #eff6ff;
  --green: #22c55e;
  --green-light: #f0fdf4;
  --orange: #f97316;
  --orange-light: #fff7ed;
  --purple: #9333ea;
  --purple-light: #f3e8ff;
  --red: #ef4444;
  --red-dark: #991b1b;
  --red-light: #fef2f2;
  --shadow-sm: 0 1px 3px rgba(0,0,0,.08), 0 1px 2px rgba(0,0,0,.04);
  --radius: 8px;
  --radius-sm: 6px;
  background: var(--bg);
  color: var(--text);
  font-family: 'Segoe UI', system-ui, sans-serif;
  font-size: 12.5px;
  display: flex;
  flex-direction: column;
  height: 74vh;
  overflow: hidden;
  width: 100%;
}

@keyframes ll-pop {
  0% { transform: scale(0.6); opacity: 0; }
  70% { transform: scale(1.15); opacity: 1; }
  100% { transform: scale(1); opacity: 1; }
}

@keyframes ll-pulse-conflict {
  0%, 100% { transform: scale(1); }
  50% { transform: scale(1.08); }
}

.slide-wrapper {
  margin-top: -10px;
  margin-left: -30px;
  width: 107%;
  max-height: 100%;
  font-size: 0.8rem;
  font-weight: 400;
}

.slide-body {
  display: flex;
  flex-direction: column;
  border-radius: 4px;
  height: 100%;
}

.navbar {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
  gap: 0.75rem;
  padding: 0 10px;
  background-color: #ffffff;
  position: fixed;
  width: 94.7%;
  z-index: 50;
}

.navbar > img {
  height: 30px;
}

.navbar-title {
  margin: 0;
  font-size: 1.35rem;
  font-weight: 700;
  background-color: #ef5050;
  color: #ffffff;
  width: 80%;
  padding: 2px 10px;
  margin-left: -10px;
  border-radius: 5px;
}

.row-main {
  width: 100%;
  height: 90%;
  margin-top: 36px;
  overflow-x: auto;
  overflow-y: hidden;
}

/* Control Toolbar */
.ll-toolbar {
  margin-top: 4px;
  display: flex;
  align-items: center;
  gap: 6px;
  padding: 6.5px 12px;
  background: var(--surface);
  border-bottom: 1px solid var(--border);
  flex-shrink: 0;
  flex-wrap: wrap;
  box-shadow: var(--shadow-sm);
}

.ll-input-group {
  display: flex;
  align-items: center;
  gap: 4px;
}

.ll-input-group label {
  font-size: 11px;
  color: var(--muted);
  font-weight: 700;
}

.ll-text-input,
.ll-num-input {
  background: var(--surface);
  border: 1px solid var(--border2);
  color: var(--text);
  border-radius: var(--radius-sm);
  padding: 3px 6px;
  font-size: 11.5px;
  font-family: monospace;
}

.ll-text-input:focus,
.ll-num-input:focus {
  outline: none;
  border-color: var(--coral);
  box-shadow: 0 0 0 3px rgba(240,77,77,.1);
}

.ll-preset-group {
  display: flex;
  gap: 3px;
}

.ll-preset-btn {
  background: var(--surface2);
  border: 1px solid var(--border2);
  color: var(--text2);
  padding: 3px 6px;
  border-radius: 4px;
  font-size: 10.5px;
  cursor: pointer;
  transition: all .12s;
}

.ll-preset-btn:hover {
  background: var(--coral-light);
  border-color: var(--coral);
  color: var(--coral-dark);
}

.ll-viz-btn {
  background: var(--coral);
  color: #fff;
  border: none;
  padding: 5px 12px;
  border-radius: var(--radius-sm);
  cursor: pointer;
  font-size: 11.5px;
  font-weight: 600;
  box-shadow: var(--shadow-sm);
  transition: filter .15s;
}

.ll-viz-btn:hover {
  filter: brightness(1.08);
}

.ll-nav-controls {
  display: flex;
  margin-left: auto;
  align-items: center;
  gap: 4px;
  flex-shrink: 0;
  flex-wrap: wrap;
}

.ll-nav-btn {
  background: var(--surface2);
  border: 1px solid var(--border2);
  color: var(--text2);
  padding: 4px 9px;
  border-radius: var(--radius-sm);
  cursor: pointer;
  font-size: 11px;
  font-weight: 500;
  transition: all .15s;
  white-space: nowrap;
}

.ll-nav-btn:hover {
  background: var(--surface);
  border-color: var(--coral);
  color: var(--coral);
}

.ll-play-btn {
  background: var(--blue-light);
  border: 1px solid var(--blue);
  color: var(--blue);
  min-width: 68px;
  font-weight: 600;
  padding: 4px 9px;
  border-radius: var(--radius-sm);
  cursor: pointer;
  font-size: 11px;
  transition: all .15s;
}

.ll-play-btn:hover {
  background: var(--blue);
  color: #fff;
}

/* Main Split Layout */
.ll-main {
  display: flex;
  flex: 1;
  overflow: hidden;
  position: relative;
}

.ll-left-col {
  display: flex;
  flex-direction: column;
  overflow: hidden;
  min-width: 220px;
  max-width: 75%;
}

.ll-resizer {
  width: 5px;
  cursor: col-resize;
  background: var(--border);
  flex-shrink: 0;
  transition: background .15s;
}

.ll-resizer:hover, .ll-resizer.drag {
  background: var(--coral);
}

.ll-right-col {
  display: flex;
  flex-direction: column;
  flex: 1;
  overflow: hidden;
  min-width: 0;
  height: 100%;
}

/* Viz Area */
.ll-viz-wrap { flex-shrink: 0; background: var(--surface); border-bottom: 1px solid var(--border); position: relative; overflow-x: auto; overflow-y: auto; }
.ll-perm-area { display: flex; flex-direction: column; align-items: stretch; min-height: 100%; width: 100%; min-width: 0; box-sizing: border-box; }
.ll-ptrs { display: flex; gap: 8px; flex-wrap: wrap; padding: 4px 14px; min-height: 28px; width: 100%; box-sizing: border-box; min-width: 0; align-items: center; }
.ll-ptrs-compact {
  flex-wrap: nowrap;
  gap: 6px;
  padding: 3px 14px 5px;
  min-height: 0;
  overflow-x: auto;
  overflow-y: hidden;
  scrollbar-width: thin;
  scrollbar-color: var(--border) transparent;
}
.ll-ptrs-compact::-webkit-scrollbar {
  height: 3px;
}
.ll-ptrs-compact::-webkit-scrollbar-track {
  background: transparent;
}
.ll-ptrs-compact::-webkit-scrollbar-thumb {
  background: var(--border);
  border-radius: 3px;
}
.ll-ptrs-compact::-webkit-scrollbar-thumb:hover {
  background: var(--muted);
}
.ll-ptr-chip { background: var(--surface2); border: 1px solid var(--border); border-radius: var(--radius-sm); padding: 3px 10px; font-size: 12px; font-family: monospace; box-shadow: var(--shadow-sm); white-space: nowrap; flex-shrink: 0; }
.ll-ptr-chip-inline { display: inline-flex; align-items: center; gap: 4px; background: var(--surface2); border: 1px solid var(--border); border-radius: 6px; padding: 2px 8px; font-size: 11px; font-family: monospace; white-space: nowrap; flex-shrink: 0; line-height: 1.4; }
.ll-chip-label { color: var(--text-muted, #8899aa); font-weight: 500; margin-right: 2px; }
.ll-c-blue { color: var(--blue); } .ll-c-orange { color: var(--orange); } .ll-c-green { color: var(--green); } .ll-c-purple { color: var(--purple); } .ll-c-red { color: var(--red); }

/* Maze Board Layout */
.ll-board-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 8px 16px 14px;
}

.ll-board-frame {
  display: flex;
  flex-direction: column;
  background: #f8fafc;
  padding: 8px;
  border-radius: var(--radius);
  border: 1px solid var(--border);
  box-shadow: var(--shadow-sm);
  margin-bottom: 8px;
}

.ll-col-indices {
  display: flex;
  margin-bottom: 4px;
}

.ll-axis-spacer {
  width: 26px;
  height: 20px;
}

.ll-axis-label {
  width: 44px;
  height: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 11px;
  font-weight: 700;
  color: var(--muted);
  margin: 0 1px;
}

.ll-row-label {
  width: 24px;
  height: 44px;
  margin-right: 2px;
}

.ll-axis-cur {
  color: var(--coral) !important;
  font-weight: 800;
}

.ll-board-rows {
  display: flex;
  flex-direction: column;
  gap: 2px;
}

.ll-board-row {
  display: flex;
  align-items: center;
}

.ll-chess-cell {
  width: 44px;
  height: 44px;
  margin: 0 1px;
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
  border-radius: 4px;
  cursor: pointer;
  user-select: none;
  transition: all .15s ease;
}

.ll-cell-open {
  background: #ffffff;
  border: 1px solid #e2e8f0;
}

.ll-cell-open:hover {
  border-color: #94a3b8;
}

.ll-cell-wall {
  background: #334155;
  border: 1px solid #1e293b;
  color: #94a3b8;
}

.ll-cell-visited {
  background: #ecfdf5;
  border: 1px solid #a7f3d0;
}

.ll-cell-rat {
  background: #eff6ff !important;
  border: 2px solid var(--blue) !important;
  box-shadow: 0 0 8px rgba(59,130,246,.35);
}

.ll-cell-testing {
  background: #fffbeb !important;
  border: 2px solid var(--orange) !important;
  box-shadow: 0 0 6px rgba(249,115,22,.3);
}

.ll-cell-conflict {
  background: #fef2f2 !important;
  border: 2px solid var(--red) !important;
  animation: ll-pulse-conflict .4s ease;
}

.ll-cell-solution {
  background: #fef9c3 !important;
  border: 2px solid #eab308 !important;
}

.ll-cell-start {
  position: relative;
}

.ll-cell-start::after {
  content: 'S';
  position: absolute;
  top: 2px;
  left: 3px;
  font-size: 9px;
  font-weight: 800;
  color: #3b82f6;
}

.ll-cell-goal::after {
  content: 'G';
  position: absolute;
  bottom: 2px;
  right: 3px;
  font-size: 9px;
  font-weight: 800;
  color: #15803d;
}

.ll-rat-piece {
  font-size: 20px;
  line-height: 1;
  animation: ll-pop .25s ease forwards;
  z-index: 2;
}

.ll-goal-marker {
  font-size: 16px;
  line-height: 1;
}

.ll-test-marker {
  font-size: 14px;
  font-weight: 800;
  color: var(--orange);
}

.ll-conflict-marker {
  font-size: 14px;
  font-weight: 800;
  color: var(--red);
}

.ll-visited-marker {
  font-size: 16px;
  font-weight: 800;
  color: #10b981;
}

.ll-cell-subval {
  position: absolute;
  bottom: 2px;
  right: 4px;
  font-size: 8.5px;
  color: #94a3b8;
  font-family: monospace;
}

/* Solutions Strip */
.ll-solutions-strip {
  display: flex;
  flex-direction: column;
  gap: 4px;
  width: 100%;
  max-width: 420px;
  background: #f8fafc;
  padding: 6px 10px;
  border-radius: var(--radius-sm);
  border: 1px solid var(--border);
}

.ll-sol-title {
  font-size: 11px;
  font-weight: 700;
  color: var(--text2);
}

.ll-sol-cards {
  display: flex;
  gap: 4px;
  flex-wrap: wrap;
}

.ll-sol-badge-btn {
  padding: 3px 8px;
  border-radius: 4px;
  border: 1px solid var(--border2);
  background: var(--surface);
  color: var(--text2);
  font-size: 10.5px;
  font-family: monospace;
  cursor: pointer;
  transition: all .15s ease;
}

.ll-sol-badge-btn:hover {
  border-color: #eab308;
  color: #ca8a04;
}

.ll-sol-badge-btn.active {
  background: #fef08a;
  border-color: #ca8a04;
  color: #854d0e;
  font-weight: 700;
}

/* Resizers */
.ll-vresizer {
  height: 5px;
  cursor: row-resize;
  background: var(--border);
  flex-shrink: 0;
  transition: background .15s;
}

.ll-vresizer:hover, .ll-vresizer.drag {
  background: var(--coral);
}

/* Legend */
.ll-legend { display: flex; flex-wrap: wrap; gap: 6px 14px; padding: 6px 12px; border-bottom: 1px solid var(--border); flex-shrink: 0; background: var(--surface2); }
.ll-leg { display: flex; align-items: center; gap: 5px; font-size: 11px; color: var(--text2); font-weight: 500; }
.ll-legdot { width: 11px; height: 11px; border-radius: 3px; flex-shrink: 0; display: inline-block; }
.ll-legdot-safe { background: #dcfce7; border: 1.5px solid #10b981; }
.ll-legdot-active { background: #fef3c7; border: 1.5px solid #f59e0b; }
.ll-legdot-mismatch { background: #fee2e2; border: 1.5px solid #ef4444; }
.ll-legdot-max { background: #fef9c3; border: 1.5px solid #eab308; }

/* Call Stack Area */
.ll-table-area { flex-shrink: 0; padding: 8px 14px; border-bottom: 1px solid var(--border); overflow-x: hidden; overflow-y: auto; background: var(--surface); min-width: 0; box-sizing: border-box; }
.ll-table-title { font-size: 10px; color: var(--muted); margin-bottom: 4px; font-style: italic; }
.ll-stack-line { font-family: 'Consolas', monospace; font-size: 12px; line-height: 1.8; }
.ll-frame { font-family: 'Consolas', monospace; font-size: 11.5px; color: var(--text2); padding: 1px 0; white-space: nowrap; }
.ll-frame-cur { color: var(--orange); background: var(--orange-light); border-radius: 4px; padding: 1px 5px; }
.ll-fname { color: var(--text2); }
.ll-now { color: var(--orange); font-size: 10px; margin-left: 6px; }

/* Badge Area */
.ll-badge-wrap { padding: 6px 10px; border-bottom: 1px solid var(--border); flex-shrink: 0; min-height: 36px; display: flex; align-items: center; background: var(--surface); }
.ll-badge { display: inline-block; padding: 4px 12px; border-radius: var(--radius-sm); border-left: 3px solid var(--coral); background: var(--coral-light); font-size: 11px; color: var(--coral-dark); line-height: 1.4; word-break: break-word; font-weight: 500; }
.ll-badge-error { border-left-color: var(--red) !important; background: var(--red-light) !important; color: var(--red-dark) !important; }
.ll-badge-success { border-left-color: var(--green) !important; background: var(--green-light) !important; color: #15803d !important; }

/* Right Column: Code Panel */
.ll-code-panel {
  display: flex;
  flex-direction: column;
  height: 100%;
  overflow: hidden;
}

.ll-code-header {
  display: flex;
  align-items: center;
  gap: 6px;
  padding: 5px 12px;
  background: var(--surface);
  border-bottom: 1px solid var(--border);
  box-shadow: var(--shadow-sm);
  flex-shrink: 0;
  flex-wrap: wrap;
}

.ll-tabbar {
  display: flex;
  gap: 3px;
  flex-wrap: wrap;
}

.ll-tab-btn {
  padding: 4px 9px;
  font-size: 10.5px;
  font-weight: 600;
  border: 1px solid var(--border2);
  background: var(--surface2);
  color: var(--text2);
  border-radius: var(--radius-sm);
  cursor: pointer;
  transition: all .15s ease;
  white-space: nowrap;
}
.ll-tab-btn:hover { border-color: var(--coral); color: var(--coral); }
.ll-tab-btn.active {
  background: var(--coral);
  border-color: var(--coral);
  color: #fff;
}

.ll-lang-select {
  margin-left: auto;
  padding: 4px 24px 4px 8px;
  font-size: 11px;
  font-weight: 500;
  border: 1px solid var(--border2);
  border-radius: var(--radius-sm);
  background: var(--surface2);
  color: var(--text);
  cursor: pointer;
  appearance: none;
  background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='10' height='6' viewBox='0 0 10 6'%3E%3Cpath d='M0 0l5 6 5-6z' fill='%2394a3b8'/%3E%3C/svg%3E");
  background-repeat: no-repeat;
  background-position: right 8px center;
  min-width: 95px;
  transition: border-color .15s;
}
.ll-lang-select:focus {
  outline: none;
  border-color: var(--coral);
  box-shadow: 0 0 0 3px rgba(240,77,77,.1);
}

.ll-code-scroll {
  flex: 1;
  overflow: auto;
  background: #f8fafc;
  padding: 10px 14px;
  min-width: 0;
}

.ll-pre {
  margin: 0;
  font-family: 'Cascadia Code', 'Fira Code', 'Consolas', monospace;
  font-size: 11px;
  line-height: 1.5;
  color: var(--text);
  white-space: pre;
  padding-bottom: 150px;
}

.ll-codeline {
  display: block;
  padding: 0 14px;
  margin: 0 -14px;
}

.ll-hl {
  background: #dcfce7;
  color: #15803d;
  font-weight: 600;
  border-left: 3px solid var(--green);
  border-radius: 3px;
}

/* Theory & Complexity Tab */
.ll-info-scroll {
  flex: 1;
  overflow: auto;
  padding: 12px 16px;
  background: var(--surface);
  color: var(--text2);
  font-size: 12px;
  line-height: 1.55;
}
.ll-info-scroll h3 { font-size: 13px; font-weight: 700; color: var(--text); margin: 0 0 6px; }
.ll-info-scroll h3:not(:first-child) { margin-top: 14px; }
.ll-info-scroll h4 { font-size: 12px; font-weight: 700; color: var(--text); margin: 10px 0 4px; }
.ll-info-scroll p { margin: 0 0 6px; }
.ll-info-scroll ul { margin: 0 0 10px 16px; padding: 0; }
.ll-info-scroll li { margin-bottom: 3px; }
.ll-info-scroll code {
  background: var(--surface2);
  padding: 1px 4px;
  border-radius: 3px;
  font-family: 'Cascadia Code', monospace;
  font-size: 11px;
  color: var(--coral-dark);
}

.ll-cx-heading { font-size: 13px; font-weight: 700; color: var(--text); margin: 0 0 6px; }
.ll-cx-intro { font-size: 10.5px; color: var(--text2); margin: 0 0 10px; line-height: 1.55; }
.ll-cx-sub { font-size: 11px; font-weight: 700; color: var(--text2); margin: 10px 0 4px; border-bottom: 1px solid var(--border); padding-bottom: 3px; }

.ll-complexity-table {
  width: 100%;
  border-collapse: collapse;
  font-size: 10.5px;
  margin: 8px 0;
}
.ll-complexity-table th, .ll-complexity-table td {
  border: 1px solid var(--border);
  padding: 4px 8px;
  text-align: left;
}
.ll-complexity-table th {
  background: var(--surface2);
  font-weight: 700;
  color: var(--text2);
}
.ll-cx-good { color: #15803d; font-weight: 700; }
.ll-cx-mid { color: #b45309; font-weight: 700; }
.ll-cx-bad { color: #b91c1c; font-weight: 700; }

.ll-cx-summary-grid {
  display: flex;
  gap: 8px;
  flex-wrap: wrap;
  margin: 6px 0 10px;
}
.ll-cx-card {
  flex: 1;
  min-width: 90px;
  border-radius: var(--radius-sm);
  padding: 8px 10px;
  text-align: center;
  border: 1.5px solid var(--border);
}
.ll-cx-card-good { background: #f0fdf4; border-color: #86efac; color: #15803d; }
.ll-cx-card-mid { background: #fff7ed; border-color: #fed7aa; color: #c2410c; }
.ll-cx-card-bad { background: #fef2f2; border-color: #fca5a5; color: #b91c1c; }
.ll-cx-card-label { font-size: 9px; font-weight: 700; text-transform: uppercase; letter-spacing: .05em; opacity: .7; margin-bottom: 4px; }
.ll-cx-card-val { font-size: 13px; font-weight: 800; font-family: monospace; margin-bottom: 3px; }
.ll-cx-card-note { font-size: 8.5px; opacity: .75; line-height: 1.3; }

.ll-note {
  background: #fefce8;
  border: 1px solid #fef08a;
  border-left: 3px solid #eab308;
  padding: 6px 10px;
  font-size: 10.5px;
  color: #854d0e;
  border-radius: 0 4px 4px 0;
  margin-top: 10px;
  margin-bottom: 120px;
}

/* Footer */
.ll-footer {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 4px 14px;
  background: var(--surface);
  border-top: 1px solid var(--border);
  font-size: 11px;
  color: var(--muted);
  flex-shrink: 0;
}

.ll-speed-wrap {
  display: flex;
  align-items: center;
  gap: 6px;
}

.ll-speed-wrap input[type="range"] {
  width: 90px;
  accent-color: var(--coral);
}
</style>
