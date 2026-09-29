<script setup>
import { ref, reactive, computed, onMounted, onBeforeUnmount, watch, nextTick } from 'vue';

defineProps({
  topic:    { type: String, default: 'Two Pointer Algorithms' },
  subTopic: { type: String, default: 'Rotate Array (LeetCode 189)' }
});

const CODES = {
  java: [
    ['',            'class Solution {'],
    ['fn_entry',    '    public void rotate(int[] nums, int k) {'],
    ['calc_n',      '        int n = nums.length;'],
    ['calc_k',      '        k = k % n;'],
    ['call_rev1',   '        reverse(nums, 0, n - 1);'],
    ['call_rev2',   '        reverse(nums, 0, k - 1);'],
    ['call_rev3',   '        reverse(nums, k, n - 1);'],
    ['',            '    }'],
    ['',            ''],
    ['rev_entry',   '    private void reverse(int[] nums, int start, int end) {'],
    ['rev_while',   '        while (start < end) {'],
    ['rev_temp',    '            int temp = nums[start];'],
    ['rev_assign1', '            nums[start] = nums[end];'],
    ['rev_assign2', '            nums[end] = temp;'],
    ['rev_inc',     '            start++;'],
    ['rev_dec',     '            end--;'],
    ['',            '        }'],
    ['',            '    }'],
    ['',            ''],
    ['',            '    public static void main(String[] args) {'],
    ['m_scanner',   '        java.util.Scanner sc = new java.util.Scanner(System.in);'],
    ['m_read_n',    '        int n = sc.nextInt();'],
    ['m_read_k',    '        int k = sc.nextInt();'],
    ['m_alloc',     '        int[] nums = new int[n];'],
    ['m_loop',      '        for (int i = 0; i < n; i++) {'],
    ['m_read_elem', '            nums[i] = sc.nextInt();'],
    ['',            '        }'],
    ['m_call',      '        new Solution().rotate(nums, k);'],
    ['m_print',     '        System.out.println(java.util.Arrays.toString(nums));'],
    ['',            '    }'],
    ['',            '}']
  ],
  cpp: [
    ['',            '#include <vector>'],
    ['',            '#include <algorithm>'],
    ['',            '#include <iostream>'],
    ['',            'using namespace std;'],
    ['',            ''],
    ['',            'class Solution {'],
    ['',            'public:'],
    ['fn_entry',    '    void rotate(vector<int>& nums, int k) {'],
    ['calc_n',      '        int n = nums.size();'],
    ['calc_k',      '        k = k % n;'],
    ['call_rev1',   '        reverse(nums, 0, n - 1);'],
    ['call_rev2',   '        reverse(nums, 0, k - 1);'],
    ['call_rev3',   '        reverse(nums, k, n - 1);'],
    ['',            '    }'],
    ['',            ''],
    ['rev_entry',   '    void reverse(vector<int>& nums, int start, int end) {'],
    ['rev_while',   '        while (start < end) {'],
    ['rev_temp',    '            int temp = nums[start];'],
    ['rev_assign1', '            nums[start] = nums[end];'],
    ['rev_assign2', '            nums[end] = temp;'],
    ['rev_inc',     '            start++;'],
    ['rev_dec',     '            end--;'],
    ['',            '        }'],
    ['',            '    }'],
    ['',            '};'],
    ['',            ''],
    ['',            'int main() {'],
    ['m_read_n',    '    int n, k;'],
    ['m_scanner',   '    cin >> n >> k;'],
    ['m_alloc',     '    vector<int> nums(n);'],
    ['m_loop',      '    for (int i = 0; i < n; i++) {'],
    ['m_read_elem', '        cin >> nums[i];'],
    ['',            '    }'],
    ['m_call',      '    Solution().rotate(nums, k);'],
    ['m_print',     '    for (int x : nums) cout << x << " ";'],
    ['',            '    return 0;'],
    ['',            '}']
  ],
  python: [
    ['',            'class Solution:'],
    ['fn_entry',    '    def rotate(self, nums, k):'],
    ['calc_n',      '        n = len(nums)'],
    ['calc_k',      '        k = k % n'],
    ['call_rev1',   '        self.reverse(nums, 0, n - 1)'],
    ['call_rev2',   '        self.reverse(nums, 0, k - 1)'],
    ['call_rev3',   '        self.reverse(nums, k, n - 1)'],
    ['',            ''],
    ['rev_entry',   '    def reverse(self, nums, start, end):'],
    ['rev_while',   '        while start < end:'],
    ['rev_temp',    '            temp = nums[start]'],
    ['rev_assign1', '            nums[start] = nums[end]'],
    ['rev_assign2', '            nums[end] = temp'],
    ['rev_inc',     '            start += 1'],
    ['rev_dec',     '            end -= 1'],
    ['',            ''],
    ['',            'def main():'],
    ['m_read_elem', '    data = list(map(int, input().split()))'],
    ['m_read_k',    '    k = int(input())'],
    ['m_call',      '    Solution().rotate(data, k)'],
    ['m_print',     '    print(data)'],
    ['',            ''],
    ['',            'main()']
  ],
  javascript: [
    ['',            '/**'],
    ['',            ' * @param {number[]} nums'],
    ['',            ' * @param {number} k'],
    ['',            ' * @return {void}'],
    ['',            ' */'],
    ['fn_entry',    'var rotate = function(nums, k) {'],
    ['calc_n',      '    const n = nums.length;'],
    ['calc_k',      '    k = k % n;'],
    ['call_rev1',   '    reverse(nums, 0, n - 1);'],
    ['call_rev2',   '    reverse(nums, 0, k - 1);'],
    ['call_rev3',   '    reverse(nums, k, n - 1);'],
    ['',            '};'],
    ['',            ''],
    ['rev_entry',   'function reverse(nums, start, end) {'],
    ['rev_while',   '    while (start < end) {'],
    ['rev_temp',    '        let temp = nums[start];'],
    ['rev_assign1', '        nums[start] = nums[end];'],
    ['rev_assign2', '        nums[end] = temp;'],
    ['rev_inc',     '        start++;'],
    ['rev_dec',     '        end--;'],
    ['',            '    }'],
    ['',            '}'],
    ['',            ''],
    ['',            'function main() {'],
    ['m_read_elem', '    const nums = [1,2,3,4,5,6,7];'],
    ['m_read_k',    '    const k = 3;'],
    ['m_call',      '    rotate(nums, k);'],
    ['m_print',     '    console.log(nums);'],
    ['',            '}'],
    ['',            'main();']
  ],
  c: [
    ['',            '#include <stdio.h>'],
    ['',            ''],
    ['rev_entry',   'void reverseArr(int* nums, int start, int end) {'],
    ['rev_while',   '    while (start < end) {'],
    ['rev_temp',    '        int temp = nums[start];'],
    ['rev_assign1', '        nums[start] = nums[end];'],
    ['rev_assign2', '        nums[end] = temp;'],
    ['rev_inc',     '        start++;'],
    ['rev_dec',     '        end--;'],
    ['',            '    }'],
    ['',            '}'],
    ['',            ''],
    ['fn_entry',    'void rotate(int* nums, int n, int k) {'],
    ['calc_k',      '    k = k % n;'],
    ['call_rev1',   '    reverseArr(nums, 0, n - 1);'],
    ['call_rev2',   '    reverseArr(nums, 0, k - 1);'],
    ['call_rev3',   '    reverseArr(nums, k, n - 1);'],
    ['',            '}'],
    ['',            ''],
    ['',            'int main() {'],
    ['m_read_n',    '    int n, k;'],
    ['m_scanner',   '    scanf("%d %d", &n, &k);'],
    ['m_alloc',     '    int nums[300];'],
    ['m_loop',      '    for (int i = 0; i < n; i++) {'],
    ['m_read_elem', '        scanf("%d", &nums[i]);'],
    ['',            '    }'],
    ['m_call',      '    rotate(nums, n, k);'],
    ['m_print',     '    for (int i = 0; i < n; i++) printf("%d ", nums[i]);'],
    ['',            '    return 0;'],
    ['',            '}']
  ]
};

