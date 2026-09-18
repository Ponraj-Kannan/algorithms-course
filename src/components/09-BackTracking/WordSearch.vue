<script setup>
import { ref, reactive, computed, onMounted, onBeforeUnmount, watch, nextTick } from 'vue';

defineProps({
  topic: { type: String, default: 'Backtracking Algorithms' },
  subTopic: { type: String, default: 'Word Search' }
});

const CODES = {
  java: [
    ['',                   'import java.util.Scanner;'],
    ['',                   ''],
    ['',                   'public class Main {'],
    ['s_entry',            '    static boolean search(char[][] board, String word, int r, int c, int idx, boolean[][] visited) {'],
    ['s_bounds',           '        if (r < 0 || r >= board.length || c < 0 || c >= board[0].length) {'],
    ['s_bounds_ret',       '            return false;'],
    ['',                   '        }'],
    ['s_check_vis',        '        if (visited[r][c]) {'],
    ['s_vis_ret',          '            return false;'],
    ['',                   '        }'],
    ['s_check_char',       '        if (board[r][c] != word.charAt(idx)) {'],
    ['s_char_ret',         '            return false;'],
    ['',                   '        }'],
    ['s_mark_vis',         '        visited[r][c] = true;'],
    ['s_check_last',       '        if (idx == word.length() - 1) {'],
    ['s_last_ret',         '            return true;'],
    ['',                   '        }'],
    ['s_init_dirs',        '        int[] dRow = {1, 0, 0, -1};'],
    ['s_init_cols',        '        int[] dCol = {0, -1, 1, 0};'],
    ['s_loop_dirs',        '        for (int i = 0; i < 4; i++) {'],
    ['s_calc_next_r',      '            int nextR = r + dRow[i];'],
    ['s_calc_next_c',      '            int nextC = c + dCol[i];'],
    ['s_check_recurse',    '            if (search(board, word, nextR, nextC, idx + 1, visited)) {'],
    ['s_recurse_ret',      '                return true;'],
    ['',                   '            }'],
    ['',                   '        }'],
    ['s_backtrack',        '        visited[r][c] = false;'],
    ['s_final_ret',        '        return false;'],
    ['',                   '    }'],
    ['',                   ''],
    ['e_entry',            '    static boolean exist(char[][] board, String word) {'],
    ['e_dims',             '        int rows = board.length;'],
    ['',                   '        int cols = board[0].length;'],
    ['e_init_visited',     '        boolean[][] visited = new boolean[rows][cols];'],
    ['e_loop_i',           '        for (int i = 0; i < rows; i++) {'],
    ['e_loop_j',           '            for (int j = 0; j < cols; j++) {'],
    ['e_check_first',      '                if (board[i][j] == word.charAt(0)) {'],
    ['e_call_search',      '                    if (search(board, word, i, j, 0, visited)) {'],
    ['e_return_true',      '                        return true;'],
    ['',                   '                    }'],
    ['',                   '                }'],
    ['',                   '            }'],
    ['',                   '        }'],
    ['e_return_false',     '        return false;'],
    ['',                   '    }'],
    ['',                   ''],
    ['',                   '    public static void main(String[] args) {'],
    ['m_scanner',          '        Scanner sc = new Scanner(System.in);'],
    ['m_read_dims',        '        int rows = sc.nextInt();'],
    ['m_read_cols',        '        int cols = sc.nextInt();'],
    ['m_alloc_board',      '        char[][] board = new char[rows][cols];'],
    ['m_read_board',       '        for (int i = 0; i < rows; i++) {'],
    ['m_read_board_inner', '            for (int j = 0; j < cols; j++) {'],
    ['m_read_board_body',  '                board[i][j] = sc.next().charAt(0);'],
    ['',                   '            }'],
    ['',                   '        }'],
    ['m_read_word',        '        String word = sc.next();'],
    ['m_call_solver',      '        boolean found = exist(board, word);'],
    ['m_print_result',     '        System.out.println(found ? "true" : "false");'],
    ['m_done',             '    }'],
    ['',                   '}']
  ],
  c: [
    ['',                   '#include <stdio.h>'],
    ['',                   '#include <string.h>'],
    ['',                   ''],
    ['s_entry',            'int search(char board[10][10], char* word, int r, int c, int idx, int rows, int cols, int visited[10][10]) {'],
    ['s_bounds',           '    if (r < 0 || r >= rows || c < 0 || c >= cols) {'],
    ['s_bounds_ret',       '        return 0;'],
    ['',                   '    }'],
    ['s_check_vis',        '    if (visited[r][c]) {'],
    ['s_vis_ret',          '        return 0;'],
    ['',                   '    }'],
    ['s_check_char',       '    if (board[r][c] != word[idx]) {'],
    ['s_char_ret',         '        return 0;'],
    ['',                   '    }'],
    ['s_mark_vis',         '    visited[r][c] = 1;'],
    ['s_check_last',       '    if (idx == (int)strlen(word) - 1) {'],
    ['s_last_ret',         '        return 1;'],
    ['',                   '    }'],
    ['s_init_dirs',        '    int dRow[4] = {1, 0, 0, -1};'],
    ['s_init_cols',        '    int dCol[4] = {0, -1, 1, 0};'],
    ['s_loop_dirs',        '    for (int i = 0; i < 4; i++) {'],
    ['s_calc_next_r',      '        int nextR = r + dRow[i];'],
    ['s_calc_next_c',      '        int nextC = c + dCol[i];'],
    ['s_check_recurse',    '        if (search(board, word, nextR, nextC, idx + 1, rows, cols, visited)) {'],
    ['s_recurse_ret',      '            return 1;'],
    ['',                   '        }'],
    ['',                   '    }'],
    ['s_backtrack',        '    visited[r][c] = 0;'],
    ['s_final_ret',        '    return 0;'],
    ['',                   '}'],
    ['',                   ''],
    ['e_entry',            'int exist(char board[10][10], char* word, int rows, int cols) {'],
    ['e_dims',             '    int visited[10][10] = {0};'],
    ['e_init_visited',     '    for (int i = 0; i < rows; i++) {'],
    ['e_loop_i',           '        for (int j = 0; j < cols; j++) {'],
    ['e_loop_j',           '            if (board[i][j] == word[0]) {'],
    ['e_check_first',      '                if (search(board, word, i, j, 0, rows, cols, visited)) {'],
    ['e_call_search',      '                    return 1;'],
    ['e_return_true',      '                }'],
    ['',                   '            }'],
    ['',                   '        }'],
    ['',                   '    }'],
    ['e_return_false',     '    return 0;'],
    ['',                   '}'],
    ['',                   ''],
    ['',                   'int main() {'],
    ['m_scanner',          '    int rows;'],
    ['m_read_dims',        '    int cols;'],
    ['m_read_cols',        '    scanf("%d %d", &rows, &cols);'],
    ['m_alloc_board',      '    char board[10][10];'],
    ['m_read_board',       '    for (int i = 0; i < rows; i++) {'],
    ['m_read_board_inner', '        for (int j = 0; j < cols; j++) {'],
    ['m_read_board_body',  '            scanf(" %c", &board[i][j]);'],
    ['',                   '        }'],
    ['',                   '    }'],
    ['m_read_word',        '    char word[100];'],
    ['',                   '    scanf("%s", word);'],
    ['m_call_solver',      '    int found = exist(board, word, rows, cols);'],
    ['m_print_result',     '    printf("%s\\n", found ? "true" : "false");'],
    ['m_done',             '    return 0;'],
    ['',                   '}']
  ],
  cpp: [
    ['',                   '#include <iostream>'],
    ['',                   '#include <vector>'],
    ['',                   '#include <string>'],
    ['',                   'using namespace std;'],
    ['',                   ''],
    ['s_entry',            'bool search(vector<vector<char>>& board, string& word, int r, int c, int idx, vector<vector<bool>>& visited) {'],
    ['s_bounds',           '    if (r < 0 || r >= (int)board.size() || c < 0 || c >= (int)board[0].size()) {'],
    ['s_bounds_ret',       '        return false;'],
    ['',                   '    }'],
    ['s_check_vis',        '    if (visited[r][c]) {'],
    ['s_vis_ret',          '        return false;'],
    ['',                   '    }'],
    ['s_check_char',       '    if (board[r][c] != word[idx]) {'],
    ['s_char_ret',         '        return false;'],
    ['',                   '    }'],
    ['s_mark_vis',         '    visited[r][c] = true;'],
    ['s_check_last',       '    if (idx == (int)word.length() - 1) {'],
    ['s_last_ret',         '        return true;'],
    ['',                   '    }'],
    ['s_init_dirs',        '    int dRow[4] = {1, 0, 0, -1};'],
    ['s_init_cols',        '    int dCol[4] = {0, -1, 1, 0};'],
    ['s_loop_dirs',        '    for (int i = 0; i < 4; i++) {'],
    ['s_calc_next_r',      '        int nextR = r + dRow[i];'],
    ['s_calc_next_c',      '        int nextC = c + dCol[i];'],
    ['s_check_recurse',    '        if (search(board, word, nextR, nextC, idx + 1, visited)) {'],
    ['s_recurse_ret',      '            return true;'],
    ['',                   '        }'],
    ['',                   '    }'],
    ['s_backtrack',        '    visited[r][c] = false;'],
    ['s_final_ret',        '    return false;'],
    ['',                   '}'],
    ['',                   ''],
    ['e_entry',            'bool exist(vector<vector<char>>& board, string word) {'],
    ['e_dims',             '    int rows = board.size();'],
    ['',                   '    int cols = board[0].size();'],
    ['e_init_visited',     '    vector<vector<bool>> visited(rows, vector<bool>(cols, false));'],
    ['e_loop_i',           '    for (int i = 0; i < rows; i++) {'],
    ['e_loop_j',           '        for (int j = 0; j < cols; j++) {'],
    ['e_check_first',      '            if (board[i][j] == word[0]) {'],
    ['e_call_search',      '                if (search(board, word, i, j, 0, visited)) {'],
    ['e_return_true',      '                    return true;'],
    ['',                   '                }'],
    ['',                   '            }'],
    ['',                   '        }'],
    ['',                   '    }'],
    ['e_return_false',     '    return false;'],
    ['',                   '}'],
    ['',                   ''],
    ['',                   'int main() {'],
    ['m_scanner',          '    int rows;'],
    ['m_read_dims',        '    int cols;'],
    ['m_read_cols',        '    cin >> rows >> cols;'],
    ['m_alloc_board',      '    vector<vector<char>> board(rows, vector<char>(cols));'],
    ['m_read_board',       '    for (int i = 0; i < rows; i++) {'],
    ['m_read_board_inner', '        for (int j = 0; j < cols; j++) {'],
    ['m_read_board_body',  '            cin >> board[i][j];'],
    ['',                   '        }'],
    ['',                   '    }'],
    ['m_read_word',        '    string word;'],
    ['',                   '    cin >> word;'],
    ['m_call_solver',      '    bool found = exist(board, word);'],
    ['m_print_result',     '    cout << (found ? "true" : "false") << "\\n";'],
    ['m_done',             '    return 0;'],
    ['',                   '}']
  ],
  python: [
    ['',                   'import sys'],
    ['',                   ''],
    ['s_entry',            'def search(board, word, r, c, idx, visited):'],
    ['s_bounds',           '    if r < 0 or r >= len(board) or c < 0 or c >= len(board[0]):'],
    ['s_bounds_ret',       '        return False'],
    ['s_check_vis',        '    if visited[r][c]:'],
    ['s_vis_ret',          '        return False'],
    ['s_check_char',       '    if board[r][c] != word[idx]:'],
    ['s_char_ret',         '        return False'],
    ['s_mark_vis',         '    visited[r][c] = True'],
    ['s_check_last',       '    if idx == len(word) - 1:'],
    ['s_last_ret',         '        return True'],
    ['s_init_dirs',        '    d_row = [1, 0, 0, -1]'],
    ['s_init_cols',        '    d_col = [0, -1, 1, 0]'],
    ['s_loop_dirs',        '    for i in range(4):'],
    ['s_calc_next_r',      '        next_r = r + d_row[i]'],
    ['s_calc_next_c',      '        next_c = c + d_col[i]'],
    ['s_check_recurse',    '        if search(board, word, next_r, next_c, idx + 1, visited):'],
    ['s_recurse_ret',      '            return True'],
    ['s_backtrack',        '    visited[r][c] = False'],
    ['s_final_ret',        '    return False'],
    ['',                   ''],
    ['e_entry',            'def exist(board, word):'],
    ['e_dims',             '    rows = len(board)'],
    ['',                   '    cols = len(board[0])'],
    ['e_init_visited',     '    visited = [[False for _ in range(cols)] for _ in range(rows)]'],
    ['e_loop_i',           '    for i in range(rows):'],
    ['e_loop_j',           '        for j in range(cols):'],
    ['e_check_first',      '            if board[i][j] == word[0]:'],
    ['e_call_search',      '                if search(board, word, i, j, 0, visited):'],
    ['e_return_true',      '                    return True'],
    ['e_return_false',     '    return False'],
    ['',                   ''],
    ['',                   'def main():'],
    ['m_scanner',          '    input_data = sys.stdin.read().split()'],
    ['',                   '    if not input_data:'],
    ['',                   '        return'],
    ['m_read_dims',        '    rows = int(input_data[0])'],
    ['m_read_cols',        '    cols = int(input_data[1])'],
    ['m_alloc_board',      '    board = []'],
    ['',                   '    idx = 2'],
    ['m_read_board',       '    for i in range(rows):'],
    ['m_read_board_inner', '        row_vals = []'],
    ['m_read_board_body',  '        for j in range(cols):'],
    ['',                   '            row_vals.append(input_data[idx])'],
    ['',                   '            idx += 1'],
    ['',                   '        board.append(row_vals)'],
    ['m_read_word',        '    word = input_data[idx]'],
    ['m_call_solver',      '    found = exist(board, word)'],
    ['m_print_result',     '    print("true" if found else "false")'],
    ['m_done',             ''],
    ['',                   'if __name__ == "__main__":'],
    ['',                   '    main()']
  ],
  javascript: [
    ['s_entry',            'function search(board, word, r, c, idx, visited) {'],
    ['s_bounds',           '    if (r < 0 || r >= board.length || c < 0 || c >= board[0].length) {'],
    ['s_bounds_ret',       '        return false;'],
    ['',                   '    }'],
    ['s_check_vis',        '    if (visited[r][c]) {'],
    ['s_vis_ret',          '        return false;'],
    ['',                   '    }'],
    ['s_check_char',       '    if (board[r][c] !== word[idx]) {'],
    ['s_char_ret',         '        return false;'],
    ['',                   '    }'],
    ['s_mark_vis',         '    visited[r][c] = true;'],
    ['s_check_last',       '    if (idx === word.length - 1) {'],
    ['s_last_ret',         '        return true;'],
    ['',                   '    }'],
    ['s_init_dirs',        '    const dRow = [1, 0, 0, -1];'],
    ['s_init_cols',        '    const dCol = [0, -1, 1, 0];'],
    ['s_loop_dirs',        '    for (let i = 0; i < 4; i++) {'],
    ['s_calc_next_r',      '        const nextR = r + dRow[i];'],
    ['s_calc_next_c',      '        const nextC = c + dCol[i];'],
    ['s_check_recurse',    '        if (search(board, word, nextR, nextC, idx + 1, visited)) {'],
    ['s_recurse_ret',      '            return true;'],
    ['',                   '        }'],
    ['',                   '    }'],
    ['s_backtrack',        '    visited[r][c] = false;'],
    ['s_final_ret',        '    return false;'],
    ['',                   '}'],
    ['',                   ''],
    ['e_entry',            'function exist(board, word) {'],
    ['e_dims',             '    const rows = board.length;'],
    ['',                   '    const cols = board[0].length;'],
    ['e_init_visited',     '    const visited = Array.from({ length: rows }, () => Array(cols).fill(false));'],
    ['e_loop_i',           '    for (let i = 0; i < rows; i++) {'],
    ['e_loop_j',           '        for (let j = 0; j < cols; j++) {'],
    ['e_check_first',      '            if (board[i][j] === word[0]) {'],
    ['e_call_search',      '                if (search(board, word, i, j, 0, visited)) {'],
    ['e_return_true',      '                    return true;'],
    ['',                   '                }'],
    ['',                   '            }'],
    ['',                   '        }'],
    ['',                   '    }'],
    ['e_return_false',     '    return false;'],
    ['',                   '}'],
    ['',                   ''],
    ['m_scanner',          'const rows = 3;'],
    ['m_read_dims',        'const cols = 4;'],
    ['m_alloc_board',      'const board = ['],
    ['m_read_board',       '    ["A", "B", "C", "E"],'],
    ['m_read_board_inner', '    ["S", "F", "C", "S"],'],
    ['m_read_board_body',  '    ["A", "D", "E", "E"]'],
    ['',                   '];'],
    ['m_read_word',        'const word = "ABCCED";'],
    ['m_call_solver',      'const found = exist(board, word);'],
    ['m_print_result',     'console.log(found);'],
    ['m_done',             '']
  ]
};

