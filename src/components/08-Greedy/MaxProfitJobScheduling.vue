<script setup>
import { ref, reactive, computed, onMounted, onUnmounted, watch } from 'vue';

const props = defineProps({
  topic:    { type: String, default: 'Greedy Algorithms' },
  subTopic: { type: String, default: 'Maximum Profit in Job Scheduling' }
});

const APPROACHES = [
  { id: 'brute', label: 'Brute Force', desc: 'O(2^n) — Exhaustive Recursive Subset Search' },
  { id: 'dp',    label: 'Greedy', desc: 'O(n log n) — Sort by endTime + DP + Binary Search' }
];

const CODES = {
  brute: {
    java: [
      ['',              'import java.util.Scanner;'],
      ['',              ''],
      ['',              'public class Main {'],
      ['',              '    static int maxProfit = 0;'],
      ['',              ''],
      ['c_entry',       '    static void solve(int[] s, int[] e, int[] p, int n, int idx, int lastEnd, int profit) {'],
      ['c_base_check',  '        if (idx == n) {'],
      ['c_update_check','            if (profit > maxProfit) {'],
      ['c_update_max',  '                maxProfit = profit;'],
      ['',              '            }'],
      ['c_ret_base',    '            return;'],
      ['',              '        }'],
      ['c_call_skip',   '        solve(s, e, p, n, idx + 1, lastEnd, profit);'],
      ['c_check_compat','        if (s[idx] >= lastEnd) {'],
      ['c_call_pick',   '            solve(s, e, p, n, idx + 1, e[idx], profit + p[idx]);'],
      ['',              '        }'],
      ['',              '    }'],
      ['',              ''],
      ['',              '    public static void main(String[] args) {'],
      ['m_scanner',     '        Scanner sc = new Scanner(System.in);'],
      ['m_read_n',      '        int n = sc.nextInt();'],
      ['m_alloc_s',     '        int[] s = new int[n];'],
      ['m_alloc_e',     '        int[] e = new int[n];'],
      ['m_alloc_p',     '        int[] p = new int[n];'],
      ['m_for_input',   '        for (int i = 0; i < n; i++) {'],
      ['m_read_s',      '            s[i] = sc.nextInt();'],
      ['m_read_e',      '            e[i] = sc.nextInt();'],
      ['m_read_p',      '            p[i] = sc.nextInt();'],
      ['',              '        }'],
      ['m_call_solve',  '        solve(s, e, p, n, 0, 0, 0);'],
      ['m_print_ans',   '        System.out.println(maxProfit);'],
      ['m_done',        '    }'],
      ['',              '}']
    ],
    c: [
      ['',              '#include <stdio.h>'],
      ['',              ''],
      ['',              'int maxProfit = 0;'],
      ['',              ''],
      ['c_entry',       'void solve(int s[], int e[], int p[], int n, int idx, int lastEnd, int profit) {'],
      ['c_base_check',  '    if (idx == n) {'],
      ['c_update_check','        if (profit > maxProfit) {'],
      ['c_update_max',  '            maxProfit = profit;'],
      ['',              '        }'],
      ['c_ret_base',    '        return;'],
      ['',              '    }'],
      ['c_call_skip',   '    solve(s, e, p, n, idx + 1, lastEnd, profit);'],
      ['c_check_compat','    if (s[idx] >= lastEnd) {'],
      ['c_call_pick',   '        solve(s, e, p, n, idx + 1, e[idx], profit + p[idx]);'],
      ['',              '    }'],
      ['',              '}'],
      ['',              ''],
      ['',              'int main() {'],
      ['m_scanner',     '    int n;'],
      ['m_read_n',      '    scanf("%d", &n);'],
      ['m_alloc_s',     '    int s[100];'],
      ['m_alloc_e',     '    int e[100];'],
      ['m_alloc_p',     '    int p[100];'],
      ['m_for_input',   '    for (int i = 0; i < n; i++) {'],
      ['m_read_s',      '        scanf("%d", &s[i]);'],
      ['m_read_e',      '        scanf("%d", &e[i]);'],
      ['m_read_p',      '        scanf("%d", &p[i]);'],
      ['',              '    }'],
      ['m_call_solve',  '    solve(s, e, p, n, 0, 0, 0);'],
      ['m_print_ans',   '    printf("%d\\n", maxProfit);'],
      ['m_done',        '    return 0;'],
      ['',              '}']
    ],
    cpp: [
      ['',              '#include <iostream>'],
      ['',              '#include <vector>'],
      ['',              'using namespace std;'],
      ['',              ''],
      ['',              'int maxProfit = 0;'],
      ['',              ''],
      ['c_entry',       'void solve(vector<int>& s, vector<int>& e, vector<int>& p, int n, int idx, int lastEnd, int profit) {'],
      ['c_base_check',  '    if (idx == n) {'],
      ['c_update_check','        if (profit > maxProfit) {'],
      ['c_update_max',  '            maxProfit = profit;'],
      ['',              '        }'],
      ['c_ret_base',    '        return;'],
      ['',              '    }'],
      ['c_call_skip',   '    solve(s, e, p, n, idx + 1, lastEnd, profit);'],
      ['c_check_compat','    if (s[idx] >= lastEnd) {'],
      ['c_call_pick',   '        solve(s, e, p, n, idx + 1, e[idx], profit + p[idx]);'],
      ['',              '    }'],
      ['',              '}'],
      ['',              ''],
      ['',              'int main() {'],
      ['m_scanner',     '    int n;'],
      ['m_read_n',      '    cin >> n;'],
      ['m_alloc_s',     '    vector<int> s(n);'],
      ['m_alloc_e',     '    vector<int> e(n);'],
      ['m_alloc_p',     '    vector<int> p(n);'],
      ['m_for_input',   '    for (int i = 0; i < n; i++) {'],
      ['m_read_s',      '        cin >> s[i];'],
      ['m_read_e',      '        cin >> e[i];'],
      ['m_read_p',      '        cin >> p[i];'],
      ['',              '    }'],
      ['m_call_solve',  '    solve(s, e, p, n, 0, 0, 0);'],
      ['m_print_ans',   '    cout << maxProfit << endl;'],
      ['m_done',        '    return 0;'],
      ['',              '}']
    ],
    python: [
      ['',              'import sys'],
      ['',              ''],
      ['',              'max_profit = 0'],
      ['',              ''],
      ['c_entry',       'def solve(s, e, p, n, idx, last_end, profit):'],
      ['',              '    global max_profit'],
      ['c_base_check',  '    if idx == n:'],
      ['c_update_check','        if profit > max_profit:'],
      ['c_update_max',  '            max_profit = profit'],
      ['c_ret_base',    '        return'],
      ['c_call_skip',   '    solve(s, e, p, n, idx + 1, last_end, profit)'],
      ['c_check_compat','    if s[idx] >= last_end:'],
      ['c_call_pick',   '        solve(s, e, p, n, idx + 1, e[idx], profit + p[idx])'],
      ['',              ''],
      ['',              'def main():'],
      ['m_scanner',     '    tokens = sys.stdin.read().split()'],
      ['m_read_n',      '    if not tokens: return'],
      ['',              '    n = int(tokens[0])'],
      ['m_alloc_s',     '    s = []'],
      ['m_alloc_e',     '    e = []'],
      ['m_alloc_p',     '    p = []'],
      ['m_for_input',   '    idx = 1'],
      ['',              '    for i in range(n):'],
      ['m_read_s',      '        s.append(int(tokens[idx]))'],
      ['m_read_e',      '        e.append(int(tokens[idx + 1]))'],
      ['m_read_p',      '        p.append(int(tokens[idx + 2]))'],
      ['',              '        idx += 3'],
      ['m_call_solve',  '    solve(s, e, p, n, 0, 0, 0)'],
      ['m_print_ans',   '    print(max_profit)'],
      ['m_done',        '    return'],
      ['',              ''],
      ['',              'if __name__ == "__main__":'],
      ['',              '    main()']
    ]
  },
  dp: {
    java: [
      ['',              'import java.util.Scanner;'],
      ['',              'import java.util.Arrays;'],
      ['',              ''],
      ['',              'public class Main {'],
      ['',              ''],
      ['c_entry',       '    static int maxProfitDP(int[] s, int[] e, int[] p, int n) {'],
      ['c_sort_init',   '        Integer[] idx = new Integer[n];'],
      ['c_sort_fill',   '        for (int i = 0; i < n; i++) {'],
      ['c_sort_body',   '            idx[i] = i;'],
      ['',              '        }'],
      ['c_sort_exec',   '        Arrays.sort(idx, (a, b) -> Integer.compare(e[a], e[b]));'],
      ['c_dp_alloc',    '        int[] dp = new int[n + 1];'],
      ['c_for_loop',    '        for (int i = 0; i < n; i++) {'],
      ['c_bsearch',     '            int lo = 0, hi = i - 1, j = -1;'],
      ['c_bsearch_loop','            while (lo <= hi) {'],
      ['c_bsearch_mid', '                int mid = (lo + hi) / 2;'],
      ['c_bsearch_cmp', '                if (e[idx[mid]] <= s[idx[i]]) {'],
      ['c_bsearch_hit', '                    j = mid;'],
      ['',              '                    lo = mid + 1;'],
      ['',              '                } else {'],
      ['',              '                    hi = mid - 1;'],
      ['',              '                }'],
      ['',              '            }'],
      ['c_dp_take',     '            int take = p[idx[i]] + (j >= 0 ? dp[j + 1] : 0);'],
      ['c_dp_skip',     '            int skip = dp[i];'],
      ['c_dp_assign',   '            dp[i + 1] = Math.max(take, skip);'],
      ['',              '        }'],
      ['c_ret_dp',      '        return dp[n];'],
      ['',              '    }'],
      ['',              ''],
      ['',              '    public static void main(String[] args) {'],
      ['m_scanner',     '        Scanner sc = new Scanner(System.in);'],
      ['m_read_n',      '        int n = sc.nextInt();'],
      ['m_alloc_s',     '        int[] s = new int[n];'],
      ['m_alloc_e',     '        int[] e = new int[n];'],
      ['m_alloc_p',     '        int[] p = new int[n];'],
      ['m_for_input',   '        for (int i = 0; i < n; i++) {'],
      ['m_read_s',      '            s[i] = sc.nextInt();'],
      ['m_read_e',      '            e[i] = sc.nextInt();'],
      ['m_read_p',      '            p[i] = sc.nextInt();'],
      ['',              '        }'],
      ['m_call_dp',     '        int ans = maxProfitDP(s, e, p, n);'],
      ['m_print_ans',   '        System.out.println(ans);'],
      ['m_done',        '    }'],
      ['',              '}']
    ],
    c: [
      ['',              '#include <stdio.h>'],
      ['',              '#include <stdlib.h>'],
      ['',              ''],
      ['',              'int eGlobal[100];'],
      ['',              'int cmp(const void* a, const void* b) {'],
      ['',              '    return eGlobal[*(int*)a] - eGlobal[*(int*)b];'],
      ['',              '}'],
      ['',              ''],
      ['c_entry',       'int maxProfitDP(int s[], int e[], int p[], int n) {'],
      ['c_sort_init',   '    int idx[100];'],
      ['c_sort_fill',   '    for (int i = 0; i < n; i++) {'],
      ['c_sort_body',   '        idx[i] = i;'],
      ['',              '        eGlobal[i] = e[i];'],
      ['',              '    }'],
      ['c_sort_exec',   '    qsort(idx, n, sizeof(int), cmp);'],
      ['c_dp_alloc',    '    int dp[101] = {0};'],
      ['c_for_loop',    '    for (int i = 0; i < n; i++) {'],
      ['c_bsearch',     '        int lo = 0, hi = i - 1, j = -1;'],
      ['c_bsearch_loop','        while (lo <= hi) {'],
      ['c_bsearch_mid', '            int mid = (lo + hi) / 2;'],
      ['c_bsearch_cmp', '            if (e[idx[mid]] <= s[idx[i]]) {'],
      ['c_bsearch_hit', '                j = mid;'],
      ['',              '                lo = mid + 1;'],
      ['',              '            } else {'],
      ['',              '                hi = mid - 1;'],
      ['',              '            }'],
      ['',              '        }'],
      ['c_dp_take',     '        int take = p[idx[i]] + (j >= 0 ? dp[j + 1] : 0);'],
      ['c_dp_skip',     '        int skip = dp[i];'],
      ['c_dp_assign',   '        dp[i + 1] = take > skip ? take : skip;'],
      ['',              '    }'],
      ['c_ret_dp',      '    return dp[n];'],
      ['',              '}'],
      ['',              ''],
      ['',              'int main() {'],
      ['m_scanner',     '    int n;'],
      ['m_read_n',      '    scanf("%d", &n);'],
      ['m_alloc_s',     '    int s[100];'],
      ['m_alloc_e',     '    int e[100];'],
      ['m_alloc_p',     '    int p[100];'],
      ['m_for_input',   '    for (int i = 0; i < n; i++) {'],
      ['m_read_s',      '        scanf("%d", &s[i]);'],
      ['m_read_e',      '        scanf("%d", &e[i]);'],
      ['m_read_p',      '        scanf("%d", &p[i]);'],
      ['',              '    }'],
      ['m_call_dp',     '    int ans = maxProfitDP(s, e, p, n);'],
      ['m_print_ans',   '    printf("%d\\n", ans);'],
      ['m_done',        '    return 0;'],
      ['',              '}']
    ],
    cpp: [
      ['',              '#include <iostream>'],
      ['',              '#include <vector>'],
      ['',              '#include <algorithm>'],
      ['',              'using namespace std;'],
      ['',              ''],
      ['c_entry',       'int maxProfitDP(vector<int>& s, vector<int>& e, vector<int>& p, int n) {'],
      ['c_sort_init',   '    vector<int> idx(n);'],
      ['c_sort_fill',   '    for (int i = 0; i < n; i++) {'],
      ['c_sort_body',   '        idx[i] = i;'],
      ['',              '    }'],
      ['c_sort_exec',   '    sort(idx.begin(), idx.end(), [&](int a, int b) { return e[a] < e[b]; });'],
      ['c_dp_alloc',    '    vector<int> dp(n + 1, 0);'],
      ['c_for_loop',    '    for (int i = 0; i < n; i++) {'],
      ['c_bsearch',     '        int lo = 0, hi = i - 1, j = -1;'],
      ['c_bsearch_loop','        while (lo <= hi) {'],
      ['c_bsearch_mid', '            int mid = (lo + hi) / 2;'],
      ['c_bsearch_cmp', '            if (e[idx[mid]] <= s[idx[i]]) {'],
      ['c_bsearch_hit', '                j = mid;'],
      ['',              '                lo = mid + 1;'],
      ['',              '            } else {'],
      ['',              '                hi = mid - 1;'],
      ['',              '            }'],
      ['',              '        }'],
      ['c_dp_take',     '        int take = p[idx[i]] + (j >= 0 ? dp[j + 1] : 0);'],
      ['c_dp_skip',     '        int skip = dp[i];'],
      ['c_dp_assign',   '        dp[i + 1] = max(take, skip);'],
      ['',              '    }'],
      ['c_ret_dp',      '    return dp[n];'],
      ['',              '}'],
      ['',              ''],
      ['',              'int main() {'],
      ['m_scanner',     '    int n;'],
      ['m_read_n',      '    cin >> n;'],
      ['m_alloc_s',     '    vector<int> s(n);'],
      ['m_alloc_e',     '    vector<int> e(n);'],
      ['m_alloc_p',     '    vector<int> p(n);'],
      ['m_for_input',   '    for (int i = 0; i < n; i++) {'],
      ['m_read_s',      '        cin >> s[i];'],
      ['m_read_e',      '        cin >> e[i];'],
      ['m_read_p',      '        cin >> p[i];'],
      ['',              '    }'],
      ['m_call_dp',     '    int ans = maxProfitDP(s, e, p, n);'],
      ['m_print_ans',   '    cout << ans << endl;'],
      ['m_done',        '    return 0;'],
      ['',              '}']
    ],
    python: [
      ['',              'import sys'],
      ['',              ''],
      ['c_entry',       'def max_profit_dp(s, e, p, n):'],
      ['c_sort_init',   '    idx = list(range(n))'],
      ['c_sort_exec',   '    idx.sort(key=lambda i: e[i])'],
      ['c_dp_alloc',    '    dp = [0] * (n + 1)'],
      ['c_for_loop',    '    for i in range(n):'],
      ['c_bsearch',     '        lo, hi, j = 0, i - 1, -1'],
      ['c_bsearch_loop','        while lo <= hi:'],
      ['c_bsearch_mid', '            mid = (lo + hi) // 2'],
      ['c_bsearch_cmp', '            if e[idx[mid]] <= s[idx[i]]:'],
      ['c_bsearch_hit', '                j = mid'],
      ['',              '                lo = mid + 1'],
      ['',              '            else:'],
      ['',              '                hi = mid - 1'],
      ['c_dp_take',     '        take = p[idx[i]] + (dp[j + 1] if j >= 0 else 0)'],
      ['c_dp_skip',     '        skip = dp[i]'],
      ['c_dp_assign',   '        dp[i + 1] = max(take, skip)'],
      ['c_ret_dp',      '    return dp[n]'],
      ['',              ''],
      ['',              'def main():'],
      ['m_scanner',     '    tokens = sys.stdin.read().split()'],
      ['m_read_n',      '    if not tokens: return'],
      ['',              '    n = int(tokens[0])'],
      ['m_alloc_s',     '    s = []'],
      ['m_alloc_e',     '    e = []'],
      ['m_alloc_p',     '    p = []'],
      ['m_for_input',   '    idx = 1'],
      ['',              '    for i in range(n):'],
      ['m_read_s',      '        s.append(int(tokens[idx]))'],
      ['m_read_e',      '        e.append(int(tokens[idx + 1]))'],
      ['m_read_p',      '        p.append(int(tokens[idx + 2]))'],
      ['',              '        idx += 3'],
      ['m_call_dp',     '    ans = max_profit_dp(s, e, p, n)'],
      ['m_print_ans',   '    print(ans)'],
      ['m_done',        '    return'],
      ['',              ''],
      ['',              'if __name__ == "__main__":'],
      ['',              '    main()']
    ]
  }
};