const PSEUDOCODE = [
  'function rotate(nums, k):',
  '    n = len(nums)',
  '    k = k % n',
  '    reverse(nums, 0, n-1)   // reverse whole array',
  '    reverse(nums, 0, k-1)   // reverse first k elements',
  '    reverse(nums, k, n-1)   // reverse last n-k elements',
  '',
  'function reverse(nums, start, end):',
  '    while start < end:',
  '        swap nums[start] and nums[end]',
  '        start++',
  '        end--'
];

// ─── Step Builder ─────────────────────────────────────────────────────────────
function parseNums(raw) {
  return raw.replace(/[^\d,\-\s]/g,'').split(/[,\s]+/).map(x=>parseInt(x.trim(),10)).filter(x=>!isNaN(x)).slice(0,12);
}

function buildSteps(rawNums, kRaw) {
  const steps = [];
  const original = parseNums(rawNums);
  const kIn = parseInt(kRaw, 10);
  if (original.length < 1 || isNaN(kIn) || kIn < 0) return steps;

  const N  = original.length;
  const K  = ((kIn % N) + N) % N;

  const NONE = {nReady:false, kReady:false, startReady:false, endReady:false};
  const NK   = {nReady:true,  kReady:true,  startReady:false, endReady:false};
  const ALL  = {nReady:true,  kReady:true,  startReady:true,  endReady:true};

  function snap(arr, n, k, start, end, code, phase, ist, extra) {
    return {arr:[...arr], n, k, start, end, code, phase, initState:{...ist}, ...extra};
  }

  // ── Input phase ──────────────────────────────────────────────────────────────
  const partialArr = new Array(N).fill('');
  steps.push({...snap(original, N, K, -1,-1,'m_scanner','input',NONE,{partial:[...partialArr]}), badge:'Scanner sc = new Scanner(System.in); → Init input.'});
  steps.push({...snap(original, N, K, -1,-1,'m_read_n','input',NONE,{partial:[...partialArr]}), badge:`int n = sc.nextInt(); → n = ${N}.`});
  steps.push({...snap(original, N, K, -1,-1,'m_read_k','input',NONE,{partial:[...partialArr]}), badge:`int k = sc.nextInt(); → k (raw) = ${kIn}.`});
  steps.push({...snap(original, N, K, -1,-1,'m_alloc','input',NONE,{partial:[...partialArr]}), badge:`int[] nums = new int[${N}]; → Allocated.`});
  for (let ii=0;ii<N;ii++) {
    steps.push({...snap(original,N,K,-1,-1,'m_loop','input',NONE,{partial:[...partialArr],readIdx:ii}), badge:`for i=${ii}; i<${N}: Reading nums[${ii}].`});
    partialArr[ii]=original[ii];
    steps.push({...snap(original,N,K,-1,-1,'m_read_elem','input',NONE,{partial:[...partialArr],readIdx:ii}), badge:`nums[${ii}] = ${original[ii]}.`});
  }
  steps.push({...snap(original,N,K,-1,-1,'m_loop','input',NONE,{partial:[...partialArr]}), badge:`for exits. Array read. Calling rotate(nums, ${kIn}).`});
  steps.push({...snap(original,N,K,-1,-1,'m_call','input',NONE,{partial:[...partialArr]}), badge:`rotate(nums, ${kIn}); → Calling.`});

  // ── rotate() ──────────────────────────────────────────────────────────────────
  const arr = [...original];
  steps.push({...snap(arr,N,K,-1,-1,'fn_entry','rotate',NONE,{}), badge:`rotate(int[] nums, int k) → Entry. n=${N}, k(raw)=${kIn}.`});
  steps.push({...snap(arr,N,K,-1,-1,'calc_n','rotate',{nReady:true,kReady:false,startReady:false,endReady:false},{}), badge:`n = nums.length; → n = ${N}.`});
  steps.push({...snap(arr,N,K,-1,-1,'calc_k','rotate',NK,{}), badge:`k = k % n; → k = ${kIn} % ${N} = ${K}. Will rotate right by ${K} positions.`});

  // Helper to animate a reverse sub-call
  function animateReverse(arr, start0, end0, phaseLabel, callCode) {
    steps.push({...snap(arr,N,K,start0,end0,callCode,'rotate',NK,{revPhase:phaseLabel,revStart:start0,revEnd:end0}), badge:`${callCode==='call_rev1'?'reverse(nums, 0, n-1)':callCode==='call_rev2'?`reverse(nums, 0, k-1=${K-1})`:`reverse(nums, k, n-1=${N-1})`} → Entering reverse(start=${start0}, end=${end0}).`});
    steps.push({...snap(arr,N,K,start0,end0,'rev_entry','reverse',NK,{revPhase:phaseLabel,revStart:start0,revEnd:end0}), badge:`reverse(int[] nums, int start, int end) → start=${start0}, end=${end0}.`});
    let s=start0,e=end0;
    while (s<e) {
      steps.push({...snap(arr,N,K,s,e,'rev_while','reverse',ALL,{revPhase:phaseLabel,revStart:start0,revEnd:end0}), badge:`while(start<end): ${s}<${e} → TRUE. Will swap nums[${s}]=${arr[s]} with nums[${e}]=${arr[e]}.`});
      const tmp=arr[s];
      steps.push({...snap(arr,N,K,s,e,'rev_temp','reverse',ALL,{revPhase:phaseLabel,revStart:start0,revEnd:end0,tempVal:tmp}), badge:`temp = nums[start]; → temp = nums[${s}] = ${tmp}.`});
      arr[s]=arr[e];
      steps.push({...snap(arr,N,K,s,e,'rev_assign1','reverse',ALL,{revPhase:phaseLabel,revStart:start0,revEnd:end0,tempVal:tmp,swapIdx:s}), badge:`nums[start] = nums[end]; → nums[${s}] = nums[${e}] = ${arr[s]}.`});
      arr[e]=tmp;
      steps.push({...snap(arr,N,K,s,e,'rev_assign2','reverse',ALL,{revPhase:phaseLabel,revStart:start0,revEnd:end0,tempVal:tmp,swapIdx:e}), badge:`nums[end] = temp; → nums[${e}] = ${tmp}.`});
      s++;
      steps.push({...snap(arr,N,K,s,e,'rev_inc','reverse',ALL,{revPhase:phaseLabel,revStart:start0,revEnd:end0}), badge:`start++; → start = ${s}.`});
      e--;
      steps.push({...snap(arr,N,K,s,e,'rev_dec','reverse',ALL,{revPhase:phaseLabel,revStart:start0,revEnd:end0}), badge:`end--; → end = ${e}.`});
    }
    steps.push({...snap(arr,N,K,s,e,'rev_while','reverse',NK,{revPhase:phaseLabel,revStart:start0,revEnd:end0}), badge:`while(start<end): ${s}<${e} → FALSE. reverse() done for phase: ${phaseLabel}.`});
  }

  if (K === 0) {
    steps.push({...snap(arr,N,K,-1,-1,'calc_k','done',NK,{}), badge:`k = 0 after mod — no rotation needed.`});
  } else {
    animateReverse(arr, 0, N-1, 'rev-all', 'call_rev1');
    animateReverse(arr, 0, K-1, 'rev-first-k', 'call_rev2');
    animateReverse(arr, K, N-1, 'rev-last-nk', 'call_rev3');
  }

  steps.push({...snap(arr,N,K,-1,-1,'fn_entry','done',NK,{}), badge:`rotate() complete. Result: [${arr.join(', ')}]. Rotated right by ${K} positions.`});
  return steps;
}

