<script setup>
import { ref, reactive, computed, onMounted, onBeforeUnmount, watch } from 'vue';

defineProps({
  topic: { type: String, default: 'Z Algorithm' },
  subTopic: { type: String, default: 'Linear Time Pattern Search using Z-Array' }
});

const CODES = {
  java: [
    ['', 'class ZAlgorithm {'],
    ['c_entry', '    List<Integer> search(String text, String pattern) {'],
    ['c_concat', '        String concat = pattern + "$" + text;'],
    ['c_m_n', '        int m = pattern.length(), n = concat.length();'],
    ['c_z_init', '        int[] Z = new int[n]; int L = 0, R = 0;'],
    ['c_matches_init', '        List<Integer> matches = new ArrayList<>();'],
    ['', ''],
    ['c_loop', '        for (int i = 1; i < n; i++) {'],
    ['c_out_r_check', '            if (i > R) {'],
    ['c_out_r_set', '                L = R = i;'],
    ['c_out_r_while', '                while (R < n && concat.charAt(R - L) == concat.charAt(R)) R++;'],
    ['c_out_r_store', '                Z[i] = R - L; R--;'],
    ['c_in_r_else', '            } else {'],
    ['c_k_calc', '                int k = i - L;'],
    ['c_z_k_check', '                if (Z[k] < R - i + 1) {'],
    ['c_z_k_copy', '                    Z[i] = Z[k];'],
    ['c_extend_else', '                } else {'],
    ['c_extend_set', '                    L = i;'],
    ['c_extend_while', '                    while (R < n && concat.charAt(R - L) == concat.charAt(R)) R++;'],
    ['c_extend_store', '                    Z[i] = R - L; R--;'],
    ['', '                }'],
    ['', '            }'],
    ['c_match_check', '            if (Z[i] == m) matches.add(i - m - 1);'],
    ['', '        }'],
    ['c_return', '        return matches;'],
    ['c_done', '    }'],
    ['', '}']
  ],
  c: [
    ['c_entry', 'void searchZ(const char* text, const char* pattern) {'],
    ['c_concat', '    char concat[200]; sprintf(concat, "%s$%s", pattern, text);'],
    ['c_m_n', '    int m = strlen(pattern), n = strlen(concat);'],
    ['c_z_init', '    int Z[200] = {0}; int L = 0, R = 0;'],
    ['c_matches_init', '    int matches[100], matchCount = 0;'],
    ['c_loop', '    for (int i = 1; i < n; i++) {'],
    ['c_out_r_check', '        if (i > R) {'],
    ['c_out_r_set', '            L = R = i;'],
    ['c_out_r_while', '            while (R < n && concat[R - L] == concat[R]) R++;'],
    ['c_out_r_store', '            Z[i] = R - L; R--;'],
    ['c_in_r_else', '        } else {'],
    ['c_k_calc', '            int k = i - L;'],
    ['c_z_k_check', '            if (Z[k] < R - i + 1) {'],
    ['c_z_k_copy', '                Z[i] = Z[k];'],
    ['c_extend_else', '            } else {'],
    ['c_extend_set', '                L = i;'],
    ['c_extend_while', '                while (R < n && concat[R - L] == concat[R]) R++;'],
    ['c_extend_store', '                Z[i] = R - L; R--;'],
    ['', '            }'],
    ['', '        }'],
    ['c_match_check', '        if (Z[i] == m) matches[matchCount++] = i - m - 1;'],
    ['', '    }'],
    ['c_done', '}']
  ],
  cpp: [
    ['', 'class ZAlgorithm {'],
    ['', 'public:'],
    ['c_entry', '    vector<int> search(string text, string pattern) {'],
    ['c_concat', '        string concat = pattern + "$" + text;'],
    ['c_m_n', '        int m = pattern.length(), n = concat.length();'],
    ['c_z_init', '        vector<int> Z(n, 0); int L = 0, R = 0;'],
    ['c_matches_init', '        vector<int> matches;'],
    ['c_loop', '        for (int i = 1; i < n; i++) {'],
    ['c_out_r_check', '            if (i > R) {'],
    ['c_out_r_set', '                L = R = i;'],
    ['c_out_r_while', '                while (R < n && concat[R - L] == concat[R]) R++;'],
    ['c_out_r_store', '                Z[i] = R - L; R--;'],
    ['c_in_r_else', '            } else {'],
    ['c_k_calc', '                int k = i - L;'],
    ['c_z_k_check', '                if (Z[k] < R - i + 1) {'],
    ['c_z_k_copy', '                    Z[i] = Z[k];'],
    ['c_extend_else', '                } else {'],
    ['c_extend_set', '                    L = i;'],
    ['c_extend_while', '                    while (R < n && concat[R - L] == concat[R]) R++;'],
    ['c_extend_store', '                    Z[i] = R - L; R--;'],
    ['', '                }'],
    ['', '            }'],
    ['c_match_check', '            if (Z[i] == m) matches.push_back(i - m - 1);'],
    ['', '        }'],
    ['c_return', '        return matches;'],
    ['c_done', '    }'],
    ['', '};']
  ],
  python: [
    ['', 'class ZAlgorithm:'],
    ['c_entry', '    def search(self, text, pattern):'],
    ['c_concat', '        concat = pattern + "$" + text'],
    ['c_m_n', '        m, n = len(pattern), len(concat)'],
    ['c_z_init', '        Z = [0] * n; L = R = 0'],
    ['c_matches_init', '        matches = []'],
    ['c_loop', '        for i in range(1, n):'],
    ['c_out_r_check', '            if i > R:'],
    ['c_out_r_set', '                L = R = i'],
    ['c_out_r_while', '                while R < n and concat[R - L] == concat[R]: R += 1'],
    ['c_out_r_store', '                Z[i] = R - L; R -= 1'],
    ['c_in_r_else', '            else:'],
    ['c_k_calc', '                k = i - L'],
    ['c_z_k_check', '                if Z[k] < R - i + 1:'],
    ['c_z_k_copy', '                    Z[i] = Z[k]'],
    ['c_extend_else', '                else:'],
    ['c_extend_set', '                    L = i'],
    ['c_extend_while', '                    while R < n and concat[R - L] == concat[R]: R += 1'],
    ['c_extend_store', '                    Z[i] = R - L; R -= 1'],
    ['c_match_check', '            if Z[i] == m: matches.append(i - m - 1)'],
    ['c_return', '        return matches'],
    ['c_done', '']
  ],
  javascript: [
    ['', 'class ZAlgorithm {'],
    ['c_entry', '  search(text, pattern) {'],
    ['c_concat', '    const concat = pattern + "$" + text;'],
    ['c_m_n', '    const m = pattern.length, n = concat.length;'],
    ['c_z_init', '    const Z = new Array(n).fill(0); let L = 0, R = 0;'],
    ['c_matches_init', '    const matches = [];'],
    ['c_loop', '    for (let i = 1; i < n; i++) {'],
    ['c_out_r_check', '      if (i > R) {'],
    ['c_out_r_set', '        L = R = i;'],
    ['c_out_r_while', '        while (R < n && concat[R - L] === concat[R]) R++;'],
    ['c_out_r_store', '        Z[i] = R - L; R--;'],
    ['c_in_r_else', '      } else {'],
    ['c_k_calc', '        let k = i - L;'],
    ['c_z_k_check', '        if (Z[k] < R - i + 1) {'],
    ['c_z_k_copy', '          Z[i] = Z[k];'],
    ['c_extend_else', '        } else {'],
    ['c_extend_set', '          L = i;'],
    ['c_extend_while', '          while (R < n && concat[R - L] === concat[R]) R++;'],
    ['c_extend_store', '          Z[i] = R - L; R--;'],
    ['', '        }'],
    ['', '      }'],
    ['c_match_check', '      if (Z[i] === m) matches.push(i - m - 1);'],
    ['', '    }'],
    ['c_return', '    return matches;'],
    ['c_done', '  }'],
    ['', '}']
  ]
};