const PSEUDOCODES = {
  brute: [
    '// \u2500\u2500\u2500 Brute Force: Exhaustive Recursive Subset Search \u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500',
    'maxProfit = 0',
    '',
    'function solve(s[], e[], p[], n, idx, lastEnd, profit):',
    '',
    '    if idx == n:',
    '        if profit > maxProfit:',
    '            maxProfit = profit',
    '        return',
    '',
    '    // Choice 1: SKIP job idx',
    '    solve(s, e, p, n, idx + 1, lastEnd, profit)',
    '',
    '    // Choice 2: TAKE job idx (if compatible)',
    '    if s[idx] >= lastEnd:',
    '        solve(s, e, p, n, idx + 1, e[idx], profit + p[idx])',
    '',
    '// Time: O(2^n) | Space: O(n)'
  ],
  dp: [
    '// \u2500\u2500\u2500 DP + Binary Search: Weighted Job Scheduling \u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500',
    'function maxProfitDP(s[], e[], p[], n):',
    '',
    '    idx = [0, 1, ..., n-1]',
    '    sort idx by e[idx[i]] ascending',
    '',
    '    dp[0..n] = 0',
    '',
    '    for i = 0 to n - 1:',
    '        // Binary search: latest j where e[idx[j]] <= s[idx[i]]',
    '        lo = 0,  hi = i - 1,  j = -1',
    '        while lo <= hi:',
    '            mid = (lo + hi) / 2',
    '            if e[idx[mid]] <= s[idx[i]]: j = mid; lo = mid + 1',
    '            else: hi = mid - 1',
    '',
    '        take = p[idx[i]] + (dp[j+1] if j >= 0 else 0)',
    '        skip = dp[i]',
    '        dp[i+1] = max(take, skip)',
    '',
    '    return dp[n]',
    '',
    '// Time: O(n log n) | Space: O(n)'
  ]
};

const DEFAULT_STARTS  = [1, 2, 3, 3];
const DEFAULT_ENDS    = [3, 4, 5, 6];
const DEFAULT_PROFITS = [50, 10, 40, 70];

function frame(title, rows) { return { title, rows }; }

