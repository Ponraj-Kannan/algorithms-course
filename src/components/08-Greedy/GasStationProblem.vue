<script setup>
import { ref, reactive, computed, onMounted, onUnmounted, watch } from 'vue';

const props = defineProps({
  topic: { type: String, default: 'Greedy Algorithms' },
  subTopic: { type: String, default: 'Gas Station Problem' }
});

/* ==================================================================== */
/* APPROACH METADATA                                                    */
/* Brute Force comes before Greedy                                      */
/* ==================================================================== */
const APPROACHES = [
  { id: 'brute', label: 'Brute Force', desc: 'O(n^2) Quadratic — Exhaustive Circuit Simulation from Each Station' },
  { id: 'greedy', label: 'Greedy', desc: 'O(n) Linear Time & O(1) Space — Running Balance Scan with Range Invalidation' }
];

/* ==================================================================== */
/* CODE SNIPPETS (Java, C, C++, Python)                                 */
/* Strict curly brace formatting on all control statements              */
/* ==================================================================== */
const CODES = {
  brute: {
    java: [
      ['',                 'import java.util.Scanner;'],
      ['',                 ''],
      ['',                 'public class Main {'],
      ['c_entry',          '    static int canCompleteCircuit(int[] gas, int[] cost, int n) {'],
      ['c_for_start',      '        for (int start = 0; start < n; start++) {'],
      ['c_init_tank',      '            int tank = 0;'],
      ['c_init_count',     '            int count = 0;'],
      ['c_while_loop',     '            while (count < n) {'],
      ['c_calc_curr',      '                int curr = (start + count) % n;'],
      ['c_update_tank',    '                tank += gas[curr] - cost[curr];'],
      ['c_check_tank',     '                if (tank < 0) {'],
      ['c_break',          '                    break;'],
      ['',                 '                }'],
      ['c_inc_count',      '                count++;'],
      ['',                 '            }'],
      ['c_check_full',     '            if (count == n) {'],
      ['c_ret_start',      '                return start;'],
      ['',                 '            }'],
      ['',                 '        }'],
      ['c_ret_neg',        '        return -1;'],
      ['',                 '    }'],
      ['',                 ''],
      ['',                 '    public static void main(String[] args) {'],
      ['m_scanner',        '        Scanner sc = new Scanner(System.in);'],
      ['m_read_n',         '        int n = sc.nextInt();'],
      ['m_alloc_gas',      '        int[] gas = new int[n];'],
      ['m_for_gas',        '        for (int i = 0; i < n; i++) {'],
      ['m_read_gas',       '            gas[i] = sc.nextInt();'],
      ['',                 '        }'],
      ['m_alloc_cost',     '        int[] cost = new int[n];'],
      ['m_for_cost',       '        for (int i = 0; i < n; i++) {'],
      ['m_read_cost',      '            cost[i] = sc.nextInt();'],
      ['',                 '        }'],
      ['m_call_circ',      '        int ans = canCompleteCircuit(gas, cost, n);'],
      ['m_print_ans',      '        System.out.println(ans);'],
      ['m_done',           '    }'],
      ['',                 '}']
    ],
    c: [
      ['',                 '#include <stdio.h>'],
      ['',                 ''],
      ['c_entry',          'int canCompleteCircuit(int gas[], int cost[], int n) {'],
      ['c_for_start',      '    for (int start = 0; start < n; start++) {'],
      ['c_init_tank',      '        int tank = 0;'],
      ['c_init_count',     '        int count = 0;'],
      ['c_while_loop',     '        while (count < n) {'],
      ['c_calc_curr',      '            int curr = (start + count) % n;'],
      ['c_update_tank',    '            tank += gas[curr] - cost[curr];'],
      ['c_check_tank',     '            if (tank < 0) {'],
      ['c_break',          '                break;'],
      ['',                 '            }'],
      ['c_inc_count',      '            count++;'],
      ['',                 '        }'],
      ['c_check_full',     '        if (count == n) {'],
      ['c_ret_start',      '            return start;'],
      ['',                 '        }'],
      ['',                 '    }'],
      ['c_ret_neg',        '    return -1;'],
      ['',                 '}'],
      ['',                 ''],
      ['',                 'int main() {'],
      ['m_scanner',        '    int n;'],
      ['m_read_n',         '    scanf("%d", &n);'],
      ['m_alloc_gas',      '    int gas[100];'],
      ['m_for_gas',        '    for (int i = 0; i < n; i++) {'],
      ['m_read_gas',       '        scanf("%d", &gas[i]);'],
      ['',                 '    }'],
      ['m_alloc_cost',     '    int cost[100];'],
      ['m_for_cost',       '    for (int i = 0; i < n; i++) {'],
      ['m_read_cost',      '        scanf("%d", &cost[i]);'],
      ['',                 '    }'],
      ['m_call_circ',      '    int ans = canCompleteCircuit(gas, cost, n);'],
      ['m_print_ans',      '    printf("%d\\n", ans);'],
      ['m_done',           '    return 0;'],
      ['',                 '}']
    ],
    cpp: [
      ['',                 '#include <iostream>'],
      ['',                 '#include <vector>'],
      ['',                 'using namespace std;'],
      ['',                 ''],
      ['c_entry',          'int canCompleteCircuit(const vector<int>& gas, const vector<int>& cost, int n) {'],
      ['c_for_start',      '    for (int start = 0; start < n; start++) {'],
      ['c_init_tank',      '        int tank = 0;'],
      ['c_init_count',     '        int count = 0;'],
      ['c_while_loop',     '        while (count < n) {'],
      ['c_calc_curr',      '            int curr = (start + count) % n;'],
      ['c_update_tank',    '            tank += gas[curr] - cost[curr];'],
      ['c_check_tank',     '            if (tank < 0) {'],
      ['c_break',          '                break;'],
      ['',                 '            }'],
      ['c_inc_count',      '            count++;'],
      ['',                 '        }'],
      ['c_check_full',     '        if (count == n) {'],
      ['c_ret_start',      '            return start;'],
      ['',                 '        }'],
      ['',                 '    }'],
      ['c_ret_neg',        '    return -1;'],
      ['',                 '}'],
      ['',                 ''],
      ['',                 'int main() {'],
      ['m_scanner',        '    int n;'],
      ['m_read_n',         '    cin >> n;'],
      ['m_alloc_gas',      '    vector<int> gas(n);'],
      ['m_for_gas',        '    for (int i = 0; i < n; i++) {'],
      ['m_read_gas',       '        cin >> gas[i];'],
      ['',                 '    }'],
      ['m_alloc_cost',     '    vector<int> cost(n);'],
      ['m_for_cost',       '    for (int i = 0; i < n; i++) {'],
      ['m_read_cost',      '        cin >> cost[i];'],
      ['',                 '    }'],
      ['m_call_circ',      '    int ans = canCompleteCircuit(gas, cost, n);'],
      ['m_print_ans',      '    cout << ans << endl;'],
      ['m_done',           '    return 0;'],
      ['',                 '}']
    ],
    python: [
      ['',                 'import sys'],
      ['',                 ''],
      ['c_entry',          'def can_complete_circuit(gas, cost, n):'],
      ['c_for_start',      '    for start in range(n):'],
      ['c_init_tank',      '        tank = 0'],
      ['c_init_count',     '        count = 0'],
      ['c_while_loop',     '        while count < n:'],
      ['c_calc_curr',      '            curr = (start + count) % n'],
      ['c_update_tank',    '            tank += gas[curr] - cost[curr]'],
      ['c_check_tank',     '            if tank < 0:'],
      ['c_break',          '                break'],
      ['c_inc_count',      '            count += 1'],
      ['c_check_full',     '        if count == n:'],
      ['c_ret_start',      '            return start'],
      ['c_ret_neg',        '    return -1'],
      ['',                 ''],
      ['',                 'def main():'],
      ['m_scanner',        '    tokens = sys.stdin.read().split()'],
      ['m_read_n',         '    if not tokens: return'],
      ['',                 '    n = int(tokens[0])'],
      ['m_alloc_gas',      '    gas = []'],
      ['m_for_gas',        '    idx = 1'],
      ['',                 '    for i in range(n):'],
      ['m_read_gas',       '        gas.append(int(tokens[idx])); idx += 1'],
      ['m_alloc_cost',     '    cost = []'],
      ['m_for_cost',       '    for i in range(n):'],
      ['m_read_cost',      '        cost.append(int(tokens[idx])); idx += 1'],
      ['m_call_circ',      '    ans = can_complete_circuit(gas, cost, n)'],
      ['m_print_ans',      '    print(ans)'],
      ['m_done',           '    return'],
      ['',                 ''],
      ['',                 'if __name__ == "__main__":'],
      ['',                 '    main()']
    ]
  },
  greedy: {
    java: [
      ['',                 'import java.util.Scanner;'],
      ['',                 ''],
      ['',                 'public class Main {'],
      ['c_entry',          '    static int canCompleteCircuit(int[] gas, int[] cost, int n) {'],
      ['c_init_total',     '        int totalTank = 0;'],
      ['c_init_curr',      '        int currTank = 0;'],
      ['c_init_start',     '        int startStation = 0;'],
      ['c_for_loop',       '        for (int i = 0; i < n; i++) {'],
      ['c_calc_net',       '            int net = gas[i] - cost[i];'],
      ['c_add_total',      '            totalTank += net;'],
      ['c_add_curr',       '            currTank += net;'],
      ['c_check_deficit',  '            if (currTank < 0) {'],
      ['c_reset_start',    '                startStation = i + 1;'],
      ['c_reset_curr',     '                currTank = 0;'],
      ['',                 '            }'],
      ['',                 '        }'],
      ['c_check_total',    '        if (totalTank >= 0) {'],
      ['c_ret_start',      '            return startStation;'],
      ['',                 '        } else {'],
      ['c_ret_neg',        '            return -1;'],
      ['',                 '        }'],
      ['',                 '    }'],
      ['',                 ''],
      ['',                 '    public static void main(String[] args) {'],
      ['m_scanner',        '        Scanner sc = new Scanner(System.in);'],
      ['m_read_n',         '        int n = sc.nextInt();'],
      ['m_alloc_gas',      '        int[] gas = new int[n];'],
      ['m_for_gas',        '        for (int i = 0; i < n; i++) {'],
      ['m_read_gas',       '            gas[i] = sc.nextInt();'],
      ['',                 '        }'],
      ['m_alloc_cost',     '        int[] cost = new int[n];'],
      ['m_for_cost',       '        for (int i = 0; i < n; i++) {'],
      ['m_read_cost',      '            cost[i] = sc.nextInt();'],
      ['',                 '        }'],
      ['m_call_circ',      '        int ans = canCompleteCircuit(gas, cost, n);'],
      ['m_print_ans',      '        System.out.println(ans);'],
      ['m_done',           '    }'],
      ['',                 '}']
    ],
    c: [
      ['',                 '#include <stdio.h>'],
      ['',                 ''],
      ['c_entry',          'int canCompleteCircuit(int gas[], int cost[], int n) {'],
      ['c_init_total',     '    int totalTank = 0;'],
      ['c_init_curr',      '    int currTank = 0;'],
      ['c_init_start',     '    int startStation = 0;'],
      ['c_for_loop',       '    for (int i = 0; i < n; i++) {'],
      ['c_calc_net',       '        int net = gas[i] - cost[i];'],
      ['c_add_total',      '        totalTank += net;'],
      ['c_add_curr',       '        currTank += net;'],
      ['c_check_deficit',  '        if (currTank < 0) {'],
      ['c_reset_start',    '            startStation = i + 1;'],
      ['c_reset_curr',     '            currTank = 0;'],
      ['',                 '        }'],
      ['',                 '    }'],
      ['c_check_total',    '    if (totalTank >= 0) {'],
      ['c_ret_start',      '        return startStation;'],
      ['',                 '    } else {'],
      ['c_ret_neg',        '        return -1;'],
      ['',                 '    }'],
      ['',                 '}'],
      ['',                 ''],
      ['',                 'int main() {'],
      ['m_scanner',        '    int n;'],
      ['m_read_n',         '    scanf("%d", &n);'],
      ['m_alloc_gas',      '    int gas[100];'],
      ['m_for_gas',        '    for (int i = 0; i < n; i++) {'],
      ['m_read_gas',       '        scanf("%d", &gas[i]);'],
      ['',                 '    }'],
      ['m_alloc_cost',     '    int cost[100];'],
      ['m_for_cost',       '    for (int i = 0; i < n; i++) {'],
      ['m_read_cost',      '        scanf("%d", &cost[i]);'],
      ['',                 '    }'],
      ['m_call_circ',      '    int ans = canCompleteCircuit(gas, cost, n);'],
      ['m_print_ans',      '    printf("%d\\n", ans);'],
      ['m_done',           '    return 0;'],
      ['',                 '}']
    ],
    cpp: [
      ['',                 '#include <iostream>'],
      ['',                 '#include <vector>'],
      ['',                 'using namespace std;'],
      ['',                 ''],
      ['c_entry',          'int canCompleteCircuit(const vector<int>& gas, const vector<int>& cost, int n) {'],
      ['c_init_total',     '    int totalTank = 0;'],
      ['c_init_curr',      '    int currTank = 0;'],
      ['c_init_start',     '    int startStation = 0;'],
      ['c_for_loop',       '    for (int i = 0; i < n; i++) {'],
      ['c_calc_net',       '        int net = gas[i] - cost[i];'],
      ['c_add_total',      '        totalTank += net;'],
      ['c_add_curr',       '        currTank += net;'],
      ['c_check_deficit',  '        if (currTank < 0) {'],
      ['c_reset_start',    '            startStation = i + 1;'],
      ['c_reset_curr',     '            currTank = 0;'],
      ['',                 '        }'],
      ['',                 '    }'],
      ['c_check_total',    '    if (totalTank >= 0) {'],
      ['c_ret_start',      '        return startStation;'],
      ['',                 '    } else {'],
      ['c_ret_neg',        '        return -1;'],
      ['',                 '    }'],
      ['',                 '}'],
      ['',                 ''],
      ['',                 'int main() {'],
      ['m_scanner',        '    int n;'],
      ['m_read_n',         '    cin >> n;'],
      ['m_alloc_gas',      '    vector<int> gas(n);'],
      ['m_for_gas',        '    for (int i = 0; i < n; i++) {'],
      ['m_read_gas',       '        cin >> gas[i];'],
      ['',                 '    }'],
      ['m_alloc_cost',     '    vector<int> cost(n);'],
      ['m_for_cost',       '    for (int i = 0; i < n; i++) {'],
      ['m_read_cost',      '        cin >> cost[i];'],
      ['',                 '    }'],
      ['m_call_circ',      '    int ans = canCompleteCircuit(gas, cost, n);'],
      ['m_print_ans',      '    cout << ans << endl;'],
      ['m_done',           '    return 0;'],
      ['',                 '}']
    ],
    python: [
      ['',                 'import sys'],
      ['',                 ''],
      ['c_entry',          'def can_complete_circuit(gas, cost, n):'],
      ['c_init_total',     '    total_tank = 0'],
      ['c_init_curr',      '    curr_tank = 0'],
      ['c_init_start',     '    start_station = 0'],
      ['c_for_loop',       '    for i in range(n):'],
      ['c_calc_net',       '        net = gas[i] - cost[i]'],
      ['c_add_total',      '        total_tank += net'],
      ['c_add_curr',       '        curr_tank += net'],
      ['c_check_deficit',  '        if curr_tank < 0:'],
      ['c_reset_start',    '            start_station = i + 1'],
      ['c_reset_curr',     '            curr_tank = 0'],
      ['c_check_total',    '    if total_tank >= 0:'],
      ['c_ret_start',      '        return start_station'],
      ['',                 '    else:'],
      ['c_ret_neg',        '        return -1'],
      ['',                 ''],
      ['',                 'def main():'],
      ['m_scanner',        '    tokens = sys.stdin.read().split()'],
      ['m_read_n',         '    if not tokens: return'],
      ['',                 '    n = int(tokens[0])'],
      ['m_alloc_gas',      '    gas = []'],
      ['m_for_gas',        '    idx = 1'],
      ['',                 '    for i in range(n):'],
      ['m_read_gas',       '        gas.append(int(tokens[idx])); idx += 1'],
      ['m_alloc_cost',     '    cost = []'],
      ['m_for_cost',       '    for i in range(n):'],
      ['m_read_cost',      '        cost.append(int(tokens[idx])); idx += 1'],
      ['m_call_circ',      '    ans = can_complete_circuit(gas, cost, n)'],
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
    'function canCompleteCircuit(gas, cost, n):',
    '    for start = 0 to n - 1:                         // Try every station as start candidate',
    '        tank = 0, count = 0',
    '        while count < n:                            // Traverse clockwise along circuit',
    '            curr = (start + count) % n',
    '            tank = tank + gas[curr] - cost[curr]    // Refuel and pay travel cost',
    '            if tank < 0: break                      // Deficit: cannot reach next station',
    '            count = count + 1',
    '        if count == n: return start                 // Complete circuit tour accomplished',
    '    return -1                                       // Impossible from any station'
  ],
  greedy: [
    'function canCompleteCircuit(gas, cost, n):',
    '    totalTank = 0, currTank = 0, startStation = 0',
    '    for i = 0 to n - 1:                             // Single forward scan',
    '        net = gas[i] - cost[i]',
    '        totalTank = totalTank + net                 // Track global feasibility',
    '        currTank = currTank + net                   // Track segment feasibility',
    '        if currTank < 0:                            // Deficit at station i',
    '            startStation = i + 1                    // Skip range [startStation .. i]',
    '            currTank = 0                            // Reset tank for next candidate',
    '    if totalTank >= 0: return startStation          // Feasible if global gas >= cost',
    '    else: return -1                                 // Impossible: net deficit'
  ]
};

/* ==================================================================== */
/* STEP GENERATOR (buildSteps)                                          */
/* Line-by-line zero-skip stepping engine                               */
/* ==================================================================== */
function buildSteps(approach, gasArr, costArr) {
  const steps = [];
  const n = gasArr.length;
  const gas = [...gasArr];
  const cost = [...costArr];

  function frame(fnName, vars) {
    return { name: fnName, vars, title: fnName, rows: vars };
  }

  // --- Common Input Reading Steps ---
  steps.push({
    code: 'm_scanner',
    msg: 'Initialize input reader and allocate memory.',
    badge: 'main(): Initialize scanner and allocate variables.',
    vars: [frame('main()', [['n', '?']])],
    state: {
      currentStation: -1,
      startStation: -1,
      tank: 0,
      totalTank: 0,
      status: 'init',
      displayGas: Array(n).fill('?'),
      displayCost: Array(n).fill('?'),
      gasFilled: Array(n).fill(false),
      costFilled: Array(n).fill(false),
      readingType: null,
      readingIdx: -1
    }
  });

  steps.push({
    code: 'm_read_n',
    msg: `Read total number of gas stations: n = ${n}.`,
    badge: `main(): Read n = ${n}`,
    vars: [frame('main()', [['n', String(n)]])],
    state: {
      currentStation: -1,
      startStation: -1,
      tank: 0,
      totalTank: 0,
      status: 'init',
      displayGas: Array(n).fill('?'),
      displayCost: Array(n).fill('?'),
      gasFilled: Array(n).fill(false),
      costFilled: Array(n).fill(false),
      readingType: null,
      readingIdx: -1
    }
  });

  const curGasArr = Array(n).fill(0);
  const curGasFilled = Array(n).fill(false);

  steps.push({
    code: 'm_alloc_gas',
    msg: `Allocate array gas of size ${n}: initialized with default 0s.`,
    badge: `main(): Allocate gas[${n}] -> [${curGasArr.join(', ')}]`,
    vars: [frame('main()', [['n', String(n)]])],
    state: {
      currentStation: -1,
      startStation: -1,
      tank: 0,
      totalTank: 0,
      status: 'alloc_gas',
      displayGas: [...curGasArr],
      displayCost: Array(n).fill('?'),
      gasFilled: [...curGasFilled],
      costFilled: Array(n).fill(false),
      readingType: 'gas',
      readingIdx: -1
    }
  });

  for (let i = 0; i < n; i++) {
    steps.push({
      code: 'm_for_gas',
      msg: `Loop reading gas fuel: station index i = ${i}.`,
      badge: `main(): For i = ${i} < ${n}`,
      vars: [frame('main()', [['i', String(i)], ['n', String(n)]])],
      state: {
        currentStation: -1,
        startStation: -1,
        tank: 0,
        totalTank: 0,
        status: 'reading_gas',
        displayGas: [...curGasArr],
        displayCost: Array(n).fill('?'),
        gasFilled: [...curGasFilled],
        costFilled: Array(n).fill(false),
        readingType: 'gas',
        readingIdx: i
      }
    });

    curGasArr[i] = gas[i];
    curGasFilled[i] = true;

    steps.push({
      code: 'm_read_gas',
      msg: `Read gas[${i}] = ${gas[i]} fuel units available.`,
      badge: `main(): gas[${i}] = ${gas[i]}`,
      vars: [frame('main()', [['i', String(i)], ['gas[' + i + ']', String(gas[i])]])],
      state: {
        currentStation: -1,
        startStation: -1,
        tank: 0,
        totalTank: 0,
        status: 'reading_gas',
        displayGas: [...curGasArr],
        displayCost: Array(n).fill('?'),
        gasFilled: [...curGasFilled],
        costFilled: Array(n).fill(false),
        readingType: 'gas',
        readingIdx: i
      }
    });
  }

  const curCostArr = Array(n).fill(0);
  const curCostFilled = Array(n).fill(false);

  steps.push({
    code: 'm_alloc_cost',
    msg: `Allocate array cost of size ${n}: initialized with default 0s.`,
    badge: `main(): Allocate cost[${n}] -> [${curCostArr.join(', ')}]`,
    vars: [frame('main()', [['n', String(n)]])],
    state: {
      currentStation: -1,
      startStation: -1,
      tank: 0,
      totalTank: 0,
      status: 'alloc_cost',
      displayGas: [...gas],
      displayCost: [...curCostArr],
      gasFilled: Array(n).fill(true),
      costFilled: [...curCostFilled],
      readingType: 'cost',
      readingIdx: -1
    }
  });

  for (let i = 0; i < n; i++) {
    steps.push({
      code: 'm_for_cost',
      msg: `Loop reading trip cost: station index i = ${i}.`,
      badge: `main(): For i = ${i} < ${n}`,
      vars: [frame('main()', [['i', String(i)], ['n', String(n)]])],
      state: {
        currentStation: -1,
        startStation: -1,
        tank: 0,
        totalTank: 0,
        status: 'reading_cost',
        displayGas: [...gas],
        displayCost: [...curCostArr],
        gasFilled: Array(n).fill(true),
        costFilled: [...curCostFilled],
        readingType: 'cost',
        readingIdx: i
      }
    });

    curCostArr[i] = cost[i];
    curCostFilled[i] = true;

    steps.push({
      code: 'm_read_cost',
      msg: `Read cost[${i}] = ${cost[i]} fuel units required to reach station ${(i + 1) % n}.`,
      badge: `main(): cost[${i}] = ${cost[i]}`,
      vars: [frame('main()', [['i', String(i)], ['cost[' + i + ']', String(cost[i])]])],
      state: {
        currentStation: -1,
        startStation: -1,
        tank: 0,
        totalTank: 0,
        status: 'reading_cost',
        displayGas: [...gas],
        displayCost: [...curCostArr],
        gasFilled: Array(n).fill(true),
        costFilled: [...curCostFilled],
        readingType: 'cost',
        readingIdx: i
      }
    });
  }

  steps.push({
    code: 'm_call_circ',
    msg: `Call canCompleteCircuit(gas, cost, ${n}) to determine if a valid starting station exists.`,
    badge: `main(): canCompleteCircuit(gas, cost, ${n})`,
    vars: [frame('main()', [['n', String(n)]])],
    state: {
      currentStation: 0,
      startStation: 0,
      tank: 0,
      totalTank: 0,
      status: 'call',
      displayGas: [...gas],
      displayCost: [...cost],
      gasFilled: Array(n).fill(true),
      costFilled: Array(n).fill(true),
      readingType: null,
      readingIdx: -1
    }
  });

  // --- Approach Execution ---
  let finalAns = -1;

  if (approach === 'brute') {
    steps.push({
      code: 'c_entry',
      msg: 'Enter canCompleteCircuit: test every candidate start station from 0 to n - 1.',
      badge: 'canCompleteCircuit(): Brute Force Exhaustive Simulation',
      vars: [frame('main()', [['n', String(n)]]), frame('canCompleteCircuit()', [['n', String(n)]])],
      state: { currentStation: 0, startStation: 0, tank: 0, count: 0, status: 'start_trial' }
    });

    let found = false;
    for (let start = 0; start < n; start++) {
      steps.push({
        code: 'c_for_start',
        msg: `Test candidate starting station: start = ${start}.`,
        badge: `canCompleteCircuit(): Outer loop start = ${start}`,
        vars: [frame('main()', [['n', String(n)]]), frame('canCompleteCircuit()', [['start', String(start)], ['n', String(n)]])],
        state: { currentStation: start, startStation: start, tank: 0, count: 0, status: 'testing_start' }
      });

      let tank = 0;
      steps.push({
        code: 'c_init_tank',
        msg: `Initialize vehicle fuel tank = 0 at station ${start}.`,
        badge: `canCompleteCircuit(): tank = 0 at start ${start}`,
        vars: [frame('main()', [['n', String(n)]]), frame('canCompleteCircuit()', [['start', String(start)], ['tank', '0']])],
        state: { currentStation: start, startStation: start, tank: 0, count: 0, status: 'testing_start' }
      });

      let count = 0;
      steps.push({
        code: 'c_init_count',
        msg: 'Initialize stations visited count = 0.',
        badge: 'canCompleteCircuit(): count = 0',
        vars: [frame('main()', [['n', String(n)]]), frame('canCompleteCircuit()', [['start', String(start)], ['tank', '0'], ['count', '0']])],
        state: { currentStation: start, startStation: start, tank: 0, count: 0, status: 'testing_start' }
      });

      let failed = false;
      while (count < n) {
        steps.push({
          code: 'c_while_loop',
          msg: `Check while (count=${count} < n=${n}): continue traversing circuit.`,
          badge: `canCompleteCircuit(): while (count=${count} < ${n})`,
          vars: [frame('main()', [['n', String(n)]]), frame('canCompleteCircuit()', [['start', String(start)], ['tank', String(tank)], ['count', String(count)]])],
          state: { currentStation: (start + count) % n, startStation: start, tank, count, status: 'traversing' }
        });

        const curr = (start + count) % n;
        steps.push({
          code: 'c_calc_curr',
          msg: `Current station on circuit: curr = (${start} + ${count}) % ${n} = ${curr}.`,
          badge: `canCompleteCircuit(): curr = (start + count) % n -> ${curr}`,
          vars: [frame('main()', [['n', String(n)]]), frame('canCompleteCircuit()', [['start', String(start)], ['curr', String(curr)], ['tank', String(tank)]])],
          state: { currentStation: curr, startStation: start, tank, count, status: 'at_station' }
        });

        const net = gas[curr] - cost[curr];
        tank += net;
        steps.push({
          code: 'c_update_tank',
          msg: `Station ${curr}: refuel +gas[${curr}]=${gas[curr]}, burn -cost[${curr}]=${cost[curr]} -> net delta ${net >= 0 ? '+' : ''}${net}. New tank = ${tank}.`,
          badge: `canCompleteCircuit(): tank += gas[${curr}](${gas[curr]}) - cost[${curr}](${cost[curr]}) -> tank = ${tank}`,
          vars: [frame('main()', [['n', String(n)]]), frame('canCompleteCircuit()', [['curr', String(curr)], ['gas[curr]', String(gas[curr])], ['cost[curr]', String(cost[curr])], ['tank', String(tank)]])],
          state: { currentStation: curr, startStation: start, tank, count, netDelta: net, status: tank < 0 ? 'deficit' : 'fueled' }
        });

        steps.push({
          code: 'c_check_tank',
          msg: `Check if (tank=${tank} < 0): ${tank < 0 ? 'DEFICIT! Out of fuel! Cannot reach next station.' : 'OK! Vehicle has sufficient fuel.'}`,
          badge: `canCompleteCircuit(): if (tank < 0) -> ${tank < 0 ? 'TRUE (Deficit)' : 'FALSE (Sufficient)'}`,
          vars: [frame('main()', [['n', String(n)]]), frame('canCompleteCircuit()', [['curr', String(curr)], ['tank', String(tank)]])],
          state: { currentStation: curr, startStation: start, tank, count, status: tank < 0 ? 'deficit' : 'fueled' }
        });

        if (tank < 0) {
          steps.push({
            code: 'c_break',
            msg: `Break circuit loop! Station ${curr} cannot reach ${(curr + 1) % n}. Starting at ${start} is impossible.`,
            badge: `canCompleteCircuit(): break (start = ${start} failed at station ${curr})`,
            vars: [frame('main()', [['n', String(n)]]), frame('canCompleteCircuit()', [['start', String(start)], ['curr', String(curr)], ['tank', String(tank)]])],
            state: { currentStation: curr, startStation: start, tank, count, failedAt: curr, status: 'failed_start' }
          });
          failed = true;
          break;
        }

        count++;
        steps.push({
          code: 'c_inc_count',
          msg: `Increment count to ${count}: successfully traversed station ${curr}.`,
          badge: `canCompleteCircuit(): count++ -> ${count}`,
          vars: [frame('main()', [['n', String(n)]]), frame('canCompleteCircuit()', [['start', String(start)], ['count', String(count)], ['tank', String(tank)]])],
          state: { currentStation: (start + count) % n, startStation: start, tank, count, status: 'station_cleared' }
        });
      }

      steps.push({
        code: 'c_check_full',
        msg: `Check if (count=${count} == n=${n}): ${count === n ? 'TRUE! Completed full circuit around track!' : 'FALSE (Did not complete circuit)'}`,
        badge: `canCompleteCircuit(): if (count == ${n}) -> ${count === n ? 'TRUE' : 'FALSE'}`,
        vars: [frame('main()', [['n', String(n)]]), frame('canCompleteCircuit()', [['start', String(start)], ['count', String(count)]])],
        state: { currentStation: start, startStation: start, count, status: count === n ? 'success' : 'failed' }
      });

      if (count === n) {
        steps.push({
          code: 'c_ret_start',
          msg: `Return valid starting station: ${start}! Full circular tour completed.`,
          badge: `canCompleteCircuit(): return ${start}`,
          vars: [frame('main()', [['n', String(n)]]), frame('canCompleteCircuit()', [['return', String(start)]])],
          state: { currentStation: start, startStation: start, tank, status: 'success' }
        });
        finalAns = start;
        found = true;
        break;
      }
    }

    if (!found) {
      steps.push({
        code: 'c_ret_neg',
        msg: 'All starting candidates failed. Return -1: circuit cannot be completed.',
        badge: 'canCompleteCircuit(): return -1',
        vars: [frame('main()', [['n', String(n)]]), frame('canCompleteCircuit()', [['return', '-1']])],
        state: { currentStation: -1, startStation: -1, tank: 0, status: 'impossible' }
      });
      finalAns = -1;
    }
  } else {
    // --- Greedy Approach ---
    steps.push({
      code: 'c_entry',
      msg: 'Enter canCompleteCircuit: Greedy O(n) single forward pass tracking totalTank and currTank.',
      badge: 'canCompleteCircuit(): Greedy Single Pass Scan',
      vars: [frame('main()', [['n', String(n)]]), frame('canCompleteCircuit()', [['n', String(n)]])],
      state: { currentStation: 0, startStation: 0, currTank: 0, totalTank: 0, status: 'start_greedy' }
    });

    let totalTank = 0;
    steps.push({
      code: 'c_init_total',
      msg: 'Initialize totalTank = 0: cumulative net fuel over all stations on the circuit.',
      badge: 'canCompleteCircuit(): totalTank = 0',
      vars: [frame('main()', [['n', String(n)]]), frame('canCompleteCircuit()', [['totalTank', '0']])],
      state: { currentStation: 0, startStation: 0, currTank: 0, totalTank: 0, status: 'start_greedy' }
    });

    let currTank = 0;
    steps.push({
      code: 'c_init_curr',
      msg: 'Initialize currTank = 0: fuel balance for current candidate starting station segment.',
      badge: 'canCompleteCircuit(): currTank = 0',
      vars: [frame('main()', [['n', String(n)]]), frame('canCompleteCircuit()', [['totalTank', '0'], ['currTank', '0']])],
      state: { currentStation: 0, startStation: 0, currTank: 0, totalTank: 0, status: 'start_greedy' }
    });

    let startStation = 0;
    steps.push({
      code: 'c_init_start',
      msg: 'Initialize candidate starting station startStation = 0.',
      badge: 'canCompleteCircuit(): startStation = 0',
      vars: [frame('main()', [['n', String(n)]]), frame('canCompleteCircuit()', [['startStation', '0'], ['totalTank', '0'], ['currTank', '0']])],
      state: { currentStation: 0, startStation: 0, currTank: 0, totalTank: 0, status: 'start_greedy' }
    });

    for (let i = 0; i < n; i++) {
      steps.push({
        code: 'c_for_loop',
        msg: `Inspect station i = ${i}: gas[${i}] = ${gas[i]}, cost[${i}] = ${cost[i]}.`,
        badge: `canCompleteCircuit(): For loop station i = ${i}`,
        vars: [frame('main()', [['n', String(n)]]), frame('canCompleteCircuit()', [['i', String(i)], ['startStation', String(startStation)], ['currTank', String(currTank)], ['totalTank', String(totalTank)]])],
        state: { currentStation: i, startStation, currTank, totalTank, status: 'inspecting' }
      });

      const net = gas[i] - cost[i];
      steps.push({
        code: 'c_calc_net',
        msg: `Calculate net fuel gain/loss at station ${i}: net = gas[${i}] - cost[${i}] = ${gas[i]} - ${cost[i]} = ${net}.`,
        badge: `canCompleteCircuit(): net = gas[${i}](${gas[i]}) - cost[${i}](${cost[i]}) -> net = ${net}`,
        vars: [frame('main()', [['n', String(n)]]), frame('canCompleteCircuit()', [['i', String(i)], ['net', String(net)], ['startStation', String(startStation)]])],
        state: { currentStation: i, startStation, currTank, totalTank, net, status: 'calculated_net' }
      });

      totalTank += net;
      steps.push({
        code: 'c_add_total',
        msg: `Update global track balance: totalTank += net (${net >= 0 ? '+' : ''}${net}) -> totalTank = ${totalTank}.`,
        badge: `canCompleteCircuit(): totalTank += ${net} -> ${totalTank}`,
        vars: [frame('main()', [['n', String(n)]]), frame('canCompleteCircuit()', [['i', String(i)], ['totalTank', String(totalTank)], ['currTank', String(currTank)]])],
        state: { currentStation: i, startStation, currTank, totalTank, net, status: 'updated_total' }
      });

      currTank += net;
      steps.push({
        code: 'c_add_curr',
        msg: `Update current segment tank: currTank += net (${net >= 0 ? '+' : ''}${net}) -> currTank = ${currTank}.`,
        badge: `canCompleteCircuit(): currTank += ${net} -> ${currTank}`,
        vars: [frame('main()', [['n', String(n)]]), frame('canCompleteCircuit()', [['i', String(i)], ['currTank', String(currTank)], ['totalTank', String(totalTank)]])],
        state: { currentStation: i, startStation, currTank, totalTank, net, status: currTank < 0 ? 'deficit' : 'fueled' }
      });

      steps.push({
        code: 'c_check_deficit',
        msg: `Check if (currTank=${currTank} < 0): ${currTank < 0 ? 'DEFICIT! Tank empty. Segment from ' + startStation + ' to ' + i + ' failed!' : 'OK! Fuel balance non-negative.'}`,
        badge: `canCompleteCircuit(): if (currTank < 0) -> ${currTank < 0 ? 'TRUE (Deficit)' : 'FALSE (Proceed)'}`,
        vars: [frame('main()', [['n', String(n)]]), frame('canCompleteCircuit()', [['i', String(i)], ['currTank', String(currTank)]])],
        state: { currentStation: i, startStation, currTank, totalTank, status: currTank < 0 ? 'deficit' : 'fueled' }
      });

      if (currTank < 0) {
        const oldStart = startStation;
        startStation = i + 1;
        steps.push({
          code: 'c_reset_start',
          msg: `Greedy Skip: No station in [${oldStart} .. ${i}] can reach ${i + 1}. Greedily reset startStation = ${i + 1}!`,
          badge: `canCompleteCircuit(): startStation = ${i} + 1 -> ${startStation}`,
          vars: [frame('main()', [['n', String(n)]]), frame('canCompleteCircuit()', [['i', String(i)], ['startStation', String(startStation)]])],
          state: { currentStation: i, startStation, currTank, totalTank, invalidatedRange: [oldStart, i], status: 'reset_start' }
        });

        currTank = 0;
        steps.push({
          code: 'c_reset_curr',
          msg: `Reset currTank = 0 for the new candidate start station ${startStation}.`,
          badge: `canCompleteCircuit(): currTank = 0`,
          vars: [frame('main()', [['n', String(n)]]), frame('canCompleteCircuit()', [['startStation', String(startStation)], ['currTank', '0']])],
          state: { currentStation: i, startStation, currTank: 0, totalTank, status: 'reset_tank' }
        });
      }
    }

    steps.push({
      code: 'c_check_total',
      msg: `End of loop check: if (totalTank=${totalTank} >= 0). Total fuel ${totalTank >= 0 ? '>= 0: A complete circuit is guaranteed!' : '< 0: Total gas is less than total cost, impossible.'}`,
      badge: `canCompleteCircuit(): if (totalTank >= 0) -> ${totalTank >= 0 ? 'TRUE' : 'FALSE'}`,
      vars: [frame('main()', [['n', String(n)]]), frame('canCompleteCircuit()', [['totalTank', String(totalTank)], ['startStation', String(startStation)]])],
      state: { currentStation: startStation, startStation, totalTank, status: totalTank >= 0 ? 'valid_solution' : 'no_solution' }
    });

    if (totalTank >= 0) {
      steps.push({
        code: 'c_ret_start',
        msg: `Return startStation = ${startStation}! The car can complete the entire circular route starting here.`,
        badge: `canCompleteCircuit(): return ${startStation}`,
        vars: [frame('main()', [['n', String(n)]]), frame('canCompleteCircuit()', [['return', String(startStation)]])],
        state: { currentStation: startStation, startStation, totalTank, status: 'success' }
      });
      finalAns = startStation;
    } else {
      steps.push({
        code: 'c_ret_neg',
        msg: 'Return -1: Overall gas deficit across all stations, circular tour impossible from any start.',
        badge: 'canCompleteCircuit(): return -1',
        vars: [frame('main()', [['n', String(n)]]), frame('canCompleteCircuit()', [['return', '-1']])],
        state: { currentStation: -1, startStation: -1, totalTank, status: 'impossible' }
      });
      finalAns = -1;
    }
  }

  // --- Output and Finish ---
  steps.push({
    code: 'm_print_ans',
    msg: `Print final answer: ${finalAns}.`,
    badge: `main(): System.out.println(${finalAns})`,
    vars: [frame('main()', [['ans', String(finalAns)]])],
    state: { currentStation: finalAns, startStation: finalAns, status: 'printed' }
  });

  steps.push({
    code: 'm_done',
    msg: 'Program execution completed successfully.',
    badge: 'main(): Execution finished',
    vars: [frame('main()', [['status', 'finished']])],
    state: { currentStation: finalAns, startStation: finalAns, status: 'finished' }
  });

  for (const step of steps) {
    if (!step.state.displayGas) {
      step.state.displayGas = [...gas];
      step.state.gasFilled = Array(n).fill(true);
    }
    if (!step.state.displayCost) {
      step.state.displayCost = [...cost];
      step.state.costFilled = Array(n).fill(true);
    }
  }

  return steps;
}

/* ==================================================================== */
/* COMPONENT STATE & REACTIVE SETUP                                     */
/* ==================================================================== */
const currentApproach = ref('brute');
const inputGasText = ref('1, 2, 3, 4, 5');
const inputCostText = ref('3, 4, 5, 1, 2');
const currentGas = ref([1, 2, 3, 4, 5]);
const currentCost = ref([3, 4, 5, 1, 2]);

const lang = ref('java');
const speed = ref(650);
const si = ref(0);
const playing = ref(false);
const vizHeight = ref(195);
const tableHeight = ref(42);
const leftWidth = ref(54);
const rightTab = ref('code');

function parseArray(text, fallback) {
  const parts = text.split(/[,;\s]+/).map(s => parseInt(s.trim(), 10)).filter(n => !isNaN(n));
  return parts.length > 0 ? parts : fallback;
}

function applyApproach(appId) {
  currentApproach.value = appId;
  resetAll();
}

function applyInput() {
  const parsedGas = parseArray(inputGasText.value, [1, 2, 3, 4, 5]);
  const parsedCost = parseArray(inputCostText.value, [3, 4, 5, 1, 2]);
  const minLen = Math.min(parsedGas.length, parsedCost.length);
  currentGas.value = parsedGas.slice(0, minLen);
  currentCost.value = parsedCost.slice(0, minLen);
  inputGasText.value = currentGas.value.join(', ');
  inputCostText.value = currentCost.value.join(', ');
  resetAll();
}

const steps = ref([]);

function recomputeSteps() {
  steps.value = buildSteps(currentApproach.value, currentGas.value, currentCost.value);
  si.value = 0;
}

function resetAll() {
  pause();
  recomputeSteps();
}

const s = computed(() => {
  if (!steps.value.length) return { state: {} };
  return steps.value[Math.max(0, Math.min(si.value, steps.value.length - 1))] || { state: {} };
});

const curCodeTag = computed(() => s.value?.code || '');
const curBadge = computed(() => s.value?.badge || '');
const curVars = computed(() => s.value?.vars || []);
const curState = computed(() => s.value?.state || {});

const codeLines = computed(() => {
  const appCodes = CODES[currentApproach.value] || CODES.brute;
  return appCodes[lang.value] || appCodes.java || [];
});

const pseudocodeLines = computed(() => PSEUDOCODES[currentApproach.value] || PSEUDOCODES.brute);

// Reactive array values for animation stepping
const displayGasList = computed(() => {
  return curState.value.displayGas || currentGas.value;
});

const displayCostList = computed(() => {
  return curState.value.displayCost || currentCost.value;
});

const gasFilledList = computed(() => {
  return curState.value.gasFilled || Array(currentGas.value.length).fill(true);
});

const costFilledList = computed(() => {
  return curState.value.costFilled || Array(currentCost.value.length).fill(true);
});

// Computed station delta statistics
const stationDeltas = computed(() => {
  return currentGas.value.map((g, idx) => g - currentCost.value[idx]);
});

const totalGasSum = computed(() => currentGas.value.reduce((a, b) => a + b, 0));
const totalCostSum = computed(() => currentCost.value.reduce((a, b) => a + b, 0));
const globalNetBalance = computed(() => totalGasSum.value - totalCostSum.value);

const currentTankVal = computed(() => {
  if (currentApproach.value === 'brute') {
    return curState.value.tank !== undefined ? curState.value.tank : 0;
  }
  return curState.value.currTank !== undefined ? curState.value.currTank : 0;
});

// Playback controls
let playTimer = null;

function stepBy(delta) {
  pause();
  const next = si.value + delta;
  if (next >= 0 && next < steps.value.length) {
    si.value = next;
  }
}

function togglePlay() {
  if (playing.value) {
    pause();
  } else {
    play();
  }
}

function play() {
  if (si.value >= steps.value.length - 1) {
    si.value = 0;
  }
  playing.value = true;
  scheduleNext();
}

function pause() {
  playing.value = false;
  if (playTimer) {
    clearTimeout(playTimer);
    playTimer = null;
  }
}

function scheduleNext() {
  if (!playing.value) return;
  playTimer = setTimeout(() => {
    if (!playing.value) return;
    if (si.value < steps.value.length - 1) {
      si.value++;
      scheduleNext();
    } else {
      pause();
    }
  }, speed.value);
}

function onKeydown(e) {
  const tag = e.target.tagName;
  if (tag === 'INPUT' || tag === 'SELECT' || tag === 'TEXTAREA') return;
  if (e.key === 'ArrowRight') stepBy(1);
  if (e.key === 'ArrowLeft') stepBy(-1);
  if (e.key === ' ') { e.preventDefault(); togglePlay(); }
}

/* Horizontal resizer */
const mainRef = ref(null);
const hResizerRef = ref(null);
let isDraggingH = false;

function onHMouseDown(e) {
  isDraggingH = true;
  hResizerRef.value?.classList.add('drag');
  document.addEventListener('mousemove', onHMouseMove);
  document.addEventListener('mouseup', onHMouseUp);
}
function onHMouseMove(e) {
  if (!isDraggingH || !mainRef.value) return;
  const rect = mainRef.value.getBoundingClientRect();
  const pct = ((e.clientX - rect.left) / rect.width) * 100;
  if (pct >= 25 && pct <= 75) {
    leftWidth.value = Math.round(pct);
  }
}
function onHMouseUp() {
  isDraggingH = false;
  hResizerRef.value?.classList.remove('drag');
  document.removeEventListener('mousemove', onHMouseMove);
  document.removeEventListener('mouseup', onHMouseUp);
}

/* Vertical resizers */
const leftColRef = ref(null);
const vizResizerRef = ref(null);
let isDraggingViz = false;
let startYViz = 0;
let startHeightViz = 0;

function onVizMouseDown(e) {
  isDraggingViz = true;
  startYViz = e.clientY;
  startHeightViz = vizHeight.value;
  vizResizerRef.value?.classList.add('drag');
  document.addEventListener('mousemove', onVizMouseMove);
  document.addEventListener('mouseup', onVizMouseUp);
}
function onVizMouseMove(e) {
  if (!isDraggingViz) return;
  const dy = e.clientY - startYViz;
  vizHeight.value = Math.max(160, Math.min(480, startHeightViz + dy));
}
function onVizMouseUp() {
  isDraggingViz = false;
  vizResizerRef.value?.classList.remove('drag');
  document.removeEventListener('mousemove', onVizMouseMove);
  document.removeEventListener('mouseup', onVizMouseUp);
}

const tableResizerRef = ref(null);
let isDraggingTable = false;
let startYTable = 0;
let startHeightTable = 0;

function onTableMouseDown(e) {
  isDraggingTable = true;
  startYTable = e.clientY;
  startHeightTable = tableHeight.value;
  tableResizerRef.value?.classList.add('drag');
  document.addEventListener('mousemove', onTableMouseMove);
  document.addEventListener('mouseup', onTableMouseUp);
}
function onTableMouseMove(e) {
  if (!isDraggingTable) return;
  const dy = e.clientY - startYTable;
  tableHeight.value = Math.max(30, Math.min(160, startHeightTable + dy));
}
function onTableMouseUp() {
  isDraggingTable = false;
  tableResizerRef.value?.classList.remove('drag');
  document.removeEventListener('mousemove', onTableMouseMove);
  document.removeEventListener('mouseup', onTableMouseUp);
}

onMounted(() => {
  hResizerRef.value?.addEventListener('mousedown', onHMouseDown);
  vizResizerRef.value?.addEventListener('mousedown', onVizMouseDown);
  tableResizerRef.value?.addEventListener('mousedown', onTableMouseDown);
  document.addEventListener('keydown', onKeydown);
  resetAll();
});

onUnmounted(() => {
  pause();
  document.removeEventListener('keydown', onKeydown);
  hResizerRef.value?.removeEventListener('mousedown', onHMouseDown);
  vizResizerRef.value?.removeEventListener('mousedown', onVizMouseDown);
  tableResizerRef.value?.removeEventListener('mousedown', onTableMouseDown);
});

watch(lang, () => {});
</script>

<template>
  <div class="slide-wrapper">
    <!-- Top Navbar with FACE Prep Logo -->
    <div class="navbar">
      <h2 class="navbar-title">{{ topic }} &mdash; {{ subTopic }}</h2>
      <img src="../../assets/logo.png" alt="FACE Prep" />
    </div>

    <div class="slide-body">
      <div class="row-main">
        <div class="ll-root">
          <!-- Control Toolbar -->
          <div class="ll-toolbar">
            <!-- Approach Selector Button Group (Brute Force first) -->
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

            <!-- Custom Dual Array Inputs -->
            <div class="ll-input-group">
              <label>gas =</label>
              <input
                v-model="inputGasText"
                class="ll-text-input"
                placeholder="1, 2, 3, 4, 5"
                @keyup.enter="applyInput"
                style="width: 120px;"
              />
            </div>

            <div class="ll-input-group">
              <label>cost =</label>
              <input
                v-model="inputCostText"
                class="ll-text-input"
                placeholder="3, 4, 5, 1, 2"
                @keyup.enter="applyInput"
                style="width: 120px;"
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
              <!-- Top Banner Displaying the Stepping Stone Arrays -->
              <div class="ll-house-banner">
                <div class="ll-house-title">
                  Gas Station Arrays: <code>gas[0 ... {{ currentGas.length - 1 }}]</code> &amp; <code>cost[0 ... {{ currentCost.length - 1 }}]</code>
                </div>

                <div class="ll-dual-arrays-wrap">
                  <div class="ll-arr-row">
                    <span class="ll-arr-tag">gas[]:</span>
                    <div class="ll-arr-cells">
                      <div
                        v-for="(val, idx) in displayGasList"
                        :key="'g' + idx"
                        class="ll-arr-cell"
                        :class="{
                          'll-cell-active': curState.readingType === 'gas' ? curState.readingIdx === idx : curState.currentStation === idx,
                          'll-cell-reach': curState.startStation === idx
                        }"
                      >
                        <span class="ll-cell-idx">[{{ idx }}]</span>
                        <span
                          class="ll-cell-val"
                          :class="{
                            'll-c-muted': val === '?',
                            'll-c-neutral': val === 0 && !gasFilledList[idx],
                            'll-c-green': gasFilledList[idx]
                          }"
                        >
                          {{ val === '?' ? '?' : (gasFilledList[idx] ? '+' + val : val) }}
                        </span>
                      </div>
                    </div>
                  </div>
                  <div class="ll-arr-row">
                    <span class="ll-arr-tag">cost[]:</span>
                    <div class="ll-arr-cells">
                      <div
                        v-for="(val, idx) in displayCostList"
                        :key="'c' + idx"
                        class="ll-arr-cell"
                        :class="{
                          'll-cell-active': curState.readingType === 'cost' ? curState.readingIdx === idx : curState.currentStation === idx,
                          'll-cell-reach': curState.startStation === idx
                        }"
                      >
                        <span class="ll-cell-idx">[{{ idx }}]</span>
                        <span
                          class="ll-cell-val"
                          :class="{
                            'll-c-muted': val === '?',
                            'll-c-neutral': val === 0 && !costFilledList[idx],
                            'll-c-red': costFilledList[idx]
                          }"
                        >
                          {{ val === '?' ? '?' : (costFilledList[idx] ? '-' + val : val) }}
                        </span>
                      </div>
                    </div>
                  </div>
                </div>
              </div>

              <!-- Real-time Stats Chips Bar -->
              <div class="ll-ptrs">
                <div class="ll-ptr-chip">Stations: <b class="ll-c-blue">{{ currentGas.length }}</b></div>
                <div class="ll-ptr-chip">
                  Start: <b class="ll-c-purple">{{ curState.startStation >= 0 ? '[' + curState.startStation + ']' : 'None' }}</b>
                </div>
                <div class="ll-ptr-chip">
                  Current: <b class="ll-c-orange">{{ curState.currentStation >= 0 ? '[' + curState.currentStation + ']' : (curState.status && (curState.status.includes('reading') || curState.status.includes('alloc') || curState.status === 'init') ? 'None' : 'Init') }}</b>
                </div>
                <div class="ll-ptr-chip">
                  Tank: <b :class="curState.status && (curState.status.includes('reading') || curState.status.includes('alloc') || curState.status === 'init') ? '' : (currentTankVal >= 0 ? 'll-c-green' : 'll-c-red')">
                    {{ curState.status && (curState.status.includes('reading') || curState.status.includes('alloc') || curState.status === 'init') ? '-' : currentTankVal }}
                  </b>
                </div>
                <template v-if="currentApproach === 'greedy' && (!curState.status || !curState.status.includes('reading'))">
                  <div class="ll-ptr-chip">
                    Total Track: <b :class="(curState.totalTank !== undefined ? curState.totalTank : globalNetBalance) >= 0 ? 'll-c-green' : 'll-c-red'">
                      {{ (curState.totalTank !== undefined ? curState.totalTank : globalNetBalance) >= 0 ? '+' : '' }}{{ curState.totalTank !== undefined ? curState.totalTank : globalNetBalance }}
                    </b>
                  </div>
                </template>
                <div class="ll-ptr-chip">
                  Status: <b :class="currentTankVal < 0 ? 'll-c-red' : (curState.status === 'success' ? 'll-c-green' : 'll-c-blue')">
                    {{ curState.status === 'init' ? 'Init' : (curState.status === 'alloc_gas' ? 'Alloc gas[]' : (curState.status === 'reading_gas' ? 'Reading gas[' + curState.readingIdx + ']' : (curState.status === 'alloc_cost' ? 'Alloc cost[]' : (curState.status === 'reading_cost' ? 'Reading cost[' + curState.readingIdx + ']' : (currentTankVal < 0 ? 'Deficit' : (curState.status === 'success' ? 'Circuit Complete' : (curState.status === 'impossible' ? 'Impossible (-1)' : 'Scanning'))))))) }}
                  </b>
                </div>
              </div>

              <!-- Visualization Viewport Container -->
              <div class="ll-viz-wrap" :style="{ height: vizHeight + 'px' }">
                <div class="ll-timeline-container">
                  <div class="ll-section-caption">
                    <span>{{ currentApproach === 'brute' ? 'Brute Force Circuit Simulation — Step-by-Step Tour' : 'Greedy Running Balance Track — Gas Balance & Range Invalidation' }}</span>
                    <span v-if="curState.status === 'success'" class="ll-calc-pill">&#10003; Valid Start: Station {{ curState.startStation }}</span>
                    <span v-else-if="curState.invalidatedRange" class="ll-pill-error">[Station {{ curState.invalidatedRange[0] }} ... {{ curState.invalidatedRange[1] }}] Invalidated</span>
                    <span v-else-if="curState.status === 'impossible'" class="ll-pill-error">No Solution (-1)</span>
                    <span v-else-if="currentApproach === 'greedy' && totalGasSum >= totalCostSum" class="ll-calc-pill">Circuit Net: +{{ globalNetBalance }}</span>
                  </div>

                  <div class="ll-jump-track-scroll">
                    <div class="ll-track-board" :style="{ width: Math.max(340, currentGas.length * 68) + 'px' }">
                      <!-- 1. Arc Layer -->
                      <div class="ll-arc-svg-wrap">
                        <svg class="ll-arc-svg" :style="{ width: Math.max(340, currentGas.length * 68) + 'px', height: '42px' }">
                          <path
                            v-if="curState.currentStation >= 0 && curState.status !== 'init' && curState.status !== 'impossible'"
                            :d="`M ${26 + curState.currentStation * 68} 38 Q ${26 + ((curState.currentStation + Math.min(currentGas.length - 1, (curState.currentStation + 1) % currentGas.length)) / 2) * 68} ${curState.currentStation === currentGas.length - 1 ? 6 : 18} ${curState.currentStation === currentGas.length - 1 ? 26 : 26 + (curState.currentStation + 1) * 68} 38`"
                            fill="none"
                            :stroke="currentTankVal < 0 ? '#ef4444' : '#f59e0b'"
                            stroke-width="2.5"
                            stroke-dasharray="4 2"
                            class="ll-arc-jump"
                          />
                          <text
                            v-if="curState.currentStation >= 0 && curState.status !== 'init' && curState.status !== 'impossible'"
                            :x="26 + ((curState.currentStation + Math.min(currentGas.length - 1, (curState.currentStation + 1) % currentGas.length)) / 2) * 68"
                            :y="curState.currentStation === currentGas.length - 1 ? 14 : 22"
                            text-anchor="middle"
                            class="ll-arc-txt"
                          >
                            burn -{{ currentCost[curState.currentStation] }}
                          </text>
                        </svg>
                      </div>

                      <!-- 2. Stations Track Row -->
                      <div class="ll-stones-track" :style="{ width: Math.max(340, currentGas.length * 68) + 'px' }">
                        <div
                          v-for="(val, idx) in displayGasList"
                          :key="'st-' + idx"
                          class="ll-stone-pod"
                          :class="{
                            'll-pod-active': curState.currentStation === idx || (curState.readingIdx === idx),
                            'll-pod-reach': curState.startStation === idx,
                            'll-pod-window': curState.invalidatedRange && idx >= curState.invalidatedRange[0] && idx <= curState.invalidatedRange[1],
                            'll-pod-deficit': curState.failedAt === idx || (curState.status === 'deficit' && curState.currentStation === idx),
                            'll-pod-goal': curState.status === 'success' && curState.startStation === idx
                          }"
                        >
                          <!-- Top Marker (CAR / START) -->
                          <div class="ll-pod-pointer">
                            <span v-if="curState.currentStation === idx" class="ll-pin ll-pin-active">
                              CAR
                            </span>
                            <span v-else-if="curState.startStation === idx" class="ll-pin ll-pin-goal">
                              START
                            </span>
                            <span v-else class="ll-pin ll-pin-space"></span>
                          </div>

                          <!-- Station Card -->
                          <div class="ll-stone-card">
                            <div class="ll-stone-card-idx">[{{ idx }}]</div>
                            <div
                              class="ll-stone-card-val"
                              :class="{
                                'll-c-muted': !gasFilledList[idx] || !costFilledList[idx],
                                'll-c-green': gasFilledList[idx] && costFilledList[idx] && (displayGasList[idx] - displayCostList[idx]) >= 0,
                                'll-c-red': gasFilledList[idx] && costFilledList[idx] && (displayGasList[idx] - displayCostList[idx]) < 0
                              }"
                            >
                              {{ (gasFilledList[idx] && costFilledList[idx]) ? (((displayGasList[idx] - displayCostList[idx]) >= 0 ? '+' : '') + (displayGasList[idx] - displayCostList[idx])) : '?' }}
                            </div>
                            <div class="ll-stone-card-sub">
                              g:{{ displayGasList[idx] }} | c:{{ displayCostList[idx] }}
                            </div>
                          </div>

                          <!-- Bottom Station Pins -->
                          <div class="ll-pod-barrier">
                            <div
                              v-if="curState.startStation === idx"
                              class="ll-barrier-tag"
                            >
                              <div class="ll-barrier-arrow">&#9650;</div>
                              <div class="ll-barrier-lbl">start={{ curState.startStation }}</div>
                            </div>
                            <div
                              v-else-if="curState.currentStation === idx"
                              class="ll-barrier-tag"
                            >
                              <div class="ll-barrier-arrow ll-arrow-blue">&#9650;</div>
                              <div class="ll-barrier-lbl ll-lbl-blue">tank={{ currentTankVal }}</div>
                            </div>
                          </div>
                        </div>
                      </div>

                      <!-- 3. Horizon Baseline Line below stations -->
                      <div class="ll-horizon-line-wrap" :style="{ width: Math.max(340, currentGas.length * 68) + 'px' }">
                        <div class="ll-horizon-track">
                          <div
                            class="ll-horizon-active"
                            :style="{
                              width: curState.currentStation >= 0
                                ? Math.min(100, (((curState.currentStation) + 0.5) / currentGas.length) * 100) + '%'
                                : '0%'
                            }"
                          ></div>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
              </div>

              <!-- Vertical Resizer for Viz Panel -->
              <div class="ll-vresizer" ref="vizResizerRef"></div>

              <!-- Dedicated Legend Strip (Exact match with JumpGame2) -->
              <div class="ll-legend">
                <template v-if="currentApproach === 'greedy'">
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-cur"></span>Car Location</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-purple"></span>Candidate Start</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-new"></span>Surplus (+gas)</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-red"></span>Deficit / Invalidation</span>
                </template>
                <template v-else>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-cur"></span>Car Location</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-purple"></span>Testing Start</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-new"></span>Positive Tank</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-red"></span>Tank Deficit (Break)</span>
                </template>
              </div>

              <!-- Call Stack & Variable Environment Panel -->
              <div class="ll-table-area" :style="{ height: tableHeight + 'px' }">
                <div class="ll-table-title">Call Stack &amp; Environment Variables:</div>
                <div class="ll-stack-line" v-for="(fr, fIdx) in curVars" :key="fIdx">
                  <span class="ll-fname">{{ fr.title || fr.name }}</span>
                  <span class="ll-now">&rarr;</span>
                  <span v-for="(v, vIdx) in (fr.rows || fr.vars)" :key="vIdx" class="ll-frame">
                    {{ v[0] }} = <b>{{ v[1] }}</b><span v-if="vIdx < (fr.rows || fr.vars).length - 1">, </span>
                  </span>
                </div>
              </div>

              <!-- Vertical Resizer for Call Stack Panel -->
              <div class="ll-vresizer" ref="tableResizerRef"></div>

              <!-- Current Execution Step Badge Description -->
              <div class="ll-badge-wrap">
                <div
                  class="ll-badge"
                  :class="{
                    'll-badge-error': curBadge && (curBadge.includes('DEFICIT') || curBadge.includes('Deficit') || curBadge.includes('break') || curBadge.includes('failed') || curBadge.includes('impossible')),
                    'll-badge-success': curBadge && (curBadge.includes('finished') || curBadge.includes('TRUE') || curBadge.includes('complete') || curBadge.includes('guaranteed') || curBadge.includes('return'))
                  }"
                >
                  {{ curBadge || 'Ready to visualize Gas Station Problem.' }}
                </div>
              </div>
            </div>

            <!-- Horizontal Resizer between Left and Right Columns -->
            <div class="ll-resizer" ref="hResizerRef"></div>

            <!-- Right Code & Explanation Column -->
            <div class="ll-right-col">
              <div class="ll-code-panel">
                <!-- Header Toolbar: Tabs + Language Selector -->
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
                  <pre class="ll-pre"><code class="ll-code-block"><span
                    v-for="(line, lIdx) in codeLines"
                    :key="lIdx"
                    class="ll-codeline"
                    :class="{ 'll-hl': line[0] && line[0] === curCodeTag }"
                  >{{ line[1] }}</span></code></pre>
                </div>

                <!-- Pseudocode Tab -->
                <div v-else-if="rightTab === 'pseudo'" class="ll-code-scroll">
                  <pre class="ll-pre"><code class="ll-code-block"><span
                    v-for="(line, pIdx) in pseudocodeLines"
                    :key="pIdx"
                    class="ll-codeline"
                  >{{ line }}</span></code></pre>
                </div>

                <!-- Time and Space Complexity Tab -->
                <div v-else class="ll-info-scroll">

                  <!-- ════════ BRUTE FORCE COMPLEXITY ════════ -->
                  <template v-if="currentApproach === 'brute'">
                    <h3 class="ll-cx-heading">Brute Force &mdash; Complexity Analysis</h3>
                    <p class="ll-cx-intro">
                      Tries every station as a potential starting point and simulates the full
                      circular route from that station, tracking the running fuel balance.
                      If the tank never drops below zero, that station is the answer.
                    </p>

                    <h4 class="ll-cx-sub">Step-by-step Breakdown</h4>
                    <table class="ll-complexity-table">
                      <thead>
                        <tr><th>Operation</th><th>Time</th><th>Space</th><th>Reason</th></tr>
                      </thead>
                      <tbody>
                        <tr>
                          <td>Read input gas[] &amp; cost[]</td>
                          <td class="ll-cx-good">O(n)</td>
                          <td class="ll-cx-good">O(n)</td>
                          <td>Single pass to fill two arrays</td>
                        </tr>
                        <tr>
                          <td>Outer loop over start stations</td>
                          <td class="ll-cx-bad">O(n)</td>
                          <td class="ll-cx-good">O(1)</td>
                          <td>Try each of the n stations as start</td>
                        </tr>
                        <tr>
                          <td>Inner loop: simulate full circuit</td>
                          <td class="ll-cx-bad">O(n)</td>
                          <td class="ll-cx-good">O(1)</td>
                          <td>Each simulation visits up to n stations; combined O(n&sup2;)</td>
                        </tr>
                        <tr>
                          <td>Running fuel balance check</td>
                          <td class="ll-cx-good">O(1)</td>
                          <td class="ll-cx-good">O(1)</td>
                          <td>Simple comparison per step</td>
                        </tr>
                      </tbody>
                    </table>

                    <h4 class="ll-cx-sub">Overall Complexity</h4>
                    <div class="ll-cx-summary-grid">
                      <div class="ll-cx-card ll-cx-card-mid">
                        <div class="ll-cx-card-label">Time</div>
                        <div class="ll-cx-card-val">O(n&sup2;)</div>
                        <div class="ll-cx-card-note">Quadratic &mdash; outer &times; inner loop</div>
                      </div>
                      <div class="ll-cx-card ll-cx-card-good">
                        <div class="ll-cx-card-label">Space</div>
                        <div class="ll-cx-card-val">O(1)</div>
                        <div class="ll-cx-card-note">Only scalar variables used</div>
                      </div>
                      <div class="ll-cx-card ll-cx-card-good">
                        <div class="ll-cx-card-label">Optimal?</div>
                        <div class="ll-cx-card-val">YES</div>
                        <div class="ll-cx-card-note">Exhaustive &rarr; guaranteed correct</div>
                      </div>
                    </div>

                    <div class="ll-note">
                      <strong>When is Brute Force used?</strong> Only for very small inputs (n &le; 100).
                      For large circular route problems, the Greedy single-pass approach is the only practical choice.
                    </div>
                  </template>

                  <!-- ════════ GREEDY COMPLEXITY ════════ -->
                  <template v-else>
                    <h3 class="ll-cx-heading">Greedy Algorithm &mdash; Complexity Analysis</h3>
                    <p class="ll-cx-intro">
                      Maintains two running counters: <code>currTank</code> (local fuel since the
                      current candidate start) and <code>totalTank</code> (global fuel surplus).
                      When <code>currTank</code> drops below zero, the current candidate is
                      eliminated and the next station becomes the new candidate &mdash; all in one
                      left-to-right pass.
                    </p>

                    <h4 class="ll-cx-sub">Step-by-step Breakdown</h4>
                    <table class="ll-complexity-table">
                      <thead>
                        <tr><th>Operation</th><th>Time</th><th>Space</th><th>Reason</th></tr>
                      </thead>
                      <tbody>
                        <tr>
                          <td>Read input gas[] &amp; cost[]</td>
                          <td class="ll-cx-good">O(n)</td>
                          <td class="ll-cx-good">O(n)</td>
                          <td>Single pass to fill two arrays</td>
                        </tr>
                        <tr>
                          <td>Initialise <code>currTank</code>, <code>totalTank</code>, <code>startStation</code></td>
                          <td class="ll-cx-good">O(1)</td>
                          <td class="ll-cx-good">O(1)</td>
                          <td>Three scalar integer variables</td>
                        </tr>
                        <tr>
                          <td>Greedy single forward scan</td>
                          <td class="ll-cx-good">O(n)</td>
                          <td class="ll-cx-good">O(1)</td>
                          <td>One pass; balance update and range invalidation are O(1) per step</td>
                        </tr>
                        <tr>
                          <td>Global feasibility check</td>
                          <td class="ll-cx-good">O(1)</td>
                          <td class="ll-cx-good">O(1)</td>
                          <td>Single comparison of <code>totalTank &ge; 0</code> at the end</td>
                        </tr>
                      </tbody>
                    </table>

                    <h4 class="ll-cx-sub">Overall Complexity</h4>
                    <div class="ll-cx-summary-grid">
                      <div class="ll-cx-card ll-cx-card-good">
                        <div class="ll-cx-card-label">Time</div>
                        <div class="ll-cx-card-val">O(n)</div>
                        <div class="ll-cx-card-note">Single forward scan &mdash; optimal</div>
                      </div>
                      <div class="ll-cx-card ll-cx-card-good">
                        <div class="ll-cx-card-label">Space</div>
                        <div class="ll-cx-card-val">O(1)</div>
                        <div class="ll-cx-card-note">Only three scalar variables</div>
                      </div>
                      <div class="ll-cx-card ll-cx-card-good">
                        <div class="ll-cx-card-label">Optimal?</div>
                        <div class="ll-cx-card-val">YES</div>
                        <div class="ll-cx-card-note">Provable via Range Invalidation Theorem</div>
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
                          <td class="ll-cx-mid">O(n&sup2;)</td>
                          <td class="ll-cx-good">O(1)</td>
                          <td class="ll-cx-mid">Partial &mdash; quadratic for large n</td>
                        </tr>
                        <tr>
                          <td><strong>Greedy (Running Balance)</strong></td>
                          <td class="ll-cx-good">O(n)</td>
                          <td class="ll-cx-good">O(1)</td>
                          <td class="ll-cx-good">Yes &mdash; scales to millions</td>
                        </tr>
                      </tbody>
                    </table>

                    <div class="ll-note">
                      <strong>Key Insight:</strong> If the global sum of gas is at least the sum
                      of costs, a valid start is guaranteed to exist. When the local
                      <code>currTank</code> drops below zero, the Greedy Range Invalidation
                      Theorem lets us skip the entire failed subsegment in O(1) &mdash; making
                      one pass sufficient to find the unique answer.
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
  --shadow-sm: 0 1px 3px rgba(0,0,0,0.08), 0 1px 2px rgba(0,0,0,0.04);
  --radius: 8px;
  --radius-sm: 6px;

  display: flex;
  flex-direction: column;
  width: 100%;
  height: 75vh;
  background: var(--bg);
  color: var(--text);
  font-family: 'Segoe UI', system-ui, sans-serif;
  font-size: 12.5px;
  overflow: hidden;
}

