<script setup>
import { ref, reactive, computed, onMounted, onUnmounted, watch } from 'vue';

const props = defineProps({
  topic: { type: String, default: 'Greedy Algorithms' },
  subTopic: { type: String, default: 'Activity Selection Problem' }
});

/* ==================================================================== */
/* APPROACH METADATA                                                    */
/* ==================================================================== */
const APPROACHES = [
  { id: 'brute', label: 'Brute Force', desc: 'O(2^n) Exponential — Exhaustive Recursive Subset Search' },
  { id: 'greedy', label: 'Greedy', desc: 'O(n log n) Optimal — Earliest Finish Time First Heuristic' }
];

/* ==================================================================== */
/* CODE SNIPPETS (Java, C, C++, Python)                                 */
/* Using 2 Separate Arrays: start[] and finish[]                        */
/* Strict curly brace formatting on all control statements              */
/* ==================================================================== */
const CODES = {
  brute: {
    java: [
      ['',                 'import java.util.Scanner;'],
      ['',                 ''],
      ['',                 'public class Main {'],
      ['',                 '    static int maxActivities = 0;'],
      ['',                 ''],
      ['c_entry',          '    static void solve(int[] start, int[] finish, int n, int idx, int lastFinish, int count) {'],
      ['c_base_check',     '        if (idx == n) {'],
      ['c_update_check',   '            if (count > maxActivities) {'],
      ['c_update_max',     '                maxActivities = count;'],
      ['',                 '            }'],
      ['c_ret_base',       '            return;'],
      ['',                 '        }'],
      ['c_call_skip',      '        solve(start, finish, n, idx + 1, lastFinish, count);'],
      ['c_check_compat',   '        if (start[idx] >= lastFinish) {'],
      ['c_call_pick',      '            solve(start, finish, n, idx + 1, finish[idx], count + 1);'],
      ['',                 '        }'],
      ['',                 '    }'],
      ['',                 ''],
      ['',                 '    public static void main(String[] args) {'],
      ['m_scanner',        '        Scanner sc = new Scanner(System.in);'],
      ['m_read_n',         '        int n = sc.nextInt();'],
      ['m_alloc_start',    '        int[] start = new int[n];'],
      ['m_alloc_finish',   '        int[] finish = new int[n];'],
      ['m_for_input',      '        for (int i = 0; i < n; i++) {'],
      ['m_read_start',     '            start[i] = sc.nextInt();'],
      ['m_read_finish',    '            finish[i] = sc.nextInt();'],
      ['',                 '        }'],
      ['m_call_solve',     '        solve(start, finish, n, 0, 0, 0);'],
      ['m_print_ans',      '        System.out.println(maxActivities);'],
      ['m_done',           '    }'],
      ['',                 '}']
    ],
    c: [
      ['',                 '#include <stdio.h>'],
      ['',                 ''],
      ['',                 'int maxActivities = 0;'],
      ['',                 ''],
      ['c_entry',          'void solve(int start[], int finish[], int n, int idx, int lastFinish, int count) {'],
      ['c_base_check',     '    if (idx == n) {'],
      ['c_update_check',   '        if (count > maxActivities) {'],
      ['c_update_max',     '            maxActivities = count;'],
      ['',                 '        }'],
      ['c_ret_base',       '        return;'],
      ['',                 '    }'],
      ['c_call_skip',      '    solve(start, finish, n, idx + 1, lastFinish, count);'],
      ['c_check_compat',   '    if (start[idx] >= lastFinish) {'],
      ['c_call_pick',      '        solve(start, finish, n, idx + 1, finish[idx], count + 1);'],
      ['',                 '    }'],
      ['',                 '}'],
      ['',                 ''],
      ['',                 'int main() {'],
      ['m_scanner',        '    int n;'],
      ['m_read_n',         '    scanf("%d", &n);'],
      ['m_alloc_start',    '    int start[100];'],
      ['m_alloc_finish',   '    int finish[100];'],
      ['m_for_input',      '    for (int i = 0; i < n; i++) {'],
      ['m_read_start',     '        scanf("%d", &start[i]);'],
      ['m_read_finish',    '        scanf("%d", &finish[i]);'],
      ['',                 '    }'],
      ['m_call_solve',     '    solve(start, finish, n, 0, 0, 0);'],
      ['m_print_ans',      '    printf("%d\\n", maxActivities);'],
      ['m_done',           '    return 0;'],
      ['',                 '}']
    ],
    cpp: [
      ['',                 '#include <iostream>'],
      ['',                 '#include <vector>'],
      ['',                 'using namespace std;'],
      ['',                 ''],
      ['',                 'int maxActivities = 0;'],
      ['',                 ''],
      ['c_entry',          'void solve(const vector<int>& start, const vector<int>& finish, int n, int idx, int lastFinish, int count) {'],
      ['c_base_check',     '    if (idx == n) {'],
      ['c_update_check',   '        if (count > maxActivities) {'],
      ['c_update_max',     '            maxActivities = count;'],
      ['',                 '        }'],
      ['c_ret_base',       '        return;'],
      ['',                 '    }'],
      ['c_call_skip',      '    solve(start, finish, n, idx + 1, lastFinish, count);'],
      ['c_check_compat',   '    if (start[idx] >= lastFinish) {'],
      ['c_call_pick',      '        solve(start, finish, n, idx + 1, finish[idx], count + 1);'],
      ['',                 '    }'],
      ['',                 '}'],
      ['',                 ''],
      ['',                 'int main() {'],
      ['m_scanner',        '    int n;'],
      ['m_read_n',         '    cin >> n;'],
      ['m_alloc_start',    '    vector<int> start(n);'],
      ['m_alloc_finish',   '    vector<int> finish(n);'],
      ['m_for_input',      '    for (int i = 0; i < n; i++) {'],
      ['m_read_start',     '        cin >> start[i];'],
      ['m_read_finish',    '        cin >> finish[i];'],
      ['',                 '    }'],
      ['m_call_solve',     '    solve(start, finish, n, 0, 0, 0);'],
      ['m_print_ans',      '    cout << maxActivities << endl;'],
      ['m_done',           '    return 0;'],
      ['',                 '}']
    ],
    python: [
      ['',                 'import sys'],
      ['',                 ''],
      ['',                 'max_activities = 0'],
      ['',                 ''],
      ['c_entry',          'def solve(start, finish, n, idx, last_finish, count):'],
      ['',                 '    global max_activities'],
      ['c_base_check',     '    if idx == n:'],
      ['c_update_check',   '        if count > max_activities:'],
      ['c_update_max',     '            max_activities = count'],
      ['c_ret_base',       '        return'],
      ['c_call_skip',      '    solve(start, finish, n, idx + 1, last_finish, count)'],
      ['c_check_compat',   '    if start[idx] >= last_finish:'],
      ['c_call_pick',      '        solve(start, finish, n, idx + 1, finish[idx], count + 1)'],
      ['',                 ''],
      ['',                 'def main():'],
      ['m_scanner',        '    tokens = sys.stdin.read().split()'],
      ['m_read_n',         '    if not tokens: return'],
      ['',                 '    n = int(tokens[0])'],
      ['m_alloc_start',    '    start = []'],
      ['m_alloc_finish',   '    finish = []'],
      ['m_for_input',      '    idx = 1'],
      ['',                 '    for i in range(n):'],
      ['m_read_start',     '        start.append(int(tokens[idx]))'],
      ['m_read_finish',    '        finish.append(int(tokens[idx + 1]))'],
      ['',                 '        idx += 2'],
      ['m_call_solve',     '    solve(start, finish, n, 0, 0, 0)'],
      ['m_print_ans',      '    print(max_activities)'],
      ['m_done',           '    return'],
      ['',                 ''],
      ['',                 'if __name__ == "__main__":'],
      ['',                 '    main()']
    ]
  },
  greedy: {
    java: [
      ['',                 'import java.util.Scanner;'],
      ['',                 'import java.util.Arrays;'],
      ['',                 ''],
      ['',                 'public class Main {'],
      ['c_entry',          '    static int activitySelection(int[] start, int[] finish, int n) {'],
      ['c_sort_init',      '        Integer[] idx = new Integer[n];'],
      ['c_sort_fill',      '        for (int i = 0; i < n; i++) {'],
      ['c_sort_body',      '            idx[i] = i;'],
      ['',                 '        }'],
      ['c_sort_exec',      '        Arrays.sort(idx, (a, b) -> Integer.compare(finish[a], finish[b]));'],
      ['c_init_count',     '        int count = 1;'],
      ['c_init_last',      '        int lastFinish = finish[idx[0]];'],
      ['c_for_loop',       '        for (int i = 1; i < n; i++) {'],
      ['c_check_compat',   '            if (start[idx[i]] >= lastFinish) {'],
      ['c_inc_count',      '                count++;'],
      ['c_update_last',    '                lastFinish = finish[idx[i]];'],
      ['',                 '            }'],
      ['',                 '        }'],
      ['c_ret_count',      '        return count;'],
      ['',                 '    }'],
      ['',                 ''],
      ['',                 '    public static void main(String[] args) {'],
      ['m_scanner',        '        Scanner sc = new Scanner(System.in);'],
      ['m_read_n',         '        int n = sc.nextInt();'],
      ['m_alloc_start',    '        int[] start = new int[n];'],
      ['m_alloc_finish',   '        int[] finish = new int[n];'],
      ['m_for_input',      '        for (int i = 0; i < n; i++) {'],
      ['m_read_start',     '            start[i] = sc.nextInt();'],
      ['m_read_finish',    '            finish[i] = sc.nextInt();'],
      ['',                 '        }'],
      ['m_call_select',    '        int ans = activitySelection(start, finish, n);'],
      ['m_print_ans',      '        System.out.println(ans);'],
      ['m_done',           '    }'],
      ['',                 '}']
    ],
    c: [
      ['',                 '#include <stdio.h>'],
      ['',                 '#include <stdlib.h>'],
      ['',                 ''],
      ['',                 'int finishGlobal[100];'],
      ['',                 'int compareActivities(const void* a, const void* b) {'],
      ['',                 '    return finishGlobal[*(int*)a] - finishGlobal[*(int*)b];'],
      ['',                 '}'],
      ['',                 ''],
      ['c_entry',          'int activitySelection(int start[], int finish[], int n) {'],
      ['c_sort_init',      '    int idx[100];'],
      ['c_sort_fill',      '    for (int i = 0; i < n; i++) {'],
      ['c_sort_body',      '        idx[i] = i;'],
      ['',                 '        finishGlobal[i] = finish[i];'],
      ['',                 '    }'],
      ['c_sort_exec',      '    qsort(idx, n, sizeof(int), compareActivities);'],
      ['c_init_count',     '    int count = 1;'],
      ['c_init_last',      '    int lastFinish = finish[idx[0]];'],
      ['c_for_loop',       '    for (int i = 1; i < n; i++) {'],
      ['c_check_compat',   '        if (start[idx[i]] >= lastFinish) {'],
      ['c_inc_count',      '            count++;'],
      ['c_update_last',    '            lastFinish = finish[idx[i]];'],
      ['',                 '        }'],
      ['',                 '    }'],
      ['c_ret_count',      '    return count;'],
      ['',                 '}'],
      ['',                 ''],
      ['',                 'int main() {'],
      ['m_scanner',        '    int n;'],
      ['m_read_n',         '    scanf("%d", &n);'],
      ['m_alloc_start',    '    int start[100];'],
      ['m_alloc_finish',   '    int finish[100];'],
      ['m_for_input',      '    for (int i = 0; i < n; i++) {'],
      ['m_read_start',     '        scanf("%d", &start[i]);'],
      ['m_read_finish',    '        scanf("%d", &finish[i]);'],
      ['',                 '    }'],
      ['m_call_select',    '    int ans = activitySelection(start, finish, n);'],
      ['m_print_ans',      '    printf("%d\\n", ans);'],
      ['m_done',           '    return 0;'],
      ['',                 '}']
    ],
    cpp: [
      ['',                 '#include <iostream>'],
      ['',                 '#include <vector>'],
      ['',                 '#include <algorithm>'],
      ['',                 'using namespace std;'],
      ['',                 ''],
      ['c_entry',          'int activitySelection(const vector<int>& start, const vector<int>& finish, int n) {'],
      ['c_sort_init',      '    vector<int> idx(n);'],
      ['c_sort_fill',      '    for (int i = 0; i < n; i++) {'],
      ['c_sort_body',      '        idx[i] = i;'],
      ['',                 '    }'],
      ['c_sort_exec',      '    sort(idx.begin(), idx.end(), [&](int a, int b) { return finish[a] < finish[b]; });'],
      ['c_init_count',     '    int count = 1;'],
      ['c_init_last',      '    int lastFinish = finish[idx[0]];'],
      ['c_for_loop',       '    for (int i = 1; i < n; i++) {'],
      ['c_check_compat',   '        if (start[idx[i]] >= lastFinish) {'],
      ['c_inc_count',      '            count++;'],
      ['c_update_last',    '            lastFinish = finish[idx[i]];'],
      ['',                 '        }'],
      ['',                 '    }'],
      ['c_ret_count',      '    return count;'],
      ['',                 '}'],
      ['',                 ''],
      ['',                 'int main() {'],
      ['m_scanner',        '    int n;'],
      ['m_read_n',         '    cin >> n;'],
      ['m_alloc_start',    '    vector<int> start(n);'],
      ['m_alloc_finish',   '    vector<int> finish(n);'],
      ['m_for_input',      '    for (int i = 0; i < n; i++) {'],
      ['m_read_start',     '        cin >> start[i];'],
      ['m_read_finish',    '        cin >> finish[i];'],
      ['',                 '    }'],
      ['m_call_select',    '    int ans = activitySelection(start, finish, n);'],
      ['m_print_ans',      '    cout << ans << endl;'],
      ['m_done',           '    return 0;'],
      ['',                 '}']
    ],
    python: [
      ['',                 'import sys'],
      ['',                 ''],
      ['c_entry',          'def activity_selection(start, finish, n):'],
      ['c_sort_init',      '    idx = list(range(n))'],
      ['c_sort_fill',      '    # Indices 0 to n - 1'],
      ['c_sort_exec',      '    idx.sort(key=lambda i: finish[i])'],
      ['c_init_count',     '    count = 1'],
      ['c_init_last',      '    last_finish = finish[idx[0]]'],
      ['c_for_loop',       '    for i in range(1, n):'],
      ['c_check_compat',   '        if start[idx[i]] >= last_finish:'],
      ['c_inc_count',      '            count += 1'],
      ['c_update_last',    '            last_finish = finish[idx[i]]'],
      ['c_ret_count',      '    return count'],
      ['',                 ''],
      ['',                 'def main():'],
      ['m_scanner',        '    tokens = sys.stdin.read().split()'],
      ['m_read_n',         '    if not tokens: return'],
      ['',                 '    n = int(tokens[0])'],
      ['m_alloc_start',    '    start = []'],
      ['m_alloc_finish',   '    finish = []'],
      ['m_for_input',      '    idx = 1'],
      ['',                 '    for i in range(n):'],
      ['m_read_start',     '        start.append(int(tokens[idx]))'],
      ['m_read_finish',    '        finish.append(int(tokens[idx + 1]))'],
      ['',                 '        idx += 2'],
      ['m_call_select',    '    ans = activity_selection(start, finish, n)'],
      ['m_print_ans',      '    print(ans)'],
      ['m_done',           '    return'],
      ['',                 ''],
      ['',                 'if __name__ == "__main__":'],
      ['',                 '    main()']
    ]
  }
};

