<script setup>
import { ref, reactive, computed, onMounted, onUnmounted, watch } from 'vue';

const props = defineProps({
  topic: { type: String, default: 'Greedy Algorithms' },
  subTopic: { type: String, default: 'Candy (LeetCode 135)' }
});

/* ==================================================================== */
/* APPROACH METADATA                                                    */
/* ==================================================================== */
const APPROACHES = [
  { id: 'brute', label: 'Brute Force', desc: 'O(n²) Quadratic — Iterative Neighbor Constraint Relaxation' },
  { id: 'greedy', label: 'Greedy', desc: 'O(n) Optimal — Two-Pass Left & Right Neighbor Resolution' }
];

/* ==================================================================== */
/* CODE SNIPPETS (Java, C, C++, Python)                                 */
/* Strict curly brace formatting on all control statements              */
/* ==================================================================== */
const CODES = {
  brute: {
    java: [
      ['',                  'import java.util.Scanner;'],
      ['',                  'import java.util.Arrays;'],
      ['',                  ''],
      ['',                  'public class Main {'],
      ['c_entry',           '    static int candy(int[] ratings, int n) {'],
      ['c_alloc_candies',   '        int[] candies = new int[n];'],
      ['c_init_candies_for','        for (int i = 0; i < n; i++) {'],
      ['c_init_candies_val','            candies[i] = 1;'],
      ['',                  '        }'],
      ['c_init_changed',    '        boolean hasChanged = true;'],
      ['c_while_loop',      '        while (hasChanged) {'],
      ['c_reset_changed',   '            hasChanged = false;'],
      ['c_for_relax',       '            for (int i = 0; i < n; i++) {'],
      ['c_check_left',      '                if (i > 0 && ratings[i] > ratings[i - 1]) {'],
      ['c_check_left_cond', '                    if (candies[i] <= candies[i - 1]) {'],
      ['c_update_left',     '                        candies[i] = candies[i - 1] + 1;'],
      ['c_flag_left',       '                        hasChanged = true;'],
      ['',                  '                    }'],
      ['',                  '                }'],
      ['c_check_right',     '                if (i < n - 1 && ratings[i] > ratings[i + 1]) {'],
      ['c_check_right_cond','                    if (candies[i] <= candies[i + 1]) {'],
      ['c_update_right',    '                        candies[i] = candies[i + 1] + 1;'],
      ['c_flag_right',      '                        hasChanged = true;'],
      ['',                  '                    }'],
      ['',                  '                }'],
      ['',                  '            }'],
      ['',                  '        }'],
      ['c_init_total',      '        int total = 0;'],
      ['c_for_sum',         '        for (int i = 0; i < n; i++) {'],
      ['c_add_sum',         '            total += candies[i];'],
      ['',                  '        }'],
      ['c_ret_total',       '        return total;'],
      ['',                  '    }'],
      ['',                  ''],
      ['',                  '    public static void main(String[] args) {'],
      ['m_scanner',         '        Scanner sc = new Scanner(System.in);'],
      ['m_read_n',          '        int n = sc.nextInt();'],
      ['m_alloc_ratings',   '        int[] ratings = new int[n];'],
      ['m_for_input',       '        for (int i = 0; i < n; i++) {'],
      ['m_read_rating',     '            ratings[i] = sc.nextInt();'],
      ['',                  '        }'],
      ['m_call_candy',      '        int ans = candy(ratings, n);'],
      ['m_print_ans',       '        System.out.println(ans);'],
      ['m_done',            '    }'],
      ['',                  '}']
    ],
    c: [
      ['',                  '#include <stdio.h>'],
      ['',                  '#include <stdbool.h>'],
      ['',                  ''],
      ['c_entry',           'int candy(int ratings[], int n) {'],
      ['c_alloc_candies',   '    int candies[100];'],
      ['c_init_candies_for','    for (int i = 0; i < n; i++) {'],
      ['c_init_candies_val','        candies[i] = 1;'],
      ['',                  '    }'],
      ['c_init_changed',    '    bool hasChanged = true;'],
      ['c_while_loop',      '    while (hasChanged) {'],
      ['c_reset_changed',   '        hasChanged = false;'],
      ['c_for_relax',       '        for (int i = 0; i < n; i++) {'],
      ['c_check_left',      '            if (i > 0 && ratings[i] > ratings[i - 1]) {'],
      ['c_check_left_cond', '                if (candies[i] <= candies[i - 1]) {'],
      ['c_update_left',     '                    candies[i] = candies[i - 1] + 1;'],
      ['c_flag_left',       '                    hasChanged = true;'],
      ['',                  '                }'],
      ['',                  '            }'],
      ['c_check_right',     '            if (i < n - 1 && ratings[i] > ratings[i + 1]) {'],
      ['c_check_right_cond','                if (candies[i] <= candies[i + 1]) {'],
      ['c_update_right',    '                    candies[i] = candies[i + 1] + 1;'],
      ['c_flag_right',      '                    hasChanged = true;'],
      ['',                  '                }'],
      ['',                  '            }'],
      ['',                  '        }'],
      ['',                  '    }'],
      ['c_init_total',      '    int total = 0;'],
      ['c_for_sum',         '    for (int i = 0; i < n; i++) {'],
      ['c_add_sum',         '        total += candies[i];'],
      ['',                  '    }'],
      ['c_ret_total',       '    return total;'],
      ['',                  '}'],
      ['',                  ''],
      ['int main() {'],
      ['m_scanner',         '    int n;'],
      ['m_read_n',          '    scanf("%d", &n);'],
      ['m_alloc_ratings',   '    int ratings[100];'],
      ['m_for_input',       '    for (int i = 0; i < n; i++) {'],
      ['m_read_rating',     '        scanf("%d", &ratings[i]);'],
      ['',                  '    }'],
      ['m_call_candy',      '    int ans = candy(ratings, n);'],
      ['m_print_ans',       '    printf("%d\\n", ans);'],
      ['m_done',            '    return 0;'],
      ['',                  '}']
    ],
    cpp: [
      ['',                  '#include <iostream>'],
      ['',                  '#include <vector>'],
      ['using namespace std;'],
      ['',                  ''],
      ['c_entry',           'int candy(const vector<int>& ratings, int n) {'],
      ['c_alloc_candies',   '    vector<int> candies(n, 1);'],
      ['c_init_candies_for','    for (int i = 0; i < n; i++) {'],
      ['c_init_candies_val','        candies[i] = 1;'],
      ['',                  '    }'],
      ['c_init_changed',    '    bool hasChanged = true;'],
      ['c_while_loop',      '    while (hasChanged) {'],
      ['c_reset_changed',   '        hasChanged = false;'],
      ['c_for_relax',       '        for (int i = 0; i < n; i++) {'],
      ['c_check_left',      '            if (i > 0 && ratings[i] > ratings[i - 1]) {'],
      ['c_check_left_cond', '                if (candies[i] <= candies[i - 1]) {'],
      ['c_update_left',     '                    candies[i] = candies[i - 1] + 1;'],
      ['c_flag_left',       '                    hasChanged = true;'],
      ['',                  '                }'],
      ['',                  '            }'],
      ['c_check_right',     '            if (i < n - 1 && ratings[i] > ratings[i + 1]) {'],
      ['c_check_right_cond','                if (candies[i] <= candies[i + 1]) {'],
      ['c_update_right',    '                    candies[i] = candies[i + 1] + 1;'],
      ['c_flag_right',      '                    hasChanged = true;'],
      ['',                  '                }'],
      ['',                  '            }'],
      ['',                  '        }'],
      ['',                  '    }'],
      ['c_init_total',      '    int total = 0;'],
      ['c_for_sum',         '    for (int i = 0; i < n; i++) {'],
      ['c_add_sum',         '        total += candies[i];'],
      ['',                  '    }'],
      ['c_ret_total',       '    return total;'],
      ['',                  '}'],
      ['',                  ''],
      ['int main() {'],
      ['m_scanner',         '    int n;'],
      ['m_read_n',          '    cin >> n;'],
      ['m_alloc_ratings',   '    vector<int> ratings(n);'],
      ['m_for_input',       '    for (int i = 0; i < n; i++) {'],
      ['m_read_rating',     '        cin >> ratings[i];'],
      ['',                  '    }'],
      ['m_call_candy',      '    int ans = candy(ratings, n);'],
      ['m_print_ans',       '    cout << ans << endl;'],
      ['m_done',            '    return 0;'],
      ['',                  '}']
    ],
    python: [
      ['',                  'import sys'],
      ['',                  ''],
      ['c_entry',           'def candy(ratings, n):'],
      ['c_alloc_candies',   '    candies = [1] * n'],
      ['c_init_candies_for','    for i in range(n):'],
      ['c_init_candies_val','        candies[i] = 1'],
      ['c_init_changed',    '    has_changed = True'],
      ['c_while_loop',      '    while has_changed:'],
      ['c_reset_changed',   '        has_changed = False'],
      ['c_for_relax',       '        for i in range(n):'],
      ['c_check_left',      '            if i > 0 and ratings[i] > ratings[i - 1]:'],
      ['c_check_left_cond', '                if candies[i] <= candies[i - 1]:'],
      ['c_update_left',     '                    candies[i] = candies[i - 1] + 1'],
      ['c_flag_left',       '                    has_changed = True'],
      ['c_check_right',     '            if i < n - 1 and ratings[i] > ratings[i + 1]:'],
      ['c_check_right_cond','                if candies[i] <= candies[i + 1]:'],
      ['c_update_right',    '                    candies[i] = candies[i + 1] + 1'],
      ['c_flag_right',      '                    has_changed = True'],
      ['c_init_total',      '    total = 0'],
      ['c_for_sum',         '    for i in range(n):'],
      ['c_add_sum',         '        total += candies[i]'],
      ['c_ret_total',       '    return total'],
      ['',                  ''],
      ['def main():'],
      ['m_scanner',         '    tokens = sys.stdin.read().split()'],
      ['m_read_n',          '    if not tokens: return'],
      ['',                  '    n = int(tokens[0])'],
      ['m_alloc_ratings',   '    ratings = []'],
      ['m_for_input',       '    for i in range(n):'],
      ['m_read_rating',     '        ratings.append(int(tokens[i + 1]))'],
      ['m_call_candy',      '    ans = candy(ratings, n)'],
      ['m_print_ans',       '    print(ans)'],
      ['m_done',            '    return'],
      ['',                  ''],
      ['if __name__ == "__main__":'],
      ['    main()']
    ]
  },
  greedy: {
    java: [
      ['',                  'import java.util.Scanner;'],
      ['',                  'import java.util.Arrays;'],
      ['',                  ''],
      ['',                  'public class Main {'],
      ['c_entry',           '    static int candy(int[] ratings, int n) {'],
      ['c_alloc_candies',   '        int[] candies = new int[n];'],
      ['c_init_candies_for','        for (int i = 0; i < n; i++) {'],
      ['c_init_candies_val','            candies[i] = 1;'],
      ['',                  '        }'],
      ['c_for_left',        '        for (int i = 1; i < n; i++) {'],
      ['c_check_left',      '            if (ratings[i] > ratings[i - 1]) {'],
      ['c_update_left',     '                candies[i] = candies[i - 1] + 1;'],
      ['',                  '            }'],
      ['',                  '        }'],
      ['c_for_right',       '        for (int i = n - 2; i >= 0; i--) {'],
      ['c_check_right',     '            if (ratings[i] > ratings[i + 1]) {'],
      ['c_check_right_cond','                if (candies[i] < candies[i + 1] + 1) {'],
      ['c_update_right',    '                    candies[i] = candies[i + 1] + 1;'],
      ['',                  '                }'],
      ['',                  '            }'],
      ['',                  '        }'],
      ['c_init_total',      '        int total = 0;'],
      ['c_for_sum',         '        for (int i = 0; i < n; i++) {'],
      ['c_add_sum',         '            total += candies[i];'],
      ['',                  '        }'],
      ['c_ret_total',       '        return total;'],
      ['',                  '    }'],
      ['',                  ''],
      ['',                  '    public static void main(String[] args) {'],
      ['m_scanner',         '        Scanner sc = new Scanner(System.in);'],
      ['m_read_n',          '        int n = sc.nextInt();'],
      ['m_alloc_ratings',   '        int[] ratings = new int[n];'],
      ['m_for_input',       '        for (int i = 0; i < n; i++) {'],
      ['m_read_rating',     '            ratings[i] = sc.nextInt();'],
      ['',                  '        }'],
      ['m_call_candy',      '        int ans = candy(ratings, n);'],
      ['m_print_ans',       '        System.out.println(ans);'],
      ['m_done',            '    }'],
      ['',                  '}']
    ],
    c: [
      ['',                  '#include <stdio.h>'],
      ['',                  ''],
      ['c_entry',           'int candy(int ratings[], int n) {'],
      ['c_alloc_candies',   '    int candies[100];'],
      ['c_init_candies_for','    for (int i = 0; i < n; i++) {'],
      ['c_init_candies_val','        candies[i] = 1;'],
      ['',                  '    }'],
      ['c_for_left',        '    for (int i = 1; i < n; i++) {'],
      ['c_check_left',      '        if (ratings[i] > ratings[i - 1]) {'],
      ['c_update_left',     '            candies[i] = candies[i - 1] + 1;'],
      ['',                  '        }'],
      ['    }'],
      ['c_for_right',       '    for (int i = n - 2; i >= 0; i--) {'],
      ['c_check_right',     '        if (ratings[i] > ratings[i + 1]) {'],
      ['c_check_right_cond','            if (candies[i] < candies[i + 1] + 1) {'],
      ['c_update_right',    '                candies[i] = candies[i + 1] + 1;'],
      ['',                  '            }'],
      ['',                  '        }'],
      ['    }'],
      ['c_init_total',      '    int total = 0;'],
      ['c_for_sum',         '    for (int i = 0; i < n; i++) {'],
      ['c_add_sum',         '        total += candies[i];'],
      ['',                  '    }'],
      ['c_ret_total',       '    return total;'],
      ['',                  '}'],
      ['',                  ''],
      ['int main() {'],
      ['m_scanner',         '    int n;'],
      ['m_read_n',          '    scanf("%d", &n);'],
      ['m_alloc_ratings',   '    int ratings[100];'],
      ['m_for_input',       '    for (int i = 0; i < n; i++) {'],
      ['m_read_rating',     '        scanf("%d", &ratings[i]);'],
      ['',                  '    }'],
      ['m_call_candy',      '    int ans = candy(ratings, n);'],
      ['m_print_ans',       '    printf("%d\\n", ans);'],
      ['m_done',            '    return 0;'],
      ['',                  '}']
    ],
    cpp: [
      ['',                  '#include <iostream>'],
      ['',                  '#include <vector>'],
      ['using namespace std;'],
      ['',                  ''],
      ['c_entry',           'int candy(const vector<int>& ratings, int n) {'],
      ['c_alloc_candies',   '    vector<int> candies(n, 1);'],
      ['c_init_candies_for','    for (int i = 0; i < n; i++) {'],
      ['c_init_candies_val','        candies[i] = 1;'],
      ['',                  '    }'],
      ['c_for_left',        '    for (int i = 1; i < n; i++) {'],
      ['c_check_left',      '        if (ratings[i] > ratings[i - 1]) {'],
      ['c_update_left',     '            candies[i] = candies[i - 1] + 1;'],
      ['',                  '        }'],
      ['    }'],
      ['c_for_right',       '    for (int i = n - 2; i >= 0; i--) {'],
      ['c_check_right',     '        if (ratings[i] > ratings[i + 1]) {'],
      ['c_check_right_cond','            if (candies[i] < candies[i + 1] + 1) {'],
      ['c_update_right',    '                candies[i] = candies[i + 1] + 1;'],
      ['',                  '            }'],
      ['',                  '        }'],
      ['    }'],
      ['c_init_total',      '    int total = 0;'],
      ['c_for_sum',         '    for (int i = 0; i < n; i++) {'],
      ['c_add_sum',         '        total += candies[i];'],
      ['',                  '    }'],
      ['c_ret_total',       '    return total;'],
      ['',                  '}'],
      ['',                  ''],
      ['int main() {'],
      ['m_scanner',         '    int n;'],
      ['m_read_n',          '    cin >> n;'],
      ['m_alloc_ratings',   '    vector<int> ratings(n);'],
      ['m_for_input',       '    for (int i = 0; i < n; i++) {'],
      ['m_read_rating',     '        cin >> ratings[i];'],
      ['',                  '    }'],
      ['m_call_candy',      '    int ans = candy(ratings, n);'],
      ['m_print_ans',       '    cout << ans << endl;'],
      ['m_done',            '    return 0;'],
      ['',                  '}']
    ],
    python: [
      ['',                  'import sys'],
      ['',                  ''],
      ['c_entry',           'def candy(ratings, n):'],
      ['c_alloc_candies',   '    candies = [1] * n'],
      ['c_init_candies_for','    for i in range(n):'],
      ['c_init_candies_val','        candies[i] = 1'],
      ['c_for_left',        '    for i in range(1, n):'],
      ['c_check_left',      '        if ratings[i] > ratings[i - 1]:'],
      ['c_update_left',     '            candies[i] = candies[i - 1] + 1'],
      ['c_for_right',       '    for i in range(n - 2, -1, -1):'],
      ['c_check_right',     '        if ratings[i] > ratings[i + 1]:'],
      ['c_check_right_cond','            if candies[i] < candies[i + 1] + 1:'],
      ['c_update_right',    '                candies[i] = candies[i + 1] + 1'],
      ['c_init_total',      '    total = 0'],
      ['c_for_sum',         '    for i in range(n):'],
      ['c_add_sum',         '        total += candies[i]'],
      ['c_ret_total',       '    return total'],
      ['',                  ''],
      ['def main():'],
      ['m_scanner',         '    tokens = sys.stdin.read().split()'],
      ['m_read_n',          '    if not tokens: return'],
      ['',                  '    n = int(tokens[0])'],
      ['m_alloc_ratings',   '    ratings = []'],
      ['m_for_input',       '    for i in range(n):'],
      ['m_read_rating',     '        ratings.append(int(tokens[i + 1]))'],
      ['m_call_candy',      '    ans = candy(ratings, n)'],
      ['m_print_ans',       '    print(ans)'],
      ['m_done',            '    return'],
      ['',                  ''],
      ['if __name__ == "__main__":'],
      ['    main()']
    ]
  }
};

