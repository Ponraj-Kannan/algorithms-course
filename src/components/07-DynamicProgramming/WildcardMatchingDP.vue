<script setup>
import { ref, computed, reactive, onMounted, onUnmounted, nextTick } from 'vue';

/* ==================================================================== */
/* PROPS & COMPONENT CONFIGURATION                                      */
/* ==================================================================== */
const props = defineProps({
  topic: { type: String, default: 'Dynamic Programming' },
  subTopic: { type: String, default: 'Wildcard Matching (LeetCode 44)' }
});

/* ==================================================================== */
/* APPROACHES & MULTI-LANGUAGE 100% EXECUTABLE CODE SPECIFICATIONS      */
/* ==================================================================== */
const APPROACHES = [
  { id: 'recursion', label: 'Brute Force', desc: 'O(2^(m+n)) Exponential — 2-Way Branching on Star (*)' },
  { id: 'memoization', label: 'Memoization', desc: 'O(m×n) Time & O(m×n) Space — Top-Down with 2D State Cache' },
  { id: 'tabulation', label: 'Tabulation', desc: 'O(m×n) Time & O(m×n) Space — Bottom-Up 2D Boolean DP Matrix' }
];

const PRESETS = [
  { label: 'aa & *', desc: 'Star matches entire string (True)', s: 'aa', p: '*' },
  { label: 'cb & ?a', desc: 'Mismatch at second character (False)', s: 'cb', p: '?a' },
  { label: 'adceb & *a*b', desc: 'Multiple stars matching partitions (True)', s: 'adceb', p: '*a*b' },
  { label: 'acdcb & a*c?b', desc: 'Subtle mismatch on star sequence (False)', s: 'acdcb', p: 'a*c?b' },
  { label: 'aabcz & *abc?', desc: 'Star matches prefix, ? matches "z" (True)', s: 'aabcz', p: '*abc?' },
  { label: '"" & *****', desc: 'Empty string matched by all stars (True)', s: '', p: '*****' },
  { label: 'mississippi & m*si*', desc: 'Long prefix wildcards test (True)', s: 'mississippi', p: 'm*si*' }
];

const CODES = {
  recursion: {
    java: [
      ['',              'import java.util.Scanner;'],
      ['',              ''],
      ['',              'public class Main {'],
      ['c_entry',       '    static boolean isMatch(String s, String p, int i, int j) {'],
      ['c_base_both',   '        if (i == 0 && j == 0) {'],
      ['',              '            return true;'],
      ['',              '        }'],
      ['c_base_p_empty','        if (j == 0) {'],
      ['',              '            return false;'],
      ['',              '        }'],
      ['c_base_s_empty','        if (i == 0) {'],
      ['',              '            return p.charAt(j - 1) == \'*\' && isMatch(s, p, 0, j - 1);'],
      ['',              '        }'],
      ['c_char_chk',    '        if (p.charAt(j - 1) == s.charAt(i - 1) || p.charAt(j - 1) == \'?\') {'],
      ['c_char_rec',    '            return isMatch(s, p, i - 1, j - 1);'],
      ['c_star_chk',    '        } else if (p.charAt(j - 1) == \'*\') {'],
      ['c_star_rec',    '            boolean matchEmpty = isMatch(s, p, i, j - 1);'],
      ['',              '            boolean matchChars = isMatch(s, p, i - 1, j);'],
      ['c_ret_or',      '            return matchEmpty || matchChars;'],
      ['',              '        }'],
      ['c_ret_false',   '        return false;'],
      ['',              '    }'],
      ['',              ''],
      ['',              '    public static void main(String[] args) {'],
      ['',              '        Scanner sc = new Scanner(System.in);'],
      ['m_read_s',      '        String s = sc.next();'],
      ['m_read_p',      '        String p = sc.next();'],
      ['m_call_match',  '        boolean ans = isMatch(s, p, s.length(), p.length());'],
      ['m_print',       '        System.out.println(ans);'],
      ['m_done',        '    }'],
      ['',              '}']
    ],
    c: [
      ['',              '#include <stdio.h>'],
      ['',              '#include <string.h>'],
      ['',              '#include <stdbool.h>'],
      ['',              ''],
      ['c_entry',       'bool isMatch(const char* s, const char* p, int i, int j) {'],
      ['c_base_both',   '    if (i == 0 && j == 0) {'],
      ['',              '        return true;'],
      ['',              '    }'],
      ['c_base_p_empty','    if (j == 0) {'],
      ['',              '        return false;'],
      ['',              '    }'],
      ['c_base_s_empty','    if (i == 0) {'],
      ['',              '        return p[j - 1] == \'*\' && isMatch(s, p, 0, j - 1);'],
      ['',              '    }'],
      ['c_char_chk',    '    if (p[j - 1] == s[i - 1] || p[j - 1] == \'?\') {'],
      ['c_char_rec',    '        return isMatch(s, p, i - 1, j - 1);'],
      ['c_star_chk',    '    } else if (p[j - 1] == \'*\') {'],
      ['c_star_rec',    '        bool matchEmpty = isMatch(s, p, i, j - 1);'],
      ['',              '        bool matchChars = isMatch(s, p, i - 1, j);'],
      ['c_ret_or',      '        return matchEmpty || matchChars;'],
      ['',              '    }'],
      ['c_ret_false',   '    return false;'],
      ['',              '}'],
      ['',              ''],
      ['',              'int main() {'],
      ['m_read_s',      '    char s[ 100 ] = ""; scanf("%s", s);'],
      ['m_read_p',      '    char p[ 100 ] = ""; scanf("%s", p);'],
      ['m_call_match',  '    bool ans = isMatch(s, p, strlen(s), strlen(p));'],
      ['m_print',       '    printf("%s\\n", ans ? "true" : "false");'],
      ['m_done',        '    return 0;'],
      ['',              '}']
    ],
    cpp: [
      ['',              '#include <iostream>'],
      ['',              '#include <string>'],
      ['',              'using namespace std;'],
      ['',              ''],
      ['c_entry',       'bool isMatch(const string& s, const string& p, int i, int j) {'],
      ['c_base_both',   '    if (i == 0 && j == 0) {'],
      ['',              '        return true;'],
      ['',              '    }'],
      ['c_base_p_empty','    if (j == 0) {'],
      ['',              '        return false;'],
      ['',              '    }'],
      ['c_base_s_empty','    if (i == 0) {'],
      ['',              '        return p[j - 1] == \'*\' && isMatch(s, p, 0, j - 1);'],
      ['',              '    }'],
      ['c_char_chk',    '    if (p[j - 1] == s[i - 1] || p[j - 1] == \'?\') {'],
      ['c_char_rec',    '        return isMatch(s, p, i - 1, j - 1);'],
      ['c_star_chk',    '    } else if (p[j - 1] == \'*\') {'],
      ['c_star_rec',    '        bool matchEmpty = isMatch(s, p, i, j - 1);'],
      ['',              '        bool matchChars = isMatch(s, p, i - 1, j);'],
      ['c_ret_or',      '        return matchEmpty || matchChars;'],
      ['',              '    }'],
      ['c_ret_false',   '    return false;'],
      ['',              '}'],
      ['',              ''],
      ['',              'int main() {'],
      ['m_read_s',      '    string s; cin >> s;'],
      ['m_read_p',      '    string p; cin >> p;'],
      ['m_call_match',  '    bool ans = isMatch(s, p, s.size(), p.size());'],
      ['m_print',       '    cout << (ans ? "true" : "false") << endl;'],
      ['m_done',        '    return 0;'],
      ['',              '}']
    ],
    python: [
      ['',              'import sys'],
      ['',              ''],
      ['c_entry',       'def is_match(s, p, i, j):'],
      ['c_base_both',   '    if i == 0 and j == 0: return True'],
      ['c_base_p_empty','    if j == 0: return False'],
      ['c_base_s_empty','    if i == 0: return p[j - 1] == "*" and is_match(s, p, 0, j - 1)'],
      ['c_char_chk',    '    if p[j - 1] == s[i - 1] or p[j - 1] == "?":'],
      ['c_char_rec',    '        return is_match(s, p, i - 1, j - 1)'],
      ['c_star_chk',    '    elif p[j - 1] == "*":'],
      ['c_star_rec',    '        match_empty = is_match(s, p, i, j - 1)'],
      ['',              '        match_chars = is_match(s, p, i - 1, j)'],
      ['c_ret_or',      '        return match_empty or match_chars'],
      ['c_ret_false',   '    return False'],
      ['',              ''],
      ['',              'if __name__ == "__main__":'],
      ['',              '    tokens = sys.stdin.read().split()'],
      ['m_read_s',      '    s = tokens[0]'],
      ['m_read_p',      '    p = tokens[1]'],
      ['m_call_match',  '    ans = is_match(s, p, len(s), len(p))'],
      ['m_print',       '    print(str(ans).lower())'],
      ['m_done',        '    sys.exit(0)']
    ],
    javascript: [
      ['',              'const fs = require("fs");'],
      ['',              ''],
      ['c_entry',       'function isMatch(s, p, i, j) {'],
      ['c_base_both',   '    if (i === 0 && j === 0) return true;'],
      ['c_base_p_empty','    if (j === 0) return false;'],
      ['c_base_s_empty','    if (i === 0) return p[j - 1] === \'*\' && isMatch(s, p, 0, j - 1);'],
      ['c_char_chk',    '    if (p[j - 1] === s[i - 1] || p[j - 1] === \'?\') {'],
      ['c_char_rec',    '        return isMatch(s, p, i - 1, j - 1);'],
      ['c_star_chk',    '    } else if (p[j - 1] === \'*\') {'],
      ['c_star_rec',    '        const matchEmpty = isMatch(s, p, i, j - 1);'],
      ['',              '        const matchChars = isMatch(s, p, i - 1, j);'],
      ['c_ret_or',      '        return matchEmpty || matchChars;'],
      ['',              '    }'],
      ['c_ret_false',   '    return false;'],
      ['',              '}'],
      ['',              ''],
      ['',              'const tokens = fs.readFileSync(0, "utf-8").trim().split(/\\s+/);'],
      ['m_read_s',      'const s = tokens[0];'],
      ['m_read_p',      'const p = tokens[1];'],
      ['m_call_match',  'const ans = isMatch(s, p, s.length, p.length);'],
      ['m_print',       'console.log(ans);'],
      ['m_done',        'process.exit(0);']
    ]
  },
  memoization: {
    java: [
      ['',              'import java.util.Scanner;'],
      ['',              'import java.util.Arrays;'],
      ['',              ''],
      ['',              'public class Main {'],
      ['c_entry',       '    static boolean solve(String s, String p, int i, int j, int[][] memo) {'],
      ['c_base_both',   '        if (i == 0 && j == 0) {'],
      ['',              '            return true;'],
      ['',              '        }'],
      ['c_base_p_empty','        if (j == 0) {'],
      ['',              '            return false;'],
      ['',              '        }'],
      ['c_base_s_empty','        if (i == 0) {'],
      ['',              '            return p.charAt(j - 1) == \'*\' && solve(s, p, 0, j - 1, memo);'],
      ['',              '        }'],
      ['c_memo_check',  '        if (memo[i][j] != -1) {'],
      ['',              '            return memo[i][j] == 1;'],
      ['',              '        }'],
      ['c_char_chk',    '        boolean res = false;'],
      ['',              '        if (p.charAt(j - 1) == s.charAt(i - 1) || p.charAt(j - 1) == \'?\') {'],
      ['c_char_calc',   '            res = solve(s, p, i - 1, j - 1, memo);'],
      ['c_star_chk',    '        } else if (p.charAt(j - 1) == \'*\') {'],
      ['c_star_calc',   '            res = solve(s, p, i, j - 1, memo) || solve(s, p, i - 1, j, memo);'],
      ['',              '        }'],
      ['c_ret_memo',    '        memo[i][j] = res ? 1 : 0;'],
      ['',              '        return res;'],
      ['',              '    }'],
      ['',              ''],
      ['',              '    public static void main(String[] args) {'],
      ['',              '        Scanner sc = new Scanner(System.in);'],
      ['m_read_s',      '        String s = sc.next();'],
      ['m_read_p',      '        String p = sc.next();'],
      ['m_alloc_memo',  '        int m = s.length(), n = p.length();'],
      ['',              '        int[][] memo = new int[m + 1][n + 1];'],
      ['m_fill_memo',   '        for (int[] row : memo) {'],
      ['',              '            Arrays.fill(row, -1);'],
      ['',              '        }'],
      ['m_call_match',  '        boolean ans = solve(s, p, m, n, memo);'],
      ['m_print',       '        System.out.println(ans);'],
      ['m_done',        '    }'],
      ['',              '}']
    ],
    c: [
      ['',              '#include <stdio.h>'],
      ['',              '#include <string.h>'],
      ['',              '#include <stdbool.h>'],
      ['',              ''],
      ['c_entry',       'bool solve(const char* s, const char* p, int i, int j, int memo[100][100]) {'],
      ['c_base_both',   '    if (i == 0 && j == 0) {'],
      ['',              '        return true;'],
      ['',              '    }'],
      ['c_base_p_empty','    if (j == 0) {'],
      ['',              '        return false;'],
      ['',              '    }'],
      ['c_base_s_empty','    if (i == 0) {'],
      ['',              '        return p[j - 1] == \'*\' && solve(s, p, 0, j - 1, memo);'],
      ['',              '    }'],
      ['c_memo_check',  '    if (memo[i][j] != -1) {'],
      ['',              '        return memo[i][j] == 1;'],
      ['',              '    }'],
      ['c_char_chk',    '    bool res = false;'],
      ['',              '    if (p[j - 1] == s[i - 1] || p[j - 1] == \'?\') {'],
      ['c_char_calc',   '        res = solve(s, p, i - 1, j - 1, memo);'],
      ['c_star_chk',    '    } else if (p[j - 1] == \'*\') {'],
      ['c_star_calc',   '        res = solve(s, p, i, j - 1, memo) || solve(s, p, i - 1, j, memo);'],
      ['',              '    }'],
      ['c_ret_memo',    '    memo[i][j] = res ? 1 : 0;'],
      ['',              '    return res;'],
      ['',              '}'],
      ['',              ''],
      ['',              'int main() {'],
      ['m_read_s',      '    char s[ 100 ] = ""; scanf("%s", s);'],
      ['m_read_p',      '    char p[ 100 ] = ""; scanf("%s", p);'],
      ['m_alloc_memo',  '    int memo[100][100]; memset(memo, -1, sizeof(memo));'],
      ['m_call_match',  '    bool ans = solve(s, p, strlen(s), strlen(p), memo);'],
      ['m_print',       '    printf("%s\\n", ans ? "true" : "false");'],
      ['m_done',        '    return 0;'],
      ['',              '}']
    ],
    cpp: [
      ['',              '#include <iostream>'],
      ['',              '#include <vector>'],
      ['',              '#include <string>'],
      ['',              'using namespace std;'],
      ['',              ''],
      ['c_entry',       'bool solve(const string& s, const string& p, int i, int j, vector<vector<int>>& memo) {'],
      ['c_base_both',   '    if (i == 0 && j == 0) {'],
      ['',              '        return true;'],
      ['',              '    }'],
      ['c_base_p_empty','    if (j == 0) {'],
      ['',              '        return false;'],
      ['',              '    }'],
      ['c_base_s_empty','    if (i == 0) {'],
      ['',              '        return p[j - 1] == \'*\' && solve(s, p, 0, j - 1, memo);'],
      ['',              '    }'],
      ['c_memo_check',  '    if (memo[i][j] != -1) {'],
      ['',              '        return memo[i][j] == 1;'],
      ['',              '    }'],
      ['c_char_chk',    '    bool res = false;'],
      ['',              '    if (p[j - 1] == s[i - 1] || p[j - 1] == \'?\') {'],
      ['c_char_calc',   '        res = solve(s, p, i - 1, j - 1, memo);'],
      ['c_star_chk',    '    } else if (p[j - 1] == \'*\') {'],
      ['c_star_calc',   '        res = solve(s, p, i, j - 1, memo) || solve(s, p, i - 1, j, memo);'],
      ['',              '    }'],
      ['c_ret_memo',    '    memo[i][j] = res ? 1 : 0;'],
      ['',              '    return res;'],
      ['',              '}'],
      ['',              ''],
      ['',              'int main() {'],
      ['m_read_s',      '    string s; cin >> s;'],
      ['m_read_p',      '    string p; cin >> p;'],
      ['m_alloc_memo',  '    vector<vector<int>> memo(s.size() + 1, vector<int>(p.size() + 1, -1));'],
      ['m_call_match',  '    bool ans = solve(s, p, s.size(), p.size(), memo);'],
      ['m_print',       '    cout << (ans ? "true" : "false") << endl;'],
      ['m_done',        '    return 0;'],
      ['',              '}']
    ],
    python: [
      ['',              'import sys'],
      ['',              ''],
      ['c_entry',       'def solve(s, p, i, j, memo):'],
      ['c_base_both',   '    if i == 0 and j == 0: return True'],
      ['c_base_p_empty','    if j == 0: return False'],
      ['c_base_s_empty','    if i == 0: return p[j - 1] == "*" and solve(s, p, 0, j - 1, memo)'],
      ['c_memo_check',  '    if memo[i][j] != -1: return memo[i][j]'],
      ['c_char_chk',    '    res = False'],
      ['',              '    if p[j - 1] == s[i - 1] or p[j - 1] == "?":'],
      ['c_char_calc',   '        res = solve(s, p, i - 1, j - 1, memo)'],
      ['c_star_chk',    '    elif p[j - 1] == "*":'],
      ['c_star_calc',   '        res = solve(s, p, i, j - 1, memo) or solve(s, p, i - 1, j, memo)'],
      ['c_ret_memo',    '    memo[i][j] = res'],
      ['',              '    return res'],
      ['',              ''],
      ['',              'if __name__ == "__main__":'],
      ['',              '    tokens = sys.stdin.read().split()'],
      ['m_read_s',      '    s = tokens[0]'],
      ['m_read_p',      '    p = tokens[1]'],
      ['m_alloc_memo',  '    memo = [[-1] * (len(p) + 1) for _ in range(len(s) + 1)]'],
      ['m_call_match',  '    ans = solve(s, p, len(s), len(p), memo)'],
      ['m_print',       '    print(str(ans).lower())'],
      ['m_done',        '    sys.exit(0)']
    ],
    javascript: [
      ['',              'const fs = require("fs");'],
      ['',              ''],
      ['c_entry',       'function solve(s, p, i, j, memo) {'],
      ['c_base_both',   '    if (i === 0 && j === 0) return true;'],
      ['c_base_p_empty','    if (j === 0) return false;'],
      ['c_base_s_empty','    if (i === 0) return p[j - 1] === \'*\' && solve(s, p, 0, j - 1, memo);'],
      ['c_memo_check',  '    if (memo[i][j] !== -1) return memo[i][j];'],
      ['c_char_chk',    '    let res = false;'],
      ['',              '    if (p[j - 1] === s[i - 1] || p[j - 1] === \'?\') {'],
      ['c_char_calc',   '        res = solve(s, p, i - 1, j - 1, memo);'],
      ['c_star_chk',    '    } else if (p[j - 1] === \'*\') {'],
      ['c_star_calc',   '        res = solve(s, p, i, j - 1, memo) || solve(s, p, i - 1, j, memo);'],
      ['',              '    }'],
      ['c_ret_memo',    '    memo[i][j] = res;'],
      ['',              '    return res;'],
      ['',              '}'],
      ['',              ''],
      ['',              'const tokens = fs.readFileSync(0, "utf-8").trim().split(/\\s+/);'],
      ['m_read_s',      'const s = tokens[0];'],
      ['m_read_p',      'const p = tokens[1];'],
      ['m_alloc_memo',  'const memo = Array.from({ length: s.length + 1 }, () => new Array(p.length + 1).fill(-1));'],
      ['m_call_match',  'const ans = solve(s, p, s.length, p.length, memo);'],
      ['m_print',       'console.log(ans);'],
      ['m_done',        'process.exit(0);']
    ]
  },
  tabulation: {
    java: [
      ['',              'import java.util.Scanner;'],
      ['',              ''],
      ['',              'public class Main {'],
      ['c_entry',       '    static boolean isMatch(String s, String p) {'],
      ['c_init_dims',   '        int m = s.length(), n = p.length();'],
      ['c_alloc_dp',    '        boolean[][] dp = new boolean[m + 1][n + 1];'],
      ['c_base_empty',  '        dp[0][0] = true; // Empty string matches empty pattern'],
      ['c_base_stars',  '        for (int j = 1; j <= n; j++) {'],
      ['',              '            if (p.charAt(j - 1) == \'*\') {'],
      ['',              '                dp[0][j] = dp[0][j - 1];'],
      ['',              '            }'],
      ['',              '        }'],
      ['c_loop_i',      '        for (int i = 1; i <= m; i++) {'],
      ['c_loop_j',      '            for (int j = 1; j <= n; j++) {'],
      ['c_match_chk',   '                if (p.charAt(j - 1) == s.charAt(i - 1) || p.charAt(j - 1) == \'?\') {'],
      ['c_dp_match',    '                    dp[i][j] = dp[i - 1][j - 1];'],
      ['c_star_chk',    '                } else if (p.charAt(j - 1) == \'*\') {'],
      ['c_dp_star',     '                    dp[i][j] = dp[i][j - 1] || dp[i - 1][j];'],
      ['',              '                }'],
      ['',              '            }'],
      ['',              '        }'],
      ['c_ret_dp',      '        return dp[m][n];'],
      ['',              '    }'],
      ['',              ''],
      ['',              '    public static void main(String[] args) {'],
      ['',              '        Scanner sc = new Scanner(System.in);'],
      ['m_read_s',      '        String s = sc.next();'],
      ['m_read_p',      '        String p = sc.next();'],
      ['m_call_match',  '        boolean ans = isMatch(s, p);'],
      ['m_print',       '        System.out.println(ans);'],
      ['m_done',        '    }'],
      ['',              '}']
    ],
    c: [
      ['',              '#include <stdio.h>'],
      ['',              '#include <string.h>'],
      ['',              '#include <stdbool.h>'],
      ['',              ''],
      ['c_entry',       'bool isMatch(const char* s, const char* p) {'],
      ['c_init_dims',   '    int m = strlen(s), n = strlen(p);'],
      ['c_alloc_dp',    '    bool dp[m + 1][n + 1]; memset(dp, 0, sizeof(dp));'],
      ['c_base_empty',  '    dp[0][0] = true;'],
      ['c_base_stars',  '    for (int j = 1; j <= n; j++) {'],
      ['',              '        if (p[j - 1] == \'*\') {'],
      ['',              '            dp[0][j] = dp[0][j - 1];'],
      ['',              '        }'],
      ['',              '    }'],
      ['c_loop_i',      '    for (int i = 1; i <= m; i++) {'],
      ['c_loop_j',      '        for (int j = 1; j <= n; j++) {'],
      ['c_match_chk',   '            if (p[j - 1] == s[i - 1] || p[j - 1] == \'?\') {'],
      ['c_dp_match',    '                dp[i][j] = dp[i - 1][j - 1];'],
      ['c_star_chk',    '            } else if (p[j - 1] == \'*\') {'],
      ['c_dp_star',     '                dp[i][j] = dp[i][j - 1] || dp[i - 1][j];'],
      ['',              '            }'],
      ['',              '        }'],
      ['',              '    }'],
      ['c_ret_dp',      '    return dp[m][n];'],
      ['',              '}'],
      ['',              ''],
      ['',              'int main() {'],
      ['m_read_s',      '    char s[ 100 ] = ""; scanf("%s", s);'],
      ['m_read_p',      '    char p[ 100 ] = ""; scanf("%s", p);'],
      ['m_call_match',  '    bool ans = isMatch(s, p);'],
      ['m_print',       '    printf("%s\\n", ans ? "true" : "false");'],
      ['m_done',        '    return 0;'],
      ['',              '}']
    ],
    cpp: [
      ['',              '#include <iostream>'],
      ['',              '#include <vector>'],
      ['',              '#include <string>'],
      ['',              'using namespace std;'],
      ['',              ''],
      ['c_entry',       'bool isMatch(const string& s, const string& p) {'],
      ['c_init_dims',   '    int m = s.size(), n = p.size();'],
      ['c_alloc_dp',    '    vector<vector<bool>> dp(m + 1, vector<bool>(n + 1, false));'],
      ['c_base_empty',  '    dp[0][0] = true;'],
      ['c_base_stars',  '    for (int j = 1; j <= n; j++) {'],
      ['',              '        if (p[j - 1] == \'*\') {'],
      ['',              '            dp[0][j] = dp[0][j - 1];'],
      ['',              '        }'],
      ['',              '    }'],
      ['c_loop_i',      '    for (int i = 1; i <= m; i++) {'],
      ['c_loop_j',      '        for (int j = 1; j <= n; j++) {'],
      ['c_match_chk',   '            if (p[j - 1] == s[i - 1] || p[j - 1] == \'?\') {'],
      ['c_dp_match',    '                dp[i][j] = dp[i - 1][j - 1];'],
      ['c_star_chk',    '            } else if (p[j - 1] == \'*\') {'],
      ['c_dp_star',     '                dp[i][j] = dp[i][j - 1] || dp[i - 1][j];'],
      ['',              '            }'],
      ['',              '        }'],
      ['',              '    }'],
      ['c_ret_dp',      '    return dp[m][n];'],
      ['',              '}'],
      ['',              ''],
      ['',              'int main() {'],
      ['m_read_s',      '    string s; cin >> s;'],
      ['m_read_p',      '    string p; cin >> p;'],
      ['m_call_match',  '    bool ans = isMatch(s, p);'],
      ['m_print',       '    cout << (ans ? "true" : "false") << endl;'],
      ['m_done',        '    return 0;'],
      ['',              '}']
    ],
    python: [
      ['',              'import sys'],
      ['',              ''],
      ['c_entry',       'def is_match(s, p):'],
      ['c_init_dims',   '    m, n = len(s), len(p)'],
      ['c_alloc_dp',    '    dp = [[False] * (n + 1) for _ in range(m + 1)]'],
      ['c_base_empty',  '    dp[0][0] = True'],
      ['c_base_stars',  '    for j in range(1, n + 1):'],
      ['',              '        if p[j - 1] == "*": dp[0][j] = dp[0][j - 1]'],
      ['c_loop_i',      '    for i in range(1, m + 1):'],
      ['c_loop_j',      '        for j in range(1, n + 1):'],
      ['c_match_chk',   '            if p[j - 1] == s[i - 1] or p[j - 1] == "?":'],
      ['c_dp_match',    '                dp[i][j] = dp[i - 1][j - 1]'],
      ['c_star_chk',    '            elif p[j - 1] == "*":'],
      ['c_dp_star',     '                dp[i][j] = dp[i][j - 1] or dp[i - 1][j]'],
      ['c_ret_dp',      '    return dp[m][n]'],
      ['',              ''],
      ['',              'if __name__ == "__main__":'],
      ['',              '    tokens = sys.stdin.read().split()'],
      ['m_read_s',      '    s = tokens[0]'],
      ['m_read_p',      '    p = tokens[1]'],
      ['m_call_match',  '    ans = is_match(s, p)'],
      ['m_print',       '    print(str(ans).lower())'],
      ['m_done',        '    sys.exit(0)']
    ],
    javascript: [
      ['',              'const fs = require("fs");'],
      ['',              ''],
      ['c_entry',       'function isMatch(s, p) {'],
      ['c_init_dims',   '    const m = s.length, n = p.length;'],
      ['c_alloc_dp',    '    const dp = Array.from({ length: m + 1 }, () => new Array(n + 1).fill(false));'],
      ['c_base_empty',  '    dp[0][0] = true;'],
      ['c_base_stars',  '    for (let j = 1; j <= n; j++) {'],
      ['',              '        if (p[j - 1] === \'*\' && dp[0][j - 1]) dp[0][j] = true;'],
      ['',              '    }'],
      ['c_loop_i',      '    for (let i = 1; i <= m; i++) {'],
      ['c_loop_j',      '        for (let j = 1; j <= n; j++) {'],
      ['c_match_chk',   '            if (p[j - 1] === s[i - 1] || p[j - 1] === \'?\') {'],
      ['c_dp_match',    '                dp[i][j] = dp[i - 1][j - 1];'],
      ['c_star_chk',    '            } else if (p[j - 1] === \'*\') {'],
      ['c_dp_star',     '                dp[i][j] = dp[i][j - 1] || dp[i - 1][j];'],
      ['',              '            }'],
      ['',              '        }'],
      ['',              '    }'],
      ['c_ret_dp',      '    return dp[m][n];'],
      ['',              '}'],
      ['',              ''],
      ['',              'const tokens = fs.readFileSync(0, "utf-8").trim().split(/\\s+/);'],
      ['m_read_s',      'const s = tokens[0];'],
      ['m_read_p',      'const p = tokens[1];'],
      ['m_call_match',  'const ans = isMatch(s, p);'],
      ['m_print',       'console.log(ans);'],
      ['m_done',        'process.exit(0);']
    ]
  }
};

