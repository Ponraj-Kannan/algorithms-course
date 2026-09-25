<script setup>
import { ref, reactive, computed, onMounted, onBeforeUnmount, watch, nextTick } from 'vue';

defineProps({
  topic: { type: String, default: 'Backtracking Algorithms' },
  subTopic: { type: String, default: 'Grid Paths from Top to Bottom Corner' }
});

// ─── Multi-Language Code Definitions ─────────────────────────────────────────
const CODES = {
  java: [
    ['',              'import java.util.Scanner;'],
    ['',              'import java.util.List;'],
    ['',              'import java.util.ArrayList;'],
    ['',              ''],
    ['',              'public class Main {'],
    ['c_entry',       '    static List<List<Integer>> findAllPossiblePaths(int n, int m, int[][] grid) {'],
    ['c_init_ans',    '        List<List<Integer>> ans = new ArrayList<>();'],
    ['c_init_curr',   '        List<Integer> current = new ArrayList<>();'],
    ['c_call_solve',  '        solve(0, 0, n, m, grid, current, ans);'],
    ['c_return_ans',  '        return ans;'],
    ['',              '    }'],
    ['',              ''],
    ['c_solve_entry', '    static void solve(int r, int c, int n, int m, int[][] grid, List<Integer> current, List<List<Integer>> ans) {'],
    ['c_add_curr',    '        current.add(grid[r][c]);'],
    ['c_base_check',  '        if (r == n - 1 && c == m - 1) {'],
    ['c_save_sol',    '            ans.add(new ArrayList<>(current));'],
    ['c_base_pop',    '            current.remove(current.size() - 1);'],
    ['c_base_ret',    '            return;'],
    ['',              '        }'],
    ['c_chk_down',    '        if (r + 1 < n) {'],
    ['c_recurse_down','            solve(r + 1, c, n, m, grid, current, ans);'],
    ['',              '        }'],
    ['c_chk_right',   '        if (c + 1 < m) {'],
    ['c_recurse_right','            solve(r, c + 1, n, m, grid, current, ans);'],
    ['',              '        }'],
    ['c_backtrack',   '        current.remove(current.size() - 1);'],
    ['',              '    }'],
    ['',              ''],
    ['',              '    public static void main(String[] args) {'],
    ['m_scanner',     '        Scanner sc = new Scanner(System.in);'],
    ['m_read_n',      '        int n = sc.nextInt();'],
    ['m_read_m',      '        int m = sc.nextInt();'],
    ['m_alloc_grid',  '        int[][] grid = new int[n][m];'],
    ['m_loop_i',      '        for (int i = 0; i < n; i++) {'],
    ['m_loop_j',      '            for (int j = 0; j < m; j++) {'],
    ['m_read_cell',   '                grid[i][j] = sc.nextInt();'],
    ['',              '            }'],
    ['',              '        }'],
    ['m_call_solver', '        List<List<Integer>> paths = findAllPossiblePaths(n, m, grid);'],
    ['m_print_count', '        System.out.println(paths.size());'],
    ['m_loop_print',  '        for (int i = 0; i < paths.size(); i++) {'],
    ['m_print_elem',  '            System.out.println(paths.get(i));'],
    ['',              '        }'],
    ['m_done',        '    }'],
    ['',              '}']
  ],
  cpp: [
    ['',              '#include <iostream>'],
    ['',              '#include <vector>'],
    ['',              'using namespace std;'],
    ['',              ''],
    ['c_solve_entry', 'void solve(int r, int c, int n, int m, vector<vector<int>>& grid,'],
    ['',              '           vector<int>& current, vector<vector<int>>& ans) {'],
    ['c_add_curr',    '    current.push_back(grid[r][c]);'],
    ['c_base_check',  '    if (r == n - 1 && c == m - 1) {'],
    ['c_save_sol',    '        ans.push_back(current);'],
    ['c_base_pop',    '        current.pop_back();'],
    ['c_base_ret',    '        return;'],
    ['',              '    }'],
    ['c_chk_down',    '    if (r + 1 < n)'],
    ['c_recurse_down','        solve(r + 1, c, n, m, grid, current, ans);'],
    ['c_chk_right',   '    if (c + 1 < m)'],
    ['c_recurse_right','        solve(r, c + 1, n, m, grid, current, ans);'],
    ['c_backtrack',   '    current.pop_back();'],
    ['',              '}'],
    ['',              ''],
    ['c_entry',       'vector<vector<int>> findAllPossiblePaths(int n, int m, vector<vector<int>>& grid) {'],
    ['c_init_ans',    '    vector<vector<int>> ans;'],
    ['c_init_curr',   '    vector<int> current;'],
    ['c_call_solve',  '    solve(0, 0, n, m, grid, current, ans);'],
    ['c_return_ans',  '    return ans;'],
    ['',              '}'],
    ['',              ''],
    ['',              'int main() {'],
    ['m_scanner',     '    int n, m;'],
    ['m_read_n',      '    cin >> n;'],
    ['m_read_m',      '    cin >> m;'],
    ['m_alloc_grid',  '    vector<vector<int>> grid(n, vector<int>(m));'],
    ['m_loop_i',      '    for (int i = 0; i < n; i++) {'],
    ['m_loop_j',      '        for (int j = 0; j < m; j++) {'],
    ['m_read_cell',   '            cin >> grid[i][j];'],
    ['',              '        }'],
    ['',              '    }'],
    ['m_call_solver', '    vector<vector<int>> paths = findAllPossiblePaths(n, m, grid);'],
    ['m_print_count', '    cout << paths.size() << "\\n";'],
    ['m_loop_print',  '    for (int i = 0; i < (int)paths.size(); i++) {'],
    ['m_print_elem',  '        // print paths[i]'],
    ['',              '    }'],
    ['m_done',        '    return 0;'],
    ['',              '}']
  ],
  python: [
    ['',              'import sys'],
    ['',              ''],
    ['c_solve_entry', 'def solve(r, c, n, m, grid, current, ans):'],
    ['c_add_curr',    '    current.append(grid[r][c])'],
    ['c_base_check',  '    if r == n - 1 and c == m - 1:'],
    ['c_save_sol',    '        ans.append(list(current))'],
    ['c_base_pop',    '        current.pop()'],
    ['c_base_ret',    '        return'],
    ['c_chk_down',    '    if r + 1 < n:'],
    ['c_recurse_down','        solve(r + 1, c, n, m, grid, current, ans)'],
    ['c_chk_right',   '    if c + 1 < m:'],
    ['c_recurse_right','        solve(r, c + 1, n, m, grid, current, ans)'],
    ['c_backtrack',   '    current.pop()'],
    ['',              ''],
    ['c_entry',       'def findAllPossiblePaths(n, m, grid):'],
    ['c_init_ans',    '    ans = []'],
    ['c_init_curr',   '    current = []'],
    ['c_call_solve',  '    solve(0, 0, n, m, grid, current, ans)'],
    ['c_return_ans',  '    return ans'],
    ['',              ''],
    ['',              'def main():'],
    ['m_scanner',     '    data = sys.stdin.read().split()'],
    ['m_read_n',      '    n = int(data[0])'],
    ['m_read_m',      '    m = int(data[1])'],
    ['m_alloc_grid',  '    grid = [[0] * m for _ in range(n)]'],
    ['m_loop_i',      '    idx = 2'],
    ['m_loop_j',      '    for i in range(n):'],
    ['m_read_cell',   '        for j in range(m): grid[i][j] = int(data[idx]); idx += 1'],
    ['m_call_solver', '    paths = findAllPossiblePaths(n, m, grid)'],
    ['m_print_count', '    print(len(paths))'],
    ['m_loop_print',  '    for p in paths:'],
    ['m_print_elem',  '        print(p)'],
    ['m_done',        ''],
    ['',              'if __name__ == "__main__":'],
    ['',              '    main()']
  ],
  javascript: [
    ['',              '// Node.js solution'],
    ['',              ''],
    ['c_solve_entry', 'function solve(r, c, n, m, grid, current, ans) {'],
    ['c_add_curr',    '    current.push(grid[r][c]);'],
    ['c_base_check',  '    if (r === n - 1 && c === m - 1) {'],
    ['c_save_sol',    '        ans.push([...current]);'],
    ['c_base_pop',    '        current.pop();'],
    ['c_base_ret',    '        return;'],
    ['',              '    }'],
    ['c_chk_down',    '    if (r + 1 < n)'],
    ['c_recurse_down','        solve(r + 1, c, n, m, grid, current, ans);'],
    ['c_chk_right',   '    if (c + 1 < m)'],
    ['c_recurse_right','        solve(r, c + 1, n, m, grid, current, ans);'],
    ['c_backtrack',   '    current.pop();'],
    ['',              '}'],
    ['',              ''],
    ['c_entry',       'function findAllPossiblePaths(n, m, grid) {'],
    ['c_init_ans',    '    const ans = [];'],
    ['c_init_curr',   '    const current = [];'],
    ['c_call_solve',  '    solve(0, 0, n, m, grid, current, ans);'],
    ['c_return_ans',  '    return ans;'],
    ['',              '}'],
    ['',              ''],
    ['',              'function main() {'],
    ['m_scanner',     '    // read input tokens'],
    ['m_read_n',      '    const n = parseInt(tokens[0]);'],
    ['m_read_m',      '    const m = parseInt(tokens[1]);'],
    ['m_alloc_grid',  '    const grid = Array.from({length:n},()=>new Array(m).fill(0));'],
    ['m_loop_i',      '    let idx = 2;'],
    ['m_loop_j',      '    for (let i = 0; i < n; i++) {'],
    ['m_read_cell',   '        for (let j = 0; j < m; j++) grid[i][j] = parseInt(tokens[idx++]);'],
    ['',              '    }'],
    ['m_call_solver', '    const paths = findAllPossiblePaths(n, m, grid);'],
    ['m_print_count', '    console.log(paths.length);'],
    ['m_loop_print',  '    for (const p of paths)'],
    ['m_print_elem',  '        console.log(JSON.stringify(p));'],
    ['m_done',        '}'],
    ['',              'main();']
  ],
  c: [
    ['',              '#include <stdio.h>'],
    ['',              '#include <string.h>'],
    ['',              ''],
    ['',              '#define MAXN 10'],
    ['',              'int grid[MAXN][MAXN];'],
    ['',              'int allPaths[500][2*MAXN];'],
    ['',              'int pathSizes[500];'],
    ['',              'int pathCount = 0;'],
    ['',              'int current[2*MAXN];'],
    ['',              'int currSize = 0;'],
    ['',              ''],
    ['c_solve_entry', 'void solve(int r, int c, int n, int m) {'],
    ['c_add_curr',    '    current[currSize++] = grid[r][c];'],
    ['c_base_check',  '    if (r == n-1 && c == m-1) {'],
    ['c_save_sol',    '        memcpy(allPaths[pathCount], current, currSize*sizeof(int));'],
    ['',              '        pathSizes[pathCount++] = currSize;'],
    ['c_base_pop',    '        currSize--;'],
    ['c_base_ret',    '        return;'],
    ['',              '    }'],
    ['c_chk_down',    '    if (r + 1 < n)'],
    ['c_recurse_down','        solve(r + 1, c, n, m);'],
    ['c_chk_right',   '    if (c + 1 < m)'],
    ['c_recurse_right','        solve(r, c + 1, n, m);'],
    ['c_backtrack',   '    currSize--;'],
    ['',              '}'],
    ['',              ''],
    ['c_entry',       'void findAllPossiblePaths(int n, int m) {'],
    ['c_init_ans',    '    pathCount = 0;'],
    ['c_init_curr',   '    currSize = 0;'],
    ['c_call_solve',  '    solve(0, 0, n, m);'],
    ['c_return_ans',  '}'],
    ['',              ''],
    ['',              'int main() {'],
    ['m_scanner',     '    int n, m;'],
    ['m_read_n',      '    scanf("%d", &n);'],
    ['m_read_m',      '    scanf("%d", &m);'],
    ['m_alloc_grid',  '    // grid is global array'],
    ['m_loop_i',      '    for (int i = 0; i < n; i++) {'],
    ['m_loop_j',      '        for (int j = 0; j < m; j++) {'],
    ['m_read_cell',   '            scanf("%d", &grid[i][j]);'],
    ['',              '        }'],
    ['',              '    }'],
    ['m_call_solver', '    findAllPossiblePaths(n, m);'],
    ['m_print_count', '    printf("%d\\n", pathCount);'],
    ['m_loop_print',  '    for (int i = 0; i < pathCount; i++) {'],
    ['m_print_elem',  '        // print allPaths[i]'],
    ['',              '    }'],
    ['m_done',        '    return 0;'],
    ['',              '}']
  ]
};

