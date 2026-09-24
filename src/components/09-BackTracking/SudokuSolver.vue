<script setup>
import { ref, reactive, computed, onMounted, onBeforeUnmount, watch, nextTick } from 'vue';

defineProps({
  topic: { type: String, default: 'Backtracking Algorithms' },
  subTopic: { type: String, default: 'Sudoku Solver' }
});

// ─── Puzzle Presets ────────────────────────────────────────────────────────────
const PRESETS = {
  easy: [
    [5,3,0,0,7,0,0,0,0],
    [6,0,0,1,9,5,0,0,0],
    [0,9,8,0,0,0,0,6,0],
    [8,0,0,0,6,0,0,0,3],
    [4,0,0,8,0,3,0,0,1],
    [7,0,0,0,2,0,0,0,6],
    [0,6,0,0,0,0,2,8,0],
    [0,0,0,4,1,9,0,0,5],
    [0,0,0,0,8,0,0,7,9]
  ],
  medium: [
    [0,0,0,2,6,0,7,0,1],
    [6,8,0,0,7,0,0,9,0],
    [1,9,0,0,0,4,5,0,0],
    [8,2,0,1,0,0,0,4,0],
    [0,0,4,6,0,2,9,0,0],
    [0,5,0,0,0,3,0,2,8],
    [0,0,9,3,0,0,0,7,4],
    [0,4,0,0,5,0,0,3,6],
    [7,0,3,0,1,8,0,0,0]
  ],
  hard: [
    [0,0,0,0,0,0,0,0,0],
    [0,0,0,0,0,3,0,8,5],
    [0,0,1,0,2,0,0,0,0],
    [0,0,0,5,0,7,0,0,0],
    [0,0,4,0,0,0,1,0,0],
    [0,9,0,0,0,0,0,0,0],
    [5,0,0,0,0,0,0,7,3],
    [0,0,2,0,1,0,0,0,0],
    [0,0,0,0,4,0,0,0,9]
  ]
};

