<script setup>
import { ref, reactive, computed, onMounted, onUnmounted, watch } from 'vue';

const props = defineProps({
  topic: { type: String, default: 'Greedy Algorithms' },
  subTopic: { type: String, default: 'Fractional Knapsack Problem' }
});

/* ==================================================================== */
/* APPROACH METADATA                                                    */
/* Brute Force comes before Greedy                                      */
/* ==================================================================== */
const APPROACHES = [
  { id: 'brute', label: 'Brute Force', desc: 'O(2^n · n) Exponential — Recursive Include/Exclude Decision Tree with Fractional Fill' },
  { id: 'greedy', label: 'Greedy', desc: 'O(n log n) Time & O(1) Auxiliary Space — Highest Value-to-Weight Ratio Strip' }
];

/* ==================================================================== */
/* CODE SNIPPETS (Java, C, C++, Python)                                 */
/* Multiple parallel arrays wt[] and val[] — NO array of objects        */
/* Strict curly brace formatting on all control statements              */
/* ==================================================================== */
const CODES = {
  brute: {
    java: [
      ['',                     'import java.util.Scanner;'],
      ['',                     ''],
      ['',                     'public class Main {'],
      ['',                     '    static double maxVal = 0.0;'],
      ['',                     ''],
      ['c_entry',              '    static void solve(int idx, int curW, double curV, int[] wt, int[] val, int n, int W, boolean[] taken) {'],
      ['c_base_check',         '        if (idx == n) {'],
      ['c_calc_rem',           '            int remain = W - curW;'],
      ['c_init_best_frac',     '            double bestFrac = 0.0;'],
      ['c_check_rem',          '            if (remain > 0) {'],
      ['c_for_unused',         '                for (int i = 0; i < n; i++) {'],
      ['c_check_taken',        '                    if (!taken[i]) {'],
      ['c_calc_frac',          '                        double frac = (double) remain / wt[i];'],
      ['c_check_frac_clamp',   '                        if (frac > 1.0) {'],
      ['c_clamp_frac',         '                            frac = 1.0;'],
      ['',                     '                        }'],
      ['c_calc_frac_val',      '                        double gain = val[i] * frac;'],
      ['c_check_best_frac',    '                        if (gain > bestFrac) {'],
      ['c_update_best_frac',   '                            bestFrac = gain;'],
      ['',                     '                        }'],
      ['',                     '                    }'],
      ['',                     '                }'],
      ['',                     '            }'],
      ['c_check_global_max',   '            if (curV + bestFrac > maxVal) {'],
      ['c_update_global_max',  '                maxVal = curV + bestFrac;'],
      ['',                     '            }'],
      ['c_ret_base',           '            return;'],
      ['',                     '        }'],
      ['c_set_skip',           '        taken[idx] = false;'],
      ['c_call_skip',          '        solve(idx + 1, curW, curV, wt, val, n, W, taken);'],
      ['c_check_fit',          '        if (curW + wt[idx] <= W) {'],
      ['c_set_take',           '            taken[idx] = true;'],
      ['c_call_take',          '            solve(idx + 1, curW + wt[idx], curV + val[idx], wt, val, n, W, taken);'],
      ['c_reset_take',         '            taken[idx] = false;'],
      ['',                     '        }'],
      ['',                     '    }'],
      ['',                     ''],
      ['',                     '    public static void main(String[] args) {'],
      ['m_scanner',            '        Scanner sc = new Scanner(System.in);'],
      ['m_read_n',             '        int n = sc.nextInt();'],
      ['m_read_w',             '        int W = sc.nextInt();'],
      ['m_alloc_arr',          '        int[] wt = new int[n]; int[] val = new int[n];'],
      ['m_for_input',          '        for (int i = 0; i < n; i++) {'],
      ['m_read_item',          '            wt[i] = sc.nextInt(); val[i] = sc.nextInt();'],
      ['',                     '        }'],
      ['m_alloc_taken',        '        boolean[] taken = new boolean[n];'],
      ['m_init_max',           '        maxVal = 0.0;'],
      ['m_call_solve',         '        solve(0, 0, 0.0, wt, val, n, W, taken);'],
      ['m_print_ans',          '        System.out.printf("%.2f\\n", maxVal);'],
      ['m_done',               '    }'],
      ['',                     '}']
    ],
    c: [
      ['',                     '#include <stdio.h>'],
      ['',                     ''],
      ['',                     'double maxVal = 0.0;'],
      ['',                     ''],
      ['c_entry',              'void solve(int idx, int curW, double curV, int wt[], int val[], int n, int W, int taken[]) {'],
      ['c_base_check',         '    if (idx == n) {'],
      ['c_calc_rem',           '        int remain = W - curW;'],
      ['c_init_best_frac',     '        double bestFrac = 0.0;'],
      ['c_check_rem',          '        if (remain > 0) {'],
      ['c_for_unused',         '            for (int i = 0; i < n; i++) {'],
      ['c_check_taken',        '                if (!taken[i]) {'],
      ['c_calc_frac',          '                    double frac = (double)remain / wt[i];'],
      ['c_check_frac_clamp',   '                    if (frac > 1.0) {'],
      ['c_clamp_frac',         '                        frac = 1.0;'],
      ['',                     '                    }'],
      ['c_calc_frac_val',      '                    double gain = val[i] * frac;'],
      ['c_check_best_frac',    '                    if (gain > bestFrac) {'],
      ['c_update_best_frac',   '                        bestFrac = gain;'],
      ['',                     '                    }'],
      ['',                     '                }'],
      ['',                     '            }'],
      ['',                     '        }'],
      ['c_check_global_max',   '        if (curV + bestFrac > maxVal) {'],
      ['c_update_global_max',  '            maxVal = curV + bestFrac;'],
      ['',                     '        }'],
      ['c_ret_base',           '        return;'],
      ['',                     '    }'],
      ['c_set_skip',           '    taken[idx] = 0;'],
      ['c_call_skip',          '    solve(idx + 1, curW, curV, wt, val, n, W, taken);'],
      ['c_check_fit',          '    if (curW + wt[idx] <= W) {'],
      ['c_set_take',           '        taken[idx] = 1;'],
      ['c_call_take',          '        solve(idx + 1, curW + wt[idx], curV + val[idx], wt, val, n, W, taken);'],
      ['c_reset_take',         '        taken[idx] = 0;'],
      ['',                     '    }'],
      ['',                     '}'],
      ['',                     ''],
      ['',                     'int main() {'],
      ['m_scanner',            '    int n, W;'],
      ['m_read_n',             '    scanf("%d", &n);'],
      ['m_read_w',             '    scanf("%d", &W);'],
      ['m_alloc_arr',          '    int wt[100]; int val[100];'],
      ['m_for_input',          '    for (int i = 0; i < n; i++) {'],
      ['m_read_item',          '        scanf("%d %d", &wt[i], &val[i]);'],
      ['',                     '    }'],
      ['m_alloc_taken',        '    int taken[100] = {0};'],
      ['m_init_max',           '    maxVal = 0.0;'],
      ['m_call_solve',         '    solve(0, 0, 0.0, wt, val, n, W, taken);'],
      ['m_print_ans',          '    printf("%.2f\\n", maxVal);'],
      ['m_done',               '    return 0;'],
      ['',                     '}']
    ],
    cpp: [
      ['',                     '#include <iostream>'],
      ['',                     '#include <vector>'],
      ['',                     '#include <iomanip>'],
      ['',                     'using namespace std;'],
      ['',                     ''],
      ['',                     'double maxVal = 0.0;'],
      ['',                     ''],
      ['c_entry',              'void solve(int idx, int curW, double curV, const vector<int>& wt, const vector<int>& val, int n, int W, vector<int>& taken) {'],
      ['c_base_check',         '    if (idx == n) {'],
      ['c_calc_rem',           '        int remain = W - curW;'],
      ['c_init_best_frac',     '        double bestFrac = 0.0;'],
      ['c_check_rem',          '        if (remain > 0) {'],
      ['c_for_unused',         '            for (int i = 0; i < n; i++) {'],
      ['c_check_taken',        '                if (!taken[i]) {'],
      ['c_calc_frac',          '                    double frac = (double)remain / wt[i];'],
      ['c_check_frac_clamp',   '                    if (frac > 1.0) {'],
      ['c_clamp_frac',         '                        frac = 1.0;'],
      ['',                     '                    }'],
      ['c_calc_frac_val',      '                    double gain = val[i] * frac;'],
      ['c_check_best_frac',    '                    if (gain > bestFrac) {'],
      ['c_update_best_frac',   '                        bestFrac = gain;'],
      ['',                     '                    }'],
      ['',                     '                }'],
      ['',                     '            }'],
      ['',                     '        }'],
      ['c_check_global_max',   '        if (curV + bestFrac > maxVal) {'],
      ['c_update_global_max',  '            maxVal = curV + bestFrac;'],
      ['',                     '        }'],
      ['c_ret_base',           '        return;'],
      ['',                     '    }'],
      ['c_set_skip',           '    taken[idx] = 0;'],
      ['c_call_skip',          '    solve(idx + 1, curW, curV, wt, val, n, W, taken);'],
      ['c_check_fit',          '    if (curW + wt[idx] <= W) {'],
      ['c_set_take',           '        taken[idx] = 1;'],
      ['c_call_take',          '        solve(idx + 1, curW + wt[idx], curV + val[idx], wt, val, n, W, taken);'],
      ['c_reset_take',         '        taken[idx] = 0;'],
      ['',                     '    }'],
      ['',                     '}'],
      ['',                     ''],
      ['',                     'int main() {'],
      ['m_scanner',            '    int n, W;'],
      ['m_read_n',             '    cin >> n;'],
      ['m_read_w',             '    cin >> W;'],
      ['m_alloc_arr',          '    vector<int> wt(n); vector<int> val(n);'],
      ['m_for_input',          '    for (int i = 0; i < n; i++) {'],
      ['m_read_item',          '        cin >> wt[i] >> val[i];'],
      ['',                     '    }'],
      ['m_alloc_taken',        '    vector<int> taken(n, 0);'],
      ['m_init_max',           '    maxVal = 0.0;'],
      ['m_call_solve',         '    solve(0, 0, 0.0, wt, val, n, W, taken);'],
      ['m_print_ans',          '    cout << fixed << setprecision(2) << maxVal << endl;'],
      ['m_done',               '    return 0;'],
      ['',                     '}']
    ],
    python: [
      ['',                     'import sys'],
      ['',                     ''],
      ['',                     'max_val = 0.0'],
      ['',                     ''],
      ['c_entry',              'def solve(idx, cur_w, cur_v, wt, val, n, W, taken):'],
      ['',                     '    global max_val'],
      ['c_base_check',         '    if idx == n:'],
      ['c_calc_rem',           '        remain = W - cur_w'],
      ['c_init_best_frac',     '        best_frac = 0.0'],
      ['c_check_rem',          '        if remain > 0:'],
      ['c_for_unused',         '            for i in range(n):'],
      ['c_check_taken',        '                if not taken[i]:'],
      ['c_calc_frac',          '                    frac = remain / wt[i]'],
      ['c_check_frac_clamp',   '                    if frac > 1.0:'],
      ['c_clamp_frac',         '                        frac = 1.0'],
      ['c_calc_frac_val',      '                    gain = val[i] * frac'],
      ['c_check_best_frac',    '                    if gain > best_frac:'],
      ['c_update_best_frac',   '                        best_frac = gain'],
      ['c_check_global_max',   '        if cur_v + best_frac > max_val:'],
      ['c_update_global_max',  '            max_val = cur_v + best_frac'],
      ['c_ret_base',           '        return'],
      ['c_set_skip',           '    taken[idx] = False'],
      ['c_call_skip',          '    solve(idx + 1, cur_w, cur_v, wt, val, n, W, taken)'],
      ['c_check_fit',          '    if cur_w + wt[idx] <= W:'],
      ['c_set_take',           '        taken[idx] = True'],
      ['c_call_take',          '        solve(idx + 1, cur_w + wt[idx], cur_v + val[idx], wt, val, n, W, taken)'],
      ['c_reset_take',         '        taken[idx] = False'],
      ['',                     ''],
      ['',                     'def main():'],
      ['',                     '    global max_val'],
      ['m_scanner',            '    tokens = sys.stdin.read().split()'],
      ['m_read_n',             '    if not tokens: return'],
      ['',                     '    n = int(tokens[0])'],
      ['m_read_w',             '    W = int(tokens[1])'],
      ['m_alloc_arr',          '    wt = []; val = []'],
      ['m_for_input',          '    idx = 2'],
      ['',                     '    for i in range(n):'],
      ['m_read_item',          '        wt.append(int(tokens[idx])); val.append(int(tokens[idx + 1])); idx += 2'],
      ['m_alloc_taken',        '    taken = [False] * n'],
      ['m_init_max',           '    max_val = 0.0'],
      ['m_call_solve',         '    solve(0, 0, 0.0, wt, val, n, W, taken)'],
      ['m_print_ans',          '    print(f"{max_val:.2f}")'],
      ['m_done',               '    return'],
      ['',                     ''],
      ['',                     'if __name__ == "__main__":'],
      ['',                     '    main()']
    ]
  },
  greedy: {
    java: [
      ['',                     'import java.util.Scanner;'],
      ['',                     'import java.util.Arrays;'],
      ['',                     ''],
      ['',                     'public class Main {'],
      ['c_entry',              '    static double fractionalKnapsack(int W, int[] wt, int[] val, int n) {'],
      ['c_init_index',         '        Integer[] index = new Integer[n]; for (int i = 0; i < n; i++) { index[i] = i; }'],
      ['c_sort_ratio',         '        Arrays.sort(index, (a, b) -> Double.compare((double) val[b] / wt[b], (double) val[a] / wt[a]));'],
      ['c_init_val',           '        double totalVal = 0.0;'],
      ['c_init_cap',           '        int remain = W;'],
      ['c_for_items',          '        for (int i = 0; i < n; i++) {'],
      ['c_get_idx',            '            int idx = index[i];'],
      ['c_check_fit',          '            if (remain >= wt[idx]) {'],
      ['c_take_full_cap',      '                remain -= wt[idx];'],
      ['c_take_full_val',      '                totalVal += val[idx];'],
      ['c_else_frac',          '            } else {'],
      ['c_calc_frac',          '                totalVal += val[idx] * ((double) remain / wt[idx]);'],
      ['c_zero_cap',           '                remain = 0;'],
      ['c_break',              '                break;'],
      ['',                     '            }'],
      ['',                     '        }'],
      ['c_ret_val',            '        return totalVal;'],
      ['',                     '    }'],
      ['',                     ''],
      ['',                     '    public static void main(String[] args) {'],
      ['m_scanner',            '        Scanner sc = new Scanner(System.in);'],
      ['m_read_n',             '        int n = sc.nextInt();'],
      ['m_read_w',             '        int W = sc.nextInt();'],
      ['m_alloc_arr',          '        int[] wt = new int[n]; int[] val = new int[n];'],
      ['m_for_input',          '        for (int i = 0; i < n; i++) {'],
      ['m_read_item',          '            wt[i] = sc.nextInt(); val[i] = sc.nextInt();'],
      ['',                     '        }'],
      ['m_call_knapsack',      '        double ans = fractionalKnapsack(W, wt, val, n);'],
      ['m_print_ans',          '        System.out.printf("%.2f\\n", ans);'],
      ['m_done',               '    }'],
      ['',                     '}']
    ],
    c: [
      ['',                     '#include <stdio.h>'],
      ['',                     '#include <stdlib.h>'],
      ['',                     ''],
      ['',                     'int g_wt[100]; int g_val[100];'],
      ['',                     'int compareRatio(const void* a, const void* b) {'],
      ['',                     '    int i1 = *(const int*)a; int i2 = *(const int*)b;'],
      ['',                     '    double r1 = (double)g_val[i1] / g_wt[i1];'],
      ['',                     '    double r2 = (double)g_val[i2] / g_wt[i2];'],
      ['',                     '    if (r2 > r1) { return 1; } else if (r2 < r1) { return -1; } else { return 0; }'],
      ['',                     '}'],
      ['',                     ''],
      ['c_entry',              'double fractionalKnapsack(int W, int wt[], int val[], int n) {'],
      ['c_init_index',         '    int index[100]; for (int i = 0; i < n; i++) { index[i] = i; g_wt[i] = wt[i]; g_val[i] = val[i]; }'],
      ['c_sort_ratio',         '    qsort(index, n, sizeof(int), compareRatio);'],
      ['c_init_val',           '    double totalVal = 0.0;'],
      ['c_init_cap',           '    int remain = W;'],
      ['c_for_items',          '    for (int i = 0; i < n; i++) {'],
      ['c_get_idx',            '        int idx = index[i];'],
      ['c_check_fit',          '        if (remain >= wt[idx]) {'],
      ['c_take_full_cap',      '            remain -= wt[idx];'],
      ['c_take_full_val',      '            totalVal += val[idx];'],
      ['c_else_frac',          '        } else {'],
      ['c_calc_frac',          '            totalVal += val[idx] * ((double)remain / wt[idx]);'],
      ['c_zero_cap',           '            remain = 0;'],
      ['c_break',              '            break;'],
      ['',                     '        }'],
      ['',                     '    }'],
      ['c_ret_val',            '    return totalVal;'],
      ['',                     '}'],
      ['',                     ''],
      ['',                     'int main() {'],
      ['m_scanner',            '    int n, W;'],
      ['m_read_n',             '    scanf("%d", &n);'],
      ['m_read_w',             '    scanf("%d", &W);'],
      ['m_alloc_arr',          '    int wt[100]; int val[100];'],
      ['m_for_input',          '    for (int i = 0; i < n; i++) {'],
      ['m_read_item',          '        scanf("%d %d", &wt[i], &val[i]);'],
      ['',                     '    }'],
      ['m_call_knapsack',      '    double ans = fractionalKnapsack(W, wt, val, n);'],
      ['m_print_ans',          '    printf("%.2f\\n", ans);'],
      ['m_done',               '    return 0;'],
      ['',                     '}']
    ],
    cpp: [
      ['',                     '#include <iostream>'],
      ['',                     '#include <vector>'],
      ['',                     '#include <algorithm>'],
      ['',                     '#include <iomanip>'],
      ['',                     'using namespace std;'],
      ['',                     ''],
      ['c_entry',              'double fractionalKnapsack(int W, const vector<int>& wt, const vector<int>& val, int n) {'],
      ['c_init_index',         '    vector<int> index(n); for (int i = 0; i < n; i++) { index[i] = i; }'],
      ['c_sort_ratio',         '    sort(index.begin(), index.end(), [&](int a, int b) { return (double)val[a] / wt[a] > (double)val[b] / wt[b]; });'],
      ['c_init_val',           '    double totalVal = 0.0;'],
      ['c_init_cap',           '    int remain = W;'],
      ['c_for_items',          '    for (int i = 0; i < n; i++) {'],
      ['c_get_idx',            '        int idx = index[i];'],
      ['c_check_fit',          '        if (remain >= wt[idx]) {'],
      ['c_take_full_cap',      '            remain -= wt[idx];'],
      ['c_take_full_val',      '            totalVal += val[idx];'],
      ['c_else_frac',          '        } else {'],
      ['c_calc_frac',          '            totalVal += val[idx] * ((double)remain / wt[idx]);'],
      ['c_zero_cap',           '            remain = 0;'],
      ['c_break',              '            break;'],
      ['',                     '        }'],
      ['',                     '    }'],
      ['c_ret_val',            '    return totalVal;'],
      ['',                     '}'],
      ['',                     ''],
      ['',                     'int main() {'],
      ['m_scanner',            '    int n, W;'],
      ['m_read_n',             '    cin >> n;'],
      ['m_read_w',             '    cin >> W;'],
      ['m_alloc_arr',          '    vector<int> wt(n); vector<int> val(n);'],
      ['m_for_input',          '    for (int i = 0; i < n; i++) {'],
      ['m_read_item',          '        cin >> wt[i] >> val[i];'],
      ['',                     '    }'],
      ['m_call_knapsack',      '        double ans = fractionalKnapsack(W, wt, val, n);'],
      ['m_print_ans',          '    cout << fixed << setprecision(2) << ans << endl;'],
      ['m_done',               '    return 0;'],
      ['',                     '}']
    ],
    python: [
      ['',                     'import sys'],
      ['',                     ''],
      ['c_entry',              'def fractional_knapsack(W, wt, val, n):'],
      ['c_init_index',         '    index = list(range(n))'],
      ['c_sort_ratio',         '    index.sort(key=lambda i: val[i] / wt[i], reverse=True)'],
      ['c_init_val',           '    total_val = 0.0'],
      ['c_init_cap',           '    remain = W'],
      ['c_for_items',          '    for i in range(n):'],
      ['c_get_idx',            '        idx = index[i]'],
      ['c_check_fit',          '        if remain >= wt[idx]:'],
      ['c_take_full_cap',      '            remain -= wt[idx]'],
      ['c_take_full_val',      '            total_val += val[idx]'],
      ['c_else_frac',          '        else:'],
      ['c_calc_frac',          '            total_val += val[idx] * (remain / wt[idx])'],
      ['c_zero_cap',           '            remain = 0'],
      ['c_break',              '            break'],
      ['c_ret_val',            '    return total_val'],
      ['',                     'def main():'],
      ['m_scanner',            '    tokens = sys.stdin.read().split()'],
      ['m_read_n',             '    if not tokens: return'],
      ['',                     '    n = int(tokens[0])'],
      ['m_read_w',             '    W = int(tokens[1])'],
      ['m_alloc_arr',          '    wt = []; val = []'],
      ['m_for_input',          '    idx = 2'],
      ['',                     '    for i in range(n):'],
      ['m_read_item',          '        wt.append(int(tokens[idx])); val.append(int(tokens[idx + 1])); idx += 2'],
      ['m_call_knapsack',      '    ans = fractional_knapsack(W, wt, val, n)'],
      ['m_print_ans',          '    print(f"{ans:.2f}")'],
      ['m_done',               '    return'],
      ['',                     ''],
      ['',                     'if __name__ == "__main__":'],
      ['',                     '    main()']
    ]
  }
};

