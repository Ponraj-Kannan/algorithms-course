<script setup>
import { ref, reactive, computed, onMounted, onBeforeUnmount, watch, nextTick } from 'vue';

defineProps({
  topic: { type: String, default: 'Backtracking Algorithm - ' },
  subTopic: { type: String, default: 'Check Knight Tour Configuration' }
});

const CODES = {
  java: [
    ['',                  'public class Solution {'],
    ['',                  '    // 8 possible L-shaped knight moves: dr, dc'],
    ['',                  '    static final int[] dr = {-2, -2, -1, -1, 1, 1, 2, 2};'],
    ['',                  '    static final int[] dc = {-1, 1, -2, 2, -2, 2, -1, 1};'],
    ['',                  ''],
    ['c_entry',           '    public static boolean checkValidGrid(int[][] grid) {'],
    ['c_start_check',     '        if (grid[0][0] != 0) {'],
    ['c_start_fail',      '            return false;'],
    ['',                  '        }'],
    ['',                  '        int n = grid.length;'],
    ['c_call_dfs',        '        return check(0, 0, 0, grid, n);'],
    ['',                  '    }'],
    ['',                  ''],
    ['c_dfs_entry',       '    static boolean check(int r, int c, int step, int[][] grid, int n) {'],
    ['c_base_check',      '        if (step == n * n - 1) {'],
    ['c_base_ret',        '            return true;'],
    ['',                  '        }'],
    ['c_loop_moves',      '        for (int i = 0; i < 8; i++) {'],
    ['c_calc_next',       '            int nr = r + dr[i], nc = c + dc[i];'],
    ['c_bound_check',     '            if (nr >= 0 && nr < n && nc >= 0 && nc < n) {'],
    ['c_match_check',     '                if (grid[nr][nc] == step + 1) {'],
    ['c_recurse',         '                    return check(nr, nc, step + 1, grid, n);'],
    ['',                  '                }'],
    ['',                  '            }'],
    ['',                  '        }'],
    ['c_return_false',    '        return false;'],
    ['',                  '    }'],
    ['',                  ''],
    ['m_main_entry',      '    public static void main(String[] args) {'],
    ['m_init_grid',       '        int[][] grid = {{0, 11, 16, 5, 20}, ...};'],
    ['m_call_solver',     '        boolean isValid = checkValidGrid(grid);'],
    ['m_print_result',    '        System.out.println(isValid ? "true" : "false");'],
    ['',                  '    }'],
    ['',                  '}']
  ],
  c: [
    ['',                  '#include <stdio.h>'],
    ['',                  '#include <stdbool.h>'],
    ['',                  ''],
    ['',                  'const int dr[8] = {-2, -2, -1, -1, 1, 1, 2, 2};'],
    ['',                  'const int dc[8] = {-1, 1, -2, 2, -2, 2, -1, 1};'],
    ['',                  ''],
    ['',                  'bool check(int r, int c, int step, int grid[10][10], int n);'],
    ['',                  ''],
    ['c_entry',           'bool checkValidGrid(int grid[10][10], int n) {'],
    ['c_start_check',     '    if (grid[0][0] != 0) {'],
    ['c_start_fail',      '        return false;'],
    ['',                  '    }'],
    ['c_call_dfs',        '    return check(0, 0, 0, grid, n);'],
    ['',                  '}'],
    ['',                  ''],
    ['c_dfs_entry',       'bool check(int r, int c, int step, int grid[10][10], int n) {'],
    ['c_base_check',      '    if (step == n * n - 1) {'],
    ['c_base_ret',        '        return true;'],
    ['',                  '    }'],
    ['c_loop_moves',      '    for (int i = 0; i < 8; i++) {'],
    ['c_calc_next',       '        int nr = r + dr[i], nc = c + dc[i];'],
    ['c_bound_check',     '        if (nr >= 0 && nr < n && nc >= 0 && nc < n) {'],
    ['c_match_check',     '            if (grid[nr][nc] == step + 1) {'],
    ['c_recurse',         '                return check(nr, nc, step + 1, grid, n);'],
    ['',                  '            }'],
    ['',                  '        }'],
    ['',                  '    }'],
    ['c_return_false',    '    return false;'],
    ['',                  '}'],
    ['',                  ''],
    ['m_main_entry',      'int main() {'],
    ['m_init_grid',       '    int grid[10][10] = {{0, 11, 16, 5, 20}, ...};'],
    ['m_call_solver',     '    bool isValid = checkValidGrid(grid, 5);'],
    ['m_print_result',    '    printf("%s\\n", isValid ? "true" : "false");'],
    ['',                  '    return 0;'],
    ['',                  '}']
  ],
  cpp: [
    ['',                  '#include <iostream>'],
    ['',                  '#include <vector>'],
    ['',                  'using namespace std;'],
    ['',                  ''],
    ['',                  'const int dr[8] = {-2, -2, -1, -1, 1, 1, 2, 2};'],
    ['',                  'const int dc[8] = {-1, 1, -2, 2, -2, 2, -1, 1};'],
    ['',                  ''],
    ['',                  'bool check(int r, int c, int step, vector<vector<int>>& grid, int n);'],
    ['',                  ''],
    ['c_entry',           'bool checkValidGrid(vector<vector<int>>& grid) {'],
    ['c_start_check',     '    if (grid[0][0] != 0) {'],
    ['c_start_fail',      '        return false;'],
    ['',                  '    }'],
    ['',                  '    int n = grid.size();'],
    ['c_call_dfs',        '    return check(0, 0, 0, grid, n);'],
    ['',                  '}'],
    ['',                  ''],
    ['c_dfs_entry',       'bool check(int r, int c, int step, vector<vector<int>>& grid, int n) {'],
    ['c_base_check',      '    if (step == n * n - 1) {'],
    ['c_base_ret',        '        return true;'],
    ['',                  '    }'],
    ['c_loop_moves',      '    for (int i = 0; i < 8; i++) {'],
    ['c_calc_next',       '        int nr = r + dr[i], nc = c + dc[i];'],
    ['c_bound_check',     '        if (nr >= 0 && nr < n && nc >= 0 && nc < n) {'],
    ['c_match_check',     '            if (grid[nr][nc] == step + 1) {'],
    ['c_recurse',         '                return check(nr, nc, step + 1, grid, n);'],
    ['',                  '            }'],
    ['',                  '        }'],
    ['',                  '    }'],
    ['c_return_false',    '    return false;'],
    ['',                  '}'],
    ['',                  ''],
    ['m_main_entry',      'int main() {'],
    ['m_init_grid',       '    vector<vector<int>> grid = {{0, 11, 16, 5, 20}, ...};'],
    ['m_call_solver',     '    bool isValid = checkValidGrid(grid);'],
    ['m_print_result',    '    cout << (isValid ? "true" : "false") << endl;'],
    ['',                  '    return 0;'],
    ['',                  '}']
  ],
  python: [
    ['',                  'dr = [-2, -2, -1, -1, 1, 1, 2, 2]'],
    ['',                  'dc = [-1, 1, -2, 2, -2, 2, -1, 1]'],
    ['',                  ''],
    ['c_entry',           'def checkValidGrid(grid: list[list[int]]) -> bool:'],
    ['c_start_check',     '    if grid[0][0] != 0:'],
    ['c_start_fail',      '        return False'],
    ['',                  '    n = len(grid)'],
    ['c_call_dfs',        '    return check(0, 0, 0, grid, n)'],
    ['',                  ''],
    ['c_dfs_entry',       'def check(r: int, c: int, step: int, grid: list[list[int]], n: int) -> bool:'],
    ['c_base_check',      '    if step == n * n - 1:'],
    ['c_base_ret',        '        return True'],
    ['c_loop_moves',      '    for i in range(8):'],
    ['c_calc_next',       '        nr, nc = r + dr[i], c + dc[i]'],
    ['c_bound_check',     '        if 0 <= nr < n and 0 <= nc < n:'],
    ['c_match_check',     '            if grid[nr][nc] == step + 1:'],
    ['c_recurse',         '                return check(nr, nc, step + 1, grid, n)'],
    ['c_return_false',    '    return False'],
    ['',                  ''],
    ['m_main_entry',      'def main():'],
    ['m_init_grid',       '    grid = [[0, 11, 16, 5, 20], ...]'],
    ['m_call_solver',     '    is_valid = checkValidGrid(grid)'],
    ['m_print_result',    '    print("true" if is_valid else "false")'],
    ['',                  ''],
    ['if __name__ == "__main__":'],
    ['',                  '    main()']
  ],
  javascript: [
    ['',                  'const dr = [-2, -2, -1, -1, 1, 1, 2, 2];'],
    ['',                  'const dc = [-1, 1, -2, 2, -2, 2, -1, 1];'],
    ['',                  ''],
    ['c_entry',           'function checkValidGrid(grid) {'],
    ['c_start_check',     '  if (grid[0][0] !== 0) {'],
    ['c_start_fail',      '    return false;'],
    ['',                  '  }'],
    ['',                  '  const n = grid.length;'],
    ['c_call_dfs',        '  return check(0, 0, 0, grid, n);'],
    ['',                  '}'],
    ['',                  ''],
    ['c_dfs_entry',       'function check(r, c, step, grid, n) {'],
    ['c_base_check',      '  if (step === n * n - 1) {'],
    ['c_base_ret',        '    return true;'],
    ['',                  '  }'],
    ['c_loop_moves',      '  for (let i = 0; i < 8; i++) {'],
    ['c_calc_next',       '    const nr = r + dr[i], nc = c + dc[i];'],
    ['c_bound_check',     '    if (nr >= 0 && nr < n && nc >= 0 && nc < n) {'],
    ['c_match_check',     '      if (grid[nr][nc] === step + 1) {'],
    ['c_recurse',         '        return check(nr, nc, step + 1, grid, n);'],
    ['',                  '      }'],
    ['',                  '    }'],
    ['',                  '  }'],
    ['c_return_false',    '  return false;'],
    ['',                  '}'],
    ['',                  ''],
    ['m_main_entry',      'function main() {'],
    ['m_init_grid',       '  const grid = [[0, 11, 16, 5, 20], ...];'],
    ['m_call_solver',     '  const isValid = checkValidGrid(grid);'],
    ['m_print_result',    '  console.log(isValid ? "true" : "false");'],
    ['',                  '}'],
    ['',                  ''],
    ['main();']
  ]
};