const PSEUDOCODE = [
  'function findAllPossiblePaths(n, m, grid):',
  '    ans = []',
  '    current = []',
  '    solve(0, 0, n, m, grid, current, ans)',
  '    return ans',
  '',
  'function solve(r, c, n, m, grid, current, ans):',
  '    current.push(grid[r][c])          // add cell value to path',
  '    if r == n-1 and c == m-1:         // Base case: destination reached',
  '        ans.push(copy of current)',
  '        current.pop()                 // pop before return',
  '        return',
  '    if r + 1 < n:                     // Move Down',
  '        solve(r+1, c, n, m, grid, current, ans)',
  '    if c + 1 < m:                     // Move Right',
  '        solve(r, c+1, n, m, grid, current, ans)',
  '    current.pop()                     // Backtrack'
];

// ─── Step Builder ─────────────────────────────────────────────────────────────
function buildSteps(nParam, mParam, rawGrid) {
  const steps = [];
  const n = Math.max(1, Math.min(5, parseInt(nParam, 10) || 3));
  const m = Math.max(1, Math.min(5, parseInt(mParam, 10) || 3));

  const grid = [];
  for (let i = 0; i < n; i++) {
    const row = [];
    for (let j = 0; j < m; j++) {
      row.push((rawGrid[i] && rawGrid[i][j] !== undefined) ? rawGrid[i][j] : (i * m + j + 1));
    }
    grid.push(row);
  }

  const solutions = [];
  const callStackFrames = [];

  function frm(title, rowsArr) { return { title, rows: rowsArr }; }

  function getGridSnap(curR, curC, pathSet, actionStatus, downValid, rightValid) {
    const cells = [];
    for (let r = 0; r < n; r++) {
      const row = [];
      for (let c = 0; c < m; c++) {
        const key = `${r},${c}`;
        const isStart = r === 0 && c === 0;
        const isDest = r === n - 1 && c === m - 1;
        const isCur = r === curR && c === curC;
        const inPath = pathSet && pathSet.has(key);
        row.push({ r, c, val: grid[r][c], isStart, isDest, isCur, inPath, actionStatus: isCur ? actionStatus : 'normal' });
      }
      cells.push(row);
    }
    return { cells, downValid: downValid !== undefined ? downValid : null, rightValid: rightValid !== undefined ? rightValid : null };
  }

  function getStackSnapshot(extraRows = []) {
    const list = [frm('main()', [['n', String(n)], ['m', String(m)]])];
    for (const f of callStackFrames) {
      list.push(frm(f.name, f.args));
    }
    if (extraRows.length && list.length > 0) {
      const top = list[list.length - 1];
      if (top) top.rows = [...top.rows, ...extraRows];
    }
    return list;
  }

  // Phase 1: main() input reading
  steps.push({ phase: 'input', badge: `Scanner sc = new Scanner(System.in); → Initializing input stream in main().`, code: 'm_scanner', vars: [frm('main()', [['n', '?'], ['m', '?']])], gridSnap: getGridSnap(-1, -1, null, 'normal'), current: [], solutions: [...solutions], solCount: 0, inputPhase: { readI: -1, readJ: -1, allocDone: false }, n, m });
  steps.push({ phase: 'input', badge: `int n = sc.nextInt(); → Read grid row count n = ${n}.`, code: 'm_read_n', vars: [frm('main()', [['n', String(n)], ['m', '?']])], gridSnap: getGridSnap(-1, -1, null, 'normal'), current: [], solutions: [...solutions], solCount: 0, inputPhase: { readI: -1, readJ: -1, allocDone: false }, n, m });
  steps.push({ phase: 'input', badge: `int m = sc.nextInt(); → Read grid column count m = ${m}.`, code: 'm_read_m', vars: [frm('main()', [['n', String(n)], ['m', String(m)]])], gridSnap: getGridSnap(-1, -1, null, 'normal'), current: [], solutions: [...solutions], solCount: 0, inputPhase: { readI: -1, readJ: -1, allocDone: false }, n, m });
  steps.push({ phase: 'input', badge: `int[][] grid = new int[${n}][${m}]; → Memory allocated for ${n}x${m} grid. All cells initialized with default value 0.`, code: 'm_alloc_grid', vars: [frm('main()', [['n', String(n)], ['m', String(m)], ['grid', `new int[${n}][${m}]`]])], gridSnap: getGridSnap(-1, -1, null, 'alloc'), current: [], solutions: [...solutions], solCount: 0, inputPhase: { readI: -1, readJ: -1, allocDone: true, showZeros: true }, n, m });

  const partialGrid = Array.from({ length: n }, () => new Array(m).fill(0));

  for (let i = 0; i < n; i++) {
    steps.push({ phase: 'input', badge: `for (int i = ${i}; i < ${n}; i++) → Outer loop: i = ${i}. Will read row ${i}.`, code: 'm_loop_i', vars: [frm('main()', [['n', String(n)], ['m', String(m)], ['i', String(i)]])], gridSnap: getGridSnap(i, -1, null, 'loop_row'), current: [], solutions: [...solutions], solCount: 0, inputPhase: { readI: i, readJ: -1, allocDone: true, partialGrid: partialGrid.map(r => [...r]) }, n, m });
    for (let j = 0; j < m; j++) {
      steps.push({ phase: 'input', badge: `for (int j = ${j}; j < ${m}; j++) → Inner loop: j = ${j}. Reading cell (${i}, ${j}).`, code: 'm_loop_j', vars: [frm('main()', [['n', String(n)], ['m', String(m)], ['i', String(i)], ['j', String(j)]])], gridSnap: getGridSnap(i, j, null, 'reading'), current: [], solutions: [...solutions], solCount: 0, inputPhase: { readI: i, readJ: j, allocDone: true, partialGrid: partialGrid.map(r => [...r]) }, n, m });
      partialGrid[i][j] = grid[i][j];
      steps.push({ phase: 'input', badge: `grid[${i}][${j}] = sc.nextInt(); → Read value ${grid[i][j]} for cell (${i}, ${j}). Updating cell from [DEFAULT: 0] to ${grid[i][j]}.`, code: 'm_read_cell', vars: [frm('main()', [['n', String(n)], ['m', String(m)], ['i', String(i)], ['j', String(j)], [`grid[${i}][${j}]`, String(grid[i][j])]])], gridSnap: getGridSnap(i, j, null, 'updated'), current: [], solutions: [...solutions], solCount: 0, inputPhase: { readI: i, readJ: j, allocDone: true, partialGrid: partialGrid.map(r => [...r]), justUpdated: true }, n, m });
    }
  }

  steps.push({ phase: 'input', badge: `List<List<Integer>> paths = findAllPossiblePaths(${n}, ${m}, grid); → All cells populated. Invoking recursive backtracking solver.`, code: 'm_call_solver', vars: [frm('main()', [['n', String(n)], ['m', String(m)]])], gridSnap: getGridSnap(-1, -1, null, 'normal'), current: [], solutions: [...solutions], solCount: 0, inputPhase: { readI: -1, readJ: -1, allocDone: true, allDone: true }, n, m });

  steps.push({ phase: 'solver', badge: `findAllPossiblePaths(n=${n}, m=${m}, grid) entry → Initializing result list [ans] and path buffer [current].`, code: 'c_entry', vars: [frm('main()', [['n', String(n)], ['m', String(m)]]), frm('findAllPossiblePaths()', [['n', String(n)], ['m', String(m)]])], gridSnap: getGridSnap(-1, -1, new Set(), 'normal'), current: [], solutions: [...solutions], solCount: 0, n, m });
  steps.push({ phase: 'solver', badge: `List<List<Integer>> ans = new ArrayList<>(); → Output storage [ans] initialized.`, code: 'c_init_ans', vars: [frm('main()', [['n', String(n)], ['m', String(m)]]), frm('findAllPossiblePaths()', [['ans', '[]']])], gridSnap: getGridSnap(-1, -1, new Set(), 'normal'), current: [], solutions: [...solutions], solCount: 0, n, m });
  steps.push({ phase: 'solver', badge: `List<Integer> current = new ArrayList<>(); → Path buffer [current] initialized. Holds values along active DFS path.`, code: 'c_init_curr', vars: [frm('main()', [['n', String(n)], ['m', String(m)]]), frm('findAllPossiblePaths()', [['ans', '[]'], ['current', '[]']])], gridSnap: getGridSnap(-1, -1, new Set(), 'normal'), current: [], solutions: [...solutions], solCount: 0, n, m });
  steps.push({ phase: 'solver', badge: `solve(0, 0, ${n}, ${m}, grid, current, ans); → Launching DFS from cell (0, 0) = [START].`, code: 'c_call_solve', vars: [frm('main()', [['n', String(n)], ['m', String(m)]]), frm('findAllPossiblePaths()', [['ans', '[]'], ['current', '[]']])], gridSnap: getGridSnap(0, 0, new Set(), 'push'), current: [], solutions: [...solutions], solCount: 0, n, m });

  const currentPath = [];
  const currentVals = [];

  function simulateSolve(r, c) {
    const pathSet = new Set(currentPath.map(p => `${p[0]},${p[1]}`));
    callStackFrames.push({ name: `solve(r=${r}, c=${c})`, args: [['r', String(r)], ['c', String(c)], ['current.size', String(currentVals.length)]] });

    steps.push({ phase: 'solver', badge: `solve(r=${r}, c=${c}) → Entering cell (${r}, ${c}) = grid[${r}][${c}] = ${grid[r][c]}. Recursion depth: ${currentVals.length + 1}.`, code: 'c_solve_entry', vars: getStackSnapshot(), gridSnap: getGridSnap(r, c, pathSet, 'normal'), current: [...currentVals], solutions: [...solutions], solCount: solutions.length, n, m, activeDir: null });

    currentPath.push([r, c]);
    currentVals.push(grid[r][c]);
    const newPathSet = new Set(currentPath.map(p => `${p[0]},${p[1]}`));

    steps.push({ phase: 'solver', badge: `current.add(grid[${r}][${c}] = ${grid[r][c]}); [+ PUSH] → Appended value ${grid[r][c]} to path buffer. Buffer: [${currentVals.join(', ')}]. size = ${currentVals.length}.`, code: 'c_add_curr', vars: getStackSnapshot([['+ PUSH', String(grid[r][c])]]), gridSnap: getGridSnap(r, c, newPathSet, 'push'), current: [...currentVals], solutions: [...solutions], solCount: solutions.length, n, m, activeDir: null });

    const isBase = (r === n - 1 && c === m - 1);
    steps.push({ phase: 'solver', badge: `Checking base case: if (r == n-1 && c == m-1) → (${r} == ${n-1} && ${c} == ${m-1}) → ${isBase ? 'TRUE! Destination [DEST] reached!' : 'FALSE. Continue exploring.'}`, code: 'c_base_check', vars: getStackSnapshot(), gridSnap: getGridSnap(r, c, newPathSet, isBase ? 'found' : 'normal'), current: [...currentVals], solutions: [...solutions], solCount: solutions.length, n, m, activeDir: null });

    if (isBase) {
      solutions.push([...currentVals]);
      steps.push({ phase: 'solver', badge: `[VALID] PATH FOUND #${solutions.length}: ans.add(new ArrayList<>(current)); → Saved path [${currentVals.join(', ')}] to allPaths. Total paths: ${solutions.length}.`, code: 'c_save_sol', vars: getStackSnapshot([['[VALID]', `Path #${solutions.length}`]]), gridSnap: getGridSnap(r, c, newPathSet, 'found'), current: [...currentVals], solutions: [...solutions], solCount: solutions.length, n, m, activeDir: null });
      currentPath.pop();
      currentVals.pop();
      const afterPopSet = new Set(currentPath.map(p => `${p[0]},${p[1]}`));
      steps.push({ phase: 'solver', badge: `current.remove(current.size() - 1); [- POP] → Popped ${grid[r][c]} from path buffer before return. Buffer: [${currentVals.join(', ')}].`, code: 'c_base_pop', vars: getStackSnapshot([['- POP', String(grid[r][c])]]), gridSnap: getGridSnap(r, c, afterPopSet, 'pop'), current: [...currentVals], solutions: [...solutions], solCount: solutions.length, n, m, activeDir: null });
      steps.push({ phase: 'solver', badge: `return; → Returning from base case. Backtracking to explore more paths.`, code: 'c_base_ret', vars: getStackSnapshot(), gridSnap: getGridSnap(r, c, afterPopSet, 'normal'), current: [...currentVals], solutions: [...solutions], solCount: solutions.length, n, m, activeDir: null });
      callStackFrames.pop();
      return;
    }

    const canDown = r + 1 < n;
    steps.push({ phase: 'solver', badge: `if (r + 1 < n): (${r}+1 < ${n}) → (${r+1} < ${n}) → ${canDown ? '[VALID] Can move Down to (' + (r+1) + ', ' + c + ')' : '[OUT OF BOUNDS] Cannot move Down. r+1 = ' + (r+1) + ' >= n = ' + n}`, code: 'c_chk_down', vars: getStackSnapshot([['r+1', String(r+1)], ['< n', canDown ? 'VALID' : 'OUT OF BOUNDS']]), gridSnap: getGridSnap(r, c, newPathSet, 'checking', canDown, null), current: [...currentVals], solutions: [...solutions], solCount: solutions.length, n, m, activeDir: 'down', downValid: canDown, rightValid: null });
    if (canDown) {
      steps.push({ phase: 'solver', badge: `solve(r+1=${r+1}, c=${c}, n, m, grid, current, ans); → Recursing Down to cell (${r+1}, ${c}).`, code: 'c_recurse_down', vars: getStackSnapshot(), gridSnap: getGridSnap(r+1, c, newPathSet, 'normal'), current: [...currentVals], solutions: [...solutions], solCount: solutions.length, n, m, activeDir: 'down' });
      simulateSolve(r + 1, c);
    }

    const canRight = c + 1 < m;
    steps.push({ phase: 'solver', badge: `if (c + 1 < m): (${c}+1 < ${m}) → (${c+1} < ${m}) → ${canRight ? '[VALID] Can move Right to (' + r + ', ' + (c+1) + ')' : '[OUT OF BOUNDS] Cannot move Right. c+1 = ' + (c+1) + ' >= m = ' + m}`, code: 'c_chk_right', vars: getStackSnapshot([['c+1', String(c+1)], ['< m', canRight ? 'VALID' : 'OUT OF BOUNDS']]), gridSnap: getGridSnap(r, c, newPathSet, 'checking', null, canRight), current: [...currentVals], solutions: [...solutions], solCount: solutions.length, n, m, activeDir: 'right', downValid: null, rightValid: canRight });
    if (canRight) {
      steps.push({ phase: 'solver', badge: `solve(r=${r}, c+1=${c+1}, n, m, grid, current, ans); → Recursing Right to cell (${r}, ${c+1}).`, code: 'c_recurse_right', vars: getStackSnapshot(), gridSnap: getGridSnap(r, c+1, newPathSet, 'normal'), current: [...currentVals], solutions: [...solutions], solCount: solutions.length, n, m, activeDir: 'right' });
      simulateSolve(r, c + 1);
    }

    currentPath.pop();
    currentVals.pop();
    const backPathSet = new Set(currentPath.map(p => `${p[0]},${p[1]}`));
    steps.push({ phase: 'solver', badge: `current.remove(current.size() - 1); [- POP] → Backtracking: Popped ${grid[r][c]} from path buffer. Restoring to [${currentVals.join(', ')}].`, code: 'c_backtrack', vars: getStackSnapshot([['- POP', String(grid[r][c])]]), gridSnap: getGridSnap(r, c, backPathSet, 'pop'), current: [...currentVals], solutions: [...solutions], solCount: solutions.length, n, m, activeDir: null });
    callStackFrames.pop();
  }

  simulateSolve(0, 0);

  steps.push({ phase: 'solver', badge: `return ans; → DFS complete! Returning ${solutions.length} path(s) to main(). Total unique paths from (0,0) to (${n-1},${m-1}).`, code: 'c_return_ans', vars: [frm('main()', [['n', String(n)], ['m', String(m)]]), frm('findAllPossiblePaths()', [['total_paths', String(solutions.length)]])], gridSnap: getGridSnap(-1, -1, new Set(), 'normal'), current: [], solutions: [...solutions], solCount: solutions.length, n, m });
  steps.push({ phase: 'done', badge: `System.out.println(paths.size()); → Printing total paths found: ${solutions.length}.`, code: 'm_print_count', vars: [frm('main()', [['paths.size()', String(solutions.length)]])], gridSnap: getGridSnap(-1, -1, new Set(), 'normal'), current: [], solutions: [...solutions], solCount: solutions.length, n, m });

  for (let _pi = 0; _pi < solutions.length; _pi++) {
    steps.push({ phase: 'done', badge: `for (int i = ${_pi}; i < ${solutions.length}; i++) → Printing path #${_pi+1}: [${solutions[_pi].join(', ')}].`, code: 'm_loop_print', vars: [frm('main()', [['i', String(_pi)]])], gridSnap: getGridSnap(-1, -1, new Set(), 'normal'), current: [...solutions[_pi]], solutions: [...solutions], solCount: solutions.length, selectedPath: _pi, n, m });
    steps.push({ phase: 'done', badge: `System.out.println(paths.get(${_pi})); → Output: [${solutions[_pi].join(', ')}]`, code: 'm_print_elem', vars: [frm('main()', [['i', String(_pi)], ['output', `[${solutions[_pi].join(', ')}]`]])], gridSnap: getGridSnap(-1, -1, new Set(), 'normal'), current: [...solutions[_pi]], solutions: [...solutions], solCount: solutions.length, selectedPath: _pi, n, m });
  }

  steps.push({ phase: 'done', badge: `Program execution complete. Grid path backtracking finished. Found ${solutions.length} unique path(s) from (0,0) to (${n-1},${m-1}).`, code: 'm_done', vars: [frm('main()', [['status', 'finished'], ['paths_found', String(solutions.length)]])], gridSnap: getGridSnap(-1, -1, new Set(), 'normal'), current: [], solutions: [...solutions], solCount: solutions.length, n, m });
  return steps;
}