/* ==================================================================== */
/* PSEUDOCODE FOR STUDENTS (PARALLEL ARRAYS)                            */
/* ==================================================================== */
const PSEUDOCODES = {
  brute: [
    'function solve(idx, curW, curV, wt, val, n, W, taken):',
    '    if idx == n:                                 // All items decided: evaluate leaf',
    '        remain = W - curW, bestFrac = 0.0',
    '        if remain > 0:                           // Fill leftover capacity with best unused fraction',
    '            for i = 0 to n - 1:',
    '                if not taken[i]:',
    '                    frac = min(1.0, remain / wt[i])',
    '                    bestFrac = max(bestFrac, val[i] * frac)',
    '        maxVal = max(maxVal, curV + bestFrac)    // Update global maximum profit',
    '        return',
    '    taken[idx] = false',
    '    solve(idx + 1, curW, curV, wt, val, n, W, taken)         // Branch 1: Skip item idx',
    '    if curW + wt[idx] <= W:                                  // Branch 2: Take item idx fully',
    '        taken[idx] = true',
    '        solve(idx + 1, curW + wt[idx], curV + val[idx], wt, val, n, W, taken)',
    '        taken[idx] = false'
  ],
  greedy: [
    'function fractionalKnapsack(W, wt, val, n):',
    '    index = [0, 1, ..., n - 1]',
    '    sort index by ratio (val[i] / wt[i]) descending   // Greedy Choice: Highest density first',
    '    totalVal = 0.0, remain = W',
    '    for i = 0 to n - 1:                               // Iterate over prioritized indices',
    '        idx = index[i]',
    '        if remain >= wt[idx]:                         // Knapsack has room for full item',
    '            remain = remain - wt[idx]',
    '            totalVal = totalVal + val[idx]',
    '        else:                                         // Take fractional slice and finish',
    '            totalVal = totalVal + val[idx] * (remain / wt[idx])',
    '            remain = 0',
    '            break',
    '    return totalVal                                   // Optimal Time: O(n log n), Space: O(1)'
  ]
};

