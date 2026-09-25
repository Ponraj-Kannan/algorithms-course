<script setup>
import { ref, reactive, computed, onMounted, onBeforeUnmount, watch, nextTick } from 'vue';

defineProps({
  topic: { type: String, default: 'Backtracking Algorithms' },
  subTopic: { type: String, default: 'Sudoku Solver' }
});

// ─── Sample Sudoku Puzzles ────────────────────────────────────────────────────
const SAMPLES = {
  easy: [
    [5,3,0, 6,7,8, 0,1,2],
    [6,0,2, 0,9,5, 3,0,8],
    [0,9,8, 3,0,2, 0,6,7],
    [8,5,9, 0,6,0, 4,2,3],
    [4,2,0, 8,0,3, 7,9,1],
    [7,0,3, 9,2,0, 8,5,6],
    [9,6,1, 5,0,7, 2,0,4],
    [2,8,0, 4,1,9, 6,3,0],
    [3,0,5, 2,8,0, 1,7,9]
  ],
  medium: [
    [5,3,0, 0,7,8, 9,0,0],
    [0,7,0, 1,0,5, 0,4,0],
    [1,0,8, 0,4,0, 5,0,7],
    [0,5,9, 0,0,1, 4,2,0],
    [4,0,0, 8,5,0, 0,9,1],
    [0,1,3, 0,2,4, 0,0,6],
    [9,0,1, 0,0,7, 2,0,4],
    [0,8,0, 4,1,0, 6,3,0],
    [3,0,5, 2,0,6, 0,7,0]
  ],
  hard: [
    [1,0,0, 4,8,9, 0,0,6],
    [7,3,0, 0,0,0, 0,4,0],
    [0,0,0, 0,0,1, 2,9,5],
    [0,0,7, 1,2,0, 6,0,0],
    [5,0,0, 7,0,3, 0,0,8],
    [0,0,6, 0,9,5, 7,0,0],
    [9,1,4, 6,0,0, 0,0,0],
    [0,2,0, 0,0,0, 0,3,7],
    [8,0,0, 5,1,2, 0,0,4]
  ]
};

// ─── Code Definitions ─────────────────────────────────────────────────────────
const CODES = {
  java: [
    ['',               'public class SudokuSolver {'],
    ['',               ''],
    ['c_isvalid_fn',   '    static boolean isValid(int[][] board, int row, int col, int num) {'],
    ['c_row_check',    '        for (int x = 0; x < 9; x++) {'],
    ['c_row_check_if', '            if (board[row][x] == num) {'],
    ['c_row_ret_f',    '                return false;'],
    ['',               '            }'],
    ['',               '        }'],
    ['c_col_check',    '        for (int x = 0; x < 9; x++) {'],
    ['c_col_check_if', '            if (board[x][col] == num) {'],
    ['c_col_ret_f',    '                return false;'],
    ['',               '            }'],
    ['',               '        }'],
    ['c_box_start',    '        int startRow = row - row % 3;'],
    ['c_box_start_c',  '        int startCol = col - col % 3;'],
    ['c_box_check',    '        for (int i = 0; i < 3; i++) {'],
    ['c_box_j',        '            for (int j = 0; j < 3; j++) {'],
    ['c_box_if',       '                if (board[i + startRow][j + startCol] == num) {'],
    ['c_box_ret_f',    '                    return false;'],
    ['',               '                }'],
    ['',               '            }'],
    ['',               '        }'],
    ['c_isvalid_ret',  '        return true;'],
    ['',               '    }'],
    ['',               ''],
    ['c_solve_fn',     '    static boolean solveSudoku(int[][] board, int row, int col) {'],
    ['c_find_row',     '        for (int row = 0; row < 9; row++) {'],
    ['c_find_col',     '            for (int col = 0; col < 9; col++) {'],
    ['c_check_empty',  '                if (board[row][col] == 0) {'],
    ['c_try_num',      '                    for (int num = 1; num <= 9; num++) {'],
    ['c_call_valid',   '                        if (isValid(board, row, col, num)) {'],
    ['c_place_num',    '                            board[row][col] = num;'],
    ['c_recurse',      '                            if (solveSudoku(board, row, col + 1)) {'],
    ['c_recurse_ret',  '                                return true;'],
    ['',               '                            }'],
    ['c_backtrack',    '                            board[row][col] = 0;'],
    ['',               '                        }'],
    ['',               '                    }'],
    ['c_ret_false',    '                    return false;'],
    ['',               '                }'],
    ['',               '            }'],
    ['',               '        }'],
    ['c_ret_true',     '        return true;'],
    ['',               '    }'],
    ['',               ''],
    ['',               '    public static void main(String[] args) {'],
    ['m_board',        '        int[][] board = { /* puzzle */ };'],
    ['m_call',         '        solveSudoku(board, 0, 0);'],
    ['',               '    }'],
    ['',               '}']
  ],
  cpp: [
    ['',               '#include <iostream>'],
    ['',               'using namespace std;'],
    ['',               ''],
    ['c_isvalid_fn',   'bool isValid(int board[9][9], int row, int col, int num) {'],
    ['c_row_check',    '    for (int x = 0; x < 9; x++) {'],
    ['c_row_check_if', '        if (board[row][x] == num) {'],
    ['c_row_ret_f',    '            return false;'],
    ['',               '        }'],
    ['',               '    }'],
    ['c_col_check',    '    for (int x = 0; x < 9; x++) {'],
    ['c_col_check_if', '        if (board[x][col] == num) {'],
    ['c_col_ret_f',    '            return false;'],
    ['',               '        }'],
    ['',               '    }'],
    ['c_box_start',    '    int startRow = row - row % 3;'],
    ['c_box_start_c',  '    int startCol = col - col % 3;'],
    ['c_box_check',    '    for (int i = 0; i < 3; i++) {'],
    ['c_box_j',        '        for (int j = 0; j < 3; j++) {'],
    ['c_box_if',       '            if (board[i + startRow][j + startCol] == num) {'],
    ['c_box_ret_f',    '                return false;'],
    ['',               '            }'],
    ['',               '        }'],
    ['',               '    }'],
    ['c_isvalid_ret',  '    return true;'],
    ['',               '}'],
    ['',               ''],
    ['c_solve_fn',     'bool solveSudoku(int board[9][9], int row, int col) {'],
    ['c_find_row',     '    for (int row = 0; row < 9; row++) {'],
    ['c_find_col',     '        for (int col = 0; col < 9; col++) {'],
    ['c_check_empty',  '            if (board[row][col] == 0) {'],
    ['c_try_num',      '                for (int num = 1; num <= 9; num++) {'],
    ['c_call_valid',   '                    if (isValid(board, row, col, num)) {'],
    ['c_place_num',    '                        board[row][col] = num;'],
    ['c_recurse',      '                        if (solveSudoku(board, row, col + 1)) {'],
    ['c_recurse_ret',  '                            return true;'],
    ['',               '                        }'],
    ['c_backtrack',    '                        board[row][col] = 0;'],
    ['',               '                    }'],
    ['',               '                }'],
    ['c_ret_false',    '                return false;'],
    ['',               '            }'],
    ['',               '        }'],
    ['',               '    }'],
    ['c_ret_true',     '    return true;'],
    ['',               '}'],
    ['',               ''],
    ['',               'int main() {'],
    ['m_board',        '    int board[9][9] = { /* puzzle */ };'],
    ['m_call',         '    solveSudoku(board, 0, 0);'],
    ['',               '    return 0;'],
    ['',               '}']
  ],
  python: [
    ['c_isvalid_fn',   'def isValid(board, row, col, num):'],
    ['c_row_check',    '    for x in range(9):'],
    ['c_row_check_if', '        if board[row][x] == num:'],
    ['c_row_ret_f',    '            return False'],
    ['c_col_check',    '    for x in range(9):'],
    ['c_col_check_if', '        if board[x][col] == num:'],
    ['c_col_ret_f',    '            return False'],
    ['c_box_start',    '    startRow = row - row % 3'],
    ['c_box_start_c',  '    startCol = col - col % 3'],
    ['c_box_check',    '    for i in range(3):'],
    ['c_box_j',        '        for j in range(3):'],
    ['c_box_if',       '            if board[i + startRow][j + startCol] == num:'],
    ['c_box_ret_f',    '                return False'],
    ['c_isvalid_ret',  '    return True'],
    ['',               ''],
    ['c_solve_fn',     'def solveSudoku(board, row=0, col=0):'],
    ['c_find_row',     '    for row in range(9):'],
    ['c_find_col',     '        for col in range(9):'],
    ['c_check_empty',  '            if board[row][col] == 0:'],
    ['c_try_num',      '                for num in range(1, 10):'],
    ['c_call_valid',   '                    if isValid(board, row, col, num):'],
    ['c_place_num',    '                        board[row][col] = num'],
    ['c_recurse',      '                        if solveSudoku(board, row, col + 1):'],
    ['c_recurse_ret',  '                            return True'],
    ['c_backtrack',    '                        board[row][col] = 0'],
    ['c_ret_false',    '                return False'],
    ['c_ret_true',     '    return True'],
    ['',               ''],
    ['m_board',        'board = [ ... ]'],
    ['m_call',         'solveSudoku(board, 0, 0)']
  ],
  javascript: [
    ['c_isvalid_fn',   'function isValid(board, row, col, num) {'],
    ['c_row_check',    '    for (let x = 0; x < 9; x++) {'],
    ['c_row_check_if', '        if (board[row][x] === num) {'],
    ['c_row_ret_f',    '            return false;'],
    ['',               '        }'],
    ['',               '    }'],
    ['c_col_check',    '    for (let x = 0; x < 9; x++) {'],
    ['c_col_check_if', '        if (board[x][col] === num) {'],
    ['c_col_ret_f',    '            return false;'],
    ['',               '        }'],
    ['',               '    }'],
    ['c_box_start',    '    const startRow = row - row % 3;'],
    ['c_box_start_c',  '    const startCol = col - col % 3;'],
    ['c_box_check',    '    for (let i = 0; i < 3; i++) {'],
    ['c_box_j',        '        for (let j = 0; j < 3; j++) {'],
    ['c_box_if',       '            if (board[i + startRow][j + startCol] === num) {'],
    ['c_box_ret_f',    '                return false;'],
    ['',               '            }'],
    ['',               '        }'],
    ['',               '    }'],
    ['c_isvalid_ret',  '    return true;'],
    ['',               '}'],
    ['',               ''],
    ['c_solve_fn',     'function solveSudoku(board, row = 0, col = 0) {'],
    ['c_find_row',     '    for (let row = 0; row < 9; row++) {'],
    ['c_find_col',     '        for (let col = 0; col < 9; col++) {'],
    ['c_check_empty',  '            if (board[row][col] === 0) {'],
    ['c_try_num',      '                for (let num = 1; num <= 9; num++) {'],
    ['c_call_valid',   '                    if (isValid(board, row, col, num)) {'],
    ['c_place_num',    '                        board[row][col] = num;'],
    ['c_recurse',      '                        if (solveSudoku(board, row, col + 1)) {'],
    ['c_recurse_ret',  '                            return true;'],
    ['',               '                        }'],
    ['c_backtrack',    '                        board[row][col] = 0;'],
    ['',               '                    }'],
    ['',               '                }'],
    ['c_ret_false',    '                return false;'],
    ['',               '            }'],
    ['',               '        }'],
    ['',               '    }'],
    ['c_ret_true',     '    return true;'],
    ['',               '}'],
    ['',               ''],
    ['m_board',        'const board = [ ... ];'],
    ['m_call',         'solveSudoku(board, 0, 0);']
  ],
  c: [
    ['',               '#include <stdio.h>'],
    ['',               ''],
    ['c_isvalid_fn',   'int isValid(int board[9][9], int row, int col, int num) {'],
    ['c_row_check',    '    for (int x = 0; x < 9; x++) {'],
    ['c_row_check_if', '        if (board[row][x] == num) {'],
    ['c_row_ret_f',    '            return 0;'],
    ['',               '        }'],
    ['',               '    }'],
    ['c_col_check',    '    for (int x = 0; x < 9; x++) {'],
    ['c_col_check_if', '        if (board[x][col] == num) {'],
    ['c_col_ret_f',    '            return 0;'],
    ['',               '        }'],
    ['',               '    }'],
    ['c_box_start',    '    int startRow = row - row % 3;'],
    ['c_box_start_c',  '    int startCol = col - col % 3;'],
    ['c_box_check',    '    for (int i = 0; i < 3; i++) {'],
    ['c_box_j',        '        for (int j = 0; j < 3; j++) {'],
    ['c_box_if',       '            if (board[i + startRow][j + startCol] == num) {'],
    ['c_box_ret_f',    '                return 0;'],
    ['',               '            }'],
    ['',               '        }'],
    ['',               '    }'],
    ['c_isvalid_ret',  '    return 1;'],
    ['',               '}'],
    ['',               ''],
    ['c_solve_fn',     'int solveSudoku(int board[9][9], int row, int col) {'],
    ['c_find_row',     '    for (int row = 0; row < 9; row++) {'],
    ['c_find_col',     '        for (int col = 0; col < 9; col++) {'],
    ['c_check_empty',  '            if (board[row][col] == 0) {'],
    ['c_try_num',      '                for (int num = 1; num <= 9; num++) {'],
    ['c_call_valid',   '                    if (isValid(board, row, col, num)) {'],
    ['c_place_num',    '                        board[row][col] = num;'],
    ['c_recurse',      '                        if (solveSudoku(board, row, col + 1)) {'],
    ['c_recurse_ret',  '                            return 1;'],
    ['',               '                        }'],
    ['c_backtrack',    '                        board[row][col] = 0;'],
    ['',               '                    }'],
    ['',               '                }'],
    ['c_ret_false',    '                return 0;'],
    ['',               '            }'],
    ['',               '        }'],
    ['',               '    }'],
    ['c_ret_true',     '    return 1;'],
    ['',               '}'],
    ['',               ''],
    ['',               'int main() {'],
    ['m_board',        '    int board[9][9] = { /* puzzle */ };'],
    ['m_call',         '    solveSudoku(board, 0, 0);'],
    ['',               '    return 0;'],
    ['',               '}']
  ]
};