// ─── Code Definitions ─────────────────────────────────────────────────────────
const CODES = {
  java: [
    ['',              'public class SudokuSolver {'],
    ['',              ''],
    ['c_safe_fn',     '    static boolean isSafe(int[][] board, int row, int col, int num) {'],
    ['c_safe_row',    '        for (int x = 0; x < 9; x++) {'],
    ['c_safe_row_chk','            if (board[row][x] == num) {'],
    ['c_safe_row_ret','                return false;'],
    ['',              '            }'],
    ['',              '        }'],
    ['c_safe_col',    '        for (int x = 0; x < 9; x++) {'],
    ['c_safe_col_chk','            if (board[x][col] == num) {'],
    ['c_safe_col_ret','                return false;'],
    ['',              '            }'],
    ['',              '        }'],
    ['c_safe_box',    '        int startRow = row - row % 3;'],
    ['c_safe_box2',   '        int startCol = col - col % 3;'],
    ['c_safe_box_loop','        for (int i = 0; i < 3; i++) {'],
    ['c_safe_box_loop2','            for (int j = 0; j < 3; j++) {'],
    ['c_safe_box_chk','                if (board[i + startRow][j + startCol] == num) {'],
    ['c_safe_box_ret','                    return false;'],
    ['',              '                }'],
    ['',              '            }'],
    ['',              '        }'],
    ['c_safe_true',   '        return true;'],
    ['',              '    }'],
    ['',              ''],
    ['c_solve_fn',    '    static boolean solveSudoku(int[][] board) {'],
    ['c_row_loop',    '        for (int row = 0; row < 9; row++) {'],
    ['c_col_loop',    '            for (int col = 0; col < 9; col++) {'],
    ['c_empty_check', '                if (board[row][col] == 0) {'],
    ['c_num_loop',    '                    for (int num = 1; num <= 9; num++) {'],
    ['c_if_safe',     '                        if (isSafe(board, row, col, num)) {'],
    ['c_place',       '                            board[row][col] = num;'],
    ['c_recurse',     '                            if (solveSudoku(board)) {'],
    ['c_recurse_ret', '                                return true;'],
    ['',              '                            }'],
    ['c_backtrack',   '                            board[row][col] = 0;'],
    ['',              '                        }'],
    ['',              '                    }'],
    ['c_ret_false',   '                    return false;'],
    ['',              '                }'],
    ['',              '            }'],
    ['',              '        }'],
    ['c_ret_true',    '        return true;'],
    ['',              '    }'],
    ['',              ''],
    ['',              '    public static void main(String[] args) {'],
    ['m_init',        '        int[][] board = /* read puzzle */;'],
    ['m_call',        '        boolean solved = solveSudoku(board);'],
    ['m_print',       '        System.out.println(solved ? "Solved!" : "No solution");'],
    ['',              '    }'],
    ['',              '}']
  ],
  cpp: [
    ['',              '#include <iostream>'],
    ['',              'using namespace std;'],
    ['',              ''],
    ['c_safe_fn',     'bool isSafe(int board[9][9], int row, int col, int num) {'],
    ['c_safe_row',    '    for (int x = 0; x < 9; x++) {'],
    ['c_safe_row_chk','        if (board[row][x] == num) {'],
    ['c_safe_row_ret','            return false;'],
    ['',              '        }'],
    ['',              '    }'],
    ['c_safe_col',    '    for (int x = 0; x < 9; x++) {'],
    ['c_safe_col_chk','        if (board[x][col] == num) {'],
    ['c_safe_col_ret','            return false;'],
    ['',              '        }'],
    ['',              '    }'],
    ['c_safe_box',    '    int startRow = row - row % 3;'],
    ['c_safe_box2',   '    int startCol = col - col % 3;'],
    ['c_safe_box_loop','    for (int i = 0; i < 3; i++) {'],
    ['c_safe_box_loop2','        for (int j = 0; j < 3; j++) {'],
    ['c_safe_box_chk','            if (board[i + startRow][j + startCol] == num) {'],
    ['c_safe_box_ret','                return false;'],
    ['',              '            }'],
    ['',              '        }'],
    ['',              '    }'],
    ['c_safe_true',   '    return true;'],
    ['',              '}'],
    ['',              ''],
    ['c_solve_fn',    'bool solveSudoku(int board[9][9]) {'],
    ['c_row_loop',    '    for (int row = 0; row < 9; row++) {'],
    ['c_col_loop',    '        for (int col = 0; col < 9; col++) {'],
    ['c_empty_check', '            if (board[row][col] == 0) {'],
    ['c_num_loop',    '                for (int num = 1; num <= 9; num++) {'],
    ['c_if_safe',     '                    if (isSafe(board, row, col, num)) {'],
    ['c_place',       '                        board[row][col] = num;'],
    ['c_recurse',     '                        if (solveSudoku(board)) {'],
    ['c_recurse_ret', '                            return true;'],
    ['',              '                        }'],
    ['c_backtrack',   '                        board[row][col] = 0;'],
    ['',              '                    }'],
    ['',              '                }'],
    ['c_ret_false',   '                return false;'],
    ['',              '            }'],
    ['',              '        }'],
    ['',              '    }'],
    ['c_ret_true',    '    return true;'],
    ['',              '}']
  ],
  python: [
    ['c_safe_fn',     'def isSafe(board, row, col, num):'],
    ['c_safe_row',    '    for x in range(9):'],
    ['c_safe_row_chk','        if board[row][x] == num:'],
    ['c_safe_row_ret','            return False'],
    ['c_safe_col',    '    for x in range(9):'],
    ['c_safe_col_chk','        if board[x][col] == num:'],
    ['c_safe_col_ret','            return False'],
    ['c_safe_box',    '    startRow = row - row % 3'],
    ['c_safe_box2',   '    startCol = col - col % 3'],
    ['c_safe_box_loop','    for i in range(3):'],
    ['c_safe_box_loop2','        for j in range(3):'],
    ['c_safe_box_chk','            if board[i + startRow][j + startCol] == num:'],
    ['c_safe_box_ret','                return False'],
    ['c_safe_true',   '    return True'],
    ['',              ''],
    ['c_solve_fn',    'def solveSudoku(board):'],
    ['c_row_loop',    '    for row in range(9):'],
    ['c_col_loop',    '        for col in range(9):'],
    ['c_empty_check', '            if board[row][col] == 0:'],
    ['c_num_loop',    '                for num in range(1, 10):'],
    ['c_if_safe',     '                    if isSafe(board, row, col, num):'],
    ['c_place',       '                        board[row][col] = num'],
    ['c_recurse',     '                        if solveSudoku(board):'],
    ['c_recurse_ret', '                            return True'],
    ['c_backtrack',   '                        board[row][col] = 0'],
    ['c_ret_false',   '                return False'],
    ['c_ret_true',    '    return True']
  ],
  javascript: [
    ['c_safe_fn',     'function isSafe(board, row, col, num) {'],
    ['c_safe_row',    '    for (let x = 0; x < 9; x++) {'],
    ['c_safe_row_chk','        if (board[row][x] === num) {'],
    ['c_safe_row_ret','            return false;'],
    ['',              '        }'],
    ['',              '    }'],
    ['c_safe_col',    '    for (let x = 0; x < 9; x++) {'],
    ['c_safe_col_chk','        if (board[x][col] === num) {'],
    ['c_safe_col_ret','            return false;'],
    ['',              '        }'],
    ['',              '    }'],
    ['c_safe_box',    '    const startRow = row - row % 3;'],
    ['c_safe_box2',   '    const startCol = col - col % 3;'],
    ['c_safe_box_loop','    for (let i = 0; i < 3; i++) {'],
    ['c_safe_box_loop2','        for (let j = 0; j < 3; j++) {'],
    ['c_safe_box_chk','            if (board[i + startRow][j + startCol] === num) {'],
    ['c_safe_box_ret','                return false;'],
    ['',              '            }'],
    ['',              '        }'],
    ['',              '    }'],
    ['c_safe_true',   '    return true;'],
    ['',              '}'],
    ['',              ''],
    ['c_solve_fn',    'function solveSudoku(board) {'],
    ['c_row_loop',    '    for (let row = 0; row < 9; row++) {'],
    ['c_col_loop',    '        for (let col = 0; col < 9; col++) {'],
    ['c_empty_check', '            if (board[row][col] === 0) {'],
    ['c_num_loop',    '                for (let num = 1; num <= 9; num++) {'],
    ['c_if_safe',     '                    if (isSafe(board, row, col, num)) {'],
    ['c_place',       '                        board[row][col] = num;'],
    ['c_recurse',     '                        if (solveSudoku(board)) {'],
    ['c_recurse_ret', '                            return true;'],
    ['',              '                        }'],
    ['c_backtrack',   '                        board[row][col] = 0;'],
    ['',              '                    }'],
    ['',              '                }'],
    ['c_ret_false',   '                return false;'],
    ['',              '            }'],
    ['',              '        }'],
    ['',              '    }'],
    ['c_ret_true',    '    return true;'],
    ['',              '}']
  ],
  c: [
    ['',              '#include <stdio.h>'],
    ['',              '#include <stdbool.h>'],
    ['',              ''],
    ['c_safe_fn',     'bool isSafe(int board[9][9], int row, int col, int num) {'],
    ['c_safe_row',    '    for (int x = 0; x < 9; x++) {'],
    ['c_safe_row_chk','        if (board[row][x] == num) {'],
    ['c_safe_row_ret','            return false;'],
    ['',              '        }'],
    ['',              '    }'],
    ['c_safe_col',    '    for (int x = 0; x < 9; x++) {'],
    ['c_safe_col_chk','        if (board[x][col] == num) {'],
    ['c_safe_col_ret','            return false;'],
    ['',              '        }'],
    ['',              '    }'],
    ['c_safe_box',    '    int startRow = row - row % 3;'],
    ['c_safe_box2',   '    int startCol = col - col % 3;'],
    ['c_safe_box_loop','    for (int i = 0; i < 3; i++) {'],
    ['c_safe_box_loop2','        for (int j = 0; j < 3; j++) {'],
    ['c_safe_box_chk','            if (board[i + startRow][j + startCol] == num) {'],
    ['c_safe_box_ret','                return false;'],
    ['',              '            }'],
    ['',              '        }'],
    ['',              '    }'],
    ['c_safe_true',   '    return true;'],
    ['',              '}'],
    ['',              ''],
    ['c_solve_fn',    'bool solveSudoku(int board[9][9]) {'],
    ['c_row_loop',    '    for (int row = 0; row < 9; row++) {'],
    ['c_col_loop',    '        for (int col = 0; col < 9; col++) {'],
    ['c_empty_check', '            if (board[row][col] == 0) {'],
    ['c_num_loop',    '                for (int num = 1; num <= 9; num++) {'],
    ['c_if_safe',     '                    if (isSafe(board, row, col, num)) {'],
    ['c_place',       '                        board[row][col] = num;'],
    ['c_recurse',     '                        if (solveSudoku(board)) {'],
    ['c_recurse_ret', '                            return true;'],
    ['',              '                        }'],
    ['c_backtrack',   '                        board[row][col] = 0;'],
    ['',              '                    }'],
    ['',              '                }'],
    ['c_ret_false',   '                return false;'],
    ['',              '            }'],
    ['',              '        }'],
    ['',              '    }'],
    ['c_ret_true',    '    return true;'],
    ['',              '}']
  ]
};

const PSEUDOCODE = [
  'function solveSudoku(board):',
  '    for row = 0 to 8:',
  '        for col = 0 to 8:',
  '            if board[row][col] == 0:      // Found empty cell',
  '                for num = 1 to 9:',
  '                    if isSafe(board, row, col, num):',
  '                        board[row][col] = num    // Try placing num',
  '                        if solveSudoku(board):   // Recurse',
  '                            return true',
  '                        board[row][col] = 0      // Backtrack',
  '                return false    // No valid num → dead end',
  '    return true                 // All cells filled',
  '',
  'function isSafe(board, row, col, num):',
  '    for x = 0 to 8:',
  '        if board[row][x] == num: return false  // row conflict',
  '    for x = 0 to 8:',
  '        if board[x][col] == num: return false  // col conflict',
  '    startRow = row - row % 3',
  '    startCol = col - col % 3',
  '    for i = 0 to 2:',
  '        for j = 0 to 2:',
  '            if board[i+startRow][j+startCol] == num: return false',
  '    return true'
];

