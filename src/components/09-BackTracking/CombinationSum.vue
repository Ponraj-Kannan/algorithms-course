<script setup>
import { ref, reactive, computed, onMounted, onBeforeUnmount, watch, nextTick } from 'vue';

defineProps({
  topic: { type: String, default: 'Backtracking Algorithms' },
  subTopic: { type: String, default: 'Combination Sum' }
});

const CODES = {
  java: [
    ['',                   'import java.util.Scanner;'],
    ['',                   'import java.util.List;'],
    ['',                   'import java.util.ArrayList;'],
    ['',                   'import java.util.Arrays;'],
    ['',                   ''],
    ['',                   'class Main {'],
    ['c_bt_entry',         '    static void backtrack(int[] candidates, int target, int start, List<Integer> current, List<List<Integer>> result) {'],
    ['c_base_check',       '        if (target == 0) {'],
    ['c_save_sol',         '            result.add(new ArrayList<>(current));'],
    ['c_base_ret',         '            return;'],
    ['',                   '        }'],
    ['c_loop_cand',        '        for (int i = start; i < candidates.length; i++) {'],
    ['c_check_fit',        '            if (candidates[i] <= target) {'],
    ['c_add_num',          '                current.add(candidates[i]);'],
    ['c_recurse',          '                backtrack(candidates, target - candidates[i], i, current, result);'],
    ['c_backtrack',        '                current.remove(current.size() - 1);'],
    ['',                   '            }'],
    ['',                   '        }'],
    ['',                   '    }'],
    ['',                   ''],
    ['c_entry',            '    static List<List<Integer>> combinationSum(int[] candidates, int target) {'],
    ['c_init_result',      '        List<List<Integer>> result = new ArrayList<>();'],
    ['c_init_current',     '        List<Integer> current = new ArrayList<>();'],
    ['c_sort_cand',        '        Arrays.sort(candidates);'],
    ['c_call_bt',          '        backtrack(candidates, target, 0, current, result);'],
    ['c_return_res',       '        return result;'],
    ['',                   '    }'],
    ['',                   ''],
    ['',                   '    public static void main(String[] args) {'],
    ['m_scanner',          '        Scanner sc = new Scanner(System.in);'],
    ['m_read_n',           '        int n = sc.nextInt();'],
    ['m_read_target',      '        int target = sc.nextInt();'],
    ['m_alloc_arr',        '        int[] candidates = new int[n];'],
    ['m_loop_read',        '        for (int i = 0; i < n; i++) {'],
    ['m_read_elem',        '            candidates[i] = sc.nextInt();'],
    ['',                   '        }'],
    ['m_call_solver',      '        List<List<Integer>> result = combinationSum(candidates, target);'],
    ['m_print_count',      '        System.out.println(result.size());'],
    ['m_loop_print',       '        for (int i = 0; i < result.size(); i++) {'],
    ['m_print_elem',       '            System.out.println(result.get(i));'],
    ['',                   '        }'],
    ['m_done',             '    }'],
    ['',                   '}']
  ],
  c: [
    ['',                   '#include <stdio.h>'],
    ['',                   '#include <stdlib.h>'],
    ['',                   ''],
    ['',                   'int compareInts(const void* a, const void* b) {'],
    ['',                   '    return (*(const int*)a - *(const int*)b);'],
    ['',                   '}'],
    ['',                   ''],
    ['c_bt_entry',         'void backtrack(int candidates[], int n, int target, int start, int current[], int currentSize, int result[150][40], int resultSizes[], int* resultCount) {'],
    ['c_base_check',       '    if (target == 0) {'],
    ['c_save_sol',         '        for (int i = 0; i < currentSize; i++) {'],
    ['',                   '            result[*resultCount][i] = current[i];'],
    ['',                   '        }'],
    ['',                   '        resultSizes[*resultCount] = currentSize;'],
    ['',                   '        (*resultCount)++;'],
    ['c_base_ret',         '        return;'],
    ['',                   '    }'],
    ['c_loop_cand',        '    for (int i = start; i < n; i++) {'],
    ['c_check_fit',        '        if (candidates[i] <= target) {'],
    ['c_add_num',          '            current[currentSize] = candidates[i];'],
    ['',                   '            currentSize++;'],
    ['c_recurse',          '            backtrack(candidates, n, target - candidates[i], i, current, currentSize, result, resultSizes, resultCount);'],
    ['c_backtrack',        '            currentSize--;'],
    ['',                   '        }'],
    ['',                   '    }'],
    ['',                   '}'],
    ['',                   ''],
    ['c_entry',            'int combinationSum(int candidates[], int n, int target, int result[150][40], int resultSizes[]) {'],
    ['c_init_result',      '    int resultCount = 0;'],
    ['c_init_current',     '    int current[40];'],
    ['c_sort_cand',        '    qsort(candidates, n, sizeof(int), compareInts);'],
    ['c_call_bt',          '    backtrack(candidates, n, target, 0, current, 0, result, resultSizes, &resultCount);'],
    ['c_return_res',       '    return resultCount;'],
    ['',                   '}'],
    ['',                   ''],
    ['',                   'int main() {'],
    ['m_scanner',          '    int n;'],
    ['',                   '    int target;'],
    ['m_read_n',           '    scanf("%d", &n);'],
    ['m_read_target',      '    scanf("%d", &target);'],
    ['m_alloc_arr',        '    int candidates[40];'],
    ['m_loop_read',        '    for (int i = 0; i < n; i++) {'],
    ['m_read_elem',        '        scanf("%d", &candidates[i]);'],
    ['',                   '    }'],
    ['m_call_solver',      '    int result[150][40];'],
    ['',                   '    int resultSizes[150];'],
    ['',                   '    int count = combinationSum(candidates, n, target, result, resultSizes);'],
    ['m_print_count',      '    printf("%d\\n", count);'],
    ['m_loop_print',       '    for (int i = 0; i < count; i++) {'],
    ['m_print_elem',       '        printf("[");'],
    ['',                   '        for (int j = 0; j < resultSizes[i]; j++) {'],
    ['',                   '            printf("%d%s", result[i][j], (j + 1 < resultSizes[i]) ? ", " : "");'],
    ['',                   '        }'],
    ['',                   '        printf("]\\n");'],
    ['',                   '    }'],
    ['m_done',             '    return 0;'],
    ['',                   '}']
  ],
  cpp: [
    ['',                   '#include <iostream>'],
    ['',                   '#include <vector>'],
    ['',                   '#include <algorithm>'],
    ['',                   'using namespace std;'],
    ['',                   ''],
    ['c_bt_entry',         'void backtrack(const vector<int>& candidates, int target, int start, vector<int>& current, vector<vector<int>>& result) {'],
    ['c_base_check',       '    if (target == 0) {'],
    ['c_save_sol',         '        result.push_back(current);'],
    ['c_base_ret',         '        return;'],
    ['',                   '    }'],
    ['c_loop_cand',        '    for (int i = start; i < (int)candidates.size(); i++) {'],
    ['c_check_fit',        '        if (candidates[i] <= target) {'],
    ['c_add_num',          '            current.push_back(candidates[i]);'],
    ['c_recurse',          '            backtrack(candidates, target - candidates[i], i, current, result);'],
    ['c_backtrack',        '            current.pop_back();'],
    ['',                   '        }'],
    ['',                   '    }'],
    ['',                   '}'],
    ['',                   ''],
    ['c_entry',            'vector<vector<int>> combinationSum(vector<int>& candidates, int target) {'],
    ['c_init_result',      '    vector<vector<int>> result;'],
    ['c_init_current',     '    vector<int> current;'],
    ['c_sort_cand',        '    sort(candidates.begin(), candidates.end());'],
    ['c_call_bt',          '    backtrack(candidates, target, 0, current, result);'],
    ['c_return_res',       '    return result;'],
    ['',                   '}'],
    ['',                   ''],
    ['',                   'int main() {'],
    ['m_scanner',          '    int n;'],
    ['',                   '    int target;'],
    ['m_read_n',           '    cin >> n;'],
    ['m_read_target',      '    cin >> target;'],
    ['m_alloc_arr',        '    vector<int> candidates(n);'],
    ['m_loop_read',        '    for (int i = 0; i < n; i++) {'],
    ['m_read_elem',        '        cin >> candidates[i];'],
    ['',                   '    }'],
    ['m_call_solver',      '    vector<vector<int>> result = combinationSum(candidates, target);'],
    ['m_print_count',      '    cout << result.size() << "\\n";'],
    ['m_loop_print',       '    for (int i = 0; i < (int)result.size(); i++) {'],
    ['m_print_elem',       '        cout << "[";'],
    ['',                   '        for (int j = 0; j < (int)result[i].size(); j++) {'],
    ['',                   '            cout << result[i][j] << (j + 1 < (int)result[i].size() ? ", " : "");'],
    ['',                   '        }'],
    ['',                   '        cout << "]\\n";'],
    ['',                   '    }'],
    ['m_done',             '    return 0;'],
    ['',                   '}']
  ],
  python: [
    ['',                   'import sys'],
    ['',                   ''],
    ['c_bt_entry',         'def backtrack(candidates, target, start, current, result):'],
    ['c_base_check',       '    if target == 0:'],
    ['c_save_sol',         '        result.append(list(current))'],
    ['c_base_ret',         '        return'],
    ['c_loop_cand',        '    for i in range(start, len(candidates)):'],
    ['c_check_fit',        '        if candidates[i] <= target:'],
    ['c_add_num',          '            current.append(candidates[i])'],
    ['c_recurse',          '            backtrack(candidates, target - candidates[i], i, current, result)'],
    ['c_backtrack',        '            current.pop()'],
    ['',                   ''],
    ['c_entry',            'def combination_sum(candidates, target):'],
    ['c_init_result',      '    result = []'],
    ['c_init_current',     '    current = []'],
    ['c_sort_cand',        '    candidates.sort()'],
    ['c_call_bt',          '    backtrack(candidates, target, 0, current, result)'],
    ['c_return_res',       '    return result'],
    ['',                   ''],
    ['',                   'def main():'],
    ['m_scanner',          '    input_data = sys.stdin.read().split()'],
    ['',                   '    if not input_data:'],
    ['',                   '        return'],
    ['m_read_n',           '    n = int(input_data[0])'],
    ['m_read_target',      '    target = int(input_data[1])'],
    ['m_alloc_arr',        '    candidates = []'],
    ['m_loop_read',        '    for i in range(n):'],
    ['m_read_elem',        '        candidates.append(int(input_data[2 + i]))'],
    ['m_call_solver',      '    result = combination_sum(candidates, target)'],
    ['m_print_count',      '    print(len(result))'],
    ['m_loop_print',       '    for i in range(len(result)):'],
    ['m_print_elem',       '        print(result[i])'],
    ['m_done',             ''],
    ['',                   'if __name__ == "__main__":'],
    ['',                   '    main()']
  ],
  javascript: [
    ['',                   'const fs = require("fs");'],
    ['',                   ''],
    ['c_bt_entry',         'function backtrack(candidates, target, start, current, result) {'],
    ['c_base_check',       '    if (target === 0) {'],
    ['c_save_sol',         '        result.push([...current]);'],
    ['c_base_ret',         '        return;'],
    ['',                   '    }'],
    ['c_loop_cand',        '    for (let i = start; i < candidates.length; i++) {'],
    ['c_check_fit',        '        if (candidates[i] <= target) {'],
    ['c_add_num',          '            current.push(candidates[i]);'],
    ['c_recurse',          '            backtrack(candidates, target - candidates[i], i, current, result);'],
    ['c_backtrack',        '            current.pop();'],
    ['',                   '        }'],
    ['',                   '    }'],
    ['',                   '}'],
    ['',                   ''],
    ['c_entry',            'function combinationSum(candidates, target) {'],
    ['c_init_result',      '    const result = [];'],
    ['c_init_current',     '    const current = [];'],
    ['c_sort_cand',        '    candidates.sort((a, b) => a - b);'],
    ['c_call_bt',          '    backtrack(candidates, target, 0, current, result);'],
    ['c_return_res',       '    return result;'],
    ['',                   '}'],
    ['',                   ''],
    ['',                   'function main() {'],
    ['m_scanner',          '    let input = [];'],
    ['',                   '    try {'],
    ['',                   '        const raw = fs.readFileSync(0, "utf-8").trim();'],
    ['',                   '        if (raw.length > 0) {'],
    ['',                   '            input = raw.split(/\\s+/);'],
    ['',                   '        }'],
    ['',                   '    } catch (e) {'],
    ['',                   '        input = [];'],
    ['',                   '    }'],
    ['',                   '    if (!input || input.length < 2) {'],
    ['',                   '        return;'],
    ['',                   '    }'],
    ['m_read_n',           '    const n = parseInt(input[0], 10);'],
    ['m_read_target',      '    const target = parseInt(input[1], 10);'],
    ['m_alloc_arr',        '    const candidates = [];'],
    ['m_loop_read',        '    for (let i = 0; i < n; i++) {'],
    ['m_read_elem',        '        candidates.push(parseInt(input[2 + i], 10));'],
    ['',                   '    }'],
    ['m_call_solver',      '    const result = combinationSum(candidates, target);'],
    ['m_print_count',      '    console.log(result.length);'],
    ['m_loop_print',       '    for (let i = 0; i < result.length; i++) {'],
    ['m_print_elem',       '        console.log(JSON.stringify(result[i]));'],
    ['',                   '    }'],
    ['m_done',             '    return;'],
    ['',                   '}'],
    ['',                   'main();']
  ]
};

