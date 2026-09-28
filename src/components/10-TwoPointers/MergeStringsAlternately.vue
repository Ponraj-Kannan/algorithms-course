<script setup>
import { ref, reactive, computed, onMounted, onBeforeUnmount, watch, nextTick } from 'vue';

defineProps({
  topic:    { type: String, default: 'String Algorithms' },
  subTopic: { type: String, default: 'Merge Strings Alternately (LeetCode 1768)' }
});

// ─── Multi-Language Code Definitions ─────────────────────────────────────────
const CODES = {
  java: [
    ['',            'import java.util.Scanner;'],
    ['',            ''],
    ['',            'class Solution {'],
    ['fn_entry',    '    public String mergeAlternately(String word1, String word2) {'],
    ['init_i',      '        int i = 0;'],
    ['init_j',      '        int j = 0;'],
    ['init_result', '        StringBuilder result = new StringBuilder();'],
    ['while1_cond', '        while (i < word1.length() && j < word2.length()) {'],
    ['append_w1',   '            result.append(word1.charAt(i));'],
    ['inc_i',       '            i++;'],
    ['append_w2',   '            result.append(word2.charAt(j));'],
    ['inc_j',       '            j++;'],
    ['',            '        }'],
    ['while2_cond', '        while (i < word1.length()) {'],
    ['append_rem1', '            result.append(word1.charAt(i));'],
    ['inc_i2',      '            i++;'],
    ['',            '        }'],
    ['while3_cond', '        while (j < word2.length()) {'],
    ['append_rem2', '            result.append(word2.charAt(j));'],
    ['inc_j2',      '            j++;'],
    ['',            '        }'],
    ['return_stmt', '        return result.toString();'],
    ['',            '    }'],
    ['',            ''],
    ['',            '    public static void main(String[] args) {'],
    ['m_scanner',   '        Scanner sc = new Scanner(System.in);'],
    ['m_read_w1',   '        String word1 = sc.nextLine();'],
    ['m_read_w2',   '        String word2 = sc.nextLine();'],
    ['m_call',      '        System.out.println(new Solution().mergeAlternately(word1, word2));'],
    ['',            '    }'],
    ['',            '}']
  ],
  cpp: [
    ['',            '#include <string>'],
    ['',            '#include <iostream>'],
    ['',            'using namespace std;'],
    ['',            ''],
    ['',            'class Solution {'],
    ['',            'public:'],
    ['fn_entry',    '    string mergeAlternately(string word1, string word2) {'],
    ['init_i',      '        int i = 0;'],
    ['init_j',      '        int j = 0;'],
    ['init_result', '        string result = "";'],
    ['while1_cond', '        while (i < (int)word1.size() && j < (int)word2.size()) {'],
    ['append_w1',   '            result += word1[i];'],
    ['inc_i',       '            i++;'],
    ['append_w2',   '            result += word2[j];'],
    ['inc_j',       '            j++;'],
    ['',            '        }'],
    ['while2_cond', '        while (i < (int)word1.size()) {'],
    ['append_rem1', '            result += word1[i];'],
    ['inc_i2',      '            i++;'],
    ['',            '        }'],
    ['while3_cond', '        while (j < (int)word2.size()) {'],
    ['append_rem2', '            result += word2[j];'],
    ['inc_j2',      '            j++;'],
    ['',            '        }'],
    ['return_stmt', '        return result;'],
    ['',            '    }'],
    ['',            '};'],
    ['',            ''],
    ['',            'int main() {'],
    ['m_read_w1',   '    string word1;'],
    ['m_read_w2',   '    string word2;'],
    ['m_scanner',   '    cin >> word1 >> word2;'],
    ['m_call',      '    cout << Solution().mergeAlternately(word1, word2) << endl;'],
    ['',            '    return 0;'],
    ['',            '}']
  ],
  python: [
    ['',            'class Solution:'],
    ['fn_entry',    '    def mergeAlternately(self, word1, word2):'],
    ['init_i',      '        i = 0'],
    ['init_j',      '        j = 0'],
    ['init_result', '        result = []'],
    ['while1_cond', '        while i < len(word1) and j < len(word2):'],
    ['append_w1',   '            result.append(word1[i])'],
    ['inc_i',       '            i += 1'],
    ['append_w2',   '            result.append(word2[j])'],
    ['inc_j',       '            j += 1'],
    ['while2_cond', '        while i < len(word1):'],
    ['append_rem1', '            result.append(word1[i])'],
    ['inc_i2',      '            i += 1'],
    ['while3_cond', '        while j < len(word2):'],
    ['append_rem2', '            result.append(word2[j])'],
    ['inc_j2',      '            j += 1'],
    ['return_stmt', '        return "".join(result)'],
    ['',            ''],
    ['',            'def main():'],
    ['m_read_w1',   '    word1 = input()'],
    ['m_read_w2',   '    word2 = input()'],
    ['m_call',      '    print(Solution().mergeAlternately(word1, word2))'],
    ['',            ''],
    ['',            'if __name__ == "__main__":'],
    ['',            '    main()']
  ],
  javascript: [
    ['',            '/**'],
    ['',            ' * @param {string} word1'],
    ['',            ' * @param {string} word2'],
    ['',            ' * @return {string}'],
    ['',            ' */'],
    ['fn_entry',    'var mergeAlternately = function(word1, word2) {'],
    ['init_i',      '    let i = 0;'],
    ['init_j',      '    let j = 0;'],
    ['init_result', '    let result = "";'],
    ['while1_cond', '    while (i < word1.length && j < word2.length) {'],
    ['append_w1',   '        result += word1[i];'],
    ['inc_i',       '        i++;'],
    ['append_w2',   '        result += word2[j];'],
    ['inc_j',       '        j++;'],
    ['',            '    }'],
    ['while2_cond', '    while (i < word1.length) {'],
    ['append_rem1', '        result += word1[i];'],
    ['inc_i2',      '        i++;'],
    ['',            '    }'],
    ['while3_cond', '    while (j < word2.length) {'],
    ['append_rem2', '        result += word2[j];'],
    ['inc_j2',      '        j++;'],
    ['',            '    }'],
    ['return_stmt', '    return result;'],
    ['',            '};'],
    ['',            ''],
    ['',            'function main() {'],
    ['m_read_w1',   '    const word1 = lines[0];'],
    ['m_read_w2',   '    const word2 = lines[1];'],
    ['m_call',      '    console.log(mergeAlternately(word1, word2));'],
    ['',            '}'],
    ['',            'main();']
  ],
  c: [
    ['',            '#include <stdio.h>'],
    ['',            '#include <string.h>'],
    ['',            ''],
    ['fn_entry',    'void mergeAlternately(char* w1, char* w2, char* result) {'],
    ['init_i',      '    int i = 0;'],
    ['init_j',      '    int j = 0;'],
    ['init_result', '    int k = 0;'],
    ['while1_cond', '    while (i < (int)strlen(w1) && j < (int)strlen(w2)) {'],
    ['append_w1',   '        result[k] = w1[i];'],
    ['inc_i',       '        k++; i++;'],
    ['append_w2',   '        result[k] = w2[j];'],
    ['inc_j',       '        k++; j++;'],
    ['',            '    }'],
    ['while2_cond', '    while (i < (int)strlen(w1)) {'],
    ['append_rem1', '        result[k] = w1[i];'],
    ['inc_i2',      '        k++; i++;'],
    ['',            '    }'],
    ['while3_cond', '    while (j < (int)strlen(w2)) {'],
    ['append_rem2', '        result[k] = w2[j];'],
    ['inc_j2',      '        k++; j++;'],
    ['',            '    }'],
    ['return_stmt', '    result[k] = \'\\0\';'],
    ['',            '}'],
    ['',            ''],
    ['',            'int main() {'],
    ['m_read_w1',   '    char word1[200];'],
    ['m_read_w2',   '    char word2[200];'],
    ['m_scanner',   '    scanf("%s %s", word1, word2);'],
    ['m_call',      '    char result[400];'],
    ['',            '    mergeAlternately(word1, word2, result);'],
    ['',            '    printf("%s\\n", result);'],
    ['',            '    return 0;'],
    ['',            '}']
  ]
};