const PSEUDOCODE = [
  'function exist(board, word):',
  '    rows = board.length, cols = board[0].length',
  '    visited = 2D boolean array initialized to false',
  '    for i from 0 to rows - 1:',
  '        for j from 0 to cols - 1:',
  '            if board[i][j] == word[0]:',
  '                if search(board, word, i, j, 0, visited):',
  '                    return true',
  '    return false',
  '',
  'function search(board, word, r, c, idx, visited):',
  '    if r < 0 or r >= rows or c < 0 or c >= cols:',
  '        return false',
  '    if visited[r][c] or board[r][c] != word[idx]:',
  '        return false',
  '    visited[r][c] = true',
  '    if idx == word.length - 1:',
  '        return true',
  '    for each (dr, dc) in [(1,0), (0,-1), (0,1), (-1,0)]:',
  '        if search(board, word, r + dr, c + dc, idx + 1, visited):',
  '            return true',
  '    visited[r][c] = false // Backtrack',
  '    return false'
];

function buildSteps(rows, cols, board, word) {
  const steps = [];

  function cloneBoard() {
    return board.map(row => [...row]);
  }

  function frame(name, rows) {
    return { title: name, rows };
  }

  // 1. Scanner & main read setup
  steps.push({
    badge: `Scanner sc = new Scanner(System.in); → Initializing standard input reader.`,
    code: 'm_scanner',
    vars: [frame('main()', [['rows', '?'], ['cols', '?'], ['word', `"${word}"`]])],
    board: cloneBoard(),
    activeCells: [],
    currentCell: null,
    testCell: null,
    rows, cols, loopI: null, loopJ: null, r: -1, c: -1, k: -1, char: '', word,
    found: false
  });

  steps.push({
    badge: `int rows = sc.nextInt(); → Reading number of rows = ${rows}.`,
    code: 'm_read_dims',
    vars: [frame('main()', [['rows', String(rows)], ['cols', '?'], ['word', `"${word}"`]])],
    board: cloneBoard(),
    activeCells: [], currentCell: null, testCell: null,
    rows, cols, loopI: null, loopJ: null, r: -1, c: -1, k: -1, char: '', word,
    found: false
  });

  steps.push({
    badge: `int cols = sc.nextInt(); → Reading number of columns = ${cols}.`,
    code: 'm_read_cols',
    vars: [frame('main()', [['rows', String(rows)], ['cols', String(cols)], ['word', `"${word}"`]])],
    board: cloneBoard(),
    activeCells: [], currentCell: null, testCell: null,
    rows, cols, loopI: null, loopJ: null, r: -1, c: -1, k: -1, char: '', word,
    found: false
  });

  steps.push({
    badge: `char[][] board = new char[${rows}][${cols}]; → Allocating ${rows}×${cols} character grid.`,
    code: 'm_alloc_board',
    vars: [frame('main()', [['rows', String(rows)], ['cols', String(cols)], ['word', `"${word}"`]])],
    board: cloneBoard(),
    activeCells: [], currentCell: null, testCell: null,
    rows, cols, loopI: null, loopJ: null, r: -1, c: -1, k: -1, char: '', word,
    found: false
  });

  // Reading board cells with nested loop
  for (let ri = 0; ri < rows; ri++) {
    steps.push({
      badge: `for (int i = ${ri}; i < ${rows}; i++) → (${ri} < ${rows}) is TRUE. Reading row ${ri}.`,
      code: 'm_read_board',
      vars: [frame('main()', [['i', String(ri)], ['j', '?']])],
      board: cloneBoard(),
      activeCells: [], currentCell: null, testCell: null,
      rows, cols, loopI: ri, loopJ: null, r: -1, c: -1, k: -1, char: '', word,
      found: false
    });

    for (let cj = 0; cj < cols; cj++) {
      steps.push({
        badge: `for (int j = ${cj}; j < ${cols}; j++) → (${cj} < ${cols}) is TRUE. Reading cell (${ri}, ${cj}).`,
        code: 'm_read_board_inner',
        vars: [frame('main()', [['i', String(ri)], ['j', String(cj)]])],
        board: cloneBoard(),
        activeCells: [], currentCell: null, testCell: { r: ri, c: cj, status: 'testing' },
        rows, cols, loopI: ri, loopJ: cj, r: ri, c: cj, k: -1, char: '', word,
        found: false
      });

      steps.push({
        badge: `board[${ri}][${cj}] = '${board[ri][cj]}'; → Loaded character '${board[ri][cj]}'.`,
        code: 'm_read_board_body',
        vars: [frame('main()', [['i', String(ri)], ['j', String(cj)], [`board[${ri}][${cj}]`, `'${board[ri][cj]}'`]])],
        board: cloneBoard(),
        activeCells: [], currentCell: null, testCell: { r: ri, c: cj, status: 'match' },
        rows, cols, loopI: ri, loopJ: cj, r: ri, c: cj, k: -1, char: '', word,
        found: false
      });
    }

    // Inner loop termination
    steps.push({
      badge: `for (int j = ${cols}; j < ${cols}; j++) → (${cols} < ${cols}) is FALSE. Row ${ri} input complete.`,
      code: 'm_read_board_inner',
      vars: [frame('main()', [['i', String(ri)], ['j', String(cols)]])],
      board: cloneBoard(),
      activeCells: [], currentCell: null, testCell: null,
      rows, cols, loopI: ri, loopJ: cols, r: -1, c: -1, k: -1, char: '', word,
      found: false
    });
  }

  // Outer loop termination
  steps.push({
    badge: `for (int i = ${rows}; i < ${rows}; i++) → (${rows} < ${rows}) is FALSE. All grid cells loaded.`,
    code: 'm_read_board',
    vars: [frame('main()', [['i', String(rows)], ['j', String(cols)]])],
    board: cloneBoard(),
    activeCells: [], currentCell: null, testCell: null,
    rows, cols, loopI: rows, loopJ: null, r: -1, c: -1, k: -1, char: '', word,
    found: false
  });

  steps.push({
    badge: `String word = sc.next(); → Target search word: "${word}".`,
    code: 'm_read_word',
    vars: [frame('main()', [['word', `"${word}"`]])],
    board: cloneBoard(),
    activeCells: [], currentCell: null, testCell: null,
    rows, cols, loopI: null, loopJ: null, r: -1, c: -1, k: -1, char: '', word,
    found: false
  });

  steps.push({
    badge: `boolean found = exist(board, "${word}"); → Calling exist() to start backtracking word search.`,
    code: 'm_call_solver',
    vars: [frame('main()', [['word', `"${word}"`]])],
    board: cloneBoard(),
    activeCells: [], currentCell: null, testCell: null,
    rows, cols, loopI: null, loopJ: null, r: -1, c: -1, k: -1, char: '', word,
    found: false
  });

  // Inside exist()
  steps.push({
    badge: `exist(board, word="${word}") entry → Searching for occurrences of start letter '${word[0]}'.`,
    code: 'e_entry',
    vars: [frame('main()', [['word', `"${word}"`]]), frame('exist()', [['word', `"${word}"`]])],
    board: cloneBoard(),
    activeCells: [], currentCell: null, testCell: null,
    rows, cols, loopI: null, loopJ: null, r: -1, c: -1, k: 0, char: word[0] || '', word,
    found: false
  });

  steps.push({
    badge: `int rows = board.length; int cols = board[0].length; → Dimensions: ${rows}×${cols}.`,
    code: 'e_dims',
    vars: [frame('main()', [['word', `"${word}"`]]), frame('exist()', [['rows', String(rows)], ['cols', String(cols)]])],
    board: cloneBoard(),
    activeCells: [], currentCell: null, testCell: null,
    rows, cols, loopI: null, loopJ: null, r: -1, c: -1, k: 0, char: word[0] || '', word,
    found: false
  });

  steps.push({
    badge: `boolean[][] visited = new boolean[${rows}][${cols}]; → Creating visited matrix.`,
    code: 'e_init_visited',
    vars: [frame('main()', [['word', `"${word}"`]]), frame('exist()', [['rows', String(rows)], ['cols', String(cols)], ['visited', `${rows}x${cols}`]])],
    board: cloneBoard(),
    activeCells: [], currentCell: null, testCell: null,
    rows, cols, loopI: null, loopJ: null, r: -1, c: -1, k: 0, char: word[0] || '', word,
    found: false
  });

  const visited = Array.from({ length: rows }, () => Array(cols).fill(false));
  const callStack = [];
  const currentPath = [];
  let wordFound = false;
  let finalPath = [];

  function getStackSnapshot(extra = []) {
    const list = [
      frame('main()', [['word', `"${word}"`]]),
      frame('exist()', [['rows', String(rows)], ['cols', String(cols)]])
    ];
    for (const f of callStack) {
      list.push(frame(f.name, f.args));
    }
    if (extra.length && list.length) {
      const top = list[list.length - 1];
      top.rows = [...top.rows, ...extra];
    }
    return list;
  }

  const dRow = [1, 0, 0, -1];
  const dCol = [0, -1, 1, 0];
  const dNames = ['Down(+1, 0)', 'Left(0, -1)', 'Right(0, +1)', 'Up(-1, 0)'];

  function simulateSearch(r, c, idx) {
    callStack.push({
      name: `search(r=${r}, c=${c}, idx=${idx})`,
      args: [['r', String(r)], ['c', String(c)], ['idx', String(idx)], ['target', `'${word[idx]}'`]]
    });

    steps.push({
      badge: `search(r=${r}, c=${c}, idx=${idx}) entry → Checking if square (${r}, ${c}) matches target letter '${word[idx]}'.`,
      code: 's_entry',
      vars: getStackSnapshot(),
      board: cloneBoard(),
      activeCells: [...currentPath],
      currentCell: { r, c },
      testCell: { r, c, status: 'testing' },
      rows, cols, loopI: null, loopJ: null, r, c, k: idx, char: word[idx], word,
      found: false
    });

    // Boundary check
    const outOfBounds = (r < 0 || r >= rows || c < 0 || c >= cols);
    steps.push({
      badge: `Boundary check: if (r < 0 || r >= ${rows} || c < 0 || c >= ${cols}) → (${r} < 0 || ${r} >= ${rows} || ${c} < 0 || ${c} >= ${cols}) → ${outOfBounds ? 'TRUE (OutOfBounds!)' : 'FALSE (Inside grid)'}`,
      code: 's_bounds',
      vars: getStackSnapshot([['bounds', outOfBounds ? 'INVALID' : 'VALID']]),
      board: cloneBoard(),
      activeCells: [...currentPath],
      currentCell: { r, c },
      testCell: { r, c, status: outOfBounds ? 'conflict' : 'testing' },
      rows, cols, loopI: null, loopJ: null, r, c, k: idx, char: word[idx] || '', word,
      found: false
    });

    if (outOfBounds) {
      steps.push({
        badge: `❌ Target position (${r}, ${c}) is outside the board! Executing return false;`,
        code: 's_bounds_ret',
        vars: getStackSnapshot([['result', 'false (OutOfBounds)']]),
        board: cloneBoard(),
        activeCells: [...currentPath],
        currentCell: null,
        testCell: { r, c, status: 'conflict' },
        rows, cols, loopI: null, loopJ: null, r, c, k: idx, char: word[idx] || '', word,
        found: false
      });
      callStack.pop();
      return false;
    }

    // Visited check
    const isVisited = visited[r][c];
    steps.push({
      badge: `Visited check: if (visited[${r}][${c}]) → ${isVisited ? 'TRUE (Already used in current path!)' : 'FALSE (Not used yet)'}`,
      code: 's_check_vis',
      vars: getStackSnapshot([['visited', String(isVisited)]]),
      board: cloneBoard(),
      activeCells: [...currentPath],
      currentCell: { r, c },
      testCell: { r, c, status: isVisited ? 'conflict' : 'testing' },
      rows, cols, loopI: null, loopJ: null, r, c, k: idx, char: word[idx], word,
      found: false
    });

    if (isVisited) {
      steps.push({
        badge: `❌ Cell (${r}, ${c}) is already part of the active path! Letter cannot be reused. Executing return false;`,
        code: 's_vis_ret',
        vars: getStackSnapshot([['result', 'false (AlreadyVisited)']]),
        board: cloneBoard(),
        activeCells: [...currentPath],
        currentCell: { r, c },
        testCell: { r, c, status: 'conflict' },
        rows, cols, loopI: null, loopJ: null, r, c, k: idx, char: word[idx], word,
        found: false
      });
      callStack.pop();
      return false;
    }

    // Character match check
    const cellChar = board[r][c];
    const targetChar = word[idx];
    const isCharMismatch = (cellChar !== targetChar);
    steps.push({
      badge: `Character match check: if (board[${r}][${c}] != word[${idx}]) → ('${cellChar}' != '${targetChar}') → ${isCharMismatch ? 'TRUE (Letter Mismatch!)' : 'FALSE (Letters MATCH!)'}`,
      code: 's_check_char',
      vars: getStackSnapshot([['cell_char', `'${cellChar}'`], ['target_char', `'${targetChar}'`]]),
      board: cloneBoard(),
      activeCells: [...currentPath],
      currentCell: { r, c },
      testCell: { r, c, status: isCharMismatch ? 'conflict' : 'match' },
      rows, cols, loopI: null, loopJ: null, r, c, k: idx, char: targetChar, word,
      found: false
    });

    if (isCharMismatch) {
      steps.push({
        badge: `❌ Cell character '${cellChar}' does not match expected target '${targetChar}' at index ${idx}. Executing return false;`,
        code: 's_char_ret',
        vars: getStackSnapshot([['result', 'false (Mismatch)']]),
        board: cloneBoard(),
        activeCells: [...currentPath],
        currentCell: { r, c },
        testCell: { r, c, status: 'conflict' },
        rows, cols, loopI: null, loopJ: null, r, c, k: idx, char: targetChar, word,
        found: false
      });
      callStack.pop();
      return false;
    }

    // Mark visited and add to current path
    visited[r][c] = true;
    currentPath.push({ r, c, k: idx, char: cellChar });

    steps.push({
      badge: `visited[${r}][${c}] = true; → Letter '${cellChar}' matched! Adding (${r}, ${c}) as letter #${idx + 1} of "${word}".`,
      code: 's_mark_vis',
      vars: getStackSnapshot([['matched_len', `${idx + 1}/${word.length}`]]),
      board: cloneBoard(),
      activeCells: [...currentPath],
      currentCell: { r, c },
      testCell: null,
      rows, cols, loopI: null, loopJ: null, r, c, k: idx, char: targetChar, word,
      found: false
    });

    // Check if word completed
    const isLastChar = (idx === word.length - 1);
    steps.push({
      badge: `Checking if word complete: if (idx == word.length() - 1) → (${idx} == ${word.length - 1}) → ${isLastChar ? 'TRUE! COMPLETE WORD FOUND!' : 'FALSE (more letters needed)'}`,
      code: 's_check_last',
      vars: getStackSnapshot([['complete', String(isLastChar)]]),
      board: cloneBoard(),
      activeCells: [...currentPath],
      currentCell: { r, c },
      testCell: null,
      rows, cols, loopI: null, loopJ: null, r, c, k: idx, char: targetChar, word,
      found: isLastChar
    });

    if (isLastChar) {
      wordFound = true;
      finalPath = [...currentPath];
      steps.push({
        badge: `🎉 SUCCESS! Entire word "${word}" found on the board! Executing return true;`,
        code: 's_last_ret',
        vars: getStackSnapshot([['result', 'true (WORD_FOUND)']]),
        board: cloneBoard(),
        activeCells: [...currentPath],
        currentCell: { r, c },
        testCell: null,
        rows, cols, loopI: null, loopJ: null, r, c, k: idx, char: targetChar, word,
        found: true
      });
      callStack.pop();
      return true;
    }

    // Direction vectors
    steps.push({
      badge: `int[] dRow = {1, 0, 0, -1}; → Row offsets for Down(+1), Left(0), Right(0), Up(-1).`,
      code: 's_init_dirs',
      vars: getStackSnapshot([['dRow', '{1, 0, 0, -1}']]),
      board: cloneBoard(),
      activeCells: [...currentPath],
      currentCell: { r, c },
      testCell: null,
      rows, cols, loopI: null, loopJ: null, r, c, k: idx, char: targetChar, word,
      found: false
    });

    steps.push({
      badge: `int[] dCol = {0, -1, 1, 0}; → Column offsets for Down(0), Left(-1), Right(+1), Up(0).`,
      code: 's_init_cols',
      vars: getStackSnapshot([['dCol', '{0, -1, 1, 0}']]),
      board: cloneBoard(),
      activeCells: [...currentPath],
      currentCell: { r, c },
      testCell: null,
      rows, cols, loopI: null, loopJ: null, r, c, k: idx, char: targetChar, word,
      found: false
    });

    // 4 directions exploration
    for (let di = 0; di < 4; di++) {
      const nextR = r + dRow[di];
      const nextC = c + dCol[di];
      const dirName = dNames[di];

      steps.push({
        badge: `Direction loop: for (int i = ${di}; i < 4; i++) → (${di} < 4) is TRUE. Exploring ${dirName}.`,
        code: 's_loop_dirs',
        vars: getStackSnapshot([['i', String(di)], ['dir', dirName]]),
        board: cloneBoard(),
        activeCells: [...currentPath],
        currentCell: { r, c },
        testCell: null,
        rows, cols, loopI: di, loopJ: null, r, c, k: idx, char: targetChar, word,
        found: false
      });

      steps.push({
        badge: `int nextR = ${r} + dRow[${di}] = ${r} + (${dRow[di]}) = ${nextR};`,
        code: 's_calc_next_r',
        vars: getStackSnapshot([['i', String(di)], ['dir', dirName], ['nextR', String(nextR)]]),
        board: cloneBoard(),
        activeCells: [...currentPath],
        currentCell: { r, c },
        testCell: null,
        rows, cols, loopI: di, loopJ: null, r, c, k: idx, char: targetChar, word,
        found: false
      });

      steps.push({
        badge: `int nextC = ${c} + dCol[${di}] = ${c} + (${dCol[di]}) = ${nextC}; → Target cell is (${nextR}, ${nextC}).`,
        code: 's_calc_next_c',
        vars: getStackSnapshot([['i', String(di)], ['dir', dirName], ['nextR', String(nextR)], ['nextC', String(nextC)]]),
        board: cloneBoard(),
        activeCells: [...currentPath],
        currentCell: { r, c },
        testCell: nextR >= 0 && nextR < rows && nextC >= 0 && nextC < cols ? { r: nextR, c: nextC, status: 'testing' } : null,
        rows, cols, loopI: di, loopJ: null, r, c, k: idx, char: targetChar, word,
        found: false
      });

      steps.push({
        badge: `if (search(board, word, nextR=${nextR}, nextC=${nextC}, idx=${idx + 1}, visited)) → Recursing to find letter #${idx + 2} ('${word[idx + 1]}').`,
        code: 's_check_recurse',
        vars: getStackSnapshot([['i', String(di)], ['dir', dirName], ['nextR', String(nextR)], ['nextC', String(nextC)], ['nextIdx', String(idx + 1)]]),
        board: cloneBoard(),
        activeCells: [...currentPath],
        currentCell: { r, c },
        testCell: nextR >= 0 && nextR < rows && nextC >= 0 && nextC < cols ? { r: nextR, c: nextC, status: 'testing' } : null,
        rows, cols, loopI: di, loopJ: null, r, c, k: idx, char: targetChar, word,
        found: false
      });

      const branchSuccess = simulateSearch(nextR, nextC, idx + 1);
      if (branchSuccess) {
        steps.push({
          badge: `return true; → Branch succeeded! Propagating true back to caller.`,
          code: 's_recurse_ret',
          vars: getStackSnapshot([['result', 'true (BranchSuccess)']]),
          board: cloneBoard(),
          activeCells: [...finalPath],
          currentCell: { r, c },
          testCell: null,
          rows, cols, loopI: di, loopJ: null, r, c, k: idx, char: targetChar, word,
          found: true
        });
        callStack.pop();
        return true;
      }
    }

    // Direction loop exit check (di = 4)
    steps.push({
      badge: `Direction loop finished: for (int i = 4; i < 4; i++) → (4 < 4) is FALSE. All 4 directions explored from (${r}, ${c}).`,
      code: 's_loop_dirs',
      vars: getStackSnapshot([['i', '4'], ['loop', 'TERMINATED']]),
      board: cloneBoard(),
      activeCells: [...currentPath],
      currentCell: { r, c },
      testCell: null,
      rows, cols, loopI: 4, loopJ: null, r, c, k: idx, char: targetChar, word,
      found: false
    });

    // Backtrack
    visited[r][c] = false;
    currentPath.pop();

    steps.push({
      badge: `visited[${r}][${c}] = false; → Backtracking: Unmarking cell (${r}, ${c}) as no direction completed "${word}".`,
      code: 's_backtrack',
      vars: getStackSnapshot([['backtrack', `(${r}, ${c})`]]),
      board: cloneBoard(),
      activeCells: [...currentPath],
      currentCell: { r, c },
      testCell: { r, c, status: 'conflict' },
      rows, cols, loopI: null, loopJ: null, r, c, k: idx, char: targetChar, word,
      found: false
    });

    steps.push({
      badge: `return false; → Returning false from search at (${r}, ${c}).`,
      code: 's_final_ret',
      vars: getStackSnapshot([['result', 'false (Exhausted)']]),
      board: cloneBoard(),
      activeCells: [...currentPath],
      currentCell: null,
      testCell: null,
      rows, cols, loopI: null, loopJ: null, r, c, k: idx, char: targetChar, word,
      found: false
    });

    callStack.pop();
    return false;
  }

  // Scanning grid in exist()
  for (let i = 0; i < rows; i++) {
    steps.push({
      badge: `for (int i = ${i}; i < ${rows}; i++) → (${i} < ${rows}) is TRUE. Scanning row ${i}.`,
      code: 'e_loop_i',
      vars: [frame('main()', [['word', `"${word}"`]]), frame('exist()', [['i', String(i)], ['j', '?']])],
      board: cloneBoard(),
      activeCells: [], currentCell: null, testCell: null,
      rows, cols, loopI: i, loopJ: null, r: -1, c: -1, k: 0, char: word[0] || '', word,
      found: false
    });

    for (let j = 0; j < cols; j++) {
      steps.push({
        badge: `for (int j = ${j}; j < ${cols}; j++) → (${j} < ${cols}) is TRUE. Examining cell (${i}, ${j}).`,
        code: 'e_loop_j',
        vars: [frame('main()', [['word', `"${word}"`]]), frame('exist()', [['i', String(i)], ['j', String(j)]])],
        board: cloneBoard(),
        activeCells: [], currentCell: null, testCell: { r: i, c: j, status: 'testing' },
        rows, cols, loopI: i, loopJ: j, r: i, c: j, k: 0, char: word[0] || '', word,
        found: false
      });

      const firstCharMatches = (board[i][j] === word[0]);
      steps.push({
        badge: `if (board[${i}][${j}] == word.charAt(0)) → ('${board[i][j]}' == '${word[0]}') → ${firstCharMatches ? 'TRUE! Starting letter MATCHES!' : 'FALSE (Skip to next cell)'}`,
        code: 'e_check_first',
        vars: [frame('main()', [['word', `"${word}"`]]), frame('exist()', [['i', String(i)], ['j', String(j)], ['match', String(firstCharMatches)]])],
        board: cloneBoard(),
        activeCells: [], currentCell: null, testCell: { r: i, c: j, status: firstCharMatches ? 'match' : 'conflict' },
        rows, cols, loopI: i, loopJ: j, r: i, c: j, k: 0, char: word[0] || '', word,
        found: false
      });

      if (firstCharMatches) {
        steps.push({
          badge: `if (search(board, word, ${i}, ${j}, 0, visited)) → Launching backtracking DFS from (${i}, ${j}).`,
          code: 'e_call_search',
          vars: [frame('main()', [['word', `"${word}"`]]), frame('exist()', [['i', String(i)], ['j', String(j)]])],
          board: cloneBoard(),
          activeCells: [], currentCell: { r: i, c: j }, testCell: { r: i, c: j, status: 'match' },
          rows, cols, loopI: i, loopJ: j, r: i, c: j, k: 0, char: word[0] || '', word,
          found: false
        });

        const foundFromHere = simulateSearch(i, j, 0);
        if (foundFromHere) {
          steps.push({
            badge: `return true; → Word "${word}" successfully found on board starting at (${i}, ${j})!`,
            code: 'e_return_true',
            vars: [frame('main()', [['word', `"${word}"`]]), frame('exist()', [['result', 'true']])],
            board: cloneBoard(),
            activeCells: [...finalPath], currentCell: null, testCell: null,
            rows, cols, loopI: i, loopJ: j, r: -1, c: -1, k: word.length - 1, char: '', word,
            found: true
          });
          wordFound = true;
          break;
        }
      }
    }
    if (wordFound) break;

    // Inner loop termination
    steps.push({
      badge: `for (int j = ${cols}; j < ${cols}; j++) → (${cols} < ${cols}) is FALSE. Row ${i} scan finished.`,
      code: 'e_loop_j',
      vars: [frame('main()', [['word', `"${word}"`]]), frame('exist()', [['i', String(i)], ['j', String(cols)]])],
      board: cloneBoard(),
      activeCells: [], currentCell: null, testCell: null,
      rows, cols, loopI: i, loopJ: cols, r: -1, c: -1, k: 0, char: word[0] || '', word,
      found: false
    });
  }

  if (!wordFound) {
    // Outer loop termination
    steps.push({
      badge: `for (int i = ${rows}; i < ${rows}; i++) → (${rows} < ${rows}) is FALSE. Entire board scanned, word not found.`,
      code: 'e_loop_i',
      vars: [frame('main()', [['word', `"${word}"`]]), frame('exist()', [['i', String(rows)]])],
      board: cloneBoard(),
      activeCells: [], currentCell: null, testCell: null,
      rows, cols, loopI: rows, loopJ: null, r: -1, c: -1, k: -1, char: '', word,
      found: false
    });

    steps.push({
      badge: `return false; → Word "${word}" does NOT exist on this board.`,
      code: 'e_return_false',
      vars: [frame('main()', [['word', `"${word}"`]]), frame('exist()', [['result', 'false']])],
      board: cloneBoard(),
      activeCells: [], currentCell: null, testCell: null,
      rows, cols, loopI: null, loopJ: null, r: -1, c: -1, k: -1, char: '', word,
      found: false
    });
  }

  // Print result in main()
  steps.push({
    badge: `System.out.println(found ? "true" : "false"); → Printing final answer: ${wordFound ? 'true' : 'false'}.`,
    code: 'm_print_result',
    vars: [frame('main()', [['found', wordFound ? 'true' : 'false']])],
    board: cloneBoard(),
    activeCells: wordFound ? [...finalPath] : [], currentCell: null, testCell: null,
    rows, cols, loopI: null, loopJ: null, r: -1, c: -1, k: wordFound ? word.length - 1 : -1, char: '', word,
    found: wordFound
  });

  steps.push({
    badge: `Program execution finished. Result: ${wordFound ? 'TRUE (Word Found)' : 'FALSE (Word Not Found)'}.`,
    code: 'm_done',
    vars: [frame('main()', [['status', 'FINISHED']])],
    board: cloneBoard(),
    activeCells: wordFound ? [...finalPath] : [], currentCell: null, testCell: null,
    rows, cols, loopI: null, loopJ: null, r: -1, c: -1, k: wordFound ? word.length - 1 : -1, char: '', word,
    found: wordFound
  });

  return steps;
}