const PSEUDOCODE = [
  'function ZSearch(text, pattern):',
  '    concat = pattern + "$" + text',
  '    m = length(pattern), n = length(concat)',
  '    Z = array of size n (0), L = 0, R = 0',
  '    matches = []',
  '    for i = 1 to n - 1:',
  '        if i > R:',
  '            L = R = i',
  '            while R < n and concat[R - L] == concat[R]: R++',
  '            Z[i] = R - L; R--',
  '        else:',
  '            k = i - L',
  '            if Z[k] < R - i + 1: Z[i] = Z[k]',
  '            else:',
  '                L = i',
  '                while R < n and concat[R - L] == concat[R]: R++',
  '                Z[i] = R - L; R--',
  '        if Z[i] == m: matches.append(i - m - 1)',
  '    return matches',
];

function frame(title, rows) { return { title, rows }; }

function buildSteps(initialText, initialPattern) {
  const steps = [];
  const text = String(initialText).trim();
  const pattern = String(initialPattern).trim();
  const fnLabel = 'search(text, pattern)';

  const concat = pattern + '$' + text;
  const m = pattern.length;
  const n = concat.length;
  const Z = new Array(n).fill(null);
  const matches = [];

  function getCells(curI = -1, curL = -1, curR = -1, prefixIdx = -1, suffixIdx = -1, isMatch = false, isMismatch = false) {
    const textCells = concat.split('').map((val, idx) => {
      let state = 'normal';
      if (idx < m) {
        state = 'pattern_part';
      } else if (idx === m) {
        state = 'delim_part';
      }

      if (idx === prefixIdx || idx === suffixIdx) state = isMismatch ? 'mismatch' : (isMatch ? 'match' : 'cur_i');
      else if (idx === curI) state = 'cur_i';
      else if (curL >= 0 && curR >= 0 && idx >= curL && idx <= curR) state = 'z_box';

      return { idx, val, state };
    });

    const zCells = Z.map((val, idx) => {
      let state = 'normal';
      if (val === m && idx > m) state = 'full_match';
      else if (idx === curI) state = 'cur_i';
      else if (curL >= 0 && curR >= 0 && idx >= curL && idx <= curR) state = 'z_box';
      return { idx, val: val !== null ? val : '-', state };
    });

    return { textCells, zCells };
  }

  // 1. Entry
  steps.push({
    badge: `ZSearch called with text="${text}" and pattern="${pattern}"`,
    code: 'c_entry',
    vars: [
      frame('main()', []),
      frame(fnLabel, [['text', `"${text}"`], ['pattern', `"${pattern}"`]])
    ],
    ...getCells(),
    i: -1, L: 0, R: 0, k: -1, matches: [], m, n
  });

  if (n === 0 || m === 0) {
    steps.push({
      badge: `Empty text or pattern → return []`,
      code: 'c_return',
      vars: [frame('main()', []), frame(fnLabel, [['return', '[]']])],
      ...getCells(),
      i: -1, L: 0, R: 0, k: -1, matches: [], m, n
    });
    return steps;
  }

  // 2. String concat = pattern + "$" + text
  steps.push({
    badge: `String concat = pattern + "$" + text → concat = "${concat}"`,
    code: 'c_concat',
    vars: [
      frame('main()', []),
      frame(fnLabel, [['concat', `"${concat}"`]])
    ],
    ...getCells(),
    i: -1, L: 0, R: 0, k: -1, matches: [], m, n
  });

  // 3. int m = pattern.length(), n = concat.length()
  steps.push({
    badge: `int m = pattern.length() (${m}), n = concat.length() (${n})`,
    code: 'c_m_n',
    vars: [
      frame('main()', []),
      frame(fnLabel, [['m', String(m)], ['n', String(n)], ['concat', `"${concat}"`]])
    ],
    ...getCells(),
    i: -1, L: 0, R: 0, k: -1, matches: [], m, n
  });

  // 4. int[] Z = new int[n]; Z[0] = 0; int L = 0, R = 0;
  Z[0] = 0;
  let L = 0, R = 0;
  steps.push({
    badge: `int[] Z = new int[${n}]; Z[0] = 0; int L = 0, R = 0;`,
    code: 'c_z_init',
    vars: [
      frame('main()', []),
      frame(fnLabel, [['L', '0'], ['R', '0'], ['Z[0]', '0']])
    ],
    ...getCells(0, L, R),
    i: 0, L, R, k: -1, matches: [], m, n
  });

  // 5. List<Integer> matches = new ArrayList<>()
  steps.push({
    badge: `List<Integer> matches = new ArrayList<>() → Initialized empty matches list`,
    code: 'c_matches_init',
    vars: [
      frame('main()', []),
      frame(fnLabel, [['matches', '[]'], ['L', '0'], ['R', '0']])
    ],
    ...getCells(0, L, R),
    i: 0, L, R, k: -1, matches: [], m, n
  });

  // 6. Main loop i = 1 to n - 1
  for (let i = 1; i < n; i++) {
    steps.push({
      badge: `for (int i = ${i}; i < ${n}; i++) → computing Z[${i}] for concat['${concat[i]}']`,
      code: 'c_loop',
      vars: [
        frame('main()', []),
        frame(fnLabel, [['i', String(i)], ['L', String(L)], ['R', String(R)], ['matches', JSON.stringify(matches)]])
      ],
      ...getCells(i, L, R),
      i, L, R, k: -1, matches: [...matches], m, n
    });

    if (i > R) {
      steps.push({
        badge: `if (i > R) → ${i} > ${R} → TRUE (i is outside current Z-box [${L}, ${R}])`,
        code: 'c_out_r_check',
        vars: [
          frame('main()', []),
          frame(fnLabel, [['i', String(i)], ['R', String(R)]])
        ],
        ...getCells(i, L, R),
        i, L, R, k: -1, matches: [...matches], m, n
      });

      L = R = i;
      steps.push({
        badge: `L = R = i → reset Z-box window L = ${i}, R = ${i}`,
        code: 'c_out_r_set',
        vars: [
          frame('main()', []),
          frame(fnLabel, [['L', String(L)], ['R', String(R)]])
        ],
        ...getCells(i, L, R),
        i, L, R, k: -1, matches: [...matches], m, n
      });

      while (R < n && concat[R - L] === concat[R]) {
        steps.push({
          badge: `while (R < n && concat[R-L] == concat[R]) → comparing prefix concat[${R - L}] ('${concat[R - L]}') vs concat[${R}] ('${concat[R]}') → MATCH!`,
          code: 'c_out_r_while',
          vars: [
            frame('main()', []),
            frame(fnLabel, [['R-L', String(R - L)], ['R', String(R)], ['prefix_char', `'${concat[R - L]}'`]])
          ],
          ...getCells(i, L, R, R - L, R, true, false),
          i, L, R, k: -1, matches: [...matches], m, n
        });
        R++;
      }

      if (R < n) {
        steps.push({
          badge: `Comparing prefix concat[${R - L}] ('${concat[R - L]}') vs concat[${R}] ('${concat[R]}') → MISMATCH! Stop extending R`,
          code: 'c_out_r_while',
          vars: [
            frame('main()', []),
            frame(fnLabel, [['R-L', String(R - L)], ['R', String(R)], ['mismatch', `'${concat[R - L]}' != '${concat[R]}'`]])
          ],
          ...getCells(i, L, R, R - L, R, false, true),
          i, L, R, k: -1, matches: [...matches], m, n
        });
      }

      Z[i] = R - L;
      const computedZ = Z[i];
      R--;
      steps.push({
        badge: `Z[${i}] = R - L = ${computedZ}; R-- (${R}) → Z-box set to [${L}, ${R}]`,
        code: 'c_out_r_store',
        vars: [
          frame('main()', []),
          frame(fnLabel, [[`Z[${i}]`, String(computedZ)], ['L', String(L)], ['R', String(R)]])
        ],
        ...getCells(i, L, R),
        i, L, R, k: -1, matches: [...matches], m, n
      });
    } else {
      steps.push({
        badge: `else → i (${i}) <= R (${R}) → i is inside Z-box [${L}, ${R}]`,
        code: 'c_in_r_else',
        vars: [
          frame('main()', []),
          frame(fnLabel, [['i', String(i)], ['L', String(L)], ['R', String(R)]])
        ],
        ...getCells(i, L, R),
        i, L, R, k: -1, matches: [...matches], m, n
      });

      const k = i - L;
      steps.push({
        badge: `int k = i - L → k = ${i} - ${L} = ${k} (corresponding prefix index inside Z-box)`,
        code: 'c_k_calc',
        vars: [
          frame('main()', []),
          frame(fnLabel, [['k', String(k)], ['Z[k]', String(Z[k])]])
        ],
        ...getCells(i, L, R),
        i, L, R, k, matches: [...matches], m, n
      });

      const remLen = R - i + 1;
      const isSmaller = Z[k] < remLen;
      steps.push({
        badge: `if (Z[k] < R - i + 1) → checking Z[${k}] (${Z[k]}) < ${remLen} → ${isSmaller ? 'TRUE (reuse Z[k])' : 'FALSE (must extend Z-box)'}`,
        code: 'c_z_k_check',
        vars: [
          frame('main()', []),
          frame(fnLabel, [[`Z[${k}]`, String(Z[k])], ['R-i+1', String(remLen)]])
        ],
        ...getCells(i, L, R),
        i, L, R, k, matches: [...matches], m, n
      });

      if (isSmaller) {
        Z[i] = Z[k];
        steps.push({
          badge: `Z[${i}] = Z[${k}] (${Z[k]}) → Directly copied value inside Z-box boundaries without character comparisons!`,
          code: 'c_z_k_copy',
          vars: [
            frame('main()', []),
            frame(fnLabel, [[`Z[${i}]`, String(Z[i])], ['copied_from_Z_k', String(Z[k])]])
          ],
          ...getCells(i, L, R),
          i, L, R, k, matches: [...matches], m, n
        });
      } else {
        steps.push({
          badge: `else → Z-box boundary reached, expanding Z-box beyond R (${R})...`,
          code: 'c_extend_else',
          vars: [
            frame('main()', []),
            frame(fnLabel, [['i', String(i)], ['L', String(L)], ['R', String(R)]])
          ],
          ...getCells(i, L, R),
          i, L, R, k, matches: [...matches], m, n
        });

        L = i;
        steps.push({
          badge: `L = i → reset Z-box start L to ${i}`,
          code: 'c_extend_set',
          vars: [
            frame('main()', []),
            frame(fnLabel, [['L', String(L)], ['R', String(R)]])
          ],
          ...getCells(i, L, R),
          i, L, R, k, matches: [...matches], m, n
        });

        while (R < n && concat[R - L] === concat[R]) {
          steps.push({
            badge: `while (R < n && concat[R-L] == concat[R]) → comparing prefix concat[${R - L}] ('${concat[R - L]}') vs concat[${R}] ('${concat[R]}') → MATCH!`,
            code: 'c_extend_while',
            vars: [
              frame('main()', []),
              frame(fnLabel, [['R-L', String(R - L)], ['R', String(R)], ['prefix_char', `'${concat[R - L]}'`]])
            ],
            ...getCells(i, L, R, R - L, R, true, false),
            i, L, R, k, matches: [...matches], m, n
          });
          R++;
        }

        if (R < n) {
          steps.push({
            badge: `Comparing prefix concat[${R - L}] ('${concat[R - L]}') vs concat[${R}] ('${concat[R]}') → MISMATCH! Stop extending R`,
            code: 'c_extend_while',
            vars: [
              frame('main()', []),
              frame(fnLabel, [['R-L', String(R - L)], ['R', String(R)], ['mismatch', `'${concat[R - L]}' != '${concat[R]}'`]])
            ],
            ...getCells(i, L, R, R - L, R, false, true),
            i, L, R, k, matches: [...matches], m, n
          });
        }

        Z[i] = R - L;
        const computedZ = Z[i];
        R--;
        steps.push({
          badge: `Z[${i}] = R - L = ${computedZ}; R-- (${R}) → Z-box set to [${L}, ${R}]`,
          code: 'c_extend_store',
          vars: [
            frame('main()', []),
            frame(fnLabel, [[`Z[${i}]`, String(computedZ)], ['L', String(L)], ['R', String(R)]])
          ],
          ...getCells(i, L, R),
          i, L, R, k, matches: [...matches], m, n
        });
      }
    }

    // Check if pattern match is found
    if (Z[i] === m && i > m) {
      const matchTextIdx = i - m - 1;
      matches.push(matchTextIdx);
      steps.push({
        badge: `if (Z[${i}] == m (${m})) → Z[${i}] equals pattern length! Pattern match found at text index ${matchTextIdx} (concat index ${i})`,
        code: 'c_match_check',
        vars: [
          frame('main()', []),
          frame(fnLabel, [[`Z[${i}]`, String(Z[i])], ['match_text_index', String(matchTextIdx)], ['matches', JSON.stringify(matches)]])
        ],
        ...getCells(i, L, R),
        i, L, R, k: -1, matches: [...matches], m, n
      });
    }
  }

  // 7. Return matches
  steps.push({
    badge: `return matches → Found ${matches.length} pattern match(es) at text indices: [${matches.join(', ')}]`,
    code: 'c_return',
    vars: [
      frame('main()', []),
      frame(fnLabel, [['return', JSON.stringify(matches)]])
    ],
    ...getCells(),
    i: n, L, R, k: -1, matches: [...matches], m, n
  });

  steps.push({
    badge: `Z Algorithm Search Complete! Pattern "${pattern}" found ${matches.length} time(s) at text indices: [${matches.join(', ')}]`,
    code: 'c_done',
    vars: [
      frame('main()', []),
      frame(fnLabel, [['result_matches', JSON.stringify(matches)]])
    ],
    ...getCells(),
    i: n, L, R, k: -1, matches: [...matches], m, n
  });

  return steps;
}

