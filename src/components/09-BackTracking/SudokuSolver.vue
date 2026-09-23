<script setup>
import { ref, reactive, computed, onMounted, onBeforeUnmount, watch, nextTick } from 'vue';

defineProps({
  topic: { type: String, default: 'Backtracking Algorithms' },
  subTopic: { type: String, default: 'Solve the Sudoku' }
});

// ─── Multi-Language Code Definitions ─────────────────────────────────────────
const CODES = {
  java: [
    ['',                  'import java.util.Scanner;'],
    ['',                  ''],
    ['',                  'public class Main {'],
    ['c_safe_fn',         '    static boolean isSafe(int[][] board, int row, int col, int num) {'],
    ['c_safe_row',        '        for (int x = 0; x < 9; x++) {'],
    ['',                  '            if (board[row][x] == num) return false;'],
    ['c_safe_col',        '            if (board[x][col] == num) return false;'],
    ['',                  '        }'],
    ['c_safe_box',        '        int startRow = row - row % 3, startCol = col - col % 3;'],
    ['',                  '        for (int i = 0; i < 3; i++) {'],
    ['',                  '            for (int j = 0; j < 3; j++) {'],
    ['',                  '                if (board[i + startRow][j + startCol] == num) return false;'],
    ['',                  '            }'],
    ['',                  '        }'],
    ['c_safe_ret_true',   '        return true;'],
    ['',                  '    }'],
    ['',                  ''],
    ['c_solve_entry',     '    static boolean solveSudoku(int[][] board) {'],
    ['c_find_empty',      '        int row = -1, col = -1;'],
    ['',                  '        boolean isEmpty = false;'],
    ['',                  '        for (int i = 0; i < 9; i++) {'],
    ['',                  '            for (int j = 0; j < 9; j++) {'],
    ['',                  '                if (board[i][j] == 0) {'],
    ['',                  '                    row = i; col = j;'],
    ['',                  '                    isEmpty = true;'],
    ['',                  '                    break;'],
    ['',                  '                }'],
    ['',                  '            }'],
    ['',                  '            if (isEmpty) break;'],
    ['',                  '        }'],
    ['c_base_check',      '        if (!isEmpty) return true;'],
    ['',                  ''],
    ['c_loop_digits',     '        for (int num = 1; num <= 9; num++) {'],
    ['c_check_safe',      '            if (isSafe(board, row, col, num)) {'],
    ['c_place_digit',     '                board[row][col] = num;'],
    ['c_recurse',         '                if (solveSudoku(board)) return true;'],
    ['c_backtrack',       '                board[row][col] = 0;'],
    ['',                  '            }'],
    ['',                  '        }'],
    ['c_ret_false',       '        return false;'],
    ['',                  '    }'],
    ['',                  ''],
    ['',                  '    public static void main(String[] args) {'],
    ['m_scanner',         '        Scanner sc = new Scanner(System.in);'],
    ['m_alloc_board',     '        int[][] board = new int[9][9];'],
    ['m_read_grid',       '        for (int i = 0; i < 9; i++) {'],
    ['',                  '            for (int j = 0; j < 9; j++) {'],
    ['',                  '                board[i][j] = sc.nextInt();'],
    ['',                  '            }'],
    ['',                  '        }'],
    ['m_call_solver',     '        boolean solved = solveSudoku(board);'],
    ['m_print_result',    '        if (solved) {'],
    ['',                  '            for (int i = 0; i < 9; i++) {'],
    ['',                  '                for (int j = 0; j < 9; j++) System.out.print(board[i][j] + " ");'],
    ['',                  '                System.out.println();'],
    ['',                  '            }'],
    ['',                  '        }'],
    ['m_done',            '    }'],
    ['',                  '}']
  ],
  cpp: [
    ['',                  '#include <iostream>'],
    ['',                  'using namespace std;'],
    ['',                  ''],
    ['c_safe_fn',         'bool isSafe(int board[9][9], int row, int col, int num) {'],
    ['c_safe_row',        '    for (int x = 0; x < 9; x++) {'],
    ['',                  '        if (board[row][x] == num) return false;'],
    ['c_safe_col',        '        if (board[x][col] == num) return false;'],
    ['',                  '    }'],
    ['c_safe_box',        '    int startRow = row - row % 3, startCol = col - col % 3;'],
    ['',                  '    for (int i = 0; i < 3; i++) {'],
    ['',                  '        for (int j = 0; j < 3; j++) {'],
    ['',                  '            if (board[i + startRow][j + startCol] == num) return false;'],
    ['',                  '        }'],
    ['',                  '    }'],
    ['c_safe_ret_true',   '    return true;'],
    ['',                  '}'],
    ['',                  ''],
    ['c_solve_entry',     'bool solveSudoku(int board[9][9]) {'],
    ['c_find_empty',      '    int row = -1, col = -1;'],
    ['',                  '    bool isEmpty = false;'],
    ['',                  '    for (int i = 0; i < 9; i++) {'],
    ['',                  '        for (int j = 0; j < 9; j++) {'],
    ['',                  '            if (board[i][j] == 0) {'],
    ['',                  '                row = i; col = j; isEmpty = true; break;'],
    ['',                  '            }'],
    ['',                  '        }'],
    ['',                  '        if (isEmpty) break;'],
    ['',                  '    }'],
    ['c_base_check',      '    if (!isEmpty) return true;'],
    ['',                  ''],
    ['c_loop_digits',     '    for (int num = 1; num <= 9; num++) {'],
    ['c_check_safe',      '        if (isSafe(board, row, col, num)) {'],
    ['c_place_digit',     '            board[row][col] = num;'],
    ['c_recurse',         '            if (solveSudoku(board)) return true;'],
    ['c_backtrack',       '            board[row][col] = 0;'],
    ['',                  '        }'],
    ['',                  '    }'],
    ['c_ret_false',       '    return false;'],
    ['',                  '}'],
    ['',                  ''],
    ['int main() {'],
    ['m_scanner',         '    // Standard input reader'],
    ['m_alloc_board',     '    int board[9][9];'],
    ['m_read_grid',       '    for (int i = 0; i < 9; i++)'],
    ['',                  '        for (int j = 0; j < 9; j++) cin >> board[i][j];'],
    ['m_call_solver',     '    bool solved = solveSudoku(board);'],
    ['m_print_result',    '    if (solved) {'],
    ['',                  '        for (int i = 0; i < 9; i++) {'],
    ['',                  '            for (int j = 0; j < 9; j++) cout << board[i][j] << " ";'],
    ['',                  '            cout << "\\n";'],
    ['',                  '        }'],
    ['',                  '    }'],
    ['m_done',            '    return 0;'],
    ['',                  '}']
  ],
  c: [
    ['',                  '#include <stdio.h>'],
    ['',                  '#include <stdbool.h>'],
    ['',                  ''],
    ['c_safe_fn',         'bool isSafe(int board[9][9], int row, int col, int num) {'],
    ['c_safe_row',        '    for (int x = 0; x < 9; x++) {'],
    ['',                  '        if (board[row][x] == num) return false;'],
    ['c_safe_col',        '        if (board[x][col] == num) return false;'],
    ['',                  '    }'],
    ['c_safe_box',        '    int startRow = row - row % 3, startCol = col - col % 3;'],
    ['',                  '    for (int i = 0; i < 3; i++) {'],
    ['',                  '        for (int j = 0; j < 3; j++) {'],
    ['',                  '            if (board[i + startRow][j + startCol] == num) return false;'],
    ['',                  '        }'],
    ['',                  '    }'],
    ['c_safe_ret_true',   '    return true;'],
    ['',                  '}'],
    ['',                  ''],
    ['c_solve_entry',     'bool solveSudoku(int board[9][9]) {'],
    ['c_find_empty',      '    int row = -1, col = -1;'],
    ['',                  '    bool isEmpty = false;'],
    ['',                  '    for (int i = 0; i < 9; i++) {'],
    ['',                  '        for (int j = 0; j < 9; j++) {'],
    ['',                  '            if (board[i][j] == 0) {'],
    ['',                  '                row = i; col = j; isEmpty = true; break;'],
    ['',                  '            }'],
    ['',                  '        }'],
    ['',                  '        if (isEmpty) break;'],
    ['',                  '    }'],
    ['c_base_check',      '    if (!isEmpty) return true;'],
    ['',                  ''],
    ['c_loop_digits',     '    for (int num = 1; num <= 9; num++) {'],
    ['c_check_safe',      '        if (isSafe(board, row, col, num)) {'],
    ['c_place_digit',     '            board[row][col] = num;'],
    ['c_recurse',         '            if (solveSudoku(board)) return true;'],
    ['c_backtrack',       '            board[row][col] = 0;'],
    ['',                  '        }'],
    ['',                  '    }'],
    ['c_ret_false',       '    return false;'],
    ['',                  '}'],
    ['',                  ''],
    ['int main() {'],
    ['m_scanner',         '    // Standard input reader'],
    ['m_alloc_board',     '    int board[9][9];'],
    ['m_read_grid',       '    for (int i = 0; i < 9; i++)'],
    ['',                  '        for (int j = 0; j < 9; j++) scanf("%d", &board[i][j]);'],
    ['m_call_solver',     '    bool solved = solveSudoku(board);'],
    ['m_print_result',    '    if (solved) {'],
    ['',                  '        for (int i = 0; i < 9; i++) {'],
    ['',                  '            for (int j = 0; j < 9; j++) printf("%d ", board[i][j]);'],
    ['',                  '            printf("\\n");'],
    ['',                  '        }'],
    ['',                  '    }'],
    ['m_done',            '    return 0;'],
    ['',                  '}']
  ],
  python: [
    ['',                  'import sys'],
    ['',                  ''],
    ['c_safe_fn',         'def is_safe(board, row, col, num):'],
    ['c_safe_row',        '    for x in range(9):'],
    ['',                  '        if board[row][x] == num: return False'],
    ['c_safe_col',        '        if board[x][col] == num: return False'],
    ['c_safe_box',        '    start_r, start_c = row - row % 3, col - col % 3'],
    ['',                  '    for i in range(3):'],
    ['',                  '        for j in range(3):'],
    ['',                  '            if board[i + start_r][j + start_c] == num: return False'],
    ['c_safe_ret_true',   '    return True'],
    ['',                  ''],
    ['c_solve_entry',     'def solve_sudoku(board):'],
    ['c_find_empty',      '    row, col = -1, -1'],
    ['',                  '    is_empty = False'],
    ['',                  '    for i in range(9):'],
    ['',                  '        for j in range(9):'],
    ['',                  '            if board[i][j] == 0:'],
    ['',                  '                row, col = i, j; is_empty = True; break'],
    ['',                  '        if is_empty: break'],
    ['c_base_check',      '    if not is_empty: return True'],
    ['',                  ''],
    ['c_loop_digits',     '    for num in range(1, 10):'],
    ['c_check_safe',      '        if is_safe(board, row, col, num):'],
    ['c_place_digit',     '            board[row][col] = num'],
    ['c_recurse',         '            if solve_sudoku(board): return True'],
    ['c_backtrack',       '            board[row][col] = 0'],
    ['c_ret_false',       '    return False'],
    ['',                  ''],
    ['m_scanner',         'lines = sys.stdin.read().split()'],
    ['m_alloc_board',     'board = [[0]*9 for _ in range(9)]'],
    ['m_read_grid',       'idx = 0'],
    ['',                  'for i in range(9):'],
    ['',                  '    for j in range(9):'],
    ['',                  '        board[i][j] = int(lines[idx]); idx += 1'],
    ['m_call_solver',     'solved = solve_sudoku(board)'],
    ['m_print_result',    'if solved:'],
    ['',                  '    for row in board: print(*(row))'],
    ['m_done',            '']
  ],
  javascript: [
    ['c_safe_fn',         'function isSafe(board, row, col, num) {'],
    ['c_safe_row',        '    for (let x = 0; x < 9; x++) {'],
    ['',                  '        if (board[row][x] === num) return false;'],
    ['c_safe_col',        '        if (board[x][col] === num) return false;'],
    ['',                  '    }'],
    ['c_safe_box',        '    const startRow = row - row % 3, startCol = col - col % 3;'],
    ['',                  '    for (let i = 0; i < 3; i++) {'],
    ['',                  '        for (let j = 0; j < 3; j++) {'],
    ['',                  '            if (board[i + startRow][j + startCol] === num) return false;'],
    ['',                  '        }'],
    ['',                  '    }'],
    ['c_safe_ret_true',   '    return true;'],
    ['',                  '}'],
    ['',                  ''],
    ['c_solve_entry',     'function solveSudoku(board) {'],
    ['c_find_empty',      '    let row = -1, col = -1, isEmpty = false;'],
    ['',                  '    for (let i = 0; i < 9; i++) {'],
    ['',                  '        for (let j = 0; j < 9; j++) {'],
    ['',                  '            if (board[i][j] === 0) {'],
    ['',                  '                row = i; col = j; isEmpty = true; break;'],
    ['',                  '            }'],
    ['',                  '        }'],
    ['',                  '        if (isEmpty) break;'],
    ['',                  '    }'],
    ['c_base_check',      '    if (!isEmpty) return true;'],
    ['',                  ''],
    ['c_loop_digits',     '    for (let num = 1; num <= 9; num++) {'],
    ['c_check_safe',      '        if (isSafe(board, row, col, num)) {'],
    ['c_place_digit',     '            board[row][col] = num;'],
    ['c_recurse',         '            if (solveSudoku(board)) return true;'],
    ['c_backtrack',       '            board[row][col] = 0;'],
    ['',                  '        }'],
    ['',                  '    }'],
    ['c_ret_false',       '    return false;'],
    ['',                  '}'],
    ['',                  ''],
    ['m_scanner',         'const input = readInput();'],
    ['m_alloc_board',     'const board = Array.from({ length: 9 }, () => new Array(9).fill(0));'],
    ['m_read_grid',       'for (let i = 0; i < 9; i++) {'],
    ['',                  '    for (let j = 0; j < 9; j++) board[i][j] = input[i][j];'],
    ['',                  '}'],
    ['m_call_solver',     'const solved = solveSudoku(board);'],
    ['m_print_result',    'if (solved) console.log(board.map(r => r.join(" ")).join("\\n"));'],
    ['m_done',            '']
  ]
};