function buildSteps(approach, rawStarts, rawEnds, rawProfits) {
  const steps   = [];
  const starts  = (rawStarts  && rawStarts.length)  ? [...rawStarts]  : [...DEFAULT_STARTS];
  const ends    = (rawEnds    && rawEnds.length)    ? [...rawEnds]    : [...DEFAULT_ENDS];
  const profits = (rawProfits && rawProfits.length) ? [...rawProfits] : [...DEFAULT_PROFITS];
  const n = Math.min(starts.length, ends.length, profits.length);
  starts.length = ends.length = profits.length = n;
  const jobs = Array.from({length: n}, (_, i) => ({ id: i, start: starts[i], end: ends[i], profit: profits[i] }));

  if (approach === 'brute') {
    let maxSelected = 0, bestSubset = [];
    const allNodes = [];
    let nid = 0;

    function bTree(idx, lastEnd, profit, parentId, picked) {
      const nodeId = nid++;
      const node = { id: nodeId, idx, lastEnd, profit, parentId, picked: [...picked], children: [], isLeaf: false };
      allNodes.push(node);
      if (parentId !== null) { const par = allNodes.find(x => x.id === parentId); if (par) par.children.push(nodeId); }
      if (idx === n) {
        node.isLeaf = true;
        if (profit > maxSelected) { maxSelected = profit; bestSubset = [...picked]; }
        return nodeId;
      }
      bTree(idx + 1, lastEnd, profit, nodeId, picked);
      if (starts[idx] >= lastEnd) bTree(idx + 1, ends[idx], profit + profits[idx], nodeId, [...picked, idx]);
      return nodeId;
    }
    bTree(0, 0, 0, null, []);

    const nsm = {};
    allNodes.forEach(nd => { nsm[nd.id] = { ...nd, state: 'hidden', retVal: null }; });

    const LS = 44, LH = 36;
    let lc = 0;
    function asl(nodeId, depth) {
      const nd = allNodes.find(x => x.id === nodeId);
      if (!nd) return;
      nd._depth = depth;
      if (nd.children.length === 0) nd._leafIdx = lc++;
      else nd.children.forEach(cid => asl(cid, depth + 1));
    }
    asl(0, 0);
    const tW = Math.max(220, lc * LS + 30);
    const tH = Math.max(120, Math.max(0, ...allNodes.map(nd => nd._depth || 0)) * LH + 46);

    function ac(nodeId) {
      const nd = allNodes.find(x => x.id === nodeId);
      if (!nd) return 0;
      if (nd.children.length === 0) nd._x = 15 + nd._leafIdx * LS + LS / 2;
      else { const cxs = nd.children.map(cid => ac(cid)); nd._x = (cxs[0] + cxs[cxs.length - 1]) / 2; }
      nd._y = 20 + nd._depth * LH;
      return nd._x;
    }
    ac(0);

    const gvn = () => allNodes.filter(nd => nsm[nd.id].state !== 'hidden').map(nd => ({...nd, ...nsm[nd.id], x: nd._x, y: nd._y}));
    const gve = () => { const ed = []; allNodes.forEach(nd => { if (nd.parentId !== null && nsm[nd.id].state !== 'hidden' && nsm[nd.parentId].state !== 'hidden') { const par = allNodes.find(x => x.id === nd.parentId); if (par) ed.push({x1: par._x, y1: par._y + 11, x2: nd._x, y2: nd._y - 11, isPick: nd.picked.length > (par.picked ? par.picked.length : 0)}); } }); return ed; };
    const csB = (extra = {}) => ({ approach: 'brute', n, starts, ends, profits, jobs, treeWidth: tW, treeHeight: tH, treeNodes: gvn(), treeEdges: gve(), ...extra });

    const eb = { callCounter: 0, maxSelected: 0, bestSubset: [], activeNodeId: null, currentIdx: -1, lastEnd: 0, currentPicked: [], sFilled: 0, eFilled: 0, pFilled: 0 };
    steps.push({ code: 'm_scanner',   badge: 'Scanner sc = new Scanner(System.in); \u2014 Initializing input stream.', vars: [frame('main()', [['n', '?']])], state: csB(eb) });
    steps.push({ code: 'm_read_n',    badge: 'int n = sc.nextInt(); \u2192 n = ' + n + ' jobs to schedule.', vars: [frame('main()', [['n', String(n)]])], state: csB({...eb}) });
    steps.push({ code: 'm_alloc_s',   badge: 'int[] s = new int[' + n + ']; \u2014 Allocating startTime array.', vars: [frame('main()', [['n', String(n)]])], state: csB({...eb}) });
    steps.push({ code: 'm_alloc_e',   badge: 'int[] e = new int[' + n + ']; \u2014 Allocating endTime array.', vars: [frame('main()', [['n', String(n)]])], state: csB({...eb}) });
    steps.push({ code: 'm_alloc_p',   badge: 'int[] p = new int[' + n + ']; \u2014 Allocating profit array.', vars: [frame('main()', [['n', String(n)]])], state: csB({...eb}) });

    for (let i = 0; i < n; i++) {
      steps.push({ code: 'm_for_input', badge: 'Loop i = ' + i + ': reading startTime, endTime, profit for Job ' + i + '.', vars: [frame('main()', [['i', String(i)], ['n', String(n)]])], state: csB({...eb, currentIdx: i, sFilled: i, eFilled: i, pFilled: i}) });
      steps.push({ code: 'm_read_s',    badge: 's[' + i + '] = sc.nextInt(); \u2192 s[' + i + '] = ' + starts[i] + ' stored.', vars: [frame('main()', [['i', String(i)], ['s[' + i + ']', String(starts[i])]])], state: csB({...eb, currentIdx: i, sFilled: i + 1, eFilled: i, pFilled: i}) });
      steps.push({ code: 'm_read_e',    badge: 'e[' + i + '] = sc.nextInt(); \u2192 e[' + i + '] = ' + ends[i] + ' stored.', vars: [frame('main()', [['i', String(i)], ['e[' + i + ']', String(ends[i])]])], state: csB({...eb, currentIdx: i, sFilled: i + 1, eFilled: i + 1, pFilled: i}) });
      steps.push({ code: 'm_read_p',    badge: 'p[' + i + '] = sc.nextInt(); \u2192 p[' + i + '] = ' + profits[i] + ' stored.', vars: [frame('main()', [['i', String(i)], ['p[' + i + ']', String(profits[i])]])], state: csB({...eb, currentIdx: i, sFilled: i + 1, eFilled: i + 1, pFilled: i + 1}) });
    }

    steps.push({ code: 'm_call_solve', badge: 'Calling solve(s, e, p, n=' + n + ', idx=0, lastEnd=0, profit=0). Exhaustive recursion begins!', vars: [frame('main()', [['n', String(n)]])], state: csB({callCounter: 0, maxSelected: 0, bestSubset: [], activeNodeId: null, currentIdx: -1, lastEnd: 0, currentPicked: []}) });

    let sm = 0, sc2 = 0, sb = [];
    function simB(idx, lastEnd, profit, parentId, picked, nodeId) {
      sc2++;
      nsm[nodeId].state = 'active';
      const cs = [frame('main()', [['n', String(n)]]), frame('solve(idx=' + idx + ')', [['idx', String(idx)], ['lastEnd', String(lastEnd)], ['profit', String(profit)]])];
      steps.push({ code: 'c_entry', badge: 'Entering solve(idx=' + idx + ', lastEnd=' + lastEnd + ', profit=' + profit + '). Call #' + sc2 + '.', vars: cs, state: csB({callCounter: sc2, maxSelected: sm, bestSubset: [...sb], activeNodeId: nodeId, currentIdx: idx, lastEnd, currentPicked: [...picked]}) });
      const isBase = (idx === n);
      steps.push({ code: 'c_base_check', badge: 'if (idx == ' + n + ') \u2192 ' + (isBase ? 'TRUE (leaf reached!)' : 'FALSE'), vars: cs, state: csB({callCounter: sc2, maxSelected: sm, bestSubset: [...sb], activeNodeId: nodeId, currentIdx: idx, lastEnd, currentPicked: [...picked]}) });
      if (isBase) {
        nsm[nodeId].state = 'leaf'; nsm[nodeId].retVal = profit;
        const ib = profit > sm;
        steps.push({ code: 'c_update_check', badge: 'profit=' + profit + ' > maxProfit=' + sm + ' \u2192 ' + (ib ? 'TRUE! New best!' : 'FALSE'), vars: cs, state: csB({callCounter: sc2, maxSelected: sm, bestSubset: [...sb], activeNodeId: nodeId, currentIdx: idx, lastEnd, currentPicked: [...picked]}) });
        if (ib) { sm = profit; sb = [...picked]; steps.push({ code: 'c_update_max', badge: 'maxProfit = ' + sm + '. Best: {' + sb.map(x => 'J' + x).join(', ') + '}', vars: cs, state: csB({callCounter: sc2, maxSelected: sm, bestSubset: [...sb], activeNodeId: nodeId, currentIdx: idx, lastEnd, currentPicked: [...picked]}) }); }
        steps.push({ code: 'c_ret_base', badge: 'return; \u2014 Backtracking from leaf.', vars: cs, state: csB({callCounter: sc2, maxSelected: sm, bestSubset: [...sb], activeNodeId: nodeId, currentIdx: idx, lastEnd, currentPicked: [...picked]}) });
        nsm[nodeId].state = 'solved'; return;
      }
      steps.push({ code: 'c_call_skip', badge: 'SKIP J' + idx + ' [s=' + starts[idx] + ', e=' + ends[idx] + ', p=' + profits[idx] + ']. Branch left...', vars: cs, state: csB({callCounter: sc2, maxSelected: sm, bestSubset: [...sb], activeNodeId: nodeId, currentIdx: idx, lastEnd, currentPicked: [...picked]}) });
      const sc3 = allNodes.find(nd => nd.parentId === nodeId && nd.picked.length === picked.length);
      if (sc3) simB(idx + 1, lastEnd, profit, nodeId, picked, sc3.id);
      nsm[nodeId].state = 'active';
      const compat = starts[idx] >= lastEnd;
      steps.push({ code: 'c_check_compat', badge: 's[' + idx + ']=' + starts[idx] + ' >= lastEnd=' + lastEnd + ' \u2192 ' + (compat ? 'TRUE! Can TAKE.' : 'FALSE! Overlaps.'), vars: cs, state: csB({callCounter: sc2, maxSelected: sm, bestSubset: [...sb], activeNodeId: nodeId, currentIdx: idx, lastEnd, currentPicked: [...picked]}) });
      if (compat) {
        steps.push({ code: 'c_call_pick', badge: 'TAKE J' + idx + ' +$' + profits[idx] + '. lastEnd \u2192 ' + ends[idx] + '.', vars: cs, state: csB({callCounter: sc2, maxSelected: sm, bestSubset: [...sb], activeNodeId: nodeId, currentIdx: idx, lastEnd, currentPicked: [...picked]}) });
        const pc = allNodes.find(nd => nd.parentId === nodeId && nd.picked.length > picked.length);
        if (pc) simB(idx + 1, ends[idx], profit + profits[idx], nodeId, [...picked, idx], pc.id);
      }
      nsm[nodeId].state = 'solved';
    }
    simB(0, 0, 0, null, [], 0);
    steps.push({ code: 'm_print_ans', badge: 'Brute Force Complete! Max profit = $' + sm + '. Optimal: {' + sb.map(x => 'J' + x).join(', ') + '}', vars: [frame('main()', [['maxProfit', String(sm)]])], state: csB({callCounter: sc2, maxSelected: sm, bestSubset: [...sb], activeNodeId: null, currentIdx: -1, lastEnd: 0, currentPicked: []}) });
    steps.push({ code: 'm_done', badge: 'Program complete. Total recursive calls: ' + sc2, vars: [frame('main()', [['status', 'finished']])], state: csB({callCounter: sc2, maxSelected: sm, bestSubset: [...sb], activeNodeId: null, currentIdx: -1, lastEnd: 0, currentPicked: []}) });
    return steps;
  }

  // DP + Binary Search approach
  const sortedIndices = Array.from({length: n}, (_, i) => i);
  sortedIndices.sort((a, b) => ends[a] - ends[b]);
  const sortedJobs = sortedIndices.map(i => ({id: i, start: starts[i], end: ends[i], profit: profits[i]}));
  const csDP = (extra = {}) => ({ approach: 'dp', n, starts, ends, profits, jobs, sortedIndices, sortedJobs, ...extra });

  const nullDP = new Array(n + 1).fill(null);
  const ed0 = { phase: 'input', sFilled: 0, eFilled: 0, pFilled: 0, dpArr: nullDP, currentI: -1, bsLo: -1, bsHi: -1, bsMid: -1, bsJ: -1 };
  steps.push({ code: 'm_scanner',  badge: 'Scanner sc = new Scanner(System.in); \u2014 Initializing input stream.', vars: [frame('main()', [['n', '?']])], state: csDP(ed0) });
  steps.push({ code: 'm_read_n',   badge: 'int n = sc.nextInt(); \u2192 n = ' + n + ' jobs.', vars: [frame('main()', [['n', String(n)]])], state: csDP({...ed0}) });
  steps.push({ code: 'm_alloc_s',  badge: 'int[] s = new int[' + n + ']; \u2014 Allocating startTime array.', vars: [frame('main()', [['n', String(n)]])], state: csDP({...ed0}) });
  steps.push({ code: 'm_alloc_e',  badge: 'int[] e = new int[' + n + ']; \u2014 Allocating endTime array.', vars: [frame('main()', [['n', String(n)]])], state: csDP({...ed0}) });
  steps.push({ code: 'm_alloc_p',  badge: 'int[] p = new int[' + n + ']; \u2014 Allocating profit array.', vars: [frame('main()', [['n', String(n)]])], state: csDP({...ed0}) });

  for (let i = 0; i < n; i++) {
    steps.push({ code: 'm_for_input', badge: 'Loop i = ' + i + ': reading job ' + i + '.', vars: [frame('main()', [['i', String(i)], ['n', String(n)]])], state: csDP({phase: 'input', sFilled: i, eFilled: i, pFilled: i, dpArr: nullDP, currentI: i, bsLo: -1, bsHi: -1, bsMid: -1, bsJ: -1}) });
    steps.push({ code: 'm_read_s',    badge: 's[' + i + '] = sc.nextInt(); \u2192 s[' + i + '] = ' + starts[i] + '.', vars: [frame('main()', [['i', String(i)], ['s[' + i + ']', String(starts[i])]])], state: csDP({phase: 'input', sFilled: i + 1, eFilled: i, pFilled: i, dpArr: nullDP, currentI: i, bsLo: -1, bsHi: -1, bsMid: -1, bsJ: -1}) });
    steps.push({ code: 'm_read_e',    badge: 'e[' + i + '] = sc.nextInt(); \u2192 e[' + i + '] = ' + ends[i] + '.', vars: [frame('main()', [['i', String(i)], ['e[' + i + ']', String(ends[i])]])], state: csDP({phase: 'input', sFilled: i + 1, eFilled: i + 1, pFilled: i, dpArr: nullDP, currentI: i, bsLo: -1, bsHi: -1, bsMid: -1, bsJ: -1}) });
    steps.push({ code: 'm_read_p',    badge: 'p[' + i + '] = sc.nextInt(); \u2192 p[' + i + '] = ' + profits[i] + '.', vars: [frame('main()', [['i', String(i)], ['p[' + i + ']', String(profits[i])]])], state: csDP({phase: 'input', sFilled: i + 1, eFilled: i + 1, pFilled: i + 1, dpArr: nullDP, currentI: i, bsLo: -1, bsHi: -1, bsMid: -1, bsJ: -1}) });
  }

  steps.push({ code: 'm_call_dp', badge: 'Calling maxProfitDP(s, e, p, n=' + n + '). DP + Binary Search begins!', vars: [frame('main()', [['n', String(n)]])], state: csDP({phase: 'call', sFilled: n, eFilled: n, pFilled: n, dpArr: nullDP, currentI: -1, bsLo: -1, bsHi: -1, bsMid: -1, bsJ: -1}) });
  steps.push({ code: 'c_entry', badge: 'Entering maxProfitDP(). Sort by endTime, then dp[i+1] = max(take, skip) using binary search.', vars: [frame('main()', [['n', String(n)]]), frame('maxProfitDP()', [['n', String(n)]])], state: csDP({phase: 'entry', sFilled: n, eFilled: n, pFilled: n, dpArr: nullDP, currentI: -1, bsLo: -1, bsHi: -1, bsMid: -1, bsJ: -1}) });
  steps.push({ code: 'c_sort_init', badge: 'Integer[] idx = new Integer[' + n + ']; \u2014 Index array to sort without altering input.', vars: [frame('main()', [['n', String(n)]]), frame('maxProfitDP()', [['idx', 'new Integer[' + n + ']']])], state: csDP({phase: 'sort', sFilled: n, eFilled: n, pFilled: n, dpArr: nullDP, currentI: -1, idxFilled: 0, sortLoopI: -1, bsLo: -1, bsHi: -1, bsMid: -1, bsJ: -1}) });

  for (let fi = 0; fi < n; fi++) {
    steps.push({ code: 'c_sort_fill', badge: 'for i = ' + fi + ': about to assign idx[' + fi + '] = ' + fi + '.', vars: [frame('main()', [['n', String(n)]]), frame('maxProfitDP()', [['i', String(fi)]])], state: csDP({phase: 'sort', sFilled: n, eFilled: n, pFilled: n, dpArr: nullDP, currentI: -1, idxFilled: fi, sortLoopI: fi, bsLo: -1, bsHi: -1, bsMid: -1, bsJ: -1}) });
    steps.push({ code: 'c_sort_body', badge: 'idx[' + fi + '] = ' + fi + ' \u2014 Points to Job J' + fi + '.', vars: [frame('main()', [['n', String(n)]]), frame('maxProfitDP()', [['idx[' + fi + ']', String(fi)]])], state: csDP({phase: 'sort', sFilled: n, eFilled: n, pFilled: n, dpArr: nullDP, currentI: -1, idxFilled: fi + 1, sortLoopI: fi, bsLo: -1, bsHi: -1, bsMid: -1, bsJ: -1}) });
  }

  steps.push({ code: 'c_sort_exec', badge: 'Arrays.sort by e[idx]. Sorted: [' + sortedIndices.map(i => 'J' + i + '(e=' + ends[i] + ')').join(' \u2192 ') + '].', vars: [frame('main()', [['n', String(n)]]), frame('maxProfitDP()', [['sorted', sortedIndices.map(i => 'J' + i).join(',')]])], state: csDP({phase: 'sorted', sFilled: n, eFilled: n, pFilled: n, dpArr: nullDP, currentI: -1, bsLo: -1, bsHi: -1, bsMid: -1, bsJ: -1}) });
  steps.push({ code: 'c_dp_alloc', badge: 'int[] dp = new int[' + (n + 1) + ']; \u2014 dp[0..' + n + '] = 0.', vars: [frame('main()', [['n', String(n)]]), frame('maxProfitDP()', [['dp', '[' + new Array(n + 1).fill('0').join(', ') + ']']])], state: csDP({phase: 'dp_init', sFilled: n, eFilled: n, pFilled: n, dpArr: new Array(n + 1).fill(0), currentI: -1, bsLo: -1, bsHi: -1, bsMid: -1, bsJ: -1}) });

  const dpArr = new Array(n + 1).fill(0);
  for (let i = 0; i < n; i++) {
    const jobId = sortedIndices[i];
    steps.push({ code: 'c_for_loop', badge: 'for i = ' + i + ': Processing J' + jobId + ' [s=' + starts[jobId] + ', e=' + ends[jobId] + ', p=$' + profits[jobId] + '].', vars: [frame('main()', [['n', String(n)]]), frame('maxProfitDP()', [['i', String(i)], ['dp[i]', String(dpArr[i])]])], state: csDP({phase: 'dp_loop', sFilled: n, eFilled: n, pFilled: n, dpArr: [...dpArr], currentI: i, bsLo: -1, bsHi: -1, bsMid: -1, bsJ: -1}) });
    steps.push({ code: 'c_bsearch', badge: 'Binary search init: lo=0, hi=' + (i - 1) + ', j=-1. Find latest idx[j] where e[idx[j]] <= s[J' + jobId + ']=' + starts[jobId] + '.', vars: [frame('main()', [['n', String(n)]]), frame('maxProfitDP()', [['i', String(i)], ['lo', '0'], ['hi', String(i - 1)], ['j', '-1']])], state: csDP({phase: 'bs_init', sFilled: n, eFilled: n, pFilled: n, dpArr: [...dpArr], currentI: i, bsLo: 0, bsHi: i - 1, bsMid: -1, bsJ: -1}) });
    let lo = 0, hi = i - 1, j = -1;
    while (lo <= hi) {
      const mid = (lo + hi) >> 1;
      steps.push({ code: 'c_bsearch_loop', badge: 'lo=' + lo + ' <= hi=' + hi + ' \u2192 TRUE. mid=' + mid + '.', vars: [frame('main()', [['n', String(n)]]), frame('maxProfitDP()', [['lo', String(lo)], ['hi', String(hi)], ['mid', String(mid)]])], state: csDP({phase: 'bs_mid', sFilled: n, eFilled: n, pFilled: n, dpArr: [...dpArr], currentI: i, bsLo: lo, bsHi: hi, bsMid: mid, bsJ: j}) });
      steps.push({ code: 'c_bsearch_mid', badge: 'mid=' + mid + '. e[idx[' + mid + ']]=e[J' + sortedIndices[mid] + ']=' + ends[sortedIndices[mid]] + '.', vars: [frame('main()', [['n', String(n)]]), frame('maxProfitDP()', [['mid', String(mid)], ['e[idx[mid]]', String(ends[sortedIndices[mid]])]])], state: csDP({phase: 'bs_mid', sFilled: n, eFilled: n, pFilled: n, dpArr: [...dpArr], currentI: i, bsLo: lo, bsHi: hi, bsMid: mid, bsJ: j}) });
      if (ends[sortedIndices[mid]] <= starts[sortedIndices[i]]) {
        steps.push({ code: 'c_bsearch_cmp', badge: ends[sortedIndices[mid]] + ' <= ' + starts[jobId] + ' \u2192 TRUE. j=' + mid + ', lo=' + (mid + 1) + '.', vars: [frame('main()', [['n', String(n)]]), frame('maxProfitDP()', [['j', String(mid)], ['lo', String(mid + 1)]])], state: csDP({phase: 'bs_hit', sFilled: n, eFilled: n, pFilled: n, dpArr: [...dpArr], currentI: i, bsLo: mid + 1, bsHi: hi, bsMid: mid, bsJ: mid}) });
        steps.push({ code: 'c_bsearch_hit', badge: 'j = ' + mid + '. Compatible job found. lo = ' + (mid + 1) + '.', vars: [frame('main()', [['n', String(n)]]), frame('maxProfitDP()', [['j', String(mid)]])], state: csDP({phase: 'bs_hit', sFilled: n, eFilled: n, pFilled: n, dpArr: [...dpArr], currentI: i, bsLo: mid + 1, bsHi: hi, bsMid: mid, bsJ: mid}) });
        j = mid; lo = mid + 1;
      } else {
        steps.push({ code: 'c_bsearch_cmp', badge: ends[sortedIndices[mid]] + ' <= ' + starts[jobId] + ' \u2192 FALSE. hi=' + (mid - 1) + '.', vars: [frame('main()', [['n', String(n)]]), frame('maxProfitDP()', [['hi', String(mid - 1)]])], state: csDP({phase: 'bs_miss', sFilled: n, eFilled: n, pFilled: n, dpArr: [...dpArr], currentI: i, bsLo: lo, bsHi: mid - 1, bsMid: mid, bsJ: j}) });
        hi = mid - 1;
      }
    }
    const takeVal = profits[jobId] + (j >= 0 ? dpArr[j + 1] : 0);
    const skipVal = dpArr[i];
    dpArr[i + 1] = Math.max(takeVal, skipVal);
    const chose = takeVal >= skipVal ? 'TAKE' : 'SKIP';
    steps.push({ code: 'c_dp_take',   badge: 'take = p[J' + jobId + '] + dp[' + (j + 1) + '] = ' + profits[jobId] + ' + ' + (j >= 0 ? dpArr[j + 1] : 0) + ' = ' + takeVal + '.', vars: [frame('main()', [['n', String(n)]]), frame('maxProfitDP()', [['take', String(takeVal)], ['j', String(j)]])], state: csDP({phase: 'dp_take', sFilled: n, eFilled: n, pFilled: n, dpArr: [...dpArr], currentI: i, bsLo: -1, bsHi: -1, bsMid: -1, bsJ: j, takeVal, skipVal}) });
    steps.push({ code: 'c_dp_skip',   badge: 'skip = dp[' + i + '] = ' + skipVal + '.', vars: [frame('main()', [['n', String(n)]]), frame('maxProfitDP()', [['skip', String(skipVal)]])], state: csDP({phase: 'dp_skip', sFilled: n, eFilled: n, pFilled: n, dpArr: [...dpArr], currentI: i, bsLo: -1, bsHi: -1, bsMid: -1, bsJ: j, takeVal, skipVal}) });
    steps.push({ code: 'c_dp_assign', badge: 'dp[' + (i + 1) + '] = max(' + takeVal + ', ' + skipVal + ') = ' + dpArr[i + 1] + '. Decision: ' + chose + ' J' + jobId + '.', vars: [frame('main()', [['n', String(n)]]), frame('maxProfitDP()', [['dp[' + (i + 1) + ']', String(dpArr[i + 1])]])], state: csDP({phase: 'dp_assign', sFilled: n, eFilled: n, pFilled: n, dpArr: [...dpArr], currentI: i, bsLo: -1, bsHi: -1, bsMid: -1, bsJ: j, takeVal, skipVal}) });
  }

  steps.push({ code: 'c_ret_dp',   badge: 'return dp[' + n + '] = ' + dpArr[n] + '. Maximum profit = $' + dpArr[n] + '.', vars: [frame('main()', [['n', String(n)]]), frame('maxProfitDP()', [['return', String(dpArr[n])]])], state: csDP({phase: 'done', sFilled: n, eFilled: n, pFilled: n, dpArr: [...dpArr], currentI: -1, bsLo: -1, bsHi: -1, bsMid: -1, bsJ: -1}) });
  steps.push({ code: 'm_print_ans', badge: 'System.out.println(ans = ' + dpArr[n] + '); \u2014 Optimal profit in O(n log n)!', vars: [frame('main()', [['ans', String(dpArr[n])]])], state: csDP({phase: 'done', sFilled: n, eFilled: n, pFilled: n, dpArr: [...dpArr], currentI: -1, bsLo: -1, bsHi: -1, bsMid: -1, bsJ: -1}) });
  steps.push({ code: 'm_done',     badge: 'Program finished. DP + Binary Search proved globally optimal!', vars: [frame('main()', [['status', 'completed']])], state: csDP({phase: 'done', sFilled: n, eFilled: n, pFilled: n, dpArr: [...dpArr], currentI: -1, bsLo: -1, bsHi: -1, bsMid: -1, bsJ: -1}) });
  return steps;
}