const inpText = ref('aabzaab');
const inpPattern = ref('ab');
const lang = ref('java');
const speed = ref(650);
const si = ref(0);
const playing = ref(false);
const vizHeight = ref(265);
const tableHeight = ref(60);
const leftWidth = ref(50);
const rightTab = ref('code');

const stepsData = reactive({ steps: buildSteps('aabzaab', 'ab') });
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

function cellLabel(cellIdx) {
  const tags = [];
  if (s.value.i === cellIdx) tags.push('i');
  if (s.value.L === cellIdx) tags.push('L');
  if (s.value.R === cellIdx) tags.push('R');
  if (tags.length) return tags.join(', ');
  return '';
}

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
            <input type="text" v-model="inpText" style="width: 150px;" class="ll-text-input" placeholder="e.g. aabzaab" />

            <label>Pattern</label>
            <input type="text" v-model="inpPattern" style="width: 90px;" class="ll-text-input" placeholder="e.g. ab" />

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
                    <div class="ll-ptr-chip">i = <b class="ll-c-blue">{{ s.i >= 0 ? s.i : 'N/A' }}</b></div>
                    <div class="ll-ptr-chip">Z-Box [L, R] = <b class="ll-c-orange">[{{ s.L }}, {{ s.R }}]</b></div>
                    <div class="ll-ptr-chip">k = i - L = <b class="ll-c-purple">{{ s.k >= 0 ? s.k : 'N/A' }}</b></div>
                    <div class="ll-ptr-chip">Matches = <b class="ll-c-green">[{{ (s.matches || []).join(', ') }}]</b></div>
                  </div>

                  <!-- Text & Z Array Tracks -->
                  <div class="ll-z-tracks">
                    <div class="ll-track-group">
                      <div class="ll-track-label">Concatenated String (pattern + "$" + text):</div>
                      <div class="ll-arr-track">
                        <div v-for="cell in s.textCells" :key="'t-' + cell.idx" class="ll-arr-cell-wrap">
                          <div class="ll-ptr-tag-wrap">
                            <span v-if="cell.idx === s.i" class="ll-ptr-lbl ll-lbl-blue">i</span>
                            <span v-if="cell.idx === s.L && s.L >= 0" class="ll-ptr-lbl ll-lbl-orange">L</span>
                            <span v-if="cell.idx === s.R && s.R >= 0" class="ll-ptr-lbl ll-lbl-orange">R</span>
                          </div>
                          <div class="ll-arr-box" :class="{ 'll-box-pat': cell.state === 'pattern_part', 'll-box-delim': cell.state === 'delim_part', 'll-box-i': cell.state === 'cur_i', 'll-box-zbox': cell.state === 'z_box', 'll-box-match': cell.state === 'match', 'll-box-mismatch': cell.state === 'mismatch' }">
                            {{ cell.val }}
                          </div>
                          <div class="ll-arr-idx">[{{ cell.idx }}]</div>
                        </div>
                      </div>
                    </div>

                    <div class="ll-track-group" style="margin-top: 10px;">
                      <div class="ll-track-label">Z Array (Z[i]):</div>
                      <div class="ll-arr-track">
                        <div v-for="cell in s.zCells" :key="'z-' + cell.idx" class="ll-arr-cell-wrap">
                          <div class="ll-arr-box ll-box-z" :class="{ 'll-box-fullmatch': cell.state === 'full_match', 'll-box-i': cell.state === 'cur_i', 'll-box-zbox': cell.state === 'z_box' }">
                            {{ cell.val }}
                          </div>
                          <div class="ll-arr-idx">Z[{{ cell.idx }}]</div>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
              </div>

              <div class="ll-vresizer" ref="vizResizerRef"></div>

              <!-- Color Legend -->
              <div class="ll-legend">
                <span class="ll-leg"><span class="ll-legdot ll-legdot-pattern"></span>Pattern</span>
                <span class="ll-leg"><span class="ll-legdot ll-legdot-delim"></span>Delimiter ($)</span>
                <span class="ll-leg"><span class="ll-legdot ll-legdot-cur-i"></span>i Pointer</span>
                <span class="ll-leg"><span class="ll-legdot ll-legdot-zbox"></span>Z-Box [L, R]</span>
                <span class="ll-leg"><span class="ll-legdot ll-legdot-match"></span>Match (Z[i] == m)</span>
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
                      <tr><td>Best Case</td><td>O(n + m)</td><td>Linear construction time on concatenated string <code>pattern + "$" + text</code>.</td></tr>
                      <tr><td>Average Case</td><td>O(n + m)</td><td>Reuses previously computed Z[k] values inside Z-box [L, R].</td></tr>
                      <tr><td>Worst Case</td><td>O(n + m)</td><td>Linear time; R boundary only moves to the right.</td></tr>
                      <tr><td>Space</td><td>O(n + m)</td><td>Auxiliary array for Z values of concatenated string.</td></tr>
                    </tbody>
                  </table>
                  <p class="ll-note">
                    Key observation: <b>Z Algorithm</b> constructs <code>concat = pattern + "$" + text</code>. Wherever <code>Z[i] == pattern.length()</code> (for <code>i &gt; m</code>), a full pattern match is found at <code>text</code> index <code>i - pattern.length() - 1</code>!
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

