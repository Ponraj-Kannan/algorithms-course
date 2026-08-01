<script setup>
import { ref, reactive, computed, onMounted, onBeforeUnmount, watch } from 'vue';

defineProps({
  topic: { type: String, default: 'String Hashing (Rolling Hash)' },
  subTopic: { type: String, default: 'Polynomial Prefix Hash & O(1) Substring Query' }
});

const CODES = {
  java: [
    ['', 'class StringHashing {'],
    ['c_entry', '    List<Integer> search(String text, String pattern, int mod) {'],
    ['c_matches', '        List<Integer> matches = new ArrayList<>();'],
    ['c_n', '        int n = text.length();'],
    ['c_m', '        int m = pattern.length();'],
    ['c_d', '        int d = 256;'],
    ['c_vars', '        int p = 0; int t = 0; int h = 1;'],
    ['', ''],
    ['c_h_loop', '        for (int i = 0; i < m - 1; i++) {'],
    ['c_h_calc', '            h = (h * d) % mod;'],
    ['', '        }'],
    ['', ''],
    ['c_pat_loop', '        for (int i = 0; i < m; i++) {'],
    ['c_pat_calc', '            p = (d * p + pattern.charAt(i)) % mod;'],
    ['', '        }'],
    ['', ''],
    ['c_win_loop', '        for (int i = 0; i < m; i++) {'],
    ['c_win_calc', '            t = (d * t + text.charAt(i)) % mod;'],
    ['', '        }'],
    ['', ''],
    ['c_loop', '        for (int i = 0; i <= n - m; i++) {'],
    ['c_compare', '            if (p == t) {'],
    ['c_match_add', '                matches.add(i);'],
    ['', '            }'],
    ['c_roll_check', '            if (i < n - m) {'],
    ['c_roll_calc', '                t = (d * (t - text.charAt(i) * h) + text.charAt(i + m)) % mod;'],
    ['c_roll_mod', '                if (t < 0) { t = t + mod; }'],
    ['', '            }'],
    ['', '        }'],
    ['c_return', '        return matches;'],
    ['c_done', '    }'],
    ['', '}']
  ],
  c: [
    ['c_entry', 'void rollingHashSearch(const char* text, const char* pattern, int mod) {'],
    ['c_n', '    int n = strlen(text);'],
    ['c_m', '    int m = strlen(pattern);'],
    ['c_d', '    int d = 256;'],
    ['c_vars', '    int p = 0; int t = 0; int h = 1;'],
    ['c_h_loop', '    for (int i = 0; i < m - 1; i++) {'],
    ['c_h_calc', '        h = (h * d) % mod;'],
    ['', '    }'],
    ['c_pat_loop', '    for (int i = 0; i < m; i++) {'],
    ['c_pat_calc', '        p = (d * p + pattern[i]) % mod;'],
    ['', '    }'],
    ['c_win_loop', '    for (int i = 0; i < m; i++) {'],
    ['c_win_calc', '        t = (d * t + text[i]) % mod;'],
    ['', '    }'],
    ['c_loop', '    for (int i = 0; i <= n - m; i++) {'],
    ['c_compare', '        if (p == t) {'],
    ['c_match_add', '            printf("Match at %d\\n", i);'],
    ['', '        }'],
    ['c_roll_check', '        if (i < n - m) {'],
    ['c_roll_calc', '            t = (d * (t - text[i] * h) + text[i + m]) % mod;'],
    ['c_roll_mod', '            if (t < 0) { t += mod; }'],
    ['', '        }'],
    ['', '    }'],
    ['c_done', '}']
  ],
  cpp: [
    ['', 'class StringHashing {'],
    ['', 'public:'],
    ['c_entry', '    vector<int> search(string text, string pattern, int mod) {'],
    ['c_matches', '        vector<int> matches;'],
    ['c_n', '        int n = text.length();'],
    ['c_m', '        int m = pattern.length();'],
    ['c_d', '        int d = 256;'],
    ['c_vars', '        int p = 0; int t = 0; int h = 1;'],
    ['c_h_loop', '        for (int i = 0; i < m - 1; i++) {'],
    ['c_h_calc', '            h = (h * d) % mod;'],
    ['', '        }'],
    ['c_pat_loop', '        for (int i = 0; i < m; i++) {'],
    ['c_pat_calc', '            p = (d * p + pattern[i]) % mod;'],
    ['', '        }'],
    ['c_win_loop', '        for (int i = 0; i < m; i++) {'],
    ['c_win_calc', '            t = (d * t + text[i]) % mod;'],
    ['', '        }'],
    ['c_loop', '        for (int i = 0; i <= n - m; i++) {'],
    ['c_compare', '            if (p == t) {'],
    ['c_match_add', '                matches.push_back(i);'],
    ['', '            }'],
    ['c_roll_check', '            if (i < n - m) {'],
    ['c_roll_calc', '                t = (d * (t - text[i] * h) + text[i + m]) % mod;'],
    ['c_roll_mod', '                if (t < 0) { t += mod; }'],
    ['', '            }'],
    ['', '        }'],
    ['c_return', '        return matches;'],
    ['c_done', '    }'],
    ['', '};']
  ],
  python: [
    ['', 'class StringHashing:'],
    ['c_entry', '    def search(self, text: str, pattern: str, mod: int):'],
    ['c_matches', '        matches = []'],
    ['c_n', '        n = len(text)'],
    ['c_m', '        m = len(pattern)'],
    ['c_d', '        d = 256'],
    ['c_vars', '        p, t, h = 0, 0, 1'],
    ['c_h_loop', '        for i in range(m - 1):'],
    ['c_h_calc', '            h = (h * d) % mod'],
    ['c_pat_loop', '        for i in range(m):'],
    ['c_pat_calc', '            p = (d * p + ord(pattern[i])) % mod'],
    ['c_win_loop', '        for i in range(m):'],
    ['c_win_calc', '            t = (d * t + ord(text[i])) % mod'],
    ['c_loop', '        for i in range(n - m + 1):'],
    ['c_compare', '            if p == t:'],
    ['c_match_add', '                matches.append(i)'],
    ['c_roll_check', '            if i < n - m:'],
    ['c_roll_calc', '                t = (d * (t - ord(text[i]) * h) + ord(text[i + m])) % mod'],
    ['c_roll_mod', '                if t < 0: t += mod'],
    ['c_return', '        return matches'],
    ['c_done', '']
  ],
  javascript: [
    ['', 'class StringHashing {'],
    ['c_entry', '  search(text, pattern, mod) {'],
    ['c_matches', '    const matches = [];'],
    ['c_n', '    const n = text.length;'],
    ['c_m', '    const m = pattern.length;'],
    ['c_d', '    const d = 256;'],
    ['c_vars', '    let p = 0; let t = 0; let h = 1;'],
    ['c_h_loop', '    for (let i = 0; i < m - 1; i++) {'],
    ['c_h_calc', '      h = (h * d) % mod;'],
    ['', '    }'],
    ['c_pat_loop', '    for (let i = 0; i < m; i++) {'],
    ['c_pat_calc', '      p = (d * p + pattern.charCodeAt(i)) % mod;'],
    ['', '    }'],
    ['c_win_loop', '    for (let i = 0; i < m; i++) {'],
    ['c_win_calc', '      t = (d * t + text.charCodeAt(i)) % mod;'],
    ['', '    }'],
    ['c_loop', '    for (let i = 0; i <= n - m; i++) {'],
    ['c_compare', '      if (p === t) {'],
    ['c_match_add', '        matches.push(i);'],
    ['', '      }'],
    ['c_roll_check', '      if (i < n - m) {'],
    ['c_roll_calc', '        t = (d * (t - text.charCodeAt(i) * h) + text.charCodeAt(i + m)) % mod;'],
    ['c_roll_mod', '        if (t < 0) { t += mod; }'],
    ['', '      }'],
    ['', '    }'],
    ['c_return', '    return matches;'],
    ['c_done', '  }'],
    ['', '}']
  ]
};