const PSEUDOCODE = [
  'function solveSudoku(board):',
  '    row, col = findEmptyCell(board)      // Locate next cell with board[r][c] == 0',
  '    if no empty cell:                    // Base case: all 81 cells validly filled',
  '        return true',
  '    for num = 1 to 9:                    // Try candidate digits 1 through 9',
  '        if isSafe(board, row, col, num): // Check row, column, and 3x3 subgrid',
  '            board[row][col] = num        // Place candidate digit',
  '            if solveSudoku(board):       // Recurse to solve next empty cell',
  '                return true              // Success: propagate upwards',
  '            board[row][col] = 0          // Backtrack: erase and reset cell to 0',
  '    return false                         // Dead end: all digits 1..9 failed',
  '',
  'function isSafe(board, row, col, num):',
  '    for x = 0 to 8:                      // Check row and column conflicts',
  '        if board[row][x] == num: return false',
  '        if board[x][col] == num: return false',
  '    startRow = row - row % 3, startCol = col - col % 3',
  '    for i = 0 to 2:                      // Check 3x3 sub-box conflict',
  '        for j = 0 to 2:',
  '            if board[startRow + i][startCol + j] == num: return false',
  '    return true                          // Safe to place num'
];

// ─── Preset Configurations ───────────────────────────────────────────────────
const PRESETS = {
  standard: {
    id: 'standard',
    name: 'Standard 9x9',
    description: 'Classic 32-clue newspaper Sudoku (55 recursive calls to full solution)',
    n: 9,
    boxSize: 3,
    grid: [
      [0, 0, 0, 2, 6, 0, 7, 0, 1],
      [6, 8, 0, 0, 7, 0, 0, 9, 0],
      [1, 9, 0, 0, 0, 4, 5, 0, 0],
      [8, 2, 0, 1, 0, 0, 0, 4, 0],
      [0, 0, 4, 6, 0, 2, 9, 0, 0],
      [0, 5, 0, 0, 0, 3, 0, 2, 8],
      [0, 0, 9, 3, 0, 0, 0, 7, 4],
      [0, 4, 0, 0, 5, 0, 0, 3, 6],
      [7, 0, 3, 0, 1, 8, 0, 0, 0]
    ]
  },
  simple: {
    id: 'simple',
    name: 'Simple 9x9',
    description: '63 clues given; solves 2 boxes with 5 backtracks for fast classroom demonstration',
    n: 9,
    boxSize: 3,
    grid: [
      [0, 0, 0, 0, 0, 0, 9, 1, 2],
      [0, 0, 0, 0, 0, 0, 3, 4, 8],
      [0, 0, 0, 0, 0, 0, 5, 6, 7],
      [8, 5, 9, 7, 6, 1, 4, 2, 3],
      [4, 2, 6, 8, 5, 3, 7, 9, 1],
      [7, 1, 3, 9, 2, 4, 8, 5, 6],
      [9, 6, 1, 5, 3, 7, 2, 8, 4],
      [2, 8, 7, 4, 1, 9, 6, 3, 5],
      [3, 4, 5, 2, 8, 6, 1, 7, 9]
    ]
  },
  mini4: {
    id: 'mini4',
    name: '4x4 Mini-Sudoku',
    description: '4x4 grid with 2x2 boxes, digits 1..4 (16 recursive calls, 3 backtracks)',
    n: 4,
    boxSize: 2,
    grid: [
      [1, 0, 0, 0],
      [0, 0, 2, 0],
      [0, 3, 0, 0],
      [0, 0, 0, 4]
    ]
  },
  gfg: {
    id: 'gfg',
    name: 'GFG Benchmark 9x9',
    description: 'Official GeeksforGeeks problem configuration',
    n: 9,
    boxSize: 3,
    grid: [
      [3, 0, 6, 5, 0, 8, 4, 0, 0],
      [5, 2, 0, 0, 0, 0, 0, 0, 0],
      [0, 8, 7, 0, 0, 0, 0, 3, 1],
      [0, 0, 3, 0, 1, 0, 0, 8, 0],
      [9, 0, 0, 8, 6, 3, 0, 0, 5],
      [0, 5, 0, 0, 9, 0, 6, 0, 0],
      [1, 3, 0, 0, 0, 0, 2, 5, 0],
      [0, 0, 0, 0, 0, 0, 0, 7, 4],
      [0, 0, 5, 2, 0, 6, 3, 0, 0]
    ]
  }
};

