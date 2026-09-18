<script setup>
import { ref, reactive, computed, onMounted, onBeforeUnmount, watch, nextTick } from 'vue';

defineProps({
  topic: { type: String, default: 'Backtracking Algorithm - ' },
  subTopic: { type: String, default: 'N-Queens' }
});

const CODES = {
  java: [
    ['',                  'import java.util.Scanner;'],
    ['',                  'import java.util.List;'],
    ['',                  'import java.util.ArrayList;'],
    ['',                  ''],
    ['',                  'public class Main {'],
    ['c_entry',           '    static List<List<String>> solveNQueens(int n) {'],
    ['c_board_init',      '        char[][] board = new char[n][n];'],
    ['c_vars',            '        List<List<String>> solutions = new ArrayList<>();'],
    ['c_call_solve',      '        solve(0, board, solutions, n);'],
    ['c_return',          '        return solutions;'],
    ['',                  '    }'],
    ['',                  ''],
    ['c_solve_entry',     '    static void solve(int row, char[][] board, List<List<String>> solutions, int n) {'],
    ['c_base_check',      '        if (row == n) {'],
    ['c_save_sol',        '            solutions.add(construct(board, n));'],
    ['c_base_ret',        '            return;'],
    ['',                  '        }'],
    ['c_loop_col',        '        for (int col = 0; col < n; col++) {'],
    ['c_check_safe',      '            if (isSafe(row, col, board, n)) {'],
    ['c_place_queen',     '                board[row][col] = \'Q\';'],
    ['c_recurse',         '                solve(row + 1, board, solutions, n);'],
    ['c_backtrack',       '                board[row][col] = \'.\';'],
    ['',                  '            }'],
    ['',                  '        }'],
    ['',                  '    }'],
    ['',                  ''],
    ['c_safe_fn',         '    static boolean isSafe(int row, int col, char[][] board, int n) {'],
    ['c_safe_col_loop',   '        for (int i = 0; i < row; i++) {'],
    ['c_safe_col_check',  '            if (board[i][col] == \'Q\') {'],
    ['c_safe_col_ret',    '                return false;'],
    ['',                  '            }'],
    ['',                  '        }'],
    ['c_safe_d1_loop',    '        for (int i = row - 1, j = col - 1; i >= 0 && j >= 0; i--, j--) {'],
    ['c_safe_d1_check',   '            if (board[i][j] == \'Q\') {'],
    ['c_safe_d1_ret',     '                return false;'],
    ['',                  '            }'],
    ['',                  '        }'],
    ['c_safe_d2_loop',    '        for (int i = row - 1, j = col + 1; i >= 0 && j < n; i--, j++) {'],
    ['c_safe_d2_check',   '            if (board[i][j] == \'Q\') {'],
    ['c_safe_d2_ret',     '                return false;'],
    ['',                  '            }'],
    ['',                  '        }'],
    ['c_safe_ret_true',   '        return true;'],
    ['',                  '    }'],
    ['',                  ''],
    ['',                  '    static List<String> construct(char[][] board, int n) {'],
    ['',                  '        List<String> res = new ArrayList<>();'],
    ['',                  '        for (int i = 0; i < n; i++) {'],
    ['',                  '            StringBuilder sb = new StringBuilder();'],
    ['',                  '            for (int j = 0; j < n; j++) {'],
    ['',                  '                if (board[i][j] == \'Q\') {'],
    ['',                  '                    sb.append(\'Q\');'],
    ['',                  '                } else {'],
    ['',                  '                    sb.append(\'.\');'],
    ['',                  '                }'],
    ['',                  '            }'],
    ['',                  '            res.add(sb.toString());'],
    ['',                  '        }'],
    ['',                  '        return res;'],
    ['',                  '    }'],
    ['',                  ''],
    ['',                  '    public static void main(String[] args) {'],
    ['m_scanner',         '        Scanner sc = new Scanner(System.in);'],
    ['m_read_n',          '        int n = sc.nextInt();'],
    ['m_call_solver',     '        List<List<String>> solutions = solveNQueens(n);'],
    ['m_print_ans',       '        System.out.println(solutions.size());'],
    ['m_done',            '    }'],
    ['',                  '}']
  ],
  c: [
    ['',                  '#include <stdio.h>'],
    ['',                  ''],
    ['',                  'void solve(int row, char board[10][10], int n, int* count);'],
    ['',                  'int isSafe(int row, int col, char board[10][10], int n);'],
    ['',                  ''],
    ['c_entry',           'int solveNQueens(int n) {'],
    ['c_board_init',      '    char board[10][10] = {0};'],
    ['c_vars',            '    int solutionCount = 0;'],
    ['c_call_solve',      '    solve(0, board, n, &solutionCount);'],
    ['c_return',          '    return solutionCount;'],
    ['',                  '}'],
    ['',                  ''],
    ['c_solve_entry',     'void solve(int row, char board[10][10], int n, int* count) {'],
    ['c_base_check',      '    if (row == n) {'],
    ['c_save_sol',        '        (*count)++;'],
    ['c_base_ret',        '        return;'],
    ['',                  '    }'],
    ['c_loop_col',        '    for (int col = 0; col < n; col++) {'],
    ['c_check_safe',      '        if (isSafe(row, col, board, n)) {'],
    ['c_place_queen',     '            board[row][col] = \'Q\';'],
    ['c_recurse',         '            solve(row + 1, board, n, count);'],
    ['c_backtrack',       '            board[row][col] = \'.\';'],
    ['',                  '        }'],
    ['',                  '    }'],
    ['',                  '}'],
    ['',                  ''],
    ['c_safe_fn',         'int isSafe(int row, int col, char board[10][10], int n) {'],
    ['c_safe_col_loop',   '    for (int i = 0; i < row; i++) {'],
    ['c_safe_col_check',  '        if (board[i][col] == \'Q\') {'],
    ['c_safe_col_ret',    '            return 0;'],
    ['',                  '        }'],
    ['',                  '    }'],
    ['c_safe_d1_loop',    '    for (int i = row - 1, j = col - 1; i >= 0 && j >= 0; i--, j--) {'],
    ['c_safe_d1_check',   '        if (board[i][j] == \'Q\') {'],
    ['c_safe_d1_ret',     '            return 0;'],
    ['',                  '        }'],
    ['',                  '    }'],
    ['c_safe_d2_loop',    '    for (int i = row - 1, j = col + 1; i >= 0 && j < n; i--, j++) {'],
    ['c_safe_d2_check',   '        if (board[i][j] == \'Q\') {'],
    ['c_safe_d2_ret',     '            return 0;'],
    ['',                  '        }'],
    ['',                  '    }'],
    ['c_safe_ret_true',   '    return 1;'],
    ['',                  '}'],
    ['',                  ''],
    ['',                  'int main() {'],
    ['m_scanner',         '    int n;'],
    ['m_read_n',          '    scanf("%d", &n);'],
    ['m_call_solver',     '    int solutionCount = solveNQueens(n);'],
    ['m_print_ans',       '    printf("%d\\n", solutionCount);'],
    ['m_done',            '    return 0;'],
    ['',                  '}']
  ],
  cpp: [
    ['',                  '#include <iostream>'],
    ['',                  '#include <vector>'],
    ['',                  '#include <string>'],
    ['',                  'using namespace std;'],
    ['',                  ''],
    ['',                  'bool isSafe(int row, int col, vector<string>& board, int n);'],
    ['',                  'void solve(int row, vector<string>& board, vector<vector<string>>& solutions, int n);'],
    ['',                  ''],
    ['c_entry',           'vector<vector<string>> solveNQueens(int n) {'],
    ['c_board_init',      '    vector<string> board(n, string(n, \'.\'));'],
    ['c_vars',            '    vector<vector<string>> solutions;'],
    ['c_call_solve',      '    solve(0, board, solutions, n);'],
    ['c_return',          '    return solutions;'],
    ['',                  '}'],
    ['',                  ''],
    ['c_solve_entry',     'void solve(int row, vector<string>& board, vector<vector<string>>& solutions, int n) {'],
    ['c_base_check',      '    if (row == n) {'],
    ['c_save_sol',        '        solutions.push_back(board);'],
    ['c_base_ret',        '        return;'],
    ['',                  '    }'],
    ['c_loop_col',        '    for (int col = 0; col < n; col++) {'],
    ['c_check_safe',      '        if (isSafe(row, col, board, n)) {'],
    ['c_place_queen',     '            board[row][col] = \'Q\';'],
    ['c_recurse',         '            solve(row + 1, board, solutions, n);'],
    ['c_backtrack',       '            board[row][col] = \'.\';'],
    ['',                  '        }'],
    ['',                  '    }'],
    ['',                  '}'],
    ['',                  ''],
    ['c_safe_fn',         'bool isSafe(int row, int col, vector<string>& board, int n) {'],
    ['c_safe_col_loop',   '    for (int i = 0; i < row; i++) {'],
    ['c_safe_col_check',  '        if (board[i][col] == \'Q\') {'],
    ['c_safe_col_ret',    '            return false;'],
    ['',                  '        }'],
    ['',                  '    }'],
    ['c_safe_d1_loop',    '    for (int i = row - 1, j = col - 1; i >= 0 && j >= 0; i--, j--) {'],
    ['c_safe_d1_check',   '        if (board[i][j] == \'Q\') {'],
    ['c_safe_d1_ret',     '            return false;'],
    ['',                  '        }'],
    ['',                  '    }'],
    ['c_safe_d2_loop',    '    for (int i = row - 1, j = col + 1; i >= 0 && j < n; i--, j++) {'],
    ['c_safe_d2_check',   '        if (board[i][j] == \'Q\') {'],
    ['c_safe_d2_ret',     '            return false;'],
    ['',                  '        }'],
    ['',                  '    }'],
    ['c_safe_ret_true',   '    return true;'],
    ['',                  '}'],
    ['',                  ''],
    ['',                  'int main() {'],
    ['m_scanner',         '    int n;'],
    ['m_read_n',          '    cin >> n;'],
    ['m_call_solver',     '    vector<vector<string>> solutions = solveNQueens(n);'],
    ['m_print_ans',       '    cout << solutions.size() << endl;'],
    ['m_done',            '    return 0;'],
    ['',                  '}']
  ],
  python: [
    ['',                  'import sys'],
    ['',                  ''],
    ['c_entry',           'def solveNQueens(n: int) -> list[list[str]]:'],
    ['c_board_init',      '    board = [["."] * n for _ in range(n)]'],
    ['c_vars',            '    solutions = []'],
    ['c_call_solve',      '    solve(0, board, solutions, n)'],
    ['c_return',          '    return solutions'],
    ['',                  ''],
    ['c_solve_entry',     'def solve(row, board, solutions, n):'],
    ['c_base_check',      '    if row == n:'],
    ['c_save_sol',        '        solutions.append(["".join(r) for r in board])'],
    ['c_base_ret',        '        return'],
    ['c_loop_col',        '    for col in range(n):'],
    ['c_check_safe',      '        if is_safe(row, col, board, n):'],
    ['c_place_queen',     '            board[row][col] = "Q"'],
    ['c_recurse',         '            solve(row + 1, board, solutions, n)'],
    ['c_backtrack',       '            board[row][col] = "."'],
    ['',                  ''],
    ['c_safe_fn',         'def is_safe(row, col, board, n):'],
    ['c_safe_col_loop',   '    for i in range(row):'],
    ['c_safe_col_check',  '        if board[i][col] == "Q":'],
    ['c_safe_col_ret',    '            return False'],
    ['c_safe_d1_loop',    '    for i, j in zip(range(row - 1, -1, -1), range(col - 1, -1, -1)):'],
    ['c_safe_d1_check',   '        if board[i][j] == "Q":'],
    ['c_safe_d1_ret',     '            return False'],
    ['c_safe_d2_loop',    '    for i, j in zip(range(row - 1, -1, -1), range(col + 1, n)):'],
    ['c_safe_d2_check',   '        if board[i][j] == "Q":'],
    ['c_safe_d2_ret',     '            return False'],
    ['c_safe_ret_true',   '    return True'],
    ['',                  ''],
    ['',                  'def main():'],
    ['m_scanner',         '    tokens = sys.stdin.read().split()'],
    ['',                  '    if not tokens:'],
    ['',                  '        return'],
    ['m_read_n',          '    n = int(tokens[0])'],
    ['m_call_solver',     '    solutions = solveNQueens(n)'],
    ['m_print_ans',       '    print(len(solutions))'],
    ['m_done',            '    return'],
    ['',                  ''],
    ['',                  'if __name__ == "__main__":'],
    ['',                  '    main()']
  ],
  javascript: [
    ['c_entry',           'function solveNQueens(n) {'],
    ['c_board_init',      '  const board = Array.from({ length: n }, () => new Array(n).fill(\'.\'));'],
    ['c_vars',            '  const solutions = [];'],
    ['c_call_solve',      '  solve(0, board, solutions, n);'],
    ['c_return',          '  return solutions;'],
    ['',                  '}'],
    ['',                  ''],
    ['c_solve_entry',     'function solve(row, board, solutions, n) {'],
    ['c_base_check',      '  if (row === n) {'],
    ['c_save_sol',        '    solutions.push(board.map(r => r.join(\'\')));'],
    ['c_base_ret',        '    return;'],
    ['',                  '  }'],
    ['c_loop_col',        '  for (let col = 0; col < n; col++) {'],
    ['c_check_safe',      '    if (isSafe(row, col, board, n)) {'],
    ['c_place_queen',     '      board[row][col] = \'Q\';'],
    ['c_recurse',         '      solve(row + 1, board, solutions, n);'],
    ['c_backtrack',       '      board[row][col] = \'.\';'],
    ['',                  '    }'],
    ['',                  '  }'],
    ['',                  '}'],
    ['',                  ''],
    ['c_safe_fn',         'function isSafe(row, col, board, n) {'],
    ['c_safe_col_loop',   '  for (let i = 0; i < row; i++) {'],
    ['c_safe_col_check',  '    if (board[i][col] === \'Q\') {'],
    ['c_safe_col_ret',    '      return false;'],
    ['',                  '    }'],
    ['',                  '  }'],
    ['c_safe_d1_loop',    '  for (let i = row - 1, j = col - 1; i >= 0 && j >= 0; i--, j--) {'],
    ['c_safe_d1_check',   '    if (board[i][j] === \'Q\') {'],
    ['c_safe_d1_ret',     '      return false;'],
    ['',                  '    }'],
    ['',                  '  }'],
    ['c_safe_d2_loop',    '  for (let i = row - 1, j = col + 1; i >= 0 && j < n; i--, j++) {'],
    ['c_safe_d2_check',   '    if (board[i][j] === \'Q\') {'],
    ['c_safe_d2_ret',     '      return false;'],
    ['',                  '    }'],
    ['',                  '  }'],
    ['c_safe_ret_true',   '  return true;'],
    ['',                  '}'],
    ['',                  ''],
    ['',                  'function main() {'],
    ['m_scanner',         '  const n = 4;'],
    ['m_read_n',          '  // n = 4'],
    ['m_call_solver',     '  const solutions = solveNQueens(n);'],
    ['m_print_ans',       '  console.log(solutions.length);'],
    ['m_done',            '}'],
    ['',                  ''],
    ['',                  'main();']
  ]
};