/* ==================================================================== */
/* PSEUDOCODE FOR STUDENTS                                              */
/* ==================================================================== */
const PSEUDOCODES = {
  brute: [
    '// ─── Brute Force: Iterative Constraint Relaxation ───────────────────────',
    'function candy(ratings[], n):',
    '    // Step 1: Give every child at least 1 candy',
    '    candies = array of size n, initialized to 1',
    '',
    '    // Step 2: Keep adjusting until all neighbor constraints are met',
    '    hasChanged = true',
    '    while hasChanged:',
    '        hasChanged = false',
    '        for i = 0 to n - 1:',
    '            // Left neighbor check',
    '            if i > 0 and ratings[i] > ratings[i - 1] and candies[i] <= candies[i - 1]:',
    '                candies[i] = candies[i - 1] + 1',
    '                hasChanged = true',
    '',
    '            // Right neighbor check',
    '            if i < n - 1 and ratings[i] > ratings[i + 1] and candies[i] <= candies[i + 1]:',
    '                candies[i] = candies[i + 1] + 1',
    '                hasChanged = true',
    '',
    '    // Step 3: Sum up the minimal candies',
    '    total = sum(candies)',
    '    return total',
    '',
    '// Time  : O(n²)  — up to n passes in descending sequences',
    '// Space : O(n)   — candies allocation array'
  ],
  greedy: [
    '// ─── Greedy: Two-Pass Left & Right Neighbor Resolution ──────────────────',
    'function candy(ratings[], n):',
    '    // Step 1: Base allocation — every child receives 1 candy',
    '    candies = array of size n, filled with 1',
    '',
    '    // Step 2: Forward Pass (Left-to-Right)',
    '    // Enforce: If child[i] has higher rating than child[i-1], give more candies',
    '    for i = 1 to n - 1:',
    '        if ratings[i] > ratings[i - 1]:',
    '            candies[i] = candies[i - 1] + 1',
    '',
    '    // Step 3: Backward Pass (Right-to-Left)',
    '    // Enforce: If child[i] has higher rating than child[i+1], maintain max candies',
    '    for i = n - 2 down to 0:',
    '        if ratings[i] > ratings[i + 1]:',
    '            candies[i] = max(candies[i], candies[i + 1] + 1)',
    '',
    '    // Step 4: Total minimal distribution',
    '    total = sum of all candies[i]',
    '    return total',
    '',
    '// Time  : O(n)  — exactly two linear passes over the line',
    '// Space : O(n)  — auxiliary candies[] array'
  ]
};