const PSEUDOCODES = {
  recursion: [
    'function isMatch(s, p, i, j):',
    '  if i == 0 and j == 0: return true             // Both empty',
    '  if j == 0: return false                       // Non-empty string, empty pattern',
    '  if i == 0: return p[j - 1] == \'*\' and isMatch(s, p, 0, j-1)',
    '  if p[j - 1] == s[i - 1] or p[j - 1] == \'?\':',
    '    return isMatch(s, p, i-1, j-1)              // Consume 1 character from both',
    '  else if p[j - 1] == \'*\':',
    '    return isMatch(s, p, i, j-1)                // Branch 1: \'*\' matches 0 characters',
    '        or isMatch(s, p, i-1, j)                // Branch 2: \'*\' matches 1+ characters',
    '  return false',
    '',
    'Time Complexity:  O(2^(m+n)) Worst Case Exponential',
    'Space Complexity: O(m+n) Call Stack'
  ],
  memoization: [
    'memo = 2D array of size (m+1) x (n+1), filled with -1',
    '',
    'function solve(s, p, i, j, memo):',
    '  if i == 0 and j == 0: return true',
    '  if j == 0: return false',
    '  if i == 0: return p[j - 1] == \'*\' and solve(s, p, 0, j-1, memo)',
    '  if memo[i][j] != -1: return memo[i][j]        // O(1) Cache Hit',
    '  if p[j - 1] == s[i - 1] or p[j - 1] == \'?\':',
    '    res = solve(s, p, i-1, j-1, memo)',
    '  else if p[j - 1] == \'*\':',
    '    res = solve(s, p, i, j-1, memo) or solve(s, p, i-1, j, memo)',
    '  else:',
    '    res = false',
    '  memo[i][j] = res',
    '  return res',
    '',
    'Time Complexity:  O(m * n) Polynomial Time',
    'Space Complexity: O(m * n) 2D Cache + O(m+n) Call Stack'
  ],
  tabulation: [
    'function isMatch(s, p):',
    '  dp = 2D boolean array of size (m+1) x (n+1), default false',
    '  dp[0][0] = true                               // Empty string matches empty pattern',
    '  for j = 1 to n:',
    '    if p[j - 1] == \'*\': dp[0][j] = dp[0][j - 1]',
    '  for i = 1 to m:',
    '    for j = 1 to n:',
    '      if p[j - 1] == s[i - 1] or p[j - 1] == \'?\':',
    '        dp[i][j] = dp[i - 1][j - 1]                 // Diagonal Copy',
    '      else if p[j - 1] == \'*\':',
    '        dp[i][j] = dp[i][j - 1] or dp[i - 1][j]     // Left (empty) OR Top (consume)',
    '  return dp[m][n]',
    '',
    'Time Complexity:  O(m * n) Systematic Grid Fill',
    'Space Complexity: O(m * n) Table'
  ]
};