/* ==================================================================== */
/* DEFAULT SAMPLE DATA                                                  */
/* ==================================================================== */
const DEFAULT_WEIGHTS = [10, 20, 30];
const DEFAULT_VALUES = [60, 100, 120];
const DEFAULT_W = 50;

function frame(title, rows) {
  return { title, rows };
}

/* ==================================================================== */
/* BUILD STEPS FOR ANIMATION (STRICT 1-TO-1 LINE-BY-LINE)               */
/* ==================================================================== */
function buildSteps(approach, rawWeights, rawValues, rawW) {
  const steps = [];
  const wt = (rawWeights && rawWeights.length) ? [...rawWeights] : [...DEFAULT_WEIGHTS];
  const val = (rawValues && rawValues.length) ? [...rawValues] : [...DEFAULT_VALUES];
  const n = Math.min(wt.length, val.length);
  const W = Number(rawW) > 0 ? Number(rawW) : DEFAULT_W;

  /* ------------------------------------------------------------------ */
  /* APPROACH 1: BRUTE FORCE (DECISION TREE WITH MULTIPLE ARRAYS)       */
  /* ------------------------------------------------------------------ */
  if (approach === 'brute') {
    const allNodes = [];
    let nodeIdCounter = 0;

    function buildBruteTreeStructure(idx, curW, curV, takenArr, parentId, branchType, label) {
      const nodeId = nodeIdCounter++;
      const node = {
        id: nodeId,
        idx,
        curW,
        curV,
        branchType,
        label,
        parentId,
        children: [],
        isLeaf: (idx === n),
        subResult: 0
      };
      allNodes.push(node);
      if (parentId !== null) {
        const par = allNodes.find(x => x.id === parentId);
        if (par) par.children.push(nodeId);
      }

      if (idx === n) {
        const remain = W - curW;
        let bestFrac = 0.0;
        if (remain > 0) {
          for (let i = 0; i < n; i++) {
            if (!takenArr[i]) {
              const frac = Math.min(1.0, remain / wt[i]);
              const gain = val[i] * frac;
              if (gain > bestFrac) bestFrac = gain;
            }
          }
        }
        node.subResult = curV + bestFrac;
        return node.subResult;
      }

      const nextTakenSkip = [...takenArr];
      nextTakenSkip[idx] = false;
      const skipRes = buildBruteTreeStructure(idx + 1, curW, curV, nextTakenSkip, nodeId, 'nottake', 'Skip [' + idx + ']');

      let takeRes = 0;
      if (curW + wt[idx] <= W) {
        const nextTakenTake = [...takenArr];
        nextTakenTake[idx] = true;
        takeRes = buildBruteTreeStructure(idx + 1, curW + wt[idx], curV + val[idx], nextTakenTake, nodeId, 'take', '+wt[' + idx + ']');
      }

      node.subResult = Math.max(skipRes, takeRes);
      return node.subResult;
    }

    buildBruteTreeStructure(0, 0, 0.0, new Array(n).fill(false), null, 'root', 'Root');

    const nodeStateMap = {};
    allNodes.forEach(nd => {
      nodeStateMap[nd.id] = { ...nd, state: 'hidden', retVal: null };
    });

    const leafSpacing = 52;
    const levelHeight = 44;
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
    const treeWidth = Math.max(280, totalLeaves * leafSpacing + 40);
    const treeHeight = Math.max(140, maxDepth * levelHeight + 54);

    function assignCoords(nodeId) {
      const nd = allNodes.find(x => x.id === nodeId);
      if (!nd) return 0;
      if (nd.children.length === 0) {
        nd._x = 24 + nd._leafIdx * leafSpacing + leafSpacing / 2;
      } else {
        const childXs = nd.children.map(cid => assignCoords(cid));
        nd._x = (childXs[0] + childXs[childXs.length - 1]) / 2;
      }
      nd._y = 24 + nd._depth * levelHeight;
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
              x1: par._x, y1: par._y + 12,
              x2: nd._x, y2: nd._y - 12,
              branchType: nd.branchType,
              label: nd.label,
              isOptimal: nd.subResult === allNodes[0].subResult
            });
          }
        }
      });
      return edges;
    }

    function curBruteState(extra = {}) {
      return {
        approach: 'brute',
        n,
        W,
        wt,
        val,
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
      vars: [frame('main()', [['n', '?'], ['W', '?']])],
      state: curBruteState({ callCounter: 0, activeNodeId: null, maxVal: 0.0, currentIdx: -1, numsFilled: 0 })
    });
    steps.push({
      code: 'm_read_n',
      badge: 'Faculty Note: int n = sc.nextInt(); &rarr; Read number of items n = ' + n + '.',
      vars: [frame('main()', [['n', String(n)], ['W', '?']])],
      state: curBruteState({ callCounter: 0, activeNodeId: null, maxVal: 0.0, currentIdx: -1, numsFilled: 0 })
    });
    steps.push({
      code: 'm_read_w',
      badge: 'Faculty Note: int W = sc.nextInt(); &rarr; Read knapsack capacity W = ' + W + ' kg.',
      vars: [frame('main()', [['n', String(n)], ['W', String(W)]])],
      state: curBruteState({ callCounter: 0, activeNodeId: null, maxVal: 0.0, currentIdx: -1, numsFilled: 0 })
    });
    steps.push({
      code: 'm_alloc_arr',
      badge: 'Faculty Note: int[] wt = new int[' + n + ']; int[] val = new int[' + n + ']; — Allocating parallel arrays.',
      vars: [frame('main()', [['n', String(n)], ['W', String(W)]])],
      state: curBruteState({ callCounter: 0, activeNodeId: null, maxVal: 0.0, currentIdx: -1, numsFilled: 0 })
    });

    for (let idx = 0; idx < n; idx++) {
      steps.push({
        code: 'm_for_input',
        badge: 'Reading inputs for item index ' + idx + '.',
        vars: [frame('main()', [['i', String(idx)], ['n', String(n)]])],
        state: curBruteState({ callCounter: 0, activeNodeId: null, maxVal: 0.0, currentIdx: idx, numsFilled: idx })
      });
      steps.push({
        code: 'm_read_item',
        badge: 'wt[' + idx + '] = sc.nextInt(); val[' + idx + '] = sc.nextInt(); \u2192 wt[' + idx + '] = ' + wt[idx] + ' kg, val[' + idx + '] = $' + val[idx] + '.',
        vars: [frame('main()', [['i', String(idx)], ['wt[' + idx + ']', String(wt[idx])], ['val[' + idx + ']', String(val[idx])]])],
        state: curBruteState({ callCounter: 0, activeNodeId: null, maxVal: 0.0, currentIdx: idx, numsFilled: idx + 1 })
      });
    }

    steps.push({
      code: 'm_alloc_taken',
      badge: 'boolean[] taken = new boolean[' + n + ']; — Allocating decision tracker array of size ' + n + ' (initialized to false).',
      vars: [frame('main()', [['n', String(n)], ['W', String(W)], ['taken', '[' + new Array(n).fill('false').join(', ') + ']']])],
      state: curBruteState({ callCounter: 0, activeNodeId: null, maxVal: 0.0, currentIdx: -1, taken: new Array(n).fill(false) })
    });

    steps.push({
      code: 'm_init_max',
      badge: 'maxVal = 0.0; — Initializing global best solution tracker to 0.0.',
      vars: [frame('main()', [['maxVal', '0.00'], ['taken', '[' + new Array(n).fill('false').join(', ') + ']']])],
      state: curBruteState({ callCounter: 0, activeNodeId: null, maxVal: 0.0, currentIdx: -1, taken: new Array(n).fill(false) })
    });

    steps.push({
      code: 'm_call_solve',
      badge: 'Invoking solve(idx=0, curW=0, curV=0.0, wt, val, n=' + n + ', W=' + W + ', taken). Exhaustive decision tree begins!',
      vars: [frame('main()', [['maxVal', '0.00'], ['W', String(W)], ['taken', '[' + new Array(n).fill('false').join(', ') + ']']])],
      state: curBruteState({ callCounter: 0, activeNodeId: 0, maxVal: 0.0, currentIdx: 0, taken: new Array(n).fill(false) })
    });

    let calls = 0;
    let globalMax = 0.0;

    function simulateBrute(idx, curW, curV, takenArr, nodeId, callStack) {
      calls++;
      nodeStateMap[nodeId].state = 'active';
      const cs = [
        frame('main()', [['maxVal', globalMax.toFixed(2)], ['W', String(W)]]),
        ...callStack,
        frame('solve(idx=' + idx + ')', [['curW', String(curW)], ['curV', curV.toFixed(2)], ['remain', String(W - curW)], ['taken', '[' + takenArr.map(t => t ? 'T' : 'F').join(',') + ']']])
      ];

      steps.push({
        code: 'c_entry',
        badge: 'Entering solve(idx=' + idx + ', curW=' + curW + ', curV=' + curV.toFixed(2) + '). Call #' + calls + '.',
        vars: cs,
        state: curBruteState({ callCounter: calls, activeNodeId: nodeId, maxVal: globalMax, activeIdx: idx, curW, curV, taken: [...takenArr] })
      });

      const isBase = (idx === n);
      steps.push({
        code: 'c_base_check',
        badge: 'Base check: if (idx == n) &rarr; ' + (isBase ? 'TRUE (Leaf node reached! All items decided)' : 'FALSE (Branching into Skip and Take)'),
        vars: cs,
        state: curBruteState({ callCounter: calls, activeNodeId: nodeId, maxVal: globalMax, activeIdx: idx, curW, curV, taken: [...takenArr] })
      });

      if (isBase) {
        nodeStateMap[nodeId].state = 'leaf';
        const remain = W - curW;
        steps.push({
          code: 'c_calc_rem',
          badge: 'int remain = W - curW = ' + W + ' - ' + curW + ' = ' + remain + ' kg remaining.',
          vars: cs,
          state: curBruteState({ callCounter: calls, activeNodeId: nodeId, maxVal: globalMax, activeIdx: idx, curW, curV, remain, taken: [...takenArr] })
        });

        let bestFrac = 0.0;
        let bestItemIdx = -1;
        steps.push({
          code: 'c_init_best_frac',
          badge: 'double bestFrac = 0.0; — Finding maximum fractional gain from unused items.',
          vars: cs,
          state: curBruteState({ callCounter: calls, activeNodeId: nodeId, maxVal: globalMax, activeIdx: idx, curW, curV, remain, bestFrac, taken: [...takenArr] })
        });

        const hasRem = (remain > 0);
        steps.push({
          code: 'c_check_rem',
          badge: 'if (remain > 0) &rarr; ' + (hasRem ? 'TRUE (Can fill remaining space with fractional slice)' : 'FALSE (Knapsack 100% full)'),
          vars: cs,
          state: curBruteState({ callCounter: calls, activeNodeId: nodeId, maxVal: globalMax, activeIdx: idx, curW, curV, remain, bestFrac, taken: [...takenArr] })
        });

        if (hasRem) {
          steps.push({
            code: 'c_for_unused',
            badge: 'Scanning items to evaluate potential fractional gain for leftover ' + remain + ' kg.',
            vars: cs,
            state: curBruteState({ callCounter: calls, activeNodeId: nodeId, maxVal: globalMax, activeIdx: idx, curW, curV, remain, bestFrac, taken: [...takenArr] })
          });

          for (let i = 0; i < n; i++) {
            const isUnused = !takenArr[i];
            steps.push({
              code: 'c_check_taken',
              badge: 'Check item [' + i + ']: if (!taken[' + i + ']) &rarr; ' + (isUnused ? 'TRUE (Unused, calculate slice)' : 'FALSE (Already taken fully)'),
              vars: cs,
              state: curBruteState({ callCounter: calls, activeNodeId: nodeId, maxVal: globalMax, activeIdx: i, curW, curV, remain, bestFrac, taken: [...takenArr] })
            });

            if (isUnused) {
              let frac = remain / wt[i];
              steps.push({
                code: 'c_calc_frac',
                badge: 'double frac = (double)remain / wt[' + i + '] = ' + remain + ' / ' + wt[i] + ' = ' + frac.toFixed(3) + '.',
                vars: cs,
                state: curBruteState({ callCounter: calls, activeNodeId: nodeId, maxVal: globalMax, activeIdx: i, curW, curV, remain, bestFrac, frac, taken: [...takenArr] })
              });

              const isClamped = frac > 1.0;
              steps.push({
                code: 'c_check_frac_clamp',
                badge: 'if (frac > 1.0) &rarr; ' + (isClamped ? 'TRUE (Cap at 1.0)' : 'FALSE (Slice is < 1.0)'),
                vars: cs,
                state: curBruteState({ callCounter: calls, activeNodeId: nodeId, maxVal: globalMax, activeIdx: i, curW, curV, remain, bestFrac, frac, taken: [...takenArr] })
              });

              if (isClamped) {
                frac = 1.0;
                steps.push({
                  code: 'c_clamp_frac',
                  badge: 'frac = 1.0; — Item fits within remaining space.',
                  vars: cs,
                  state: curBruteState({ callCounter: calls, activeNodeId: nodeId, maxVal: globalMax, activeIdx: i, curW, curV, remain, bestFrac, frac, taken: [...takenArr] })
                });
              }

              const gain = val[i] * frac;
              steps.push({
                code: 'c_calc_frac_val',
                badge: 'double gain = val[' + i + '] * frac = $' + val[i] + ' * ' + frac.toFixed(2) + ' = $' + gain.toFixed(2) + '.',
                vars: cs,
                state: curBruteState({ callCounter: calls, activeNodeId: nodeId, maxVal: globalMax, activeIdx: i, curW, curV, remain, bestFrac, gain, taken: [...takenArr] })
              });

              const isBetterFrac = (gain > bestFrac);
              steps.push({
                code: 'c_check_best_frac',
                badge: 'if (gain > bestFrac) &rarr; ' + (isBetterFrac ? 'TRUE (New best fractional gain!)' : 'FALSE (Previous option is better)'),
                vars: cs,
                state: curBruteState({ callCounter: calls, activeNodeId: nodeId, maxVal: globalMax, activeIdx: i, curW, curV, remain, bestFrac, gain, taken: [...takenArr] })
              });

              if (isBetterFrac) {
                bestFrac = gain;
                bestItemIdx = i;
                steps.push({
                  code: 'c_update_best_frac',
                  badge: 'bestFrac = $' + bestFrac.toFixed(2) + ' (from item [' + i + ']).',
                  vars: cs,
                  state: curBruteState({ callCounter: calls, activeNodeId: nodeId, maxVal: globalMax, activeIdx: i, curW, curV, remain, bestFrac, taken: [...takenArr] })
                });
              }
            }
          }
        }

        const candVal = curV + bestFrac;
        const isNewGlobal = (candVal > globalMax);
        steps.push({
          code: 'c_check_global_max',
          badge: 'Check global best: curV + bestFrac = $' + curV.toFixed(2) + ' + $' + bestFrac.toFixed(2) + ' = $' + candVal.toFixed(2) + ' > maxVal ($' + globalMax.toFixed(2) + ') &rarr; ' + (isNewGlobal ? 'TRUE! New optimal profit found!' : 'FALSE (Does not beat best)'),
          vars: cs,
          state: curBruteState({ callCounter: calls, activeNodeId: nodeId, maxVal: globalMax, activeIdx: idx, curW, curV, candVal, fractionalIdx: bestItemIdx, taken: [...takenArr] })
        });

        if (isNewGlobal) {
          globalMax = candVal;
          steps.push({
            code: 'c_update_global_max',
            badge: 'maxVal updated to $' + globalMax.toFixed(2) + '!',
            vars: cs,
            state: curBruteState({ callCounter: calls, activeNodeId: nodeId, maxVal: globalMax, activeIdx: idx, curW, curV, fractionalIdx: bestItemIdx, taken: [...takenArr] })
          });
        }

        nodeStateMap[nodeId].state = 'solved';
        nodeStateMap[nodeId].retVal = candVal;

        steps.push({
          code: 'c_ret_base',
          badge: 'return; Leaf evaluated. Total branch profit = $' + candVal.toFixed(2) + '.',
          vars: cs,
          state: curBruteState({ callCounter: calls, activeNodeId: nodeId, maxVal: globalMax, activeIdx: idx, curW, curV, fractionalIdx: bestItemIdx, taken: [...takenArr] })
        });
        return;
      }

      // Internal Node:
      steps.push({
        code: 'c_set_skip',
        badge: 'taken[' + idx + '] = false; — Decision 1: Skip item [' + idx + '] (wt=' + wt[idx] + 'kg, val=$' + val[idx] + ').',
        vars: cs,
        state: curBruteState({ callCounter: calls, activeNodeId: nodeId, maxVal: globalMax, activeIdx: idx, curW, curV, taken: [...takenArr] })
      });

      steps.push({
        code: 'c_call_skip',
        badge: 'solve(idx + 1 = ' + (idx + 1) + ', curW=' + curW + ', curV=' + curV.toFixed(2) + '). Recursing on SKIP branch...',
        vars: cs,
        state: curBruteState({ callCounter: calls, activeNodeId: nodeId, maxVal: globalMax, activeIdx: idx, curW, curV, taken: [...takenArr] })
      });

      const ndObj = allNodes.find(x => x.id === nodeId);
      const childIds = ndObj ? ndObj.children : [];

      if (childIds[0] !== undefined) {
        const nextTakenSkip = [...takenArr];
        nextTakenSkip[idx] = false;
        simulateBrute(idx + 1, curW, curV, nextTakenSkip, childIds[0], [...callStack, frame('solve(idx=' + idx + ')', [['curW', String(curW)], ['curV', curV.toFixed(2)]])]);
      }

      const fits = (curW + wt[idx] <= W);
      steps.push({
        code: 'c_check_fit',
        badge: 'Check capacity: if (curW + wt[' + idx + '] = ' + (curW + wt[idx]) + ' <= W = ' + W + ') &rarr; ' + (fits ? 'TRUE (Item fits fully! Explore TAKE branch)' : 'FALSE (Exceeds capacity, cannot take fully)'),
        vars: cs,
        state: curBruteState({ callCounter: calls, activeNodeId: nodeId, maxVal: globalMax, activeIdx: idx, curW, curV, taken: [...takenArr] })
      });

      if (fits) {
        const nextTakenTake = [...takenArr];
        nextTakenTake[idx] = true;

        steps.push({
          code: 'c_set_take',
          badge: 'taken[' + idx + '] = true; — Decision 2: Take item [' + idx + '] fully (+ ' + wt[idx] + ' kg, + $' + val[idx] + ').',
          vars: [
            ...cs.slice(0, -1),
            frame('solve(idx=' + idx + ')', [['curW', String(curW)], ['curV', curV.toFixed(2)], ['remain', String(W - curW)], ['taken', '[' + nextTakenTake.map(t => t ? 'T' : 'F').join(',') + ']']])
          ],
          state: curBruteState({ callCounter: calls, activeNodeId: nodeId, maxVal: globalMax, activeIdx: idx, curW, curV, taken: nextTakenTake })
        });

        steps.push({
          code: 'c_call_take',
          badge: 'solve(idx + 1 = ' + (idx + 1) + ', curW=' + (curW + wt[idx]) + ', curV=' + (curV + val[idx]).toFixed(2) + '). Recursing on TAKE branch...',
          vars: [
            ...cs.slice(0, -1),
            frame('solve(idx=' + idx + ')', [['curW', String(curW)], ['curV', curV.toFixed(2)], ['remain', String(W - curW)], ['taken', '[' + nextTakenTake.map(t => t ? 'T' : 'F').join(',') + ']']])
          ],
          state: curBruteState({ callCounter: calls, activeNodeId: nodeId, maxVal: globalMax, activeIdx: idx, curW, curV, taken: nextTakenTake })
        });

        if (childIds[1] !== undefined) {
          simulateBrute(idx + 1, curW + wt[idx], curV + val[idx], nextTakenTake, childIds[1], [...callStack, frame('solve(idx=' + idx + ')', [['curW', String(curW)], ['curV', curV.toFixed(2)]])]);
        }

        steps.push({
          code: 'c_reset_take',
          badge: 'taken[' + idx + '] = false; — Backtracking: reset taken status of item [' + idx + '].',
          vars: cs,
          state: curBruteState({ callCounter: calls, activeNodeId: nodeId, maxVal: globalMax, activeIdx: idx, curW, curV, taken: [...takenArr] })
        });
      }

      nodeStateMap[nodeId].state = 'solved';
      nodeStateMap[nodeId].retVal = ndObj.subResult;
    }

    simulateBrute(0, 0, 0.0, new Array(n).fill(false), 0, []);

    steps.push({
      code: 'm_print_ans',
      badge: 'Output Result: System.out.printf("%.2f\n", ' + globalMax.toFixed(2) + '); — Exhaustive search complete! Explored ' + calls + ' recursive states.',
      vars: [frame('main()', [['ans', globalMax.toFixed(2)], ['taken', '[' + new Array(n).fill('false').join(', ') + ']']])],
      state: curBruteState({ callCounter: calls, activeNodeId: null, maxVal: globalMax, activeIdx: -1, taken: new Array(n).fill(false) })
    });

    steps.push({
      code: 'm_done',
      badge: 'Program execution finished. Maximum achievable profit = $' + globalMax.toFixed(2) + '.',
      vars: [frame('main()', [['status', 'finished']])],
      state: curBruteState({ callCounter: calls, activeNodeId: null, maxVal: globalMax, activeIdx: -1, taken: new Array(n).fill(false) })
    });

    return steps;
  }

  /* ------------------------------------------------------------------ */
  /* APPROACH 2: GREEDY (MULTIPLE ARRAYS: WT[], VAL[], RATIO[])          */
  /* ------------------------------------------------------------------ */
  else {
    const ratios = [];
    for (let i = 0; i < n; i++) {
      ratios.push((val[i] / wt[i]));
    }

    // Sorted index array
    const sortedIndices = Array.from({ length: n }, (_, i) => i)
      .sort((a, b) => ratios[b] - ratios[a]);

    function curGreedyState(extra = {}) {
      return {
        approach: 'greedy',
        n,
        W,
        wt,
        val,
        ratios,
        sortedIndices,
        ...extra
      };
    }

    const fractions = new Array(n).fill(0);
    const gaugeSegments = [];

    steps.push({
      code: 'm_scanner',
      badge: 'Faculty Note: Scanner sc = new Scanner(System.in); — Initializing input stream.',
      vars: [frame('main()', [['n', '?'], ['W', '?']])],
      state: curGreedyState({ i: -1, remain: W, totalVal: 0.0, fractions: [...fractions], gaugeSegments: [], status: 'input', numsFilled: 0 })
    });
    steps.push({
      code: 'm_read_n',
      badge: 'Faculty Note: int n = sc.nextInt(); &rarr; Read n = ' + n + ' items.',
      vars: [frame('main()', [['n', String(n)], ['W', '?']])],
      state: curGreedyState({ i: -1, remain: W, totalVal: 0.0, fractions: [...fractions], gaugeSegments: [], status: 'input', numsFilled: 0 })
    });
    steps.push({
      code: 'm_read_w',
      badge: 'Faculty Note: int W = sc.nextInt(); &rarr; Read knapsack capacity W = ' + W + ' kg.',
      vars: [frame('main()', [['n', String(n)], ['W', String(W)]])],
      state: curGreedyState({ i: -1, remain: W, totalVal: 0.0, fractions: [...fractions], gaugeSegments: [], status: 'input', numsFilled: 0 })
    });
    steps.push({
      code: 'm_alloc_arr',
      badge: 'Faculty Note: int[] wt = new int[' + n + ']; int[] val = new int[' + n + ']; — Allocating parallel arrays.',
      vars: [frame('main()', [['n', String(n)], ['W', String(W)]])],
      state: curGreedyState({ i: -1, remain: W, totalVal: 0.0, fractions: [...fractions], gaugeSegments: [], status: 'input', numsFilled: 0 })
    });

    for (let idx = 0; idx < n; idx++) {
      steps.push({
        code: 'm_for_input',
        badge: 'Reading inputs for item index ' + idx + '.',
        vars: [frame('main()', [['i', String(idx)], ['n', String(n)]])],
        state: curGreedyState({ i: -1, remain: W, totalVal: 0.0, fractions: [...fractions], gaugeSegments: [], currentIdx: idx, status: 'input', numsFilled: idx })
      });
      steps.push({
        code: 'm_read_item',
        badge: 'wt[' + idx + '] = sc.nextInt(); val[' + idx + '] = sc.nextInt(); \u2192 wt[' + idx + '] = ' + wt[idx] + ' kg, val[' + idx + '] = $' + val[idx] + '. (Ratio = ' + ratios[idx].toFixed(2) + '/kg)',
        vars: [frame('main()', [['i', String(idx)], ['wt[' + idx + ']', String(wt[idx])], ['val[' + idx + ']', String(val[idx])]])],
        state: curGreedyState({ i: -1, remain: W, totalVal: 0.0, fractions: [...fractions], gaugeSegments: [], currentIdx: idx, status: 'input', numsFilled: idx + 1 })
      });
    }

    steps.push({
      code: 'm_call_knapsack',
      badge: 'Invoking fractionalKnapsack(W=' + W + ', wt, val, n=' + n + '). Optimal Greedy scan begins!',
      vars: [frame('main()', [['W', String(W)], ['n', String(n)]])],
      state: curGreedyState({ i: -1, remain: W, totalVal: 0.0, fractions: [...fractions], gaugeSegments: [], status: 'entry' })
    });

    steps.push({
      code: 'c_entry',
      badge: 'Entering fractionalKnapsack(W=' + W + ', wt, val, n=' + n + '). Goal: maximize profit under capacity ' + W + ' kg.',
      vars: [frame('main()', [['W', String(W)]]), frame('fractionalKnapsack()', [['W', String(W)], ['n', String(n)]])],
      state: curGreedyState({ i: -1, remain: W, totalVal: 0.0, fractions: [...fractions], gaugeSegments: [], status: 'entry' })
    });

    steps.push({
      code: 'c_init_index',
      badge: 'Integer[] index = new Integer[' + n + ']; for (int i = 0; i < n; i++) index[i] = i; — Initializing parallel index array.',
      vars: [frame('main()', [['W', String(W)]]), frame('fractionalKnapsack()', [['index', '[' + sortedIndices.join(', ') + ']']])],
      state: curGreedyState({ i: -1, remain: W, totalVal: 0.0, fractions: [...fractions], gaugeSegments: [], status: 'index_init' })
    });

    steps.push({
      code: 'c_sort_ratio',
      badge: 'Arrays.sort(index by ratio val[i]/wt[i] descending). Prioritizing indices: [' + sortedIndices.join(', ') + '] strictly by value density!',
      vars: [frame('main()', [['W', String(W)]]), frame('fractionalKnapsack()', [['sorted_indices', '[' + sortedIndices.join(', ') + ']']])],
      state: curGreedyState({ i: -1, remain: W, totalVal: 0.0, fractions: [...fractions], gaugeSegments: [], status: 'sorted' })
    });

    let totalVal = 0.0;
    steps.push({
      code: 'c_init_val',
      badge: 'double totalVal = 0.0; — Total profit accumulator initialized to $0.00.',
      vars: [frame('main()', [['W', String(W)]]), frame('fractionalKnapsack()', [['totalVal', '0.00']])],
      state: curGreedyState({ i: -1, remain: W, totalVal: 0.0, fractions: [...fractions], gaugeSegments: [], status: 'running' })
    });

    let remain = W;
    steps.push({
      code: 'c_init_cap',
      badge: 'int remain = W = ' + W + '; — Remaining capacity initialized to ' + W + ' kg.',
      vars: [frame('main()', [['W', String(W)]]), frame('fractionalKnapsack()', [['remain', String(W)], ['totalVal', '0.00']])],
      state: curGreedyState({ i: -1, remain, totalVal: 0.0, fractions: [...fractions], gaugeSegments: [], status: 'running' })
    });

    for (let i = 0; i < n; i++) {
      const idx = sortedIndices[i];

      steps.push({
        code: 'c_for_items',
        badge: 'Inspecting rank #' + (i + 1) + ' index ' + idx + ': wt[' + idx + '] = ' + wt[idx] + ' kg, val[' + idx + '] = $' + val[idx] + ', ratio = ' + ratios[idx].toFixed(2) + '/kg.',
        vars: [frame('main()', [['W', String(W)]]), frame('fractionalKnapsack()', [['i', String(i)], ['remain', String(remain)], ['totalVal', totalVal.toFixed(2)]])],
        state: curGreedyState({ i, activeIdx: idx, remain, totalVal, fractions: [...fractions], gaugeSegments: [...gaugeSegments], status: 'inspecting' })
      });

      steps.push({
        code: 'c_get_idx',
        badge: 'int idx = index[' + i + '] = ' + idx + '; — Examining item at original array index ' + idx + '.',
        vars: [frame('main()', [['W', String(W)]]), frame('fractionalKnapsack()', [['i', String(i)], ['idx', String(idx)], ['wt[idx]', String(wt[idx])], ['val[idx]', String(val[idx])]])],
        state: curGreedyState({ i, activeIdx: idx, remain, totalVal, fractions: [...fractions], gaugeSegments: [...gaugeSegments], status: 'inspecting' })
      });

      const fits = (remain >= wt[idx]);
      steps.push({
        code: 'c_check_fit',
        badge: 'Check capacity: if (remain=' + remain + ' >= wt[' + idx + ']=' + wt[idx] + ') &rarr; ' + (fits ? 'TRUE (Knapsack has room for entire item!)' : 'FALSE (Not enough space for full item; take fractional slice)'),
        vars: [frame('main()', [['W', String(W)]]), frame('fractionalKnapsack()', [['i', String(i)], ['idx', String(idx)], ['remain', String(remain)], ['wt[idx]', String(wt[idx])]])],
        state: curGreedyState({ i, activeIdx: idx, remain, totalVal, fractions: [...fractions], gaugeSegments: [...gaugeSegments], status: fits ? 'full_fit' : 'partial_fit' })
      });

      if (fits) {
        remain -= wt[idx];
        steps.push({
          code: 'c_take_full_cap',
          badge: 'remain -= wt[' + idx + '] &rarr; Updated remain: ' + (remain + wt[idx]) + ' - ' + wt[idx] + ' = ' + remain + ' kg.',
          vars: [frame('main()', [['W', String(W)]]), frame('fractionalKnapsack()', [['i', String(i)], ['idx', String(idx)], ['remain', String(remain)], ['totalVal', totalVal.toFixed(2)]])],
          state: curGreedyState({ i, activeIdx: idx, remain, totalVal, fractions: [...fractions], gaugeSegments: [...gaugeSegments], status: 'taking_full' })
        });

        totalVal += val[idx];
        fractions[idx] = 1.0;
        gaugeSegments.push({
          idx,
          weight: wt[idx],
          fraction: 1.0,
          pct: ((wt[idx] / W) * 100).toFixed(1)
        });

        steps.push({
          code: 'c_take_full_val',
          badge: 'totalVal += val[' + idx + '] &rarr; totalVal = $' + (totalVal - val[idx]).toFixed(2) + ' + $' + val[idx] + ' = $' + totalVal.toFixed(2) + ' (100% item loaded).',
          vars: [frame('main()', [['W', String(W)]]), frame('fractionalKnapsack()', [['i', String(i)], ['idx', String(idx)], ['remain', String(remain)], ['totalVal', totalVal.toFixed(2)]])],
          state: curGreedyState({ i, activeIdx: idx, remain, totalVal, fractions: [...fractions], gaugeSegments: [...gaugeSegments], status: 'full_loaded' })
        });
      } else {
        steps.push({
          code: 'c_else_frac',
          badge: 'else branch: Capacity deficit (' + remain + ' kg available < ' + wt[idx] + ' kg required). Take fractional part of item [' + idx + ']!',
          vars: [frame('main()', [['W', String(W)]]), frame('fractionalKnapsack()', [['i', String(i)], ['idx', String(idx)], ['remain', String(remain)], ['wt[idx]', String(wt[idx])]])],
          state: curGreedyState({ i, activeIdx: idx, remain, totalVal, fractions: [...fractions], gaugeSegments: [...gaugeSegments], status: 'fraction_branch' })
        });

        const fraction = remain / wt[idx];
        const fracGain = val[idx] * fraction;
        totalVal += fracGain;
        fractions[idx] = fraction;
        gaugeSegments.push({
          idx,
          weight: remain,
          fraction,
          pct: ((remain / W) * 100).toFixed(1)
        });

        steps.push({
          code: 'c_calc_frac',
          badge: 'totalVal += val[' + idx + '] * ((double)remain / wt[' + idx + ']) &rarr; Added $' + val[idx] + ' * (' + remain + ' / ' + wt[idx] + ') = $' + fracGain.toFixed(2) + '. Total = $' + totalVal.toFixed(2) + '.',
          vars: [frame('main()', [['W', String(W)]]), frame('fractionalKnapsack()', [['i', String(i)], ['idx', String(idx)], ['totalVal', totalVal.toFixed(2)], ['fraction', fraction.toFixed(3)]])],
          state: curGreedyState({ i, activeIdx: idx, remain, totalVal, fractions: [...fractions], gaugeSegments: [...gaugeSegments], fraction, status: 'frac_loaded' })
        });

        remain = 0;
        steps.push({
          code: 'c_zero_cap',
          badge: 'remain = 0; — Knapsack is now 100% full!',
          vars: [frame('main()', [['W', String(W)]]), frame('fractionalKnapsack()', [['i', String(i)], ['remain', '0'], ['totalVal', totalVal.toFixed(2)]])],
          state: curGreedyState({ i, activeIdx: idx, remain: 0, totalVal, fractions: [...fractions], gaugeSegments: [...gaugeSegments], status: 'knapsack_full' })
        });

        steps.push({
          code: 'c_break',
          badge: 'break; Knapsack completely saturated. Greedy scan terminates early!',
          vars: [frame('main()', [['W', String(W)]]), frame('fractionalKnapsack()', [['status', 'break']])],
          state: curGreedyState({ i, activeIdx: idx, remain: 0, totalVal, fractions: [...fractions], gaugeSegments: [...gaugeSegments], status: 'knapsack_full' })
        });

        break;
      }
    }

    steps.push({
      code: 'c_ret_val',
      badge: 'return totalVal = $' + totalVal.toFixed(2) + '; — Returning optimal total knapsack profit.',
      vars: [frame('main()', [['W', String(W)]]), frame('fractionalKnapsack()', [['return', totalVal.toFixed(2)]])],
      state: curGreedyState({ i: n, remain, totalVal, fractions: [...fractions], gaugeSegments: [...gaugeSegments], status: 'complete' })
    });

    steps.push({
      code: 'm_print_ans',
      badge: 'Output Result: System.out.printf("%.2f\n", ' + totalVal.toFixed(2) + '); — Solved in O(n log n) sorting pass!',
      vars: [frame('main()', [['ans', totalVal.toFixed(2)]])],
      state: curGreedyState({ i: -1, remain, totalVal, fractions: [...fractions], gaugeSegments: [...gaugeSegments], status: 'complete' })
    });

    steps.push({
      code: 'm_done',
      badge: 'Program execution finished. Maximum achievable profit = $' + totalVal.toFixed(2) + '.',
      vars: [frame('main()', [['status', 'finished']])],
      state: curGreedyState({ i: -1, remain, totalVal, fractions: [...fractions], gaugeSegments: [...gaugeSegments], status: 'complete' })
    });

    return steps;
  }
}