// ─── Step Generator (Step-by-Step, Never Skipping Any Statement) ─────────────
function buildSteps(presetKey) {
  const preset = PRESETS[presetKey] || PRESETS.standard;
  const n = preset.n;
  const boxSize = preset.boxSize;
  const initialGrid = preset.grid.map(row => [...row]);
  const MAX_STEPS = 4500;
  const steps = [];

  const board = Array.from({ length: n }, () => new Array(n).fill(0));
  const givenMask = Array.from({ length: n }, (_, r) =>
    Array.from({ length: n }, (_, c) => initialGrid[r][c] !== 0)
  );

  let initialClueCount = 0;
  for (let r = 0; r < n; r++) {
    for (let c = 0; c < n; c++) {
      if (initialGrid[r][c] !== 0) initialClueCount++;
    }
  }

  function boardSnap() { return board.map(r => [...r]); }
  function frm(title, rows) { return { title, rows }; }

  const callStack = [];
  function stackSnap(extra = []) {
    const list = [frm('main()', [['N', `${n}x${n}`], ['clues', String(initialClueCount)]])];
    for (const f of callStack) list.push(frm(f.name, [...f.args]));
    if (extra.length && list.length > 0) {
      const top = list[list.length - 1];
      top.rows = [...top.rows, ...extra];
    }
    return list;
  }

  function push(step) {
    if (steps.length < MAX_STEPS) steps.push(step);
  }

  // ── Phase 1: User Input & Allocation (main()) ──────────────────────────────
  push({
    phase: 'input',
    code: 'm_scanner',
    badge: 'Scanner sc = new Scanner(System.in); → Initializing input stream in main().',
    vars: [frm('main()', [['board', 'null']])],
    board: boardSnap(),
    givenMask,
    n,
    boxSize,
    curR: -1,
    curC: -1,
    curX: -1,
    curNum: null,
    candidates: [],
    trayAction: null,
    safety: null,
    backtrackCount: 0,
    solved: false,
    depth: 0
  });

  push({
    phase: 'input',
    code: 'm_alloc_board',
    badge: `int[][] board = new int[${n}][${n}]; → Memory allocated for ${n}x${n} matrix (${n * n} cells). Default initialized to [EMPTY: 0].`,
    vars: [frm('main()', [['board', `new int[${n}][${n}]`]])],
    board: boardSnap(),
    givenMask,
    n,
    boxSize,
    curR: -1,
    curC: -1,
    curX: -1,
    curNum: null,
    candidates: [],
    trayAction: null,
    safety: null,
    backtrackCount: 0,
    solved: false,
    depth: 0
  });

  // Step through populating initial clues from user input row by row
  for (let r = 0; r < n; r++) {
    const rowClues = [];
    for (let c = 0; c < n; c++) {
      board[r][c] = initialGrid[r][c];
      if (initialGrid[r][c] !== 0) rowClues.push(`(${r},${c})=${initialGrid[r][c]}`);
    }
    push({
      phase: 'input',
      code: 'm_read_grid',
      badge: rowClues.length > 0
        ? `Row ${r} clues loaded: ${rowClues.join(', ')}. Cells locked as immutable [GIVEN].`
        : `Row ${r} loaded: all cells empty [EMPTY: 0].`,
      vars: [frm('main()', [['reading row', String(r)], ['given clues', String(initialClueCount)]])],
      board: boardSnap(),
      givenMask,
      n,
      boxSize,
      curR: r,
      curC: -1,
      curX: -1,
      curNum: null,
      candidates: [],
      trayAction: null,
      safety: null,
      backtrackCount: 0,
      solved: false,
      depth: 0
    });
  }

  push({
    phase: 'input',
    code: 'm_call_solver',
    badge: `boolean solved = solveSudoku(board); → All ${initialClueCount} clues loaded into board. Invoking recursive backtracking solver.`,
    vars: [frm('main()', [['status', 'invoking solver'], ['clues', String(initialClueCount)]])],
    board: boardSnap(),
    givenMask,
    n,
    boxSize,
    curR: -1,
    curC: -1,
    curX: -1,
    curNum: null,
    candidates: [],
    trayAction: null,
    safety: null,
    backtrackCount: 0,
    solved: false,
    depth: 0
  });

  function findEmptyCell() {
    for (let i = 0; i < n; i++) {
      for (let j = 0; j < n; j++) {
        if (board[i][j] === 0) return { r: i, c: j };
      }
    }
    return null;
  }

  // ── Phase 2: Recursive Backtracking Search ─────────────────────────────────
  let backtrackCount = 0;
  let solved = false;

  function simulate(depth) {
    if (steps.length >= MAX_STEPS) return false;

    callStack.push({
      name: `solveSudoku(d=${depth})`,
      args: [['depth', String(depth)]]
    });

    // 1. solveSudoku function entry
    push({
      phase: 'solver',
      code: 'c_solve_entry',
      badge: `solveSudoku(board) → Function entry at depth ${depth}. Initializing row = -1, col = -1, isEmpty = false.`,
      vars: stackSnap([['row', '-1'], ['col', '-1'], ['isEmpty', 'false']]),
      board: boardSnap(),
      givenMask,
      n,
      boxSize,
      curR: -1,
      curC: -1,
      curX: -1,
      curNum: null,
      candidates: [],
      trayAction: null,
      safety: null,
      backtrackCount,
      solved: false,
      depth
    });

    const empty = findEmptyCell();

    // 2. findEmpty loop execution
    push({
      phase: 'solver',
      code: 'c_find_empty',
      badge: empty
        ? `for (int i = 0; i < ${n}; i++) ... if (board[i][j] == 0) → Found empty cell at (${empty.r}, ${empty.c}). row = ${empty.r}; col = ${empty.c}; isEmpty = true;`
        : `for (int i = 0; i < ${n}; i++) ... if (board[i][j] == 0) → No empty cell found. isEmpty = false.`,
      vars: stackSnap(empty ? [['row', String(empty.r)], ['col', String(empty.c)], ['isEmpty', 'true']] : [['isEmpty', 'false']]),
      board: boardSnap(),
      givenMask,
      n,
      boxSize,
      curR: empty ? empty.r : -1,
      curC: empty ? empty.c : -1,
      curX: -1,
      curNum: null,
      candidates: [],
      trayAction: null,
      safety: null,
      backtrackCount,
      solved: false,
      depth
    });

    // 3. Base check
    if (!empty) {
      solved = true;
      push({
        phase: 'solver',
        code: 'c_base_check',
        badge: `if (!isEmpty) return true; → [COMPLETE] !isEmpty is TRUE! All ${n * n} cells satisfied without constraint violations! Returning true.`,
        vars: stackSnap([['!isEmpty', 'true'], ['RESULT', 'SOLVED']]),
        board: boardSnap(),
        givenMask,
        n,
        boxSize,
        curR: -1,
        curC: -1,
        curX: -1,
        curNum: null,
        candidates: [],
        trayAction: null,
        safety: null,
        backtrackCount,
        solved: true,
        depth
      });
      callStack.pop();
      return true;
    }

    push({
      phase: 'solver',
      code: 'c_base_check',
      badge: `if (!isEmpty) → Empty cell found at (${empty.r}, ${empty.c}). Condition !isEmpty is false. Entering digit candidate loop (num = 1 .. ${n}).`,
      vars: stackSnap([['!isEmpty', 'false'], ['empty', `(${empty.r},${empty.c})`]]),
      board: boardSnap(),
      givenMask,
      n,
      boxSize,
      curR: empty.r,
      curC: empty.c,
      curX: -1,
      curNum: null,
      candidates: Array.from({ length: n }, () => 'pending'),
      trayAction: null,
      safety: null,
      backtrackCount,
      solved: false,
      depth
    });

    const candidateStates = Array.from({ length: n }, () => 'pending');

    for (let num = 1; num <= n; num++) {
      if (steps.length >= MAX_STEPS) break;

      candidateStates[num - 1] = 'trying';

      // ── Step: Loop header for candidate digit ──
      push({
        phase: 'solver',
        code: 'c_loop_digits',
        badge: `for (int num = ${num}; num <= ${n}; num++) → Testing candidate digit ${num} for cell (${empty.r}, ${empty.c}).`,
        vars: stackSnap([['row', String(empty.r)], ['col', String(empty.c)], ['num', String(num)]]),
        board: boardSnap(),
        givenMask,
        n,
        boxSize,
        curR: empty.r,
        curC: empty.c,
        curX: -1,
        curNum: num,
        candidates: [...candidateStates],
        trayAction: null,
        safety: null,
        backtrackCount,
        solved: false,
        depth
      });

      if (steps.length >= MAX_STEPS) break;

      // ── Step: Call site before entering isSafe ──
      push({
        phase: 'solver',
        code: 'c_check_safe',
        badge: `if (isSafe(board, ${empty.r}, ${empty.c}, ${num})) → Checking safety of digit ${num} at (${empty.r}, ${empty.c}). Calling isSafe()...`,
        vars: stackSnap([['evaluating', `isSafe(${empty.r},${empty.c},${num})`]]),
        board: boardSnap(),
        givenMask,
        n,
        boxSize,
        curR: empty.r,
        curC: empty.c,
        curX: -1,
        curNum: num,
        candidates: [...candidateStates],
        trayAction: null,
        safety: { status: 'checking', checkingArea: 'init', row: { pass: null }, col: { pass: null }, box: { pass: null } },
        backtrackCount,
        solved: false,
        depth
      });

      if (steps.length >= MAX_STEPS) break;

      // ── Step: Inside isSafe function header ──
      push({
        phase: 'solver',
        code: 'c_safe_fn',
        badge: `isSafe(row=${empty.r}, col=${empty.c}, num=${num}) → Entering isSafe(). Initializing verification loop: for (int x = 0; x < ${n}; x++).`,
        vars: stackSnap([['isSafe', `(${empty.r},${empty.c})`], ['num', String(num)]]),
        board: boardSnap(),
        givenMask,
        n,
        boxSize,
        curR: empty.r,
        curC: empty.c,
        curX: 0,
        curNum: num,
        candidates: [...candidateStates],
        trayAction: null,
        safety: { status: 'checking', checkingArea: 'row', row: { pass: null }, col: { pass: null }, box: { pass: null } },
        backtrackCount,
        solved: false,
        depth
      });

      if (steps.length >= MAX_STEPS) break;

      // ── 1. Row Check Execution ──
      let rowConflictCol = -1;
      for (let x = 0; x < n; x++) {
        if (board[empty.r][x] === num) {
          rowConflictCol = x;
          break;
        }
      }

      if (rowConflictCol !== -1) {
        candidateStates[num - 1] = 'blocked';
        push({
          phase: 'solver',
          code: 'c_safe_row',
          badge: `if (board[${empty.r}][${rowConflictCol}] == ${num}) return false; → [ROW CONFLICT at col ${rowConflictCol}]. Cell (${empty.r}, ${rowConflictCol}) contains duplicate digit ${num}. Row check failed! Returning false.`,
          vars: stackSnap([['x', String(rowConflictCol)], [`board[${empty.r}][${rowConflictCol}]`, String(num)], ['isSafe →', 'false']]),
          board: boardSnap(),
          givenMask,
          n,
          boxSize,
          curR: empty.r,
          curC: empty.c,
          curX: rowConflictCol,
          curNum: num,
          candidates: [...candidateStates],
          trayAction: null,
          safety: {
            status: 'conflict',
            checkingArea: 'row',
            row: { pass: false, conflictCol: rowConflictCol },
            col: { pass: null },
            box: { pass: null },
            conflicts: [[empty.r, rowConflictCol]]
          },
          backtrackCount,
          solved: false,
          depth
        });

        // Back to call site: ifSafe returned false
        push({
          phase: 'solver',
          code: 'c_check_safe',
          badge: `if (isSafe(...)) → FALSE (Row conflict at col ${rowConflictCol}). Skipping if-branch. Advancing to num=${num + 1}.`,
          vars: stackSnap([['if-branch', 'SKIP: row conflict']]),
          board: boardSnap(),
          givenMask,
          n,
          boxSize,
          curR: empty.r,
          curC: empty.c,
          curX: -1,
          curNum: num,
          candidates: [...candidateStates],
          trayAction: null,
          safety: {
            status: 'conflict',
            checkingArea: 'row',
            row: { pass: false, conflictCol: rowConflictCol },
            col: { pass: null },
            box: { pass: null },
            conflicts: [[empty.r, rowConflictCol]]
          },
          backtrackCount,
          solved: false,
          depth
        });

        continue;
      }

      // Row check passed
      push({
        phase: 'solver',
        code: 'c_safe_row',
        badge: `if (board[${empty.r}][x] == ${num}) → Checked all columns x in [0..${n - 1}]. [PASS: NO ROW CONFLICT]. No duplicate ${num} in row ${empty.r}. Proceeding to column check.`,
        vars: stackSnap([['row check', 'PASSED: no duplicates']]),
        board: boardSnap(),
        givenMask,
        n,
        boxSize,
        curR: empty.r,
        curC: empty.c,
        curX: n - 1,
        curNum: num,
        candidates: [...candidateStates],
        trayAction: null,
        safety: {
          status: 'checking',
          checkingArea: 'col',
          row: { pass: true, conflictCol: -1 },
          col: { pass: null },
          box: { pass: null },
          conflicts: []
        },
        backtrackCount,
        solved: false,
        depth
      });

      if (steps.length >= MAX_STEPS) break;

      // ── 2. Column Check Execution ──
      let colConflictRow = -1;
      for (let x = 0; x < n; x++) {
        if (board[x][empty.c] === num) {
          colConflictRow = x;
          break;
        }
      }

      if (colConflictRow !== -1) {
        candidateStates[num - 1] = 'blocked';
        push({
          phase: 'solver',
          code: 'c_safe_col',
          badge: `if (board[${colConflictRow}][${empty.c}] == ${num}) return false; → [COL CONFLICT at row ${colConflictRow}]. Cell (${colConflictRow}, ${empty.c}) contains duplicate digit ${num}. Column check failed! Returning false.`,
          vars: stackSnap([['x', String(colConflictRow)], [`board[${colConflictRow}][${empty.c}]`, String(num)], ['isSafe →', 'false']]),
          board: boardSnap(),
          givenMask,
          n,
          boxSize,
          curR: empty.r,
          curC: empty.c,
          curX: colConflictRow,
          curNum: num,
          candidates: [...candidateStates],
          trayAction: null,
          safety: {
            status: 'conflict',
            checkingArea: 'col',
            row: { pass: true, conflictCol: -1 },
            col: { pass: false, conflictRow: colConflictRow },
            box: { pass: null },
            conflicts: [[colConflictRow, empty.c]]
          },
          backtrackCount,
          solved: false,
          depth
        });

        // Back to call site: ifSafe returned false
        push({
          phase: 'solver',
          code: 'c_check_safe',
          badge: `if (isSafe(...)) → FALSE (Column conflict at row ${colConflictRow}). Skipping if-branch. Advancing to num=${num + 1}.`,
          vars: stackSnap([['if-branch', 'SKIP: col conflict']]),
          board: boardSnap(),
          givenMask,
          n,
          boxSize,
          curR: empty.r,
          curC: empty.c,
          curX: -1,
          curNum: num,
          candidates: [...candidateStates],
          trayAction: null,
          safety: {
            status: 'conflict',
            checkingArea: 'col',
            row: { pass: true, conflictCol: -1 },
            col: { pass: false, conflictRow: colConflictRow },
            box: { pass: null },
            conflicts: [[colConflictRow, empty.c]]
          },
          backtrackCount,
          solved: false,
          depth
        });

        continue;
      }

      // Column check passed
      push({
        phase: 'solver',
        code: 'c_safe_col',
        badge: `if (board[x][${empty.c}] == ${num}) → Checked all rows x in [0..${n - 1}]. [PASS: NO COL CONFLICT]. No duplicate ${num} in column ${empty.c}. Proceeding to ${boxSize}x${boxSize} subgrid check.`,
        vars: stackSnap([['col check', 'PASSED: no duplicates']]),
        board: boardSnap(),
        givenMask,
        n,
        boxSize,
        curR: empty.r,
        curC: empty.c,
        curX: n - 1,
        curNum: num,
        candidates: [...candidateStates],
        trayAction: null,
        safety: {
          status: 'checking',
          checkingArea: 'box',
          row: { pass: true, conflictCol: -1 },
          col: { pass: true, conflictRow: -1 },
          box: { pass: null },
          conflicts: []
        },
        backtrackCount,
        solved: false,
        depth
      });

      if (steps.length >= MAX_STEPS) break;

      // ── 3. Box Check Execution ──
      const startRow = empty.r - (empty.r % boxSize);
      const startCol = empty.c - (empty.c % boxSize);
      let boxConflictR = -1;
      let boxConflictC = -1;

      for (let i = 0; i < boxSize && boxConflictR === -1; i++) {
        for (let j = 0; j < boxSize; j++) {
          if (board[startRow + i][startCol + j] === num) {
            boxConflictR = startRow + i;
            boxConflictC = startCol + j;
            break;
          }
        }
      }

      if (boxConflictR !== -1) {
        candidateStates[num - 1] = 'blocked';
        push({
          phase: 'solver',
          code: 'c_safe_box',
          badge: `if (board[${boxConflictR}][${boxConflictC}] == ${num}) return false; → [BOX CONFLICT at (${boxConflictR}, ${boxConflictC})]. Subgrid contains duplicate ${num}. Box check failed! Returning false.`,
          vars: stackSnap([['box conflict', `(${boxConflictR},${boxConflictC})`], ['isSafe →', 'false']]),
          board: boardSnap(),
          givenMask,
          n,
          boxSize,
          curR: empty.r,
          curC: empty.c,
          curX: -1,
          curNum: num,
          candidates: [...candidateStates],
          trayAction: null,
          safety: {
            status: 'conflict',
            checkingArea: 'box',
            row: { pass: true, conflictCol: -1 },
            col: { pass: true, conflictRow: -1 },
            box: { pass: false, conflictR: boxConflictR, conflictC: boxConflictC, startRow, startCol },
            conflicts: [[boxConflictR, boxConflictC]]
          },
          backtrackCount,
          solved: false,
          depth
        });

        // Back to call site: ifSafe returned false
        push({
          phase: 'solver',
          code: 'c_check_safe',
          badge: `if (isSafe(...)) → FALSE (Box conflict at (${boxConflictR}, ${boxConflictC})). Skipping if-branch. Advancing to num=${num + 1}.`,
          vars: stackSnap([['if-branch', 'SKIP: box conflict']]),
          board: boardSnap(),
          givenMask,
          n,
          boxSize,
          curR: empty.r,
          curC: empty.c,
          curX: -1,
          curNum: num,
          candidates: [...candidateStates],
          trayAction: null,
          safety: {
            status: 'conflict',
            checkingArea: 'box',
            row: { pass: true, conflictCol: -1 },
            col: { pass: true, conflictRow: -1 },
            box: { pass: false, conflictR: boxConflictR, conflictC: boxConflictC, startRow, startCol },
            conflicts: [[boxConflictR, boxConflictC]]
          },
          backtrackCount,
          solved: false,
          depth
        });

        continue;
      }

      // Box check passed
      push({
        phase: 'solver',
        code: 'c_safe_box',
        badge: `for (int i=0; i<${boxSize}; i++) ... → Scanned all cells in ${boxSize}x${boxSize} box starting at (${startRow}, ${startCol}). [PASS: NO BOX CONFLICT].`,
        vars: stackSnap([['box check', 'PASSED: no duplicates']]),
        board: boardSnap(),
        givenMask,
        n,
        boxSize,
        curR: empty.r,
        curC: empty.c,
        curX: -1,
        curNum: num,
        candidates: [...candidateStates],
        trayAction: null,
        safety: {
          status: 'checking',
          checkingArea: 'all_pass',
          row: { pass: true, conflictCol: -1 },
          col: { pass: true, conflictRow: -1 },
          box: { pass: true, conflictR: -1, conflictC: -1, startRow, startCol },
          conflicts: []
        },
        backtrackCount,
        solved: false,
        depth
      });

      if (steps.length >= MAX_STEPS) break;

      // ── 4. isSafe return true ──
      candidateStates[num - 1] = 'accepted';
      push({
        phase: 'solver',
        code: 'c_safe_ret_true',
        badge: `return true; → [VALID CANDIDATE] Candidate digit ${num} satisfies row, column, and ${boxSize}x${boxSize} subgrid checks without duplicates! Returning true to call site.`,
        vars: stackSnap([['isSafe →', 'true'], ['verdict', 'SAFE']]),
        board: boardSnap(),
        givenMask,
        n,
        boxSize,
        curR: empty.r,
        curC: empty.c,
        curX: -1,
        curNum: num,
        candidates: [...candidateStates],
        trayAction: null,
        safety: {
          status: 'valid',
          checkingArea: 'all_pass',
          isSafe: true,
          row: { pass: true, conflictCol: -1 },
          col: { pass: true, conflictRow: -1 },
          box: { pass: true, conflictR: -1, conflictC: -1, startRow, startCol },
          conflicts: []
        },
        backtrackCount,
        solved: false,
        depth
      });

      if (steps.length >= MAX_STEPS) break;

      // ── Step: Decision at Call Site ──
      push({
        phase: 'solver',
        code: 'c_check_safe',
        badge: `if (isSafe(...)) → TRUE. Digit ${num} is safe. Entering if-branch to place candidate ${num} on board.`,
        vars: stackSnap([['if-branch', 'ENTER: board[row][col] = num']]),
        board: boardSnap(),
        givenMask,
        n,
        boxSize,
        curR: empty.r,
        curC: empty.c,
        curX: -1,
        curNum: num,
        candidates: [...candidateStates],
        trayAction: null,
        safety: {
          status: 'valid',
          checkingArea: 'all_pass',
          isSafe: true,
          row: { pass: true, conflictCol: -1 },
          col: { pass: true, conflictRow: -1 },
          box: { pass: true, conflictR: -1, conflictC: -1, startRow, startCol },
          conflicts: []
        },
        backtrackCount,
        solved: false,
        depth
      });

      if (steps.length >= MAX_STEPS) break;

      // ── Step: Place Digit ──
      board[empty.r][empty.c] = num;

      push({
        phase: 'solver',
        code: 'c_place_digit',
        badge: `board[${empty.r}][${empty.c}] = ${num}; → [PLACED: ${num}] Candidate ${num} assigned to cell (${empty.r}, ${empty.c}). (+ PLACED)`,
        vars: stackSnap([[`board[${empty.r}][${empty.c}]`, String(num)]]),
        board: boardSnap(),
        givenMask,
        n,
        boxSize,
        curR: empty.r,
        curC: empty.c,
        curX: -1,
        curNum: num,
        candidates: [...candidateStates],
        trayAction: 'push',
        safety: {
          status: 'valid',
          checkingArea: 'all_pass',
          isSafe: true,
          row: { pass: true, conflictCol: -1 },
          col: { pass: true, conflictRow: -1 },
          box: { pass: true, conflictR: -1, conflictC: -1, startRow, startCol },
          conflicts: []
        },
        backtrackCount,
        solved: false,
        depth,
        justPlaced: true
      });

      if (steps.length >= MAX_STEPS) {
        callStack.pop();
        return false;
      }

      // ── Step: Recurse ──
      push({
        phase: 'solver',
        code: 'c_recurse',
        badge: `if (solveSudoku(board)) → Recursing into next stack frame (depth ${depth + 1}) to solve remaining empty cells.`,
        vars: stackSnap([['recursing', `depth ${depth + 1}`]]),
        board: boardSnap(),
        givenMask,
        n,
        boxSize,
        curR: empty.r,
        curC: empty.c,
        curX: -1,
        curNum: num,
        candidates: [...candidateStates],
        trayAction: null,
        safety: null,
        backtrackCount,
        solved: false,
        depth
      });

      if (simulate(depth + 1)) {
        callStack.pop();
        return true;
      }

      if (steps.length >= MAX_STEPS) {
        callStack.pop();
        return false;
      }

      // ── Step: Backtrack ──
      board[empty.r][empty.c] = 0;
      backtrackCount++;
      candidateStates[num - 1] = 'backtracked';

      push({
        phase: 'solver',
        code: 'c_backtrack',
        badge: `board[${empty.r}][${empty.c}] = 0; → [BACKTRACK] Subtree failed. Erased digit ${num} from (${empty.r}, ${empty.c}), reset to [EMPTY: 0]. (- RESET TO 0). Total backtracks: ${backtrackCount}.`,
        vars: stackSnap([['RESET', `board[${empty.r}][${empty.c}] ← 0`], ['backtracks', String(backtrackCount)]]),
        board: boardSnap(),
        givenMask,
        n,
        boxSize,
        curR: empty.r,
        curC: empty.c,
        curX: -1,
        curNum: num,
        candidates: [...candidateStates],
        trayAction: 'pop',
        safety: null,
        backtrackCount,
        solved: false,
        depth,
        isBacktrack: true
      });
    }

    if (steps.length < MAX_STEPS) {
      push({
        phase: 'solver',
        code: 'c_ret_false',
        badge: `[DEAD END: NO VALID DIGIT IN 1..${n}] All candidate digits 1..${n} failed for cell (${empty.r}, ${empty.c}). Returning false.`,
        vars: stackSnap([['result', 'false'], ['backtracks', String(backtrackCount)]]),
        board: boardSnap(),
        givenMask,
        n,
        boxSize,
        curR: empty.r,
        curC: empty.c,
        curX: -1,
        curNum: null,
        candidates: [...candidateStates],
        trayAction: null,
        safety: null,
        backtrackCount,
        solved: false,
        depth,
        isDeadEnd: true
      });
    }

    callStack.pop();
    return false;
  }

  simulate(0);

  if (steps.length >= MAX_STEPS && steps.length > 0) {
    steps[steps.length - 1].stepsCapped = true;
  }

  // ── Phase 3: Program Termination (main()) ──────────────────────────────────
  if (steps.length < MAX_STEPS) {
    push({
      phase: 'done',
      code: 'm_print_result',
      badge: solved
        ? `[COMPLETE] Sudoku solved successfully! All ${n * n} cells satisfied constraints after ${backtrackCount} backtracks.`
        : `No solution exists for this Sudoku configuration. All candidate branches exhausted.`,
      vars: [frm('main()', [['solved', String(solved)], ['total backtracks', String(backtrackCount)]])],
      board: boardSnap(),
      givenMask,
      n,
      boxSize,
      curR: -1,
      curC: -1,
      curX: -1,
      curNum: null,
      candidates: [],
      trayAction: null,
      safety: null,
      backtrackCount,
      solved,
      depth: 0
    });

    push({
      phase: 'done',
      code: 'm_done',
      badge: `Program execution complete. ${solved ? `Sudoku matrix completely solved on ${n}x${n} grid.` : 'Search finished.'} Total backtracks: ${backtrackCount}.`,
      vars: [frm('main()', [['status', 'finished']])],
      board: boardSnap(),
      givenMask,
      n,
      boxSize,
      curR: -1,
      curC: -1,
      curX: -1,
      curNum: null,
      candidates: [],
      trayAction: null,
      safety: null,
      backtrackCount,
      solved,
      depth: 0
    });
  }

  return steps;
}

