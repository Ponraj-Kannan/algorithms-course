<script setup>
import { ref, reactive, computed, onMounted, onBeforeUnmount, watch, nextTick } from 'vue';

defineProps({
  topic:    { type: String, default: 'Two Pointer Algorithms' },
  subTopic: { type: String, default: '3Sum (LeetCode 15)' }
});

const CODES = {
  java: [
    ['',           'import java.util.*;'],
    ['',           ''],
    ['',           'class Solution {'],
    ['fn_entry',   '    public List<List<Integer>> threeSum(int[] nums) {'],
    ['sort_call',  '        Arrays.sort(nums);'],
    ['init_res',   '        List<List<Integer>> res = new ArrayList<>();'],
    ['outer_init', '        for (int i = 0; i < nums.length - 2; i++) {'],
    ['skip_cond',  '            if (i > 0 && nums[i] == nums[i - 1]) {'],
    ['skip_cont',  '                continue;'],
    ['',           '            }'],
    ['init_left',  '            int left = i + 1;'],
    ['init_right', '            int right = nums.length - 1;'],
    ['while_cond', '            while (left < right) {'],
    ['calc_sum',   '                int sum = nums[i] + nums[left] + nums[right];'],
    ['if_zero',    '                if (sum == 0) {'],
    ['add_result', '                    res.add(Arrays.asList(nums[i], nums[left], nums[right]));'],
    ['skip_l_cond','                    while (left < right && nums[left] == nums[left + 1]) {'],
    ['skip_l_inc', '                        left++;'],
    ['',           '                    }'],
    ['skip_r_cond','                    while (left < right && nums[right] == nums[right - 1]) {'],
    ['skip_r_dec', '                        right--;'],
    ['',           '                    }'],
    ['inc_left',   '                    left++;'],
    ['dec_right',  '                    right--;'],
    ['',           '                } else if (sum < 0) {'],
    ['elif_neg',   '                    left++;'],
    ['',           '                } else {'],
    ['else_pos',   '                    right--;'],
    ['',           '                }'],
    ['',           '            }'],
    ['',           '        }'],
    ['return_res', '        return res;'],
    ['',           '    }'],
    ['',           ''],
    ['',           '    public static void main(String[] args) {'],
    ['m_scanner',  '        Scanner sc = new Scanner(System.in);'],
    ['m_read_n',   '        int n = sc.nextInt();'],
    ['m_alloc',    '        int[] nums = new int[n];'],
    ['m_loop',     '        for (int i = 0; i < n; i++) {'],
    ['m_read_elem','            nums[i] = sc.nextInt();'],
    ['',           '        }'],
    ['m_call',     '        List<List<Integer>> result = new Solution().threeSum(nums);'],
    ['m_print',    '        System.out.println(result);'],
    ['',           '    }'],
    ['',           '}']
  ],
  cpp: [
    ['',           '#include <vector>'],
    ['',           '#include <algorithm>'],
    ['',           '#include <iostream>'],
    ['',           'using namespace std;'],
    ['',           ''],
    ['',           'class Solution {'],
    ['',           'public:'],
    ['fn_entry',   '    vector<vector<int>> threeSum(vector<int>& nums) {'],
    ['sort_call',  '        sort(nums.begin(), nums.end());'],
    ['init_res',   '        vector<vector<int>> res;'],
    ['outer_init', '        for (int i = 0; i < (int)nums.size() - 2; i++) {'],
    ['skip_cond',  '            if (i > 0 && nums[i] == nums[i - 1]) {'],
    ['skip_cont',  '                continue;'],
    ['',           '            }'],
    ['init_left',  '            int left = i + 1;'],
    ['init_right', '            int right = (int)nums.size() - 1;'],
    ['while_cond', '            while (left < right) {'],
    ['calc_sum',   '                int sum = nums[i] + nums[left] + nums[right];'],
    ['if_zero',    '                if (sum == 0) {'],
    ['add_result', '                    res.push_back({nums[i], nums[left], nums[right]});'],
    ['skip_l_cond','                    while (left < right && nums[left] == nums[left + 1]) {'],
    ['skip_l_inc', '                        left++;'],
    ['',           '                    }'],
    ['skip_r_cond','                    while (left < right && nums[right] == nums[right - 1]) {'],
    ['skip_r_dec', '                        right--;'],
    ['',           '                    }'],
    ['inc_left',   '                    left++;'],
    ['dec_right',  '                    right--;'],
    ['',           '                } else if (sum < 0) {'],
    ['elif_neg',   '                    left++;'],
    ['',           '                } else {'],
    ['else_pos',   '                    right--;'],
    ['',           '                }'],
    ['',           '            }'],
    ['',           '        }'],
    ['return_res', '        return res;'],
    ['',           '    }'],
    ['',           '};'],
    ['',           ''],
    ['',           'int main() {'],
    ['m_read_n',   '    int n;'],
    ['m_scanner',  '    cin >> n;'],
    ['m_alloc',    '    vector<int> nums(n);'],
    ['m_loop',     '    for (int i = 0; i < n; i++) {'],
    ['m_read_elem','        cin >> nums[i];'],
    ['',           '    }'],
    ['m_call',     '    auto result = Solution().threeSum(nums);'],
    ['m_print',    '    for (auto& t : result) cout << "[" << t[0] << "," << t[1] << "," << t[2] << "] ";'],
    ['',           '    return 0;'],
    ['',           '}']
  ],
  python: [
    ['',           'class Solution:'],
    ['fn_entry',   '    def threeSum(self, nums):'],
    ['sort_call',  '        nums.sort()'],
    ['init_res',   '        res = []'],
    ['outer_init', '        for i in range(len(nums) - 2):'],
    ['skip_cond',  '            if i > 0 and nums[i] == nums[i - 1]:'],
    ['skip_cont',  '                continue'],
    ['init_left',  '            left = i + 1'],
    ['init_right', '            right = len(nums) - 1'],
    ['while_cond', '            while left < right:'],
    ['calc_sum',   '                s = nums[i] + nums[left] + nums[right]'],
    ['if_zero',    '                if s == 0:'],
    ['add_result', '                    res.append([nums[i], nums[left], nums[right]])'],
    ['skip_l_cond','                    while left < right and nums[left] == nums[left + 1]:'],
    ['skip_l_inc', '                        left += 1'],
    ['skip_r_cond','                    while left < right and nums[right] == nums[right - 1]:'],
    ['skip_r_dec', '                        right -= 1'],
    ['inc_left',   '                    left += 1'],
    ['dec_right',  '                    right -= 1'],
    ['',           '                elif s < 0:'],
    ['elif_neg',   '                    left += 1'],
    ['',           '                else:'],
    ['else_pos',   '                    right -= 1'],
    ['return_res', '        return res'],
    ['',           ''],
    ['',           'def main():'],
    ['m_read_elem','    nums = list(map(int, input().split()))'],
    ['m_call',     '    print(Solution().threeSum(nums))'],
    ['',           ''],
    ['',           'if __name__ == "__main__":'],
    ['',           '    main()']
  ],
  javascript: [
    ['',           '/**'],
    ['',           ' * @param {number[]} nums'],
    ['',           ' * @return {number[][]}'],
    ['',           ' */'],
    ['fn_entry',   'var threeSum = function(nums) {'],
    ['sort_call',  '    nums.sort((a, b) => a - b);'],
    ['init_res',   '    const res = [];'],
    ['outer_init', '    for (let i = 0; i < nums.length - 2; i++) {'],
    ['skip_cond',  '        if (i > 0 && nums[i] === nums[i - 1]) {'],
    ['skip_cont',  '            continue;'],
    ['',           '        }'],
    ['init_left',  '        let left = i + 1;'],
    ['init_right', '        let right = nums.length - 1;'],
    ['while_cond', '        while (left < right) {'],
    ['calc_sum',   '            const sum = nums[i] + nums[left] + nums[right];'],
    ['if_zero',    '            if (sum === 0) {'],
    ['add_result', '                res.push([nums[i], nums[left], nums[right]]);'],
    ['skip_l_cond','                while (left < right && nums[left] === nums[left + 1]) {'],
    ['skip_l_inc', '                    left++;'],
    ['',           '                }'],
    ['skip_r_cond','                while (left < right && nums[right] === nums[right - 1]) {'],
    ['skip_r_dec', '                    right--;'],
    ['',           '                }'],
    ['inc_left',   '                left++;'],
    ['dec_right',  '                right--;'],
    ['',           '            } else if (sum < 0) {'],
    ['elif_neg',   '                left++;'],
    ['',           '            } else {'],
    ['else_pos',   '                right--;'],
    ['',           '            }'],
    ['',           '        }'],
    ['',           '    }'],
    ['return_res', '    return res;'],
    ['',           '};'],
    ['',           ''],
    ['',           'function main() {'],
    ['m_read_elem','    const nums = lines[0].split(\' \').map(Number);'],
    ['m_call',     '    console.log(JSON.stringify(threeSum(nums)));'],
    ['',           '}'],
    ['',           'main();']
  ],
  c: [
    ['',           '#include <stdio.h>'],
    ['',           '#include <stdlib.h>'],
    ['',           ''],
    ['',           'int cmp(const void* a, const void* b) {'],
    ['',           '    return *(int*)a - *(int*)b;'],
    ['',           '}'],
    ['',           ''],
    ['fn_entry',   'void threeSum(int* nums, int n) {'],
    ['sort_call',  '    qsort(nums, n, sizeof(int), cmp);'],
    ['outer_init', '    for (int i = 0; i < n - 2; i++) {'],
    ['skip_cond',  '        if (i > 0 && nums[i] == nums[i - 1]) {'],
    ['skip_cont',  '            continue;'],
    ['',           '        }'],
    ['init_left',  '        int left = i + 1;'],
    ['init_right', '        int right = n - 1;'],
    ['while_cond', '        while (left < right) {'],
    ['calc_sum',   '            int sum = nums[i] + nums[left] + nums[right];'],
    ['if_zero',    '            if (sum == 0) {'],
    ['add_result', '                printf("[%d,%d,%d] ", nums[i], nums[left], nums[right]);'],
    ['skip_l_cond','                while (left < right && nums[left] == nums[left + 1]) {'],
    ['skip_l_inc', '                    left++;'],
    ['',           '                }'],
    ['skip_r_cond','                while (left < right && nums[right] == nums[right - 1]) {'],
    ['skip_r_dec', '                    right--;'],
    ['',           '                }'],
    ['inc_left',   '                left++;'],
    ['dec_right',  '                right--;'],
    ['',           '            } else if (sum < 0) {'],
    ['elif_neg',   '                left++;'],
    ['',           '            } else {'],
    ['else_pos',   '                right--;'],
    ['',           '            }'],
    ['',           '        }'],
    ['',           '    }'],
    ['',           '}'],
    ['',           ''],
    ['',           'int main() {'],
    ['m_read_n',   '    int n;'],
    ['m_scanner',  '    scanf("%d", &n);'],
    ['m_alloc',    '    int nums[300];'],
    ['m_loop',     '    for (int i = 0; i < n; i++) {'],
    ['m_read_elem','        scanf("%d", &nums[i]);'],
    ['',           '    }'],
    ['m_call',     '    threeSum(nums, n);'],
    ['',           '    return 0;'],
    ['',           '}']
  ]
};