/* Z Diagram */
.ll-z-tracks { display: flex; flex-direction: column; gap: 4px; padding: 4px 16px; width: 100%; }
.ll-track-group { display: flex; flex-direction: column; gap: 2px; }
.ll-track-label { font-size: 11px; font-weight: 700; color: var(--text2); font-family: monospace; }
.ll-arr-track { display: flex; align-items: flex-start; gap: 6px; width: 100%; }
.ll-ptr-tag-wrap { height: 28px; display: flex; align-items: flex-end; justify-content: center; gap: 4px; margin-bottom: 2px; }
.ll-ptr-lbl { font-size: 13px; font-weight: 800; font-family: 'Consolas', 'Fira Code', monospace; display: inline-flex; flex-direction: column; align-items: center; line-height: 1; gap: 1px; white-space: nowrap; animation: ll-pop 0.2s ease; }
.ll-ptr-lbl::after { content: '↓'; font-size: 11px; font-weight: 900; line-height: 1; margin-top: 1px; }
.ll-lbl-blue { color: #3b82f6; }
.ll-lbl-orange { color: #f97316; }
.ll-lbl-purple { color: #9333ea; }
.ll-lbl-green { color: #10b981; }
.ll-arr-box { width: 38px; height: 38px; display: flex; align-items: center; justify-content: center; border: 2px solid var(--blue); border-radius: var(--radius-sm); background: #eff6ff; color: #1e293b; font-weight: 700; font-size: 15px; font-family: monospace; box-shadow: var(--shadow-sm); transition: all 0.25s ease; animation: ll-pop .3s ease; }
.ll-box-pat { border-color: #a855f7 !important; background: #faf5ff !important; color: #7e22ce !important; }
.ll-box-delim { border-color: #64748b !important; background: #f1f5f9 !important; color: #334155 !important; font-weight: 800; }
.ll-box-z { border-color: #cbd5e1; background: #f8fafc; }
.ll-box-i { border-color: #3b82f6 !important; background: #dbeafe !important; color: #1e40af !important; transform: translateY(-2px); }
.ll-box-zbox { border-color: #f59e0b !important; background: #fef3c7 !important; color: #92400e !important; transform: translateY(-2px); }
.ll-box-match { border-color: #10b981 !important; background: #dcfce7 !important; color: #065f46 !important; transform: translateY(-2px); }
.ll-box-fullmatch { border-color: #10b981 !important; background: #dcfce7 !important; color: #065f46 !important; font-weight: 800; transform: translateY(-2px); }
.ll-box-mismatch { border-color: #ef4444 !important; background: #fef2f2 !important; color: #991b1b !important; transform: translateY(-2px); }

.ll-arr-idx { font-size: 10px; color: var(--muted); margin-top: 2px; font-family: 'Consolas', monospace; font-weight: 600; }
.ll-vresizer { height: 5px; cursor: row-resize; background: var(--border); flex-shrink: 0; transition: background .15s; position: relative; z-index: 20; }
.ll-vresizer:hover, .ll-vresizer.drag { background: var(--coral); }
.ll-legend { display: flex; flex-wrap: wrap; gap: 6px 14px; padding: 6px 12px; border-bottom: 1px solid var(--border); flex-shrink: 0; background: var(--surface2); }
.ll-leg { display: flex; align-items: center; gap: 5px; font-size: 11px; color: var(--text2); font-weight: 500; }
.ll-legdot { width: 11px; height: 11px; border-radius: 3px; flex-shrink: 0; display: inline-block; }
.ll-legdot-pattern { background: #faf5ff; border: 1.5px solid #a855f7; }
.ll-legdot-delim { background: #f1f5f9; border: 1.5px solid #64748b; }
.ll-legdot-cur-i { background: #dbeafe; border: 1.5px solid #3b82f6; }
.ll-legdot-zbox { background: #fef3c7; border: 1.5px solid #f59e0b; }
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