// Reactive state
const inputRows = ref(3);
const inputCols = ref(4);
const inputWord = ref('ABCCED');
const boardInputStr = ref(JSON.stringify([
  ['A', 'B', 'C', 'E'],
  ['S', 'F', 'C', 'S'],
  ['A', 'D', 'E', 'E']
]));

const activeBoard = ref([
  ['A', 'B', 'C', 'E'],
  ['S', 'F', 'C', 'S'],
  ['A', 'D', 'E', 'E']
]);
const activeWord = ref('ABCCED');

const si = ref(0);
const playing = ref(false);
const speed = ref(800);
const lang = ref('java');
const rightTab = ref('code');

const stepsData = reactive({ steps: buildSteps(3, 4, activeBoard.value, activeWord.value) });
const steps = computed(() => stepsData.steps);
const s = computed(() => steps.value[Math.max(0, Math.min(si.value, steps.value.length - 1))] || {});
const codeLines = computed(() => CODES[lang.value] || []);

let playTimer = null;

function parseBoardInput(str, expectedR, expectedC) {
  try {
    const parsed = JSON.parse(str);
    if (!Array.isArray(parsed) || parsed.length !== expectedR) return null;
    for (let i = 0; i < expectedR; i++) {
      if (!Array.isArray(parsed[i]) || parsed[i].length !== expectedC) return null;
      for (let j = 0; j < expectedC; j++) {
        if (typeof parsed[i][j] !== 'string' || parsed[i][j].length === 0) return null;
      }
    }
    return parsed.map(row => row.map(c => String(c).charAt(0).toUpperCase()));
  } catch (e) {
    return null;
  }
}

