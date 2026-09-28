<script setup>
import { ref, reactive, computed, onMounted, onBeforeUnmount, watch, nextTick } from 'vue';

defineProps({
  topic:    { type: String, default: 'Two Pointer Algorithms' },
  subTopic: { type: String, default: 'Move Zeroes (LeetCode 283)' }
});

const CODES = {
  java: [
    ['',            'class Solution {'],
    ['fn_entry',    '    public void moveZeroes(int[] nums) {'],
    ['init_slow',   '        int slow = 0;'],
    ['for_init',    '        for (int fast = 0; fast < nums.length; fast++) {'],
    ['if_nonzero',  '            if (nums[fast] != 0) {'],
    ['swap_temp',   '                int temp = nums[slow];'],
    ['swap_assign1','                nums[slow] = nums[fast];'],
    ['swap_assign2','                nums[fast] = temp;'],
    ['inc_slow',    '                slow++;'],
    ['',            '            }'],
    ['',            '        }'],
    ['',            '    }'],
    ['',            ''],
    ['',            '    public static void main(String[] args) {'],
    ['m_scanner',   '        java.util.Scanner sc = new java.util.Scanner(System.in);'],
    ['m_read_n',    '        int n = sc.nextInt();'],
    ['m_alloc',     '        int[] nums = new int[n];'],
    ['m_loop',      '        for (int i = 0; i < n; i++) {'],
    ['m_read_elem', '            nums[i] = sc.nextInt();'],
    ['',            '        }'],
    ['m_call',      '        new Solution().moveZeroes(nums);'],
    ['m_print',     '        System.out.println(java.util.Arrays.toString(nums));'],
    ['',            '    }'],
    ['',            '}']
  ],
  cpp: [
    ['',            '#include <vector>'],
    ['',            '#include <iostream>'],
    ['',            'using namespace std;'],
    ['',            ''],
    ['',            'class Solution {'],
    ['',            'public:'],
    ['fn_entry',    '    void moveZeroes(vector<int>& nums) {'],
    ['init_slow',   '        int slow = 0;'],
    ['for_init',    '        for (int fast = 0; fast < (int)nums.size(); fast++) {'],
    ['if_nonzero',  '            if (nums[fast] != 0) {'],
    ['swap_temp',   '                int temp = nums[slow];'],
    ['swap_assign1','                nums[slow] = nums[fast];'],
    ['swap_assign2','                nums[fast] = temp;'],
    ['inc_slow',    '                slow++;'],
    ['',            '            }'],
    ['',            '        }'],
    ['',            '    }'],
    ['',            '};'],
    ['',            ''],
    ['',            'int main() {'],
    ['m_read_n',    '    int n;'],
    ['m_scanner',   '    cin >> n;'],
    ['m_alloc',     '    vector<int> nums(n);'],
    ['m_loop',      '    for (int i = 0; i < n; i++) {'],
    ['m_read_elem', '        cin >> nums[i];'],
    ['',            '    }'],
    ['m_call',      '    Solution().moveZeroes(nums);'],
    ['m_print',     '    for (int x : nums) cout << x << " ";'],
    ['',            '    return 0;'],
    ['',            '}']
  ],
  python: [
    ['',            'class Solution:'],
    ['fn_entry',    '    def moveZeroes(self, nums):'],
    ['init_slow',   '        slow = 0'],
    ['for_init',    '        for fast in range(len(nums)):'],
    ['if_nonzero',  '            if nums[fast] != 0:'],
    ['swap_temp',   '                temp = nums[slow]'],
    ['swap_assign1','                nums[slow] = nums[fast]'],
    ['swap_assign2','                nums[fast] = temp'],
    ['inc_slow',    '                slow += 1'],
    ['',            ''],
    ['',            'def main():'],
    ['m_read_elem', '    nums = list(map(int, input().split()))'],
    ['m_call',      '    Solution().moveZeroes(nums)'],
    ['m_print',     '    print(nums)'],
    ['',            ''],
    ['',            'main()']
  ],
  javascript: [
    ['',            '/**'],
    ['',            ' * @param {number[]} nums'],
    ['',            ' * @return {void}'],
    ['',            ' */'],
    ['fn_entry',    'var moveZeroes = function(nums) {'],
    ['init_slow',   '    let slow = 0;'],
    ['for_init',    '    for (let fast = 0; fast < nums.length; fast++) {'],
    ['if_nonzero',  '        if (nums[fast] !== 0) {'],
    ['swap_temp',   '            let temp = nums[slow];'],
    ['swap_assign1','            nums[slow] = nums[fast];'],
    ['swap_assign2','            nums[fast] = temp;'],
    ['inc_slow',    '            slow++;'],
    ['',            '        }'],
    ['',            '    }'],
    ['',            '};'],
    ['',            ''],
    ['',            'function main() {'],
    ['m_read_elem', '    const nums = [0,1,0,3,12];'],
    ['m_call',      '    moveZeroes(nums);'],
    ['m_print',     '    console.log(nums);'],
    ['',            '}'],
    ['',            'main();']
  ],
  c: [
    ['',            '#include <stdio.h>'],
    ['',            ''],
    ['fn_entry',    'void moveZeroes(int* nums, int n) {'],
    ['init_slow',   '    int slow = 0;'],
    ['for_init',    '    for (int fast = 0; fast < n; fast++) {'],
    ['if_nonzero',  '        if (nums[fast] != 0) {'],
    ['swap_temp',   '            int temp = nums[slow];'],
    ['swap_assign1','            nums[slow] = nums[fast];'],
    ['swap_assign2','            nums[fast] = temp;'],
    ['inc_slow',    '            slow++;'],
    ['',            '        }'],
    ['',            '    }'],
    ['',            '}'],
    ['',            ''],
    ['',            'int main() {'],
    ['m_read_n',    '    int n;'],
    ['m_scanner',   '    scanf("%d", &n);'],
    ['m_alloc',     '    int nums[300];'],
    ['m_loop',      '    for (int i = 0; i < n; i++) {'],
    ['m_read_elem', '        scanf("%d", &nums[i]);'],
    ['',            '    }'],
    ['m_call',      '    moveZeroes(nums, n);'],
    ['m_print',     '    for (int i = 0; i < n; i++) printf("%d ", nums[i]);'],
    ['',            '    return 0;'],
    ['',            '}']
  ]
};