/* ==================================================================== */
/* PSEUDOCODE FOR STUDENTS                                              */
/* ==================================================================== */
const PSEUDOCODES = {
  brute: [
    '// ─── Brute Force: Exhaustive Subset Search ─────────────────────────────',
    'maxActivities = 0',
    '',
    'function solve(start[], finish[], n, idx, lastFinish, count):',
    '',
    '    // Base case: all n activities have been considered',
    '    if idx == n:',
    '        if count > maxActivities:          // Update best count found so far',
    '            maxActivities = count',
    '        return',
    '',
    '    // Choice 1: SKIP activity idx (always a valid branch)',
    '    solve(start, finish, n, idx + 1, lastFinish, count)',
    '',
    '    // Choice 2: PICK activity idx (only if it does not overlap)',
    '    if start[idx] >= lastFinish:',
    '        solve(start, finish, n, idx + 1, finish[idx], count + 1)',
    '',
    '// ─── Entry Point ────────────────────────────────────────────────────────',
    'function main():',
    '    read n, start[], finish[]',
    '    solve(start, finish, n, 0, 0, 0)',
    '    print maxActivities',
    '',
    '// Time  : O(2^n)  — two recursive branches per activity',
    '// Space : O(n)    — recursion call stack depth'
  ],
  greedy: [
    '// ─── Greedy: Earliest Finish Time First ────────────────────────────────',
    'function activitySelection(start[], finish[], n):',
    '',
    '    // Step 1: Create an index array (avoids modifying input arrays)',
    '    idx = new Integer[n]',
    '    for i = 0 to n - 1:',
    '        idx[i] = i',
    '',
    '    // Step 2: Sort idx by finish[idx[i]] ascending',
    '    sort idx such that finish[idx[0]] <= finish[idx[1]] <= ... <= finish[idx[n-1]]',
    '',
    '    // Step 3: Greedily select the first activity (smallest finish time)',
    '    count      = 1',
    '    lastFinish = finish[idx[0]]',
    '',
    '    // Step 4: Scan remaining activities in sorted order',
    '    for i = 1 to n - 1:',
    '        if start[idx[i]] >= lastFinish:    // No overlap with last selected',
    '            count      = count + 1',
    '            lastFinish = finish[idx[i]]    // Advance the finish boundary',
    '',
    '    return count',
    '',
    '// Time  : O(n log n)  — dominated by sorting step',
    '// Space : O(n)        — auxiliary index array idx[]'
  ]
};

/* ==================================================================== */
/* DEFAULT SAMPLE DATA (2 SEPARATE ARRAYS)                              */
/* ==================================================================== */
const DEFAULT_STARTS = [1, 3, 0, 5, 3, 5];
const DEFAULT_FINISHES = [4, 5, 6, 7, 9, 9];

function frame(title, rows) {
  return { title, rows };
}