const PSEUDOCODE = [
  'function combinationSum(candidates, target):',
  '    result = []',
  '    current = []',
  '    sort(candidates)',
  '    backtrack(candidates, target, 0, current, result)',
  '    return result',
  '',
  'function backtrack(candidates, target, start, current, result):',
  '    if target == 0:',
  '        result.append(copy of current)',
  '        return',
  '    for i from start to length(candidates) - 1:',
  '        if candidates[i] <= target:',
  '            current.append(candidates[i])',
  '            backtrack(candidates, target - candidates[i], i, current, result)',
  '            current.pop()    // Backtrack'
];

function buildSteps(n, rawCandidates, target) {
  const steps = [];
  const frame = (title, rowsArr) => ({ title, rows: rowsArr });

  const solutions = [];
  const initialTarget = target;
  const initialArr = [...rawCandidates];
  const sortedCandidates = [...rawCandidates].sort((a, b) => a - b);

  // ─── Main: Scanner setup ──────────────────────────────────────────────────
  steps.push({
    badge: `Scanner sc = new Scanner(System.in); → Preparing standard input reader.`,
    code: 'm_scanner',
    vars: [frame('main()', [['n', '?'], ['target', '?']])],
    n,
    target,
    start: 0,
    loopI: null,
    candVal: null,
    current: [],
    currentSum: 0,
    remainingTarget: target,
    candidates: [...initialArr],
    sorted: false,
    actionStatus: 'normal',
    solCount: 0,
    solutions: []
  });

  // ─── Main: Read n ──────────────────────────────────────────────────────────
  steps.push({
    badge: `int n = sc.nextInt(); → Reading candidate count n = ${n}.`,
    code: 'm_read_n',
    vars: [frame('main()', [['n', String(n)], ['target', '?']])],
    n,
    target,
    start: 0,
    loopI: null,
    candVal: null,
    current: [],
    currentSum: 0,
    remainingTarget: target,
    candidates: [...initialArr],
    sorted: false,
    actionStatus: 'normal',
    solCount: 0,
    solutions: []
  });

  // ─── Main: Read target ────────────────────────────────────────────────────
  steps.push({
    badge: `int target = sc.nextInt(); → Reading target value = ${target}.`,
    code: 'm_read_target',
    vars: [frame('main()', [['n', String(n)], ['target', String(target)]])],
    n,
    target,
    start: 0,
    loopI: null,
    candVal: null,
    current: [],
    currentSum: 0,
    remainingTarget: target,
    candidates: [...initialArr],
    sorted: false,
    actionStatus: 'normal',
    solCount: 0,
    solutions: []
  });

  // ─── Main: Alloc array ────────────────────────────────────────────────────
  steps.push({
    badge: `int[] candidates = new int[${n}]; → Allocating array for ${n} candidates.`,
    code: 'm_alloc_arr',
    vars: [frame('main()', [['n', String(n)], ['target', String(target)], ['candidates', `new int[${n}]`]])],
    n,
    target,
    start: 0,
    loopI: null,
    candVal: null,
    current: [],
    currentSum: 0,
    remainingTarget: target,
    candidates: [...initialArr],
    sorted: false,
    actionStatus: 'normal',
    solCount: 0,
    solutions: []
  });

  // ─── Main: Read candidates per iteration ─────────────────────────────────
  for (let _ri = 0; _ri < n; _ri++) {
    // Loop condition: i < n → TRUE
    steps.push({
      badge: `for (int i = ${_ri}; i < ${n}; i++) → (${_ri} < ${n}) is TRUE. Reading candidate element at index ${_ri}.`,
      code: 'm_loop_read',
      vars: [frame('main()', [['n', String(n)], ['target', String(target)], ['i', String(_ri)]])],
      n,
      target,
      start: 0,
      loopI: _ri,
      candVal: initialArr[_ri],
      current: [],
      currentSum: 0,
      remainingTarget: target,
      candidates: [...initialArr],
      sorted: false,
      actionStatus: 'normal',
      solCount: 0,
      solutions: []
    });

    // Body: candidates[i] = sc.nextInt();
    steps.push({
      badge: `candidates[${_ri}] = sc.nextInt(); → Reading value ${initialArr[_ri]} into candidates[${_ri}].`,
      code: 'm_read_elem',
      vars: [frame('main()', [['n', String(n)], ['target', String(target)], ['i', String(_ri)], [`candidates[${_ri}]`, String(initialArr[_ri])]])],
      n,
      target,
      start: 0,
      loopI: _ri,
      candVal: initialArr[_ri],
      current: [],
      currentSum: 0,
      remainingTarget: target,
      candidates: [...initialArr],
      sorted: false,
      actionStatus: 'normal',
      solCount: 0,
      solutions: []
    });
  }

  // Loop condition: i = n → FALSE
  steps.push({
    badge: `for (int i = ${n}; i < ${n}; i++) → (${n} < ${n}) is FALSE. All ${n} candidates read successfully.`,
    code: 'm_loop_read',
    vars: [frame('main()', [['n', String(n)], ['target', String(target)], ['i', String(n)], ['loop', 'EXIT']])],
    n,
    target,
    start: 0,
    loopI: n,
    candVal: null,
    current: [],
    currentSum: 0,
    remainingTarget: target,
    candidates: [...initialArr],
    sorted: false,
    actionStatus: 'normal',
    solCount: 0,
    solutions: []
  });

  // ─── Main: Call Solver ────────────────────────────────────────────────────
  steps.push({
    badge: `List<List<Integer>> result = combinationSum(candidates, ${target}); → Calling combination sum solver.`,
    code: 'm_call_solver',
    vars: [frame('main()', [['n', String(n)], ['target', String(target)]])],
    n,
    target,
    start: 0,
    loopI: null,
    candVal: null,
    current: [],
    currentSum: 0,
    remainingTarget: target,
    candidates: [...initialArr],
    sorted: false,
    actionStatus: 'normal',
    solCount: 0,
    solutions: []
  });

  // ─── combinationSum: Entry ────────────────────────────────────────────────
  steps.push({
    badge: `combinationSum(candidates, target = ${target}) entry → Initializing combination collection containers.`,
    code: 'c_entry',
    vars: [frame('main()', [['n', String(n)], ['target', String(target)]]), frame('combinationSum()', [['target', String(target)]])],
    n,
    target,
    start: 0,
    loopI: null,
    candVal: null,
    current: [],
    currentSum: 0,
    remainingTarget: target,
    candidates: [...initialArr],
    sorted: false,
    actionStatus: 'normal',
    solCount: 0,
    solutions: []
  });

  // ─── combinationSum: Init result ──────────────────────────────────────────
  steps.push({
    badge: `List<List<Integer>> result = new ArrayList<>(); → Creating list to store valid combinations.`,
    code: 'c_init_result',
    vars: [frame('main()', [['n', String(n)], ['target', String(target)]]), frame('combinationSum()', [['result', 'new ArrayList<>()']])],
    n,
    target,
    start: 0,
    loopI: null,
    candVal: null,
    current: [],
    currentSum: 0,
    remainingTarget: target,
    candidates: [...initialArr],
    sorted: false,
    actionStatus: 'normal',
    solCount: 0,
    solutions: []
  });

  // ─── combinationSum: Init current ─────────────────────────────────────────
  steps.push({
    badge: `List<Integer> current = new ArrayList<>(); → Creating tracking container for current combination path.`,
    code: 'c_init_current',
    vars: [frame('main()', [['n', String(n)], ['target', String(target)]]), frame('combinationSum()', [['current', '[]']])],
    n,
    target,
    start: 0,
    loopI: null,
    candVal: null,
    current: [],
    currentSum: 0,
    remainingTarget: target,
    candidates: [...initialArr],
    sorted: false,
    actionStatus: 'normal',
    solCount: 0,
    solutions: []
  });

  // ─── combinationSum: Sort candidates ──────────────────────────────────────
  steps.push({
    badge: `Arrays.sort(candidates); → Sorting candidates in ascending order: [${sortedCandidates.join(', ')}].`,
    code: 'c_sort_cand',
    vars: [frame('main()', [['n', String(n)], ['target', String(target)]]), frame('combinationSum()', [['candidates', `[${sortedCandidates.join(', ')}]`]])],
    n,
    target,
    start: 0,
    loopI: null,
    candVal: null,
    current: [],
    currentSum: 0,
    remainingTarget: target,
    candidates: [...sortedCandidates],
    sorted: true,
    actionStatus: 'normal',
    solCount: 0,
    solutions: []
  });

  // ─── combinationSum: Call backtrack ───────────────────────────────────────
  steps.push({
    badge: `backtrack(candidates, target = ${target}, start = 0, current, result); → Initiating recursive exploration.`,
    code: 'c_call_bt',
    vars: [frame('main()', [['n', String(n)], ['target', String(target)]]), frame('combinationSum()', [['start', '0'], ['target', String(target)]])],
    n,
    target,
    start: 0,
    loopI: null,
    candVal: null,
    current: [],
    currentSum: 0,
    remainingTarget: target,
    candidates: [...sortedCandidates],
    sorted: true,
    actionStatus: 'normal',
    solCount: 0,
    solutions: []
  });

  const callStackFrames = [];

  function getStackSnapshot(extraRows = []) {
    const list = [
      frame('main()', [['target', String(initialTarget)], ['n', String(n)]]),
      frame('combinationSum()', [['result_size', String(solutions.length)]])
    ];
    for (const f of callStackFrames) {
      list.push(frame(f.name, f.args));
    }
    if (extraRows.length && list.length > 0) {
      const top = list[list.length - 1];
      if (top) {
        top.rows = [...top.rows, ...extraRows];
      }
    }
    return list;
  }

  function simulateBacktrack(remTarget, startIdx, currentList) {
    const currentSum = initialTarget - remTarget;
    callStackFrames.push({
      name: `backtrack(target=${remTarget}, start=${startIdx})`,
      args: [
        ['target', String(remTarget)],
        ['start', String(startIdx)],
        ['current', `[${currentList.join(', ')}]`]
      ]
    });

    // ── Entry Step ──────────────────────────────────────────────────────────
    steps.push({
      badge: `backtrack(candidates, target = ${remTarget}, start = ${startIdx}, current = [${currentList.join(', ')}]) → Exploring combinations. Sum: ${currentSum} / ${initialTarget}.`,
      code: 'c_bt_entry',
      vars: getStackSnapshot(),
      n,
      target: initialTarget,
      start: startIdx,
      loopI: null,
      candVal: null,
      current: [...currentList],
      currentSum,
      remainingTarget: remTarget,
      candidates: [...sortedCandidates],
      sorted: true,
      actionStatus: 'normal',
      solCount: solutions.length,
      solutions: [...solutions]
    });

    // ── Base check: if (target == 0) ────────────────────────────────────────
    const isTargetZero = (remTarget === 0);
    steps.push({
      badge: `Checking base case: if (target == 0) → (${remTarget} == 0) is ${isTargetZero ? 'TRUE! EXACT TARGET REACHED!' : 'FALSE (remaining target > 0, continuing)'}.`,
      code: 'c_base_check',
      vars: getStackSnapshot(),
      n,
      target: initialTarget,
      start: startIdx,
      loopI: null,
      candVal: null,
      current: [...currentList],
      currentSum,
      remainingTarget: remTarget,
      candidates: [...sortedCandidates],
      sorted: true,
      actionStatus: isTargetZero ? 'found' : 'normal',
      solCount: solutions.length,
      solutions: [...solutions]
    });

    if (isTargetZero) {
      solutions.push([...currentList]);

      // ── Save Solution ─────────────────────────────────────────────────────
      steps.push({
        badge: `🎉 COMBINATION FOUND! #${solutions.length}: [${currentList.join(', ')}] sums to ${initialTarget}. Adding to results.`,
        code: 'c_save_sol',
        vars: getStackSnapshot([['status', 'FOUND_COMBINATION']]),
        n,
        target: initialTarget,
        start: startIdx,
        loopI: null,
        candVal: null,
        current: [...currentList],
        currentSum,
        remainingTarget: remTarget,
        candidates: [...sortedCandidates],
        sorted: true,
        actionStatus: 'found',
        solCount: solutions.length,
        solutions: [...solutions]
      });

      // ── Return from Base Case ─────────────────────────────────────────────
      steps.push({
        badge: `return; → Returning from base case to explore alternate combination branches.`,
        code: 'c_base_ret',
        vars: getStackSnapshot(),
        n,
        target: initialTarget,
        start: startIdx,
        loopI: null,
        candVal: null,
        current: [...currentList],
        currentSum,
        remainingTarget: remTarget,
        candidates: [...sortedCandidates],
        sorted: true,
        actionStatus: 'normal',
        solCount: solutions.length,
        solutions: [...solutions]
      });

      callStackFrames.pop();
      return;
    }

    // ── Loop over candidates starting from startIdx ──────────────────────────
    for (let i = startIdx; i < sortedCandidates.length; i++) {
      const cand = sortedCandidates[i];

      // Loop header check: condition check is explicitly evaluated
      steps.push({
        badge: `Candidate loop: for (int i = ${i}; i < ${sortedCandidates.length}; i++) → (${i} < ${sortedCandidates.length}) is TRUE. Examining candidate[${i}] = ${cand}.`,
        code: 'c_loop_cand',
        vars: getStackSnapshot([['i', String(i)], ['cand', String(cand)]]),
        n,
        target: initialTarget,
        start: startIdx,
        loopI: i,
        candVal: cand,
        current: [...currentList],
        currentSum,
        remainingTarget: remTarget,
        candidates: [...sortedCandidates],
        sorted: true,
        actionStatus: 'normal',
        solCount: solutions.length,
        solutions: [...solutions]
      });

      // ── Condition check: if (candidates[i] <= target) ──────────────────────
      const fits = (cand <= remTarget);
      steps.push({
        badge: `Fit check: if (candidates[${i}] <= target) → (${cand} <= ${remTarget}) is ${fits ? 'TRUE. Candidate can be selected!' : 'FALSE (' + cand + ' > ' + remTarget + '). Exceeds target!'}`,
        code: 'c_check_fit',
        vars: getStackSnapshot([['i', String(i)], ['cand', String(cand)], ['fits', fits ? 'TRUE' : 'FALSE']]),
        n,
        target: initialTarget,
        start: startIdx,
        loopI: i,
        candVal: cand,
        current: [...currentList],
        currentSum,
        remainingTarget: remTarget,
        candidates: [...sortedCandidates],
        sorted: true,
        actionStatus: fits ? 'fit' : 'exceeds',
        solCount: solutions.length,
        solutions: [...solutions]
      });

      if (fits) {
        // ── Append number: current.add(candidates[i]); ───────────────────────
        const nextList = [...currentList, cand];
        const newSum = currentSum + cand;
        const newRem = remTarget - cand;

        steps.push({
          badge: `current.add(${cand}); → Appending ${cand} to current combination tray: [${nextList.join(', ')}]. New sum: ${newSum} / ${initialTarget}.`,
          code: 'c_add_num',
          vars: getStackSnapshot([['i', String(i)], ['added', String(cand)]]),
          n,
          target: initialTarget,
          start: startIdx,
          loopI: i,
          candVal: cand,
          current: [...nextList],
          currentSum: newSum,
          remainingTarget: newRem,
          candidates: [...sortedCandidates],
          sorted: true,
          actionStatus: 'fit',
          solCount: solutions.length,
          solutions: [...solutions]
        });

        // ── Recursive call: backtrack(candidates, target - candidates[i], i, current, result);
        steps.push({
          badge: `backtrack(candidates, target = ${remTarget} - ${cand} = ${newRem}, start = ${i}, current = [${nextList.join(', ')}]); → Recursing with same index ${i} to allow reuse.`,
          code: 'c_recurse',
          vars: getStackSnapshot([['recurse_target', String(newRem)], ['recurse_start', String(i)]]),
          n,
          target: initialTarget,
          start: startIdx,
          loopI: i,
          candVal: cand,
          current: [...nextList],
          currentSum: newSum,
          remainingTarget: newRem,
          candidates: [...sortedCandidates],
          sorted: true,
          actionStatus: 'fit',
          solCount: solutions.length,
          solutions: [...solutions]
        });

        simulateBacktrack(newRem, i, nextList);

        // ── Backtrack: current.remove(current.size() - 1); ───────────────────
        steps.push({
          badge: `current.remove(current.size() - 1); → Backtracking: Popping ${cand} from [${nextList.join(', ')}] to restore combination to [${currentList.join(', ')}].`,
          code: 'c_backtrack',
          vars: getStackSnapshot([['backtrack_popped', String(cand)]]),
          n,
          target: initialTarget,
          start: startIdx,
          loopI: i,
          candVal: cand,
          current: [...currentList],
          currentSum,
          remainingTarget: remTarget,
          candidates: [...sortedCandidates],
          sorted: true,
          actionStatus: 'backtrack',
          solCount: solutions.length,
          solutions: [...solutions]
        });
      }
    }

    // ── Loop termination check ──────────────────────────────────────────────
    steps.push({
      badge: `Candidate loop finished: for (int i = ${sortedCandidates.length}; i < ${sortedCandidates.length}; i++) → (${sortedCandidates.length} < ${sortedCandidates.length}) is FALSE. All branches from start = ${startIdx} explored.`,
      code: 'c_loop_cand',
      vars: getStackSnapshot([['i', String(sortedCandidates.length)], ['loop_status', 'TERMINATED']]),
      n,
      target: initialTarget,
      start: startIdx,
      loopI: sortedCandidates.length,
      candVal: null,
      current: [...currentList],
      currentSum,
      remainingTarget: remTarget,
      candidates: [...sortedCandidates],
      sorted: true,
      actionStatus: 'normal',
      solCount: solutions.length,
      solutions: [...solutions]
    });

    callStackFrames.pop();
  }

  simulateBacktrack(target, 0, []);

  // ─── combinationSum: return result ────────────────────────────────────────
  steps.push({
    badge: `return result; → Returning ${solutions.length} discovered combinations to main().`,
    code: 'c_return_res',
    vars: [frame('main()', [['n', String(n)], ['target', String(target)]]), frame('combinationSum()', [['total_solutions', String(solutions.length)]])],
    n,
    target,
    start: 0,
    loopI: null,
    candVal: null,
    current: [],
    currentSum: 0,
    remainingTarget: 0,
    candidates: [...sortedCandidates],
    sorted: true,
    actionStatus: 'normal',
    solCount: solutions.length,
    solutions: [...solutions]
  });

  // ─── Main: Print count ────────────────────────────────────────────────────
  steps.push({
    badge: `System.out.println(result.size()); → Printing total combinations count: ${solutions.length}.`,
    code: 'm_print_count',
    vars: [frame('main()', [['total_combinations', String(solutions.length)]])],
    n,
    target,
    start: 0,
    loopI: null,
    candVal: null,
    current: [],
    currentSum: 0,
    remainingTarget: 0,
    candidates: [...sortedCandidates],
    sorted: true,
    actionStatus: 'normal',
    solCount: solutions.length,
    solutions: [...solutions]
  });

  // ─── Main: Print each combination per iteration ───────────────────────────
  for (let _pi = 0; _pi < solutions.length; _pi++) {
    // Loop condition: i < result.size() → TRUE
    steps.push({
      badge: `for (int i = ${_pi}; i < ${solutions.length}; i++) → (${_pi} < ${solutions.length}) is TRUE. Will print result.get(${_pi}).`,
      code: 'm_loop_print',
      vars: [frame('main()', [['i', String(_pi)]])],
      n,
      target,
      start: 0,
      loopI: _pi,
      candVal: null,
      current: [...solutions[_pi]],
      currentSum: target,
      remainingTarget: 0,
      candidates: [...sortedCandidates],
      sorted: true,
      actionStatus: 'found',
      solCount: solutions.length,
      solutions: [...solutions]
    });

    // Body: System.out.println(result.get(i));
    steps.push({
      badge: `System.out.println(result.get(${_pi})); → Printing: [${solutions[_pi].join(', ')}]`,
      code: 'm_print_elem',
      vars: [frame('main()', [['i', String(_pi)], ['output', `[${solutions[_pi].join(', ')}]`]])],
      n,
      target,
      start: 0,
      loopI: _pi,
      candVal: null,
      current: [...solutions[_pi]],
      currentSum: target,
      remainingTarget: 0,
      candidates: [...sortedCandidates],
      sorted: true,
      actionStatus: 'found',
      solCount: solutions.length,
      solutions: [...solutions]
    });
  }

  if (solutions.length > 0) {
    steps.push({
      badge: `for (int i = ${solutions.length}; i < ${solutions.length}; i++) → (${solutions.length} < ${solutions.length}) is FALSE. All combinations printed.`,
      code: 'm_loop_print',
      vars: [frame('main()', [['i', String(solutions.length)], ['loop_status', 'DONE']])],
      n,
      target,
      start: 0,
      loopI: solutions.length,
      candVal: null,
      current: [],
      currentSum: 0,
      remainingTarget: 0,
      candidates: [...sortedCandidates],
      sorted: true,
      actionStatus: 'normal',
      solCount: solutions.length,
      solutions: [...solutions]
    });
  }

  // ─── Main: Done ───────────────────────────────────────────────────────────
  steps.push({
    badge: `Program execution complete. Backtracking found ${solutions.length} unique combination(s) summing to ${target}.`,
    code: 'm_done',
    vars: [frame('main()', [['status', 'finished'], ['combinations_found', String(solutions.length)]])],
    n,
    target,
    start: 0,
    loopI: null,
    candVal: null,
    current: [],
    currentSum: 0,
    remainingTarget: 0,
    candidates: [...sortedCandidates],
    sorted: true,
    actionStatus: 'normal',
    solCount: solutions.length,
    solutions: [...solutions]
  });

  return steps;
}