/* ==================================================================== */
/* STATIC TREE GENERATOR FOR DECISION TREE (WILDCARD RECURSION / MEMO)  */
/* ==================================================================== */
function generateStaticTree(approach, s, p) {
  const m = s.length;
  const n = p.length;
  const nodes = [];
  const edges = [];
  let nextId = 0;

  if (approach === 'recursion') {
    function build(i, j, parentId, branchType) {
      const id = nextId++;
      const node = { id, i, j, parentId, branchType, isRedundant: false, children: [] };
      nodes.push(node);

      if (parentId !== null) {
        edges.push({ from: parentId, to: id, branchType });
      }

      if (i === 0 && j === 0) {
        // base
      } else if (j === 0) {
        // base
      } else if (i === 0) {
        if (p[j - 1] === '*') {
          const cId = build(0, j - 1, id, 'star-empty');
          node.children = [cId];
        }
      } else if (p[j - 1] === s[i - 1] || p[j - 1] === '?') {
        const cId = build(i - 1, j - 1, id, 'match');
        node.children = [cId];
      } else if (p[j - 1] === '*') {
        const leftId = build(i, j - 1, id, 'star-empty');
        const topId = build(i - 1, j, id, 'star-consume');
        node.children = [leftId, topId];
      }
      return id;
    }
    if (m >= 0 && n >= 0) build(m, n, null, 'root');
  } else if (approach === 'memoization') {
    const memo = Array.from({ length: m + 1 }, () => new Array(n + 1).fill(false));
    function buildMemo(i, j, parentId, branchType) {
      const id = nextId++;
      const isCacheHit = i >= 0 && j >= 0 && memo[i][j];
      const node = { id, i, j, parentId, branchType, isCacheHit, children: [] };
      nodes.push(node);

      if (parentId !== null) {
        edges.push({ from: parentId, to: id, branchType });
      }

      if (i === 0 && j === 0) {
        // base
      } else if (j === 0) {
        // base
      } else if (isCacheHit) {
        // pruned
      } else {
        memo[i][j] = true;
        if (i === 0) {
          if (p[j - 1] === '*') {
            const cId = buildMemo(0, j - 1, id, 'star-empty');
            node.children = [cId];
          }
        } else if (p[j - 1] === s[i - 1] || p[j - 1] === '?') {
          const cId = buildMemo(i - 1, j - 1, id, 'match');
          node.children = [cId];
        } else if (p[j - 1] === '*') {
          const leftId = buildMemo(i, j - 1, id, 'star-empty');
          const topId = buildMemo(i - 1, j, id, 'star-consume');
          node.children = [leftId, topId];
        }
      }
      return id;
    }
    if (m >= 0 && n >= 0) buildMemo(m, n, null, 'root');
  }

  // Compute Layout Positions (x, y)
  const nodeMap = new Map();
  nodes.forEach(nd => nodeMap.set(nd.id, { ...nd }));

  let leafCounter = 0;
  function assignSubtreeMetrics(nId, depth = 0) {
    const nd = nodeMap.get(nId);
    if (!nd) return;
    nd.depth = depth;
    if (!nd.children || !nd.children.length) {
      nd.leafIndex = leafCounter++;
    } else {
      nd.children.forEach(cId => assignSubtreeMetrics(cId, depth + 1));
    }
  }
  if (nodes.length > 0) {
    assignSubtreeMetrics(0, 0);
  }

  const totalLeaves = Math.max(1, leafCounter);
  const maxDepth = Math.max(0, ...Array.from(nodeMap.values()).map(nd => nd.depth || 0));

  const leafSpacing = 64;
  const levelHeight = 44;

  const computedWidth = Math.max(180, totalLeaves * leafSpacing + 36);
  const computedHeight = Math.max(70, maxDepth * levelHeight + 44);

  function assignCoords(nId) {
    const nd = nodeMap.get(nId);
    if (!nd) return 0;
    if (!nd.children || !nd.children.length) {
      nd.x = 18 + nd.leafIndex * leafSpacing + leafSpacing / 2;
    } else {
      const childXs = nd.children.map(cId => assignCoords(cId));
      nd.x = (childXs[0] + childXs[childXs.length - 1]) / 2;
    }
    nd.y = 24 + nd.depth * levelHeight;
    return nd.x;
  }
  if (nodes.length > 0) {
    assignCoords(0);
  }

  const layoutMap = {};
  nodeMap.forEach((nd, id) => {
    layoutMap[id] = { x: nd.x, y: nd.y, depth: nd.depth };
  });

  return { nodes, edges, layoutMap, width: computedWidth, height: computedHeight };
}