// ─── Step Builder ──────────────────────────────────────────────────────────────
// KEY FIX: We do NOT push a step for every non-empty cell during scanning.
// Each recursive call pushes: entry + find-empty (1 step) + digit-loop steps.
// isSafe is condensed to 3-5 steps (row result, col result, box result, verdict).
// This keeps total steps manageable so the solver always reaches the answer.
function buildSteps(presetKey) {
  const MAX_STEPS = 25000;
  const steps = [];
  let backtrackCount = 0;
  let solved = false;

  const initialBoard = PRESETS[presetKey].map(r => [...r]);
  const givenMask = initialBoard.map(r => r.map(v => v !== 0));
  const board = initialBoard.map(r => [...r]);

  function boardSnap() { return board.map(r => [...r]); }

  // ── isSafe (pure logic, no steps pushed) ─────────────────────────────────
  function isSafeCheck(row, col, num) {
    for (let x = 0; x < 9; x++) {
      if (board[row][x] === num) { return { safe: false, conflict: 'row', cx: row, cy: x }; }
    }
    for (let x = 0; x < 9; x++) {
      if (board[x][col] === num) { return { safe: false, conflict: 'col', cx: x, cy: col }; }
    }
    const sr = row - row % 3;
    const sc = col - col % 3;
    for (let i = 0; i < 3; i++) {
      for (let j = 0; j < 3; j++) {
        if (board[sr + i][sc + j] === num) {
          return { safe: false, conflict: 'box', cx: sr + i, cy: sc + j };
        }
      }
    }
    return { safe: true, conflict: null, cx: -1, cy: -1 };
  }

  const callStack = [];
  function stackSnap(extra = []) {
    const list = [{ title: 'main()', rows: [['puzzle', presetKey]] }];
    for (const f of callStack) { list.push({ title: f.name, rows: [...f.args] }); }
    if (extra.length && list.length > 0) {
      const top = list[list.length - 1];
      top.rows = [...top.rows, ...extra];
    }
    return list;
  }

  const STEP_CAP_SENTINEL = {}; // unique object — thrown to abort recursion cleanly
  function push(step) {
    if (steps.length >= MAX_STEPS) { throw STEP_CAP_SENTINEL; }
    steps.push(step);
  }

  // ── Phase 1: Init ────────────────────────────────────────────────────────────
  push({
    phase: 'init', code: 'm_init',
    badge: `int[][] board = puzzle; → Loading "${presetKey}" puzzle. Pre-filled cells are locked. Empty cells are 0.`,
    vars: stackSnap(), board: boardSnap(), given: givenMask,
    curRow: -1, curCol: -1, tryNum: -1, safeStatus: null, safePhase: null,
    backtrackCount: 0, solved: false
  });
  push({
    phase: 'init', code: 'm_call',
    badge: `boolean solved = solveSudoku(board); → Launching backtracking solver.`,
    vars: stackSnap(), board: boardSnap(), given: givenMask,
    curRow: -1, curCol: -1, tryNum: -1, safeStatus: null, safePhase: null,
    backtrackCount: 0, solved: false
  });

  // ── Phase 2: Recursive Solver ─────────────────────────────────────────────────
  // Each call finds the first empty cell, then tries digits 1-9.
  // Steps pushed per call:
  //   1. c_solve_fn  – function entry
  //   2. c_row_loop / c_col_loop / c_empty_check – "scanning → found [r][c]" (1-3 steps)
  //   3. c_ret_true  – if no empty cell (base case)
  //   Per digit tried (only at the empty cell):
  //   4. c_num_loop  – "trying num=N"
  //   5. c_if_safe   – "calling isSafe"
  //   6. c_safe_fn   – entering isSafe
  //   7. c_safe_row  – row check result
  //   8. (if conflict) c_safe_row_ret – return false row
  //      OR c_safe_col – col check result
  //   9. (if conflict) c_safe_col_ret – return false col
  //      OR c_safe_box – box check result
  //  10. c_safe_true or c_safe_box_ret – final isSafe result
  //  11. c_if_safe   – branch taken (true/false)
  //  12. c_place     – place digit
  //  13. c_recurse   – about to recurse
  //      (recursive call)
  //  14. c_recurse_ret or c_backtrack
  //  15. c_ret_false – dead end

  function simulate() {
    if (steps.length >= MAX_STEPS) { return false; }

    callStack.push({ name: `solve()`, args: [['depth', String(callStack.length + 1)]] });

    push({
      phase: 'solver', code: 'c_solve_fn',
      badge: `solveSudoku(board) → Entered at depth ${callStack.length}. Scanning for first empty cell (value == 0).`,
      vars: stackSnap(), board: boardSnap(), given: givenMask,
      curRow: -1, curCol: -1, tryNum: -1, safeStatus: null, safePhase: null,
      backtrackCount, solved: false
    });

    // ── Find first empty cell (no per-cell steps for non-empty cells) ──────────
    let emptyRow = -1;
    let emptyCol = -1;
    outer:
    for (let r = 0; r < 9; r++) {
      for (let c = 0; c < 9; c++) {
        if (board[r][c] === 0) {
          emptyRow = r;
          emptyCol = c;
          break outer;
        }
      }
    }

    // ── Base case: no empty cell → solved ─────────────────────────────────────
    if (emptyRow === -1) {
      solved = true;
      push({
        phase: 'solver', code: 'c_ret_true',
        badge: `return true; → [COMPLETE] All 81 cells filled! No empty cell found. Sudoku solved!`,
        vars: stackSnap([['result', 'true']]), board: boardSnap(), given: givenMask,
        curRow: -1, curCol: -1, tryNum: -1, safeStatus: null, safePhase: null,
        backtrackCount, solved: true
      });
      callStack.pop();
      return true;
    }

    // ── Scanning steps (just 3 to show the loop reaching the empty cell) ──────
    push({
      phase: 'solver', code: 'c_row_loop',
      badge: `for (int row = ${emptyRow}; row < 9; row++) → Scanning row ${emptyRow}.`,
      vars: stackSnap([['row', String(emptyRow)]]), board: boardSnap(), given: givenMask,
      curRow: emptyRow, curCol: -1, tryNum: -1, safeStatus: null, safePhase: null,
      backtrackCount, solved: false
    });
    if (steps.length >= MAX_STEPS) { callStack.pop(); return false; }

    push({
      phase: 'solver', code: 'c_col_loop',
      badge: `for (int col = ${emptyCol}; col < 9; col++) → Checking cell [${emptyRow}][${emptyCol}] = 0 (empty).`,
      vars: stackSnap([['row', String(emptyRow)], ['col', String(emptyCol)]]), board: boardSnap(), given: givenMask,
      curRow: emptyRow, curCol: emptyCol, tryNum: -1, safeStatus: null, safePhase: null,
      backtrackCount, solved: false
    });
    if (steps.length >= MAX_STEPS) { callStack.pop(); return false; }

    push({
      phase: 'solver', code: 'c_empty_check',
      badge: `if (board[${emptyRow}][${emptyCol}] == 0) → TRUE ✓ Empty cell found at [${emptyRow}][${emptyCol}]. Trying digits 1–9.`,
      vars: stackSnap([['row', String(emptyRow)], ['col', String(emptyCol)], ['val', '0 (empty)']]),
      board: boardSnap(), given: givenMask,
      curRow: emptyRow, curCol: emptyCol, tryNum: -1, safeStatus: null, safePhase: 'found',
      backtrackCount, solved: false
    });
    if (steps.length >= MAX_STEPS) { callStack.pop(); return false; }

    // ── Digit loop ─────────────────────────────────────────────────────────────
    for (let num = 1; num <= 9; num++) {
      // ── Run isSafe (condensed: 2 steps per digit — call-site + verdict) ─────
      const safeResult = isSafeCheck(emptyRow, emptyCol, num);
      const sr = emptyRow - emptyRow % 3;
      const sc = emptyCol - emptyCol % 3;

      push({
        phase: 'solver', code: 'c_num_loop',
        badge: `for (int num = ${num}; num <= 9; num++) → Trying digit ${num} at [${emptyRow}][${emptyCol}].`,
        vars: stackSnap([['row', String(emptyRow)], ['col', String(emptyCol)], ['num', String(num)]]),
        board: boardSnap(), given: givenMask,
        curRow: emptyRow, curCol: emptyCol, tryNum: num, safeStatus: null, safePhase: null,
        backtrackCount, solved: false
      });

      // Step 1: call-site
      push({
        phase: 'solver', code: 'c_if_safe',
        badge: `if (isSafe(board, ${emptyRow}, ${emptyCol}, ${num})) → Calling isSafe() to validate digit ${num}…`,
        vars: stackSnap([['row', String(emptyRow)], ['col', String(emptyCol)], ['num', String(num)]]),
        board: boardSnap(), given: givenMask,
        curRow: emptyRow, curCol: emptyCol, tryNum: num, safeStatus: null, safePhase: 'checking',
        boxStartRow: sr, boxStartCol: sc,
        backtrackCount, solved: false
      });

      if (!safeResult.safe) {
        // Step 2 (conflict): verdict — show which constraint failed
        const conflictLabels = { row: 'ROW CONFLICT', col: 'COL CONFLICT', box: 'BOX CONFLICT' };
        const conflictPhases = { row: 'check_row_val', col: 'check_col_val', box: 'check_box_val' };
        const conflictStatus = { row: 'row_conflict', col: 'col_conflict', box: 'box_conflict' };
        push({
          phase: 'solver', code: 'c_safe_row_ret',
          badge: `isSafe → false: [${conflictLabels[safeResult.conflict]}] ${num} already exists at [${safeResult.cx}][${safeResult.cy}]. Skipping.`,
          vars: stackSnap([['isSafe →', `false (${safeResult.conflict})`]]), board: boardSnap(), given: givenMask,
          curRow: emptyRow, curCol: emptyCol, tryNum: num,
          safeStatus: conflictStatus[safeResult.conflict],
          safePhase: conflictPhases[safeResult.conflict],
          safeCheckRow: safeResult.cx, safeCheckCol: safeResult.cy,
          boxStartRow: sr, boxStartCol: sc,
          backtrackCount, solved: false
        });
        continue;
      }

      // Step 2 (safe): verdict — all checks passed
      push({
        phase: 'solver', code: 'c_safe_true',
        badge: `isSafe → true: [SAFE] ${num} passes row ✓, col ✓, box ✓ at [${emptyRow}][${emptyCol}]. Placing.`,
        vars: stackSnap([['isSafe →', 'true']]), board: boardSnap(), given: givenMask,
        curRow: emptyRow, curCol: emptyCol, tryNum: num,
        safeStatus: 'safe', safePhase: 'safe_ret',
        boxStartRow: sr, boxStartCol: sc,
        backtrackCount, solved: false
      });

      // ── Place digit ────────────────────────────────────────────────────────
      board[emptyRow][emptyCol] = num;
      push({
        phase: 'solver', code: 'c_place',
        badge: `board[${emptyRow}][${emptyCol}] = ${num}; → [PLACED] digit ${num} placed. Recursing into solveSudoku().`,
        vars: stackSnap([['board[row][col]', String(num)]]), board: boardSnap(), given: givenMask,
        curRow: emptyRow, curCol: emptyCol, tryNum: num,
        safeStatus: 'safe', safePhase: 'placed',
        backtrackCount, solved: false, justPlaced: true
      });

      // ── Recurse ────────────────────────────────────────────────────────────
      push({
        phase: 'solver', code: 'c_recurse',
        badge: `if (solveSudoku(board)) → Recursing deeper with ${num} at [${emptyRow}][${emptyCol}].`,
        vars: stackSnap([['recurse →', `board[${emptyRow}][${emptyCol}]=${num}`]]), board: boardSnap(), given: givenMask,
        curRow: emptyRow, curCol: emptyCol, tryNum: num,
        safeStatus: null, safePhase: null,
        backtrackCount, solved: false
      });

      if (simulate()) {
        push({
          phase: 'solver', code: 'c_recurse_ret',
          badge: `solveSudoku returned TRUE → Propagating success upward. Puzzle solved!`,
          vars: stackSnap([['recursed', 'true']]), board: boardSnap(), given: givenMask,
          curRow: emptyRow, curCol: emptyCol, tryNum: num,
          safeStatus: null, safePhase: null,
          backtrackCount, solved: true
        });
        callStack.pop();
        return true;
      }

      // ── Backtrack ──────────────────────────────────────────────────────────
      board[emptyRow][emptyCol] = 0;
      backtrackCount++;
      push({
        phase: 'solver', code: 'c_backtrack',
        badge: `board[${emptyRow}][${emptyCol}] = 0; → [BACKTRACK] Recursion failed for ${num}. Erasing. Backtracks: ${backtrackCount}.`,
        vars: stackSnap([['ERASE', `board[${emptyRow}][${emptyCol}] ← 0`]]), board: boardSnap(), given: givenMask,
        curRow: emptyRow, curCol: emptyCol, tryNum: num,
        safeStatus: null, safePhase: null,
        backtrackCount, solved: false, isBacktrack: true
      });
    }

    // ── Dead end ───────────────────────────────────────────────────────────────
    push({
      phase: 'solver', code: 'c_ret_false',
      badge: `return false; → [DEAD END] All digits 1–9 failed at [${emptyRow}][${emptyCol}]. Backtracking.`,
      vars: stackSnap([['result', 'false']]), board: boardSnap(), given: givenMask,
      curRow: emptyRow, curCol: emptyCol, tryNum: -1,
      safeStatus: null, safePhase: null,
      backtrackCount, solved: false, isDeadEnd: true
    });

    callStack.pop();
    return false;
  }

  // Wrap simulate() so a STEP_CAP_SENTINEL throw aborts cleanly without
  // being mistaken for a dead-end false return inside the recursion.
  try {
    simulate();
  } catch (e) {
    if (e !== STEP_CAP_SENTINEL) { throw e; }
    if (steps.length > 0) { steps[steps.length - 1].stepsCapped = true; }
  }

  // ── Done ───────────────────────────────────────────────────────────────────
  if (!steps.length || !steps[steps.length - 1].stepsCapped) {
    push({
      phase: 'done', code: 'm_print',
      badge: solved
        ? `System.out.println("Solved!"); → [COMPLETE] Sudoku solved! Total backtracks: ${backtrackCount}.`
        : `System.out.println("No solution"); → No solution exists after ${backtrackCount} backtracks.`,
      vars: [{ title: 'main()', rows: [['solved', String(solved)], ['backtracks', String(backtrackCount)]] }],
      board: boardSnap(), given: givenMask,
      curRow: -1, curCol: -1, tryNum: -1,
      safeStatus: null, safePhase: null, backtrackCount, solved
    });
  }

  return steps;
}

