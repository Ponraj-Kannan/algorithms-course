<script setup>
import { ref, reactive, computed, onMounted, onBeforeUnmount, watch, nextTick } from 'vue';

defineProps({
  topic:    { type: String, default: 'String Algorithms' },
  subTopic: { type: String, default: 'Valid Palindrome II (LeetCode 680)' }
});

// ─── Multi-Language Code Definitions ──────────────────────────────────────────
const CODES = {
  java: [
    ['',            'import java.util.Scanner;'],
    ['',            ''],
    ['',            'class Solution {'],
    ['ip_entry',    '    private boolean isPalindrome(String s, int lo, int hi) {'],
    ['ip_while',    '        while (lo < hi) {'],
    ['ip_if',       '            if (s.charAt(lo) != s.charAt(hi)) {'],
    ['ip_false',    '                return false;'],
    ['',            '            }'],
    ['ip_lo',       '            lo++;'],
    ['ip_hi',       '            hi--;'],
    ['',            '        }'],
    ['ip_true',     '        return true;'],
    ['',            '    }'],
    ['',            ''],
    ['fn_entry',    '    public boolean validPalindrome(String s) {'],
    ['init_left',   '        int left = 0;'],
    ['init_right',  '        int right = s.length() - 1;'],
    ['while_cond',  '        while (left < right) {'],
    ['if_mismatch', '            if (s.charAt(left) != s.charAt(right)) {'],
    ['try_skip',    '                return isPalindrome(s, left + 1, right) || isPalindrome(s, left, right - 1);'],
    ['',            '            }'],
    ['inc_left',    '            left++;'],
    ['dec_right',   '            right--;'],
    ['',            '        }'],
    ['return_true', '        return true;'],
    ['',            '    }'],
    ['',            ''],
    ['',            '    public static void main(String[] args) {'],
    ['m_scanner',   '        Scanner sc = new Scanner(System.in);'],
    ['m_read_s',    '        String s = sc.nextLine();'],
    ['m_call',      '        System.out.println(new Solution().validPalindrome(s));'],
    ['',            '    }'],
    ['',            '}']
  ],
  cpp: [
    ['',            '#include <string>'],
    ['',            '#include <iostream>'],
    ['',            'using namespace std;'],
    ['',            ''],
    ['',            'class Solution {'],
    ['',            'private:'],
    ['ip_entry',    '    bool isPalindrome(const string& s, int lo, int hi) {'],
    ['ip_while',    '        while (lo < hi) {'],
    ['ip_if',       '            if (s[lo] != s[hi]) {'],
    ['ip_false',    '                return false;'],
    ['',            '            }'],
    ['ip_lo',       '            lo++;'],
    ['ip_hi',       '            hi--;'],
    ['',            '        }'],
    ['ip_true',     '        return true;'],
    ['',            '    }'],
    ['',            'public:'],
    ['fn_entry',    '    bool validPalindrome(const string& s) {'],
    ['init_left',   '        int left = 0;'],
    ['init_right',  '        int right = (int)s.size() - 1;'],
    ['while_cond',  '        while (left < right) {'],
    ['if_mismatch', '            if (s[left] != s[right]) {'],
    ['try_skip',    '                return isPalindrome(s, left + 1, right) || isPalindrome(s, left, right - 1);'],
    ['',            '            }'],
    ['inc_left',    '            left++;'],
    ['dec_right',   '            right--;'],
    ['',            '        }'],
    ['return_true', '        return true;'],
    ['',            '    }'],
    ['',            '};'],
    ['',            ''],
    ['',            'int main() {'],
    ['m_read_s',    '    string s;'],
    ['m_scanner',   '    cin >> s;'],
    ['m_call',      '    cout << (Solution().validPalindrome(s) ? "true" : "false") << endl;'],
    ['',            '    return 0;'],
    ['',            '}']
  ],
  python: [
    ['',            'class Solution:'],
    ['ip_entry',    '    def isPalindrome(self, s, lo, hi):'],
    ['ip_while',    '        while lo < hi:'],
    ['ip_if',       '            if s[lo] != s[hi]:'],
    ['ip_false',    '                return False'],
    ['ip_lo',       '            lo += 1'],
    ['ip_hi',       '            hi -= 1'],
    ['ip_true',     '        return True'],
    ['',            ''],
    ['fn_entry',    '    def validPalindrome(self, s):'],
    ['init_left',   '        left = 0'],
    ['init_right',  '        right = len(s) - 1'],
    ['while_cond',  '        while left < right:'],
    ['if_mismatch', '            if s[left] != s[right]:'],
    ['try_skip',    '                return self.isPalindrome(s, left + 1, right) or self.isPalindrome(s, left, right - 1)'],
    ['inc_left',    '            left += 1'],
    ['dec_right',   '            right -= 1'],
    ['return_true', '        return True'],
    ['',            ''],
    ['',            'def main():'],
    ['m_read_s',    '    s = input()'],
    ['m_call',      '    print(Solution().validPalindrome(s))'],
    ['',            ''],
    ['',            'if __name__ == "__main__":'],
    ['',            '    main()']
  ],
  javascript: [
    ['',            '/**'],
    ['',            ' * @param {string} s'],
    ['',            ' * @return {boolean}'],
    ['',            ' */'],
    ['fn_entry',    'var validPalindrome = function(s) {'],
    ['ip_entry',    '    function isPalindrome(lo, hi) {'],
    ['ip_while',    '        while (lo < hi) {'],
    ['ip_if',       '            if (s[lo] !== s[hi]) {'],
    ['ip_false',    '                return false;'],
    ['',            '            }'],
    ['ip_lo',       '            lo++;'],
    ['ip_hi',       '            hi--;'],
    ['',            '        }'],
    ['ip_true',     '        return true;'],
    ['',            '    }'],
    ['init_left',   '    let left = 0;'],
    ['init_right',  '    let right = s.length - 1;'],
    ['while_cond',  '    while (left < right) {'],
    ['if_mismatch', '        if (s[left] !== s[right]) {'],
    ['try_skip',    '            return isPalindrome(left + 1, right) || isPalindrome(left, right - 1);'],
    ['',            '        }'],
    ['inc_left',    '        left++;'],
    ['dec_right',   '        right--;'],
    ['',            '    }'],
    ['return_true', '    return true;'],
    ['',            '};'],
    ['',            ''],
    ['',            'function main() {'],
    ['m_read_s',    '    const s = lines[0];'],
    ['m_call',      '    console.log(validPalindrome(s));'],
    ['',            '}'],
    ['',            'main();']
  ],
  c: [
    ['',            '#include <stdio.h>'],
    ['',            '#include <string.h>'],
    ['',            '#include <stdbool.h>'],
    ['',            ''],
    ['ip_entry',    'bool isPalindrome(char* s, int lo, int hi) {'],
    ['ip_while',    '    while (lo < hi) {'],
    ['ip_if',       '        if (s[lo] != s[hi]) {'],
    ['ip_false',    '            return false;'],
    ['',            '        }'],
    ['ip_lo',       '        lo++;'],
    ['ip_hi',       '        hi--;'],
    ['',            '    }'],
    ['ip_true',     '    return true;'],
    ['',            '}'],
    ['',            ''],
    ['fn_entry',    'bool validPalindrome(char* s) {'],
    ['init_left',   '    int left = 0;'],
    ['init_right',  '    int right = strlen(s) - 1;'],
    ['while_cond',  '    while (left < right) {'],
    ['if_mismatch', '        if (s[left] != s[right]) {'],
    ['try_skip',    '            return isPalindrome(s, left + 1, right) || isPalindrome(s, left, right - 1);'],
    ['',            '        }'],
    ['inc_left',    '        left++;'],
    ['dec_right',   '        right--;'],
    ['',            '    }'],
    ['return_true', '    return true;'],
    ['',            '}'],
    ['',            ''],
    ['',            'int main() {'],
    ['m_read_s',    '    char s[200];'],
    ['m_scanner',   '    scanf("%s", s);'],
    ['m_call',      '    printf("%s\\n", validPalindrome(s) ? "true" : "false");'],
    ['',            '    return 0;'],
    ['',            '}']
  ]
};