const PSEUDOCODE = [
  'function mergeAlternately(word1, word2):',
  '    i      = 0',
  '    j      = 0',
  '    result = ""',
  '',
  '    // Interleave characters from both strings',
  '    while i < len(word1) and j < len(word2):',
  '        result += word1[i]',
  '        i += 1',
  '        result += word2[j]',
  '        j += 1',
  '',
  '    // Append any remaining from word1',
  '    while i < len(word1):',
  '        result += word1[i]',
  '        i += 1',
  '',
  '    // Append any remaining from word2',
  '    while j < len(word2):',
  '        result += word2[j]',
  '        j += 1',
  '',
  '    return result'
];

// ─── Step Builder ─────────────────────────────────────────────────────────────
function buildSteps(word1raw, word2raw) {
  const steps = [];
  const word1 = word1raw.slice(0, 15);
  const word2 = word2raw.slice(0, 15);
  const n1 = word1.length;
  const n2 = word2.length;

  const NONE = { iReady: false, jReady: false, resultReady: false };
  const ALL  = { iReady: true,  jReady: true,  resultReady: true  };

  function snap(iv, jv, resultStr, resultSrcs, code, phase, initState, extra) {
    return {
      i: iv,
      j: jv,
      resultStr,
      resultSrcs: [...resultSrcs],
      word1,
      word2,
      n1,
      n2,
      code,
      phase,
      initState: { ...initState },
      ...extra
    };
  }

  let resultSrcs = [];

  // ── Phase 1: main() ────────────────────────────────────────────────────────
  const NONE_SRCS = [];

  steps.push({
    ...snap(-1, -1, '', NONE_SRCS, 'm_scanner', 'input', NONE, {}),
    badge: `Scanner sc = new Scanner(System.in); → Initializing input stream in main().`
  });
  steps.push({
    ...snap(-1, -1, '', NONE_SRCS, 'm_read_w1', 'input', NONE, {}),
    badge: `String word1 = sc.nextLine(); → Read word1 = "${word1}" (length ${n1}).`
  });
  steps.push({
    ...snap(-1, -1, '', NONE_SRCS, 'm_read_w2', 'input', NONE, {}),
    badge: `String word2 = sc.nextLine(); → Read word2 = "${word2}" (length ${n2}).`
  });
  steps.push({
    ...snap(-1, -1, '', NONE_SRCS, 'm_call', 'input', NONE, {}),
    badge: `System.out.println(new Solution().mergeAlternately(word1, word2)); → Calling mergeAlternately().`
  });

  // ── Phase 2: mergeAlternately() ───────────────────────────────────────────
  const INIT_ENTRY  = { iReady: false, jReady: false, resultReady: false };
  const AFTER_I     = { iReady: true,  jReady: false, resultReady: false };
  const AFTER_J     = { iReady: true,  jReady: true,  resultReady: false };

  steps.push({
    ...snap(-1, -1, '', [], 'fn_entry', 'init', INIT_ENTRY, {}),
    badge: `public String mergeAlternately(String word1, String word2) → Function entry. Will merge characters alternately.`
  });
  steps.push({
    ...snap(0, -1, '', [], 'init_i', 'init', AFTER_I, {}),
    badge: `int i = 0; → i = 0. Points to the first character of word1.`
  });
  steps.push({
    ...snap(0, 0, '', [], 'init_j', 'init', AFTER_J, {}),
    badge: `int j = 0; → j = 0. Points to the first character of word2.`
  });
  steps.push({
    ...snap(0, 0, '', [], 'init_result', 'init', ALL, {}),
    badge: `StringBuilder result = new StringBuilder(); → result = "". Will accumulate the merged string.`
  });

  let i = 0;
  let j = 0;
  let result = '';

  // ── Main alternating while loop ───────────────────────────────────────────
  while (i < n1 && j < n2) {
    steps.push({
      ...snap(i, j, result, resultSrcs, 'while1_cond', 'solve', ALL, {}),
      badge: `while (i < word1.length() && j < word2.length()): i=${i}<${n1} && j=${j}<${n2} → TRUE. Continue interleaving.`
    });

    result += word1[i];
    resultSrcs = [...resultSrcs, 'w1'];
    steps.push({
      ...snap(i, j, result, resultSrcs, 'append_w1', 'solve', ALL, { justAppended: result.length - 1, appendedFrom: 'w1' }),
      badge: `result.append(word1.charAt(i)); → Appended '${word1[i]}' (word1[${i}]). result = "${result}"`
    });

    i++;
    steps.push({
      ...snap(i, j, result, resultSrcs, 'inc_i', 'solve', ALL, {}),
      badge: `i++; → i = ${i}. Move pointer in word1 forward.`
    });

    result += word2[j];
    resultSrcs = [...resultSrcs, 'w2'];
    steps.push({
      ...snap(i, j, result, resultSrcs, 'append_w2', 'solve', ALL, { justAppended: result.length - 1, appendedFrom: 'w2' }),
      badge: `result.append(word2.charAt(j)); → Appended '${word2[j]}' (word2[${j}]). result = "${result}"`
    });

    j++;
    steps.push({
      ...snap(i, j, result, resultSrcs, 'inc_j', 'solve', ALL, {}),
      badge: `j++; → j = ${j}. Move pointer in word2 forward.`
    });
  }

  steps.push({
    ...snap(i, j, result, resultSrcs, 'while1_cond', 'solve', ALL, {}),
    badge: `while (i < word1.length() && j < word2.length()): i=${i}, j=${j} → FALSE. At least one string exhausted.`
  });

  // ── Tail: remaining word1 ─────────────────────────────────────────────────
  const hasRem1 = i < n1;
  steps.push({
    ...snap(i, j, result, resultSrcs, 'while2_cond', hasRem1 ? 'tail1' : 'solve', ALL, {}),
    badge: `while (i < word1.length()): i=${i} < ${n1} → ${hasRem1 ? 'TRUE. Append remaining word1 characters.' : 'FALSE. word1 exhausted.'}`
  });

  while (i < n1) {
    result += word1[i];
    resultSrcs = [...resultSrcs, 'w1'];
    steps.push({
      ...snap(i, j, result, resultSrcs, 'append_rem1', 'tail1', ALL, { justAppended: result.length - 1, appendedFrom: 'w1' }),
      badge: `result.append(word1.charAt(i)); → Appended '${word1[i]}' (word1[${i}]). result = "${result}"`
    });

    i++;
    steps.push({
      ...snap(i, j, result, resultSrcs, 'inc_i2', 'tail1', ALL, {}),
      badge: `i++; → i = ${i}.`
    });

    steps.push({
      ...snap(i, j, result, resultSrcs, 'while2_cond', 'tail1', ALL, {}),
      badge: `while (i < word1.length()): i=${i} < ${n1} → ${i < n1 ? 'TRUE.' : 'FALSE. Done with word1.'}`
    });
  }

  // ── Tail: remaining word2 ─────────────────────────────────────────────────
  const hasRem2 = j < n2;
  steps.push({
    ...snap(i, j, result, resultSrcs, 'while3_cond', hasRem2 ? 'tail2' : 'solve', ALL, {}),
    badge: `while (j < word2.length()): j=${j} < ${n2} → ${hasRem2 ? 'TRUE. Append remaining word2 characters.' : 'FALSE. word2 exhausted.'}`
  });

  while (j < n2) {
    result += word2[j];
    resultSrcs = [...resultSrcs, 'w2'];
    steps.push({
      ...snap(i, j, result, resultSrcs, 'append_rem2', 'tail2', ALL, { justAppended: result.length - 1, appendedFrom: 'w2' }),
      badge: `result.append(word2.charAt(j)); → Appended '${word2[j]}' (word2[${j}]). result = "${result}"`
    });

    j++;
    steps.push({
      ...snap(i, j, result, resultSrcs, 'inc_j2', 'tail2', ALL, {}),
      badge: `j++; → j = ${j}.`
    });

    steps.push({
      ...snap(i, j, result, resultSrcs, 'while3_cond', 'tail2', ALL, {}),
      badge: `while (j < word2.length()): j=${j} < ${n2} → ${j < n2 ? 'TRUE.' : 'FALSE. Done with word2.'}`
    });
  }

  steps.push({
    ...snap(i, j, result, resultSrcs, 'return_stmt', 'done', ALL, {}),
    badge: `return result.toString(); → Final merged string: "${result}"`
  });

  return steps;
}

