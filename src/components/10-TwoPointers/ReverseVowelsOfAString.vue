<script setup>
import { ref, reactive, computed, onMounted, onBeforeUnmount, watch, nextTick } from 'vue';

defineProps({
  topic:    { type: String, default: 'Two Pointer Algorithms' },
  subTopic: { type: String, default: 'Reverse Vowels of a String (LeetCode 345)' }
});

const CODES = {
  java: [
    ['',             'class Solution {'],
    ['fn_entry',     '    public String reverseVowels(String s) {'],
    ['to_char',      '        char[] chars = s.toCharArray();'],
    ['init_left',    '        int left = 0;'],
    ['init_right',   '        int right = chars.length - 1;'],
    ['outer_while',  '        while (left < right) {'],
    ['scan_l_cond',  '            while (left < right && !isVowel(chars[left])) {'],
    ['scan_l_inc',   '                left++;'],
    ['',             '            }'],
    ['scan_r_cond',  '            while (left < right && !isVowel(chars[right])) {'],
    ['scan_r_dec',   '                right--;'],
    ['',             '            }'],
    ['if_swap',      '            if (left < right) {'],
    ['swap_temp',    '                char temp = chars[left];'],
    ['swap_assign1', '                chars[left] = chars[right];'],
    ['swap_assign2', '                chars[right] = temp;'],
    ['inc_left',     '                left++;'],
    ['dec_right',    '                right--;'],
    ['',             '            }'],
    ['',             '        }'],
    ['return_stmt',  '        return new String(chars);'],
    ['',             '    }'],
    ['',             ''],
    ['is_vowel',     '    private boolean isVowel(char c) {'],
    ['',             '        return "aeiouAEIOU".indexOf(c) != -1;'],
    ['',             '    }'],
    ['',             ''],
    ['',             '    public static void main(String[] args) {'],
    ['m_scanner',    '        java.util.Scanner sc = new java.util.Scanner(System.in);'],
    ['m_read_s',     '        String s = sc.next();'],
    ['m_call',       '        String result = new Solution().reverseVowels(s);'],
    ['m_print',      '        System.out.println(result);'],
    ['',             '    }'],
    ['',             '}']
  ],
  cpp: [
    ['',             '#include <string>'],
    ['',             '#include <iostream>'],
    ['',             'using namespace std;'],
    ['',             ''],
    ['',             'class Solution {'],
    ['',             'public:'],
    ['fn_entry',     '    string reverseVowels(string s) {'],
    ['init_left',    '        int left = 0;'],
    ['init_right',   '        int right = s.size() - 1;'],
    ['outer_while',  '        while (left < right) {'],
    ['scan_l_cond',  '            while (left < right && !isVowel(s[left])) {'],
    ['scan_l_inc',   '                left++;'],
    ['',             '            }'],
    ['scan_r_cond',  '            while (left < right && !isVowel(s[right])) {'],
    ['scan_r_dec',   '                right--;'],
    ['',             '            }'],
    ['if_swap',      '            if (left < right) {'],
    ['swap_temp',    '                char temp = s[left];'],
    ['swap_assign1', '                s[left] = s[right];'],
    ['swap_assign2', '                s[right] = temp;'],
    ['inc_left',     '                left++;'],
    ['dec_right',    '                right--;'],
    ['',             '            }'],
    ['',             '        }'],
    ['return_stmt',  '        return s;'],
    ['',             '    }'],
    ['',             ''],
    ['is_vowel',     '    bool isVowel(char c) {'],
    ['',             '        string v = "aeiouAEIOU";'],
    ['',             '        return v.find(c) != string::npos;'],
    ['',             '    }'],
    ['',             '};'],
    ['',             ''],
    ['',             'int main() {'],
    ['m_read_s',     '    string s;'],
    ['m_scanner',    '    cin >> s;'],
    ['m_call',       '    cout << Solution().reverseVowels(s) << endl;'],
    ['',             '    return 0;'],
    ['',             '}']
  ],
  python: [
    ['',             'class Solution:'],
    ['fn_entry',     '    def reverseVowels(self, s):'],
    ['to_char',      '        chars = list(s)'],
    ['init_left',    '        left = 0'],
    ['init_right',   '        right = len(chars) - 1'],
    ['outer_while',  '        while left < right:'],
    ['scan_l_cond',  '            while left < right and chars[left] not in "aeiouAEIOU":'],
    ['scan_l_inc',   '                left += 1'],
    ['scan_r_cond',  '            while left < right and chars[right] not in "aeiouAEIOU":'],
    ['scan_r_dec',   '                right -= 1'],
    ['if_swap',      '            if left < right:'],
    ['swap_temp',    '                chars[left], chars[right] = chars[right], chars[left]'],
    ['inc_left',     '                left += 1'],
    ['dec_right',    '                right -= 1'],
    ['return_stmt',  '        return "".join(chars)'],
    ['',             ''],
    ['',             'def main():'],
    ['m_read_s',     '    s = input()'],
    ['m_call',       '    print(Solution().reverseVowels(s))'],
    ['',             ''],
    ['',             'main()']
  ],
  javascript: [
    ['',             '/**'],
    ['',             ' * @param {string} s'],
    ['',             ' * @return {string}'],
    ['',             ' */'],
    ['fn_entry',     'var reverseVowels = function(s) {'],
    ['to_char',      '    const chars = s.split(\'\');'],
    ['init_left',    '    let left = 0;'],
    ['init_right',   '    let right = chars.length - 1;'],
    ['outer_while',  '    while (left < right) {'],
    ['scan_l_cond',  '        while (left < right && !isVowel(chars[left])) {'],
    ['scan_l_inc',   '            left++;'],
    ['',             '        }'],
    ['scan_r_cond',  '        while (left < right && !isVowel(chars[right])) {'],
    ['scan_r_dec',   '            right--;'],
    ['',             '        }'],
    ['if_swap',      '        if (left < right) {'],
    ['swap_temp',    '            let temp = chars[left];'],
    ['swap_assign1', '            chars[left] = chars[right];'],
    ['swap_assign2', '            chars[right] = temp;'],
    ['inc_left',     '            left++;'],
    ['dec_right',    '            right--;'],
    ['',             '        }'],
    ['',             '    }'],
    ['return_stmt',  '    return chars.join(\'\');'],
    ['',             '};'],
    ['',             ''],
    ['is_vowel',     'function isVowel(c) {'],
    ['',             '    return "aeiouAEIOU".includes(c);'],
    ['',             '}'],
    ['',             ''],
    ['',             'function main() {'],
    ['m_read_s',     '    const s = "hello";'],
    ['m_call',       '    console.log(reverseVowels(s));'],
    ['',             '}'],
    ['',             'main();']
  ],
  c: [
    ['',             '#include <stdio.h>'],
    ['',             '#include <string.h>'],
    ['',             ''],
    ['is_vowel',     'int isVowel(char c) {'],
    ['',             '    char* v = "aeiouAEIOU";'],
    ['',             '    return strchr(v, c) != NULL;'],
    ['',             '}'],
    ['',             ''],
    ['fn_entry',     'char* reverseVowels(char* s) {'],
    ['init_left',    '    int left = 0;'],
    ['init_right',   '    int right = strlen(s) - 1;'],
    ['outer_while',  '    while (left < right) {'],
    ['scan_l_cond',  '        while (left < right && !isVowel(s[left])) {'],
    ['scan_l_inc',   '            left++;'],
    ['',             '        }'],
    ['scan_r_cond',  '        while (left < right && !isVowel(s[right])) {'],
    ['scan_r_dec',   '            right--;'],
    ['',             '        }'],
    ['if_swap',      '        if (left < right) {'],
    ['swap_temp',    '            char temp = s[left];'],
    ['swap_assign1', '            s[left] = s[right];'],
    ['swap_assign2', '            s[right] = temp;'],
    ['inc_left',     '            left++;'],
    ['dec_right',    '            right--;'],
    ['',             '        }'],
    ['',             '    }'],
    ['return_stmt',  '    return s;'],
    ['',             '}'],
    ['',             ''],
    ['',             'int main() {'],
    ['m_read_s',     '    char s[300];'],
    ['m_scanner',    '    scanf("%s", s);'],
    ['m_call',       '    printf("%s\\n", reverseVowels(s));'],
    ['',             '    return 0;'],
    ['',             '}']
  ]
};