/* ==================================================================== */
/* STRICT ZERO-SKIP LINE-BY-LINE EXECUTION STEP GENERATOR               */
/* ==================================================================== */
function buildSteps(approach, rawS, rawP) {
  const steps = [];
  const maxLimit = approach === 'tabulation' ? 8 : 5;
  const s = (rawS || '').slice(0, maxLimit);
  const p = (rawP || '*').slice(0, maxLimit);
  const m = s.length;
  const n = p.length;

  /* ------------------------------------------------------------------ */
  /* APPROACH 1: BRUTE FORCE RECURSION                                   */
  /* ------------------------------------------------------------------ */
  if (approach === 'recursion') {
    const staticTree = generateStaticTree('recursion', s, p);
    const nodeStateMap = {};
    staticTree.nodes.forEach(nd => {
      nodeStateMap[nd.id] = {
        ...nd,
        x: staticTree.layoutMap[nd.id].x,
        y: staticTree.layoutMap[nd.id].y,
        state: 'hidden',
        retVal: null
      };
    });

    const stackFrames = [];
    let nextNodeId = 0;
    let callCounter = 0;
    let redundantCounter = 0;
    const visitedSubproblems = new Set();

    function getVisibleNodes() {
      return Object.values(nodeStateMap)
        .filter(nd => nd.state !== 'hidden')
        .map(nd => ({ ...nd }));
    }
    function getVisibleEdges() {
      return staticTree.edges.filter(e => {
        const fromNd = nodeStateMap[e.from];
        const toNd = nodeStateMap[e.to];
        return fromNd && toNd && fromNd.state !== 'hidden' && toNd.state !== 'hidden';
      }).map(e => ({
        ...e,
        x1: staticTree.layoutMap[e.from].x,
        y1: staticTree.layoutMap[e.from].y + 12,
        x2: staticTree.layoutMap[e.to].x,
        y2: staticTree.layoutMap[e.to].y - 12
      }));
    }

    steps.push({
      code: 'm_read_s',
      badge: `Input String s = "${s}" (length m=${m})`,
      badgeType: 'info',
      state: {
        s, p, m, n,
        totalCalls: 0,
        redundantCalls: 0,
        treeNodes: [],
        treeEdges: [],
        treeWidth: staticTree.width,
        treeHeight: staticTree.height,
        activeNodeId: null,
        currentReturn: null,
        frames: []
      }
    });

    steps.push({
      code: 'm_read_p',
      badge: `Input Pattern p = "${p}" (length n=${n})`,
      badgeType: 'info',
      state: {
        s, p, m, n,
        totalCalls: 0,
        redundantCalls: 0,
        treeNodes: [],
        treeEdges: [],
        treeWidth: staticTree.width,
        treeHeight: staticTree.height,
        activeNodeId: null,
        currentReturn: null,
        frames: []
      }
    });

    steps.push({
      code: 'm_call_match',
      badge: `Invoking root recursive call isMatch("${s}", "${p}", i=${m}, j=${n})`,
      badgeType: 'info',
      state: {
        s, p, m, n,
        totalCalls: 0,
        redundantCalls: 0,
        treeNodes: [],
        treeEdges: [],
        treeWidth: staticTree.width,
        treeHeight: staticTree.height,
        activeNodeId: null,
        currentReturn: null,
        frames: [{ name: 'main()', args: `s="${s}", p="${p}"` }]
      }
    });

    function solveRec(i, j) {
      const myId = nextNodeId++;
      callCounter++;
      const subKey = `${i},${j}`;
      const isRedundant = visitedSubproblems.has(subKey);
      if (isRedundant) redundantCounter++;
      visitedSubproblems.add(subKey);

      if (nodeStateMap[myId]) {
        nodeStateMap[myId].state = 'active';
        nodeStateMap[myId].isRedundant = isRedundant;
      }

      stackFrames.push({ name: `isMatch(i=${i}, j=${j})`, args: `s[${i - 1}], p[${j - 1}]` });

      steps.push({
        code: 'c_entry',
        badge: `Entering isMatch(i=${i}, j=${j}) &mdash; examining prefixes s[0...${i-1}] and p[0...${j-1}]`,
        badgeType: isRedundant ? 'warn' : 'info',
        state: {
          s, p, m, n, curI: i, curJ: j,
          charS: i > 0 ? s[i - 1] : '',
          charP: j > 0 ? p[j - 1] : '',
          totalCalls: callCounter,
          redundantCalls: redundantCounter,
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height,
          activeNodeId: myId,
          currentReturn: null,
          frames: [...stackFrames]
        }
      });

      // Base Case 1: Both empty
      steps.push({
        code: 'c_base_both',
        badge: (i === 0 && j === 0)
          ? `Base Case: (i=0 && j=0) &rarr; Empty string matches empty pattern &rarr; Return True`
          : `Base Case Check: (i=${i}, j=${j}) != (0, 0) &rarr; Proceeding`,
        badgeType: (i === 0 && j === 0) ? 'success' : 'info',
        state: {
          s, p, m, n, curI: i, curJ: j,
          charS: i > 0 ? s[i - 1] : '',
          charP: j > 0 ? p[j - 1] : '',
          totalCalls: callCounter,
          redundantCalls: redundantCounter,
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height,
          activeNodeId: myId,
          currentReturn: (i === 0 && j === 0) ? true : null,
          frames: [...stackFrames]
        }
      });

      if (i === 0 && j === 0) {
        if (nodeStateMap[myId]) {
          nodeStateMap[myId].state = 'solved';
          nodeStateMap[myId].retVal = 'T';
        }
        stackFrames.pop();
        return true;
      }

      // Base Case 2: Pattern empty, string non-empty
      steps.push({
        code: 'c_base_p_empty',
        badge: j === 0
          ? `Base Case: j=0 but i=${i} > 0 &rarr; Non-empty string cannot match empty pattern &rarr; Return False`
          : `Base Case Check: Pattern not empty (j=${j} != 0) &rarr; Proceeding`,
        badgeType: j === 0 ? 'error' : 'info',
        state: {
          s, p, m, n, curI: i, curJ: j,
          charS: i > 0 ? s[i - 1] : '',
          charP: j > 0 ? p[j - 1] : '',
          totalCalls: callCounter,
          redundantCalls: redundantCounter,
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height,
          activeNodeId: myId,
          currentReturn: j === 0 ? false : null,
          frames: [...stackFrames]
        }
      });

      if (j === 0) {
        if (nodeStateMap[myId]) {
          nodeStateMap[myId].state = 'solved';
          nodeStateMap[myId].retVal = 'F';
        }
        stackFrames.pop();
        return false;
      }

      // Base Case 3: String empty, pattern non-empty
      const isStringEmpty = (i === 0);
      steps.push({
        code: 'c_base_s_empty',
        badge: isStringEmpty
          ? (p[j - 1] === '*'
            ? `String empty (i=0): p[${j - 1}] == '*' &rarr; Recursing isMatch(0, ${j - 1})`
            : `String empty (i=0) but p[${j - 1}]='${p[j - 1]}' != '*' &rarr; Cannot match &rarr; Return False`)
          : `Base Case Check: String not empty (i=${i} > 0) &rarr; Proceeding`,
        badgeType: isStringEmpty ? (p[j - 1] === '*' ? 'info' : 'error') : 'info',
        state: {
          s, p, m, n, curI: i, curJ: j,
          charS: '',
          charP: p[j - 1],
          totalCalls: callCounter,
          redundantCalls: redundantCounter,
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height,
          activeNodeId: myId,
          currentReturn: isStringEmpty ? (p[j - 1] === '*' ? null : false) : null,
          frames: [...stackFrames]
        }
      });

      if (isStringEmpty) {
        const star = p[j - 1] === '*';
        if (!star) {
          if (nodeStateMap[myId]) {
            nodeStateMap[myId].state = 'solved';
            nodeStateMap[myId].retVal = 'F';
          }
          stackFrames.pop();
          return false;
        }

        const sub = solveRec(0, j - 1);
        if (nodeStateMap[myId]) {
          nodeStateMap[myId].state = 'solved';
          nodeStateMap[myId].retVal = sub ? 'T' : 'F';
        }
        stackFrames.pop();
        return sub;
      }

      // Non-empty string & pattern
      const pChar = p[j - 1];
      const sChar = s[i - 1];
      const match = (pChar === sChar || pChar === '?');

      steps.push({
        code: 'c_char_chk',
        badge: match
          ? `Direct Match or '?': s[${i - 1}]='${sChar}' matches p[${j - 1}]='${pChar}' &rarr; Shrink both`
          : `Not a direct char match: s[${i - 1}]='${sChar}', p[${j - 1}]='${pChar}' &rarr; Check for '*'`,
        badgeType: match ? 'success' : 'info',
        state: {
          s, p, m, n, curI: i, curJ: j,
          charS: sChar, charP: pChar,
          isMatch: match,
          totalCalls: callCounter,
          redundantCalls: redundantCounter,
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height,
          activeNodeId: myId,
          currentReturn: null,
          frames: [...stackFrames]
        }
      });

      if (match) {
        steps.push({
          code: 'c_char_rec',
          badge: `Recursing on consumed characters &rarr; isMatch(i=${i - 1}, j=${j - 1})`,
          badgeType: 'info',
          state: {
            s, p, m, n, curI: i, curJ: j,
            charS: sChar, charP: pChar,
            isMatch: true,
            totalCalls: callCounter,
            redundantCalls: redundantCounter,
            treeNodes: getVisibleNodes(),
            treeEdges: getVisibleEdges(),
            treeWidth: staticTree.width,
            treeHeight: staticTree.height,
            activeNodeId: myId,
            currentReturn: null,
            frames: [...stackFrames]
          }
        });

        const res = solveRec(i - 1, j - 1);
        if (nodeStateMap[myId]) {
          nodeStateMap[myId].state = 'solved';
          nodeStateMap[myId].retVal = res ? 'T' : 'F';
        }
        stackFrames.pop();
        return res;
      }

      if (pChar === '*') {
        steps.push({
          code: 'c_star_chk',
          badge: `STAR WILDCARD (*): Branch 1 &rarr; Match empty string (i=${i}, j=${j - 1})`,
          badgeType: 'info',
          state: {
            s, p, m, n, curI: i, curJ: j,
            charS: sChar, charP: pChar,
            isStar: true,
            totalCalls: callCounter,
            redundantCalls: redundantCounter,
            treeNodes: getVisibleNodes(),
            treeEdges: getVisibleEdges(),
            treeWidth: staticTree.width,
            treeHeight: staticTree.height,
            activeNodeId: myId,
            currentReturn: null,
            frames: [...stackFrames]
          }
        });

        const emptyRes = solveRec(i, j - 1);

        steps.push({
          code: 'c_star_rec',
          badge: `STAR WILDCARD (*): Branch 2 &rarr; Consume 1 character of s (i=${i - 1}, j=${j})`,
          badgeType: 'info',
          state: {
            s, p, m, n, curI: i, curJ: j,
            charS: sChar, charP: pChar,
            isStar: true,
            totalCalls: callCounter,
            redundantCalls: redundantCounter,
            treeNodes: getVisibleNodes(),
            treeEdges: getVisibleEdges(),
            treeWidth: staticTree.width,
            treeHeight: staticTree.height,
            activeNodeId: myId,
            currentReturn: null,
            frames: [...stackFrames]
          }
        });

        const consumeRes = solveRec(i - 1, j);
        const starRes = emptyRes || consumeRes;

        steps.push({
          code: 'c_ret_or',
          badge: `Star resolution for (i=${i}, j=${j}): empty(${emptyRes}) || consume(${consumeRes}) &rarr; ${starRes}`,
          badgeType: starRes ? 'success' : 'error',
          state: {
            s, p, m, n, curI: i, curJ: j,
            charS: sChar, charP: pChar,
            isStar: true,
            totalCalls: callCounter,
            redundantCalls: redundantCounter,
            treeNodes: getVisibleNodes(),
            treeEdges: getVisibleEdges(),
            treeWidth: staticTree.width,
            treeHeight: staticTree.height,
            activeNodeId: myId,
            currentReturn: starRes,
            frames: [...stackFrames]
          }
        });

        if (nodeStateMap[myId]) {
          nodeStateMap[myId].state = 'solved';
          nodeStateMap[myId].retVal = starRes ? 'T' : 'F';
        }
        stackFrames.pop();
        return starRes;
      }

      // Hard mismatch
      steps.push({
        code: 'c_ret_false',
        badge: `Mismatch: s[${i - 1}]='${sChar}' cannot match pattern p[${j - 1}]='${pChar}' &rarr; Return False`,
        badgeType: 'error',
        state: {
          s, p, m, n, curI: i, curJ: j,
          charS: sChar, charP: pChar,
          totalCalls: callCounter,
          redundantCalls: redundantCounter,
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height,
          activeNodeId: myId,
          currentReturn: false,
          frames: [...stackFrames]
        }
      });

      if (nodeStateMap[myId]) {
        nodeStateMap[myId].state = 'solved';
        nodeStateMap[myId].retVal = 'F';
      }
      stackFrames.pop();
      return false;
    }

    const finalAns = solveRec(m, n);

    steps.push({
      code: 'm_print',
      badge: `Recursive Execution Finished! Result: ${finalAns ? 'MATCHED (True)' : 'NOT MATCHED (False)'} (Calls: ${callCounter}, Redundant: ${redundantCounter})`,
      badgeType: finalAns ? 'success' : 'error',
      state: {
        s, p, m, n,
        totalCalls: callCounter,
        redundantCalls: redundantCounter,
        treeNodes: getVisibleNodes(),
        treeEdges: getVisibleEdges(),
        treeWidth: staticTree.width,
        treeHeight: staticTree.height,
        activeNodeId: null,
        currentReturn: finalAns,
        frames: []
      }
    });

    steps.push({
      code: 'm_done',
      badge: `Program execution completed. Output: ${finalAns}`,
      badgeType: 'success',
      state: {
        s, p, m, n,
        totalCalls: callCounter,
        redundantCalls: redundantCounter,
        treeNodes: getVisibleNodes(),
        treeEdges: getVisibleEdges(),
        treeWidth: staticTree.width,
        treeHeight: staticTree.height,
        activeNodeId: null,
        currentReturn: finalAns,
        frames: []
      }
    });

    return steps;
  }

  /* ------------------------------------------------------------------ */
  /* APPROACH 2: DP MEMOIZATION                                         */
  /* ------------------------------------------------------------------ */
  if (approach === 'memoization') {
    const staticTree = generateStaticTree('memoization', s, p);
    const nodeStateMap = {};
    staticTree.nodes.forEach(nd => {
      nodeStateMap[nd.id] = {
        ...nd,
        x: staticTree.layoutMap[nd.id].x,
        y: staticTree.layoutMap[nd.id].y,
        state: 'hidden',
        retVal: null
      };
    });

    const memoGrid = Array.from({ length: m + 1 }, () => new Array(n + 1).fill(-1));
    const stackFrames = [];
    let nextNodeId = 0;
    let callCounter = 0;
    let hitCounter = 0;

    function getVisibleNodes() {
      return Object.values(nodeStateMap)
        .filter(nd => nd.state !== 'hidden')
        .map(nd => ({ ...nd }));
    }
    function getVisibleEdges() {
      return staticTree.edges.filter(e => {
        const fromNd = nodeStateMap[e.from];
        const toNd = nodeStateMap[e.to];
        return fromNd && toNd && fromNd.state !== 'hidden' && toNd.state !== 'hidden';
      }).map(e => ({
        ...e,
        x1: staticTree.layoutMap[e.from].x,
        y1: staticTree.layoutMap[e.from].y + 12,
        x2: staticTree.layoutMap[e.to].x,
        y2: staticTree.layoutMap[e.to].y - 12
      }));
    }

    steps.push({
      code: 'm_read_s',
      badge: `Input String s = "${s}" (length m=${m})`,
      badgeType: 'info',
      state: {
        s, p, m, n,
        totalCalls: 0,
        cacheHits: 0,
        memo: memoGrid.map(r => [...r]),
        treeNodes: [],
        treeEdges: [],
        treeWidth: staticTree.width,
        treeHeight: staticTree.height,
        activeNodeId: null,
        currentReturn: null,
        frames: []
      }
    });

    steps.push({
      code: 'm_read_p',
      badge: `Input Pattern p = "${p}" (length n=${n})`,
      badgeType: 'info',
      state: {
        s, p, m, n,
        totalCalls: 0,
        cacheHits: 0,
        memo: memoGrid.map(r => [...r]),
        treeNodes: [],
        treeEdges: [],
        treeWidth: staticTree.width,
        treeHeight: staticTree.height,
        activeNodeId: null,
        currentReturn: null,
        frames: []
      }
    });

    steps.push({
      code: 'm_alloc_memo',
      badge: `Allocating 2D memo cache table of size (${m + 1}) \u00d7 (${n + 1}) initialized with -1`,
      badgeType: 'info',
      state: {
        s, p, m, n,
        totalCalls: 0,
        cacheHits: 0,
        memo: memoGrid.map(r => [...r]),
        treeNodes: [],
        treeEdges: [],
        treeWidth: staticTree.width,
        treeHeight: staticTree.height,
        activeNodeId: null,
        currentReturn: null,
        frames: [{ name: 'main()', args: `s="${s}", p="${p}"` }]
      }
    });

    steps.push({
      code: 'm_fill_memo',
      badge: `Initializing memo table cells to -1 (indicating uncomputed subproblems)`,
      badgeType: 'info',
      state: {
        s, p, m, n,
        totalCalls: 0,
        cacheHits: 0,
        memo: memoGrid.map(r => [...r]),
        treeNodes: [],
        treeEdges: [],
        treeWidth: staticTree.width,
        treeHeight: staticTree.height,
        activeNodeId: null,
        currentReturn: null,
        frames: [{ name: 'main()', args: `s="${s}", p="${p}"` }]
      }
    });

    steps.push({
      code: 'm_call_match',
      badge: `Calling solve("${s}", "${p}", m=${m}, n=${n}, memo) in main()`,
      badgeType: 'info',
      state: {
        s, p, m, n,
        totalCalls: 0,
        cacheHits: 0,
        memo: memoGrid.map(r => [...r]),
        treeNodes: [],
        treeEdges: [],
        treeWidth: staticTree.width,
        treeHeight: staticTree.height,
        activeNodeId: null,
        currentReturn: null,
        frames: [{ name: 'main()', args: `s="${s}", p="${p}"` }]
      }
    });

    function solveMem(i, j) {
      const myId = nextNodeId++;
      callCounter++;
      const isHit = i >= 0 && j >= 0 && memoGrid[i][j] !== -1;
      if (isHit) hitCounter++;

      if (nodeStateMap[myId]) {
        nodeStateMap[myId].state = isHit ? 'cache-hit' : 'active';
      }
      stackFrames.push({ name: `solve(i=${i}, j=${j})`, args: `memo[${i}][${j}]` });

      steps.push({
        code: 'c_entry',
        badge: `Entering solve(i=${i}, j=${j}) &mdash; checking state`,
        badgeType: isHit ? 'success' : 'info',
        state: {
          s, p, m, n, curI: i, curJ: j,
          charS: i > 0 ? s[i - 1] : '',
          charP: j > 0 ? p[j - 1] : '',
          totalCalls: callCounter,
          cacheHits: hitCounter,
          memo: memoGrid.map(r => [...r]),
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height,
          activeNodeId: myId,
          currentReturn: null,
          frames: [...stackFrames]
        }
      });

      // Base Case 1: Both empty
      steps.push({
        code: 'c_base_both',
        badge: (i === 0 && j === 0)
          ? `Base Case: (i=0 && j=0) &rarr; Empty string matches empty pattern &rarr; Return True`
          : `Base Case Check: (i=${i}, j=${j}) != (0, 0) &rarr; Proceeding`,
        badgeType: (i === 0 && j === 0) ? 'success' : 'info',
        state: {
          s, p, m, n, curI: i, curJ: j,
          charS: i > 0 ? s[i - 1] : '',
          charP: j > 0 ? p[j - 1] : '',
          totalCalls: callCounter,
          cacheHits: hitCounter,
          memo: memoGrid.map(r => [...r]),
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height,
          activeNodeId: myId,
          currentReturn: (i === 0 && j === 0) ? true : null,
          frames: [...stackFrames]
        }
      });

      if (i === 0 && j === 0) {
        if (nodeStateMap[myId]) {
          nodeStateMap[myId].state = 'solved';
          nodeStateMap[myId].retVal = 'T';
        }
        stackFrames.pop();
        return true;
      }

      // Base Case 2: Pattern empty, string non-empty
      steps.push({
        code: 'c_base_p_empty',
        badge: j === 0
          ? `Base Case: j=0 but i=${i} > 0 &rarr; Non-empty string cannot match empty pattern &rarr; Return False`
          : `Base Case Check: Pattern not empty (j=${j} != 0) &rarr; Proceeding`,
        badgeType: j === 0 ? 'error' : 'info',
        state: {
          s, p, m, n, curI: i, curJ: j,
          charS: i > 0 ? s[i - 1] : '',
          charP: j > 0 ? p[j - 1] : '',
          totalCalls: callCounter,
          cacheHits: hitCounter,
          memo: memoGrid.map(r => [...r]),
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height,
          activeNodeId: myId,
          currentReturn: j === 0 ? false : null,
          frames: [...stackFrames]
        }
      });

      if (j === 0) {
        if (nodeStateMap[myId]) {
          nodeStateMap[myId].state = 'solved';
          nodeStateMap[myId].retVal = 'F';
        }
        stackFrames.pop();
        return false;
      }

      // Base Case 3: String empty, pattern non-empty
      const isMemStringEmpty = (i === 0);
      steps.push({
        code: 'c_base_s_empty',
        badge: isMemStringEmpty
          ? (p[j - 1] === '*'
            ? `String empty (i=0): p[${j - 1}] == '*' &rarr; Recursing solve(0, ${j - 1})`
            : `String empty (i=0) but p[${j - 1}]='${p[j - 1]}' != '*' &rarr; Cannot match &rarr; Return False`)
          : `Base Case Check: String not empty (i=${i} > 0) &rarr; Proceeding`,
        badgeType: isMemStringEmpty ? (p[j - 1] === '*' ? 'info' : 'error') : 'info',
        state: {
          s, p, m, n, curI: i, curJ: j,
          charS: '',
          charP: p[j - 1],
          totalCalls: callCounter,
          cacheHits: hitCounter,
          memo: memoGrid.map(r => [...r]),
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height,
          activeNodeId: myId,
          currentReturn: isMemStringEmpty ? (p[j - 1] === '*' ? null : false) : null,
          frames: [...stackFrames]
        }
      });

      if (isMemStringEmpty) {
        const star = p[j - 1] === '*';
        if (!star) {
          memoGrid[0][j] = 0;
          if (nodeStateMap[myId]) {
            nodeStateMap[myId].state = 'solved';
            nodeStateMap[myId].retVal = 'F';
          }
          stackFrames.pop();
          return false;
        }
        const sub = solveMem(0, j - 1);
        memoGrid[0][j] = sub ? 1 : 0;
        if (nodeStateMap[myId]) {
          nodeStateMap[myId].state = 'solved';
          nodeStateMap[myId].retVal = sub ? 'T' : 'F';
        }
        stackFrames.pop();
        return sub;
      }

      // Check Cache
      steps.push({
        code: 'c_memo_check',
        badge: memoGrid[i][j] !== -1
          ? `O(1) CACHE HIT! memo[${i}][${j}] = ${memoGrid[i][j] === 1 ? 'True' : 'False'} &rarr; Pruning whole subtree!`
          : `Cache Miss: memo[${i}][${j}] == -1 &rarr; Evaluating subproblem logic`,
        badgeType: memoGrid[i][j] !== -1 ? 'success' : 'info',
        state: {
          s, p, m, n, curI: i, curJ: j,
          charS: i > 0 ? s[i - 1] : '',
          charP: j > 0 ? p[j - 1] : '',
          totalCalls: callCounter,
          cacheHits: hitCounter,
          memo: memoGrid.map(r => [...r]),
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height,
          activeNodeId: myId,
          currentReturn: memoGrid[i][j] !== -1 ? (memoGrid[i][j] === 1) : null,
          frames: [...stackFrames]
        }
      });

      if (memoGrid[i][j] !== -1) {
        const val = memoGrid[i][j] === 1;
        if (nodeStateMap[myId]) {
          nodeStateMap[myId].state = 'cache-hit';
          nodeStateMap[myId].retVal = val ? 'T' : 'F';
        }
        stackFrames.pop();
        return val;
      }

      const pChar = p[j - 1];
      const sChar = s[i - 1];
      const match = (pChar === sChar || pChar === '?');

      steps.push({
        code: 'c_char_chk',
        badge: match
          ? `Direct Match or '?': s[${i - 1}]='${sChar}' matches p[${j - 1}]='${pChar}' &rarr; Shrink both`
          : `Not a direct char match: s[${i - 1}]='${sChar}', p[${j - 1}]='${pChar}' &rarr; Check for '*'`,
        badgeType: match ? 'success' : 'info',
        state: {
          s, p, m, n, curI: i, curJ: j,
          charS: sChar, charP: pChar,
          isMatch: match,
          totalCalls: callCounter,
          cacheHits: hitCounter,
          memo: memoGrid.map(r => [...r]),
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height,
          activeNodeId: myId,
          currentReturn: null,
          frames: [...stackFrames]
        }
      });

      let res = false;
      if (match) {
        steps.push({
          code: 'c_char_calc',
          badge: `Character match / '?' &rarr; Solving inner subproblem solve(${i - 1}, ${j - 1})`,
          badgeType: 'info',
          state: {
            s, p, m, n, curI: i, curJ: j,
            charS: sChar, charP: pChar,
            isMatch: true,
            totalCalls: callCounter,
            cacheHits: hitCounter,
            memo: memoGrid.map(r => [...r]),
            treeNodes: getVisibleNodes(),
            treeEdges: getVisibleEdges(),
            treeWidth: staticTree.width,
            treeHeight: staticTree.height,
            activeNodeId: myId,
            currentReturn: null,
            frames: [...stackFrames]
          }
        });

        res = solveMem(i - 1, j - 1);
      } else {
        const isStar = (pChar === '*');
        steps.push({
          code: 'c_star_chk',
          badge: isStar
            ? `STAR WILDCARD (*): p[${j - 1}] == '*' &rarr; Branching empty and consume`
            : `Mismatch: p[${j - 1}]='${pChar}' != '*' &rarr; Return False`,
          badgeType: isStar ? 'info' : 'error',
          state: {
            s, p, m, n, curI: i, curJ: j,
            charS: sChar, charP: pChar,
            isStar,
            totalCalls: callCounter,
            cacheHits: hitCounter,
            memo: memoGrid.map(r => [...r]),
            treeNodes: getVisibleNodes(),
            treeEdges: getVisibleEdges(),
            treeWidth: staticTree.width,
            treeHeight: staticTree.height,
            activeNodeId: myId,
            currentReturn: isStar ? null : false,
            frames: [...stackFrames]
          }
        });

        if (isStar) {
          steps.push({
            code: 'c_star_calc',
            badge: `Star Wildcard (*) &rarr; Solving branches solve(${i}, ${j - 1}) || solve(${i - 1}, ${j})`,
            badgeType: 'info',
            state: {
              s, p, m, n, curI: i, curJ: j,
              charS: sChar, charP: pChar,
              isStar: true,
              totalCalls: callCounter,
              cacheHits: hitCounter,
              memo: memoGrid.map(r => [...r]),
              treeNodes: getVisibleNodes(),
              treeEdges: getVisibleEdges(),
              treeWidth: staticTree.width,
              treeHeight: staticTree.height,
              activeNodeId: myId,
              currentReturn: null,
              frames: [...stackFrames]
            }
          });

          const branch1 = solveMem(i, j - 1);
          const branch2 = solveMem(i - 1, j);
          res = branch1 || branch2;
        } else {
          res = false;
        }
      }

      memoGrid[i][j] = res ? 1 : 0;

      steps.push({
        code: 'c_ret_memo',
        badge: `Storing memo[${i}][${j}] = ${res ? 'True' : 'False'} and returning`,
        badgeType: res ? 'success' : 'error',
        state: {
          s, p, m, n, curI: i, curJ: j,
          charS: sChar, charP: pChar,
          totalCalls: callCounter,
          cacheHits: hitCounter,
          memo: memoGrid.map(r => [...r]),
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height,
          activeNodeId: myId,
          currentReturn: res,
          frames: [...stackFrames]
        }
      });

      if (nodeStateMap[myId]) {
        nodeStateMap[myId].state = 'solved';
        nodeStateMap[myId].retVal = res ? 'T' : 'F';
      }
      stackFrames.pop();
      return res;
    }

    const finalAns = solveMem(m, n);

    steps.push({
      code: 'm_print',
      badge: `Memoization Finished! Match Result = ${finalAns ? 'MATCHED (True)' : 'NOT MATCHED (False)'} (Calls: ${callCounter}, Cache Hits: ${hitCounter})`,
      badgeType: finalAns ? 'success' : 'error',
      state: {
        s, p, m, n,
        totalCalls: callCounter,
        cacheHits: hitCounter,
        memo: memoGrid.map(r => [...r]),
        treeNodes: getVisibleNodes(),
        treeEdges: getVisibleEdges(),
        treeWidth: staticTree.width,
        treeHeight: staticTree.height,
        activeNodeId: null,
        currentReturn: finalAns,
        frames: []
      }
    });

    steps.push({
      code: 'm_done',
      badge: `Program execution completed. Output: ${finalAns}`,
      badgeType: 'success',
      state: {
        s, p, m, n,
        totalCalls: callCounter,
        cacheHits: hitCounter,
        memo: memoGrid.map(r => [...r]),
        treeNodes: getVisibleNodes(),
        treeEdges: getVisibleEdges(),
        treeWidth: staticTree.width,
        treeHeight: staticTree.height,
        activeNodeId: null,
        currentReturn: finalAns,
        frames: []
      }
    });

    return steps;
  }

  /* ------------------------------------------------------------------ */
  /* APPROACH 3: DP TABULATION (2D Grid Matrix + Backtrack)              */
  /* ------------------------------------------------------------------ */
  if (approach === 'tabulation') {
    const dpGrid = Array.from({ length: m + 1 }, () => new Array(n + 1).fill('?'));
    let opCounter = 0;

    steps.push({
      code: 'm_read_s',
      badge: `Input String s = "${s}" (length m=${m})`,
      badgeType: 'info',
      state: {
        s, p, m, n,
        iterations: 0,
        totalOps: 0,
        i: null, j: null,
        dpGrid: dpGrid.map(r => [...r]),
        tracebackCells: [],
        currentReturn: null
      }
    });

    steps.push({
      code: 'm_read_p',
      badge: `Input Pattern p = "${p}" (length n=${n})`,
      badgeType: 'info',
      state: {
        s, p, m, n,
        iterations: 0,
        totalOps: 0,
        i: null, j: null,
        dpGrid: dpGrid.map(r => [...r]),
        tracebackCells: [],
        currentReturn: null
      }
    });

    steps.push({
      code: 'm_call_match',
      badge: `Calling isMatch("${s}", "${p}") in main()`,
      badgeType: 'info',
      state: {
        s, p, m, n,
        iterations: 0,
        totalOps: 0,
        i: null, j: null,
        dpGrid: dpGrid.map(r => [...r]),
        tracebackCells: [],
        currentReturn: null
      }
    });

    steps.push({
      code: 'c_entry',
      badge: `Entering isMatch(s="${s}", p="${p}")`,
      badgeType: 'info',
      state: {
        s, p, m, n,
        iterations: 0,
        totalOps: 0,
        i: null, j: null,
        dpGrid: dpGrid.map(r => [...r]),
        tracebackCells: [],
        currentReturn: null
      }
    });

    steps.push({
      code: 'c_init_dims',
      badge: `Initializing dimensions: m = s.length() = ${m}, n = p.length() = ${n}`,
      badgeType: 'info',
      state: {
        s, p, m, n,
        iterations: 0,
        totalOps: 0,
        i: null, j: null,
        dpGrid: dpGrid.map(r => [...r]),
        tracebackCells: [],
        currentReturn: null
      }
    });

    steps.push({
      code: 'c_alloc_dp',
      badge: `Allocating 2D Boolean Table dp[${m + 1}][${n + 1}] initialized with false`,
      badgeType: 'info',
      state: {
        s, p, m, n,
        iterations: 0,
        totalOps: opCounter,
        i: null, j: null,
        dpGrid: dpGrid.map(r => [...r]),
        tracebackCells: [],
        currentReturn: null
      }
    });

    // Base Case 1: dp[0][0] = true
    dpGrid[0][0] = true;
    for (let r = 1; r <= m; r++) dpGrid[r][0] = false;

    steps.push({
      code: 'c_base_empty',
      badge: `Base Case: dp[0][0] = True (empty string matches empty pattern). dp[i][0] = False for all i > 0`,
      badgeType: 'success',
      state: {
        s, p, m, n,
        iterations: 0,
        totalOps: ++opCounter,
        i: 0, j: 0,
        dpGrid: dpGrid.map(r => [...r]),
        tracebackCells: [],
        currentReturn: null
      }
    });

    // Base Case 2: Row 0 star prefixes
    for (let c = 1; c <= n; c++) {
      dpGrid[0][c] = (p[c - 1] === '*' && dpGrid[0][c - 1]);
      opCounter++;
    }

    steps.push({
      code: 'c_base_stars',
      badge: `Base Case Row 0: Evaluating leading stars in pattern. dp[0][j] = dp[0][j - 1] if p[j - 1]=='*' else False`,
      badgeType: 'info',
      state: {
        s, p, m, n,
        iterations: 0,
        totalOps: opCounter,
        i: 0, j: n,
        dpGrid: dpGrid.map(r => [...r]),
        tracebackCells: [],
        currentReturn: null
      }
    });

    for (let i = 1; i <= m; i++) {
      steps.push({
        code: 'c_loop_i',
        badge: `Outer Loop: Iterating row i = ${i} (character s[${i - 1}] = '${s[i - 1]}')`,
        badgeType: 'info',
        state: {
          s, p, m, n,
          iterations: Math.max(0, (i - 1) * n),
          totalOps: opCounter,
          i, j: null,
          charS: s[i - 1],
          dpGrid: dpGrid.map(r => [...r]),
          tracebackCells: [],
          currentReturn: null
        }
      });

      for (let j = 1; j <= n; j++) {
        opCounter++;
        const currentIter = (i - 1) * n + j;
        const pChar = p[j - 1];
        const sChar = s[i - 1];
        const match = (pChar === sChar || pChar === '?');

        steps.push({
          code: 'c_loop_j',
          badge: `Inner Loop (Iteration #${currentIter}/${m * n}): Cell dp[${i}][${j}] (s[${i - 1}]='${sChar}' vs p[${j - 1}]='${pChar}')`,
          badgeType: 'info',
          state: {
            s, p, m, n,
            iterations: currentIter,
            totalOps: opCounter,
            i, j,
            charS: sChar,
            charP: pChar,
            isMatch: match,
            dpGrid: dpGrid.map(r => [...r]),
            tracebackCells: [],
            currentReturn: null
          }
        });

        // Strictly step through condition checking line-by-line
        steps.push({
          code: 'c_match_chk',
          badge: match
            ? `Checking Condition: p[${j - 1}] ('${pChar}') == s[${i - 1}] ('${sChar}') or '?' &rarr; TRUE`
            : `Checking Condition: p[${j - 1}] ('${pChar}') == s[${i - 1}] ('${sChar}') or '?' &rarr; FALSE`,
          badgeType: match ? 'success' : 'info',
          state: {
            s, p, m, n,
            iterations: currentIter,
            totalOps: opCounter,
            i, j,
            charS: sChar,
            charP: pChar,
            isMatch: match,
            dpGrid: dpGrid.map(r => [...r]),
            tracebackCells: [],
            currentReturn: null
          }
        });

        if (match) {
          const diagVal = dpGrid[i - 1][j - 1];
          dpGrid[i][j] = diagVal;

          steps.push({
            code: 'c_dp_match',
            badge: `MATCH DETECTED: s[${i - 1}] == p[${j - 1}] ('${pChar}') &rarr; dp[${i}][${j}] = dp[${i - 1}][${j - 1}] = ${diagVal ? 'True' : 'False'}`,
            badgeType: diagVal ? 'success' : 'warn',
            state: {
              s, p, m, n,
              iterations: currentIter,
              totalOps: opCounter,
              i, j,
              charS: sChar,
              charP: pChar,
              isMatch: true,
              dpGrid: dpGrid.map(r => [...r]),
              tracebackCells: [],
              currentReturn: diagVal
            }
          });
        } else {
          const isStar = (pChar === '*');
          steps.push({
            code: 'c_star_chk',
            badge: isStar
              ? `Checking else if: p[${j - 1}] == '*' &rarr; TRUE (Star Wildcard)`
              : `Checking else if: p[${j - 1}] ('${pChar}') == '*' &rarr; FALSE (Mismatch)`,
            badgeType: isStar ? 'success' : 'info',
            state: {
              s, p, m, n,
              iterations: currentIter,
              totalOps: opCounter,
              i, j,
              charS: sChar,
              charP: pChar,
              isStar,
              dpGrid: dpGrid.map(r => [...r]),
              tracebackCells: [],
              currentReturn: null
            }
          });

          if (isStar) {
            const leftVal = dpGrid[i][j - 1];  // Star matches empty
            const topVal = dpGrid[i - 1][j];   // Star consumes s[i - 1]
            const res = (leftVal || topVal);
            dpGrid[i][j] = res;

            steps.push({
              code: 'c_dp_star',
              badge: `STAR (*) WILDCARD: dp[${i}][${j}] = Left: dp[${i}][${j-1}](${leftVal}) || Top: dp[${i-1}][${j}](${topVal}) &rarr; ${res ? 'True' : 'False'}`,
              badgeType: res ? 'success' : 'warn',
              state: {
                s, p, m, n,
                iterations: currentIter,
                totalOps: opCounter,
                i, j,
                charS: sChar,
                charP: pChar,
                isStar: true,
                dpGrid: dpGrid.map(r => [...r]),
                tracebackCells: [],
                currentReturn: res
              }
            });
          } else {
            dpGrid[i][j] = false;
          }
        }
      }
    }

    const totalIterations = m * n;
    const finalAns = dpGrid[m][n] === true;

    // Reconstruction (Backtracking) of Match Path if True
    const pathCells = [];
    if (finalAns) {
      let r = m, c = n;
      while (r > 0 || c > 0) {
        pathCells.push({ i: r, j: c });
        if (r > 0 && c > 0 && (p[c - 1] === s[r - 1] || p[c - 1] === '?') && dpGrid[r - 1][c - 1]) {
          r--;
          c--;
        } else if (c > 0 && p[c - 1] === '*' && dpGrid[r][c - 1]) {
          c--;
        } else if (r > 0 && c > 0 && p[c - 1] === '*' && dpGrid[r - 1][c]) {
          r--;
        } else if (r === 0 && c > 0 && p[c - 1] === '*' && dpGrid[0][c - 1]) {
          c--;
        } else {
          break;
        }
      }
      pathCells.push({ i: 0, j: 0 });
    }

    steps.push({
      code: 'c_ret_dp',
      badge: `Table Complete! Returning dp[${m}][${n}] = ${finalAns ? 'True (MATCHED)' : 'False (NOT MATCHED)'}`,
      badgeType: finalAns ? 'success' : 'error',
      state: {
        s, p, m, n,
        iterations: totalIterations,
        totalOps: opCounter,
        i: m, j: n,
        dpGrid: dpGrid.map(r => [...r]),
        tracebackCells: pathCells,
        currentReturn: finalAns
      }
    });

    steps.push({
      code: 'm_print',
      badge: `Tabulation Complete (${totalIterations} iterations)! Wildcard Matching Result = ${finalAns ? 'MATCHED (True)' : 'NOT MATCHED (False)'}`,
      badgeType: finalAns ? 'success' : 'error',
      state: {
        s, p, m, n,
        iterations: totalIterations,
        totalOps: opCounter,
        i: null, j: null,
        dpGrid: dpGrid.map(r => [...r]),
        tracebackCells: pathCells,
        currentReturn: finalAns
      }
    });

    steps.push({
      code: 'm_done',
      badge: `Program execution completed. Output: ${finalAns}`,
      badgeType: 'success',
      state: {
        s, p, m, n,
        iterations: totalIterations,
        totalOps: opCounter,
        i: null, j: null,
        dpGrid: dpGrid.map(r => [...r]),
        tracebackCells: pathCells,
        currentReturn: finalAns
      }
    });

    return steps;
  }

  return steps;
}

