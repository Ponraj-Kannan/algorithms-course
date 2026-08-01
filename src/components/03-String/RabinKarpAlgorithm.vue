<script setup>
import { ref, reactive, computed, onMounted, onBeforeUnmount, watch } from 'vue';

defineProps({
  topic: { type: String, default: 'Rabin–Karp Algorithm' },
  subTopic: { type: String, default: 'Rolling Hash Pattern Matching' }
});

const CODES = {
  java: [
    ['', 'class RabinKarpAlgorithm {'],
    ['c_entry', '    List<Integer> search(String text, String pattern) {'],
    ['c_matches', '        List<Integer> matches = new ArrayList<>();'],
    ['c_n', '        int n = text.length();'],
    ['c_m', '        int m = pattern.length();'],
    ['', ''],
    ['c_dq', '        int d = 256, q = 101;'],
    ['c_vars', '        int p = 0, t = 0, h = 1;'],
    ['', ''],
    ['c_h_loop', '        for (int i = 0; i < m - 1; i++) {'],
    ['c_h_calc', '            h = (h * d) % q;'],
    ['', '        }'],
    ['', ''],
    ['c_hash_loop', '        for (int i = 0; i < m; i++) {'],
    ['c_calc_p', '            p = (d * p + pattern.charAt(i)) % q;'],
    ['c_calc_t', '            t = (d * t + text.charAt(i)) % q;'],
    ['', '        }'],
    ['', ''],
    ['c_loop', '        for (int i = 0; i <= n - m; i++) {'],
    ['c_hash_check', '            if (p == t) {'],
    ['c_char_verify', '                int j;'],
    ['c_verify_loop', '                for (j = 0; j < m; j++) {'],
    ['c_verify_check', '                    if (text.charAt(i + j) != pattern.charAt(j)) break;'],
    ['', '                }'],
    ['c_match_found', '                if (j == m) matches.add(i);'],
    ['', '            }'],
    ['', ''],
    ['c_roll_check', '            if (i < n - m) {'],
    ['c_roll_calc', '                t = (d * (t - text.charAt(i) * h) + text.charAt(i + m)) % q;'],
    ['c_roll_mod', '                if (t < 0) t += q;'],
    ['', '            }'],
    ['', '        }'],
    ['c_return', '        return matches;'],
    ['c_done', '    }'],
    ['', '}']
  ],
  c: [
    ['c_entry', 'void searchRabinKarp(const char* text, const char* pattern) {'],
    ['c_matches', '    int matches[100], matchCount = 0;'],
    ['c_n', '    int n = strlen(text);'],
    ['c_m', '    int m = strlen(pattern);'],
    ['', ''],
    ['c_dq', '    int d = 256, q = 101;'],
    ['c_vars', '    int p = 0, t = 0, h = 1;'],
    ['', ''],
    ['c_h_loop', '    for (int i = 0; i < m - 1; i++) {'],
    ['c_h_calc', '        h = (h * d) % q;'],
    ['', '    }'],
    ['', ''],
    ['c_hash_loop', '    for (int i = 0; i < m; i++) {'],
    ['c_calc_p', '        p = (d * p + pattern[i]) % q;'],
    ['c_calc_t', '        t = (d * t + text[i]) % q;'],
    ['', '    }'],
    ['', ''],
    ['c_loop', '    for (int i = 0; i <= n - m; i++) {'],
    ['c_hash_check', '        if (p == t) {'],
    ['c_char_verify', '            int j;'],
    ['c_verify_loop', '            for (j = 0; j < m; j++) {'],
    ['c_verify_check', '                if (text[i + j] != pattern[j]) break;'],
    ['', '            }'],
    ['c_match_found', '            if (j == m) matches[matchCount++] = i;'],
    ['', '        }'],
    ['', ''],
    ['c_roll_check', '        if (i < n - m) {'],
    ['c_roll_calc', '            t = (d * (t - text[i] * h) + text[i + m]) % q;'],
    ['c_roll_mod', '            if (t < 0) t += q;'],
    ['', '        }'],
    ['', '    }'],
    ['c_done', '}']
  ],
  cpp: [
    ['', 'class RabinKarpAlgorithm {'],
    ['', 'public:'],
    ['c_entry', '    vector<int> search(string text, string pattern) {'],
    ['c_matches', '        vector<int> matches;'],
    ['c_n', '        int n = text.length();'],
    ['c_m', '        int m = pattern.length();'],
    ['', ''],
    ['c_dq', '        int d = 256, q = 101;'],
    ['c_vars', '        int p = 0, t = 0, h = 1;'],
    ['', ''],
    ['c_h_loop', '        for (int i = 0; i < m - 1; i++) {'],
    ['c_h_calc', '            h = (h * d) % q;'],
    ['', '        }'],
    ['', ''],
    ['c_hash_loop', '        for (int i = 0; i < m; i++) {'],
    ['c_calc_p', '            p = (d * p + pattern[i]) % q;'],
    ['c_calc_t', '            t = (d * t + text[i]) % q;'],
    ['', '        }'],
    ['', ''],
    ['c_loop', '        for (int i = 0; i <= n - m; i++) {'],
    ['c_hash_check', '            if (p == t) {'],
    ['c_char_verify', '                int j;'],
    ['c_verify_loop', '                for (j = 0; j < m; j++) {'],
    ['c_verify_check', '                    if (text[i + j] != pattern[j]) break;'],
    ['', '                }'],
    ['c_match_found', '                if (j == m) matches.push_back(i);'],
    ['', '            }'],
    ['', ''],
    ['c_roll_check', '            if (i < n - m) {'],
    ['c_roll_calc', '                t = (d * (t - text[i] * h) + text[i + m]) % q;'],
    ['c_roll_mod', '                if (t < 0) t += q;'],
    ['', '            }'],
    ['', '        }'],
    ['c_return', '        return matches;'],
    ['c_done', '    }'],
    ['', '};']
  ],
  python: [
    ['', 'class RabinKarpAlgorithm:'],
    ['c_entry', '    def search(self, text, pattern):'],
    ['c_matches', '        matches = []'],
    ['c_n', '        n = len(text)'],
    ['c_m', '        m = len(pattern)'],
    ['c_dq', '        d, q = 256, 101'],
    ['c_vars', '        p = t = 0; h = 1'],
    ['c_h_loop', '        for i in range(m - 1):'],
    ['c_h_calc', '            h = (h * d) % q'],
    ['c_hash_loop', '        for i in range(m):'],
    ['c_calc_p', '            p = (d * p + ord(pattern[i])) % q'],
    ['c_calc_t', '            t = (d * t + ord(text[i])) % q'],
    ['c_loop', '        for i in range(n - m + 1):'],
    ['c_hash_check', '            if p == t:'],
    ['c_verify_check', '                if text[i:i+m] == pattern:'],
    ['c_match_found', '                    matches.append(i)'],
    ['c_roll_check', '            if i < n - m:'],
    ['c_roll_calc', '                t = (d * (t - ord(text[i]) * h) + ord(text[i + m])) % q'],
    ['c_roll_mod', '                if t < 0: t += q'],
    ['c_return', '        return matches'],
    ['c_done', '']
  ],
  javascript: [
    ['', 'class RabinKarpAlgorithm {'],
    ['c_entry', '  search(text, pattern) {'],
    ['c_matches', '    const matches = [];'],
    ['c_n', '    const n = text.length;'],
    ['c_m', '    const m = pattern.length;'],
    ['', ''],
    ['c_dq', '    const d = 256, q = 101;'],
    ['c_vars', '    let p = 0, t = 0, h = 1;'],
    ['', ''],
    ['c_h_loop', '    for (let i = 0; i < m - 1; i++) {'],
    ['c_h_calc', '      h = (h * d) % q;'],
    ['', '    }'],
    ['', ''],
    ['c_hash_loop', '    for (let i = 0; i < m; i++) {'],
    ['c_calc_p', '      p = (d * p + pattern.charCodeAt(i)) % q;'],
    ['c_calc_t', '      t = (d * t + text.charCodeAt(i)) % q;'],
    ['', '    }'],
    ['', ''],
    ['c_loop', '    for (let i = 0; i <= n - m; i++) {'],
    ['c_hash_check', '      if (p === t) {'],
    ['c_char_verify', '        let j;'],
    ['c_verify_loop', '        for (j = 0; j < m; j++) {'],
    ['c_verify_check', '          if (text[i + j] !== pattern[j]) break;'],
    ['', '        }'],
    ['c_match_found', '        if (j === m) matches.push(i);'],
    ['', '      }'],
    ['', ''],
    ['c_roll_check', '      if (i < n - m) {'],
    ['c_roll_calc', '        t = (d * (t - text.charCodeAt(i) * h) + text.charCodeAt(i + m)) % q;'],
    ['c_roll_mod', '        if (t < 0) t += q;'],
    ['', '      }'],
    ['', '    }'],
    ['c_return', '    return matches;'],
    ['c_done', '  }'],
    ['', '}']
  ]
};