@keyframes ll-pop {
  0% { transform: scale(0.85); opacity: 0; }
  100% { transform: scale(1); opacity: 1; }
}
@keyframes ll-pulse {
  0%, 100% { transform: scale(1); }
  50% { transform: scale(1.05); }
}

.slide-wrapper {
  width: 107%;
  max-height: 100%;
  margin-top: -10px;
  margin-left: -30px;
  font-size: 0.8rem;
  font-weight: 400;
}

.slide-body {
  display: flex;
  flex-direction: column;
  height: 100%;
  border-radius: 4px;
}

.navbar {
  display: flex;
  position: fixed;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
  width: 94.7%;
  padding: 0 10px;
  gap: 0.75rem;
  background-color: #ffffff;
}
.navbar > img { height: 30px; }
.navbar-title {
  color: white;
  margin: 0 0 0 -10px;
  padding: 2px 10px;
  background-color: #ef5050;
  border-radius: 5px;
  width: 80%;
  font-weight: 700;
  font-size: 1.35rem;
}

.row-main {
  width: 100%;
  height: 90%;
  margin-top: 36px;
  overflow: auto;
}

/* Control Toolbar */
.ll-toolbar {
  display: flex;
  align-items: center;
  gap: 6px;
  padding: 5px 12px;
  background: var(--surface);
  border-bottom: 1px solid var(--border);
  box-shadow: var(--shadow-sm);
  flex-shrink: 0;
  flex-wrap: wrap;
  margin-top: 4px;
}