/* ==================================================================== */
/* REACTIVE STATE & INTERACTIVE CONTROLS                                */
/* ==================================================================== */
const currentApproach = ref('tabulation');
const rightTab = ref('code');
const lang = ref('java');
const speed = ref(600);
const sInputStr = ref('adceb');
const pInputStr = ref('*a*b');
const currentS = ref('adceb');
const currentP = ref('*a*b');

const maxAllowedChars = computed(() => {
  return currentApproach.value === 'tabulation' ? 8 : 5;
});

// Warning Modal State
const showWarningModal = ref(false);
const warningModalTitle = ref('Input Limit Exceeded');
const warningModalMsg = ref('');
const requestedS = ref('');
const requestedP = ref('');
const warningModalLimit = ref(8);

const currentStepIdx = ref(0);
const playing = ref(false);
let playTimer = null;

// Presets Dropdown
const showPresetsDropdown = ref(false);
const presetDropdownRef = ref(null);

function selectPreset(pr) {
  sInputStr.value = pr.s;
  pInputStr.value = pr.p;
  applyInput();
  showPresetsDropdown.value = false;
}

function handleDocClick(e) {
  if (presetDropdownRef.value && !presetDropdownRef.value.contains(e.target)) {
    showPresetsDropdown.value = false;
  }
}

// Resizable workspace panes
const leftWidth = ref(54);
const vizHeight = ref(280);
const mainRef = ref(null);
const resizerRef = ref(null);
const vizResizerRef = ref(null);

const steps = computed(() => {
  return buildSteps(currentApproach.value, currentS.value, currentP.value);
});

const currentStep = computed(() => {
  if (!steps.value.length) return { code: '', badge: '', badgeType: 'info', state: {} };
  const idx = Math.min(currentStepIdx.value, steps.value.length - 1);
  return steps.value[idx];
});

const s = computed(() => currentStep.value);
const st = computed(() => currentStep.value.state || {});

const codeLines = computed(() => {
  const byApp = CODES[currentApproach.value] || CODES.tabulation;
  return byApp[lang.value] || byApp.java;
});

const pseudocodeLines = computed(() => {
  return PSEUDOCODES[currentApproach.value] || PSEUDOCODES.tabulation;
});

function applyInput() {
  let strS = (sInputStr.value !== undefined) ? sInputStr.value.trim() : '';
  let strP = (pInputStr.value !== undefined) ? pInputStr.value.trim() : '*';

  const limit = maxAllowedChars.value;
  let wasClamped = false;

  requestedS.value = strS;
  requestedP.value = strP;
  warningModalLimit.value = limit;

  if (strS.length > limit) {
    strS = strS.slice(0, limit);
    sInputStr.value = strS;
    wasClamped = true;
  }
  if (strP.length > limit) {
    strP = strP.slice(0, limit);
    pInputStr.value = strP;
    wasClamped = true;
  }

  if (wasClamped) {
    warningModalMsg.value = `Input lengths were adjusted to the maximum allowed limit of ${limit} characters for ${
      currentApproach.value === 'tabulation' ? '2D Tabulation Grid' : 'Recursion / Memoization Tree'
    } to ensure optimal rendering and smooth performance.`;
    showWarningModal.value = true;
  }

  currentS.value = strS;
  currentP.value = strP;
  currentStepIdx.value = 0;
  pause();
}

function applyApproach(appId) {
  currentApproach.value = appId;
  applyInput();
}

function closeWarningModal() {
  showWarningModal.value = false;
}

/* Playback */
function togglePlay() {
  if (playing.value) pause();
  else play();
}

function play() {
  if (currentStepIdx.value >= steps.value.length - 1) {
    currentStepIdx.value = 0;
  }
  playing.value = true;
  scheduleNext();
}

function pause() {
  playing.value = false;
  if (playTimer) clearTimeout(playTimer);
}

function scheduleNext() {
  if (!playing.value) return;
  playTimer = setTimeout(() => {
    if (currentStepIdx.value < steps.value.length - 1) {
      currentStepIdx.value++;
      scheduleNext();
    } else {
      pause();
    }
  }, speed.value);
}

function stepBy(delta) {
  pause();
  const next = currentStepIdx.value + delta;
  currentStepIdx.value = Math.max(0, Math.min(next, steps.value.length - 1));
}

function seekTo(idx) {
  pause();
  currentStepIdx.value = Math.max(0, Math.min(idx, steps.value.length - 1));
}

/* Horizontal Resizer */
function initHorizontalResize(e) {
  e.preventDefault();
  const startX = e.clientX;
  const startWidth = leftWidth.value;
  const containerWidth = mainRef.value ? mainRef.value.offsetWidth : 900;

  function onMouseMove(me) {
    const dx = me.clientX - startX;
    const newPercent = startWidth + (dx / containerWidth) * 100;
    leftWidth.value = Math.max(30, Math.min(75, newPercent));
  }
  function onMouseUp() {
    window.removeEventListener('mousemove', onMouseMove);
    window.removeEventListener('mouseup', onMouseUp);
  }
  window.addEventListener('mousemove', onMouseMove);
  window.addEventListener('mouseup', onMouseUp);
}

/* Vertical Resizer */
function initVerticalResize(e) {
  e.preventDefault();
  const startY = e.clientY;
  const startHeight = vizHeight.value;

  function onMouseMove(me) {
    const dy = me.clientY - startY;
    vizHeight.value = Math.max(160, Math.min(480, startHeight + dy));
  }
  function onMouseUp() {
    window.removeEventListener('mousemove', onMouseMove);
    window.removeEventListener('mouseup', onMouseUp);
  }
  window.addEventListener('mousemove', onMouseMove);
  window.addEventListener('mouseup', onMouseUp);
}

function isTracebackCell(r, c) {
  if (!st.value.tracebackCells || !st.value.tracebackCells.length) return false;
  return st.value.tracebackCells.some(cell => cell.i === r && cell.j === c);
}

onMounted(() => {
  document.addEventListener('click', handleDocClick);
  if (resizerRef.value) {
    resizerRef.value.addEventListener('mousedown', initHorizontalResize);
  }
  if (vizResizerRef.value) {
    vizResizerRef.value.addEventListener('mousedown', initVerticalResize);
  }
});