const PSEUDOCODE = [
  'function RabinKarp(text, pattern):',
  '    n = length(text), m = length(pattern)',
  '    d = 256, q = 101   // Base and prime modulus',
  '    p = 0, t = 0, h = 1',
  '    for i = 0 to m - 2: h = (h * d) mod q',
  '    for i = 0 to m - 1:',
  '        p = (d * p + pattern[i]) mod q',
  '        t = (d * t + text[i]) mod q',
  '    matches = []',
  '    for i = 0 to n - m:',
  '        if p == t:',
  '            for j = 0 to m - 1:',
  '                if text[i+j] != pattern[j]: break',
  '            if j == m: matches.append(i)',
  '        if i < n - m:',
  '            t = (d * (t - text[i]*h) + text[i+m]) mod q',
  '    return matches',
];

function frame(title, rows) { return { title, rows }; }

function buildSteps(initialText, initialPattern) {
  const steps = [];
  const text = String(initialText).trim();
  const pattern = String(initialPattern).trim();
  const n = text.length;
  const m = pattern.length;
  const fnLabel = 'search(text, pattern)';
  const d = 256, q = 101;
  const matches = [];

  function getTextCells(curI = -1, curJ = -1, matchUpTo = -1, isHashMatch = false, isMismatch = false, countLimit = -1) {
    return text.split('').map((val, idx) => {
      const isFoundMatch = matches.some(matchStart => idx >= matchStart && idx < matchStart + m);

      let state = 'normal';
      if (countLimit >= 0 && idx < countLimit) {
        state = 'cur_window';
      } else if (curI >= 0 && idx >= curI && idx < curI + m) {
        const offset = idx - curI;
        if (isFoundMatch) {
          state = 'match';
        } else if (isMismatch && offset === curJ) {
          state = 'mismatch';
        } else if (matchUpTo >= 0 && offset <= matchUpTo) {
          state = 'match';
        } else if (isHashMatch) {
          state = 'hash_match';
        } else {
          state = 'cur_window';
        }
      } else if (isFoundMatch) {
        state = 'match';
      }

      return { idx, val, state };
    });
  }

  function getPatternCells(curJ = -1, matchUpTo = -1, isMismatch = false, isFullMatch = false, countLimit = -1) {
    return pattern.split('').map((val, idx) => {
      let state = 'normal';
      if (countLimit >= 0 && idx < countLimit) {
        state = 'cur_window';
      } else if (isFullMatch) {
        state = 'match';
      } else if (isMismatch && idx === curJ) {
        state = 'mismatch';
      } else if (matchUpTo >= 0 && idx <= matchUpTo) {
        state = 'match';
      } else if (idx === curJ) {
        state = 'hash_match';
      }
      return { idx, val, state };
    });
  }

  // 1. Entry
  steps.push({
    badge: `search called with text="${text}" and pattern="${pattern}"`,
    code: 'c_entry',
    vars: [
      frame('main()', []),
      frame(fnLabel, [['text', `"${text}"`], ['pattern', `"${pattern}"`]])
    ],
    textCells: getTextCells(),
    patternCells: getPatternCells(),
    i: -1, j: -1, p: 0, t: 0, h: 1, matches: [], n, m
  });

  if (n === 0 || m === 0 || m > n) {
    steps.push({
      badge: `Invalid bounds (n=${n}, m=${m}) → return []`,
      code: 'c_return',
      vars: [frame('main()', []), frame(fnLabel, [['return', '[]']])],
      textCells: getTextCells(),
      patternCells: getPatternCells(),
      i: -1, j: -1, p: 0, t: 0, h: 1, matches: [], n, m
    });
    return steps;
  }

  // 2. matches = []
  steps.push({
    badge: `List<Integer> matches = new ArrayList<>() → Initialized empty matches list`,
    code: 'c_matches',
    vars: [
      frame('main()', []),
      frame(fnLabel, [['text', `"${text}"`], ['pattern', `"${pattern}"`], ['matches', '[]']])
    ],
    textCells: getTextCells(),
    patternCells: getPatternCells(),
    i: -1, j: -1, p: 0, t: 0, h: 1, matches: [], n, m
  });

  // 3. int n = text.length()
  steps.push({
    badge: `int n = text.length() → n = ${n}`,
    code: 'c_n',
    vars: [
      frame('main()', []),
      frame(fnLabel, [['n', String(n)], ['matches', '[]']])
    ],
    textCells: getTextCells(),
    patternCells: getPatternCells(),
    i: -1, j: -1, p: 0, t: 0, h: 1, matches: [], n, m
  });

  // 4. int m = pattern.length()
  steps.push({
    badge: `int m = pattern.length() → m = ${m}`,
    code: 'c_m',
    vars: [
      frame('main()', []),
      frame(fnLabel, [['n', String(n)], ['m', String(m)], ['matches', '[]']])
    ],
    textCells: getTextCells(),
    patternCells: getPatternCells(),
    i: -1, j: -1, p: 0, t: 0, h: 1, matches: [], n, m
  });

  // 5. int d = 256, q = 101
  steps.push({
    badge: `int d = 256, q = 101 → d = 256 (alphabet size), q = 101 (prime modulus)`,
    code: 'c_dq',
    vars: [
      frame('main()', []),
      frame(fnLabel, [['n', String(n)], ['m', String(m)], ['d', String(d)], ['q', String(q)]])
    ],
    textCells: getTextCells(),
    patternCells: getPatternCells(),
    i: -1, j: -1, p: 0, t: 0, h: 1, matches: [], n, m
  });

  // 6. int p = 0, t = 0, h = 1
  let p = 0, t = 0, h = 1;
  steps.push({
    badge: `int p = 0, t = 0, h = 1 → Initialized p=0, t=0, h=1`,
    code: 'c_vars',
    vars: [
      frame('main()', []),
      frame(fnLabel, [['p', '0'], ['t', '0'], ['h', '1'], ['d', '256'], ['q', '101']])
    ],
    textCells: getTextCells(),
    patternCells: getPatternCells(),
    i: -1, j: -1, p: 0, t: 0, h: 1, matches: [], n, m
  });

  // 7. Calculate h multiplier: h = d^(m-1) % q
  for (let k = 0; k < m - 1; k++) {
    steps.push({
      badge: `for (int i = ${k}; i < m - 1 (${m - 1}); i++) → calculating h multiplier step ${k + 1}`,
      code: 'c_h_loop',
      vars: [
        frame('main()', []),
        frame(fnLabel, [['i', String(k)], ['h', String(h)], ['d', String(d)], ['q', String(q)]])
      ],
      textCells: getTextCells(),
      patternCells: getPatternCells(),
      i: -1, j: -1, p, t, h, matches: [], n, m
    });

    const prevH = h;
    h = (h * d) % q;
    steps.push({
      badge: `h = (h * d) % q → h = (${prevH} * ${d}) % ${q} = ${h}`,
      code: 'c_h_calc',
      vars: [
        frame('main()', []),
        frame(fnLabel, [['i', String(k)], ['h', String(h)], ['prev_h', String(prevH)]])
      ],
      textCells: getTextCells(),
      patternCells: getPatternCells(),
      i: -1, j: -1, p, t, h, matches: [], n, m
    });
  }

  // 8. Calculate initial hashes p and t for first window of length m
  for (let k = 0; k < m; k++) {
    steps.push({
      badge: `for (int i = ${k}; i < m (${m}); i++) → computing initial hash for char index ${k} ('${pattern[k]}' & '${text[k]}')`,
      code: 'c_hash_loop',
      vars: [
        frame('main()', []),
        frame(fnLabel, [['i', String(k)], ['pattern[i]', `'${pattern[k]}'`], ['text[i]', `'${text[k]}'`]])
      ],
      textCells: getTextCells(-1, -1, -1, false, false, k + 1),
      patternCells: getPatternCells(-1, -1, false, false, k + 1),
      i: -1, j: -1, p, t, h, matches: [], n, m
    });

    p = (d * p + pattern.charCodeAt(k)) % q;
    steps.push({
      badge: `p = (d * p + pattern.charAt(${k})) % q → patternHash (p) set to ${p}`,
      code: 'c_calc_p',
      vars: [
        frame('main()', []),
        frame(fnLabel, [['i', String(k)], ['patternHash (p)', String(p)], ['char', `'${pattern[k]}'`]])
      ],
      textCells: getTextCells(-1, -1, -1, false, false, k + 1),
      patternCells: getPatternCells(-1, -1, false, false, k + 1),
      i: -1, j: -1, p, t, h, matches: [], n, m
    });

    t = (d * t + text.charCodeAt(k)) % q;
    steps.push({
      badge: `t = (d * t + text.charAt(${k})) % q → initial windowHash (t) set to ${t}`,
      code: 'c_calc_t',
      vars: [
        frame('main()', []),
        frame(fnLabel, [['i', String(k)], ['windowHash (t)', String(t)], ['char', `'${text[k]}'`]])
      ],
      textCells: getTextCells(-1, -1, -1, false, false, k + 1),
      patternCells: getPatternCells(-1, -1, false, false, k + 1),
      i: -1, j: -1, p, t, h, matches: [], n, m
    });
  }

  // 9. Main outer loop: slide window i = 0 to n - m
  for (let i = 0; i <= n - m; i++) {
    steps.push({
      badge: `for (int i = ${i}; i <= n - m (${n - m}); i++) → window aligned at index i=${i}`,
      code: 'c_loop',
      vars: [
        frame('main()', []),
        frame(fnLabel, [['i', String(i)], ['patternHash (p)', String(p)], ['windowHash (t)', String(t)]])
      ],
      textCells: getTextCells(i),
      patternCells: getPatternCells(),
      i, j: -1, p, t, h, matches: [...matches], n, m
    });

    const hashMatch = p === t;
    steps.push({
      badge: `if (p == t) → comparing patternHash (${p}) vs windowHash (${t}) at i = ${i} → ${hashMatch ? 'HASH MATCH!' : 'HASH MISMATCH'}`,
      code: 'c_hash_check',
      vars: [
        frame('main()', []),
        frame(fnLabel, [['i', String(i)], ['p', String(p)], ['t', String(t)]])
      ],
      textCells: getTextCells(i, -1, -1, hashMatch, !hashMatch),
      patternCells: getPatternCells(),
      i, j: -1, p, t, h, matches: [...matches], n, m
    });

    if (hashMatch) {
      let j;
      steps.push({
        badge: `Hash match detected (p == t == ${p}) at window i=${i}! Initializing character verification loop (int j)...`,
        code: 'c_char_verify',
        vars: [
          frame('main()', []),
          frame(fnLabel, [['i', String(i)], ['p', String(p)], ['t', String(t)], ['h', String(h)]])
        ],
        textCells: getTextCells(i, -1, -1, true),
        patternCells: getPatternCells(),
        i, j: -1, p, t, h, matches: [...matches], n, m
      });

      for (j = 0; j < m; j++) {
        const charMatch = text[i + j] === pattern[j];

        // 1. Loop iteration step
        steps.push({
          badge: `for (j = ${j}; j < ${m}; j++) → checking character position j=${j}`,
          code: 'c_verify_loop',
          vars: [
            frame('main()', []),
            frame(fnLabel, [['i', String(i)], ['j', String(j)], ['text[i+j]', `'${text[i + j]}'`, charMatch], ['pattern[j]', `'${pattern[j]}'`]])
          ],
          textCells: getTextCells(i, j, j - 1, true),
          patternCells: getPatternCells(j, j - 1),
          i, j, p, t, h, matches: [...matches], n, m
        });

        // 2. Character comparison check step
        if (charMatch) {
          steps.push({
            badge: `text.charAt(${i}+${j}) ('${text[i + j]}') == pattern.charAt(${j}) ('${pattern[j]}') → MATCH! Proceed to next char`,
            code: 'c_verify_check',
            vars: [
              frame('main()', []),
              frame(fnLabel, [['i', String(i)], ['j', String(j)], ['text[i+j]', `'${text[i + j]}'`, true], ['pattern[j]', `'${pattern[j]}'`, true]])
            ],
            textCells: getTextCells(i, j, j, true),
            patternCells: getPatternCells(j, j),
            i, j, p, t, h, matches: [...matches], n, m
          });
        } else {
          steps.push({
            badge: `text.charAt(${i}+${j}) ('${text[i + j]}') != pattern.charAt(${j}) ('${pattern[j]}') → MISMATCH! Spurious hash collision (p == t but string differs). Break loop!`,
            code: 'c_verify_check',
            vars: [
              frame('main()', []),
              frame(fnLabel, [['i', String(i)], ['j', String(j)], ['text[i+j]', `'${text[i + j]}'`, false], ['pattern[j]', `'${pattern[j]}'`, false]])
            ],
            textCells: getTextCells(i, j, j - 1, false, true),
            patternCells: getPatternCells(j, j - 1, true),
            i, j, p, t, h, matches: [...matches], n, m
          });
          break;
        }
      }

      // 3. Full match check step
      const isFullMatch = j === m;
      if (isFullMatch) {
        matches.push(i);
        steps.push({
          badge: `if (j == m) → j=${j} == m=${m}! All ${m} characters verified! Added index ${i} to matches`,
          code: 'c_match_found',
          vars: [
            frame('main()', []),
            frame(fnLabel, [['match_found_at', String(i)], ['matches', JSON.stringify(matches)]])
          ],
          textCells: getTextCells(i, -1, m - 1, true),
          patternCells: getPatternCells(-1, m - 1, false, true),
          i, j: m, p, t, h, matches: [...matches], n, m
        });
      }
    }

    if (i < n - m) {
      steps.push({
        badge: `if (i < n - m) → checking ${i} < ${n - m} → TRUE (rolling hash to next window)`,
        code: 'c_roll_check',
        vars: [
          frame('main()', []),
          frame(fnLabel, [['i', String(i)], ['n-m', String(n - m)]])
        ],
        textCells: getTextCells(i),
        patternCells: getPatternCells(),
        i, j: -1, p, t, h, matches: [...matches], n, m
      });

      const oldT = t;
      t = (d * (t - text.charCodeAt(i) * h) + text.charCodeAt(i + m)) % q;

      steps.push({
        badge: `t = (d * (t - text[i]*h) + text[i+m]) % q → Subtract '${text[i]}', add '${text[i + m]}' → t calculated as ${t}`,
        code: 'c_roll_calc',
        vars: [
          frame('main()', []),
          frame(fnLabel, [['i', String(i)], ['old_t', String(oldT)], ['calc_t', String(t)]])
        ],
        textCells: getTextCells(i),
        patternCells: getPatternCells(),
        i, j: -1, p, t, h, matches: [...matches], n, m
      });

      if (t < 0) {
        t += q;
        steps.push({
          badge: `if (t < 0) → t was negative → added modulus q (${q}) → t set to ${t}`,
          code: 'c_roll_mod',
          vars: [
            frame('main()', []),
            frame(fnLabel, [['new_t', String(t)], ['q', String(q)]])
          ],
          textCells: getTextCells(i + 1),
          patternCells: getPatternCells(),
          i: i + 1, j: -1, p, t, h, matches: [...matches], n, m
        });
      }
    }
  }

  // 10. Return
  steps.push({
    badge: `return matches → Found ${matches.length} Rabin-Karp match(es) at indices: [${matches.join(', ')}]`,
    code: 'c_return',
    vars: [
      frame('main()', []),
      frame(fnLabel, [['return', JSON.stringify(matches)]])
    ],
    textCells: getTextCells(),
    patternCells: getPatternCells(),
    i: -1, j: -1, p, t, h, matches: [...matches], n, m
  });

  steps.push({
    badge: `Rabin-Karp Search Complete! Pattern "${pattern}" found ${matches.length} time(s) at indices: [${matches.join(', ')}]`,
    code: 'c_done',
    vars: [
      frame('main()', []),
      frame(fnLabel, [['result_matches', JSON.stringify(matches)]])
    ],
    textCells: getTextCells(),
    patternCells: getPatternCells(),
    i: -1, j: -1, p, t, h, matches: [...matches], n, m
  });

  return steps;
}