// ─── Reactive State ───────────────────────────────────────────────────────────
const DEFAULT_N = 3;
const DEFAULT_M = 3;
const DEFAULT_GRID = [[1,2,3],[4,5,6],[7,8,9]];

const inputN = ref(DEFAULT_N);
const inputM = ref(DEFAULT_M);
const gridInputStr = ref('1,2,3\n4,5,6\n7,8,9');
const activeN = ref(DEFAULT_N);
const activeM = ref(DEFAULT_M);
const activeGrid = ref(DEFAULT_GRID.map(r => [...r]));
const lang = ref('java');
const speed = ref(650);
const si = ref(0);
const playing = ref(false);
const vizHeight = ref(340);
const tableHeight = ref(60);
const leftWidth = ref(52);
const rightTab = ref('code');
const selectedSolutionIdx = ref(-1);

const stepsData = reactive({ steps: buildSteps(DEFAULT_N, DEFAULT_M, DEFAULT_GRID) });
const steps = computed(() => stepsData.steps);
const s = computed(() => steps.value[Math.max(0, Math.min(si.value, steps.value.length - 1))] || {});
const codeLines = computed(() => CODES[lang.value] || []);

let playTimer = null;

function onChipsWheel(e) { if (e.currentTarget) e.currentTarget.scrollLeft += e.deltaY; }