const PSEUDOCODE = [
  'function isPalindrome(s, lo, hi):',
  '    while lo < hi:',
  '        if s[lo] != s[hi]:',
  '            return False',
  '        lo  += 1',
  '        hi  -= 1',
  '    return True',
  '',
  'function validPalindrome(s):',
  '    left  = 0',
  '    right = len(s) - 1',
  '',
  '    while left < right:',
  '        if s[left] != s[right]:',
  '            // try skipping one char',
  '            return isPalindrome(s, left+1, right)',
  '                or isPalindrome(s, left, right-1)',
  '        left  += 1',
  '        right -= 1',
  '',
  '    return True   // already a palindrome'
];

// ─── Step Builder ─────────────────────────────────────────────────────────────
function buildSteps(rawS) {
  const steps = [];
  const str = rawS.slice(0, 20).toLowerCase();
  const n   = str.length;

  const NONE = { leftReady: false, rightReady: false };
  const ALL  = { leftReady: true,  rightReady: true  };

  function snap(leftV, rightV, loV, hiV, mL, mR, skipMode, resultVal, code, phase, initState, extra) {
    return {
      left: leftV, right: rightV,
      lo: loV, hi: hiV,
      mismatchL: mL, mismatchR: mR,
      activeSkip: skipMode,
      resultVal,
      str, n,
      code, phase,
      initState: { ...initState },
      ...extra
    };
  }

  // ── Input phase ─────────────────────────────────────────────────────────────
  steps.push({
    ...snap(-1, -1, -1, -1, -1, -1, null, null, 'm_scanner', 'input', NONE, {}),
    badge: `Scanner sc = new Scanner(System.in); → Initializing input stream.`
  });
  steps.push({
    ...snap(-1, -1, -1, -1, -1, -1, null, null, 'm_read_s', 'input', NONE, {}),
    badge: `String s = sc.nextLine(); → Read s = "${str}" (length ${n}).`
  });
  steps.push({
    ...snap(-1, -1, -1, -1, -1, -1, null, null, 'm_call', 'input', NONE, {}),
    badge: `System.out.println(new Solution().validPalindrome(s)); → Calling validPalindrome().`
  });

  // ── validPalindrome entry ───────────────────────────────────────────────────
  steps.push({
    ...snap(-1, -1, -1, -1, -1, -1, null, null, 'fn_entry', 'init', NONE, {}),
    badge: `public boolean validPalindrome(String s) → Function entry. Two-pointer + at-most-1 deletion strategy.`
  });

  let left  = 0;
  let right = n - 1;

  steps.push({
    ...snap(0, -1, -1, -1, -1, -1, null, null, 'init_left', 'init', { leftReady: true, rightReady: false }, {}),
    badge: `int left = 0; → left = 0. Points to first character '${str[0]}'.`
  });
  steps.push({
    ...snap(0, right, -1, -1, -1, -1, null, null, 'init_right', 'init', ALL, {}),
    badge: `int right = s.length() - 1; → right = ${right}. Points to last character '${str[right]}'.`
  });

  // ── Helper: trace isPalindrome sub-calls ────────────────────────────────────
  function doPalSteps(loStart, hiStart, skipMode, mL, mR) {
    let lo = loStart;
    let hi = hiStart;

    steps.push({
      ...snap(left, right, lo, hi, mL, mR, skipMode, null, 'ip_entry', skipMode, ALL, {}),
      badge: `isPalindrome(s, ${lo}, ${hi}) → Checking if "${str.slice(lo, hi + 1)}" [indices ${lo}..${hi}] is a palindrome.`
    });

    while (lo < hi) {
      steps.push({
        ...snap(left, right, lo, hi, mL, mR, skipMode, null, 'ip_while', skipMode, ALL, {}),
        badge: `while (lo < hi): lo=${lo} < hi=${hi} → TRUE. Compare s[${lo}]='${str[lo]}' and s[${hi}]='${str[hi]}'.`
      });

      if (str[lo] !== str[hi]) {
        steps.push({
          ...snap(left, right, lo, hi, mL, mR, skipMode, null, 'ip_if', skipMode, ALL, { subMismatch: true }),
          badge: `if (s.charAt(lo) != s.charAt(hi)): '${str[lo]}' != '${str[hi]}' → TRUE. Inner mismatch found.`
        });
        steps.push({
          ...snap(left, right, lo, hi, mL, mR, skipMode, false, 'ip_false', skipMode, ALL, {}),
          badge: `return false; → "${str.slice(loStart, hiStart + 1)}" is NOT a palindrome.`
        });
        return false;
      }

      steps.push({
        ...snap(left, right, lo, hi, mL, mR, skipMode, null, 'ip_if', skipMode, ALL, { subMismatch: false }),
        badge: `if (s.charAt(lo) != s.charAt(hi)): '${str[lo]}' == '${str[hi]}' → FALSE. Characters match.`
      });

      lo++;
      steps.push({
        ...snap(left, right, lo, hi, mL, mR, skipMode, null, 'ip_lo', skipMode, ALL, {}),
        badge: `lo++; → lo = ${lo}.`
      });

      hi--;
      steps.push({
        ...snap(left, right, lo, hi, mL, mR, skipMode, null, 'ip_hi', skipMode, ALL, {}),
        badge: `hi--; → hi = ${hi}.`
      });
    }

    steps.push({
      ...snap(left, right, lo, hi, mL, mR, skipMode, null, 'ip_while', skipMode, ALL, {}),
      badge: `while (lo < hi): lo=${lo}, hi=${hi} → FALSE. All characters matched in this range!`
    });
    steps.push({
      ...snap(left, right, lo, hi, mL, mR, skipMode, true, 'ip_true', skipMode, ALL, {}),
      badge: `return true; → "${str.slice(loStart, hiStart + 1)}" IS a palindrome.`
    });
    return true;
  }

  // ── Main while loop ────────────────────────────────────────────────────────
  while (left < right) {
    steps.push({
      ...snap(left, right, -1, -1, -1, -1, null, null, 'while_cond', 'main', ALL, {}),
      badge: `while (left < right): left=${left} < right=${right} → TRUE. Compare s[${left}]='${str[left]}' and s[${right}]='${str[right]}'.`
    });

    if (str[left] !== str[right]) {
      const mL = left;
      const mR = right;

      steps.push({
        ...snap(left, right, -1, -1, mL, mR, null, null, 'if_mismatch', 'main', ALL, { subMismatch: true }),
        badge: `if (s.charAt(left) != s.charAt(right)): '${str[left]}' != '${str[right]}' → TRUE. Mismatch! Try deleting one character.`
      });

      // Show the try_skip line (the return statement)
      steps.push({
        ...snap(left, right, -1, -1, mL, mR, null, null, 'try_skip', 'main', ALL, {}),
        badge: `return isPalindrome(s, left+1, right) || isPalindrome(s, left, right-1); → First, try skipping left[${mL}]='${str[mL]}'.`
      });

      // Try skip-left
      const res1 = doPalSteps(mL + 1, mR, 'isPal_skipL', mL, mR);

      if (res1) {
        steps.push({
          ...snap(left, right, -1, -1, mL, mR, 'isPal_skipL', true, 'try_skip', 'done', ALL, { finalResult: true }),
          badge: `return true; → Skipping '${str[mL]}' at [${mL}] makes the rest a palindrome. Answer: true.`
        });
        return steps;
      }

      // Skip-left failed; try skip-right
      steps.push({
        ...snap(left, right, -1, -1, mL, mR, null, null, 'try_skip', 'main', ALL, {}),
        badge: `... || isPalindrome(s, left, right-1); → Skip-left failed. Now try skipping right[${mR}]='${str[mR]}'.`
      });

      const res2 = doPalSteps(mL, mR - 1, 'isPal_skipR', mL, mR);
      const finalRes = res2;

      steps.push({
        ...snap(left, right, -1, -1, mL, mR, 'isPal_skipR', finalRes, 'try_skip', 'done', ALL, { finalResult: finalRes }),
        badge: `return ${finalRes}; → Skipping '${str[mR]}' at [${mR}] makes it${finalRes ? '' : ' NOT'} a palindrome. Answer: ${finalRes}.`
      });
      return steps;
    }

    steps.push({
      ...snap(left, right, -1, -1, -1, -1, null, null, 'if_mismatch', 'main', ALL, { subMismatch: false }),
      badge: `if (s.charAt(left) != s.charAt(right)): '${str[left]}' == '${str[right]}' → FALSE. Characters match. Move both pointers.`
    });

    left++;
    steps.push({
      ...snap(left, right, -1, -1, -1, -1, null, null, 'inc_left', 'main', ALL, {}),
      badge: `left++; → left = ${left}.`
    });

    right--;
    steps.push({
      ...snap(left, right, -1, -1, -1, -1, null, null, 'dec_right', 'main', ALL, {}),
      badge: `right--; → right = ${right}.`
    });
  }

  steps.push({
    ...snap(left, right, -1, -1, -1, -1, null, null, 'while_cond', 'done', ALL, {}),
    badge: `while (left < right): left=${left}, right=${right} → FALSE. Pointers met. No mismatches found!`
  });
  steps.push({
    ...snap(left, right, -1, -1, -1, -1, null, true, 'return_true', 'done', ALL, { finalResult: true }),
    badge: `return true; → The string is already a palindrome. Answer: true.`
  });

  return steps;
}