const inputN = ref(4);
const candidatesInputStr = ref('2, 3, 6, 7');
const inputTarget = ref(7);

const activeN = ref(4);
const activeCandidates = ref([2, 3, 6, 7]);
const activeTarget = ref(7);

const lang = ref('java');
const speed = ref(650);
const si = ref(0);
const playing = ref(false);
const vizHeight = ref(235);
const tableHeight = ref(60);
const leftWidth = ref(50);
const rightTab = ref('code');
const selectedSolutionIdx = ref(-1);

const stepsData = reactive({ steps: buildSteps(4, [2, 3, 6, 7], 7) });
const steps = computed(() => stepsData.steps);
const s = computed(() => steps.value[Math.max(0, Math.min(si.value, steps.value.length - 1))] || {});
const codeLines = computed(() => CODES[lang.value] || []);

let playTimer = null;

function onChipsWheel(e) {
  if (e.currentTarget) {
    e.currentTarget.scrollLeft += e.deltaY;
  }
}

function parseCandidates(str, expectedN) {
  try {
    let clean = str.trim();
    if (clean.startsWith('[') && clean.endsWith(']')) {
      clean = clean.slice(1, -1);
    }
    const parts = clean.split(/[,\\s]+/).filter(x => x.length > 0);
    const nums = parts.map(x => parseInt(x, 10)).filter(x => !isNaN(x) && x > 0);
    if (nums.length === 0) return null;
    const unique = Array.from(new Set(nums));
    return unique;
  } catch (e) {
    return null;
  }
}