/* ==================================================================== */
/* BUILD STEPS FOR ANIMATION (STRICT 1-TO-1 LINE-BY-LINE)               */
/* ==================================================================== */
function buildSteps(approach, rawStarts, rawFinishes) {
  const steps = [];
  const starts = (rawStarts && rawStarts.length) ? [...rawStarts] : [...DEFAULT_STARTS];
  const finishes = (rawFinishes && rawFinishes.length) ? [...rawFinishes] : [...DEFAULT_FINISHES];
  const n = Math.min(starts.length, finishes.length);
  starts.length = n;
  finishes.length = n;

  const activities = [];
  for (let i = 0; i < n; i++) {
    activities.push({ id: i, start: starts[i], finish: finishes[i] });
  }

  /* ------------------------------------------------------------------ */
  /* APPROACH 1: BRUTE FORCE (RECURSIVE SUBSET SEARCH)                   */
  /* ------------------------------------------------------------------ */
  if (approach === 'brute') {
    let maxSelected = 0;
    let bestSubset = [];
    const allNodes = [];
    let nodeIdCounter = 0;

    function buildBruteTree(idx, lastFinish, count, parentId, picked) {
      const nodeId = nodeIdCounter++;
      const node = { id: nodeId, idx, lastFinish, count, parentId, picked: [...picked], children: [], isLeaf: false };
      allNodes.push(node);
      if (parentId !== null) {
        const par = allNodes.find(x => x.id === parentId);
        if (par) par.children.push(nodeId);
      }
      if (idx === n) {
        node.isLeaf = true;
        if (count > maxSelected) { maxSelected = count; bestSubset = [...picked]; }
        return nodeId;
      }
      buildBruteTree(idx + 1, lastFinish, count, nodeId, picked);
      if (starts[idx] >= lastFinish) {
        buildBruteTree(idx + 1, finishes[idx], count + 1, nodeId, [...picked, idx]);
      }
      return nodeId;
    }
    buildBruteTree(0, 0, 0, null, []);

    const nodeStateMap = {};
    allNodes.forEach(nd => { nodeStateMap[nd.id] = { ...nd, state: 'hidden', retVal: null }; });

    const leafSpacing = 44;
    const levelHeight = 36;
    let leafCounter = 0;

    function assignSubtreeLeaves(nodeId, depth) {
      const nd = allNodes.find(x => x.id === nodeId);
      if (!nd) return;
      nd._depth = depth;
      if (nd.children.length === 0) {
        nd._leafIdx = leafCounter++;
      } else {
        nd.children.forEach(cid => assignSubtreeLeaves(cid, depth + 1));
      }
    }
    assignSubtreeLeaves(0, 0);

    const totalLeaves = Math.max(1, leafCounter);
    const maxDepth = Math.max(0, ...allNodes.map(nd => nd._depth || 0));
    const treeWidth = Math.max(220, totalLeaves * leafSpacing + 30);
    const treeHeight = Math.max(120, maxDepth * levelHeight + 46);

    function assignCoords(nodeId) {
      const nd = allNodes.find(x => x.id === nodeId);
      if (!nd) return 0;
      if (nd.children.length === 0) {
        nd._x = 15 + nd._leafIdx * leafSpacing + leafSpacing / 2;
      } else {
        const childXs = nd.children.map(cid => assignCoords(cid));
        nd._x = (childXs[0] + childXs[childXs.length - 1]) / 2;
      }
      nd._y = 20 + nd._depth * levelHeight;
      return nd._x;
    }
    assignCoords(0);

    function getVisibleNodes() {
      return allNodes.filter(nd => nodeStateMap[nd.id].state !== 'hidden')
        .map(nd => ({ ...nd, ...nodeStateMap[nd.id], x: nd._x, y: nd._y }));
    }
    function getVisibleEdges() {
      const edges = [];
      allNodes.forEach(nd => {
        if (nd.parentId !== null && nodeStateMap[nd.id].state !== 'hidden' && nodeStateMap[nd.parentId].state !== 'hidden') {
          const par = allNodes.find(x => x.id === nd.parentId);
          if (par) {
            edges.push({
              x1: par._x, y1: par._y + 11,
              x2: nd._x, y2: nd._y - 11,
              isPick: nd.picked.length > (par.picked ? par.picked.length : 0)
            });
          }
        }
      });
      return edges;
    }

    function curState(extra = {}) {
      return {
        approach: 'brute',
        n,
        starts,
        finishes,
        activities,
        treeWidth,
        treeHeight,
        treeNodes: getVisibleNodes(),
        treeEdges: getVisibleEdges(),
        ...extra
      };
    }

    steps.push({
      code: 'm_scanner',
      badge: 'Faculty Note: Scanner sc = new Scanner(System.in); — Initializing input stream.',
      vars: [frame('main()', [['n', '?']])],
      state: curState({ callCounter: 0, maxSelected: 0, bestSubset: [], activeNodeId: null, currentIdx: -1, lastFinish: 0, currentPicked: [], startFilled: 0, finishFilled: 0 })
    });
    steps.push({
      code: 'm_read_n',
      badge: `Faculty Note: int n = sc.nextInt(); &rarr; Reading total number of activities n = ${n}.`,
      vars: [frame('main()', [['n', String(n)]])],
      state: curState({ callCounter: 0, maxSelected: 0, bestSubset: [], activeNodeId: null, currentIdx: -1, lastFinish: 0, currentPicked: [], startFilled: 0, finishFilled: 0 })
    });
    steps.push({
      code: 'm_alloc_start',
      badge: `Faculty Note: int[] start = new int[${n}]; &mdash; Allocating 1st array for start times.`,
      vars: [frame('main()', [['n', String(n)]])],
      state: curState({ callCounter: 0, maxSelected: 0, bestSubset: [], activeNodeId: null, currentIdx: -1, lastFinish: 0, currentPicked: [], startFilled: 0, finishFilled: 0 })
    });
    steps.push({
      code: 'm_alloc_finish',
      badge: `Faculty Note: int[] finish = new int[${n}]; &mdash; Allocating 2nd array for finish times.`,
      vars: [frame('main()', [['n', String(n)]])],
      state: curState({ callCounter: 0, maxSelected: 0, bestSubset: [], activeNodeId: null, currentIdx: -1, lastFinish: 0, currentPicked: [], startFilled: 0, finishFilled: 0 })
    });

    for (let i = 0; i < n; i++) {
      steps.push({
        code: 'm_for_input',
        badge: `Loop index i = ${i} of ${n}: reading start and finish times for activity ${i}.`,
        vars: [frame('main()', [['i', String(i)], ['n', String(n)]])],
        state: curState({ callCounter: 0, maxSelected: 0, bestSubset: [], activeNodeId: null, currentIdx: i, lastFinish: 0, currentPicked: [], startFilled: i, finishFilled: i })
      });
      steps.push({
        code: 'm_read_start',
        badge: `start[${i}] = sc.nextInt(); \u2192 start[${i}] = ${starts[i]} read into the start[] array.`,
        vars: [frame('main()', [['i', String(i)], ['start[' + i + ']', String(starts[i])]])],
        state: curState({ callCounter: 0, maxSelected: 0, bestSubset: [], activeNodeId: null, currentIdx: i, lastFinish: 0, currentPicked: [], startFilled: i + 1, finishFilled: i })
      });
      steps.push({
        code: 'm_read_finish',
        badge: `finish[${i}] = sc.nextInt(); \u2192 finish[${i}] = ${finishes[i]} read into the finish[] array.`,
        vars: [frame('main()', [['i', String(i)], ['start[' + i + ']', String(starts[i])], ['finish[' + i + ']', String(finishes[i])]])],
        state: curState({ callCounter: 0, maxSelected: 0, bestSubset: [], activeNodeId: null, currentIdx: i, lastFinish: 0, currentPicked: [], startFilled: i + 1, finishFilled: i + 1 })
      });
    }

    steps.push({
      code: 'm_call_solve',
      badge: `Calling solve(start, finish, n=${n}, idx=0, lastFinish=0, count=0). Exhaustive subset recursion begins!`,
      vars: [frame('main()', [['n', String(n)]])],
      state: curState({ callCounter: 0, maxSelected: 0, bestSubset: [], activeNodeId: null, currentIdx: -1, lastFinish: 0, currentPicked: [] })
    });

    let simMax = 0, simCalls = 0, simBest = [];

    function simulateBrute(idx, lastFinish, count, parentId, picked, nodeId) {
      simCalls++;
      nodeStateMap[nodeId].state = 'active';
      const cs = [
        frame('main()', [['n', String(n)]]),
        frame(`solve(idx=${idx})`, [['idx', String(idx)], ['lastFinish', String(lastFinish)], ['count', String(count)]])
      ];

      steps.push({
        code: 'c_entry',
        badge: `Entering solve(idx=${idx}, lastFinish=${lastFinish}, count=${count}). Recursive Call #${simCalls}.`,
        vars: cs,
        state: curState({ callCounter: simCalls, maxSelected: simMax, bestSubset: [...simBest], activeNodeId: nodeId, currentIdx: idx, lastFinish, currentPicked: [...picked] })
      });

      const isBase = (idx === n);
      steps.push({
        code: 'c_base_check',
        badge: `Base check: if (idx == ${n}) &rarr; ${isBase ? 'TRUE (Reached leaf! All activities evaluated)' : 'FALSE (More activities remaining)'}`,
        vars: cs,
        state: curState({ callCounter: simCalls, maxSelected: simMax, bestSubset: [...simBest], activeNodeId: nodeId, currentIdx: idx, lastFinish, currentPicked: [...picked] })
      });

      if (isBase) {
        nodeStateMap[nodeId].state = 'leaf';
        nodeStateMap[nodeId].retVal = count;
        const isBetter = count > simMax;

        steps.push({
          code: 'c_update_check',
          badge: `Subset complete! Check: if (count=${count} > maxActivities=${simMax}) &rarr; ${isBetter ? 'TRUE! New optimal subset found!' : 'FALSE (Does not exceed current best)'}`,
          vars: cs,
          state: curState({ callCounter: simCalls, maxSelected: simMax, bestSubset: [...simBest], activeNodeId: nodeId, currentIdx: idx, lastFinish, currentPicked: [...picked] })
        });

        if (isBetter) {
          simMax = count;
          simBest = [...picked];
          steps.push({
            code: 'c_update_max',
            badge: `maxActivities updated to ${simMax}! Optimal subset so far: {${simBest.map(i => `A${i}`).join(', ')}}`,
            vars: cs,
            state: curState({ callCounter: simCalls, maxSelected: simMax, bestSubset: [...simBest], activeNodeId: nodeId, currentIdx: idx, lastFinish, currentPicked: [...picked] })
          });
        }

        steps.push({
          code: 'c_ret_base',
          badge: `return; &mdash; Backtracking from leaf at depth ${idx}.`,
          vars: cs,
          state: curState({ callCounter: simCalls, maxSelected: simMax, bestSubset: [...simBest], activeNodeId: nodeId, currentIdx: idx, lastFinish, currentPicked: [...picked] })
        });
        nodeStateMap[nodeId].state = 'solved';
        return;
      }

      // Choice 1: c_call_skip
      steps.push({
        code: 'c_call_skip',
        badge: `Choice 1: SKIP Activity A${idx} [start=${starts[idx]}, finish=${finishes[idx]}]. Branching left...`,
        vars: cs,
        state: curState({ callCounter: simCalls, maxSelected: simMax, bestSubset: [...simBest], activeNodeId: nodeId, currentIdx: idx, lastFinish, currentPicked: [...picked] })
      });

      const skipChild = allNodes.find(nd => nd.parentId === nodeId && nd.picked.length === picked.length);
      if (skipChild) {
        simulateBrute(idx + 1, lastFinish, count, nodeId, picked, skipChild.id);
      }
      nodeStateMap[nodeId].state = 'active';

      // Choice 2: c_check_compat
      const isCompat = starts[idx] >= lastFinish;
      steps.push({
        code: 'c_check_compat',
        badge: `Check compatibility: if (start[${idx}]=${starts[idx]} >= lastFinish=${lastFinish}) &rarr; ${isCompat ? 'TRUE (Compatible! Can branch to PICK)' : 'FALSE (Overlaps with last selected activity! Cannot pick)'}`,
        vars: cs,
        state: curState({ callCounter: simCalls, maxSelected: simMax, bestSubset: [...simBest], activeNodeId: nodeId, currentIdx: idx, lastFinish, currentPicked: [...picked] })
      });

      if (isCompat) {
        steps.push({
          code: 'c_call_pick',
          badge: `Choice 2: PICK Activity A${idx} [start=${starts[idx]}, finish=${finishes[idx]}]. Advances lastFinish to ${finishes[idx]}. Branching right...`,
          vars: cs,
          state: curState({ callCounter: simCalls, maxSelected: simMax, bestSubset: [...simBest], activeNodeId: nodeId, currentIdx: idx, lastFinish, currentPicked: [...picked] })
        });

        const pickChild = allNodes.find(nd => nd.parentId === nodeId && nd.picked.length > picked.length);
        if (pickChild) {
          simulateBrute(idx + 1, finishes[idx], count + 1, nodeId, [...picked, idx], pickChild.id);
        }
      }

      nodeStateMap[nodeId].state = 'solved';
    }

    simulateBrute(0, 0, 0, null, [], 0);

    steps.push({
      code: 'm_print_ans',
      badge: `Brute Force Complete! Maximum compatible activities = ${simMax}. Optimal subset: {${simBest.map(i => `A${i}`).join(', ')}}`,
      vars: [frame('main()', [['maxActivities', String(simMax)], ['subset', simBest.map(i => `A${i}`).join(', ')]])],
      state: curState({ callCounter: simCalls, maxSelected: simMax, bestSubset: [...simBest], activeNodeId: null, currentIdx: -1, lastFinish: 0, currentPicked: [] })
    });
    steps.push({
      code: 'm_done',
      badge: 'Program execution completed. Total recursive calls evaluated: ' + simCalls,
      vars: [frame('main()', [['status', 'finished']])],
      state: curState({ callCounter: simCalls, maxSelected: simMax, bestSubset: [...simBest], activeNodeId: null, currentIdx: -1, lastFinish: 0, currentPicked: [] })
    });

    return steps;
  }

  /* ------------------------------------------------------------------ */
  /* APPROACH 2: GREEDY WITH 2 SEPARATE ARRAYS                          */
  /* ------------------------------------------------------------------ */
  else {
    // Indices sorted by finish[idx]
    const sortedIndices = Array.from({ length: n }, (_, i) => i);
    sortedIndices.sort((a, b) => finishes[a] - finishes[b]);

    const sortedActivities = sortedIndices.map(i => ({
      id: i,
      start: starts[i],
      finish: finishes[i]
    }));

    function makeTimeline(currRank, selectedIds, lastF) {
      return sortedActivities.map((act, r) => {
        let state = 'pending';
        if (selectedIds.includes(act.id)) {
          state = 'selected';
        } else if (currRank !== -1 && r === currRank) {
          state = 'checking';
        } else if (currRank !== -1 && r < currRank) {
          state = 'skipped';
        }
        return {
          ...act,
          sortedRank: r,
          state
        };
      });
    }

    function curGreedyState(extra = {}) {
      return {
        approach: 'greedy',
        n,
        starts,
        finishes,
        activities,
        sortedActivities,
        sortedIndices,
        ...extra
      };
    }

    // Step 1: m_scanner
    steps.push({
      code: 'm_scanner',
      badge: 'Faculty Note: Scanner sc = new Scanner(System.in); — Initializing input stream.',
      vars: [frame('main()', [['n', '?']])],
      state: curGreedyState({ timeline: makeTimeline(-1, [], 0), selectedIds: [], count: 0, lastFinish: 0, currentI: -1, phase: 'input', startFilled: 0, finishFilled: 0 })
    });

    // Step 2: m_read_n
    steps.push({
      code: 'm_read_n',
      badge: `Faculty Note: int n = sc.nextInt(); &rarr; Read n = ${n} activities to schedule.`,
      vars: [frame('main()', [['n', String(n)]])],
      state: curGreedyState({ timeline: makeTimeline(-1, [], 0), selectedIds: [], count: 0, lastFinish: 0, currentI: -1, phase: 'input', startFilled: 0, finishFilled: 0 })
    });

    // Step 3: m_alloc_start & m_alloc_finish (2 SEPARATE ARRAYS)
    steps.push({
      code: 'm_alloc_start',
      badge: `Faculty Note: int[] start = new int[${n}]; &mdash; Allocating 1st separate array for start times.`,
      vars: [frame('main()', [['n', String(n)]])],
      state: curGreedyState({ timeline: makeTimeline(-1, [], 0), selectedIds: [], count: 0, lastFinish: 0, currentI: -1, phase: 'input', startFilled: 0, finishFilled: 0 })
    });
    steps.push({
      code: 'm_alloc_finish',
      badge: `Faculty Note: int[] finish = new int[${n}]; &mdash; Allocating 2nd separate array for finish times.`,
      vars: [frame('main()', [['n', String(n)]])],
      state: curGreedyState({ timeline: makeTimeline(-1, [], 0), selectedIds: [], count: 0, lastFinish: 0, currentI: -1, phase: 'input', startFilled: 0, finishFilled: 0 })
    });

    // Step 4: m_for_input, m_read_start, m_read_finish — one value at a time
    for (let i = 0; i < n; i++) {
      steps.push({
        code: 'm_for_input',
        badge: `Reading activity ${i} of ${n} in loop (i = ${i}).`,
        vars: [frame('main()', [['i', String(i)], ['n', String(n)]])],
        state: curGreedyState({ timeline: makeTimeline(-1, [], 0), selectedIds: [], count: 0, lastFinish: 0, currentI: -1, phase: 'input', startFilled: i, finishFilled: i })
      });
      steps.push({
        code: 'm_read_start',
        badge: `start[${i}] = sc.nextInt(); \u2192 start[${i}] = ${starts[i]} read into the start[] array.`,
        vars: [frame('main()', [['i', String(i)], ['start[' + i + ']', String(starts[i])]])],
        state: curGreedyState({ timeline: makeTimeline(-1, [], 0), selectedIds: [], count: 0, lastFinish: 0, currentI: -1, phase: 'input', startFilled: i + 1, finishFilled: i })
      });
      steps.push({
        code: 'm_read_finish',
        badge: `finish[${i}] = sc.nextInt(); \u2192 finish[${i}] = ${finishes[i]} read into the finish[] array.`,
        vars: [frame('main()', [['i', String(i)], ['start[' + i + ']', String(starts[i])], ['finish[' + i + ']', String(finishes[i])]])],
        state: curGreedyState({ timeline: makeTimeline(-1, [], 0), selectedIds: [], count: 0, lastFinish: 0, currentI: -1, phase: 'input', startFilled: i + 1, finishFilled: i + 1 })
      });
    }

    // Step 5: m_call_select
    steps.push({
      code: 'm_call_select',
      badge: `Calling activitySelection(start, finish, n=${n}) with 2 separate arrays!`,
      vars: [frame('main()', [['n', String(n)]])],
      state: curGreedyState({ timeline: makeTimeline(-1, [], 0), selectedIds: [], count: 0, lastFinish: 0, currentI: -1, phase: 'call' })
    });

    // Step 6: c_entry
    steps.push({
      code: 'c_entry',
      badge: 'Entering activitySelection(start, finish, n). Strategy: Sort activities by finish[] and pick earliest finish!',
      vars: [frame('main()', [['n', String(n)]]), frame('activitySelection()', [['n', String(n)]])],
      state: curGreedyState({ timeline: makeTimeline(-1, [], 0), selectedIds: [], count: 0, lastFinish: 0, currentI: -1, phase: 'entry' })
    });

    // Step 7: Index array sort-fill loop — animate line by line
    steps.push({
      code: 'c_sort_init',
      badge: `Faculty Note: Integer[] idx = new Integer[${n}]; &mdash; Creating index array to sort activities without destroying original start[] and finish[].`,
      vars: [frame('main()', [['n', String(n)]]), frame('activitySelection()', [['idx', 'new Integer[' + n + ']']])],
      state: curGreedyState({ selectedIds: [], count: 0, lastFinish: 0, currentI: -1, phase: 'sort', idxFilled: 0, sortLoopI: -1 })
    });

    // Animate each iteration of: for (int i = 0; i < n; i++) { idx[i] = i; }
    for (let fi = 0; fi < n; fi++) {
      // Loop header highlight
      steps.push({
        code: 'c_sort_fill',
        badge: `for loop: i = ${fi} (i < ${n}) &mdash; About to assign idx[${fi}] = ${fi}.`,
        vars: [frame('main()', [['n', String(n)]]), frame('activitySelection()', [['i', String(fi)], ['idx', `[0..${fi > 0 ? fi - 1 : '?'}]`]])],
        state: curGreedyState({ selectedIds: [], count: 0, lastFinish: 0, currentI: -1, phase: 'sort', idxFilled: fi, sortLoopI: fi })
      });
      // Body: idx[i] = i
      steps.push({
        code: 'c_sort_body',
        badge: `idx[${fi}] = ${fi} &mdash; Index ${fi} now points to Activity A${fi}.`,
        vars: [frame('main()', [['n', String(n)]]), frame('activitySelection()', [['idx[' + fi + ']', String(fi)]])],
        state: curGreedyState({ selectedIds: [], count: 0, lastFinish: 0, currentI: -1, phase: 'sort', idxFilled: fi + 1, sortLoopI: fi })
      });
    }

    steps.push({
      code: 'c_sort_exec',
      badge: `Arrays.sort(idx, by finish[idx]). Sorted order by finish times: [${sortedIndices.map(i => `A${i}(f=${finishes[i]})`).join(' &rarr; ')}].`,
      vars: [frame('main()', [['n', String(n)]]), frame('activitySelection()', [['sortedIdx', sortedIndices.map(i => `A${i}`).join(',')]])],
      state: curGreedyState({ timeline: makeTimeline(-1, [], 0), selectedIds: [], count: 0, lastFinish: 0, currentI: -1, phase: 'sorted' })
    });

    // Step 8: c_init_count (Pick first activity)
    const firstActId = sortedIndices[0];
    const selectedIds = [firstActId];
    let count = 1;
    let lastFinish = finishes[firstActId];

    steps.push({
      code: 'c_init_count',
      badge: `Greedy choice: Always pick 1st activity A${firstActId} (finish[${firstActId}] = ${lastFinish})! Frees up resource earliest. count = 1.`,
      vars: [frame('main()', [['n', String(n)]]), frame('activitySelection()', [['count', '1'], ['selected', `A${firstActId}`]])],
      state: curGreedyState({ timeline: makeTimeline(0, selectedIds, lastFinish), selectedIds: [...selectedIds], count, lastFinish, currentI: 0, phase: 'select' })
    });

    // Step 9: c_init_last
    steps.push({
      code: 'c_init_last',
      badge: `lastFinish = finish[idx[0]] = ${lastFinish}. Laser guide placed at time ${lastFinish}. Next activity must satisfy start >= ${lastFinish}.`,
      vars: [frame('main()', [['n', String(n)]]), frame('activitySelection()', [['lastFinish', String(lastFinish)]])],
      state: curGreedyState({ timeline: makeTimeline(0, selectedIds, lastFinish), selectedIds: [...selectedIds], count, lastFinish, currentI: 0, phase: 'select' })
    });

    // Step 10: Scan remaining activities 1 to n-1
    for (let i = 1; i < n; i++) {
      const actId = sortedIndices[i];
      const isCompat = starts[actId] >= lastFinish;

      steps.push({
        code: 'c_for_loop',
        badge: `for (int i = ${i}; i < ${n}; i++) &mdash; Examining Activity A${actId} (start[${actId}]=${starts[actId]}, finish[${actId}]=${finishes[actId]}).`,
        vars: [frame('main()', [['n', String(n)]]), frame('activitySelection()', [['i', String(i)], ['idx[i]', String(actId)], ['lastFinish', String(lastFinish)]])],
        state: curGreedyState({ timeline: makeTimeline(i, selectedIds, lastFinish), selectedIds: [...selectedIds], count, lastFinish, currentI: i, phase: 'loop' })
      });

      steps.push({
        code: 'c_check_compat',
        badge: `Compatibility Check: if (start[idx[${i}]] = ${starts[actId]} >= lastFinish = ${lastFinish}) &rarr; ${isCompat ? 'TRUE! No overlap with scheduled activities.' : 'FALSE! Conflict: starts before last finish time.'}`,
        vars: [frame('main()', [['n', String(n)]]), frame('activitySelection()', [['start[idx[i]]', String(starts[actId])], ['lastFinish', String(lastFinish)]])],
        state: curGreedyState({ timeline: makeTimeline(i, selectedIds, lastFinish), selectedIds: [...selectedIds], count, lastFinish, currentI: i, phase: 'check' })
      });

      if (isCompat) {
        count++;
        selectedIds.push(actId);

        steps.push({
          code: 'c_inc_count',
          badge: `Activity A${actId} [${starts[actId]}, ${finishes[actId]}] SELECTED! count increments to ${count}.`,
          vars: [frame('main()', [['n', String(n)]]), frame('activitySelection()', [['count', String(count)]])],
          state: curGreedyState({ timeline: makeTimeline(i, selectedIds, lastFinish), selectedIds: [...selectedIds], count, lastFinish, currentI: i, phase: 'select' })
        });

        lastFinish = finishes[actId];
        steps.push({
          code: 'c_update_last',
          badge: `lastFinish = finish[idx[${i}]] = ${lastFinish}. Laser boundary advances forward to time ${lastFinish}.`,
          vars: [frame('main()', [['n', String(n)]]), frame('activitySelection()', [['lastFinish', String(lastFinish)]])],
          state: curGreedyState({ timeline: makeTimeline(i, selectedIds, lastFinish), selectedIds: [...selectedIds], count, lastFinish, currentI: i, phase: 'select' })
        });
      }
    }

    // Step 11: c_ret_count
    steps.push({
      code: 'c_ret_count',
      badge: `Scan complete! return count = ${count}. Mutually compatible activities: {${selectedIds.map(id => `A${id}`).join(', ')}}.`,
      vars: [frame('main()', [['n', String(n)]]), frame('activitySelection()', [['return', String(count)]])],
      state: curGreedyState({ timeline: makeTimeline(-1, selectedIds, lastFinish), selectedIds: [...selectedIds], count, lastFinish, currentI: -1, phase: 'done' })
    });

    // Step 12: m_print_ans
    steps.push({
      code: 'm_print_ans',
      badge: `System.out.println(ans = ${count}); &mdash; Optimal solution computed in O(n log n) time!`,
      vars: [frame('main()', [['ans', String(count)]])],
      state: curGreedyState({ timeline: makeTimeline(-1, selectedIds, lastFinish), selectedIds: [...selectedIds], count, lastFinish, currentI: -1, phase: 'done' })
    });

    // Step 13: m_done
    steps.push({
      code: 'm_done',
      badge: 'Program finished. Greedy strategy proved globally optimal via Exchange Argument!',
      vars: [frame('main()', [['status', 'completed']])],
      state: curGreedyState({ timeline: makeTimeline(-1, selectedIds, lastFinish), selectedIds: [...selectedIds], count, lastFinish, currentI: -1, phase: 'done' })
    });

    return steps;
  }
}