// ─── Reactive State ───────────────────────────────────────────────────────────
const inputWord1 = ref('abc');
const inputWord2 = ref('pqr');
const lang        = ref('java');
const speed       = ref(650);
const si          = ref(0);
const playing     = ref(false);
const vizHeight   = ref(350);
const tableHeight = ref(60);
const leftWidth   = ref(52);
const rightTab    = ref('code');

const stepsData = reactive({ steps: buildSteps('abc', 'pqr') });
const steps     = computed(() => stepsData.steps);
const s         = computed(() => steps.value[Math.max(0, Math.min(si.value, steps.value.length - 1))] || {});
const codeLines = computed(() => CODES[lang.value] || []);

let playTimer = null;

function onChipsWheel(e) {
  if (e.currentTarget) {
    e.currentTarget.scrollLeft += e.deltaY;
  }
}

function applyInput() {
  const w1 = inputWord1.value.trim().replace(/[^a-zA-Z0-9]/g, '').slice(0, 15);
  const w2 = inputWord2.value.trim().replace(/[^a-zA-Z0-9]/g, '').slice(0, 15);
  if (!w1 || !w2) {
    alert('Both word1 and word2 must be non-empty.');
    return;
  }
  playing.value = false;
  stepsData.steps = buildSteps(w1, w2);
  si.value = 0;
}

function stepBy(d) {
  si.value = Math.max(0, Math.min(steps.value.length - 1, si.value + d));
}

function togglePlay() {
  const next = !playing.value;
  if (next && si.value >= steps.value.length - 1) {
    si.value = 0;
  }
  playing.value = next;
}

function tick() {
  clearTimeout(playTimer);
  if (!playing.value) {
    return;
  }
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
  if (v) {
    tick();
  } else {
    clearTimeout(playTimer);
  }
});

const codeScrollRef = ref(null);

function scrollActiveCodeLine() {
  nextTick(() => {
    const container = codeScrollRef.value;
    if (!container) {
      return;
    }
    const activeEl = container.querySelector('.ll-hl');
    if (!activeEl) {
      return;
    }
    const contRect   = container.getBoundingClientRect();
    const activeRect = activeEl.getBoundingClientRect();
    if (activeRect.top < contRect.top) {
      container.scrollTop = Math.max(0, container.scrollTop - (contRect.top - activeRect.top) - 24);
    } else if (activeRect.bottom > contRect.bottom) {
      container.scrollTop = container.scrollTop + (activeRect.bottom - contRect.bottom) + 24;
    }
  });
}

watch(() => s.value.code, scrollActiveCodeLine);
watch(lang, scrollActiveCodeLine);
watch(rightTab, v => {
  if (v === 'code') {
    scrollActiveCodeLine();
  }
});

function onKeydown(e) {
  const tag = e.target.tagName;
  if (tag === 'INPUT' || tag === 'SELECT' || tag === 'TEXTAREA') {
    return;
  }
  if (e.key === 'ArrowRight') {
    stepBy(1);
  }
  if (e.key === 'ArrowLeft') {
    stepBy(-1);
  }
  if (e.key === ' ') {
    e.preventDefault();
    togglePlay();
  }
}

// ─── Computed display helpers ──────────────────────────────────────────────────
const initState     = computed(() => s.value.initState || { iReady: false, jReady: false, resultReady: false });
const displayI      = computed(() => initState.value.iReady      ? s.value.i         : '?');
const displayJ      = computed(() => initState.value.jReady      ? s.value.j         : '?');
const displayResult = computed(() => initState.value.resultReady ? s.value.resultStr : '?');
const displayWord1  = computed(() => s.value.word1 || inputWord1.value);
const displayWord2  = computed(() => s.value.word2 || inputWord2.value);
const displayN1     = computed(() => s.value.n1 || 0);
const displayN2     = computed(() => s.value.n2 || 0);
const displaySrcs   = computed(() => s.value.resultSrcs || []);

function charClass1(idx) {
  const phase = s.value.phase;
  const ist   = initState.value;
  const ci    = s.value.i;
  const classes = [];
  if (phase === 'input' || !ist.iReady) {
    return '';
  }
  if (phase === 'done') {
    classes.push('msa-char-done');
    return classes.join(' ');
  }
  if (idx === ci) {
    classes.push('msa-char-active-w1');
  } else if (idx < ci) {
    classes.push('msa-char-used');
  }
  if (s.value.appendedFrom === 'w1' && s.value.justAppended !== undefined && idx === ci - 1) {
    classes.push('msa-char-just-appended');
  }
  return classes.join(' ');
}