const PSEUDOCODE = [
  'function RollingHashSearch(text, pattern, mod):',
  '    n = length(text), m = length(pattern), d = 256',
  '    matches = [], p = 0, t = 0, h = 1',
  '    for i = 0 to m - 2:',
  '        h = (h * d) % mod',
  '    for i = 0 to m - 1:',
  '        p = (d * p + pattern[i]) % mod',
  '    for i = 0 to m - 1:',
  '        t = (d * t + text[i]) % mod',
  '    for i = 0 to n - m:',
  '        if p == t:',
  '            matches.append(i)',
  '        if i < n - m:',
  '            t = (d * (t - text[i] * h) + text[i + m]) % mod',
  '            if t < 0: t += mod',
  '    return matches',
];

function frame(title, rows) { return { title, rows }; }

function buildSteps(initialText, initialPattern, initialMod) {
  const steps = [];
  const text = String(initialText).trim();
  const pattern = String(initialPattern).trim();
  const mod = Number(initialMod) || 101;
  const n = text.length;
  const m = pattern.length;
  const d = 256;
  const fnLabel = 'search(text, pattern, mod)';
  const matches = [];

  function getTextCells(curI = -1, isFoundMatch = false, isMatch = false, isMismatch = false, countLimit = -1, isWinHash = false) {
    return text.split('').map((val, idx) => {
      let state = 'normal';
      if (countLimit >= 0 && idx < countLimit) {
        state = 'win_hash';
      } else if (curI >= 0 && idx >= curI && idx < curI + m) {
        if (isMatch) state = 'match';
        else if (isMismatch) state = 'mismatch';
        else if (isWinHash) state = 'win_hash';
        else state = 'cur_window';
      } else if (isFoundMatch) {
        state = 'match';
      }

      return { idx, val, state };
    });
  }

  function getPatternCells(isPatHash = false, countLimit = -1) {
    return pattern.split('').map((val, idx) => {
      let state = 'normal';
      if (countLimit >= 0 && idx < countLimit) state = 'pat_hash';
      else if (isPatHash) state = 'pat_hash';
      return { idx, val, state };
    });
  }

  // 1. Entry
  steps.push({
    badge: `search called with text="${text}", pattern="${pattern}", mod=${mod}`,
    code: 'c_entry',
    vars: [
      frame('main()', []),
      frame(fnLabel, [['text', `"${text}"`], ['pattern', `"${pattern}"`], ['mod', String(mod)]])
    ],
    textCells: getTextCells(),
    patternCells: getPatternCells(),
    i: -1, patI: -1, patOffset: 0, p: 0, t: 0, h: 1, matches: [], n, m, mod
  });

  if (n === 0 || m === 0 || m > n) {
    steps.push({
      badge: `Invalid bounds (n=${n}, m=${m}) → return []`,
      code: 'c_return',
      vars: [frame('main()', []), frame(fnLabel, [['return', '[]']])],
      textCells: getTextCells(),
      patternCells: getPatternCells(),
      i: -1, patI: -1, patOffset: 0, p: 0, t: 0, h: 1, matches: [], n, m, mod
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
    i: -1, patI: -1, patOffset: 0, p: 0, t: 0, h: 1, matches: [], n, m, mod
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
    i: -1, patI: -1, patOffset: 0, p: 0, t: 0, h: 1, matches: [], n, m, mod
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
    i: -1, patI: -1, patOffset: 0, p: 0, t: 0, h: 1, matches: [], n, m, mod
  });

  // 5. int d = 256
  steps.push({
    badge: `int d = 256 → d set to 256 (alphabet size)`,
    code: 'c_d',
    vars: [
      frame('main()', []),
      frame(fnLabel, [['n', String(n)], ['m', String(m)], ['d', String(d)]])
    ],
    textCells: getTextCells(),
    patternCells: getPatternCells(),
    i: -1, patI: -1, patOffset: 0, p: 0, t: 0, h: 1, matches: [], n, m, mod
  });

  // 6. int p = 0, t = 0, h = 1
  let p = 0, t = 0, h = 1;
  steps.push({
    badge: `int p = 0, t = 0, h = 1 → Initialized p=0, t=0, h=1`,
    code: 'c_vars',
    vars: [
      frame('main()', []),
      frame(fnLabel, [['p', '0'], ['t', '0'], ['h', '1'], ['d', '256'], ['mod', String(mod)]])
    ],
    textCells: getTextCells(),
    patternCells: getPatternCells(),
    i: -1, patI: -1, patOffset: 0, p: 0, t: 0, h: 1, matches: [], n, m, mod
  });

  // 7. Calculate multiplier h = d^(m-1) % mod step by step
  for (let k = 0; k < m - 1; k++) {
    steps.push({
      badge: `for (int i = ${k}; i < m - 1 (${m - 1}); i++) → calculating h multiplier step ${k + 1}`,
      code: 'c_h_loop',
      vars: [
        frame('main()', []),
        frame(fnLabel, [['i', String(k)], ['h', String(h)], ['d', String(d)], ['mod', String(mod)]])
      ],
      textCells: getTextCells(),
      patternCells: getPatternCells(),
      i: -1, patI: -1, patOffset: 0, p: 0, t: 0, h, matches: [], n, m, mod
    });

    const prevH = h;
    h = (h * d) % mod;
    steps.push({
      badge: `h = (h * d) % mod → h = (${prevH} * ${d}) % ${mod} = ${h}`,
      code: 'c_h_calc',
      vars: [
        frame('main()', []),
        frame(fnLabel, [['i', String(k)], ['h', String(h)], ['prev_h', String(prevH)]])
      ],
      textCells: getTextCells(),
      patternCells: getPatternCells(),
      i: -1, patI: -1, patOffset: 0, p: 0, t: 0, h, matches: [], n, m, mod
    });
  }

  // 8. Calculate pattern hash (p) step by step
  for (let k = 0; k < m; k++) {
    steps.push({
      badge: `for (int i = ${k}; i < m (${m}); i++) → calculating pattern hash for char index ${k} ('${pattern[k]}')`,
      code: 'c_pat_loop',
      vars: [
        frame('main()', []),
        frame(fnLabel, [['i', String(k)], ['pattern[i]', `'${pattern[k]}'`]])
      ],
      textCells: getTextCells(),
      patternCells: getPatternCells(false, k + 1),
      i: -1, patI: k, patOffset: 0, p, t: 0, h, matches: [], n, m, mod
    });

    const codeK = pattern.charCodeAt(k);
    const prevP = p;
    p = (d * p + codeK) % mod;
    steps.push({
      badge: `p = (d * p + pattern.charAt(${k})) % mod → (${d} * ${prevP} + ${codeK}) % ${mod} → patternHash (p) = ${p}`,
      code: 'c_pat_calc',
      vars: [
        frame('main()', []),
        frame(fnLabel, [['i', String(k)], ['patternHash (p)', String(p)], ['char', `'${pattern[k]}'`]])
      ],
      textCells: getTextCells(),
      patternCells: getPatternCells(false, k + 1),
      i: -1, patI: k, patOffset: 0, p, t: 0, h, matches: [], n, m, mod
    });
  }

  // 9. Calculate first window hash (t) step by step for text[0..m-1]
  for (let k = 0; k < m; k++) {
    steps.push({
      badge: `for (int i = ${k}; i < m (${m}); i++) → calculating first window hash for text index ${k} ('${text[k]}')`,
      code: 'c_win_loop',
      vars: [
        frame('main()', []),
        frame(fnLabel, [['i', String(k)], ['text[i]', `'${text[k]}'`]])
      ],
      textCells: getTextCells(-1, false, false, false, k + 1),
      patternCells: getPatternCells(true),
      i: k, patI: -1, patOffset: 0, p, t, h, matches: [], n, m, mod
    });

    const codeK = text.charCodeAt(k);
    const prevT = t;
    t = (d * t + codeK) % mod;
    steps.push({
      badge: `t = (d * t + text.charAt(${k})) % mod → (${d} * ${prevT} + ${codeK}) % ${mod} → windowHash (t) = ${t}`,
      code: 'c_win_calc',
      vars: [
        frame('main()', []),
        frame(fnLabel, [['i', String(k)], ['windowHash (t)', String(t)], ['char', `'${text[k]}'`]])
      ],
      textCells: getTextCells(-1, false, false, false, k + 1),
      patternCells: getPatternCells(true),
      i: k, patI: -1, patOffset: 0, p, t, h, matches: [], n, m, mod
    });
  }

  // 10. Compare patternHash (p) vs windowHash (t) and slide window
  for (let i = 0; i <= n - m; i++) {
    steps.push({
      badge: `for (int i = ${i}; i <= n - m (${n - m}); i++) → window aligned at text index i=${i}`,
      code: 'c_loop',
      vars: [
        frame('main()', []),
        frame(fnLabel, [['i', String(i)], ['patternHash (p)', String(p)], ['windowHash (t)', String(t)]])
      ],
      textCells: getTextCells(i),
      patternCells: getPatternCells(true),
      i, patI: -1, patOffset: i, p, t, h, matches: [...matches], n, m, mod
    });

    const isMatch = p === t;
    steps.push({
      badge: `if (p == t) → comparing patternHash (${p}) vs windowHash (${t}) at i=${i} ("${text.substring(i, i + m)}") → ${isMatch ? 'HASH MATCH!' : 'HASH MISMATCH'}`,
      code: 'c_compare',
      vars: [
        frame('main()', []),
        frame(fnLabel, [['i', String(i)], ['p', String(p)], ['t', String(t)], ['match', String(isMatch)]])
      ],
      textCells: getTextCells(i, false, isMatch, !isMatch),
      patternCells: getPatternCells(true),
      i, patI: -1, patOffset: i, p, t, h, matches: [...matches], n, m, mod
    });

    if (isMatch) {
      matches.push(i);
      steps.push({
        badge: `matches.add(${i}) → Added match index ${i} to matches`,
        code: 'c_match_add',
        vars: [
          frame('main()', []),
          frame(fnLabel, [['match_added', String(i)], ['matches', JSON.stringify(matches)]])
        ],
        textCells: getTextCells(i, false, true, false),
        patternCells: getPatternCells(true),
        i, patI: -1, patOffset: i, p, t, h, matches: [...matches], n, m, mod
      });
    }

    if (i < n - m) {
      steps.push({
        badge: `if (i < n - m) → ${i} < ${n - m} → TRUE (sliding window to index ${i + 1})`,
        code: 'c_roll_check',
        vars: [
          frame('main()', []),
          frame(fnLabel, [['i', String(i)], ['n-m', String(n - m)]])
        ],
        textCells: getTextCells(i),
        patternCells: getPatternCells(true),
        i, patI: -1, patOffset: i, p, t, h, matches: [...matches], n, m, mod
      });

      const oldT = t;
      t = (d * (t - text.charCodeAt(i) * h) + text.charCodeAt(i + m)) % mod;

      steps.push({
        badge: `t = (d * (t - text[i]*h) + text[i+m]) % mod → Subtract '${text[i]}', add '${text[i + m]}' → t calculated as ${t}`,
        code: 'c_roll_calc',
        vars: [
          frame('main()', []),
          frame(fnLabel, [['i', String(i)], ['old_t', String(oldT)], ['calc_t', String(t)]])
        ],
        textCells: getTextCells(i),
        patternCells: getPatternCells(true),
        i, patI: -1, patOffset: i, p, t, h, matches: [...matches], n, m, mod
      });

      if (t < 0) {
        t += mod;
        steps.push({
          badge: `if (t < 0) → t was negative → added modulus mod (${mod}) → t set to ${t}`,
          code: 'c_roll_mod',
          vars: [
            frame('main()', []),
            frame(fnLabel, [['i', String(i)], ['windowHash (t)', String(t)], ['mod', String(mod)]])
          ],
          textCells: getTextCells(i + 1),
          patternCells: getPatternCells(true),
          i: i + 1, patI: -1, patOffset: i + 1, p, t, h, matches: [...matches], n, m, mod
        });
      }
    }
  }

  // 11. Return matches
  steps.push({
    badge: `return matches → Found ${matches.length} pattern match(es) at indices: [${matches.join(', ')}]`,
    code: 'c_return',
    vars: [
      frame('main()', []),
      frame(fnLabel, [['return', JSON.stringify(matches)]])
    ],
    textCells: getTextCells(),
    patternCells: getPatternCells(),
    i: -1, patI: -1, patOffset: 0, p, t, h, matches: [...matches], n, m, mod
  });

  steps.push({
    badge: `Rolling Hash Search Complete! Found ${matches.length} match(es) at indices: [${matches.join(', ')}]`,
    code: 'c_done',
    vars: [
      frame('main()', []),
      frame(fnLabel, [['result_matches', JSON.stringify(matches)]])
    ],
    textCells: getTextCells(),
    patternCells: getPatternCells(),
    i: -1, patI: -1, patOffset: 0, p, t, h, matches: [...matches], n, m, mod
  });

  return steps;
}

const inpText = ref('DAACABCDBA');
const inpPattern = ref('CAB');
const inpMod = ref(101);
const lang = ref('java');
const speed = ref(650);
const si = ref(0);
const playing = ref(false);
const vizHeight = ref(265);
const tableHeight = ref(60);
const leftWidth = ref(50);
const rightTab = ref('code');

const stepsData = reactive({ steps: buildSteps('DAACABCDBA', 'CAB', 101) });
const steps = computed(() => stepsData.steps);
const s = computed(() => steps.value[Math.max(0, Math.min(si.value, steps.value.length - 1))] || {});
const codeLines = computed(() => CODES[lang.value] || []);

let playTimer = null;

function applyInput() {
  const textStr = String(inpText.value || '').trim();
  const patStr = String(inpPattern.value || '').trim();
  const modVal = Number(inpMod.value) || 101;
  if (!textStr.length || !patStr.length) {
    alert('Please enter non-empty text and pattern strings.');
    return;
  }

  playing.value = false;
  stepsData.steps = buildSteps(textStr, patStr, modVal);
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
            <input type="text" v-model="inpText" style="width: 140px;" class="ll-text-input" placeholder="e.g. DAACABCDBA" />

            <label>Pattern</label>
            <input type="text" v-model="inpPattern" style="width: 90px;" class="ll-text-input" placeholder="e.g. CAB" />

            <label>Mod q</label>
            <input type="number" v-model.number="inpMod" style="width: 60px;" class="ll-num-input" min="1" />

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
                  <div class="ll-hash-tracks">
                    <!-- Text Track -->
                    <div class="ll-track-group">
                      <div class="ll-track-label">Text string:</div>
                      <div class="ll-arr-track">
                        <div v-for="cell in s.textCells" :key="'t-' + cell.idx" class="ll-arr-cell-wrap">
                          <div class="ll-ptr-tag-wrap">
                            <span v-if="cell.idx === s.i && s.i >= 0" class="ll-ptr-lbl ll-lbl-blue">i</span>
                          </div>
                          <div class="ll-arr-box" :class="{ 'll-box-window': cell.state === 'cur_window', 'll-box-winhash': cell.state === 'win_hash', 'll-box-match': cell.state === 'match', 'll-box-mismatch': cell.state === 'mismatch' }">
                            {{ cell.val === ' ' ? '\u2423' : cell.val }}
                          </div>
                          <div class="ll-arr-idx">[{{ cell.idx }}]</div>
                        </div>
                      </div>
                    </div>

                    <!-- Pattern Track -->
                    <div class="ll-track-group" style="margin-top: 10px;">
                      <div class="ll-track-label">Pattern:</div>
                      <div class="ll-arr-track" :style="{ paddingLeft: Math.max(0, (s.patOffset >= 0 ? s.patOffset : 0) * 44) + 'px' }">
                        <div v-for="cell in s.patternCells" :key="'p-' + cell.idx" class="ll-arr-cell-wrap">
                          <div class="ll-ptr-tag-wrap">
                            <span v-if="cell.idx === s.patI && s.patI >= 0" class="ll-ptr-lbl ll-lbl-blue">i</span>
                          </div>
                          <div class="ll-arr-box" :class="{ 'll-box-pathash': cell.state === 'pat_hash' }">
                            {{ cell.val }}
                          </div>
                          <div class="ll-arr-idx">[{{ cell.idx }}]</div>
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
                <span class="ll-leg"><span class="ll-legdot ll-legdot-window"></span>Current Window (text[i..i+m-1])</span>
                <span class="ll-leg"><span class="ll-legdot ll-legdot-winhash"></span>Window Hash Calc</span>
                <span class="ll-leg"><span class="ll-legdot ll-legdot-pathash"></span>Pattern Hash Calc</span>
                <span class="ll-leg"><span class="ll-legdot ll-legdot-match"></span>Match Found (p == t)</span>
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
                      <tr><td>Best / Average Case</td><td>O(n + m)</td><td>Calculates pattern hash and initial window hash in O(m), then slides window in O(1) per step.</td></tr>
                      <tr><td>Worst Case</td><td>O(n &times; m)</td><td>Occurs when spurious hash collisions happen frequently requiring character checks.</td></tr>
                      <tr><td>Space Complexity</td><td>O(1)</td><td>Only a few hash accumulator variables allocated.</td></tr>
                    </tbody>
                  </table>
                  <p class="ll-note">
                    Key observation: <b>Rolling Hash</b> enables O(1) sliding window updates: <code>t = (d * (t - text[i]*h) + text[i+m]) % mod</code>!
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
.ll-viz-wrap { flex-shrink: 0; background: var(--surface); border-bottom: 1px solid var(--border); position: relative; overflow-x: auto; overflow-y: auto; }
.ll-perm-area { display: flex; flex-direction: column; align-items: stretch; min-height: 100%; width: 100%; min-width: 0; box-sizing: border-box; }
.ll-ptrs { display: flex; gap: 8px; flex-wrap: wrap; padding: 10px 16px 4px; min-height: 36px; width: 100%; box-sizing: border-box; min-width: 0; }
.ll-ptr-chip { background: var(--surface2); border: 1px solid var(--border); border-radius: var(--radius-sm); padding: 3px 10px; font-size: 12px; font-family: monospace; box-shadow: var(--shadow-sm); white-space: nowrap; flex-shrink: 0; }
.ll-c-blue { color: var(--blue); } .ll-c-orange { color: var(--orange); } .ll-c-green { color: var(--green); } .ll-c-purple { color: var(--purple); } .ll-c-red { color: var(--red); }

/* Hash Diagram */
.ll-hash-tracks { display: flex; flex-direction: column; gap: 4px; padding: 4px 16px; width: 100%; }
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
.ll-box-winhash { border-color: #3b82f6 !important; background: #dbeafe !important; color: #1e40af !important; transform: translateY(-2px); }
.ll-box-pathash { border-color: #a855f7 !important; background: #faf5ff !important; color: #7e22ce !important; transform: translateY(-2px); }
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
.ll-legdot-winhash { background: #dbeafe; border: 1.5px solid #3b82f6; }
.ll-legdot-pathash { background: #faf5ff; border: 1.5px solid #a855f7; }
.ll-legdot-match { background: #dcfce7; border: 1.5px solid #10b981; }

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