/* ==================================================================== */
/* COMPONENT STATE & REACTIVE SETUP                                     */
/* ==================================================================== */
const currentApproach = ref('greedy');
const inputStartText = ref('1, 3, 0, 5, 3, 5');
const inputFinishText = ref('4, 5, 6, 7, 9, 9');
const lang = ref('java');
const speed = ref(650);
const si = ref(0);
const playing = ref(false);
const vizHeight = ref(185);
const tableHeight = ref(42);
const leftWidth = ref(54);
const rightTab = ref('code');

function parseArray(text) {
  return text.split(/[,;\s]+/)
    .map(x => parseInt(x.trim(), 10))
    .filter(x => !isNaN(x));
}

const currentStarts = computed(() => {
  const arr = parseArray(inputStartText.value);
  return arr.length >= 2 ? arr : DEFAULT_STARTS;
});

const currentFinishes = computed(() => {
  const arr = parseArray(inputFinishText.value);
  return arr.length >= 2 ? arr : DEFAULT_FINISHES;
});

const currentActivities = computed(() => {
  const s = currentStarts.value;
  const f = currentFinishes.value;
  const n = Math.min(s.length, f.length);
  const res = [];
  for (let i = 0; i < n; i++) {
    res.push({ id: i, start: s[i], finish: f[i] });
  }
  return res;
});