/* ==================================================================== */
/* DEFAULT SAMPLE DATA & PRESETS                                        */
/* ==================================================================== */
const DEFAULT_RATINGS = [1, 0, 2];

const PRESETS = [
  { label: 'Ex 1: [1, 0, 2]', val: '1, 0, 2' },
  { label: 'Ex 2: [1, 2, 2]', val: '1, 2, 2' },
  { label: 'Valley & Peak: [1, 3, 2, 2, 1]', val: '1, 3, 2, 2, 1' },
  { label: 'Plateau: [1, 2, 87, 87, 87, 2, 1]', val: '1, 2, 87, 87, 87, 2, 1' },
  { label: 'Strict Descending: [5, 4, 3, 2, 1]', val: '5, 4, 3, 2, 1' }
];

function frame(title, rows) {
  return { title, rows };
}

/* ==================================================================== */
/* BUILD STEPS FOR ANIMATION (STRICT 1-TO-1 LINE-BY-LINE)               */
/* ==================================================================== */
function buildSteps(approach, rawRatings) {
  const steps = [];
  const ratings = (rawRatings && rawRatings.length >= 1) ? [...rawRatings] : [...DEFAULT_RATINGS];
  const n = ratings.length;

  /* Helper to produce state snapshots */
  function curState(extra = {}) {
    return {
      approach,
      n,
      ratings,
      ...extra
    };
  }

  /* ------------------------------------------------------------------ */
  /* APPROACH 1: BRUTE FORCE (ITERATIVE NEIGHBOR RELAXATION)            */
  /* ------------------------------------------------------------------ */
  if (approach === 'brute') {
    // 1. Scanner & Read N
    steps.push({
      code: 'm_scanner',
      badge: 'Faculty Note: Scanner sc = new Scanner(System.in); — Initializing input stream.',
      vars: [frame('main()', [['n', '?']])],
      state: curState({ phase: 'input', ratingsFilled: 0, candies: Array(n).fill('?'), currentI: -1 })
    });

    steps.push({
      code: 'm_read_n',
      badge: `Faculty Note: int n = sc.nextInt(); &rarr; Reading total number of children n = ${n}.`,
      vars: [frame('main()', [['n', String(n)]])],
      state: curState({ phase: 'input', ratingsFilled: 0, candies: Array(n).fill('?'), currentI: -1 })
    });

    // 2. Alloc ratings
    steps.push({
      code: 'm_alloc_ratings',
      badge: `Faculty Note: int[] ratings = new int[${n}]; &mdash; Allocating ratings array.`,
      vars: [frame('main()', [['n', String(n)]])],
      state: curState({ phase: 'input', ratingsFilled: 0, candies: Array(n).fill('?'), currentI: -1 })
    });

    // 3. Read ratings elements
    for (let i = 0; i < n; i++) {
      steps.push({
        code: 'm_for_input',
        badge: `Loop index i = ${i} of ${n}: reading rating for child ${i}.`,
        vars: [frame('main()', [['i', String(i)], ['n', String(n)]])],
        state: curState({ phase: 'input', ratingsFilled: i, candies: Array(n).fill('?'), currentI: i })
      });
      steps.push({
        code: 'm_read_rating',
        badge: `ratings[${i}] = sc.nextInt(); &rarr; ratings[${i}] = ${ratings[i]} stored.`,
        vars: [frame('main()', [['i', String(i)], ['ratings[' + i + ']', String(ratings[i])]])],
        state: curState({ phase: 'input', ratingsFilled: i + 1, candies: Array(n).fill('?'), currentI: i })
      });
    }

    // 4. Call candy()
    steps.push({
      code: 'm_call_candy',
      badge: `Calling candy(ratings, n=${n}). Iterative constraint relaxation begins!`,
      vars: [frame('main()', [['n', String(n)]])],
      state: curState({ phase: 'call', ratingsFilled: n, candies: Array(n).fill('?'), currentI: -1 })
    });

    // 5. Function Entry
    steps.push({
      code: 'c_entry',
      badge: `Entering candy(ratings, n=${n}). Rule: Every child receives &ge; 1 candy, and higher rating than neighbor receives strictly more!`,
      vars: [frame('main()', [['n', String(n)]]), frame('candy()', [['n', String(n)]])],
      state: curState({ phase: 'init', candies: Array(n).fill('?'), currentI: -1 })
    });

    // 6. Alloc candies array
    steps.push({
      code: 'c_alloc_candies',
      badge: `int[] candies = new int[${n}]; &mdash; Allocating candies distribution array.`,
      vars: [frame('main()', [['n', String(n)]]), frame('candy()', [['candies', `new int[${n}]`]])],
      state: curState({ phase: 'init', candies: Array(n).fill('?'), currentI: -1 })
    });

    // 7. Initialize all candies to 1
    const currentCandies = Array(n).fill(1);
    for (let i = 0; i < n; i++) {
      steps.push({
        code: 'c_init_candies_for',
        badge: `for (int i = 0; i < ${n}; i++) &mdash; Setting baseline 1 candy for Child [${i}].`,
        vars: [frame('main()', [['n', String(n)]]), frame('candy()', [['i', String(i)]])],
        state: curState({
          phase: 'init',
          candies: currentCandies.map((val, idx) => idx < i ? 1 : '?'),
          currentI: i
        })
      });
      steps.push({
        code: 'c_init_candies_val',
        badge: `candies[${i}] = 1; &mdash; Requirement: each child must have at least one candy.`,
        vars: [frame('main()', [['n', String(n)]]), frame('candy()', [['i', String(i)], ['candies[' + i + ']', '1']])],
        state: curState({
          phase: 'init',
          candies: currentCandies.map((val, idx) => idx <= i ? 1 : '?'),
          currentI: i
        })
      });
    }

    // 8. hasChanged = true
    let hasChanged = true;
    let passCount = 0;
    steps.push({
      code: 'c_init_changed',
      badge: 'boolean hasChanged = true; &mdash; Flag tracks if any neighbor violation was resolved during a pass.',
      vars: [frame('main()', [['n', String(n)]]), frame('candy()', [['hasChanged', 'true']])],
      state: curState({ phase: 'relax', candies: [...currentCandies], currentI: -1, passCount: 0, hasChanged: true })
    });

    // 9. Relaxation While Loop
    while (hasChanged) {
      passCount++;
      hasChanged = false;

      steps.push({
        code: 'c_while_loop',
        badge: `while (hasChanged) &mdash; Starting Relaxation Pass #${passCount}. Checking all ${n} children.`,
        vars: [frame('main()', [['n', String(n)]]), frame('candy()', [['pass', String(passCount)], ['hasChanged', 'true']])],
        state: curState({ phase: 'relax', candies: [...currentCandies], currentI: -1, passCount, hasChanged: true })
      });

      steps.push({
        code: 'c_reset_changed',
        badge: `hasChanged = false; &mdash; Resetting change tracker for Pass #${passCount}.`,
        vars: [frame('main()', [['n', String(n)]]), frame('candy()', [['pass', String(passCount)], ['hasChanged', 'false']])],
        state: curState({ phase: 'relax', candies: [...currentCandies], currentI: -1, passCount, hasChanged: false })
      });

      for (let i = 0; i < n; i++) {
        // Loop step
        steps.push({
          code: 'c_for_relax',
          badge: `Pass #${passCount}, Examining Child [${i}] (rating=${ratings[i]}, candies=${currentCandies[i]}).`,
          vars: [frame('main()', [['n', String(n)]]), frame('candy()', [['i', String(i)], ['candies[' + i + ']', String(currentCandies[i])]])],
          state: curState({ phase: 'relax', candies: [...currentCandies], currentI: i, passCount, hasChanged })
        });

        // Left neighbor check: i > 0 && ratings[i] > ratings[i-1]
        const leftCheck = i > 0 && ratings[i] > ratings[i - 1];
        steps.push({
          code: 'c_check_left',
          badge: i > 0
            ? `Left check: if (ratings[${i}] = ${ratings[i]} > ratings[${i - 1}] = ${ratings[i - 1]}) &rarr; ${leftCheck ? 'TRUE' : 'FALSE'}`
            : `Left check: i = 0 has no left neighbor &rarr; FALSE`,
          vars: [frame('main()', [['n', String(n)]]), frame('candy()', [['i', String(i)], ['leftCheck', String(leftCheck)]])],
          state: curState({ phase: 'relax', candies: [...currentCandies], currentI: i, compareNeighbor: i > 0 ? i - 1 : null, passCount, hasChanged })
        });

        if (leftCheck) {
          const leftCond = currentCandies[i] <= currentCandies[i - 1];
          steps.push({
            code: 'c_check_left_cond',
            badge: `Constraint violation check: if (candies[${i}] = ${currentCandies[i]} <= candies[${i - 1}] = ${currentCandies[i - 1]}) &rarr; ${leftCond ? 'TRUE (Violation! Child ' + i + ' needs more candies than Child ' + (i - 1) + ')' : 'FALSE (Already satisfied)'}`,
            vars: [frame('main()', [['n', String(n)]]), frame('candy()', [['i', String(i)], ['candies[' + i + ']', String(currentCandies[i])], ['candies[' + (i - 1) + ']', String(currentCandies[i - 1])]])],
            state: curState({ phase: 'relax', candies: [...currentCandies], currentI: i, compareNeighbor: i - 1, passCount, hasChanged })
          });

          if (leftCond) {
            currentCandies[i] = currentCandies[i - 1] + 1;
            steps.push({
              code: 'c_update_left',
              badge: `Relaxation: candies[${i}] updated to candies[${i - 1}] + 1 = ${currentCandies[i]}!`,
              vars: [frame('main()', [['n', String(n)]]), frame('candy()', [['i', String(i)], ['candies[' + i + ']', String(currentCandies[i])]])],
              state: curState({ phase: 'relax', candies: [...currentCandies], currentI: i, updatedIdx: i, compareNeighbor: i - 1, passCount, hasChanged })
            });

            hasChanged = true;
            steps.push({
              code: 'c_flag_left',
              badge: 'hasChanged = true; &mdash; A value was updated, so another pass will be needed to ensure stability.',
              vars: [frame('main()', [['n', String(n)]]), frame('candy()', [['hasChanged', 'true']])],
              state: curState({ phase: 'relax', candies: [...currentCandies], currentI: i, passCount, hasChanged: true })
            });
          }
        }

        // Right neighbor check: i < n - 1 && ratings[i] > ratings[i+1]
        const rightCheck = i < n - 1 && ratings[i] > ratings[i + 1];
        steps.push({
          code: 'c_check_right',
          badge: i < n - 1
            ? `Right check: if (ratings[${i}] = ${ratings[i]} > ratings[${i + 1}] = ${ratings[i + 1]}) &rarr; ${rightCheck ? 'TRUE' : 'FALSE'}`
            : `Right check: i = ${i} has no right neighbor &rarr; FALSE`,
          vars: [frame('main()', [['n', String(n)]]), frame('candy()', [['i', String(i)], ['rightCheck', String(rightCheck)]])],
          state: curState({ phase: 'relax', candies: [...currentCandies], currentI: i, compareNeighbor: i < n - 1 ? i + 1 : null, passCount, hasChanged })
        });

        if (rightCheck) {
          const rightCond = currentCandies[i] <= currentCandies[i + 1];
          steps.push({
            code: 'c_check_right_cond',
            badge: `Constraint violation check: if (candies[${i}] = ${currentCandies[i]} <= candies[${i + 1}] = ${currentCandies[i + 1]}) &rarr; ${rightCond ? 'TRUE (Violation! Child ' + i + ' needs more candies than Child ' + (i + 1) + ')' : 'FALSE (Already satisfied)'}`,
            vars: [frame('main()', [['n', String(n)]]), frame('candy()', [['i', String(i)], ['candies[' + i + ']', String(currentCandies[i])], ['candies[' + (i + 1) + ']', String(currentCandies[i + 1])]])],
            state: curState({ phase: 'relax', candies: [...currentCandies], currentI: i, compareNeighbor: i + 1, passCount, hasChanged })
          });

          if (rightCond) {
            currentCandies[i] = currentCandies[i + 1] + 1;
            steps.push({
              code: 'c_update_right',
              badge: `Relaxation: candies[${i}] updated to candies[${i + 1}] + 1 = ${currentCandies[i]}!`,
              vars: [frame('main()', [['n', String(n)]]), frame('candy()', [['i', String(i)], ['candies[' + i + ']', String(currentCandies[i])]])],
              state: curState({ phase: 'relax', candies: [...currentCandies], currentI: i, updatedIdx: i, compareNeighbor: i + 1, passCount, hasChanged })
            });

            hasChanged = true;
            steps.push({
              code: 'c_flag_right',
              badge: 'hasChanged = true; &mdash; Flagged for another verification pass.',
              vars: [frame('main()', [['n', String(n)]]), frame('candy()', [['hasChanged', 'true']])],
              state: curState({ phase: 'relax', candies: [...currentCandies], currentI: i, passCount, hasChanged: true })
            });
          }
        }
      }
    }

    // While loop terminates: no more changes!
    steps.push({
      code: 'c_while_loop',
      badge: `while (hasChanged) &rarr; FALSE! Convergence reached in ${passCount} passes. All neighbor constraints are satisfied.`,
      vars: [frame('main()', [['n', String(n)]]), frame('candy()', [['hasChanged', 'false'], ['totalPasses', String(passCount)]])],
      state: curState({ phase: 'sum', candies: [...currentCandies], currentI: -1, passCount, hasChanged: false })
    });

    // 10. Summing total candies
    let total = 0;
    steps.push({
      code: 'c_init_total',
      badge: 'int total = 0; &mdash; Accumulating total minimal candies to distribute.',
      vars: [frame('main()', [['n', String(n)]]), frame('candy()', [['total', '0']])],
      state: curState({ phase: 'sum', candies: [...currentCandies], currentI: -1, total: 0 })
    });

    for (let i = 0; i < n; i++) {
      steps.push({
        code: 'c_for_sum',
        badge: `for (int i = ${i}; i < ${n}; i++) &mdash; Adding Child [${i}] candies (${currentCandies[i]}).`,
        vars: [frame('main()', [['n', String(n)]]), frame('candy()', [['i', String(i)], ['candies[' + i + ']', String(currentCandies[i])], ['total', String(total)]])],
        state: curState({ phase: 'sum', candies: [...currentCandies], currentI: i, sumUpTo: i, total })
      });

      total += currentCandies[i];
      steps.push({
        code: 'c_add_sum',
        badge: `total += candies[${i}] (${currentCandies[i]}) &rarr; total = ${total}.`,
        vars: [frame('main()', [['n', String(n)]]), frame('candy()', [['i', String(i)], ['total', String(total)]])],
        state: curState({ phase: 'sum', candies: [...currentCandies], currentI: i, sumUpTo: i + 1, total })
      });
    }

    // 11. return total
    steps.push({
      code: 'c_ret_total',
      badge: `return total = ${total}; &mdash; Minimum candies required calculated via iterative relaxation.`,
      vars: [frame('main()', [['n', String(n)]]), frame('candy()', [['return', String(total)]])],
      state: curState({ phase: 'done', candies: [...currentCandies], currentI: -1, total })
    });

    // 12. main print & done
    steps.push({
      code: 'm_print_ans',
      badge: `System.out.println(ans = ${total}); &mdash; Optimal solution printed to stdout.`,
      vars: [frame('main()', [['ans', String(total)]])],
      state: curState({ phase: 'done', candies: [...currentCandies], currentI: -1, total })
    });

    steps.push({
      code: 'm_done',
      badge: `Program execution complete. Total candies distributed: ${total} across ${n} children in ${passCount} passes.`,
      vars: [frame('main()', [['status', 'completed']])],
      state: curState({ phase: 'done', candies: [...currentCandies], currentI: -1, total })
    });

    return steps;
  }

  /* ------------------------------------------------------------------ */
  /* APPROACH 2: GREEDY (TWO-PASS LEFT & RIGHT RESOLUTION)              */
  /* ------------------------------------------------------------------ */
  else {
    // 1. Scanner & Read N
    steps.push({
      code: 'm_scanner',
      badge: 'Faculty Note: Scanner sc = new Scanner(System.in); — Initializing input stream.',
      vars: [frame('main()', [['n', '?']])],
      state: curState({ phase: 'input', ratingsFilled: 0, candies: Array(n).fill('?'), currentI: -1 })
    });

    steps.push({
      code: 'm_read_n',
      badge: `Faculty Note: int n = sc.nextInt(); &rarr; Reading total number of children n = ${n}.`,
      vars: [frame('main()', [['n', String(n)]])],
      state: curState({ phase: 'input', ratingsFilled: 0, candies: Array(n).fill('?'), currentI: -1 })
    });

    // 2. Alloc ratings
    steps.push({
      code: 'm_alloc_ratings',
      badge: `Faculty Note: int[] ratings = new int[${n}]; &mdash; Allocating ratings array.`,
      vars: [frame('main()', [['n', String(n)]])],
      state: curState({ phase: 'input', ratingsFilled: 0, candies: Array(n).fill('?'), currentI: -1 })
    });

    // 3. Read ratings elements
    for (let i = 0; i < n; i++) {
      steps.push({
        code: 'm_for_input',
        badge: `Loop index i = ${i} of ${n}: reading rating for child ${i}.`,
        vars: [frame('main()', [['i', String(i)], ['n', String(n)]])],
        state: curState({ phase: 'input', ratingsFilled: i, candies: Array(n).fill('?'), currentI: i })
      });
      steps.push({
        code: 'm_read_rating',
        badge: `ratings[${i}] = sc.nextInt(); &rarr; ratings[${i}] = ${ratings[i]} stored.`,
        vars: [frame('main()', [['i', String(i)], ['ratings[' + i + ']', String(ratings[i])]])],
        state: curState({ phase: 'input', ratingsFilled: i + 1, candies: Array(n).fill('?'), currentI: i })
      });
    }

    // 4. Call candy()
    steps.push({
      code: 'm_call_candy',
      badge: `Calling candy(ratings, n=${n}) with Two-Pass Greedy approach!`,
      vars: [frame('main()', [['n', String(n)]])],
      state: curState({ phase: 'call', ratingsFilled: n, candies: Array(n).fill('?'), currentI: -1 })
    });

    // 5. Function Entry
    steps.push({
      code: 'c_entry',
      badge: 'Entering candy(ratings, n). Strategy: Pass 1 resolves Left constraints (L&rarr;R), Pass 2 resolves Right constraints (R&rarr;L)!',
      vars: [frame('main()', [['n', String(n)]]), frame('candy()', [['n', String(n)]])],
      state: curState({ phase: 'entry', candies: Array(n).fill('?'), currentI: -1 })
    });

    // 6. Alloc candies array
    steps.push({
      code: 'c_alloc_candies',
      badge: `Faculty Note: int[] candies = new int[${n}]; &mdash; Allocating candies distribution array.`,
      vars: [frame('main()', [['n', String(n)]]), frame('candy()', [['candies', `new int[${n}]`]])],
      state: curState({ phase: 'init', candies: Array(n).fill('?'), currentI: -1 })
    });

    // 7. Base initialization of all candies to 1
    const currentCandies = Array(n).fill(1);
    for (let i = 0; i < n; i++) {
      steps.push({
        code: 'c_init_candies_for',
        badge: `for (int i = 0; i < ${n}; i++) &mdash; Baseline 1 candy for Child [${i}].`,
        vars: [frame('main()', [['n', String(n)]]), frame('candy()', [['i', String(i)]])],
        state: curState({
          phase: 'init',
          candies: currentCandies.map((val, idx) => idx < i ? 1 : '?'),
          currentI: i
        })
      });
      steps.push({
        code: 'c_init_candies_val',
        badge: `candies[${i}] = 1; &mdash; Condition 1 satisfied: Every child receives at least 1 candy.`,
        vars: [frame('main()', [['n', String(n)]]), frame('candy()', [['i', String(i)], ['candies[' + i + ']', '1']])],
        state: curState({
          phase: 'init',
          candies: currentCandies.map((val, idx) => idx <= i ? 1 : '?'),
          currentI: i
        })
      });
    }

    // 8. Pass 1: Forward Pass (Left-to-Right)
    for (let i = 1; i < n; i++) {
      const isGreater = ratings[i] > ratings[i - 1];

      steps.push({
        code: 'c_for_left',
        badge: `Pass 1 (Left &rarr; Right): for (int i = ${i}; i < ${n}; i++) &mdash; Comparing Child [${i}] with left neighbor Child [${i - 1}].`,
        vars: [frame('main()', [['n', String(n)]]), frame('candy()', [['i', String(i)], ['ratings[' + i + ']', String(ratings[i])], ['ratings[' + (i - 1) + ']', String(ratings[i - 1])]])],
        state: curState({ phase: 'pass-left', candies: [...currentCandies], currentI: i, compareNeighbor: i - 1 })
      });

      steps.push({
        code: 'c_check_left',
        badge: `Check Left constraint: if (ratings[${i}] = ${ratings[i]} > ratings[${i - 1}] = ${ratings[i - 1]}) &rarr; ${isGreater ? 'TRUE! Child ' + i + ' must receive more candies than Child ' + (i - 1) : 'FALSE! Child ' + i + ' rating is not greater than left neighbor'}`,
        vars: [frame('main()', [['n', String(n)]]), frame('candy()', [['ratings[' + i + ']', String(ratings[i])], ['ratings[' + (i - 1) + ']', String(ratings[i - 1])], ['condition', String(isGreater)]])],
        state: curState({ phase: 'pass-left', candies: [...currentCandies], currentI: i, compareNeighbor: i - 1 })
      });

      if (isGreater) {
        currentCandies[i] = currentCandies[i - 1] + 1;
        steps.push({
          code: 'c_update_left',
          badge: `candies[${i}] = candies[${i - 1}] + 1 = ${currentCandies[i]}! Left-neighbor constraint satisfied.`,
          vars: [frame('main()', [['n', String(n)]]), frame('candy()', [['candies[' + i + ']', String(currentCandies[i])], ['candies[' + (i - 1) + ']', String(currentCandies[i - 1])]])],
          state: curState({ phase: 'pass-left', candies: [...currentCandies], currentI: i, updatedIdx: i, compareNeighbor: i - 1 })
        });
      }
    }

    // 9. Pass 2: Backward Pass (Right-to-Left)
    for (let i = n - 2; i >= 0; i--) {
      const isGreater = ratings[i] > ratings[i + 1];

      steps.push({
        code: 'c_for_right',
        badge: `Pass 2 (Right &rarr; Left): for (int i = ${i}; i >= 0; i--) &mdash; Comparing Child [${i}] with right neighbor Child [${i + 1}].`,
        vars: [frame('main()', [['n', String(n)]]), frame('candy()', [['i', String(i)], ['ratings[' + i + ']', String(ratings[i])], ['ratings[' + (i + 1) + ']', String(ratings[i + 1])]])],
        state: curState({ phase: 'pass-right', candies: [...currentCandies], currentI: i, compareNeighbor: i + 1 })
      });

      steps.push({
        code: 'c_check_right',
        badge: `Check Right constraint: if (ratings[${i}] = ${ratings[i]} > ratings[${i + 1}] = ${ratings[i + 1]}) &rarr; ${isGreater ? 'TRUE! Child ' + i + ' must receive more candies than Child ' + (i + 1) : 'FALSE! Child ' + i + ' rating is not greater than right neighbor'}`,
        vars: [frame('main()', [['n', String(n)]]), frame('candy()', [['ratings[' + i + ']', String(ratings[i])], ['ratings[' + (i + 1) + ']', String(ratings[i + 1])], ['condition', String(isGreater)]])],
        state: curState({ phase: 'pass-right', candies: [...currentCandies], currentI: i, compareNeighbor: i + 1 })
      });

      if (isGreater) {
        const needsUpdate = currentCandies[i] < currentCandies[i + 1] + 1;
        steps.push({
          code: 'c_check_right_cond',
          badge: `Check max condition: if (candies[${i}] = ${currentCandies[i]} < candies[${i + 1}] + 1 = ${currentCandies[i + 1] + 1}) &rarr; ${needsUpdate ? 'TRUE! Candies count must increase to satisfy right neighbor' : 'FALSE! candies[' + i + '] is already ' + currentCandies[i] + ', which exceeds right neighbor requirement'}`,
          vars: [frame('main()', [['n', String(n)]]), frame('candy()', [['candies[' + i + ']', String(currentCandies[i])], ['needed', String(currentCandies[i + 1] + 1)]])],
          state: curState({ phase: 'pass-right', candies: [...currentCandies], currentI: i, compareNeighbor: i + 1 })
        });

        if (needsUpdate) {
          currentCandies[i] = currentCandies[i + 1] + 1;
          steps.push({
            code: 'c_update_right',
            badge: `candies[${i}] = candies[${i + 1}] + 1 = ${currentCandies[i]}! Both left and right neighbor constraints are now simultaneously preserved.`,
            vars: [frame('main()', [['n', String(n)]]), frame('candy()', [['candies[' + i + ']', String(currentCandies[i])]])],
            state: curState({ phase: 'pass-right', candies: [...currentCandies], currentI: i, updatedIdx: i, compareNeighbor: i + 1 })
          });
        }
      }
    }

    // 10. Summing total candies
    let total = 0;
    steps.push({
      code: 'c_init_total',
      badge: 'int total = 0; &mdash; Initializing total counter to sum up optimal candies.',
      vars: [frame('main()', [['n', String(n)]]), frame('candy()', [['total', '0']])],
      state: curState({ phase: 'sum', candies: [...currentCandies], currentI: -1, total: 0 })
    });

    for (let i = 0; i < n; i++) {
      steps.push({
        code: 'c_for_sum',
        badge: `for (int i = ${i}; i < ${n}; i++) &mdash; Adding Child [${i}] candies (${currentCandies[i]}).`,
        vars: [frame('main()', [['n', String(n)]]), frame('candy()', [['i', String(i)], ['candies[' + i + ']', String(currentCandies[i])], ['total', String(total)]])],
        state: curState({ phase: 'sum', candies: [...currentCandies], currentI: i, sumUpTo: i, total })
      });

      total += currentCandies[i];
      steps.push({
        code: 'c_add_sum',
        badge: `total += candies[${i}] (${currentCandies[i]}) &rarr; total = ${total}.`,
        vars: [frame('main()', [['n', String(n)]]), frame('candy()', [['i', String(i)], ['total', String(total)]])],
        state: curState({ phase: 'sum', candies: [...currentCandies], currentI: i, sumUpTo: i + 1, total })
      });
    }

    // 11. return total
    steps.push({
      code: 'c_ret_total',
      badge: `Scan complete! return total = ${total}. Optimal distribution: [${currentCandies.join(', ')}].`,
      vars: [frame('main()', [['n', String(n)]]), frame('candy()', [['return', String(total)]])],
      state: curState({ phase: 'done', candies: [...currentCandies], currentI: -1, total })
    });

    // 12. main print & done
    steps.push({
      code: 'm_print_ans',
      badge: `System.out.println(ans = ${total}); &mdash; Computed in strictly linear O(n) time!`,
      vars: [frame('main()', [['ans', String(total)]])],
      state: curState({ phase: 'done', candies: [...currentCandies], currentI: -1, total })
    });

    steps.push({
      code: 'm_done',
      badge: `Program execution finished. Minimum candies required is ${total}.`,
      vars: [frame('main()', [['status', 'completed']])],
      state: curState({ phase: 'done', candies: [...currentCandies], currentI: -1, total })
    });

    return steps;
  }
}