const PSEUDOCODE = [
  'function solveSudoku(board):',
  '    for row = 0 to 8:',
  '        for col = 0 to 8:',
  '            if board[row][col] == 0:     // Empty cell found',
  '                for num = 1 to 9:',
  '                    if isValid(board, row, col, num):',
  '                        board[row][col] = num    // Place digit',
  '                        if solveSudoku(board):   // Recurse',
  '                            return true',
  '                        board[row][col] = 0      // Backtrack',
  '                return false              // No digit worked',
  '    return true                           // All cells filled',
  '',
  'function isValid(board, row, col, num):',
  '    for x = 0 to 8:',
  '        if board[row][x] == num: return false   // Row check',
  '    for x = 0 to 8:',
  '        if board[x][col] == num: return false   // Col check',
  '    startRow = row - row % 3',
  '    startCol = col - col % 3',
  '    for i = 0 to 2:',
  '        for j = 0 to 2:',
  '            if board[i+startRow][j+startCol] == num: return false',
  '    return true'
];

// ─── Step Builder ─────────────────────────────────────────────────────────────
function buildSteps(puzzleKey) {
  const MAX_STEPS = 30000;
  const steps = [];
  let backtrackCount = 0;
  let solved = false;

  const initialPuzzle = SAMPLES[puzzleKey].map(r => [...r]);
  const board = SAMPLES[puzzleKey].map(r => [...r]);

  function boardSnap() {
    return board.map(r => [...r]);
  }

  function push(step) {
    if (steps.length < MAX_STEPS) {
      steps.push(step);
    }
  }

  const emptyCells = initialPuzzle.flat().filter(x => x === 0).length;
  const givenCells = 81 - emptyCells;

  // ── Phase 1: Init ────────────────────────────────────────────────────────────
  push({
    phase: 'init', code: 'm_board',
    badge: `int[][] board = puzzle; → Sudoku loaded. ${givenCells} clues given, ${emptyCells} cells to fill.`,
    board: boardSnap(), curRow: -1, curCol: -1, tryNum: -1,
    validStatus: null, backtrackCount: 0, solved: false,
    checkRow: -1, checkCol: -1
  });
  push({
    phase: 'init', code: 'm_call',
    badge: 'solveSudoku(board); → Starting backtracking solver. Will scan for empty cells and try digits 1-9.',
    board: boardSnap(), curRow: -1, curCol: -1, tryNum: -1,
    validStatus: null, backtrackCount: 0, solved: false,
    checkRow: -1, checkCol: -1
  });

  // ── Phase 2: Recursive Solver ────────────────────────────────────────────────
  function simulate(startRow = 0, startCol = 0) {
    if (steps.length >= MAX_STEPS) {
      return false;
    }

    push({
      phase: 'solver', code: 'c_solve_fn',
      badge: `solveSudoku(board, ${startRow}, ${startCol}) -> Entered solver. Scanning from [${startRow}][${startCol}] onward.`,
      board: boardSnap(), curRow: -1, curCol: -1, tryNum: -1,
      validStatus: null, backtrackCount, solved: false,
      checkRow: -1, checkCol: -1
    });

    for (let row = startRow; row < 9; row++) {
      if (steps.length >= MAX_STEPS) {
        break;
      }

      push({
        phase: 'solver', code: 'c_find_row',
        badge: `for (int row = 0; row < 9; row++) -> Checking row ${row}.`,
        board: boardSnap(), curRow: row, curCol: -1, tryNum: -1,
        validStatus: null, backtrackCount, solved: false,
        checkRow: row, checkCol: -1
      });

      for (let col = (row === startRow ? startCol : 0); col < 9; col++) {
        if (steps.length >= MAX_STEPS) {
          break;
        }

        push({
          phase: 'solver', code: 'c_find_col',
          badge: `for (int col = 0; col < 9; col++) -> Checking cell [${row}][${col}] = ${board[row][col] === 0 ? 'EMPTY(0)' : board[row][col]}.`,
          board: boardSnap(), curRow: row, curCol: col, tryNum: -1,
          validStatus: null, backtrackCount, solved: false,
          checkRow: row, checkCol: col
        });

        if (board[row][col] === 0) {
          push({
            phase: 'solver', code: 'c_check_empty',
            badge: `if (board[${row}][${col}] == 0) -> TRUE! Empty cell found at [${row}][${col}]. Will try digits 1-9.`,
            board: boardSnap(), curRow: row, curCol: col, tryNum: -1,
            validStatus: null, backtrackCount, solved: false,
            checkRow: row, checkCol: col
          });

          for (let num = 1; num <= 9; num++) {
            if (steps.length >= MAX_STEPS) {
              break;
            }

            push({
              phase: 'solver', code: 'c_try_num',
              badge: `for (int num = 1; num <= 9; num++) -> Trying num = ${num} at [${row}][${col}].`,
              board: boardSnap(), curRow: row, curCol: col, tryNum: num,
              validStatus: null, backtrackCount, solved: false,
              checkRow: row, checkCol: col
            });

            if (steps.length >= MAX_STEPS) {
              break;
            }

            push({
              phase: 'solver', code: 'c_call_valid',
              badge: `if (isValid(board, ${row}, ${col}, ${num})) -> Calling isValid(). Checking row, col, and 3x3 box for num=${num}.`,
              board: boardSnap(), curRow: row, curCol: col, tryNum: num,
              validStatus: null, backtrackCount, solved: false,
              checkRow: row, checkCol: col
            });

            // ── isValid: function entry ───────────────────────────────────────
            push({
              phase: 'solver', code: 'c_isvalid_fn',
              badge: `isValid(board, row=${row}, col=${col}, num=${num}) -> Entered isValid(). Will check row, col, and 3x3 box.`,
              board: boardSnap(), curRow: row, curCol: col, tryNum: num,
              validStatus: null, backtrackCount, solved: false,
              checkRow: row, checkCol: col
            });

            // ── Row check ────────────────────────────────────────────────────
            let rowConflict = false;
            for (let x = 0; x < 9; x++) {
              if (steps.length >= MAX_STEPS) {
                break;
              }

              push({
                phase: 'solver', code: 'c_row_check',
                badge: `for (int x = 0; x < 9; x++) -> [ROW CHECK] Iteration x=${x}. Inspecting board[${row}][${x}] = ${board[row][x]}.`,
                board: boardSnap(), curRow: row, curCol: col, tryNum: num,
                validStatus: null, backtrackCount, solved: false,
                checkRow: row, checkCol: x
              });

              if (board[row][x] === num) {
                push({
                  phase: 'solver', code: 'c_row_check_if',
                  badge: `if (board[${row}][${x}] == ${num}) -> TRUE! [ROW CONFLICT] Duplicate found in row ${row} at col ${x}!`,
                  board: boardSnap(), curRow: row, curCol: col, tryNum: num,
                  validStatus: 'row_conflict', backtrackCount, solved: false,
                  checkRow: row, checkCol: x
                });
                push({
                  phase: 'solver', code: 'c_row_ret_f',
                  badge: `return false; -> [ROW CONFLICT] num=${num} already exists in row ${row}. isValid() returns false.`,
                  board: boardSnap(), curRow: row, curCol: col, tryNum: num,
                  validStatus: 'row_conflict', backtrackCount, solved: false,
                  checkRow: row, checkCol: -1
                });
                rowConflict = true;
                break;
              } else {
                push({
                  phase: 'solver', code: 'c_row_check_if',
                  badge: `if (board[${row}][${x}] == ${num}) -> FALSE. board[${row}][${x}] = ${board[row][x]} ≠ ${num}. No conflict at x=${x}, continuing.`,
                  board: boardSnap(), curRow: row, curCol: col, tryNum: num,
                  validStatus: null, backtrackCount, solved: false,
                  checkRow: row, checkCol: x
                });
              }
            }

            if (rowConflict) {
              continue;
            }

            // ── Col check ────────────────────────────────────────────────────
            let colConflict = false;
            for (let x = 0; x < 9; x++) {
              if (steps.length >= MAX_STEPS) {
                break;
              }

              push({
                phase: 'solver', code: 'c_col_check',
                badge: `for (int x = 0; x < 9; x++) -> [COL CHECK] Iteration x=${x}. Inspecting board[${x}][${col}] = ${board[x][col]}.`,
                board: boardSnap(), curRow: row, curCol: col, tryNum: num,
                validStatus: null, backtrackCount, solved: false,
                checkRow: x, checkCol: col
              });

              if (board[x][col] === num) {
                push({
                  phase: 'solver', code: 'c_col_check_if',
                  badge: `if (board[${x}][${col}] == ${num}) -> TRUE! [COL CONFLICT] Duplicate found in col ${col} at row ${x}!`,
                  board: boardSnap(), curRow: row, curCol: col, tryNum: num,
                  validStatus: 'col_conflict', backtrackCount, solved: false,
                  checkRow: x, checkCol: col
                });
                push({
                  phase: 'solver', code: 'c_col_ret_f',
                  badge: `return false; -> [COL CONFLICT] num=${num} already exists in col ${col}. isValid() returns false.`,
                  board: boardSnap(), curRow: row, curCol: col, tryNum: num,
                  validStatus: 'col_conflict', backtrackCount, solved: false,
                  checkRow: -1, checkCol: col
                });
                colConflict = true;
                break;
              } else {
                push({
                  phase: 'solver', code: 'c_col_check_if',
                  badge: `if (board[${x}][${col}] == ${num}) -> FALSE. board[${x}][${col}] = ${board[x][col]} ≠ ${num}. No conflict at x=${x}, continuing.`,
                  board: boardSnap(), curRow: row, curCol: col, tryNum: num,
                  validStatus: null, backtrackCount, solved: false,
                  checkRow: x, checkCol: col
                });
              }
            }

            if (colConflict) {
              continue;
            }

            // ── Box check ────────────────────────────────────────────────────
            const startRow = row - row % 3;
            const startCol = col - col % 3;

            push({
              phase: 'solver', code: 'c_box_start',
              badge: `int startRow = ${row} - ${row} % 3 = ${startRow}; -> Computing 3x3 box origin row.`,
              board: boardSnap(), curRow: row, curCol: col, tryNum: num,
              validStatus: null, backtrackCount, solved: false,
              checkRow: -1, checkCol: -1, boxRow: startRow, boxCol: startCol
            });

            push({
              phase: 'solver', code: 'c_box_start_c',
              badge: `int startCol = ${col} - ${col} % 3 = ${startCol}; -> Box top-left corner is [${startRow}][${startCol}].`,
              board: boardSnap(), curRow: row, curCol: col, tryNum: num,
              validStatus: null, backtrackCount, solved: false,
              checkRow: -1, checkCol: -1, boxRow: startRow, boxCol: startCol
            });

            let boxConflict = false;
            outerLoop:
            for (let i = 0; i < 3; i++) {
              if (steps.length >= MAX_STEPS) {
                break;
              }

              push({
                phase: 'solver', code: 'c_box_check',
                badge: `for (int i = 0; i < 3; i++) -> [BOX CHECK] Scanning box row offset i=${i}.`,
                board: boardSnap(), curRow: row, curCol: col, tryNum: num,
                validStatus: null, backtrackCount, solved: false,
                checkRow: -1, checkCol: -1, boxRow: startRow, boxCol: startCol
              });

              for (let j = 0; j < 3; j++) {
                if (steps.length >= MAX_STEPS) {
                  break;
                }

                push({
                  phase: 'solver', code: 'c_box_j',
                  badge: `for (int j = 0; j < 3; j++) -> [BOX CHECK] Iteration i=${i}, j=${j}. Inspecting board[${i + startRow}][${j + startCol}] = ${board[i + startRow][j + startCol]}.`,
                  board: boardSnap(), curRow: row, curCol: col, tryNum: num,
                  validStatus: null, backtrackCount, solved: false,
                  checkRow: i + startRow, checkCol: j + startCol, boxRow: startRow, boxCol: startCol
                });

                if (board[i + startRow][j + startCol] === num) {
                  push({
                    phase: 'solver', code: 'c_box_if',
                    badge: `if (board[${i + startRow}][${j + startCol}] == ${num}) -> TRUE! [BOX CONFLICT] Duplicate found in 3x3 box!`,
                    board: boardSnap(), curRow: row, curCol: col, tryNum: num,
                    validStatus: 'box_conflict', backtrackCount, solved: false,
                    checkRow: i + startRow, checkCol: j + startCol, boxRow: startRow, boxCol: startCol
                  });
                  push({
                    phase: 'solver', code: 'c_box_ret_f',
                    badge: `return false; -> [BOX CONFLICT] num=${num} already exists in 3x3 box. isValid() returns false.`,
                    board: boardSnap(), curRow: row, curCol: col, tryNum: num,
                    validStatus: 'box_conflict', backtrackCount, solved: false,
                    checkRow: -1, checkCol: -1, boxRow: startRow, boxCol: startCol
                  });
                  boxConflict = true;
                  break outerLoop;
                } else {
                  push({
                    phase: 'solver', code: 'c_box_if',
                    badge: `if (board[${i + startRow}][${j + startCol}] == ${num}) -> FALSE. board[${i + startRow}][${j + startCol}] = ${board[i + startRow][j + startCol]} ≠ ${num}. No conflict, continuing.`,
                    board: boardSnap(), curRow: row, curCol: col, tryNum: num,
                    validStatus: null, backtrackCount, solved: false,
                    checkRow: i + startRow, checkCol: j + startCol, boxRow: startRow, boxCol: startCol
                  });
                }
              }
            }

            if (boxConflict) {
              continue;
            }

            // ── isValid returns true ──────────────────────────────────────────
            push({
              phase: 'solver', code: 'c_isvalid_ret',
              badge: `return true; -> [VALID] num=${num} passes row, col, and box checks for [${row}][${col}].`,
              board: boardSnap(), curRow: row, curCol: col, tryNum: num,
              validStatus: 'valid', backtrackCount, solved: false,
              checkRow: -1, checkCol: -1
            });

            // ── Place num ─────────────────────────────────────────────────────
            board[row][col] = num;
            push({
              phase: 'solver', code: 'c_place_num',
              badge: `board[${row}][${col}] = ${num}; -> [PLACED] Digit ${num} placed at [${row}][${col}]. ${board.flat().filter(x => x !== 0).length} of 81 cells filled.`,
              board: boardSnap(), curRow: row, curCol: col, tryNum: num,
              validStatus: 'placed', backtrackCount, solved: false,
              checkRow: -1, checkCol: -1, justPlaced: true
            });

            // ── Recurse ───────────────────────────────────────────────────────
            push({
              phase: 'solver', code: 'c_recurse',
              badge: `if (solveSudoku(board, ${row}, ${col + 1})) -> Placed ${num} at [${row}][${col}]. Recursing from [${row}][${col + 1}].`,
              board: boardSnap(), curRow: row, curCol: col, tryNum: num,
              validStatus: null, backtrackCount, solved: false,
              checkRow: -1, checkCol: -1
            });

            const result = simulate(row, col + 1);

            if (result) {
              push({
                phase: 'solver', code: 'c_recurse_ret',
                badge: 'return true; -> [SUCCESS] Recursion returned true. Propagating success upward.',
                board: boardSnap(), curRow: row, curCol: col, tryNum: num,
                validStatus: 'placed', backtrackCount, solved: true,
                checkRow: -1, checkCol: -1
              });
              return true;
            }

            // ── Backtrack ─────────────────────────────────────────────────────
            board[row][col] = 0;
            backtrackCount++;
            push({
              phase: 'solver', code: 'c_backtrack',
              badge: `board[${row}][${col}] = 0; -> [BACKTRACK] num=${num} led to dead end. Erasing. Total backtracks: ${backtrackCount}.`,
              board: boardSnap(), curRow: row, curCol: col, tryNum: num,
              validStatus: 'backtrack', backtrackCount, solved: false,
              checkRow: -1, checkCol: -1, isBacktrack: true
            });
          }

          // ── No num worked ─────────────────────────────────────────────────
          push({
            phase: 'solver', code: 'c_ret_false',
            badge: `return false; -> [DEAD END] No digit 1-9 works for [${row}][${col}]. Returning false to parent.`,
            board: boardSnap(), curRow: row, curCol: col, tryNum: -1,
            validStatus: 'dead_end', backtrackCount, solved: false,
            checkRow: -1, checkCol: -1, isDeadEnd: true
          });
          return false;
        } else {
          push({
            phase: 'solver', code: 'c_check_empty',
            badge: `if (board[${row}][${col}] == 0) -> FALSE. Cell [${row}][${col}] = ${board[row][col]}. Already filled. Skipping.`,
            board: boardSnap(), curRow: row, curCol: col, tryNum: -1,
            validStatus: null, backtrackCount, solved: false,
            checkRow: row, checkCol: col
          });
        }
      }
    }

    // ── All cells filled ──────────────────────────────────────────────────────
    solved = true;
    push({
      phase: 'done', code: 'c_ret_true',
      badge: `return true; -> [COMPLETE] All 81 cells filled! Sudoku solved after ${backtrackCount} backtracks!`,
      board: boardSnap(), curRow: -1, curCol: -1, tryNum: -1,
      validStatus: null, backtrackCount, solved: true,
      checkRow: -1, checkCol: -1
    });
    return true;
  }

  simulate(0, 0);

  if (steps.length >= MAX_STEPS && steps.length > 0) {
    steps[steps.length - 1].stepsCapped = true;
  }

  if (steps.length < MAX_STEPS && !solved) {
    push({
      phase: 'done', code: 'm_call',
      badge: `No solution found. All possibilities exhausted. Total backtracks: ${backtrackCount}.`,
      board: boardSnap(), curRow: -1, curCol: -1, tryNum: -1,
      validStatus: null, backtrackCount, solved: false,
      checkRow: -1, checkCol: -1
    });
  }

  return steps;
}