const stepsData = reactive({
  steps: buildSteps('greedy', DEFAULT_STARTS, DEFAULT_FINISHES)
});

const steps = computed(() => stepsData.steps);
const s = computed(() => steps.value[Math.max(0, Math.min(si.value, steps.value.length - 1))] || { state: {} });
const st = computed(() => s.value.state || {});

const codeLines = computed(() => {
  const appCodes = CODES[currentApproach.value] || CODES.greedy;
  return appCodes[lang.value] || appCodes.java || [];
});
const pseudocodeLines = computed(() => PSEUDOCODES[currentApproach.value] || PSEUDOCODES.greedy);

let playTimer = null;

function applyApproach(appId) {
  currentApproach.value = appId;
  playing.value = false;
  stepsData.steps = buildSteps(appId, currentStarts.value, currentFinishes.value);
  si.value = 0;
}

function applyInput() {
  playing.value = false;
  stepsData.steps = buildSteps(currentApproach.value, currentStarts.value, currentFinishes.value);
  si.value = 0;
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

function onKeydown(e) {
  const tag = e.target.tagName;
  if (tag === 'INPUT' || tag === 'SELECT' || tag === 'TEXTAREA') return;
  if (e.key === 'ArrowRight') stepBy(1);
  if (e.key === 'ArrowLeft') stepBy(-1);
  if (e.key === ' ') { e.preventDefault(); togglePlay(); }
}

/* ==================================================================== */
/* PANEL RESIZERS                                                       */
/* ==================================================================== */
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
    leftWidth.value = (Math.max(220, Math.min(mainW - 220, startW + e.clientX - startX)) / mainW) * 100;
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
  cleanupFns.push(initVResizer(vizResizerRef, vizHeight, 130, 420));
  cleanupFns.push(initVResizer(tableResizerRef, tableHeight, 30, 140));
});

onUnmounted(() => {
  document.removeEventListener('keydown', onKeydown);
  clearTimeout(playTimer);
  cleanupFns.forEach(fn => fn && fn());
});

const maxTime = computed(() => {
  const f = st.value.finishes || currentFinishes.value;
  if (!f || !f.length) return 12;
  const maxF = Math.max(...f);
  return Math.max(10, maxF + 2);
});

const TIME_SCALE = computed(() => {
  return Math.min(26, Math.max(16, 280 / maxTime.value));
});
</script>