onUnmounted(() => {
  document.removeEventListener('click', handleDocClick);
  pause();
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

            <!-- Input for String s -->
            <div class="ll-input-group">
              <label>s :</label>
              <input
                type="text"
                v-model="sInputStr"
                class="ll-text-input"
                placeholder="adceb"
                @keyup.enter="applyInput"
              />
            </div>

            <!-- Input for Pattern p -->
            <div class="ll-input-group">
              <label>p :</label>
              <input
                type="text"
                v-model="pInputStr"
                class="ll-text-input"
                placeholder="*a*b"
                @keyup.enter="applyInput"
              />
            </div>

            <!-- Presets Dropdown Button -->
            <div class="ll-dropdown-wrap" ref="presetDropdownRef">
              <button
                class="ll-dropdown-btn"
                :class="{ active: showPresetsDropdown }"
                @click.stop="showPresetsDropdown = !showPresetsDropdown"
                title="Choose a preset pair"
              >
                &#9662;
              </button>
              <div v-if="showPresetsDropdown" class="ll-dropdown-menu">
                <button
                  v-for="(p, pi) in PRESETS"
                  :key="pi"
                  class="ll-dropdown-item"
                  @click="selectPreset(p)"
                >
                  <span class="ll-drop-label">{{ p.label }}</span>
                  <span class="ll-drop-desc" v-if="p.desc">{{ p.desc }}</span>
                </button>
              </div>
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

          <!-- Main Workspace Split: Left Visuals & Right Code/Theory -->
          <div class="ll-main" ref="mainRef">
            <!-- Left Column: Dynamic Visualizations -->
            <div class="ll-left-col" :style="{ width: leftWidth + '%' }">
              <!-- Top Strings Banner -->
              <div class="ll-house-banner">
                <div class="ll-house-banner-title">Input String &amp; Wildcard Pattern Trackers:</div>
                <div class="ll-strings-track">
                  <div class="ll-str-line">
                    <span class="ll-str-tag">s:</span>
                    <span v-if="!currentS" class="ll-empty-tag">&empty; (empty)</span>
                    <div
                      v-for="(ch, idx) in currentS"
                      :key="'s-' + idx"
                      class="ll-char-card"
                      :class="{
                        'll-char-active': st.curI === idx + 1 || st.i === idx + 1,
                        'll-char-match': (st.curI === idx + 1 || st.i === idx + 1) && st.isMatch
                      }"
                    >
                      <span class="ll-char-letter">{{ ch }}</span>
                      <span class="ll-char-sub">i={{ idx + 1 }}</span>
                    </div>
                  </div>
                  <div class="ll-str-line">
                    <span class="ll-str-tag">p:</span>
                    <div
                      v-for="(ch, jdx) in currentP"
                      :key="'p-' + jdx"
                      class="ll-char-card"
                      :class="{
                        'll-char-active': st.curJ === jdx + 1 || st.j === jdx + 1,
                        'll-char-match': (st.curJ === jdx + 1 || st.j === jdx + 1) && st.isMatch,
                        'll-char-wildcard': ch === '*' || ch === '?'
                      }"
                    >
                      <span class="ll-char-letter">{{ ch }}</span>
                      <span class="ll-char-sub">j={{ jdx + 1 }}</span>
                    </div>
                  </div>
                </div>
              </div>

              <!-- Visual Workspace Panel -->
              <div class="ll-viz-wrap" :style="{ height: vizHeight + 'px' }">
                <div class="ll-perm-area">
                  <!-- Metrics Ribbon Chips -->
                  <div class="ll-ptrs">
                    <div class="ll-ptr-chip">
                      Sizes: <b class="ll-c-blue">{{ currentS.length }} &times; {{ currentP.length }}</b>
                    </div>
                    <div class="ll-ptr-chip" v-if="currentApproach === 'recursion' || currentApproach === 'memoization'">
                      Calls: <b class="ll-c-orange">{{ st.totalCalls || 0 }}</b>
                    </div>
                    <div class="ll-ptr-chip" v-if="currentApproach === 'tabulation'">
                      Iterations: <b class="ll-c-orange">{{ st.iterations !== undefined ? st.iterations : 0 }}</b>
                    </div>
                    <div class="ll-ptr-chip" v-if="currentApproach === 'recursion'">
                      Redundant: <b class="ll-c-red">{{ st.redundantCalls || 0 }}</b>
                    </div>
                    <div class="ll-ptr-chip" v-if="currentApproach === 'memoization'">
                      Cache Hits: <b class="ll-c-green">{{ st.cacheHits || 0 }}</b>
                    </div>
                    <div class="ll-ptr-chip" v-if="st.currentReturn !== null && st.currentReturn !== undefined">
                      Status: <b :class="st.currentReturn ? 'll-c-green' : 'll-c-red'">{{ st.currentReturn ? 'MATCHED' : 'NOT MATCHED' }}</b>
                    </div>
                  </div>

                  <!-- Visual Diagram 1: DP Tabulation (Bottom-Up 2D Matrix Grid) -->
                  <div v-if="currentApproach === 'tabulation'" class="ll-dp-tab-view">
                    <div class="ll-section-caption">
                      <span>2D Boolean Table &mdash; <code>dp[0...{{ currentS.length }}][0...{{ currentP.length }}]</code></span>
                      <span v-if="st.i >= 1 && st.j >= 1 && st.i <= currentS.length && st.j <= currentP.length" class="ll-calc-pill">
                        {{ st.isStar ? `Star '*' &rarr; dp[i][j - 1] || dp[i - 1][j]` : (st.isMatch ? `Match '${st.charP}' &rarr; dp[i - 1][j - 1]` : `Mismatch &rarr; False`) }}
                      </span>
                    </div>

                    <div class="ll-grid-scroll-wrap">
                      <table class="ll-dp-table">
                        <thead>
                          <tr>
                            <th class="ll-th-corner">s \ p</th>
                            <th class="ll-th-col">
                              <span class="ll-col-char">&empty;</span>
                              <span class="ll-col-idx">j=0</span>
                            </th>
                            <th v-for="(chP, jdx) in currentP" :key="'th-col-' + jdx" class="ll-th-col" :class="{ 'll-th-active': st.j === jdx + 1 }">
                              <span class="ll-col-char">{{ chP }}</span>
                              <span class="ll-col-idx">j={{ jdx + 1 }}</span>
                            </th>
                          </tr>
                        </thead>
                        <tbody>
                          <tr v-for="(rRow, rIdx) in st.dpGrid" :key="'tr-row-' + rIdx">
                            <th class="ll-th-row" :class="{ 'll-th-active': st.i === rIdx }">
                              <span class="ll-row-char">{{ rIdx === 0 ? '&empty;' : currentS[rIdx - 1] }}</span>
                              <span class="ll-row-idx">i={{ rIdx }}</span>
                            </th>
                            <td
                              v-for="(cellVal, cIdx) in rRow"
                              :key="'td-cell-' + rIdx + '-' + cIdx"
                              class="ll-grid-cell"
                              :class="{
                                'll-cell-base': rIdx === 0 || cIdx === 0,
                                'll-cell-cur': st.i === rIdx && st.j === cIdx,
                                'll-cell-true': cellVal === true,
                                'll-cell-false': cellVal === false,
                                'll-cell-source-diag': st.i && st.j && rIdx === st.i - 1 && cIdx === st.j - 1,
                                'll-cell-source-left': st.i && st.j && rIdx === st.i && cIdx === st.j - 1 && st.isStar,
                                'll-cell-source-top': st.i && st.j && rIdx === st.i - 1 && cIdx === st.j && st.isStar,
                                'll-cell-traceback': isTracebackCell(rIdx, cIdx),
                                'll-cell-uncalc': cellVal === '?'
                              }"
                            >
                              <div class="ll-cell-inner">
                                <span class="ll-cell-val">{{ cellVal === true ? 'T' : (cellVal === false ? 'F' : '?') }}</span>
                                <span v-if="isTracebackCell(rIdx, cIdx) && cellVal === true" class="ll-match-star">&starf;</span>
                              </div>
                            </td>
                          </tr>
                        </tbody>
                      </table>
                    </div>
                  </div>

                  <!-- Visual Diagram 2: Recursion & Memoization Decision Tree -->
                  <div v-else class="ll-tree-container">
                    <div class="ll-section-caption">
                      <span>{{ currentApproach === 'recursion' ? 'Wildcard Decision Tree O(2^(m+n))' : 'Pruned Memoization Tree O(m×n)' }}</span>
                      <span v-if="currentApproach === 'memoization'" class="ll-memo-badge-info">2D Boolean Cache Active</span>
                    </div>

                    <div class="ll-tree-scroll-area">
                      <svg
                        :viewBox="`0 0 ${st.treeWidth || 488} ${st.treeHeight || 216}`"
                        :style="{
                          width: '100%',
                          maxWidth: (st.treeWidth || 488) + 'px',
                          height: (st.treeHeight || 216) + 'px',
                          maxHeight: (st.treeHeight || 216) + 'px'
                        }"
                        class="ll-tree-svg"
                      >
                        <!-- Connector Edges -->
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
                              'll-edge-match': edge.branchType === 'match',
                              'll-edge-left': edge.branchType === 'star-empty',
                              'll-edge-right': edge.branchType === 'star-consume'
                            }"
                          />
                        </g>

                        <!-- Tree Node Badges -->
                        <g class="ll-tree-nodes">
                          <g
                            v-for="node in st.treeNodes"
                            :key="node.id"
                            :transform="`translate(${node.x}, ${node.y})`"
                            class="ll-tree-node-group"
                          >
                            <!-- Redundant Warning Badge Above Node -->
                            <g v-if="node.isRedundant" transform="translate(0, -17)">
                              <rect x="-26" y="-6" width="52" height="12" rx="3" fill="#ef4444" stroke="#dc2626" stroke-width="0.5" />
                              <text x="0" y="0" text-anchor="middle" class="ll-badge-text-red">Recomputing!</text>
                            </g>
                            <!-- Cache Hit Badge Above Node -->
                            <g v-else-if="node.state === 'cache-hit'" transform="translate(0, -17)">
                              <rect x="-26" y="-6" width="52" height="12" rx="3" fill="#9333ea" stroke="#7e22ce" stroke-width="0.5" />
                              <text x="0" y="0" text-anchor="middle" class="ll-badge-text-purple">Cache Hit O(1)</text>
                            </g>

                            <!-- Node Outer Card -->
                            <rect
                              x="-24"
                              y="-12"
                              width="48"
                              height="24"
                              rx="4"
                              class="ll-node-rect"
                              :class="{
                                'll-node-active': node.id === st.activeNodeId,
                                'll-node-solved': node.state === 'solved',
                                'll-node-redundant': node.isRedundant,
                                'll-node-cachehit': node.state === 'cache-hit'
                              }"
                            />
                            <!-- Node Text: (i, j) -->
                            <text x="0" y="-3" text-anchor="middle" class="ll-node-text-call">
                              ({{ node.i }}, {{ node.j }})
                            </text>
                            <!-- Node Return Value or Status -->
                            <text
                              x="0"
                              y="7.5"
                              text-anchor="middle"
                              class="ll-node-text-val"
                              :class="{
                                'll-val-active': node.id === st.activeNodeId,
                                'll-val-solved': node.state === 'solved',
                                'll-val-hit': node.state === 'cache-hit'
                              }"
                            >
                              {{
                                node.retVal !== null ? `=${node.retVal}` :
                                (node.state === 'cache-hit' ? '⚡ Hit' : '?')
                              }}
                            </text>
                          </g>
                        </g>
                      </svg>

                      <!-- Memoization Lookup Cache Table Strip -->
                      <div v-if="currentApproach === 'memoization' && st.memo" class="ll-memo-strip-wrap">
                        <div class="ll-memo-title">Memoization 2D Cache Table &mdash; <code>memo[0...{{ currentS.length }}][0...{{ currentP.length }}]</code>:</div>
                        <div class="ll-memo-matrix-scroll">
                          <table class="ll-memo-table">
                            <thead>
                              <tr>
                                <th class="ll-mth-corner">i \ j</th>
                                <th v-for="cIdx in (currentP.length + 1)" :key="'mth-' + (cIdx - 1)" class="ll-mth">
                                  {{ cIdx - 1 }}
                                </th>
                              </tr>
                            </thead>
                            <tbody>
                              <tr v-for="(mRow, rIdx) in st.memo" :key="'mtr-' + rIdx">
                                <th class="ll-mth">{{ rIdx }}</th>
                                <td
                                  v-for="(val, cIdx) in mRow"
                                  :key="'mtd-' + rIdx + '-' + cIdx"
                                  class="ll-memo-grid-cell"
                                  :class="{
                                    'll-memo-cell-hit': val !== -1,
                                    'll-memo-cell-empty': val === -1,
                                    'll-memo-cell-cur': st.curI === rIdx && st.curJ === cIdx
                                  }"
                                >
                                  {{ val === 1 ? 'T' : (val === 0 ? 'F' : '?') }}
                                </td>
                              </tr>
                            </tbody>
                          </table>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
              </div>

              <!-- Vertical Resizer -->
              <div class="ll-vresizer" ref="vizResizerRef"></div>

              <!-- Legend -->
              <div class="ll-legend">
                <template v-if="currentApproach === 'tabulation'">
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-base"></span>Base Row/Col</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-cur"></span>Active Cell (i, j)</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-new"></span>True (Match)</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-purple"></span>Traceback Route</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-uncalc"></span>Uncalculated (?)</span>
                </template>
                <template v-else>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-cur"></span>Active Call</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-new"></span>Solved Subproblem</span>
                  <span class="ll-leg" v-if="currentApproach === 'recursion'"><span class="ll-legdot ll-legdot-red"></span>Recomputed (Redundant)</span>
                  <span class="ll-leg" v-if="currentApproach === 'memoization'"><span class="ll-legdot ll-legdot-purple"></span>Cache Hit (O(1))</span>
                </template>
              </div>

              <!-- Variable Frames / Call Stack Table -->
              <div class="ll-table-area">
                <div class="ll-table-title">Variable frames &mdash; innermost = current</div>
                <div class="ll-stack-line">
                  <span class="ll-frame" :class="{ 'll-frame-cur': true }">
                    <span class="ll-fname">isMatch(s, p, i={{ st.i !== null && st.i !== undefined ? st.i : (st.curI !== undefined ? st.curI : 'len') }}, j={{ st.j !== null && st.j !== undefined ? st.j : (st.curJ !== undefined ? st.curJ : 'len') }})</span>
                    <span class="ll-now">&larr; active</span>
                  </span>
                </div>
              </div>

              <!-- Step Badge -->
              <div class="ll-badge-wrap">
                <div
                  class="ll-badge"
                  :class="{
                    'll-badge-error': s.badgeType === 'error',
                    'll-badge-warn': s.badgeType === 'warn',
                    'll-badge-success': s.badgeType === 'success'
                  }"
                  v-html="s.badge || 'Initializing algorithm state...'"
                ></div>
              </div>
            </div>

            <!-- Horizontal Resizer -->
            <div class="ll-resizer" ref="resizerRef"></div>

            <!-- Right Column: Code, Pseudocode, Complexity -->
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

                <!-- Code Tab with 1-to-1 Synchronized Highlighting -->
                <div v-if="rightTab === 'code'" class="ll-code-scroll">
                  <pre class="ll-pre"><span
                    v-for="(line, i) in codeLines"
                    :key="i"
                    class="ll-codeline"
                    :class="{ 'll-hl': line[0] && line[0] === s.code }"
                  >{{ (line[1] !== undefined ? line[1] : line[0]) === '' ? ' ' : (line[1] !== undefined ? line[1] : line[0]) }}</span></pre>
                </div>

                <!-- Pseudocode Tab -->
                <div v-else-if="rightTab === 'pseudo'" class="ll-code-scroll">
                  <pre class="ll-pre"><span
                    v-for="(line, i) in pseudocodeLines"
                    :key="i"
                    class="ll-codeline"
                  >{{ line }}</span></pre>
                </div>

                <!-- Complexity & Faculty Analysis Tab -->
                <div v-else class="ll-info-scroll">
                  <h3>Why Dynamic Programming? (Wildcard Matching LeetCode 44)</h3>
                  <p>
                    <strong>Wildcard Matching</strong> determines whether string <code>s</code> matches pattern <code>p</code> containing <code>'?'</code> (matches 1 char) and <code>'*'</code> (matches 0 or more chars).
                  </p>
                  <ul>
                    <li>
                      <strong>1. Exact Character or '?' Match:</strong>
                      <br /><code>if (p[j - 1] == s[i - 1] || p[j - 1] == '?') dp[i][j] = dp[i - 1][j - 1]</code>
                      <br />Both characters consume each other; the result is directly copied from the diagonal.
                    </li>
                    <li>
                      <strong>2. Star Wildcard (*):</strong>
                      <br /><code>if (p[j - 1] == '*') dp[i][j] = dp[i][j - 1] || dp[i - 1][j]</code>
                      <br />&bull; <code>dp[i][j - 1]</code>: The <code>'*'</code> matches an <strong>empty sequence</strong> (ignore star).
                      <br />&bull; <code>dp[i - 1][j]</code>: The <code>'*'</code> matches <strong>one or more characters</strong> (consume 1 char of <code>s</code>, keep star).
                    </li>
                  </ul>

                  <h3>Approaches Comparison</h3>
                  <table class="ll-complexity-table">
                    <thead>
                      <tr>
                        <th>Approach</th>
                        <th>Time Complexity</th>
                        <th>Space Complexity</th>
                        <th>Performance Characteristic</th>
                      </tr>
                    </thead>
                    <tbody>
                      <tr>
                        <td><strong>Naive Recursion</strong></td>
                        <td>O(2<sup>m+n</sup>) Exponential</td>
                        <td>O(m + n) Stack</td>
                        <td><span class="ll-c-red" style="font-weight:700">Time Limit Exceeded</span> on multiple stars</td>
                      </tr>
                      <tr>
                        <td><strong>DP Memoization</strong></td>
                        <td>O(m &times; n) Quadratic</td>
                        <td>O(m &times; n) Cache + Stack</td>
                        <td><span class="ll-c-green" style="font-weight:700">Top-Down Optimal</span> with O(1) cache lookup</td>
                      </tr>
                      <tr>
                        <td><strong>DP Tabulation</strong></td>
                        <td>O(m &times; n) Quadratic</td>
                        <td>O(m &times; n) 2D Grid</td>
                        <td><span class="ll-c-green" style="font-weight:700">Iterative Zero-Overhead</span> matrix fill</td>
                      </tr>
                      <tr>
                        <td><strong>Greedy Two-Pointers</strong></td>
                        <td>O(m &times; n) Worst, O(m) Avg</td>
                        <td>O(1) Auxiliary Space</td>
                        <td><span class="ll-c-green" style="font-weight:700">Optimal Space</span> backtrack pointers</td>
                      </tr>
                    </tbody>
                  </table>

                  <h3>Recurrence Formula</h3>
                  <p class="ll-math-box">
                    dp[i][j] = (p[j - 1]=='*' ? (dp[i][j - 1] || dp[i - 1][j]) : ((p[j - 1]==s[i - 1] || p[j - 1]=='?') && dp[i - 1][j - 1]))
                  </p>
                  <p>
                    Base conditions: <code>dp[0][0] = true</code> and <code>dp[0][j] = dp[0][j - 1]</code> if <code>p[j - 1] == '*'</code>.
                  </p>

                  <div class="ll-note">
                    <strong>Faculty Takeaway:</strong> Unlike Regular Expression Matching (LeetCode 10) where <code>'*'</code> pairs with its preceding character (e.g. <code>a*</code>), Wildcard Matching treats <code>'*'</code> as a standalone token that matches any sequence of arbitrary characters.
                  </div>
                </div>
              </div>
            </div>
          </div>

          <!-- Bottom Footer -->
          <div class="ll-footer">
            Step {{ currentStepIdx + 1 }} / {{ steps.length }}
            <span class="ll-speed-wrap">Speed <input type="range" min="100" max="2000" step="100" v-model.number="speed" /></span>
          </div>

          <!-- Warning Modal for Input Limit Exceeded -->
          <transition name="ll-modal-fade">
            <div v-if="showWarningModal" class="ll-modal-backdrop" @click.self="closeWarningModal">
              <div class="ll-modal-card" @click.stop>
                <!-- Modal Header -->
                <div class="ll-modal-header">
                  <div class="ll-modal-title-wrap">
                    <div class="ll-modal-icon-badge">
                      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" class="ll-modal-svg-icon">
                        <path d="M10.29 3.86L1.82 18a2 2 0 0 0 1.71 3h16.94a2 2 0 0 0 1.71-3L13.71 3.86a2 2 0 0 0-3.42 0z"/>
                        <line x1="12" y1="9" x2="12" y2="13"/>
                        <line x1="12" y1="17" x2="12.01" y2="17"/>
                      </svg>
                    </div>
                    <span class="ll-modal-title">{{ warningModalTitle }}</span>
                  </div>
                  <button class="ll-modal-close-btn" @click="closeWarningModal" title="Close warning">&times;</button>
                </div>

                <!-- Modal Body -->
                <div class="ll-modal-body">
                  <div class="ll-modal-badge-row">
                    <span class="ll-modal-tag-entered">Entered: "{{ requestedS }}" &amp; "{{ requestedP }}"</span>
                    <span class="ll-modal-arrow">&rarr;</span>
                    <span class="ll-modal-tag-applied">Max Limit: {{ warningModalLimit }} Chars</span>
                  </div>
                  <p class="ll-modal-message">{{ warningModalMsg }}</p>
                </div>

                <!-- Modal Footer -->
                <div class="ll-modal-footer">
                  <button class="ll-modal-confirm-btn" @click="closeWarningModal">
                    Got it, Proceed &#10003;
                  </button>
                </div>
              </div>
            </div>
          </transition>
        </div>
      </div>
    </div>
  </div>