// ─── Reactive State ───────────────────────────────────────────────────────────
const inputNums   = ref('1,2,3,4,5,6,7');
const inputK      = ref('3');
const lang        = ref('java');
const speed       = ref(650);
const si          = ref(0);
const playing     = ref(false);
const vizHeight   = ref(340);
const tableHeight = ref(60);
const leftWidth   = ref(52);
const rightTab    = ref('code');

const stepsData = reactive({steps: buildSteps('1,2,3,4,5,6,7','3')});
const steps     = computed(()=>stepsData.steps);
const s         = computed(()=>steps.value[Math.max(0,Math.min(si.value,steps.value.length-1))]||{});
const codeLines = computed(()=>CODES[lang.value]||[]);
let playTimer=null;

function onChipsWheel(e){if(e.currentTarget)e.currentTarget.scrollLeft+=e.deltaY;}
function applyInput(){
  const nums=parseNums(inputNums.value);
  const k=parseInt(inputK.value,10);
  if(nums.length<1){alert('Enter at least 1 number.');return;}
  if(isNaN(k)||k<0){alert('Enter a valid k (>=0).');return;}
  playing.value=false;
  stepsData.steps=buildSteps(inputNums.value,inputK.value);
  si.value=0;
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

// ─── Computed display helpers ─────────────────────────────────────────────────
const initState    = computed(()=>s.value.initState||{nReady:false,kReady:false,startReady:false,endReady:false});
const displayArr   = computed(()=>s.value.phase==='input'?(s.value.partial||[]):(s.value.arr||[]));
const displayN     = computed(()=>initState.value.nReady?s.value.n:'?');
const displayK     = computed(()=>initState.value.kReady?s.value.k:'?');
const displayStart = computed(()=>initState.value.startReady?s.value.start:'?');
const displayEnd   = computed(()=>initState.value.endReady?s.value.end:'?');

// Phase label for the banner
const phaseBanner = computed(()=>{
  const rp=s.value.revPhase;
  if(!rp)return null;
  if(rp==='rev-all')return{label:'Phase 1: Reverse All',color:'#7c3aed',bg:'#ede9fe'};
  if(rp==='rev-first-k')return{label:'Phase 2: Reverse First k',color:'#0369a1',bg:'#e0f2fe'};
  return {label:'Phase 3: Reverse Last n-k',color:'#065f46',bg:'#d1fae5'};
});

function numClass(idx){
  const phase=s.value.phase,ist=initState.value,rp=s.value.revPhase;
  if(phase==='input')return s.value.readIdx===idx?'ra-num-reading':'';
  if(phase==='done')return 'ra-num-done';
  const start=s.value.start,end=s.value.end;
  const rs=s.value.revStart,re=s.value.revEnd;
  // mark the reverse sub-range
  if(rp&&rs!==undefined&&re!==undefined){
    if(idx===start&&idx===end)return 'ra-num-start ra-num-end';
    if(idx===start)return 'ra-num-start';
    if(idx===end)return 'ra-num-end';
    if(rs!==undefined&&idx>rs&&idx<re&&idx!==start&&idx!==end)return 'ra-num-inrange';
  }
  return '';
}

function showPtr(type,idx){
  const ist=initState.value,phase=s.value.phase;
  if(type==='start')return ist.startReady&&idx===s.value.start&&phase==='reverse';
  if(type==='end')return ist.endReady&&idx===s.value.end&&phase==='reverse';
  return false;
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
  cleanupFns.push(initVResizer(vizResizerRef,vizHeight,220,700));
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
              <input type="text" v-model="inputNums" class="ll-text-input ra-nums-input" @keyup.enter="applyInput" placeholder="e.g. 1,2,3,4,5,6,7" />
            </div>
            <div class="ll-input-group">
              <label>k:</label>
              <input type="number" min="0" v-model="inputK" class="ll-text-input" style="width:54px" @keyup.enter="applyInput" placeholder="3" />
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
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">n</span><b class="ll-c-blue">{{ displayN }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">k</span><b class="ll-c-orange">{{ displayK }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">start</span><b class="ll-c-purple">{{ displayStart }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">end</span><b class="ll-c-green">{{ displayEnd }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">temp</span><b class="ll-c-orange">{{ s.tempVal !== undefined ? s.tempVal : '?' }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">phase</span><b class="ll-c-orange">{{ s.phase || 'input' }}</b></span>
                  </div>

                  <!-- BOARD CONTAINER -->
                  <div class="ll-board-container">

                    <!-- Phase banner -->
                    <div v-if="phaseBanner" class="ra-phase-banner" :style="{ background: phaseBanner.bg, color: phaseBanner.color, borderColor: phaseBanner.color }">
                      {{ phaseBanner.label }}
                      <span v-if="s.revPhase" class="ra-phase-range">&nbsp;[{{ s.revStart }}..{{ s.revEnd }}]</span>
                    </div>

                    <!-- Tier 1: Array -->
                    <div class="ra-tier-title">
                      int[] nums
                      <span class="ra-badge">n = {{ displayN }}</span>
                      <span v-if="initState.kReady" class="ra-badge ra-badge-k">k = {{ displayK }}</span>
                    </div>

                    <div class="ra-array-frame">
                      <!-- Pointer label row -->
                      <div class="ra-ptr-row">
                        <div v-for="(val, idx) in displayArr" :key="'ptr-'+idx" class="ra-ptr-cell">
                          <span v-if="initState.startReady && s.phase==='reverse' && idx===s.start" class="ra-ptr-label ra-ptr-start">S</span>
                          <span v-if="initState.endReady   && s.phase==='reverse' && idx===s.end"   class="ra-ptr-label ra-ptr-end">E</span>
                        </div>
                      </div>
                      <!-- Number boxes -->
                      <div class="ra-num-row">
                        <div
                          v-for="(val, idx) in displayArr"
                          :key="'n-'+idx"
                          class="ra-num-box"
                          :class="numClass(idx)"
                        >
                          <span class="ra-num-val">{{ val !== '' ? val : '\u00A0' }}</span>
                          <span class="ra-num-idx">[{{ idx }}]</span>
                        </div>
                      </div>
                      <!-- Phase range underline bar -->
                      <div v-if="s.revPhase && s.revStart !== undefined" class="ra-range-bar-wrap">
                        <div
                          class="ra-range-bar"
                          :class="{
                            'ra-range-rev-all':   s.revPhase==='rev-all',
                            'ra-range-first-k':   s.revPhase==='rev-first-k',
                            'ra-range-last-nk':   s.revPhase==='rev-last-nk'
                          }"
                          :style="{
                            marginLeft: (s.revStart * 48) + 'px',
                            width: ((s.revEnd - s.revStart + 1) * 48 - 4) + 'px'
                          }"
                        ></div>
                      </div>
                    </div>

                    <!-- Tier 2: Swap state panel (shown during reverse) -->
                    <div v-if="s.phase==='reverse' && initState.startReady" class="ra-swap-panel">
                      <div class="ra-swap-item ra-swap-start">
                        <span class="ra-swap-label">nums[start]</span>
                        <span class="ra-swap-idx">[{{ s.start }}]</span>
                        <span class="ra-swap-val">{{ s.arr && s.arr[s.start] }}</span>
                      </div>
                      <div class="ra-swap-arrow">&#8644;</div>
                      <div class="ra-swap-item ra-swap-end">
                        <span class="ra-swap-label">nums[end]</span>
                        <span class="ra-swap-idx">[{{ s.end }}]</span>
                        <span class="ra-swap-val">{{ s.arr && s.arr[s.end] }}</span>
                      </div>
                      <div v-if="s.tempVal !== undefined" class="ra-temp-item">
                        <span class="ra-swap-label">temp</span>
                        <span class="ra-swap-val">{{ s.tempVal }}</span>
                      </div>
                    </div>

                    <!-- Tier 3: Progress strip showing phase regions -->
                    <div class="ra-tier-title" style="margin-top:4px">
                      Phase Progress
                    </div>
                    <div class="ra-progress-strip">
                      <div
                        v-for="(val, idx) in displayArr"
                        :key="'pr-'+idx"
                        class="ra-progress-cell"
                        :class="{
                          'ra-prog-first-k':  initState.kReady && idx < s.k,
                          'ra-prog-last-nk':  initState.kReady && idx >= s.k,
                          'ra-prog-reversed': s.phase==='done'
                        }"
                      >
                        {{ val !== '' ? val : '' }}
                      </div>
                    </div>
                    <div v-if="initState.kReady" class="ra-progress-labels">
                      <span class="ra-prog-label-first" :style="{ width: (s.k * 48) + 'px' }">first k={{s.k}}</span>
                      <span class="ra-prog-label-last" :style="{ width: ((s.n - s.k) * 48 - 4) + 'px' }">last n-k={{ s.n - s.k }}</span>
                    </div>

                  </div>
                  <!-- END ll-board-container -->
                </div>
              </div>

              <div class="ll-vresizer" ref="vizResizerRef"></div>

              <!-- Legend -->
              <div class="ll-legend">
                <span class="ll-leg"><span class="ll-legdot" style="background:#f3e8ff;border:1.5px solid #9333ea;"></span>start</span>
                <span class="ll-leg"><span class="ll-legdot" style="background:#f0fdf4;border:1.5px solid #22c55e;"></span>end</span>
                <span class="ll-leg"><span class="ll-legdot ra-legdot-range"></span>in range</span>
                <span class="ll-leg"><span class="ll-legdot ra-legdot-done"></span>reversed</span>
                <span class="ll-leg"><span class="ll-legdot" style="background:#e0f2fe;border:1.5px solid #0369a1;"></span>first k</span>
                <span class="ll-leg"><span class="ll-legdot" style="background:#d1fae5;border:1.5px solid #059669;"></span>last n-k</span>
              </div>

              <!-- Call Stack -->
              <div class="ll-table-area" :style="{ height: tableHeight + 'px' }">
                <div class="ll-table-title">Call Stack &mdash; Current Execution Frame</div>
                <div class="ll-stack-line">
                  <template v-if="s.phase==='input'">
                    <div class="ll-frame ll-frame-cur">main() &nbsp; <span class="ll-fname">n</span>=<span class="ll-c-blue" style="font-weight:700">{{ displayN }}</span>, <span class="ll-fname">k</span>=<span class="ll-c-orange" style="font-weight:700">{{ displayK }}</span><span class="ll-now"> &#9668; active</span></div>
                  </template>
                  <template v-else-if="s.phase==='reverse'">
                    <div class="ll-frame" style="color:var(--text2);">main()</div>
                    <div class="ll-frame" style="color:var(--text2);margin-left:14px">rotate(nums, k={{ displayK }})</div>
                    <div class="ll-frame ll-frame-cur" style="margin-left:28px">
                      reverse(nums, start={{ displayStart }}, end={{ displayEnd }})
                      <span class="ll-now"> &#9668; active</span>
                    </div>
                  </template>
                  <template v-else>
                    <div class="ll-frame" style="color:var(--text2);">main()</div>
                    <div class="ll-frame ll-frame-cur" style="margin-left:14px">
                      rotate(nums, k={{ displayK }}) &nbsp;
                      <span class="ll-fname">n</span>=<span class="ll-c-blue" style="font-weight:700">{{ displayN }}</span>
                      <span class="ll-now"> &#9668; active</span>
                    </div>
                  </template>
                </div>
              </div>

              <div class="ll-vresizer" ref="tableResizerRef"></div>

              <!-- Badge -->
              <div class="ll-badge-wrap">
                <div class="ll-badge" :class="{'ll-badge-error':s.badge&&s.badge.includes('FALSE'),'ll-badge-success':s.badge&&(s.badge.includes('complete')||s.badge.includes('TRUE')||s.badge.includes('done'))}">
                  {{ s.badge || 'Ready to visualize Rotate Array.' }}
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
                  <h3 class="ll-cx-heading">Rotate Array (LeetCode 189) &mdash; Complexity Analysis</h3>
                  <p class="ll-cx-intro">
                    Rotate array <code>nums</code> to the right by <code>k</code> steps in-place.
                    The three-reverse trick works by reversing the entire array, then reversing the first
                    <code>k</code> elements, then reversing the remaining <code>n-k</code> elements.
                  </p>
                  <h4 class="ll-cx-sub">Step-by-step Breakdown</h4>
                  <table class="ll-complexity-table">
                    <thead><tr><th>Operation</th><th>Time</th><th>Space</th><th>Reason</th></tr></thead>
                    <tbody>
                      <tr><td>k = k % n</td><td class="ll-cx-good">O(1)</td><td class="ll-cx-good">O(1)</td><td>Normalize k</td></tr>
                      <tr><td>reverse(0, n-1)</td><td class="ll-cx-good">O(n)</td><td class="ll-cx-good">O(1)</td><td>n/2 swaps</td></tr>
                      <tr><td>reverse(0, k-1)</td><td class="ll-cx-good">O(k)</td><td class="ll-cx-good">O(1)</td><td>k/2 swaps</td></tr>
                      <tr><td>reverse(k, n-1)</td><td class="ll-cx-good">O(n-k)</td><td class="ll-cx-good">O(1)</td><td>(n-k)/2 swaps</td></tr>
                      <tr><td><strong>Total</strong></td><td class="ll-cx-good"><strong>O(n)</strong></td><td class="ll-cx-good"><strong>O(1)</strong></td><td>Three linear passes, no extra array</td></tr>
                    </tbody>
                  </table>
                  <h4 class="ll-cx-sub">Overall Complexity</h4>
                  <div class="ll-cx-summary-grid">
                    <div class="ll-cx-card ll-cx-card-good"><div class="ll-cx-card-label">Time</div><div class="ll-cx-card-val">O(n)</div><div class="ll-cx-card-note">Three in-place reversal passes</div></div>
                    <div class="ll-cx-card ll-cx-card-good"><div class="ll-cx-card-label">Space</div><div class="ll-cx-card-val">O(1)</div><div class="ll-cx-card-note">Only a temp variable for swap</div></div>
                    <div class="ll-cx-card ll-cx-card-good"><div class="ll-cx-card-label">Passes</div><div class="ll-cx-card-val">3</div><div class="ll-cx-card-note">Rev-all, Rev-first-k, Rev-last-nk</div></div>
                  </div>
                  <div class="ll-note">
                    <strong>Key insight:</strong> Reversing the whole array moves the last <code>k</code> elements to the front (but reversed). Then reversing each half separately un-reverses them into the correct order. Result: right-rotation by <code>k</code> with zero extra memory.
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
.ll-root *{box-sizing:border-box;}
.ll-root *,.ll-root,.row-main{scrollbar-width:none!important;-ms-overflow-style:none!important;}
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
@keyframes ra-swap-flash{0%{background:#fef9c3;}60%{background:#bfdbfe;}100%{background:initial;}}
@keyframes ra-done-glow{0%{box-shadow:0 0 0 0 rgba(34,197,94,0);}60%{box-shadow:0 0 8px 3px rgba(34,197,94,.4);}100%{box-shadow:none;}}

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
.ra-nums-input{width:190px;}
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

.ra-phase-banner{
  display:flex;align-items:center;padding:4px 12px;border-radius:var(--radius-sm);
  border-left:3px solid;font-size:11px;font-weight:700;width:100%;box-sizing:border-box;
}
.ra-phase-range{font-size:10px;font-weight:500;margin-left:4px;opacity:.8;}

.ra-tier-title{font-size:10px;font-weight:700;text-transform:uppercase;letter-spacing:.04em;color:var(--muted);font-family:'Consolas',monospace;border-left:3px solid var(--coral);padding-left:6px;display:flex;align-items:center;gap:6px;}
.ra-badge{font-size:9px;color:var(--muted);background:var(--surface2);border:1px solid var(--border);border-radius:3px;padding:0 4px;font-weight:500;text-transform:none;letter-spacing:0;}
.ra-badge-k{background:#fff7ed;border-color:#fdba74;color:#c2410c;}

.ra-array-frame{background:#f8fafc;border:1px solid var(--border);border-radius:var(--radius);padding:8px 10px 6px;box-shadow:var(--shadow-sm);display:flex;flex-direction:column;gap:3px;width:100%;box-sizing:border-box;}
.ra-ptr-row{display:flex;gap:4px;min-height:20px;align-items:flex-end;}
.ra-ptr-cell{width:44px;display:flex;justify-content:center;align-items:flex-end;gap:2px;flex-shrink:0;}
.ra-ptr-label{font-size:8.5px;font-weight:800;font-family:monospace;padding:1px 4px;border-radius:3px;line-height:1.2;}
.ra-ptr-start{background:#f3e8ff;color:#7e22ce;border:1px solid #d8b4fe;}
.ra-ptr-end{background:#f0fdf4;color:#15803d;border:1px solid #86efac;}
.ra-num-row{display:flex;gap:4px;flex-wrap:nowrap;}
.ra-num-box{width:44px;height:50px;border-radius:var(--radius-sm);background:#e2e8f0;border:1.5px solid #94a3b8;display:flex;flex-direction:column;align-items:center;justify-content:center;gap:2px;flex-shrink:0;transition:all .2s ease;}
.ra-num-val{font-family:'Cascadia Code','Fira Code',monospace;font-size:14px;font-weight:900;color:var(--text);line-height:1;}
.ra-num-idx{font-family:monospace;font-size:7.5px;color:var(--muted);font-weight:600;}

/* Cell states */
.ra-num-reading{background:#fef9c3!important;border:2px solid #eab308!important;}
.ra-num-start{background:#f3e8ff!important;border:2px solid #9333ea!important;box-shadow:0 0 8px rgba(147,51,234,.35);transform:scale(1.06);}
.ra-num-end{background:#f0fdf4!important;border:2px solid var(--green)!important;box-shadow:0 0 8px rgba(34,197,94,.35);transform:scale(1.06);}
.ra-num-inrange{background:#faf5ff!important;border:1.5px solid #d8b4fe!important;}
.ra-num-done{background:#d1fae5!important;border:1.5px solid #6ee7b7!important;animation:ra-done-glow .6s ease;}

/* Range bar under array */
.ra-range-bar-wrap{position:relative;height:4px;margin-top:2px;}
.ra-range-bar{height:4px;border-radius:2px;position:absolute;top:0;left:0;transition:all .2s;}
.ra-range-rev-all{background:#9333ea;}
.ra-range-first-k{background:#0369a1;}
.ra-range-last-nk{background:#059669;}

/* Swap panel */
.ra-swap-panel{display:flex;align-items:center;gap:10px;padding:6px 12px;background:var(--surface2);border:1.5px solid var(--border);border-radius:var(--radius-sm);flex-wrap:wrap;width:100%;box-sizing:border-box;}
.ra-swap-item{display:flex;flex-direction:column;align-items:center;gap:2px;padding:4px 10px;border-radius:var(--radius-sm);min-width:60px;}
.ra-swap-start{background:#f3e8ff;border:1.5px solid #d8b4fe;}
.ra-swap-end{background:#f0fdf4;border:1.5px solid #86efac;}
.ra-swap-label{font-size:8.5px;color:var(--muted);font-weight:700;font-family:monospace;}
.ra-swap-idx{font-size:8px;color:var(--muted);}
.ra-swap-val{font-size:16px;font-weight:900;font-family:monospace;color:var(--text);}
.ra-swap-arrow{font-size:18px;color:var(--muted);}
.ra-temp-item{display:flex;flex-direction:column;align-items:center;gap:2px;padding:4px 10px;border-radius:var(--radius-sm);background:#fff7ed;border:1.5px solid #fdba74;min-width:50px;}

/* Progress strip */
.ra-progress-strip{display:flex;gap:4px;flex-wrap:nowrap;}
.ra-progress-cell{width:44px;height:26px;border-radius:3px;display:flex;align-items:center;justify-content:center;font-size:10px;font-weight:700;font-family:monospace;background:var(--surface2);border:1px solid var(--border);color:var(--muted);transition:all .2s;flex-shrink:0;}
.ra-prog-first-k{background:#e0f2fe!important;border-color:#7dd3fc!important;color:#0369a1!important;}
.ra-prog-last-nk{background:#d1fae5!important;border-color:#6ee7b7!important;color:#065f46!important;}
.ra-prog-reversed{background:#f0fdf4!important;border-color:#86efac!important;color:#15803d!important;}
.ra-progress-labels{display:flex;font-size:8.5px;font-family:monospace;color:var(--muted);gap:4px;overflow:hidden;}
.ra-prog-label-first{color:#0369a1;font-weight:600;text-align:center;overflow:hidden;white-space:nowrap;text-overflow:ellipsis;flex-shrink:0;}
.ra-prog-label-last{color:#059669;font-weight:600;text-align:center;overflow:hidden;white-space:nowrap;text-overflow:ellipsis;flex-shrink:0;}

/* Legend */
.ra-legdot-range{background:#f3e8ff;border:1.5px solid #d8b4fe;display:inline-block;width:11px;height:11px;border-radius:3px;}
.ra-legdot-done{background:#d1fae5;border:1.5px solid #6ee7b7;display:inline-block;width:11px;height:11px;border-radius:3px;}

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
.ll-cx-card-label{font-size:9px;font-weight:700;text-transform:uppercase;letter-spacing:.05em;opacity:.7;margin-bottom:4px;}
.ll-cx-card-val{font-size:13px;font-weight:800;font-family:monospace;margin-bottom:3px;}
.ll-cx-card-note{font-size:8.5px;opacity:.75;line-height:1.3;}
.ll-note{background:#fefce8;border:1px solid #fef08a;border-left:3px solid #eab308;padding:6px 10px;font-size:10.5px;color:#854d0e;border-radius:0 4px 4px 0;margin-top:10px;margin-bottom:120px;}
.ll-footer{display:flex;align-items:center;justify-content:space-between;padding:4px 12px;background:var(--surface);border-top:1px solid var(--border);font-size:11px;color:var(--muted);font-weight:600;flex-shrink:0;}
.ll-speed-wrap{display:flex;align-items:center;gap:6px;}
.ll-speed-wrap input[type="range"]{width:80px;accent-color:var(--coral);}
</style>