const currentApproach  = ref('dp');
const inputStartText   = ref('1, 2, 3, 3');
const inputEndText     = ref('3, 4, 5, 6');
const inputProfitText  = ref('50, 10, 40, 70');
const lang             = ref('java');
const speed            = ref(650);
const si               = ref(0);
const playing          = ref(false);
const vizHeight        = ref(195);
const tableHeight      = ref(42);
const leftWidth        = ref(54);
const rightTab         = ref('code');

function parseArray(text) {
  return text.split(/[,;\s]+/).map(x => parseInt(x.trim(), 10)).filter(x => !isNaN(x));
}

const currentStarts  = computed(() => { const a = parseArray(inputStartText.value);  return a.length >= 2 ? a : DEFAULT_STARTS;  });
const currentEnds    = computed(() => { const a = parseArray(inputEndText.value);    return a.length >= 2 ? a : DEFAULT_ENDS;    });
const currentProfits = computed(() => { const a = parseArray(inputProfitText.value); return a.length >= 2 ? a : DEFAULT_PROFITS; });

const currentJobs = computed(() => {
  const s = currentStarts.value, e = currentEnds.value, p = currentProfits.value;
  const n = Math.min(s.length, e.length, p.length);
  return Array.from({length: n}, (_, i) => ({id: i, start: s[i], end: e[i], profit: p[i]}));
});