const PSEUDOCODE = [
  'function moveZeroes(nums):',
  '    slow = 0              // write pointer',
  '',
  '    for fast = 0 to n-1: // read pointer',
  '        if nums[fast] != 0:',
  '            swap(nums[slow], nums[fast])',
  '            slow++',
  '',
  '    // all non-zeros shifted left, zeros fill the tail'
];

// ─── Step Builder ─────────────────────────────────────────────────────────────
function parseNums(raw) {
  return raw.replace(/[^\d,\-\s]/g,'').split(/[,\s]+/).map(x=>parseInt(x.trim(),10)).filter(x=>!isNaN(x)).slice(0,12);
}

function buildSteps(rawNums) {
  const steps = [];
  const original = parseNums(rawNums);
  if (original.length < 1) return steps;

  const N = original.length;

  const NONE = {slowReady:false, fastReady:false, tempReady:false};
  const S    = {slowReady:true,  fastReady:false, tempReady:false};
  const SF   = {slowReady:true,  fastReady:true,  tempReady:false};
  const ALL  = {slowReady:true,  fastReady:true,  tempReady:true};

  function snap(arr, slow, fast, temp, code, phase, ist, extra) {
    return {arr:[...arr], n:arr.length, slow, fast, temp, code, phase, initState:{...ist}, ...extra};
  }

  // ── Input phase ──────────────────────────────────────────────────────────────
  const partialArr = new Array(N).fill('');
  steps.push({...snap(original,-1,-1,null,'m_scanner','input',NONE,{partial:[...partialArr]}),badge:'Scanner sc = new Scanner(System.in); → Init input.'});
  steps.push({...snap(original,-1,-1,null,'m_read_n','input',NONE,{partial:[...partialArr]}),badge:`int n = sc.nextInt(); → n = ${N}.`});
  steps.push({...snap(original,-1,-1,null,'m_alloc','input',NONE,{partial:[...partialArr]}),badge:`int[] nums = new int[${N}]; → Allocated.`});
  for (let ii=0;ii<N;ii++) {
    steps.push({...snap(original,-1,-1,null,'m_loop','input',NONE,{partial:[...partialArr],readIdx:ii}),badge:`for i=${ii}; i<${N}: Reading nums[${ii}].`});
    partialArr[ii]=original[ii];
    steps.push({...snap(original,-1,-1,null,'m_read_elem','input',NONE,{partial:[...partialArr],readIdx:ii}),badge:`nums[${ii}] = ${original[ii]}.`});
  }
  steps.push({...snap(original,-1,-1,null,'m_loop','input',NONE,{partial:[...partialArr]}),badge:`for exits. Array read. Calling moveZeroes().`});
  steps.push({...snap(original,-1,-1,null,'m_call','input',NONE,{partial:[...partialArr]}),badge:'new Solution().moveZeroes(nums); → Calling.'});

  // ── moveZeroes() ──────────────────────────────────────────────────────────────
  const arr = [...original];
  steps.push({...snap(arr,-1,-1,null,'fn_entry','fn',NONE,{}),badge:'moveZeroes(int[] nums) → Function entry.'});
  let slow = 0;
  steps.push({...snap(arr,slow,-1,null,'init_slow','fn',S,{}),badge:`int slow = 0; → slow = ${slow}. Write pointer at index 0.`});

  for (let fast=0; fast<N; fast++) {
    // for condition check
    steps.push({...snap(arr,slow,fast,null,'for_init','fn',SF,{condTrue:fast<N}),badge:`for(fast=${fast}; fast<${N}): ${fast}<${N} → ${fast<N?'TRUE.':'FALSE, exit.'}`});
    if (fast >= N) break;

    const isNonZero = arr[fast] !== 0;
    steps.push({...snap(arr,slow,fast,null,'if_nonzero','fn',SF,{isNonZero}),badge:`if(nums[fast]!=0): nums[${fast}]=${arr[fast]} ${isNonZero?'!= 0 → TRUE. Swap and advance slow.':'== 0 → FALSE. Skip (zero stays).'}`});

    if (isNonZero) {
      const tmp = arr[slow];
      steps.push({...snap(arr,slow,fast,tmp,'swap_temp','fn',ALL,{tempVal:tmp,swapSlow:slow,swapFast:fast}),badge:`temp = nums[slow]; → temp = nums[${slow}] = ${tmp}.`});
      arr[slow] = arr[fast];
      steps.push({...snap(arr,slow,fast,tmp,'swap_assign1','fn',ALL,{tempVal:tmp,justSwapped:slow,swapSlow:slow,swapFast:fast}),badge:`nums[slow] = nums[fast]; → nums[${slow}] = nums[${fast}] = ${arr[slow]}.`});
      arr[fast] = tmp;
      steps.push({...snap(arr,slow,fast,tmp,'swap_assign2','fn',ALL,{tempVal:tmp,justSwapped:fast,swapSlow:slow,swapFast:fast}),badge:`nums[fast] = temp; → nums[${fast}] = ${tmp}.`});
      slow++;
      steps.push({...snap(arr,slow,fast,null,'inc_slow','fn',SF,{swapDone:true}),badge:`slow++; → slow = ${slow}. Write pointer advances.`});
    }
  }

  // for exits (fast === N)
  steps.push({...snap(arr,slow,N,null,'for_init','done',SF,{condTrue:false}),badge:`for(fast=${N}; fast<${N}): ${N}<${N} → FALSE. Loop exits. All non-zeros moved to front.`});
  steps.push({...snap(arr,slow,N,null,'fn_entry','done',SF,{}),badge:`moveZeroes() complete. Result: [${arr.join(', ')}]. Non-zeros: ${slow}, zeros: ${N-slow}.`});

  return steps;
}