function applyInput() {
  const r = parseInt(inputRows.value, 10);
  const c = parseInt(inputCols.value, 10);
  const w = (inputWord.value || '').trim().toUpperCase();

  if (isNaN(r) || r < 1 || r > 6 || isNaN(c) || c < 1 || c > 6) {
    alert('Please enter rows and columns between 1 and 6 for clear visualization.');
    return;
  }
  if (!w) {
    alert('Please enter a target word to search.');
    return;
  }

  let matrix = parseBoardInput(boardInputStr.value, r, c);
  if (!matrix) {
    matrix = Array.from({ length: r }, () => Array.from({ length: c }, () => 'A'));
    boardInputStr.value = JSON.stringify(matrix);
  }

  activeBoard.value = matrix;
  activeWord.value = w;
  inputWord.value = w;
  playing.value = false;
  stepsData.steps = buildSteps(r, c, activeBoard.value, activeWord.value);
  si.value = 0;
}

function loadPreset(presetType) {
  if (presetType === 'ex1') {
    inputRows.value = 3;
    inputCols.value = 4;
    inputWord.value = 'ABCCED';
    boardInputStr.value = JSON.stringify([
      ['A', 'B', 'C', 'E'],
      ['S', 'F', 'C', 'S'],
      ['A', 'D', 'E', 'E']
    ]);
  } else if (presetType === 'ex2') {
    inputRows.value = 3;
    inputCols.value = 4;
    inputWord.value = 'SEE';
    boardInputStr.value = JSON.stringify([
      ['A', 'B', 'C', 'E'],
      ['S', 'F', 'C', 'S'],
      ['A', 'D', 'E', 'E']
    ]);
  } else if (presetType === 'ex3') {
    inputRows.value = 3;
    inputCols.value = 4;
    inputWord.value = 'ABCB';
    boardInputStr.value = JSON.stringify([
      ['A', 'B', 'C', 'E'],
      ['S', 'F', 'C', 'S'],
      ['A', 'D', 'E', 'E']
    ]);
  } else if (presetType === '3x3') {
    inputRows.value = 3;
    inputCols.value = 3;
    inputWord.value = 'CATS';
    boardInputStr.value = JSON.stringify([
      ['C', 'A', 'T'],
      ['A', 'T', 'S'],
      ['R', 'A', 'T']
    ]);
  }
  applyInput();
}