// ─── Reactive State ───────────────────────────────────────────────────────────
const selectedPuzzle = ref('easy');
const lang = ref('java');
const speed = ref(650);
const si = ref(0);
const playing = ref(false);
const vizHeight = ref(300);
const tableHeight = ref(60);
const leftWidth = ref(52);
const rightTab = ref('code');

const stepsData = reactive({ steps: buildSteps('easy') });
const steps = computed(() => stepsData.steps);
const s = computed(() => steps.value[Math.max(0, Math.min(si.value, steps.value.length - 1))] || {});
const codeLines = computed(() => CODES[lang.value] || []);

let playTimer = null;

function onChipsWheel(e) {
  if (e.currentTarget) {
    e.currentTarget.scrollLeft += e.deltaY;
  }
}

function applyInput() {
  playing.value = false;
  stepsData.steps = buildSteps(selectedPuzzle.value);
  si.value = 0;
  if (typeof window !== 'undefined') {
    window.scrollTo(0, 0);
  }
}

function loadPreset(key) {
  selectedPuzzle.value = key;
  applyInput();
}

function stepBy(d) {
  si.value = Math.max(0, Math.min(steps.value.length - 1, si.value + d));
}

function togglePlay() {
  const next = !playing.value;
  if (next && si.value >= steps.value.length - 1) {
    si.value = 0;
  }
  playing.value = next;
}