const PSEUDOCODE = [
  'function checkValidGrid(grid):',
  '    if grid[0][0] != 0:                      // Rule 1: Tour must start at top-left (0,0)',
  '        return false',
  '    n = length(grid)',
  '    return check(r = 0, c = 0, step = 0, grid, n)',
  '',
  'function check(r, c, step, grid, n):',
  '    if step == n * n - 1:                   // Base case: All N^2 squares visited in order',
  '        return true',
  '    // 8 possible L-shaped knight moves (dr, dc)',
  '    dr = [-2, -2, -1, -1,  1, 1,  2, 2]',
  '    dc = [-1,  1, -2,  2, -2, 2, -1, 1]',
  '    for i = 0 to 7:',
  '        nr = r + dr[i]',
  '        nc = c + dc[i]',
  '        if 0 <= nr < n and 0 <= nc < n:      // Check chessboard boundaries',
  '            if grid[nr][nc] == step + 1:     // Match consecutive tour number',
  '                return check(nr, nc, step + 1, grid, n)',
  '    return false                            // No valid knight move leads to step + 1'
];

const PRESETS = {
  ex1: {
    name: 'Ex 1: Valid 5×5',
    n: 5,
    grid: [
      [0, 11, 16, 5, 20],
      [17, 4, 19, 10, 15],
      [12, 1, 8, 21, 6],
      [3, 18, 23, 14, 9],
      [24, 13, 2, 7, 22]
    ]
  },
  ex2: {
    name: 'Ex 2: Invalid Jump',
    n: 5,
    grid: [
      [0, 11, 16, 5, 20],
      [17, 4, 19, 10, 15],
      [12, 1, 8, 21, 6],
      [3, 18, 22, 14, 9],
      [24, 13, 2, 7, 23]
    ]
  },
  ex3: {
    name: 'Ex 3: Bad Start',
    n: 5,
    grid: [
      [24, 11, 16, 5, 20],
      [17, 4, 19, 10, 15],
      [12, 1, 8, 21, 6],
      [3, 18, 23, 14, 9],
      [0, 13, 2, 7, 22]
    ]
  },
  '3x3': {
    name: '3×3 Grid',
    n: 3,
    grid: [
      [0, 5, 2],
      [3, 8, 7],
      [6, 1, 4]
    ]
  }
};

function frame(title, rows) { return { title, rows }; }