<template>
  <div class="slide-wrapper">
    <!-- Top Navbar -->
    <div class="navbar">
      <h2 class="navbar-title">{{ topic }} &mdash; {{ subTopic }}</h2>
      <img src="../../assets/logo.png" alt="Logo" />
    </div>

    <div class="slide-body">
      <div class="row-main">
        <div class="ll-root">
          <!-- Control Panel Toolbar -->
          <div class="ll-toolbar">
            <!-- Approach Selector Button Group -->
            <div class="ll-approach-group">
              <button
                v-for="app in APPROACHES"
                :key="app.id"
                class="ll-approach-btn"
                :class="{ active: currentApproach === app.id }"
                :title="app.desc"
                @click="applyApproach(app.id)"
              >
                {{ app.label }}
              </button>
            </div>

            <!-- Custom 2 Separate Array Inputs -->
            <div class="ll-input-group">
              <label>start =</label>
              <input
                type="text"
                v-model="inputStartText"
                class="ll-text-input"
                placeholder="1, 3, 0, 5, 3, 5"
                @keyup.enter="applyInput"
                style="width: 125px;"
              />
            </div>

            <div class="ll-input-group">
              <label>finish =</label>
              <input
                type="text"
                v-model="inputFinishText"
                class="ll-text-input"
                placeholder="4, 5, 6, 7, 9, 9"
                @keyup.enter="applyInput"
                style="width: 125px;"
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
              <!-- Top Banner: start[] and finish[] with ?-until-filled behaviour -->
              <div class="ll-house-banner">
                <div class="ll-dual-arrays-wrap">
                  <!-- Row 1: start[] -->
                  <div class="ll-arr-row">
                    <span class="ll-arr-tag">start[]:</span>
                    <div class="ll-arr-cells">
                      <div
                        v-for="(act, idx) in currentActivities"
                        :key="'s' + idx"
                        class="ll-arr-cell"
                        :class="{
                          'll-cell-active': st.currentIdx === idx || (st.sortedActivities && st.sortedActivities[st.currentI] && st.sortedActivities[st.currentI].id === idx)
                        }"
                      >
                        <span class="ll-cell-idx">[{{ idx }}]</span>
                        <span class="ll-cell-val">
                          {{ (currentApproach === 'brute' && st.startFilled !== undefined && idx >= st.startFilled) ? '?' : (currentApproach === 'greedy' && st.phase === 'input' && st.startFilled !== undefined && idx >= st.startFilled) ? '?' : act.start }}
                        </span>
                      </div>
                    </div>
                  </div>

                  <!-- Row 2: finish[] -->
                  <div class="ll-arr-row">
                    <span class="ll-arr-tag">finish[]:</span>
                    <div class="ll-arr-cells">
                      <div
                        v-for="(act, idx) in currentActivities"
                        :key="'f' + idx"
                        class="ll-arr-cell"
                        :class="{
                          'll-cell-active': st.currentIdx === idx || (st.sortedActivities && st.sortedActivities[st.currentI] && st.sortedActivities[st.currentI].id === idx),
                          'll-cell-selected': st.selectedIds && st.selectedIds.includes(act.id)
                        }"
                      >
                        <span class="ll-cell-idx">[{{ idx }}]</span>
                        <span class="ll-cell-val">
                          {{ (currentApproach === 'brute' && st.finishFilled !== undefined && idx >= st.finishFilled) ? '?' : (currentApproach === 'greedy' && st.phase === 'input' && st.finishFilled !== undefined && idx >= st.finishFilled) ? '?' : act.finish }}
                        </span>
                      </div>
                    </div>
                  </div>
                </div>
              </div>

              <!-- Real-time Stats Chips Bar -->
              <div class="ll-ptrs">
                <div class="ll-ptr-chip">N: <b class="ll-c-blue">{{ st.n || currentActivities.length }}</b></div>
                <template v-if="currentApproach === 'greedy'">
                  <div class="ll-ptr-chip">
                    Selected Count: <b class="ll-c-green">{{ st.count || 0 }}</b>
                  </div>
                  <div class="ll-ptr-chip" v-if="st.lastFinish !== undefined">
                    Last Finish: <b class="ll-c-orange">{{ st.lastFinish }}</b>
                  </div>
                  <!-- <div class="ll-ptr-chip" v-if="st.selectedIds && st.selectedIds.length > 0">
                    Optimal Set: <b class="ll-c-purple">{ {{ st.selectedIds.map(id => 'A' + id).join(', ') }} }</b>
                  </div> -->
                </template>
                <template v-else>
                  <div class="ll-ptr-chip">Recursive Calls: <b class="ll-c-orange">{{ st.callCounter || 0 }}</b></div>
                  <div class="ll-ptr-chip" :class="{ 'll-chip-warn': (st.maxSelected || 0) > 0 }">
                    Max Compatible: <b class="ll-c-green">{{ st.maxSelected || 0 }}</b>
                  </div>
                  <div class="ll-ptr-chip" v-if="st.bestSubset && st.bestSubset.length > 0">
                    Best Subset: <b class="ll-c-purple">{ {{ st.bestSubset.map(id => 'A' + id).join(', ') }} }</b>
                  </div>
                </template>
              </div>

              <!-- Visualization Container -->
              <div class="ll-viz-wrap" :style="{ height: vizHeight + 'px' }">
                <!-- VIEW 1: GREEDY ARRAY VISUALIZATION (3 arrays: start[], finish[], idx[]) -->
                <div v-if="currentApproach === 'greedy'" class="ll-arr-viz-container">

                  <div class="ll-arr-viz-scroll">




                    <!-- ===== IDX[] ARRAY ===== -->
                    <!-- fill phase: show cells one at a time; sorted phase: show sorted order -->
                    <div class="ll-viz-arr-section ll-viz-idx-section">
                      <div class="ll-viz-arr-label ll-viz-arr-label-idx">
                        idx[]<span class="ll-viz-arr-sub"> (index array &mdash; sorted by finish[])</span>
                        <span class="ll-idx-sort-badge" v-if="st.phase === 'sorted' || st.phase === 'select' || st.phase === 'loop' || st.phase === 'check' || st.phase === 'done'">SORTED</span>
                        <span class="ll-idx-unsort-badge" v-else-if="st.phase === 'sort'">FILLING&hellip; ({{ st.idxFilled || 0 }}/{{ st.n || currentActivities.length }})</span>
                        <span class="ll-idx-unsort-badge ll-idx-init-badge" v-else>NOT CREATED YET</span>
                      </div>
                      <div class="ll-viz-arr-track">
                        <!-- POST-SORT: show sorted idx[] values -->
                        <template v-if="st.phase === 'sorted' || st.phase === 'select' || st.phase === 'loop' || st.phase === 'check' || st.phase === 'done'">
                          <div
                            v-for="(origId, rank) in (st.sortedIndices || [])"
                            :key="'vidx' + rank"
                            class="ll-viz-cell ll-viz-cell-idx"
                            :class="{
                              'll-viz-cell-idx-active': st.currentI === rank,
                              'll-viz-cell-idx-sel': st.selectedIds && st.selectedIds.includes(origId),
                              'll-viz-cell-idx-skip': st.currentI > rank && st.selectedIds && !st.selectedIds.includes(origId) && rank > 0,
                            }"
                          >
                            <span class="ll-viz-cell-index">[{{ rank }}]</span>
                            <span class="ll-viz-cell-val">{{ origId }}</span>
                            <span class="ll-viz-cell-ann-idx">f={{ (st.finishes || currentFinishes)[origId] }}</span>
                            <span class="ll-viz-cell-sel-mark" v-if="st.selectedIds && st.selectedIds.includes(origId)">&#10003;</span>
                          </div>
                        </template>

                        <!-- FILL PHASE: show cells appearing one by one as loop runs -->
                        <template v-else-if="st.phase === 'sort'">
                          <div
                            v-for="i in (st.idxFilled || 0)"
                            :key="'vidx-fill' + (i - 1)"
                            class="ll-viz-cell ll-viz-cell-idx"
                            :class="{
                              'll-viz-cell-idx-active': (i - 1) === st.sortLoopI && st.idxFilled === i,
                            }"
                          >
                            <span class="ll-viz-cell-index">[{{ i - 1 }}]</span>
                            <span class="ll-viz-cell-val">{{ i - 1 }}</span>
                            <span class="ll-viz-cell-ann-idx">f={{ (st.finishes || currentFinishes)[i - 1] }}</span>
                          </div>
                          <!-- Placeholder for the cell currently being written (loop header step) -->
                          <div
                            v-if="st.sortLoopI !== undefined && st.sortLoopI >= 0 && st.sortLoopI >= (st.idxFilled || 0)"
                            class="ll-viz-cell ll-viz-cell-idx ll-viz-cell-idx-pending"
                          >
                            <span class="ll-viz-cell-index">[{{ st.sortLoopI }}]</span>
                            <span class="ll-viz-cell-val">?</span>
                          </div>
                        </template>

                        <!-- BEFORE LOOP: not yet initialized -->
                        <template v-else>
                          <div class="ll-viz-cell-empty-hint">idx[] not yet created</div>
                        </template>
                      </div>
                    </div>


                    <!-- Selected activities summary strip -->
                    <div class="ll-arr-selected-strip" v-if="st.selectedIds && st.selectedIds.length">
                      <span class="ll-sel-strip-label">Selected via idx[]:</span>
                      <div
                        v-for="aid in st.selectedIds"
                        :key="'sel' + aid"
                        class="ll-sel-strip-chip"
                      >
                        A{{ aid }} [s={{ (st.starts || currentStarts)[aid] }}, f={{ (st.finishes || currentFinishes)[aid] }}]
                      </div>
                    </div>

                    <!-- lastFinish boundary indicator -->
                    <div class="ll-lastfinish-bar" v-if="(st.phase === 'select' || st.phase === 'loop' || st.phase === 'check') && st.lastFinish !== undefined">
                      <span class="ll-lf-label">&#9654; lastFinish boundary:</span>
                      <span class="ll-lf-val">{{ st.lastFinish }}</span>
                      <span class="ll-lf-hint">Next activity must have start &ge; {{ st.lastFinish }}</span>
                    </div>
                  </div>
                </div>

                <!-- VIEW 2: BRUTE FORCE DECISION TREE -->
                <div v-else class="ll-tree-container">
                  <!-- <div class="ll-section-caption">
                    <span>Brute Force Decision Tree &mdash; Exploring all 2<sup>n</sup> Subsets (Skip vs Pick)</span>
                    <span v-if="st.currentIdx >= 0 && st.currentIdx < st.n" class="ll-calc-pill">
                      Decision on A{{ st.currentIdx }} [start={{ (st.starts || currentStarts)[st.currentIdx] }}, finish={{ (st.finishes || currentFinishes)[st.currentIdx] }}]
                    </span>
                  </div> -->

                  <div class="ll-tree-scroll-area">
                    <svg
                      :viewBox="`0 0 ${st.treeWidth || 480} ${st.treeHeight || 240}`"
                      :style="{
                        width: '100%',
                        maxWidth: (st.treeWidth || 480) + 'px',
                        height: (st.treeHeight || 240) + 'px',
                        maxHeight: (st.treeHeight || 240) + 'px'
                      }"
                      class="ll-tree-svg"
                    >
                      <!-- Dynamic Edges -->
                      <g class="ll-tree-edges">
                        <line
                          v-for="(edge, idx) in st.treeEdges"
                          :key="idx"
                          :x1="edge.x1"
                          :y1="edge.y1"
                          :x2="edge.x2"
                          :y2="edge.y2"
                          class="ll-tree-edge"
                          :class="{ 'll-edge-pick': edge.isPick, 'll-edge-skip': !edge.isPick }"
                        />
                      </g>

                      <!-- Dynamic Nodes -->
                      <g class="ll-tree-nodes">
                        <g
                          v-for="node in st.treeNodes"
                          :key="node.id"
                          :transform="`translate(${node.x}, ${node.y})`"
                          class="ll-tree-node-group"
                        >
                          <rect
                            x="-19"
                            y="-11"
                            width="38"
                            height="22"
                            rx="4"
                            class="ll-node-rect"
                            :class="{
                              'll-node-active': node.id === st.activeNodeId,
                              'll-node-solved': node.state === 'solved',
                              'll-node-leaf': node.state === 'leaf'
                            }"
                          />
                          <text x="0" y="-2.5" text-anchor="middle" class="ll-node-text-call">
                            idx={{ node.idx }}
                          </text>
                          <text
                            x="0"
                            y="6.5"
                            text-anchor="middle"
                            class="ll-node-text-val"
                            :class="{
                              'll-val-active': node.id === st.activeNodeId,
                              'll-val-solved': node.state === 'solved'
                            }"
                          >
                            {{ node.retVal !== null ? `cnt=${node.retVal}` : (node.state === 'leaf' ? `cnt=${node.count}` : `cnt=${node.count}`) }}
                          </text>
                        </g>
                      </g>
                    </svg>

                    <!-- Active branch summary strip -->
                    <div class="ll-act-table-wrap" v-if="st.currentPicked">
                      <div class="ll-act-table-title">Currently Chosen in this Branch:</div>
                      <div class="ll-act-row-wrap">
                        <span v-if="!st.currentPicked.length" class="ll-empty-set">&#8709; (Empty subset)</span>
                        <div
                          v-for="pid in st.currentPicked"
                          :key="pid"
                          class="ll-act-chip ll-act-picked"
                        >
                          A{{ pid }} [start={{ (st.starts || currentStarts)[pid] }}, finish={{ (st.finishes || currentFinishes)[pid] }}]
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
              </div>

              <!-- Vertical Resizer between Viz and Variable Table -->
              <div class="ll-vresizer" ref="vizResizerRef"></div>

              <!-- Legend Strip -->
              <div class="ll-legend">
                <template v-if="currentApproach === 'greedy'">
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-purple"></span>idx[] — index array (sorted by finish[])</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-new"></span>Selected cell</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-cur"></span>Evaluating (active idx)</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-red"></span>Conflict / Skipped</span>
                </template>
                <template v-else>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-cur"></span>Active Decision</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-new"></span>Backtracked / Solved</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-purple"></span>Leaf Evaluation</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-green"></span>Solid Edge: Pick</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-dash"></span>Dashed Edge: Skip</span>
                </template>
              </div>

              <!-- Call Stack & Variables Area -->
              <div class="ll-table-area" :style="{ height: tableHeight + 'px' }">
                <div class="ll-table-title">Variable Frames &mdash; Innermost = Current Function Scope</div>
                <div class="ll-stack-line">
                  <template v-if="s.vars && s.vars.length">
                    <div
                      v-for="(f, depth) in s.vars"
                      :key="depth"
                      class="ll-frame"
                      :class="{ 'll-frame-cur': depth === s.vars.length - 1 }"
                      :style="{ marginLeft: depth * 12 + 'px' }"
                    >
                      {{ f.title }}(<span v-for="(r, i) in f.rows" :key="i"><span v-if="i > 0">, </span><span class="ll-fname">{{ r[0] }}</span>=<span :class="r[0] === 'return' || r[0] === 'ans' ? 'll-c-green' : (depth === s.vars.length - 1 ? 'll-c-orange' : 'll-c-blue')" style="font-weight:700">{{ r[1] }}</span></span>)<span v-if="depth === s.vars.length - 1" class="ll-now"> &#9668; current</span>
                    </div>
                  </template>
                  <template v-else>&mdash;</template>
                </div>
              </div>

              <!-- Vertical Resizer between Variable Table and Status Badge -->
              <div class="ll-vresizer" ref="tableResizerRef"></div>

              <!-- Status Badge Area -->
              <div class="ll-badge-wrap">
                <div
                  class="ll-badge"
                  :class="{
                    'll-badge-error': s.badge && s.badge.includes('Conflict'),
                    'll-badge-success': s.badge && (s.badge.includes('Complete') || s.badge.includes('SELECTED') || s.badge.includes('Optimal'))
                  }"
                >
                  {{ s.badge || 'Ready to visualize Activity Selection.' }}
                </div>
              </div>
            </div>

            <!-- Horizontal Resizer between Left and Right Columns -->
            <div class="ll-resizer" ref="hResizerRef"></div>

            <!-- Right Column: Code & Faculty Lecture Panel -->
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
                  </select>
                </div>

                <!-- Code Tab -->
                <div v-if="rightTab === 'code'" class="ll-code-scroll">
                  <pre class="ll-pre"><span v-for="(line, i) in codeLines" :key="i" class="ll-codeline" :class="{ 'll-hl': line[0] && line[0] === s.code }">{{ line[1] === '' ? ' ' : line[1] }}</span></pre>
                </div>

                <!-- Pseudocode Tab -->
                <div v-else-if="rightTab === 'pseudo'" class="ll-code-scroll">
                  <pre class="ll-pre"><span v-for="(line, i) in pseudocodeLines" :key="i" class="ll-codeline">{{ line }}</span></pre>
                </div>

                <!-- Time and Space Complexity Tab -->
                <div v-else class="ll-info-scroll">

                  <!-- ════════ BRUTE FORCE COMPLEXITY ════════ -->
                  <template v-if="currentApproach === 'brute'">
                    <h3 class="ll-cx-heading">Brute Force &mdash; Complexity Analysis</h3>
                    <p class="ll-cx-intro">
                      Exhaustively explores every possible subset of activities using recursion.
                      At each step, two choices are made: <strong>skip</strong> or <strong>pick</strong> the current activity.
                    </p>

                    <h4 class="ll-cx-sub">Step-by-step Breakdown</h4>
                    <table class="ll-complexity-table">
                      <thead>
                        <tr><th>Operation</th><th>Time</th><th>Space</th><th>Reason</th></tr>
                      </thead>
                      <tbody>
                        <tr>
                          <td>Read input (start[], finish[])</td>
                          <td class="ll-cx-good">O(n)</td>
                          <td class="ll-cx-good">O(n)</td>
                          <td>One pass to fill two arrays</td>
                        </tr>
                        <tr>
                          <td>Recursive branching (skip / pick)</td>
                          <td class="ll-cx-bad">O(2<sup>n</sup>)</td>
                          <td class="ll-cx-mid">O(n)</td>
                          <td>Binary tree of depth n &rarr; 2<sup>n</sup> leaves; stack depth = n</td>
                        </tr>
                        <tr>
                          <td>Base case check &amp; update max</td>
                          <td class="ll-cx-good">O(1)</td>
                          <td class="ll-cx-good">O(1)</td>
                          <td>Simple comparison at each leaf</td>
                        </tr>
                      </tbody>
                    </table>

                    <h4 class="ll-cx-sub">Overall Complexity</h4>
                    <div class="ll-cx-summary-grid">
                      <div class="ll-cx-card ll-cx-card-bad">
                        <div class="ll-cx-card-label">Time</div>
                        <div class="ll-cx-card-val">O(2<sup>n</sup>)</div>
                        <div class="ll-cx-card-note">Exponential &mdash; infeasible for large n</div>
                      </div>
                      <div class="ll-cx-card ll-cx-card-mid">
                        <div class="ll-cx-card-label">Space</div>
                        <div class="ll-cx-card-val">O(n)</div>
                        <div class="ll-cx-card-note">Recursion call stack depth</div>
                      </div>
                      <div class="ll-cx-card ll-cx-card-good">
                        <div class="ll-cx-card-label">Optimal?</div>
                        <div class="ll-cx-card-val">YES</div>
                        <div class="ll-cx-card-note">Exhaustive &rarr; guaranteed correct</div>
                      </div>
                    </div>

                    <div class="ll-note">
                      <strong>When is Brute Force used?</strong> Only for very small inputs (n &le; 20).
                      For real scheduling problems with hundreds of activities, Greedy is the only practical choice.
                    </div>
                  </template>

                  <!-- ════════ GREEDY COMPLEXITY ════════ -->
                  <template v-else>
                    <h3 class="ll-cx-heading">Greedy Algorithm &mdash; Complexity Analysis</h3>
                    <p class="ll-cx-intro">
                      Sorts activities by <strong>earliest finish time</strong> using an auxiliary index array
                      <code>idx[]</code>, then makes one greedy pass to count mutually compatible activities.
                    </p>

                    <h4 class="ll-cx-sub">Step-by-step Breakdown</h4>
                    <table class="ll-complexity-table">
                      <thead>
                        <tr><th>Operation</th><th>Time</th><th>Space</th><th>Reason</th></tr>
                      </thead>
                      <tbody>
                        <tr>
                          <td>Read input (start[], finish[])</td>
                          <td class="ll-cx-good">O(n)</td>
                          <td class="ll-cx-good">O(n)</td>
                          <td>Single pass, two arrays</td>
                        </tr>
                        <tr>
                          <td>Create &amp; fill idx[] array</td>
                          <td class="ll-cx-good">O(n)</td>
                          <td class="ll-cx-good">O(n)</td>
                          <td>idx[i] = i for i in 0..n-1</td>
                        </tr>
                        <tr>
                          <td>Sort idx[] by finish[idx[i]]</td>
                          <td class="ll-cx-mid">O(n log n)</td>
                          <td class="ll-cx-good">O(log n)</td>
                          <td>Comparison-based sort (Arrays.sort / qsort); O(log n) stack for quicksort</td>
                        </tr>
                        <tr>
                          <td>Greedy scan (count compatible)</td>
                          <td class="ll-cx-good">O(n)</td>
                          <td class="ll-cx-good">O(1)</td>
                          <td>Single left-to-right pass over sorted idx[]</td>
                        </tr>
                      </tbody>
                    </table>

                    <h4 class="ll-cx-sub">Overall Complexity</h4>
                    <div class="ll-cx-summary-grid">
                      <div class="ll-cx-card ll-cx-card-good">
                        <div class="ll-cx-card-label">Time</div>
                        <div class="ll-cx-card-val">O(n log n)</div>
                        <div class="ll-cx-card-note">Dominated by sorting step</div>
                      </div>
                      <div class="ll-cx-card ll-cx-card-good">
                        <div class="ll-cx-card-label">Space</div>
                        <div class="ll-cx-card-val">O(n)</div>
                        <div class="ll-cx-card-note">Auxiliary index array idx[]</div>
                      </div>
                      <div class="ll-cx-card ll-cx-card-good">
                        <div class="ll-cx-card-label">Optimal?</div>
                        <div class="ll-cx-card-val">YES</div>
                        <div class="ll-cx-card-note">Provable via Exchange Argument</div>
                      </div>
                    </div>

                    <h4 class="ll-cx-sub">Approach Comparison</h4>
                    <table class="ll-complexity-table">
                      <thead>
                        <tr><th>Approach</th><th>Time</th><th>Space</th><th>Practical?</th></tr>
                      </thead>
                      <tbody>
                        <tr>
                          <td><strong>Brute Force</strong></td>
                          <td class="ll-cx-bad">O(2<sup>n</sup>)</td>
                          <td class="ll-cx-good">O(n)</td>
                          <td class="ll-cx-bad">No &mdash; exponential blowup</td>
                        </tr>
                        <tr>
                          <td><strong>Greedy</strong></td>
                          <td class="ll-cx-good">O(n log n)</td>
                          <td class="ll-cx-good">O(n)</td>
                          <td class="ll-cx-good">Yes &mdash; scales to millions</td>
                        </tr>
                      </tbody>
                    </table>

                    <div class="ll-note">
                      <strong>Key Insight:</strong> Sorting the auxiliary <code>idx[]</code> array instead of
                      reordering <code>start[]</code> / <code>finish[]</code> preserves the original input
                      while achieving O(n log n) time with O(n) extra space.
                    </div>
                  </template>

                </div>
              </div>
            </div>
          </div>

          <!-- Bottom Footer Toolbar -->
          <div class="ll-footer">
            Step {{ si + 1 }} / {{ steps.length }}
            <span class="ll-speed-wrap">
              Speed
              <input type="range" min="100" max="2000" step="100" v-model.number="speed" />
            </span>
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