// ─── Reactive State ───────────────────────────────────────────────────────────
const selectedPreset = ref('standard');
const lang = ref('java');
const speed = ref(650);
const si = ref(0);
const playing = ref(false);
const vizHeight = ref(430);
const tableHeight = ref(65);
const leftWidth = ref(54);
const rightTab = ref('code');

const stepsData = reactive({ steps: buildSteps('standard') });
const steps = computed(() => stepsData.steps);
const s = computed(() => steps.value[Math.max(0, Math.min(si.value, steps.value.length - 1))] || {});
const codeLines = computed(() => CODES[lang.value] || []);

let playTimer = null;

function onChipsWheel(e) {
  if (e.currentTarget) e.currentTarget.scrollLeft += e.deltaY;
}

function selectPreset(key) {
  selectedPreset.value = key;
  applyPreset();
}

function applyPreset() {
  playing.value = false;
  clearTimeout(playTimer);
  stepsData.steps = buildSteps(selectedPreset.value);
  si.value = 0;
  if (typeof window !== 'undefined') window.scrollTo(0, 0);
}

function stepBy(d) {
  si.value = Math.max(0, Math.min(steps.value.length - 1, si.value + d));
}

function togglePlay() {
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

// Synchronized code scrolling: only scrolls .ll-code-scroll, never window or Slidev page
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
  if (e.key === ' ') {
    e.preventDefault();
    togglePlay();
  }
}

// ─── Computed Display Stats ──────────────────────────────────────────────────
const displayN = computed(() => s.value.n || PRESETS[selectedPreset.value].n);
const displayBoxSize = computed(() => s.value.boxSize || PRESETS[selectedPreset.value].boxSize);
const displayBoard = computed(() => s.value.board || []);
const displayMask = computed(() => s.value.givenMask || []);
const displayBacktracks = computed(() => s.value.backtrackCount || 0);

const filledCount = computed(() => {
  let count = 0;
  const b = displayBoard.value;
  for (let r = 0; r < b.length; r++) {
    for (let c = 0; c < b[r].length; c++) {
      if (b[r][c] !== 0) count++;
    }
  }
  return count;
});

const initialCluesCount = computed(() => {
  let count = 0;
  const m = displayMask.value;
  for (let r = 0; r < m.length; r++) {
    for (let c = 0; c < m[r].length; c++) {
      if (m[r][c]) count++;
    }
  }
  return count;
});

const memBoard = computed(() => {
  const n = displayN.value;
  return `${n}x${n}x4B = ${n * n * 4}B`;
});

const stackDepthStr = computed(() => {
  const d = s.value.depth || 0;
  const n = displayN.value;
  return `${d} / ${n * n} (max)`;
});

// Conflict cell lookup set
const conflictCellMap = computed(() => {
  const map = new Set();
  if (s.value.safety && s.value.safety.conflicts) {
    for (const [r, c] of s.value.safety.conflicts) {
      map.add(`${r},${c}`);
    }
  }
  return map;
});

// ─── Resizer Logic ────────────────────────────────────────────────────────────
const mainRef = ref(null);
const leftColRef = ref(null);
const hResizerRef = ref(null);
const vizResizerRef = ref(null);
const tableResizerRef = ref(null);

function initHResizer() {
  const rsz = hResizerRef.value, main = mainRef.value;
  if (!rsz || !main) return;
  let dragging = false, startX = 0, startW = 0;
  const onDown = e => {
    dragging = true; startX = e.clientX; startW = leftColRef.value.offsetWidth;
    rsz.classList.add('drag'); document.body.style.userSelect = 'none';
  };
  const onMove = e => {
    if (!dragging) return;
    const mainW = main.offsetWidth;
    leftWidth.value = (Math.max(200, Math.min(mainW - 200, startW + e.clientX - startX)) / mainW) * 100;
  };
  const onUp = () => {
    if (!dragging) return;
    dragging = false; rsz.classList.remove('drag'); document.body.style.userSelect = '';
  };
  rsz.addEventListener('mousedown', onDown);
  document.addEventListener('mousemove', onMove);
  document.addEventListener('mouseup', onUp);
  return () => {
    rsz.removeEventListener('mousedown', onDown);
    document.removeEventListener('mousemove', onMove);
    document.removeEventListener('mouseup', onUp);
  };
}