function buildSteps(inputGrid) {
  const steps = [];
  const n = inputGrid.length;
  const grid = inputGrid.map(row => [...row]);

  function findCellByVal(targetVal) {
    for (let r = 0; r < n; r++) {
      for (let c = 0; c < n; c++) {
        if (grid[r][c] === targetVal) return { r, c };
      }
    }
    return null;
  }

  function getGridCells(knightPos, visitedSet, activeTarget, candidateCell, cellStatusType) {
    const res = [];
    for (let r = 0; r < n; r++) {
      const row = [];
      for (let c = 0; c < n; c++) {
        const isLight = (r + c) % 2 === 0;
        const isKnight = knightPos && knightPos.r === r && knightPos.c === c;
        const isVisited = visitedSet && visitedSet.has(`${r},${c}`);
        const isTarget = activeTarget && activeTarget.r === r && activeTarget.c === c;
        const isCandidate = candidateCell && candidateCell.r === r && candidateCell.c === c;

        let state = 'normal';
        if (isCandidate) {
          state = cellStatusType || 'candidate_check';
        } else if (isKnight) {
          state = 'knight_current';
        } else if (isTarget) {
          state = 'target_next';
        } else if (isVisited) {
          state = 'visited_path';
        }

        row.push({
          r,
          c,
          val: grid[r][c],
          isLight,
          isKnight,
          isVisited,
          isTarget,
          isCandidate,
          state
        });
      }
      res.push(row);
    }
    return res;
  }

  const dr = [-2, -2, -1, -1, 1, 1, 2, 2];
  const dc = [-1, 1, -2, 2, -2, 2, -1, 1];

  const visitedTrail = [];
  const visitedSet = new Set();

  function getStackFrames(curStep, moveIdx = -1, curNr = null, curNc = null) {
    const frames = [
      frame('main()', [['n', String(n)]]),
      frame('checkValidGrid()', [['n', String(n)]])
    ];

    const maxFrames = 6;
    if (curStep < maxFrames) {
      for (let sIdx = 0; sIdx <= curStep; sIdx++) {
        const pt = visitedTrail[sIdx];
        if (pt) {
          const fRows = [['r', String(pt.r)], ['c', String(pt.c)], ['step', String(sIdx)]];
          if (sIdx === curStep && moveIdx >= 0) {
            fRows.push(['i', String(moveIdx)]);
            if (curNr !== null && curNc !== null) {
              fRows.push(['nr', String(curNr)]);
              fRows.push(['nc', String(curNc)]);
            }
          }
          frames.push(frame(`check(step=${sIdx})`, fRows));
        }
      }
    } else {
      const firstPt = visitedTrail[0];
      if (firstPt) {
        frames.push(frame('check(step=0)', [['r', String(firstPt.r)], ['c', String(firstPt.c)], ['step', '0']]));
      }
      frames.push(frame('... [earlier calls]', []));
      for (let sIdx = curStep - 3; sIdx <= curStep; sIdx++) {
        const pt = visitedTrail[sIdx];
        if (pt) {
          const fRows = [['r', String(pt.r)], ['c', String(pt.c)], ['step', String(sIdx)]];
          if (sIdx === curStep && moveIdx >= 0) {
            fRows.push(['i', String(moveIdx)]);
            if (curNr !== null && curNc !== null) {
              fRows.push(['nr', String(curNr)]);
              fRows.push(['nc', String(curNc)]);
            }
          }
          frames.push(frame(`check(step=${sIdx})`, fRows));
        }
      }
    }
    return frames;
  }

  // 1. Main Entry
  steps.push({
    badge: `public static void main(String[] args) — Program execution begins. Verifying ${n}×${n} knight tour configuration with ${n * n} squares.`,
    code: 'm_main_entry',
    vars: [frame('main()', [['n', String(n)]])],
    grid: getGridCells(null, visitedSet, null, null, 'normal'),
    n, step: 0, targetStep: 1, knightPos: null, jumpStatus: 'checking', statusText: 'Initializing',
    deltaStr: '—', vectorStr: '(Δr: —, Δc: —)', isVectorValid: false
  });

  // 2. Init Grid
  steps.push({
    badge: `int[][] grid = ...; → Initialized ${n}×${n} chessboard matrix containing tour steps 0 to ${n * n - 1}.`,
    code: 'm_init_grid',
    vars: [frame('main()', [['n', String(n)]])],
    grid: getGridCells(null, visitedSet, null, null, 'normal'),
    n, step: 0, targetStep: 1, knightPos: null, jumpStatus: 'checking', statusText: 'Grid Loaded',
    deltaStr: '—', vectorStr: '(Δr: —, Δc: —)', isVectorValid: false
  });

  // 3. Call Solver
  steps.push({
    badge: `boolean isValid = checkValidGrid(grid); → Invoking solver to verify if configuration represents a valid Knight's Tour.`,
    code: 'm_call_solver',
    vars: [frame('main()', [['n', String(n)]])],
    grid: getGridCells(null, visitedSet, null, null, 'normal'),
    n, step: 0, targetStep: 1, knightPos: null, jumpStatus: 'checking', statusText: 'Calling Solver',
    deltaStr: '—', vectorStr: '(Δr: —, Δc: —)', isVectorValid: false
  });

  // 4. Solver Entry
  steps.push({
    badge: `checkValidGrid(grid) entry → Commencing step-by-step verification on ${n}×${n} grid.`,
    code: 'c_entry',
    vars: [frame('main()', [['n', String(n)]]), frame('checkValidGrid()', [['n', String(n)]])],
    grid: getGridCells(null, visitedSet, null, null, 'normal'),
    n, step: 0, targetStep: 1, knightPos: null, jumpStatus: 'checking', statusText: 'Checking Grid',
    deltaStr: '—', vectorStr: '(Δr: —, Δc: —)', isVectorValid: false
  });

  // 5. Start check: grid[0][0] != 0
  const startVal = grid[0][0];
  const startOk = (startVal === 0);
  steps.push({
    badge: `Checking Rule 1: if (grid[0][0] != 0) → Cell (0, 0) value is ${startVal}. Condition (${startVal} != 0) is ${!startOk ? 'TRUE (Rule 1 violated! Must start at 0)' : 'FALSE (Knight correctly starts at (0, 0) with step 0)'}.`,
    code: 'c_start_check',
    vars: [frame('main()', [['n', String(n)]]), frame('checkValidGrid()', [['grid[0][0]', String(startVal)]])],
    grid: getGridCells(startOk ? { r: 0, c: 0 } : null, visitedSet, null, { r: 0, c: 0 }, startOk ? 'valid_jump' : 'conflict_mismatch'),
    n, step: 0, targetStep: 1, knightPos: startOk ? { r: 0, c: 0 } : null, jumpStatus: startOk ? 'checking' : 'invalid_start',
    statusText: startOk ? 'Start (0,0) Valid' : 'Invalid Start != 0',
    deltaStr: '—', vectorStr: '(Δr: —, Δc: —)', isVectorValid: false
  });

  if (!startOk) {
    steps.push({
      badge: `❌ Invalid Start Configuration! grid[0][0] = ${startVal} != 0. Rule 1 requires the tour to start at square (0, 0) at step 0. Executing return false;`,
      code: 'c_start_fail',
      vars: [frame('main()', [['n', String(n)]]), frame('checkValidGrid()', [['grid[0][0]', String(startVal)]])],
      grid: getGridCells(null, visitedSet, null, { r: 0, c: 0 }, 'conflict_mismatch'),
      n, step: 0, targetStep: 1, knightPos: null, jumpStatus: 'invalid_start',
      statusText: 'Failed: Start != 0', deltaStr: '—', vectorStr: '(Δr: —, Δc: —)', isVectorValid: false
    });

    steps.push({
      badge: `System.out.println(isValid ? "true" : "false"); → Printing result: false. Tour configuration is INVALID.`,
      code: 'm_print_result',
      vars: [frame('main()', [['isValid', 'false']])],
      grid: getGridCells(null, visitedSet, null, { r: 0, c: 0 }, 'conflict_mismatch'),
      n, step: 0, targetStep: 1, knightPos: null, jumpStatus: 'invalid_start',
      statusText: 'Tour Invalid (false)', deltaStr: '—', vectorStr: '(Δr: —, Δc: —)', isVectorValid: false
    });

    return steps;
  }

  // 6. Call recursive DFS
  visitedTrail.push({ r: 0, c: 0 });
  visitedSet.add('0,0');
  const target1 = findCellByVal(1);

  steps.push({
    badge: `return check(0, 0, 0, grid, ${n}); → Initiating recursive DFS verification from knight origin at square (0, 0).`,
    code: 'c_call_dfs',
    vars: [frame('main()', [['n', String(n)]]), frame('checkValidGrid()', [['n', String(n)]])],
    grid: getGridCells({ r: 0, c: 0 }, visitedSet, target1, null, 'normal'),
    n, step: 0, targetStep: 1, knightPos: { r: 0, c: 0 }, jumpStatus: 'checking',
    statusText: 'Origin Verified', deltaStr: '—', vectorStr: '(Δr: —, Δc: —)', isVectorValid: false
  });

  function simulateCheck(r, c, step) {
    const targetStep = step + 1;
    const targetPos = (step < n * n - 1) ? findCellByVal(targetStep) : null;

    // DFS Entry
    steps.push({
      badge: `check(r=${r}, c=${c}, step=${step}) entry → Knight positioned at (${r}, ${c}) verifying step ${step} of ${n * n - 1}.`,
      code: 'c_dfs_entry',
      vars: getStackFrames(step),
      grid: getGridCells({ r, c }, visitedSet, targetPos, null, 'normal'),
      n, step, targetStep, knightPos: { r, c }, jumpStatus: 'checking',
      statusText: `Step ${step}`, deltaStr: '—', vectorStr: '(Δr: —, Δc: —)', isVectorValid: false
    });

    // Base condition check
    const isBase = (step === n * n - 1);
    steps.push({
      badge: `Checking base condition: if (step == n * n - 1) → (${step} == ${n * n - 1}) is ${isBase ? 'TRUE! All ' + (n * n) + ' squares visited!' : 'FALSE (inspecting 8 knight moves for step ' + targetStep + ')'}`,
      code: 'c_base_check',
      vars: getStackFrames(step),
      grid: getGridCells({ r, c }, visitedSet, targetPos, null, 'normal'),
      n, step, targetStep, knightPos: { r, c }, jumpStatus: isBase ? 'completed' : 'checking',
      statusText: isBase ? 'All Steps Reached' : `Step ${step} OK`, deltaStr: '—', vectorStr: '(Δr: —, Δc: —)', isVectorValid: false
    });

    if (isBase) {
      steps.push({
        badge: `🎉 Base case reached: return true; → All ${n * n} squares in the tour are verified in valid consecutive knight moves!`,
        code: 'c_base_ret',
        vars: getStackFrames(step),
        grid: getGridCells({ r, c }, visitedSet, null, null, 'normal'),
        n, step, targetStep, knightPos: { r, c }, jumpStatus: 'completed',
        statusText: 'Valid Knight Tour!', deltaStr: '—', vectorStr: '(Δr: —, Δc: —)', isVectorValid: true
      });
      return true;
    }

    // Inspect 8 moves
    let foundNext = false;
    for (let i = 0; i < 8; i++) {
      const nr = r + dr[i];
      const nc = c + dc[i];
      const inBounds = (nr >= 0 && nr < n && nc >= 0 && nc < n);
      const valAtCand = inBounds ? grid[nr][nc] : null;
      const isMatch = inBounds && (valAtCand === targetStep);

      // Loop moves
      steps.push({
        badge: `for (int i = ${i}; i < 8; i++) → Checking move direction ${i + 1}/8 with offset (Δr=${dr[i]}, Δc=${dc[i]}).`,
        code: 'c_loop_moves',
        vars: getStackFrames(step, i),
        grid: getGridCells({ r, c }, visitedSet, targetPos, inBounds ? { r: nr, c: nc } : null, 'candidate_check'),
        n, step, targetStep, knightPos: { r, c }, jumpStatus: 'checking',
        statusText: `Testing move ${i + 1}/8`, deltaStr: `(${dr[i] >= 0 ? '+' : ''}${dr[i]}, ${dc[i] >= 0 ? '+' : ''}${dc[i]})`,
        vectorStr: `(Δr: ${dr[i] >= 0 ? '+' : ''}${dr[i]}, Δc: ${dc[i] >= 0 ? '+' : ''}${dc[i]})`,
        isVectorValid: Math.abs(dr[i] * dc[i]) === 2
      });

      // Calculate next coordinates
      steps.push({
        badge: `int nr = r + dr[${i}] = ${r} + (${dr[i]}) = ${nr}; int nc = c + dc[${i}] = ${c} + (${dc[i]}) = ${nc}; → Candidate square (${nr}, ${nc}).`,
        code: 'c_calc_next',
        vars: getStackFrames(step, i, nr, nc),
        grid: getGridCells({ r, c }, visitedSet, targetPos, inBounds ? { r: nr, c: nc } : null, 'candidate_check'),
        n, step, targetStep, knightPos: { r, c }, jumpStatus: 'checking',
        statusText: `Square (${nr}, ${nc})`, deltaStr: `(${dr[i] >= 0 ? '+' : ''}${dr[i]}, ${dc[i] >= 0 ? '+' : ''}${dc[i]})`,
        vectorStr: `(Δr: ${dr[i] >= 0 ? '+' : ''}${dr[i]}, Δc: ${dc[i] >= 0 ? '+' : ''}${dc[i]})`,
        isVectorValid: Math.abs(dr[i] * dc[i]) === 2
      });

      // Bound check
      steps.push({
        badge: `Boundary check: if (nr >= 0 && nr < ${n} && nc >= 0 && nc < ${n}) → (${nr}, ${nc}) is ${inBounds ? 'TRUE (within chessboard)' : 'FALSE (OUT OF BOUNDS — skip move)'}.`,
        code: 'c_bound_check',
        vars: getStackFrames(step, i, nr, nc),
        grid: getGridCells({ r, c }, visitedSet, targetPos, inBounds ? { r: nr, c: nc } : null, inBounds ? 'candidate_check' : 'conflict_mismatch'),
        n, step, targetStep, knightPos: { r, c }, jumpStatus: 'checking',
        statusText: inBounds ? 'In Bounds' : 'Out of Bounds', deltaStr: `(${dr[i] >= 0 ? '+' : ''}${dr[i]}, ${dc[i] >= 0 ? '+' : ''}${dc[i]})`,
        vectorStr: `(Δr: ${dr[i] >= 0 ? '+' : ''}${dr[i]}, Δc: ${dc[i] >= 0 ? '+' : ''}${dc[i]})`,
        isVectorValid: Math.abs(dr[i] * dc[i]) === 2
      });

      if (!inBounds) {
        continue;
      }

      // Match check
      steps.push({
        badge: `Step check: if (grid[${nr}][${nc}] == ${targetStep}) → cell value is ${valAtCand}. Condition (${valAtCand} == ${targetStep}) is ${isMatch ? 'TRUE! Match found!' : 'FALSE (mismatch, looking for ' + targetStep + ')'}.`,
        code: 'c_match_check',
        vars: getStackFrames(step, i, nr, nc),
        grid: getGridCells({ r, c }, visitedSet, targetPos, { r: nr, c: nc }, isMatch ? 'valid_jump' : 'candidate_check'),
        n, step, targetStep, knightPos: { r, c }, jumpStatus: isMatch ? 'valid' : 'checking',
        statusText: isMatch ? `Step ${targetStep} Found!` : `Not Step ${targetStep}`,
        deltaStr: `(${dr[i] >= 0 ? '+' : ''}${dr[i]}, ${dc[i] >= 0 ? '+' : ''}${dc[i]})`,
        vectorStr: `(Δr: ${dr[i] >= 0 ? '+' : ''}${dr[i]}, Δc: ${dc[i] >= 0 ? '+' : ''}${dc[i]})`,
        isVectorValid: true
      });

      if (isMatch) {
        foundNext = true;
        // Recurse
        steps.push({
          badge: `✅ Valid Knight Move! Jumping knight from (${r}, ${c}) [step ${step}] to (${nr}, ${nc}) [step ${targetStep}]. Recursing: return check(${nr}, ${nc}, ${targetStep}, grid, ${n});`,
          code: 'c_recurse',
          vars: getStackFrames(step, i, nr, nc),
          grid: getGridCells({ r, c }, visitedSet, targetPos, { r: nr, c: nc }, 'valid_jump'),
          n, step, targetStep, knightPos: { r, c }, jumpStatus: 'valid',
          statusText: `Jump to (${nr}, ${nc})`, deltaStr: `(${dr[i] >= 0 ? '+' : ''}${dr[i]}, ${dc[i] >= 0 ? '+' : ''}${dc[i]})`,
          vectorStr: `(Δr: ${dr[i] >= 0 ? '+' : ''}${dr[i]}, Δc: ${dc[i] >= 0 ? '+' : ''}${dc[i]})`,
          isVectorValid: true
        });

        visitedTrail.push({ r: nr, c: nc });
        visitedSet.add(`${nr},${nc}`);

        const result = simulateCheck(nr, nc, targetStep);
        return result;
      }
    }

    if (!foundNext) {
      steps.push({
        badge: `❌ ILLEGAL TOUR CONFIGURATION! All 8 knight moves from (${r}, ${c}) [step ${step}] examined, but none reach step ${targetStep}. Expected step ${targetStep} is at (${targetPos ? targetPos.r + ', ' + targetPos.c : '?'}) which is NOT a valid knight jump! Executing return false;`,
        code: 'c_return_false',
        vars: getStackFrames(step),
        grid: getGridCells({ r, c }, visitedSet, targetPos, targetPos, 'conflict_mismatch'),
        n, step, targetStep, knightPos: { r, c }, jumpStatus: 'no_move_found',
        statusText: 'No Valid Knight Move', deltaStr: 'Illegal Move',
        vectorStr: targetPos ? `(Δr: ${targetPos.r - r}, Δc: ${targetPos.c - c}) [INVALID]` : 'Unreachable',
        isVectorValid: false
      });

      return false;
    }
  }

  const finalResult = simulateCheck(0, 0, 0);

  // Print result in main()
  steps.push({
    badge: `System.out.println(isValid ? "true" : "false"); → Output: ${finalResult ? 'true' : 'false'}. ${finalResult ? 'Valid Knight\'s Tour confirmed! 🎉' : 'Tour configuration is INVALID. ❌'}`,
    code: 'm_print_result',
    vars: [frame('main()', [['isValid', finalResult ? 'true' : 'false']])],
    grid: getGridCells(finalResult ? visitedTrail[visitedTrail.length - 1] : null, visitedSet, null, null, finalResult ? 'normal' : 'conflict_mismatch'),
    n, step: visitedTrail.length - 1, targetStep: visitedTrail.length, knightPos: finalResult ? visitedTrail[visitedTrail.length - 1] : null,
    jumpStatus: finalResult ? 'completed' : 'no_move_found',
    statusText: finalResult ? 'Finished: true' : 'Finished: false',
    deltaStr: '—', vectorStr: '(Δr: —, Δc: —)', isVectorValid: finalResult
  });

  return steps;
}