// ─── Reactive State ────────────────────────────────────────────────────────────
const preset = ref('easy');
const lang = ref('java');
const speed = ref(650);
const si = ref(0);
const playing = ref(false);
const vizHeight = ref(250);
const tableHeight = ref(60);
const leftWidth = ref(52);
const rightTab = ref('code');

const stepsData = reactive({ steps: buildSteps('easy') });
const steps = computed(() => stepsData.steps);
const s = computed(() => steps.value[Math.max(0, Math.min(si.value, steps.value.length - 1))] || {});
const codeLines = computed(() => CODES[lang.value] || []);

let playTimer = null;

function onChipsWheel(e) { if (e.currentTarget) { e.currentTarget.scrollLeft += e.deltaY; } }

function applyInput() {
  playing.value = false;
  stepsData.steps = buildSteps(preset.value);
  si.value = 0;
  if (typeof window !== 'undefined') { window.scrollTo(0, 0); }
}

function loadPreset(p) { preset.value = p; applyInput(); }
function stepBy(d) { si.value = Math.max(0, Math.min(steps.value.length - 1, si.value + d)); }

function togglePlay() {
  const next = !playing.value;
  if (next && si.value >= steps.value.length - 1) { si.value = 0; }
  playing.value = next;
}

function tick() {
  clearTimeout(playTimer);
  if (!playing.value) { return; }
  if (si.value >= steps.value.length - 1) { playing.value = false; return; }
  playTimer = setTimeout(() => {
    si.value = Math.min(steps.value.length - 1, si.value + 1);
    tick();
  }, 2100 - speed.value);
}