.ll-approach-group {
  display: flex;
  background: var(--surface2);
  border: 1px solid var(--border);
  border-radius: var(--radius-sm);
  padding: 2px;
  gap: 2px;
}

.ll-approach-btn {
  padding: 4px 8px;
  border: none;
  background: transparent;
  color: var(--text2);
  font-size: 11px;
  font-weight: 600;
  border-radius: 4px;
  cursor: pointer;
  transition: all 0.15s;
  white-space: nowrap;
}
.ll-approach-btn:hover { color: var(--coral); }
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
  font-weight: 700;
  color: var(--muted);
}

.ll-text-input {
  padding: 3px 6px;
  border: 1px solid var(--border2);
  border-radius: var(--radius-sm);
  font-family: monospace;
  font-size: 11px;
  color: var(--text);
  background: var(--surface);
}
.ll-text-input:focus {
  outline: none;
  border-color: var(--coral);
  box-shadow: 0 0 0 3px rgba(240,77,77,0.1);
}

.ll-viz-btn {
  padding: 5px 12px;
  border: none;
  border-radius: var(--radius-sm);
  background: var(--coral);
  color: #fff;
  font-size: 11.5px;
  font-weight: 600;
  cursor: pointer;
  transition: filter 0.15s;
  box-shadow: var(--shadow-sm);
}
.ll-viz-btn:hover { filter: brightness(1.08); }