const currentPreset = ref('ex1');
const activeGrid = ref(PRESETS.ex1.grid);
const inputN = ref(5);
const lang = ref('java');
const speed = ref(650);
const si = ref(0);
const playing = ref(false);
const vizHeight = ref(310);
const tableHeight = ref(65);
const leftWidth = ref(50);
const rightTab = ref('code');

const stepsData = reactive({ steps: buildSteps(PRESETS.ex1.grid) });
const steps = computed(() => stepsData.steps);
const s = computed(() => steps.value[Math.max(0, Math.min(si.value, steps.value.length - 1))] || {});
const codeLines = computed(() => CODES[lang.value] || []);
const currentN = computed(() => activeGrid.value.length);

let playTimer = null;

function loadPreset(key) {
  if (!PRESETS[key]) return;
  currentPreset.value = key;
  inputN.value = PRESETS[key].n;
  activeGrid.value = PRESETS[key].grid.map(row => [...row]);
  applyInput();
}

function applyInput() {
  playing.value = false;
  const nVal = parseInt(inputN.value, 10);
  if (isNaN(nVal) || nVal < 3 || nVal > 8) {
    alert('Please enter N between 3 and 8 for optimal visualization.');
    inputN.value = activeGrid.value.length;
    return;
  }
  if (nVal !== activeGrid.value.length) {
    if (nVal === 3) {
      loadPreset('3x3');
      return;
    } else {
      loadPreset('ex1');
      return;
    }
  }
  stepsData.steps = buildSteps(activeGrid.value);
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

function onChipsWheel(e) {
  if (Math.abs(e.deltaY) > Math.abs(e.deltaX)) {
    e.currentTarget.scrollLeft += e.deltaY;
  }
}

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
  cleanupFns.push(initVResizer(vizResizerRef, vizHeight, 220, 560));
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
              <label>Board:</label>
              <!-- <input
                type="number"
                min="3"
                max="8"
                v-model.number="inputN"
                class="ll-text-input"
                @keyup.enter="applyInput"
                style="width: 48px;"
              /> -->
            </div>

            <!-- Preset Configuration Selector -->
            <div class="ll-preset-group">
              <button
                class="ll-preset-btn"
                :class="{ active: currentPreset === 'ex1' }"
                @click="loadPreset('ex1')"
                title="LeetCode Example 1: Valid 5×5 Tour"
              >
                Valid 5×5
              </button>
              <button
                class="ll-preset-btn"
                :class="{ active: currentPreset === 'ex2' }"
                @click="loadPreset('ex2')"
                title="LeetCode Example 2: Invalid Jump"
              >
                Invalid Jump
              </button>
              <button
                class="ll-preset-btn"
                :class="{ active: currentPreset === 'ex3' }"
                @click="loadPreset('ex3')"
                title="Test Case 3: Invalid Start (grid[0][0] != 0)"
              >
                Bad Start
              </button>
              <button
                class="ll-preset-btn"
                :class="{ active: currentPreset === '3x3' }"
                @click="loadPreset('3x3')"
                title="Test Case 4: Compact 3×3 Grid"
              >
                3×3 Grid
              </button>
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
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">N = </span><b class="ll-c-blue">{{ s.n || currentN }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">Knight (r, c) = </span><b class="ll-c-orange">{{ s.knightPos ? `(${s.knightPos.r}, ${s.knightPos.c})` : '—' }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">Step = </span><b class="ll-c-purple">{{ s.step !== undefined ? s.step : '—' }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">Target = </span><b class="ll-c-green">{{ s.targetStep !== undefined ? s.targetStep : '—' }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">Progress = </span><b class="ll-c-green">{{ s.step !== undefined ? `${s.step + 1} / ${(s.n || currentN) * (s.n || currentN)}` : '0 / ' + ((s.n || currentN) * (s.n || currentN)) }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">Jump Vector = </span><b class="ll-c-blue">{{ s.deltaStr || '—' }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">Status = </span><b :class="s.jumpStatus === 'valid' || s.jumpStatus === 'completed' ? 'll-c-green' : s.jumpStatus === 'invalid_start' || s.jumpStatus === 'no_move_found' ? 'll-c-red' : 'll-c-orange'">{{ s.statusText || 'Ready' }}</b></span>
                  </div>

                  <!-- Chessboard Container (Knight Tour Visualization) -->
                  <div class="ll-board-container">

                    <!-- Chessboard Frame -->
                    <div class="gp-tier-title">Chessboard Matrix <code>int[][] grid</code></div>
                    <div class="ll-board-frame kt-board-frame">
                      <!-- Column Headers -->
                      <div class="ll-col-indices">
                        <div class="ll-axis-spacer"></div>
                        <div
                          v-for="c in (s.n || currentN)"
                          :key="'col-h-' + c"
                          class="ll-axis-label"
                          :class="{ 'll-axis-cur': s.knightPos && s.knightPos.c === c - 1 }"
                        >
                          {{ c - 1 }}
                        </div>
                      </div>

                      <!-- Board Rows -->
                      <div class="ll-board-rows">
                        <div
                          v-for="(row, rIdx) in s.grid"
                          :key="'row-' + rIdx"
                          class="ll-board-row"
                        >
                          <!-- Row Label -->
                          <div class="ll-axis-label ll-row-label" :class="{ 'll-axis-cur': s.knightPos && s.knightPos.r === rIdx }">
                            {{ rIdx }}
                          </div>

                          <!-- Chess Squares -->
                          <div
                            v-for="(cell, cIdx) in row"
                            :key="'cell-' + rIdx + '-' + cIdx"
                            class="ll-chess-cell ll-knight-cell"
                            :class="{
                              'll-cell-light': cell.isLight,
                              'll-cell-dark': !cell.isLight,
                              'll-cell-knight': cell.state === 'knight_current',
                              'll-cell-target': cell.state === 'target_next',
                              'll-cell-candidate': cell.state === 'candidate_check',
                              'll-cell-valid': cell.state === 'valid_jump',
                              'll-cell-visited': cell.state === 'visited_path',
                              'll-cell-conflict': cell.state === 'conflict_mismatch'
                            }"
                            :title="`Square (${rIdx}, ${cIdx}) = Step ${cell.val}`"
                          >
                            <!-- Designated tour step number -->
                            <span class="ll-step-num" :class="{ 'll-step-num-active': cell.state === 'knight_current' || cell.state === 'valid_jump' }">
                              {{ cell.val }}
                            </span>

                            <!-- Animated Knight Piece -->
                            <transition name="knight-pop">
                              <div v-if="cell.isKnight" class="ll-knight-piece">
                                ♞
                              </div>
                            </transition>

                            <!-- Candidate question marker -->
                            <div v-if="cell.state === 'candidate_check'" class="ll-candidate-marker">
                              ?
                            </div>

                            <!-- Valid jump check marker -->
                            <div v-if="cell.state === 'valid_jump'" class="ll-valid-marker">
                              ✓
                            </div>

                            <!-- Conflict mismatch marker -->
                            <div v-if="cell.state === 'conflict_mismatch'" class="ll-conflict-marker">
                              ✕
                            </div>

                            <!-- Visited Trail Order Dot -->
                            <div v-if="cell.state === 'visited_path'" class="ll-trail-badge">
                              •
                            </div>

                            <!-- Target Next Pulse Ring -->
                            <div v-if="cell.state === 'target_next' && !cell.isKnight" class="ll-target-pulse"></div>
                          </div>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
              </div>

              <!-- Resizer -->
              <div class="ll-vresizer" ref="vizResizerRef"></div>

              <!-- Color Legend -->
              <div class="ll-legend">
                <span class="ll-leg"><span class="ll-legdot ll-legdot-target"></span>Next Target</span>
                <span class="ll-leg"><span class="ll-legdot ll-legdot-candidate"></span>Candidate Move ?</span>
                <span class="ll-leg"><span class="ll-legdot ll-legdot-valid"></span>Valid Jump</span>
                <!-- <span class="ll-leg"><span class="ll-legdot ll-legdot-visited"></span>Visited Path</span> -->
                <span class="ll-leg"><span class="ll-legdot ll-legdot-conflict"></span>Conflict / Illegal</span>
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
                    'll-badge-error': s.badge && (s.badge.includes('Conflict') || s.badge.includes('violated') || s.badge.includes('NO VALID') || s.badge.includes('FALSE') || s.badge.includes('Failed') || s.badge.includes('mismatch') || s.badge.includes('ILLEGAL') || s.badge.includes('INVALID')),
                    'll-badge-success': s.badge && (s.badge.includes('VALID') || s.badge.includes('MATCH') || s.badge.includes('passed') || s.badge.includes('completed') || s.badge.includes('true') || s.badge.includes('Found'))
                  }"
                >
                  {{ s.badge || 'Ready to check Knight Tour Configuration.' }}
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
                  <h3 class="ll-cx-heading">LeetCode 2596 &mdash; Check Knight Tour Configuration</h3>
                  <p class="ll-cx-intro">
                    Determines if a given N&times;N matrix represents a valid Knight's Tour where every square is visited
                    in sequential order from <code>0</code> to <code>N<sup>2</sup> &minus; 1</code> starting at <code>(0, 0)</code>.
                  </p>

                  <h4 class="ll-cx-sub">Step-by-step Complexity Breakdown</h4>
                  <table class="ll-complexity-table">
                    <thead>
                      <tr><th>Operation / Phase</th><th>Time Complexity</th><th>Space Complexity</th><th>Reasoning</th></tr>
                    </thead>
                    <tbody>
                      <tr>
                        <td>Start Check: <code>grid[0][0] == 0</code></td>
                        <td class="ll-cx-good">O(1)</td>
                        <td class="ll-cx-good">O(1)</td>
                        <td>Constant time validation of the starting square at origin (0, 0).</td>
                      </tr>
                      <tr>
                        <td>8 Knight Moves Inspection</td>
                        <td class="ll-cx-good">O(1) per step</td>
                        <td class="ll-cx-good">O(1)</td>
                        <td>At most 8 coordinate offsets <code>(dr[i], dc[i])</code> inspected per square.</td>
                      </tr>
                      <tr>
                        <td>Move Transitions &amp; DFS Traversal</td>
                        <td class="ll-cx-good">O(N<sup>2</sup>)</td>
                        <td class="ll-cx-mid">O(N<sup>2</sup>)</td>
                        <td>At most N<sup>2</sup> steps along the tour path; call stack recursion depth reaches up to N<sup>2</sup>.</td>
                      </tr>
                      <tr>
                        <td>Verification vs Generation</td>
                        <td class="ll-cx-good">O(N<sup>2</sup>) total</td>
                        <td class="ll-cx-good">O(N<sup>2</sup>) stack</td>
                        <td>Verifying an existing numbered tour is deterministic O(N<sup>2</sup>), avoiding exponential backtracking branching!</td>
                      </tr>
                    </tbody>
                  </table>

                  <h4 class="ll-cx-sub">Overall Complexity Summary</h4>
                  <div class="ll-cx-summary-grid">
                    <div class="ll-cx-card ll-cx-card-good">
                      <div class="ll-cx-card-label">Time Complexity</div>
                      <div class="ll-cx-card-val">O(N<sup>2</sup>)</div>
                      <div class="ll-cx-card-note">At most 8 &times; N<sup>2</sup> move inspections</div>
                    </div>
                    <div class="ll-cx-card ll-cx-card-mid">
                      <div class="ll-cx-card-label">Space Complexity</div>
                      <div class="ll-cx-card-val">O(N<sup>2</sup>)</div>
                      <div class="ll-cx-card-note">Recursion call stack depth up to N<sup>2</sup></div>
                    </div>
                    <div class="ll-cx-card ll-cx-card-good">
                      <div class="ll-cx-card-label">Verification Result</div>
                      <div class="ll-cx-card-val">Boolean</div>
                      <div class="ll-cx-card-note">Valid (true) vs Invalid (false)</div>
                    </div>
                  </div>

                  <div class="ll-note">
                    <strong>Backtracking &amp; Verification Insight:</strong>
                    A knight's move is uniquely characterized by moving 2 squares along one axis and 1 square along the perpendicular axis:
                    <code>|Δr| &times; |Δc| = 2</code>, or equivalently <code>Δr<sup>2</sup> + Δc<sup>2</sup> = 5</code>.
                    Because each cell in a valid tour has a unique designated step number from 0 to N<sup>2</sup> &minus; 1, each step has at most <strong>one</strong> valid candidate with value <code>step + 1</code>.
                    Thus, verification avoids exponential state-space exploration and completes in deterministic linear time relative to chessboard area: <strong>O(N<sup>2</sup>)</strong>!
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

@keyframes ll-pulse-target {
  0%, 100% { transform: scale(1); opacity: 0.9; }
  50% { transform: scale(1.06); opacity: 1; }
}

@keyframes ll-pulse-conflict {
  0%, 100% { transform: scale(1); }
  50% { transform: scale(1.08); }
}

.knight-pop-enter-active {
  animation: ll-pop 0.25s cubic-bezier(0.175, 0.885, 0.32, 1.275);
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

.ll-text-input {
  background: var(--surface);
  border: 1px solid var(--border2);
  color: var(--text);
  border-radius: var(--radius-sm);
  padding: 3px 6px;
  font-size: 11.5px;
  font-family: monospace;
}

.ll-text-input:focus {
  outline: none;
  border-color: var(--coral);
  box-shadow: 0 0 0 3px rgba(240,77,77,.1);
}

.ll-preset-group {
  display: flex;
  align-items: center;
  gap: 4px;
  flex-wrap: wrap;
}

.ll-preset-btn {
  background: var(--surface2);
  border: 1px solid var(--border2);
  color: var(--text2);
  padding: 3px 8px;
  border-radius: var(--radius-sm);
  cursor: pointer;
  font-size: 11px;
  font-weight: 600;
  transition: all .15s ease;
  white-space: nowrap;
}

.ll-preset-btn:hover {
  border-color: var(--coral);
  color: var(--coral);
}

.ll-preset-btn.active {
  background: var(--coral);
  border-color: var(--coral);
  color: #fff;
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
  scrollbar-width: thin;
  scrollbar-color: var(--border) transparent;
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
  color: var(--muted);
  font-weight: 500;
  margin-right: 2px;
}

.ll-c-blue { color: var(--blue); }
.ll-c-orange { color: var(--orange); }
.ll-c-green { color: var(--green); }
.ll-c-purple { color: var(--purple); }
.ll-c-red { color: var(--red); }

/* Chessboard Layout & Knight Tour Customizations */
.ll-board-container {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  padding: 6px 14px 10px;
  gap: 5px;
}

.gp-tier-title {
  font-size: 10px;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: .04em;
  color: var(--muted);
  margin-top: 4px;
  margin-bottom: 2px;
  font-family: 'Consolas', monospace;
  border-left: 3px solid var(--coral);
  padding-left: 6px;
}

.ll-tour-info-card {
  display: flex;
  align-items: center;
  gap: 12px;
  background: var(--surface2);
  border: 1px solid var(--border);
  border-radius: var(--radius-sm);
  padding: 4px 12px;
  font-size: 11px;
  width: 100%;
  max-width: 440px;
  justify-content: space-around;
  box-shadow: var(--shadow-sm);
}

.ll-tour-info-item {
  display: flex;
  align-items: center;
  gap: 5px;
}

.ll-tour-label {
  color: var(--muted);
  font-weight: 600;
  font-size: 10px;
  text-transform: uppercase;
  letter-spacing: 0.03em;
}

.ll-tour-val {
  font-family: monospace;
  font-weight: 700;
}

.ll-tour-info-divider {
  width: 1px;
  height: 16px;
  background: var(--border2);
}

.ll-arrow {
  color: var(--muted);
  font-size: 10px;
  margin: 0 2px;
}

.ll-pct {
  color: var(--muted);
  font-size: 9.5px;
  font-weight: normal;
  margin-left: 2px;
}

.ll-board-frame {
  display: flex;
  flex-direction: column;
  background: #f8fafc;
  border: 1px solid var(--border);
  border-radius: var(--radius);
  padding: 8px 12px;
  box-shadow: var(--shadow-sm);
}

.kt-board-frame { gap: 4px; }

.ll-col-indices {
  display: flex;
  margin-bottom: 2px;
}

.ll-axis-spacer {
  width: 28px;
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
  font-family: monospace;
  margin: 0 1px;
}

.ll-row-label {
  width: 28px;
  height: 44px;
  font-size: 10px;
  font-weight: 700;
  color: var(--muted);
  font-family: monospace;
}

.ll-axis-cur {
  color: var(--coral-dark);
  font-weight: 900;
}

.ll-board-rows {
  display: flex;
  flex-direction: column;
  gap: 1px;
}

.ll-board-row {
  display: flex;
  align-items: center;
  gap: 1px;
}

.ll-chess-cell {
  position: relative;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  border-radius: 3px;
  user-select: none;
  transition: all .18s;
  width: 44px;
  height: 44px;
  font-family: monospace;
}

.ll-cell-light {
  background-color: #f0d9b5;
}

.ll-cell-dark {
  background-color: #b58863;
}

.ll-cell-knight {
  background-color: #fef3c7 !important;
  border: 2px solid #f59e0b !important;
  transform: scale(1.05);
  z-index: 10;
  box-shadow: 0 2px 4px rgba(245, 158, 11, 0.25);
}

.ll-cell-target {
  background-color: #f0f9ff !important;
  border: 2px dashed #0284c7 !important;
  animation: ll-pulse-target 1.2s infinite ease-in-out;
  z-index: 8;
}

.ll-cell-candidate {
  background-color: #ede9fe !important;
  border: 2px solid #8b5cf6 !important;
  transform: scale(1.04);
  z-index: 9;
}

.ll-cell-valid {
  background-color: #dcfce7 !important;
  border: 2px solid #10b981 !important;
  transform: scale(1.06);
  z-index: 10;
  box-shadow: 0 2px 4px rgba(16, 185, 129, 0.25);
}

.ll-cell-visited {
  background-color: #f0fdf4 !important;
  border: 1.5px solid #86efac !important;
}

.ll-cell-conflict {
  background-color: #fee2e2 !important;
  border: 2px solid #ef4444 !important;
  animation: ll-pulse-conflict 0.6s ease-in-out;
  z-index: 10;
  box-shadow: 0 2px 4px rgba(239, 68, 68, 0.25);
}

.ll-step-num {
  position: absolute;
  top: 2px;
  left: 3px;
  font-size: 11px;
  font-weight: 700;
  color: rgba(0,0,0,.35);
  line-height: 1;
  z-index: 2;
  font-family: monospace;
}

/* Light text on dark (brown) squares */
.ll-cell-dark .ll-step-num {
  color: rgba(255,255,255,.55);
}

/* Dark text on light (tan) squares */
.ll-cell-light .ll-step-num {
  color: rgba(0,0,0,.4);
}

.ll-step-num-active {
  color: #1d4ed8 !important;
  font-weight: 900;
}

.ll-knight-piece {
  font-size: 26px;
  line-height: 1;
  color: #7a5a3f;
  text-shadow: 0 2px 4px rgba(0, 0, 0, 0.4);
  z-index: 12;
  filter: drop-shadow(0 1px 2px rgba(0, 0, 0, 0.3));
}

.ll-candidate-marker {
  font-size: 15px;
  font-weight: 900;
  color: #6d28d9;
  z-index: 5;
  text-shadow: 0 1px 2px rgba(0,0,0,.15);
}

.ll-valid-marker {
  font-size: 17px;
  font-weight: 900;
  color: #15803d;
  z-index: 5;
  text-shadow: 0 1px 2px rgba(0,0,0,.12);
}

.ll-conflict-marker {
  font-size: 17px;
  font-weight: 900;
  color: #b91c1c;
  z-index: 5;
  text-shadow: 0 1px 2px rgba(0,0,0,.12);
}

.ll-trail-badge {
  position: absolute;
  bottom: 2px;
  right: 3px;
  font-size: 14px;
  line-height: 1;
  color: #15803d;
  font-weight: 900;
  text-shadow: 0 1px 2px rgba(0,0,0,.1);
}

.ll-target-pulse {
  position: absolute;
  inset: 2px;
  border: 1.5px dashed #0284c7;
  border-radius: 4px;
  pointer-events: none;
  animation: ll-pulse-target 1s infinite alternate;
}

.ll-rule-pill {
  display: flex;
  align-items: center;
  gap: 6px;
  background: #f8fafc;
  border: 1px solid var(--border);
  border-radius: 4px;
  padding: 3px 8px;
  font-size: 10px;
  color: var(--text2);
}

.ll-rule-tag {
  font-weight: 700;
  color: var(--purple);
}

.ll-rule-pill code {
  font-family: monospace;
  background: #ede9fe;
  color: #6d28d9;
  padding: 1px 4px;
  border-radius: 3px;
  font-size: 10px;
}

.ll-rule-desc {
  color: var(--muted);
  font-size: 9.5px;
}

/* Resizer */
.ll-vresizer {
  height: 5px;
  cursor: row-resize;
  background: var(--border);
  flex-shrink: 0;
  transition: background .15s;
  position: relative;
  z-index: 20;
}

.ll-vresizer:hover, .ll-vresizer.drag {
  background: var(--coral);
}

/* Legend */
.ll-legend {
  display: flex;
  flex-wrap: wrap;
  gap: 6px 14px;
  padding: 6px 12px;
  border-bottom: 1px solid var(--border);
  flex-shrink: 0;
  background: var(--surface2);
}

.ll-leg {
  display: flex;
  align-items: center;
  gap: 5px;
  font-size: 11px;
  color: var(--text2);
  font-weight: 500;
}

.ll-legdot {
  width: 11px;
  height: 11px;
  border-radius: 3px;
  flex-shrink: 0;
  display: inline-block;
}

.ll-legdot-knight { background: #fef3c7; border: 1.5px solid #f59e0b; }
.ll-legdot-target { background: #e0f2fe; border: 1.5px dashed #0284c7; }
.ll-legdot-candidate { background: #ede9fe; border: 1.5px solid #8b5cf6; }
.ll-legdot-valid { background: #dcfce7; border: 1.5px solid #10b981; }
.ll-legdot-visited { background: #f0fdf4; border: 1.5px solid #86efac; }
.ll-legdot-conflict { background: #fee2e2; border: 1.5px solid #ef4444; }

/* Stack Frame Area */
.ll-table-area {
  flex-shrink: 0;
  padding: 8px 14px;
  border-bottom: 1px solid var(--border);
  overflow-x: hidden;
  overflow-y: auto;
  background: var(--surface);
  min-width: 0;
  box-sizing: border-box;
}

.ll-table-title {
  font-size: 10px;
  color: var(--muted);
  margin-bottom: 4px;
  font-style: italic;
}

.ll-stack-line {
  font-family: 'Consolas', monospace;
  font-size: 12px;
  line-height: 1.8;
}

.ll-frame {
  font-family: 'Consolas', monospace;
  font-size: 11.5px;
  color: var(--text2);
  padding: 1px 0;
  white-space: nowrap;
}

.ll-frame-cur {
  color: var(--orange);
  background: var(--orange-light);
  border-radius: 4px;
  padding: 1px 5px;
}

.ll-fname {
  color: var(--text2);
}

.ll-now {
  color: var(--orange);
  font-size: 10px;
  margin-left: 6px;
}

/* Badge Area */
.ll-badge-wrap {
  padding: 6px 10px;
  border-bottom: 1px solid var(--border);
  flex-shrink: 0;
  min-height: 36px;
  display: flex;
  align-items: center;
  background: var(--surface);
}

.ll-badge {
  display: inline-block;
  padding: 4px 12px;
  border-radius: var(--radius-sm);
  border-left: 3px solid var(--coral);
  background: var(--coral-light);
  font-size: 11px;
  color: var(--coral-dark);
  line-height: 1.4;
  word-break: break-word;
  font-weight: 500;
}

.ll-badge-error {
  border-left-color: var(--red) !important;
  background: var(--red-light) !important;
  color: var(--red-dark) !important;
}

.ll-badge-success {
  border-left-color: var(--green) !important;
  background: var(--green-light) !important;
  color: #15803d !important;
}

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

.ll-tab-btn:hover {
  border-color: var(--coral);
  color: var(--coral);
}

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

/* Faculty Lecture & Complexity Tab */
.ll-info-scroll {
  flex: 1;
  overflow: auto;
  padding: 12px 16px;
  background: var(--surface);
  color: var(--text2);
  font-size: 12px;
  line-height: 1.55;
}

.ll-info-scroll h3 {
  font-size: 13px;
  font-weight: 700;
  color: var(--text);
  margin: 0 0 6px;
}

.ll-info-scroll h4 {
  font-size: 12px;
  font-weight: 700;
  color: var(--text);
  margin: 10px 0 4px;
}

.ll-info-scroll p {
  margin: 0 0 6px;
}

.ll-info-scroll code {
  font-family: Consolas, monospace;
  font-size: 11px;
  background: var(--surface2);
  padding: 1px 4px;
  border-radius: 4px;
  border: 1px solid var(--border);
  color: var(--coral-dark);
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

.ll-cx-good { color: #15803d; font-weight: 700; }
.ll-cx-mid  { color: #b45309; font-weight: 700; }
.ll-cx-bad  { color: #b91c1c; font-weight: 700; }

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
