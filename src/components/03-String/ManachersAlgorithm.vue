<script setup>
import { ref, reactive, computed, onMounted, onBeforeUnmount, watch } from 'vue';

defineProps({
  topic: { type: String, default: "Manacher's Algorithm" },
  subTopic: { type: String, default: 'O(N) Longest Palindromic Substring with Center Mirroring' }
});

const CODES = {
  java: [
    ['', 'class ManachersAlgorithm {'],
    ['c_entry', '    String longestPalindrome(String s) {'],
    ['c_check_empty', '        if (s == null || s.length() == 0) {'],
    ['c_return_empty', '            return "";'],
    ['', '        }'],
    ['c_transform_init', '        StringBuilder sb = new StringBuilder("^");'],
    ['c_transform_loop', '        for (int i = 0; i < s.length(); i++) {'],
    ['c_transform_append', '            sb.append("#").append(s.charAt(i));'],
    ['', '        }'],
    ['c_transform_end', '        sb.append("#$");'],
    ['c_t_assign', '        String t = sb.toString();'],
    ['c_n_assign', '        int n = t.length();'],
    ['c_p_init', '        int[] p = new int[n];'],
    ['c_cr_init', '        int c = 0; int r = 0;'],
    ['', ''],
    ['c_main_loop', '        for (int i = 1; i < n - 1; i++) {'],
    ['c_mirror_calc', '            int iMirror = 2 * c - i;'],
    ['c_check_r_i', '            if (r > i) {'],
    ['c_p_min', '                p[i] = Math.min(r - i, p[iMirror]);'],
    ['', '            } else {'],
    ['c_p_zero', '                p[i] = 0;'],
    ['', '            }'],
    ['c_while_expand', '            while (t.charAt(i + 1 + p[i]) == t.charAt(i - 1 - p[i])) {'],
    ['c_p_inc', '                p[i]++;'],
    ['', '            }'],
    ['c_check_update_r', '            if (i + p[i] > r) {'],
    ['c_update_c', '                c = i;'],
    ['c_update_r', '                r = i + p[i];'],
    ['', '            }'],
    ['', '        }'],
    ['c_max_init', '        int maxLen = 0; int centerIndex = 0;'],
    ['c_find_max_loop', '        for (int i = 1; i < n - 1; i++) {'],
    ['c_check_max', '            if (p[i] > maxLen) {'],
    ['c_set_max', '                maxLen = p[i]; centerIndex = i;'],
    ['', '            }'],
    ['', '        }'],
    ['c_start_calc', '        int start = (centerIndex - maxLen) / 2;'],
    ['c_return_sub', '        return s.substring(start, start + maxLen);'],
    ['c_done', '    }'],
    ['', '}']
  ],
  c: [
    ['c_entry', 'void manachersAlgorithm(const char* s, char* result) {'],
    ['c_check_empty', '    if (s == NULL || strlen(s) == 0) {'],
    ['c_return_empty', '        result[0] = \'\\0\'; return;'],
    ['', '    }'],
    ['c_transform_init', '    char t[500] = "^";'],
    ['c_transform_loop', '    for (int i = 0; s[i] != \'\\0\'; i++) {'],
    ['c_transform_append', '        sprintf(t + strlen(t), "#%c", s[i]);'],
    ['', '    }'],
    ['c_transform_end', '    strcat(t, "#$");'],
    ['c_t_assign', '    int n = strlen(t);'],
    ['c_p_init', '    int p[500] = {0};'],
    ['c_cr_init', '    int c = 0; int r = 0;'],
    ['c_main_loop', '    for (int i = 1; i < n - 1; i++) {'],
    ['c_mirror_calc', '        int iMirror = 2 * c - i;'],
    ['c_check_r_i', '        if (r > i) {'],
    ['c_p_min', '            p[i] = (r - i < p[iMirror]) ? r - i : p[iMirror];'],
    ['', '        } else {'],
    ['c_p_zero', '            p[i] = 0;'],
    ['', '        }'],
    ['c_while_expand', '        while (t[i + 1 + p[i]] == t[i - 1 - p[i]]) {'],
    ['c_p_inc', '            p[i]++;'],
    ['', '        }'],
    ['c_check_update_r', '        if (i + p[i] > r) {'],
    ['c_update_c', '            c = i; r = i + p[i];'],
    ['', '        }'],
    ['', '    }'],
    ['c_max_init', '    int maxLen = 0, centerIndex = 0;'],
    ['c_find_max_loop', '    for (int i = 1; i < n - 1; i++) {'],
    ['c_check_max', '        if (p[i] > maxLen) {'],
    ['c_set_max', '            maxLen = p[i]; centerIndex = i;'],
    ['', '        }'],
    ['', '    }'],
    ['c_start_calc', '    int start = (centerIndex - maxLen) / 2;'],
    ['c_return_sub', '    strncpy(result, s + start, maxLen); result[maxLen] = \'\\0\';'],
    ['c_done', '}']
  ],
  cpp: [
    ['', 'class ManachersAlgorithm {'],
    ['', 'public:'],
    ['c_entry', '    string longestPalindrome(string s) {'],
    ['c_check_empty', '        if (s.empty()) {'],
    ['c_return_empty', '            return "";'],
    ['', '        }'],
    ['c_transform_init', '        string t = "^";'],
    ['c_transform_loop', '        for (char ch : s) {'],
    ['c_transform_append', '            t += "#"; t += ch;'],
    ['', '        }'],
    ['c_transform_end', '        t += "#$";'],
    ['c_n_assign', '        int n = t.length();'],
    ['c_p_init', '        vector<int> p(n, 0);'],
    ['c_cr_init', '        int c = 0; int r = 0;'],
    ['c_main_loop', '        for (int i = 1; i < n - 1; i++) {'],
    ['c_mirror_calc', '            int iMirror = 2 * c - i;'],
    ['c_check_r_i', '            if (r > i) {'],
    ['c_p_min', '                p[i] = min(r - i, p[iMirror]);'],
    ['', '            } else {'],
    ['c_p_zero', '                p[i] = 0;'],
    ['', '            }'],
    ['c_while_expand', '            while (t[i + 1 + p[i]] == t[i - 1 - p[i]]) {'],
    ['c_p_inc', '                p[i]++;'],
    ['', '            }'],
    ['c_check_update_r', '            if (i + p[i] > r) {'],
    ['c_update_c', '                c = i; r = i + p[i];'],
    ['', '            }'],
    ['', '        }'],
    ['c_max_init', '        int maxLen = 0, centerIndex = 0;'],
    ['c_find_max_loop', '        for (int i = 1; i < n - 1; i++) {'],
    ['c_check_max', '            if (p[i] > maxLen) {'],
    ['c_set_max', '                maxLen = p[i]; centerIndex = i;'],
    ['', '            }'],
    ['', '        }'],
    ['c_start_calc', '        int start = (centerIndex - maxLen) / 2;'],
    ['c_return_sub', '        return s.substr(start, maxLen);'],
    ['c_done', '    }'],
    ['', '};']
  ],
  python: [
    ['', 'class ManachersAlgorithm:'],
    ['c_entry', '    def longestPalindrome(self, s: str) -> str:'],
    ['c_check_empty', '        if not s:'],
    ['c_return_empty', '            return ""'],
    ['c_transform_init', '        t = "^" + "#".join(s) + "#$"'],
    ['c_n_assign', '        n = len(t)'],
    ['c_p_init', '        p = [0] * n'],
    ['c_cr_init', '        c = 0; r = 0'],
    ['c_main_loop', '        for i in range(1, n - 1):'],
    ['c_mirror_calc', '            i_mirror = 2 * c - i'],
    ['c_check_r_i', '            if r > i:'],
    ['c_p_min', '                p[i] = min(r - i, p[i_mirror])'],
    ['', '            else:'],
    ['c_p_zero', '                p[i] = 0'],
    ['c_while_expand', '            while t[i + 1 + p[i]] == t[i - 1 - p[i]]:'],
    ['c_p_inc', '                p[i] += 1'],
    ['c_check_update_r', '            if i + p[i] > r:'],
    ['c_update_c', '                c = i'],
    ['c_update_r', '                r = i + p[i]'],
    ['c_max_init', '        max_len = 0; center_index = 0'],
    ['c_find_max_loop', '        for i in range(1, n - 1):'],
    ['c_check_max', '            if p[i] > max_len:'],
    ['c_set_max', '                max_len = p[i]; center_index = i'],
    ['c_start_calc', '        start = (center_index - max_len) // 2'],
    ['c_return_sub', '        return s[start : start + max_len]'],
    ['c_done', '']
  ],
  javascript: [
    ['', 'class ManachersAlgorithm {'],
    ['c_entry', '  longestPalindrome(s) {'],
    ['c_check_empty', '    if (!s || s.length === 0) {'],
    ['c_return_empty', '      return "";'],
    ['', '    }'],
    ['c_transform_init', '    let t = "^" + s.split("").join("#") + "#$";'],
    ['c_n_assign', '    let n = t.length;'],
    ['c_p_init', '    let p = new Array(n).fill(0);'],
    ['c_cr_init', '    let c = 0; let r = 0;'],
    ['c_main_loop', '    for (let i = 1; i < n - 1; i++) {'],
    ['c_mirror_calc', '      let iMirror = 2 * c - i;'],
    ['c_check_r_i', '      if (r > i) {'],
    ['c_p_min', '        p[i] = Math.min(r - i, p[iMirror]);'],
    ['', '      } else {'],
    ['c_p_zero', '        p[i] = 0;'],
    ['', '      }'],
    ['c_while_expand', '      while (t[i + 1 + p[i]] === t[i - 1 - p[i]]) {'],
    ['c_p_inc', '        p[i]++;'],
    ['', '      }'],
    ['c_check_update_r', '      if (i + p[i] > r) {'],
    ['c_update_c', '        c = i;'],
    ['c_update_r', '        r = i + p[i];'],
    ['', '      }'],
    ['', '    }'],
    ['c_max_init', '    let maxLen = 0; let centerIndex = 0;'],
    ['c_find_max_loop', '    for (let i = 1; i < n - 1; i++) {'],
    ['c_check_max', '      if (p[i] > maxLen) {'],
    ['c_set_max', '        maxLen = p[i]; centerIndex = i;'],
    ['', '      }'],
    ['', '    }'],
    ['c_start_calc', '    let start = Math.floor((centerIndex - maxLen) / 2);'],
    ['c_return_sub', '    return s.substring(start, start + maxLen);'],
    ['c_done', '  }'],
    ['', '}']
  ]
};