.ll-nav-controls {
  display: flex;
  align-items: center;
  gap: 4px;
  margin-left: auto;
  flex-shrink: 0;
  flex-wrap: wrap;
}

.ll-nav-btn {
  padding: 4px 9px;
  border: 1px solid var(--border2);
  border-radius: var(--radius-sm);
  background: var(--surface2);
  color: var(--text2);
  font-size: 11px;
  font-weight: 500;
  cursor: pointer;
  transition: all 0.15s;
  white-space: nowrap;
}
.ll-nav-btn:hover {
  border-color: var(--coral);
  color: var(--coral);
  background: var(--surface);
}

.ll-play-btn {
  padding: 4px 9px;
  border: 1px solid var(--blue);
  border-radius: var(--radius-sm);
  background: var(--blue-light);
  color: var(--blue);
  font-size: 11px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.15s;
  min-width: 68px;
}
.ll-play-btn:hover {
  background: var(--blue);
  color: #fff;
}

/* Main Split View */
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
  min-width: 240px;
  max-width: 75%;
}

.ll-right-col {
  display: flex;
  flex-direction: column;
  flex: 1;
  overflow: hidden;
  min-width: 0;
}

/* Stepping Stone Array Banner */
.ll-house-banner {
  padding: 4px 10px;
  background: var(--surface2);
  border-bottom: 1px solid var(--border);
  flex-shrink: 0;
}
.ll-house-title {
  font-size: 10.5px;
  font-weight: 700;
  color: var(--text2);
  margin-bottom: 3px;
}
.ll-dual-arrays-wrap {
  display: flex;
  flex-direction: column;
  gap: 3px;
}
.ll-arr-row {
  display: flex;
  align-items: center;
  gap: 6px;
}
.ll-arr-tag {
  font-family: monospace;
  font-size: 10.5px;
  font-weight: 700;
  color: var(--coral);
  width: 48px;
  flex-shrink: 0;
}
.ll-arr-cells {
  display: flex;
  gap: 6px;
  overflow-x: auto;
  padding: 1px 0;
}
.ll-arr-cell {
  width: 38px;
  height: 34px;
  background: var(--surface);
  border: 1.5px solid var(--border2);
  border-radius: var(--radius-sm);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  box-shadow: var(--shadow-sm);
  transition: all 0.2s ease;
  flex-shrink: 0;
}
.ll-cell-idx {
  font-size: 8px;
  color: var(--muted);
  font-family: monospace;
}
.ll-cell-val {
  font-size: 12.5px;
  font-weight: 800;
  font-family: monospace;
  color: var(--text);
}
.ll-cell-active {
  border-color: #f59e0b !important;
  background: #fffbeb !important;
  box-shadow: 0 0 0 2.5px rgba(245,158,11,0.25) !important;
  transform: translateY(-1px);
}
.ll-cell-reach {
  border-color: #10b981 !important;
}
.ll-cell-goal {
  border-color: #9333ea !important;
}