watch(playing, v => { if (v) { tick(); } else { clearTimeout(playTimer); } });

const codeScrollRef = ref(null);
function scrollActiveCodeLine() {
  nextTick(() => {
    const container = codeScrollRef.value;
    if (!container) { return; }
    const activeEl = container.querySelector('.ll-hl');
    if (!activeEl) { return; }
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
watch(rightTab, v => { if (v === 'code') { scrollActiveCodeLine(); } });

function onKeydown(e) {
  const tag = e.target.tagName;
  if (tag === 'INPUT' || tag === 'SELECT' || tag === 'TEXTAREA') { return; }
  if (e.key === 'ArrowRight') { stepBy(1); }
  if (e.key === 'ArrowLeft') { stepBy(-1); }
  if (e.key === ' ') { e.preventDefault(); togglePlay(); }
}

// Computed display values
const displayBoard = computed(() => s.value.board || []);
const givenMask = computed(() => s.value.given || []);
const displayBacktracks = computed(() => s.value.backtrackCount || 0);

const filledCount = computed(() => {
  const board = displayBoard.value;
  let count = 0;
  for (let r = 0; r < 9; r++) {
    for (let c = 0; c < 9; c++) {
      if (board[r] && board[r][c] !== 0) { count++; }
    }
  }
  return count;
});

const givenCount = computed(() => {
  const mask = givenMask.value;
  let count = 0;
  for (let r = 0; r < 9; r++) {
    for (let c = 0; c < 9; c++) {
      if (mask[r] && mask[r][c]) { count++; }
    }
  }
  return count;
});

const curBoxRow = computed(() => s.value.curRow >= 0 ? Math.floor(s.value.curRow / 3) : -1);
const curBoxCol = computed(() => s.value.curCol >= 0 ? Math.floor(s.value.curCol / 3) : -1);

const mainRef = ref(null);
const leftColRef = ref(null);
const hResizerRef = ref(null);
const vizResizerRef = ref(null);
const tableResizerRef = ref(null);

function initHResizer() {
  const rsz = hResizerRef.value;
  const main = mainRef.value;
  if (!rsz || !main) { return; }
  let dragging = false;
  let startX = 0;
  let startW = 0;
  const onDown = e => {
    dragging = true;
    startX = e.clientX;
    startW = leftColRef.value.offsetWidth;
    rsz.classList.add('drag');
    document.body.style.userSelect = 'none';
  };
  const onMove = e => {
    if (!dragging) { return; }
    const mainW = main.offsetWidth;
    leftWidth.value = (Math.max(200, Math.min(mainW - 200, startW + e.clientX - startX)) / mainW) * 100;
  };
  const onUp = () => {
    if (!dragging) { return; }
    dragging = false;
    rsz.classList.remove('drag');
    document.body.style.userSelect = '';
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
  if (!rsz) { return; }
  let dragging = false;
  let startY = 0;
  let startH = 0;
  const onDown = e => {
    dragging = true;
    startY = e.clientY;
    startH = valueRef.value;
    rsz.classList.add('drag');
    document.body.style.userSelect = 'none';
    e.preventDefault();
  };
  const onMove = e => {
    if (!dragging) { return; }
    valueRef.value = Math.max(minH, Math.min(maxH, startH + (e.clientY - startY)));
  };
  const onUp = () => {
    if (!dragging) { return; }
    dragging = false;
    rsz.classList.remove('drag');
    document.body.style.userSelect = '';
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
  cleanupFns.push(initVResizer(vizResizerRef, vizHeight, 300, 750));
  cleanupFns.push(initVResizer(tableResizerRef, tableHeight, 50, 200));
  if (typeof window !== 'undefined') { window.scrollTo(0, 0); }
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
          <!-- Toolbar -->
          <div class="ll-toolbar">
            <div class="ll-input-group">
              <label>Puzzle:</label>
              <select v-model="preset" class="ll-text-input" style="width:80px" @change="applyInput">
                <option value="medium">Easy</option>
                <option value="easy">Medium</option>
                <option value="hard">Hard</option>
              </select>
            </div>
            <div class="ll-preset-group">
              <button class="ll-preset-btn" @click="loadPreset('medium')" title="Medium difficulty">Easy</button>
              <button class="ll-preset-btn" @click="loadPreset('easy')" title="Classic easy Sudoku">Medium</button>
              <button class="ll-preset-btn" @click="loadPreset('hard')" title="Hard — many backtracks">Hard</button>
            </div>
            <button class="ll-viz-btn" @click="applyInput">&#9654; Visualize</button>
            <div class="ll-nav-controls">
              <button class="ll-nav-btn" @click="stepBy(-steps.length)">&#171;</button>
              <button class="ll-nav-btn" @click="stepBy(-1)">&#8249; Prev</button>
              <button class="ll-play-btn" @click="togglePlay">{{ playing ? '\u23F8 Pause' : '\u25B6 Play' }}</button>
              <button class="ll-nav-btn" @click="stepBy(1)">Next &#8250;</button>
              <button class="ll-nav-btn" @click="stepBy(steps.length)">&#187;</button>
            </div>
          </div>

          <div class="ll-main" ref="mainRef">
            <!-- Left Column -->
            <div class="ll-left-col" ref="leftColRef" :style="{ width: leftWidth + '%' }">
              <div class="ll-viz-wrap" :style="{ height: vizHeight + 'px' }">
                <div class="ll-perm-area">

                  <!-- Stats Chips -->
                  <div class="ll-ptrs ll-ptrs-compact" @wheel.passive="onChipsWheel">
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">puzzle</span><b class="ll-c-blue">{{ preset }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">phase</span><b class="ll-c-orange">{{ s.phase || 'init' }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">cell</span><b class="ll-c-purple">{{ s.curRow >= 0 && s.curCol >= 0 ? `[${s.curRow}][${s.curCol}]` : '—' }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">trying</span><b class="ll-c-orange">{{ s.tryNum > 0 ? s.tryNum : '—' }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">filled</span><b class="ll-c-green">{{ filledCount }} / 81</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">backtracks</span><b class="ll-c-red">{{ displayBacktracks }}</b></span>
                    <span class="ll-ptr-chip-inline" v-if="s.solved"><b class="ll-c-green">[SOLVED]</b></span>
                    <span class="ll-ptr-chip-inline" v-if="s.stepsCapped"><b class="ll-c-red">[STEP LIMIT]</b></span>
                  </div>

                  <!-- BOARD CONTAINER -->
                  <div class="ll-board-container">

                    <!-- Sudoku Grid (no tier label) -->
                    <div class="sd-board-wrap">
                      <div class="sd-board" :class="{ 'sd-board-solved': s.solved }">
                        <template v-for="(row, rIdx) in displayBoard" :key="'row-' + rIdx">
                          <div
                            v-for="(cell, cIdx) in row"
                            :key="'cell-' + rIdx + '-' + cIdx"
                            class="sd-cell"
                            :class="{
                              'sd-cell-given':    givenMask[rIdx] && givenMask[rIdx][cIdx],
                              'sd-cell-filled':   !givenMask[rIdx]?.[cIdx] && cell !== 0,
                              'sd-cell-empty':    cell === 0,
                              'sd-cell-active':   rIdx === s.curRow && cIdx === s.curCol,
                              'sd-cell-backtrack': s.isBacktrack && rIdx === s.curRow && cIdx === s.curCol,
                              'sd-cell-solved':   s.solved && !givenMask[rIdx]?.[cIdx] && cell !== 0,
                              'sd-cell-row-hl':   (s.safePhase === 'check_row' || s.safePhase === 'check_row_val') && rIdx === s.curRow && cIdx !== s.curCol,
                              'sd-cell-col-hl':   (s.safePhase === 'check_col' || s.safePhase === 'check_col_val') && cIdx === s.curCol && rIdx !== s.curRow,
                              'sd-cell-box-hl':   (s.safePhase === 'check_box' || s.safePhase === 'check_box_val' || s.safePhase === 'box_start' || s.safePhase === 'box_start2') && Math.floor(rIdx/3) === curBoxRow && Math.floor(cIdx/3) === curBoxCol && !(rIdx === s.curRow && cIdx === s.curCol),
                              'sd-cell-conflict': s.safeCheckRow !== undefined && s.safeCheckRow >= 0 && rIdx === s.safeCheckRow && cIdx === s.safeCheckCol,
                              'sd-border-right':  cIdx === 2 || cIdx === 5,
                              'sd-border-bottom': rIdx === 2 || rIdx === 5
                            }"
                            :title="`board[${rIdx}][${cIdx}] = ${cell}`"
                          >
                            <span v-if="cell !== 0" class="sd-digit" :class="{ 'sd-digit-given': givenMask[rIdx] && givenMask[rIdx][cIdx] }">{{ cell }}</span>
                            <span v-else class="sd-digit-empty">·</span>
                            <span v-if="rIdx === s.curRow && cIdx === s.curCol && s.tryNum > 0 && cell === 0" class="sd-try-badge">{{ s.tryNum }}?</span>
                          </div>
                        </template>
                      </div>

                      <!-- Board legend -->
                      <div class="sd-board-meta">
                        <span class="sd-meta-item"><span class="sd-meta-dot sd-dot-given"></span>Given</span>
                        <span class="sd-meta-item"><span class="sd-meta-dot sd-dot-placed"></span>Placed</span>
                        <span class="sd-meta-item"><span class="sd-meta-dot sd-dot-active"></span>Current</span>
                        <span class="sd-meta-item"><span class="sd-meta-dot sd-dot-bt"></span>Backtrack</span>
                        <span class="sd-meta-item"><span class="sd-meta-dot sd-dot-row-hl"></span>Row</span>
                        <span class="sd-meta-item"><span class="sd-meta-dot sd-dot-col-hl"></span>Col</span>
                        <span class="sd-meta-item"><span class="sd-meta-dot sd-dot-box-hl"></span>Box</span>
                      </div>
                    </div>



                  </div>
                  <!-- END ll-board-container -->
                </div>
              </div>

              <div class="ll-vresizer" ref="vizResizerRef"></div>

              <!-- Legend -->
              <div class="ll-legend">
                <span class="ll-leg"><span class="ll-legdot sd-legdot-given"></span>[GIVEN]</span>
                <span class="ll-leg"><span class="ll-legdot sd-legdot-placed"></span>[PLACED]</span>
                <span class="ll-leg"><span class="ll-legdot sd-legdot-active"></span>[CURRENT]</span>
                <span class="ll-leg"><span class="ll-legdot sd-legdot-bt"></span>[BACKTRACK]</span>
                <span class="ll-leg"><span class="ll-legdot sd-legdot-row"></span>[ROW]</span>
                <span class="ll-leg"><span class="ll-legdot sd-legdot-col"></span>[COL]</span>
                <span class="ll-leg"><span class="ll-legdot sd-legdot-box"></span>[BOX]</span>
                <span class="ll-leg"><span class="ll-legdot sd-legdot-done"></span>[SOLVED]</span>
              </div>

              <!-- Call Stack -->
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

              <div class="ll-vresizer" ref="tableResizerRef"></div>

              <!-- Step Badge -->
              <div class="ll-badge-wrap">
                <div class="ll-badge" :class="{
                  'll-badge-error':   s.badge && (s.badge.includes('[ROW CONFLICT]') || s.badge.includes('[COL CONFLICT]') || s.badge.includes('[BOX CONFLICT]') || s.badge.includes('[DEAD END]') || s.badge.includes('[BACKTRACK]')),
                  'll-badge-success': s.badge && (s.badge.includes('[COMPLETE]') || s.badge.includes('[SAFE]') || s.badge.includes('[PLACED]') || s.badge.includes('solved'))
                }">
                  {{ s.badge || 'Ready to run Sudoku Solver backtracking algorithm.' }}
                </div>
              </div>
            </div>

            <!-- Horizontal Resizer -->
            <div class="ll-resizer" ref="hResizerRef"></div>

            <!-- Right Column -->
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

                <div v-if="rightTab === 'code'" class="ll-code-scroll" ref="codeScrollRef">
                  <pre class="ll-pre"><span v-for="(line, idx) in codeLines" :key="idx" class="ll-codeline" :class="{ 'll-hl': line[0] && line[0] === s.code }">{{ line[1] === '' ? ' ' : line[1] }}</span></pre>
                </div>
                <div v-else-if="rightTab === 'pseudo'" class="ll-code-scroll">
                  <pre class="ll-pre"><span v-for="(line, idx) in PSEUDOCODE" :key="idx" class="ll-codeline">{{ line }}</span></pre>
                </div>
                <div v-else class="ll-info-scroll">
                  <h3 class="ll-cx-heading">Sudoku Solver &mdash; Complexity Analysis</h3>
                  <p class="ll-cx-intro">
                    Given a 9&times;9 partially filled Sudoku board, fill every empty cell (0) with a digit 1–9 such that each row,
                    column, and 3&times;3 box contains every digit exactly once. Uses backtracking: for each empty cell, try
                    digits 1–9, checking validity with <code>isSafe()</code>. If no digit works, reset cell to 0 and backtrack.
                  </p>
                  <h4 class="ll-cx-sub">Complexity Breakdown</h4>
                  <table class="ll-complexity-table">
                    <thead><tr><th>Operation</th><th>Time</th><th>Space</th><th>Notes</th></tr></thead>
                    <tbody>
                      <tr><td>isSafe() check</td><td class="ll-cx-good">O(1)</td><td class="ll-cx-good">O(1)</td><td>27 cells checked: 9 (row) + 9 (col) + 9 (box)</td></tr>
                      <tr><td>Recursive DFS</td><td class="ll-cx-bad">O(9<sup>81</sup>)</td><td class="ll-cx-good">O(81)</td><td>Worst case: 81 empty cells × 9 choices</td></tr>
                      <tr><td>In practice</td><td class="ll-cx-good">Much faster</td><td class="ll-cx-good">O(81)</td><td>Pruning eliminates most branches early</td></tr>
                    </tbody>
                  </table>
                  <h4 class="ll-cx-sub">Overall Complexity</h4>
                  <div class="ll-cx-summary-grid">
                    <div class="ll-cx-card ll-cx-card-bad">
                      <div class="ll-cx-card-label">Worst-Case Time</div>
                      <div class="ll-cx-card-val">O(9<sup>81</sup>)</div>
                      <div class="ll-cx-card-note">Theoretical; pruning reduces drastically</div>
                    </div>
                    <div class="ll-cx-card ll-cx-card-good">
                      <div class="ll-cx-card-label">Space</div>
                      <div class="ll-cx-card-val">O(81)</div>
                      <div class="ll-cx-card-note">Board + recursion stack ≤ 81 deep</div>
                    </div>
                  </div>
                  <h4 class="ll-cx-sub">Algorithm Steps</h4>
                  <table class="ll-complexity-table">
                    <thead><tr><th>Step</th><th>Description</th></tr></thead>
                    <tbody>
                      <tr><td>1. Scan</td><td>Find first empty cell (value 0) scanning row by row</td></tr>
                      <tr><td>2. Try</td><td>Try digits 1–9 at the empty cell</td></tr>
                      <tr><td>3. Check</td><td>Validate with isSafe(): row, column, 3×3 box</td></tr>
                      <tr><td>4. Place</td><td>If safe, place digit and recurse</td></tr>
                      <tr><td>5. Backtrack</td><td>If recursion fails, reset cell to 0 and try next digit</td></tr>
                      <tr><td>6. Base case</td><td>No empty cell found → puzzle solved, return true</td></tr>
                    </tbody>
                  </table>
                  <div class="ll-note">
                    <strong>Note:</strong> This visualization caps at 8,000 steps for performance.
                    The "hard" puzzle may hit this limit. Easy and Medium puzzles always animate to completion.
                  </div>
                </div>
              </div>
            </div>
          </div>

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
@keyframes sd-flash-bt { 0%{background:#fef2f2} 50%{background:#ef4444;color:#fff} 100%{background:#fee2e2} }
@keyframes sd-glow-done { 0%,100%{box-shadow:0 0 6px rgba(34,197,94,.3)} 50%{box-shadow:0 0 16px rgba(34,197,94,.8)} }

.slide-wrapper { margin-top:-10px; margin-left:-30px; width:107%; max-height:100%; font-size:0.8rem; }
.slide-body { display:flex; flex-direction:column; border-radius:4px; height:100%; }
.navbar { display:flex; justify-content:space-between; align-items:center; gap:.75rem; padding:0 10px; background:#fff; position:fixed; width:94.7%; z-index:50; }
.navbar > img { height:30px; }
.navbar-title { margin:0; font-size:1.35rem; font-weight:700; background:#ef5050; color:#fff; width:80%; padding:2px 10px; margin-left:-10px; border-radius:5px; }
.row-main { width:100%; height:90%; margin-top:36px; overflow-x:auto; overflow-y:hidden; }

.ll-toolbar { margin-top:4px; display:flex; align-items:center; gap:6px; padding:6.5px 12px; background:var(--surface); border-bottom:1px solid var(--border); flex-shrink:0; flex-wrap:wrap; box-shadow:var(--shadow-sm); }
.ll-input-group { display:flex; align-items:center; gap:4px; }
.ll-input-group label { font-size:11px; color:var(--muted); font-weight:700; }
.ll-text-input { background:var(--surface); border:1px solid var(--border2); color:var(--text); border-radius:var(--radius-sm); padding:3px 6px; font-size:11.5px; font-family:monospace; }
.ll-text-input:focus { outline:none; border-color:var(--coral); box-shadow:0 0 0 3px rgba(240,77,77,.1); }
.ll-preset-group { display:flex; gap:3px; }
.ll-preset-btn { background:var(--surface2); border:1px solid var(--border2); color:var(--text2); padding:3px 6px; border-radius:4px; font-size:10.5px; cursor:pointer; font-family:monospace; font-weight:700; transition:all .12s; }
.ll-preset-btn:hover { background:var(--coral-light); border-color:var(--coral); color:var(--coral-dark); }
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

/* ── ll-board-container ───────────────────────────────────────────────────────── */
.ll-board-container { display:flex; flex-direction:column; align-items:flex-start; padding:6px 14px 10px; gap:8px; }

/* ── Sudoku Board ─────────────────────────────────────────────────────────────── */
.sd-board-wrap { display:flex; flex-direction:column; gap:5px; }

/* FIX: use fit-content so all 9 columns are fully visible */
.sd-board {
  display: grid;
  grid-template-columns: repeat(9, 32px);
  gap: 1px;
  background: #334155;
  border: 2.5px solid #334155;
  border-radius: 6px;
  overflow: hidden;
  width: fit-content;
}
.sd-board-solved { background:#15803d !important; border-color:#15803d !important; animation:sd-glow-done 1.5s infinite; }

.sd-cell {
  position: relative;
  width: 32px;
  height: 32px;
  background: #f8fafc;
  display: flex;
  align-items: center;
  justify-content: center;
  font-family: 'Consolas', monospace;
  font-size: 15px;
  font-weight: 700;
  cursor: default;
  transition: background 0.15s, transform 0.12s;
  user-select: none;
}

/* 3×3 box thick separators */
.sd-border-right  { border-right:  2.5px solid #334155; }
.sd-border-bottom { border-bottom: 2.5px solid #334155; }

.sd-cell-given    { background: #e2e8f0; }
.sd-digit-given   { color: #1e293b; font-weight: 900; }

.sd-cell-filled   { background: #eff6ff; }
.sd-digit         { color: #1d4ed8; font-weight: 800; }

.sd-cell-empty    { background: #f8fafc; }
.sd-digit-empty   { color: #cbd5e1; font-size: 18px; line-height: 1; }

.sd-cell-active {
  background: #fef9c3 !important;
  border: 2px solid #f59e0b !important;
  transform: scale(1.1);
  z-index: 10;
  animation: ll-pop 0.2s ease;
}
.sd-cell-backtrack {
  background: #fee2e2 !important;
  border: 2px solid #ef4444 !important;
  animation: sd-flash-bt 0.4s ease;
}
.sd-cell-solved { background: #dcfce7 !important; }
.sd-cell-solved .sd-digit { color: #15803d !important; }

.sd-cell-row-hl  { background: #eff6ff !important; border: 1px solid #93c5fd !important; }
.sd-cell-col-hl  { background: #f0fdf4 !important; border: 1px solid #86efac !important; }
.sd-cell-box-hl  { background: #fef3c7 !important; border: 1px solid #fcd34d !important; }
.sd-cell-conflict { outline: 2px solid var(--red); outline-offset: -1px; background: #fecaca !important; }

.sd-try-badge {
  position: absolute;
  top: 1px; right: 1px;
  font-size: 7px; font-weight: 900; font-family: monospace;
  background: #f97316; color: #fff;
  padding: 1px 2px; border-radius: 2px;
}

.sd-board-meta { display:flex; flex-wrap:wrap; gap:4px 8px; font-size:9px; color:var(--text2); padding:1px 0; }
.sd-meta-item  { display:flex; align-items:center; gap:3px; font-family:monospace; }
.sd-meta-dot   { width:9px; height:9px; border-radius:2px; display:inline-block; flex-shrink:0; }
.sd-dot-given  { background:#e2e8f0; border:1px solid #94a3b8; }
.sd-dot-placed { background:#eff6ff; border:1px solid #93c5fd; }
.sd-dot-active { background:#fef9c3; border:1.5px solid #f59e0b; }
.sd-dot-bt     { background:#fee2e2; border:1px solid var(--red); }
.sd-dot-row-hl { background:#eff6ff; border:1px solid #93c5fd; }
.sd-dot-col-hl { background:#f0fdf4; border:1px solid #86efac; }
.sd-dot-box-hl { background:#fef3c7; border:1px solid #fcd34d; }



/* ── Resizers ─────────────────────────────────────────────────────────────────── */
.ll-vresizer { height:5px; cursor:row-resize; background:var(--border); flex-shrink:0; transition:background .15s; z-index:20; }
.ll-vresizer:hover, .ll-vresizer.drag { background:var(--coral); }

/* ── Legend ──────────────────────────────────────────────────────────────────── */
.ll-legend { display:flex; flex-wrap:wrap; gap:5px 12px; padding:5px 12px; border-bottom:1px solid var(--border); flex-shrink:0; background:var(--surface2); }
.ll-leg    { display:flex; align-items:center; gap:4px; font-size:10.5px; color:var(--text2); font-weight:500; }
.ll-legdot { width:10px; height:10px; border-radius:3px; flex-shrink:0; display:inline-block; }
.sd-legdot-given  { background:#e2e8f0; border:1.5px solid #94a3b8; }
.sd-legdot-placed { background:#eff6ff; border:1.5px solid #93c5fd; }
.sd-legdot-active { background:#fef9c3; border:1.5px solid #f59e0b; }
.sd-legdot-bt     { background:#fee2e2; border:1.5px solid var(--red); }
.sd-legdot-row    { background:#eff6ff; border:1.5px solid #93c5fd; }
.sd-legdot-col    { background:#f0fdf4; border:1.5px solid #86efac; }
.sd-legdot-box    { background:#fef3c7; border:1.5px solid #fcd34d; }
.sd-legdot-done   { background:#dcfce7; border:1.5px solid var(--green); }

/* ── Call Stack ──────────────────────────────────────────────────────────────── */
.ll-table-area  { flex-shrink:0; padding:6px 14px; border-bottom:1px solid var(--border); overflow:auto; background:var(--surface); min-width:0; box-sizing:border-box; }
.ll-table-title { font-size:10px; color:var(--muted); margin-bottom:3px; font-style:italic; }
.ll-stack-line  { font-family:'Consolas',monospace; font-size:12px; line-height:1.8; }
.ll-frame       { font-family:'Consolas',monospace; font-size:11px; color:var(--text2); padding:1px 0; white-space:nowrap; }
.ll-frame-cur   { color:var(--orange); background:var(--orange-light); border-radius:4px; padding:1px 5px; }
.ll-fname { color:var(--text2); }
.ll-now   { color:var(--orange); font-size:10px; margin-left:6px; }

/* ── Step Badge ──────────────────────────────────────────────────────────────── */
.ll-badge-wrap { padding:5px 10px; border-bottom:1px solid var(--border); flex-shrink:0; min-height:34px; display:flex; align-items:center; background:var(--surface); }
.ll-badge { display:inline-block; padding:3px 10px; border-radius:var(--radius-sm); border-left:3px solid var(--coral); background:var(--coral-light); font-size:10.5px; color:var(--coral-dark); line-height:1.4; word-break:break-word; font-weight:500; }
.ll-badge-error   { border-left-color:var(--red)   !important; background:var(--red-light)   !important; color:var(--red-dark) !important; }
.ll-badge-success { border-left-color:var(--green) !important; background:var(--green-light) !important; color:#15803d        !important; }

/* ── Code Panel ──────────────────────────────────────────────────────────────── */
.ll-code-panel  { display:flex; flex-direction:column; height:100%; overflow:hidden; }
.ll-code-header { display:flex; align-items:center; gap:6px; padding:5px 12px; background:var(--surface); border-bottom:1px solid var(--border); flex-shrink:0; flex-wrap:wrap; }
.ll-tabbar      { display:flex; gap:3px; flex-wrap:wrap; }
.ll-tab-btn { padding:4px 9px; font-size:10.5px; font-weight:600; border:1px solid var(--border2); background:var(--surface2); color:var(--text2); border-radius:var(--radius-sm); cursor:pointer; transition:all .15s; white-space:nowrap; }
.ll-tab-btn:hover  { border-color:var(--coral); color:var(--coral); }
.ll-tab-btn.active { background:var(--coral); border-color:var(--coral); color:#fff; }
.ll-lang-select { margin-left:auto; padding:4px 24px 4px 8px; font-size:11px; font-weight:500; border:1px solid var(--border2); border-radius:var(--radius-sm); background:var(--surface2); color:var(--text); cursor:pointer; appearance:none; background-image:url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='10' height='6' viewBox='0 0 10 6'%3E%3Cpath d='M0 0l5 6 5-6z' fill='%2394a3b8'/%3E%3C/svg%3E"); background-repeat:no-repeat; background-position:right 8px center; min-width:95px; }
.ll-lang-select:focus { outline:none; border-color:var(--coral); }
.ll-code-scroll { flex:1; overflow:auto; background:#f8fafc; padding:10px 14px; }
.ll-pre { margin:0; font-family:'Cascadia Code','Fira Code','Consolas',monospace; font-size:11px; line-height:1.5; color:var(--text); white-space:pre; padding-bottom:150px; }
.ll-codeline { display:block; padding:0 14px; margin:0 -14px; }
.ll-hl { background:#dcfce7; color:#15803d; font-weight:600; border-left:3px solid var(--green); border-radius:3px; }
.ll-info-scroll { flex:1; overflow:auto; padding:12px 16px; background:var(--surface); font-size:12px; line-height:1.55; }
.ll-cx-heading { font-size:13px; font-weight:700; color:var(--text); margin:0 0 6px; }
.ll-cx-intro   { font-size:10.5px; color:var(--text2); margin:0 0 10px; line-height:1.55; }
.ll-cx-sub     { font-size:11px; font-weight:700; color:var(--text2); margin:10px 0 4px; border-bottom:1px solid var(--border); padding-bottom:3px; }
.ll-complexity-table { width:100%; border-collapse:collapse; font-size:10.5px; margin:8px 0; }
.ll-complexity-table th, .ll-complexity-table td { border:1px solid var(--border); padding:4px 8px; text-align:left; }
.ll-complexity-table th { background:var(--surface2); font-weight:700; color:var(--text2); }
.ll-cx-good{color:#15803d;font-weight:700} .ll-cx-bad{color:#b91c1c;font-weight:700}
.ll-cx-summary-grid { display:flex; gap:8px; flex-wrap:wrap; margin:6px 0 10px; }
.ll-cx-card { flex:1; min-width:90px; border-radius:var(--radius-sm); padding:8px 10px; text-align:center; border:1.5px solid var(--border); }
.ll-cx-card-good { background:#f0fdf4; border-color:#86efac; color:#15803d; }
.ll-cx-card-bad  { background:#fef2f2; border-color:#fca5a5; color:#b91c1c; }
.ll-cx-card-label { font-size:9px; font-weight:700; text-transform:uppercase; letter-spacing:.05em; opacity:.7; margin-bottom:4px; }
.ll-cx-card-val   { font-size:13px; font-weight:800; font-family:monospace; margin-bottom:3px; }
.ll-cx-card-note  { font-size:8.5px; opacity:.75; line-height:1.3; }
.ll-note { background:#fefce8; border:1px solid #fef08a; border-left:3px solid #eab308; padding:6px 10px; font-size:10.5px; color:#854d0e; border-radius:0 4px 4px 0; margin-top:10px; margin-bottom:120px; }

.ll-footer { display:flex; align-items:center; justify-content:space-between; padding:4px 12px; background:var(--surface); border-top:1px solid var(--border); font-size:11px; color:var(--muted); font-weight:600; flex-shrink:0; }
.ll-speed-wrap { display:flex; align-items:center; gap:6px; }
.ll-speed-wrap input[type="range"] { width:80px; accent-color:var(--coral); }
</style>