const PSEUDOCODE = [
  'function reverseVowels(s):',
  '    chars = list(s)',
  '    left = 0',
  '    right = len(chars) - 1',
  '',
  '    while left < right:',
  '        // scan left for vowel',
  '        while left < right and not isVowel(chars[left]):',
  '            left++',
  '        // scan right for vowel',
  '        while left < right and not isVowel(chars[right]):',
  '            right--',
  '        // swap if both are vowels',
  '        if left < right:',
  '            swap(chars[left], chars[right])',
  '            left++; right--',
  '',
  '    return join(chars)',
  '',
  'isVowel(c): return c in "aeiouAEIOU"'
];

// ─── Step Builder ─────────────────────────────────────────────────────────────
const VOWELS = new Set(['a','e','i','o','u','A','E','I','O','U']);
function isVowel(c) { return VOWELS.has(c); }

function buildSteps(rawS) {
  const s = rawS.trim().slice(0, 16);
  if (!s) return [];
  const steps = [];
  const N = s.length;

  const NONE = {leftReady:false, rightReady:false, tempReady:false, charsReady:false};
  const CH   = {leftReady:false, rightReady:false, tempReady:false, charsReady:true};
  const L    = {leftReady:true,  rightReady:false, tempReady:false, charsReady:true};
  const LR   = {leftReady:true,  rightReady:true,  tempReady:false, charsReady:true};
  const ALL  = {leftReady:true,  rightReady:true,  tempReady:true,  charsReady:true};

  function snap(chars, left, right, temp, code, phase, ist, extra) {
    return {chars:[...chars], n:chars.length, left, right, temp, code, phase, initState:{...ist}, ...extra};
  }

  // ── Input phase ──────────────────────────────────────────────────────────────
  steps.push({...snap([...s],-1,-1,null,'m_scanner','input',NONE,{partial:''}),badge:'Scanner sc = new Scanner(System.in); → Init input.'});
  steps.push({...snap([...s],-1,-1,null,'m_read_s','input',NONE,{partial:s}),badge:`String s = sc.next(); → s = "${s}".`});
  steps.push({...snap([...s],-1,-1,null,'m_call','input',NONE,{partial:s}),badge:`new Solution().reverseVowels("${s}"); → Calling.`});

  // ── reverseVowels() ──────────────────────────────────────────────────────────
  const chars = [...s];
  steps.push({...snap(chars,-1,-1,null,'fn_entry','fn',NONE,{}),badge:`reverseVowels("${s}") → Function entry. Length = ${N}.`});
  steps.push({...snap(chars,-1,-1,null,'to_char','fn',CH,{}),badge:`char[] chars = s.toCharArray(); → chars = [${chars.map(c=>"'"+c+"'").join(', ')}].`});

  let left=0, right=N-1;
  steps.push({...snap(chars,left,-1,null,'init_left','fn',L,{}),badge:`int left = 0; → left = ${left}. chars[left] = '${chars[left]}' (${isVowel(chars[left])?'vowel':'non-vowel'}).`});
  steps.push({...snap(chars,left,right,null,'init_right','fn',LR,{}),badge:`int right = chars.length - 1 = ${right}; → chars[right] = '${chars[right]}' (${isVowel(chars[right])?'vowel':'non-vowel'}).`});

  while (left < right) {
    steps.push({...snap(chars,left,right,null,'outer_while','fn',LR,{outerTrue:true}),badge:`while(left<right): ${left}<${right} → TRUE.`});

    // scan left for vowel
    let scanLTrue = left < right && !isVowel(chars[left]);
    steps.push({...snap(chars,left,right,null,'scan_l_cond','fn',LR,{scanL:true}),badge:`while(left<right && !isVowel(chars[left])): chars[${left}]='${chars[left]}' ${isVowel(chars[left])?'is vowel → FALSE':'is not vowel → TRUE, advance left'}.`});
    while (left < right && !isVowel(chars[left])) {
      left++;
      steps.push({...snap(chars,left,right,null,'scan_l_inc','fn',LR,{scanL:true}),badge:`left++; → left = ${left}. chars[left]='${chars[left]}'.`});
      scanLTrue = left < right && !isVowel(chars[left]);
      steps.push({...snap(chars,left,right,null,'scan_l_cond','fn',LR,{scanL:true}),badge:`while(left<right && !isVowel(chars[left])): chars[${left}]='${chars[left]}' ${isVowel(chars[left])?'is vowel → FALSE, found vowel':'is not vowel → TRUE, keep scanning'}.`});
    }

    // scan right for vowel
    let scanRTrue = left < right && !isVowel(chars[right]);
    steps.push({...snap(chars,left,right,null,'scan_r_cond','fn',LR,{scanR:true}),badge:`while(left<right && !isVowel(chars[right])): chars[${right}]='${chars[right]}' ${isVowel(chars[right])?'is vowel → FALSE':'is not vowel → TRUE, advance right'}.`});
    while (left < right && !isVowel(chars[right])) {
      right--;
      steps.push({...snap(chars,left,right,null,'scan_r_dec','fn',LR,{scanR:true}),badge:`right--; → right = ${right}. chars[right]='${chars[right]}'.`});
      scanRTrue = left < right && !isVowel(chars[right]);
      steps.push({...snap(chars,left,right,null,'scan_r_cond','fn',LR,{scanR:true}),badge:`while(left<right && !isVowel(chars[right])): chars[${right}]='${chars[right]}' ${isVowel(chars[right])?'is vowel → FALSE, found vowel':'is not vowel → TRUE, keep scanning'}.`});
    }

    // if swap
    const canSwap = left < right;
    steps.push({...snap(chars,left,right,null,'if_swap','fn',LR,{canSwap}),badge:`if(left<right): ${left}<${right} → ${canSwap?'TRUE. Both vowels found — swap.':'FALSE. Pointers met — skip swap.'}`});

    if (canSwap) {
      const tmp = chars[left];
      steps.push({...snap(chars,left,right,tmp,'swap_temp','fn',ALL,{tempVal:tmp,swapL:left,swapR:right}),badge:`char temp = chars[left]; → temp = chars[${left}] = '${tmp}'.`});
      chars[left] = chars[right];
      steps.push({...snap(chars,left,right,tmp,'swap_assign1','fn',ALL,{tempVal:tmp,justSwapped:left,swapL:left,swapR:right}),badge:`chars[left] = chars[right]; → chars[${left}] = chars[${right}] = '${chars[left]}'.`});
      chars[right] = tmp;
      steps.push({...snap(chars,left,right,tmp,'swap_assign2','fn',ALL,{tempVal:tmp,justSwapped:right,swapL:left,swapR:right}),badge:`chars[right] = temp; → chars[${right}] = '${tmp}'.`});
      left++;
      steps.push({...snap(chars,left,right,null,'inc_left','fn',LR,{}),badge:`left++; → left = ${left}.`});
      right--;
      steps.push({...snap(chars,left,right,null,'dec_right','fn',LR,{}),badge:`right--; → right = ${right}.`});
    }
  }

  // outer_while exits
  steps.push({...snap(chars,left,right,null,'outer_while','done',LR,{outerTrue:false}),badge:`while(left<right): ${left}<${right} → FALSE. All vowels reversed.`});
  const result = chars.join('');
  steps.push({...snap(chars,left,right,null,'return_stmt','done',LR,{}),badge:`return new String(chars); → "${result}". Vowels reversed!`});

  return steps;
}