/* Stats Chips Bar */
.ll-ptrs {
  display: flex;
  align-items: center;
  gap: 5px;
  padding: 4px 10px;
  background: var(--surface2);
  border-bottom: 1px solid var(--border);
  min-height: 26px;
  flex-shrink: 0;
  flex-wrap: wrap;
  box-sizing: border-box;
}
.ll-ptr-chip {
  font-size: 10.5px;
  font-family: monospace;
  background: var(--surface);
  border: 1px solid var(--border);
  padding: 2px 7px;
  border-radius: var(--radius-sm);
  box-shadow: var(--shadow-sm);
  white-space: nowrap;
}
.ll-c-blue { color: var(--blue); }
.ll-c-orange { color: var(--orange); }
.ll-c-green { color: var(--green); }
.ll-c-purple { color: var(--purple); }
.ll-c-red { color: var(--red); }

/* Visualization Viewport */
.ll-viz-wrap {
  position: relative;
  background: var(--surface);
  border-bottom: 1px solid var(--border);
  overflow: hidden;
  flex-shrink: 0;
  display: flex;
  flex-direction: column;
}

.ll-timeline-container {
  display: flex;
  flex-direction: column;
  width: 100%;
  height: 100%;
  overflow: hidden;
}

.ll-section-caption {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 3px 10px;
  background: var(--surface2);
  border-top: 1px solid var(--border);
  border-bottom: 1px solid var(--border);
  font-size: 10.5px;
  font-weight: 700;
  color: var(--text2);
}