function parseGridInput(str, n, m) {
  try {
    const rows = str.trim().split(/\n+/);
    const g = [];
    for (let i = 0; i < n; i++) {
      const rowStr = rows[i] || '';
      const vals = rowStr.trim().split(/[\s,]+/).map(x => parseInt(x, 10)).filter(x => !isNaN(x));
      const row = [];
      for (let j = 0; j < m; j++) row.push(vals[j] !== undefined ? vals[j] : (i * m + j + 1));
      g.push(row);
    }
    return g;
  } catch (e) {
    return Array.from({ length: n }, (_, i) => Array.from({ length: m }, (_, j) => i * m + j + 1));
  }
}

function applyInput() {
  const nVal = parseInt(inputN.value, 10);
  const mVal = parseInt(inputM.value, 10);
  if (isNaN(nVal) || nVal < 1 || nVal > 5) { alert('Please enter N between 1 and 5.'); inputN.value = 3; return; }
  if (isNaN(mVal) || mVal < 1 || mVal > 5) { alert('Please enter M between 1 and 5.'); inputM.value = 3; return; }
  const parsedGrid = parseGridInput(gridInputStr.value, nVal, mVal);
  activeN.value = nVal; activeM.value = mVal; activeGrid.value = parsedGrid;
  playing.value = false; selectedSolutionIdx.value = -1;
  stepsData.steps = buildSteps(nVal, mVal, parsedGrid);
  si.value = 0;
  if (typeof window !== 'undefined') window.scrollTo(0, 0);
}

function loadPreset(p) {
  if (p === 'default') { inputN.value = 3; inputM.value = 3; gridInputStr.value = '1,2,3\n4,5,6\n7,8,9'; }
  else if (p === 'small') { inputN.value = 2; inputM.value = 2; gridInputStr.value = '1,2\n3,4'; }
  else if (p === 'rect') { inputN.value = 2; inputM.value = 3; gridInputStr.value = '1,2,3\n4,5,6'; }
  else if (p === '4x4') { inputN.value = 4; inputM.value = 4; gridInputStr.value = '1,2,3,4\n5,6,7,8\n9,10,11,12\n13,14,15,16'; }
  applyInput();
}

function stepBy(d) { selectedSolutionIdx.value = -1; si.value = Math.max(0, Math.min(steps.value.length - 1, si.value + d)); }

function togglePlay() {
  selectedSolutionIdx.value = -1;
  const next = !playing.value;
  if (next && si.value >= steps.value.length - 1) si.value = 0;
  playing.value = next;
}

function tick() {
  clearTimeout(playTimer);
  if (!playing.value) return;
  if (si.value >= steps.value.length - 1) { playing.value = false; return; }
  playTimer = setTimeout(() => { si.value = Math.min(steps.value.length - 1, si.value + 1); tick(); }, 2100 - speed.value);
}

watch(playing, v => { if (v) tick(); else clearTimeout(playTimer); });

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
      container.scrollTop = Math.max(0, container.scrollTop - (contRect.top - activeRect.top) - 24);
    } else if (activeRect.bottom > contRect.bottom) {
      container.scrollTop = container.scrollTop + (activeRect.bottom - contRect.bottom) + 24;
    }
  });
}

watch(() => s.value.code, scrollActiveCodeLine);
watch(lang, scrollActiveCodeLine);
watch(rightTab, v => { if (v === 'code') scrollActiveCodeLine(); });

function onKeydown(e) {
  const tag = e.target.tagName;
  if (tag === 'INPUT' || tag === 'SELECT' || tag === 'TEXTAREA') return;
  if (e.key === 'ArrowRight') stepBy(1);
  if (e.key === 'ArrowLeft') stepBy(-1);
  if (e.key === ' ') { e.preventDefault(); togglePlay(); }
}

function viewSolution(idx) { selectedSolutionIdx.value = idx; playing.value = false; }