const stepsData = reactive({ steps: buildSteps('dp', DEFAULT_STARTS, DEFAULT_ENDS, DEFAULT_PROFITS) });
const steps     = computed(() => stepsData.steps);
const s         = computed(() => steps.value[Math.max(0, Math.min(si.value, steps.value.length - 1))] || { state: {} });
const st        = computed(() => s.value.state || {});
const codeLines       = computed(() => { const c = CODES[currentApproach.value] || CODES.dp; return c[lang.value] || c.java || []; });
const pseudocodeLines = computed(() => PSEUDOCODES[currentApproach.value] || PSEUDOCODES.dp);

let playTimer = null;
function applyApproach(appId) { currentApproach.value = appId; playing.value = false; stepsData.steps = buildSteps(appId, currentStarts.value, currentEnds.value, currentProfits.value); si.value = 0; }
function applyInput() { playing.value = false; stepsData.steps = buildSteps(currentApproach.value, currentStarts.value, currentEnds.value, currentProfits.value); si.value = 0; }
function stepBy(d) { si.value = Math.max(0, Math.min(steps.value.length - 1, si.value + d)); }
function togglePlay() { const next = !playing.value; if (next && si.value >= steps.value.length - 1) si.value = 0; playing.value = next; }
function tick() { clearTimeout(playTimer); if (!playing.value) return; if (si.value >= steps.value.length - 1) { playing.value = false; return; } playTimer = setTimeout(() => { si.value = Math.min(steps.value.length - 1, si.value + 1); tick(); }, 2100 - speed.value); }
watch(playing, v => { if (v) tick(); else clearTimeout(playTimer); });
function onKeydown(e) { const t = e.target.tagName; if (t === 'INPUT' || t === 'SELECT' || t === 'TEXTAREA') return; if (e.key === 'ArrowRight') stepBy(1); if (e.key === 'ArrowLeft') stepBy(-1); if (e.key === ' ') { e.preventDefault(); togglePlay(); } }

const mainRef = ref(null), leftColRef = ref(null), hResizerRef = ref(null), vizResizerRef = ref(null), tableResizerRef = ref(null);

function initHResizer() {
  const rsz = hResizerRef.value, main = mainRef.value;
  if (!rsz || !main) return;
  let drag = false, sx = 0, sw = 0;
  const dn = e => { drag = true; sx = e.clientX; sw = leftColRef.value.offsetWidth; rsz.classList.add('drag'); document.body.style.userSelect = 'none'; };
  const mv = e => { if (!drag) return; leftWidth.value = (Math.max(220, Math.min(main.offsetWidth - 220, sw + e.clientX - sx)) / main.offsetWidth) * 100; };
  const up = () => { if (!drag) return; drag = false; rsz.classList.remove('drag'); document.body.style.userSelect = ''; };
  rsz.addEventListener('mousedown', dn); document.addEventListener('mousemove', mv); document.addEventListener('mouseup', up);
  return () => { rsz.removeEventListener('mousedown', dn); document.removeEventListener('mousemove', mv); document.removeEventListener('mouseup', up); };
}
function initVResizer(elRef, valueRef, minH, maxH) {
  const rsz = elRef.value;
  if (!rsz) return;
  let drag = false, sy = 0, sh = 0;
  const dn = e => { drag = true; sy = e.clientY; sh = valueRef.value; rsz.classList.add('drag'); document.body.style.userSelect = 'none'; e.preventDefault(); };
  const mv = e => { if (!drag) return; valueRef.value = Math.max(minH, Math.min(maxH, sh + (e.clientY - sy))); };
  const up = () => { if (!drag) return; drag = false; rsz.classList.remove('drag'); document.body.style.userSelect = ''; };
  rsz.addEventListener('mousedown', dn); document.addEventListener('mousemove', mv); document.addEventListener('mouseup', up);
  return () => { rsz.removeEventListener('mousedown', dn); document.removeEventListener('mousemove', mv); document.removeEventListener('mouseup', up); };
}