const PSEUDOCODE = [
  'function SolveNQueens(n):',
  '    board = n x n chessboard initialized with \'.\'',
  '    solutions = []',
  '    solve(row = 0, board, solutions, n)',
  '    return solutions',
  '',
  'function solve(row, board, solutions, n):',
  '    if row == n:                      // Base case: All N queens placed safely',
  '        solutions.append(copy(board))',
  '        return',
  '    for col = 0 to n - 1:',
  '        if isSafe(row, col, board, n): // Prune branch if under attack',
  '            board[row][col] = \'Q\'       // Place Queen',
  '            solve(row + 1, board)     // Recurse to next row',
  '            board[row][col] = \'.\'       // Backtrack: Remove Queen',
  '',
  'function isSafe(row, col, board, n):',
  '    for i = 0 to row - 1:            // Vertical column check',
  '        if board[i][col] == \'Q\':',
  '            return false',
  '    for i, j along upper-left diagonal:',
  '        if board[i][j] == \'Q\':',
  '            return false',
  '    for i, j along upper-right diagonal:',
  '        if board[i][j] == \'Q\':',
  '            return false',
  '    return true'
];

function frame(title, rows) { return { title, rows }; }

function buildSteps(nParam) {
  const steps = [];
  const n = Math.max(1, Math.min(8, parseInt(nParam, 10) || 4));
  const queens = new Array(n).fill(-1);
  const solutions = [];

  function getBoardCells(curRow = -1, curCol = -1, stateType = 'normal', conflictRow = -1, conflictCol = -1, rayCells = []) {
    const raySet = new Set(rayCells.map(p => `${p.r},${p.c}`));
    const grid = [];
    for (let r = 0; r < n; r++) {
      const rowCells = [];
      for (let c = 0; c < n; c++) {
        const isLight = (r + c) % 2 === 0;
        let cellState = 'normal';
        const hasQueen = queens[r] === c;

        if (r === curRow && c === curCol) {
          cellState = stateType;
        } else if (hasQueen) {
          if (r === conflictRow && c === conflictCol) {
            cellState = 'conflict_source';
          } else {
            cellState = 'queen';
          }
        } else if (raySet.has(`${r},${c}`)) {
          cellState = 'ray';
        }
        rowCells.push({ r, c, hasQueen, isLight, state: cellState });
      }
      grid.push(rowCells);
    }
    return grid;
  }

  function getStackFrames(row = 0, col = -1, inSafe = false, safeI = -1, safeJ = -1) {
    const frames = [
      frame('main()', [['n', String(n)]]),
      frame('solveNQueens()', [['n', String(n)]])
    ];
    for (let r = 0; r <= row && r < n; r++) {
      const rowsData = [['row', String(r)]];
      if (r === row && col >= 0) {
        rowsData.push(['col', String(col)]);
      }
      if (queens[r] >= 0) {
        rowsData.push(['queen_col', String(queens[r])]);
      }
      frames.push(frame(`solve(row=${r})`, rowsData));
    }
    if (inSafe) {
      const safeData = [['row', String(row)], ['col', String(col)]];
      if (safeI >= 0) safeData.push(['i', String(safeI)]);
      if (safeJ >= 0) safeData.push(['j', String(safeJ)]);
      frames.push(frame('isSafe()', safeData));
    }
    return frames;
  }

  // 1. Scanner Init
  steps.push({
    badge: `Scanner sc = new Scanner(System.in); — Initializing input stream in main().`,
    code: 'm_scanner',
    vars: [frame('main()', [['n', '?']])],
    grid: getBoardCells(),
    n, row: -1, col: -1, placedCount: 0, solCount: 0, solutions: []
  });

  // 2. Read N
  steps.push({
    badge: `int n = sc.nextInt(); → Read chessboard size n = ${n}.`,
    code: 'm_read_n',
    vars: [frame('main()', [['n', String(n)]])],
    grid: getBoardCells(),
    n, row: -1, col: -1, placedCount: 0, solCount: 0, solutions: []
  });

  // 3. Call solveNQueens
  steps.push({
    badge: `List<List<String>> solutions = solveNQueens(${n}); → Calling N-Queens solver from main().`,
    code: 'm_call_solver',
    vars: [frame('main()', [['n', String(n)]])],
    grid: getBoardCells(),
    n, row: -1, col: -1, placedCount: 0, solCount: 0, solutions: []
  });

  // 4. Entry solveNQueens
  steps.push({
    badge: `solveNQueens(n=${n}) entry → Preparing ${n}x${n} chessboard state and solutions container.`,
    code: 'c_entry',
    vars: [frame('main()', [['n', String(n)]]), frame('solveNQueens()', [['n', String(n)]])],
    grid: getBoardCells(),
    n, row: -1, col: -1, placedCount: 0, solCount: 0, solutions: []
  });

  // 5. Board Init
  steps.push({
    badge: `char[][] board = new char[${n}][${n}]; → Board grid memory allocated for ${n}x${n} chessboard.`,
    code: 'c_board_init',
    vars: [frame('main()', [['n', String(n)]]), frame('solveNQueens()', [['n', String(n)]])],
    grid: getBoardCells(),
    n, row: -1, col: -1, placedCount: 0, solCount: 0, solutions: []
  });

  // 6. Solutions list
  steps.push({
    badge: `List<List<String>> solutions = new ArrayList<>(); → Container initialized to collect all valid queen placements.`,
    code: 'c_vars',
    vars: [frame('main()', [['n', String(n)]]), frame('solveNQueens()', [['n', String(n)]])],
    grid: getBoardCells(),
    n, row: -1, col: -1, placedCount: 0, solCount: 0, solutions: []
  });

  // 7. Call solve
  steps.push({
    badge: `solve(0, board, solutions, ${n}); → Starting recursive backtracking search from row 0.`,
    code: 'c_call_solve',
    vars: [frame('main()', [['n', String(n)]]), frame('solveNQueens()', [['n', String(n)]])],
    grid: getBoardCells(),
    n, row: 0, col: -1, placedCount: 0, solCount: 0, solutions: []
  });

  function simulateSolve(row) {
    // Solve function entry
    steps.push({
      badge: `solve(row=${row}) → Current recursion depth: row ${row} of ${n}.`,
      code: 'c_solve_entry',
      vars: getStackFrames(row),
      grid: getBoardCells(row, -1, 'normal'),
      n, row, col: -1, placedCount: row, solCount: solutions.length, solutions: [...solutions]
    });

    // Base condition check
    const isBase = (row === n);
    steps.push({
      badge: `Checking base condition: if (row == n) → (${row} == ${n}) → ${isBase ? 'TRUE! All queens safely placed!' : 'FALSE (proceed to column loop)'}`,
      code: 'c_base_check',
      vars: getStackFrames(row),
      grid: getBoardCells(row, -1, isBase ? 'safe' : 'normal'),
      n, row, col: -1, placedCount: row, solCount: solutions.length, solutions: [...solutions]
    });

    if (isBase) {
      // Condition was true: execute block statements
      const solBoard = [...queens];
      solutions.push(solBoard);

      steps.push({
        badge: `🎉 SOLUTION FOUND! Solution #${solutions.length}: [${solBoard.map((c, r) => `Row ${r}: Col ${c}`).join(', ')}]. Adding to solutions.`,
        code: 'c_save_sol',
        vars: getStackFrames(row),
        grid: getBoardCells(-1, -1, 'solution'),
        n, row, col: -1, placedCount: n, solCount: solutions.length, solutions: [...solutions]
      });

      steps.push({
        badge: `return; → Returning from base case to backtrack and search for more solutions.`,
        code: 'c_base_ret',
        vars: getStackFrames(row),
        grid: getBoardCells(-1, -1, 'normal'),
        n, row, col: -1, placedCount: n, solCount: solutions.length, solutions: [...solutions]
      });
      return;
    }

    // Loop through columns
    for (let col = 0; col < n; col++) {
      // Loop header: each iteration step-by-step
      steps.push({
        badge: `Row ${row}: for (int col = ${col}; col < ${n}; col++) → Testing square (${row}, ${col})`,
        code: 'c_loop_col',
        vars: getStackFrames(row, col),
        grid: getBoardCells(row, col, 'testing'),
        n, row, col, placedCount: row, solCount: solutions.length, solutions: [...solutions]
      });

      // Control statement: check condition first
      steps.push({
        badge: `Checking condition: if (isSafe(row=${row}, col=${col}, board, n=${n})) → Invoking isSafe()`,
        code: 'c_check_safe',
        vars: getStackFrames(row, col),
        grid: getBoardCells(row, col, 'testing'),
        n, row, col, placedCount: row, solCount: solutions.length, solutions: [...solutions]
      });

      // Step-by-step isSafe execution
      let safe = true;
      const rayPath = [];

      // isSafe entry
      steps.push({
        badge: `isSafe(row=${row}, col=${col}) → Checking vertical column and diagonals for conflicting queens.`,
        code: 'c_safe_fn',
        vars: getStackFrames(row, col, true),
        grid: getBoardCells(row, col, 'testing'),
        n, row, col, placedCount: row, solCount: solutions.length, solutions: [...solutions]
      });

      // 1. Column check
      if (row === 0) {
        // Condition fails in the first iteration itself (0 < 0 is false)
        steps.push({
          badge: `Vertical Column Check: for (int i = 0; i < ${row}; i++) → Condition (0 < ${row}) is FALSE. No queens above in column ${col}. Exiting loop.`,
          code: 'c_safe_col_loop',
          vars: getStackFrames(row, col, true, 0, col),
          grid: getBoardCells(row, col, 'testing', -1, -1, rayPath),
          n, row, col, placedCount: row, solCount: solutions.length, solutions: [...solutions]
        });
      } else {
        for (let i = 0; i < row; i++) {
          rayPath.push({ r: i, c: col });
          // Loop header
          steps.push({
            badge: `Vertical Column Check: for (int i = ${i}; i < ${row}; i++) → Condition (${i} < ${row}) is TRUE. Inspecting cell (${i}, ${col})`,
            code: 'c_safe_col_loop',
            vars: getStackFrames(row, col, true, i, col),
            grid: getBoardCells(row, col, 'testing', -1, -1, rayPath),
            n, row, col, placedCount: row, solCount: solutions.length, solutions: [...solutions]
          });

          // Condition check
          const hasConflict = (queens[i] === col);
          steps.push({
            badge: `Checking condition: if (board[${i}][${col}] == 'Q') → ${hasConflict ? 'TRUE (Queen found at (' + i + ', ' + col + ')!)' : "FALSE (Cell is empty '.')" }`,
            code: 'c_safe_col_check',
            vars: getStackFrames(row, col, true, i, col),
            grid: getBoardCells(row, col, hasConflict ? 'conflict' : 'testing', hasConflict ? i : -1, hasConflict ? col : -1, rayPath),
            n, row, col, placedCount: row, solCount: solutions.length, solutions: [...solutions]
          });

          if (hasConflict) {
            // Condition true: execute block
            steps.push({
              badge: `❌ Column Conflict! Queen at (${i}, ${col}) attacks square (${row}, ${col}). Executing return false;`,
              code: 'c_safe_col_ret',
              vars: getStackFrames(row, col, true, i, col),
              grid: getBoardCells(row, col, 'conflict', i, col, rayPath),
              n, row, col, placedCount: row, solCount: solutions.length, solutions: [...solutions]
            });
            safe = false;
            break;
          }
        }
      }

      // 2. Upper-left diagonal check
      if (safe) {
        const initD1_i = row - 1;
        const initD1_j = col - 1;
        if (initD1_i < 0 || initD1_j < 0) {
          // Condition fails in the first iteration itself
          steps.push({
            badge: `Upper-Left Diagonal Check: for (int i = ${initD1_i}, j = ${initD1_j}; i >= 0 && j >= 0; i--, j--) → Condition (${initD1_i} >= 0 && ${initD1_j} >= 0) is FALSE (boundary reached). Exiting loop.`,
            code: 'c_safe_d1_loop',
            vars: getStackFrames(row, col, true, initD1_i, initD1_j),
            grid: getBoardCells(row, col, 'testing', -1, -1, rayPath),
            n, row, col, placedCount: row, solCount: solutions.length, solutions: [...solutions]
          });
        } else {
          for (let i = initD1_i, j = initD1_j; i >= 0 && j >= 0; i--, j--) {
            rayPath.push({ r: i, c: j });
            steps.push({
              badge: `Upper-Left Diagonal Check: for (int i = ${i}, j = ${j}; i >= 0 && j >= 0; i--, j--) → Condition is TRUE. Inspecting diagonal square (${i}, ${j})`,
              code: 'c_safe_d1_loop',
              vars: getStackFrames(row, col, true, i, j),
              grid: getBoardCells(row, col, 'testing', -1, -1, rayPath),
              n, row, col, placedCount: row, solCount: solutions.length, solutions: [...solutions]
            });

            const hasConflict = (queens[i] === j);
            steps.push({
              badge: `Checking condition: if (board[${i}][${j}] == 'Q') → ${hasConflict ? 'TRUE (Queen found at (' + i + ', ' + j + ')!)' : "FALSE (Cell is empty '.')" }`,
              code: 'c_safe_d1_check',
              vars: getStackFrames(row, col, true, i, j),
              grid: getBoardCells(row, col, hasConflict ? 'conflict' : 'testing', hasConflict ? i : -1, hasConflict ? j : -1, rayPath),
              n, row, col, placedCount: row, solCount: solutions.length, solutions: [...solutions]
            });

            if (hasConflict) {
              steps.push({
                badge: `❌ Upper-Left Diagonal Conflict! Queen at (${i}, ${j}) attacks square (${row}, ${col}). Executing return false;`,
                code: 'c_safe_d1_ret',
                vars: getStackFrames(row, col, true, i, j),
                grid: getBoardCells(row, col, 'conflict', i, j, rayPath),
                n, row, col, placedCount: row, solCount: solutions.length, solutions: [...solutions]
              });
              safe = false;
              break;
            }
          }
        }
      }

      // 3. Upper-right diagonal check
      if (safe) {
        const initD2_i = row - 1;
        const initD2_j = col + 1;
        if (initD2_i < 0 || initD2_j >= n) {
          // Condition fails in the first iteration itself
          steps.push({
            badge: `Upper-Right Diagonal Check: for (int i = ${initD2_i}, j = ${initD2_j}; i >= 0 && j < ${n}; i--, j++) → Condition (${initD2_i} >= 0 && ${initD2_j} < ${n}) is FALSE (boundary reached). Exiting loop.`,
            code: 'c_safe_d2_loop',
            vars: getStackFrames(row, col, true, initD2_i, initD2_j),
            grid: getBoardCells(row, col, 'testing', -1, -1, rayPath),
            n, row, col, placedCount: row, solCount: solutions.length, solutions: [...solutions]
          });
        } else {
          for (let i = initD2_i, j = initD2_j; i >= 0 && j < n; i--, j++) {
            rayPath.push({ r: i, c: j });
            steps.push({
              badge: `Upper-Right Diagonal Check: for (int i = ${i}, j = ${j}; i >= 0 && j < ${n}; i--, j++) → Condition is TRUE. Inspecting diagonal square (${i}, ${j})`,
              code: 'c_safe_d2_loop',
              vars: getStackFrames(row, col, true, i, j),
              grid: getBoardCells(row, col, 'testing', -1, -1, rayPath),
              n, row, col, placedCount: row, solCount: solutions.length, solutions: [...solutions]
            });

            const hasConflict = (queens[i] === j);
            steps.push({
              badge: `Checking condition: if (board[${i}][${j}] == 'Q') → ${hasConflict ? 'TRUE (Queen found at (' + i + ', ' + j + ')!)' : "FALSE (Cell is empty '.')" }`,
              code: 'c_safe_d2_check',
              vars: getStackFrames(row, col, true, i, j),
              grid: getBoardCells(row, col, hasConflict ? 'conflict' : 'testing', hasConflict ? i : -1, hasConflict ? j : -1, rayPath),
              n, row, col, placedCount: row, solCount: solutions.length, solutions: [...solutions]
            });

            if (hasConflict) {
              steps.push({
                badge: `❌ Upper-Right Diagonal Conflict! Queen at (${i}, ${j}) attacks square (${row}, ${col}). Executing return false;`,
                code: 'c_safe_d2_ret',
                vars: getStackFrames(row, col, true, i, j),
                grid: getBoardCells(row, col, 'conflict', i, j, rayPath),
                n, row, col, placedCount: row, solCount: solutions.length, solutions: [...solutions]
              });
              safe = false;
              break;
            }
          }
        }
      }

      // If all passed, return true
      if (safe) {
        steps.push({
          badge: `✅ All safety checks passed! No queens threaten (${row}, ${col}). Executing return true;`,
          code: 'c_safe_ret_true',
          vars: getStackFrames(row, col, true),
          grid: getBoardCells(row, col, 'safe', -1, -1, rayPath),
          n, row, col, placedCount: row, solCount: solutions.length, solutions: [...solutions]
        });
      }

      // Back in solve: if condition was true, execute corresponding block
      if (safe) {
        // Place queen
        queens[row] = col;
        steps.push({
          badge: `isSafe returned true → Executing block: board[${row}][${col}] = 'Q'; (Queen ♛ placed!)`,
          code: 'c_place_queen',
          vars: getStackFrames(row, col),
          grid: getBoardCells(row, col, 'safe'),
          n, row, col, placedCount: row + 1, solCount: solutions.length, solutions: [...solutions]
        });

        // Recurse
        steps.push({
          badge: `Executing block: solve(row + 1 = ${row + 1}, board, solutions, ${n}); → Recursing to next row`,
          code: 'c_recurse',
          vars: getStackFrames(row, col),
          grid: getBoardCells(row + 1, -1, 'normal'),
          n, row: row + 1, col: -1, placedCount: row + 1, solCount: solutions.length, solutions: [...solutions]
        });

        simulateSolve(row + 1);

        // Backtrack
        queens[row] = -1;
        steps.push({
          badge: `Executing block: board[${row}][${col}] = '.'; → Backtrack: Removed Queen ♛ from (${row}, ${col})`,
          code: 'c_backtrack',
          vars: getStackFrames(row, col),
          grid: getBoardCells(row, col, 'testing'),
          n, row, col, placedCount: row, solCount: solutions.length, solutions: [...solutions]
        });
      } else {
        // Condition was false: block is skipped!
      }
    }
  }

  simulateSolve(0);

  // Return solutions to main
  steps.push({
    badge: `return solutions; → Search complete! Returning ${solutions.length} valid distinct arrangements to main().`,
    code: 'c_return',
    vars: [frame('main()', [['n', String(n)]]), frame('solveNQueens()', [['solutions_count', String(solutions.length)]])],
    grid: getBoardCells(),
    n, row: -1, col: -1, placedCount: 0, solCount: solutions.length, solutions: [...solutions]
  });

  // Print result in main()
  steps.push({
    badge: `System.out.println(solutions.size()); → Printing total valid solutions found: ${solutions.length}.`,
    code: 'm_print_ans',
    vars: [frame('main()', [['n', String(n)], ['solutions_count', String(solutions.length)]])],
    grid: getBoardCells(),
    n, row: -1, col: -1, placedCount: 0, solCount: solutions.length, solutions: [...solutions]
  });

  // Execution complete
  steps.push({
    badge: `Program execution finished successfully. N-Queens search completed with ${solutions.length} solutions.`,
    code: 'm_done',
    vars: [frame('main()', [['status', 'finished'], ['solutions_count', String(solutions.length)]])],
    grid: getBoardCells(),
    n, row: -1, col: -1, placedCount: 0, solCount: solutions.length, solutions: [...solutions]
  });

  return steps;
}