// ─── Reactive State ───────────────────────────────────────────────────────────
const inputNums   = ref('0,1,0,3,12');
const lang        = ref('java');
const speed       = ref(650);
const si          = ref(0);
const playing     = ref(false);
const vizHeight   = ref(310);
const tableHeight = ref(60);
const leftWidth   = ref(52);
const rightTab    = ref('code');

const stepsData = reactive({steps: buildSteps('0,1,0,3,12')});
const steps     = computed(()=>stepsData.steps);
const s         = computed(()=>steps.value[Math.max(0,Math.min(si.value,steps.value.length-1))]||{});
const codeLines = computed(()=>CODES[lang.value]||[]);
let playTimer = null;

function onChipsWheel(e){if(e.currentTarget)e.currentTarget.scrollLeft+=e.deltaY;}
function applyInput(){
  const nums=parseNums(inputNums.value);
  if(nums.length<1){alert('Enter at least 1 number.');return;}
  playing.value=false;stepsData.steps=buildSteps(inputNums.value);si.value=0;
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
const initState    = computed(()=>s.value.initState||{slowReady:false,fastReady:false,tempReady:false});
const displayArr   = computed(()=>s.value.phase==='input'?(s.value.partial||[]):(s.value.arr||[]));
const displaySlow  = computed(()=>initState.value.slowReady?s.value.slow:'?');
const displayFast  = computed(()=>initState.value.fastReady?s.value.fast:'?');
const displayTemp  = computed(()=>initState.value.tempReady?s.value.tempVal:'?');

// Count zeros from current arr state for the progress bar
const zeroCount = computed(()=>{
  if(!s.value.arr)return 0;
  return s.value.arr.filter(v=>v===0).length;
});

function numClass(idx){
  const phase=s.value.phase,ist=initState.value;
  if(phase==='input')return s.value.readIdx===idx?'mz-num-reading':'';
  if(phase==='done')return s.value.arr&&s.value.arr[idx]===0?'mz-num-zero-final':'mz-num-nz-final';
  const slow=s.value.slow,fast=s.value.fast;
  const js=s.value.justSwapped,ss=s.value.swapSlow,sf=s.value.swapFast;
  if(js===idx)return 'mz-num-just-swapped';
  if(ist.fastReady&&idx===fast&&idx===slow)return 'mz-num-both';
  if(ist.slowReady&&idx===slow)return 'mz-num-slow';
  if(ist.fastReady&&idx===fast)return 'mz-num-fast';
  if(ist.slowReady&&idx<slow)return 'mz-num-placed';
  return s.value.arr&&s.value.arr[idx]===0?'mz-num-zero':'';
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
              <label>nums[]:</label>
              <input type="text" v-model="inputNums" class="ll-text-input mz-nums-input" @keyup.enter="applyInput" placeholder="e.g. 0,1,0,3,12" />
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
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">n</span><b class="ll-c-blue">{{ s.n || displayArr.length }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">slow</span><b class="ll-c-orange">{{ displaySlow }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">fast</span><b class="ll-c-blue">{{ displayFast }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">temp</span><b class="ll-c-purple">{{ displayTemp }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">zeros</span><b class="ll-c-muted">{{ zeroCount }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">phase</span><b class="ll-c-orange">{{ s.phase || 'input' }}</b></span>
                  </div>

                  <!-- BOARD CONTAINER -->
                  <div class="ll-board-container">

                    <!-- If-result banner: shown at if_nonzero step -->
                    <div v-if="s.code==='if_nonzero'" class="mz-if-banner" :class="s.isNonZero?'mz-if-nonzero':'mz-if-zero'">
                      <span class="mz-if-icon">{{ s.isNonZero ? '&#10003;' : '&#10007;' }}</span>
                      <span>
                        nums[fast={{ s.fast }}] = {{ s.arr && s.arr[s.fast] }}
                        {{ s.isNonZero ? '&ne; 0 &rarr; Swap and advance slow' : '= 0 &rarr; Skip (zero stays)' }}
                      </span>
                    </div>

                    <!-- Tier 1: Array -->
                    <div class="mz-tier-title">
                      int[] nums
                      <span class="mz-badge">n = {{ s.n || displayArr.length }}</span>
                    </div>
                    <div class="mz-array-frame">
                      <!-- Pointer label row -->
                      <div class="mz-ptr-row">
                        <div v-for="(val, idx) in displayArr" :key="'ptr-'+idx" class="mz-ptr-cell">
                          <span v-if="initState.slowReady && s.phase!=='input' && idx===s.slow && s.phase!=='done'" class="mz-ptr-label mz-ptr-slow">slow</span>
                          <span v-if="initState.fastReady && s.phase!=='input' && idx===s.fast && s.phase!=='done' && s.fast < displayArr.length" class="mz-ptr-label mz-ptr-fast">fast</span>
                        </div>
                      </div>
                      <!-- Number boxes -->
                      <div class="mz-num-row">
                        <div
                          v-for="(val, idx) in displayArr"
                          :key="'n-'+idx"
                          class="mz-num-box"
                          :class="numClass(idx)"
                        >
                          <span class="mz-num-val">{{ val !== '' ? val : '\u00A0' }}</span>
                          <span class="mz-num-idx">[{{ idx }}]</span>
                        </div>
                      </div>
                    </div>

                    <!-- Swap panel: visible during swap steps -->
                    <div v-if="initState.tempReady" class="mz-swap-panel">
                      <div class="mz-swap-item mz-swap-slow">
                        <span class="mz-swap-lbl">nums[slow]</span>
                        <span class="mz-swap-idx">[{{ s.swapSlow }}]</span>
                        <span class="mz-swap-val">{{ s.arr && s.arr[s.swapSlow] }}</span>
                      </div>
                      <span class="mz-swap-arrow">&#8644;</span>
                      <div class="mz-swap-item mz-swap-fast">
                        <span class="mz-swap-lbl">nums[fast]</span>
                        <span class="mz-swap-idx">[{{ s.swapFast }}]</span>
                        <span class="mz-swap-val">{{ s.arr && s.arr[s.swapFast] }}</span>
                      </div>
                      <div class="mz-temp-item">
                        <span class="mz-swap-lbl">temp</span>
                        <span class="mz-swap-val">{{ s.tempVal }}</span>
                      </div>
                    </div>

                    <!-- Tier 2: Zone bar -->
                    <div class="mz-tier-title" style="margin-top:2px">
                      Partition State
                      <span v-if="initState.slowReady && s.phase!=='input'" class="mz-badge mz-badge-nz">non-zero: 0..{{ s.slow > 0 ? s.slow - 1 : '—' }}</span>
                      <span v-if="initState.slowReady && s.phase!=='input'" class="mz-badge mz-badge-z">zeros: tail</span>
                    </div>
                    <div class="mz-zone-bar-wrap">
                      <div
                        v-for="(val, idx) in displayArr"
                        :key="'z-'+idx"
                        class="mz-zone-cell"
                        :class="{
                          'mz-zone-nz':    s.phase!=='input' && s.phase!=='done' && initState.slowReady && idx < s.slow,
                          'mz-zone-done':  s.phase==='done' && s.arr && s.arr[idx]!==0,
                          'mz-zone-zeros': s.phase==='done' && s.arr && s.arr[idx]===0
                        }"
                      ></div>
                    </div>

                  </div>
                  <!-- END ll-board-container -->
                </div>
              </div>

              <div class="ll-vresizer" ref="vizResizerRef"></div>

              <!-- Legend -->
              <div class="ll-legend">
                <span class="ll-leg"><span class="ll-legdot" style="background:#fff7ed;border:1.5px solid #f97316;"></span>slow (write pointer)</span>
                <span class="ll-leg"><span class="ll-legdot" style="background:#eff6ff;border:1.5px solid #3b82f6;"></span>fast (read pointer)</span>
                <span class="ll-leg"><span class="ll-legdot mz-legdot-placed"></span>placed (non-zero)</span>
                <span class="ll-leg"><span class="ll-legdot mz-legdot-zero"></span>zero</span>
                <span class="ll-leg"><span class="ll-legdot mz-legdot-swapped"></span>swapped</span>
              </div>

              <!-- Call Stack -->
              <div class="ll-table-area" :style="{ height: tableHeight + 'px' }">
                <div class="ll-table-title">Call Stack &mdash; Current Execution Frame</div>
                <div class="ll-stack-line">
                  <template v-if="s.phase==='input'">
                    <div class="ll-frame ll-frame-cur">main() &nbsp; <span class="ll-fname">n</span>=<span class="ll-c-blue" style="font-weight:700">{{ s.n || displayArr.length }}</span><span class="ll-now"> &#9668; active</span></div>
                  </template>
                  <template v-else>
                    <div class="ll-frame" style="color:var(--text2);">main()</div>
                    <div class="ll-frame ll-frame-cur" style="margin-left:14px">
                      moveZeroes(nums) &nbsp;
                      <span class="ll-fname">slow</span>=<span class="ll-c-orange" style="font-weight:700">{{ displaySlow }}</span>,
                      <span class="ll-fname">fast</span>=<span class="ll-c-blue" style="font-weight:700">{{ displayFast }}</span>
                      <template v-if="initState.tempReady">, <span class="ll-fname">temp</span>=<span class="ll-c-purple" style="font-weight:700">{{ displayTemp }}</span></template>
                      <span class="ll-now"> &#9668; active</span>
                    </div>
                  </template>
                </div>
              </div>

              <div class="ll-vresizer" ref="tableResizerRef"></div>

              <!-- Badge -->
              <div class="ll-badge-wrap">
                <div class="ll-badge" :class="{'ll-badge-error':s.badge&&(s.badge.includes('FALSE')||s.badge.includes('== 0')),'ll-badge-success':s.badge&&(s.badge.includes('TRUE')||s.badge.includes('complete')||s.badge.includes('!= 0'))}">
                  {{ s.badge || 'Ready to visualize Move Zeroes.' }}
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
                  <h3 class="ll-cx-heading">Move Zeroes (LeetCode 283) &mdash; Complexity Analysis</h3>
                  <p class="ll-cx-intro">
                    Move all zeros in <code>nums</code> to the end while maintaining the relative order of non-zero elements.
                    Uses two pointers: <code>slow</code> (write) and <code>fast</code> (read). When <code>nums[fast] != 0</code>,
                    swap <code>nums[slow]</code> with <code>nums[fast]</code> and advance <code>slow</code>.
                  </p>
                  <h4 class="ll-cx-sub">Step-by-step Breakdown</h4>
                  <table class="ll-complexity-table">
                    <thead><tr><th>Operation</th><th>Time</th><th>Space</th><th>Reason</th></tr></thead>
                    <tbody>
                      <tr><td>Initialization (slow=0)</td><td class="ll-cx-good">O(1)</td><td class="ll-cx-good">O(1)</td><td>Single pointer setup</td></tr>
                      <tr><td>for loop (fast scan)</td><td class="ll-cx-good">O(n)</td><td class="ll-cx-good">O(1)</td><td>Each element visited once</td></tr>
                      <tr><td>Swap (when nonzero)</td><td class="ll-cx-good">O(1)</td><td class="ll-cx-good">O(1)</td><td>Three-step temp swap</td></tr>
                      <tr><td><strong>Total</strong></td><td class="ll-cx-good"><strong>O(n)</strong></td><td class="ll-cx-good"><strong>O(1)</strong></td><td>Single pass, in-place</td></tr>
                    </tbody>
                  </table>
                  <h4 class="ll-cx-sub">Overall Complexity</h4>
                  <div class="ll-cx-summary-grid">
                    <div class="ll-cx-card ll-cx-card-good"><div class="ll-cx-card-label">Time</div><div class="ll-cx-card-val">O(n)</div><div class="ll-cx-card-note">Single pass with fast pointer</div></div>
                    <div class="ll-cx-card ll-cx-card-good"><div class="ll-cx-card-label">Space</div><div class="ll-cx-card-val">O(1)</div><div class="ll-cx-card-note">In-place, only temp variable</div></div>
                    <div class="ll-cx-card ll-cx-card-good"><div class="ll-cx-card-label">Swaps</div><div class="ll-cx-card-val">&le; n</div><div class="ll-cx-card-note">At most n swaps (one per nonzero)</div></div>
                  </div>
                  <div class="ll-note">
                    <strong>Key insight:</strong> <code>slow</code> always points to the next position to write a non-zero value. When <code>nums[fast]</code> is non-zero, it gets swapped into <code>nums[slow]</code>. Since a zero was there previously (or <code>slow==fast</code>), all zeros naturally bubble to the end. Relative order of non-zeros is preserved because <code>fast</code> scans left to right.
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
@keyframes mz-swap-flash{0%{background:#fef9c3;transform:scale(1.12);}60%{background:#bfdbfe;}100%{transform:scale(1);}}
@keyframes mz-done-glow{0%{box-shadow:0 0 0 0 rgba(34,197,94,0);}60%{box-shadow:0 0 8px 3px rgba(34,197,94,.4);}100%{box-shadow:none;}}

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
.mz-nums-input{width:200px;}
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
.ll-c-blue{color:var(--blue);}.ll-c-green{color:var(--green);}.ll-c-orange{color:var(--orange);}.ll-c-purple{color:var(--purple);}.ll-c-muted{color:var(--muted);}

/* ─── Board Container ─────────────────────────────────────────────────────── */
.ll-board-container{display:flex;flex-direction:column;align-items:flex-start;padding:6px 14px 10px;gap:8px;min-width:0;width:100%;}

/* If-result banner */
.mz-if-banner{display:flex;align-items:center;gap:8px;padding:5px 12px;border-radius:var(--radius-sm);border-left:3px solid;font-size:11px;font-weight:600;width:100%;box-sizing:border-box;}
.mz-if-nonzero{background:#f0fdf4;border-color:#22c55e;color:#15803d;}
.mz-if-zero{background:#fef2f2;border-color:#ef4444;color:#991b1b;}
.mz-if-icon{font-size:13px;font-weight:900;}

.mz-tier-title{font-size:10px;font-weight:700;text-transform:uppercase;letter-spacing:.04em;color:var(--muted);font-family:'Consolas',monospace;border-left:3px solid var(--coral);padding-left:6px;display:flex;align-items:center;gap:6px;}
.mz-badge{font-size:9px;color:var(--muted);background:var(--surface2);border:1px solid var(--border);border-radius:3px;padding:0 4px;font-weight:500;text-transform:none;letter-spacing:0;}
.mz-badge-nz{background:#f0fdf4;border-color:#86efac;color:#065f46;}
.mz-badge-z{background:#fef2f2;border-color:#fca5a5;color:#991b1b;}

.mz-array-frame{background:#f8fafc;border:1px solid var(--border);border-radius:var(--radius);padding:8px 10px 6px;box-shadow:var(--shadow-sm);display:flex;flex-direction:column;gap:3px;width:100%;box-sizing:border-box;}
.mz-ptr-row{display:flex;gap:4px;min-height:22px;align-items:flex-end;}
.mz-ptr-cell{width:44px;display:flex;justify-content:center;align-items:flex-end;gap:2px;flex-shrink:0;}
.mz-ptr-label{font-size:7.5px;font-weight:800;font-family:monospace;padding:1px 3px;border-radius:3px;line-height:1.2;white-space:nowrap;}
.mz-ptr-slow{background:#fff7ed;color:#c2410c;border:1px solid #fdba74;}
.mz-ptr-fast{background:#eff6ff;color:#1d4ed8;border:1px solid #93c5fd;}
.mz-num-row{display:flex;gap:4px;flex-wrap:nowrap;}
.mz-num-box{width:44px;height:50px;border-radius:var(--radius-sm);background:#e2e8f0;border:1.5px solid #94a3b8;display:flex;flex-direction:column;align-items:center;justify-content:center;gap:2px;flex-shrink:0;transition:all .2s ease;}
.mz-num-val{font-family:'Cascadia Code','Fira Code',monospace;font-size:14px;font-weight:900;color:var(--text);line-height:1;}
.mz-num-idx{font-family:monospace;font-size:7.5px;color:var(--muted);font-weight:600;}

/* Cell states */
.mz-num-reading{background:#fef9c3!important;border:2px solid #eab308!important;}
.mz-num-slow{background:#fff7ed!important;border:2px solid #f97316!important;box-shadow:0 0 8px rgba(249,115,22,.35);transform:scale(1.06);}
.mz-num-fast{background:#eff6ff!important;border:2px solid var(--blue)!important;box-shadow:0 0 8px rgba(59,130,246,.35);transform:scale(1.06);}
.mz-num-both{background:#faf5ff!important;border:2px solid var(--purple)!important;box-shadow:0 0 10px rgba(147,51,234,.35);transform:scale(1.06);}
.mz-num-placed{background:#dcfce7!important;border:1.5px solid #86efac!important;}
.mz-num-zero{background:#fef2f2!important;border:1.5px solid #fca5a5!important;color:#991b1b;}
.mz-num-just-swapped{background:#fef9c3!important;border:2px solid #eab308!important;animation:mz-swap-flash .4s ease;z-index:5;}
.mz-num-nz-final{background:#dcfce7!important;border:1.5px solid #6ee7b7!important;animation:mz-done-glow .5s ease;}
.mz-num-zero-final{background:#f1f5f9!important;border:1.5px dashed #94a3b8!important;opacity:.7;}

/* Swap panel */
.mz-swap-panel{display:flex;align-items:center;gap:10px;padding:6px 12px;background:var(--surface2);border:1.5px solid var(--border);border-radius:var(--radius-sm);flex-wrap:wrap;width:100%;box-sizing:border-box;}
.mz-swap-item{display:flex;flex-direction:column;align-items:center;gap:2px;padding:4px 10px;border-radius:var(--radius-sm);min-width:64px;}
.mz-swap-slow{background:#fff7ed;border:1.5px solid #fdba74;}
.mz-swap-fast{background:#eff6ff;border:1.5px solid #93c5fd;}
.mz-swap-lbl{font-size:8.5px;color:var(--muted);font-weight:700;font-family:monospace;}
.mz-swap-idx{font-size:8px;color:var(--muted);}
.mz-swap-val{font-size:16px;font-weight:900;font-family:monospace;color:var(--text);}
.mz-swap-arrow{font-size:18px;color:var(--muted);}
.mz-temp-item{display:flex;flex-direction:column;align-items:center;gap:2px;padding:4px 10px;border-radius:var(--radius-sm);background:var(--purple-light);border:1.5px solid #d8b4fe;min-width:50px;}

/* Zone bar */
.mz-zone-bar-wrap{display:flex;gap:4px;height:8px;width:100%;flex-wrap:nowrap;}
.mz-zone-cell{height:8px;flex:1;border-radius:2px;background:var(--surface2);border:1px solid var(--border);transition:all .2s;}
.mz-zone-nz{background:#86efac!important;border-color:#22c55e!important;}
.mz-zone-done{background:#6ee7b7!important;border-color:#22c55e!important;}
.mz-zone-zeros{background:#e2e8f0!important;border-color:#94a3b8!important;opacity:.5;}

/* Legend */
.mz-legdot-placed{background:#dcfce7;border:1.5px solid #86efac;display:inline-block;width:11px;height:11px;border-radius:3px;}
.mz-legdot-zero{background:#fef2f2;border:1.5px solid #fca5a5;display:inline-block;width:11px;height:11px;border-radius:3px;}
.mz-legdot-swapped{background:#fef9c3;border:1.5px solid #eab308;display:inline-block;width:11px;height:11px;border-radius:3px;}

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
.ll-cx-good{color:#15803d;font-weight:700;}
.ll-cx-summary-grid{display:flex;gap:8px;flex-wrap:wrap;margin:6px 0 10px;}
.ll-cx-card{flex:1;min-width:90px;border-radius:var(--radius-sm);padding:8px 10px;text-align:center;border:1.5px solid var(--border);}
.ll-cx-card-good{background:#f0fdf4;border-color:#86efac;color:#15803d;}
.ll-cx-card-label{font-size:9px;font-weight:700;text-transform:uppercase;letter-spacing:.05em;opacity:.7;margin-bottom:4px;}
.ll-cx-card-val{font-size:13px;font-weight:800;font-family:monospace;margin-bottom:3px;}
.ll-cx-card-note{font-size:8.5px;opacity:.75;line-height:1.3;}
.ll-note{background:#fefce8;border:1px solid #fef08a;border-left:3px solid #eab308;padding:6px 10px;font-size:10.5px;color:#854d0e;border-radius:0 4px 4px 0;margin-top:10px;margin-bottom:120px;}
.ll-footer{display:flex;align-items:center;justify-content:space-between;padding:4px 12px;background:var(--surface);border-top:1px solid var(--border);font-size:11px;color:var(--muted);font-weight:600;flex-shrink:0;}
.ll-speed-wrap{display:flex;align-items:center;gap:6px;}
.ll-speed-wrap input[type="range"]{width:80px;accent-color:var(--coral);}
</style>