function charClass2(idx) {
  const phase = s.value.phase;
  const ist   = initState.value;
  const cj    = s.value.j;
  const classes = [];
  if (phase === 'input' || !ist.jReady) {
    return '';
  }
  if (phase === 'done') {
    classes.push('msa-char-done');
    return classes.join(' ');
  }
  if (idx === cj) {
    classes.push('msa-char-active-w2');
  } else if (idx < cj) {
    classes.push('msa-char-used');
  }
  if (s.value.appendedFrom === 'w2' && s.value.justAppended !== undefined && idx === cj - 1) {
    classes.push('msa-char-just-appended');
  }
  return classes.join(' ');
}

function resultCharClass(idx) {
  const src = displaySrcs.value[idx];
  const isJust = s.value.justAppended === idx;
  const phase = s.value.phase;
  const classes = [];
  if (src === 'w1') {
    classes.push('msa-res-w1');
  } else if (src === 'w2') {
    classes.push('msa-res-w2');
  }
  if (isJust && (phase === 'solve' || phase === 'tail1' || phase === 'tail2')) {
    classes.push('msa-res-just');
  }
  if (phase === 'done') {
    classes.push('msa-res-done');
  }
  return classes.join(' ');
}

// ─── Resizer setup ────────────────────────────────────────────────────────────
const mainRef         = ref(null);
const leftColRef      = ref(null);
const hResizerRef     = ref(null);
const vizResizerRef   = ref(null);
const tableResizerRef = ref(null);