function tick() {
  clearTimeout(playTimer);
  if (!playing.value) {
    return;
  }
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
  if (v) {
    tick();
  } else {
    clearTimeout(playTimer);
  }
});

const codeScrollRef = ref(null);
function scrollActiveCodeLine() {
  nextTick(() => {
    const container = codeScrollRef.value;
    if (!container) {
      return;
    }
    const activeEl = container.querySelector('.ll-hl');
    if (!activeEl) {
      return;
    }
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
watch(rightTab, v => {
  if (v === 'code') {
    scrollActiveCodeLine();
  }
});

function onKeydown(e) {
  const tag = e.target.tagName;
  if (tag === 'INPUT' || tag === 'SELECT' || tag === 'TEXTAREA') {
    return;
  }
  if (e.key === 'ArrowRight') {
    stepBy(1);
  }
  if (e.key === 'ArrowLeft') {
    stepBy(-1);
  }
  if (e.key === ' ') {
    e.preventDefault();
    togglePlay();
  }
}

const displayBoard = computed(() => s.value.board || SAMPLES[selectedPuzzle.value]);
const displayBacktracks = computed(() => s.value.backtrackCount || 0);
const filledCells = computed(() => displayBoard.value.flat().filter(x => x !== 0).length);
const initialBoard = computed(() => SAMPLES[selectedPuzzle.value]);

function isFixedCell(r, c) {
  return initialBoard.value[r][c] !== 0;
}

function boxIndex(r, c) {
  return Math.floor(r / 3) * 3 + Math.floor(c / 3);
}

const mainRef = ref(null);
const leftColRef = ref(null);
const hResizerRef = ref(null);
const vizResizerRef = ref(null);
const tableResizerRef = ref(null);

function initHResizer() {
  const rsz = hResizerRef.value;
  const main = mainRef.value;
  if (!rsz || !main) {
    return;
  }
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
    if (!dragging) {
      return;
    }
    const mainW = main.offsetWidth;
    leftWidth.value = (Math.max(200, Math.min(mainW - 200, startW + e.clientX - startX)) / mainW) * 100;
  };
  const onUp = () => {
    if (!dragging) {
      return;
    }
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
  if (!rsz) {
    return;
  }
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
    if (!dragging) {
      return;
    }
    valueRef.value = Math.max(minH, Math.min(maxH, startH + (e.clientY - startY)));
  };
  const onUp = () => {
    if (!dragging) {
      return;
    }
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
  cleanupFns.push(initVResizer(vizResizerRef, vizHeight, 240, 720));
  cleanupFns.push(initVResizer(tableResizerRef, tableHeight, 50, 200));
  if (typeof window !== 'undefined') {
    window.scrollTo(0, 0);
  }
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
              <select v-model="selectedPuzzle" class="ll-text-input" style="width:80px">
                <option value="easy">Easy 1</option>
                <option value="medium">Easy 2</option>
                <option value="hard">Easy 3</option>
              </select>
            </div>
            <div class="ll-preset-group">
              <button class="ll-preset-btn" @click="loadPreset('easy')" title="Easy puzzle 1">Easy 1</button>
              <button class="ll-preset-btn" @click="loadPreset('medium')" title="Easy puzzle 2">Easy 2</button>
              <button class="ll-preset-btn" @click="loadPreset('hard')" title="Easy puzzle 3">Easy 3</button>
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
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">row</span><b class="ll-c-orange">{{ s.curRow !== undefined && s.curRow >= 0 ? s.curRow : '-' }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">col</span><b class="ll-c-orange">{{ s.curCol !== undefined && s.curCol >= 0 ? s.curCol : '-' }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">num</span><b class="ll-c-purple">{{ s.tryNum !== undefined && s.tryNum > 0 ? s.tryNum : '-' }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">backtracks</span><b class="ll-c-red">{{ displayBacktracks }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">filled</span><b class="ll-c-green">{{ filledCells }} / 81</b></span>
                    <span class="ll-ptr-chip-inline" v-if="s.solved"><b class="ll-c-green">[COMPLETE]</b></span>
                    <span class="ll-ptr-chip-inline" v-if="s.stepsCapped"><b class="ll-c-red">[STEP LIMIT]</b></span>
                  </div>

                  <!-- BOARD CONTAINER (Sudoku-specific layout) -->
                  <div class="ll-board-container">

                    <!-- Tier 1: 9x9 Sudoku Grid -->
                    <div class="gp-tier-title">Tier 1 &mdash; Sudoku Board <code>int[][] board[9][9]</code></div>
                    <div class="sdk-board-wrap">
                      <div class="sdk-board">
                        <template v-for="(row, rIdx) in displayBoard" :key="'row-' + rIdx">
                          <div
                            v-for="(cell, cIdx) in row"
                            :key="'cell-' + rIdx + '-' + cIdx"
                            class="sdk-cell"
                            :class="{
                              'sdk-cell-fixed':    isFixedCell(rIdx, cIdx),
                              'sdk-cell-placed':   !isFixedCell(rIdx, cIdx) && cell !== 0 && !(rIdx === s.curRow && cIdx === s.curCol && (s.isBacktrack || s.validStatus === 'backtrack')),
                              'sdk-cell-current':  rIdx === s.curRow && cIdx === s.curCol && !s.isBacktrack && s.phase !== 'done',
                              'sdk-cell-empty':    cell === 0 && !(rIdx === s.curRow && cIdx === s.curCol),
                              'sdk-cell-placed-ok': rIdx === s.curRow && cIdx === s.curCol && s.validStatus === 'placed' && !s.isBacktrack,
                              'sdk-cell-backtrack': rIdx === s.curRow && cIdx === s.curCol && s.isBacktrack,
                              'sdk-cell-solved':   s.solved && !isFixedCell(rIdx, cIdx) && cell !== 0,
                              'sdk-cell-check-highlight': s.checkRow === rIdx && s.checkCol === cIdx && !(rIdx === s.curRow && cIdx === s.curCol),
                              'sdk-cell-check-row': s.checkRow === rIdx && s.checkCol !== cIdx && !(rIdx === s.curRow && cIdx === s.curCol) && s.code && (s.code === 'c_row_check' || s.code === 'c_row_check_if' || s.code === 'c_row_ret_f'),
                              'sdk-cell-check-col': s.checkCol === cIdx && s.checkRow !== rIdx && !(rIdx === s.curRow && cIdx === s.curCol) && s.code && (s.code === 'c_col_check' || s.code === 'c_col_check_if' || s.code === 'c_col_ret_f'),
                              'sdk-cell-check-box': s.boxRow !== undefined && rIdx >= s.boxRow && rIdx < s.boxRow + 3 && cIdx >= s.boxCol && cIdx < s.boxCol + 3 && !(rIdx === s.curRow && cIdx === s.curCol) && s.code && (s.code === 'c_box_check' || s.code === 'c_box_j' || s.code === 'c_box_if' || s.code === 'c_box_ret_f' || s.code === 'c_box_start' || s.code === 'c_box_start_c'),
                              'sdk-border-right-thick':  cIdx === 2 || cIdx === 5,
                              'sdk-border-bottom-thick': rIdx === 2 || rIdx === 5
                            }"
                            :title="`board[${rIdx}][${cIdx}] = ${cell}`"
                          >
                            <span v-if="cell !== 0" class="sdk-digit" :class="{ 'sdk-digit-fixed': isFixedCell(rIdx, cIdx), 'sdk-digit-placed': !isFixedCell(rIdx, cIdx) }">{{ cell }}</span>
                            <span v-else class="sdk-dot">·</span>
                            <span v-if="rIdx === s.curRow && cIdx === s.curCol && s.tryNum > 0 && cell === 0" class="sdk-try-badge">{{ s.tryNum }}?</span>
                          </div>
                        </template>
                      </div>
                    </div>

                    <!-- Tier 2: isValid Check Panel -->
                    <div class="sdk-right-panels">
                      <!-- <div class="gp-tier-title">Tier 2 &mdash; isValid() Check Status</div>
                      <div class="sdk-valid-panel">
                        <div class="sdk-check-item"
                          :class="{
                            'sdk-check-active':   s.code && (s.code === 'c_row_check' || s.code === 'c_row_check_if' || s.code === 'c_row_ret_f'),
                            'sdk-check-conflict': s.validStatus === 'row_conflict',
                            'sdk-check-pass':     s.code && (s.code === 'c_col_check' || s.code === 'c_col_check_if' || s.code === 'c_col_ret_f' || s.code === 'c_box_check' || s.code === 'c_box_j' || s.code === 'c_box_if' || s.code === 'c_box_ret_f' || s.code === 'c_box_start' || s.code === 'c_box_start_c' || s.code === 'c_isvalid_ret' || s.code === 'c_place_num')
                          }"
                        >
                          <span class="sdk-check-icon">
                            <template v-if="s.validStatus === 'row_conflict'">✗</template>
                            <template v-else-if="s.code === 'c_col_check' || s.code === 'c_box_check' || s.code === 'c_isvalid_ret' || s.code === 'c_place_num'">✓</template>
                            <template v-else>→</template>
                          </span>
                          <div class="sdk-check-text">
                            <span class="sdk-check-label">Row Check</span>
                            <span class="sdk-check-detail">
                              <template v-if="s.validStatus === 'row_conflict'"><b class="ll-c-red">[CONFLICT]</b> num={{ s.tryNum }} in row {{ s.curRow }}</template>
                              <template v-else-if="s.code === 'c_col_check' || s.code === 'c_isvalid_ret' || s.code === 'c_place_num'"><b class="ll-c-green">[PASS]</b></template>
                              <template v-else-if="s.tryNum > 0">Checking board[{{ s.curRow }}][0..8] for {{ s.tryNum }}</template>
                              <template v-else>Waiting…</template>
                            </span>
                          </div>
                        </div>
                        <div class="sdk-check-item"
                          :class="{
                            'sdk-check-active':   s.code && (s.code === 'c_col_check' || s.code === 'c_col_check_if' || s.code === 'c_col_ret_f'),
                            'sdk-check-conflict': s.validStatus === 'col_conflict',
                            'sdk-check-pass':     s.code && (s.code === 'c_box_check' || s.code === 'c_box_j' || s.code === 'c_box_if' || s.code === 'c_box_ret_f' || s.code === 'c_box_start' || s.code === 'c_box_start_c' || s.code === 'c_isvalid_ret' || s.code === 'c_place_num')
                          }"
                        >
                          <span class="sdk-check-icon">
                            <template v-if="s.validStatus === 'col_conflict'">✗</template>
                            <template v-else-if="s.code === 'c_box_check' || s.code === 'c_box_start' || s.code === 'c_isvalid_ret' || s.code === 'c_place_num'">✓</template>
                            <template v-else>→</template>
                          </span>
                          <div class="sdk-check-text">
                            <span class="sdk-check-label">Col Check</span>
                            <span class="sdk-check-detail">
                              <template v-if="s.validStatus === 'col_conflict'"><b class="ll-c-red">[CONFLICT]</b> num={{ s.tryNum }} in col {{ s.curCol }}</template>
                              <template v-else-if="s.code === 'c_box_check' || s.code === 'c_isvalid_ret' || s.code === 'c_place_num'"><b class="ll-c-green">[PASS]</b></template>
                              <template v-else-if="s.tryNum > 0">Checking board[0..8][{{ s.curCol }}] for {{ s.tryNum }}</template>
                              <template v-else>Waiting…</template>
                            </span>
                          </div>
                        </div>
                        <div class="sdk-check-item"
                          :class="{
                            'sdk-check-active':   s.code && (s.code === 'c_box_check' || s.code === 'c_box_j' || s.code === 'c_box_if' || s.code === 'c_box_ret_f' || s.code === 'c_box_start' || s.code === 'c_box_start_c'),
                            'sdk-check-conflict': s.validStatus === 'box_conflict',
                            'sdk-check-pass':     s.code && (s.code === 'c_isvalid_ret' || s.code === 'c_place_num')
                          }"
                        >
                          <span class="sdk-check-icon">
                            <template v-if="s.validStatus === 'box_conflict'">✗</template>
                            <template v-else-if="s.code === 'c_isvalid_ret' || s.code === 'c_place_num'">✓</template>
                            <template v-else>→</template>
                          </span>
                          <div class="sdk-check-text">
                            <span class="sdk-check-label">Box Check</span>
                            <span class="sdk-check-detail">
                              <template v-if="s.validStatus === 'box_conflict'"><b class="ll-c-red">[CONFLICT]</b> num={{ s.tryNum }} in 3×3 box</template>
                              <template v-else-if="s.code === 'c_isvalid_ret' || s.code === 'c_place_num'"><b class="ll-c-green">[PASS]</b></template>
                              <template v-else-if="s.boxRow !== undefined">Checking 3×3 box at [{{ s.boxRow }}..{{ (s.boxRow||0)+2 }}][{{ s.boxCol }}..{{ (s.boxCol||0)+2 }}]</template>
                              <template v-else>Waiting…</template>
                            </span>
                          </div>
                        </div>
                      </div> -->

                      <!-- Tier 3: Backtrack Monitor -->
                      <!-- <div class="gp-tier-title">Tier 3 &mdash; Backtrack Monitor</div>
                      <div class="sdk-deadend-panel"
                        :class="{
                          'sdk-de-active': s.isDeadEnd || s.isBacktrack,
                          'sdk-de-solved': s.solved
                        }"
                      >
                        <template v-if="s.solved">
                          <span class="sdk-de-icon sdk-de-solved-icon">OK</span>
                          <div>
                            <div class="sdk-de-title">[COMPLETE] Sudoku Solved!</div>
                            <div class="sdk-de-detail">All 81 cells filled after {{ displayBacktracks }} backtracks.</div>
                          </div>
                        </template>
                        <template v-else-if="s.isDeadEnd">
                          <span class="sdk-de-icon sdk-de-dead-icon">!</span>
                          <div>
                            <div class="sdk-de-title">[DEAD END] No digit valid for [{{ s.curRow }}][{{ s.curCol }}]</div>
                            <div class="sdk-de-detail">Digits 1-9 all fail. Returning false. Backtracks: {{ displayBacktracks }}.</div>
                          </div>
                        </template>
                        <template v-else-if="s.isBacktrack">
                          <span class="sdk-de-icon sdk-de-bt-icon">BT</span>
                          <div>
                            <div class="sdk-de-title">[BACKTRACKING] Erasing board[{{ s.curRow }}][{{ s.curCol }}]</div>
                            <div class="sdk-de-detail">Resetting to 0. Trying next digit. Total backtracks: {{ displayBacktracks }}.</div>
                          </div>
                        </template>
                        <template v-else>
                          <span class="sdk-de-icon sdk-de-idle-icon">--</span>
                          <div>
                            <div class="sdk-de-title">Monitoring</div>
                            <div class="sdk-de-detail">Watching for dead ends. Condition: no digit 1-9 valid → backtrack.</div>
                          </div>
                        </template>
                      </div> -->

                      <!-- Tier 4: Memory & Stats -->
                      <!-- <div class="gp-tier-title">Tier 4 &mdash; Memory &amp; Statistics</div>
                      <div class="gp-mem-bar">
                        <span class="gp-mem-item"><span class="gp-mem-label">Board:</span> <code>9×9×4B = 324B</code></span>
                        <span class="gp-mem-sep">|</span>
                        <span class="gp-mem-item"><span class="gp-mem-label">Filled:</span> <code>{{ filledCells }}/81</code></span>
                        <span class="gp-mem-sep">|</span>
                        <span class="gp-mem-item"><span class="gp-mem-label">BT:</span> <code>{{ displayBacktracks }}</code></span>
                        <span class="gp-mem-sep">|</span>
                        <span class="gp-mem-item"><span class="gp-mem-label">Step:</span> <code>{{ si + 1 }}/{{ steps.length }}</code></span>
                      </div> -->
                    </div>

                  </div>
                  <!-- END ll-board-container -->
                </div>
              </div>

              <div class="ll-vresizer" ref="vizResizerRef"></div>

              <!-- Legend -->
              <div class="ll-legend">
                <span class="ll-leg"><span class="ll-legdot sdk-legdot-fixed"></span>CLUE</span>
                <span class="ll-leg"><span class="ll-legdot sdk-legdot-placed"></span>PLACED</span>
                <span class="ll-leg"><span class="ll-legdot sdk-legdot-current"></span>CURRENT</span>
                <span class="ll-leg"><span class="ll-legdot sdk-legdot-bt"></span>BACKTRACK</span>
                <span class="ll-leg"><span class="ll-legdot sdk-legdot-done"></span>SOLVED</span>
              </div>

              <!-- Call Stack -->
              <div class="ll-table-area" :style="{ height: tableHeight + 'px' }">
                <div class="ll-table-title">Execution State &mdash; current cell and num being tried</div>
                <div class="ll-stack-line">
                  <template v-if="s.curRow !== undefined && s.curRow >= 0">
                    <div class="ll-frame ll-frame-cur">
                      solveSudoku(<span class="ll-fname">row</span>=<span class="ll-c-orange" style="font-weight:700">{{ s.curRow }}</span>, <span class="ll-fname">col</span>=<span class="ll-c-orange" style="font-weight:700">{{ s.curCol }}</span>, <span class="ll-fname">num</span>=<span class="ll-c-purple" style="font-weight:700">{{ s.tryNum > 0 ? s.tryNum : '—' }}</span>) <span class="ll-now">&#9668; active</span>
                    </div>
                  </template>
                  <template v-else>&mdash;</template>
                </div>
              </div>

              <div class="ll-vresizer" ref="tableResizerRef"></div>

              <!-- Step Badge -->
              <div class="ll-badge-wrap">
                <div class="ll-badge" :class="{
                  'll-badge-error':   s.badge && (s.badge.includes('[CONFLICT]') || s.badge.includes('[DEAD END]') || s.badge.includes('[BACKTRACK]')),
                  'll-badge-success': s.badge && (s.badge.includes('[COMPLETE]') || s.badge.includes('[VALID]') || s.badge.includes('[PLACED]') || s.badge.includes('[SUCCESS]'))
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
                    Given a partially filled 9&times;9 grid, fill empty cells (0s) so every row, column, and
                    3&times;3 sub-box contains digits 1&ndash;9 exactly once.
                    Uses backtracking: scan for the first 0, try digits 1&ndash;9, recurse on each valid placement,
                    and backtrack on failure.
                  </p>
                  <h4 class="ll-cx-sub">Complexity Breakdown</h4>
                  <table class="ll-complexity-table">
                    <thead><tr><th>Operation</th><th>Time</th><th>Space</th><th>Notes</th></tr></thead>
                    <tbody>
                      <tr><td>isValid()</td><td class="ll-cx-good">O(1)</td><td class="ll-cx-good">O(1)</td><td>Fixed 9-cell checks for row, col, box</td></tr>
                      <tr><td>solveSudoku() worst</td><td class="ll-cx-bad">O(9<sup>81</sup>)</td><td class="ll-cx-good">O(81)</td><td>Up to 81 empty cells, 9 choices each</td></tr>
                      <tr><td>solveSudoku() practical</td><td class="ll-cx-good">Very fast</td><td class="ll-cx-good">O(81)</td><td>Constraints prune the tree heavily</td></tr>
                    </tbody>
                  </table>
                  <h4 class="ll-cx-sub">Overall Complexity</h4>
                  <div class="ll-cx-summary-grid">
                    <div class="ll-cx-card ll-cx-card-bad">
                      <div class="ll-cx-card-label">Time (Worst)</div>
                      <div class="ll-cx-card-val">O(9<sup>81</sup>)</div>
                      <div class="ll-cx-card-note">Theoretical; pruned heavily in practice</div>
                    </div>
                    <div class="ll-cx-card ll-cx-card-good">
                      <div class="ll-cx-card-label">Space</div>
                      <div class="ll-cx-card-val">O(81)</div>
                      <div class="ll-cx-card-note">Board (81 cells) + recursion depth &le; 81</div>
                    </div>
                  </div>
                  <h4 class="ll-cx-sub">Algorithm Steps</h4>
                  <table class="ll-complexity-table">
                    <thead><tr><th>Step</th><th>Action</th></tr></thead>
                    <tbody>
                      <tr><td>1. Find empty</td><td>Scan board left-to-right, top-to-bottom for first 0</td></tr>
                      <tr><td>2. Try digits</td><td>For num = 1 to 9, call isValid()</td></tr>
                      <tr><td>3. Place &amp; recurse</td><td>board[row][col] = num, then solveSudoku()</td></tr>
                      <tr><td>4. Backtrack</td><td>If recursion fails, board[row][col] = 0</td></tr>
                      <tr><td>5. Return false</td><td>No digit works → signal parent to backtrack</td></tr>
                      <tr><td>6. Return true</td><td>No empty cell found → puzzle solved</td></tr>
                    </tbody>
                  </table>
                  <div class="ll-note">
                    <strong>Note:</strong> For well-formed puzzles the solver is extremely fast.
                    The visualization caps steps at 6000 for browser performance.
                    Advanced solvers combine backtracking with constraint propagation (AC-3)
                    for even greater efficiency.
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
@keyframes sdk-flash-bt { 0%{background:#fef2f2} 50%{background:#ef4444;color:#fff} 100%{background:#fee2e2} }
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

/* ─── Board Container: side-by-side grid + panels ─────────────────────────── */
.ll-board-container {
  display: flex;
  flex-direction: row;
  align-items: flex-start;
  padding: 6px 14px 10px;
  gap: 16px;
  flex-wrap: wrap;
}
.gp-tier-title {
  font-size: 10px; font-weight: 700; text-transform: uppercase; letter-spacing: .04em;
  color: var(--muted); margin-top: 6px; margin-bottom: 4px;
  font-family: 'Consolas', monospace;
  border-left: 3px solid var(--coral); padding-left: 6px;
  width: 100%;
}

/* ─── 9x9 Sudoku Grid ────────────────────────────────────────────────────── */
.sdk-board-wrap { display: flex; flex-direction: column; align-items: flex-start; }
.sdk-board {
  display: grid;
  grid-template-columns: repeat(9, 34px);
  grid-template-rows:    repeat(9, 34px);
  border: 2.5px solid #1e293b;
  border-radius: 5px;
  overflow: hidden;
  background: #475569;
  gap: 1px;
}
.sdk-cell {
  position: relative;
  width: 34px; height: 34px;
  display: flex; align-items: center; justify-content: center;
  background: #f8fafc;
  transition: all .14s;
  cursor: default; user-select: none;
}
.sdk-border-right-thick  { border-right:  2.5px solid #1e293b !important; }
.sdk-border-bottom-thick { border-bottom: 2.5px solid #1e293b !important; }

/* Cell states */
.sdk-cell-fixed     { background: #dde3ec !important; }
.sdk-cell-empty     { background: #f8fafc; }
.sdk-cell-placed    { background: #eff6ff; }
.sdk-cell-current   { background: #fef3c7 !important; border: 2px solid #f59e0b !important; transform: scale(1.08); z-index: 10; animation: ll-pop .18s ease; }
.sdk-cell-placed-ok { background: #dcfce7 !important; border: 1.5px solid var(--green) !important; }
.sdk-cell-backtrack { background: #fee2e2 !important; border: 2px solid var(--red) !important; animation: sdk-flash-bt .35s ease; }
.sdk-cell-solved    { background: #f0fdf4 !important; }

/* Highlight during checks */
.sdk-cell-check-row       { background: #fef9c3 !important; }
.sdk-cell-check-col       { background: #fce7f3 !important; }
.sdk-cell-check-box       { background: #ede9fe !important; }
.sdk-cell-check-highlight { background: #fed7aa !important; border: 1.5px solid var(--orange) !important; z-index: 5; }

/* Digit styles */
.sdk-digit { font-size: 15px; font-weight: 800; font-family: 'Consolas', monospace; line-height: 1; }
.sdk-digit-fixed  { color: #1e293b; }
.sdk-digit-placed { color: #1d4ed8; }
.sdk-cell-solved .sdk-digit-placed { color: #15803d !important; }
.sdk-dot { font-size: 16px; color: #cbd5e1; line-height: 1; }
.sdk-try-badge {
  position: absolute; top: 1px; right: 2px;
  font-size: 7px; font-weight: 900; font-family: monospace;
  background: var(--orange); color: #fff;
  padding: 1px 2px; border-radius: 2px; line-height: 1;
}

/* ─── Right Panels (validity + monitor + stats) ──────────────────────────── */
.sdk-right-panels {
  display: flex;
  flex-direction: column;
  gap: 0;
  min-width: 200px;
  max-width: 280px;
  flex: 1;
}

/* ─── Validity Check Panel ──────────────────────────────────────────────── */
.sdk-valid-panel {
  display: flex; flex-direction: column; gap: 4px;
  background: var(--surface2); border: 1px solid var(--border);
  border-radius: var(--radius-sm); padding: 6px 8px;
  font-family: monospace; font-size: 10px;
}
.sdk-check-item {
  display: flex; align-items: center; gap: 6px;
  padding: 4px 6px; border-radius: 4px;
  border: 1.5px solid transparent; background: var(--surface);
  transition: all .15s;
}
.sdk-check-active   { border-color: var(--orange) !important; background: var(--orange-light) !important; }
.sdk-check-conflict { border-color: var(--red) !important; background: var(--red-light) !important; }
.sdk-check-pass     { border-color: var(--green) !important; background: var(--green-light) !important; }
.sdk-check-icon {
  width: 18px; height: 18px; border-radius: 50%;
  display: flex; align-items: center; justify-content: center;
  font-size: 11px; font-weight: 900; border: 1.5px solid var(--border2);
  background: var(--surface2); flex-shrink: 0;
}
.sdk-check-active   .sdk-check-icon { border-color: var(--orange); color: var(--orange); }
.sdk-check-conflict .sdk-check-icon { border-color: var(--red);    color: var(--red);    background: var(--red-light); }
.sdk-check-pass     .sdk-check-icon { border-color: var(--green);  color: var(--green);  background: var(--green-light); }
.sdk-check-text  { display: flex; flex-direction: column; gap: 1px; }
.sdk-check-label { font-weight: 700; color: var(--text2); font-size: 10px; }
.sdk-check-detail{ font-size: 9.5px; color: var(--muted); }

/* ─── Backtrack Monitor ──────────────────────────────────────────────────── */
.sdk-deadend-panel {
  display: flex; align-items: center; gap: 8px;
  padding: 7px 10px; border-radius: var(--radius-sm);
  border: 1.5px solid var(--border); background: var(--surface2);
  transition: all .2s; margin-top: 2px;
}
.sdk-de-active { border-color: var(--red) !important; background: var(--red-light) !important; }
.sdk-de-solved { border-color: var(--green) !important; background: var(--green-light) !important; animation: sdk-glow-done 1.5s infinite; }
.sdk-de-icon {
  width: 26px; height: 26px; border-radius: 50%;
  display: flex; align-items: center; justify-content: center;
  font-size: 9px; font-weight: 900; font-family: monospace;
  flex-shrink: 0; border: 2px solid currentColor;
}
.sdk-de-idle-icon   { color: var(--muted);  background: var(--surface); }
.sdk-de-dead-icon   { color: #b91c1c; background: #fee2e2; }
.sdk-de-bt-icon     { color: #c2410c; background: #fff7ed; }
.sdk-de-solved-icon { color: #15803d; background: #dcfce7; }
.sdk-de-title  { font-size: 10.5px; font-weight: 800; font-family: monospace; color: var(--text); }
.sdk-de-active .sdk-de-title { color: #b91c1c; }
.sdk-de-solved .sdk-de-title { color: #15803d; }
.sdk-de-detail { font-size: 9.5px; color: var(--text2); font-family: monospace; line-height: 1.4; }

/* ─── Memory Bar ─────────────────────────────────────────────────────────── */
.gp-mem-bar { display:flex; flex-wrap:wrap; gap:4px 8px; background:#1e293b; color:#94a3b8; font-family:monospace; font-size:9.5px; padding:5px 8px; border-radius:var(--radius-sm); align-items:center; margin-top:4px; }
.gp-mem-item { display:flex; align-items:center; gap:4px; }
.gp-mem-label { color:#64748b; font-weight:700; }
.gp-mem-bar code { color:#38bdf8; font-size:9.5px; }
.gp-mem-sep { color:#334155; font-size:11px; }

.ll-vresizer { height:5px; cursor:row-resize; background:var(--border); flex-shrink:0; transition:background .15s; z-index:20; }
.ll-vresizer:hover, .ll-vresizer.drag { background:var(--coral); }

/* ─── Legend ─────────────────────────────────────────────────────────────── */
.ll-legend { display:flex; flex-wrap:wrap; gap:6px 14px; padding:6px 12px; border-bottom:1px solid var(--border); flex-shrink:0; background:var(--surface2); }
.ll-leg { display:flex; align-items:center; gap:5px; font-size:11px; color:var(--text2); font-weight:500; }
.ll-legdot { width:11px; height:11px; border-radius:3px; flex-shrink:0; display:inline-block; }
.sdk-legdot-fixed   { background:#dde3ec; border:1.5px solid #94a3b8; }
.sdk-legdot-placed  { background:#eff6ff; border:1.5px solid #93c5fd; }
.sdk-legdot-current { background:#fef3c7; border:1.5px solid #f59e0b; }
.sdk-legdot-empty   { background:#f8fafc; border:1.5px solid var(--border2); }
.sdk-legdot-bt      { background:#fee2e2; border:1.5px solid var(--red); }
.sdk-legdot-done    { background:#dcfce7; border:1.5px solid var(--green); }

/* ─── Call Stack ─────────────────────────────────────────────────────────── */
.ll-table-area { flex-shrink:0; padding:8px 14px; border-bottom:1px solid var(--border); overflow:auto; background:var(--surface); min-width:0; box-sizing:border-box; }
.ll-table-title { font-size:10px; color:var(--muted); margin-bottom:4px; font-style:italic; }
.ll-stack-line { font-family:'Consolas',monospace; font-size:12px; line-height:1.8; }
.ll-frame { font-family:'Consolas',monospace; font-size:11.5px; color:var(--text2); padding:1px 0; white-space:nowrap; }
.ll-frame-cur { color:var(--orange); background:var(--orange-light); border-radius:4px; padding:1px 5px; }
.ll-fname { color:var(--text2); }
.ll-now { color:var(--orange); font-size:10px; margin-left:6px; }

/* ─── Badge ──────────────────────────────────────────────────────────────── */
.ll-badge-wrap { padding:6px 10px; border-bottom:1px solid var(--border); flex-shrink:0; min-height:36px; display:flex; align-items:center; background:var(--surface); }
.ll-badge { display:inline-block; padding:4px 12px; border-radius:var(--radius-sm); border-left:3px solid var(--coral); background:var(--coral-light); font-size:11px; color:var(--coral-dark); line-height:1.4; word-break:break-word; font-weight:500; }
.ll-badge-error   { border-left-color:var(--red) !important; background:var(--red-light) !important; color:var(--red-dark) !important; }
.ll-badge-success { border-left-color:var(--green) !important; background:var(--green-light) !important; color:#15803d !important; }

/* ─── Code Panel ─────────────────────────────────────────────────────────── */
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
.ll-cx-card-bad  { background:#fef2f2; border-color:#fca5a5; color:#b91c1c; }
.ll-cx-card-label { font-size:9px; font-weight:700; text-transform:uppercase; letter-spacing:.05em; opacity:.7; margin-bottom:4px; }
.ll-cx-card-val   { font-size:13px; font-weight:800; font-family:monospace; margin-bottom:3px; }
.ll-cx-card-note  { font-size:8.5px; opacity:.75; line-height:1.3; }
.ll-note { background:#fefce8; border:1px solid #fef08a; border-left:3px solid #eab308; padding:6px 10px; font-size:10.5px; color:#854d0e; border-radius:0 4px 4px 0; margin-top:10px; margin-bottom:120px; }

.ll-footer { display:flex; align-items:center; justify-content:space-between; padding:4px 12px; background:var(--surface); border-top:1px solid var(--border); font-size:11px; color:var(--muted); font-weight:600; flex-shrink:0; }
.ll-speed-wrap { display:flex; align-items:center; gap:6px; }
.ll-speed-wrap input[type="range"] { width:80px; accent-color:var(--coral); }
</style>