let cleanupFns = [];
onMounted(() => { document.addEventListener('keydown', onKeydown); cleanupFns.push(initHResizer()); cleanupFns.push(initVResizer(vizResizerRef, vizHeight, 130, 420)); cleanupFns.push(initVResizer(tableResizerRef, tableHeight, 30, 140)); });
onUnmounted(() => { document.removeEventListener('keydown', onKeydown); clearTimeout(playTimer); cleanupFns.forEach(fn => fn && fn()); });
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
            <div class="ll-approach-group">
              <button v-for="app in APPROACHES" :key="app.id" class="ll-approach-btn"
                :class="{ active: currentApproach === app.id }" :title="app.desc" @click="applyApproach(app.id)">
                {{ app.label }}
              </button>
            </div>
            <div class="ll-input-group">
              <label>start</label>
              <input type="text" v-model="inputStartText" class="ll-text-input" placeholder="1, 2, 3, 3" @keyup.enter="applyInput" style="width:90px;" />
            </div>
            <div class="ll-input-group">
              <label>end</label>
              <input type="text" v-model="inputEndText" class="ll-text-input" placeholder="3, 4, 5, 6" @keyup.enter="applyInput" style="width:90px;" />
            </div>
            <div class="ll-input-group">
              <label>profit</label>
              <input type="text" v-model="inputProfitText" class="ll-text-input" placeholder="50, 10, 40, 70" @keyup.enter="applyInput" style="width:90px;" />
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

              <!-- 3-array banner -->
              <div class="ll-house-banner">
                <div class="ll-dual-arrays-wrap">
                  <!-- start[] -->
                  <div class="ll-arr-row">
                    <span class="ll-arr-tag">start[]:</span>
                    <div class="ll-arr-cells">
                      <div v-for="(job, idx) in currentJobs" :key="'s' + idx" class="ll-arr-cell"
                        :class="{ 'll-cell-active': st.currentIdx === idx || st.currentI === idx }">
                        <span class="ll-cell-idx">[{{ idx }}]</span>
                        <span class="ll-cell-val">{{ (st.sFilled !== undefined && idx >= st.sFilled) ? '?' : job.start }}</span>
                      </div>
                    </div>
                  </div>
                  <!-- end[] -->
                  <div class="ll-arr-row">
                    <span class="ll-arr-tag">end[]:</span>
                    <div class="ll-arr-cells">
                      <div v-for="(job, idx) in currentJobs" :key="'e' + idx" class="ll-arr-cell"
                        :class="{ 'll-cell-active': st.currentIdx === idx || st.currentI === idx }">
                        <span class="ll-cell-idx">[{{ idx }}]</span>
                        <span class="ll-cell-val">{{ (st.eFilled !== undefined && idx >= st.eFilled) ? '?' : job.end }}</span>
                      </div>
                    </div>
                  </div>
                  <!-- profit[] -->
                  <div class="ll-arr-row">
                    <span class="ll-arr-tag">profit[]:</span>
                    <div class="ll-arr-cells">
                      <div v-for="(job, idx) in currentJobs" :key="'p' + idx" class="ll-arr-cell"
                        :class="{ 'll-cell-active': st.currentIdx === idx || st.currentI === idx, 'll-cell-selected': st.bestSubset && st.bestSubset.includes(idx) }">
                        <span class="ll-cell-idx">[{{ idx }}]</span>
                        <span class="ll-cell-val">{{ (st.pFilled !== undefined && idx >= st.pFilled) ? '?' : (job.profit) }}</span>
                      </div>
                    </div>
                  </div>
                </div>
              </div>

              <!-- Status chips -->
              <div class="ll-ptrs">
                <div class="ll-ptr-chip">N: <b class="ll-c-blue">{{ st.n || currentJobs.length }}</b></div>
                <template v-if="currentApproach === 'dp'">
                  <div class="ll-ptr-chip" v-if="st.dpArr && st.dpArr[st.dpArr.length - 1] > 0">Max Profit: <b class="ll-c-green">${{ st.dpArr[st.dpArr.length - 1] }}</b></div>
                  <div class="ll-ptr-chip" v-if="st.currentI >= 0">Processing: <b class="ll-c-orange">J{{ (st.sortedIndices || [])[st.currentI] }}</b></div>
                  <div class="ll-ptr-chip" v-if="st.bsJ >= 0">BS j: <b class="ll-c-purple">{{ st.bsJ }}</b></div>
                </template>
                <template v-else>
                  <div class="ll-ptr-chip">Calls: <b class="ll-c-orange">{{ st.callCounter || 0 }}</b></div>
                  <div class="ll-ptr-chip" :class="{ 'll-chip-warn': (st.maxSelected || 0) > 0 }">Max Profit: <b class="ll-c-green">${{ st.maxSelected || 0 }}</b></div>
                  <div class="ll-ptr-chip" v-if="st.bestSubset && st.bestSubset.length">Best: <b class="ll-c-purple">{ {{ st.bestSubset.map(id => 'J' + id).join(', ') }} }</b></div>
                </template>
              </div>

              <!-- Visualization panel -->
              <div class="ll-viz-wrap" :style="{ height: vizHeight + 'px' }">

                <!-- DP Viz -->
                <div v-if="currentApproach === 'dp'" class="ll-arr-viz-container">
                  <div class="ll-arr-viz-scroll">

                    <!-- idx[] sorted array -->
                    <div class="ll-viz-arr-section ll-viz-idx-section">
                      <div class="ll-viz-arr-label ll-viz-arr-label-idx">
                        idx[]<span class="ll-viz-arr-sub"> (sorted by end[])</span>
                        <span class="ll-idx-sort-badge" v-if="['sorted','dp_init','dp_loop','bs_init','bs_mid','bs_hit','bs_miss','dp_take','dp_skip','dp_assign','done'].includes(st.phase)">SORTED</span>
                        <span class="ll-idx-unsort-badge" v-else-if="st.phase === 'sort'">FILLING&hellip; ({{ st.idxFilled || 0 }}/{{ st.n || currentJobs.length }})</span>
                        <span class="ll-idx-unsort-badge ll-idx-init-badge" v-else>NOT YET</span>
                      </div>
                      <div class="ll-viz-arr-track">
                        <template v-if="['sorted','dp_init','dp_loop','bs_init','bs_mid','bs_hit','bs_miss','dp_take','dp_skip','dp_assign','done'].includes(st.phase)">
                          <div v-for="(origId, rank) in (st.sortedIndices || [])" :key="'vidx' + rank"
                            class="ll-viz-cell ll-viz-cell-idx"
                            :class="{ 'll-viz-cell-idx-active': st.currentI === rank, 'll-viz-cell-idx-sel': st.currentI > rank, 'll-dp-bs-lo': st.bsLo >= 0 && rank === st.bsLo, 'll-dp-bs-hi': st.bsHi >= 0 && rank === st.bsHi, 'll-dp-bs-mid': st.bsMid >= 0 && rank === st.bsMid }">
                            <span class="ll-viz-cell-index">[{{ rank }}]</span>
                            <span class="ll-viz-cell-val">J{{ origId }}</span>
                            <span class="ll-viz-cell-ann-idx">e={{ (st.ends || currentEnds)[origId] }}</span>
                          </div>
                        </template>
                        <template v-else-if="st.phase === 'sort'">
                          <div v-for="i in (st.idxFilled || 0)" :key="'vf' + (i-1)"
                            class="ll-viz-cell ll-viz-cell-idx"
                            :class="{ 'll-viz-cell-idx-active': (i-1) === st.sortLoopI && st.idxFilled === i }">
                            <span class="ll-viz-cell-index">[{{ i - 1 }}]</span>
                            <span class="ll-viz-cell-val">{{ i - 1 }}</span>
                            <span class="ll-viz-cell-ann-idx">e={{ (st.ends || currentEnds)[i - 1] }}</span>
                          </div>
                        </template>
                        <template v-else>
                          <div class="ll-viz-cell-empty-hint">idx[] not yet created</div>
                        </template>
                      </div>
                    </div>

                    <!-- dp[] array -->
                    <div class="ll-viz-arr-section" v-if="st.dpArr">
                      <div class="ll-viz-arr-label">dp[]<span class="ll-viz-arr-sub"> (max profit using first i sorted jobs)</span></div>
                      <div class="ll-viz-arr-track">
                        <div v-for="(val, i) in (st.dpArr || [])" :key="'dp' + i"
                          class="ll-viz-cell ll-dp-cell"
                          :class="{ 'll-dp-cell-active': st.currentI >= 0 && i === st.currentI + 1, 'll-dp-cell-filled': val !== null && i <= (st.currentI + 1), 'll-dp-cell-null': val === null }">
                          <span class="ll-viz-cell-index">dp[{{ i }}]</span>
                          <span class="ll-viz-cell-val">{{ val !== null ? val : '?' }}</span>
                        </div>
                      </div>
                    </div>

                    <!-- BS info bar -->
                    <div class="ll-lastfinish-bar" v-if="st.bsLo >= 0 || st.bsMid >= 0">
                      <span class="ll-lf-label">&#9654; Binary Search:</span>
                      <span class="ll-lf-val">lo={{ st.bsLo }}</span>
                      <span class="ll-lf-val" style="background:#9333ea;">hi={{ st.bsHi }}</span>
                      <span class="ll-lf-val" style="background:#3b82f6;" v-if="st.bsMid >= 0">mid={{ st.bsMid }}</span>
                      <span class="ll-lf-hint">j={{ st.bsJ >= 0 ? st.bsJ : 'none yet' }}</span>
                    </div>

                    <!-- take vs skip -->
                    <div class="ll-lastfinish-bar" v-if="['dp_take','dp_skip','dp_assign'].includes(st.phase)">
                      <span class="ll-lf-label">Decision:</span>
                      <span class="ll-lf-val" style="background:#10b981;">take=${{ st.takeVal }}</span>
                      <span class="ll-lf-val" style="background:#64748b;">skip=${{ st.skipVal }}</span>
                      <span class="ll-lf-hint">dp[{{ (st.currentI || 0) + 1 }}] = max({{ st.takeVal }}, {{ st.skipVal }}) = {{ Math.max(st.takeVal || 0, st.skipVal || 0) }}</span>
                    </div>

                  </div>
                </div>

                <!-- Brute Force Tree -->
                <div v-else class="ll-tree-container">
                  <div class="ll-tree-scroll-area">
                    <svg :viewBox="'0 0 ' + (st.treeWidth || 480) + ' ' + (st.treeHeight || 240)"
                      :style="{ width: '100%', maxWidth: (st.treeWidth || 480) + 'px', height: (st.treeHeight || 240) + 'px', maxHeight: (st.treeHeight || 240) + 'px' }"
                      class="ll-tree-svg">
                      <g>
                        <line v-for="(edge, i) in st.treeEdges" :key="'edge' + i"
                          :x1="edge.x1" :y1="edge.y1" :x2="edge.x2" :y2="edge.y2"
                          class="ll-tree-edge" :class="{ 'll-edge-pick': edge.isPick, 'll-edge-skip': !edge.isPick }" />
                      </g>
                      <g>
                        <g v-for="node in st.treeNodes" :key="node.id" :transform="'translate(' + node.x + ',' + node.y + ')'" class="ll-tree-node-group">
                          <rect x="-19" y="-11" width="38" height="22" rx="4" class="ll-node-rect"
                            :class="{ 'll-node-active': node.id === st.activeNodeId, 'll-node-solved': node.state === 'solved', 'll-node-leaf': node.state === 'leaf' }" />
                          <text x="0" y="-2.5" text-anchor="middle" class="ll-node-text-call">idx={{ node.idx }}</text>
                          <text x="0" y="6.5" text-anchor="middle" class="ll-node-text-val"
                            :class="{ 'll-val-active': node.id === st.activeNodeId, 'll-val-solved': node.state === 'solved' }">
                            ${{ node.profit }}
                          </text>
                        </g>
                      </g>
                    </svg>
                    <div class="ll-act-table-wrap" v-if="st.currentPicked">
                      <div class="ll-act-table-title">Currently Chosen Jobs in this Branch:</div>
                      <div class="ll-act-row-wrap">
                        <span v-if="!st.currentPicked.length" class="ll-empty-set">&empty; (Empty)</span>
                        <div v-for="pid in st.currentPicked" :key="pid" class="ll-act-chip ll-act-picked">
                          J{{ pid }} [s={{ (st.starts || currentStarts)[pid] }}, e={{ (st.ends || currentEnds)[pid] }}, ${{ (st.profits || currentProfits)[pid] }}]
                        </div>
                      </div>
                    </div>
                  </div>
                </div>

              </div>

              <!-- Vertical resizer: viz/table -->
              <div class="ll-vresizer" ref="vizResizerRef"></div>

              <!-- Legend -->
              <div class="ll-legend">
                <template v-if="currentApproach === 'dp'">
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-purple"></span>idx[] sorted by end[]</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-cur"></span>Current job (i)</span>
                  <span class="ll-leg"><span class="ll-legdot" style="background:#3b82f6;border:1.5px solid #1d4ed8;"></span>Binary Search mid</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-new"></span>dp[] filled</span>
                </template>
                <template v-else>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-cur"></span>Active Decision</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-new"></span>Solved/Backtracked</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-purple"></span>Leaf Node</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-green"></span>Take Edge</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-dash"></span>Skip Edge</span>
                </template>
              </div>

              <!-- Variable frames -->
              <div class="ll-table-area" :style="{ height: tableHeight + 'px' }">
                <div class="ll-table-title">Variable Frames &mdash; Innermost = Current Scope</div>
                <div class="ll-stack-line">
                  <template v-if="s.vars && s.vars.length">
                    <div v-for="(f, depth) in s.vars" :key="depth" class="ll-frame"
                      :class="{ 'll-frame-cur': depth === s.vars.length - 1 }"
                      :style="{ marginLeft: depth * 12 + 'px' }">
                      {{ f.title }}(<span v-for="(r, i) in f.rows" :key="i"><span v-if="i > 0">, </span><span class="ll-fname">{{ r[0] }}</span>=<span :class="r[0] === 'return' || r[0] === 'ans' ? 'll-c-green' : (depth === s.vars.length - 1 ? 'll-c-orange' : 'll-c-blue')" style="font-weight:700">{{ r[1] }}</span></span>)<span v-if="depth === s.vars.length - 1" class="ll-now"> &#9668; current</span>
                    </div>
                  </template>
                  <template v-else>&mdash;</template>
                </div>
              </div>

              <!-- Vertical resizer: table/badge -->
              <div class="ll-vresizer" ref="tableResizerRef"></div>

              <!-- Status badge -->
              <div class="ll-badge-wrap">
                <div class="ll-badge"
                  :class="{ 'll-badge-error': s.badge && (s.badge.includes('Overlaps') || s.badge.includes('FALSE')), 'll-badge-success': s.badge && (s.badge.includes('Complete') || s.badge.includes('Optimal') || s.badge.includes('optimal') || s.badge.includes('TAKE')) }">
                  {{ s.badge || 'Ready to visualize Maximum Profit in Job Scheduling.' }}
                </div>
              </div>

            </div>

            <!-- Horizontal resizer -->
            <div class="ll-resizer" ref="hResizerRef"></div>

            <!-- Right Column: Code Panel -->
            <div class="ll-right-col">
              <div class="ll-code-panel">
                <div class="ll-code-header">
                  <div class="ll-tabbar">
                    <button class="ll-tab-btn" :class="{ active: rightTab === 'code' }"       @click="rightTab = 'code'">Code</button>
                    <button class="ll-tab-btn" :class="{ active: rightTab === 'pseudo' }"     @click="rightTab = 'pseudo'">Pseudocode</button>
                    <button class="ll-tab-btn" :class="{ active: rightTab === 'complexity' }" @click="rightTab = 'complexity'">Complexity</button>
                  </div>
                  <select v-if="rightTab === 'code'" v-model="lang" class="ll-lang-select">
                    <option value="java">Java</option>
                    <option value="c">C</option>
                    <option value="cpp">C++</option>
                    <option value="python">Python</option>
                  </select>
                </div>

                <!-- Code tab -->
                <div v-if="rightTab === 'code'" class="ll-code-scroll">
                  <pre class="ll-pre"><span v-for="(line, i) in codeLines" :key="i" class="ll-codeline" :class="{ 'll-hl': line[0] && line[0] === s.code }">{{ line[1] === '' ? ' ' : line[1] }}</span></pre>
                </div>

                <!-- Pseudocode tab -->
                <div v-else-if="rightTab === 'pseudo'" class="ll-code-scroll">
                  <pre class="ll-pre"><span v-for="(line, i) in pseudocodeLines" :key="i" class="ll-codeline">{{ line }}</span></pre>
                </div>

                <!-- Complexity tab -->
                <div v-else class="ll-info-scroll">

                  <!-- BRUTE FORCE -->
                  <template v-if="currentApproach === 'brute'">
                    <h3 class="ll-cx-heading">Brute Force &mdash; Complexity Analysis</h3>
                    <p class="ll-cx-intro">
                      Exhaustively explores every possible <strong>subset</strong> of jobs via recursion.
                      At each step two choices exist: <strong>skip</strong> or <strong>take</strong> the current job (if compatible).
                      Unlike unweighted Activity Selection, a pure greedy strategy cannot be applied here
                      because each job has a distinct profit value.
                    </p>
                    <h4 class="ll-cx-sub">Step-by-step Breakdown</h4>
                    <table class="ll-complexity-table">
                      <thead><tr><th>Operation</th><th>Time</th><th>Space</th><th>Reason</th></tr></thead>
                      <tbody>
                        <tr><td>Read input (s[], e[], p[])</td><td class="ll-cx-good">O(n)</td><td class="ll-cx-good">O(n)</td><td>One pass, three parallel arrays</td></tr>
                        <tr><td>Recursive branching (skip / take)</td><td class="ll-cx-bad">O(2<sup>n</sup>)</td><td class="ll-cx-mid">O(n)</td><td>Binary tree of depth n &rarr; 2<sup>n</sup> leaves; stack depth = n</td></tr>
                        <tr><td>Compatibility check &amp; profit update</td><td class="ll-cx-good">O(1)</td><td class="ll-cx-good">O(1)</td><td>Simple comparison at each node and leaf</td></tr>
                      </tbody>
                    </table>
                    <h4 class="ll-cx-sub">Overall Complexity</h4>
                    <div class="ll-cx-summary-grid">
                      <div class="ll-cx-card ll-cx-card-bad"><div class="ll-cx-card-label">Time</div><div class="ll-cx-card-val">O(2<sup>n</sup>)</div><div class="ll-cx-card-note">Exponential &mdash; infeasible for large n</div></div>
                      <div class="ll-cx-card ll-cx-card-mid"><div class="ll-cx-card-label">Space</div><div class="ll-cx-card-val">O(n)</div><div class="ll-cx-card-note">Recursion call stack depth</div></div>
                      <div class="ll-cx-card ll-cx-card-good"><div class="ll-cx-card-label">Correct?</div><div class="ll-cx-card-val">YES</div><div class="ll-cx-card-note">Exhaustive &rarr; guaranteed optimal</div></div>
                    </div>
                    <div class="ll-note">
                      <strong>Note:</strong> Brute force is correct but only feasible for n &le; 20.
                      For larger inputs use the DP + Binary Search approach which achieves O(n log n).
                    </div>
                  </template>

                  <!-- DP + BINARY SEARCH -->
                  <template v-else>
                    <h3 class="ll-cx-heading">DP + Binary Search &mdash; Complexity Analysis</h3>
                    <p class="ll-cx-intro">
                      Sorts jobs by <strong>endTime</strong> using an auxiliary index array, then fills
                      <code>dp[i+1] = max(take, skip)</code> where <em>take</em> is computed
                      via binary search on the sorted end-times. This is the classic
                      <em>Weighted Job Scheduling</em> algorithm.
                    </p>
                    <h4 class="ll-cx-sub">Step-by-step Breakdown</h4>
                    <table class="ll-complexity-table">
                      <thead><tr><th>Operation</th><th>Time</th><th>Space</th><th>Reason</th></tr></thead>
                      <tbody>
                        <tr><td>Read input (s[], e[], p[])</td><td class="ll-cx-good">O(n)</td><td class="ll-cx-good">O(n)</td><td>Single pass, three arrays</td></tr>
                        <tr><td>Create &amp; sort idx[] by e[idx[i]]</td><td class="ll-cx-mid">O(n log n)</td><td class="ll-cx-good">O(n)</td><td>Comparison sort on n indices &mdash; dominates total time</td></tr>
                        <tr><td>Binary search per DP step</td><td class="ll-cx-good">O(log n) each</td><td class="ll-cx-good">O(1)</td><td>n steps &times; O(log n) = O(n log n) total</td></tr>
                        <tr><td>DP recurrence dp[i+1] = max(take, skip)</td><td class="ll-cx-good">O(1) each</td><td class="ll-cx-good">O(n)</td><td>n iterations &times; O(1) = O(n) total; dp[] array = O(n)</td></tr>
                      </tbody>
                    </table>
                    <h4 class="ll-cx-sub">Overall Complexity</h4>
                    <div class="ll-cx-summary-grid">
                      <div class="ll-cx-card ll-cx-card-mid"><div class="ll-cx-card-label">Time</div><div class="ll-cx-card-val">O(n log n)</div><div class="ll-cx-card-note">Sort + binary searches dominate</div></div>
                      <div class="ll-cx-card ll-cx-card-good"><div class="ll-cx-card-label">Space</div><div class="ll-cx-card-val">O(n)</div><div class="ll-cx-card-note">idx[] + dp[] auxiliary arrays</div></div>
                      <div class="ll-cx-card ll-cx-card-good"><div class="ll-cx-card-label">Optimal?</div><div class="ll-cx-card-val">YES</div><div class="ll-cx-card-note">DP optimal substructure proved</div></div>
                    </div>
                    <h4 class="ll-cx-sub">Approach Comparison</h4>
                    <table class="ll-complexity-table">
                      <thead><tr><th>Approach</th><th>Time</th><th>Space</th><th>Practical?</th></tr></thead>
                      <tbody>
                        <tr><td><strong>Brute Force</strong></td><td class="ll-cx-bad">O(2<sup>n</sup>)</td><td class="ll-cx-mid">O(n)</td><td class="ll-cx-bad">No &mdash; exponential blowup</td></tr>
                        <tr><td><strong>DP + Binary Search</strong></td><td class="ll-cx-mid">O(n log n)</td><td class="ll-cx-good">O(n)</td><td class="ll-cx-good">Yes &mdash; scales to millions</td></tr>
                      </tbody>
                    </table>
                    <div class="ll-note">
                      <strong>Key Insight:</strong> This is the <em>weighted</em> job scheduling problem.
                      Pure greedy (earliest finish first) fails because profits differ &mdash; we need DP.
                      The binary search finds the latest compatible job in O(log n), making the full
                      DP fill just O(n log n). The auxiliary <code>idx[]</code> array preserves the
                      original input while sorting.
                    </div>
                  </template>

                </div>
              </div>
            </div>
          </div>

          <!-- Footer -->
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
  --coral: #F04D4D; --coral-dark: #d93e3e; --coral-light: #fff0f0;
  --bg: #f5f6fa; --surface: #ffffff; --surface2: #f1f4f9;
  --border: #e2e8f0; --border2: #cbd5e1;
  --text: #1e293b; --text2: #475569; --muted: #94a3b8;
  --blue: #3b82f6; --blue-light: #eff6ff;
  --green: #22c55e; --green-light: #f0fdf4;
  --orange: #f97316; --orange-light: #fff7ed;
  --purple: #9333ea; --purple-light: #f3e8ff;
  --red: #ef4444; --red-dark: #991b1b; --red-light: #fef2f2;
  --shadow-sm: 0 1px 3px rgba(0,0,0,.08), 0 1px 2px rgba(0,0,0,.04);
  --radius: 8px; --radius-sm: 6px;
  background: var(--bg); color: var(--text);
  font-family: 'Segoe UI', system-ui, sans-serif;
  font-size: 12.5px; display: flex; flex-direction: column;
  height: 74vh; overflow: hidden; width: 100%;
}

