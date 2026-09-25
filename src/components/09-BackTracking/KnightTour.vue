<script setup>
import { ref, reactive, computed, onMounted, onBeforeUnmount, watch, nextTick } from 'vue';

defineProps({
  topic: { type: String, default: 'Backtracking Algorithms' },
  subTopic: { type: String, default: "The Knight's Tour Problem" }
});

const XMOVE = [2, 1, -1, -2, -2, -1, 1, 2];
const YMOVE = [1, 2, 2, 1, -1, -2, -2, -1];
const MOVE_LABELS = ['(+2,+1)', '(+1,+2)', '(-1,+2)', '(-2,+1)', '(-2,-1)', '(-1,-2)', '(+1,-2)', '(+2,-1)'];

const CODES = {
  java: [
    ['',              'import java.util.Scanner;'],
    ['',              ''],
    ['',              'public class Main {'],
    ['',              '    static final int[] xMove = {2,1,-1,-2,-2,-1,1,2};'],
    ['',              '    static final int[] yMove = {1,2,2,1,-1,-2,-2,-1};'],
    ['',              ''],
    ['c_safe_fn',     '    static boolean isSafe(int x, int y, int[][] board, int n) {'],
    ['c_safe_ret',    '        return (x>=0 && x<n && y>=0 && y<n && board[x][y]==-1);'],
    ['',              '    }'],
    ['',              ''],
    ['c_solve_entry', '    static boolean solveKTUtil(int x, int y, int move_i, int[][] board, int n) {'],
    ['c_base_check',  '        if (move_i == n * n) {'],
    ['c_base_ret',    '            return true;'],
    ['',              '        }'],
    ['c_loop_moves',  '        for (int k = 0; k < 8; k++) {'],
    ['c_calc_next',   '            int next_x = x + xMove[k];'],
    ['c_calc_next_y', '            int next_y = y + yMove[k];'],
    ['c_check_safe',  '            if (isSafe(next_x, next_y, board, n)) {'],
    ['c_place_step',  '                board[next_x][next_y] = move_i;'],
    ['c_recurse',     '                if (solveKTUtil(next_x, next_y, move_i+1, board, n)) {'],
    ['c_recurse_ret', '                    return true;'],
    ['',              '                }'],
    ['c_backtrack',   '                board[next_x][next_y] = -1;'],
    ['',              '            }'],
    ['',              '        }'],
    ['c_ret_false',   '        return false;'],
    ['',              '    }'],
    ['',              ''],
    ['c_entry',       '    static boolean solveKT(int n, int[][] board) {'],
    ['c_board_init',  '        for (int i=0; i<n; i++) Arrays.fill(board[i], -1);'],
    ['c_set_start',   '        board[0][0] = 0;'],
    ['c_call_solve',  '        return solveKTUtil(0, 0, 1, board, n);'],
    ['',              '    }'],
    ['',              ''],
    ['',              '    public static void main(String[] args) {'],
    ['m_scanner',     '        Scanner sc = new Scanner(System.in);'],
    ['m_read_n',      '        int n = sc.nextInt();'],
    ['m_alloc_board', '        int[][] board = new int[n][n];'],
    ['m_call_solver', '        boolean solved = solveKT(n, board);'],
    ['m_print_result','        System.out.println(solved ? "Solution exists" : "No solution exists");'],
    ['m_done',        '    }'],
    ['',              '}']
  ],
  cpp: [
    ['',              '#include <iostream>'],
    ['',              '#include <cstring>'],
    ['',              'using namespace std;'],
    ['',              ''],
    ['',              'int xMove[]={2,1,-1,-2,-2,-1,1,2};'],
    ['',              'int yMove[]={1,2,2,1,-1,-2,-2,-1};'],
    ['',              ''],
    ['c_safe_fn',     'bool isSafe(int x,int y,int board[][10],int n){'],
    ['c_safe_ret',    '    return x>=0&&x<n&&y>=0&&y<n&&board[x][y]==-1;'],
    ['',              '}'],
    ['',              ''],
    ['c_solve_entry', 'bool solveKTUtil(int x,int y,int mi,int board[][10],int n){'],
    ['c_base_check',  '    if(mi==n*n) return true;'],
    ['c_loop_moves',  '    for(int k=0;k<8;k++){'],
    ['c_calc_next',   '        int nx=x+xMove[k], ny=y+yMove[k];'],
    ['c_check_safe',  '        if(isSafe(nx,ny,board,n)){'],
    ['c_place_step',  '            board[nx][ny]=mi;'],
    ['c_recurse',     '            if(solveKTUtil(nx,ny,mi+1,board,n)) return true;'],
    ['c_backtrack',   '            board[nx][ny]=-1;'],
    ['',              '        }'],
    ['',              '    }'],
    ['c_ret_false',   '    return false;'],
    ['',              '}'],
    ['',              ''],
    ['c_entry',       'bool solveKT(int n,int board[][10]){'],
    ['c_board_init',  '    memset(board,-1,sizeof(int)*n*n);'],
    ['c_set_start',   '    board[0][0]=0;'],
    ['c_call_solve',  '    return solveKTUtil(0,0,1,board,n);'],
    ['',              '}'],
    ['',              ''],
    ['',              'int main(){'],
    ['m_scanner',     '    int n; cin>>n;'],
    ['m_read_n',      '    // n read above'],
    ['m_alloc_board', '    int board[10][10];'],
    ['m_call_solver', '    bool ok=solveKT(n,board);'],
    ['m_print_result','    cout<<(ok?"Solution exists":"No solution")<<endl;'],
    ['m_done',        '    return 0;'],
    ['',              '}']
  ],
  python: [
    ['',              'import sys'],
    ['',              ''],
    ['',              'xMove=[2,1,-1,-2,-2,-1,1,2]'],
    ['',              'yMove=[1,2,2,1,-1,-2,-2,-1]'],
    ['',              ''],
    ['c_safe_fn',     'def isSafe(x,y,board,n):'],
    ['c_safe_ret',    '    return 0<=x<n and 0<=y<n and board[x][y]==-1'],
    ['',              ''],
    ['c_solve_entry', 'def solveKTUtil(x,y,mi,board,n):'],
    ['c_base_check',  '    if mi==n*n: return True'],
    ['c_loop_moves',  '    for k in range(8):'],
    ['c_calc_next',   '        nx,ny=x+xMove[k],y+yMove[k]'],
    ['c_check_safe',  '        if isSafe(nx,ny,board,n):'],
    ['c_place_step',  '            board[nx][ny]=mi'],
    ['c_recurse',     '            if solveKTUtil(nx,ny,mi+1,board,n): return True'],
    ['c_backtrack',   '            board[nx][ny]=-1'],
    ['c_ret_false',   '    return False'],
    ['',              ''],
    ['c_entry',       'def solveKT(n):'],
    ['c_board_init',  '    board=[[-1]*n for _ in range(n)]'],
    ['c_set_start',   '    board[0][0]=0'],
    ['c_call_solve',  '    return solveKTUtil(0,0,1,board,n),board'],
    ['',              ''],
    ['',              'n=int(input())'],
    ['m_read_n',      '# n read above'],
    ['m_call_solver', 'ok,board=solveKT(n)'],
    ['m_print_result','print("Solution exists" if ok else "No solution")'],
    ['m_done',        '']
  ],
  javascript: [
    ['',              'const xMove=[2,1,-1,-2,-2,-1,1,2];'],
    ['',              'const yMove=[1,2,2,1,-1,-2,-2,-1];'],
    ['',              ''],
    ['c_safe_fn',     'function isSafe(x,y,board,n){'],
    ['c_safe_ret',    '    return x>=0&&x<n&&y>=0&&y<n&&board[x][y]===-1;'],
    ['',              '}'],
    ['',              ''],
    ['c_solve_entry', 'function solveKTUtil(x,y,mi,board,n){'],
    ['c_base_check',  '    if(mi===n*n) return true;'],
    ['c_loop_moves',  '    for(let k=0;k<8;k++){'],
    ['c_calc_next',   '        const nx=x+xMove[k], ny=y+yMove[k];'],
    ['c_check_safe',  '        if(isSafe(nx,ny,board,n)){'],
    ['c_place_step',  '            board[nx][ny]=mi;'],
    ['c_recurse',     '            if(solveKTUtil(nx,ny,mi+1,board,n)) return true;'],
    ['c_backtrack',   '            board[nx][ny]=-1;'],
    ['',              '        }'],
    ['',              '    }'],
    ['c_ret_false',   '    return false;'],
    ['',              '}'],
    ['',              ''],
    ['c_entry',       'function solveKT(n){'],
    ['c_board_init',  '    const b=Array.from({length:n},()=>new Array(n).fill(-1));'],
    ['c_set_start',   '    b[0][0]=0;'],
    ['c_call_solve',  '    solveKTUtil(0,0,1,b,n); return b;'],
    ['',              '}'],
    ['',              ''],
    ['m_read_n',      'const n=parseInt(input);'],
    ['m_call_solver', 'const board=solveKT(n);'],
    ['m_print_result','console.log("done");'],
    ['m_done',        '']
  ],
  c: [
    ['',              '#include <stdio.h>'],
    ['',              '#include <string.h>'],
    ['',              ''],
    ['',              'int xMove[]={2,1,-1,-2,-2,-1,1,2};'],
    ['',              'int yMove[]={1,2,2,1,-1,-2,-2,-1};'],
    ['',              'int board[10][10], n;'],
    ['',              ''],
    ['c_safe_fn',     'int isSafe(int x,int y){'],
    ['c_safe_ret',    '    return x>=0&&x<n&&y>=0&&y<n&&board[x][y]==-1;'],
    ['',              '}'],
    ['',              ''],
    ['c_solve_entry', 'int solveKTUtil(int x,int y,int mi){'],
    ['c_base_check',  '    if(mi==n*n) return 1;'],
    ['c_loop_moves',  '    for(int k=0;k<8;k++){'],
    ['c_calc_next',   '        int nx=x+xMove[k],ny=y+yMove[k];'],
    ['c_check_safe',  '        if(isSafe(nx,ny)){'],
    ['c_place_step',  '            board[nx][ny]=mi;'],
    ['c_recurse',     '            if(solveKTUtil(nx,ny,mi+1)) return 1;'],
    ['c_backtrack',   '            board[nx][ny]=-1;'],
    ['',              '        }'],
    ['',              '    }'],
    ['c_ret_false',   '    return 0;'],
    ['',              '}'],
    ['',              ''],
    ['c_entry',       'int solveKT(){'],
    ['c_board_init',  '    memset(board,-1,sizeof(board));'],
    ['c_set_start',   '    board[0][0]=0;'],
    ['c_call_solve',  '    return solveKTUtil(0,0,1);'],
    ['',              '}'],
    ['',              ''],
    ['',              'int main(){'],
    ['m_scanner',     '    scanf("%d",&n);'],
    ['m_read_n',      '    // n read above'],
    ['m_alloc_board', '    // board is global'],
    ['m_call_solver', '    int ok=solveKT();'],
    ['m_print_result','    puts(ok?"Solution exists":"No solution");'],
    ['m_done',        '    return 0;'],
    ['',              '}']
  ]
};