const inputN = ref(4);
const lang = ref('java');
const speed = ref(650);
const si = ref(0);
const playing = ref(false);
const vizHeight = ref(290);
const tableHeight = ref(60);
const leftWidth = ref(50);
const rightTab = ref('code');
const selectedSolutionIdx = ref(-1);

const stepsData = reactive({ steps: buildSteps(4) });
const steps = computed(() => stepsData.steps);
const s = computed(() => steps.value[Math.max(0, Math.min(si.value, steps.value.length - 1))] || {});
const codeLines = computed(() => CODES[lang.value] || []);

let playTimer = null;

function applyInput() {
  const val = parseInt(inputN.value, 10);
  if (isNaN(val) || val < 1 || val > 8) {
    alert('Please enter N between 1 and 8 for optimal visualization.');
    inputN.value = 4;
    return;
  }
  playing.value = false;
  selectedSolutionIdx.value = -1;
  stepsData.steps = buildSteps(val);
  si.value = 0;
  if (typeof window !== 'undefined') window.scrollTo(0, 0);
}

function selectN(num) {
  inputN.value = num;
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

    // Scroll ONLY the code container, NEVER calling scrollIntoView which scrolls the window and slide containers
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

// Compute active grid: if a solution is selected from thumbnails, display that solution board
const displayGrid = computed(() => {
  if (selectedSolutionIdx.value >= 0 && s.value.solutions && s.value.solutions[selectedSolutionIdx.value]) {
    const sol = s.value.solutions[selectedSolutionIdx.value];
    const n = s.value.n || inputN.value;
    const g = [];
    for (let r = 0; r < n; r++) {
      const rowCells = [];
      for (let c = 0; c < n; c++) {
        const hasQueen = sol[r] === c;
        rowCells.push({
          r, c,
          hasQueen,
          isLight: (r + c) % 2 === 0,
          state: hasQueen ? 'solution_queen' : 'normal'
        });
      }
      g.push(rowCells);
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
              <label>Board Size (N):</label>
              <input
                type="number"
                min="1"
                max="8"
                v-model.number="inputN"
                class="ll-text-input"
                @keyup.enter="applyInput"
                style="width: 55px;"
              />
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
                  <div class="ll-ptrs">
                    <div class="ll-ptr-chip">N = <b class="ll-c-blue">{{ s.n || inputN }}</b></div>
                    <div class="ll-ptr-chip">Row = <b class="ll-c-orange">{{ s.row !== undefined && s.row >= 0 ? s.row : '—' }}</b></div>
                    <div class="ll-ptr-chip">Col = <b class="ll-c-purple">{{ s.col !== undefined && s.col >= 0 ? s.col : '—' }}</b></div>
                    <div class="ll-ptr-chip">Queens Placed = <b class="ll-c-green">{{ s.placedCount || 0 }} / {{ s.n || inputN }}</b></div>
                    <div class="ll-ptr-chip">Solutions = <b class="ll-c-green">{{ s.solCount || 0 }}</b></div>
                  </div>

                  <!-- Chessboard Container -->
                  <div class="ll-board-container">
                    <div class="ll-board-frame">
                      <!-- Column Headers -->
                      <div class="ll-col-indices">
                        <div class="ll-axis-spacer"></div>
                        <div
                          v-for="c in (s.n || inputN)"
                          :key="'col-h-' + c"
                          class="ll-axis-label"
                          :class="{ 'll-axis-cur': s.col === c - 1 }"
                        >
                          {{ c - 1 }}
                        </div>
                      </div>

                      <!-- Board Rows -->
                      <div class="ll-board-rows">
                        <div
                          v-for="(row, rIdx) in displayGrid"
                          :key="'row-' + rIdx"
                          class="ll-board-row"
                        >
                          <!-- Row Label -->
                          <div class="ll-axis-label ll-row-label" :class="{ 'll-axis-cur': s.row === rIdx }">
                            {{ rIdx }}
                          </div>

                          <!-- Chess Squares -->
                          <div
                            v-for="(cell, cIdx) in row"
                            :key="'cell-' + rIdx + '-' + cIdx"
                            class="ll-chess-cell"
                            :class="{
                              'll-cell-light': cell.isLight,
                              'll-cell-dark': !cell.isLight,
                              'll-cell-testing': cell.state === 'testing',
                              'll-cell-safe': cell.state === 'safe',
                              'll-cell-conflict': cell.state === 'conflict',
                              'll-cell-conflict-src': cell.state === 'conflict_source',
                              'll-cell-sol-queen': cell.state === 'solution_queen',
                              'll-cell-ray': cell.state === 'ray'
                            }"
                            :title="`Square (${rIdx}, ${cIdx})`"
                          >
                            <!-- Queen Piece -->
                            <transition name="queen-pop">
                              <div v-if="cell.hasQueen" class="ll-queen-piece">
                                ♛
                              </div>
                            </transition>

                            <div v-if="cell.state === 'testing' && !cell.hasQueen" class="ll-test-marker">
                              ?
                            </div>

                            <div v-if="cell.state === 'conflict'" class="ll-conflict-marker">
                              ✕
                            </div>

                            <div v-if="cell.state === 'ray' && !cell.hasQueen" class="ll-ray-marker">
                              ·
                            </div>
                          </div>
                        </div>
                      </div>
                    </div>

                    <!-- Discovered Solutions Gallery -->
                    <div class="ll-solutions-strip" v-if="s.solutions && s.solutions.length">
                      <div class="ll-sol-title">Discovered Solutions ({{ s.solutions.length }}):</div>
                      <div class="ll-sol-cards">
                        <button
                          v-for="(sol, idx) in s.solutions"
                          :key="'sol-btn-' + idx"
                          class="ll-sol-badge-btn"
                          :class="{ active: selectedSolutionIdx === idx }"
                          @click="viewSolution(idx)"
                          :title="`Click to inspect Solution #${idx + 1}`"
                        >
                          Sol #{{ idx + 1 }}
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
                <span class="ll-leg"><span class="ll-legdot ll-legdot-active"></span>Testing (r, c)</span>
                <span class="ll-leg"><span class="ll-legdot ll-legdot-safe"></span>Safe Placement</span>
                <span class="ll-leg"><span class="ll-legdot ll-legdot-mismatch"></span>Conflict ✕</span>
                <span class="ll-leg"><span class="ll-legdot ll-legdot-max"></span>Valid Solution</span>
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
                    'll-badge-error': s.badge && (s.badge.includes('Conflict') || s.badge.includes('UNSAFE') || s.badge.includes('attacks')),
                    'll-badge-success': s.badge && (s.badge.includes('SOLUTION') || s.badge.includes('safely') || s.badge.includes('passed') || s.badge.includes('Complete'))
                  }"
                >
                  {{ s.badge || 'Ready to run N-Queens Backtracking algorithm.' }}
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
                  <h3 class="ll-cx-heading">N-Queens &mdash; Complexity Analysis</h3>
                  <p class="ll-cx-intro">
                    Places N non-attacking queens on an N&times;N chessboard using backtracking.
                    Search branches are pruned as soon as a queen conflicts with an already-placed queen.
                  </p>

                  <h4 class="ll-cx-sub">Step-by-step Breakdown</h4>
                  <table class="ll-complexity-table">
                    <thead>
                      <tr><th>Operation / Phase</th><th>Time</th><th>Space</th><th>Reason</th></tr>
                    </thead>
                    <tbody>
                      <tr>
                        <td>Safe check: <code>isSafe(row, col)</code></td>
                        <td class="ll-cx-good">O(N)</td>
                        <td class="ll-cx-good">O(1)</td>
                        <td>Checks 1 column + 2 upper diagonal directions</td>
                      </tr>
                      <tr>
                        <td>Backtracking search (pruned)</td>
                        <td class="ll-cx-mid">O(N!)</td>
                        <td class="ll-cx-good">O(N)</td>
                        <td>Row 0 has N choices, row 1 &le; N-2, row 2 &le; N-4; recursion stack depth = N</td>
                      </tr>
                      <tr>
                        <td>Naive exhaustive search</td>
                        <td class="ll-cx-bad">O(N<sup>N</sup>)</td>
                        <td class="ll-cx-good">O(N)</td>
                        <td>Without pruning: all configurations of row/column placements</td>
                      </tr>
                    </tbody>
                  </table>

                  <h4 class="ll-cx-sub">Overall Complexity</h4>
                  <div class="ll-cx-summary-grid">
                    <div class="ll-cx-card ll-cx-card-mid">
                      <div class="ll-cx-card-label">Time</div>
                      <div class="ll-cx-card-val">O(N!)</div>
                      <div class="ll-cx-card-note">Pruned backtracking search</div>
                    </div>
                    <div class="ll-cx-card ll-cx-card-good">
                      <div class="ll-cx-card-label">Space</div>
                      <div class="ll-cx-card-val">O(N)</div>
                      <div class="ll-cx-card-note">Recursion call stack &amp; board</div>
                    </div>
                    <div class="ll-cx-card ll-cx-card-good">
                      <div class="ll-cx-card-label">Solutions</div>
                      <div class="ll-cx-card-val">Depends on N</div>
                      <div class="ll-cx-card-note">N=4: 2 | N=8: 92 sols</div>
                    </div>
                  </div>

                  <div class="ll-note">
                    <strong>Backtracking Insight:</strong> Rather than evaluating all N<sup>N</sup> combinations, we test safe squares row-by-row.
                    The moment a queen at <code>(row, col)</code> conflicts on a column or diagonal with an existing queen above, we immediately prune the entire subtree!
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
  position: relative;
  z-index: 20;
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
.ll-ptrs { display: flex; gap: 8px; flex-wrap: wrap; padding: 8px 16px 4px; min-height: 36px; width: 100%; box-sizing: border-box; min-width: 0; }
.ll-ptr-chip { background: var(--surface2); border: 1px solid var(--border); border-radius: var(--radius-sm); padding: 3px 10px; font-size: 12px; font-family: monospace; box-shadow: var(--shadow-sm); white-space: nowrap; flex-shrink: 0; }
.ll-c-blue { color: var(--blue); } .ll-c-orange { color: var(--orange); } .ll-c-green { color: var(--green); } .ll-c-purple { color: var(--purple); } .ll-c-red { color: var(--red); }

/* Chessboard Layout */
.ll-board-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 8px 16px 14px;
  gap: 10px;
}

.ll-board-frame {
  display: inline-flex;
  flex-direction: column;
  background: #cbd5e1;
  border: 2px solid #94a3b8;
  border-radius: 6px;
  padding: 4px;
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
}

.ll-col-indices {
  display: flex;
  align-items: center;
  height: 20px;
}

.ll-axis-spacer {
  width: 22px;
  height: 20px;
}

.ll-axis-label {
  width: 38px;
  height: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-family: monospace;
  font-size: 11px;
  font-weight: 700;
  color: #64748b;
}

.ll-row-label {
  width: 22px;
  height: 38px;
}

.ll-axis-cur {
  color: var(--coral-dark);
  font-weight: 900;
}

.ll-board-rows {
  display: flex;
  flex-direction: column;
}

.ll-board-row {
  display: flex;
  align-items: center;
}

.ll-chess-cell {
  width: 38px;
  height: 38px;
  display: flex;
  align-items: center;
  justify-content: center;
  border: 1px solid #cbd5e1;
  font-family: monospace;
  position: relative;
  transition: all .2s ease;
  user-select: none;
}

.ll-cell-light {
  background-color: #f8fafc;
}

.ll-cell-dark {
  background-color: #e2e8f0;
}

.ll-cell-testing {
  background-color: #fef3c7 !important;
  border: 2px solid #f59e0b !important;
  transform: scale(1.06);
  z-index: 10;
}

.ll-cell-safe {
  background-color: #dcfce7 !important;
  border: 2px solid #10b981 !important;
  transform: scale(1.06);
  z-index: 10;
}

.ll-cell-conflict {
  background-color: #fee2e2 !important;
  border: 2px solid #ef4444 !important;
  animation: ll-pulse-conflict 0.6s ease-in-out;
  z-index: 10;
}

.ll-cell-conflict-src {
  background-color: #fce7f3 !important;
  border: 2px dashed #ec4899 !important;
  animation: ll-pulse-conflict 0.8s infinite ease-in-out;
  z-index: 10;
}

.ll-cell-ray {
  background-color: #e0f2fe !important;
  border: 1.5px dashed #0284c7 !important;
  z-index: 5;
}

.ll-cell-sol-queen {
  background-color: #fef9c3 !important;
  border: 2px solid #eab308 !important;
}

.ll-queen-piece {
  font-size: 24px;
  color: #1e1b4b;
  text-shadow: 0 2px 4px rgba(0, 0, 0, 0.25);
  animation: ll-pop 0.25s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}

.ll-test-marker {
  font-size: 16px;
  font-weight: 900;
  color: #f59e0b;
}

.ll-conflict-marker {
  font-size: 18px;
  font-weight: 900;
  color: #dc2626;
}

.ll-ray-marker {
  font-size: 22px;
  line-height: 1;
  font-weight: 900;
  color: #0284c7;
}

/* Solutions Gallery */
.ll-solutions-strip {
  display: flex;
  align-items: center;
  gap: 8px;
  width: 100%;
  max-width: 440px;
  background: var(--surface2);
  border: 1px solid var(--border);
  border-radius: var(--radius-sm);
  padding: 4px 10px;
  flex-wrap: wrap;
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
  background: var(--surface);
  border: 1px solid var(--border2);
  color: var(--text2);
  padding: 2px 8px;
  border-radius: 4px;
  font-size: 10.5px;
  font-weight: 600;
  cursor: pointer;
  transition: all .15s ease;
}

.ll-sol-badge-btn:hover {
  border-color: var(--coral);
  color: var(--coral);
}

.ll-sol-badge-btn.active {
  background: #10b981;
  color: #ffffff;
  border-color: #10b981;
}

/* Resizer */
.ll-vresizer { height: 5px; cursor: row-resize; background: var(--border); flex-shrink: 0; transition: background .15s; position: relative; z-index: 20; }
.ll-vresizer:hover, .ll-vresizer.drag { background: var(--coral); }

/* Legend */
.ll-legend { display: flex; flex-wrap: wrap; gap: 6px 14px; padding: 6px 12px; border-bottom: 1px solid var(--border); flex-shrink: 0; background: var(--surface2); }
.ll-leg { display: flex; align-items: center; gap: 5px; font-size: 11px; color: var(--text2); font-weight: 500; }
.ll-legdot { width: 11px; height: 11px; border-radius: 3px; flex-shrink: 0; display: inline-block; }
.ll-legdot-existing { background: #f8fafc; border: 1.5px solid var(--border2); }
.ll-legdot-active { background: #fef3c7; border: 1.5px solid #f59e0b; }
.ll-legdot-ray { background: #e0f2fe; border: 1.5px dashed #0284c7; }
.ll-legdot-queen { background: #1e1b4b; border: 1.5px solid #0f172a; }
.ll-legdot-safe { background: #dcfce7; border: 1.5px solid #10b981; }
.ll-legdot-mismatch { background: #fee2e2; border: 1.5px solid #ef4444; }
.ll-legdot-max { background: #fef9c3; border: 1.5px solid #eab308; }

/* Stack Frame Area */
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

/* Faculty Lecture & Proof Tab */
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
  font-family: Consolas, monospace;
  font-size: 11px;
  background: var(--surface2);
  padding: 1px 4px;
  border-radius: 4px;
  border: 1px solid var(--border);
  color: var(--coral-dark);
}

.ll-math-box {
  background: var(--surface2);
  border-left: 3px solid var(--purple);
  padding: 6px 10px;
  font-family: 'Cambria Math', 'Times New Roman', serif;
  font-size: 11px !important;
  color: var(--text) !important;
  border-radius: 0 4px 4px 0;
}

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

/* Complexity Tab Styles */
.ll-cx-heading {
  font-size: 13px;
  font-weight: 700;
  color: var(--text);
  margin: 0 0 6px;
}

.ll-cx-intro {
  font-size: 10.5px;
  color: var(--text2);
  margin: 0 0 10px;
  line-height: 1.55;
}

.ll-cx-sub {
  font-size: 11px;
  font-weight: 700;
  color: var(--text2);
  margin: 10px 0 4px;
  border-bottom: 1px solid var(--border);
  padding-bottom: 3px;
}

/* Colored complexity cells */
.ll-cx-good { color: #15803d; font-weight: 700; }
.ll-cx-mid  { color: #b45309; font-weight: 700; }
.ll-cx-bad  { color: #b91c1c; font-weight: 700; }

/* Summary card grid */
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

.ll-cx-card-label {
  font-size: 9px;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: .05em;
  opacity: .7;
  margin-bottom: 4px;
}

.ll-cx-card-val {
  font-size: 13px;
  font-weight: 800;
  font-family: monospace;
  margin-bottom: 3px;
}

.ll-cx-card-note {
  font-size: 8.5px;
  opacity: .75;
  line-height: 1.3;
}

.ll-cx-card-good {
  background: #f0fdf4;
  border-color: #86efac;
  color: #15803d;
}

.ll-cx-card-mid {
  background: #fff7ed;
  border-color: #fed7aa;
  color: #c2410c;
}

.ll-cx-card-bad {
  background: #fef2f2;
  border-color: #fca5a5;
  color: #b91c1c;
}

/* Footer Toolbar */
.ll-footer {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 4px 12px;
  background: var(--surface);
  border-top: 1px solid var(--border);
  font-size: 11px;
  color: var(--muted);
  font-weight: 600;
  flex-shrink: 0;
}

.ll-speed-wrap {
  display: flex;
  align-items: center;
  gap: 6px;
}

.ll-speed-wrap input[type="range"] {
  width: 80px;
  accent-color: var(--coral);
}
</style>