const inpText = ref('DAACABCDBA');
const inpPattern = ref('CAB');
const lang = ref('java');
const speed = ref(650);
const si = ref(0);
const playing = ref(false);
const vizHeight = ref(265);
const tableHeight = ref(60);
const leftWidth = ref(50);
const rightTab = ref('code');

const stepsData = reactive({ steps: buildSteps('DAACABCDBA', 'CAB') });
const steps = computed(() => stepsData.steps);
const s = computed(() => steps.value[Math.max(0, Math.min(si.value, steps.value.length - 1))] || {});
const codeLines = computed(() => CODES[lang.value] || []);

let playTimer = null;

function applyInput() {
  const textStr = String(inpText.value || '').trim();
  const patStr = String(inpPattern.value || '').trim();
  if (!textStr.length || !patStr.length) {
    alert('Please enter non-empty text and pattern strings.');
    return;
  }

  playing.value = false;
  stepsData.steps = buildSteps(textStr, patStr);
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
            <label>Text</label>
            <input type="text" v-model="inpText" style="width: 150px;" class="ll-text-input" placeholder="e.g. DAACABCDBA" />

            <label>Pattern</label>
            <input type="text" v-model="inpPattern" style="width: 90px;" class="ll-text-input" placeholder="e.g. CAB" />

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
                    <div class="ll-ptr-chip">patternHash (p) = <b class="ll-c-purple">{{ s.p }}</b></div>
                    <div class="ll-ptr-chip">windowHash (t) = <b :class="s.p === s.t ? 'll-c-green' : 'll-c-orange'">{{ s.t }}</b></div>
                    <div class="ll-ptr-chip">h = <b class="ll-c-blue">{{ s.h }}</b></div>
                    <div class="ll-ptr-chip">Matches = <b class="ll-c-green">[{{ (s.matches || []).join(', ') }}]</b></div>
                  </div>

                  <!-- Text & Pattern Alignment Tracks -->
                  <div class="ll-rk-tracks">
                    <div class="ll-track-group">
                      <div class="ll-track-label">Text (dynamic pointer i):</div>
                      <div class="ll-arr-track">
                        <div v-for="cell in s.textCells" :key="'t-' + cell.idx" class="ll-arr-cell-wrap">
                          <div class="ll-ptr-tag-wrap">
                            <span v-if="cell.idx === s.i" class="ll-ptr-lbl ll-lbl-blue">i</span>
                            <span v-if="s.i >= 0 && s.j >= 0 && cell.idx === (s.i + s.j)" class="ll-ptr-lbl ll-lbl-orange">i+j</span>
                          </div>
                          <div class="ll-arr-box" :class="{ 'll-box-window': cell.state === 'cur_window', 'll-box-hash': cell.state === 'hash_match', 'll-box-match': cell.state === 'match', 'll-box-mismatch': cell.state === 'mismatch' }">
                            {{ cell.val === ' ' ? '\u2423' : cell.val }}
                          </div>
                          <div class="ll-arr-idx">[{{ cell.idx }}]</div>
                        </div>
                      </div>
                    </div>

                    <div class="ll-track-group" style="margin-top: 10px;">
                      <div class="ll-track-label">Pattern (dynamic pointer j):</div>
                      <div class="ll-arr-track" :style="{ paddingLeft: Math.max(0, (s.i >= 0 ? s.i : 0) * 44) + 'px' }">
                        <div v-for="cell in s.patternCells" :key="'p-' + cell.idx" class="ll-arr-cell-wrap">
                          <div class="ll-ptr-tag-wrap">
                            <span v-if="cell.idx === s.j && s.j < s.m" class="ll-ptr-lbl ll-lbl-purple">j</span>
                          </div>
                          <div class="ll-arr-box" :class="{ 'll-box-match': cell.state === 'match', 'll-box-mismatch': cell.state === 'mismatch', 'll-box-hash': cell.state === 'hash_match', 'll-box-window': cell.state === 'cur_window' }">
                            {{ cell.val }}
                          </div>
                          <div class="ll-arr-idx">j=[{{ cell.idx }}]</div>
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
                <span class="ll-leg"><span class="ll-legdot ll-legdot-window"></span>Sliding Window</span>
                <span class="ll-leg"><span class="ll-legdot ll-legdot-hash"></span>Hash Match (Verifying)</span>
                <span class="ll-leg"><span class="ll-legdot ll-legdot-match"></span>Verified Match</span>
                <span class="ll-leg"><span class="ll-legdot ll-legdot-mismatch"></span>Mismatch</span>
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
                <div class="ll-badge" :class="{ 'll-badge-error': s.badge.includes('SPURIOUS') || s.badge.includes('MISMATCH') }">{{ s.badge }}</div>
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
                      <tr><td>Best Case</td><td>O(n + m)</td><td>No hash collisions; sliding window computes hash in O(1).</td></tr>
                      <tr><td>Average Case</td><td>O(n + m)</td><td>Fast hash comparisons before character verification.</td></tr>
                      <tr><td>Worst Case</td><td>O(n &times; m)</td><td>All window hashes collide (spurious hits), forcing character checks.</td></tr>
                      <tr><td>Space</td><td>O(1)</td><td>Constant space for rolling hash state variables (p, t, h).</td></tr>
                    </tbody>
                  </table>
                  <p class="ll-note">
                    Key observation: <b>Rabin–Karp Algorithm</b> uses a rolling hash function to update window hash in <code>O(1)</code> time: <code>t = (d*(t - text[i]*h) + text[i+m]) % q</code>.
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
.ll-num-input, .ll-text-input { background: var(--surface); border: 1px solid var(--border2); color: var(--text); border-radius: var(--radius-sm); padding: 5px 8px; font-size: 13px; font-family: monospace; transition: border-color .15s; }
.ll-num-input { width: 60px; }
.ll-text-input { padding: 5px 10px; }
.ll-num-input:focus, .ll-text-input:focus { outline: none; border-color: var(--coral); box-shadow: 0 0 0 3px rgba(240,77,77,.1); }
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
.ll-viz-wrap { flex-shrink: 0; background: var(--surface); border-bottom: 1px solid var(--border); position: relative; overflow-x: hidden; overflow-y: auto; }
.ll-perm-area { display: flex; flex-direction: column; align-items: stretch; min-height: 100%; width: 100%; min-width: 0; box-sizing: border-box; }
.ll-ptrs { display: flex; gap: 8px; flex-wrap: wrap; padding: 10px 16px 4px; min-height: 36px; width: 100%; box-sizing: border-box; min-width: 0; }
.ll-ptr-chip { background: var(--surface2); border: 1px solid var(--border); border-radius: var(--radius-sm); padding: 3px 10px; font-size: 12px; font-family: monospace; box-shadow: var(--shadow-sm); white-space: nowrap; flex-shrink: 0; }
.ll-c-blue { color: var(--blue); } .ll-c-orange { color: var(--orange); } .ll-c-green { color: var(--green); } .ll-c-purple { color: var(--purple); } .ll-c-red { color: var(--red); }

/* Rabin-Karp Alignment Diagram */
.ll-rk-tracks { display: flex; flex-direction: column; gap: 4px; padding: 4px 16px; width: 100%; }
.ll-track-group { display: flex; flex-direction: column; gap: 2px; }
.ll-track-label { font-size: 11px; font-weight: 700; color: var(--text2); font-family: monospace; }
.ll-arr-track { display: flex; align-items: flex-start; transition: padding-left 0.25s ease; gap: 6px; width: 100%; padding-top: 4px; }
.ll-arr-cell-wrap { display: flex; flex-direction: column; align-items: center; min-width: 0; }

.ll-ptr-tag-wrap { height: 28px; display: flex; align-items: flex-end; justify-content: center; gap: 4px; margin-bottom: 2px; }
.ll-ptr-lbl { font-size: 13px; font-weight: 800; font-family: 'Consolas', 'Fira Code', monospace; display: inline-flex; flex-direction: column; align-items: center; line-height: 1; gap: 1px; white-space: nowrap; animation: ll-pop 0.2s ease; }
.ll-ptr-lbl::after { content: '↓'; font-size: 11px; font-weight: 900; line-height: 1; margin-top: 1px; }
.ll-lbl-blue { color: #3b82f6; }
.ll-lbl-orange { color: #f97316; }
.ll-lbl-purple { color: #9333ea; }
.ll-lbl-green { color: #10b981; }

.ll-arr-box { width: 38px; height: 38px; display: flex; align-items: center; justify-content: center; border: 2px solid var(--blue); border-radius: var(--radius-sm); background: #eff6ff; color: #1e293b; font-weight: 700; font-size: 15px; font-family: monospace; box-shadow: var(--shadow-sm); transition: all 0.25s ease; animation: ll-pop .3s ease; }
.ll-box-window { border-color: #f59e0b !important; background: #fef3c7 !important; color: #92400e !important; transform: translateY(-2px); }
.ll-box-hash { border-color: #3b82f6 !important; background: #dbeafe !important; color: #1e40af !important; transform: translateY(-2px); }
.ll-box-match { border-color: #10b981 !important; background: #dcfce7 !important; color: #065f46 !important; transform: translateY(-2px); }
.ll-box-mismatch { border-color: #ef4444 !important; background: #fef2f2 !important; color: #991b1b !important; transform: translateY(-2px); }

.ll-arr-idx { font-size: 10px; color: var(--muted); margin-top: 2px; font-family: 'Consolas', monospace; font-weight: 600; }
.ll-vresizer { height: 5px; cursor: row-resize; background: var(--border); flex-shrink: 0; transition: background .15s; position: relative; z-index: 20; }
.ll-vresizer:hover, .ll-vresizer.drag { background: var(--coral); }
.ll-legend { display: flex; flex-wrap: wrap; gap: 6px 14px; padding: 6px 12px; border-bottom: 1px solid var(--border); flex-shrink: 0; background: var(--surface2); }
.ll-leg { display: flex; align-items: center; gap: 5px; font-size: 11px; color: var(--text2); font-weight: 500; }
.ll-legdot { width: 11px; height: 11px; border-radius: 3px; flex-shrink: 0; display: inline-block; }
.ll-legdot-existing { background: #eff6ff; border: 1.5px solid var(--blue); }
.ll-legdot-window { background: #fef3c7; border: 1.5px solid #f59e0b; }
.ll-legdot-hash { background: #dbeafe; border: 1.5px solid #3b82f6; }
.ll-legdot-match { background: #dcfce7; border: 1.5px solid #10b981; }
.ll-legdot-mismatch { background: #fef2f2; border: 1.5px solid #ef4444; }

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