</template>

<style>
/* Unscoped Global Scrollbar Remover for Slidev Slides */
.slide-wrapper *::-webkit-scrollbar,
.slide-wrapper::-webkit-scrollbar,
.row-main::-webkit-scrollbar,
.row-main *::-webkit-scrollbar,
.ll-root::-webkit-scrollbar,
.ll-root *::-webkit-scrollbar,
.ll-grid-scroll-wrap::-webkit-scrollbar,
.ll-code-scroll::-webkit-scrollbar,
.ll-info-scroll::-webkit-scrollbar,
.ll-table-area::-webkit-scrollbar,
.ll-viz-wrap::-webkit-scrollbar,
.ll-tree-scroll-area::-webkit-scrollbar,
.ll-memo-matrix-scroll::-webkit-scrollbar {
  display: none !important;
  width: 0 !important;
  height: 0 !important;
  background: transparent !important;
  -webkit-appearance: none !important;
}

.slide-wrapper,
.slide-wrapper *,
.row-main,
.row-main *,
.ll-root,
.ll-root * {
  scrollbar-width: none !important;
  -ms-overflow-style: none !important;
}
</style>

<style scoped>
.ll-root * { box-sizing: border-box; }
.ll-root *, .ll-root, .row-main {
  scrollbar-width: none !important;
  -ms-overflow-style: none !important;
}
.ll-root *::-webkit-scrollbar,
.ll-root::-webkit-scrollbar,
.row-main::-webkit-scrollbar {
  display: none !important;
  width: 0 !important;
  height: 0 !important;
}

.ll-root {
  --coral: #F04D4D; --coral-dark: #d93e3e; --coral-light: #fff0f0;
  --bg: #f5f6fa; --surface: #ffffff; --surface2: #f1f4f9;
  --border: #e2e8f0; --border2: #cbd5e1; --text: #1e293b; --text2: #475569; --muted: #94a3b8;
  --blue: #3b82f6; --blue-light: #eff6ff; --green: #22c55e; --green-light: #f0fdf4;
  --orange: #f97316; --orange-light: #fff7ed; --purple: #9333ea; --purple-light: #f3e8ff;
  --red: #ef4444; --red-dark: #991b1b; --red-light: #fef2f2;
  --shadow-sm: 0 1px 3px rgba(0,0,0,.08), 0 1px 2px rgba(0,0,0,.04);
  --radius: 8px; --radius-sm: 6px;
  background: var(--bg); color: var(--text);
  font-family: 'Segoe UI', system-ui, sans-serif; font-size: 12.5px;
  display: flex; flex-direction: column; overflow: hidden; width: 100%; height: 80vh;
}

@keyframes ll-pop { from { transform: scale(.85); opacity: 0; } to { transform: scale(1); opacity: 1; } }
@keyframes ll-pulse { 0%, 100% { transform: scale(1); } 50% { transform: scale(1.05); } }