// ─── Reactive State ───────────────────────────────────────────────────────────
const inputS      = ref('abcca');
const lang        = ref('java');
const speed       = ref(650);
const si          = ref(0);
const playing     = ref(false);
const vizHeight   = ref(360);
const tableHeight = ref(60);
const leftWidth   = ref(52);
const rightTab    = ref('code');

const stepsData = reactive({ steps: buildSteps('abcca') });
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
  const raw = inputS.value.trim().replace(/[^a-zA-Z0-9]/g, '').slice(0, 20);
  if (raw.length < 2) {
    alert('Please enter at least 2 characters.');
    return;
  }
  playing.value = false;
  stepsData.steps = buildSteps(raw);
  si.value = 0;
}

function stepBy(d) {
  si.value = Math.max(0, Math.min(steps.value.length - 1, si.value + d));
}

function togglePlay() {
  if (!playing.value && si.value >= steps.value.length - 1) {
    si.value = 0;
  }
  playing.value = !playing.value;
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
    const contRect  = container.getBoundingClientRect();
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
const initState    = computed(() => s.value.initState || { leftReady: false, rightReady: false });
const displayLeft  = computed(() => initState.value.leftReady  ? s.value.left  : '?');
const displayRight = computed(() => initState.value.rightReady ? s.value.right : '?');
const displayLo    = computed(() => s.value.lo >= 0 ? s.value.lo : '-');
const displayHi    = computed(() => s.value.hi >= 0 ? s.value.hi : '-');
const displayStr   = computed(() => s.value.str || inputS.value);
const displayN     = computed(() => s.value.n || 0);
const displaySkip  = computed(() => {
  if (s.value.activeSkip === 'isPal_skipL') {
    return `Skip left[${s.value.mismatchL}]`;
  }
  if (s.value.activeSkip === 'isPal_skipR') {
    return `Skip right[${s.value.mismatchR}]`;
  }
  return '—';
});
const displayResult = computed(() => {
  if (s.value.resultVal === true) {
    return 'true';
  }
  if (s.value.resultVal === false) {
    return 'false';
  }
  return '—';
});
const inIsPalPhase = computed(() => {
  return s.value.phase === 'isPal_skipL' || s.value.phase === 'isPal_skipR';
});

function charClass(idx) {
  const phase = s.value.phase;
  const ist   = initState.value;
  if (phase === 'input' || !ist.leftReady) {
    return '';
  }
  const left = s.value.left;
  const right = s.value.right;
  const lo    = s.value.lo;
  const hi    = s.value.hi;
  const mL    = s.value.mismatchL;
  const mR    = s.value.mismatchR;

  if (phase === 'done') {
    return s.value.finalResult ? 'vp-char-done-true' : 'vp-char-done-false';
  }

  if (phase === 'init' || phase === 'main') {
    if (!ist.rightReady) {
      return '';
    }
    if (idx < left || idx > right) {
      return 'vp-char-processed';
    }
    if (idx === left && idx === right) {
      return 'vp-char-left';
    }
    if (idx === left) {
      return 'vp-char-left';
    }
    if (idx === right) {
      return 'vp-char-right';
    }
    return 'vp-char-inner';
  }

  // isPal phase
  if (phase === 'isPal_skipL' || phase === 'isPal_skipR') {
    const skippedIdx = phase === 'isPal_skipL' ? mL : mR;
    const rangeStart = phase === 'isPal_skipL' ? mL + 1 : mL;
    const rangeEnd   = phase === 'isPal_skipL' ? mR     : mR - 1;

    if (idx < left || idx > right) {
      return 'vp-char-processed';
    }
    if (idx === skippedIdx) {
      return 'vp-char-skipped';
    }
    if (idx < rangeStart || idx > rangeEnd) {
      return 'vp-char-processed';
    }
    if (lo >= 0 && hi >= 0) {
      if (idx === lo && idx === hi) {
        return 'vp-char-lo';
      }
      if (idx === lo) {
        return 'vp-char-lo';
      }
      if (idx === hi) {
        return 'vp-char-hi';
      }
      if (idx < lo || idx > hi) {
        return 'vp-char-sub-processed';
      }
    }
    return 'vp-char-in-range';
  }

  return '';
}

function showMainLeft(idx) {
  const phase = s.value.phase;
  const ist   = initState.value;
  return (phase === 'main' || phase === 'init') && ist.leftReady && ist.rightReady && idx === s.value.left;
}

function showMainRight(idx) {
  const phase = s.value.phase;
  const ist   = initState.value;
  return (phase === 'main' || phase === 'init') && ist.rightReady && idx === s.value.right && idx !== s.value.left;
}

function showLo(idx) {
  return inIsPalPhase.value && s.value.lo >= 0 && idx === s.value.lo;
}

function showHi(idx) {
  return inIsPalPhase.value && s.value.hi >= 0 && idx === s.value.hi && idx !== s.value.lo;
}

function showSkipped(idx) {
  if (!inIsPalPhase.value) {
    return false;
  }
  const mL  = s.value.mismatchL;
  const mR  = s.value.mismatchR;
  const skip = s.value.phase === 'isPal_skipL' ? mL : mR;
  return idx === skip;
}

function showMismatch(idx) {
  const phase = s.value.phase;
  if (phase !== 'main') {
    return false;
  }
  const mL = s.value.mismatchL;
  const mR = s.value.mismatchR;
  return mL >= 0 && (idx === mL || idx === mR);
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
              <label>s:</label>
              <input
                type="text"
                v-model="inputS"
                class="ll-text-input vp-str-input"
                @keyup.enter="applyInput"
                placeholder="e.g. abcca"
                maxlength="20"
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
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">n</span><b class="ll-c-blue">{{ displayN }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">left</span><b class="ll-c-blue">{{ displayLeft }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">right</span><b class="ll-c-green">{{ displayRight }}</b></span>
                    <span class="ll-ptr-chip-inline" v-if="inIsPalPhase"><span class="ll-chip-label">lo</span><b class="ll-c-purple">{{ displayLo }}</b></span>
                    <span class="ll-ptr-chip-inline" v-if="inIsPalPhase"><span class="ll-chip-label">hi</span><b class="ll-c-orange">{{ displayHi }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">skip</span><b class="ll-c-orange">{{ displaySkip }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">result</span>
                      <b :class="{ 'll-c-result-true': displayResult === 'true', 'll-c-result-false': displayResult === 'false', 'll-c-muted': displayResult === '—' }">{{ displayResult }}</b>
                    </span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">phase</span><b class="ll-c-orange">{{ s.phase || 'input' }}</b></span>
                  </div>

                  <!-- BOARD CONTAINER -->
                  <div class="ll-board-container">

                    <!-- Phase Banner -->
                    <div
                      class="vp-phase-banner"
                      :class="{
                        'vp-phase-main':    s.phase === 'main' || s.phase === 'init',
                        'vp-phase-skipl':   s.phase === 'isPal_skipL',
                        'vp-phase-skipr':   s.phase === 'isPal_skipR',
                        'vp-phase-done-t':  s.phase === 'done' && s.finalResult === true,
                        'vp-phase-done-f':  s.phase === 'done' && s.finalResult === false,
                        'vp-phase-input':   s.phase === 'input'
                      }"
                    >
                      <template v-if="s.phase === 'input' || s.phase === 'init'">Reading input &amp; initializing</template>
                      <template v-else-if="s.phase === 'main'">validPalindrome() — Two-pointer scan</template>
                      <template v-else-if="s.phase === 'isPal_skipL'">isPalindrome() — Trying skip left[{{ s.mismatchL }}]='{{ displayStr[s.mismatchL] }}'</template>
                      <template v-else-if="s.phase === 'isPal_skipR'">isPalindrome() — Trying skip right[{{ s.mismatchR }}]='{{ displayStr[s.mismatchR] }}'</template>
                      <template v-else-if="s.phase === 'done' && s.finalResult === true">&#10003; Answer: <strong>true</strong> — valid palindrome (at most 1 deletion)</template>
                      <template v-else-if="s.phase === 'done' && s.finalResult === false">&#10007; Answer: <strong>false</strong> — cannot be made palindrome with 1 deletion</template>
                    </div>

                    <!-- Tier 1 — String Character Boxes -->
                    <div class="vp-tier-title">
                      Tier 1 &mdash; String s
                      <code>"{{ displayStr }}"</code>
                      <span class="vp-len-badge">(length {{ displayN }})</span>
                    </div>
                    <div class="vp-str-frame">
                      <!-- Pointer label row above boxes -->
                      <div class="vp-ptr-row">
                        <div
                          v-for="(ch, idx) in displayStr.split('')"
                          :key="'ptr-' + idx"
                          class="vp-ptr-cell"
                        >
                          <!-- Main-phase pointers -->
                          <span v-if="showMainLeft(idx)"  class="vp-ptr-label vp-ptr-left">left</span>
                          <span v-if="showMainRight(idx)" class="vp-ptr-label vp-ptr-right">right</span>
                          <!-- isPalindrome-phase pointers -->
                          <span v-if="showLo(idx)"      class="vp-ptr-label vp-ptr-lo">lo</span>
                          <span v-if="showHi(idx)"      class="vp-ptr-label vp-ptr-hi">hi</span>
                          <span v-if="showSkipped(idx)" class="vp-ptr-label vp-ptr-skip">skip</span>
                          <!-- Mismatch indicator in main phase -->
                          <span v-if="showMismatch(idx)" class="vp-ptr-label vp-ptr-mismatch">&#x2260;</span>
                        </div>
                      </div>

                      <!-- Character boxes -->
                      <div class="vp-char-row">
                        <div
                          v-for="(ch, idx) in displayStr.split('')"
                          :key="'ch-' + idx"
                          class="vp-char-box"
                          :class="charClass(idx)"
                        >
                          <span class="vp-char-val">{{ ch }}</span>
                          <span class="vp-char-idx">[{{ idx }}]</span>
                        </div>
                      </div>
                    </div>

                    <!-- Tier 2 — Variable State Panel -->
                    <div class="vp-tier-title">Tier 2 &mdash; Variable State</div>
                    <div class="vp-var-panel">
                      <!-- Main pointers -->
                      <div class="vp-var-card" :class="{ 'vp-var-active-l': initState.leftReady }">
                        <span class="vp-var-name vp-name-left">left</span>
                        <span class="vp-var-val">{{ displayLeft }}</span>
                        <span class="vp-var-desc">outer left</span>
                      </div>
                      <div class="vp-var-card" :class="{ 'vp-var-active-r': initState.rightReady }">
                        <span class="vp-var-name vp-name-right">right</span>
                        <span class="vp-var-val">{{ displayRight }}</span>
                        <span class="vp-var-desc">outer right</span>
                      </div>

                      <!-- isPalindrome inner pointers (always shown, dim when inactive) -->
                      <div class="vp-var-card" :class="{ 'vp-var-active-lo': inIsPalPhase, 'vp-var-dim': !inIsPalPhase }">
                        <span class="vp-var-name vp-name-lo">lo</span>
                        <span class="vp-var-val">{{ displayLo }}</span>
                        <span class="vp-var-desc">inner left</span>
                      </div>
                      <div class="vp-var-card" :class="{ 'vp-var-active-hi': inIsPalPhase, 'vp-var-dim': !inIsPalPhase }">
                        <span class="vp-var-name vp-name-hi">hi</span>
                        <span class="vp-var-val">{{ displayHi }}</span>
                        <span class="vp-var-desc">inner right</span>
                      </div>

                      <!-- Skip mode card -->
                      <div class="vp-var-card" :class="{ 'vp-var-active-skip': inIsPalPhase }">
                        <span class="vp-var-name vp-name-skip">skip</span>
                        <span class="vp-var-val" style="font-size:11px;font-weight:700;">{{ displaySkip }}</span>
                        <span class="vp-var-desc">deletion trial</span>
                      </div>

                      <!-- Result card -->
                      <div
                        class="vp-var-card"
                        :class="{
                          'vp-var-result-true':  displayResult === 'true',
                          'vp-var-result-false': displayResult === 'false'
                        }"
                      >
                        <span class="vp-var-name vp-name-result">result</span>
                        <span
                          class="vp-var-val"
                          :style="{ color: displayResult === 'true' ? '#15803d' : displayResult === 'false' ? '#ef4444' : '#94a3b8' }"
                        >{{ displayResult }}</span>
                        <span class="vp-var-desc">final answer</span>
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
                <span class="ll-leg"><span class="ll-legdot" style="background:#bfdbfe;border:1.5px solid #3b82f6;"></span>left</span>
                <span class="ll-leg"><span class="ll-legdot" style="background:#bbf7d0;border:1.5px solid #22c55e;"></span>right</span>
                <span class="ll-leg"><span class="ll-legdot vp-legdot-lo"></span>lo</span>
                <span class="ll-leg"><span class="ll-legdot vp-legdot-hi"></span>hi</span>
                <span class="ll-leg"><span class="ll-legdot vp-legdot-skip"></span>skip</span>
                <span class="ll-leg"><span class="ll-legdot vp-legdot-mismatch"></span>mismatch</span>
                <span class="ll-leg"><span class="ll-legdot" style="background:#f1f5f9;border:1.5px solid #cbd5e1;opacity:.6"></span>processed</span>
              </div>

              <!-- Call Stack Area -->
              <div class="ll-table-area" :style="{ height: tableHeight + 'px' }">
                <div class="ll-table-title">Call Stack &mdash; Current Execution Frame</div>
                <div class="ll-stack-line">
                  <template v-if="s.phase === 'input'">
                    <div class="ll-frame ll-frame-cur">
                      main()
                      &nbsp;
                      <span class="ll-fname">s</span>=<span class="ll-c-blue" style="font-weight:700">"{{ displayStr }}"</span>
                      <span class="ll-now"> &#9668; active</span>
                    </div>
                  </template>
                  <template v-else-if="s.phase === 'init' || s.phase === 'main' || s.phase === 'done'">
                    <div class="ll-frame" style="color:var(--text2);">main()</div>
                    <div class="ll-frame ll-frame-cur" style="margin-left:14px">
                      validPalindrome("{{ displayStr }}")
                      &nbsp;
                      <span class="ll-fname">left</span>=<span class="ll-c-blue" style="font-weight:700">{{ displayLeft }}</span>,
                      <span class="ll-fname">right</span>=<span class="ll-c-green" style="font-weight:700">{{ displayRight }}</span>
                      <span class="ll-now"> &#9668; active</span>
                    </div>
                  </template>
                  <template v-else>
                    <div class="ll-frame" style="color:var(--text2);">main()</div>
                    <div class="ll-frame" style="color:var(--text2);margin-left:14px">validPalindrome("{{ displayStr }}")</div>
                    <div class="ll-frame ll-frame-cur" style="margin-left:28px">
                      isPalindrome(s, {{ displayLo }}, {{ displayHi }})
                      &nbsp;
                      <span class="ll-fname">lo</span>=<span class="ll-c-purple" style="font-weight:700">{{ displayLo }}</span>,
                      <span class="ll-fname">hi</span>=<span class="ll-c-orange" style="font-weight:700">{{ displayHi }}</span>
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
                    'll-badge-error':   s.badge && (s.badge.includes('FALSE') || s.badge.includes('NOT') || s.badge.includes('false')),
                    'll-badge-success': s.badge && (s.badge.includes('true') || s.badge.includes('match') || s.badge.includes('palindrome') || s.badge.includes('TRUE'))
                  }"
                >
                  {{ s.badge || 'Ready to visualize Valid Palindrome II.' }}
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
                  <h3 class="ll-cx-heading">Valid Palindrome II (LeetCode 680) &mdash; Complexity Analysis</h3>
                  <p class="ll-cx-intro">
                    Given string <code>s</code>, return <code>true</code> if it can be a palindrome after deleting
                    <strong>at most one</strong> character. The two-pointer approach compares <code>s[left]</code>
                    and <code>s[right]</code> moving inward. On the first mismatch, try both possible single-char
                    deletions using a helper <code>isPalindrome()</code>.
                  </p>

                  <h4 class="ll-cx-sub">Step-by-step Breakdown</h4>
                  <table class="ll-complexity-table">
                    <thead>
                      <tr><th>Operation</th><th>Time</th><th>Space</th><th>Reason</th></tr>
                    </thead>
                    <tbody>
                      <tr><td>Init left, right</td><td class="ll-cx-good">O(1)</td><td class="ll-cx-good">O(1)</td><td>Two assignments</td></tr>
                      <tr><td>Outer while (no mismatch)</td><td class="ll-cx-good">O(n)</td><td class="ll-cx-good">O(1)</td><td>At most n/2 iterations</td></tr>
                      <tr><td>isPalindrome (on mismatch)</td><td class="ll-cx-good">O(n)</td><td class="ll-cx-good">O(1)</td><td>Called at most twice, each scans O(n)</td></tr>
                      <tr><td><strong>Total</strong></td><td class="ll-cx-good"><strong>O(n)</strong></td><td class="ll-cx-good"><strong>O(1)</strong></td><td>Linear overall, constant extra space</td></tr>
                    </tbody>
                  </table>

                  <h4 class="ll-cx-sub">Overall Complexity</h4>
                  <div class="ll-cx-summary-grid">
                    <div class="ll-cx-card ll-cx-card-good">
                      <div class="ll-cx-card-label">Time</div>
                      <div class="ll-cx-card-val">O(n)</div>
                      <div class="ll-cx-card-note">Single pass + at most 2 sub-scans</div>
                    </div>
                    <div class="ll-cx-card ll-cx-card-good">
                      <div class="ll-cx-card-label">Space</div>
                      <div class="ll-cx-card-val">O(1)</div>
                      <div class="ll-cx-card-note">Only integer pointers used</div>
                    </div>
                    <div class="ll-cx-card ll-cx-card-good">
                      <div class="ll-cx-card-label">Deletions</div>
                      <div class="ll-cx-card-val">&le; 1</div>
                      <div class="ll-cx-card-note">At most one character removed</div>
                    </div>
                  </div>

                  <div class="ll-note">
                    <strong>Key Insight:</strong> The outer loop finds the first mismatch at positions <code>[left, right]</code>.
                    At that point, exactly one of the following must be a palindrome for the answer to be <code>true</code>:
                    <code>s[left+1..right]</code> (skip left) or <code>s[left..right-1]</code> (skip right).
                    If neither is, the answer is <code>false</code>. No further deletions are tried.
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

@keyframes vp-flash-match    { 0%{background:#fef9c3;} 60%{background:#bbf7d0;} 100%{} }
@keyframes vp-flash-mismatch { 0%{background:#fee2e2;transform:scale(1.1);} 100%{transform:scale(1);} }
@keyframes vp-pulse-result   { 0%{transform:scale(1);} 50%{transform:scale(1.05);} 100%{transform:scale(1);} }

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
.vp-str-input    { width: 180px; }
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
.ll-c-muted  { color: var(--muted); }
.ll-c-result-true  { color: #15803d; font-weight: 800; }
.ll-c-result-false { color: #dc2626; font-weight: 800; }

/* ─── Board Container — Valid Palindrome II ──────────────────────────────── */
.ll-board-container { display: flex; flex-direction: column; align-items: stretch; padding: 6px 14px 10px; gap: 8px; min-width: 0; }

/* Phase banner */
.vp-phase-banner {
  font-size: 11px; font-weight: 700; padding: 5px 12px; border-radius: var(--radius-sm);
  border-left: 3px solid var(--muted); color: var(--text2); background: var(--surface2);
  transition: all .2s;
}
.vp-phase-input  { border-color: var(--muted); }
.vp-phase-main   { border-color: var(--blue); color: #1d4ed8; background: #eff6ff; }
.vp-phase-skipl  { border-color: var(--purple); color: #7c3aed; background: var(--purple-light); }
.vp-phase-skipr  { border-color: var(--orange); color: #c2410c; background: var(--orange-light); }
.vp-phase-done-t { border-color: var(--green); color: #15803d; background: var(--green-light); animation: vp-pulse-result .4s ease; }
.vp-phase-done-f { border-color: var(--red); color: var(--red-dark); background: var(--red-light); animation: vp-pulse-result .4s ease; }

.vp-tier-title {
  font-size: 10px; font-weight: 700; text-transform: uppercase; letter-spacing: .04em;
  color: var(--muted); margin-top: 4px; margin-bottom: 2px; font-family: 'Consolas', monospace;
  border-left: 3px solid var(--coral); padding-left: 6px; display: flex; align-items: center; gap: 6px;
}
.vp-len-badge { font-size: 9px; color: var(--muted); background: var(--surface2); border: 1px solid var(--border); border-radius: 3px; padding: 0 4px; font-weight: 500; text-transform: none; letter-spacing: 0; }

/* String frame */
.vp-str-frame {
  display: flex; flex-direction: column; gap: 2px;
  background: #f8fafc; border: 1px solid var(--border);
  border-radius: var(--radius); padding: 6px 10px 8px; box-shadow: var(--shadow-sm);
}

/* Pointer row */
.vp-ptr-row { display: flex; gap: 4px; min-height: 18px; align-items: flex-end; margin-bottom: 2px; }
.vp-ptr-cell { width: 36px; display: flex; flex-direction: column; align-items: center; justify-content: flex-end; flex-shrink: 0; gap: 1px; }
.vp-ptr-label { font-size: 8px; font-weight: 800; font-family: monospace; padding: 1px 3px; border-radius: 3px; line-height: 1.2; white-space: nowrap; }
.vp-ptr-left     { background: #eff6ff; color: #1d4ed8; border: 1px solid #93c5fd; }
.vp-ptr-right    { background: #f0fdf4; color: #15803d; border: 1px solid #86efac; }
.vp-ptr-lo       { background: #f3e8ff; color: #7c3aed; border: 1px solid #c4b5fd; }
.vp-ptr-hi       { background: #fff7ed; color: #c2410c; border: 1px solid #fdba74; }
.vp-ptr-skip     { background: #fee2e2; color: #991b1b; border: 1px solid #fca5a5; text-decoration: line-through; }
.vp-ptr-mismatch { background: #fee2e2; color: #dc2626; border: 1px solid #fca5a5; }

/* Character row */
.vp-char-row { display: flex; flex-wrap: wrap; gap: 4px; }

/* Character boxes */
.vp-char-box {
  width: 36px; height: 44px; border-radius: var(--radius-sm);
  background: #e2e8f0; border: 1.5px solid #94a3b8;
  display: flex; flex-direction: column; align-items: center; justify-content: center;
  gap: 2px; flex-shrink: 0; transition: all .18s ease; cursor: default;
}
.vp-char-val { font-family: 'Cascadia Code', 'Fira Code', monospace; font-size: 16px; font-weight: 900; color: var(--text); line-height: 1; }
.vp-char-idx { font-family: monospace; font-size: 7.5px; color: var(--muted); font-weight: 600; }

/* Character state classes */
.vp-char-processed    { opacity: .35; background: #f8fafc !important; border-color: #e2e8f0 !important; }
.vp-char-inner        { background: #f1f5f9 !important; }
.vp-char-left         { background: #bfdbfe !important; border: 2px solid var(--blue) !important; box-shadow: 0 0 8px rgba(59,130,246,.35); }
.vp-char-right        { background: #bbf7d0 !important; border: 2px solid var(--green) !important; box-shadow: 0 0 8px rgba(34,197,94,.35); }
.vp-char-lo           { background: #ddd6fe !important; border: 2px solid var(--purple) !important; box-shadow: 0 0 7px rgba(147,51,234,.3); }
.vp-char-hi           { background: #fed7aa !important; border: 2px solid var(--orange) !important; box-shadow: 0 0 7px rgba(249,115,22,.3); }
.vp-char-in-range     { background: #fef9c3 !important; border-color: #fbbf24 !important; }
.vp-char-sub-processed{ opacity: .45; background: #f1f5f9 !important; border-color: #cbd5e1 !important; }
.vp-char-skipped      { background: #fee2e2 !important; border: 2px dashed #f87171 !important; opacity: .5; }
.vp-char-done-true    { background: #dcfce7 !important; border: 1.5px solid #86efac !important; }
.vp-char-done-false   { background: #fee2e2 !important; border: 1.5px solid #fca5a5 !important; }

/* Variable state panel */
.vp-var-panel { display: flex; gap: 6px; flex-wrap: wrap; }
.vp-var-card {
  display: flex; flex-direction: column; align-items: center; gap: 2px;
  padding: 5px 10px; border-radius: var(--radius-sm); border: 1.5px solid var(--border);
  background: var(--surface2); min-width: 68px; font-size: 10.5px; font-family: monospace;
  transition: all .15s;
}
.vp-var-dim { opacity: .4; }
.vp-var-active-l    { border-color: var(--blue) !important;   background: #eff6ff !important; }
.vp-var-active-r    { border-color: var(--green) !important;  background: #f0fdf4 !important; }
.vp-var-active-lo   { border-color: var(--purple) !important; background: var(--purple-light) !important; }
.vp-var-active-hi   { border-color: var(--orange) !important; background: var(--orange-light) !important; }
.vp-var-active-skip { border-color: #f87171 !important; background: #fee2e2 !important; }
.vp-var-result-true  { border-color: var(--green) !important; background: var(--green-light) !important; }
.vp-var-result-false { border-color: var(--red) !important;   background: var(--red-light) !important; }
.vp-var-name   { font-size: 9.5px; font-weight: 800; padding: 1px 5px; border-radius: 3px; }
.vp-var-val    { font-size: 17px; font-weight: 900; color: var(--text); line-height: 1.2; }
.vp-var-desc   { font-size: 8.5px; color: var(--muted); text-align: center; }
.vp-name-left   { background: #eff6ff; color: #1d4ed8; border: 1px solid #93c5fd; }
.vp-name-right  { background: #f0fdf4; color: #15803d; border: 1px solid #86efac; }
.vp-name-lo     { background: #f3e8ff; color: #7c3aed; border: 1px solid #c4b5fd; }
.vp-name-hi     { background: #fff7ed; color: #c2410c; border: 1px solid #fdba74; }
.vp-name-skip   { background: #fee2e2; color: #991b1b; border: 1px solid #fca5a5; }
.vp-name-result { background: var(--surface2); color: var(--text2); border: 1px solid var(--border2); }

/* Legend dots */
.vp-legdot-lo       { background: #ddd6fe; border: 1.5px solid #8b5cf6; display: inline-block; width: 11px; height: 11px; border-radius: 3px; }
.vp-legdot-hi       { background: #fed7aa; border: 1.5px solid #f97316; display: inline-block; width: 11px; height: 11px; border-radius: 3px; }
.vp-legdot-skip     { background: #fee2e2; border: 1.5px dashed #f87171; display: inline-block; width: 11px; height: 11px; border-radius: 3px; }
.vp-legdot-mismatch { background: #fee2e2; border: 1.5px solid #f87171; display: inline-block; width: 11px; height: 11px; border-radius: 3px; }

/* Shared helpers */
.ll-vresizer { height: 5px; cursor: row-resize; background: var(--border); flex-shrink: 0; transition: background .15s; position: relative; z-index: 20; }
.ll-vresizer:hover, .ll-vresizer.drag { background: var(--coral); }
.ll-legend { display: flex; flex-wrap: wrap; gap: 6px 14px; padding: 6px 12px; border-bottom: 1px solid var(--border); flex-shrink: 0; background: var(--surface2); }
.ll-leg    { display: flex; align-items: center; gap: 5px; font-size: 11px; color: var(--text2); font-weight: 500; }
.ll-legdot { width: 11px; height: 11px; border-radius: 3px; flex-shrink: 0; display: inline-block; }

.ll-table-area  { flex-shrink: 0; padding: 8px 14px; border-bottom: 1px solid var(--border); overflow-x: hidden; overflow-y: auto; background: var(--surface); min-width: 0; box-sizing: border-box; }
.ll-table-title { font-size: 10px; color: var(--muted); margin-bottom: 4px; font-style: italic; }
.ll-stack-line  { font-family: 'Consolas', monospace; font-size: 12px; line-height: 1.8; }
.ll-frame       { font-family: 'Consolas', monospace; font-size: 11px; color: var(--text2); padding: 1px 0; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
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