function initVResizer(elRef, valueRef, minH, maxH) {
  const rsz = elRef.value;
  if (!rsz) return;
  let dragging = false, startY = 0, startH = 0;
  const onDown = e => {
    dragging = true; startY = e.clientY; startH = valueRef.value;
    rsz.classList.add('drag'); document.body.style.userSelect = 'none'; e.preventDefault();
  };
  const onMove = e => {
    if (!dragging) return;
    valueRef.value = Math.max(minH, Math.min(maxH, startH + (e.clientY - startY)));
  };
  const onUp = () => {
    if (!dragging) return;
    dragging = false; rsz.classList.remove('drag'); document.body.style.userSelect = '';
  };
  rsz.addEventListener('mousedown', onDown);
  document.addEventListener('mousemove', onMove);
  document.addEventListener('mouseup', onUp);
  return () => {
    rsz.removeEventListener('mousedown', onDown);
    document.removeEventListener('mousemove', onMove);
    document.removeEventListener('mouseup', onUp);
  };
}

let cleanupFns = [];
onMounted(() => {
  document.addEventListener('keydown', onKeydown);
  cleanupFns.push(initHResizer());
  cleanupFns.push(initVResizer(vizResizerRef, vizHeight, 260, 680));
  cleanupFns.push(initVResizer(tableResizerRef, tableHeight, 50, 220));
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
    <!-- Navbar -->
    <div class="navbar">
      <h2 class="navbar-title">{{ topic }} &mdash; {{ subTopic }}</h2>
      <img src="../../assets/logo.png" alt="Logo" />
    </div>

    <div class="slide-body">
      <div class="row-main">
        <div class="ll-root">
          <!-- Toolbar -->
          <div class="ll-toolbar">
            <div class="ll-input-group">
              <label>Preset:</label>
            </div>
            <div class="ll-preset-group">
              <button
                v-for="p in PRESETS"
                :key="p.id"
                class="ll-preset-btn"
                :class="{ active: selectedPreset === p.id }"
                :title="p.description"
                @click="selectPreset(p.id)"
              >
                {{ p.name }}
              </button>
            </div>

            <button class="ll-viz-btn" @click="applyPreset">&#9654; Visualize</button>

            <!-- Navigation Controls -->
            <div class="ll-nav-controls">
              <button class="ll-nav-btn" @click="stepBy(-steps.length)">&#171;</button>
              <button class="ll-nav-btn" @click="stepBy(-1)">&#8249; Prev</button>
              <button class="ll-play-btn" @click="togglePlay">{{ playing ? '\u23F8 Pause' : '\u25B6 Play' }}</button>
              <button class="ll-nav-btn" @click="stepBy(1)">Next &#8250;</button>
              <button class="ll-nav-btn" @click="stepBy(steps.length)">&#187;</button>
            </div>
          </div>

          <div class="ll-main" ref="mainRef">
            <!-- Left Column: Visualizer Area -->
            <div class="ll-left-col" ref="leftColRef" :style="{ width: leftWidth + '%' }">
              <div class="ll-viz-wrap" :style="{ height: vizHeight + 'px' }">
                <div class="ll-perm-area">
                  <!-- Real-Time Status Chips -->
                  <div class="ll-ptrs ll-ptrs-compact" @wheel.passive="onChipsWheel">
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">grid</span><b class="ll-c-blue">{{ displayN }}x{{ displayN }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">active</span><b class="ll-c-orange">{{ s.curR >= 0 ? `(${s.curR},${s.curC})` : '-' }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">x</span><b class="ll-c-purple">{{ s.curX !== undefined && s.curX >= 0 ? s.curX : '-' }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">testing</span><b class="ll-c-purple">{{ s.curNum !== null && s.curNum !== undefined ? s.curNum : '-' }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">filled</span><b class="ll-c-green">{{ filledCount }} / {{ displayN * displayN }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">backtracks</span><b class="ll-c-red">{{ displayBacktracks }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">phase</span><b class="ll-c-orange">{{ s.phase || 'input' }}</b></span>
                    <span class="ll-ptr-chip-inline" v-if="s.solved"><b class="ll-c-green">[COMPLETE]</b></span>
                    <span class="ll-ptr-chip-inline" v-if="s.stepsCapped"><b class="ll-c-red">[STEP LIMIT]</b></span>
                  </div>

                  <!-- BOARD CONTAINER (5 TIERS) -->
                  <div class="ll-board-container">

                    <!-- Tier 1: N x N Sudoku Grid Matrix -->
                    <div class="gp-tier-title">Tier 1 &mdash; Sudoku Grid Matrix <code>int[][] board</code></div>
                    <div class="sdk-board-frame">
                      <!-- Column Headers -->
                      <div class="sdk-col-indices">
                        <div class="sdk-axis-spacer"></div>
                        <div v-for="c in displayN" :key="'col-h-' + c" class="sdk-axis-label">{{ c - 1 }}</div>
                      </div>

                      <!-- Grid Rows -->
                      <div class="sdk-grid-body">
                        <div v-for="(row, rIdx) in displayBoard" :key="'row-' + rIdx" class="sdk-grid-row">
                          <div class="sdk-axis-label sdk-row-label">{{ rIdx }}</div>
                          <div
                            v-for="(val, cIdx) in row"
                            :key="'cell-' + rIdx + '-' + cIdx"
                            class="sdk-cell"
                            :class="{
                              'sdk-cell-given': displayMask[rIdx] && displayMask[rIdx][cIdx],
                              'sdk-cell-empty': val === 0,
                              'sdk-cell-active': rIdx === s.curR && cIdx === s.curC && s.phase !== 'done',
                              'sdk-cell-placed': val !== 0 && !(displayMask[rIdx] && displayMask[rIdx][cIdx]) && !(rIdx === s.curR && cIdx === s.curC && s.isBacktrack),
                              'sdk-cell-conflict': conflictCellMap.has(`${rIdx},${cIdx}`),
                              'sdk-cell-backtrack': s.isBacktrack && rIdx === s.curR && cIdx === s.curC,
                              'sdk-cell-solved': s.solved,
                              'sdk-cell-scan-row': s.safety && (s.safety.checkingArea === 'row' || s.code === 'c_safe_row') && rIdx === s.curR,
                              'sdk-cell-scan-col': s.safety && (s.safety.checkingArea === 'col' || s.code === 'c_safe_col') && cIdx === s.curC,
                              'sdk-cell-scan-box': s.safety && (s.safety.checkingArea === 'box' || s.code === 'c_safe_box') && s.safety.box && rIdx >= s.safety.box.startRow && rIdx < s.safety.box.startRow + displayBoxSize && cIdx >= s.safety.box.startCol && cIdx < s.safety.box.startCol + displayBoxSize,
                              'sdk-b-right': (cIdx + 1) % displayBoxSize === 0 && cIdx + 1 < displayN,
                              'sdk-b-bottom': (rIdx + 1) % displayBoxSize === 0 && rIdx + 1 < displayN,
                              'sdk-mini-cell': displayN === 4
                            }"
                            :title="`board[${rIdx}][${cIdx}] = ${val}${displayMask[rIdx] && displayMask[rIdx][cIdx] ? ' [GIVEN]' : ''}`"
                          >
                            <span v-if="val === 0" class="sdk-zero">0</span>
                            <span v-else class="sdk-num">{{ val }}</span>

                            <span v-if="displayMask[rIdx] && displayMask[rIdx][cIdx]" class="sdk-badge-given">[GIVEN]</span>
                            <span v-else-if="rIdx === s.curR && cIdx === s.curC && s.phase !== 'done'" class="sdk-badge-active">[ACTIVE]</span>
                            <span v-else-if="s.isBacktrack && rIdx === s.curR && cIdx === s.curC" class="sdk-badge-bt">[RESET: 0]</span>
                            <span v-else-if="conflictCellMap.has(`${rIdx},${cIdx}`)" class="sdk-badge-conflict">[CONFLICT]</span>
                          </div>
                        </div>
                      </div>
                    </div>

                    <!-- Tier 2: 3-Way Safety Validation Inspector -->
                    <div class="gp-tier-title">Tier 2 &mdash; 3-Way Safety Inspector <code>isSafe(board, {{ s.curR >= 0 ? s.curR : 'r' }}, {{ s.curC >= 0 ? s.curC : 'c' }}, {{ s.curNum || 'd' }})</code></div>
                    <div class="sdk-inspect-panel">
                      <div class="sdk-inspect-grid">
                        <!-- Row Check -->
                        <div class="sdk-check-card" :class="{
                          'sdk-card-pass': s.safety && s.safety.row && s.safety.row.pass === true,
                          'sdk-card-fail': s.safety && s.safety.row && s.safety.row.pass === false,
                          'sdk-card-checking': s.safety && s.safety.checkingArea === 'row'
                        }">
                          <div class="sdk-check-hdr">
                            <span class="sdk-check-title">1. Row Check</span>
                            <span v-if="s.curR >= 0" class="sdk-check-coord">Row {{ s.curR }}</span>
                          </div>
                          <div class="sdk-check-body">
                            <div class="sdk-check-code">board[{{ s.curR >= 0 ? s.curR : 'r' }}][x] == {{ s.curNum || 'd' }}</div>
                            <div v-if="s.safety && s.safety.row && s.safety.row.pass === true" class="sdk-status-badge sdk-badge-pass">[PASS: NO CONFLICT]</div>
                            <div v-else-if="s.safety && s.safety.row && s.safety.row.pass === false" class="sdk-status-badge sdk-badge-fail">[ROW CONFLICT at col {{ s.safety.row.conflictCol }}]</div>
                            <div v-else-if="s.safety && s.safety.checkingArea === 'row'" class="sdk-status-badge sdk-badge-checking">[CHECKING...]</div>
                            <div v-else class="sdk-status-badge sdk-badge-idle">&mdash;</div>
                          </div>
                        </div>

                        <!-- Column Check -->
                        <div class="sdk-check-card" :class="{
                          'sdk-card-pass': s.safety && s.safety.col && s.safety.col.pass === true,
                          'sdk-card-fail': s.safety && s.safety.col && s.safety.col.pass === false,
                          'sdk-card-checking': s.safety && s.safety.checkingArea === 'col'
                        }">
                          <div class="sdk-check-hdr">
                            <span class="sdk-check-title">2. Column Check</span>
                            <span v-if="s.curC >= 0" class="sdk-check-coord">Col {{ s.curC }}</span>
                          </div>
                          <div class="sdk-check-body">
                            <div class="sdk-check-code">board[x][{{ s.curC >= 0 ? s.curC : 'c' }}] == {{ s.curNum || 'd' }}</div>
                            <div v-if="s.safety && s.safety.col && s.safety.col.pass === true" class="sdk-status-badge sdk-badge-pass">[PASS: NO CONFLICT]</div>
                            <div v-else-if="s.safety && s.safety.col && s.safety.col.pass === false" class="sdk-status-badge sdk-badge-fail">[COL CONFLICT at row {{ s.safety.col.conflictRow }}]</div>
                            <div v-else-if="s.safety && s.safety.checkingArea === 'col'" class="sdk-status-badge sdk-badge-checking">[CHECKING...]</div>
                            <div v-else class="sdk-status-badge sdk-badge-idle">&mdash;</div>
                          </div>
                        </div>

                        <!-- Box Check -->
                        <div class="sdk-check-card" :class="{
                          'sdk-card-pass': s.safety && s.safety.box && s.safety.box.pass === true,
                          'sdk-card-fail': s.safety && s.safety.box && s.safety.box.pass === false,
                          'sdk-card-checking': s.safety && s.safety.checkingArea === 'box'
                        }">
                          <div class="sdk-check-hdr">
                            <span class="sdk-check-title">3. {{ displayBoxSize }}x{{ displayBoxSize }} Box Check</span>
                            <span v-if="s.safety && s.safety.box && s.safety.box.startRow !== undefined" class="sdk-check-coord">Box ({{ s.safety.box.startRow }},{{ s.safety.box.startCol }})</span>
                          </div>
                          <div class="sdk-check-body">
                            <div class="sdk-check-code">Subgrid duplicate scan</div>
                            <div v-if="s.safety && s.safety.box && s.safety.box.pass === true" class="sdk-status-badge sdk-badge-pass">[PASS: NO CONFLICT]</div>
                            <div v-else-if="s.safety && s.safety.box && s.safety.box.pass === false" class="sdk-status-badge sdk-badge-fail">[BOX CONFLICT at ({{ s.safety.box.conflictR }},{{ s.safety.box.conflictC }})]</div>
                            <div v-else-if="s.safety && s.safety.checkingArea === 'box'" class="sdk-status-badge sdk-badge-checking">[CHECKING...]</div>
                            <div v-else class="sdk-status-badge sdk-badge-idle">&mdash;</div>
                          </div>
                        </div>
                      </div>

                      <!-- Overall Safety Verdict Banner -->
                      <div class="sdk-verdict-bar" :class="{
                        'sdk-verdict-safe': s.safety && s.safety.status === 'valid',
                        'sdk-verdict-unsafe': s.safety && s.safety.status === 'conflict'
                      }">
                        <span class="sdk-verdict-lbl">Verdict:</span>
                        <span v-if="s.safety && s.safety.status === 'valid'" class="ll-c-green"><b>[VALID CANDIDATE]</b> &mdash; Digit {{ s.curNum }} satisfies row, column, and subgrid constraints.</span>
                        <span v-else-if="s.safety && s.safety.status === 'conflict'" class="ll-c-red"><b>[CONFLICT DETECTED]</b> &mdash; Digit {{ s.curNum }} violates Sudoku uniqueness rules.</span>
                        <span v-else-if="s.safety && s.safety.status === 'checking'" class="ll-c-blue"><b>[EVALUATING CONSTRAINTS]</b> &mdash; Scanning row, column, and subgrid for duplicates.</span>
                        <span v-else class="sdk-verdict-muted">Awaiting safety inspection at active cell.</span>
                      </div>
                    </div>

                    <!-- Tier 3: Digit Candidates Tray (1..N) -->
                    <div class="gp-tier-title">Tier 3 &mdash; Digit Candidates Tray <code>1 &dots; {{ displayN }}</code></div>
                    <div class="sdk-tray-panel">
                      <div class="sdk-tray-meta">
                        <span>Testing Cell: <b class="ll-c-orange">{{ s.curR >= 0 ? `(${s.curR},${s.curC})` : 'none' }}</b></span>
                        <span class="sdk-tray-action" v-if="s.trayAction === 'push'"><b class="ll-c-green">+ PLACED: board[{{ s.curR }}][{{ s.curC }}] = {{ s.curNum }}</b></span>
                        <span class="sdk-tray-action" v-else-if="s.trayAction === 'pop'"><b class="ll-c-red">- RESET TO 0: board[{{ s.curR }}][{{ s.curC }}] = 0</b></span>
                        <span class="sdk-tray-action" v-else><b class="ll-c-blue">Iterating candidate digits</b></span>
                      </div>

                      <div class="sdk-tray-slots">
                        <div
                          v-for="digit in displayN"
                          :key="'digit-slot-' + digit"
                          class="sdk-digit-slot"
                          :class="{
                            'sdk-slot-trying': s.candidates && s.candidates[digit - 1] === 'trying',
                            'sdk-slot-blocked': s.candidates && s.candidates[digit - 1] === 'blocked',
                            'sdk-slot-accepted': s.candidates && s.candidates[digit - 1] === 'accepted',
                            'sdk-slot-backtracked': s.candidates && s.candidates[digit - 1] === 'backtracked'
                          }"
                        >
                          <span class="sdk-slot-num">{{ digit }}</span>
                          <span v-if="s.candidates && s.candidates[digit - 1] === 'trying'" class="sdk-slot-tag tag-trying">[TRYING]</span>
                          <span v-else-if="s.candidates && s.candidates[digit - 1] === 'blocked'" class="sdk-slot-tag tag-blocked">[BLOCKED]</span>
                          <span v-else-if="s.candidates && s.candidates[digit - 1] === 'accepted'" class="sdk-slot-tag tag-accepted">[ACCEPTED]</span>
                          <span v-else-if="s.candidates && s.candidates[digit - 1] === 'backtracked'" class="sdk-slot-tag tag-bt">[POPPED]</span>
                          <span v-else class="sdk-slot-tag tag-idle">[IDLE]</span>
                        </div>
                      </div>
                    </div>

                    <!-- Tier 4: Backtracking & Dead-End Monitor -->
                    <div class="gp-tier-title">Tier 4 &mdash; Backtracking &amp; Dead-End Monitor</div>
                    <div class="sdk-deadend-panel" :class="{
                      'sdk-de-active': s.isDeadEnd || s.isBacktrack,
                      'sdk-de-solved': s.solved
                    }">
                      <template v-if="s.solved">
                        <span class="sdk-de-icon sdk-de-solved-icon">OK</span>
                        <div>
                          <div class="sdk-de-title">[COMPLETE] Sudoku Solved Successfully</div>
                          <div class="sdk-de-detail">All {{ displayN * displayN }} cells satisfied with valid digits. Solved after {{ displayBacktracks }} backtracks.</div>
                        </div>
                      </template>
                      <template v-else-if="s.isDeadEnd">
                        <span class="sdk-de-icon sdk-de-dead-icon">!</span>
                        <div>
                          <div class="sdk-de-title">[DEAD END: NO VALID DIGIT IN 1..{{ displayN }}]</div>
                          <div class="sdk-de-detail">All candidate digits 1..{{ displayN }} failed for cell ({{ s.curR }}, {{ s.curC }}). Returning false to trigger backtrack in parent frame. Total backtracks: {{ displayBacktracks }}.</div>
                        </div>
                      </template>
                      <template v-else-if="s.isBacktrack">
                        <span class="sdk-de-icon sdk-de-bt-icon">BT</span>
                        <div>
                          <div class="sdk-de-title">[BACKTRACKING TO PREVIOUS EMPTY CELL]</div>
                          <div class="sdk-de-detail">board[{{ s.curR }}][{{ s.curC }}] = 0. Erased digit {{ s.curNum }}. Solver retracts stack frame to explore next candidate.</div>
                        </div>
                      </template>
                      <template v-else>
                        <span class="sdk-de-icon sdk-de-idle-icon">--</span>
                        <div>
                          <div class="sdk-de-title">Monitoring Recursion &amp; Constraints</div>
                          <div class="sdk-de-detail">Active search in progress. If all candidate digits 1..{{ displayN }} fail for a cell, solver triggers dead end and backtracks.</div>
                        </div>
                      </template>
                    </div>

                    <!-- Tier 5: Memory & Statistics Readout Strip -->
                    <div class="gp-tier-title">Tier 5 &mdash; Memory &amp; Statistics</div>
                    <div class="gp-mem-bar">
                      <span class="gp-mem-item"><span class="gp-mem-label">Grid Memory:</span> <code>{{ memBoard }}</code></span>
                      <span class="gp-mem-sep">|</span>
                      <span class="gp-mem-item"><span class="gp-mem-label">Filled Cells:</span> <code>{{ filledCount }} / {{ displayN * displayN }}</code></span>
                      <span class="gp-mem-sep">|</span>
                      <span class="gp-mem-item"><span class="gp-mem-label">Initial Clues:</span> <code>{{ initialCluesCount }}</code></span>
                      <span class="gp-mem-sep">|</span>
                      <span class="gp-mem-item"><span class="gp-mem-label">Total Backtracks:</span> <code>{{ displayBacktracks }}</code></span>
                      <span class="gp-mem-sep">|</span>
                      <span class="gp-mem-item"><span class="gp-mem-label">Stack Depth:</span> <code>{{ stackDepthStr }}</code></span>
                    </div>

                  </div>
                  <!-- END ll-board-container -->
                </div>
              </div>

              <!-- Vertical Resizer 1 -->
              <div class="ll-vresizer" ref="vizResizerRef"></div>

              <!-- Legend Container -->
              <div class="ll-legend">
                <span class="ll-leg"><span class="ll-legdot sdk-legdot-given"></span>[GIVEN] Clue</span>
                <span class="ll-leg"><span class="ll-legdot sdk-legdot-empty"></span>[EMPTY: 0]</span>
                <span class="ll-leg"><span class="ll-legdot sdk-legdot-active"></span>[ACTIVE] Current</span>
                <span class="ll-leg"><span class="ll-legdot sdk-legdot-placed"></span>[PLACED] Digit</span>
                <span class="ll-leg"><span class="ll-legdot sdk-legdot-conflict"></span>[CONFLICT] Rule Breach</span>
                <span class="ll-leg"><span class="ll-legdot sdk-legdot-bt"></span>[BACKTRACK]</span>
                <span class="ll-leg"><span class="ll-legdot sdk-legdot-done"></span>[COMPLETE]</span>
              </div>

              <!-- Recursion Call Stack Frame Area -->
              <div class="ll-table-area" :style="{ height: tableHeight + 'px' }">
                <div class="ll-table-title">Recursion Call Stack &mdash; innermost = current depth</div>
                <div class="ll-stack-line">
                  <template v-if="s.vars && s.vars.length">
                    <div
                      v-for="(f, depth) in s.vars"
                      :key="depth"
                      class="ll-frame"
                      :class="{ 'll-frame-cur': depth === s.vars.length - 1 }"
                      :style="{ marginLeft: depth * 12 + 'px' }"
                    >
                      {{ f.title }}(<span v-for="(r, idx) in f.rows" :key="idx"><span v-if="idx > 0">, </span><span class="ll-fname">{{ r[0] }}</span>=<span :class="depth === s.vars.length - 1 ? 'll-c-orange' : 'll-c-blue'" style="font-weight:700">{{ r[1] }}</span></span>)<span v-if="depth === s.vars.length - 1" class="ll-now"> &#9668; active</span>
                    </div>
                  </template>
                  <template v-else>&mdash;</template>
                </div>
              </div>

              <!-- Vertical Resizer 2 -->
              <div class="ll-vresizer" ref="tableResizerRef"></div>

              <!-- Step Badge Banner -->
              <div class="ll-badge-wrap">
                <div
                  class="ll-badge"
                  :class="{
                    'll-badge-error': s.badge && (s.badge.includes('[ROW CONFLICT') || s.badge.includes('[COL CONFLICT') || s.badge.includes('[BOX CONFLICT') || s.badge.includes('[DEAD END') || s.badge.includes('[BACKTRACK]')),
                    'll-badge-success': s.badge && (s.badge.includes('[COMPLETE]') || s.badge.includes('[VALID CANDIDATE]') || s.badge.includes('[PLACED:') || s.badge.includes('solved') || s.badge.includes('[PASS:'))
                  }"
                >
                  {{ s.badge || 'Ready to run Sudoku backtracking solver.' }}
                </div>
              </div>
            </div>

            <!-- Horizontal Resizer -->
            <div class="ll-resizer" ref="hResizerRef"></div>

            <!-- Right Column: Code & Complexity Tabs -->
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

                <!-- Tab 1: Code View -->
                <div v-if="rightTab === 'code'" class="ll-code-scroll" ref="codeScrollRef">
                  <pre class="ll-pre"><span v-for="(line, idx) in codeLines" :key="idx" class="ll-codeline" :class="{ 'll-hl': line[0] && line[0] === s.code }">{{ line[1] === '' ? ' ' : line[1] }}</span></pre>
                </div>

                <!-- Tab 2: Pseudocode -->
                <div v-else-if="rightTab === 'pseudo'" class="ll-code-scroll">
                  <pre class="ll-pre"><span v-for="(line, idx) in PSEUDOCODE" :key="idx" class="ll-codeline">{{ line }}</span></pre>
                </div>

                <!-- Tab 3: Complexity Analysis -->
                <div v-else class="ll-info-scroll">
                  <h3 class="ll-cx-heading">Solve the Sudoku &mdash; Complexity Analysis</h3>
                  <p class="ll-cx-intro">
                    Solve an incomplete Sudoku configuration using recursive backtracking. The algorithm scans for the next empty cell (value 0), evaluates digits 1 through 9 with <code>isSafe()</code>, places valid candidates, and recurses. If a placement leads to a dead end, it backtracks by resetting the cell to 0.
                  </p>

                  <h4 class="ll-cx-sub">Complexity Breakdown</h4>
                  <table class="ll-complexity-table">
                    <thead><tr><th>Operation</th><th>Time</th><th>Space</th><th>Notes</th></tr></thead>
                    <tbody>
                      <tr><td>isSafe check</td><td class="ll-cx-good">O(N) = O(9)</td><td class="ll-cx-good">O(1)</td><td>Scans 1 row (9), 1 column (9), and 1 box (9)</td></tr>
                      <tr><td>findEmptyCell</td><td class="ll-cx-good">O(N&sup2;) = O(81)</td><td class="ll-cx-good">O(1)</td><td>Scans board for next empty cell with value 0</td></tr>
                      <tr><td>Worst-Case DFS</td><td class="ll-cx-bad">O(9<sup>N&sup2;</sup>) = O(9<sup>81</sup>)</td><td class="ll-cx-good">O(N&sup2;) = O(81)</td><td>Worst case tree size; aggressive pruning reduces it drastically in practice</td></tr>
                    </tbody>
                  </table>

                  <h4 class="ll-cx-sub">Overall Complexity</h4>
                  <div class="ll-cx-summary-grid">
                    <div class="ll-cx-card ll-cx-card-bad">
                      <div class="ll-cx-card-label">Time</div>
                      <div class="ll-cx-card-val">O(9<sup>K</sup>)</div>
                      <div class="ll-cx-card-note">K = number of empty cells (at most 81). Rapidly pruned by uniqueness constraints</div>
                    </div>
                    <div class="ll-cx-card ll-cx-card-good">
                      <div class="ll-cx-card-label">Space</div>
                      <div class="ll-cx-card-val">O(N&sup2;) = O(81)</div>
                      <div class="ll-cx-card-note">9&times;9 board matrix (324 B) + call stack depth at most 81 frames</div>
                    </div>
                  </div>

                  <h4 class="ll-cx-sub">Algorithm Invariants</h4>
                  <table class="ll-complexity-table">
                    <thead><tr><th>Rule</th><th>Constraint</th><th>Validation Method</th></tr></thead>
                    <tbody>
                      <tr><td>Row Uniqueness</td><td>Digits 1..9 appear at most once per row</td><td><code>board[row][x] == num</code></td></tr>
                      <tr><td>Column Uniqueness</td><td>Digits 1..9 appear at most once per column</td><td><code>board[x][col] == num</code></td></tr>
                      <tr><td>Subgrid Uniqueness</td><td>Digits 1..9 appear at most once per 3&times;3 box</td><td><code>board[i + startRow][j + startCol] == num</code></td></tr>
                    </tbody>
                  </table>

                  <div class="ll-note">
                    <strong>Teaching Insight:</strong> Pure backtracking solves Sudoku by exhaustive depth-first search.
                    Constraints in Sudoku eliminate 80-95% of branches before recursive descent, demonstrating the immense power of search space pruning over brute force.
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
.ll-root *::-webkit-scrollbar { display: none !important; width: 0 !important; height: 0 !important; }
.ll-root {
  --coral: #F04D4D; --coral-dark: #d93e3e; --coral-light: #fff0f0;
  --bg: #f5f6fa; --surface: #ffffff; --surface2: #f1f4f9;
  --border: #e2e8f0; --border2: #cbd5e1;
  --text: #1e293b; --text2: #475569; --muted: #94a3b8;
  --blue: #3b82f6; --blue-light: #eff6ff;
  --green: #22c55e; --green-light: #f0fdf4;
  --orange: #f97316; --orange-light: #fff7ed;
  --purple: #9333ea; --red: #ef4444; --red-dark: #991b1b; --red-light: #fef2f2;
  --shadow-sm: 0 1px 3px rgba(0,0,0,.08);
  --radius: 8px; --radius-sm: 6px;
  background: var(--bg); color: var(--text);
  font-family: 'Segoe UI', system-ui, sans-serif; font-size: 12.5px;
  display: flex; flex-direction: column; height: 74vh; overflow: hidden; width: 100%;
}
@keyframes ll-pop { 0%{transform:scale(0.7);opacity:0} 70%{transform:scale(1.12)} 100%{transform:scale(1);opacity:1} }
@keyframes sdk-flash-bt { 0%{background:#fef2f2} 50%{background:#ef4444;color:#fff} 100%{background:#fee2e2} }
@keyframes sdk-conflict-pulse { 0%,100%{border-color:#ef4444;background:#fee2e2} 50%{border-color:#b91c1c;background:#fecaca} }
@keyframes sdk-glow-done { 0%,100%{box-shadow:0 0 6px rgba(34,197,94,.3)} 50%{box-shadow:0 0 16px rgba(34,197,94,.8)} }

.slide-wrapper { margin-top:-10px; margin-left:-30px; width:107%; max-height:100%; font-size:0.8rem; }
.slide-body { display:flex; flex-direction:column; border-radius:4px; height:100%; }
.navbar { display:flex; justify-content:space-between; align-items:center; gap:.75rem; padding:0 10px; background:#fff; position:fixed; width:94.7%; z-index:50; }
.navbar > img { height:30px; }
.navbar-title { margin:0; font-size:1.35rem; font-weight:700; background:#ef5050; color:#fff; width:80%; padding:2px 10px; margin-left:-10px; border-radius:5px; }
.row-main { width:100%; height:90%; margin-top:36px; overflow-x:auto; overflow-y:hidden; }

.ll-toolbar { margin-top:4px; display:flex; align-items:center; gap:6px; padding:6.5px 12px; background:var(--surface); border-bottom:1px solid var(--border); flex-shrink:0; flex-wrap:wrap; box-shadow:var(--shadow-sm); }
.ll-input-group { display:flex; align-items:center; gap:4px; }
.ll-input-group label { font-size:11px; color:var(--muted); font-weight:700; }
.ll-preset-group { display:flex; gap:3px; }
.ll-preset-btn { background:var(--surface2); border:1px solid var(--border2); color:var(--text2); padding:3px 8px; border-radius:4px; font-size:10.5px; cursor:pointer; font-family:monospace; font-weight:700; transition:all .12s; }
.ll-preset-btn:hover { background:var(--coral-light); border-color:var(--coral); color:var(--coral-dark); }
.ll-preset-btn.active { background:var(--coral); border-color:var(--coral); color:#fff; }
.ll-viz-btn { background:var(--coral); color:#fff; border:none; padding:5px 12px; border-radius:var(--radius-sm); cursor:pointer; font-size:11.5px; font-weight:600; transition:filter .15s; }
.ll-viz-btn:hover { filter:brightness(1.08); }
.ll-nav-controls { display:flex; margin-left:auto; align-items:center; gap:4px; flex-wrap:wrap; flex-shrink:0; }
.ll-nav-btn { background:var(--surface2); border:1px solid var(--border2); color:var(--text2); padding:4px 9px; border-radius:var(--radius-sm); cursor:pointer; font-size:11px; font-weight:500; transition:all .15s; white-space:nowrap; }
.ll-nav-btn:hover { background:var(--surface); border-color:var(--coral); color:var(--coral); }
.ll-play-btn { background:var(--blue-light); border:1px solid var(--blue); color:var(--blue); min-width:68px; font-weight:600; padding:4px 9px; border-radius:var(--radius-sm); cursor:pointer; font-size:11px; transition:all .15s; }
.ll-play-btn:hover { background:var(--blue); color:#fff; }

.ll-main { display:flex; flex:1; overflow:hidden; position:relative; }
.ll-left-col { display:flex; flex-direction:column; overflow:hidden; min-width:220px; max-width:75%; }
.ll-resizer { width:5px; cursor:col-resize; background:var(--border); flex-shrink:0; transition:background .15s; z-index:20; }
.ll-resizer:hover, .ll-resizer.drag { background:var(--coral); }
.ll-right-col { display:flex; flex-direction:column; flex:1; overflow:hidden; min-width:0; height:100%; }
.ll-viz-wrap { flex-shrink:0; background:var(--surface); border-bottom:1px solid var(--border); overflow:auto; }
.ll-perm-area { display:flex; flex-direction:column; align-items:stretch; min-height:100%; }
.ll-ptrs { display:flex; gap:8px; flex-wrap:wrap; padding:4px 14px; align-items:center; }
.ll-ptrs-compact { flex-wrap:nowrap; gap:6px; padding:3px 14px 5px; overflow-x:auto; }
.ll-ptr-chip-inline { display:inline-flex; align-items:center; gap:4px; background:var(--surface2); border:1px solid var(--border); border-radius:6px; padding:2px 8px; font-size:11px; font-family:monospace; white-space:nowrap; flex-shrink:0; }
.ll-chip-label { color:var(--muted); font-weight:500; margin-right:2px; }
.ll-c-blue{color:var(--blue)} .ll-c-orange{color:var(--orange)} .ll-c-green{color:var(--green)} .ll-c-purple{color:var(--purple)} .ll-c-red{color:var(--red)}

/* Board Container & Tiers */
.ll-board-container { display:flex; flex-direction:column; align-items:flex-start; padding:6px 14px 10px; gap:6px; }
.gp-tier-title { font-size:10px; font-weight:700; text-transform:uppercase; letter-spacing:.04em; color:var(--muted); margin-top:4px; margin-bottom:2px; font-family:'Consolas',monospace; border-left:3px solid var(--coral); padding-left:6px; }

/* Tier 1: Sudoku Board */
.sdk-board-frame { display:flex; flex-direction:column; background:#f8fafc; border:1px solid var(--border); border-radius:var(--radius); padding:8px 12px; box-shadow:var(--shadow-sm); }
.sdk-col-indices { display:flex; margin-bottom:2px; }
.sdk-axis-spacer { width:22px; height:16px; }
.sdk-axis-label { width:32px; height:16px; display:flex; align-items:center; justify-content:center; font-size:10px; font-weight:700; color:var(--muted); font-family:monospace; }
.sdk-row-label { width:22px; height:32px; font-size:10px; font-weight:700; color:var(--muted); font-family:monospace; }
.sdk-grid-body { display:flex; flex-direction:column; border:2.5px solid #1e293b; background:#fff; border-radius:2px; }
.sdk-grid-row { display:flex; align-items:center; }
.sdk-cell {
  position:relative; width:32px; height:32px; display:flex; align-items:center; justify-content:center;
  border-right:1px solid #cbd5e1; border-bottom:1px solid #cbd5e1; background:#ffffff;
  user-select:none; font-family:'Consolas',monospace; transition:all .15s ease;
}
.sdk-mini-cell { width:44px; height:44px; }
.sdk-b-right { border-right:2.5px solid #1e293b !important; }
.sdk-b-bottom { border-bottom:2.5px solid #1e293b !important; }
.sdk-grid-row:last-child .sdk-cell { border-bottom:none !important; }
.sdk-cell:last-child { border-right:none !important; }

.sdk-zero { font-size:10px; color:#cbd5e1; font-weight:600; }
.sdk-num { font-size:13px; font-weight:800; color:#1e293b; }
.sdk-cell-given { background:#f1f5f9 !important; }
.sdk-cell-given .sdk-num { color:#0f172a !important; font-weight:900; }
.sdk-cell-placed { background:#f0fdf4 !important; }
.sdk-cell-placed .sdk-num { color:#16a34a !important; font-weight:800; }
.sdk-cell-active { background:#eff6ff !important; border:2px solid #3b82f6 !important; transform:scale(1.08); z-index:10; animation:ll-pop .18s ease; box-shadow:0 0 6px rgba(59,130,246,.4); }
.sdk-cell-active .sdk-num { color:#2563eb !important; }

/* In-progress inspection scan outlines */
.sdk-cell-scan-row { background:#f0f9ff !important; border-top:1px dashed #38bdf8 !important; border-bottom:1px dashed #38bdf8 !important; }
.sdk-cell-scan-col { background:#f0f9ff !important; border-left:1px dashed #38bdf8 !important; border-right:1px dashed #38bdf8 !important; }
.sdk-cell-scan-box { background:#f8fafc !important; }

.sdk-cell-conflict { animation:sdk-conflict-pulse .4s infinite ease !important; z-index:8; transform:scale(1.04); }
.sdk-cell-conflict .sdk-num { color:#b91c1c !important; font-weight:900; }
.sdk-cell-backtrack { background:#fee2e2 !important; border:2px solid #ef4444 !important; animation:sdk-flash-bt .35s ease; }
.sdk-cell-solved .sdk-num { color:#15803d !important; }

.sdk-badge-given { position:absolute; font-size:6px; font-weight:800; font-family:monospace; background:#64748b; color:#fff; padding:0 2px; border-radius:1.5px; top:1px; left:1px; line-height:1; }
.sdk-badge-active { position:absolute; font-size:6.5px; font-weight:800; font-family:monospace; background:#3b82f6; color:#fff; padding:1px 2px; border-radius:2px; bottom:1px; left:50%; transform:translateX(-50%); white-space:nowrap; line-height:1; }
.sdk-badge-bt { position:absolute; font-size:6px; font-weight:800; font-family:monospace; background:#ef4444; color:#fff; padding:1px 2px; border-radius:2px; bottom:1px; left:50%; transform:translateX(-50%); white-space:nowrap; line-height:1; }
.sdk-badge-conflict { position:absolute; font-size:6px; font-weight:800; font-family:monospace; background:#dc2626; color:#fff; padding:1px 2px; border-radius:2px; bottom:1px; left:50%; transform:translateX(-50%); white-space:nowrap; line-height:1; }

/* Tier 2: 3-Way Safety Inspector */
.sdk-inspect-panel { display:flex; flex-direction:column; gap:5px; background:var(--surface2); border:1px solid var(--border); border-radius:var(--radius-sm); padding:7px 10px; width:100%; max-width:620px; }
.sdk-inspect-grid { display:grid; grid-template-columns:repeat(3, 1fr); gap:6px; }
.sdk-check-card { display:flex; flex-direction:column; padding:6px 8px; border-radius:var(--radius-sm); border:1.5px solid var(--border); background:var(--surface); font-family:monospace; font-size:10px; transition:all .15s; }
.sdk-card-pass { border-color:var(--green) !important; background:#f0fdf4 !important; }
.sdk-card-fail { border-color:var(--red) !important; background:#fef2f2 !important; }
.sdk-card-checking { border-color:var(--blue) !important; background:#eff6ff !important; box-shadow:0 0 5px rgba(59,130,246,0.3); }
.sdk-check-hdr { display:flex; justify-content:space-between; align-items:center; margin-bottom:2px; }
.sdk-check-title { font-weight:800; color:var(--text); font-size:10px; }
.sdk-check-coord { color:var(--muted); font-size:9px; }
.sdk-check-body { display:flex; flex-direction:column; gap:2px; }
.sdk-check-code { font-size:9.5px; color:var(--text2); }
.sdk-status-badge { font-size:8.5px; font-weight:800; padding:1px 4px; border-radius:3px; width:fit-content; white-space:nowrap; }
.sdk-badge-pass { background:#dcfce7; color:#15803d; border:1px solid #86efac; }
.sdk-badge-fail { background:#fee2e2; color:#b91c1c; border:1px solid #fca5a5; }
.sdk-badge-checking { background:#dbeafe; color:#1d4ed8; border:1px solid #93c5fd; }
.sdk-badge-idle { color:var(--muted); }
.sdk-verdict-bar { display:flex; align-items:center; gap:6px; font-size:10.5px; font-family:monospace; padding:3px 6px; border-radius:4px; background:var(--surface); border:1px solid var(--border); }
.sdk-verdict-lbl { font-weight:800; color:var(--muted); }
.sdk-verdict-safe { border-color:var(--green); background:#f0fdf4; }
.sdk-verdict-unsafe { border-color:var(--red); background:#fef2f2; }
.sdk-verdict-muted { color:var(--muted); font-style:italic; }

/* Tier 3: Candidates Tray */
.sdk-tray-panel { display:flex; flex-direction:column; gap:5px; background:var(--surface2); border:1px solid var(--border); border-radius:var(--radius-sm); padding:7px 10px; width:100%; max-width:620px; }
.sdk-tray-meta { display:flex; justify-content:space-between; align-items:center; font-size:10px; font-family:monospace; }
.sdk-tray-slots { display:flex; gap:4px; flex-wrap:wrap; }
.sdk-digit-slot { display:flex; flex-direction:column; align-items:center; justify-content:center; padding:3px 6px; min-width:44px; border-radius:4px; border:1.5px solid var(--border); background:var(--surface); font-family:monospace; transition:all .15s; }
.sdk-slot-num { font-size:12px; font-weight:800; color:var(--text); }
.sdk-slot-tag { font-size:7.5px; font-weight:800; margin-top:1px; white-space:nowrap; }
.tag-trying { color:var(--orange); }
.tag-blocked { color:var(--red); }
.tag-accepted { color:var(--green); }
.tag-bt { color:#b91c1c; }
.tag-idle { color:var(--muted); }
.sdk-slot-trying { border-color:var(--orange) !important; background:var(--orange-light) !important; transform:scale(1.06); }
.sdk-slot-blocked { border-color:var(--red) !important; background:var(--red-light) !important; opacity:0.8; }
.sdk-slot-accepted { border-color:var(--green) !important; background:var(--green-light) !important; transform:scale(1.06); }
.sdk-slot-backtracked { border-color:#fca5a5 !important; background:#fff1f2 !important; }

/* Tier 4: Dead-End Monitor */
.sdk-deadend-panel { display:flex; align-items:center; gap:10px; padding:7px 12px; border-radius:var(--radius-sm); border:1.5px solid var(--border); background:var(--surface2); width:100%; max-width:620px; transition:all .2s; }
.sdk-de-active { border-color:var(--red) !important; background:var(--red-light) !important; }
.sdk-de-solved { border-color:var(--green) !important; background:var(--green-light) !important; animation:sdk-glow-done 1.5s infinite; }
.sdk-de-icon { width:26px; height:26px; border-radius:50%; display:flex; align-items:center; justify-content:center; font-size:10px; font-weight:900; font-family:monospace; flex-shrink:0; border:2px solid currentColor; }
.sdk-de-idle-icon { color:var(--muted); background:var(--surface); }
.sdk-de-dead-icon { color:#b91c1c; background:#fee2e2; }
.sdk-de-bt-icon { color:#c2410c; background:#fff7ed; }
.sdk-de-solved-icon { color:#15803d; background:#dcfce7; }
.sdk-de-title { font-size:11px; font-weight:800; font-family:monospace; color:var(--text); }
.sdk-de-active .sdk-de-title { color:#b91c1c; }
.sdk-de-solved .sdk-de-title { color:#15803d; }
.sdk-de-detail { font-size:10px; color:var(--text2); font-family:monospace; line-height:1.35; }

/* Tier 5: Memory bar */
.gp-mem-bar { display:flex; flex-wrap:wrap; gap:4px 10px; background:#1e293b; color:#94a3b8; font-family:monospace; font-size:10px; padding:5px 10px; border-radius:var(--radius-sm); align-items:center; width:100%; max-width:620px; }
.gp-mem-item { display:flex; align-items:center; gap:4px; }
.gp-mem-label { color:#64748b; font-weight:700; }
.gp-mem-bar code { color:#38bdf8; font-size:10px; }
.gp-mem-sep { color:#334155; font-size:12px; }

/* Resizers, Legend, Call Stack, Badges */
.ll-vresizer { height:5px; cursor:row-resize; background:var(--border); flex-shrink:0; transition:background .15s; z-index:20; }
.ll-vresizer:hover, .ll-vresizer.drag { background:var(--coral); }

.ll-legend { display:flex; flex-wrap:wrap; gap:6px 14px; padding:6px 12px; border-bottom:1px solid var(--border); flex-shrink:0; background:var(--surface2); }
.ll-leg { display:flex; align-items:center; gap:5px; font-size:11px; color:var(--text2); font-weight:500; }
.ll-legdot { width:11px; height:11px; border-radius:3px; flex-shrink:0; display:inline-block; }
.sdk-legdot-given { background:#f1f5f9; border:1.5px solid #64748b; }
.sdk-legdot-empty { background:#ffffff; border:1.5px solid #cbd5e1; }
.sdk-legdot-active { background:#eff6ff; border:1.5px solid #3b82f6; }
.sdk-legdot-placed { background:#f0fdf4; border:1.5px solid #22c55e; }
.sdk-legdot-conflict { background:#fee2e2; border:1.5px solid #ef4444; }
.sdk-legdot-bt { background:#fee2e2; border:1.5px solid #dc2626; }
.sdk-legdot-done { background:#dcfce7; border:1.5px solid var(--green); }

.ll-table-area { flex-shrink:0; padding:8px 14px; border-bottom:1px solid var(--border); overflow:auto; background:var(--surface); min-width:0; box-sizing:border-box; }
.ll-table-title { font-size:10px; color:var(--muted); margin-bottom:4px; font-style:italic; }
.ll-stack-line { font-family:'Consolas',monospace; font-size:12px; line-height:1.8; }
.ll-frame { font-family:'Consolas',monospace; font-size:11.5px; color:var(--text2); padding:1px 0; white-space:nowrap; }
.ll-frame-cur { color:var(--orange); background:var(--orange-light); border-radius:4px; padding:1px 5px; }
.ll-fname { color:var(--text2); }
.ll-now { color:var(--orange); font-size:10px; margin-left:6px; }

.ll-badge-wrap { padding:6px 10px; border-bottom:1px solid var(--border); flex-shrink:0; min-height:36px; display:flex; align-items:center; background:var(--surface); }
.ll-badge { display:inline-block; padding:4px 12px; border-radius:var(--radius-sm); border-left:3px solid var(--coral); background:var(--coral-light); font-size:11px; color:var(--coral-dark); line-height:1.4; word-break:break-word; font-weight:500; }
.ll-badge-error { border-left-color:var(--red) !important; background:var(--red-light) !important; color:var(--red-dark) !important; }
.ll-badge-success { border-left-color:var(--green) !important; background:var(--green-light) !important; color:#15803d !important; }

/* Right Column Panel */
.ll-code-panel { display:flex; flex-direction:column; height:100%; overflow:hidden; }
.ll-code-header { display:flex; align-items:center; gap:6px; padding:5px 12px; background:var(--surface); border-bottom:1px solid var(--border); flex-shrink:0; flex-wrap:wrap; }
.ll-tabbar { display:flex; gap:3px; flex-wrap:wrap; }
.ll-tab-btn { padding:4px 9px; font-size:10.5px; font-weight:600; border:1px solid var(--border2); background:var(--surface2); color:var(--text2); border-radius:var(--radius-sm); cursor:pointer; transition:all .15s; white-space:nowrap; }
.ll-tab-btn:hover { border-color:var(--coral); color:var(--coral); }
.ll-tab-btn.active { background:var(--coral); border-color:var(--coral); color:#fff; }
.ll-lang-select { margin-left:auto; padding:4px 24px 4px 8px; font-size:11px; font-weight:500; border:1px solid var(--border2); border-radius:var(--radius-sm); background:var(--surface2); color:var(--text); cursor:pointer; appearance:none; background-image:url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='10' height='6' viewBox='0 0 10 6'%3E%3Cpath d='M0 0l5 6 5-6z' fill='%2394a3b8'/%3E%3C/svg%3E"); background-repeat:no-repeat; background-position:right 8px center; min-width:95px; }
.ll-lang-select:focus { outline:none; border-color:var(--coral); }
.ll-code-scroll { flex:1; overflow:auto; background:#f8fafc; padding:10px 14px; }
.ll-pre { margin:0; font-family:'Cascadia Code','Fira Code','Consolas',monospace; font-size:11px; line-height:1.5; color:var(--text); white-space:pre; padding-bottom:150px; }
.ll-codeline { display:block; padding:0 14px; margin:0 -14px; }
.ll-hl { background:#dcfce7; color:#15803d; font-weight:600; border-left:3px solid var(--green); border-radius:3px; }
.ll-info-scroll { flex:1; overflow:auto; padding:12px 16px; background:var(--surface); font-size:12px; line-height:1.55; }
.ll-cx-heading { font-size:13px; font-weight:700; color:var(--text); margin:0 0 6px; }
.ll-cx-intro { font-size:10.5px; color:var(--text2); margin:0 0 10px; line-height:1.55; }
.ll-cx-sub { font-size:11px; font-weight:700; color:var(--text2); margin:10px 0 4px; border-bottom:1px solid var(--border); padding-bottom:3px; }
.ll-complexity-table { width:100%; border-collapse:collapse; font-size:10.5px; margin:8px 0; }
.ll-complexity-table th, .ll-complexity-table td { border:1px solid var(--border); padding:4px 8px; text-align:left; }
.ll-complexity-table th { background:var(--surface2); font-weight:700; color:var(--text2); }
.ll-cx-good{color:#15803d;font-weight:700} .ll-cx-bad{color:#b91c1c;font-weight:700}
.ll-cx-summary-grid { display:flex; gap:8px; flex-wrap:wrap; margin:6px 0 10px; }
.ll-cx-card { flex:1; min-width:90px; border-radius:var(--radius-sm); padding:8px 10px; text-align:center; border:1.5px solid var(--border); }
.ll-cx-card-good { background:#f0fdf4; border-color:#86efac; color:#15803d; }
.ll-cx-card-bad { background:#fef2f2; border-color:#fca5a5; color:#b91c1c; }
.ll-cx-card-label { font-size:9px; font-weight:700; text-transform:uppercase; letter-spacing:.05em; opacity:.7; margin-bottom:4px; }
.ll-cx-card-val { font-size:13px; font-weight:800; font-family:monospace; margin-bottom:3px; }
.ll-cx-card-note { font-size:8.5px; opacity:.75; line-height:1.3; }
.ll-note { background:#fefce8; border:1px solid #fef08a; border-left:3px solid #eab308; padding:6px 10px; font-size:10.5px; color:#854d0e; border-radius:0 4px 4px 0; margin-top:10px; margin-bottom:120px; }

.ll-footer { display:flex; align-items:center; justify-content:space-between; padding:4px 12px; background:var(--surface); border-top:1px solid var(--border); font-size:11px; color:var(--muted); font-weight:600; flex-shrink:0; }
.ll-speed-wrap { display:flex; align-items:center; gap:6px; }
.ll-speed-wrap input[type="range"] { width:80px; accent-color:var(--coral); }
</style>