// ─── Reactive State ───────────────────────────────────────────────────────────
const inputS      = ref('hello');
const lang        = ref('java');
const speed       = ref(650);
const si          = ref(0);
const playing     = ref(false);
const vizHeight   = ref(320);
const tableHeight = ref(60);
const leftWidth   = ref(52);
const rightTab    = ref('code');

const stepsData = reactive({steps: buildSteps('hello')});
const steps     = computed(()=>stepsData.steps);
const s         = computed(()=>steps.value[Math.max(0,Math.min(si.value,steps.value.length-1))]||{});
const codeLines = computed(()=>CODES[lang.value]||[]);
let playTimer = null;

function onChipsWheel(e){if(e.currentTarget)e.currentTarget.scrollLeft+=e.deltaY;}
function applyInput(){
  const val=inputS.value.trim();
  if(!val){alert('Enter a non-empty string.');return;}
  playing.value=false;stepsData.steps=buildSteps(val);si.value=0;
}
function stepBy(d){si.value=Math.max(0,Math.min(steps.value.length-1,si.value+d));}
function togglePlay(){if(!playing.value&&si.value>=steps.value.length-1)si.value=0;playing.value=!playing.value;}
function tick(){
  clearTimeout(playTimer);if(!playing.value)return;
  if(si.value>=steps.value.length-1){playing.value=false;return;}
  playTimer=setTimeout(()=>{si.value=Math.min(steps.value.length-1,si.value+1);tick();},2100-speed.value);
}
watch(playing,v=>{if(v)tick();else clearTimeout(playTimer);});

const codeScrollRef=ref(null);
function scrollActiveCodeLine(){
  nextTick(()=>{
    const c=codeScrollRef.value;if(!c)return;
    const a=c.querySelector('.ll-hl');if(!a)return;
    const cr=c.getBoundingClientRect(),ar=a.getBoundingClientRect();
    if(ar.top<cr.top)c.scrollTop=Math.max(0,c.scrollTop-(cr.top-ar.top)-24);
    else if(ar.bottom>cr.bottom)c.scrollTop=c.scrollTop+(ar.bottom-cr.bottom)+24;
  });
}
watch(()=>s.value.code,scrollActiveCodeLine);
watch(lang,scrollActiveCodeLine);
watch(rightTab,v=>{if(v==='code')scrollActiveCodeLine();});
function onKeydown(e){
  const t=e.target.tagName;
  if(t==='INPUT'||t==='SELECT'||t==='TEXTAREA')return;
  if(e.key==='ArrowRight')stepBy(1);
  if(e.key==='ArrowLeft')stepBy(-1);
  if(e.key===' '){e.preventDefault();togglePlay();}
}

// ─── Computed display ─────────────────────────────────────────────────────────
const initState    = computed(()=>s.value.initState||{leftReady:false,rightReady:false,tempReady:false,charsReady:false});
const displayChars = computed(()=>s.value.phase==='input'?(s.value.partial?s.value.partial.split(''):['']):(s.value.chars||[]));
const displayLeft  = computed(()=>initState.value.leftReady ?s.value.left :'?');
const displayRight = computed(()=>initState.value.rightReady?s.value.right:'?');
const displayTemp  = computed(()=>initState.value.tempReady ?s.value.tempVal:'?');

function charClass(idx){
  const phase=s.value.phase,ist=initState.value;
  const ch=s.value.chars;
  if(phase==='input')return '';
  const left=s.value.left,right=s.value.right;
  const js=s.value.justSwapped,sl=s.value.swapL,sr=s.value.swapR;
  if(js===idx)return 'rv-char-just-swapped';
  if(phase==='done'){
    if(ch&&isVowel(ch[idx]))return 'rv-char-vowel-done';
    return 'rv-char-done';
  }
  if(ist.leftReady&&idx===left&&idx===right)return 'rv-char-both';
  if(ist.leftReady&&idx===left)return 'rv-char-left';
  if(ist.rightReady&&idx===right)return 'rv-char-right';
  if(ch&&isVowel(ch[idx])&&ist.charsReady){
    if(ist.leftReady&&idx<left)return 'rv-char-vowel-swapped';
    if(ist.rightReady&&idx>right)return 'rv-char-vowel-swapped';
    return 'rv-char-vowel';
  }
  return '';
}