const PSEUDOCODE = [
  'function threeSum(nums):',
  '    sort(nums)',
  '    res = []',
  '',
  '    for i = 0 to len-3:',
  '        if i > 0 and nums[i] == nums[i-1]:',
  '            continue',
  '',
  '        left  = i + 1',
  '        right = len - 1',
  '',
  '        while left < right:',
  '            sum = nums[i] + nums[left] + nums[right]',
  '            if sum == 0:',
  '                res.append([nums[i], nums[left], nums[right]])',
  '                skip dup lefts and rights',
  '                left += 1; right -= 1',
  '            elif sum < 0:',
  '                left += 1',
  '            else:',
  '                right -= 1',
  '',
  '    return res'
];

function parseInput(raw) {
  return raw.replace(/[^\d,\-\s]/g,'').split(/[,\s]+/).map(x=>parseInt(x.trim(),10)).filter(x=>!isNaN(x)).slice(0,10);
}

function buildSteps(rawInput) {
  const steps = [];
  const original = parseInput(rawInput);
  if (original.length < 3) return steps;
  const NONE = {iReady:false,leftReady:false,rightReady:false,sumReady:false,sorted:false};
  const SORT = {iReady:false,leftReady:false,rightReady:false,sumReady:false,sorted:true};
  const IRES = {iReady:true, leftReady:false,rightReady:false,sumReady:false,sorted:true};
  const LR   = {iReady:true, leftReady:true, rightReady:true, sumReady:false,sorted:true};
  const ALL  = {iReady:true, leftReady:true, rightReady:true, sumReady:true, sorted:true};
  function snap(iIdx,left,right,arrState,sum,code,phase,ist,extra) {
    return {iIdx,left,right,arr:[...arrState],n:arrState.length,sum,code,phase,initState:{...ist},...extra};
  }
  const n = original.length;
  const partialArr = new Array(n).fill('');
  steps.push({...snap(-1,-1,-1,original,null,'m_scanner','input',NONE,{partial:[...partialArr]}),badge:'Scanner sc = new Scanner(System.in); → Initializing input stream.'});
  steps.push({...snap(-1,-1,-1,original,null,'m_read_n','input',NONE,{partial:[...partialArr]}),badge:`int n = sc.nextInt(); → n = ${n}.`});
  steps.push({...snap(-1,-1,-1,original,null,'m_alloc','input',NONE,{partial:[...partialArr]}),badge:`int[] nums = new int[${n}]; → Allocated array.`});
  for (let ii=0;ii<n;ii++) {
    steps.push({...snap(-1,-1,-1,original,null,'m_loop','input',NONE,{partial:[...partialArr],readIdx:ii}),badge:`for i=${ii}; i<${n} → Reading nums[${ii}].`});
    partialArr[ii]=original[ii];
    steps.push({...snap(-1,-1,-1,original,null,'m_read_elem','input',NONE,{partial:[...partialArr],readIdx:ii}),badge:`nums[${ii}] = sc.nextInt(); → nums[${ii}] = ${original[ii]}.`});
  }
  steps.push({...snap(-1,-1,-1,original,null,'m_loop','input',NONE,{partial:[...partialArr]}),badge:`for exits (i=${n}). All read. Calling threeSum().`});
  steps.push({...snap(-1,-1,-1,original,null,'m_call','input',NONE,{partial:[...partialArr]}),badge:'Calling threeSum(nums).'});
  steps.push({...snap(-1,-1,-1,original,null,'fn_entry','init',NONE,{}),badge:'threeSum(int[] nums) → Function entry. Will sort then fix+two-pointer.'});
  const arr = [...original].sort((a,b)=>a-b);
  steps.push({...snap(-1,-1,-1,arr,null,'sort_call','init',SORT,{}),badge:`Arrays.sort(nums); → Sorted: [${arr.join(', ')}].`});
  const results = [];
  steps.push({...snap(-1,-1,-1,arr,null,'init_res','init',SORT,{results:[]}),badge:'res = new ArrayList<>(); → Result list initialized.'});
  for (let i=0;i<=arr.length-3;i++) {
    steps.push({...snap(i,-1,-1,arr,null,'outer_init','outer',IRES,{results:results.map(r=>[...r])}),badge:`for i=${i}: i < ${arr.length-2} → ${i<arr.length-2?'TRUE. nums[i]='+arr[i]+'.':'FALSE, exit.'}`});
    if (i>=arr.length-2) break;
    const isDupI = i>0 && arr[i]===arr[i-1];
    steps.push({...snap(i,-1,-1,arr,null,'skip_cond','outer',IRES,{isDupI,results:results.map(r=>[...r])}),badge:`if (i>0 && nums[i]==nums[i-1]): ${i>0?arr[i]+(isDupI?'==':'!=')+arr[i-1]:'i=0 skip'} → ${isDupI?'TRUE — dup, skip.':'FALSE — unique, proceed.'}`});
    if (isDupI) {
      steps.push({...snap(i,-1,-1,arr,null,'skip_cont','outer',IRES,{results:results.map(r=>[...r])}),badge:`continue; → nums[i]=${arr[i]} is dup. Skip.`});
      continue;
    }
    let left=i+1, right=arr.length-1;
    steps.push({...snap(i,left,-1,arr,null,'init_left','inner',LR,{results:results.map(r=>[...r])}),badge:`left = i+1 = ${left}. nums[left]=${arr[left]}.`});
    steps.push({...snap(i,left,right,arr,null,'init_right','inner',LR,{results:results.map(r=>[...r])}),badge:`right = n-1 = ${right}. nums[right]=${arr[right]}.`});
    while (left<right) {
      steps.push({...snap(i,left,right,arr,null,'while_cond','inner',LR,{condTrue:true,results:results.map(r=>[...r])}),badge:`while(left<right): ${left}<${right} → TRUE.`});
      const sum=arr[i]+arr[left]+arr[right];
      steps.push({...snap(i,left,right,arr,sum,'calc_sum','inner',ALL,{results:results.map(r=>[...r])}),badge:`sum = ${arr[i]}+${arr[left]}+${arr[right]} = ${sum}.`});
      if (sum===0) {
        steps.push({...snap(i,left,right,arr,sum,'if_zero','inner',ALL,{results:results.map(r=>[...r])}),badge:`if(sum==0): ${sum}==0 → TRUE. Triplet found!`});
        results.push([arr[i],arr[left],arr[right]]);
        steps.push({...snap(i,left,right,arr,sum,'add_result','inner',ALL,{results:results.map(r=>[...r]),justFound:true}),badge:`res.add([${arr[i]},${arr[left]},${arr[right]}]); → ${results.length} triplet(s) so far.`});
        let skipL=left<right&&arr[left]===arr[left+1];
        steps.push({...snap(i,left,right,arr,sum,'skip_l_cond','inner',ALL,{results:results.map(r=>[...r]),skipL}),badge:`while(dup left): ${skipL?arr[left]+'=='+arr[left+1]+' TRUE':'FALSE — no dup left.'}`});
        while (left<right&&arr[left]===arr[left+1]) {
          left++;
          steps.push({...snap(i,left,right,arr,sum,'skip_l_inc','inner',ALL,{results:results.map(r=>[...r])}),badge:`left++; → left=${left}.`});
          skipL=left<right&&arr[left]===arr[left+1];
          steps.push({...snap(i,left,right,arr,sum,'skip_l_cond','inner',ALL,{results:results.map(r=>[...r]),skipL}),badge:`while(dup left): ${skipL?arr[left]+'=='+arr[left+1]+' TRUE':'FALSE — exit skip-left.'}`});
        }
        let skipR=left<right&&arr[right]===arr[right-1];
        steps.push({...snap(i,left,right,arr,sum,'skip_r_cond','inner',ALL,{results:results.map(r=>[...r]),skipR}),badge:`while(dup right): ${skipR?arr[right]+'=='+arr[right-1]+' TRUE':'FALSE — no dup right.'}`});
        while (left<right&&arr[right]===arr[right-1]) {
          right--;
          steps.push({...snap(i,left,right,arr,sum,'skip_r_dec','inner',ALL,{results:results.map(r=>[...r])}),badge:`right--; → right=${right}.`});
          skipR=left<right&&arr[right]===arr[right-1];
          steps.push({...snap(i,left,right,arr,sum,'skip_r_cond','inner',ALL,{results:results.map(r=>[...r]),skipR}),badge:`while(dup right): ${skipR?arr[right]+'=='+arr[right-1]+' TRUE':'FALSE — exit skip-right.'}`});
        }
        left++;right--;
        steps.push({...snap(i,left,right,arr,sum,'inc_left','inner',LR,{results:results.map(r=>[...r])}),badge:`left++; → left=${left}.`});
        steps.push({...snap(i,left,right,arr,null,'dec_right','inner',LR,{results:results.map(r=>[...r])}),badge:`right--; → right=${right}.`});
      } else if (sum<0) {
        steps.push({...snap(i,left,right,arr,sum,'if_zero','inner',ALL,{results:results.map(r=>[...r])}),badge:`if(sum==0): ${sum}==0 → FALSE.`});
        steps.push({...snap(i,left,right,arr,sum,'elif_neg','inner',ALL,{results:results.map(r=>[...r])}),badge:`else if(sum<0): ${sum}<0 → TRUE. Sum too small, move left right.`});
        left++;
        steps.push({...snap(i,left,right,arr,null,'elif_neg','inner',LR,{results:results.map(r=>[...r]),moveLeft:true}),badge:`left++; → left=${left}.`});
      } else {
        steps.push({...snap(i,left,right,arr,sum,'if_zero','inner',ALL,{results:results.map(r=>[...r])}),badge:`if(sum==0): ${sum}==0 → FALSE.`});
        steps.push({...snap(i,left,right,arr,sum,'elif_neg','inner',ALL,{results:results.map(r=>[...r])}),badge:`else if(sum<0): ${sum}<0 → FALSE.`});
        steps.push({...snap(i,left,right,arr,sum,'else_pos','inner',ALL,{results:results.map(r=>[...r])}),badge:`else(sum>0): ${sum}>0. Sum too large, move right left.`});
        right--;
        steps.push({...snap(i,left,right,arr,null,'else_pos','inner',LR,{results:results.map(r=>[...r]),moveRight:true}),badge:`right--; → right=${right}.`});
      }
    }
    steps.push({...snap(i,left,right,arr,null,'while_cond','inner',LR,{condTrue:false,results:results.map(r=>[...r])}),badge:`while(left<right): ${left}<${right} → FALSE. Inner loop exits for i=${i}.`});
  }
  steps.push({...snap(arr.length-2,-1,-1,arr,null,'outer_init','done',SORT,{results:results.map(r=>[...r])}),badge:`Outer for exits. ${results.length} triplet(s) found.`});
  steps.push({...snap(-1,-1,-1,arr,null,'return_res','done',SORT,{results:results.map(r=>[...r])}),badge:`return res; → ${results.length} unique triplet(s): ${results.map(t=>'['+t.join(',')+']').join(', ')||'[]'}.`});
  return steps;
}