const PSEUDOCODE = [
  'function ManachersAlgorithm(s):',
  '    if length(s) == 0: return ""',
  '    t = "^" + join(s, "#") + "#$"',
  '    n = length(t), P = array of size n filled with 0',
  '    C = 0, R = 0',
  '    for i = 1 to n - 2:',
  '        iMirror = 2 * C - i',
  '        if R > i:',
  '            P[i] = min(R - i, P[iMirror])',
  '        else:',
  '            P[i] = 0',
  '        while t[i + 1 + P[i]] == t[i - 1 - P[i]]:',
  '            P[i] = P[i] + 1',
  '        if i + P[i] > R:',
  '            C = i',
  '            R = i + P[i]',
  '    maxLen = max(P), centerIndex = argmax(P)',
  '    start = (centerIndex - maxLen) / 2',
  '    return s[start : start + maxLen]',
];

function frame(title, rows) { return { title, rows }; }

function buildSteps(inputStr) {
  const steps = [];
  const s = String(inputStr || '').trim();
  const fnLabel = 'longestPalindrome(s)';

  // 1. Entry
  steps.push({
    badge: `longestPalindrome called with s="${s}"`,
    code: 'c_entry',
    vars: [frame('main()', []), frame(fnLabel, [['s', `"${s}"`]])],
    s, t: '', p: [], i: -1, c: -1, r: -1, iMirror: -1, bestPal: ''
  });

  // 2. Check Empty
  steps.push({
    badge: `if (s == null || s.length() == 0) → ${s.length === 0 ? 'TRUE (empty string)' : 'FALSE'}`,
    code: 'c_check_empty',
    vars: [frame('main()', []), frame(fnLabel, [['s', `"${s}"`]])],
    s, t: '', p: [], i: -1, c: -1, r: -1, iMirror: -1, bestPal: ''
  });

  if (s.length === 0) {
    steps.push({
      badge: `return "" → Return empty string`,
      code: 'c_return_empty',
      vars: [frame('main()', []), frame(fnLabel, [['return', '""']])],
      s, t: '', p: [], i: -1, c: -1, r: -1, iMirror: -1, bestPal: ''
    });
    return steps;
  }

  // 3. Preprocess Transform S into T line-by-line
  let sbStr = '^';
  steps.push({
    badge: `StringBuilder sb = new StringBuilder("^"); → Initialized sb with sentinel "^"`,
    code: 'c_transform_init',
    vars: [frame('main()', []), frame(fnLabel, [['s', `"${s}"`], ['sb', `"${sbStr}"`]])],
    s, t: sbStr, p: [], i: -1, c: -1, r: -1, iMirror: -1, bestPal: ''
  });

  for (let i = 0; i < s.length; i++) {
    steps.push({
      badge: `for (int i = ${i}; i < s.length() (${s.length}); i++) → processing character '${s[i]}'`,
      code: 'c_transform_loop',
      vars: [frame('main()', []), frame(fnLabel, [['s', `"${s}"`], ['i', String(i)], ['char', `'${s[i]}'`]])],
      s, t: sbStr, p: [], i: -1, c: -1, r: -1, iMirror: -1, bestPal: ''
    });

    sbStr += '#' + s[i];
    steps.push({
      badge: `sb.append("#").append(s.charAt(${i})) → Appended "#${s[i]}" → sb = "${sbStr}"`,
      code: 'c_transform_append',
      vars: [frame('main()', []), frame(fnLabel, [['i', String(i)], ['sb', `"${sbStr}"`]])],
      s, t: sbStr, p: [], i: -1, c: -1, r: -1, iMirror: -1, bestPal: ''
    });
  }

  sbStr += '#$';
  steps.push({
    badge: `sb.append("#$"); → Appended final "#$" sentinels → sb = "${sbStr}"`,
    code: 'c_transform_end',
    vars: [frame('main()', []), frame(fnLabel, [['sb', `"${sbStr}"`]])],
    s, t: sbStr, p: [], i: -1, c: -1, r: -1, iMirror: -1, bestPal: ''
  });

  const t = sbStr;
  steps.push({
    badge: `String t = sb.toString(); → Transformed string t = "${t}"`,
    code: 'c_t_assign',
    vars: [frame('main()', []), frame(fnLabel, [['t', `"${t}"`]])],
    s, t, p: [], i: -1, c: -1, r: -1, iMirror: -1, bestPal: ''
  });

  const n = t.length;
  steps.push({
    badge: `int n = t.length(); → n = ${n}`,
    code: 'c_n_assign',
    vars: [frame('main()', []), frame(fnLabel, [['t', `"${t}"`], ['n', String(n)]])],
    s, t, p: [], i: -1, c: -1, r: -1, iMirror: -1, bestPal: ''
  });

  const p = new Array(n).fill(0);
  steps.push({
    badge: `int[] p = new int[${n}]; → Allocated radius array P of size ${n}`,
    code: 'c_p_init',
    vars: [frame('main()', []), frame(fnLabel, [['n', String(n)], ['P.length', String(n)]])],
    s, t, p: [...p], i: -1, c: -1, r: -1, iMirror: -1, bestPal: ''
  });

  let c = 0;
  let r = 0;
  steps.push({
    badge: `int c = 0; int r = 0; → Initialized center pointer C = 0, right boundary pointer R = 0`,
    code: 'c_cr_init',
    vars: [frame('main()', []), frame(fnLabel, [['c', '0'], ['r', '0']])],
    s, t, p: [...p], i: -1, c, r, iMirror: -1, bestPal: ''
  });

  // 4. Main Manacher Loop
  for (let i = 1; i < n - 1; i++) {
    const iMirror = 2 * c - i;

    steps.push({
      badge: `for (int i = ${i}; i < n - 1 (${n - 1}); i++) → evaluating center index i=${i} ('${t[i]}')`,
      code: 'c_main_loop',
      vars: [frame('main()', []), frame(fnLabel, [['i', String(i)], ['c', String(c)], ['r', String(r)]])],
      s, t, p: [...p], i, c, r, iMirror: -1, bestPal: ''
    });

    steps.push({
      badge: `int iMirror = 2 * c - i → 2 * ${c} - ${i} = ${iMirror}`,
      code: 'c_mirror_calc',
      vars: [frame('main()', []), frame(fnLabel, [['i', String(i)], ['c', String(c)], ['iMirror', String(iMirror)]])],
      s, t, p: [...p], i, c, r, iMirror, bestPal: ''
    });

    steps.push({
      badge: `if (r > i) → ${r} > ${i} → ${r > i ? 'TRUE (i is inside right boundary R)' : 'FALSE (i is outside boundary R)'}`,
      code: 'c_check_r_i',
      vars: [frame('main()', []), frame(fnLabel, [['r', String(r)], ['i', String(i)], ['inside', String(r > i)]])],
      s, t, p: [...p], i, c, r, iMirror, bestPal: ''
    });

    if (r > i) {
      const minVal = Math.min(r - i, p[iMirror]);
      p[i] = minVal;
      steps.push({
        badge: `p[${i}] = Math.min(r - i, p[iMirror]) → Math.min(${r - i}, ${p[iMirror]}) = ${minVal}`,
        code: 'c_p_min',
        vars: [frame('main()', []), frame(fnLabel, [['i', String(i)], ['p[i]', String(minVal)], ['p[iMirror]', String(p[iMirror])]])],
        s, t, p: [...p], i, c, r, iMirror, bestPal: ''
      });
    } else {
      p[i] = 0;
      steps.push({
        badge: `p[${i}] = 0 → i is at or beyond boundary R, reset radius p[${i}] to 0`,
        code: 'c_p_zero',
        vars: [frame('main()', []), frame(fnLabel, [['i', String(i)], ['p[i]', '0']])],
        s, t, p: [...p], i, c, r, iMirror, bestPal: ''
      });
    }

    // Expand palindrome centered at i
    while (t[i + 1 + p[i]] === t[i - 1 - p[i]]) {
      const leftIdx = i - 1 - p[i];
      const rightIdx = i + 1 + p[i];
      const matchChar = t[leftIdx];

      steps.push({
        badge: `while (t[${rightIdx}] == t[${leftIdx}]) → '${t[rightIdx]}' == '${t[leftIdx]}' → MATCH! Expanding palindrome radius`,
        code: 'c_while_expand',
        vars: [frame('main()', []), frame(fnLabel, [['i', String(i)], ['leftIdx', String(leftIdx)], ['rightIdx', String(rightIdx)], ['matchChar', `'${matchChar}'`]])],
        s, t, p: [...p], i, c, r, iMirror, expandL: leftIdx, expandR: rightIdx, bestPal: ''
      });

      p[i]++;
      steps.push({
        badge: `p[${i}]++ → radius p[${i}] updated to ${p[i]}`,
        code: 'c_p_inc',
        vars: [frame('main()', []), frame(fnLabel, [['i', String(i)], ['p[i]', String(p[i])]])],
        s, t, p: [...p], i, c, r, iMirror, expandL: leftIdx, expandR: rightIdx, bestPal: ''
      });
    }

    // Mismatch or boundary reached check
    const mismatchL = i - 1 - p[i];
    const mismatchR = i + 1 + p[i];
    steps.push({
      badge: `while (t[${mismatchR}] == t[${mismatchL}]) → '${t[mismatchR]}' != '${t[mismatchL]}' → MISMATCH / Boundary! Stop expansion`,
      code: 'c_while_expand',
      vars: [frame('main()', []), frame(fnLabel, [['i', String(i)], ['p[i]', String(p[i])]])],
      s, t, p: [...p], i, c, r, iMirror, bestPal: ''
    });

    // Check if right boundary needs update
    steps.push({
      badge: `if (i + p[i] > r) → ${i} + ${p[i]} (${i + p[i]}) > ${r} → ${i + p[i] > r ? 'TRUE (expanding beyond R)' : 'FALSE'}`,
      code: 'c_check_update_r',
      vars: [frame('main()', []), frame(fnLabel, [['i+p[i]', String(i + p[i])], ['r', String(r)]])],
      s, t, p: [...p], i, c, r, iMirror, bestPal: ''
    });

    if (i + p[i] > r) {
      c = i;
      steps.push({
        badge: `c = i → Center C updated to ${c}`,
        code: 'c_update_c',
        vars: [frame('main()', []), frame(fnLabel, [['c', String(c)]])],
        s, t, p: [...p], i, c, r, iMirror, bestPal: ''
      });

      r = i + p[i];
      steps.push({
        badge: `r = i + p[i] → Right boundary R updated to ${r}`,
        code: 'c_update_r',
        vars: [frame('main()', []), frame(fnLabel, [['r', String(r)]])],
        s, t, p: [...p], i, c, r, iMirror, bestPal: ''
      });
    }
  }

  // 5. Find Maximum Radius & Extract Substring line-by-line
  let maxLen = 0;
  let centerIndex = 0;

  steps.push({
    badge: `int maxLen = 0; int centerIndex = 0; → Finding maximum palindrome radius in P array`,
    code: 'c_max_init',
    vars: [frame('main()', []), frame(fnLabel, [['maxLen', '0'], ['centerIndex', '0']])],
    s, t, p: [...p], i: -1, c: -1, r: -1, iMirror: -1, bestPal: ''
  });

  for (let i = 1; i < n - 1; i++) {
    steps.push({
      badge: `for (int i = ${i}; i < n - 1 (${n - 1}); i++) → checking radius P[${i}] = ${p[i]}`,
      code: 'c_find_max_loop',
      vars: [frame('main()', []), frame(fnLabel, [['i', String(i)], ['P[i]', String(p[i])], ['maxLen', String(maxLen)]])],
      s, t, p: [...p], i, c: centerIndex, r: centerIndex + maxLen, iMirror: -1, bestPal: ''
    });

    const isNewMax = p[i] > maxLen;
    steps.push({
      badge: `if (p[${i}] > maxLen) → ${p[i]} > ${maxLen} → ${isNewMax ? 'TRUE (new maximum radius found!)' : 'FALSE'}`,
      code: 'c_check_max',
      vars: [frame('main()', []), frame(fnLabel, [['i', String(i)], ['P[i]', String(p[i])], ['maxLen', String(maxLen)]])],
      s, t, p: [...p], i, c: centerIndex, r: centerIndex + maxLen, iMirror: -1, bestPal: ''
    });

    if (isNewMax) {
      maxLen = p[i];
      centerIndex = i;
      steps.push({
        badge: `maxLen = ${maxLen}; centerIndex = ${centerIndex}; → Updated maxLen=${maxLen}, centerIndex=${centerIndex}`,
        code: 'c_set_max',
        vars: [frame('main()', []), frame(fnLabel, [['i', String(i)], ['maxLen', String(maxLen)], ['centerIndex', String(centerIndex)]])],
        s, t, p: [...p], i, c: centerIndex, r: centerIndex + maxLen, iMirror: -1, bestPal: ''
      });
    }
  }

  const start = Math.floor((centerIndex - maxLen) / 2);
  const result = s.substring(start, start + maxLen);

  steps.push({
    badge: `int start = (centerIndex - maxLen) / 2 = (${centerIndex} - ${maxLen}) / 2 = ${start}`,
    code: 'c_start_calc',
    vars: [frame('main()', []), frame(fnLabel, [['start', String(start)], ['maxLen', String(maxLen)]])],
    s, t, p: [...p], i: -1, c: centerIndex, r: centerIndex + maxLen, iMirror: -1, bestPal: result
  });

  steps.push({
    badge: `return s.substring(${start}, ${start + maxLen}) → "${result}"`,
    code: 'c_return_sub',
    vars: [frame('main()', []), frame(fnLabel, [['return', `"${result}"`]])],
    s, t, p: [...p], i: -1, c: centerIndex, r: centerIndex + maxLen, iMirror: -1, bestPal: result
  });

  steps.push({
    badge: `Manacher's Algorithm Complete! Longest Palindromic Substring is "${result}" (Length: ${maxLen})`,
    code: 'c_done',
    vars: [frame('main()', []), frame(fnLabel, [['result', `"${result}"`]])],
    s, t, p: [...p], i: -1, c: centerIndex, r: centerIndex + maxLen, iMirror: -1, bestPal: result
  });

  return steps;
}