const displayGridSnap = computed(() => s.value.gridSnap || { cells: [], downValid: null, rightValid: null });
const displayCurrent = computed(() => {
  if (selectedSolutionIdx.value >= 0 && s.value.solutions && s.value.solutions[selectedSolutionIdx.value]) return s.value.solutions[selectedSolutionIdx.value];
  return s.value.current || [];
});
const memGrid = computed(() => { const n = s.value.n || activeN.value; const m = s.value.m || activeM.value; return `${n} x ${m} x 4 B = ${n*m*4} B`; });
const memPathBuf = computed(() => { const n = s.value.n || activeN.value; const m = s.value.m || activeM.value; return `${n+m-1} elements (max)`; });
const memAllPaths = computed(() => { const sc = s.value.solCount || 0; const n = s.value.n || activeN.value; const m = s.value.m || activeM.value; return `${sc} x ${n+m-1} elements`; });
const stackDepth = computed(() => { const cur = displayCurrent.value.length; const n = s.value.n || activeN.value; const m = s.value.m || activeM.value; return `${cur} / ${n+m-1} (max)`; });
const currentRC = computed(() => {
  const cells = displayGridSnap.value.cells || [];
  for (const row of cells) {
    for (const cell of row) {
      if (cell.isCur) return { r: cell.r, c: cell.c };
    }
  }
  return { r: '-', c: '-' };
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
  rsz.addEventListener('mousedown', onDown); document.addEventListener('mousemove', onMove); document.addEventListener('mouseup', onUp);
  return () => { rsz.removeEventListener('mousedown', onDown); document.removeEventListener('mousemove', onMove); document.removeEventListener('mouseup', onUp); };
}

function initVResizer(elRef, valueRef, minH, maxH) {
  const rsz = elRef.value;
  if (!rsz) return;
  let dragging = false, startY = 0, startH = 0;
  const onDown = e => { dragging = true; startY = e.clientY; startH = valueRef.value; rsz.classList.add('drag'); document.body.style.userSelect = 'none'; e.preventDefault(); };
  const onMove = e => { if (!dragging) return; valueRef.value = Math.max(minH, Math.min(maxH, startH + (e.clientY - startY))); };
  const onUp = () => { if (!dragging) return; dragging = false; rsz.classList.remove('drag'); document.body.style.userSelect = ''; };
  rsz.addEventListener('mousedown', onDown); document.addEventListener('mousemove', onMove); document.addEventListener('mouseup', onUp);
  return () => { rsz.removeEventListener('mousedown', onDown); document.removeEventListener('mousemove', onMove); document.removeEventListener('mouseup', onUp); };
}

let cleanupFns = [];
onMounted(() => {
  document.addEventListener('keydown', onKeydown);
  cleanupFns.push(initHResizer());
  cleanupFns.push(initVResizer(vizResizerRef, vizHeight, 200, 600));
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
              <label>N (rows):</label>
              <input type="number" min="1" max="5" v-model.number="inputN" class="ll-text-input" @keyup.enter="applyInput" style="width: 44px;" />
            </div>
            <div class="ll-input-group">
              <label>M (cols):</label>
              <input type="number" min="1" max="5" v-model.number="inputM" class="ll-text-input" @keyup.enter="applyInput" style="width: 44px;" />
            </div>
            <div class="ll-input-group">
              <label>grid[][]:</label>
              <textarea v-model="gridInputStr" class="ll-text-input gp-grid-input" @keyup.ctrl.enter="applyInput" placeholder="e.g. 1,2,3&#10;4,5,6&#10;7,8,9" title="Enter grid values row by row, comma or space separated" rows="1"></textarea>
            </div>
            <div class="ll-preset-group">
              <button class="ll-preset-btn" @click="loadPreset('small')" title="2x2 grid">2x2</button>
              <button class="ll-preset-btn" @click="loadPreset('default')" title="3x3 grid">3x3</button>
              <button class="ll-preset-btn" @click="loadPreset('rect')" title="2x3 grid">2x3</button>
              <button class="ll-preset-btn" @click="loadPreset('4x4')" title="4x4 grid">4x4</button>
            </div>
            <button class="ll-viz-btn" @click="applyInput">&#9654; Visualize</button>
            <div class="ll-nav-controls">
              <button class="ll-nav-btn" title="First step" @click="stepBy(-steps.length)">&#171;</button>
              <button class="ll-nav-btn" title="Previous step" @click="stepBy(-1)">&#8249; Prev</button>
              <button class="ll-play-btn" @click="togglePlay">{{ playing ? '\u23F8 Pause' : '\u25B6 Play' }}</button>
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
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">N</span><b class="ll-c-blue">{{ s.n || activeN }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">M</span><b class="ll-c-blue">{{ s.m || activeM }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">r</span><b class="ll-c-purple">{{ currentRC.r }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">c</span><b class="ll-c-purple">{{ currentRC.c }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">phase</span><b class="ll-c-orange">{{ s.phase || 'input' }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">path.size</span><b class="ll-c-purple">{{ displayCurrent.length }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">allPaths</span><b class="ll-c-green">{{ s.solCount || 0 }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">stackDepth</span><b class="ll-c-orange">{{ stackDepth }}</b></span>
                  </div>

                  <!-- BOARD CONTAINER -->
                  <div class="ll-board-container">

                    <!-- Tier 1: 2D Grid Matrix -->
                    <div class="gp-tier-title">Tier 1 &mdash; Grid Matrix <code>int[][] grid</code></div>
                    <div class="ll-board-frame gp-grid-frame">
                      <div class="ll-col-indices">
                        <div class="ll-axis-spacer"></div>
                        <div v-for="c in (s.m || activeM)" :key="'col-h-' + c" class="ll-axis-label">{{ c - 1 }}</div>
                      </div>
                      <div class="ll-board-rows">
                        <div v-for="(row, rIdx) in (displayGridSnap.cells || [])" :key="'row-' + rIdx" class="ll-board-row">
                          <div class="ll-axis-label ll-row-label">{{ rIdx }}</div>
                          <div
                            v-for="(cell, cIdx) in row"
                            :key="'cell-' + rIdx + '-' + cIdx"
                            class="ll-chess-cell gp-grid-cell"
                            :class="{
                              'gp-cell-start': cell.isStart && !cell.isCur,
                              'gp-cell-dest': cell.isDest && !cell.isCur,
                              'gp-cell-curr': cell.isCur && cell.actionStatus === 'push',
                              'gp-cell-pop': cell.isCur && cell.actionStatus === 'pop',
                              'gp-cell-found': cell.isCur && cell.actionStatus === 'found',
                              'gp-cell-active': cell.isCur && !['push','pop','found','reading','updated'].includes(cell.actionStatus),
                              'gp-cell-path': cell.inPath && !cell.isCur,
                              'gp-cell-alloc': s.inputPhase && s.inputPhase.showZeros && !s.inputPhase.allDone,
                              'gp-cell-reading': s.inputPhase && s.inputPhase.readI === rIdx && s.inputPhase.readJ === cIdx && !s.inputPhase.justUpdated,
                              'gp-cell-updated': s.inputPhase && s.inputPhase.readI === rIdx && s.inputPhase.readJ === cIdx && s.inputPhase.justUpdated
                            }"
                            :title="`grid[${rIdx}][${cIdx}]`"
                          >
                            <template v-if="s.inputPhase && !s.inputPhase.allDone">
                              <span class="gp-cell-val">{{ (s.inputPhase.partialGrid && s.inputPhase.partialGrid[rIdx] && s.inputPhase.partialGrid[rIdx][cIdx] !== undefined) ? s.inputPhase.partialGrid[rIdx][cIdx] : 0 }}</span>
                              <span v-if="s.inputPhase.readI === rIdx && s.inputPhase.readJ === cIdx && !s.inputPhase.justUpdated" class="gp-badge-marker gp-badge-reading">[READING]</span>
                              <span v-else-if="s.inputPhase.readI === rIdx && s.inputPhase.readJ === cIdx && s.inputPhase.justUpdated" class="gp-badge-marker gp-badge-updated">[UPDATED]</span>
                              <span v-else-if="s.inputPhase.showZeros" class="gp-badge-marker gp-badge-default">[DEFAULT: 0]</span>
                            </template>
                            <template v-else>
                              <span class="gp-cell-val">{{ cell.val }}</span>
                              <span v-if="cell.isStart && !cell.isDest" class="gp-badge-marker gp-badge-start">[START]</span>
                              <span v-else-if="cell.isDest && !cell.isStart" class="gp-badge-marker gp-badge-dest">[DEST]</span>
                              <span v-else-if="cell.isCur" class="gp-badge-marker gp-badge-curr">[CURR]</span>
                            </template>
                          </div>
                        </div>
                      </div>
                    </div>

                    <!-- Tier 2: Direction Vectors & Bounds Checks -->
                    <div class="gp-tier-title">Tier 2 &mdash; Direction Vectors &amp; Bounds Check</div>
                    <div class="gp-dir-panel">
                      <div class="gp-dir-card" :class="{ 'gp-dir-active': s.activeDir === 'down', 'gp-dir-valid': s.activeDir === 'down' && s.downValid === true, 'gp-dir-oob': s.activeDir === 'down' && s.downValid === false }">
                        <div class="gp-dir-arrow">&darr;</div>
                        <div class="gp-dir-label">Down</div>
                        <div class="gp-dir-expr"><code>(r+1, c)</code></div>
                        <div class="gp-dir-check">Check: <code>r+1 &lt; n</code></div>
                        <div class="gp-dir-result">
                          <span v-if="s.activeDir === 'down' && s.downValid === true" class="gp-valid-badge">[VALID]</span>
                          <span v-else-if="s.activeDir === 'down' && s.downValid === false" class="gp-oob-badge">[OUT OF BOUNDS]</span>
                          <span v-else class="gp-neutral-badge">—</span>
                        </div>
                      </div>
                      <div class="gp-dir-card" :class="{ 'gp-dir-active': s.activeDir === 'right', 'gp-dir-valid': s.activeDir === 'right' && s.rightValid === true, 'gp-dir-oob': s.activeDir === 'right' && s.rightValid === false }">
                        <div class="gp-dir-arrow">&rarr;</div>
                        <div class="gp-dir-label">Right</div>
                        <div class="gp-dir-expr"><code>(r, c+1)</code></div>
                        <div class="gp-dir-check">Check: <code>c+1 &lt; m</code></div>
                        <div class="gp-dir-result">
                          <span v-if="s.activeDir === 'right' && s.rightValid === true" class="gp-valid-badge">[VALID]</span>
                          <span v-else-if="s.activeDir === 'right' && s.rightValid === false" class="gp-oob-badge">[OUT OF BOUNDS]</span>
                          <span v-else class="gp-neutral-badge">—</span>
                        </div>
                      </div>
                    </div>

                    <!-- Tier 3: Dynamic Path Buffer -->
                    <div class="gp-tier-title">Tier 3 &mdash; Path Buffer <code>ArrayList&lt;Integer&gt; current</code></div>
                    <div class="gp-buf-panel">
                      <div class="gp-buf-meta">
                        <span>size = <b class="ll-c-purple">{{ displayCurrent.length }}</b></span>
                        <span style="margin-left:8px">capacity = <b class="ll-c-blue">{{ (s.n || activeN) + (s.m || activeM) - 1 }}</b></span>
                      </div>
                      <div class="gp-buf-slots">
                        <template v-for="idx in ((s.n || activeN) + (s.m || activeM) - 1)" :key="'slot-' + idx">
                          <div class="gp-buf-slot" :class="{ 'gp-buf-filled': idx - 1 < displayCurrent.length, 'gp-buf-empty': idx - 1 >= displayCurrent.length, 'gp-buf-just-pushed': (idx - 1 === displayCurrent.length - 1) && s.code === 'c_add_curr', 'gp-buf-just-popped': (idx - 1 === displayCurrent.length) && (s.code === 'c_backtrack' || s.code === 'c_base_pop') }">
                            <span class="gp-buf-idx">[{{ idx - 1 }}]</span>
                            <span class="gp-buf-val">{{ idx - 1 < displayCurrent.length ? displayCurrent[idx - 1] : '\u00a0' }}</span>
                            <span v-if="idx - 1 === displayCurrent.length - 1 && s.code === 'c_add_curr'" class="gp-push-badge">+ PUSH</span>
                            <span v-if="idx - 1 === displayCurrent.length && (s.code === 'c_backtrack' || s.code === 'c_base_pop')" class="gp-pop-badge">- POP</span>
                          </div>
                        </template>
                        <div v-if="displayCurrent.length === 0" class="gp-buf-empty-msg">&lang; empty &rang;</div>
                      </div>
                    </div>

                    <!-- Tier 4: Collected Solutions -->
                    <div class="gp-tier-title">Tier 4 &mdash; Output Storage <code>allPaths</code></div>
                    <div class="gp-solutions-panel">
                      <div v-if="!s.solutions || s.solutions.length === 0" class="gp-no-solutions">No paths collected yet.</div>
                      <template v-else>
                        <button v-for="(sol, idx) in s.solutions" :key="'path-btn-' + idx" class="gp-sol-btn" :class="{ active: selectedSolutionIdx === idx }" @click="viewSolution(idx)" :title="`Click to highlight Path #${idx + 1} on grid`">
                          Path #{{ idx + 1 }}: [{{ sol.join(', ') }}]
                        </button>
                      </template>
                    </div>

                    <!-- Tier 5: Memory Footprint Summary Bar -->
                    <!-- <div class="gp-tier-title">Tier 5 &mdash; Memory Footprint</div>
                    <div class="gp-mem-bar">
                      <span class="gp-mem-item"><span class="gp-mem-label">Grid Matrix:</span> <code>{{ memGrid }}</code></span>
                      <span class="gp-mem-sep">|</span>
                      <span class="gp-mem-item"><span class="gp-mem-label">Path Buffer:</span> <code>{{ memPathBuf }}</code></span>
                      <span class="gp-mem-sep">|</span>
                      <span class="gp-mem-item"><span class="gp-mem-label">allPaths:</span> <code>{{ memAllPaths }}</code></span>
                      <span class="gp-mem-sep">|</span>
                      <span class="gp-mem-item"><span class="gp-mem-label">Stack Depth:</span> <code>{{ stackDepth }}</code></span>
                    </div> -->

                  </div>
                  <!-- END ll-board-container -->
                </div>
              </div>

              <!-- Vertical Resizer -->
              <div class="ll-vresizer" ref="vizResizerRef"></div>

              <!-- Color Legend -->
              <div class="ll-legend">
                <span class="ll-leg"><span class="ll-legdot" style="background:#dbeafe;border:1.5px solid #3b82f6;"></span>[START] (0,0)</span>
                <span class="ll-leg"><span class="ll-legdot" style="background:#fef9c3;border:1.5px solid #eab308;"></span>[DEST] (n-1,m-1)</span>
                <span class="ll-leg"><span class="ll-legdot gp-legdot-curr"></span>[CURR] Active</span>
                <span class="ll-leg"><span class="ll-legdot gp-legdot-path"></span>In Path</span>
                <span class="ll-leg"><span class="ll-legdot gp-legdot-found"></span>Path Found</span>
                <span class="ll-leg"><span class="ll-legdot gp-legdot-pop"></span>Backtrack</span>
              </div>

              <!-- Recursion Call Stack Frame Area -->
              <div class="ll-table-area" :style="{ height: tableHeight + 'px' }">
                <div class="ll-table-title">Recursion Call Stack &mdash; innermost = current depth</div>
                <div class="ll-stack-line">
                  <template v-if="s.vars && s.vars.length">
                    <div v-for="(f, depth) in s.vars" :key="depth" class="ll-frame" :class="{ 'll-frame-cur': depth === s.vars.length - 1 }" :style="{ marginLeft: depth * 14 + 'px' }">
                      {{ f.title }}(<span v-for="(r, idx) in f.rows" :key="idx"><span v-if="idx > 0">, </span><span class="ll-fname">{{ r[0] }}</span>=<span :class="depth === s.vars.length - 1 ? 'll-c-orange' : 'll-c-blue'" style="font-weight:700">{{ r[1] }}</span></span>)<span v-if="depth === s.vars.length - 1" class="ll-now"> &#9668; active</span>
                    </div>
                  </template>
                  <template v-else>&mdash;</template>
                </div>
              </div>

              <!-- Vertical Resizer -->
              <div class="ll-vresizer" ref="tableResizerRef"></div>

              <!-- Step Badge Banner -->
              <div class="ll-badge-wrap">
                <div class="ll-badge" :class="{ 'll-badge-error': s.badge && (s.badge.includes('[OUT OF BOUNDS]') || s.badge.includes('- POP') || s.badge.includes('FALSE')), 'll-badge-success': s.badge && (s.badge.includes('[VALID] PATH FOUND') || s.badge.includes('complete') || s.badge.includes('TRUE')) }">
                  {{ s.badge || 'Ready to run Grid Paths backtracking algorithm.' }}
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

                <!-- Code Scroll with 1-to-1 Active Line Highlighting -->
                <div v-if="rightTab === 'code'" class="ll-code-scroll" ref="codeScrollRef">
                  <pre class="ll-pre"><span v-for="(line, idx) in codeLines" :key="idx" class="ll-codeline" :class="{ 'll-hl': line[0] && line[0] === s.code }">{{ line[1] === '' ? ' ' : line[1] }}</span></pre>
                </div>

                <!-- Pseudocode Scroll -->
                <div v-else-if="rightTab === 'pseudo'" class="ll-code-scroll">
                  <pre class="ll-pre"><span v-for="(line, idx) in PSEUDOCODE" :key="idx" class="ll-codeline">{{ line }}</span></pre>
                </div>

                <!-- Complexity Tab -->
                <div v-else class="ll-info-scroll">
                  <h3 class="ll-cx-heading">Grid Paths from Top to Bottom Corner &mdash; Complexity Analysis</h3>
                  <p class="ll-cx-intro">
                    Finds all possible paths from cell <code>(0, 0)</code> to cell <code>(n-1, m-1)</code> in an N&times;M grid.
                    Movement is restricted to two directions: <strong>Down</strong> (r+1, c) and <strong>Right</strong> (r, c+1).
                    Each path has exactly <code>n + m - 1</code> cells. The algorithm uses DFS backtracking,
                    exploring all valid paths and storing them in the output list.
                  </p>

                  <h4 class="ll-cx-sub">Step-by-step Breakdown</h4>
                  <table class="ll-complexity-table">
                    <thead><tr><th>Operation / Phase</th><th>Time</th><th>Space</th><th>Reason</th></tr></thead>
                    <tbody>
                      <tr><td>Path length per solution</td><td class="ll-cx-good">O(N + M - 1)</td><td class="ll-cx-good">O(N + M)</td><td>Each path visits exactly N+M-1 cells; recursion stack depth = N+M-1</td></tr>
                      <tr><td>Total paths (combinatorial)</td><td class="ll-cx-mid">C(N+M-2, N-1)</td><td class="ll-cx-mid">O(paths &times; path_len)</td><td>Choosing N-1 Down moves from N+M-2 total moves</td></tr>
                      <tr><td>DFS traversal (all nodes)</td><td class="ll-cx-mid">O(2<sup>N+M</sup>)</td><td class="ll-cx-good">O(N + M)</td><td>Each cell branches into at most 2 sub-calls; auxiliary stack is path depth only</td></tr>
                      <tr><td>Storing all solutions</td><td class="ll-cx-good">O(paths)</td><td class="ll-cx-mid">O(C(N+M-2,N-1) &times; (N+M-1))</td><td>Each of the C(N+M-2,N-1) paths stores N+M-1 integers</td></tr>
                    </tbody>
                  </table>

                  <h4 class="ll-cx-sub">Overall Complexity</h4>
                  <div class="ll-cx-summary-grid">
                    <div class="ll-cx-card ll-cx-card-mid">
                      <div class="ll-cx-card-label">Time</div>
                      <div class="ll-cx-card-val">O(2<sup>N+M</sup>)</div>
                      <div class="ll-cx-card-note">Exponential — all DFS branches explored</div>
                    </div>
                    <div class="ll-cx-card ll-cx-card-good">
                      <div class="ll-cx-card-label">Aux Space</div>
                      <div class="ll-cx-card-val">O(N + M)</div>
                      <div class="ll-cx-card-note">Recursion stack + current path buffer</div>
                    </div>
                    <div class="ll-cx-card ll-cx-card-mid">
                      <div class="ll-cx-card-label">Output Space</div>
                      <div class="ll-cx-card-val">C(N+M-2, N-1)</div>
                      <div class="ll-cx-card-note">Number of distinct root-to-dest paths</div>
                    </div>
                  </div>

                  <div class="ll-note">
                    <strong>Backtracking Insight:</strong> The algorithm builds the path incrementally
                    by <code>current.add(grid[r][c])</code> on entry and <code>current.remove(size-1)</code> on exit (backtrack).
                    Since only Down and Right moves are allowed, no cell is visited twice per path,
                    and the total number of paths equals the binomial coefficient <code>C(N+M-2, N-1)</code>.
                    For a 3&times;3 grid: C(4,2) = 6 paths. For a 4&times;4: C(6,3) = 20 paths.
                  </div>
                </div>
              </div>
            </div>
          </div>

          <!-- Footer Toolbar -->
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
  --coral: #F04D4D; --coral-dark: #d93e3e; --coral-light: #fff0f0;
  --bg: #f5f6fa; --surface: #ffffff; --surface2: #f1f4f9;
  --border: #e2e8f0; --border2: #cbd5e1;
  --text: #1e293b; --text2: #475569; --muted: #94a3b8;
  --blue: #3b82f6; --blue-light: #eff6ff;
  --green: #22c55e; --green-light: #f0fdf4;
  --orange: #f97316; --orange-light: #fff7ed;
  --purple: #9333ea; --purple-light: #f3e8ff;
  --red: #ef4444; --red-dark: #991b1b; --red-light: #fef2f2;
  --shadow-sm: 0 1px 3px rgba(0,0,0,.08), 0 1px 2px rgba(0,0,0,.04);
  --radius: 8px; --radius-sm: 6px;
  background: var(--bg); color: var(--text);
  font-family: 'Segoe UI', system-ui, sans-serif; font-size: 12.5px;
  display: flex; flex-direction: column; height: 74vh; overflow: hidden; width: 100%;
}

@keyframes ll-pop { 0% { transform: scale(0.6); opacity: 0; } 70% { transform: scale(1.15); opacity: 1; } 100% { transform: scale(1); opacity: 1; } }
@keyframes ll-pulse-conflict { 0%, 100% { transform: scale(1); } 50% { transform: scale(1.08); } }
@keyframes gp-flash-found { 0% { background: #fef9c3; } 50% { background: #22c55e; } 100% { background: #dcfce7; } }
@keyframes gp-pulse-pop { 0% { opacity: 1; } 50% { opacity: 0.4; background: #fee2e2; } 100% { opacity: 1; } }

.slide-wrapper { margin-top: -10px; margin-left: -30px; width: 107%; max-height: 100%; font-size: 0.8rem; font-weight: 400; }
.slide-body { display: flex; flex-direction: column; border-radius: 4px; height: 100%; }
.navbar { display: flex; flex-direction: row; justify-content: space-between; align-items: center; gap: 0.75rem; padding: 0 10px; background-color: #ffffff; position: fixed; width: 94.7%; z-index: 50; }
.navbar > img { height: 30px; }
.navbar-title { margin: 0; font-size: 1.35rem; font-weight: 700; background-color: #ef5050; color: #ffffff; width: 80%; padding: 2px 10px; margin-left: -10px; border-radius: 5px; }
.row-main { width: 100%; height: 90%; margin-top: 36px; overflow-x: auto; overflow-y: hidden; }

.ll-toolbar { margin-top: 4px; display: flex; align-items: center; gap: 6px; padding: 6.5px 12px; background: var(--surface); border-bottom: 1px solid var(--border); flex-shrink: 0; flex-wrap: wrap; box-shadow: var(--shadow-sm); }
.ll-input-group { display: flex; align-items: center; gap: 4px; }
.ll-input-group label { font-size: 11px; color: var(--muted); font-weight: 700; }
.ll-text-input, .ll-num-input { background: var(--surface); border: 1px solid var(--border2); color: var(--text); border-radius: var(--radius-sm); padding: 3px 6px; font-size: 11.5px; font-family: monospace; }
.ll-text-input:focus, .ll-num-input:focus { outline: none; border-color: var(--coral); box-shadow: 0 0 0 3px rgba(240,77,77,.1); }
.gp-grid-input { width: 160px; min-height: 24px; max-height: 52px; resize: none; overflow: auto; font-size: 10.5px; line-height: 1.3; }
.ll-preset-group { display: flex; gap: 3px; }
.ll-preset-btn { background: var(--surface2); border: 1px solid var(--border2); color: var(--text2); padding: 3px 6px; border-radius: 4px; font-size: 10.5px; cursor: pointer; transition: all .12s; }
.ll-preset-btn:hover { background: var(--coral-light); border-color: var(--coral); color: var(--coral-dark); }
.ll-viz-btn { background: var(--coral); color: #fff; border: none; padding: 5px 12px; border-radius: var(--radius-sm); cursor: pointer; font-size: 11.5px; font-weight: 600; box-shadow: var(--shadow-sm); transition: filter .15s; }
.ll-viz-btn:hover { filter: brightness(1.08); }
.ll-nav-controls { display: flex; margin-left: auto; align-items: center; gap: 4px; flex-shrink: 0; flex-wrap: wrap; }
.ll-nav-btn { background: var(--surface2); border: 1px solid var(--border2); color: var(--text2); padding: 4px 9px; border-radius: var(--radius-sm); cursor: pointer; font-size: 11px; font-weight: 500; transition: all .15s; white-space: nowrap; }
.ll-nav-btn:hover { background: var(--surface); border-color: var(--coral); color: var(--coral); }
.ll-play-btn { background: var(--blue-light); border: 1px solid var(--blue); color: var(--blue); min-width: 68px; font-weight: 600; padding: 4px 9px; border-radius: var(--radius-sm); cursor: pointer; font-size: 11px; transition: all .15s; }
.ll-play-btn:hover { background: var(--blue); color: #fff; }

.ll-main { display: flex; flex: 1; overflow: hidden; position: relative; }
.ll-left-col { display: flex; flex-direction: column; overflow: hidden; min-width: 220px; max-width: 75%; }
.ll-resizer { width: 5px; cursor: col-resize; background: var(--border); flex-shrink: 0; transition: background .15s; position: relative; z-index: 20; }
.ll-resizer:hover, .ll-resizer.drag { background: var(--coral); }
.ll-right-col { display: flex; flex-direction: column; flex: 1; overflow: hidden; min-width: 0; height: 100%; }

.ll-viz-wrap { flex-shrink: 0; background: var(--surface); border-bottom: 1px solid var(--border); position: relative; overflow-x: auto; overflow-y: auto; }
.ll-perm-area { display: flex; flex-direction: column; align-items: stretch; min-height: 100%; width: 100%; min-width: 0; box-sizing: border-box; }
.ll-ptrs { display: flex; gap: 8px; flex-wrap: wrap; padding: 4px 14px; min-height: 28px; width: 100%; box-sizing: border-box; min-width: 0; align-items: center; }
.ll-ptrs-compact { flex-wrap: nowrap; gap: 6px; padding: 3px 14px 5px; min-height: 0; overflow-x: auto; overflow-y: hidden; scrollbar-width: thin; scrollbar-color: var(--border) transparent; }
.ll-ptrs-compact::-webkit-scrollbar { height: 3px; }
.ll-ptrs-compact::-webkit-scrollbar-track { background: transparent; }
.ll-ptrs-compact::-webkit-scrollbar-thumb { background: var(--border); border-radius: 3px; }
.ll-ptrs-compact::-webkit-scrollbar-thumb:hover { background: var(--muted); }
.ll-ptr-chip-inline { display: inline-flex; align-items: center; gap: 4px; background: var(--surface2); border: 1px solid var(--border); border-radius: 6px; padding: 2px 8px; font-size: 11px; font-family: monospace; white-space: nowrap; flex-shrink: 0; line-height: 1.4; }
.ll-chip-label { color: var(--text-muted, #8899aa); font-weight: 500; margin-right: 2px; }
.ll-c-blue { color: var(--blue); } .ll-c-orange { color: var(--orange); } .ll-c-green { color: var(--green); } .ll-c-purple { color: var(--purple); } .ll-c-red { color: var(--red); }

/* Board Container */
.ll-board-container { display: flex; flex-direction: column; align-items: flex-start; padding: 6px 14px 10px; gap: 5px; min-width: 0; }

.gp-tier-title { font-size: 10px; font-weight: 700; text-transform: uppercase; letter-spacing: 0.04em; color: var(--muted); margin-top: 4px; margin-bottom: 2px; font-family: 'Consolas', monospace; border-left: 3px solid var(--coral); padding-left: 6px; }

.ll-board-frame { display: flex; flex-direction: column; background: #f8fafc; border: 1px solid var(--border); border-radius: var(--radius); padding: 8px 12px; box-shadow: var(--shadow-sm); }
.ll-col-indices { display: flex; margin-bottom: 2px; }
.ll-axis-spacer { width: 28px; height: 20px; }
.ll-axis-label { width: 44px; height: 20px; display: flex; align-items: center; justify-content: center; font-size: 11px; font-weight: 700; color: var(--muted); font-family: monospace; margin: 0 1px; }
.ll-row-label { width: 28px; height: 44px; font-size: 10px; font-weight: 700; text-transform: uppercase; color: var(--muted); font-family: monospace; }
.ll-board-rows { display: flex; flex-direction: column; gap: 1px; }
.ll-board-row { display: flex; align-items: center; gap: 1px; }

.ll-chess-cell { position: relative; display: flex; flex-direction: column; align-items: center; justify-content: center; border-radius: 4px; user-select: none; transition: all .18s ease; }
.gp-grid-cell { width: 44px; height: 44px; background: #f8fafc; border: 1.5px solid var(--border2); }

.gp-cell-start { background: #dbeafe !important; border: 2px solid var(--blue) !important; box-shadow: 0 0 6px rgba(59,130,246,.25); }
.gp-cell-dest { background: #fef9c3 !important; border: 2px solid #eab308 !important; box-shadow: 0 0 6px rgba(234,179,8,.3); }
.gp-cell-curr { background: #fffbeb !important; border: 2px solid var(--orange) !important; transform: scale(1.08); z-index: 10; box-shadow: 0 0 10px rgba(249,115,22,.4); }
.gp-cell-pop { background: #fef2f2 !important; border: 2px solid var(--red) !important; animation: gp-pulse-pop 0.4s ease; }
.gp-cell-found { background: #dcfce7 !important; border: 2px solid #10b981 !important; animation: gp-flash-found 0.5s ease; z-index: 10; }
.gp-cell-active { background: #f3e8ff !important; border: 2px solid var(--purple) !important; }
.gp-cell-path { background: #eff6ff !important; border: 1.5px solid #93c5fd !important; }
.gp-cell-alloc { background: #f1f5f9 !important; border: 1.5px dashed #94a3b8 !important; }
.gp-cell-reading { background: #fef3c7 !important; border: 2px solid #f59e0b !important; transform: scale(1.06); z-index: 10; }
.gp-cell-updated { background: #dcfce7 !important; border: 2px solid #10b981 !important; animation: ll-pop 0.2s ease; }

.gp-cell-val { font-size: 15px; font-weight: 800; font-family: monospace; color: var(--text); line-height: 1; }
.gp-badge-marker { position: absolute; font-size: 7px; font-weight: 800; font-family: monospace; line-height: 1; padding: 1px 2px; border-radius: 2px; white-space: nowrap; bottom: 2px; left: 50%; transform: translateX(-50%); }
.gp-badge-start { background: #dbeafe; color: #1d4ed8; border: 1px solid #93c5fd; }
.gp-badge-dest { background: #fef9c3; color: #854d0e; border: 1px solid #fcd34d; }
.gp-badge-curr { background: #fff7ed; color: #c2410c; border: 1px solid #fdba74; }
.gp-badge-reading { background: #fef3c7; color: #b45309; border: 1px solid #fcd34d; }
.gp-badge-updated { background: #dcfce7; color: #15803d; border: 1px solid #86efac; }
.gp-badge-default { background: #f1f5f9; color: #94a3b8; border: 1px solid #cbd5e1; }

/* Tier 2: Direction Panel */
.gp-dir-panel { display: flex; gap: 8px; flex-wrap: wrap; }
.gp-dir-card { display: flex; flex-direction: column; align-items: center; gap: 2px; padding: 6px 14px; border-radius: var(--radius-sm); border: 1.5px solid var(--border); background: var(--surface2); min-width: 100px; font-size: 10.5px; font-family: monospace; transition: all .15s; }
.gp-dir-active { border-color: var(--orange) !important; background: var(--orange-light) !important; }
.gp-dir-valid { border-color: var(--green) !important; background: var(--green-light) !important; }
.gp-dir-oob { border-color: var(--red) !important; background: var(--red-light) !important; }
.gp-dir-arrow { font-size: 18px; font-weight: 900; color: var(--text2); }
.gp-dir-label { font-weight: 800; color: var(--text); font-size: 11px; }
.gp-dir-expr code, .gp-dir-check code { font-size: 9.5px; background: var(--surface); padding: 1px 4px; border-radius: 3px; border: 1px solid var(--border); color: var(--coral-dark); }
.gp-dir-result { margin-top: 2px; }
.gp-valid-badge { font-size: 9.5px; font-weight: 800; font-family: monospace; background: #dcfce7; color: #15803d; border: 1px solid #86efac; padding: 1px 5px; border-radius: 3px; }
.gp-oob-badge { font-size: 9.5px; font-weight: 800; font-family: monospace; background: #fee2e2; color: #b91c1c; border: 1px solid #fca5a5; padding: 1px 5px; border-radius: 3px; }
.gp-neutral-badge { font-size: 9.5px; color: var(--muted); font-family: monospace; }

/* Tier 3: Path Buffer */
.gp-buf-panel { display: flex; flex-direction: column; gap: 4px; background: var(--surface2); border: 1px solid var(--border); border-radius: var(--radius-sm); padding: 6px 10px; min-width: 200px; }
.gp-buf-meta { font-size: 10.5px; font-family: monospace; color: var(--text2); }
.gp-buf-slots { display: flex; gap: 3px; flex-wrap: wrap; align-items: center; }
.gp-buf-slot { display: flex; flex-direction: column; align-items: center; justify-content: center; width: 36px; min-height: 42px; border-radius: 4px; border: 1.5px solid var(--border); background: var(--surface); font-family: monospace; position: relative; transition: all .15s; }
.gp-buf-filled { background: #eff6ff; border-color: #93c5fd; }
.gp-buf-empty { background: #f8fafc; border-color: var(--border); opacity: 0.55; }
.gp-buf-just-pushed { background: #dcfce7 !important; border-color: #10b981 !important; animation: ll-pop 0.25s ease; }
.gp-buf-just-popped { background: #fee2e2 !important; border-color: var(--red) !important; animation: gp-pulse-pop 0.3s ease; }
.gp-buf-idx { font-size: 8px; color: var(--muted); font-weight: 700; margin-bottom: 2px; }
.gp-buf-val { font-size: 14px; font-weight: 800; color: var(--text); }
.gp-push-badge { position: absolute; top: -8px; left: 50%; transform: translateX(-50%); font-size: 8px; font-weight: 800; font-family: monospace; background: #dcfce7; color: #15803d; border: 1px solid #86efac; padding: 1px 4px; border-radius: 3px; white-space: nowrap; }
.gp-pop-badge { position: absolute; top: -8px; left: 50%; transform: translateX(-50%); font-size: 8px; font-weight: 800; font-family: monospace; background: #fee2e2; color: #b91c1c; border: 1px solid #fca5a5; padding: 1px 4px; border-radius: 3px; white-space: nowrap; }
.gp-buf-empty-msg { color: var(--muted); font-style: italic; font-size: 11px; }

/* Tier 4: Solutions Panel */
.gp-solutions-panel { display: flex; flex-wrap: wrap; gap: 4px; max-width: 460px; background: #f8fafc; padding: 6px 10px; border-radius: var(--radius-sm); border: 1px solid var(--border); }
.gp-no-solutions { color: var(--muted); font-style: italic; font-size: 11px; }
.gp-sol-btn { padding: 3px 8px; border-radius: 4px; border: 1px solid var(--border2); background: var(--surface); color: var(--text2); font-size: 10.5px; font-family: monospace; cursor: pointer; transition: all .15s ease; }
.gp-sol-btn:hover { border-color: #10b981; color: #059669; }
.gp-sol-btn.active { background: #dcfce7; border-color: #10b981; color: #15803d; font-weight: 700; }

/* Tier 5: Memory Footprint Bar */
.gp-mem-bar { display: flex; flex-wrap: wrap; gap: 4px 10px; background: #1e293b; color: #94a3b8; font-family: monospace; font-size: 10px; padding: 5px 10px; border-radius: var(--radius-sm); align-items: center; }
.gp-mem-item { display: flex; align-items: center; gap: 4px; }
.gp-mem-label { color: #64748b; font-weight: 700; }
.gp-mem-bar code { color: #38bdf8; font-size: 10px; }
.gp-mem-sep { color: #334155; font-size: 12px; }

.ll-vresizer { height: 5px; cursor: row-resize; background: var(--border); flex-shrink: 0; transition: background .15s; position: relative; z-index: 20; }
.ll-vresizer:hover, .ll-vresizer.drag { background: var(--coral); }
.ll-legend { display: flex; flex-wrap: wrap; gap: 6px 14px; padding: 6px 12px; border-bottom: 1px solid var(--border); flex-shrink: 0; background: var(--surface2); }
.ll-leg { display: flex; align-items: center; gap: 5px; font-size: 11px; color: var(--text2); font-weight: 500; }
.ll-legdot { width: 11px; height: 11px; border-radius: 3px; flex-shrink: 0; display: inline-block; }
.gp-legdot-curr { background: #fffbeb; border: 1.5px solid var(--orange); }
.gp-legdot-path { background: #eff6ff; border: 1.5px solid #93c5fd; }
.gp-legdot-found { background: #dcfce7; border: 1.5px solid #10b981; }
.gp-legdot-pop { background: #fef2f2; border: 1.5px solid var(--red); }

.ll-table-area { flex-shrink: 0; padding: 8px 14px; border-bottom: 1px solid var(--border); overflow-x: hidden; overflow-y: auto; background: var(--surface); min-width: 0; box-sizing: border-box; }
.ll-table-title { font-size: 10px; color: var(--muted); margin-bottom: 4px; font-style: italic; }
.ll-stack-line { font-family: 'Consolas', monospace; font-size: 12px; line-height: 1.8; }
.ll-frame { font-family: 'Consolas', monospace; font-size: 11.5px; color: var(--text2); padding: 1px 0; white-space: nowrap; }
.ll-frame-cur { color: var(--orange); background: var(--orange-light); border-radius: 4px; padding: 1px 5px; }
.ll-fname { color: var(--text2); }
.ll-now { color: var(--orange); font-size: 10px; margin-left: 6px; }

.ll-badge-wrap { padding: 6px 10px; border-bottom: 1px solid var(--border); flex-shrink: 0; min-height: 36px; display: flex; align-items: center; background: var(--surface); }
.ll-badge { display: inline-block; padding: 4px 12px; border-radius: var(--radius-sm); border-left: 3px solid var(--coral); background: var(--coral-light); font-size: 11px; color: var(--coral-dark); line-height: 1.4; word-break: break-word; font-weight: 500; }
.ll-badge-error { border-left-color: var(--red) !important; background: var(--red-light) !important; color: var(--red-dark) !important; }
.ll-badge-success { border-left-color: var(--green) !important; background: var(--green-light) !important; color: #15803d !important; }

.ll-code-panel { display: flex; flex-direction: column; height: 100%; overflow: hidden; }
.ll-code-header { display: flex; align-items: center; gap: 6px; padding: 5px 12px; background: var(--surface); border-bottom: 1px solid var(--border); box-shadow: var(--shadow-sm); flex-shrink: 0; flex-wrap: wrap; }
.ll-tabbar { display: flex; gap: 3px; flex-wrap: wrap; }
.ll-tab-btn { padding: 4px 9px; font-size: 10.5px; font-weight: 600; border: 1px solid var(--border2); background: var(--surface2); color: var(--text2); border-radius: var(--radius-sm); cursor: pointer; transition: all .15s ease; white-space: nowrap; }
.ll-tab-btn:hover { border-color: var(--coral); color: var(--coral); }
.ll-tab-btn.active { background: var(--coral); border-color: var(--coral); color: #fff; }
.ll-lang-select { margin-left: auto; padding: 4px 24px 4px 8px; font-size: 11px; font-weight: 500; border: 1px solid var(--border2); border-radius: var(--radius-sm); background: var(--surface2); color: var(--text); cursor: pointer; appearance: none; background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='10' height='6' viewBox='0 0 10 6'%3E%3Cpath d='M0 0l5 6 5-6z' fill='%2394a3b8'/%3E%3C/svg%3E"); background-repeat: no-repeat; background-position: right 8px center; min-width: 95px; transition: border-color .15s; }
.ll-lang-select:focus { outline: none; border-color: var(--coral); box-shadow: 0 0 0 3px rgba(240,77,77,.1); }
.ll-code-scroll { flex: 1; overflow: auto; background: #f8fafc; padding: 10px 14px; min-width: 0; }
.ll-pre { margin: 0; font-family: 'Cascadia Code', 'Fira Code', 'Consolas', monospace; font-size: 11px; line-height: 1.5; color: var(--text); white-space: pre; padding-bottom: 150px; }
.ll-codeline { display: block; padding: 0 14px; margin: 0 -14px; }
.ll-hl { background: #dcfce7; color: #15803d; font-weight: 600; border-left: 3px solid var(--green); border-radius: 3px; }

.ll-info-scroll { flex: 1; overflow: auto; padding: 12px 16px; background: var(--surface); color: var(--text2); font-size: 12px; line-height: 1.55; }
.ll-info-scroll h3 { font-size: 13px; font-weight: 700; color: var(--text); margin: 0 0 6px; }
.ll-info-scroll h4 { font-size: 12px; font-weight: 700; color: var(--text); margin: 10px 0 4px; }
.ll-info-scroll p { margin: 0 0 6px; }
.ll-info-scroll code { background: var(--surface2); padding: 1px 4px; border-radius: 3px; font-family: 'Cascadia Code', monospace; font-size: 11px; color: var(--coral-dark); }
.ll-cx-heading { font-size: 13px; font-weight: 700; color: var(--text); margin: 0 0 6px; }
.ll-cx-intro { font-size: 10.5px; color: var(--text2); margin: 0 0 10px; line-height: 1.55; }
.ll-cx-sub { font-size: 11px; font-weight: 700; color: var(--text2); margin: 10px 0 4px; border-bottom: 1px solid var(--border); padding-bottom: 3px; }
.ll-complexity-table { width: 100%; border-collapse: collapse; font-size: 10.5px; margin: 8px 0; }
.ll-complexity-table th, .ll-complexity-table td { border: 1px solid var(--border); padding: 4px 8px; text-align: left; }
.ll-complexity-table th { background: var(--surface2); font-weight: 700; color: var(--text2); }
.ll-cx-good { color: #15803d; font-weight: 700; } .ll-cx-mid { color: #b45309; font-weight: 700; } .ll-cx-bad { color: #b91c1c; font-weight: 700; }
.ll-cx-summary-grid { display: flex; gap: 8px; flex-wrap: wrap; margin: 6px 0 10px; }
.ll-cx-card { flex: 1; min-width: 90px; border-radius: var(--radius-sm); padding: 8px 10px; text-align: center; border: 1.5px solid var(--border); }
.ll-cx-card-good { background: #f0fdf4; border-color: #86efac; color: #15803d; }
.ll-cx-card-mid { background: #fff7ed; border-color: #fed7aa; color: #c2410c; }
.ll-cx-card-bad { background: #fef2f2; border-color: #fca5a5; color: #b91c1c; }
.ll-cx-card-label { font-size: 9px; font-weight: 700; text-transform: uppercase; letter-spacing: .05em; opacity: .7; margin-bottom: 4px; }
.ll-cx-card-val { font-size: 13px; font-weight: 800; font-family: monospace; margin-bottom: 3px; }
.ll-cx-card-note { font-size: 8.5px; opacity: .75; line-height: 1.3; }
.ll-note { background: #fefce8; border: 1px solid #fef08a; border-left: 3px solid #eab308; padding: 6px 10px; font-size: 10.5px; color: #854d0e; border-radius: 0 4px 4px 0; margin-top: 10px; margin-bottom: 120px; }

.ll-footer { display: flex; align-items: center; justify-content: space-between; padding: 4px 12px; background: var(--surface); border-top: 1px solid var(--border); font-size: 11px; color: var(--muted); font-weight: 600; flex-shrink: 0; }
.ll-speed-wrap { display: flex; align-items: center; gap: 6px; }
.ll-speed-wrap input[type="range"] { width: 80px; accent-color: var(--coral); }
</style>