/* ==================================================================== */
/* COMPONENT STATE & REACTIVE SETUP                                     */
/* ==================================================================== */
const currentApproach = ref('brute');
const inputWeightsText = ref('10, 20, 30');
const inputValuesText = ref('60, 100, 120');
const inputWText = ref('50');
const lang = ref('java');
const speed = ref(650);
const si = ref(0);
const playing = ref(false);
const vizHeight = ref(250);
const tableHeight = ref(42);
const leftWidth = ref(54);
const rightTab = ref('code');

function parseArray(text) {
  return text.split(/[,;\s]+/)
    .map(x => parseInt(x.trim(), 10))
    .filter(x => !isNaN(x));
}

const currentWeights = computed(() => {
  const arr = parseArray(inputWeightsText.value);
  return arr.length >= 1 ? arr : DEFAULT_WEIGHTS;
});

const currentValues = computed(() => {
  const arr = parseArray(inputValuesText.value);
  return arr.length >= 1 ? arr : DEFAULT_VALUES;
});

const currentW = computed(() => {
  const w = parseInt(inputWText.value, 10);
  return (!isNaN(w) && w > 0) ? w : DEFAULT_W;
});

const stepsData = reactive({
  steps: buildSteps('brute', DEFAULT_WEIGHTS, DEFAULT_VALUES, DEFAULT_W)
});