const inpStr = ref('babad');
const lang = ref('java');
const speed = ref(650);
const si = ref(0);
const playing = ref(false);
const vizHeight = ref(275);
const tableHeight = ref(60);
const leftWidth = ref(50);
const rightTab = ref('code');

const stepsData = reactive({ steps: buildSteps('babad') });
const steps = computed(() => stepsData.steps);
const s = computed(() => steps.value[Math.max(0, Math.min(si.value, steps.value.length - 1))] || {});
const codeLines = computed(() => CODES[lang.value] || []);

let playTimer = null;

function applyInput() {
  const strInput = String(inpStr.value || '').trim();
  if (!strInput.length) {
    alert('Please enter a non-empty string.');
    return;
  }

  playing.value = false;
  stepsData.steps = buildSteps(strInput);
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
  cleanupFns.push(initVResizer(vizResizerRef, vizHeight, 160, 480));
  cleanupFns.push(initVResizer(tableResizerRef, tableHeight, 50, 200));
});

onBeforeUnmount(() => {
  document.removeEventListener('keydown', onKeydown);
  clearTimeout(playTimer);
  cleanupFns.forEach(fn => fn && fn());
});

const tCells = computed(() => {
  if (!s.value.t) return [];
  const tStr = s.value.t;
  const curI = s.value.i;
  const curC = s.value.c;
  const curR = s.value.r;
  const curMirror = s.value.iMirror;
  const expL = s.value.expandL;
  const expR = s.value.expandR;

  return tStr.split('').map((val, idx) => {
    let state = 'normal';
    if (idx === curI) state = 'cur_i';
    else if (idx === curMirror) state = 'mirror';
    else if (idx === expL || idx === expR) state = 'expanding';
    else if (curC >= 0 && curR >= 0 && idx >= curC - (curR - curC) && idx <= curR) state = 'z_box';
    return { idx, val, state };
  });
});
</script>