@keyframes ll-pulse {
  0%, 100% { transform: scale(1); }
  50% { transform: scale(1.06); }
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
  overflow-y: auto;
}

/* Control Toolbar */
.ll-toolbar {
  margin-top: 4px;
  display: flex;
  align-items: center;
  gap: 6px;
  padding: 5px 12px;
  background: var(--surface);
  border-bottom: 1px solid var(--border);
  flex-shrink: 0;
  flex-wrap: wrap;
  box-shadow: var(--shadow-sm);
}

.ll-approach-group {
  display: flex;
  gap: 2px;
  background: var(--surface2);
  padding: 2px;
  border-radius: var(--radius-sm);
  border: 1px solid var(--border);
}

.ll-approach-btn {
  background: transparent;
  border: none;
  padding: 4px 9px;
  font-size: 11px;
  font-weight: 600;
  color: var(--text2);
  border-radius: 4px;
  cursor: pointer;
  transition: all .15s;
  white-space: nowrap;
}

.ll-approach-btn:hover {
  color: var(--coral);
}

.ll-approach-btn.active {
  background: var(--coral);
  color: #fff;
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

/* Top Banner Dual Arrays Display */
.ll-house-banner {
  background: var(--surface);
  border-bottom: 1px solid var(--border);
  padding: 5px 10px 7px;
  flex-shrink: 0;
}

.ll-house-title {
  font-size: 10.5px;
  font-weight: 700;
  color: var(--text2);
  margin-bottom: 4px;
}

.ll-dual-arrays-wrap {
  display: flex;
  flex-direction: column;
  gap: 3px;
  overflow-x: auto;
}

.ll-arr-row {
  display: flex;
  align-items: center;
  gap: 6px;
}

.ll-arr-tag {
  width: 50px;
  font-size: 10.5px;
  font-family: monospace;
  font-weight: 700;
  color: var(--text2);
  flex-shrink: 0;
}

.ll-arr-cells {
  display: flex;
  gap: 7px;
}

.ll-arr-cell {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  background: var(--surface2);
  border: 1px solid var(--border2);
  border-radius: 5px;
  width: 40px;
  padding: 2px 3px;
  transition: all .2s ease;
}

.ll-cell-idx {
  font-size: 8.5px;
  color: var(--muted);
  font-family: monospace;
}

.ll-cell-val {
  font-size: 11px;
  font-weight: 700;
  color: var(--text);
  font-family: monospace;
}

.ll-cell-active {
  border-color: #f59e0b !important;
  background: #fffbeb !important;
  box-shadow: 0 0 5px rgba(245, 158, 11, .45);
}

.ll-cell-selected {
  border-color: #10b981 !important;
  background: #dcfce7 !important;
}

/* Real-time Status Chips */
.ll-ptrs {
  display: flex;
  gap: 6px;
  flex-wrap: wrap;
  padding: 4px 10px;
  min-height: 28px;
  background: var(--surface2);
  border-bottom: 1px solid var(--border);
  box-sizing: border-box;
}

.ll-ptr-chip {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: var(--radius-sm);
  padding: 2px 7px;
  font-size: 11px;
  font-family: monospace;
  box-shadow: var(--shadow-sm);
  white-space: nowrap;
}

.ll-chip-warn {
  background: #fef2f2 !important;
  border-color: #fca5a5 !important;
}

.ll-c-blue { color: var(--blue); }
.ll-c-orange { color: var(--orange); }
.ll-c-green { color: var(--green); }
.ll-c-purple { color: var(--purple); }

/* Visualization Workspace */
.ll-viz-wrap {
  flex-shrink: 0;
  background: var(--surface);
  position: relative;
  overflow-x: auto;
  overflow-y: auto;
}

.ll-section-caption {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 3px 10px;
  font-size: 10.5px;
  font-weight: 700;
  color: var(--text2);
  background: var(--surface2);
  border-bottom: 1px solid var(--border);
}

.ll-calc-pill {
  background: #dcfce7;
  color: #15803d;
  padding: 1px 6px;
  border-radius: 10px;
  font-size: 10.5px;
  font-family: monospace;
  font-weight: 700;
}

.ll-memo-badge-info {
  background: var(--purple-light);
  color: var(--purple);
  padding: 1px 6px;
  border-radius: 10px;
  font-size: 10px;
}

/* ================================================================ */
/* GREEDY ARRAY VISUALIZATION (replaces Gantt Timeline)            */
/* ================================================================ */
.ll-arr-viz-container {
  display: flex;
  flex-direction: column;
  width: 100%;
  height: 100%;
}

.ll-arr-viz-scroll {
  flex: 1;
  overflow: auto;
  padding: 8px 12px;
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.ll-arr-viz-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
  gap: 6px;
  padding: 4px 6px;
  background: var(--surface2);
  border: 1px solid var(--border);
  border-radius: var(--radius-sm);
  font-size: 10px;
  color: var(--text2);
}

.ll-arr-viz-note {
  display: flex;
  align-items: center;
  gap: 4px;
}

.ll-note-dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  display: inline-block;
  flex-shrink: 0;
}