const PSEUDOCODE = [
  'function solveKT(n):',
  '    board = [[-1] * n for n rows]',
  '    board[0][0] = 0                 // Place knight at start',
  '    return solveKTUtil(0, 0, 1, board, n)',
  '',
  'function solveKTUtil(x, y, move_i, board, n):',
  '    if move_i == n*n:               // Base case: all cells visited',
  '        return true',
  '    for k = 0 to 7:                 // Try all 8 L-shaped moves',
  '        next_x = x + xMove[k]',
  '        next_y = y + yMove[k]',
  '        if isSafe(next_x, next_y, board, n):',
  '            board[next_x][next_y] = move_i   // Place step',
  '            if solveKTUtil(next_x, next_y, move_i+1, board, n):',
  '                return true',
  '            board[next_x][next_y] = -1        // Backtrack',
  '    return false                    // Dead end: all moves failed',
  '',
  'function isSafe(x, y, board, n):',
  '    return 0 <= x < n and 0 <= y < n and board[x][y] == -1'
];

// ─── Step Builder ─────────────────────────────────────────────────────────────
function buildSteps(nParam) {
  const n = Math.max(3, Math.min(6, parseInt(nParam, 10) || 5));
  const MAX_STEPS = 4500;
  const steps = [];
  let backtrackCount = 0;
  let solved = false;

  const board = Array.from({ length: n }, () => new Array(n).fill(-1));

  function boardSnap() { return board.map(r => [...r]); }
  function frm(title, rows) { return { title, rows }; }

  const callStack = [];
  function stackSnap(extra = []) {
    const list = [frm('main()', [['n', String(n)]])];
    for (const f of callStack) list.push(frm(f.name, [...f.args]));
    if (extra.length && list.length > 0) {
      const top = list[list.length - 1];
      top.rows = [...top.rows, ...extra];
    }
    return list;
  }

  function push(step) { if (steps.length < MAX_STEPS) steps.push(step); }

  // ── Phase 1: Input ──────────────────────────────────────────────────────────
  push({ phase: 'input', code: 'm_scanner', badge: `Scanner sc = new Scanner(System.in); → Initializing input reader in main().`, vars: [frm('main()', [['n', '?']])], board: boardSnap(), curX: -1, curY: -1, moveI: 0, k: -1, nextX: -1, nextY: -1, safeStatus: null, backtrackCount: 0, n, solved: false });
  push({ phase: 'input', code: 'm_read_n', badge: `int n = sc.nextInt(); → Board dimension n = ${n} read. Will create ${n}x${n} chessboard.`, vars: [frm('main()', [['n', String(n)]])], board: boardSnap(), curX: -1, curY: -1, moveI: 0, k: -1, nextX: -1, nextY: -1, safeStatus: null, backtrackCount: 0, n, solved: false });
  push({ phase: 'input', code: 'm_alloc_board', badge: `int[][] board = new int[${n}][${n}]; → Memory allocated for ${n}x${n} board (${n*n} cells). All cells initialized to [UNVISITED: -1].`, vars: [frm('main()', [['n', String(n)], ['board', `new int[${n}][${n}]`]])], board: boardSnap(), curX: -1, curY: -1, moveI: 0, k: -1, nextX: -1, nextY: -1, safeStatus: null, backtrackCount: 0, n, solved: false });
  push({ phase: 'input', code: 'm_call_solver', badge: `boolean solved = solveKT(${n}, board); → Handing off to solver. Entering solveKT().`, vars: [frm('main()', [['n', String(n)]])], board: boardSnap(), curX: -1, curY: -1, moveI: 0, k: -1, nextX: -1, nextY: -1, safeStatus: null, backtrackCount: 0, n, solved: false });

  // ── Phase 2: Init ───────────────────────────────────────────────────────────
  push({ phase: 'init', code: 'c_entry', badge: `solveKT(n=${n}, board) → Function entry. Initializing board to -1.`, vars: [frm('main()', [['n', String(n)]]), frm('solveKT()', [['n', String(n)]])], board: boardSnap(), curX: -1, curY: -1, moveI: 0, k: -1, nextX: -1, nextY: -1, safeStatus: null, backtrackCount: 0, n, solved: false });
  push({ phase: 'init', code: 'c_board_init', badge: `Arrays.fill(board[i], -1); → All ${n*n} cells set to -1. Board initialized [UNVISITED: -1].`, vars: [frm('main()', [['n', String(n)]]), frm('solveKT()', [['n', String(n)]])], board: boardSnap(), curX: -1, curY: -1, moveI: 0, k: -1, nextX: -1, nextY: -1, safeStatus: null, backtrackCount: 0, n, solved: false });
  board[0][0] = 0;
  push({ phase: 'init', code: 'c_set_start', badge: `board[0][0] = 0; → [STEP: 0] Knight placed at (0,0). Starting position set with move index 0.`, vars: [frm('main()', [['n', String(n)]]), frm('solveKT()', [['board[0][0]', '0']])], board: boardSnap(), curX: 0, curY: 0, moveI: 1, k: -1, nextX: -1, nextY: -1, safeStatus: null, backtrackCount: 0, n, solved: false });
  push({ phase: 'init', code: 'c_call_solve', badge: `solveKTUtil(0, 0, 1, board, ${n}); → Launching recursive backtracking from (0,0), move_i=1.`, vars: [frm('main()', [['n', String(n)]]), frm('solveKT()', [['start', '(0,0)']])], board: boardSnap(), curX: 0, curY: 0, moveI: 1, k: -1, nextX: -1, nextY: -1, safeStatus: null, backtrackCount: 0, n, solved: false });

  // ── Warnsdorff helper: count accessible unvisited neighbours of (x,y) ─────────
  function countDegree(x, y) {
    let d = 0;
    for (let k = 0; k < 8; k++) {
      const nx = x + XMOVE[k], ny = y + YMOVE[k];
      if (nx >= 0 && nx < n && ny >= 0 && ny < n && board[nx][ny] === -1) d++;
    }
    return d;
  }

  // ── Phase 3: Recursive Solver ───────────────────────────────────────────────
  function simulate(x, y, moveI, depth) {
    if (steps.length >= MAX_STEPS) return false;

    callStack.push({ name: `KTUtil(${x},${y},mi=${moveI})`, args: [['x', String(x)], ['y', String(y)], ['move_i', String(moveI)]] });

    push({ phase: 'solver', code: 'c_solve_entry', badge: `solveKTUtil(x=${x}, y=${y}, move_i=${moveI}) → Entered at cell (${x},${y}). Attempting to place step ${moveI}.`, vars: stackSnap(), board: boardSnap(), curX: x, curY: y, moveI, k: -1, nextX: -1, nextY: -1, safeStatus: null, backtrackCount, n, solved: false, depth });

    if (moveI === n * n) {
      solved = true;
      push({ phase: 'solver', code: 'c_base_check', badge: `[COMPLETE] move_i == n*n = ${n*n}. ALL ${n*n} cells visited! Knight's Tour found!`, vars: stackSnap(), board: boardSnap(), curX: x, curY: y, moveI, k: -1, nextX: -1, nextY: -1, safeStatus: null, backtrackCount, n, solved: true, isBase: true, depth });
      push({ phase: 'solver', code: 'c_base_ret', badge: `return true; → Propagating success back through call stack.`, vars: stackSnap(), board: boardSnap(), curX: x, curY: y, moveI, k: -1, nextX: -1, nextY: -1, safeStatus: null, backtrackCount, n, solved: true, depth });
      callStack.pop();
      return true;
    }

    push({ phase: 'solver', code: 'c_base_check', badge: `move_i=${moveI} != n*n=${n*n} → Not done yet. ${n >= 5 ? '[WARNSDORFF] Sorting 8 moves by ascending onward-degree.' : 'Entering move loop to try all 8 L-shaped jumps.'}`, vars: stackSnap(), board: boardSnap(), curX: x, curY: y, moveI, k: -1, nextX: -1, nextY: -1, safeStatus: null, backtrackCount, n, solved: false, depth });

    // Build candidate list. For N>=5 sort by Warnsdorff's degree (ascending)
    // so the first valid move tried almost always leads directly to a solution.
    // For N<=4 (unsolvable) keep natural k order to show exhaustive backtracking.
    const candidates = [];
    for (let k = 0; k < 8; k++) {
      const nx = x + XMOVE[k];
      const ny = y + YMOVE[k];
      const oob = nx < 0 || nx >= n || ny < 0 || ny >= n;
      const alreadyVis = !oob && board[nx][ny] !== -1;
      const safe = !oob && !alreadyVis;
      candidates.push({ k, nx, ny, oob, alreadyVis, safe, degree: safe ? countDegree(nx, ny) : 9999 });
    }
    if (n >= 5) candidates.sort((a, b) => a.degree - b.degree);

    for (const cand of candidates) {
      if (steps.length >= MAX_STEPS) break;
      const { k, nx, ny, oob, alreadyVis, safe } = cand;
      const safeStatus = oob ? 'oob' : (alreadyVis ? 'visited' : 'valid');

      // ── Step 1: Loop header "for (int k = K; k < 8; k++)" ──────────────────
      push({
        phase: 'solver', code: 'c_loop_moves',
        badge: `for (int k = ${k}; k < 8; k++) → Entering iteration k=${k}. Testing move ${MOVE_LABELS[k]} from (${x},${y}).`,
        vars: stackSnap([['k', String(k)]]),
        board: boardSnap(), curX: x, curY: y, moveI, k, nextX: -1, nextY: -1,
        safeStatus: null, backtrackCount, n, solved: false, depth
      });

      if (steps.length >= MAX_STEPS) break;

      // ── Step 2: Calculate next_x ─────────────────────────────────────────────
      push({
        phase: 'solver', code: 'c_calc_next',
        badge: `int next_x = x + xMove[${k}] = ${x} + (${XMOVE[k]}) = ${nx};`,
        vars: stackSnap([['k', String(k)], ['next_x', String(nx)]]),
        board: boardSnap(), curX: x, curY: y, moveI, k, nextX: nx, nextY: -1,
        safeStatus: null, backtrackCount, n, solved: false, depth
      });

      if (steps.length >= MAX_STEPS) break;

      // ── Step 3: Calculate next_y ─────────────────────────────────────────────
      push({
        phase: 'solver', code: 'c_calc_next_y',
        badge: `int next_y = y + yMove[${k}] = ${y} + (${YMOVE[k]}) = ${ny};  →  Target cell: (${nx}, ${ny}).`,
        vars: stackSnap([['k', String(k)], ['next_x', String(nx)], ['next_y', String(ny)]]),
        board: boardSnap(), curX: x, curY: y, moveI, k, nextX: nx, nextY: ny,
        safeStatus: null, backtrackCount, n, solved: false, depth
      });

      if (steps.length >= MAX_STEPS) break;

      // ── Step 4: if (isSafe(...)) — call-site highlight before entering body ──
      push({
        phase: 'solver', code: 'c_check_safe',
        badge: `if (isSafe(${nx}, ${ny}, board, ${n})) → Evaluating safety of (${nx},${ny}). Calling isSafe()...`,
        vars: stackSnap([['k', String(k)], ['calling', `isSafe(${nx},${ny})`]]),
        board: boardSnap(), curX: x, curY: y, moveI, k, nextX: nx, nextY: ny,
        safeStatus: null, backtrackCount, n, solved: false, depth
      });

      if (steps.length >= MAX_STEPS) break;

      // ── Step 5: Enter isSafe() function body ─────────────────────────────────
      push({
        phase: 'solver', code: 'c_safe_fn',
        badge: `isSafe(x=${nx}, y=${ny}, board, n=${n}) → Inside isSafe(). Checking: (1) 0 <= x < n, (2) 0 <= y < n, (3) board[x][y] == -1.`,
        vars: stackSnap([['k', String(k)], ['isSafe args', `(${nx},${ny})`]]),
        board: boardSnap(), curX: x, curY: y, moveI, k, nextX: nx, nextY: ny,
        safeStatus: null, backtrackCount, n, solved: false, depth
      });

      if (steps.length >= MAX_STEPS) break;

      // ── Step 6: isSafe returns with reason ───────────────────────────────────
      push({
        phase: 'solver', code: 'c_safe_ret',
        badge: oob
          ? `return false; → [OUT OF BOUNDS] (${nx},${ny}) violates 0 <= r,c < ${n}. Bounds check failed.`
          : alreadyVis
            ? `return false; → [ALREADY VISITED] board[${nx}][${ny}] = ${board[nx][ny]} != -1. Cell already stepped on.`
            : `return true; → [VALID] (${nx},${ny}) is within bounds AND board[${nx}][${ny}] == -1 (unvisited). Safe to move.`,
        vars: stackSnap([['k', String(k)], ['isSafe →', safe ? 'true' : 'false']]),
        board: boardSnap(), curX: x, curY: y, moveI, k, nextX: nx, nextY: ny,
        safeStatus, backtrackCount, n, solved: false, depth
      });

      if (steps.length >= MAX_STEPS) break;

      // ── Step 7: if-branch decision (back at call-site) ───────────────────────
      push({
        phase: 'solver', code: 'c_check_safe',
        badge: safe
          ? `if (isSafe(...)) → TRUE ✓  Entering if-block. Will execute: board[${nx}][${ny}] = ${moveI};`
          : oob
            ? `if (isSafe(...)) → FALSE ✗  [OUT OF BOUNDS] k=${k} skipped. Advancing loop to k=${k+1}.`
            : `if (isSafe(...)) → FALSE ✗  [ALREADY VISITED] k=${k} skipped. Advancing loop to k=${k+1}.`,
        vars: stackSnap([['k', String(k)], ['if-branch', safe ? 'ENTER' : 'SKIP']]),
        board: boardSnap(), curX: x, curY: y, moveI, k, nextX: nx, nextY: ny,
        safeStatus, backtrackCount, n, solved: false, depth
      });

      if (!safe || steps.length >= MAX_STEPS) continue;

      // ── Step 6: Place step ───────────────────────────────────────────────────
      board[nx][ny] = moveI;
      push({
        phase: 'solver', code: 'c_place_step',
        badge: `board[${nx}][${ny}] = ${moveI}; → [STEP: ${moveI}] Knight placed at (${nx},${ny}). ${moveI} of ${n*n-1} cells filled.`,
        vars: stackSnap([['k', String(k)], [`board[${nx}][${ny}]`, String(moveI)]]),
        board: boardSnap(), curX: nx, curY: ny, moveI, k, nextX: nx, nextY: ny,
        safeStatus: 'valid', backtrackCount, n, solved: false, depth, justPlaced: true
      });

      if (steps.length >= MAX_STEPS) { callStack.pop(); return false; }

      // ── Step 7: Recurse ──────────────────────────────────────────────────────
      push({
        phase: 'solver', code: 'c_recurse',
        badge: `if (solveKTUtil(${nx}, ${ny}, ${moveI+1}, board, ${n})) → Recursing. Depth becomes ${depth+1}. Entering next stack frame.`,
        vars: stackSnap([['k', String(k)], ['recurse →', `(${nx},${ny}), mi=${moveI+1}`]]),
        board: boardSnap(), curX: nx, curY: ny, moveI: moveI+1, k, nextX: -1, nextY: -1,
        safeStatus: null, backtrackCount, n, solved: false, depth
      });

      if (simulate(nx, ny, moveI + 1, depth + 1)) {
        // ── Step 8: Recurse returned true ──────────────────────────────────────
        push({
          phase: 'solver', code: 'c_recurse_ret',
          badge: `solveKTUtil returned TRUE → Propagating success upward. return true; from depth ${depth}.`,
          vars: stackSnap([['recursed', 'true']]),
          board: boardSnap(), curX: nx, curY: ny, moveI, k, nextX: -1, nextY: -1,
          safeStatus: null, backtrackCount, n, solved: true, depth
        });
        callStack.pop();
        return true;
      }

      if (steps.length >= MAX_STEPS) { callStack.pop(); return false; }

      // ── Step 9: Backtrack ────────────────────────────────────────────────────
      board[nx][ny] = -1;
      backtrackCount++;
      push({
        phase: 'solver', code: 'c_backtrack',
        badge: `board[${nx}][${ny}] = -1; → [BACKTRACK] Recursion failed. Erasing step ${moveI} from (${nx},${ny}). Total backtracks: ${backtrackCount}.`,
        vars: stackSnap([['k', String(k)], ['ERASE', `board[${nx}][${ny}] ← -1`]]),
        board: boardSnap(), curX: x, curY: y, moveI, k, nextX: nx, nextY: ny,
        safeStatus: null, backtrackCount, n, solved: false, depth, isBacktrack: true
      });
    }

    if (steps.length < MAX_STEPS) {
      push({
        phase: 'solver', code: 'c_ret_false',
        badge: `[DEAD END] All 8 moves from (${x},${y}) are blocked. Returning false. Backtracks: ${backtrackCount}.`,
        vars: stackSnap([['result', 'false']]),
        board: boardSnap(), curX: x, curY: y, moveI, k: -1, nextX: -1, nextY: -1,
        safeStatus: null, backtrackCount, n, solved: false, depth, isDeadEnd: true
      });
    }

    callStack.pop();
    return false;
  }

  simulate(0, 0, 1, 0);

  if (steps.length >= MAX_STEPS && steps.length > 0) {
    steps[steps.length - 1].stepsCapped = true;
  }

  // ── Phase 4: Done ───────────────────────────────────────────────────────────
  if (steps.length < MAX_STEPS) {
    push({ phase: 'done', code: 'm_print_result', badge: solved ? `System.out.println("Solution exists"); → [COMPLETE] Knight's Tour found on ${n}x${n} board after ${backtrackCount} backtracks!` : `System.out.println("No solution exists"); → No valid Knight's Tour on ${n}x${n} board. All paths exhausted.`, vars: [frm('main()', [['solved', String(solved)], ['backtracks', String(backtrackCount)]])], board: boardSnap(), curX: -1, curY: -1, moveI: n*n, k: -1, nextX: -1, nextY: -1, safeStatus: null, backtrackCount, n, solved });
    push({ phase: 'done', code: 'm_done', badge: `Program execution complete. ${solved ? `Knight's Tour solved on ${n}x${n} board.` : `No Knight's Tour exists for ${n}x${n}.`} Total backtracks: ${backtrackCount}.`, vars: [frm('main()', [['status', 'finished']])], board: boardSnap(), curX: -1, curY: -1, moveI: n*n, k: -1, nextX: -1, nextY: -1, safeStatus: null, backtrackCount, n, solved });
  }

  return steps;
}