// ─── Resizers ──────────────────────────────────────────────────────────────────
const mainRef=ref(null),leftColRef=ref(null),hResizerRef=ref(null),vizResizerRef=ref(null),tableResizerRef=ref(null);
function initHResizer(){
  const rsz=hResizerRef.value,main=mainRef.value;if(!rsz||!main)return;
  let dragging=false,startX=0,startW=0;
  const onDown=e=>{dragging=true;startX=e.clientX;startW=leftColRef.value.offsetWidth;rsz.classList.add('drag');document.body.style.userSelect='none';};
  const onMove=e=>{if(!dragging)return;const mW=main.offsetWidth;leftWidth.value=(Math.max(200,Math.min(mW-200,startW+e.clientX-startX))/mW)*100;};
  const onUp=()=>{if(!dragging)return;dragging=false;rsz.classList.remove('drag');document.body.style.userSelect='';};
  rsz.addEventListener('mousedown',onDown);document.addEventListener('mousemove',onMove);document.addEventListener('mouseup',onUp);
  return()=>{rsz.removeEventListener('mousedown',onDown);document.removeEventListener('mousemove',onMove);document.removeEventListener('mouseup',onUp);};
}
function initVResizer(elRef,valueRef,minH,maxH){
  const rsz=elRef.value;if(!rsz)return;
  let dragging=false,startY=0,startH=0;
  const onDown=e=>{dragging=true;startY=e.clientY;startH=valueRef.value;rsz.classList.add('drag');document.body.style.userSelect='none';e.preventDefault();};
  const onMove=e=>{if(!dragging)return;valueRef.value=Math.max(minH,Math.min(maxH,startH+(e.clientY-startY)));};
  const onUp=()=>{if(!dragging)return;dragging=false;rsz.classList.remove('drag');document.body.style.userSelect='';};
  rsz.addEventListener('mousedown',onDown);document.addEventListener('mousemove',onMove);document.addEventListener('mouseup',onUp);
  return()=>{rsz.removeEventListener('mousedown',onDown);document.removeEventListener('mousemove',onMove);document.removeEventListener('mouseup',onUp);};
}
let cleanupFns=[];
onMounted(()=>{
  document.addEventListener('keydown',onKeydown);
  cleanupFns.push(initHResizer());
  cleanupFns.push(initVResizer(vizResizerRef,vizHeight,200,700));
  cleanupFns.push(initVResizer(tableResizerRef,tableHeight,50,200));
});
onBeforeUnmount(()=>{
  document.removeEventListener('keydown',onKeydown);
  clearTimeout(playTimer);
  cleanupFns.forEach(fn=>fn&&fn());
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
              <input type="text" v-model="inputS" class="ll-text-input rv-s-input" @keyup.enter="applyInput" placeholder='e.g. hello' maxlength="16" />
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
            <!-- Left col -->
            <div class="ll-left-col" ref="leftColRef" :style="{ width: leftWidth + '%' }">
              <div class="ll-viz-wrap" :style="{ height: vizHeight + 'px' }">
                <div class="ll-perm-area">
                  <!-- Chips -->
                  <div class="ll-ptrs ll-ptrs-compact" @wheel.passive="onChipsWheel">
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">n</span><b class="ll-c-blue">{{ s.n || displayChars.length }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">left</span><b class="ll-c-orange">{{ displayLeft }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">right</span><b class="ll-c-green">{{ displayRight }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">temp</span><b class="ll-c-purple">{{ displayTemp }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">result</span><b class="ll-c-blue" style="font-family:monospace">{{ s.phase==='done'&&s.chars?s.chars.join(''):'—' }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">phase</span><b class="ll-c-orange">{{ s.phase || 'input' }}</b></span>
                  </div>

                  <!-- BOARD CONTAINER -->
                  <div class="ll-board-container">

                    <!-- Scan state banner -->
                    <div v-if="s.code==='scan_l_cond'||s.code==='scan_l_inc'" class="rv-scan-banner rv-scan-left">
                      Scanning left &rarr; for vowel &nbsp;
                      <span v-if="s.chars&&initState.leftReady">'{{ s.chars[s.left] }}' {{ isVowel(s.chars[s.left]) ? 'is a vowel — stop' : 'is not a vowel — advance' }}</span>
                    </div>
                    <div v-else-if="s.code==='scan_r_cond'||s.code==='scan_r_dec'" class="rv-scan-banner rv-scan-right">
                      Scanning right &larr; for vowel &nbsp;
                      <span v-if="s.chars&&initState.rightReady">'{{ s.chars[s.right] }}' {{ isVowel(s.chars[s.right]) ? 'is a vowel — stop' : 'is not a vowel — advance' }}</span>
                    </div>
                    <div v-else-if="s.code==='if_swap'&&s.canSwap" class="rv-scan-banner rv-scan-swap">
                      Both vowels found: '{{ s.chars&&s.chars[s.left] }}' &harr; '{{ s.chars&&s.chars[s.right] }}' &mdash; swapping
                    </div>

                    <!-- Tier 1: Character array -->
                    <div class="rv-tier-title">
                      char[] chars
                      <span class="rv-badge">n = {{ s.n || displayChars.length }}</span>
                      <span v-if="s.phase==='done'" class="rv-badge rv-badge-done">reversed</span>
                    </div>

                    <div class="rv-array-frame">
                      <!-- Pointer label row -->
                      <div class="rv-ptr-row">
                        <div v-for="(ch, idx) in displayChars" :key="'ptr-'+idx" class="rv-ptr-cell">
                          <span v-if="initState.leftReady  && s.phase!=='input' && s.phase!=='done' && idx===s.left"  class="rv-ptr-label rv-ptr-left">L</span>
                          <span v-if="initState.rightReady && s.phase!=='input' && s.phase!=='done' && idx===s.right && s.right < displayChars.length" class="rv-ptr-label rv-ptr-right">R</span>
                        </div>
                      </div>
                      <!-- Character boxes -->
                      <div class="rv-char-row">
                        <div
                          v-for="(ch, idx) in displayChars"
                          :key="'ch-'+idx"
                          class="rv-char-box"
                          :class="charClass(idx)"
                        >
                          <span class="rv-char-val">{{ ch }}</span>
                          <span class="rv-char-idx">[{{ idx }}]</span>
                          <span v-if="s.chars&&isVowel(s.chars[idx])" class="rv-vowel-dot"></span>
                        </div>
                      </div>
                    </div>

                    <!-- Swap panel: visible during swap steps -->
                    <div v-if="initState.tempReady" class="rv-swap-panel">
                      <div class="rv-swap-item rv-swap-left">
                        <span class="rv-swap-lbl">chars[left]</span>
                        <span class="rv-swap-idx">[{{ s.swapL }}]</span>
                        <span class="rv-swap-val">{{ s.chars&&s.chars[s.swapL] }}</span>
                      </div>
                      <span class="rv-swap-arrow">&#8644;</span>
                      <div class="rv-swap-item rv-swap-right">
                        <span class="rv-swap-lbl">chars[right]</span>
                        <span class="rv-swap-idx">[{{ s.swapR }}]</span>
                        <span class="rv-swap-val">{{ s.chars&&s.chars[s.swapR] }}</span>
                      </div>
                      <div class="rv-temp-item">
                        <span class="rv-swap-lbl">temp</span>
                        <span class="rv-swap-val">'{{ s.tempVal }}'</span>
                      </div>
                    </div>

                    <!-- Tier 2: Result string display -->
                    <div class="rv-tier-title" style="margin-top:2px">
                      Current String
                    </div>
                    <div class="rv-result-row">
                      <span
                        v-for="(ch, idx) in (s.chars || displayChars)"
                        :key="'r-'+idx"
                        class="rv-result-char"
                        :class="{
                          'rv-rc-vowel': s.chars&&isVowel(s.chars[idx]),
                          'rv-rc-done':  s.phase==='done'
                        }"
                      >{{ ch }}</span>
                    </div>
                    <div class="rv-vowel-hint">
                      Vowels: aeiou AEIOU &mdash; {{ s.chars ? s.chars.filter(c=>isVowel(c)).length : 0 }} vowel(s) in string
                    </div>

                  </div>
                  <!-- END ll-board-container -->
                </div>
              </div>

              <div class="ll-vresizer" ref="vizResizerRef"></div>

              <!-- Legend -->
              <div class="ll-legend">
                <span class="ll-leg"><span class="ll-legdot" style="background:#fff7ed;border:1.5px solid #f97316;"></span>left</span>
                <span class="ll-leg"><span class="ll-legdot" style="background:#f0fdf4;border:1.5px solid #22c55e;"></span>right</span>
                <span class="ll-leg"><span class="ll-legdot rv-legdot-vowel"></span>vowel found</span>
                <span class="ll-leg"><span class="ll-legdot rv-legdot-swapped"></span>swapped</span>
                <span class="ll-leg"><span class="ll-legdot rv-legdot-done"></span>reversed</span>
              </div>

              <!-- Call Stack -->
              <div class="ll-table-area" :style="{ height: tableHeight + 'px' }">
                <div class="ll-table-title">Call Stack &mdash; Current Execution Frame</div>
                <div class="ll-stack-line">
                  <template v-if="s.phase==='input'">
                    <div class="ll-frame ll-frame-cur">main() &nbsp; s="{{ s.partial || '' }}"<span class="ll-now"> &#9668; active</span></div>
                  </template>
                  <template v-else>
                    <div class="ll-frame" style="color:var(--text2);">main()</div>
                    <div class="ll-frame ll-frame-cur" style="margin-left:14px">
                      reverseVowels(s) &nbsp;
                      <span class="ll-fname">left</span>=<span class="ll-c-orange" style="font-weight:700">{{ displayLeft }}</span>,
                      <span class="ll-fname">right</span>=<span class="ll-c-green" style="font-weight:700">{{ displayRight }}</span>
                      <template v-if="initState.tempReady">, <span class="ll-fname">temp</span>=<span class="ll-c-purple" style="font-weight:700">'{{ displayTemp }}'</span></template>
                      <span class="ll-now"> &#9668; active</span>
                    </div>
                  </template>
                </div>
              </div>

              <div class="ll-vresizer" ref="tableResizerRef"></div>

              <!-- Badge -->
              <div class="ll-badge-wrap">
                <div class="ll-badge" :class="{'ll-badge-error':s.badge&&(s.badge.includes('FALSE')||s.badge.includes('not vowel')),'ll-badge-success':s.badge&&(s.badge.includes('vowel')||s.badge.includes('TRUE')||s.badge.includes('reversed'))}">
                  {{ s.badge || 'Ready to visualize Reverse Vowels.' }}
                </div>
              </div>
            </div>

            <div class="ll-resizer" ref="hResizerRef"></div>

            <!-- Right col -->
            <div class="ll-right-col">
              <div class="ll-code-panel">
                <div class="ll-code-header">
                  <div class="ll-tabbar">
                    <button class="ll-tab-btn" :class="{active:rightTab==='code'}"       @click="rightTab='code'">Code</button>
                    <button class="ll-tab-btn" :class="{active:rightTab==='pseudo'}"     @click="rightTab='pseudo'">Pseudocode</button>
                    <button class="ll-tab-btn" :class="{active:rightTab==='complexity'}" @click="rightTab='complexity'">Complexity</button>
                  </div>
                  <select v-if="rightTab==='code'" v-model="lang" class="ll-lang-select">
                    <option value="java">Java</option>
                    <option value="c">C</option>
                    <option value="cpp">C++</option>
                    <option value="python">Python</option>
                    <option value="javascript">JavaScript</option>
                  </select>
                </div>
                <div v-if="rightTab==='code'" class="ll-code-scroll" ref="codeScrollRef">
                  <pre class="ll-pre"><span v-for="(line,idx) in codeLines" :key="idx" class="ll-codeline" :class="{'ll-hl':line[0]&&line[0]===s.code}">{{ line[1]===''?' ':line[1] }}</span></pre>
                </div>
                <div v-else-if="rightTab==='pseudo'" class="ll-code-scroll">
                  <pre class="ll-pre"><span v-for="(line,idx) in PSEUDOCODE" :key="idx" class="ll-codeline">{{ line }}</span></pre>
                </div>
                <div v-else class="ll-info-scroll">
                  <h3 class="ll-cx-heading">Reverse Vowels of a String (LeetCode 345) &mdash; Complexity Analysis</h3>
                  <p class="ll-cx-intro">
                    Given string <code>s</code>, reverse only its vowels (a, e, i, o, u — case-insensitive) in-place using
                    two pointers <code>left</code> and <code>right</code>. Each pointer scans toward the center,
                    skipping non-vowels, then swaps when both land on vowels.
                  </p>
                  <h4 class="ll-cx-sub">Step-by-step Breakdown</h4>
                  <table class="ll-complexity-table">
                    <thead><tr><th>Operation</th><th>Time</th><th>Space</th><th>Reason</th></tr></thead>
                    <tbody>
                      <tr><td>toCharArray</td><td class="ll-cx-good">O(n)</td><td class="ll-cx-good">O(n)</td><td>String to char array copy</td></tr>
                      <tr><td>Outer while loop</td><td class="ll-cx-good">O(n)</td><td class="ll-cx-good">O(1)</td><td>Each character visited at most once by either pointer</td></tr>
                      <tr><td>Inner scan loops</td><td class="ll-cx-good">O(n) total</td><td class="ll-cx-good">O(1)</td><td>Total moves of left+right never exceed n</td></tr>
                      <tr><td>Swap</td><td class="ll-cx-good">O(1)</td><td class="ll-cx-good">O(1)</td><td>Three-step temp swap per vowel pair</td></tr>
                      <tr><td><strong>Total</strong></td><td class="ll-cx-good"><strong>O(n)</strong></td><td class="ll-cx-good"><strong>O(n)</strong></td><td>O(n) for char array copy</td></tr>
                    </tbody>
                  </table>
                  <h4 class="ll-cx-sub">Overall Complexity</h4>
                  <div class="ll-cx-summary-grid">
                    <div class="ll-cx-card ll-cx-card-good"><div class="ll-cx-card-label">Time</div><div class="ll-cx-card-val">O(n)</div><div class="ll-cx-card-note">Single convergent pass</div></div>
                    <div class="ll-cx-card ll-cx-card-mid"><div class="ll-cx-card-label">Space</div><div class="ll-cx-card-val">O(n)</div><div class="ll-cx-card-note">char[] copy of string</div></div>
                    <div class="ll-cx-card ll-cx-card-good"><div class="ll-cx-card-label">Passes</div><div class="ll-cx-card-val">1</div><div class="ll-cx-card-note">One convergent scan</div></div>
                  </div>
                  <div class="ll-note">
                    <strong>Key insight:</strong> The inner <code>while</code> loops skip non-vowels efficiently. The outer <code>while</code> guarantees the two pointers always converge. The total work across all inner loop iterations is O(n) because each index is visited at most once. Space is O(n) only because Java strings are immutable — the char array is O(n).
                  </div>
                </div>
              </div>
            </div>
          </div>

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
.ll-root*{box-sizing:border-box;}
.ll-root*,.ll-root,.row-main{scrollbar-width:none!important;-ms-overflow-style:none!important;}
.ll-root *::-webkit-scrollbar,.ll-root::-webkit-scrollbar,.row-main::-webkit-scrollbar{display:none!important;width:0!important;height:0!important;}
.ll-root{
  --coral:#F04D4D;--coral-dark:#d93e3e;--coral-light:#fff0f0;
  --bg:#f5f6fa;--surface:#ffffff;--surface2:#f1f4f9;
  --border:#e2e8f0;--border2:#cbd5e1;
  --text:#1e293b;--text2:#475569;--muted:#94a3b8;
  --blue:#3b82f6;--blue-light:#eff6ff;
  --green:#22c55e;--green-light:#f0fdf4;
  --orange:#f97316;--orange-light:#fff7ed;
  --purple:#9333ea;--purple-light:#f3e8ff;
  --red:#ef4444;--red-dark:#991b1b;--red-light:#fef2f2;
  --shadow-sm:0 1px 3px rgba(0,0,0,.08),0 1px 2px rgba(0,0,0,.04);
  --radius:8px;--radius-sm:6px;
  background:var(--bg);color:var(--text);
  font-family:'Segoe UI',system-ui,sans-serif;font-size:12.5px;
  display:flex;flex-direction:column;height:74vh;overflow:hidden;width:100%;
}
@keyframes rv-swap-flash{0%{background:#fef9c3;transform:scale(1.14);}60%{background:#c7d2fe;}100%{transform:scale(1);}}
@keyframes rv-done-pop{0%{transform:scale(.9);}70%{transform:scale(1.06);}100%{transform:scale(1);}}

.slide-wrapper{margin-top:-10px;margin-left:-30px;width:107%;max-height:100%;font-size:.8rem;font-weight:400;}
.slide-body{display:flex;flex-direction:column;border-radius:4px;height:100%;}
.navbar{display:flex;flex-direction:row;justify-content:space-between;align-items:center;gap:.75rem;padding:0 10px;background-color:#fff;position:fixed;width:94.7%;z-index:50;}
.navbar>img{height:30px;}
.navbar-title{margin:0;font-size:1.35rem;font-weight:700;background-color:#ef5050;color:#fff;width:80%;padding:2px 10px;margin-left:-10px;border-radius:5px;}
.row-main{width:100%;height:90%;margin-top:36px;overflow-x:auto;overflow-y:hidden;}
.ll-toolbar{margin-top:4px;display:flex;align-items:center;gap:6px;padding:6.5px 12px;background:var(--surface);border-bottom:1px solid var(--border);flex-shrink:0;flex-wrap:wrap;box-shadow:var(--shadow-sm);}
.ll-input-group{display:flex;align-items:center;gap:4px;}
.ll-input-group label{font-size:11px;color:var(--muted);font-weight:700;}
.ll-text-input{background:var(--surface);border:1px solid var(--border2);color:var(--text);border-radius:var(--radius-sm);padding:3px 6px;font-size:11.5px;font-family:monospace;}
.ll-text-input:focus{outline:none;border-color:var(--coral);box-shadow:0 0 0 3px rgba(240,77,77,.1);}
.rv-s-input{width:180px;}
.ll-viz-btn{background:var(--coral);color:#fff;border:none;padding:5px 12px;border-radius:var(--radius-sm);cursor:pointer;font-size:11.5px;font-weight:600;box-shadow:var(--shadow-sm);transition:filter .15s;}
.ll-viz-btn:hover{filter:brightness(1.08);}
.ll-nav-controls{display:flex;margin-left:auto;align-items:center;gap:4px;flex-shrink:0;flex-wrap:wrap;}
.ll-nav-btn{background:var(--surface2);border:1px solid var(--border2);color:var(--text2);padding:4px 9px;border-radius:var(--radius-sm);cursor:pointer;font-size:11px;font-weight:500;transition:all .15s;white-space:nowrap;}
.ll-nav-btn:hover{background:var(--surface);border-color:var(--coral);color:var(--coral);}
.ll-play-btn{background:var(--blue-light);border:1px solid var(--blue);color:var(--blue);min-width:68px;font-weight:600;padding:4px 9px;border-radius:var(--radius-sm);cursor:pointer;font-size:11px;transition:all .15s;}
.ll-play-btn:hover{background:var(--blue);color:#fff;}
.ll-main{display:flex;flex:1;overflow:hidden;position:relative;}
.ll-left-col{display:flex;flex-direction:column;overflow:hidden;min-width:220px;max-width:75%;}
.ll-resizer{width:5px;cursor:col-resize;background:var(--border);flex-shrink:0;transition:background .15s;position:relative;z-index:20;}
.ll-resizer:hover,.ll-resizer.drag{background:var(--coral);}
.ll-right-col{display:flex;flex-direction:column;flex:1;overflow:hidden;min-width:0;height:100%;}
.ll-viz-wrap{flex-shrink:0;background:var(--surface);border-bottom:1px solid var(--border);position:relative;overflow-x:auto;overflow-y:auto;}
.ll-perm-area{display:flex;flex-direction:column;align-items:stretch;min-height:100%;width:100%;min-width:0;box-sizing:border-box;}
.ll-ptrs{display:flex;gap:8px;flex-wrap:wrap;padding:4px 14px;min-height:28px;width:100%;box-sizing:border-box;min-width:0;align-items:center;}
.ll-ptrs-compact{flex-wrap:nowrap;gap:6px;padding:3px 14px 5px;min-height:0;overflow-x:auto;overflow-y:hidden;}
.ll-ptr-chip-inline{display:inline-flex;align-items:center;gap:4px;background:var(--surface2);border:1px solid var(--border);border-radius:6px;padding:2px 8px;font-size:11px;font-family:monospace;white-space:nowrap;flex-shrink:0;line-height:1.4;}
.ll-chip-label{color:#8899aa;font-weight:500;margin-right:2px;}
.ll-c-blue{color:var(--blue);}.ll-c-green{color:var(--green);}.ll-c-orange{color:var(--orange);}.ll-c-purple{color:var(--purple);}

/* ─── Board Container ─────────────────────────────────────────────────────── */
.ll-board-container{display:flex;flex-direction:column;align-items:flex-start;padding:6px 14px 10px;gap:8px;min-width:0;width:100%;}

/* Scan banner */
.rv-scan-banner{display:flex;align-items:center;padding:4px 12px;border-radius:var(--radius-sm);border-left:3px solid;font-size:11px;font-weight:600;width:100%;box-sizing:border-box;}
.rv-scan-left{background:#fff7ed;border-color:#f97316;color:#c2410c;}
.rv-scan-right{background:#f0fdf4;border-color:#22c55e;color:#15803d;}
.rv-scan-swap{background:#ede9fe;border-color:#9333ea;color:#6d28d9;}

.rv-tier-title{font-size:10px;font-weight:700;text-transform:uppercase;letter-spacing:.04em;color:var(--muted);font-family:'Consolas',monospace;border-left:3px solid var(--coral);padding-left:6px;display:flex;align-items:center;gap:6px;}
.rv-badge{font-size:9px;color:var(--muted);background:var(--surface2);border:1px solid var(--border);border-radius:3px;padding:0 4px;font-weight:500;text-transform:none;letter-spacing:0;}
.rv-badge-done{background:#d1fae5;border-color:#6ee7b7;color:#065f46;}

.rv-array-frame{background:#f8fafc;border:1px solid var(--border);border-radius:var(--radius);padding:8px 10px 8px;box-shadow:var(--shadow-sm);display:flex;flex-direction:column;gap:4px;width:100%;box-sizing:border-box;}
.rv-ptr-row{display:flex;gap:4px;min-height:20px;align-items:flex-end;}
.rv-ptr-cell{width:40px;display:flex;justify-content:center;align-items:flex-end;gap:2px;flex-shrink:0;}
.rv-ptr-label{font-size:8.5px;font-weight:800;font-family:monospace;padding:1px 4px;border-radius:3px;line-height:1.2;}
.rv-ptr-left{background:#fff7ed;color:#c2410c;border:1px solid #fdba74;}
.rv-ptr-right{background:#f0fdf4;color:#15803d;border:1px solid #86efac;}
.rv-char-row{display:flex;gap:4px;flex-wrap:nowrap;}
.rv-char-box{
  width:40px;min-height:52px;border-radius:var(--radius-sm);
  background:#e2e8f0;border:1.5px solid #94a3b8;
  display:flex;flex-direction:column;align-items:center;justify-content:center;gap:2px;
  flex-shrink:0;transition:all .2s ease;position:relative;
}
.rv-char-val{font-family:'Cascadia Code','Fira Code',monospace;font-size:17px;font-weight:900;color:var(--text);line-height:1;}
.rv-char-idx{font-family:monospace;font-size:7.5px;color:var(--muted);font-weight:600;}
.rv-vowel-dot{position:absolute;bottom:3px;left:50%;transform:translateX(-50%);width:5px;height:5px;border-radius:50%;background:#9333ea;opacity:.7;}

/* Char box states */
.rv-char-left{background:#fff7ed!important;border:2px solid #f97316!important;box-shadow:0 0 8px rgba(249,115,22,.35);transform:scale(1.07);}
.rv-char-right{background:#f0fdf4!important;border:2px solid var(--green)!important;box-shadow:0 0 8px rgba(34,197,94,.35);transform:scale(1.07);}
.rv-char-both{background:#ede9fe!important;border:2px solid #9333ea!important;box-shadow:0 0 10px rgba(147,51,234,.4);transform:scale(1.07);}
.rv-char-vowel{background:#faf5ff!important;border:1.5px solid #d8b4fe!important;}
.rv-char-vowel-swapped{background:#d1fae5!important;border:1.5px solid #6ee7b7!important;}
.rv-char-just-swapped{background:#fef9c3!important;border:2px solid #eab308!important;animation:rv-swap-flash .4s ease;z-index:5;}
.rv-char-done{background:#f1f5f9!important;border:1.5px solid #e2e8f0!important;}
.rv-char-vowel-done{background:#d1fae5!important;border:1.5px solid #6ee7b7!important;animation:rv-done-pop .3s ease;}

/* Swap panel */
.rv-swap-panel{display:flex;align-items:center;gap:10px;padding:6px 12px;background:var(--surface2);border:1.5px solid var(--border);border-radius:var(--radius-sm);flex-wrap:wrap;width:100%;box-sizing:border-box;}
.rv-swap-item{display:flex;flex-direction:column;align-items:center;gap:2px;padding:4px 10px;border-radius:var(--radius-sm);min-width:64px;}
.rv-swap-left{background:#fff7ed;border:1.5px solid #fdba74;}
.rv-swap-right{background:#f0fdf4;border:1.5px solid #86efac;}
.rv-swap-lbl{font-size:8.5px;color:var(--muted);font-weight:700;font-family:monospace;}
.rv-swap-idx{font-size:8px;color:var(--muted);}
.rv-swap-val{font-size:18px;font-weight:900;font-family:monospace;color:var(--text);}
.rv-swap-arrow{font-size:18px;color:var(--muted);}
.rv-temp-item{display:flex;flex-direction:column;align-items:center;gap:2px;padding:4px 10px;border-radius:var(--radius-sm);background:var(--purple-light);border:1.5px solid #d8b4fe;min-width:50px;}

/* Result string */
.rv-result-row{display:flex;gap:3px;flex-wrap:nowrap;padding:4px 0;}
.rv-result-char{
  width:22px;height:28px;border-radius:3px;display:flex;align-items:center;justify-content:center;
  font-size:14px;font-weight:900;font-family:'Cascadia Code','Fira Code',monospace;
  background:var(--surface2);border:1px solid var(--border);color:var(--text);
  flex-shrink:0;transition:all .15s;
}
.rv-rc-vowel{background:#ede9fe!important;border-color:#c4b5fd!important;color:#6d28d9!important;}
.rv-rc-done{animation:rv-done-pop .3s ease;}
.rv-vowel-hint{font-size:9px;color:var(--muted);font-family:monospace;margin-top:-2px;}

/* Legend */
.rv-legdot-vowel{background:#faf5ff;border:1.5px solid #d8b4fe;display:inline-block;width:11px;height:11px;border-radius:3px;}
.rv-legdot-swapped{background:#fef9c3;border:1.5px solid #eab308;display:inline-block;width:11px;height:11px;border-radius:3px;}
.rv-legdot-done{background:#d1fae5;border:1.5px solid #6ee7b7;display:inline-block;width:11px;height:11px;border-radius:3px;}

/* Shared */
.ll-vresizer{height:5px;cursor:row-resize;background:var(--border);flex-shrink:0;transition:background .15s;position:relative;z-index:20;}
.ll-vresizer:hover,.ll-vresizer.drag{background:var(--coral);}
.ll-legend{display:flex;flex-wrap:wrap;gap:6px 14px;padding:6px 12px;border-bottom:1px solid var(--border);flex-shrink:0;background:var(--surface2);}
.ll-leg{display:flex;align-items:center;gap:5px;font-size:11px;color:var(--text2);font-weight:500;}
.ll-legdot{width:11px;height:11px;border-radius:3px;flex-shrink:0;display:inline-block;}
.ll-table-area{flex-shrink:0;padding:8px 14px;border-bottom:1px solid var(--border);overflow-x:hidden;overflow-y:auto;background:var(--surface);min-width:0;box-sizing:border-box;}
.ll-table-title{font-size:10px;color:var(--muted);margin-bottom:4px;font-style:italic;}
.ll-stack-line{font-family:'Consolas',monospace;font-size:12px;line-height:1.8;}
.ll-frame{font-family:'Consolas',monospace;font-size:11.5px;color:var(--text2);padding:1px 0;white-space:nowrap;overflow:hidden;text-overflow:ellipsis;}
.ll-frame-cur{color:var(--orange);background:var(--orange-light);border-radius:4px;padding:1px 5px;}
.ll-fname{color:var(--text2);}.ll-now{color:var(--orange);font-size:10px;margin-left:6px;}
.ll-badge-wrap{padding:6px 10px;border-bottom:1px solid var(--border);flex-shrink:0;min-height:36px;display:flex;align-items:center;background:var(--surface);}
.ll-badge{display:inline-block;padding:4px 12px;border-radius:var(--radius-sm);border-left:3px solid var(--coral);background:var(--coral-light);font-size:11px;color:var(--coral-dark);line-height:1.4;word-break:break-word;font-weight:500;}
.ll-badge-error{border-left-color:var(--red)!important;background:var(--red-light)!important;color:var(--red-dark)!important;}
.ll-badge-success{border-left-color:var(--green)!important;background:var(--green-light)!important;color:#15803d!important;}
.ll-code-panel{display:flex;flex-direction:column;height:100%;overflow:hidden;}
.ll-code-header{display:flex;align-items:center;gap:6px;padding:5px 12px;background:var(--surface);border-bottom:1px solid var(--border);box-shadow:var(--shadow-sm);flex-shrink:0;flex-wrap:wrap;}
.ll-tabbar{display:flex;gap:3px;flex-wrap:wrap;}
.ll-tab-btn{padding:4px 9px;font-size:10.5px;font-weight:600;border:1px solid var(--border2);background:var(--surface2);color:var(--text2);border-radius:var(--radius-sm);cursor:pointer;transition:all .15s ease;white-space:nowrap;}
.ll-tab-btn:hover{border-color:var(--coral);color:var(--coral);}
.ll-tab-btn.active{background:var(--coral);border-color:var(--coral);color:#fff;}
.ll-lang-select{margin-left:auto;padding:4px 24px 4px 8px;font-size:11px;font-weight:500;border:1px solid var(--border2);border-radius:var(--radius-sm);background:var(--surface2);color:var(--text);cursor:pointer;appearance:none;background-image:url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='10' height='6' viewBox='0 0 10 6'%3E%3Cpath d='M0 0l5 6 5-6z' fill='%2394a3b8'/%3E%3C/svg%3E");background-repeat:no-repeat;background-position:right 8px center;min-width:95px;transition:border-color .15s;}
.ll-lang-select:focus{outline:none;border-color:var(--coral);box-shadow:0 0 0 3px rgba(240,77,77,.1);}
.ll-code-scroll{flex:1;overflow:auto;background:#f8fafc;padding:10px 14px;min-width:0;}
.ll-pre{margin:0;font-family:'Cascadia Code','Fira Code','Consolas',monospace;font-size:11px;line-height:1.5;color:var(--text);white-space:pre;padding-bottom:150px;}
.ll-codeline{display:block;padding:0 14px;margin:0 -14px;}
.ll-hl{background:#dcfce7;color:#15803d;font-weight:600;border-left:3px solid var(--green);border-radius:3px;}
.ll-info-scroll{flex:1;overflow:auto;padding:12px 16px;background:var(--surface);color:var(--text2);font-size:12px;line-height:1.55;}
.ll-info-scroll h3,.ll-info-scroll h4{color:var(--text);margin:0 0 6px;}
.ll-info-scroll p{margin:0 0 6px;}
.ll-info-scroll code{background:var(--surface2);padding:1px 4px;border-radius:3px;font-family:'Cascadia Code',monospace;font-size:11px;color:var(--coral-dark);}
.ll-cx-heading{font-size:13px;font-weight:700;}
.ll-cx-intro{font-size:10.5px;margin:0 0 10px;line-height:1.55;}
.ll-cx-sub{font-size:11px;font-weight:700;color:var(--text2);margin:10px 0 4px;border-bottom:1px solid var(--border);padding-bottom:3px;}
.ll-complexity-table{width:100%;border-collapse:collapse;font-size:10.5px;margin:8px 0;}
.ll-complexity-table th,.ll-complexity-table td{border:1px solid var(--border);padding:4px 8px;text-align:left;}
.ll-complexity-table th{background:var(--surface2);font-weight:700;color:var(--text2);}
.ll-cx-good{color:#15803d;font-weight:700;}.ll-cx-mid{color:#b45309;font-weight:700;}
.ll-cx-summary-grid{display:flex;gap:8px;flex-wrap:wrap;margin:6px 0 10px;}
.ll-cx-card{flex:1;min-width:90px;border-radius:var(--radius-sm);padding:8px 10px;text-align:center;border:1.5px solid var(--border);}
.ll-cx-card-good{background:#f0fdf4;border-color:#86efac;color:#15803d;}
.ll-cx-card-mid{background:#fff7ed;border-color:#fed7aa;color:#c2410c;}
.ll-cx-card-label{font-size:9px;font-weight:700;text-transform:uppercase;letter-spacing:.05em;opacity:.7;margin-bottom:4px;}
.ll-cx-card-val{font-size:13px;font-weight:800;font-family:monospace;margin-bottom:3px;}
.ll-cx-card-note{font-size:8.5px;opacity:.75;line-height:1.3;}
.ll-note{background:#fefce8;border:1px solid #fef08a;border-left:3px solid #eab308;padding:6px 10px;font-size:10.5px;color:#854d0e;border-radius:0 4px 4px 0;margin-top:10px;margin-bottom:120px;}
.ll-footer{display:flex;align-items:center;justify-content:space-between;padding:4px 12px;background:var(--surface);border-top:1px solid var(--border);font-size:11px;color:var(--muted);font-weight:600;flex-shrink:0;}
.ll-speed-wrap{display:flex;align-items:center;gap:6px;}
.ll-speed-wrap input[type="range"]{width:80px;accent-color:var(--coral);}
</style>