.ll-note-blue  { background: #3b82f6; }
.ll-note-green { background: #10b981; }
.ll-note-amber { background: #f59e0b; }
.ll-note-red   { background: #ef4444; }

/* Each array row section */
.ll-viz-arr-section {
  display: flex;
  flex-direction: column;
  gap: 4px;
}

.ll-viz-idx-section {
  background: #f8f5ff;
  border: 1px solid #d8b4fe;
  border-radius: var(--radius-sm);
  padding: 6px 8px;
}

.ll-viz-arr-label {
  font-size: 10.5px;
  font-weight: 700;
  font-family: monospace;
  color: var(--text2);
  display: flex;
  align-items: center;
  gap: 5px;
  flex-wrap: wrap;
}

.ll-viz-arr-label-idx {
  color: var(--purple);
}

.ll-viz-arr-sub {
  font-weight: 400;
  font-size: 9.5px;
  color: var(--muted);
  font-family: sans-serif;
}

.ll-idx-sort-badge {
  background: #9333ea;
  color: #fff;
  font-size: 8.5px;
  font-weight: 700;
  padding: 1px 5px;
  border-radius: 3px;
  font-family: sans-serif;
  letter-spacing: 0.03em;
}

.ll-idx-unsort-badge {
  background: #f59e0b;
  color: #fff;
  font-size: 8.5px;
  font-weight: 700;
  padding: 1px 5px;
  border-radius: 3px;
  font-family: sans-serif;
}

.ll-idx-init-badge {
  background: #94a3b8 !important;
}

.ll-viz-arr-track {
  display: flex;
  gap: 6px;
  flex-wrap: wrap;
  align-items: flex-start;
}

/* Base cell style */
.ll-viz-cell {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  background: var(--surface);
  border: 1.5px solid var(--border2);
  border-radius: 6px;
  width: 44px;
  min-height: 48px;
  padding: 3px 2px;
  transition: all .22s ease;
  position: relative;
}

.ll-viz-cell-index {
  font-size: 8px;
  color: var(--muted);
  font-family: monospace;
}

.ll-viz-cell-val {
  font-size: 12px;
  font-weight: 700;
  color: var(--text);
  font-family: monospace;
  line-height: 1.2;
}

.ll-viz-cell-ann {
  font-size: 9px;
  color: #10b981;
  font-weight: 900;
}

/* idx[] cell extra annotation (finish value) */
.ll-viz-cell-ann-idx {
  font-size: 8px;
  color: var(--purple);
  font-family: monospace;
  margin-top: 1px;
}

.ll-viz-cell-sel-mark {
  position: absolute;
  top: 2px;
  right: 3px;
  font-size: 8px;
  color: #10b981;
  font-weight: 900;
}

/* Cell states */
.ll-viz-cell-active {
  border-color: #f59e0b !important;
  background: #fffbeb !important;
  box-shadow: 0 0 6px rgba(245,158,11,.45);
  animation: ll-pulse 1.2s infinite ease-in-out;
}

.ll-viz-cell-sel {
  border-color: #10b981 !important;
  background: #dcfce7 !important;
}

/* idx[] specific cell states */
.ll-viz-cell-idx {
  background: #f5f0ff;
  border-color: #c4b5fd;
  width: 48px;
  min-height: 54px;
}

.ll-viz-cell-idx-init {
  opacity: 0.7;
}

.ll-viz-cell-idx-active {
  border-color: #f59e0b !important;
  background: #fffbeb !important;
  box-shadow: 0 0 7px rgba(245,158,11,.5);
  animation: ll-pulse 1.2s infinite ease-in-out;
}

.ll-viz-cell-idx-sel {
  border-color: #10b981 !important;
  background: #dcfce7 !important;
}

.ll-viz-cell-idx-skip {
  opacity: 0.45;
  border-color: #fca5a5 !important;
  background: #fef2f2 !important;
}

.ll-viz-cell-idx-pending {
  border-color: #f59e0b !important;
  background: #fffbeb !important;
  border-style: dashed !important;
  opacity: 0.7;
}

.ll-viz-cell-empty-hint {
  font-size: 10px;
  color: var(--muted);
  font-style: italic;
  padding: 6px 8px;
  border: 1.5px dashed var(--border2);
  border-radius: 6px;
  background: var(--surface2);
}

/* Selected activities summary strip */
.ll-arr-selected-strip {
  display: flex;
  align-items: center;
  gap: 5px;
  flex-wrap: wrap;
  background: #f0fdf4;
  border: 1px dashed #10b981;
  border-radius: var(--radius-sm);
  padding: 5px 8px;
}

.ll-sel-strip-label {
  font-size: 10px;
  font-weight: 700;
  color: #15803d;
  white-space: nowrap;
}

.ll-sel-strip-chip {
  background: #10b981;
  color: #fff;
  font-size: 9.5px;
  font-weight: 700;
  font-family: monospace;
  padding: 2px 6px;
  border-radius: 3px;
}

/* lastFinish boundary bar */
.ll-lastfinish-bar {
  display: flex;
  align-items: center;
  gap: 6px;
  background: #fff7ed;
  border: 1px solid #fed7aa;
  border-radius: var(--radius-sm);
  padding: 4px 10px;
  font-size: 10.5px;
  flex-wrap: wrap;
}

.ll-lf-label {
  color: #9a3412;
  font-weight: 700;
}

.ll-lf-val {
  background: #f97316;
  color: #fff;
  font-weight: 700;
  font-family: monospace;
  font-size: 11px;
  padding: 1px 6px;
  border-radius: 4px;
}

.ll-lf-hint {
  color: #c2410c;
  font-size: 9.5px;
  font-style: italic;
}

.ll-memo-badge-warn {
  background: #fef3c7;
  color: #b45309;
  padding: 1px 6px;
  border-radius: 10px;
  font-size: 10px;
  font-weight: 600;
}

/* Brute Force Decision Tree */
.ll-tree-container {
  display: flex;
  flex-direction: column;
  width: 100%;
  height: 100%;
}

.ll-tree-scroll-area {
  flex: 1;
  overflow: auto;
  padding: 4px 6px;
  display: flex;
  flex-direction: column;
  align-items: center;
  min-height: 0;
  width: 100%;
  box-sizing: border-box;
}

.ll-tree-svg {
  display: block;
  flex-shrink: 0;
  max-width: 100%;
  margin: 0 auto;
}

.ll-tree-edge {
  stroke: #cbd5e1;
  stroke-width: 1.5px;
}

.ll-edge-pick {
  stroke: #22c55e !important;
  stroke-width: 2px !important;
}

.ll-edge-skip {
  stroke: #94a3b8 !important;
  stroke-dasharray: 4,3 !important;
}

.ll-tree-node-group { cursor: default; }

.ll-node-rect {
  fill: #ffffff;
  stroke: #cbd5e1;
  stroke-width: 1.4px;
  filter: drop-shadow(0 1px 2px rgba(0,0,0,.05));
  transition: all .25s ease;
}

.ll-node-active {
  stroke: #f59e0b !important;
  stroke-width: 2.2px !important;
  fill: #fffbeb !important;
  filter: drop-shadow(0 0 5px rgba(245,158,11,.45)) !important;
  animation: ll-pulse 1.3s infinite ease-in-out;
}

.ll-node-solved {
  stroke: #10b981 !important;
  fill: #dcfce7 !important;
  stroke-width: 1.6px !important;
}

.ll-node-leaf {
  stroke: #9333ea !important;
  fill: #f3e8ff !important;
  stroke-width: 1.6px !important;
}

.ll-tree-svg text {
  font-family: 'Segoe UI', system-ui, sans-serif !important;
  user-select: none;
}

.ll-node-text-call {
  font-family: 'Consolas','Fira Code',monospace !important;
  font-size: 8.5px !important;
  font-weight: 700 !important;
  fill: #1e293b !important;
  dominant-baseline: central !important;
}

.ll-node-text-val {
  font-family: 'Consolas','Fira Code',monospace !important;
  font-size: 7.5px !important;
  font-weight: 700 !important;
  fill: #64748b !important;
  dominant-baseline: central !important;
}

.ll-val-active { fill: #b45309 !important; }
.ll-val-solved { fill: #047857 !important; }

.ll-act-table-wrap {
  width: 100%;
  padding: 4px 8px;
  background: var(--surface2);
  border-top: 1px dashed var(--border);
  margin-top: 4px;
}

.ll-act-table-title {
  font-size: 10px;
  font-weight: 700;
  color: var(--text2);
  margin-bottom: 3px;
}

.ll-act-row-wrap {
  display: flex;
  gap: 4px;
  flex-wrap: wrap;
}

.ll-act-chip {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: 4px;
  padding: 2px 6px;
  font-size: 9.5px;
  font-family: monospace;
}

.ll-act-picked {
  background: #dcfce7 !important;
  border-color: #10b981 !important;
  color: #15803d !important;
  font-weight: 700;
}

.ll-empty-set {
  font-size: 10px;
  color: var(--muted);
  font-style: italic;
}

/* Legend */
.ll-legend {
  display: flex;
  flex-wrap: wrap;
  gap: 4px 10px;
  padding: 4px 10px;
  border-bottom: 1px solid var(--border);
  flex-shrink: 0;
  background: var(--surface2);
}

.ll-leg {
  display: flex;
  align-items: center;
  gap: 4px;
  font-size: 10px;
  color: var(--text2);
  font-weight: 500;
}

.ll-legdot {
  width: 8px;
  height: 8px;
  border-radius: 2px;
  display: inline-block;
}

.ll-legdot-cur { background: #fef3c7; border: 1.5px solid #f59e0b; }
.ll-legdot-new { background: #dcfce7; border: 1.5px solid #10b981; }
.ll-legdot-red { background: #fee2e2; border: 1.5px solid #ef4444; }
.ll-legdot-purple { background: #f3e8ff; border: 1.5px solid #9333ea; }
.ll-legdot-green { background: #10b981; }
.ll-legdot-dash { background: #94a3b8; }
.ll-legdot-uncalc { background: var(--surface2); border: 1.5px dashed var(--border2); }

/* Variable Frames & Stack Area */
.ll-table-area {
  flex-shrink: 0;
  padding: 4px 10px;
  border-bottom: 1px solid var(--border);
  overflow-x: hidden;
  overflow-y: auto;
  background: var(--surface);
  min-width: 0;
  box-sizing: border-box;
}

.ll-table-title {
  font-size: 9.5px;
  color: var(--muted);
  margin-bottom: 2px;
  font-style: italic;
}

.ll-stack-line {
  font-family: 'Consolas', monospace;
  font-size: 11px;
  line-height: 1.6;
}

.ll-frame {
  font-family: 'Consolas', monospace;
  font-size: 10.5px;
  color: var(--text2);
  padding: 1px 0;
  white-space: nowrap;
}

.ll-frame-cur {
  font-weight: 700;
  color: var(--text);
}

.ll-fname { color: var(--muted); }
.ll-now { color: var(--orange); font-size: 9px; font-weight: 700; }

/* Status Badge */
.ll-badge-wrap {
  padding: 4px 10px;
  flex-shrink: 0;
  background: var(--surface2);
}

.ll-badge {
  background: #eff6ff;
  border: 1px solid #bfdbfe;
  color: #1d4ed8;
  font-size: 10.5px;
  font-weight: 600;
  padding: 3px 8px;
  border-radius: 4px;
}

.ll-badge-error {
  background: #fef2f2 !important;
  border-color: #fca5a5 !important;
  color: #b91c1c !important;
}

.ll-badge-success {
  background: #f0fdf4 !important;
  border-color: #86efac !important;
  color: #15803d !important;
}

/* Right Column Code Panel */
.ll-code-panel {
  display: flex;
  flex-direction: column;
  height: 100%;
  background: var(--surface);
  overflow: hidden;
}

.ll-code-header {
  display: flex;
  align-items: center;
  border-bottom: 1px solid var(--border);
  background: var(--surface2);
  padding: 0 8px;
  flex-shrink: 0;
}

.ll-tabbar {
  display: flex;
  gap: 2px;
}

.ll-tab-btn {
  background: transparent;
  border: none;
  padding: 5px 10px;
  font-size: 11px;
  font-weight: 600;
  color: var(--text2);
  cursor: pointer;
  border-bottom: 2px solid transparent;
  transition: all .15s;
}

.ll-tab-btn:hover {
  color: var(--coral);
}

.ll-tab-btn.active {
  color: var(--coral);
  border-bottom-color: var(--coral);
  background: var(--surface);
}

.ll-lang-select {
  background: var(--surface);
  border: 1px solid var(--border2);
  color: var(--text);
  padding: 2px 8px;
  border-radius: var(--radius-sm);
  font-size: 10.5px;
  font-weight: 600;
  margin-left: auto;
  cursor: pointer;
}

.ll-code-scroll {
  flex: 1;
  overflow: auto;
  padding: 6px 10px;
  background: #ffffff;
  height: 100%;
  padding-bottom: 30%;
}

.ll-pre {
  margin: 0;
  font-family: 'Consolas', 'Fira Code', monospace;
  font-size: 10.5px;
  line-height: 1.55;
  color: #334155;
  padding-bottom: 50px;
}

.ll-codeline {
  display: block;
  padding: 0 6px;
  border-radius: 2px;
  white-space: pre-wrap;
  word-break: break-all;
}

.ll-hl {
  background: #dcfce7 !important;
  color: #15803d !important;
  font-weight: 700;
  border-left: 3px solid var(--green);
  padding-left: 3px;
}

/* Faculty Lecture & Proof Tab */
.ll-info-scroll {
  flex: 1;
  overflow: auto;
  padding: 10px 14px;
  color: var(--text);
  line-height: 1.6;
}

.ll-info-scroll h3 {
  margin: 0 0 6px;
  font-size: 13px;
  color: var(--coral-dark);
}

.ll-info-scroll h4 {
  margin: 10px 0 4px;
  font-size: 11.5px;
  color: var(--text);
}

.ll-info-scroll p, .ll-info-scroll ul {
  font-size: 10.5px;
  margin: 0 0 8px;
  color: var(--text2);
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

/* Resizers */
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