.ll-calc-pill {
  font-size: 10px;
  font-weight: 700;
  font-family: monospace;
  padding: 1px 6px;
  border-radius: 10px;
  background: #dcfce7;
  color: #15803d;
}

.ll-jump-track-scroll {
  flex: 1;
  width: 100%;
  overflow-x: auto;
  overflow-y: auto;
  padding: 6px 12px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.ll-track-board {
  display: flex;
  flex-direction: column !important;
  align-items: center;
  justify-content: flex-start;
  margin: 0 auto;
  padding: 4px 0 8px;
}

/* Modern Greedy Jump Track Styles */
.ll-arc-svg-wrap {
  height: 42px;
  display: flex;
  justify-content: center;
  align-items: flex-end;
  width: 100%;
}

.ll-arc-svg {
  display: block;
  overflow: visible;
}

.ll-arc-txt {
  font-family: Consolas, 'Fira Code', monospace !important;
  font-size: 9.5px !important;
  font-weight: 700 !important;
  fill: #b45309 !important;
  user-select: none !important;
}

.ll-stones-track {
  display: flex;
  flex-direction: row;
  justify-content: flex-start;
  align-items: flex-start;
  gap: 16px;
  padding: 0;
}

.ll-stone-pod {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 52px;
  flex-shrink: 0;
}

.ll-pod-pointer {
  height: 18px;
  display: flex;
  align-items: flex-end;
  justify-content: center;
  margin-bottom: 3px;
  width: 100%;
}

.ll-pin {
  font-family: Consolas, 'Fira Code', monospace;
  font-size: 8px;
  font-weight: 800;
  padding: 1px 4px;
  border-radius: 3px;
  letter-spacing: 0.3px;
  line-height: 1.1;
  text-align: center;
  white-space: nowrap;
}

.ll-pin-active {
  color: #b45309;
  background: #fef3c7;
  border: 1px solid #f59e0b;
}

.ll-pin-goal {
  color: #6b21a8;
  background: #f3e8ff;
  border: 1px solid #9333ea;
}

.ll-pin-space {
  visibility: hidden;
}

.ll-stone-card {
  width: 52px;
  height: 48px;
  background: var(--surface);
  border: 1.5px solid var(--border2);
  border-radius: 6px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  box-shadow: var(--shadow-sm);
  transition: all 0.2s ease;
  position: relative;
}

.ll-stone-card-idx {
  font-family: Consolas, monospace;
  font-size: 8.5px;
  color: var(--muted);
  font-weight: 600;
  line-height: 1;
  margin-bottom: 1px;
}

.ll-stone-card-val {
  font-family: Consolas, monospace;
  font-size: 15px;
  font-weight: 800;
  color: var(--text);
  line-height: 1.1;
}

.ll-stone-card-sub {
  font-family: Consolas, monospace;
  font-size: 7.5px;
  color: var(--muted);
  font-weight: 600;
  line-height: 1;
  margin-top: 1px;
}

/* Pod states */
.ll-pod-active .ll-stone-card {
  border-color: #f59e0b !important;
  background: #fffbeb !important;
  box-shadow: 0 0 0 3px rgba(245, 158, 11, 0.25) !important;
  transform: translateY(-2px);
}
.ll-pod-active .ll-stone-card-val {
  color: #b45309 !important;
}

.ll-pod-window .ll-stone-card {
  border-color: #9333ea !important;
}

.ll-pod-reach .ll-stone-card {
  border-color: #10b981 !important;
  background: #f0fdf4 !important;
}
.ll-pod-reach .ll-stone-card-val {
  color: #065f46 !important;
}

.ll-pod-goal .ll-stone-card {
  border-color: #9333ea !important;
  background: #faf5ff !important;
}
.ll-pod-goal .ll-stone-card-val {
  color: #6b21a8 !important;
}

/* Bottom barrier */
.ll-pod-barrier {
  height: 22px;
  display: flex;
  justify-content: center;
  align-items: flex-start;
  margin-top: 3px;
  width: 100%;
}

.ll-barrier-tag {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.ll-barrier-arrow {
  color: #9333ea;
  font-size: 7.5px;
  line-height: 1;
}
.ll-arrow-blue {
  color: #2563eb !important;
}

.ll-barrier-lbl {
  font-family: Consolas, monospace;
  font-size: 8px;
  font-weight: 700;
  color: #ffffff;
  background: #9333ea;
  border-radius: 3px;
  padding: 1px 4px;
  white-space: nowrap;
}
.ll-lbl-blue {
  background: #2563eb !important;
}

/* Horizon progress bar */
.ll-horizon-line-wrap {
  width: 100%;
  display: flex;
  justify-content: center;
  padding: 4px 0 0;
}

.ll-horizon-track {
  width: 100%;
  height: 4px;
  background: #e2e8f0;
  border-radius: 2px;
  overflow: hidden;
}

.ll-horizon-active {
  height: 100%;
  background: #10b981;
  transition: width 0.25s ease;
}

.ll-arc-jump {
  animation: ll-pop 0.3s ease;
}

/* Standard Legend Strip */
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

/* Tree view for Brute Force */
.ll-tree-container {
  display: flex;
  flex-direction: column;
  width: 100%;
  height: 100%;
}

.ll-tree-scroll-area {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-start;
  flex: 1;
  padding: 4px 6px;
  overflow: auto;
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

/* Resizers */
.ll-resizer {
  width: 5px;
  cursor: col-resize;
  background: var(--border);
  transition: background .15s;
  flex-shrink: 0;
  position: relative;
  z-index: 20;
}
.ll-resizer:hover, .ll-resizer.drag { background: var(--coral); }

.ll-vresizer {
  height: 5px;
  cursor: row-resize;
  background: var(--border);
  transition: background .15s;
  flex-shrink: 0;
  position: relative;
  z-index: 20;
}
.ll-vresizer:hover, .ll-vresizer.drag { background: var(--coral); }

/* Call Stack & Environment Panel */
.ll-table-area {
  padding: 4px 10px;
  background: var(--surface);
  border-bottom: 1px solid var(--border);
  flex-shrink: 0;
  overflow: auto;
  box-sizing: border-box;
}

.ll-table-title {
  font-size: 9.5px;
  color: var(--muted);
  margin-bottom: 2px;
  font-style: italic;
}

.ll-stack-line {
  font-family: Consolas, monospace;
  font-size: 11px;
  line-height: 1.6;
}

.ll-frame {
  color: var(--text2);
  font-size: 10.5px;
  padding: 1px 0;
  white-space: nowrap;
}

.ll-fname { color: var(--text2); }
.ll-now { color: var(--orange); margin-left: 4px; font-size: 9.5px; }

/* Step Badge Wrap */
.ll-badge-wrap {
  display: flex;
  align-items: center;
  padding: 4px 8px;
  background: var(--surface);
  border-bottom: 1px solid var(--border);
  min-height: 30px;
  flex-shrink: 0;
}

.ll-badge {
  display: inline-block;
  padding: 3px 10px;
  font-size: 10.5px;
  line-height: 1.35;
  font-weight: 500;
  color: var(--coral-dark);
  background: var(--coral-light);
  border-left: 3px solid var(--coral);
  border-radius: var(--radius-sm);
  word-break: break-word;
}
.ll-badge-error {
  border-left-color: var(--red);
  background: var(--red-light);
  color: var(--red-dark);
  font-weight: 600;
}
.ll-badge-success {
  border-left-color: var(--green);
  background: var(--green-light);
  color: #166534;
  font-weight: 600;
}

/* Right Panel Styles */
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
  border: 1px solid var(--border2);
  border-radius: var(--radius-sm);
  background: var(--surface2);
  color: var(--text2);
  font-size: 10.5px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.15s;
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
  transition: border-color 0.15s;
  min-width: 95px;
}
.ll-lang-select:focus {
  outline: none;
  border-color: var(--coral);
  box-shadow: 0 0 0 3px rgba(240,77,77,0.1);
}

.ll-code-scroll {
  flex: 1;
  overflow: auto;
  padding: 10px 14px;
  background: #f8fafc;
  min-width: 0;
}

.ll-pre {
  margin: 0;
  font-family: 'Cascadia Code', 'Fira Code', 'Consolas', monospace;
  font-size: 11px;
  line-height: 1.5;
  color: var(--text);
  white-space: pre;
  padding-bottom: 100px;
}

.ll-codeline {
  display: block;
  padding: 0 14px;
  margin: 0 -14px;
}
.ll-hl {
  background: #dcfce7;
  border-left: 3px solid var(--green);
  color: #15803d;
  font-weight: 600;
  border-radius: 3px;
}

/* Time & Space Complexity Tab */
.ll-info-scroll {
  flex: 1;
  overflow: auto;
  padding: 12px 16px;
  font-size: 12px;
  line-height: 1.55;
  color: var(--text2);
  background: var(--surface);
}

.ll-info-scroll h3 {
  font-size: 13px;
  font-weight: 700;
  color: var(--text);
  margin: 0 0 6px;
}
.ll-info-scroll h3:not(:first-child) { margin-top: 14px; }

.ll-info-scroll h4 {
  font-size: 12px;
  font-weight: 700;
  color: var(--text);
  margin: 10px 0 4px;
}

.ll-info-scroll p { margin: 0 0 6px; }
.ll-info-scroll ul { margin: 0 0 10px; padding-left: 18px; }
.ll-info-scroll li { margin-bottom: 4px; }

.ll-complexity-table {
  width: 100%;
  border-collapse: collapse;
  margin: 8px 0 12px;
  font-size: 11px;
}
.ll-complexity-table th, .ll-complexity-table td {
  border: 1px solid var(--border);
  padding: 5px 8px;
  text-align: left;
}
.ll-complexity-table th {
  background: var(--surface2);
  font-weight: 700;
  color: var(--text);
}

.ll-math-box {
  background: var(--surface2);
  border-left: 3px solid var(--blue);
  border-radius: var(--radius-sm);
  padding: 6px 12px;
  font-family: monospace;
  font-size: 11.5px;
  color: var(--text);
  margin: 6px 0;
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
  flex-shrink: 0;
}

.ll-speed-wrap {
  display: flex;
  align-items: center;
  gap: 6px;
}
.ll-speed-wrap input {
  width: 80px;
  accent-color: var(--coral);
}

/* Gas station specific additions */
.ll-c-muted { color: var(--muted) !important; font-style: italic; }
.ll-c-neutral { color: var(--text2) !important; opacity: 0.65; }

.ll-pod-deficit .ll-stone-card {
  border-color: #ef4444 !important;
  background: #fef2f2 !important;
  box-shadow: 0 0 0 2.5px rgba(239, 68, 68, 0.25) !important;
}
.ll-pod-deficit .ll-stone-card-val {
  color: #dc2626 !important;
}
.ll-pill-error {
  font-size: 10px;
  font-weight: 700;
  font-family: monospace;
  padding: 1px 6px;
  border-radius: 10px;
  background: #fee2e2;
  color: #991b1b;
}
</style>