<template>
  <div class="slide-wrapper">
    <div class="navbar">
      <h2 class="navbar-title">{{ topic }}</h2>
      <img src="../../assets/logo.png" alt="Logo" />
    </div>

    <div class="slide-body">
      <div class="row-main">
        <div class="ll-root">
          <!-- Control Panel Toolbar -->
          <div class="ll-toolbar">
            <label>Input String</label>
            <input type="text" v-model="inpStr" style="width: 180px;" class="ll-text-input" placeholder="e.g. babad" />

            <button class="ll-viz-btn" @click="applyInput">&#9654; Visualize</button>

            <div class="ll-nav-controls">
              <button class="ll-nav-btn" title="First step" @click="stepBy(-steps.length)">&#171;</button>
              <button class="ll-nav-btn" @click="stepBy(-1)">&#8249; Prev</button>
              <button class="ll-play-btn" @click="togglePlay">{{ playing ? '\u23F8 Pause' : '\u25B6 Play' }}</button>
              <button class="ll-nav-btn" @click="stepBy(1)">Next &#8250;</button>
              <button class="ll-nav-btn" title="Last step" @click="stepBy(steps.length)">&#187;</button>
            </div>
          </div>

          <div class="ll-main" ref="mainRef">
            <!-- Left Visualization Column -->
            <div class="ll-left-col" ref="leftColRef" :style="{ width: leftWidth + '%' }">
              <div class="ll-viz-wrap" :style="{ height: vizHeight + 'px' }">
                <div class="ll-perm-area">
                  <div class="ll-ptrs">
                    <div class="ll-ptr-chip">Center C = <b class="ll-c-orange">{{ s.c >= 0 ? s.c : 'N/A' }}</b></div>
                    <div class="ll-ptr-chip">Right R = <b class="ll-c-purple">{{ s.r >= 0 ? s.r : 'N/A' }}</b></div>
                    <div class="ll-ptr-chip">iMirror = <b class="ll-c-green">{{ s.iMirror >= 0 ? s.iMirror : 'N/A' }}</b></div>
                    <div class="ll-ptr-chip">Longest Palindrome = <b class="ll-c-green">"{{ s.bestPal }}"</b></div>
                  </div>

                  <!-- Transformed String & Radius P Array Tracks -->
                  <div class="ll-manacher-tracks">
                    <!-- Transformed String T Track -->
                    <div class="ll-track-group">
                      <div class="ll-track-label">Transformed String (T):</div>
                      <div class="ll-arr-track">
                        <div v-for="cell in tCells" :key="'t-' + cell.idx" class="ll-arr-cell-wrap">
                          <div class="ll-ptr-tag-wrap">
                            <span v-if="cell.idx === s.i" class="ll-ptr-lbl ll-lbl-blue">i</span>
                            <span v-if="cell.idx === s.c && s.c >= 0" class="ll-ptr-lbl ll-lbl-orange">C</span>
                            <span v-if="cell.idx === s.r && s.r >= 0" class="ll-ptr-lbl ll-lbl-purple">R</span>
                            <span v-if="cell.idx === s.iMirror && s.iMirror >= 0" class="ll-ptr-lbl ll-lbl-green">i'</span>
                          </div>
                          <div class="ll-arr-box" :class="{ 'll-box-i': cell.state === 'cur_i', 'll-box-mirror': cell.state === 'mirror', 'll-box-expanding': cell.state === 'expanding', 'll-box-zbox': cell.state === 'z_box' }">
                            {{ cell.val }}
                          </div>
                          <div class="ll-arr-idx">[{{ cell.idx }}]</div>
                        </div>
                      </div>
                    </div>

                    <!-- Radius P Array Track -->
                    <div class="ll-track-group" style="margin-top: 10px;">
                      <div class="ll-track-label">Palindrome Radius Array P[i]:</div>
                      <div class="ll-arr-track">
                        <div v-for="(val, idx) in s.p" :key="'p-' + idx" class="ll-arr-cell-wrap">
                          <div class="ll-arr-box ll-box-p" :class="{ 'll-box-i': idx === s.i, 'll-box-mirror': idx === s.iMirror }">
                            {{ val }}
                          </div>
                          <div class="ll-arr-idx">P[{{ idx }}]</div>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
              </div>

              <div class="ll-vresizer" ref="vizResizerRef"></div>

              <!-- Color Legend -->
              <div class="ll-legend">
                <span class="ll-leg"><span class="ll-legdot ll-legdot-existing"></span>Normal</span>
                <span class="ll-leg"><span class="ll-legdot ll-legdot-cur-i"></span>Current Center (i)</span>
                <span class="ll-leg"><span class="ll-legdot ll-legdot-mirror"></span>Mirror Index (i' = 2C - i)</span>
                <span class="ll-leg"><span class="ll-legdot ll-legdot-zbox"></span>Rightmost Window [C-R, R]</span>
                <span class="ll-leg"><span class="ll-legdot ll-legdot-expanding"></span>Expanding Match</span>
              </div>

              <!-- Variable Frames & Call Stack -->
              <div class="ll-table-area" :style="{ height: tableHeight + 'px' }">
                <div class="ll-table-title">Variable frames &mdash; innermost = current</div>
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
                        <span class="ll-fname">{{ r[0] }}</span>=<span :class="r[2] ? 'll-c-blue' : (depth === s.vars.length - 1 ? 'll-c-orange' : 'll-c-green')" style="font-weight:700">{{ r[1] }}</span>
                      </span>)<span v-if="depth === s.vars.length - 1" class="ll-now"> &#9668; current</span>
                    </div>
                  </template>
                  <template v-else>&mdash;</template>
                </div>
              </div>

              <div class="ll-vresizer" ref="tableResizerRef"></div>

              <!-- Step Badge -->
              <div class="ll-badge-wrap">
                <div class="ll-badge" :class="{ 'll-badge-error': s.badge.includes('MISMATCH') }">{{ s.badge }}</div>
              </div>
            </div>

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
                <div v-if="rightTab === 'code'" class="ll-code-scroll">
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

                <!-- Complexity Scroll -->
                <div v-else class="ll-info-scroll">
                  <h3>Time &amp; Space Complexity</h3>
                  <table class="ll-complexity-table">
                    <thead><tr><th>Case</th><th>Time</th><th>Why</th></tr></thead>
                    <tbody>
                      <tr><td>Time Complexity</td><td>O(N)</td><td>Each character boundary R moves strictly rightward at most 2N times total.</td></tr>
                      <tr><td>Space Complexity</td><td>O(N)</td><td>Allocates transformed string T and radius array P of size 2N + 3.</td></tr>
                    </tbody>
                  </table>
                  <p class="ll-note">
                    Key insight: <b>Manacher's Algorithm</b> uses symmetry (mirror index <code>i' = 2C - i</code>) to reuse previously computed palindrome radii in O(1) time!
                  </p>
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
.ll-root {
  --coral: #F04D4D; --coral-dark: #d93e3e; --coral-light: #fff0f0;
  --bg: #f5f6fa; --surface: #ffffff; --surface2: #f1f4f9;
  --border: #e2e8f0; --border2: #cbd5e1; --text: #1e293b; --text2: #475569; --muted: #94a3b8;
  --blue: #3b82f6; --blue-light: #eff6ff; --green: #22c55e; --green-light: #f0fdf4;
  --orange: #f97316; --orange-light: #fff7ed; --purple: #9333ea;
  --red: #ef4444; --red-dark: #991b1b; --red-light: #fef2f2;
  --shadow-sm: 0 1px 3px rgba(0,0,0,.08), 0 1px 2px rgba(0,0,0,.04);
  --radius: 8px; --radius-sm: 6px;
  background: var(--bg); color: var(--text);
  font-family: 'Segoe UI', system-ui, sans-serif; font-size: 13px;
  display: flex; flex-direction: column; height: 50vh; min-height: 600px; overflow: hidden; width: 100%;
}
@keyframes ll-pop { from { transform: scale(.85); opacity: 0; } to { transform: scale(1); opacity: 1; } }
.slide-wrapper { margin-top: -10px; margin-left: -30px; width: 107%; max-height: 100%; font-size: 0.8rem; font-weight: 400; }
.slide-body { display: flex; flex-direction: column; border-radius: 4px; height: 100%; }
.navbar { display: flex; flex-direction: row; justify-content: space-between; align-items: center; gap: 0.75rem; padding: 0 10px; background-color: #ffffff; position: fixed; width: 94.7%; }
.navbar > img { height: 30px; }
.navbar-title { margin: 0; font-size: 1.5rem; font-weight: 700; background-color: #ef5050; color: #ffffff; width: 80%; padding-left: 10px; margin-left: -10px; border-radius: 5px; }
.row-main { width: 100%; height: 90%; margin-top: 37px; overflow-x: auto; overflow-y: auto; scrollbar-width: thin; }
.ll-toolbar { display: flex; align-items: center; gap: 8px; padding: 7px 16px; background: var(--surface); border-bottom: 1px solid var(--border); flex-shrink: 0; flex-wrap: wrap; box-shadow: var(--shadow-sm); }
.ll-toolbar label { font-size: 11px; color: var(--muted); white-space: nowrap; font-weight: 600; }
.ll-text-input { background: var(--surface); border: 1px solid var(--border2); color: var(--text); border-radius: var(--radius-sm); padding: 5px 10px; font-size: 13px; font-family: monospace; transition: border-color .15s; }
.ll-text-input:focus { outline: none; border-color: var(--coral); box-shadow: 0 0 0 3px rgba(240,77,77,.1); }
.ll-viz-btn { background: var(--coral); color: #fff; border: none; padding: 6px 16px; border-radius: var(--radius-sm); cursor: pointer; font-size: 12px; font-weight: 600; box-shadow: var(--shadow-sm); transition: filter .15s; }
.ll-viz-btn:hover { filter: brightness(1.08); }
.ll-nav-controls { display: flex; margin-left: auto; align-items: center; gap: 4px; flex-shrink: 0; flex-wrap: wrap; }
.ll-nav-btn { background: var(--surface2); border: 1px solid var(--border2); color: var(--text2); padding: 5px 11px; border-radius: var(--radius-sm); cursor: pointer; font-size: 12px; font-weight: 500; transition: all .15s; white-space: nowrap; }
.ll-nav-btn:hover { background: var(--surface); border-color: var(--coral); color: var(--coral); }
.ll-play-btn { background: var(--blue-light); border: 1px solid var(--blue); color: var(--blue); min-width: 72px; font-weight: 600; padding: 5px 11px; border-radius: var(--radius-sm); cursor: pointer; font-size: 12px; transition: all .15s; }
.ll-play-btn:hover { background: var(--blue); color: #fff; }
.ll-main { display: flex; flex: 1; overflow: hidden; position: relative; }
.ll-left-col { display: flex; flex-direction: column; overflow: hidden; min-width: 200px; max-width: 72%; }
.ll-resizer { width: 5px; cursor: col-resize; background: var(--border); flex-shrink: 0; transition: background .15s; position: relative; z-index: 20; }
.ll-resizer:hover, .ll-resizer.drag { background: var(--coral); }
.ll-right-col { display: flex; flex-direction: column; flex: 1; overflow: hidden; min-width: 0; height: 78%; }
.ll-viz-wrap { flex-shrink: 0; background: var(--surface); border-bottom: 1px solid var(--border); position: relative; overflow-x: auto; overflow-y: auto; }
.ll-perm-area { display: flex; flex-direction: column; align-items: stretch; min-height: 100%; width: 100%; min-width: 0; box-sizing: border-box; }
.ll-ptrs { display: flex; gap: 8px; flex-wrap: wrap; padding: 10px 16px 4px; min-height: 36px; width: 100%; box-sizing: border-box; min-width: 0; }
.ll-ptr-chip { background: var(--surface2); border: 1px solid var(--border); border-radius: var(--radius-sm); padding: 3px 10px; font-size: 12px; font-family: monospace; box-shadow: var(--shadow-sm); white-space: nowrap; flex-shrink: 0; }
.ll-c-blue { color: var(--blue); } .ll-c-orange { color: var(--orange); } .ll-c-green { color: var(--green); } .ll-c-purple { color: var(--purple); } .ll-c-red { color: var(--red); }

/* Manacher Tracks */
.ll-manacher-tracks { display: flex; flex-direction: column; gap: 4px; padding: 4px 16px; width: 100%; }
.ll-track-group { display: flex; flex-direction: column; gap: 2px; }
.ll-track-label { font-size: 11px; font-weight: 700; color: var(--text2); font-family: monospace; }
.ll-arr-track { display: flex; align-items: flex-start; gap: 6px; width: 100%; padding-top: 4px; }
.ll-arr-cell-wrap { display: flex; flex-direction: column; align-items: center; min-width: 0; }

.ll-ptr-tag-wrap { height: 28px; display: flex; align-items: flex-end; justify-content: center; gap: 4px; margin-bottom: 2px; }
.ll-ptr-lbl { font-size: 13px; font-weight: 800; font-family: 'Consolas', 'Fira Code', monospace; display: inline-flex; flex-direction: column; align-items: center; line-height: 1; gap: 1px; white-space: nowrap; animation: ll-pop 0.2s ease; }
.ll-ptr-lbl::after { content: '↓'; font-size: 11px; font-weight: 900; line-height: 1; margin-top: 1px; }
.ll-lbl-blue { color: #3b82f6; }
.ll-lbl-orange { color: #f97316; }
.ll-lbl-purple { color: #9333ea; }
.ll-lbl-green { color: #10b981; }

.ll-arr-box { width: 38px; height: 38px; display: flex; align-items: center; justify-content: center; border: 2px solid var(--blue); border-radius: var(--radius-sm); background: #eff6ff; color: #1e293b; font-weight: 700; font-size: 15px; font-family: monospace; box-shadow: var(--shadow-sm); transition: all 0.25s ease; animation: ll-pop .3s ease; }
.ll-box-p { border-color: #cbd5e1; background: #f8fafc; color: #475569; }
.ll-box-i { border-color: #3b82f6 !important; background: #dbeafe !important; color: #1e40af !important; transform: translateY(-2px); }
.ll-box-mirror { border-color: #10b981 !important; background: #dcfce7 !important; color: #065f46 !important; transform: translateY(-2px); }
.ll-box-zbox { border-color: #f59e0b !important; background: #fef3c7 !important; color: #92400e !important; transform: translateY(-2px); }
.ll-box-expanding { border-color: #9333ea !important; background: #faf5ff !important; color: #7e22ce !important; transform: translateY(-2px); }

.ll-arr-idx { font-size: 10px; color: var(--muted); margin-top: 2px; font-family: 'Consolas', monospace; font-weight: 600; }
.ll-vresizer { height: 5px; cursor: row-resize; background: var(--border); flex-shrink: 0; transition: background .15s; position: relative; z-index: 20; }
.ll-vresizer:hover, .ll-vresizer.drag { background: var(--coral); }
.ll-legend { display: flex; flex-wrap: wrap; gap: 6px 14px; padding: 6px 12px; border-bottom: 1px solid var(--border); flex-shrink: 0; background: var(--surface2); }
.ll-leg { display: flex; align-items: center; gap: 5px; font-size: 11px; color: var(--text2); font-weight: 500; }
.ll-legdot { width: 11px; height: 11px; border-radius: 3px; flex-shrink: 0; display: inline-block; }
.ll-legdot-existing { background: #eff6ff; border: 1.5px solid var(--blue); }
.ll-legdot-cur-i { background: #dbeafe; border: 1.5px solid #3b82f6; }
.ll-legdot-mirror { background: #dcfce7; border: 1.5px solid #10b981; }
.ll-legdot-zbox { background: #fef3c7; border: 1.5px solid #f59e0b; }
.ll-legdot-expanding { background: #faf5ff; border: 1.5px solid #9333ea; }

.ll-table-area { flex-shrink: 0; padding: 8px 14px; border-bottom: 1px solid var(--border); overflow-x: hidden; overflow-y: auto; background: var(--surface); min-width: 0; box-sizing: border-box; }
.ll-table-title { font-size: 10px; color: var(--muted); margin-bottom: 4px; font-style: italic; }
.ll-stack-line { font-family: 'Consolas', monospace; font-size: 12px; line-height: 1.8; }
.ll-frame { font-family: 'Consolas', monospace; font-size: 11.5px; color: var(--text2); padding: 1px 0; white-space: nowrap; }
.ll-frame-cur { color: var(--orange); background: var(--orange-light); border-radius: 4px; padding: 1px 5px; }
.ll-fname { color: var(--text2); }
.ll-now { color: var(--orange); font-size: 10px; margin-left: 6px; }
.ll-badge-wrap { padding: 6px 10px; border-bottom: 1px solid var(--border); flex-shrink: 0; min-height: 36px; display: flex; align-items: center; background: var(--surface); }
.ll-badge { display: inline-block; padding: 4px 12px; border-radius: var(--radius-sm); border-left: 3px solid var(--coral); background: var(--coral-light); font-size: 11px; color: var(--coral-dark); line-height: 1.4; word-break: break-word; font-weight: 500; }
.ll-badge-error { border-left-color: var(--red); background: var(--red-light); color: var(--red-dark); }
.ll-code-panel { display: flex; flex-direction: column; height: 100%; overflow: hidden; }
.ll-code-header { display: flex; align-items: center; gap: 8px; padding: 8px 14px; background: var(--surface); border-bottom: 1px solid var(--border); flex-shrink: 0; box-shadow: var(--shadow-sm); flex-wrap: wrap; }
.ll-tabbar { display: flex; gap: 4px; flex-wrap: wrap; }
.ll-tab-btn { background: var(--surface2); border: 1px solid var(--border2); color: var(--text2); padding: 5px 11px; border-radius: var(--radius-sm); cursor: pointer; font-size: 11px; font-weight: 600; transition: all .15s; white-space: nowrap; }
.ll-tab-btn:hover { border-color: var(--coral); color: var(--coral); }
.ll-tab-btn.active { background: var(--coral); border-color: var(--coral); color: #fff; }
.ll-lang-select { background: var(--surface2); border: 1px solid var(--border2); color: var(--text); padding: 5px 28px 5px 10px; border-radius: var(--radius-sm); font-size: 12px; font-weight: 500; cursor: pointer; min-width: 110px; margin-left: auto; transition: border-color .15s; appearance: none; background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='10' height='6' viewBox='0 0 10 6'%3E%3Cpath d='M0 0l5 6 5-6z' fill='%2394a3b8'/%3E%3C/svg%3E"); background-repeat: no-repeat; background-position: right 10px center; }
.ll-lang-select:focus { outline: none; border-color: var(--coral); box-shadow: 0 0 0 3px rgba(240,77,77,.1); }
.ll-code-scroll { flex: 1; overflow: auto; padding: 14px 16px; background: #f8fafc; min-width: 0; }
.ll-pre { font-family: 'Cascadia Code', 'Fira Code', 'Consolas', monospace; font-size: 12px; line-height: 1.65; white-space: pre-wrap; word-break: break-all; color: var(--text); margin: 0; }
.ll-codeline { display: block; padding: 0 16px; margin: 0 -16px; }
.ll-hl { background: #dcfce7; color: #15803d; border-radius: 3px; border-left: 3px solid var(--green); font-weight: 600; }
.ll-info-scroll { flex: 1; overflow: auto; padding: 16px 20px; background: var(--surface); color: var(--text2); font-size: 13px; line-height: 1.6; }
.ll-info-scroll h3 { margin: 0 0 10px; color: var(--text); font-size: 15px; font-weight: 700; }
.ll-info-scroll h3:not(:first-child) { margin-top: 18px; }
.ll-info-scroll p { margin: 0 0 10px; }
.ll-info-scroll code { background: var(--surface2); border: 1px solid var(--border); border-radius: 4px; padding: 1px 5px; font-family: 'Consolas', monospace; font-size: 12px; color: var(--coral-dark); }
.ll-complexity-table { width: 100%; border-collapse: collapse; margin-bottom: 14px; font-size: 12.5px; }
.ll-complexity-table th, .ll-complexity-table td { border: 1px solid var(--border); padding: 8px 10px; text-align: left; }
.ll-complexity-table th { background: var(--surface2); color: var(--text); font-weight: 700; }
.ll-complexity-table td:nth-child(2) { font-family: 'Consolas', monospace; font-weight: 700; color: var(--coral-dark); }
.ll-note { background: var(--orange-light); border-left: 3px solid var(--orange); border-radius: var(--radius-sm); padding: 8px 12px; font-size: 12.5px; color: var(--text2); }
.ll-footer { padding: 4px 16px; font-size: 11px; color: var(--muted); border-top: 1px solid var(--border); background: var(--surface); flex-shrink: 0; display: flex; align-items: center; }
.ll-speed-wrap { display: flex; align-items: center; gap: 5px; margin-left: 16px; }
.ll-speed-wrap input[type=range] { width: 90px; accent-color: var(--coral); }
@media (max-width: 900px) {
  .ll-main { flex-direction: column; }
  .ll-left-col, .ll-right-col { max-width: 100% !important; width: 100% !important; }
  .ll-resizer { display: none; }
  .ll-toolbar { flex-direction: column; align-items: stretch; }
  .ll-nav-controls { margin-left: 0; justify-content: center; }
}
</style>