const inputNums  = ref('-1,0,1,2,-1,-4');
const lang       = ref('java');
const speed      = ref(650);
const si         = ref(0);
const playing    = ref(false);
const vizHeight  = ref(380);
const tableHeight= ref(60);
const leftWidth  = ref(52);
const rightTab   = ref('code');

const stepsData = reactive({steps:buildSteps('-1,0,1,2,-1,-4')});
const steps     = computed(()=>stepsData.steps);
const s         = computed(()=>steps.value[Math.max(0,Math.min(si.value,steps.value.length-1))]||{});
const codeLines = computed(()=>CODES[lang.value]||[]);
let playTimer=null;

function onChipsWheel(e){if(e.currentTarget)e.currentTarget.scrollLeft+=e.deltaY;}
function applyInput(){
  const parsed=parseInput(inputNums.value);
  if(parsed.length<3){alert('Please enter at least 3 integers.');return;}
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

const initState   =computed(()=>s.value.initState||{iReady:false,leftReady:false,rightReady:false,sumReady:false,sorted:false});
const displayArr  =computed(()=>s.value.phase==='input'?(s.value.partial||[]):(s.value.arr||[]));
const displayI    =computed(()=>initState.value.iReady    ?s.value.iIdx :'?');
const displayLeft =computed(()=>initState.value.leftReady ?s.value.left :'?');
const displayRight=computed(()=>initState.value.rightReady?s.value.right:'?');
const displaySum  =computed(()=>initState.value.sumReady  ?s.value.sum  :'?');
const displayResults=computed(()=>s.value.results||[]);

function numClass(idx){
  const phase=s.value.phase,ist=initState.value;
  if(phase==='input')return s.value.readIdx===idx?'ts-num-reading':'';
  if(!ist.sorted)return '';
  const iIdx=s.value.iIdx,left=s.value.left,right=s.value.right;
  if(phase==='done')return 'ts-num-done';
  if(phase==='outer'||phase==='init'){
    if(ist.iReady&&idx===iIdx)return 'ts-num-i';
    if(ist.iReady&&idx<iIdx)return 'ts-num-processed';
    return '';
  }
  if(idx===iIdx)return 'ts-num-i';
  if(idx<iIdx)return 'ts-num-processed';
  if(idx===left&&idx===right)return 'ts-num-lr';
  if(idx===left)return 'ts-num-left';
  if(idx===right)return 'ts-num-right';
  if(idx>iIdx&&idx<left)return 'ts-num-skipped';
  if(idx>right)return 'ts-num-skipped';
  return 'ts-num-window';
}
function showPtr(type,idx){
  const ist=initState.value,phase=s.value.phase;
  if(type==='i')return ist.iReady&&idx===s.value.iIdx;
  if(type==='left')return ist.leftReady&&idx===s.value.left&&(phase==='inner'||phase==='done');
  if(type==='right')return ist.rightReady&&idx===s.value.right&&(phase==='inner'||phase==='done');
  return false;
}

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
  cleanupFns.push(initVResizer(vizResizerRef,vizHeight,240,700));
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
          <div class="ll-toolbar">
            <div class="ll-input-group">
              <label>nums[]:</label>
              <input type="text" v-model="inputNums" class="ll-text-input ts-nums-input" @keyup.enter="applyInput" placeholder="e.g. -1,0,1,2,-1,-4" />
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
            <div class="ll-left-col" ref="leftColRef" :style="{ width: leftWidth + '%' }">
              <div class="ll-viz-wrap" :style="{ height: vizHeight + 'px' }">
                <div class="ll-perm-area">
                  <div class="ll-ptrs ll-ptrs-compact" @wheel.passive="onChipsWheel">
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">n</span><b class="ll-c-blue">{{ s.n || displayArr.length }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">i</span><b class="ll-c-orange">{{ displayI }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">left</span><b class="ll-c-blue">{{ displayLeft }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">right</span><b class="ll-c-green">{{ displayRight }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">sum</span><b :class="displaySum===0?'ll-c-found':(typeof displaySum==='number'&&displaySum<0?'ll-c-blue':'ll-c-green')">{{ displaySum }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">triplets</span><b class="ll-c-found">{{ displayResults.length }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">phase</span><b class="ll-c-orange">{{ s.phase || 'input' }}</b></span>
                  </div>

                  <div class="ll-board-container">
                    <div class="ts-tier-title">
                      Tier 1 &mdash; int[] nums
                      <span v-if="initState.sorted" class="ts-badge ts-badge-sorted">sorted</span>
                      <span class="ts-badge">n = {{ s.n || displayArr.length }}</span>
                    </div>
                    <div class="ts-array-frame">
                      <div class="ts-ptr-row">
                        <div v-for="(val, idx) in displayArr" :key="'ptr-'+idx" class="ts-ptr-cell">
                          <span v-if="showPtr('i',idx)"     class="ts-ptr-label ts-ptr-i">i</span>
                          <span v-if="showPtr('left',idx)"  class="ts-ptr-label ts-ptr-left">L</span>
                          <span v-if="showPtr('right',idx)" class="ts-ptr-label ts-ptr-right">R</span>
                        </div>
                      </div>
                      <div class="ts-num-row">
                        <div v-for="(val, idx) in displayArr" :key="'num-'+idx" class="ts-num-box" :class="numClass(idx)">
                          <span class="ts-num-val">{{ val !== '' ? val : '\u00A0' }}</span>
                          <span class="ts-num-idx">[{{ idx }}]</span>
                        </div>
                      </div>
                    </div>

                    <div class="ts-sum-panel" v-if="initState.sumReady">
                      <span class="ts-sum-label">sum</span>
                      <span class="ts-sum-val" :class="{'ts-sum-zero':s.sum===0,'ts-sum-neg':typeof s.sum==='number'&&s.sum<0,'ts-sum-pos':typeof s.sum==='number'&&s.sum>0}">{{ s.sum }}</span>
                      <span class="ts-sum-expr">= nums[{{ s.iIdx }}]({{ s.arr&&s.arr[s.iIdx] }}) + nums[{{ s.left }}]({{ s.arr&&s.arr[s.left] }}) + nums[{{ s.right }}]({{ s.arr&&s.arr[s.right] }})</span>
                      <span class="ts-sum-verdict" :class="{'ts-verdict-zero':s.sum===0,'ts-verdict-neg':typeof s.sum==='number'&&s.sum<0,'ts-verdict-pos':typeof s.sum==='number'&&s.sum>0}">
                        {{ s.sum===0 ? '&#10003; Found!' : (typeof s.sum==='number'&&s.sum<0 ? '&#9654; left++' : '&#9664; right--') }}
                      </span>
                    </div>

                    <div class="ts-tier-title">
                      Tier 2 &mdash; Result Triplets
                      <span class="ts-badge ts-badge-count">{{ displayResults.length }} found</span>
                    </div>
                    <div class="ts-results-panel">
                      <div v-for="(triplet, ti) in displayResults" :key="'t-'+ti" class="ts-triplet" :class="{'ts-triplet-new':s.justFound&&ti===displayResults.length-1}">
                        <span class="ts-triplet-idx">{{ ti+1 }}</span>
                        <span class="ts-triplet-bracket">[</span>
                        <span class="ts-triplet-val ts-tv-i">{{ triplet[0] }}</span>
                        <span class="ts-triplet-comma">,</span>
                        <span class="ts-triplet-val ts-tv-l">{{ triplet[1] }}</span>
                        <span class="ts-triplet-comma">,</span>
                        <span class="ts-triplet-val ts-tv-r">{{ triplet[2] }}</span>
                        <span class="ts-triplet-bracket">]</span>
                      </div>
                      <div v-if="displayResults.length===0" class="ts-no-results">No triplets found yet.</div>
                    </div>
                  </div>
                </div>
              </div>

              <div class="ll-vresizer" ref="vizResizerRef"></div>

              <div class="ll-legend">
                <span class="ll-leg"><span class="ll-legdot" style="background:#fff7ed;border:1.5px solid #f97316;"></span>i (fixed)</span>
                <span class="ll-leg"><span class="ll-legdot" style="background:#eff6ff;border:1.5px solid #3b82f6;"></span>left</span>
                <span class="ll-leg"><span class="ll-legdot" style="background:#f0fdf4;border:1.5px solid #22c55e;"></span>right</span>
                <span class="ll-leg"><span class="ll-legdot" style="background:#faf5ff;border:1.5px solid #a855f7;"></span>window</span>
                <span class="ll-leg"><span class="ll-legdot ts-legdot-found"></span>sum found</span>
                <span class="ll-leg"><span class="ll-legdot" style="background:#f1f5f9;border:1.5px solid #cbd5e1;opacity:.5"></span>processed</span>
              </div>

              <div class="ll-table-area" :style="{ height: tableHeight + 'px' }">
                <div class="ll-table-title">Call Stack &mdash; Current Execution Frame</div>
                <div class="ll-stack-line">
                  <template v-if="s.phase==='input'">
                    <div class="ll-frame ll-frame-cur">main() &nbsp; <span class="ll-fname">n</span>=<span class="ll-c-blue" style="font-weight:700">{{ s.n||displayArr.length }}</span><span class="ll-now"> &#9668; active</span></div>
                  </template>
                  <template v-else>
                    <div class="ll-frame" style="color:var(--text2);">main()</div>
                    <div class="ll-frame ll-frame-cur" style="margin-left:14px">
                      threeSum(nums) &nbsp;
                      <span class="ll-fname">i</span>=<span class="ll-c-orange" style="font-weight:700">{{ displayI }}</span>,
                      <span class="ll-fname">L</span>=<span class="ll-c-blue" style="font-weight:700">{{ displayLeft }}</span>,
                      <span class="ll-fname">R</span>=<span class="ll-c-green" style="font-weight:700">{{ displayRight }}</span>,
                      <span class="ll-fname">sum</span>=<span class="ll-c-found" style="font-weight:700">{{ displaySum }}</span>
                      <span class="ll-now"> &#9668; active</span>
                    </div>
                  </template>
                </div>
              </div>

              <div class="ll-vresizer" ref="tableResizerRef"></div>

              <div class="ll-badge-wrap">
                <div class="ll-badge" :class="{'ll-badge-error':s.badge&&(s.badge.includes('FALSE')||s.badge.includes('dup')||s.badge.includes('skip')),'ll-badge-success':s.badge&&(s.badge.includes('Found')||s.badge.includes('TRUE')||s.badge.includes('triplet'))}">
                  {{ s.badge || 'Ready to visualize 3Sum.' }}
                </div>
              </div>
            </div>

            <div class="ll-resizer" ref="hResizerRef"></div>

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
                  <h3 class="ll-cx-heading">3Sum (LeetCode 15) &mdash; Complexity Analysis</h3>
                  <p class="ll-cx-intro">
                    Given integer array <code>nums</code>, find all unique triplets summing to 0.
                    Sort first, fix <code>i</code>, then use two-pointer (<code>left</code>, <code>right</code>) convergence.
                    Skip duplicate values at all three pointer levels for unique results.
                  </p>
                  <h4 class="ll-cx-sub">Step-by-step Breakdown</h4>
                  <table class="ll-complexity-table">
                    <thead><tr><th>Operation</th><th>Time</th><th>Space</th><th>Reason</th></tr></thead>
                    <tbody>
                      <tr><td>Sorting</td><td class="ll-cx-mid">O(n log n)</td><td class="ll-cx-good">O(1)</td><td>Required before two-pointer</td></tr>
                      <tr><td>Outer for loop</td><td class="ll-cx-mid">O(n)</td><td class="ll-cx-good">O(1)</td><td>i iterates 0 to n-3</td></tr>
                      <tr><td>Inner while loop</td><td class="ll-cx-mid">O(n)</td><td class="ll-cx-good">O(1)</td><td>left/right converge each outer iteration</td></tr>
                      <tr><td><strong>Total</strong></td><td class="ll-cx-mid"><strong>O(n²)</strong></td><td class="ll-cx-good"><strong>O(1)</strong></td><td>O(n) × O(n)</td></tr>
                    </tbody>
                  </table>
                  <h4 class="ll-cx-sub">Overall Complexity</h4>
                  <div class="ll-cx-summary-grid">
                    <div class="ll-cx-card ll-cx-card-mid"><div class="ll-cx-card-label">Time</div><div class="ll-cx-card-val">O(n²)</div><div class="ll-cx-card-note">After O(n log n) sort</div></div>
                    <div class="ll-cx-card ll-cx-card-good"><div class="ll-cx-card-label">Space</div><div class="ll-cx-card-val">O(1)</div><div class="ll-cx-card-note">No extra structures (excl. output)</div></div>
                    <div class="ll-cx-card ll-cx-card-good"><div class="ll-cx-card-label">Passes</div><div class="ll-cx-card-val">2</div><div class="ll-cx-card-note">Sort + single two-pointer scan</div></div>
                  </div>
                  <div class="ll-note">
                    <strong>Key insight:</strong> Sorting enables the two-pointer technique. If <code>sum &lt; 0</code> → move <code>left</code> right; if <code>sum &gt; 0</code> → move <code>right</code> left. Duplicate skipping at all three pointer levels guarantees unique triplets without a hash set.
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
  --found:#7c3aed;
  --shadow-sm:0 1px 3px rgba(0,0,0,.08),0 1px 2px rgba(0,0,0,.04);
  --radius:8px;--radius-sm:6px;
  background:var(--bg);color:var(--text);
  font-family:'Segoe UI',system-ui,sans-serif;font-size:12.5px;
  display:flex;flex-direction:column;height:74vh;overflow:hidden;width:100%;
}
@keyframes ts-flash-found{0%{background:#fef9c3;transform:scale(1.15);}60%{background:#e9d5ff;}100%{transform:scale(1);}}
@keyframes ts-pop-in{0%{opacity:0;transform:translateY(-6px) scale(.85);}80%{transform:translateY(2px) scale(1.04);}100%{opacity:1;transform:none;}}
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
.ts-nums-input{width:210px;}
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
.ll-c-blue{color:var(--blue);}.ll-c-green{color:var(--green);}.ll-c-orange{color:var(--orange);}.ll-c-found{color:var(--found);font-weight:800;}
.ll-board-container{display:flex;flex-direction:column;align-items:flex-start;padding:6px 14px 10px;gap:8px;min-width:0;width:100%;}
.ts-tier-title{font-size:10px;font-weight:700;text-transform:uppercase;letter-spacing:.04em;color:var(--muted);font-family:'Consolas',monospace;border-left:3px solid var(--coral);padding-left:6px;display:flex;align-items:center;gap:6px;}
.ts-badge{font-size:9px;color:var(--muted);background:var(--surface2);border:1px solid var(--border);border-radius:3px;padding:0 4px;font-weight:500;text-transform:none;letter-spacing:0;}
.ts-badge-sorted{background:#d1fae5;border-color:#6ee7b7;color:#065f46;}
.ts-badge-count{background:var(--purple-light);border-color:#d8b4fe;color:#6d28d9;}
.ts-array-frame{background:#f8fafc;border:1px solid var(--border);border-radius:var(--radius);padding:6px 10px 6px;box-shadow:var(--shadow-sm);display:flex;flex-direction:column;gap:3px;}
.ts-ptr-row{display:flex;gap:4px;min-height:20px;align-items:flex-end;}
.ts-ptr-cell{width:44px;display:flex;justify-content:center;align-items:flex-end;gap:2px;flex-shrink:0;}
.ts-ptr-label{font-size:8.5px;font-weight:800;font-family:monospace;padding:1px 4px;border-radius:3px;line-height:1.2;white-space:nowrap;}
.ts-ptr-i{background:#fff7ed;color:#c2410c;border:1px solid #fdba74;}
.ts-ptr-left{background:#eff6ff;color:#1d4ed8;border:1px solid #93c5fd;}
.ts-ptr-right{background:#f0fdf4;color:#15803d;border:1px solid #86efac;}
.ts-num-row{display:flex;gap:4px;flex-wrap:wrap;}
.ts-num-box{width:44px;height:50px;border-radius:var(--radius-sm);background:#e2e8f0;border:1.5px solid #94a3b8;display:flex;flex-direction:column;align-items:center;justify-content:center;gap:2px;flex-shrink:0;transition:all .2s ease;cursor:default;}
.ts-num-val{font-family:'Cascadia Code','Fira Code',monospace;font-size:14px;font-weight:900;color:var(--text);line-height:1;}
.ts-num-idx{font-family:monospace;font-size:7.5px;color:var(--muted);font-weight:600;}
.ts-num-reading{background:#fef9c3!important;border:2px solid #eab308!important;box-shadow:0 0 6px rgba(234,179,8,.35);}
.ts-num-i{background:#fff7ed!important;border:2px solid #f97316!important;box-shadow:0 0 8px rgba(249,115,22,.35);}
.ts-num-left{background:#bfdbfe!important;border:2px solid var(--blue)!important;box-shadow:0 0 8px rgba(59,130,246,.35);}
.ts-num-right{background:#bbf7d0!important;border:2px solid var(--green)!important;box-shadow:0 0 8px rgba(34,197,94,.35);}
.ts-num-lr{background:#e9d5ff!important;border:2px solid var(--found)!important;box-shadow:0 0 10px rgba(124,58,237,.4);animation:ts-flash-found .4s ease;}
.ts-num-window{background:#faf5ff!important;border:1.5px solid #d8b4fe!important;}
.ts-num-processed{background:#f1f5f9!important;border-color:#e2e8f0!important;opacity:.4;}
.ts-num-skipped{background:#f8fafc!important;border-color:#e2e8f0!important;opacity:.55;}
.ts-num-done{background:#d1fae5!important;border:1.5px solid #6ee7b7!important;}
.ts-sum-panel{display:flex;align-items:center;gap:8px;padding:6px 12px;background:var(--surface2);border:1.5px solid var(--border);border-radius:var(--radius-sm);font-size:11px;font-family:monospace;flex-wrap:wrap;width:100%;box-sizing:border-box;}
.ts-sum-label{font-size:9.5px;font-weight:700;color:var(--muted);}
.ts-sum-val{font-size:18px;font-weight:900;min-width:36px;text-align:center;transition:color .15s;}
.ts-sum-zero{color:#7c3aed;}.ts-sum-neg{color:var(--blue);}.ts-sum-pos{color:var(--green);}
.ts-sum-expr{color:var(--text2);font-size:10px;flex:1;white-space:nowrap;}
.ts-sum-verdict{font-size:10.5px;font-weight:700;padding:2px 7px;border-radius:4px;white-space:nowrap;}
.ts-verdict-zero{background:#ede9fe;color:#7c3aed;border:1px solid #d8b4fe;}
.ts-verdict-neg{background:#eff6ff;color:#1d4ed8;border:1px solid #93c5fd;}
.ts-verdict-pos{background:#f0fdf4;color:#15803d;border:1px solid #86efac;}
.ts-results-panel{display:flex;gap:6px;flex-wrap:wrap;min-height:36px;background:var(--surface2);border:1.5px solid var(--border);border-radius:var(--radius-sm);padding:6px 10px;width:100%;box-sizing:border-box;}
.ts-triplet{display:flex;align-items:center;gap:1px;background:#faf5ff;border:1.5px solid #d8b4fe;border-radius:var(--radius-sm);padding:3px 8px;font-family:monospace;font-size:12px;font-weight:700;transition:all .15s;}
.ts-triplet-new{animation:ts-pop-in .35s ease;border-color:var(--found);background:#ede9fe;}
.ts-triplet-idx{font-size:8px;color:var(--muted);margin-right:3px;}
.ts-triplet-bracket{color:var(--muted);}
.ts-triplet-comma{color:var(--muted);margin:0 1px;}
.ts-triplet-val{font-size:13px;font-weight:900;padding:0 2px;border-radius:2px;}
.ts-tv-i{color:var(--muted);}.ts-tv-l{color:var(--muted);}.ts-tv-r{color:var(--muted);}
.ts-no-results{font-size:10.5px;color:var(--muted);font-style:italic;}
.ts-legdot-found{background:#ede9fe;border:1.5px solid #a855f7;display:inline-block;width:11px;height:11px;border-radius:3px;}
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
.ll-badge-success{border-left-color:#7c3aed!important;background:#ede9fe!important;color:#5b21b6!important;}
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