/* ==================================================================== */
/* COMPONENT STATE & REACTIVE SETUP                                     */
/* ==================================================================== */
const currentApproach = ref('greedy');
const inputRatingsText = ref('1, 0, 2');
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

const currentRatings = computed(() => {
  const arr = parseArray(inputRatingsText.value);
  return arr.length >= 1 ? arr : DEFAULT_RATINGS;
});

const stepsData = reactive({
  steps: buildSteps('greedy', DEFAULT_RATINGS)
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
  stepsData.steps = buildSteps(appId, currentRatings.value);
  si.value = 0;
}

function applyInput() {
  playing.value = false;
  stepsData.steps = buildSteps(currentApproach.value, currentRatings.value);
  si.value = 0;
}

function selectPreset(val) {
  inputRatingsText.value = val;
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

            <!-- Custom Input Text -->
            <div class="ll-input-group">
              <label>ratings =</label>
              <input
                type="text"
                v-model="inputRatingsText"
                class="ll-text-input"
                placeholder="1, 0, 2"
                @keyup.enter="applyInput"
                style="width: 130px;"
              />
            </div>

            <!-- Preset Dropdown -->
            <!-- <select
              class="ll-preset-select"
              @change="selectPreset($event.target.value)"
              title="Load Sample Preset"
            >
              <option disabled selected value="">Presets</option>
              <option v-for="p in PRESETS" :key="p.val" :value="p.val">{{ p.label }}</option>
            </select> -->

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
              <!-- Top Banner: ratings[] and candies[] with ?-until-filled behaviour -->
              <div class="ll-house-banner">
                <div class="ll-dual-arrays-wrap">
                  <!-- Row 1: ratings[] -->
                  <div class="ll-arr-row">
                    <span class="ll-arr-tag">ratings[]:</span>
                    <div class="ll-arr-cells">
                      <div
                        v-for="(rVal, idx) in currentRatings"
                        :key="'r' + idx"
                        class="ll-arr-cell"
                        :class="{
                          'll-cell-active': st.currentI === idx,
                          'll-cell-neighbor': st.compareNeighbor === idx
                        }"
                      >
                        <span class="ll-cell-idx">[{{ idx }}]</span>
                        <span class="ll-cell-val">
                          {{ (st.ratingsFilled !== undefined && idx >= st.ratingsFilled) ? '?' : rVal }}
                        </span>
                      </div>
                    </div>
                  </div>

                  <!-- Row 2: candies[] -->
                  <div class="ll-arr-row">
                    <span class="ll-arr-tag">candies[]:</span>
                    <div class="ll-arr-cells">
                      <div
                        v-for="(rVal, idx) in currentRatings"
                        :key="'c' + idx"
                        class="ll-arr-cell"
                        :class="{
                          'll-cell-active': st.currentI === idx,
                          'll-cell-updated': st.updatedIdx === idx,
                          'll-cell-selected': st.candies && st.candies[idx] !== '?' && st.candies[idx] !== undefined
                        }"
                      >
                        <span class="ll-cell-idx">[{{ idx }}]</span>
                        <span class="ll-cell-val">
                          {{ (st.candies && st.candies[idx] !== undefined) ? st.candies[idx] : '?' }}
                        </span>
                      </div>
                    </div>
                  </div>
                </div>
              </div>

              <!-- Real-time Stats Chips Bar -->
              <div class="ll-ptrs">
                <div class="ll-ptr-chip">N: <b class="ll-c-blue">{{ st.n || currentRatings.length }}</b></div>
                <div class="ll-ptr-chip" v-if="st.phase">
                  Phase:
                  <b v-if="st.phase === 'pass-left'" class="ll-c-blue">&rarr; Left-to-Right Pass</b>
                  <b v-else-if="st.phase === 'pass-right'" class="ll-c-orange">&larr; Right-to-Left Pass</b>
                  <b v-else-if="st.phase === 'relax'" class="ll-c-purple">Relaxation Pass #{{ st.passCount || 1 }}</b>
                  <b v-else-if="st.phase === 'sum'" class="ll-c-orange">Summation</b>
                  <b v-else-if="st.phase === 'done'" class="ll-c-green">Completed</b>
                  <b v-else class="ll-c-blue">Input / Baseline</b>
                </div>
                <div class="ll-ptr-chip" v-if="st.currentI !== undefined && st.currentI >= 0">
                  Active Child: <b class="ll-c-orange">Child [{{ st.currentI }}]</b>
                </div>
                <div class="ll-ptr-chip" v-if="st.total !== undefined">
                  Total Candies: <b class="ll-c-green">{{ st.total }}</b>
                </div>
              </div>

              <!-- Visualization Container -->
              <div class="ll-viz-wrap" :style="{ height: vizHeight + 'px' }">
                <div class="ll-candy-viz-container">
                  <!-- Directional Pass Banner Indicator -->
                  <div class="ll-direction-banner" v-if="currentApproach === 'greedy'">
                    <span v-if="st.phase === 'pass-left'" class="ll-dir-tag ll-dir-left">
                      &#9654; PASS 1: SCAN LEFT &rarr; RIGHT &mdash; If ratings[i] &gt; ratings[i-1], give child[i] more candies!
                    </span>
                    <span v-else-if="st.phase === 'pass-right'" class="ll-dir-tag ll-dir-right">
                      &#9664; PASS 2: SCAN RIGHT &rarr; LEFT &mdash; If ratings[i] &gt; ratings[i+1], maintain max(candies[i], candies[i+1] + 1)!
                    </span>
                    <span v-else-if="st.phase === 'sum' || st.phase === 'done'" class="ll-dir-tag ll-dir-done">
                      &#10003; BOTH CONSTRAINTS SATISFIED &mdash; All neighbor conditions hold globally! Total = {{ st.total !== undefined ? st.total : '?' }}
                    </span>
                    <span v-else class="ll-dir-tag ll-dir-init">
                      &#9679; INITIAL BASELINE &mdash; Every child receives minimum 1 candy.
                    </span>
                  </div>
                  <div class="ll-direction-banner" v-else>
                    <span v-if="st.phase === 'relax'" class="ll-dir-tag ll-dir-relax">
                      &#8635; RELAXATION PASS #{{ st.passCount || 1 }} &mdash; Iteratively resolving neighbor constraint violations until convergence.
                    </span>
                    <span v-else-if="st.phase === 'sum' || st.phase === 'done'" class="ll-dir-tag ll-dir-done">
                      &#10003; CONVERGENCE REACHED &mdash; No more violations detected. Total Candies = {{ st.total }}
                    </span>
                    <span v-else class="ll-dir-tag ll-dir-init">
                      &#9679; INITIAL BASELINE &mdash; Every child receives minimum 1 candy.
                    </span>
                  </div>

                  <!-- Children Lineup Stage -->
                  <div class="ll-stage-track">
                    <div
                      v-for="(rVal, i) in currentRatings"
                      :key="'child' + i"
                      class="ll-child-pod"
                      :class="{
                        'll-pod-active': st.currentI === i,
                        'll-pod-neighbor': st.compareNeighbor === i,
                        'll-pod-updated': st.updatedIdx === i
                      }"
                    >
                      <!-- Child Header -->
                      <div class="ll-pod-header">
                        <span class="ll-pod-id">Kid {{ i }}</span>
                        <span class="ll-pod-avatar">{{ i % 2 === 0 ? '👦' : '👧' }}</span>
                      </div>

                      <!-- Rating Badge -->
                      <div class="ll-pod-rating" title="Rating">
                        <span class="ll-rating-star">&#9733;</span>
                        <span class="ll-rating-val">{{ (st.ratingsFilled !== undefined && i >= st.ratingsFilled) ? '?' : rVal }}</span>
                      </div>

                      <!-- Relation arrow between this child and next child -->
                      <div
                        v-if="i < currentRatings.length - 1"
                        class="ll-relation-badge"
                        :class="{
                          'll-rel-gt': rVal > currentRatings[i + 1],
                          'll-rel-lt': rVal < currentRatings[i + 1],
                          'll-rel-eq': rVal === currentRatings[i + 1]
                        }"
                      >
                        {{ rVal > currentRatings[i + 1] ? '>' : (rVal < currentRatings[i + 1] ? '<' : '=') }}
                      </div>

                      <!-- Candy Counter Pedestal -->
                      <div class="ll-pod-candies">
                        <div class="ll-candy-count">
                          <span class="ll-candy-num">{{ (st.candies && st.candies[i] !== undefined && st.candies[i] !== '?') ? st.candies[i] : '?' }}</span>
                          <span class="ll-candy-icon">🍬</span>
                        </div>

                        <!-- Stacked Candies Preview -->
                        <div class="ll-candy-stack" v-if="st.candies && typeof st.candies[i] === 'number'">
                          <span
                            v-for="cIdx in Math.min(6, st.candies[i])"
                            :key="cIdx"
                            class="ll-candy-dot"
                            title="1 Candy"
                          ></span>
                          <span v-if="st.candies[i] > 6" class="ll-candy-more">+{{ st.candies[i] - 6 }}</span>
                        </div>
                      </div>
                    </div>
                  </div>

                  <!-- Active Evaluation Detail Pill -->
                  <div class="ll-eval-strip" v-if="st.currentI !== undefined && st.currentI >= 0">
                    <span class="ll-eval-title">Current Focus:</span>
                    <span class="ll-eval-detail">
                      Child [{{ st.currentI }}] &mdash; Rating:
                      <strong>{{ currentRatings[st.currentI] }}</strong>, Candies:
                      <strong>{{ (st.candies && st.candies[st.currentI] !== undefined) ? st.candies[st.currentI] : '?' }}</strong>
                      <template v-if="st.compareNeighbor !== null && st.compareNeighbor !== undefined">
                        &nbsp;|&nbsp; Compared with Neighbor Child [{{ st.compareNeighbor }}]
                        (Rating: {{ currentRatings[st.compareNeighbor] }}, Candies: {{ (st.candies && st.candies[st.compareNeighbor] !== undefined) ? st.candies[st.compareNeighbor] : '?' }})
                      </template>
                    </span>
                  </div>
                </div>
              </div>

              <!-- Vertical Resizer between Viz and Variable Table -->
              <div class="ll-vresizer" ref="vizResizerRef"></div>

              <!-- Legend Strip -->
              <div class="ll-legend">
                <span class="ll-leg"><span class="ll-legdot ll-legdot-cur"></span>Active Child Evaluated</span>
                <span class="ll-leg"><span class="ll-legdot ll-legdot-purple"></span>Neighbor Compared</span>
                <span class="ll-leg"><span class="ll-legdot ll-legdot-new"></span>Candies Updated / Increment</span>
                <span class="ll-leg"><span class="ll-legdot ll-legdot-green"></span>Constraint Satisfied</span>
                <span class="ll-leg"><span class="ll-legdot ll-legdot-blue"></span>Directional Scan</span>
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
                      {{ f.title }}(<span v-for="(r, i) in f.rows" :key="i"><span v-if="i > 0">, </span><span class="ll-fname">{{ r[0] }}</span>=<span :class="r[0] === 'return' || r[0] === 'ans' || r[0] === 'total' ? 'll-c-green' : (depth === s.vars.length - 1 ? 'll-c-orange' : 'll-c-blue')" style="font-weight:700">{{ r[1] }}</span></span>)<span v-if="depth === s.vars.length - 1" class="ll-now"> &#9668; current</span>
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
                    'll-badge-error': s.badge && s.badge.includes('Violation'),
                    'll-badge-success': s.badge && (s.badge.includes('complete') || s.badge.includes('Complete') || s.badge.includes('Optimal') || s.badge.includes('Convergence') || s.badge.includes('satisfied'))
                  }"
                >
                  {{ s.badge || 'Ready to visualize Candy distribution.' }}
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
                      Iteratively scans the entire array of children and relaxes neighbor constraints.
                      When a child with a higher rating has less than or equal candies compared to its neighbor,
                      its candy count is incremented. The passes repeat until no more violations exist.
                    </p>

                    <h4 class="ll-cx-sub">Step-by-step Breakdown</h4>
                    <table class="ll-complexity-table">
                      <thead>
                        <tr><th>Operation</th><th>Time</th><th>Space</th><th>Reason</th></tr>
                      </thead>
                      <tbody>
                        <tr>
                          <td>Read input (ratings[])</td>
                          <td class="ll-cx-good">O(n)</td>
                          <td class="ll-cx-good">O(n)</td>
                          <td>One pass to store array elements</td>
                        </tr>
                        <tr>
                          <td>Initialize candies[] array</td>
                          <td class="ll-cx-good">O(n)</td>
                          <td class="ll-cx-good">O(n)</td>
                          <td>Fill all n elements with 1 candy</td>
                        </tr>
                        <tr>
                          <td>Iterative Neighbor Relaxation</td>
                          <td class="ll-cx-bad">O(n²)</td>
                          <td class="ll-cx-good">O(1)</td>
                          <td>In worst case (strictly descending), requires n passes over n items</td>
                        </tr>
                        <tr>
                          <td>Sum total candies</td>
                          <td class="ll-cx-good">O(n)</td>
                          <td class="ll-cx-good">O(1)</td>
                          <td>Single accumulation loop</td>
                        </tr>
                      </tbody>
                    </table>

                    <h4 class="ll-cx-sub">Overall Complexity</h4>
                    <div class="ll-cx-summary-grid">
                      <div class="ll-cx-card ll-cx-card-bad">
                        <div class="ll-cx-card-label">Time</div>
                        <div class="ll-cx-card-val">O(n²)</div>
                        <div class="ll-cx-card-note">Quadratic &mdash; TLE on large arrays</div>
                      </div>
                      <div class="ll-cx-card ll-cx-card-good">
                        <div class="ll-cx-card-label">Space</div>
                        <div class="ll-cx-card-val">O(n)</div>
                        <div class="ll-cx-card-note">Candies distribution array</div>
                      </div>
                      <div class="ll-cx-card ll-cx-card-good">
                        <div class="ll-cx-card-label">Optimal?</div>
                        <div class="ll-cx-card-val">YES</div>
                        <div class="ll-cx-card-note">Guaranteed correct distribution</div>
                      </div>
                    </div>

                    <div class="ll-note">
                      <strong>Why does Brute Force degrade to O(n²)?</strong>
                      Consider a descending sequence like <code>[5, 4, 3, 2, 1]</code>.
                      Each pass only propagates candy increases by one step to the left.
                      Thus, it requires $n$ passes, each taking $O(n)$ time!
                    </div>
                  </template>

                  <!-- ════════ GREEDY COMPLEXITY ════════ -->
                  <template v-else>
                    <h3 class="ll-cx-heading">Greedy Algorithm &mdash; Complexity Analysis</h3>
                    <p class="ll-cx-intro">
                      Decomposes the bidirectional neighbor requirements into two independent passes:
                      a <strong>left-to-right pass</strong> to satisfy left neighbors, and a
                      <strong>right-to-left pass</strong> to satisfy right neighbors while preserving maximum values.
                    </p>

                    <h4 class="ll-cx-sub">Step-by-step Breakdown</h4>
                    <table class="ll-complexity-table">
                      <thead>
                        <tr><th>Operation</th><th>Time</th><th>Space</th><th>Reason</th></tr>
                      </thead>
                      <tbody>
                        <tr>
                          <td>Read input (ratings[])</td>
                          <td class="ll-cx-good">O(n)</td>
                          <td class="ll-cx-good">O(n)</td>
                          <td>Single pass input reading</td>
                        </tr>
                        <tr>
                          <td>Initialize candies[] to 1</td>
                          <td class="ll-cx-good">O(n)</td>
                          <td class="ll-cx-good">O(n)</td>
                          <td>Fills array of size n</td>
                        </tr>
                        <tr>
                          <td>Forward Pass (Left &rarr; Right)</td>
                          <td class="ll-cx-good">O(n)</td>
                          <td class="ll-cx-good">O(1)</td>
                          <td>i = 1..n-1: checks ratings[i] &gt; ratings[i-1]</td>
                        </tr>
                        <tr>
                          <td>Backward Pass (Right &rarr; Left)</td>
                          <td class="ll-cx-good">O(n)</td>
                          <td class="ll-cx-good">O(1)</td>
                          <td>i = n-2..0: checks ratings[i] &gt; ratings[i+1]</td>
                        </tr>
                        <tr>
                          <td>Sum total candies</td>
                          <td class="ll-cx-good">O(n)</td>
                          <td class="ll-cx-good">O(1)</td>
                          <td>Single loop summing array values</td>
                        </tr>
                      </tbody>
                    </table>

                    <h4 class="ll-cx-sub">Overall Complexity</h4>
                    <div class="ll-cx-summary-grid">
                      <div class="ll-cx-card ll-cx-card-good">
                        <div class="ll-cx-card-label">Time</div>
                        <div class="ll-cx-card-val">O(n)</div>
                        <div class="ll-cx-card-note">Linear time &mdash; exactly 2 passes</div>
                      </div>
                      <div class="ll-cx-card ll-cx-card-good">
                        <div class="ll-cx-card-label">Space</div>
                        <div class="ll-cx-card-val">O(n)</div>
                        <div class="ll-cx-card-note">Auxiliary candies[] array</div>
                      </div>
                      <div class="ll-cx-card ll-cx-card-good">
                        <div class="ll-cx-card-label">Optimal?</div>
                        <div class="ll-cx-card-val">YES</div>
                        <div class="ll-cx-card-note">Global minimum proved correct</div>
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
                          <td class="ll-cx-bad">O(n²)</td>
                          <td class="ll-cx-good">O(n)</td>
                          <td class="ll-cx-bad">Slow &mdash; repeats passes until convergence</td>
                        </tr>
                        <tr>
                          <td><strong>Greedy (Two-Pass)</strong></td>
                          <td class="ll-cx-good">O(n)</td>
                          <td class="ll-cx-good">O(n)</td>
                          <td class="ll-cx-good">Optimal &mdash; solves in linear passes</td>
                        </tr>
                      </tbody>
                    </table>

                    <div class="ll-note">
                      <strong>Core Mathematical Proof:</strong>
                      By taking <code>candies[i] = max(candies[i], candies[i+1] + 1)</code> in the reverse pass,
                      we ensure child <code>i</code> satisfies both the right neighbor constraint AND the previously
                      established left neighbor constraint simultaneously, yielding the global minimum distribution!
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