@keyframes ll-pulse { 0%, 100% { transform: scale(1); } 50% { transform: scale(1.06); } }

.slide-wrapper { margin-top: -10px; margin-left: -30px; width: 107%; max-height: 100%; font-size: 0.8rem; font-weight: 400; }
.slide-body { display: flex; flex-direction: column; border-radius: 4px; height: 100%; }
.navbar { display: flex; flex-direction: row; justify-content: space-between; align-items: center; gap: 0.75rem; padding: 0 10px; background-color: #ffffff; position: fixed; width: 94.7%; z-index: 50; }
.navbar > img { height: 30px; }
.navbar-title { margin: 0; font-size: 1.35rem; font-weight: 700; background-color: #ef5050; color: #ffffff; width: 80%; padding: 2px 10px; margin-left: -10px; border-radius: 5px; }
.row-main { width: 100%; height: 90%; margin-top: 36px; overflow-x: auto; overflow-y: auto; }

.ll-toolbar { margin-top: 4px; display: flex; align-items: center; gap: 6px; padding: 5px 12px; background: var(--surface); border-bottom: 1px solid var(--border); flex-shrink: 0; flex-wrap: wrap; box-shadow: var(--shadow-sm); }
.ll-approach-group { display: flex; gap: 2px; background: var(--surface2); padding: 2px; border-radius: var(--radius-sm); border: 1px solid var(--border); }
.ll-approach-btn { background: transparent; border: none; padding: 4px 9px; font-size: 11px; font-weight: 600; color: var(--text2); border-radius: 4px; cursor: pointer; transition: all .15s; white-space: nowrap; }
.ll-approach-btn:hover { color: var(--coral); }
.ll-approach-btn.active { background: var(--coral); color: #fff; box-shadow: var(--shadow-sm); }
.ll-input-group { display: flex; align-items: center; gap: 4px; }
.ll-input-group label { font-size: 11px; color: var(--muted); font-weight: 700; }
.ll-text-input { background: var(--surface); border: 1px solid var(--border2); color: var(--text); border-radius: var(--radius-sm); padding: 3px 6px; font-size: 11.5px; font-family: monospace; }
.ll-text-input:focus { outline: none; border-color: var(--coral); box-shadow: 0 0 0 3px rgba(240,77,77,.1); }
.ll-viz-btn { background: var(--coral); color: #fff; border: none; padding: 5px 12px; border-radius: var(--radius-sm); cursor: pointer; font-size: 11.5px; font-weight: 600; box-shadow: var(--shadow-sm); transition: filter .15s; }
.ll-viz-btn:hover { filter: brightness(1.08); }
.ll-nav-controls { display: flex; margin-left: auto; align-items: center; gap: 4px; flex-shrink: 0; flex-wrap: wrap; }
.ll-nav-btn { background: var(--surface2); border: 1px solid var(--border2); color: var(--text2); padding: 4px 9px; border-radius: var(--radius-sm); cursor: pointer; font-size: 11px; font-weight: 500; transition: all .15s; white-space: nowrap; }
.ll-nav-btn:hover { background: var(--surface); border-color: var(--coral); color: var(--coral); }
.ll-play-btn { background: var(--blue-light); border: 1px solid var(--blue); color: var(--blue); min-width: 68px; font-weight: 600; padding: 4px 9px; border-radius: var(--radius-sm); cursor: pointer; font-size: 11px; transition: all .15s; }
.ll-play-btn:hover { background: var(--blue); color: #fff; }

.ll-main { display: flex; flex: 1; overflow: hidden; position: relative; }
.ll-left-col { display: flex; flex-direction: column; overflow: hidden; min-width: 220px; max-width: 75%; }
.ll-resizer { width: 5px; cursor: col-resize; background: var(--border); flex-shrink: 0; transition: background .15s; position: relative; z-index: 20; }
.ll-resizer:hover, .ll-resizer.drag { background: var(--coral); }
.ll-right-col { display: flex; flex-direction: column; flex: 1; overflow: hidden; min-width: 0; height: 100%; }

.ll-house-banner { background: var(--surface); border-bottom: 1px solid var(--border); padding: 5px 10px 7px; flex-shrink: 0; }
.ll-dual-arrays-wrap { display: flex; flex-direction: column; gap: 3px; overflow-x: auto; }
.ll-arr-row { display: flex; align-items: center; gap: 6px; }
.ll-arr-tag { width: 58px; font-size: 10.5px; font-family: monospace; font-weight: 700; color: var(--text2); flex-shrink: 0; }
.ll-arr-cells { display: flex; gap: 7px; }
.ll-arr-cell { display: flex; flex-direction: column; align-items: center; justify-content: center; background: var(--surface2); border: 1px solid var(--border2); border-radius: 5px; width: 42px; padding: 2px 3px; transition: all .2s ease; }
.ll-cell-idx { font-size: 8.5px; color: var(--muted); font-family: monospace; }
.ll-cell-val { font-size: 11px; font-weight: 700; color: var(--text); font-family: monospace; }
.ll-cell-active { border-color: #f59e0b !important; background: #fffbeb !important; box-shadow: 0 0 5px rgba(245,158,11,.45); }
.ll-cell-selected { border-color: #10b981 !important; background: #dcfce7 !important; }

.ll-ptrs { display: flex; gap: 6px; flex-wrap: wrap; padding: 4px 10px; min-height: 28px; background: var(--surface2); border-bottom: 1px solid var(--border); box-sizing: border-box; }
.ll-ptr-chip { background: var(--surface); border: 1px solid var(--border); border-radius: var(--radius-sm); padding: 2px 7px; font-size: 11px; font-family: monospace; box-shadow: var(--shadow-sm); white-space: nowrap; }
.ll-chip-warn { background: #fef2f2 !important; border-color: #fca5a5 !important; }
.ll-c-blue { color: var(--blue); } .ll-c-orange { color: var(--orange); } .ll-c-green { color: var(--green); } .ll-c-purple { color: var(--purple); }

.ll-viz-wrap { flex-shrink: 0; background: var(--surface); position: relative; overflow-x: auto; overflow-y: auto; }
.ll-arr-viz-container { display: flex; flex-direction: column; width: 100%; height: 100%; }
.ll-arr-viz-scroll { flex: 1; overflow: auto; padding: 8px 12px; display: flex; flex-direction: column; gap: 10px; }
.ll-viz-arr-section { display: flex; flex-direction: column; gap: 4px; }
.ll-viz-idx-section { background: #f8f5ff; border: 1px solid #d8b4fe; border-radius: var(--radius-sm); padding: 6px 8px; }
.ll-viz-arr-label { font-size: 10.5px; font-weight: 700; font-family: monospace; color: var(--text2); display: flex; align-items: center; gap: 5px; flex-wrap: wrap; }
.ll-viz-arr-label-idx { color: var(--purple); }
.ll-viz-arr-sub { font-weight: 400; font-size: 9.5px; color: var(--muted); font-family: sans-serif; }
.ll-idx-sort-badge { background: #9333ea; color: #fff; font-size: 8.5px; font-weight: 700; padding: 1px 5px; border-radius: 3px; letter-spacing: .03em; }
.ll-idx-unsort-badge { background: #f59e0b; color: #fff; font-size: 8.5px; font-weight: 700; padding: 1px 5px; border-radius: 3px; }
.ll-idx-init-badge { background: #94a3b8 !important; }
.ll-viz-arr-track { display: flex; gap: 6px; flex-wrap: wrap; align-items: flex-start; }

.ll-viz-cell { display: flex; flex-direction: column; align-items: center; justify-content: center; background: var(--surface); border: 1.5px solid var(--border2); border-radius: 6px; width: 44px; min-height: 48px; padding: 3px 2px; transition: all .22s ease; position: relative; }
.ll-viz-cell-index { font-size: 8px; color: var(--muted); font-family: monospace; }
.ll-viz-cell-val { font-size: 12px; font-weight: 700; color: var(--text); font-family: monospace; line-height: 1.2; }
.ll-viz-cell-ann-idx { font-size: 8px; color: var(--purple); font-family: monospace; margin-top: 1px; }
.ll-viz-cell-idx { background: #f5f0ff; border-color: #c4b5fd; width: 48px; min-height: 54px; }
.ll-viz-cell-idx-active { border-color: #f59e0b !important; background: #fffbeb !important; box-shadow: 0 0 7px rgba(245,158,11,.5); animation: ll-pulse 1.2s infinite ease-in-out; }
.ll-viz-cell-idx-sel { border-color: #94a3b8 !important; opacity: .5; }
.ll-viz-cell-empty-hint { font-size: 10px; color: var(--muted); font-style: italic; padding: 6px 8px; border: 1.5px dashed var(--border2); border-radius: 6px; background: var(--surface2); }
.ll-dp-cell { background: #f1f5f9; border-color: #94a3b8; width: 52px; min-height: 48px; }
.ll-dp-cell-filled { background: #dcfce7 !important; border-color: #10b981 !important; }
.ll-dp-cell-active { border-color: #f59e0b !important; background: #fffbeb !important; box-shadow: 0 0 7px rgba(245,158,11,.5); animation: ll-pulse 1.2s infinite ease-in-out; }
.ll-dp-cell-null { opacity: .5; }
.ll-dp-bs-lo  { border-color: #10b981 !important; background: #dcfce7 !important; }
.ll-dp-bs-hi  { border-color: #9333ea !important; background: #f3e8ff !important; }
.ll-dp-bs-mid { border-color: #3b82f6 !important; background: #dbeafe !important; box-shadow: 0 0 6px rgba(59,130,246,.4); }

.ll-lastfinish-bar { display: flex; align-items: center; gap: 6px; background: #fff7ed; border: 1px solid #fed7aa; border-radius: var(--radius-sm); padding: 4px 10px; font-size: 10.5px; flex-wrap: wrap; }
.ll-lf-label { color: #9a3412; font-weight: 700; } .ll-lf-val { background: #f97316; color: #fff; font-weight: 700; font-family: monospace; font-size: 11px; padding: 1px 6px; border-radius: 4px; } .ll-lf-hint { color: #c2410c; font-size: 9.5px; font-style: italic; }

.ll-tree-container { display: flex; flex-direction: column; width: 100%; height: 100%; }
.ll-tree-scroll-area { flex: 1; overflow: auto; padding: 4px 6px; display: flex; flex-direction: column; align-items: center; min-height: 0; width: 100%; box-sizing: border-box; }
.ll-tree-svg { display: block; flex-shrink: 0; max-width: 100%; margin: 0 auto; }
.ll-tree-edge { stroke: #cbd5e1; stroke-width: 1.5px; }
.ll-edge-pick { stroke: #22c55e !important; stroke-width: 2px !important; }
.ll-edge-skip { stroke: #94a3b8 !important; stroke-dasharray: 4,3 !important; }
.ll-tree-node-group { cursor: default; }
.ll-node-rect { fill: #ffffff; stroke: #cbd5e1; stroke-width: 1.4px; filter: drop-shadow(0 1px 2px rgba(0,0,0,.05)); transition: all .25s ease; }
.ll-node-active { stroke: #f59e0b !important; stroke-width: 2.2px !important; fill: #fffbeb !important; filter: drop-shadow(0 0 5px rgba(245,158,11,.45)) !important; animation: ll-pulse 1.3s infinite ease-in-out; }
.ll-node-solved { stroke: #10b981 !important; fill: #dcfce7 !important; stroke-width: 1.6px !important; }
.ll-node-leaf { stroke: #9333ea !important; fill: #f3e8ff !important; stroke-width: 1.6px !important; }
.ll-tree-svg text { font-family: 'Segoe UI', system-ui, sans-serif !important; user-select: none; }
.ll-node-text-call { font-family: 'Consolas','Fira Code',monospace !important; font-size: 8.5px !important; font-weight: 700 !important; fill: #1e293b !important; dominant-baseline: central !important; }
.ll-node-text-val { font-family: 'Consolas','Fira Code',monospace !important; font-size: 7.5px !important; font-weight: 700 !important; fill: #64748b !important; dominant-baseline: central !important; }
.ll-val-active { fill: #b45309 !important; } .ll-val-solved { fill: #047857 !important; }
.ll-act-table-wrap { width: 100%; padding: 4px 8px; background: var(--surface2); border-top: 1px dashed var(--border); margin-top: 4px; }
.ll-act-table-title { font-size: 10px; font-weight: 700; color: var(--text2); margin-bottom: 3px; }
.ll-act-row-wrap { display: flex; gap: 4px; flex-wrap: wrap; }
.ll-act-chip { background: var(--surface); border: 1px solid var(--border); border-radius: 4px; padding: 2px 6px; font-size: 9.5px; font-family: monospace; }
.ll-act-picked { background: #dcfce7 !important; border-color: #10b981 !important; color: #15803d !important; font-weight: 700; }
.ll-empty-set { font-size: 10px; color: var(--muted); font-style: italic; }

.ll-legend { display: flex; flex-wrap: wrap; gap: 4px 10px; padding: 4px 10px; border-bottom: 1px solid var(--border); flex-shrink: 0; background: var(--surface2); }
.ll-leg { display: flex; align-items: center; gap: 4px; font-size: 10px; color: var(--text2); font-weight: 500; }
.ll-legdot { width: 8px; height: 8px; border-radius: 2px; display: inline-block; }
.ll-legdot-cur { background: #fef3c7; border: 1.5px solid #f59e0b; } .ll-legdot-new { background: #dcfce7; border: 1.5px solid #10b981; }
.ll-legdot-purple { background: #f3e8ff; border: 1.5px solid #9333ea; } .ll-legdot-green { background: #10b981; } .ll-legdot-dash { background: #94a3b8; }

.ll-table-area { flex-shrink: 0; padding: 4px 10px; border-bottom: 1px solid var(--border); overflow-x: hidden; overflow-y: auto; background: var(--surface); min-width: 0; box-sizing: border-box; }
.ll-table-title { font-size: 9.5px; color: var(--muted); margin-bottom: 2px; font-style: italic; }
.ll-stack-line { font-family: 'Consolas', monospace; font-size: 11px; line-height: 1.6; }
.ll-frame { font-family: 'Consolas', monospace; font-size: 10.5px; color: var(--text2); padding: 1px 0; white-space: nowrap; }
.ll-frame-cur { font-weight: 700; color: var(--text); }
.ll-fname { color: var(--muted); } .ll-now { color: var(--orange); font-size: 9px; font-weight: 700; }

.ll-badge-wrap { padding: 4px 10px; flex-shrink: 0; background: var(--surface2); }
.ll-badge { background: #eff6ff; border: 1px solid #bfdbfe; color: #1d4ed8; font-size: 10.5px; font-weight: 600; padding: 3px 8px; border-radius: 4px; }
.ll-badge-error { background: #fef2f2 !important; border-color: #fca5a5 !important; color: #b91c1c !important; }
.ll-badge-success { background: #f0fdf4 !important; border-color: #86efac !important; color: #15803d !important; }

.ll-vresizer { height: 5px; cursor: row-resize; background: var(--border); flex-shrink: 0; transition: background .15s; }
.ll-vresizer:hover, .ll-vresizer.drag { background: var(--coral); }

.ll-code-panel { display: flex; flex-direction: column; height: 100%; background: var(--surface); overflow: hidden; }
.ll-code-header { display: flex; align-items: center; border-bottom: 1px solid var(--border); background: var(--surface2); padding: 0 8px; flex-shrink: 0; }
.ll-tabbar { display: flex; gap: 2px; }
.ll-tab-btn { background: transparent; border: none; padding: 5px 10px; font-size: 11px; font-weight: 600; color: var(--text2); cursor: pointer; border-bottom: 2px solid transparent; transition: all .15s; }
.ll-tab-btn:hover { color: var(--coral); }
.ll-tab-btn.active { color: var(--coral); border-bottom-color: var(--coral); background: var(--surface); }
.ll-lang-select { background: var(--surface); border: 1px solid var(--border2); color: var(--text); padding: 2px 8px; border-radius: var(--radius-sm); font-size: 10.5px; font-weight: 600; margin-left: auto; cursor: pointer; }
.ll-code-scroll { flex: 1; overflow: auto; padding: 6px 10px; background: #ffffff; height: 100%; padding-bottom: 30%; }
.ll-pre { margin: 0; font-family: 'Consolas', 'Fira Code', monospace; font-size: 10.5px; line-height: 1.55; color: #334155; padding-bottom: 50px; }
.ll-codeline { display: block; padding: 0 6px; border-radius: 2px; white-space: pre-wrap; word-break: break-all; }
.ll-hl { background: #dcfce7 !important; color: #15803d !important; font-weight: 700; border-left: 3px solid var(--green); padding-left: 3px; }

.ll-info-scroll { flex: 1; overflow: auto; padding: 10px 14px; color: var(--text); line-height: 1.6; }
.ll-info-scroll h3 { margin: 0 0 6px; font-size: 13px; color: var(--coral-dark); }
.ll-info-scroll h4 { margin: 10px 0 4px; font-size: 11.5px; color: var(--text); }
.ll-info-scroll p, .ll-info-scroll ul { font-size: 10.5px; margin: 0 0 8px; color: var(--text2); }

.ll-complexity-table { width: 100%; border-collapse: collapse; font-size: 10.5px; margin: 8px 0; }
.ll-complexity-table th, .ll-complexity-table td { border: 1px solid var(--border); padding: 4px 8px; text-align: left; }
.ll-complexity-table th { background: var(--surface2); font-weight: 700; color: var(--text2); }

.ll-cx-heading { font-size: 13px; font-weight: 700; color: var(--text); margin: 0 0 6px; }
.ll-cx-intro { font-size: 10.5px; color: var(--text2); margin: 0 0 10px; line-height: 1.55; }
.ll-cx-sub { font-size: 11px; font-weight: 700; color: var(--text2); margin: 10px 0 4px; border-bottom: 1px solid var(--border); padding-bottom: 3px; }
.ll-cx-good { color: #15803d; font-weight: 700; } .ll-cx-mid { color: #b45309; font-weight: 700; } .ll-cx-bad { color: #b91c1c; font-weight: 700; }
.ll-cx-summary-grid { display: flex; gap: 8px; flex-wrap: wrap; margin: 6px 0 10px; }
.ll-cx-card { flex: 1; min-width: 90px; border-radius: var(--radius-sm); padding: 8px 10px; text-align: center; border: 1.5px solid var(--border); }
.ll-cx-card-label { font-size: 9px; font-weight: 700; text-transform: uppercase; letter-spacing: .05em; opacity: .7; margin-bottom: 4px; }
.ll-cx-card-val { font-size: 13px; font-weight: 800; font-family: monospace; margin-bottom: 3px; }
.ll-cx-card-note { font-size: 8.5px; opacity: .75; line-height: 1.3; }
.ll-cx-card-good { background: #f0fdf4; border-color: #86efac; color: #15803d; }
.ll-cx-card-mid  { background: #fff7ed; border-color: #fed7aa; color: #c2410c; }
.ll-cx-card-bad  { background: #fef2f2; border-color: #fca5a5; color: #b91c1c; }
.ll-note { background: #fefce8; border: 1px solid #fef08a; border-left: 3px solid #eab308; padding: 6px 10px; font-size: 10.5px; color: #854d0e; border-radius: 0 4px 4px 0; margin-top: 10px; margin-bottom: 120px; }

.ll-footer { display: flex; align-items: center; justify-content: space-between; padding: 4px 12px; background: var(--surface); border-top: 1px solid var(--border); font-size: 11px; color: var(--muted); flex-shrink: 0; }
.ll-speed-wrap { display: flex; align-items: center; gap: 6px; }
.ll-speed-wrap input { width: 80px; accent-color: var(--coral); }
</style>