function initHResizer() {
  const rsz  = hResizerRef.value;
  const main = mainRef.value;
  if (!rsz || !main) {
    return;
  }
  let dragging = false;
  let startX   = 0;
  let startW   = 0;
  const onDown = e => {
    dragging = true;
    startX   = e.clientX;
    startW   = leftColRef.value.offsetWidth;
    rsz.classList.add('drag');
    document.body.style.userSelect = 'none';
  };
  const onMove = e => {
    if (!dragging) {
      return;
    }
    const mainW = main.offsetWidth;
    leftWidth.value = (Math.max(200, Math.min(mainW - 200, startW + e.clientX - startX)) / mainW) * 100;
  };
  const onUp = () => {
    if (!dragging) {
      return;
    }
    dragging = false;
    rsz.classList.remove('drag');
    document.body.style.userSelect = '';
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
  if (!rsz) {
    return;
  }
  let dragging = false;
  let startY   = 0;
  let startH   = 0;
  const onDown = e => {
    dragging = true;
    startY   = e.clientY;
    startH   = valueRef.value;
    rsz.classList.add('drag');
    document.body.style.userSelect = 'none';
    e.preventDefault();
  };
  const onMove = e => {
    if (!dragging) {
      return;
    }
    valueRef.value = Math.max(minH, Math.min(maxH, startH + (e.clientY - startY)));
  };
  const onUp = () => {
    if (!dragging) {
      return;
    }
    dragging = false;
    rsz.classList.remove('drag');
    document.body.style.userSelect = '';
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
  cleanupFns.push(initVResizer(vizResizerRef,   vizHeight,   220, 700));
  cleanupFns.push(initVResizer(tableResizerRef, tableHeight,  50, 200));
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
      <h2 class="navbar-title">{{ topic }} &mdash; {{ subTopic }}</h2>
      <img src="../../assets/logo.png" alt="Logo" />
    </div>

    <div class="slide-body">
      <div class="row-main">
        <div class="ll-root">

          <!-- Toolbar -->
          <div class="ll-toolbar">
            <div class="ll-input-group">
              <label>word1:</label>
              <input
                type="text"
                v-model="inputWord1"
                class="ll-text-input msa-word-input"
                @keyup.enter="applyInput"
                placeholder="e.g. abc"
                maxlength="15"
              />
            </div>
            <div class="ll-input-group">
              <label>word2:</label>
              <input
                type="text"
                v-model="inputWord2"
                class="ll-text-input msa-word-input"
                @keyup.enter="applyInput"
                placeholder="e.g. pqr"
                maxlength="15"
              />
            </div>
            <button class="ll-viz-btn" @click="applyInput">&#9654; Visualize</button>
            <div class="ll-nav-controls">
              <button class="ll-nav-btn" title="First step"    @click="stepBy(-steps.length)">&#171;</button>
              <button class="ll-nav-btn" title="Previous step" @click="stepBy(-1)">&#8249; Prev</button>
              <button class="ll-play-btn"                      @click="togglePlay">{{ playing ? '\u23F8 Pause' : '\u25B6 Play' }}</button>
              <button class="ll-nav-btn" title="Next step"     @click="stepBy(1)">Next &#8250;</button>
              <button class="ll-nav-btn" title="Last step"     @click="stepBy(steps.length)">&#187;</button>
            </div>
          </div>

          <div class="ll-main" ref="mainRef">
            <!-- Left Visualization Column -->
            <div class="ll-left-col" ref="leftColRef" :style="{ width: leftWidth + '%' }">
              <div class="ll-viz-wrap" :style="{ height: vizHeight + 'px' }">
                <div class="ll-perm-area">

                  <!-- Stats Chips -->
                  <div class="ll-ptrs ll-ptrs-compact" @wheel.passive="onChipsWheel">
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">n1</span><b class="ll-c-blue">{{ displayN1 }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">n2</span><b class="ll-c-green">{{ displayN2 }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">i</span><b class="ll-c-blue">{{ displayI }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">j</span><b class="ll-c-green">{{ displayJ }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">result</span><b class="ll-c-orange" style="font-family:monospace;max-width:160px;overflow:hidden;text-overflow:ellipsis;display:inline-block;white-space:nowrap;">"{{ displayResult }}"</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">phase</span><b class="ll-c-orange">{{ s.phase || 'input' }}</b></span>
                  </div>

                  <!-- BOARD CONTAINER -->
                  <div class="ll-board-container">

                    <!-- Tier 1 — word1 -->
                    <div class="msa-tier-title">
                      Tier 1 &mdash; word1
                      <code>"{{ displayWord1 }}"</code>
                      <span class="msa-len-badge">(length {{ displayN1 }})</span>
                    </div>
                    <div class="msa-str-frame msa-frame-w1">
                      <!-- Pointer label row -->
                      <div class="msa-ptr-row">
                        <div
                          v-for="(ch, idx) in displayWord1.split('')"
                          :key="'p1-' + idx"
                          class="msa-ptr-cell"
                        >
                          <span
                            v-if="initState.iReady && idx === s.i && (s.phase === 'solve' || s.phase === 'tail1' || s.phase === 'init')"
                            class="msa-ptr-label msa-ptr-i"
                          >i</span>
                        </div>
                        <!-- exhausted marker when i >= n1 -->
                        <div v-if="initState.iReady && s.i >= displayN1" class="msa-ptr-cell msa-exhausted-cell">
                          <span class="msa-ptr-label msa-ptr-exhausted">i (done)</span>
                        </div>
                      </div>
                      <!-- Character boxes -->
                      <div class="msa-char-row">
                        <div
                          v-for="(ch, idx) in displayWord1.split('')"
                          :key="'w1-' + idx"
                          class="msa-char-box"
                          :class="charClass1(idx)"
                        >
                          <span class="msa-char-val">{{ ch }}</span>
                          <span class="msa-char-idx">[{{ idx }}]</span>
                        </div>
                      </div>
                    </div>

                    <!-- Tier 2 — word2 -->
                    <div class="msa-tier-title">
                      Tier 2 &mdash; word2
                      <code>"{{ displayWord2 }}"</code>
                      <span class="msa-len-badge">(length {{ displayN2 }})</span>
                    </div>
                    <div class="msa-str-frame msa-frame-w2">
                      <!-- Pointer label row -->
                      <div class="msa-ptr-row">
                        <div
                          v-for="(ch, idx) in displayWord2.split('')"
                          :key="'p2-' + idx"
                          class="msa-ptr-cell"
                        >
                          <span
                            v-if="initState.jReady && idx === s.j && (s.phase === 'solve' || s.phase === 'tail2' || s.phase === 'init')"
                            class="msa-ptr-label msa-ptr-j"
                          >j</span>
                        </div>
                        <!-- exhausted marker when j >= n2 -->
                        <div v-if="initState.jReady && s.j >= displayN2" class="msa-ptr-cell msa-exhausted-cell">
                          <span class="msa-ptr-label msa-ptr-exhausted">j (done)</span>
                        </div>
                      </div>
                      <!-- Character boxes -->
                      <div class="msa-char-row">
                        <div
                          v-for="(ch, idx) in displayWord2.split('')"
                          :key="'w2-' + idx"
                          class="msa-char-box"
                          :class="charClass2(idx)"
                        >
                          <span class="msa-char-val">{{ ch }}</span>
                          <span class="msa-char-idx">[{{ idx }}]</span>
                        </div>
                      </div>
                    </div>

                    <!-- Tier 3 — result string -->
                    <div class="msa-tier-title">
                      Tier 3 &mdash; result
                      <code v-if="s.resultStr">"{{ s.resultStr }}"</code>
                      <code v-else>""</code>
                      <span class="msa-len-badge">({{ s.resultStr ? s.resultStr.length : 0 }} chars)</span>
                    </div>
                    <div class="msa-result-frame">
                      <div class="msa-char-row msa-result-row">
                        <div
                          v-for="(ch, idx) in (s.resultStr || '').split('')"
                          :key="'res-' + idx"
                          class="msa-char-box msa-res-box"
                          :class="resultCharClass(idx)"
                        >
                          <span class="msa-char-val">{{ ch }}</span>
                          <span class="msa-char-idx">[{{ idx }}]</span>
                        </div>
                        <div
                          v-if="initState.resultReady && (!s.resultStr || s.resultStr.length === 0)"
                          class="msa-empty-result"
                        >
                          &lang; empty &rang;
                        </div>
                      </div>
                    </div>

                  </div>
                  <!-- END ll-board-container -->
                </div>
              </div>

              <!-- Vertical Resizer -->
              <div class="ll-vresizer" ref="vizResizerRef"></div>

              <!-- Color Legend -->
              <div class="ll-legend">
                <span class="ll-leg"><span class="ll-legdot" style="background:#bfdbfe;border:1.5px solid #3b82f6;"></span>i (word1)</span>
                <span class="ll-leg"><span class="ll-legdot" style="background:#bbf7d0;border:1.5px solid #22c55e;"></span>j (word2)</span>
                <span class="ll-leg"><span class="ll-legdot msa-legdot-used"></span>used</span>
                <span class="ll-leg"><span class="ll-legdot msa-legdot-res-w1"></span>result (w1)</span>
                <span class="ll-leg"><span class="ll-legdot msa-legdot-res-w2"></span>result (w2)</span>
                <span class="ll-leg"><span class="ll-legdot msa-legdot-done"></span>done</span>
              </div>

              <!-- Call Stack Area -->
              <div class="ll-table-area" :style="{ height: tableHeight + 'px' }">
                <div class="ll-table-title">Call Stack &mdash; Current Execution Frame</div>
                <div class="ll-stack-line">
                  <template v-if="s.phase === 'input'">
                    <div class="ll-frame ll-frame-cur">
                      main()
                      &nbsp;&nbsp;
                      <span class="ll-fname">word1</span>=<span class="ll-c-blue" style="font-weight:700">"{{ displayWord1 }}"</span>,
                      <span class="ll-fname">word2</span>=<span class="ll-c-green" style="font-weight:700">"{{ displayWord2 }}"</span>
                      <span class="ll-now"> &#9668; active</span>
                    </div>
                  </template>
                  <template v-else>
                    <div class="ll-frame" style="color:var(--text2);">main()</div>
                    <div class="ll-frame ll-frame-cur" style="margin-left:14px">
                      mergeAlternately("{{ displayWord1 }}", "{{ displayWord2 }}")
                      &nbsp;&nbsp;
                      <span class="ll-fname">i</span>=<span class="ll-c-blue" style="font-weight:700">{{ displayI }}</span>,
                      <span class="ll-fname">j</span>=<span class="ll-c-green" style="font-weight:700">{{ displayJ }}</span>,
                      <span class="ll-fname">result</span>=<span class="ll-c-orange" style="font-weight:700">"{{ displayResult }}"</span>
                      <span class="ll-now"> &#9668; active</span>
                    </div>
                  </template>
                </div>
              </div>

              <!-- Vertical Resizer -->
              <div class="ll-vresizer" ref="tableResizerRef"></div>

              <!-- Step Badge -->
              <div class="ll-badge-wrap">
                <div class="ll-badge"
                  :class="{
                    'll-badge-error':   s.badge && (s.badge.includes('FALSE') || s.badge.includes('exhausted')),
                    'll-badge-success': s.badge && (s.badge.includes('Final') || s.badge.includes('TRUE') || s.badge.includes('Appended'))
                  }"
                >
                  {{ s.badge || 'Ready to visualize Merge Strings Alternately.' }}
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
                    <button class="ll-tab-btn" :class="{ active: rightTab === 'code' }"       @click="rightTab = 'code'">Code</button>
                    <button class="ll-tab-btn" :class="{ active: rightTab === 'pseudo' }"     @click="rightTab = 'pseudo'">Pseudocode</button>
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

                <div v-if="rightTab === 'code'" class="ll-code-scroll" ref="codeScrollRef">
                  <pre class="ll-pre"><span
                    v-for="(line, idx) in codeLines"
                    :key="idx"
                    class="ll-codeline"
                    :class="{ 'll-hl': line[0] && line[0] === s.code }"
                  >{{ line[1] === '' ? ' ' : line[1] }}</span></pre>
                </div>

                <div v-else-if="rightTab === 'pseudo'" class="ll-code-scroll">
                  <pre class="ll-pre"><span
                    v-for="(line, idx) in PSEUDOCODE"
                    :key="idx"
                    class="ll-codeline"
                  >{{ line }}</span></pre>
                </div>

                <div v-else class="ll-info-scroll">
                  <h3 class="ll-cx-heading">Merge Strings Alternately (LeetCode 1768) &mdash; Complexity Analysis</h3>
                  <p class="ll-cx-intro">
                    Given two strings <code>word1</code> and <code>word2</code>, merge them alternately by taking one
                    character from each in turn. Once one string is exhausted, append the remainder of the other.
                    The two-pointer approach uses indices <code>i</code> and <code>j</code> walking each string simultaneously.
                  </p>

                  <h4 class="ll-cx-sub">Step-by-step Breakdown</h4>
                  <table class="ll-complexity-table">
                    <thead>
                      <tr><th>Operation</th><th>Time</th><th>Space</th><th>Reason</th></tr>
                    </thead>
                    <tbody>
                      <tr><td>Init i, j, result</td><td class="ll-cx-good">O(1)</td><td class="ll-cx-good">O(1)</td><td>Three constant-time assignments</td></tr>
                      <tr><td>Main while loop</td><td class="ll-cx-good">O(min(n,m))</td><td class="ll-cx-good">O(1)</td><td>Runs until shorter string exhausted</td></tr>
                      <tr><td>Tail loop 1 (rem word1)</td><td class="ll-cx-good">O(n-m)</td><td class="ll-cx-good">O(1)</td><td>Appends remaining chars if word1 longer</td></tr>
                      <tr><td>Tail loop 2 (rem word2)</td><td class="ll-cx-good">O(m-n)</td><td class="ll-cx-good">O(1)</td><td>Appends remaining chars if word2 longer</td></tr>
                      <tr><td>Build result string</td><td class="ll-cx-good">O(n+m)</td><td class="ll-cx-good">O(n+m)</td><td>Result has n+m characters total</td></tr>
                      <tr><td><strong>Total</strong></td><td class="ll-cx-good"><strong>O(n+m)</strong></td><td class="ll-cx-good"><strong>O(n+m)</strong></td><td>Each character visited exactly once</td></tr>
                    </tbody>
                  </table>

                  <h4 class="ll-cx-sub">Overall Complexity</h4>
                  <div class="ll-cx-summary-grid">
                    <div class="ll-cx-card ll-cx-card-good">
                      <div class="ll-cx-card-label">Time</div>
                      <div class="ll-cx-card-val">O(n+m)</div>
                      <div class="ll-cx-card-note">Linear in total input length</div>
                    </div>
                    <div class="ll-cx-card ll-cx-card-good">
                      <div class="ll-cx-card-label">Space</div>
                      <div class="ll-cx-card-val">O(n+m)</div>
                      <div class="ll-cx-card-note">Output string holds all chars</div>
                    </div>
                    <div class="ll-cx-card ll-cx-card-good">
                      <div class="ll-cx-card-label">Passes</div>
                      <div class="ll-cx-card-val">1</div>
                      <div class="ll-cx-card-note">Single linear scan of both strings</div>
                    </div>
                  </div>

                  <div class="ll-note">
                    <strong>n = word1.length(), m = word2.length().</strong> Three while loops together cover every character
                    exactly once: the first loop handles characters where both pointers are valid; the second and third
                    handle the tail of whichever string is longer. Only one of the tail loops executes per call.
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
  font-family: 'Segoe UI', system-ui, sans-serif; font-size: 12.5px;
  display: flex; flex-direction: column; height: 74vh; overflow: hidden; width: 100%;
}

@keyframes msa-flash-append { 0% { background:#fef9c3; transform:scale(1.12); } 60% { background:#a7f3d0; } 100% { transform:scale(1); } }
@keyframes msa-flash-used   { 0% { opacity:1; } 100% { opacity:.5; } }

.slide-wrapper { margin-top: -10px; margin-left: -30px; width: 107%; max-height: 100%; font-size: .8rem; font-weight: 400; }
.slide-body    { display: flex; flex-direction: column; border-radius: 4px; height: 100%; }
.navbar { display: flex; flex-direction: row; justify-content: space-between; align-items: center; gap: .75rem; padding: 0 10px; background-color: #fff; position: fixed; width: 94.7%; z-index: 50; }
.navbar > img  { height: 30px; }
.navbar-title  { margin: 0; font-size: 1.35rem; font-weight: 700; background-color: #ef5050; color: #fff; width: 80%; padding: 2px 10px; margin-left: -10px; border-radius: 5px; }
.row-main      { width: 100%; height: 90%; margin-top: 36px; overflow-x: auto; overflow-y: hidden; }

/* Toolbar */
.ll-toolbar      { margin-top: 4px; display: flex; align-items: center; gap: 6px; padding: 6.5px 12px; background: var(--surface); border-bottom: 1px solid var(--border); flex-shrink: 0; flex-wrap: wrap; box-shadow: var(--shadow-sm); }
.ll-input-group  { display: flex; align-items: center; gap: 4px; }
.ll-input-group label { font-size: 11px; color: var(--muted); font-weight: 700; }
.ll-text-input   { background: var(--surface); border: 1px solid var(--border2); color: var(--text); border-radius: var(--radius-sm); padding: 3px 6px; font-size: 11.5px; font-family: monospace; }
.ll-text-input:focus { outline: none; border-color: var(--coral); box-shadow: 0 0 0 3px rgba(240,77,77,.1); }
.msa-word-input  { width: 120px; }
.ll-viz-btn      { background: var(--coral); color: #fff; border: none; padding: 5px 12px; border-radius: var(--radius-sm); cursor: pointer; font-size: 11.5px; font-weight: 600; box-shadow: var(--shadow-sm); transition: filter .15s; }
.ll-viz-btn:hover { filter: brightness(1.08); }
.ll-nav-controls { display: flex; margin-left: auto; align-items: center; gap: 4px; flex-shrink: 0; flex-wrap: wrap; }
.ll-nav-btn  { background: var(--surface2); border: 1px solid var(--border2); color: var(--text2); padding: 4px 9px; border-radius: var(--radius-sm); cursor: pointer; font-size: 11px; font-weight: 500; transition: all .15s; white-space: nowrap; }
.ll-nav-btn:hover  { background: var(--surface); border-color: var(--coral); color: var(--coral); }
.ll-play-btn  { background: var(--blue-light); border: 1px solid var(--blue); color: var(--blue); min-width: 68px; font-weight: 600; padding: 4px 9px; border-radius: var(--radius-sm); cursor: pointer; font-size: 11px; transition: all .15s; }
.ll-play-btn:hover { background: var(--blue); color: #fff; }

/* Layout */
.ll-main      { display: flex; flex: 1; overflow: hidden; position: relative; }
.ll-left-col  { display: flex; flex-direction: column; overflow: hidden; min-width: 220px; max-width: 75%; }
.ll-resizer   { width: 5px; cursor: col-resize; background: var(--border); flex-shrink: 0; transition: background .15s; position: relative; z-index: 20; }
.ll-resizer:hover, .ll-resizer.drag { background: var(--coral); }
.ll-right-col { display: flex; flex-direction: column; flex: 1; overflow: hidden; min-width: 0; height: 100%; }
.ll-viz-wrap  { flex-shrink: 0; background: var(--surface); border-bottom: 1px solid var(--border); position: relative; overflow-x: auto; overflow-y: auto; }
.ll-perm-area { display: flex; flex-direction: column; align-items: stretch; min-height: 100%; width: 100%; min-width: 0; box-sizing: border-box; }
.ll-ptrs          { display: flex; gap: 8px; flex-wrap: wrap; padding: 4px 14px; min-height: 28px; width: 100%; box-sizing: border-box; min-width: 0; align-items: center; }
.ll-ptrs-compact  { flex-wrap: nowrap; gap: 6px; padding: 3px 14px 5px; min-height: 0; overflow-x: auto; overflow-y: hidden; }
.ll-ptr-chip-inline { display: inline-flex; align-items: center; gap: 4px; background: var(--surface2); border: 1px solid var(--border); border-radius: 6px; padding: 2px 8px; font-size: 11px; font-family: monospace; white-space: nowrap; flex-shrink: 0; line-height: 1.4; }
.ll-chip-label { color: #8899aa; font-weight: 500; margin-right: 2px; }
.ll-c-blue   { color: var(--blue); }
.ll-c-green  { color: var(--green); }
.ll-c-orange { color: var(--orange); }
.ll-c-purple { color: var(--purple); }

/* ─── Board Container — Merge Strings Alternately ───────────────────────────── */
.ll-board-container { display: flex; flex-direction: column; align-items: flex-start; padding: 6px 14px 10px; gap: 8px; min-width: 0; width: 100%; }

.msa-tier-title {
  font-size: 10px; font-weight: 700; text-transform: uppercase; letter-spacing: .04em;
  color: var(--muted); margin-top: 4px; margin-bottom: 2px; font-family: 'Consolas', monospace;
  border-left: 3px solid var(--coral); padding-left: 6px; display: flex; align-items: center; gap: 6px;
}
.msa-len-badge { font-size: 9px; color: var(--muted); background: var(--surface2); border: 1px solid var(--border); border-radius: 3px; padding: 0 4px; font-weight: 500; text-transform: none; letter-spacing: 0; }

/* String frame */
.msa-str-frame, .msa-result-frame {
  display: flex; flex-direction: column; gap: 2px;
  background: #f8fafc; border: 1px solid var(--border);
  border-radius: var(--radius); padding: 6px 10px 8px; box-shadow: var(--shadow-sm);
}
.msa-frame-w1 { border-left: 3px solid var(--blue); }
.msa-frame-w2 { border-left: 3px solid var(--green); }
.msa-result-frame { border-left: 3px solid var(--orange); min-height: 62px; }

/* Pointer row */
.msa-ptr-row { display: flex; gap: 4px; height: 18px; align-items: flex-end; margin-bottom: 2px; }
.msa-ptr-cell { width: 38px; display: flex; justify-content: center; align-items: flex-end; flex-shrink: 0; }
.msa-exhausted-cell { width: auto; }
.msa-ptr-label { font-size: 8.5px; font-weight: 800; font-family: monospace; padding: 1px 4px; border-radius: 3px; line-height: 1.2; white-space: nowrap; }
.msa-ptr-i    { background: #eff6ff; color: #1d4ed8; border: 1px solid #93c5fd; }
.msa-ptr-j    { background: #f0fdf4; color: #15803d; border: 1px solid #86efac; }
.msa-ptr-exhausted { background: #f1f5f9; color: #94a3b8; border: 1px solid #cbd5e1; }

/* Character row */
.msa-char-row { display: flex; flex-wrap: wrap; gap: 4px; }
.msa-result-row { flex-wrap: wrap; min-height: 44px; }

/* Character box */
.msa-char-box {
  width: 38px; height: 44px; border-radius: var(--radius-sm);
  background: #e2e8f0; border: 1.5px solid #94a3b8;
  display: flex; flex-direction: column; align-items: center; justify-content: center;
  gap: 2px; flex-shrink: 0; transition: all .18s ease; cursor: default;
}
.msa-char-val { font-family: 'Cascadia Code', 'Fira Code', monospace; font-size: 16px; font-weight: 800; color: var(--text); line-height: 1; }
.msa-char-idx { font-family: monospace; font-size: 7.5px; color: var(--muted); font-weight: 600; }

/* word1 active (i pointer) */
.msa-char-active-w1 { background: #bfdbfe !important; border: 2px solid var(--blue) !important; box-shadow: 0 0 8px rgba(59,130,246,.3); }
/* word2 active (j pointer) */
.msa-char-active-w2 { background: #bbf7d0 !important; border: 2px solid var(--green) !important; box-shadow: 0 0 8px rgba(34,197,94,.3); }
/* used / exhausted */
.msa-char-used { opacity: .45; background: #f1f5f9 !important; border-color: #cbd5e1 !important; }
/* just appended flash */
.msa-char-just-appended { animation: msa-flash-append .55s ease; }
/* done phase */
.msa-char-done { background: #dcfce7 !important; border-color: #86efac !important; }

/* Result char boxes */
.msa-res-box  { width: 38px; height: 44px; border-radius: var(--radius-sm); }
.msa-res-w1   { background: #bfdbfe !important; border: 1.5px solid var(--blue) !important; }
.msa-res-w2   { background: #bbf7d0 !important; border: 1.5px solid var(--green) !important; }
.msa-res-just { animation: msa-flash-append .5s ease; box-shadow: 0 0 10px rgba(245,158,11,.5); }
.msa-res-done { border-width: 2px !important; }

.msa-empty-result { font-size: 11px; color: var(--muted); font-style: italic; align-self: center; padding: 10px 0; }

/* Legend dots */
.msa-legdot-used    { background: #f1f5f9; border: 1.5px solid #cbd5e1; display: inline-block; width: 11px; height: 11px; border-radius: 3px; }
.msa-legdot-res-w1  { background: #bfdbfe; border: 1.5px solid var(--blue); display: inline-block; width: 11px; height: 11px; border-radius: 3px; }
.msa-legdot-res-w2  { background: #bbf7d0; border: 1.5px solid var(--green); display: inline-block; width: 11px; height: 11px; border-radius: 3px; }
.msa-legdot-done    { background: #dcfce7; border: 1.5px solid #86efac; display: inline-block; width: 11px; height: 11px; border-radius: 3px; }

/* Shared helpers */
.ll-vresizer { height: 5px; cursor: row-resize; background: var(--border); flex-shrink: 0; transition: background .15s; position: relative; z-index: 20; }
.ll-vresizer:hover, .ll-vresizer.drag { background: var(--coral); }
.ll-legend { display: flex; flex-wrap: wrap; gap: 6px 14px; padding: 6px 12px; border-bottom: 1px solid var(--border); flex-shrink: 0; background: var(--surface2); }
.ll-leg    { display: flex; align-items: center; gap: 5px; font-size: 11px; color: var(--text2); font-weight: 500; }
.ll-legdot { width: 11px; height: 11px; border-radius: 3px; flex-shrink: 0; display: inline-block; }

.ll-table-area  { flex-shrink: 0; padding: 8px 14px; border-bottom: 1px solid var(--border); overflow-x: hidden; overflow-y: auto; background: var(--surface); min-width: 0; box-sizing: border-box; }
.ll-table-title { font-size: 10px; color: var(--muted); margin-bottom: 4px; font-style: italic; }
.ll-stack-line  { font-family: 'Consolas', monospace; font-size: 12px; line-height: 1.8; }
.ll-frame       { font-family: 'Consolas', monospace; font-size: 11.5px; color: var(--text2); padding: 1px 0; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
.ll-frame-cur   { color: var(--orange); background: var(--orange-light); border-radius: 4px; padding: 1px 5px; }
.ll-fname       { color: var(--text2); }
.ll-now         { color: var(--orange); font-size: 10px; margin-left: 6px; }

.ll-badge-wrap { padding: 6px 10px; border-bottom: 1px solid var(--border); flex-shrink: 0; min-height: 36px; display: flex; align-items: center; background: var(--surface); }
.ll-badge      { display: inline-block; padding: 4px 12px; border-radius: var(--radius-sm); border-left: 3px solid var(--coral); background: var(--coral-light); font-size: 11px; color: var(--coral-dark); line-height: 1.4; word-break: break-word; font-weight: 500; }
.ll-badge-error   { border-left-color: var(--red)   !important; background: var(--red-light)   !important; color: var(--red-dark) !important; }
.ll-badge-success { border-left-color: var(--green) !important; background: var(--green-light) !important; color: #15803d !important; }

/* Code panel */
.ll-code-panel  { display: flex; flex-direction: column; height: 100%; overflow: hidden; }
.ll-code-header { display: flex; align-items: center; gap: 6px; padding: 5px 12px; background: var(--surface); border-bottom: 1px solid var(--border); box-shadow: var(--shadow-sm); flex-shrink: 0; flex-wrap: wrap; }
.ll-tabbar      { display: flex; gap: 3px; flex-wrap: wrap; }
.ll-tab-btn     { padding: 4px 9px; font-size: 10.5px; font-weight: 600; border: 1px solid var(--border2); background: var(--surface2); color: var(--text2); border-radius: var(--radius-sm); cursor: pointer; transition: all .15s ease; white-space: nowrap; }
.ll-tab-btn:hover  { border-color: var(--coral); color: var(--coral); }
.ll-tab-btn.active { background: var(--coral); border-color: var(--coral); color: #fff; }
.ll-lang-select {
  margin-left: auto; padding: 4px 24px 4px 8px; font-size: 11px; font-weight: 500;
  border: 1px solid var(--border2); border-radius: var(--radius-sm); background: var(--surface2); color: var(--text);
  cursor: pointer; appearance: none;
  background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='10' height='6' viewBox='0 0 10 6'%3E%3Cpath d='M0 0l5 6 5-6z' fill='%2394a3b8'/%3E%3C/svg%3E");
  background-repeat: no-repeat; background-position: right 8px center; min-width: 95px; transition: border-color .15s;
}
.ll-lang-select:focus { outline: none; border-color: var(--coral); box-shadow: 0 0 0 3px rgba(240,77,77,.1); }
.ll-code-scroll { flex: 1; overflow: auto; background: #f8fafc; padding: 10px 14px; min-width: 0; }
.ll-pre         { margin: 0; font-family: 'Cascadia Code', 'Fira Code', 'Consolas', monospace; font-size: 11px; line-height: 1.5; color: var(--text); white-space: pre; padding-bottom: 150px; }
.ll-codeline    { display: block; padding: 0 14px; margin: 0 -14px; }
.ll-hl          { background: #dcfce7; color: #15803d; font-weight: 600; border-left: 3px solid var(--green); border-radius: 3px; }

.ll-info-scroll { flex: 1; overflow: auto; padding: 12px 16px; background: var(--surface); color: var(--text2); font-size: 12px; line-height: 1.55; }
.ll-info-scroll h3, .ll-info-scroll h4 { color: var(--text); margin: 0 0 6px; }
.ll-info-scroll p { margin: 0 0 6px; }
.ll-info-scroll code { background: var(--surface2); padding: 1px 4px; border-radius: 3px; font-family: 'Cascadia Code', monospace; font-size: 11px; color: var(--coral-dark); }
.ll-cx-heading  { font-size: 13px; font-weight: 700; }
.ll-cx-intro    { font-size: 10.5px; margin: 0 0 10px; line-height: 1.55; }
.ll-cx-sub      { font-size: 11px; font-weight: 700; color: var(--text2); margin: 10px 0 4px; border-bottom: 1px solid var(--border); padding-bottom: 3px; }
.ll-complexity-table { width: 100%; border-collapse: collapse; font-size: 10.5px; margin: 8px 0; }
.ll-complexity-table th, .ll-complexity-table td { border: 1px solid var(--border); padding: 4px 8px; text-align: left; }
.ll-complexity-table th { background: var(--surface2); font-weight: 700; color: var(--text2); }
.ll-cx-good { color: #15803d; font-weight: 700; }
.ll-cx-summary-grid { display: flex; gap: 8px; flex-wrap: wrap; margin: 6px 0 10px; }
.ll-cx-card     { flex: 1; min-width: 90px; border-radius: var(--radius-sm); padding: 8px 10px; text-align: center; border: 1.5px solid var(--border); }
.ll-cx-card-good { background: #f0fdf4; border-color: #86efac; color: #15803d; }
.ll-cx-card-label { font-size: 9px; font-weight: 700; text-transform: uppercase; letter-spacing: .05em; opacity: .7; margin-bottom: 4px; }
.ll-cx-card-val  { font-size: 13px; font-weight: 800; font-family: monospace; margin-bottom: 3px; }
.ll-cx-card-note { font-size: 8.5px; opacity: .75; line-height: 1.3; }
.ll-note { background: #fefce8; border: 1px solid #fef08a; border-left: 3px solid #eab308; padding: 6px 10px; font-size: 10.5px; color: #854d0e; border-radius: 0 4px 4px 0; margin-top: 10px; margin-bottom: 120px; }

.ll-footer { display: flex; align-items: center; justify-content: space-between; padding: 4px 12px; background: var(--surface); border-top: 1px solid var(--border); font-size: 11px; color: var(--muted); font-weight: 600; flex-shrink: 0; }
.ll-speed-wrap { display: flex; align-items: center; gap: 6px; }
.ll-speed-wrap input[type="range"] { width: 80px; accent-color: var(--coral); }
</style>