.ll-preset-select {
  background: var(--surface2);
  border: 1px solid var(--border2);
  color: var(--text2);
  border-radius: var(--radius-sm);
  padding: 3px 6px;
  font-size: 11px;
  font-weight: 600;
  cursor: pointer;
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
  width: 60px;
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

.ll-cell-neighbor {
  border-color: #8b5cf6 !important;
  background: #f5f3ff !important;
}

.ll-cell-updated {
  border-color: #10b981 !important;
  background: #dcfce7 !important;
  animation: ll-pulse 1.2s ease-in-out;
}

.ll-cell-selected {
  border-color: #10b981 !important;
  background: #f0fdf4;
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

.ll-candy-viz-container {
  display: flex;
  flex-direction: column;
  width: 100%;
  height: 100%;
  padding: 8px 12px;
  gap: 8px;
}

.ll-direction-banner {
  display: flex;
  align-items: center;
  border-radius: var(--radius-sm);
  overflow: hidden;
}

.ll-dir-tag {
  font-size: 10px;
  font-weight: 700;
  padding: 3px 8px;
  width: 100%;
  border-radius: 4px;
}

.ll-dir-left { background: #eff6ff; color: #1d4ed8; border: 1px solid #bfdbfe; }
.ll-dir-right { background: #fff7ed; color: #c2410c; border: 1px solid #fed7aa; }
.ll-dir-relax { background: #fdf4ff; color: #86198f; border: 1px solid #f0abfc; }
.ll-dir-done { background: #f0fdf4; color: #15803d; border: 1px solid #86efac; }
.ll-dir-init { background: #f8fafc; color: #475569; border: 1px solid #e2e8f0; }

/* Stage Track for Children */
.ll-stage-track {
  display: flex;
  gap: 12px;
  align-items: flex-start;
  padding: 8px 6px;
  overflow-x: auto;
  position: relative;
}

.ll-child-pod {
  display: flex;
  flex-direction: column;
  align-items: center;
  background: var(--surface);
  border: 1.5px solid var(--border2);
  border-radius: 8px;
  width: 62px;
  padding: 6px 4px;
  transition: all .25s ease;
  position: relative;
  box-shadow: var(--shadow-sm);
  flex-shrink: 0;
}

.ll-pod-active {
  border-color: #f59e0b !important;
  background: #fffbeb !important;
  box-shadow: 0 0 8px rgba(245, 158, 11, .5);
  animation: ll-pulse 1.2s infinite ease-in-out;
}

.ll-pod-neighbor {
  border-color: #8b5cf6 !important;
  background: #f5f3ff !important;
}

.ll-pod-updated {
  border-color: #10b981 !important;
  background: #dcfce7 !important;
}

.ll-pod-header {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1px;
}

.ll-pod-id {
  font-size: 8.5px;
  font-family: monospace;
  color: var(--muted);
  font-weight: 700;
}

.ll-pod-avatar {
  font-size: 16px;
  line-height: 1.1;
}

.ll-pod-rating {
  display: flex;
  align-items: center;
  gap: 2px;
  background: #f1f5f9;
  border-radius: 10px;
  padding: 1px 6px;
  margin: 3px 0;
}

.ll-rating-star {
  font-size: 9px;
  color: #eab308;
}

.ll-rating-val {
  font-size: 10.5px;
  font-weight: 800;
  font-family: monospace;
  color: var(--text);
}

.ll-relation-badge {
  position: absolute;
  right: -9px;
  top: 40%;
  font-size: 9px;
  font-weight: 900;
  background: #ffffff;
  border: 1px solid var(--border2);
  border-radius: 50%;
  width: 14px;
  height: 14px;
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 10;
  box-shadow: 0 1px 2px rgba(0,0,0,.1);
}

.ll-rel-gt { color: #16a34a; border-color: #86efac; background: #f0fdf4; }
.ll-rel-lt { color: #dc2626; border-color: #fca5a5; background: #fef2f2; }
.ll-rel-eq { color: #64748b; border-color: #cbd5e1; background: #f8fafc; }

.ll-pod-candies {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
  background: #faf5ff;
  border: 1px dashed #d8b4fe;
  border-radius: 6px;
  padding: 3px 2px;
  margin-top: 2px;
}

.ll-candy-count {
  display: flex;
  align-items: center;
  gap: 2px;
}

.ll-candy-num {
  font-size: 13px;
  font-weight: 800;
  font-family: monospace;
  color: #7e22ce;
}

.ll-candy-icon {
  font-size: 11px;
}

.ll-candy-stack {
  display: flex;
  gap: 1px;
  flex-wrap: wrap;
  justify-content: center;
  margin-top: 2px;
}

.ll-candy-dot {
  font-size: 7.5px;
  line-height: 1;
}

.ll-candy-more {
  font-size: 7.5px;
  font-weight: 700;
  color: #9333ea;
}

.ll-eval-strip {
  display: flex;
  align-items: center;
  gap: 6px;
  background: #f8fafc;
  border: 1px solid var(--border);
  border-radius: var(--radius-sm);
  padding: 4px 8px;
  font-size: 10px;
}

.ll-eval-title {
  font-weight: 700;
  color: var(--text2);
}

.ll-eval-detail {
  font-family: monospace;
  color: var(--text);
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
.ll-legdot-purple { background: #f3e8ff; border: 1.5px solid #9333ea; }
.ll-legdot-green { background: #10b981; }
.ll-legdot-blue { background: #3b82f6; }

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