// ─── Reactive State ───────────────────────────────────────────────────────────
const DEFAULT_N = 5;
const inputN = ref(DEFAULT_N);
const lang = ref('java');
const speed = ref(650);
const si = ref(0);
const playing = ref(false);
const vizHeight = ref(360);
const tableHeight = ref(60);
const leftWidth = ref(52);
const rightTab = ref('code');

const stepsData = reactive({ steps: buildSteps(DEFAULT_N) });
const steps = computed(() => stepsData.steps);
const s = computed(() => steps.value[Math.max(0, Math.min(si.value, steps.value.length - 1))] || {});
const codeLines = computed(() => CODES[lang.value] || []);

let playTimer = null;

function onChipsWheel(e) { if (e.currentTarget) e.currentTarget.scrollLeft += e.deltaY; }

function applyInput() {
  const nVal = parseInt(inputN.value, 10);
  if (isNaN(nVal) || nVal < 3 || nVal > 6) { alert('Please enter N between 3 and 6.'); inputN.value = 5; return; }
  playing.value = false;
  stepsData.steps = buildSteps(nVal);
  si.value = 0;
  if (typeof window !== 'undefined') window.scrollTo(0, 0);
}

function loadPreset(n) { inputN.value = n; applyInput(); }
function stepBy(d) { si.value = Math.max(0, Math.min(steps.value.length - 1, si.value + d)); }