.slide-wrapper { margin-top: -10px; margin-left: -30px; width: 107%; max-height: 100%; font-size: 0.8rem; font-weight: 400; }
.slide-body { display: flex; flex-direction: column; border-radius: 4px; height: 100%; }
.navbar { display: flex; flex-direction: row; justify-content: space-between; align-items: center; gap: 0.75rem; padding: 0 10px; background-color: #ffffff; position: fixed; width: 94.7%; }
.navbar > img { height: 30px; }
.navbar-title { margin: 0; font-size: 1.35rem; font-weight: 700; background-color: #ef5050; color: #ffffff; width: 80%; padding: 2px 10px; margin-left: -10px; border-radius: 5px; }
.row-main { width: 100%; height: 90%; margin-top: 36px; overflow-x: auto; overflow-y: auto; }

/* Control Panel Toolbar */
.ll-toolbar { margin-top: 4px; display: flex; align-items: center; gap: 6px; padding: 5px 12px; background: var(--surface); border-bottom: 1px solid var(--border); flex-shrink: 0; flex-wrap: wrap; box-shadow: var(--shadow-sm); }
.ll-approach-group { display: flex; gap: 2px; background: var(--surface2); padding: 2px; border-radius: var(--radius-sm); border: 1px solid var(--border); }
.ll-approach-btn { background: transparent; border: none; padding: 4px 8px; font-size: 11px; font-weight: 600; color: var(--text2); border-radius: 4px; cursor: pointer; transition: all .15s; white-space: nowrap; }
.ll-approach-btn:hover { color: var(--coral); }
.ll-approach-btn.active { background: var(--coral); color: #fff; box-shadow: var(--shadow-sm); }

.ll-input-group { display: flex; align-items: center; gap: 4px; }
.ll-input-group label { font-size: 11.5px; color: var(--muted); font-weight: 700; }
.ll-text-input { background: var(--surface); border: 1px solid var(--border2); color: var(--text); border-radius: var(--radius-sm); padding: 3px 6px; font-size: 11.5px; font-family: monospace; width: 85px; }
.ll-text-input:focus { outline: none; border-color: var(--coral); box-shadow: 0 0 0 3px rgba(240,77,77,.1); }

.ll-dropdown-wrap {
  position: relative;
  display: inline-block;
}
.ll-dropdown-btn {
  background: var(--surface2);
  border: 1px solid var(--border2);
  color: var(--text2);
  padding: 4px 10px;
  border-radius: var(--radius-sm);
  font-size: 11px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.15s ease;
  display: flex;
  align-items: center;
  gap: 4px;
  white-space: nowrap;
}
.ll-dropdown-btn:hover,
.ll-dropdown-btn.active {
  background: var(--surface);
  border-color: var(--coral);
  color: var(--coral);
}
.ll-dropdown-menu {
  position: absolute;
  top: calc(100% + 4px);
  left: 0;
  background: #ffffff;
  border: 1px solid var(--border2);
  border-radius: 6px;
  box-shadow: 0 4px 14px rgba(0, 0, 0, 0.14);
  padding: 4px;
  z-index: 9999;
  min-width: 210px;
  display: flex;
  flex-direction: column;
  gap: 2px;
  animation: ll-pop 0.15s ease;
}
.ll-dropdown-item {
  background: transparent;
  border: none;
  padding: 6px 10px;
  border-radius: 4px;
  text-align: left;
  cursor: pointer;
  display: flex;
  flex-direction: column;
  transition: background 0.15s ease;
  width: 100%;
}
.ll-dropdown-item:hover {
  background: var(--coral-light);
}
.ll-dropdown-item:hover .ll-drop-label {
  color: var(--coral);
}
.ll-drop-label {
  font-family: 'Consolas', monospace;
  font-size: 11.5px;
  font-weight: 700;
  color: var(--text);
}
.ll-drop-desc {
  font-size: 9.5px;
  color: var(--muted);
  margin-top: 1px;
}

.ll-viz-btn { background: var(--coral); color: #fff; border: none; padding: 5px 12px; border-radius: var(--radius-sm); cursor: pointer; font-size: 11.5px; font-weight: 600; box-shadow: var(--shadow-sm); transition: filter .15s; }
.ll-viz-btn:hover { filter: brightness(1.08); }

.ll-nav-controls { display: flex; margin-left: auto; align-items: center; gap: 4px; flex-shrink: 0; flex-wrap: wrap; }
.ll-nav-btn { background: var(--surface2); border: 1px solid var(--border2); color: var(--text2); padding: 4px 9px; border-radius: var(--radius-sm); cursor: pointer; font-size: 11px; font-weight: 500; transition: all .15s; white-space: nowrap; }
.ll-nav-btn:hover { background: var(--surface); border-color: var(--coral); color: var(--coral); }
.ll-play-btn { background: var(--blue-light); border: 1px solid var(--blue); color: var(--blue); min-width: 68px; font-weight: 600; padding: 4px 9px; border-radius: var(--radius-sm); cursor: pointer; font-size: 11px; transition: all .15s; }
.ll-play-btn:hover { background: var(--blue); color: #fff; }

/* Main Split Layout */
.ll-main { display: flex; flex: 1; overflow: hidden; position: relative; }
.ll-left-col { display: flex; flex-direction: column; overflow: hidden; min-width: 220px; max-width: 75%; }
.ll-resizer { width: 5px; cursor: col-resize; background: var(--border); flex-shrink: 0; transition: background .15s; position: relative; z-index: 20; }
.ll-resizer:hover, .ll-resizer.drag { background: var(--coral); }
.ll-right-col { display: flex; flex-direction: column; flex: 1; overflow: hidden; min-width: 0; }

/* Top Strings Banner */
.ll-house-banner { padding: 4px 10px; background: var(--surface2); border-bottom: 1px solid var(--border); flex-shrink: 0; }
.ll-house-banner-title { font-size: 10px; font-weight: 700; color: var(--text2); margin-bottom: 3px; }
.ll-strings-track { display: flex; flex-direction: column; gap: 3px; }
.ll-str-line { display: flex; align-items: center; gap: 6px; overflow-x: auto; }
.ll-str-tag { font-family: monospace; font-size: 10.5px; font-weight: 800; color: var(--muted); min-width: 22px; }
.ll-empty-tag { font-size: 10.5px; color: var(--muted); font-style: italic; }
.ll-char-card { display: flex; flex-direction: column; align-items: center; background: var(--surface); border: 1.5px solid var(--border2); border-radius: 5px; padding: 2px 5px; min-width: 26px; transition: all .2s ease; box-shadow: var(--shadow-sm); }
.ll-char-letter { font-family: monospace; font-size: 11px; font-weight: 800; color: var(--text); }
.ll-char-sub { font-size: 8.5px; color: var(--muted); margin-top: -1px; }
.ll-char-active { border-color: #f59e0b !important; background: #fffbeb !important; transform: translateY(-1px); box-shadow: 0 0 0 2.5px rgba(245, 158, 11, 0.3) !important; }
.ll-char-match { border-color: #10b981 !important; background: #dcfce7 !important; color: #065f46 !important; }
.ll-char-wildcard { border-color: #a855f7 !important; background: #f3e8ff !important; color: #6b21a8 !important; }

/* Left Visualization Wrappers */
.ll-viz-wrap { flex-shrink: 0; background: var(--surface); border-bottom: 1px solid var(--border); position: relative; overflow-x: auto; overflow-y: auto; }
.ll-perm-area { display: flex; flex-direction: column; align-items: stretch; min-height: 100%; width: 100%; min-width: 0; box-sizing: border-box; }
.ll-ptrs { display: flex; gap: 5px; flex-wrap: wrap; padding: 4px 10px; min-height: 26px; width: 100%; box-sizing: border-box; min-width: 0; }
.ll-ptr-chip { background: var(--surface2); border: 1px solid var(--border); border-radius: var(--radius-sm); padding: 2px 7px; font-size: 10.5px; font-family: monospace; box-shadow: var(--shadow-sm); white-space: nowrap; flex-shrink: 0; }

.ll-c-blue { color: var(--blue); } .ll-c-orange { color: var(--orange); } .ll-c-green { color: var(--green); } .ll-c-purple { color: var(--purple); } .ll-c-red { color: var(--red); }

.ll-section-caption { display: flex; justify-content: space-between; align-items: center; padding: 3px 10px; font-size: 10.5px; font-weight: 700; color: var(--text2); background: var(--surface2); border-top: 1px solid var(--border); border-bottom: 1px solid var(--border); }
.ll-calc-pill { background: #dcfce7; color: #15803d; padding: 1px 6px; border-radius: 10px; font-size: 10.5px; font-family: monospace; font-weight: 700; }
.ll-memo-badge-info { background: var(--purple-light); color: var(--purple); padding: 1px 6px; border-radius: 10px; font-size: 10px; }

/* Diagram 1: DP Tabulation 2D Matrix Grid */
.ll-dp-tab-view { display: flex; flex-direction: column; padding: 4px 10px; }
.ll-grid-scroll-wrap { overflow-x: auto; overflow-y: auto; padding: 3px 0; width: 100%; box-sizing: border-box; }
.ll-dp-table { border-collapse: separate; border-spacing: 2px; font-family: monospace; font-size: 11px; }
.ll-th-corner { background: var(--surface2); border: 1px solid var(--border2); padding: 2px 5px; font-size: 9.5px; font-weight: 700; color: var(--muted); border-radius: 3px; text-align: center; }
.ll-th-col { background: var(--surface2); border: 1px solid var(--border2); padding: 2px 5px; border-radius: 3px; text-align: center; min-width: 28px; }
.ll-th-row { background: var(--surface2); border: 1px solid var(--border2); padding: 2px 5px; border-radius: 3px; text-align: center; min-width: 28px; }
.ll-th-active { background: #fef3c7 !important; border-color: #f59e0b !important; color: #92400e !important; font-weight: 800; }
.ll-col-char, .ll-row-char { display: block; font-size: 10.5px; font-weight: 800; color: var(--text); }
.ll-col-idx, .ll-row-idx { display: block; font-size: 8px; color: var(--muted); }

.ll-grid-cell { width: 32px; height: 28px; border: 1.4px solid var(--border2); border-radius: 4px; background: var(--surface); text-align: center; font-weight: 700; color: var(--text); transition: all .2s ease; position: relative; }
.ll-cell-inner { display: flex; align-items: center; justify-content: center; width: 100%; height: 100%; position: relative; }
.ll-cell-val { font-size: 11px; font-weight: 800; }
.ll-match-star { position: absolute; top: 0px; right: 1px; font-size: 7.5px; color: #16a34a; }

.ll-cell-base { background: #f8fafc !important; color: #64748b !important; }
.ll-cell-true { background: #dcfce7 !important; color: #15803d !important; border-color: #86efac !important; font-weight: 900; }
.ll-cell-false { background: #ffffff; color: #94a3b8; }
.ll-cell-cur { border-color: #f59e0b !important; background: #fef3c7 !important; color: #92400e !important; box-shadow: 0 0 0 2.5px rgba(245, 158, 11, 0.3) !important; transform: scale(1.04); z-index: 5; }
.ll-cell-source-diag { border-color: #a855f7 !important; background: #f3e8ff !important; color: #6b21a8 !important; }
.ll-cell-source-left { border-color: #3b82f6 !important; background: #eff6ff !important; color: #1d4ed8 !important; }
.ll-cell-source-top { border-color: #f97316 !important; background: #fff7ed !important; color: #c2410c !important; }
.ll-cell-traceback { border-color: #10b981 !important; box-shadow: 0 0 0 2px rgba(16, 185, 129, 0.3) !important; }
.ll-cell-uncalc { border-style: dashed !important; color: var(--muted) !important; background: var(--surface2) !important; }

/* Diagram 2: Recursion & Memoization Call Tree */
.ll-tree-container { display: flex; flex-direction: column; width: 100%; height: 100%; min-height: 0; }
.ll-tree-scroll-area { flex: 1; overflow: auto; padding: 2px 6px 4px; display: flex; flex-direction: column; align-items: center; justify-content: flex-start; min-height: 0; width: 100%; box-sizing: border-box; }
.ll-tree-svg { display: block; flex-shrink: 0; max-width: 100%; margin: 0 auto; }
.ll-tree-edge { stroke: #cbd5e1; stroke-width: 1.6px; transition: stroke .2s; }
.ll-edge-match { stroke: #10b981; stroke-width: 2px; }
.ll-edge-left { stroke: #3b82f6; }
.ll-edge-right { stroke: #f97316; stroke-dasharray: 4, 3; }

.ll-tree-node-group { cursor: default; }
.ll-node-rect { fill: #ffffff; stroke: #cbd5e1; stroke-width: 1.3px; filter: drop-shadow(0 1px 2px rgba(0,0,0,0.05)); transition: all .25s ease; }
.ll-node-active { stroke: #f59e0b !important; stroke-width: 2px !important; fill: #fffbeb !important; filter: drop-shadow(0 0 4px rgba(245, 158, 11, 0.45)) !important; animation: ll-pulse 1.3s infinite ease-in-out; }
.ll-node-solved { stroke: #10b981 !important; fill: #dcfce7 !important; stroke-width: 1.4px !important; }
.ll-node-redundant { stroke: #ef4444 !important; stroke-dasharray: 3, 2 !important; fill: #fef2f2 !important; }
.ll-node-cachehit { stroke: #9333ea !important; stroke-width: 1.6px !important; fill: #f3e8ff !important; }

.ll-tree-svg text {
  font-family: 'Segoe UI', system-ui, sans-serif !important;
  user-select: none;
}
.ll-node-text-call {
  font-family: 'Consolas', 'Fira Code', monospace !important;
  font-size: 9.5px !important;
  font-weight: 700 !important;
  fill: #1e293b !important;
  dominant-baseline: central !important;
}
.ll-node-text-val {
  font-family: 'Consolas', 'Fira Code', monospace !important;
  font-size: 9px !important;
  font-weight: 700 !important;
  fill: #64748b !important;
  dominant-baseline: central !important;
}
.ll-val-active { fill: #b45309 !important; }
.ll-val-solved { fill: #047857 !important; }
.ll-val-hit { fill: #7e22ce !important; }

.ll-badge-text-red {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif !important;
  font-size: 6.8px !important;
  font-weight: 700 !important;
  fill: #ffffff !important;
  dominant-baseline: central !important;
}
.ll-badge-text-purple {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif !important;
  font-size: 6.8px !important;
  font-weight: 700 !important;
  fill: #ffffff !important;
  dominant-baseline: central !important;
}

/* Memoization Cache Table Strip */
.ll-memo-strip-wrap { width: 100%; padding: 3px 6px 4px; border-top: 1px dashed var(--border); background: var(--surface2); flex-shrink: 0; margin-top: 3px; box-sizing: border-box; }
.ll-memo-title { font-size: 10px; font-weight: 700; color: var(--text2); margin-bottom: 2px; }
.ll-memo-matrix-scroll { overflow-x: auto; overflow-y: auto; }
.ll-memo-table { border-collapse: separate; border-spacing: 2px; font-family: monospace; font-size: 10px; }
.ll-mth-corner, .ll-mth { background: var(--surface); border: 1px solid var(--border); padding: 1px 4px; font-weight: 700; text-align: center; border-radius: 2px; font-size: 9px; }
.ll-memo-grid-cell { width: 26px; height: 22px; text-align: center; border: 1px solid var(--border); border-radius: 2px; background: var(--surface); font-weight: 700; font-size: 10px; }
.ll-memo-cell-hit { background: #f3e8ff !important; border-color: #a855f7 !important; color: #6b21a8 !important; }
.ll-memo-cell-empty { color: var(--muted); border-style: dashed; }
.ll-memo-cell-cur { border-color: #f59e0b !important; background: #fef3c7 !important; color: #92400e !important; }

/* Resizers & Legend */
.ll-vresizer { height: 5px; cursor: row-resize; background: var(--border); flex-shrink: 0; transition: background .15s; position: relative; z-index: 20; }
.ll-vresizer:hover, .ll-vresizer.drag { background: var(--coral); }

.ll-legend { display: flex; flex-wrap: wrap; gap: 4px 10px; padding: 4px 10px; border-bottom: 1px solid var(--border); flex-shrink: 0; background: var(--surface2); }
.ll-leg { display: flex; align-items: center; gap: 4px; font-size: 10.5px; color: var(--text2); font-weight: 500; }
.ll-legdot { width: 9px; height: 9px; border-radius: 2px; flex-shrink: 0; display: inline-block; }
.ll-legdot-base { background: #eff6ff; border: 1.5px solid #3b82f6; }
.ll-legdot-cur { background: #fef3c7; border: 1.5px solid #f59e0b; }
.ll-legdot-new { background: #dcfce7; border: 1.5px solid #10b981; }
.ll-legdot-purple { background: #f3e8ff; border: 1.5px solid #9333ea; }
.ll-legdot-red { background: #fef2f2; border: 1.5px dashed #ef4444; }
.ll-legdot-uncalc { background: var(--surface2); border: 1.5px dashed var(--border2); }

/* Variable Frames & Call Stack Area */
.ll-table-area { flex-shrink: 0; padding: 4px 10px; border-bottom: 1px solid var(--border); overflow-x: hidden; overflow-y: auto; background: var(--surface); min-width: 0; box-sizing: border-box; }
.ll-table-title { font-size: 9.5px; color: var(--muted); margin-bottom: 2px; font-style: italic; }
.ll-stack-line { font-family: 'Consolas', monospace; font-size: 11px; line-height: 1.6; }
.ll-frame { font-family: 'Consolas', monospace; font-size: 10.5px; color: var(--text2); padding: 1px 0; white-space: nowrap; }
.ll-frame-cur { color: var(--orange); background: var(--orange-light); border-radius: 4px; padding: 1px 4px; }
.ll-fname { color: var(--text2); }
.ll-now { color: var(--orange); font-size: 9.5px; margin-left: 4px; }

/* Step Badge */
.ll-badge-wrap { padding: 4px 8px; border-bottom: 1px solid var(--border); flex-shrink: 0; min-height: 30px; display: flex; align-items: center; background: var(--surface); }
.ll-badge { display: inline-block; padding: 3px 10px; border-radius: var(--radius-sm); border-left: 3px solid var(--coral); background: var(--coral-light); font-size: 10.5px; color: var(--coral-dark); line-height: 1.35; word-break: break-word; font-weight: 500; }
.ll-badge-error { border-left-color: var(--red); background: var(--red-light); color: var(--red-dark); font-weight: 600; }
.ll-badge-warn { border-left-color: var(--orange); background: var(--orange-light); color: #c2410c; font-weight: 600; }
.ll-badge-success { border-left-color: var(--green); background: var(--green-light); color: #166534; font-weight: 600; }

/* Right Column: Code & Theory */
.ll-code-panel { display: flex; flex-direction: column; height: 100%; overflow: hidden; }
.ll-code-header { display: flex; align-items: center; gap: 6px; padding: 5px 12px; background: var(--surface); border-bottom: 1px solid var(--border); flex-shrink: 0; box-shadow: var(--shadow-sm); flex-wrap: wrap; }
.ll-tabbar { display: flex; gap: 3px; flex-wrap: wrap; }
.ll-tab-btn { background: var(--surface2); border: 1px solid var(--border2); color: var(--text2); padding: 4px 9px; border-radius: var(--radius-sm); cursor: pointer; font-size: 10.5px; font-weight: 600; transition: all .15s; white-space: nowrap; }
.ll-tab-btn:hover { border-color: var(--coral); color: var(--coral); }
.ll-tab-btn.active { background: var(--coral); border-color: var(--coral); color: #fff; }
.ll-lang-select { background: var(--surface2); border: 1px solid var(--border2); color: var(--text); padding: 4px 24px 4px 8px; border-radius: var(--radius-sm); font-size: 11px; font-weight: 500; cursor: pointer; min-width: 95px; margin-left: auto; transition: border-color .15s; appearance: none; background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='10' height='6' viewBox='0 0 10 6'%3E%3Cpath d='M0 0l5 6 5-6z' fill='%2394a3b8'/%3E%3C/svg%3E"); background-repeat: no-repeat; background-position: right 8px center; }
.ll-lang-select:focus { outline: none; border-color: var(--coral); box-shadow: 0 0 0 3px rgba(240,77,77,.1); }

.ll-code-scroll { flex: 1; overflow: auto; padding: 10px 14px; background: #f8fafc; min-width: 0; }
.ll-pre { font-family: 'Cascadia Code', 'Fira Code', 'Consolas', monospace; font-size: 11px; line-height: 1.5; white-space: pre; color: var(--text); margin: 0; min-height: 700px;}
.ll-codeline { display: block; padding: 0 14px; margin: 0 -14px; }
.ll-hl { background: #dcfce7; color: #15803d; border-radius: 3px; border-left: 3px solid var(--green); font-weight: 600; }

.ll-info-scroll { flex: 1; overflow: auto; padding: 12px 16px; background: var(--surface); color: var(--text2); font-size: 12px; padding-bottom:130px;}
.ll-info-scroll h3 { margin: 0 0 6px; color: var(--text); font-size: 13px; font-weight: 700; }
.ll-info-scroll h3:not(:first-child) { margin-top: 14px; }
.ll-info-scroll p { margin: 0 0 6px; }
.ll-info-scroll ul { margin: 0 0 10px 16px; padding: 0; }
.ll-info-scroll li { margin-bottom: 3px; }
.ll-info-scroll code { background: var(--surface2); border: 1px solid var(--border); border-radius: 4px; padding: 1px 4px; font-family: 'Consolas', monospace; font-size: 11px; color: var(--coral-dark); }
.ll-complexity-table { width: 100%; border-collapse: collapse; margin-bottom: 10px; font-size: 11.5px; }
.ll-complexity-table th, .ll-complexity-table td { border: 1px solid var(--border); padding: 6px 8px; text-align: left; }
.ll-complexity-table th { background: var(--surface2); color: var(--text); font-weight: 700; }
.ll-complexity-table td:nth-child(2), .ll-complexity-table td:nth-child(3) { font-family: 'Consolas', monospace; }
.ll-math-box { background: var(--surface2); border: 1px solid var(--border2); padding: 6px 10px; border-radius: var(--radius-sm); font-family: 'Consolas', monospace; font-weight: 700; color: #1e293b; margin-bottom: 6px; }
.ll-note { background: var(--orange-light); border-left: 3px solid var(--orange); border-radius: var(--radius-sm); padding: 6px 10px; font-size: 11.5px; color: var(--text2); margin-top: 8px; }

/* Footer */
.ll-footer { padding: 3px 12px; font-size: 10.5px; color: var(--muted); border-top: 1px solid var(--border); background: var(--surface); flex-shrink: 0; display: flex; align-items: center; }
.ll-speed-wrap { display: flex; align-items: center; gap: 4px; margin-left: 14px; }
.ll-speed-wrap input[type=range] { width: 80px; accent-color: var(--coral); }

@media (max-width: 900px) {
  .ll-main { flex-direction: column; }
  .ll-left-col, .ll-right-col { max-width: 100% !important; width: 100% !important; }
  .ll-resizer { display: none; }
  .ll-toolbar { flex-direction: column; align-items: stretch; }
  .ll-nav-controls { margin-left: 0; justify-content: center; }
}

/* ==================================================================== */
/* WARNING POPUP MODAL STYLING                                          */
/* ==================================================================== */
.ll-modal-backdrop {
  position: fixed;
  inset: 0;
  background: rgba(15, 23, 42, 0.45);
  backdrop-filter: blur(4px);
  z-index: 99999;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 16px;
}

.ll-modal-card {
  background: #ffffff;
  border-radius: 12px;
  border: 1px solid #fee2e2;
  box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.2), 0 8px 10px -6px rgba(0, 0, 0, 0.1);
  width: 440px;
  max-width: 92vw;
  overflow: hidden;
  display: flex;
  flex-direction: column;
}

.ll-modal-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 12px 16px;
  background: #fff5f5;
  border-bottom: 1px solid #fee2e2;
}

.ll-modal-title-wrap {
  display: flex;
  align-items: center;
  gap: 10px;
}

.ll-modal-icon-badge {
  width: 30px;
  height: 30px;
  border-radius: 50%;
  background: #fee2e2;
  color: #ef4444;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}

.ll-modal-svg-icon {
  width: 16px;
  height: 16px;
}

.ll-modal-title {
  font-size: 14px;
  font-weight: 700;
  color: #991b1b;
}

.ll-modal-close-btn {
  background: transparent;
  border: none;
  font-size: 20px;
  line-height: 1;
  color: #94a3b8;
  cursor: pointer;
  padding: 2px 6px;
  border-radius: 4px;
  transition: all 0.15s ease;
}

.ll-modal-close-btn:hover {
  background: #fee2e2;
  color: #ef4444;
}

.ll-modal-body {
  padding: 16px 18px;
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.ll-modal-badge-row {
  display: flex;
  align-items: center;
  gap: 8px;
  font-size: 11.5px;
  font-weight: 600;
}

.ll-modal-tag-entered {
  background: #fee2e2;
  color: #dc2626;
  padding: 3px 8px;
  border-radius: 4px;
}

.ll-modal-arrow {
  color: #94a3b8;
  font-weight: 700;
}

.ll-modal-tag-applied {
  background: #dcfce7;
  color: #16a34a;
  padding: 3px 8px;
  border-radius: 4px;
}

.ll-modal-message {
  font-size: 12px;
  line-height: 1.55;
  color: #334155;
  margin: 0;
}

.ll-modal-footer {
  padding: 10px 16px 14px;
  display: flex;
  justify-content: flex-end;
  background: #fafafa;
  border-top: 1px solid #f1f5f9;
}

.ll-modal-confirm-btn {
  background: #ef5050;
  color: #ffffff;
  border: none;
  padding: 7px 18px;
  border-radius: 6px;
  font-size: 12px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.15s ease;
  box-shadow: 0 2px 4px rgba(239, 80, 80, 0.25);
}

.ll-modal-confirm-btn:hover {
  background: #dc2626;
  transform: translateY(-1px);
  box-shadow: 0 4px 8px rgba(239, 80, 80, 0.35);
}

.ll-modal-fade-enter-active,
.ll-modal-fade-leave-active {
  transition: opacity 0.2s ease;
}

.ll-modal-fade-enter-active .ll-modal-card,
.ll-modal-fade-leave-active .ll-modal-card {
  transition: transform 0.2s cubic-bezier(0.16, 1, 0.3, 1), opacity 0.2s ease;
}

.ll-modal-fade-enter-from,
.ll-modal-fade-leave-to {
  opacity: 0;
}

.ll-modal-fade-enter-from .ll-modal-card,
.ll-modal-fade-leave-to .ll-modal-card {
  opacity: 0;
  transform: scale(0.95) translateY(-8px);
}
</style>