function applyInput() {
  const nVal = parseInt(inputN.value, 10);
  const tgtVal = parseInt(inputTarget.value, 10);

  if (isNaN(nVal) || nVal < 1 || nVal > 10) {
    alert('Please enter a valid candidate count n between 1 and 10.');
    inputN.value = 4;
    return;
  }

  if (isNaN(tgtVal) || tgtVal < 1 || tgtVal > 40) {
    alert('Please enter a valid target between 1 and 40.');
    inputTarget.value = 7;
    return;
  }

  let parsed = parseCandidates(candidatesInputStr.value, nVal);
  if (!parsed || parsed.length === 0) {
    parsed = [2, 3, 6, 7];
    candidatesInputStr.value = '2, 3, 6, 7';
  }

  inputN.value = parsed.length;
  activeN.value = parsed.length;
  activeCandidates.value = parsed;
  activeTarget.value = tgtVal;

  playing.value = false;
  selectedSolutionIdx.value = -1;
  stepsData.steps = buildSteps(activeN.value, activeCandidates.value, activeTarget.value);
  si.value = 0;
  if (typeof window !== 'undefined') window.scrollTo(0, 0);
}

function loadPreset(presetType) {
  if (presetType === 'default') {
    inputN.value = 4;
    candidatesInputStr.value = '2, 3, 6, 7';
    inputTarget.value = 7;
  } else if (presetType === 'ex2') {
    inputN.value = 3;
    candidatesInputStr.value = '2, 3, 5';
    inputTarget.value = 8;
  } else if (presetType === 'simple') {
    inputN.value = 2;
    candidatesInputStr.value = '2, 3';
    inputTarget.value = 5;
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

const displayCurrent = computed(() => {
  if (selectedSolutionIdx.value >= 0 && s.value.solutions && s.value.solutions[selectedSolutionIdx.value]) {
    return s.value.solutions[selectedSolutionIdx.value];
  }
  return s.value.current || [];
});

const displayCurrentSum = computed(() => {
  const arr = displayCurrent.value;
  return arr.reduce((acc, v) => acc + v, 0);
});

const displayRemaining = computed(() => {
  return Math.max(0, (s.value.target || activeTarget.value) - displayCurrentSum.value);
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
    <!-- Navbar Header (Exact same as RatInAMaze) -->
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
              <label>n:</label>
              <input
                type="number"
                min="1"
                max="10"
                v-model.number="inputN"
                class="ll-text-input"
                @keyup.enter="applyInput"
                style="width: 42px;"
              />
            </div>

            <div class="ll-input-group">
              <label>target:</label>
              <input
                type="number"
                min="1"
                max="40"
                v-model.number="inputTarget"
                class="ll-text-input"
                @keyup.enter="applyInput"
                style="width: 42px;"
              />
            </div>

            <div class="ll-input-group">
              <label>candidates[]:</label>
              <input
                type="text"
                v-model="candidatesInputStr"
                class="ll-text-input"
                @keyup.enter="applyInput"
                title="Input array e.g. [2, 3, 6, 7]"
                style="width: 170px; font-size: 10.5px;"
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
                  <div class="ll-ptrs ll-ptrs-compact" @wheel.passive="onChipsWheel">
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">n</span><b class="ll-c-blue">{{ s.n || activeN }}</b></span>
                    <!-- <span class="ll-ptr-chip-inline"><span class="ll-chip-label">target</span><b class="ll-c-blue">{{ s.target || activeTarget }}</b></span> -->
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">target</span><b class="ll-c-orange">{{ s.remainingTarget !== undefined ? s.remainingTarget : '—' }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">start</span><b class="ll-c-green">{{ s.start !== undefined ? s.start : '—' }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">i</span><b class="ll-c-purple">{{ s.loopI !== null && s.loopI !== undefined ? s.loopI : '—' }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">cand[i]</span><b class="ll-c-orange">{{ s.candVal !== null && s.candVal !== undefined ? s.candVal : '—' }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">current</span><b class="ll-c-green">[{{ (s.current || []).join(', ') }}]</b></span>
                  </div>

                  <!-- Board Container (Customized for Combination Sum with exact same frame & cell aesthetics) -->
                  <div class="ll-board-container">
                    <div class="ll-board-frame">
                      <!-- Target Progress Row -->
                      <div class="cs-prog-row">
                        <span class="cs-prog-label">Target: <b class="ll-c-blue">{{ s.target || activeTarget }}</b> &bull; Sum: <b class="ll-c-purple">{{ displayCurrentSum }}</b></span>
                        <div class="cs-prog-bar">
                          <div
                            class="cs-prog-fill"
                            :style="{ width: Math.min(100, (displayCurrentSum / (s.target || activeTarget)) * 100) + '%' }"
                            :class="{ 'cs-prog-match': displayRemaining === 0 && displayCurrentSum > 0 }"
                          ></div>
                        </div>
                        <span class="cs-prog-rem" :class="{ 'cs-rem-zero': displayRemaining === 0 && displayCurrentSum > 0 }">
                          {{ displayRemaining === 0 && displayCurrentSum > 0 ? 'Met!' : 'Rem: ' + displayRemaining }}
                        </span>
                      </div>

                      <!-- Column Headers (Indices 0, 1, 2, ...) -->
                      <div class="ll-col-indices">
                        <div class="ll-axis-spacer"></div>
                        <div
                          v-for="(cand, cIdx) in (s.candidates || activeCandidates)"
                          :key="'col-h-' + cIdx"
                          class="ll-axis-label"
                          :class="{ 'll-axis-cur': s.loopI === cIdx }"
                        >
                          {{ cIdx }}
                        </div>
                      </div>

                      <!-- Candidates Array Row using .ll-chess-cell -->
                      <div class="ll-board-row">
                        <div class="ll-axis-label ll-row-label">cand</div>
                        <div
                          v-for="(cand, cIdx) in (s.candidates || activeCandidates)"
                          :key="'cand-cell-' + cIdx"
                          class="ll-chess-cell"
                          :class="{
                            'll-cell-rat': s.loopI === cIdx,
                            'll-cell-visited': s.start !== undefined && cIdx >= s.start && s.loopI !== cIdx,
                            'll-cell-conflict': s.loopI === cIdx && s.actionStatus === 'exceeds',
                            'll-cell-testing': s.loopI === cIdx && s.actionStatus === 'fit',
                            'll-cell-open': s.loopI !== cIdx,
                            'll-cell-dimmed': s.start !== undefined && cIdx < s.start
                          }"
                          :title="`candidates[${cIdx}] = ${cand}`"
                        >
                          <span class="ll-cand-val">{{ cand }}</span>
                          <span v-if="s.start === cIdx" class="ll-ptr-badge ll-ptr-start" title="Loop start index">start</span>
                          <span v-if="s.loopI === cIdx" class="ll-ptr-badge ll-ptr-cur" title="Current pointer i">i</span>
                        </div>
                      </div>

                      <!-- Current Combination Tray Row -->
                      <div class="cs-combo-row">
                        <span class="cs-combo-lbl">current:</span>
                        <div class="cs-combo-tray">
                          <template v-if="displayCurrent && displayCurrent.length">
                            <span v-for="(num, nIdx) in displayCurrent" :key="'cn-' + nIdx" class="cs-combo-num">
                              {{ num }}
                            </span>
                            <span class="cs-combo-sum">= {{ displayCurrentSum }}</span>
                          </template>
                          <span v-else class="cs-combo-empty">&lang; empty &rang;</span>
                        </div>
                      </div>
                    </div>

                    <!-- Discovered Solutions Gallery -->
                    <div class="ll-solutions-strip" v-if="s.solutions && s.solutions.length">
                      <div class="ll-sol-title">Discovered Combinations ({{ s.solutions.length }}):</div>
                      <div class="ll-sol-cards">
                        <button
                          v-for="(sol, idx) in s.solutions"
                          :key="'sol-btn-' + idx"
                          class="ll-sol-badge-btn"
                          :class="{ active: selectedSolutionIdx === idx }"
                          @click="viewSolution(idx)"
                          :title="`Click to inspect Combination #${idx + 1}: [${sol.join(', ')}]`"
                        >
                          Combo #{{ idx + 1 }}: [{{ sol.join(', ') }}]
                        </button>
                      </div>
                    </div>
                  </div>
                </div>
              </div>

              <!-- Resizer -->
              <div class="ll-vresizer" ref="vizResizerRef"></div>

              <!-- Color Legend (Exact same as RatInAMaze) -->
              <div class="ll-legend">
                <span class="ll-leg"><span class="ll-legdot" style="background:#ffffff; border:1px solid #cbd5e1;"></span>Candidate</span>
                <span class="ll-leg"><span class="ll-legdot" style="background:#eff6ff; border:1.5px solid var(--blue);"></span>Current (i)</span>
                <span class="ll-leg"><span class="ll-legdot ll-legdot-safe"></span>Fits (&le;)</span>
                <span class="ll-leg"><span class="ll-legdot ll-legdot-mismatch"></span>Exceeds (&gt;)</span>
                <span class="ll-leg"><span class="ll-legdot ll-legdot-max"></span>Target</span>
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
                    'll-badge-error': s.badge && (s.badge.includes('Exceeds') || s.badge.includes('FALSE') || s.badge.includes('exceeds')),
                    'll-badge-success': s.badge && (s.badge.includes('FOUND') || s.badge.includes('EXACT') || s.badge.includes('complete') || s.badge.includes('TRUE'))
                  }"
                >
                  {{ s.badge || 'Ready to run Combination Sum Backtracking algorithm.' }}
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
                  <h3 class="ll-cx-heading">Combination Sum &mdash; Complexity Analysis</h3>
                  <p class="ll-cx-intro">
                    Finds all unique combinations in <code>candidates</code> whose sum equals <code>target</code>.
                    The same number may be chosen from <code>candidates</code> an unlimited number of times.
                    To avoid duplicate combinations, candidates are sorted and each recursive call only branches to candidates at indices <strong>&ge; current start index</strong>.
                  </p>

                  <h4 class="ll-cx-sub">Step-by-step Breakdown</h4>
                  <table class="ll-complexity-table">
                    <thead>
                      <tr><th>Operation / Phase</th><th>Time</th><th>Space</th><th>Reason</th></tr>
                    </thead>
                    <tbody>
                      <tr>
                        <td>Sorting Candidates</td>
                        <td class="ll-cx-good">O(N log N)</td>
                        <td class="ll-cx-good">O(1) / O(N)</td>
                        <td>Sorts candidate array of size N to allow ascending order and pruning</td>
                      </tr>
                      <tr>
                        <td>Backtracking Tree Exploration</td>
                        <td class="ll-cx-mid">O(N<sup>T / M</sup>)</td>
                        <td class="ll-cx-good">O(T / M)</td>
                        <td>Where T = target, M = min(candidates). Max tree depth is bounded by T / M</td>
                      </tr>
                      <tr>
                        <td>Copying Valid Combination</td>
                        <td class="ll-cx-good">O(T / M)</td>
                        <td class="ll-cx-good">O(T / M)</td>
                        <td>Adding a copy of current combination to result when target reaches 0</td>
                      </tr>
                    </tbody>
                  </table>

                  <h4 class="ll-cx-sub">Overall Complexity</h4>
                  <div class="ll-cx-summary-grid">
                    <div class="ll-cx-card ll-cx-card-mid">
                      <div class="ll-cx-card-label">Time Complexity</div>
                      <div class="ll-cx-card-val">O(N<sup>T / M</sup>)</div>
                      <div class="ll-cx-card-note">Bounded by total nodes in recursion tree</div>
                    </div>
                    <div class="ll-cx-card ll-cx-card-good">
                      <div class="ll-cx-card-label">Auxiliary Space</div>
                      <div class="ll-cx-card-val">O(T / M)</div>
                      <div class="ll-cx-card-note">Recursion call stack &amp; current combination path</div>
                    </div>
                    <div class="ll-cx-card ll-cx-card-good">
                      <div class="ll-cx-card-label">Combinations</div>
                      <div class="ll-cx-card-val">&lt; 150</div>
                      <div class="ll-cx-card-note">LeetCode guarantees &lt; 150 combinations per test</div>
                    </div>
                  </div>

                  <div class="ll-note">
                    <strong>Backtracking Insight:</strong> By passing <code>i</code> (instead of <code>i + 1</code>) in <code>backtrack(candidates, target - candidates[i], i, ...)</code>, the algorithm allows unlimited reuse of <code>candidates[i]</code>. Because <code>candidates</code> is sorted and the loop runs from <code>start</code> to <code>N - 1</code>, combinations are generated in non-decreasing order, naturally eliminating duplicate permutations like <code>[2, 3, 2]</code> vs <code>[2, 2, 3]</code>.
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

/* ─── Board Container (Customized for Combination Sum with exact same frame & cell aesthetics) ─── */
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
  padding: 10px 14px;
  border-radius: var(--radius);
  border: 1px solid var(--border);
  box-shadow: var(--shadow-sm);
  margin-bottom: 8px;
  min-width: 280px;
}

.ll-col-indices {
  display: flex;
  margin-bottom: 4px;
}

.ll-axis-spacer {
  width: 32px;
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
  width: 32px;
  height: 44px;
  margin-right: 2px;
  font-size: 10px;
  font-weight: 700;
  text-transform: uppercase;
  color: var(--muted);
}

.ll-axis-cur {
  color: var(--coral) !important;
  font-weight: 800;
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

.ll-cell-dimmed {
  opacity: 0.38;
  background: #f1f5f9;
}

.ll-cand-val {
  font-size: 16px;
  font-weight: 800;
  color: var(--text);
  font-family: monospace;
}

.ll-ptr-badge {
  position: absolute;
  font-size: 8.5px;
  font-weight: 800;
  line-height: 1;
}

.ll-ptr-start {
  top: 2px;
  left: 3px;
  color: #16a34a;
}

.ll-ptr-cur {
  bottom: 2px;
  right: 3px;
  color: var(--coral);
}

/* Combination Sum Progress Row */
.cs-prog-row {
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 2px 2px 8px;
  border-bottom: 1px solid var(--border);
  margin-bottom: 8px;
  font-size: 11px;
}

.cs-prog-label {
  color: var(--text2);
  white-space: nowrap;
}

.cs-prog-bar {
  flex: 1;
  height: 9px;
  background: #e2e8f0;
  border-radius: 5px;
  overflow: hidden;
  min-width: 60px;
}

.cs-prog-fill {
  height: 100%;
  background: linear-gradient(90deg, var(--blue), var(--purple));
  border-radius: 5px;
  transition: width .2s ease;
}

.cs-prog-match {
  background: linear-gradient(90deg, #10b981, #059669) !important;
}

.cs-prog-rem {
  font-size: 10.5px;
  font-weight: 700;
  color: var(--orange);
  white-space: nowrap;
}

.cs-rem-zero {
  color: #10b981 !important;
}

/* Current Combination Tray Row */
.cs-combo-row {
  display: flex;
  align-items: center;
  gap: 6px;
  padding: 8px 2px 2px;
  border-top: 1px solid var(--border);
  margin-top: 8px;
  font-size: 11px;
}

.cs-combo-lbl {
  font-weight: 700;
  color: var(--text2);
  font-family: monospace;
}

.cs-combo-tray {
  display: flex;
  align-items: center;
  gap: 4px;
  flex-wrap: wrap;
}

.cs-combo-num {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  min-width: 24px;
  height: 24px;
  background: #eff6ff;
  border: 1.5px solid var(--blue);
  color: #1d4ed8;
  font-size: 13px;
  font-weight: 800;
  font-family: monospace;
  border-radius: 4px;
  padding: 0 5px;
  animation: ll-pop 0.2s ease;
}

.cs-combo-sum {
  font-weight: 800;
  color: var(--purple);
  margin-left: 2px;
  font-family: monospace;
  font-size: 12px;
}

.cs-combo-empty {
  color: var(--muted);
  font-style: italic;
  font-size: 11px;
}

/* Solutions Strip (Exact same as RatInAMaze) */
.ll-solutions-strip {
  display: flex;
  flex-direction: column;
  gap: 4px;
  margin-top: 4px;
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