const steps = computed(() => stepsData.steps);
const s = computed(() => steps.value[Math.max(0, Math.min(si.value, steps.value.length - 1))] || { state: {} });
const st = computed(() => s.value.state || {});

const codeLines = computed(() => {
  const appCodes = CODES[currentApproach.value] || CODES.brute;
  return appCodes[lang.value] || appCodes.java || [];
});
const pseudocodeLines = computed(() => PSEUDOCODES[currentApproach.value] || PSEUDOCODES.brute);

let playTimer = null;

function applyApproach(appId) {
  currentApproach.value = appId;
  resetAll();
}

function applyInput() {
  resetAll();
}

function resetAll() {
  pause();
  stepsData.steps = buildSteps(currentApproach.value, currentWeights.value, currentValues.value, currentW.value);
  si.value = 0;
}

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
  resetAll();
});

onUnmounted(() => {
  pause();
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
      <h2 class="navbar-title">{{ topic }} — {{ subTopic }}</h2>
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

            <!-- Custom Inputs: Weights, Values, W -->
            <div class="ll-input-group">
              <label>wt[] =</label>
              <input
                v-model="inputWeightsText"
                class="ll-text-input"
                placeholder="10, 20, 30"
                @keyup.enter="applyInput"
                style="width: 86px;"
              />
            </div>

            <div class="ll-input-group">
              <label>val[] =</label>
              <input
                v-model="inputValuesText"
                class="ll-text-input"
                placeholder="60, 100, 120"
                @keyup.enter="applyInput"
                style="width: 86px;"
              />
            </div>

            <div class="ll-input-group">
              <label>W =</label>
              <input
                v-model="inputWText"
                class="ll-text-input"
                placeholder="50"
                @keyup.enter="applyInput"
                style="width: 38px;"
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
              <!-- Top Banner: Dual Arrays wt[] and val[] matching Screenshot 2 -->
              <div class="ll-house-banner">
                <div class="ll-dual-arrays-wrap">
                  <!-- Weight Array Row -->
                  <div class="ll-arr-row">
                    <span class="ll-arr-tag">wt[]:</span>
                    <div class="ll-house-strip">
                      <div
                        v-for="(w, idx) in currentWeights"
                        :key="'w' + idx"
                        class="ll-house-card"
                        :class="{
                          'll-house-cur': (st.activeIdx === idx || st.currentIdx === idx || st.i === idx),
                          'll-house-taken': (st.taken && st.taken[idx]) || (st.fractions && st.fractions[idx] === 1),
                          'll-house-frac': (st.fractionalIdx === idx) || (st.fractions && st.fractions[idx] > 0 && st.fractions[idx] < 1)
                        }"
                      >
                        <div class="ll-house-val">{{ (st.numsFilled !== undefined && idx >= st.numsFilled) ? '?' : w }}</div>
                        <div class="ll-house-idx">
                          <span v-if="st.activeIdx === idx || st.currentIdx === idx || st.i === idx" class="ll-idx-tag ll-tag-cur">i</span>
                          <span v-else>[{{ idx }}]</span>
                        </div>
                      </div>
                    </div>
                  </div>

                  <!-- Value Array Row -->
                  <div class="ll-arr-row">
                    <span class="ll-arr-tag">val[]:</span>
                    <div class="ll-house-strip">
                      <div
                        v-for="(v, idx) in currentValues"
                        :key="'v' + idx"
                        class="ll-house-card"
                        :class="{
                          'll-house-cur': (st.activeIdx === idx || st.currentIdx === idx || st.i === idx),
                          'll-house-taken': (st.taken && st.taken[idx]) || (st.fractions && st.fractions[idx] === 1),
                          'll-house-frac': (st.fractionalIdx === idx) || (st.fractions && st.fractions[idx] > 0 && st.fractions[idx] < 1)
                        }"
                      >
                        <div class="ll-house-val">{{ (st.numsFilled !== undefined && idx >= st.numsFilled) ? '?' : (v) }}</div>
                        <div class="ll-house-idx">
                          <span v-if="st.activeIdx === idx || st.currentIdx === idx || st.i === idx" class="ll-idx-tag ll-tag-cur">i</span>
                          <span v-else>[{{ idx }}]</span>
                        </div>
                      </div>
                    </div>
                  </div>

                  <!-- Taken Array Row: Dedicated Decision Tracker for boolean[] taken -->
                  <div class="ll-arr-row" v-if="currentApproach === 'brute'">
                    <span class="ll-arr-tag">taken[]:</span>
                    <div class="ll-house-strip" v-if="st.taken">
                      <div
                        v-for="(tVal, idx) in st.taken"
                        :key="'t' + idx"
                        class="ll-house-card"
                        :class="{
                          'll-house-cur': (st.activeIdx === idx || st.currentIdx === idx || s.code === 'm_alloc_taken'),
                          'll-house-taken': tVal,
                          'll-house-uncalc': !tVal
                        }"
                      >
                        <div
                          class="ll-house-val"
                          :style="{
                            fontSize: '11px',
                            fontWeight: '700',
                            color: tVal ? '#10b981' : '#64748b'
                          }"
                        >
                          {{ tVal ? 'true' : 'false' }}
                        </div>
                        <div class="ll-house-idx">
                          <span v-if="st.activeIdx === idx || st.currentIdx === idx" class="ll-idx-tag ll-tag-cur">i</span>
                          <span v-else>[{{ idx }}]</span>
                        </div>
                      </div>
                    </div>
                    <div v-else class="ll-house-strip">
                      <div
                        v-for="(_, idx) in Math.min(currentWeights.length, currentValues.length)"
                        :key="'t-uninit-' + idx"
                        class="ll-house-card ll-house-uninit"
                        title="boolean[] taken not yet allocated"
                      >
                        <div class="ll-house-val" style="color: var(--muted); font-size: 11px;">—</div>
                        <div class="ll-house-idx">[{{ idx }}]</div>
                      </div>
                    </div>
                  </div>
                </div>
              </div>

              <!-- Real-time Stats Chips Bar matching Screenshot 2 -->
              <div class="ll-ptrs">
                <div class="ll-ptr-chip">n = <b class="ll-c-blue">{{ Math.min(currentWeights.length, currentValues.length) }}</b></div>
                <div class="ll-ptr-chip">W = <b class="ll-c-purple">{{ currentW }}</b></div>
                <template v-if="currentApproach === 'greedy'">
                  <div class="ll-ptr-chip">
                    Remaining Space: <b class="ll-c-orange">{{ st.remain !== undefined ? st.remain : currentW }} kg</b>
                  </div>
                  <div class="ll-ptr-chip">
                    Total Profit: <b class="ll-c-green">&dollar;{{ (st.totalVal || 0).toFixed(2) }}</b>
                  </div>
                  <div class="ll-ptr-chip" v-if="st.activeIdx !== undefined && st.activeIdx >= 0">
                    Active: <b class="ll-c-blue">index [{{ st.activeIdx }}]</b>
                  </div>
                </template>
                <template v-else>
                  <div class="ll-ptr-chip">Calls: <b class="ll-c-orange">{{ st.callCounter || 0 }}</b></div>
                  <div class="ll-ptr-chip">
                    Current Weight: <b class="ll-c-blue">{{ st.curW !== undefined ? st.curW + ' kg' : '0 kg' }}</b>
                  </div>
                  <div class="ll-ptr-chip">
                    Current Value: <b class="ll-c-purple">&dollar;{{ (st.curV || 0).toFixed(2) }}</b>
                  </div>
                  <div class="ll-ptr-chip">
                    Max Profit: <b class="ll-c-green">&dollar;{{ (st.maxVal || 0).toFixed(2) }}</b>
                  </div>
                  <!-- <div class="ll-ptr-chip" v-if="st.taken">
                    taken[]: <b class="ll-c-blue">[{{ st.taken.map(t => t ? 'T' : 'F').join(', ') }}]</b>
                  </div> -->
                </template>
              </div>

              <!-- Visualization Viewport Container -->
              <div class="ll-viz-wrap" :style="{ height: vizHeight + 'px' }">
                <!-- VIEW 1: GREEDY ARRAY STRIP MATCHING SCREENSHOT 3 -->
                <div v-if="currentApproach === 'greedy'" class="ll-greedy-strip-view">
                  <div class="ll-section-caption">
                    <span>1D Greedy Array Strip — Ratio <code>ratio[i] = val[i] / wt[i]</code> (Highest Density Priority)</span>
                    <span v-if="st.remain === 0" class="ll-calc-pill">&#10003; Knapsack 100% Full</span>
                  </div>

                  <!-- Parallel Ratio Array Strip with Cell Boxes matching Screenshot 3 -->
                  <div class="ll-arr-track">
                    <div
                      v-for="(r, idx) in (st.ratios || [])"
                      :key="'cell-' + idx"
                      class="ll-arr-cell-wrap"
                    >
                      <!-- Pointer Tag Above Cell matching Screenshot 3 -->
                      <div class="ll-ptr-tag-wrap">
                        <span v-if="st.activeIdx === idx" class="ll-ptr-lbl ll-lbl-orange">&darr; idx</span>
                        <span v-else-if="st.fractions && st.fractions[idx] === 1" class="ll-ptr-lbl ll-lbl-green">&#10003; 100%</span>
                        <span v-else-if="st.fractions && st.fractions[idx] > 0" class="ll-ptr-lbl ll-lbl-purple">&darr; frac</span>
                      </div>

                      <!-- Flat Rounded Box matching Screenshot 3 -->
                      <div
                        class="ll-arr-box"
                        :class="{
                          'll-box-cur': st.activeIdx === idx,
                          'll-box-found': st.fractions && st.fractions[idx] === 1,
                          'll-box-comp': st.fractions && st.fractions[idx] > 0 && st.fractions[idx] < 1,
                          'll-box-uncalc': !st.fractions || st.fractions[idx] === 0
                        }"
                      >
                        <div class="ll-box-val">{{ r.toFixed(1) }}</div>
                        <div class="ll-box-sub">
                          <span v-if="st.fractions && st.fractions[idx] === 1">100%</span>
                          <span v-else-if="st.fractions && st.fractions[idx] > 0">{{ (st.fractions[idx] * 100).toFixed(0) }}%</span>
                          <span v-else>r=v/w</span>
                        </div>
                      </div>

                      <div class="ll-arr-idx">
                        [{{ idx }}] ({{ currentWeights[idx] }}kg, &dollar;{{ currentValues[idx] }})
                      </div>
                    </div>
                  </div>

                  <!-- Horizontal Knapsack Capacity Gauge -->
                  <div class="ll-gauge-container">
                    <div class="ll-gauge-header">
                      <span>Knapsack Capacity Progress: {{ currentW - (st.remain !== undefined ? st.remain : currentW) }} / {{ currentW }} kg</span>
                      <span class="ll-c-green">Accumulated Profit: &dollar;{{ (st.totalVal || 0).toFixed(2) }}</span>
                    </div>
                    <div class="ll-gauge-bar">
                      <div
                        v-for="(seg, sIdx) in (st.gaugeSegments || [])"
                        :key="'seg-' + sIdx"
                        class="ll-gauge-segment"
                        :class="['seg-' + (seg.idx % 5), { 'seg-frac': seg.fraction < 1 }]"
                        :style="{ width: seg.pct + '%' }"
                        :title="'item[' + seg.idx + ']: ' + seg.weight + 'kg (' + (seg.fraction * 100).toFixed(0) + '%)'"
                      >
                        [{{ seg.idx }}] {{ seg.weight }}kg
                      </div>
                    </div>
                  </div>

                  <!-- Formula Callout Strip matching Screenshot 3 -->
                  <div class="ll-calc-callout" v-if="st.status">
                    <div class="ll-calc-line" v-if="st.status === 'sorted'">
                      <b>Greedy Choice:</b> Parallel indices sorted by ratio <code>val[i] / wt[i]</code> descending: <code>[{{ st.sortedIndices.join(', ') }}]</code>.
                    </div>
                    <div class="ll-calc-line" v-else-if="st.status === 'full_fit' || st.status === 'full_loaded'">
                      <b>Full Intake:</b> Remaining Capacity ({{ st.remain }}kg) &ge; wt[{{ st.activeIdx }}] ({{ currentWeights[st.activeIdx] }}kg) &rarr; Take 100% (+{{ currentWeights[st.activeIdx] }}kg, +&dollar;{{ currentValues[st.activeIdx] }}).
                    </div>
                    <div class="ll-calc-line" v-else-if="st.status === 'fraction_branch' || st.status === 'frac_loaded'">
                      <b>Fractional Intake:</b> Remaining Capacity ({{ st.remain }}kg) &lt; wt[{{ st.activeIdx }}] ({{ currentWeights[st.activeIdx] }}kg) &rarr; Take fraction <code>{{ st.remain }} / {{ currentWeights[st.activeIdx] }} = {{ ((st.fraction || 0) * 100).toFixed(1) }}%</code>!
                    </div>
                    <div class="ll-calc-line" v-else-if="st.status === 'knapsack_full'">
                      <b>Capacity Saturated:</b> Knapsack filled to 100% capacity (remain = 0kg). Greedy algorithm halts early.
                    </div>
                    <div class="ll-calc-line" v-else>
                      <b>Status:</b> Ready to greedily pack items in descending ratio order.
                    </div>
                  </div>
                </div>

                <!-- VIEW 2: RECURSIVE DECISION TREE MATCHING SCREENSHOT 2 -->
                <div v-else class="ll-tree-container">
                  <div class="ll-section-caption">
                    <span>Recursive Include/Exclude Decision Tree O(2^n)</span>
                    <span class="ll-calc-pill">Exhaustive Backtracking</span>
                  </div>

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
                      <!-- Connector Edges matching Screenshot 2 -->
                      <g class="ll-tree-edges">
                        <line
                          v-for="(edge, idx) in st.treeEdges"
                          :key="idx"
                          :x1="edge.x1"
                          :y1="edge.y1"
                          :x2="edge.x2"
                          :y2="edge.y2"
                          class="ll-tree-edge"
                          :class="{
                            'll-edge-take': edge.branchType === 'take',
                            'll-edge-nottake': edge.branchType === 'nottake'
                          }"
                        />
                      </g>

                      <!-- Tree Node Badges matching Screenshot 2 -->
                      <g class="ll-tree-nodes">
                        <g
                          v-for="node in st.treeNodes"
                          :key="node.id"
                          class="ll-tree-node-group"
                          :transform="`translate(${node.x}, ${node.y})`"
                        >
                          <!-- Node Outer Card -->
                          <rect
                            :x="-27"
                            :y="-13"
                            :width="54"
                            :height="26"
                            rx="4"
                            class="ll-node-rect"
                            :class="{
                              'll-node-active': node.id === st.activeNodeId,
                              'll-node-solved': node.state === 'solved',
                              'll-node-leaf': node.isLeaf
                            }"
                          />
                          <!-- Node Text: solve(i, w) -->
                          <text
                            x="0"
                            y="-3.5"
                            text-anchor="middle"
                            class="ll-node-text-call"
                          >
                            {{ node.isLeaf ? 'leaf(' + node.curW + ')' : 'solve(' + node.idx + ',' + node.curW + ')' }}
                          </text>
                          <!-- Node Return Value: = $val or ? -->
                          <text
                            x="0"
                            y="7"
                            text-anchor="middle"
                            class="ll-node-text-val"
                            :class="{
                              'll-val-active': node.id === st.activeNodeId,
                              'll-val-solved': node.state === 'solved'
                            }"
                          >
                            {{ node.retVal !== null ? '= ' + node.retVal.toFixed(1) : (node.id === st.activeNodeId ? '?' : '') }}
                          </text>
                        </g>
                      </g>
                    </svg>
                  </div>
                </div>
              </div>

              <!-- Vertical Resizer for Viz Panel -->
              <div class="ll-vresizer" ref="vizResizerRef"></div>

              <!-- Dedicated Legend Strip matching Screenshot 2 & 3 -->
              <div class="ll-legend">
                <template v-if="currentApproach === 'greedy'">
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-cur"></span>Current Index (i)</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-new"></span>100% Full Take</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-purple"></span>Fractional Slice</span>
                  <span class="ll-leg"><span class="ll-legdot" style="background: #94a3b8;"></span>Pending Uncalculated</span>
                </template>
                <template v-else>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-cur"></span>Active Decision</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-new"></span>Take (+wt[i])</span>
                  <span class="ll-leg"><span class="ll-legdot" style="background: #94a3b8;"></span>Skip Branch</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-purple"></span>Leaf Evaluation</span>
                </template>
              </div>

              <!-- Call Stack & Variable Environment Panel -->
              <div class="ll-table-area" :style="{ height: tableHeight + 'px' }">
                <div class="ll-table-title">Call Stack &amp; Environment Variables:</div>
                <div class="ll-stack-line" v-for="(fr, fIdx) in s.vars" :key="fIdx">
                  <span class="ll-fname">{{ fr.title }}</span>
                  <span class="ll-now">&rarr;</span>
                  <span v-for="(v, vIdx) in fr.rows" :key="vIdx" class="ll-frame">
                    {{ v[0] }} = <b>{{ v[1] }}</b><span v-if="vIdx < fr.rows.length - 1">, </span>
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
                    'll-badge-error': s.badge && (s.badge.includes('Deficit') || s.badge.includes('Exceeds')),
                    'll-badge-success': s.badge && (s.badge.includes('finished') || s.badge.includes('Optimal') || s.badge.includes('complete') || s.badge.includes('New optimal'))
                  }"
                >
                  {{ s.badge || 'Ready to visualize Fractional Knapsack Problem.' }}
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
                    :class="{ 'll-hl': line[0] && line[0] === s.code }"
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
                      Exhaustively explores every possible include/exclude decision for each
                      item using recursion. At leaf nodes (all items decided), it calculates
                      the best fractional top-up from unused items to maximise the final profit.
                    </p>

                    <h4 class="ll-cx-sub">Step-by-step Breakdown</h4>
                    <table class="ll-complexity-table">
                      <thead>
                        <tr><th>Operation</th><th>Time</th><th>Space</th><th>Reason</th></tr>
                      </thead>
                      <tbody>
                        <tr>
                          <td>Read input wt[] &amp; val[]</td>
                          <td class="ll-cx-good">O(n)</td>
                          <td class="ll-cx-good">O(n)</td>
                          <td>Single pass to fill two parallel arrays</td>
                        </tr>
                        <tr>
                          <td>Recursive Skip/Take branching</td>
                          <td class="ll-cx-bad">O(2<sup>n</sup>)</td>
                          <td class="ll-cx-mid">O(n)</td>
                          <td>Binary decision tree of depth n &rarr; 2<sup>n</sup> leaves; call stack depth = n</td>
                        </tr>
                        <tr>
                          <td>Fractional top-up scan at each leaf</td>
                          <td class="ll-cx-mid">O(n)</td>
                          <td class="ll-cx-good">O(1)</td>
                          <td>Scan unused items per leaf &rarr; combined O(2<sup>n</sup> &middot; n)</td>
                        </tr>
                      </tbody>
                    </table>

                    <h4 class="ll-cx-sub">Overall Complexity</h4>
                    <div class="ll-cx-summary-grid">
                      <div class="ll-cx-card ll-cx-card-bad">
                        <div class="ll-cx-card-label">Time</div>
                        <div class="ll-cx-card-val">O(2<sup>n</sup>&middot;n)</div>
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
                      For any practical knapsack problem, the Greedy O(n log n) sort-and-strip approach is the only feasible choice.
                    </div>
                  </template>

                  <!-- ════════ GREEDY COMPLEXITY ════════ -->
                  <template v-else>
                    <h3 class="ll-cx-heading">Greedy Algorithm &mdash; Complexity Analysis</h3>
                    <p class="ll-cx-intro">
                      Sorts items by value-density ratio <code>val[i] / wt[i]</code> in descending
                      order using a separate index array. Items are then greedily taken in full
                      until capacity runs out; at most one item is ever split into a fraction.
                    </p>

                    <h4 class="ll-cx-sub">Step-by-step Breakdown</h4>
                    <table class="ll-complexity-table">
                      <thead>
                        <tr><th>Operation</th><th>Time</th><th>Space</th><th>Reason</th></tr>
                      </thead>
                      <tbody>
                        <tr>
                          <td>Read input wt[] &amp; val[]</td>
                          <td class="ll-cx-good">O(n)</td>
                          <td class="ll-cx-good">O(n)</td>
                          <td>Single pass to fill two parallel arrays</td>
                        </tr>
                        <tr>
                          <td>Build &amp; sort index array by ratio</td>
                          <td class="ll-cx-mid">O(n log n)</td>
                          <td class="ll-cx-good">O(n)</td>
                          <td>Standard comparison sort &mdash; dominates total time</td>
                        </tr>
                        <tr>
                          <td>Greedy capacity saturation scan</td>
                          <td class="ll-cx-good">O(n)</td>
                          <td class="ll-cx-good">O(1)</td>
                          <td>Single left-to-right pass; at most one fractional split</td>
                        </tr>
                      </tbody>
                    </table>

                    <h4 class="ll-cx-sub">Overall Complexity</h4>
                    <div class="ll-cx-summary-grid">
                      <div class="ll-cx-card ll-cx-card-mid">
                        <div class="ll-cx-card-label">Time</div>
                        <div class="ll-cx-card-val">O(n log n)</div>
                        <div class="ll-cx-card-note">Sort dominates &mdash; optimal for comparison model</div>
                      </div>
                      <div class="ll-cx-card ll-cx-card-good">
                        <div class="ll-cx-card-label">Space</div>
                        <div class="ll-cx-card-val">O(n)</div>
                        <div class="ll-cx-card-note">Only the index array (O(1) aux beyond input)</div>
                      </div>
                      <div class="ll-cx-card ll-cx-card-good">
                        <div class="ll-cx-card-label">Optimal?</div>
                        <div class="ll-cx-card-val">YES</div>
                        <div class="ll-cx-card-note">Provable via exchange argument</div>
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
                          <td class="ll-cx-bad">O(2<sup>n</sup>&middot;n)</td>
                          <td class="ll-cx-mid">O(n)</td>
                          <td class="ll-cx-bad">No &mdash; exponential blowup</td>
                        </tr>
                        <tr>
                          <td><strong>Greedy (Sort + Scan)</strong></td>
                          <td class="ll-cx-mid">O(n log n)</td>
                          <td class="ll-cx-good">O(n)</td>
                          <td class="ll-cx-good">Yes &mdash; scales to millions</td>
                        </tr>
                      </tbody>
                    </table>

                    <div class="ll-note">
                      <strong>Key Insight:</strong> The Fractional Knapsack is solvable optimally
                      in polynomial time. Because items can be split, the exchange argument proves
                      that always taking the highest value-density item first is globally optimal.
                      At most <em>one</em> item is ever fractionally taken.
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
.ll-cx-card-good { background: #f0fdf4; border-color: #86efac; color: #15803d; }
.ll-cx-card-mid  { background: #fff7ed; border-color: #fed7aa; color: #c2410c; }
.ll-cx-card-bad  { background: #fef2f2; border-color: #fca5a5; color: #b91c1c; }

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


/* ==================================================================== */
/* MULTIPLE ARRAYS & SCREENSHOT DESIGN STYLES                           */
/* ==================================================================== */
.ll-dual-arrays-wrap {
  display: flex;
  flex-direction: column;
  gap: 6px;
}
.ll-arr-row {
  display: flex;
  align-items: center;
  gap: 8px;
}
.ll-arr-tag {
  font-family: monospace;
  font-size: 11px;
  font-weight: 700;
  color: var(--coral);
  width: 42px;
  flex-shrink: 0;
}
.ll-house-strip {
  display: flex;
  gap: 6px;
  flex-wrap: wrap;
}
.ll-house-card {
  width: 46px;
  height: 40px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  background: var(--surface);
  border: 1.5px solid var(--border2);
  border-radius: var(--radius-sm);
  box-shadow: var(--shadow-sm);
  transition: all 0.2s ease;
}
.ll-house-cur {
  border-color: #f59e0b !important;
  background: #fffbeb !important;
  transform: translateY(-2px);
  box-shadow: 0 0 0 3px rgba(245, 158, 11, 0.25) !important;
}
.ll-house-prev {
  border-color: #3b82f6 !important;
  background: #eff6ff !important;
  transform: translateY(-2px);
}
.ll-house-taken {
  border-color: #10b981 !important;
  background: #f0fdf4 !important;
}
.ll-house-frac {
  border-color: #a855f7 !important;
  background: #faf5ff !important;
}
.ll-house-uninit {
  border-style: dashed !important;
  border-color: var(--border2) !important;
  background: var(--surface2) !important;
  opacity: 0.65;
}

/* Animation Column & Viewports Scrollable without Scrollbars */
.ll-left-col {
  display: flex;
  flex-direction: column;
  overflow-y: auto !important;
  overflow-x: hidden !important;
  min-width: 240px;
  max-width: 75%;
  scrollbar-width: none !important;
  -ms-overflow-style: none !important;
}
.ll-left-col::-webkit-scrollbar {
  display: none !important;
  width: 0 !important;
  height: 0 !important;
}

.ll-viz-wrap {
  overflow-y: auto !important;
  overflow-x: auto !important;
  scrollbar-width: none !important;
  -ms-overflow-style: none !important;
}
.ll-viz-wrap::-webkit-scrollbar {
  display: none !important;
  width: 0 !important;
  height: 0 !important;
}

.ll-tree-scroll-area {
  flex: 1;
  overflow: auto !important;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 8px;
  scrollbar-width: none !important;
  -ms-overflow-style: none !important;
}
.ll-tree-scroll-area::-webkit-scrollbar {
  display: none !important;
  width: 0 !important;
  height: 0 !important;
}

.ll-house-banner {
  overflow-x: auto !important;
  scrollbar-width: none !important;
  -ms-overflow-style: none !important;
}
.ll-house-banner::-webkit-scrollbar {
  display: none !important;
  width: 0 !important;
  height: 0 !important;
}

.ll-dual-arrays-wrap {
  overflow-x: auto !important;
  scrollbar-width: none !important;
  -ms-overflow-style: none !important;
}
.ll-dual-arrays-wrap::-webkit-scrollbar {
  display: none !important;
  width: 0 !important;
  height: 0 !important;
}

.ll-house-strip {
  display: flex;
  gap: 6px;
  flex-wrap: wrap;
  overflow-x: auto !important;
  scrollbar-width: none !important;
  -ms-overflow-style: none !important;
}
.ll-house-strip::-webkit-scrollbar {
  display: none !important;
  width: 0 !important;
  height: 0 !important;
}

.ll-greedy-strip-view {
  display: flex;
  flex-direction: column;
  height: 100%;
  overflow-y: auto !important;
  overflow-x: auto !important;
  scrollbar-width: none !important;
  -ms-overflow-style: none !important;
}
.ll-greedy-strip-view::-webkit-scrollbar {
  display: none !important;
  width: 0 !important;
  height: 0 !important;
}

.ll-arr-track {
  display: flex;
  align-items: flex-start;
  flex-wrap: wrap;
  padding: 6px 4px;
  gap: 12px;
  overflow-x: auto !important;
  scrollbar-width: none !important;
  -ms-overflow-style: none !important;
}
.ll-arr-track::-webkit-scrollbar {
  display: none !important;
  width: 0 !important;
  height: 0 !important;
}

.ll-table-area {
  overflow-y: auto !important;
  scrollbar-width: none !important;
  -ms-overflow-style: none !important;
}
.ll-table-area::-webkit-scrollbar {
  display: none !important;
  width: 0 !important;
  height: 0 !important;
}

.ll-ptrs {
  overflow-x: auto !important;
  scrollbar-width: none !important;
  -ms-overflow-style: none !important;
}
.ll-ptrs::-webkit-scrollbar {
  display: none !important;
  width: 0 !important;
  height: 0 !important;
}
.ll-house-val {
  font-size: 13px;
  font-weight: 800;
  font-family: monospace;
  color: var(--text);
}
.ll-house-idx {
  font-size: 9px;
  color: var(--muted);
  font-family: monospace;
  margin-top: 1px;
}
.ll-idx-tag {
  font-weight: 800;
  padding: 0 3px;
  border-radius: 3px;
}
.ll-tag-cur {
  color: #b45309;
  background: #fef3c7;
}

/* Screenshot 3: 1D Array Strip for Greedy Selection */
.ll-greedy-strip-view {
  display: flex;
  flex-direction: column;
  padding: 8px 12px;
  gap: 10px;
  overflow: auto;
}
.ll-arr-track {
  display: flex;
  align-items: flex-start;
  flex-wrap: wrap;
  padding: 6px 4px;
  gap: 12px;
  width: 100%;
  box-sizing: border-box;
}
.ll-arr-cell-wrap {
  display: flex;
  flex-direction: column;
  align-items: center;
  min-width: 0;
}
.ll-ptr-tag-wrap {
  height: 20px;
  display: flex;
  align-items: flex-end;
  justify-content: center;
  margin-bottom: 3px;
}
.ll-ptr-lbl {
  font-size: 11px;
  font-weight: 800;
  font-family: 'Consolas', 'Fira Code', monospace;
  display: inline-flex;
  align-items: center;
  line-height: 1;
  white-space: nowrap;
}
.ll-lbl-orange { color: #f97316; }
.ll-lbl-green { color: #10b981; }
.ll-lbl-purple { color: #9333ea; }

.ll-arr-box {
  width: 58px;
  height: 52px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  border: 2px solid var(--border2);
  border-radius: var(--radius);
  background: var(--surface);
  color: var(--text);
  font-weight: 700;
  font-size: 12.5px;
  box-shadow: var(--shadow-sm);
  transition: all 0.25s ease;
  padding: 2px;
}
.ll-box-val {
  font-size: 13.5px;
  font-weight: 800;
  font-family: monospace;
}
.ll-box-sub {
  font-size: 8.5px;
  font-weight: 700;
  font-family: monospace;
  margin-top: 2px;
}
.ll-box-cur {
  border-color: #f59e0b !important;
  background: #fef3c7 !important;
  color: #92400e !important;
  box-shadow: 0 0 0 3px rgba(245, 158, 11, 0.25) !important;
  transform: translateY(-2px);
}
.ll-box-comp {
  border-color: #a855f7 !important;
  background: #f3e8ff !important;
  color: #6b21a8 !important;
  transform: translateY(-2px);
}
.ll-box-found {
  border-color: #10b981 !important;
  background: #dcfce7 !important;
  color: #065f46 !important;
  box-shadow: 0 0 0 3px rgba(16, 185, 129, 0.2) !important;
}
.ll-box-uncalc {
  background: var(--surface2);
  color: var(--muted);
  border-color: var(--border);
}
.ll-arr-idx {
  font-size: 9.5px;
  color: var(--muted);
  font-family: monospace;
  margin-top: 4px;
  font-weight: 600;
  text-align: center;
}

/* Horizontal Knapsack Capacity Gauge */
.ll-gauge-container {
  display: flex;
  flex-direction: column;
  gap: 4px;
  background: var(--surface2);
  border: 1px solid var(--border);
  border-radius: var(--radius-sm);
  padding: 6px 12px;
}
.ll-gauge-header {
  display: flex;
  justify-content: space-between;
  font-size: 10px;
  font-weight: 700;
  font-family: monospace;
  color: var(--text2);
}
.ll-gauge-bar {
  width: 100%;
  height: 20px;
  background: #e2e8f0;
  border-radius: 4px;
  display: flex;
  overflow: hidden;
  border: 1px solid var(--border2);
}
.ll-gauge-segment {
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-family: monospace;
  font-size: 8.5px;
  font-weight: 800;
  color: #ffffff;
  transition: width 0.3s ease;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}
.seg-0 { background: #3b82f6; }
.seg-1 { background: #10b981; }
.seg-2 { background: #f59e0b; }
.seg-3 { background: #8b5cf6; }
.seg-4 { background: #ec4899; }
.seg-frac {
  background-image: repeating-linear-gradient(45deg, rgba(255,255,255,0.25), rgba(255,255,255,0.25) 5px, transparent 5px, transparent 10px) !important;
}

/* Screenshot 2: Decision Tree edges matching LIS */
.ll-edge-take {
  stroke: #10b981 !important;
  stroke-width: 2px !important;
}
.ll-edge-nottake {
  stroke: #94a3b8 !important;
  stroke-width: 1.5px !important;
  stroke-dasharray: 3 3 !important;
}
.ll-node-active {
  stroke: #f59e0b !important;
  stroke-width: 2.2px !important;
  fill: #fffbeb !important;
  filter: drop-shadow(0 0 5px rgba(245, 158, 11, 0.45)) !important;
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
.ll-calc-callout {
  background: var(--surface2);
  border-left: 3px solid var(--blue);
  border-radius: var(--radius-sm);
  padding: 6px 10px;
}
.ll-calc-line {
  font-family: monospace;
  font-size: 10px;
  color: var(--text2);
  line-height: 1.4;
}

</style>