function cycleCellLetter(r, c) {
  if (playing.value) return;
  const current = activeBoard.value[r][c];
  const charCode = current.charCodeAt(0);
  const nextChar = charCode >= 90 ? 'A' : String.fromCharCode(charCode + 1);
  activeBoard.value[r][c] = nextChar;
  boardInputStr.value = JSON.stringify(activeBoard.value);
  applyInput();
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

// Display helpers
function getCellPathIndex(r, c) {
  if (!s.value.activeCells) return -1;
  const idx = s.value.activeCells.findIndex(cell => cell.r === r && cell.c === c);
  return idx;
}

function isCurrentHead(r, c) {
  return s.value.currentCell && s.value.currentCell.r === r && s.value.currentCell.c === c;
}

function getTestStatus(r, c) {
  if (s.value.testCell && s.value.testCell.r === r && s.value.testCell.c === c) {
    return s.value.testCell.status;
  }
  return null;
}

// Resizing split panes
const mainRef = ref(null);
const leftColRef = ref(null);
const vizHeight = ref(290);
const tableHeight = ref(60);
const leftWidth = ref(50);

const hResizerRef = ref(null);
const vizResizerRef = ref(null);
const tableResizerRef = ref(null);

let isHDragging = false;
let isVizDragging = false;
let isTableDragging = false;
let startX = 0;
let startLeftW = 54;
let startY = 0;
let startVizH = 380;
let startTableH = 130;

function onHMouseDown(e) {
  isHDragging = true;
  startX = e.clientX;
  startLeftW = leftWidth.value;
  document.body.style.cursor = 'col-resize';
  document.body.style.userSelect = 'none';
}

function onVizMouseDown(e) {
  isVizDragging = true;
  startY = e.clientY;
  startVizH = vizHeight.value;
  document.body.style.cursor = 'row-resize';
  document.body.style.userSelect = 'none';
}

function onTableMouseDown(e) {
  isTableDragging = true;
  startY = e.clientY;
  startTableH = tableHeight.value;
  document.body.style.cursor = 'row-resize';
  document.body.style.userSelect = 'none';
}

function onMouseMove(e) {
  if (isHDragging && mainRef.value) {
    const totalW = mainRef.value.getBoundingClientRect().width;
    const dx = e.clientX - startX;
    const newPct = startLeftW + (dx / totalW) * 100;
    leftWidth.value = Math.max(30, Math.min(75, newPct));
  }
  if (isVizDragging) {
    const dy = e.clientY - startY;
    vizHeight.value = Math.max(220, Math.min(520, startVizH + dy));
  }
  if (isTableDragging) {
    const dy = e.clientY - startY;
    tableHeight.value = Math.max(80, Math.min(260, startTableH + dy));
  }
}

function onMouseUp() {
  if (isHDragging || isVizDragging || isTableDragging) {
    isHDragging = false;
    isVizDragging = false;
    isTableDragging = false;
    document.body.style.cursor = '';
    document.body.style.userSelect = '';
  }
}

function onChipsWheel(e) {
  if (Math.abs(e.deltaY) > Math.abs(e.deltaX)) {
    e.currentTarget.scrollLeft += e.deltaY;
  }
}

onMounted(() => {
  window.addEventListener('mousemove', onMouseMove);
  window.addEventListener('mouseup', onMouseUp);
  if (hResizerRef.value) hResizerRef.value.addEventListener('mousedown', onHMouseDown);
  if (vizResizerRef.value) vizResizerRef.value.addEventListener('mousedown', onVizMouseDown);
  if (tableResizerRef.value) tableResizerRef.value.addEventListener('mousedown', onTableMouseDown);
  if (typeof window !== 'undefined') window.scrollTo(0, 0);
});

onBeforeUnmount(() => {
  clearTimeout(playTimer);
  window.removeEventListener('mousemove', onMouseMove);
  window.removeEventListener('mouseup', onMouseUp);
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
                min="1"
                max="6"
                v-model.number="inputRows"
                class="ll-text-input"
                @keyup.enter="applyInput"
                style="width: 38px;"
              />
            </div>

            <div class="ll-input-group">
              <label>Cols:</label>
              <input
                type="number"
                min="1"
                max="6"
                v-model.number="inputCols"
                class="ll-text-input"
                @keyup.enter="applyInput"
                style="width: 38px;"
              />
            </div>

            <div class="ll-input-group">
              <label>Word:</label>
              <input
                type="text"
                v-model="inputWord"
                class="ll-text-input"
                @keyup.enter="applyInput"
                placeholder="ABCCED"
                style="width: 85px; font-weight: 500;"
              />
            </div>

            <div class="ll-input-group">
              <label>board[][]:</label>
              <input
                type="text"
                v-model="boardInputStr"
                class="ll-text-input"
                @keyup.enter="applyInput"
                title="Input 2D character array"
                style="width: 200px; font-size: 10.5px;"
              />
            </div>

            <!-- <div class="ll-preset-group">
              <button class="ll-preset-btn" @click="loadPreset('ex1')">Ex 1</button>
              <button class="ll-preset-btn" @click="loadPreset('ex2')">Ex 2</button>
              <button class="ll-preset-btn" @click="loadPreset('ex3')">Ex 3</button>
              <button class="ll-preset-btn" @click="loadPreset('3x3')">3x3</button>
            </div> -->

            <button class="ll-viz-btn" @click="applyInput">&#9654; Visualize</button>

            <!-- Playback Navigation Controls -->
            <div class="ll-nav-controls">
              <button class="ll-nav-btn" title="First step" @click="si = 0">&#171;</button>
              <button class="ll-nav-btn" title="Previous step" @click="stepBy(-1)">&#8249; Prev</button>
              <button class="ll-play-btn" @click="togglePlay">
                {{ playing ? '⏸ Pause' : '▶ Play' }}
              </button>
              <button class="ll-nav-btn" title="Next step" @click="stepBy(1)">Next &#8250;</button>
              <button class="ll-nav-btn" title="Last step" @click="si = steps.length - 1">&#187;</button>
            </div>
          </div>

          <div class="ll-main" ref="mainRef">
            <!-- Left Visualization Column -->
            <div class="ll-left-col" ref="leftColRef" :style="{ width: leftWidth + '%' }">
              <div class="ll-viz-wrap" :style="{ height: vizHeight + 'px' }">
                <div class="ll-perm-area">
                  <!-- Real-time Stats Chips (Single compact row) -->
                  <div class="ll-ptrs ll-ptrs-compact" @wheel.passive="onChipsWheel">
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">rows</span><b class="ll-c-blue">{{ s.rows || inputRows }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">cols</span><b class="ll-c-blue">{{ s.cols || inputCols }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">i</span><b class="ll-c-purple">{{ s.loopI !== null && s.loopI !== undefined ? s.loopI : '—' }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">j</span><b class="ll-c-purple">{{ s.loopJ !== null && s.loopJ !== undefined ? s.loopJ : '—' }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">r</span><b class="ll-c-orange">{{ s.r !== undefined && s.r >= 0 ? s.r : '—' }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">c</span><b class="ll-c-orange">{{ s.c !== undefined && s.c >= 0 ? s.c : '—' }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">k</span><b class="ll-c-blue">{{ s.k !== undefined && s.k >= 0 ? s.k : '—' }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">char</span><b class="ll-c-purple">{{ s.char ? `'${s.char}'` : '—' }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">word</span><b class="ll-c-green">"{{ s.word || inputWord }}"</b></span>
                  </div>

                  <!-- Board Container -->
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

                      <!-- Board Rows -->
                      <div class="ll-board-rows">
                        <div
                          v-for="(row, rIdx) in (s.board || activeBoard)"
                          :key="'row-' + rIdx"
                          class="ll-board-row"
                        >
                          <!-- Row Header -->
                          <div class="ll-axis-label ll-row-label" :class="{ 'll-axis-cur': s.r === rIdx }">
                            {{ rIdx }}
                          </div>

                          <!-- Grid Cells -->
                          <div
                            v-for="(letter, cIdx) in row"
                            :key="'cell-' + rIdx + '-' + cIdx"
                            class="ll-chess-cell ll-ws-cell"
                            :class="{
                              'll-ws-in-path': getCellPathIndex(rIdx, cIdx) >= 0,
                              'll-ws-head': isCurrentHead(rIdx, cIdx),
                              'll-ws-testing': getTestStatus(rIdx, cIdx) === 'testing',
                              'll-ws-match': getTestStatus(rIdx, cIdx) === 'match',
                              'll-ws-conflict': getTestStatus(rIdx, cIdx) === 'conflict',
                              'll-ws-found': s.found && getCellPathIndex(rIdx, cIdx) >= 0
                            }"
                            @click="cycleCellLetter(rIdx, cIdx)"
                            :title="`Cell (${rIdx}, ${cIdx}) = '${letter}'. Click to cycle letter.`"
                          >
                            <span class="ll-ws-letter">{{ letter }}</span>

                            <!-- Path order badge -->
                            <div
                              v-if="getCellPathIndex(rIdx, cIdx) >= 0"
                              class="ll-ws-order-badge"
                            >
                              {{ getCellPathIndex(rIdx, cIdx) + 1 }}
                            </div>

                            <!-- Testing or Conflict Icon -->
                            <div v-if="getTestStatus(rIdx, cIdx) === 'testing' && !isCurrentHead(rIdx, cIdx)" class="ll-ws-status-icon ll-c-orange">
                              ?
                            </div>
                            <div v-if="getTestStatus(rIdx, cIdx) === 'conflict'" class="ll-ws-status-icon ll-c-red">
                              ✕
                            </div>
                          </div>
                        </div>
                      </div>
                    </div>

                    <!-- Target Word Matching Progress Strip -->
                    <div class="ll-word-strip">
                      <span class="ll-word-strip-title">Target Word:</span>
                      <div class="ll-word-chars">
                        <div
                          v-for="(ch, idx) in (s.word || activeWord)"
                          :key="'word-ch-' + idx"
                          class="ll-word-char-card"
                          :class="{
                            'll-word-ch-matched': s.activeCells && s.activeCells.some(c => c.k === idx),
                            'll-word-ch-cur': s.k === idx,
                            'll-word-ch-pending': s.k < idx && (!s.activeCells || !s.activeCells.some(c => c.k === idx))
                          }"
                        >
                          <span class="ll-word-char-letter">{{ ch }}</span>
                          <span class="ll-word-char-sub">
                            <template v-if="s.activeCells && s.activeCells.some(c => c.k === idx)">✓</template>
                            <template v-else-if="s.k === idx">▲</template>
                            <template v-else>{{ idx }}</template>
                          </span>
                        </div>
                      </div>
                    </div>

                    <!-- Result Banner -->
                    <div v-if="s.found" class="ll-result-banner ll-result-success">
                      🎉 <strong>Word Found!</strong> Path:
                      <span v-for="(cell, pidx) in s.activeCells" :key="'path-' + pidx">
                        <span v-if="pidx > 0"> &rarr; </span>
                        ({{ cell.r }}, {{ cell.c }})['{{ cell.char }}']
                      </span>
                    </div>
                    <div v-else-if="si === steps.length - 1 && !s.found" class="ll-result-banner ll-result-failure">
                      ❌ <strong>Word Not Found:</strong> Could not form "{{ s.word || activeWord }}" from any starting square.
                    </div>
                  </div>
                </div>
              </div>

              <!-- Vertical Resizer for Viz Area -->
              <div class="ll-vresizer" ref="vizResizerRef"></div>

              <!-- Color Legend -->
              <div class="ll-legend">
                <span class="ll-leg"><span class="ll-legdot ll-legdot-match"></span>Matched Letter</span>
                <span class="ll-leg"><span class="ll-legdot ll-legdot-active"></span>Checking (?)</span>
                <span class="ll-leg"><span class="ll-legdot ll-legdot-mismatch"></span>Mismatch / Visited (✕)</span>
              </div>

              <!-- Call Stack Panel -->
              <div class="ll-table-area" :style="{ height: tableHeight + 'px' }">
                <div class="ll-table-title">Recursion Call Stack &mdash; innermost = current active call</div>
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

              <!-- Vertical Resizer for Call Stack Area -->
              <div class="ll-vresizer" ref="tableResizerRef"></div>

              <!-- Step Badge -->
              <div class="ll-badge-wrap">
                <div
                  class="ll-badge"
                  :class="{
                    'll-badge-error': s.badge && (s.badge.includes('OutOfBounds') || s.badge.includes('Mismatch') || s.badge.includes('Already') || s.badge.includes('FALSE')),
                    'll-badge-success': s.badge && (s.badge.includes('SUCCESS') || s.badge.includes('FOUND') || s.badge.includes('MATCHES') || s.badge.includes('complete') || s.badge.includes('finished'))
                  }"
                >
                  {{ s.badge || 'Ready to run Word Search Backtracking algorithm.' }}
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
                  >{{ line === '' ? ' ' : line }}</span></pre>
                </div>

                <!-- Time and Space Complexity Tab -->
                <div v-else class="ll-info-scroll">
                  <h3 class="ll-cx-heading">Word Search &mdash; Complexity Analysis</h3>
                  <p class="ll-cx-intro">
                    Word Search (LeetCode 79) uses <strong>DFS with Backtracking</strong>.
                    For each cell <code>(i, j)</code> matching <code>word[0]</code>, we recursively explore all 4 orthogonal directions while tracking visited cells.
                    Dead-end branches are pruned by unmarking the visited cell and backtracking.
                  </p>

                  <h4 class="ll-cx-sub">Step-by-step Breakdown</h4>
                  <table class="ll-complexity-table">
                    <thead>
                      <tr><th>Operation / Phase</th><th>Time</th><th>Space</th><th>Reason</th></tr>
                    </thead>
                    <tbody>
                      <tr>
                        <td>Overall search: <code>exist(board, word)</code></td>
                        <td class="ll-cx-mid">O(M&times;N &times; 3<sup>L</sup>)</td>
                        <td class="ll-cx-good">O(L)</td>
                        <td>M&times;N starting cells; each DFS step has at most 3 new branches (origin already visited)</td>
                      </tr>
                      <tr>
                        <td>Best case</td>
                        <td class="ll-cx-good">O(L)</td>
                        <td class="ll-cx-good">O(L)</td>
                        <td>Word starts at (0,&nbsp;0) and matches in a straight path immediately</td>
                      </tr>
                      <tr>
                        <td>Visited storage</td>
                        <td class="ll-cx-good">&mdash;</td>
                        <td class="ll-cx-mid">O(M&times;N)</td>
                        <td>O(1) auxiliary if in-place character mask (<code>board[r][c]&nbsp;=&nbsp;'#'</code>) is used instead</td>
                      </tr>
                    </tbody>
                  </table>

                  <h4 class="ll-cx-sub">Overall Complexity</h4>
                  <div class="ll-cx-summary-grid">
                    <div class="ll-cx-card ll-cx-card-mid">
                      <div class="ll-cx-card-label">Time</div>
                      <div class="ll-cx-card-val">O(M&times;N&times;3<sup>L</sup>)</div>
                      <div class="ll-cx-card-note">L = word length</div>
                    </div>
                    <div class="ll-cx-card ll-cx-card-good">
                      <div class="ll-cx-card-label">Call Stack</div>
                      <div class="ll-cx-card-val">O(L)</div>
                      <div class="ll-cx-card-note">Recursion bounded by word length</div>
                    </div>
                    <div class="ll-cx-card ll-cx-card-good">
                      <div class="ll-cx-card-label">Visited Matrix</div>
                      <div class="ll-cx-card-val">O(M&times;N)</div>
                      <div class="ll-cx-card-note">Prevents reuse in current path</div>
                    </div>
                  </div>

                  <div class="ll-note">
                    <strong>Backtracking Insight:</strong> The cell is temporarily marked <code>visited[r][c] = true</code> while exploring its 4 neighbours.
                    When no branch completes the word, we <strong>backtrack</strong> by restoring <code>visited[r][c] = false</code>, freeing the letter to participate in alternate valid word paths.
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
.ll-viz-wrap {
  flex-shrink: 0;
  background: var(--surface);
  border-bottom: 1px solid var(--border);
  position: relative;
  overflow-x: auto;
  overflow-y: auto;
}

.ll-perm-area {
  display: flex;
  flex-direction: column;
  align-items: stretch;
  min-height: 100%;
  width: 100%;
  min-width: 0;
  box-sizing: border-box;
}

.ll-ptrs {
  display: flex;
  gap: 8px;
  flex-wrap: wrap;
  padding: 4px 14px;
  min-height: 28px;
  width: 100%;
  box-sizing: border-box;
  min-width: 0;
  align-items: center;
}

.ll-ptrs-compact {
  flex-wrap: nowrap;
  gap: 6px;
  padding: 3px 14px 5px;
  min-height: 0;
  overflow-x: auto;
  overflow-y: hidden;
  max-width: 100%;
  -webkit-overflow-scrolling: touch;
  scroll-behavior: smooth;
}

.ll-ptrs-compact::-webkit-scrollbar {
  display: block !important;
  height: 3px !important;
}

.ll-ptrs-compact::-webkit-scrollbar-track {
  background: transparent !important;
}

.ll-ptrs-compact::-webkit-scrollbar-thumb {
  background: rgba(148, 163, 184, 0.4) !important;
  border-radius: 3px !important;
}

.ll-ptrs-compact::-webkit-scrollbar-thumb:hover {
  background: var(--coral) !important;
}

.ll-ptr-chip-inline {
  display: inline-flex;
  align-items: center;
  gap: 4px;
  background: var(--surface2);
  border: 1px solid var(--border);
  border-radius: 6px;
  padding: 2px 8px;
  font-size: 11px;
  font-family: monospace;
  white-space: nowrap;
  flex-shrink: 0;
  line-height: 1.4;
}

.ll-chip-label {
  color: var(--text-muted, #8899aa);
  font-weight: 500;
  margin-right: 2px;
}

.ll-c-blue { color: var(--blue); }
.ll-c-orange { color: var(--orange); }
.ll-c-green { color: var(--green); }
.ll-c-purple { color: var(--purple); }
.ll-c-red { color: var(--red); }

/* Board */
.ll-board-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 6px 16px 12px;
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
  width: 24px;
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
  margin: 0 2px;
}

.ll-row-label {
  width: 22px;
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
  gap: 3px;
}

.ll-board-row {
  display: flex;
  align-items: center;
}

.ll-chess-cell {
  width: 44px;
  height: 44px;
  margin: 0 2px;
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
  border-radius: 6px;
  cursor: pointer;
  user-select: none;
  transition: all .15s ease;
  background: #ffffff;
  border: 1px solid #cbd5e1;
}

.ll-ws-letter {
  font-size: 18px;
  font-weight: 700;
  color: var(--text);
  line-height: 1;
}

.ll-ws-in-path {
  background: #ecfdf5 !important;
  border: 2px solid #10b981 !important;
  box-shadow: 0 0 8px rgba(16, 185, 129, 0.35);
}

.ll-ws-in-path .ll-ws-letter {
  color: #065f46 !important;
  font-weight: 800;
}

.ll-ws-head {
  background: #eff6ff !important;
  border: 2px solid var(--blue) !important;
  box-shadow: 0 0 10px rgba(59, 130, 246, 0.45);
  animation: ll-head-pulse 1.2s infinite ease-in-out;
}

.ll-ws-head .ll-ws-letter {
  color: var(--blue) !important;
  font-weight: 800;
}

.ll-ws-testing {
  background: #fffbeb !important;
  border: 2px solid var(--orange) !important;
}

.ll-ws-match {
  background: #f0fdf4 !important;
  border: 2px solid var(--green) !important;
}

.ll-ws-conflict {
  background: #fef2f2 !important;
  border: 2px solid var(--red) !important;
}

.ll-ws-found {
  background: #fef9c3 !important;
  border: 2px solid #eab308 !important;
  animation: ll-pop .3s ease forwards;
}

.ll-ws-order-badge {
  position: absolute;
  top: 2px;
  right: 3px;
  font-size: 9px;
  font-weight: 800;
  background: #10b981;
  color: #ffffff;
  border-radius: 50%;
  width: 14px;
  height: 14px;
  display: flex;
  align-items: center;
  justify-content: center;
  line-height: 1;
}

.ll-ws-status-icon {
  position: absolute;
  bottom: 2px;
  right: 3px;
  font-size: 11px;
  font-weight: 800;
}

/* Target Word Matching Strip */
.ll-word-strip {
  display: flex;
  align-items: center;
  gap: 8px;
  background: #f8fafc;
  border: 1px solid var(--border);
  padding: 6px 12px;
  border-radius: 8px;
  box-shadow: var(--shadow-sm);
  margin-top: 4px;
}

.ll-word-strip-title {
  font-size: 11px;
  font-weight: 700;
  color: var(--text2);
}

.ll-word-chars {
  display: flex;
  gap: 5px;
}

.ll-word-char-card {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  min-width: 28px;
  height: 36px;
  border-radius: 6px;
  border: 1px solid var(--border);
  background: #ffffff;
  padding: 2px 4px;
  transition: all .2s;
}

.ll-word-char-letter {
  font-size: 14px;
  font-weight: 800;
  color: var(--text);
  line-height: 1;
}

.ll-word-char-sub {
  font-size: 9px;
  font-weight: 700;
  color: var(--muted);
  margin-top: 2px;
}

.ll-word-ch-matched {
  background: #10b981 !important;
  border-color: #059669 !important;
}

.ll-word-ch-matched .ll-word-char-letter,
.ll-word-ch-matched .ll-word-char-sub {
  color: #ffffff !important;
}

.ll-word-ch-cur {
  background: #eff6ff !important;
  border: 2px solid var(--blue) !important;
}

.ll-word-ch-cur .ll-word-char-letter {
  color: var(--blue) !important;
}

.ll-word-ch-cur .ll-word-char-sub {
  color: var(--blue) !important;
}

.ll-word-ch-pending {
  opacity: 0.7;
}

/* Result Banners */
.ll-result-banner {
  margin-top: 8px;
  padding: 6px 14px;
  border-radius: 6px;
  font-size: 11.5px;
  line-height: 1.4;
  text-align: center;
}

.ll-result-success {
  background: #ecfdf5;
  border: 1px solid #a7f3d0;
  color: #065f46;
}

.ll-result-failure {
  background: #fef2f2;
  border: 1px solid #fecaca;
  color: #991b1b;
}

/* Legend */
.ll-legend { display: flex; flex-wrap: wrap; gap: 6px 14px; padding: 6px 12px; border-bottom: 1px solid var(--border); flex-shrink: 0; background: var(--surface2); }
.ll-leg { display: flex; align-items: center; gap: 5px; font-size: 11px; color: var(--text2); font-weight: 500; }
.ll-legdot { width: 11px; height: 11px; border-radius: 3px; flex-shrink: 0; display: inline-block; }
.ll-legdot-grid { background: #f8fafc; border: 1.5px solid #cbd5e1; }
.ll-legdot-match { background: #dcfce7; border: 1.5px solid #10b981; }
.ll-legdot-active { background: #fef3c7; border: 1.5px solid #f59e0b; }
.ll-legdot-mismatch { background: #fee2e2; border: 1.5px solid #ef4444; }
.ll-legdot-max { background: #fef9c3; border: 1.5px solid #eab308; }

/* Call Stack */
.ll-table-area { flex-shrink: 0; padding: 8px 14px; border-bottom: 1px solid var(--border); overflow-x: hidden; overflow-y: auto; background: var(--surface); min-width: 0; box-sizing: border-box; }
.ll-table-title { font-size: 10px; color: var(--muted); margin-bottom: 4px; font-style: italic; }
.ll-stack-line { font-family: 'Consolas', monospace; font-size: 12px; line-height: 1.8; }
.ll-frame { font-family: 'Consolas', monospace; font-size: 11.5px; color: var(--text2); padding: 1px 0; white-space: nowrap; }
.ll-frame-cur { color: var(--orange); background: var(--orange-light); border-radius: 4px; padding: 1px 5px; }
.ll-fname { color: var(--text2); }
.ll-now { color: var(--orange); font-size: 10px; margin-left: 6px; }

/* Resizers */
.ll-vresizer { height: 5px; cursor: row-resize; background: var(--border); flex-shrink: 0; transition: background .15s; position: relative; z-index: 20; }
.ll-vresizer:hover { background: var(--coral); }

/* Step Badge */
.ll-badge-wrap { padding: 6px 10px; border-bottom: 1px solid var(--border); flex-shrink: 0; min-height: 36px; display: flex; align-items: center; background: var(--surface); }
.ll-badge { display: inline-block; padding: 4px 12px; border-radius: var(--radius-sm); border-left: 3px solid var(--coral); background: var(--coral-light); font-size: 11px; color: var(--coral-dark); line-height: 1.4; word-break: break-word; font-weight: 500; }
.ll-badge-error { border-left-color: var(--red) !important; background: var(--red-light) !important; color: var(--red-dark) !important; }
.ll-badge-success { border-left-color: var(--green) !important; background: var(--green-light) !important; color: #15803d !important; }

/* Right Column Panel */
.ll-code-panel {
  display: flex;
  flex-direction: column;
  flex: 1;
  overflow: hidden;
  height: 100%;
  background: var(--surface);
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
  font-family: Consolas, monospace;
  font-size: 11px;
  background: var(--surface2);
  padding: 1px 4px;
  border-radius: 4px;
  border: 1px solid var(--border);
  color: var(--coral-dark);
}

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

.ll-speed-wrap input {
  width: 90px;
}

@keyframes ll-pop {
  0% { transform: scale(0.7); opacity: 0; }
  70% { transform: scale(1.1); opacity: 1; }
  100% { transform: scale(1); opacity: 1; }
}

@keyframes ll-head-pulse {
  0%, 100% { box-shadow: 0 0 6px rgba(59, 130, 246, 0.4); }
  50% { box-shadow: 0 0 14px rgba(59, 130, 246, 0.8); }
}
</style>