function togglePlay() {
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

// Computed display values
const displayBoard = computed(() => s.value.board || []);
const displayN = computed(() => s.value.n || inputN.value);
const displayMoveI = computed(() => s.value.moveI || 0);
const displayBacktracks = computed(() => s.value.backtrackCount || 0);

// Move sequence: extract all visited cells in order from board
const moveSequence = computed(() => {
  const board = displayBoard.value;
  const seq = [];
  const n = displayN.value;
  for (let step = 0; step < n * n; step++) {
    let found = false;
    for (let r = 0; r < n && !found; r++) {
      for (let c = 0; c < n && !found; c++) {
        if (board[r] && board[r][c] === step) {
          seq.push({ step, r, c });
          found = true;
        }
      }
    }
    if (!found) break;
  }
  return seq;
});

const memBoard = computed(() => { const n = displayN.value; return `${n}x${n}x4B = ${n*n*4}B`; });
const cellsVisited = computed(() => moveSequence.value.length);
const stackDepthStr = computed(() => { const d = s.value.depth || 0; const n = displayN.value; return `${d} / ${n*n} (max)`; });

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
  cleanupFns.push(initVResizer(vizResizerRef, vizHeight, 200, 650));
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
          <!-- Toolbar -->
          <div class="ll-toolbar">
            <div class="ll-input-group">
              <label>N (board):</label>
              <input type="number" min="3" max="6" v-model.number="inputN" class="ll-text-input" style="width:48px" @keyup.enter="applyInput" />
            </div>
            <div class="ll-preset-group">
              <button class="ll-preset-btn" @click="loadPreset(3)" title="3x3 — quick dead ends">3</button>
              <button class="ll-preset-btn" @click="loadPreset(4)" title="4x4 — no solution">4</button>
              <button class="ll-preset-btn" @click="loadPreset(5)" title="5x5 — solvable">5</button>
              <button class="ll-preset-btn" @click="loadPreset(6)" title="6x6 — solvable">6</button>
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
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">N</span><b class="ll-c-blue">{{ displayN }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">move_i</span><b class="ll-c-orange">{{ s.moveI !== undefined ? s.moveI : 0 }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">k</span><b class="ll-c-purple">{{ s.k !== undefined && s.k >= 0 ? s.k : '-' }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">backtracks</span><b class="ll-c-red">{{ displayBacktracks }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">phase</span><b class="ll-c-orange">{{ s.phase || 'input' }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">visited</span><b class="ll-c-green">{{ cellsVisited }} / {{ displayN * displayN }}</b></span>
                    <span class="ll-ptr-chip-inline" v-if="s.solved"><b class="ll-c-green">[COMPLETE]</b></span>
                    <span class="ll-ptr-chip-inline" v-if="s.stepsCapped"><b class="ll-c-red">[STEP LIMIT]</b></span>
                  </div>

                  <!-- BOARD CONTAINER -->
                  <div class="ll-board-container">

                    <!-- Tier 1: N x N Chessboard -->
                    <div class="gp-tier-title">Tier 1 &mdash; Chessboard Matrix <code>int[][] board</code></div>
                    <div class="ll-board-frame kt-board-frame">
                      <!-- Column headers -->
                      <div class="ll-col-indices">
                        <div class="ll-axis-spacer"></div>
                        <div v-for="c in displayN" :key="'ch-' + c" class="ll-axis-label">{{ c - 1 }}</div>
                      </div>
                      <!-- Rows -->
                      <div class="ll-board-rows">
                        <div v-for="(row, rIdx) in displayBoard" :key="'row-' + rIdx" class="ll-board-row">
                          <div class="ll-axis-label ll-row-label">{{ rIdx }}</div>
                          <div
                            v-for="(cell, cIdx) in row"
                            :key="'cell-' + rIdx + '-' + cIdx"
                            class="ll-chess-cell kt-cell"
                            :class="{
                              'kt-cell-dark': (rIdx + cIdx) % 2 === 1,
                              'kt-cell-light': (rIdx + cIdx) % 2 === 0,
                              'kt-cell-knight': rIdx === s.curX && cIdx === s.curY && s.phase !== 'done',
                              'kt-cell-visited': cell >= 0 && !(rIdx === s.curX && cIdx === s.curY),
                              'kt-cell-target': rIdx === s.nextX && cIdx === s.nextY && s.nextX >= 0,
                              'kt-cell-backtrack': s.isBacktrack && rIdx === s.nextX && cIdx === s.nextY,
                              'kt-cell-solved': s.solved,
                              'kt-cell-unvisited': cell === -1
                            }"
                            :title="`board[${rIdx}][${cIdx}] = ${cell}`"
                          >
                            <span v-if="cell === -1" class="kt-cell-neg1">-1</span>
                            <span v-else class="kt-cell-step">{{ cell }}</span>
                            <span v-if="rIdx === s.curX && cIdx === s.curY && s.phase !== 'done'" class="kt-badge-knight">[KNIGHT]</span>
                            <span v-else-if="s.isBacktrack && rIdx === s.nextX && cIdx === s.nextY" class="kt-badge-backtrack">[BACKTRACK]</span>
                            <span v-else-if="rIdx === s.nextX && cIdx === s.nextY && s.nextX >= 0 && s.safeStatus === 'valid'" class="kt-badge-target">[TARGET]</span>
                          </div>
                        </div>
                      </div>
                    </div>

                    <!-- Tier 2: 8 Move Vectors -->
                    <!-- <div class="gp-tier-title">Tier 2 &mdash; 8 Knight Move Vectors &amp; Bounds Check</div>
                    <div class="kt-moves-grid">
                      <div
                        v-for="(lbl, ki) in ['(+2,+1)','(+1,+2)','(-1,+2)','(-2,+1)','(-2,-1)','(-1,-2)','(+1,-2)','(+2,-1)']"
                        :key="'mv-' + ki"
                        class="kt-move-card"
                        :class="{
                          'kt-move-active': s.k === ki,
                          'kt-move-oob': s.k === ki && s.safeStatus === 'oob',
                          'kt-move-visited': s.k === ki && s.safeStatus === 'visited',
                          'kt-move-valid': s.k === ki && s.safeStatus === 'valid'
                        }"
                      >
                        <span class="kt-move-k">k={{ ki }}</span>
                        <span class="kt-move-label">{{ lbl }}</span>
                        <span v-if="s.k === ki && s.safeStatus === 'oob'" class="kt-oob-badge">[OUT OF BOUNDS]</span>
                        <span v-else-if="s.k === ki && s.safeStatus === 'visited'" class="kt-vis-badge">[ALREADY VISITED]</span>
                        <span v-else-if="s.k === ki && s.safeStatus === 'valid'" class="kt-val-badge">[VALID MOVE]</span>
                        <span v-else class="kt-idle-badge">—</span>
                      </div>
                    </div> -->

                    <!-- Tier 3: Move Sequence Tray -->
                    <!-- <div class="gp-tier-title">Tier 3 &mdash; Move Sequence <code>Step 0 &rarr; Step {{ displayN * displayN - 1 }}</code></div>
                    <div class="kt-seq-panel">
                      <div class="kt-seq-meta">Steps visited: <b class="ll-c-green">{{ cellsVisited }}</b> / <b class="ll-c-blue">{{ displayN * displayN }}</b></div>
                      <div class="kt-seq-slots">
                        <div
                          v-for="entry in moveSequence"
                          :key="'seq-' + entry.step"
                          class="kt-seq-slot"
                          :class="{ 'kt-seq-active': entry.step === (s.moveI !== undefined ? s.moveI - 1 : -1) }"
                        >
                          <span class="kt-seq-stepnum">{{ entry.step }}</span>
                          <span class="kt-seq-coord">({{ entry.r }},{{ entry.c }})</span>
                        </div>
                        <div v-if="moveSequence.length === 0" class="kt-seq-empty">No moves yet</div>
                      </div>
                    </div> -->

                    <!-- Tier 4: Dead-End / Backtrack Monitor -->
                    <!-- <div class="gp-tier-title">Tier 4 &mdash; Backtrack Monitor</div>
                    <div class="kt-deadend-panel" :class="{ 'kt-de-active': s.isDeadEnd || s.isBacktrack, 'kt-de-solved': s.solved }">
                      <template v-if="s.solved">
                        <span class="kt-de-icon kt-de-solved-icon">OK</span>
                        <div><div class="kt-de-title">[COMPLETE] Knight's Tour Found</div><div class="kt-de-detail">All {{ displayN * displayN }} cells visited. Tour complete after {{ displayBacktracks }} backtracks.</div></div>
                      </template>
                      <template v-else-if="s.isDeadEnd">
                        <span class="kt-de-icon kt-de-dead-icon">!</span>
                        <div><div class="kt-de-title">[DEAD END DETECTED]</div><div class="kt-de-detail">All 8 moves from ({{ s.curX }}, {{ s.curY }}) blocked. Returning false. Total backtracks: {{ displayBacktracks }}.</div></div>
                      </template>
                      <template v-else-if="s.isBacktrack">
                        <span class="kt-de-icon kt-de-bt-icon">BT</span>
                        <div><div class="kt-de-title">[BACKTRACKING TO PARENT CELL]</div><div class="kt-de-detail">board[{{ s.nextX }}][{{ s.nextY }}] = -1. Erased step {{ s.moveI }}. Knight returns to ({{ s.curX }}, {{ s.curY }}).</div></div>
                      </template>
                      <template v-else>
                        <span class="kt-de-icon kt-de-idle-icon">--</span>
                        <div><div class="kt-de-title">Monitoring</div><div class="kt-de-detail">Watching for dead ends. Condition: all 8 moves blocked at current cell → backtrack.</div></div>
                      </template>
                    </div> -->

                    <!-- Tier 5: Memory Stats -->
                    <!-- <div class="gp-tier-title">Tier 5 &mdash; Memory &amp; Statistics</div>
                    <div class="gp-mem-bar">
                      <span class="gp-mem-item"><span class="gp-mem-label">Board Memory:</span> <code>{{ memBoard }}</code></span>
                      <span class="gp-mem-sep">|</span>
                      <span class="gp-mem-item"><span class="gp-mem-label">Cells Visited:</span> <code>{{ cellsVisited }} / {{ displayN * displayN }}</code></span>
                      <span class="gp-mem-sep">|</span>
                      <span class="gp-mem-item"><span class="gp-mem-label">Backtracks:</span> <code>{{ displayBacktracks }}</code></span>
                      <span class="gp-mem-sep">|</span>
                      <span class="gp-mem-item"><span class="gp-mem-label">Stack Depth:</span> <code>{{ stackDepthStr }}</code></span>
                    </div> -->

                  </div>
                  <!-- END ll-board-container -->
                </div>
              </div>

              <div class="ll-vresizer" ref="vizResizerRef"></div>

              <!-- Legend -->
              <div class="ll-legend">
                <span class="ll-leg"><span class="ll-legdot kt-legdot-visited"></span>VISITED</span>
                <span class="ll-leg"><span class="ll-legdot kt-legdot-unvisited"></span>UNVISITED</span>
                <span class="ll-leg"><span class="ll-legdot kt-legdot-target"></span>TARGET</span>
                <span class="ll-leg"><span class="ll-legdot kt-legdot-bt"></span>BACKTRACK</span>
                <span class="ll-leg"><span class="ll-legdot kt-legdot-done"></span>COMPLETE</span>
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
                  'll-badge-error': s.badge && (s.badge.includes('[OUT OF BOUNDS]') || s.badge.includes('[ALREADY VISITED]') || s.badge.includes('[DEAD END]') || s.badge.includes('[BACKTRACK]')),
                  'll-badge-success': s.badge && (s.badge.includes('[COMPLETE]') || s.badge.includes('[VALID]') || s.badge.includes('[STEP:') || s.badge.includes('found'))
                }">
                  {{ s.badge || 'Ready to run Knight\'s Tour backtracking algorithm.' }}
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
                  <h3 class="ll-cx-heading">Knight's Tour Problem &mdash; Complexity Analysis</h3>
                  <p class="ll-cx-intro">
                    Find a sequence of moves for a knight on an N&times;N chessboard such that the knight visits every cell exactly once.
                    Uses backtracking: at each cell, try all 8 L-shaped moves. If no move leads to a complete tour, backtrack and try the next move.
                    The board is initialized with <code>-1</code> (unvisited). Visited cells are labeled with their step index <code>0..N&sup2;-1</code>.
                  </p>
                  <h4 class="ll-cx-sub">Complexity Breakdown</h4>
                  <table class="ll-complexity-table">
                    <thead><tr><th>Operation</th><th>Time</th><th>Space</th><th>Notes</th></tr></thead>
                    <tbody>
                      <tr><td>Board initialization</td><td class="ll-cx-good">O(N&sup2;)</td><td class="ll-cx-good">O(N&sup2;)</td><td>Fill all cells with -1</td></tr>
                      <tr><td>Recursive DFS</td><td class="ll-cx-bad">O(8<sup>N&sup2;</sup>)</td><td class="ll-cx-good">O(N&sup2;)</td><td>Worst case: 8 branches per cell; stack depth N&sup2;</td></tr>
                      <tr><td>isSafe check</td><td class="ll-cx-good">O(1)</td><td class="ll-cx-good">O(1)</td><td>Bounds check + visited check</td></tr>
                    </tbody>
                  </table>
                  <h4 class="ll-cx-sub">Overall Complexity</h4>
                  <div class="ll-cx-summary-grid">
                    <div class="ll-cx-card ll-cx-card-bad">
                      <div class="ll-cx-card-label">Time</div>
                      <div class="ll-cx-card-val">O(8<sup>N&sup2;</sup>)</div>
                      <div class="ll-cx-card-note">Exponential worst case; pruning reduces in practice</div>
                    </div>
                    <div class="ll-cx-card ll-cx-card-good">
                      <div class="ll-cx-card-label">Space</div>
                      <div class="ll-cx-card-val">O(N&sup2;)</div>
                      <div class="ll-cx-card-note">Board matrix + recursion stack depth N&sup2;</div>
                    </div>
                  </div>
                  <h4 class="ll-cx-sub">Solvability</h4>
                  <table class="ll-complexity-table">
                    <thead><tr><th>N</th><th>Solvable</th><th>Note</th></tr></thead>
                    <tbody>
                      <tr><td>3</td><td class="ll-cx-bad">No</td><td>3x3 board has no Hamiltonian path for a knight</td></tr>
                      <tr><td>4</td><td class="ll-cx-bad">No</td><td>4x4 board is also unsolvable</td></tr>
                      <tr><td>5+</td><td class="ll-cx-good">Yes</td><td>All N &ge; 5 boards have a knight's tour</td></tr>
                    </tbody>
                  </table>
                  <div class="ll-note">
                    <strong>Step Cap Note:</strong> This visualization caps steps at 700 for browser performance.
                    For N=5+, the full backtracking tree is very large. The animation shows the beginning of the search.
                    Warnsdorff's heuristic (always move to the cell with fewest onward moves) finds solutions in O(N&sup2;) steps.
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
@keyframes kt-flash-bt { 0%{background:#fef2f2} 50%{background:#ef4444;color:#fff} 100%{background:#fee2e2} }
@keyframes kt-flash-place { 0%{background:#fef9c3} 60%{background:#22c55e;color:#fff} 100%{background:#dcfce7} }
@keyframes kt-glow-done { 0%,100%{box-shadow:0 0 6px rgba(34,197,94,.3)} 50%{box-shadow:0 0 16px rgba(34,197,94,.8)} }

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

.ll-board-container { display:flex; flex-direction:column; align-items:flex-start; padding:6px 14px 10px; gap:5px; }
.gp-tier-title { font-size:10px; font-weight:700; text-transform:uppercase; letter-spacing:.04em; color:var(--muted); margin-top:4px; margin-bottom:2px; font-family:'Consolas',monospace; border-left:3px solid var(--coral); padding-left:6px; }
.ll-board-frame { display:flex; flex-direction:column; background:#f8fafc; border:1px solid var(--border); border-radius:var(--radius); padding:8px 12px; box-shadow:var(--shadow-sm); }
.ll-col-indices { display:flex; margin-bottom:2px; }
.ll-axis-spacer { width:28px; height:20px; }
.ll-axis-label { width:44px; height:20px; display:flex; align-items:center; justify-content:center; font-size:11px; font-weight:700; color:var(--muted); font-family:monospace; margin:0 1px; }
.ll-row-label { width:28px; height:44px; font-size:10px; font-weight:700; color:var(--muted); font-family:monospace; }
.ll-board-rows { display:flex; flex-direction:column; gap:1px; }
.ll-board-row { display:flex; align-items:center; gap:1px; }
.ll-chess-cell { position:relative; display:flex; flex-direction:column; align-items:center; justify-content:center; border-radius:3px; user-select:none; transition:all .18s; }

/* Chessboard cells */
.kt-board-frame { gap:4px; }
.kt-cell { width:44px; height:44px; }
.kt-cell-dark { background:#b58863; }
.kt-cell-light { background:#f0d9b5; }
.kt-cell-unvisited .kt-cell-neg1 { font-size:11px; color:rgba(0,0,0,.3); font-family:monospace; font-weight:700; }
.kt-cell-visited { background:#eff6ff !important; border:1.5px solid #93c5fd !important; }
.kt-cell-visited .kt-cell-step { font-size:14px; font-weight:800; color:#1d4ed8; font-family:monospace; }
.kt-cell-knight { border:2px solid #7a5a3f !important; transform:scale(1.08); z-index:10; animation:ll-pop .2s ease; }
.kt-cell-knight .kt-cell-step { color:#7a5a3f; font-size:13px; font-weight:800; font-family:monospace; }
.kt-cell-target { background:#fef9c3 !important; border:2px dashed #f59e0b !important; transform:scale(1.04); z-index:5; }
.kt-cell-backtrack { background:#fee2e2 !important; border:2px solid #ef4444 !important; animation:kt-flash-bt .4s ease; }
.kt-cell-solved .kt-cell-step { color:#15803d !important; }
.kt-badge-knight { position:absolute; font-size:7px; font-weight:800; font-family:monospace; background:#f97316; color:#fff; padding:1px 3px; border-radius:2px; bottom:2px; left:50%; transform:translateX(-50%); white-space:nowrap; }
.kt-badge-backtrack { position:absolute; font-size:6.5px; font-weight:800; font-family:monospace; background:#ef4444; color:#fff; padding:1px 2px; border-radius:2px; bottom:1px; left:50%; transform:translateX(-50%); white-space:nowrap; }
.kt-badge-target { position:absolute; font-size:6.5px; font-weight:800; font-family:monospace; background:#f59e0b; color:#fff; padding:1px 2px; border-radius:2px; bottom:1px; left:50%; transform:translateX(-50%); white-space:nowrap; }

/* Tier 2: Move vectors */
.kt-moves-grid { display:flex; gap:4px; flex-wrap:wrap; max-width:500px; }
.kt-move-card { display:flex; flex-direction:column; align-items:center; gap:2px; padding:5px 8px; border-radius:var(--radius-sm); border:1.5px solid var(--border); background:var(--surface2); min-width:72px; font-family:monospace; font-size:10px; transition:all .15s; }
.kt-move-active { border-color:var(--orange) !important; background:var(--orange-light) !important; transform:scale(1.06); z-index:5; }
.kt-move-oob { border-color:var(--red) !important; background:var(--red-light) !important; }
.kt-move-visited { border-color:var(--purple) !important; background:#f3e8ff !important; }
.kt-move-valid { border-color:var(--green) !important; background:var(--green-light) !important; }
.kt-move-k { font-size:9px; font-weight:800; color:var(--muted); }
.kt-move-label { font-size:11px; font-weight:700; color:var(--text); }
.kt-oob-badge { font-size:8px; font-weight:800; background:#fee2e2; color:#b91c1c; border:1px solid #fca5a5; padding:1px 4px; border-radius:3px; white-space:nowrap; }
.kt-vis-badge { font-size:8px; font-weight:800; background:#f3e8ff; color:#7e22ce; border:1px solid #d8b4fe; padding:1px 4px; border-radius:3px; white-space:nowrap; }
.kt-val-badge { font-size:8px; font-weight:800; background:#dcfce7; color:#15803d; border:1px solid #86efac; padding:1px 4px; border-radius:3px; white-space:nowrap; }
.kt-idle-badge { font-size:9px; color:var(--muted); }

/* Tier 3: Sequence tray */
.kt-seq-panel { display:flex; flex-direction:column; gap:4px; background:var(--surface2); border:1px solid var(--border); border-radius:var(--radius-sm); padding:6px 10px; min-width:200px; max-width:600px; }
.kt-seq-meta { font-size:10.5px; font-family:monospace; color:var(--text2); }
.kt-seq-slots { display:flex; flex-wrap:wrap; gap:3px; }
.kt-seq-slot { display:flex; flex-direction:column; align-items:center; justify-content:center; min-width:38px; padding:3px 5px; border-radius:4px; border:1px solid var(--border); background:var(--surface); font-family:monospace; transition:all .15s; }
.kt-seq-active { background:#fffbeb !important; border-color:var(--orange) !important; transform:scale(1.06); }
.kt-seq-stepnum { font-size:12px; font-weight:800; color:var(--blue); }
.kt-seq-coord { font-size:9px; color:var(--text2); }
.kt-seq-empty { color:var(--muted); font-style:italic; font-size:11px; }

/* Tier 4: Dead-end monitor */
.kt-deadend-panel { display:flex; align-items:center; gap:10px; padding:8px 12px; border-radius:var(--radius-sm); border:1.5px solid var(--border); background:var(--surface2); min-width:280px; transition:all .2s; }
.kt-de-active { border-color:var(--red) !important; background:var(--red-light) !important; }
.kt-de-solved { border-color:var(--green) !important; background:var(--green-light) !important; animation:kt-glow-done 1.5s infinite; }
.kt-de-icon { width:28px; height:28px; border-radius:50%; display:flex; align-items:center; justify-content:center; font-size:10px; font-weight:900; font-family:monospace; flex-shrink:0; border:2px solid currentColor; }
.kt-de-idle-icon { color:var(--muted); background:var(--surface); }
.kt-de-dead-icon { color:#b91c1c; background:#fee2e2; }
.kt-de-bt-icon { color:#c2410c; background:#fff7ed; }
.kt-de-solved-icon { color:#15803d; background:#dcfce7; }
.kt-de-title { font-size:11px; font-weight:800; font-family:monospace; color:var(--text); }
.kt-de-active .kt-de-title { color:#b91c1c; }
.kt-de-solved .kt-de-title { color:#15803d; }
.kt-de-detail { font-size:10px; color:var(--text2); font-family:monospace; line-height:1.4; }

/* Tier 5: Memory bar */
.gp-mem-bar { display:flex; flex-wrap:wrap; gap:4px 10px; background:#1e293b; color:#94a3b8; font-family:monospace; font-size:10px; padding:5px 10px; border-radius:var(--radius-sm); align-items:center; }
.gp-mem-item { display:flex; align-items:center; gap:4px; }
.gp-mem-label { color:#64748b; font-weight:700; }
.gp-mem-bar code { color:#38bdf8; font-size:10px; }
.gp-mem-sep { color:#334155; font-size:12px; }

.ll-vresizer { height:5px; cursor:row-resize; background:var(--border); flex-shrink:0; transition:background .15s; z-index:20; }
.ll-vresizer:hover, .ll-vresizer.drag { background:var(--coral); }

.ll-legend { display:flex; flex-wrap:wrap; gap:6px 14px; padding:6px 12px; border-bottom:1px solid var(--border); flex-shrink:0; background:var(--surface2); }
.ll-leg { display:flex; align-items:center; gap:5px; font-size:11px; color:var(--text2); font-weight:500; }
.ll-legdot { width:11px; height:11px; border-radius:3px; flex-shrink:0; display:inline-block; }
.kt-legdot-knight { background:#1e293b; border:1.5px solid var(--orange); }
.kt-legdot-visited { background:#eff6ff; border:1.5px solid #93c5fd; }
.kt-legdot-unvisited { background:#b58863; border:1.5px solid #775940; }
.kt-legdot-target { background:#fef9c3; border:1.5px dashed #f59e0b; }
.kt-legdot-bt { background:#fee2e2; border:1.5px solid var(--red); }
.kt-legdot-done { background:#dcfce7; border:1.5px solid var(--green); }

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
