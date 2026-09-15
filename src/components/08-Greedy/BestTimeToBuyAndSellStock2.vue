<script setup>
import { ref, reactive, computed, onMounted, onUnmounted, watch } from 'vue';

const props = defineProps({
  topic: { type: String, default: 'Greedy Algorithms' },
  subTopic: { type: String, default: 'Best Time to Buy and Sell Stock II' }
});

/* ==================================================================== */
/* APPROACH METADATA                                                    */
/* Brute Force first, Greedy second                                     */
/* ==================================================================== */
const APPROACHES = [
  { id: 'brute', label: 'Brute Force', desc: 'O(2^n) Exponential — Exhaustive Recursive Buy/Sell Decision Tree' },
  { id: 'greedy', label: 'Greedy', desc: 'O(n) Linear Time & O(1) Space — Peak-Valley Local Ascent Accumulation' }
];

/* ==================================================================== */
/* CODE SNIPPETS (Java, C, C++, Python)                                 */
/* Strict multi-line curly braces on all control statements             */
/* ==================================================================== */
const CODES = {
  brute: {
    java: [
      ['',                  'import java.util.Scanner;'],
      ['',                  ''],
      ['',                  'public class Main {'],
      ['c_entry',           '    static int maxProfitFrom(int day, int canBuy, int[] prices, int n) {'],
      ['c_base_check',      '        if (day >= n) {'],
      ['c_ret_base',        '            return 0;'],
      ['',                  '        }'],
      ['c_check_can_buy',   '        if (canBuy == 1) {'],
      ['c_calc_buy',        '            int buy = -prices[day] + maxProfitFrom(day + 1, 0, prices, n);'],
      ['c_calc_skip',       '            int skip = maxProfitFrom(day + 1, 1, prices, n);'],
      ['c_check_buy_better','            if (buy > skip) {'],
      ['c_ret_buy',         '                return buy;'],
      ['',                  '            } else {'],
      ['c_ret_skip',        '                return skip;'],
      ['',                  '            }'],
      ['',                  '        } else {'],
      ['c_calc_sell',       '            int sell = prices[day] + maxProfitFrom(day + 1, 1, prices, n);'],
      ['c_calc_hold',       '            int hold = maxProfitFrom(day + 1, 0, prices, n);'],
      ['c_check_sell_better','           if (sell > hold) {'],
      ['c_ret_sell',        '                return sell;'],
      ['',                  '            } else {'],
      ['c_ret_hold',        '                return hold;'],
      ['',                  '            }'],
      ['',                  '        }'],
      ['',                  '    }'],
      ['',                  ''],
      ['',                  '    public static void main(String[] args) {'],
      ['m_scanner',         '        Scanner sc = new Scanner(System.in);'],
      ['m_read_n',          '        int n = sc.nextInt();'],
      ['m_alloc_prices',    '        int[] prices = new int[n];'],
      ['m_for_input',       '        for (int i = 0; i < n; i++) {'],
      ['m_read_price',      '            prices[i] = sc.nextInt();'],
      ['',                  '        }'],
      ['m_call_profit',     '        int ans = maxProfitFrom(0, 1, prices, n);'],
      ['m_print_ans',       '        System.out.println(ans);'],
      ['m_done',            '    }'],
      ['',                  '}']
    ],
    c: [
      ['',                  '#include <stdio.h>'],
      ['',                  ''],
      ['c_entry',           'int maxProfitFrom(int day, int canBuy, int prices[], int n) {'],
      ['c_base_check',      '    if (day >= n) {'],
      ['c_ret_base',        '        return 0;'],
      ['',                  '    }'],
      ['c_check_can_buy',   '    if (canBuy == 1) {'],
      ['c_calc_buy',        '        int buy = -prices[day] + maxProfitFrom(day + 1, 0, prices, n);'],
      ['c_calc_skip',       '        int skip = maxProfitFrom(day + 1, 1, prices, n);'],
      ['c_check_buy_better','        if (buy > skip) {'],
      ['c_ret_buy',         '            return buy;'],
      ['',                  '        } else {'],
      ['c_ret_skip',        '            return skip;'],
      ['',                  '        }'],
      ['',                  '    } else {'],
      ['c_calc_sell',       '        int sell = prices[day] + maxProfitFrom(day + 1, 1, prices, n);'],
      ['c_calc_hold',       '        int hold = maxProfitFrom(day + 1, 0, prices, n);'],
      ['c_check_sell_better','       if (sell > hold) {'],
      ['c_ret_sell',        '            return sell;'],
      ['',                  '        } else {'],
      ['c_ret_hold',        '            return hold;'],
      ['',                  '        }'],
      ['',                  '    }'],
      ['',                  '}'],
      ['',                  ''],
      ['',                  'int main() {'],
      ['m_scanner',         '    int n;'],
      ['m_read_n',          '    if (scanf("%d", &n) != 1) {'],
      ['',                  '        return 0;'],
      ['',                  '    }'],
      ['m_alloc_prices',    '    int prices[100];'],
      ['m_for_input',       '    for (int i = 0; i < n; i++) {'],
      ['m_read_price',      '        scanf("%d", &prices[i]);'],
      ['',                  '    }'],
      ['m_call_profit',     '    int ans = maxProfitFrom(0, 1, prices, n);'],
      ['m_print_ans',       '    printf("%d\\n", ans);'],
      ['m_done',            '    return 0;'],
      ['',                  '}']
    ],
    cpp: [
      ['',                  '#include <iostream>'],
      ['',                  '#include <vector>'],
      ['',                  'using namespace std;'],
      ['',                  ''],
      ['c_entry',           'int maxProfitFrom(int day, int canBuy, const vector<int>& prices, int n) {'],
      ['c_base_check',      '    if (day >= n) {'],
      ['c_ret_base',        '        return 0;'],
      ['',                  '    }'],
      ['c_check_can_buy',   '    if (canBuy == 1) {'],
      ['c_calc_buy',        '        int buy = -prices[day] + maxProfitFrom(day + 1, 0, prices, n);'],
      ['c_calc_skip',       '        int skip = maxProfitFrom(day + 1, 1, prices, n);'],
      ['c_check_buy_better','        if (buy > skip) {'],
      ['c_ret_buy',         '            return buy;'],
      ['',                  '        } else {'],
      ['c_ret_skip',        '            return skip;'],
      ['',                  '        }'],
      ['',                  '    } else {'],
      ['c_calc_sell',       '        int sell = prices[day] + maxProfitFrom(day + 1, 1, prices, n);'],
      ['c_calc_hold',       '        int hold = maxProfitFrom(day + 1, 0, prices, n);'],
      ['c_check_sell_better','       if (sell > hold) {'],
      ['c_ret_sell',        '            return sell;'],
      ['',                  '        } else {'],
      ['c_ret_hold',        '            return hold;'],
      ['',                  '        }'],
      ['',                  '    }'],
      ['',                  '}'],
      ['',                  ''],
      ['',                  'int main() {'],
      ['m_scanner',         '    int n;'],
      ['m_read_n',          '    if (!(cin >> n)) {'],
      ['',                  '        return 0;'],
      ['',                  '    }'],
      ['m_alloc_prices',    '    vector<int> prices(n);'],
      ['m_for_input',       '    for (int i = 0; i < n; i++) {'],
      ['m_read_price',      '        cin >> prices[i];'],
      ['',                  '    }'],
      ['m_call_profit',     '    int ans = maxProfitFrom(0, 1, prices, n);'],
      ['m_print_ans',       '    cout << ans << endl;'],
      ['m_done',            '    return 0;'],
      ['',                  '}']
    ],
    python: [
      ['',                  'import sys'],
      ['',                  ''],
      ['c_entry',           'def maxProfitFrom(day, canBuy, prices, n):'],
      ['c_base_check',      '    if day >= n:'],
      ['c_ret_base',        '        return 0'],
      ['c_check_can_buy',   '    if canBuy == 1:'],
      ['c_calc_buy',        '        buy = -prices[day] + maxProfitFrom(day + 1, 0, prices, n)'],
      ['c_calc_skip',       '        skip = maxProfitFrom(day + 1, 1, prices, n)'],
      ['c_check_buy_better','        if buy > skip:'],
      ['c_ret_buy',         '            return buy'],
      ['',                  '        else:'],
      ['c_ret_skip',        '            return skip'],
      ['',                  '    else:'],
      ['c_calc_sell',       '        sell = prices[day] + maxProfitFrom(day + 1, 1, prices, n)'],
      ['c_calc_hold',       '        hold = maxProfitFrom(day + 1, 0, prices, n)'],
      ['c_check_sell_better','        if sell > hold:'],
      ['c_ret_sell',        '            return sell'],
      ['',                  '        else:'],
      ['c_ret_hold',        '            return hold'],
      ['',                  ''],
      ['',                  'def main():'],
      ['m_scanner',         '    input_data = sys.stdin.read().split()'],
      ['m_read_n',          '    if not input_data: return'],
      ['',                  '    n = int(input_data[0])'],
      ['m_alloc_prices',    '    prices = [0] * n'],
      ['m_for_input',       '    for i in range(n):'],
      ['m_read_price',      '        prices[i] = int(input_data[1 + i])'],
      ['m_call_profit',     '    ans = maxProfitFrom(0, 1, prices, n)'],
      ['m_print_ans',       '    print(ans)'],
      ['m_done',            '    return'],
      ['',                  ''],
      ['',                  'if __name__ == "__main__":'],
      ['',                  '    main()']
    ]
  },
  greedy: {
    java: [
      ['',                  'import java.util.Scanner;'],
      ['',                  ''],
      ['',                  'public class Main {'],
      ['c_entry',           '    static int maxProfit(int[] prices, int n) {'],
      ['c_init_profit',     '        int maxProfit = 0;'],
      ['c_for_loop',        '        for (int i = 1; i < n; i++) {'],
      ['c_check_ascent',    '            if (prices[i] > prices[i - 1]) {'],
      ['c_add_profit',      '                maxProfit += prices[i] - prices[i - 1];'],
      ['',                  '            }'],
      ['',                  '        }'],
      ['c_ret_profit',      '        return maxProfit;'],
      ['',                  '    }'],
      ['',                  ''],
      ['',                  '    public static void main(String[] args) {'],
      ['m_scanner',         '        Scanner sc = new Scanner(System.in);'],
      ['m_read_n',          '        int n = sc.nextInt();'],
      ['m_alloc_prices',    '        int[] prices = new int[n];'],
      ['m_for_input',       '        for (int i = 0; i < n; i++) {'],
      ['m_read_price',      '            prices[i] = sc.nextInt();'],
      ['',                  '        }'],
      ['m_call_profit',     '        int ans = maxProfit(prices, n);'],
      ['m_print_ans',       '        System.out.println(ans);'],
      ['m_done',            '    }'],
      ['',                  '}']
    ],
    c: [
      ['',                  '#include <stdio.h>'],
      ['',                  ''],
      ['c_entry',           'int maxProfit(int prices[], int n) {'],
      ['c_init_profit',     '    int maxProfit = 0;'],
      ['c_for_loop',        '    for (int i = 1; i < n; i++) {'],
      ['c_check_ascent',    '        if (prices[i] > prices[i - 1]) {'],
      ['c_add_profit',      '            maxProfit += prices[i] - prices[i - 1];'],
      ['',                  '        }'],
      ['',                  '    }'],
      ['c_ret_profit',      '    return maxProfit;'],
      ['',                  '}'],
      ['',                  ''],
      ['',                  'int main() {'],
      ['m_scanner',         '    int n;'],
      ['m_read_n',          '    if (scanf("%d", &n) != 1) {'],
      ['',                  '        return 0;'],
      ['',                  '    }'],
      ['m_alloc_prices',    '    int prices[100];'],
      ['m_for_input',       '    for (int i = 0; i < n; i++) {'],
      ['m_read_price',      '        scanf("%d", &prices[i]);'],
      ['',                  '    }'],
      ['m_call_profit',     '    int ans = maxProfit(prices, n);'],
      ['m_print_ans',       '    printf("%d\\n", ans);'],
      ['m_done',            '    return 0;'],
      ['',                  '}']
    ],
    cpp: [
      ['',                  '#include <iostream>'],
      ['',                  '#include <vector>'],
      ['',                  'using namespace std;'],
      ['',                  ''],
      ['c_entry',           'int maxProfit(const vector<int>& prices, int n) {'],
      ['c_init_profit',     '    int maxProfit = 0;'],
      ['c_for_loop',        '    for (int i = 1; i < n; i++) {'],
      ['c_check_ascent',    '        if (prices[i] > prices[i - 1]) {'],
      ['c_add_profit',      '            maxProfit += prices[i] - prices[i - 1];'],
      ['',                  '        }'],
      ['',                  '    }'],
      ['c_ret_profit',      '    return maxProfit;'],
      ['',                  '}'],
      ['',                  ''],
      ['',                  'int main() {'],
      ['m_scanner',         '    int n;'],
      ['m_read_n',          '    if (!(cin >> n)) {'],
      ['',                  '        return 0;'],
      ['',                  '    }'],
      ['m_alloc_prices',    '    vector<int> prices(n);'],
      ['m_for_input',       '    for (int i = 0; i < n; i++) {'],
      ['m_read_price',      '        cin >> prices[i];'],
      ['',                  '    }'],
      ['m_call_profit',     '    int ans = maxProfit(prices, n);'],
      ['m_print_ans',       '    cout << ans << endl;'],
      ['m_done',            '    return 0;'],
      ['',                  '}']
    ],
    python: [
      ['',                  'import sys'],
      ['',                  ''],
      ['c_entry',           'def maxProfit(prices, n):'],
      ['c_init_profit',     '    maxProfit = 0'],
      ['c_for_loop',        '    for i in range(1, n):'],
      ['c_check_ascent',    '        if prices[i] > prices[i - 1]:'],
      ['c_add_profit',      '            maxProfit += prices[i] - prices[i - 1]'],
      ['c_ret_profit',      '    return maxProfit'],
      ['',                  ''],
      ['',                  'def main():'],
      ['m_scanner',         '    input_data = sys.stdin.read().split()'],
      ['m_read_n',          '    if not input_data: return'],
      ['',                  '    n = int(input_data[0])'],
      ['m_alloc_prices',    '    prices = [0] * n'],
      ['m_for_input',       '    for i in range(n):'],
      ['m_read_price',      '        prices[i] = int(input_data[1 + i])'],
      ['m_call_profit',     '    ans = maxProfit(prices, n)'],
      ['m_print_ans',       '    print(ans)'],
      ['m_done',            '    return'],
      ['',                  ''],
      ['',                  'if __name__ == "__main__":'],
      ['',                  '    main()']
    ]
  }
};

/* ==================================================================== */
/* PSEUDOCODE FOR STUDENTS                                              */
/* ==================================================================== */
const PSEUDOCODES = {
  brute: [
    'function maxProfitFrom(day, canBuy, prices, n):',
    '    if day >= n:                                // Base case: trading horizon ended',
    '        return 0',
    '    if canBuy == 1:                             // Case A: Holding no shares (can buy or skip)',
    '        buy = -prices[day] + maxProfitFrom(day + 1, 0, prices, n)',
    '        skip = maxProfitFrom(day + 1, 1, prices, n)',
    '        return max(buy, skip)',
    '    else:                                       // Case B: Holding share (can sell or hold)',
    '        sell = prices[day] + maxProfitFrom(day + 1, 1, prices, n)',
    '        hold = maxProfitFrom(day + 1, 0, prices, n)',
    '        return max(sell, hold)',
    '// Time: O(2^n) exponential exhaustive search'
  ],
  greedy: [
    'function maxProfit(prices, n):',
    '    maxProfit = 0                                // Running accumulated profit',
    '    for i = 1 to n - 1:                         // Scan consecutive days',
    '        if prices[i] > prices[i - 1]:           // Price rose from yesterday!',
    '            maxProfit += prices[i] - prices[i - 1] // Harvest local ascent immediately',
    '    return maxProfit                            // Optimal total profit: Time O(n), Space O(1)'
  ]
};

const DEFAULT_PRICES = [7, 1, 5, 3];

function frame(title, rows) {
  return { title, rows };
}

/* ==================================================================== */
/* BUILD STEPS FOR ANIMATION (STRICT 1-TO-1 LINE-BY-LINE)               */
/* ==================================================================== */
function buildSteps(approach, rawPrices) {
  const steps = [];
  const prices = (rawPrices && rawPrices.length) ? [...rawPrices] : [...DEFAULT_PRICES];
  const n = prices.length;

  /* ------------------------------------------------------------------ */
  /* APPROACH 1: BRUTE FORCE (RECURSIVE DECISION TREE)                  */
  /* ------------------------------------------------------------------ */
  if (approach === 'brute') {
    const allNodes = [];
    let nodeIdCounter = 0;

    function buildBruteTree(day, canBuy, parentId, action) {
      const nodeId = nodeIdCounter++;
      const node = {
        id: nodeId,
        day,
        canBuy,
        parentId,
        action,
        children: [],
        isLeaf: day >= n,
        retVal: null,
        state: 'hidden'
      };
      allNodes.push(node);
      if (parentId !== null) {
        const par = allNodes.find(x => x.id === parentId);
        if (par) par.children.push(nodeId);
      }
      if (day >= n) {
        return nodeId;
      }
      if (canBuy === 1) {
        buildBruteTree(day + 1, 0, nodeId, 'buy');
        buildBruteTree(day + 1, 1, nodeId, 'skip');
      } else {
        buildBruteTree(day + 1, 1, nodeId, 'sell');
        buildBruteTree(day + 1, 0, nodeId, 'hold');
      }
      return nodeId;
    }

    buildBruteTree(0, 1, null, 'root');

    // Layout tree coords
    const leafSpacing = 42;
    const levelHeight = 36;
    let leafCounter = 0;

    function assignSubtreeLeaves(nodeId, depth) {
      const nd = allNodes.find(x => x.id === nodeId);
      if (!nd) return;
      nd._depth = depth;
      if (nd.children.length === 0) {
        nd._leafIdx = leafCounter++;
      } else {
        nd.children.forEach(cid => assignSubtreeLeaves(cid, depth + 1));
      }
    }

    assignSubtreeLeaves(0, 0);

    const totalLeaves = Math.max(1, leafCounter);
    const maxDepth = Math.max(0, ...allNodes.map(nd => nd._depth || 0));
    const treeWidth = Math.max(340, totalLeaves * leafSpacing + 40);
    const treeHeight = Math.max(160, (maxDepth + 1) * levelHeight + 32);

    function assignCoords(nodeId) {
      const nd = allNodes.find(x => x.id === nodeId);
      if (!nd) return 0;
      if (nd.children.length === 0) {
        nd._x = 22 + nd._leafIdx * leafSpacing + leafSpacing / 2;
      } else {
        const childXs = nd.children.map(cid => assignCoords(cid));
        nd._x = (childXs[0] + childXs[childXs.length - 1]) / 2;
      }
      nd._y = 20 + nd._depth * levelHeight;
      return nd._x;
    }

    assignCoords(0);

    const nodeStateMap = {};
    allNodes.forEach(nd => {
      nodeStateMap[nd.id] = { ...nd, state: 'hidden', retVal: null };
    });

    function getVisibleNodes() {
      return allNodes
        .filter(nd => nodeStateMap[nd.id].state !== 'hidden')
        .map(nd => ({ ...nd, ...nodeStateMap[nd.id], x: nd._x, y: nd._y }));
    }

    function getVisibleEdges() {
      const edges = [];
      allNodes.forEach(nd => {
        if (nd.parentId !== null && nodeStateMap[nd.id].state !== 'hidden' && nodeStateMap[nd.parentId].state !== 'hidden') {
          const par = allNodes.find(x => x.id === nd.parentId);
          if (par) {
            edges.push({
              x1: par._x,
              y1: par._y + 11,
              x2: nd._x,
              y2: nd._y - 11,
              isTrade: nd.action === 'buy' || nd.action === 'sell',
              action: nd.action
            });
          }
        }
      });
      return edges;
    }

    function curBruteState(extra = {}) {
      return {
        approach: 'brute',
        n,
        prices,
        treeWidth,
        treeHeight,
        treeNodes: getVisibleNodes(),
        treeEdges: getVisibleEdges(),
        ...extra
      };
    }

    // m_scanner
    steps.push({
      code: 'm_scanner',
      badge: 'Faculty Note: Scanner sc = new Scanner(System.in); \u2014 Initializing input stream for daily stock quotes.',
      vars: [frame('main()', [['n', '?']])],
      state: curBruteState({ currentDay: -1, status: 'input', numsFilled: 0 })
    });

    // m_read_n
    steps.push({
      code: 'm_read_n',
      badge: `Faculty Note: int n = sc.nextInt(); &rarr; Read total trading days n = ${n}.`,
      vars: [frame('main()', [['n', String(n)]])],
      state: curBruteState({ currentDay: -1, status: 'input', numsFilled: 0 })
    });

    // m_alloc_prices
    steps.push({
      code: 'm_alloc_prices',
      badge: `Faculty Note: int[] prices = new int[${n}]; &mdash; Allocated daily price array.`,
      vars: [frame('main()', [['n', String(n)]])],
      state: curBruteState({ currentDay: -1, status: 'input', numsFilled: 0 })
    });

    // m_for_input & m_read_price
    for (let idx = 0; idx < n; idx++) {
      steps.push({
        code: 'm_for_input',
        badge: `Reading Day ${idx} stock price (${idx + 1} of ${n}).`,
        vars: [frame('main()', [['i', String(idx)], ['n', String(n)]])],
        state: curBruteState({ currentDay: idx, status: 'input', numsFilled: idx })
      });
      steps.push({
        code: 'm_read_price',
        badge: `prices[${idx}] = sc.nextInt(); \u2192 prices[${idx}] = $${prices[idx]}. Stock price for Day ${idx} is $${prices[idx]}.`,
        vars: [frame('main()', [['i', String(idx)], ['prices[' + idx + ']', '$' + prices[idx]]])],
        state: curBruteState({ currentDay: idx, status: 'input', numsFilled: idx + 1 })
      });
    }

    // m_call_profit
    steps.push({
      code: 'm_call_profit',
      badge: `Calling maxProfitFrom(0, 1, prices, ${n}). Starting recursive exploration at Day 0 with cash available (canBuy=1).`,
      vars: [frame('main()', [['n', String(n)]])],
      state: curBruteState({ currentDay: 0, status: 'call' })
    });

    let callCount = 0;

    function recBrute(day, canBuy, parentId, action, nodeId, callStack) {
      callCount++;
      nodeStateMap[nodeId].state = 'active';

      const thisFrame = frame(`maxProfitFrom(day=${day}, canBuy=${canBuy})`, [
        ['day', String(day)],
        ['canBuy', canBuy === 1 ? '1 (Can Buy)' : '0 (Holding)'],
        ['price', day < n ? '$' + prices[day] : 'N/A']
      ]);
      const cs = [frame('main()', [['n', String(n)]]), ...callStack, thisFrame];

      // c_entry
      steps.push({
        code: 'c_entry',
        badge: `Call #${callCount}: Entering maxProfitFrom(day=${day}, canBuy=${canBuy}). Evaluating trading choices for Day ${day}.`,
        vars: cs,
        state: curBruteState({
          activeNodeId: nodeId,
          currentDay: day,
          canBuy,
          callCounter: callCount,
          status: 'evaluating'
        })
      });

      // c_base_check
      const isBase = day >= n;
      steps.push({
        code: 'c_base_check',
        badge: `Base check: if (day=${day} >= n=${n}) &rarr; ${isBase ? 'TRUE (Trading horizon ended, leaf node)' : 'FALSE (Valid market day)'}`,
        vars: cs,
        state: curBruteState({
          activeNodeId: nodeId,
          currentDay: day,
          canBuy,
          callCounter: callCount,
          status: isBase ? 'leaf' : 'evaluating'
        })
      });

      if (isBase) {
        nodeStateMap[nodeId].retVal = 0;
        nodeStateMap[nodeId].state = 'solved';
        steps.push({
          code: 'c_ret_base',
          badge: 'return 0; No more trading days left. Additional future profit is $0.',
          vars: [frame('main()', [['n', String(n)]]), ...callStack, frame(`maxProfitFrom(day=${day}, canBuy=${canBuy})`, [['return', '0']])],
          state: curBruteState({
            activeNodeId: nodeId,
            currentDay: day,
            canBuy,
            callCounter: callCount,
            profit: 0,
            status: 'solved'
          })
        });
        return 0;
      }

      // c_check_can_buy
      steps.push({
        code: 'c_check_can_buy',
        badge: `Check portfolio status: if (canBuy == 1) &rarr; ${canBuy === 1 ? 'TRUE (Currently hold cash: choose Buy vs Skip)' : 'FALSE (Currently hold stock: choose Sell vs Hold)'}`,
        vars: cs,
        state: curBruteState({
          activeNodeId: nodeId,
          currentDay: day,
          canBuy,
          callCounter: callCount,
          status: 'branching'
        })
      });

      let resVal = 0;

      if (canBuy === 1) {
        // Child 0: Buy
        const buyNodeId = allNodes.find(x => x.parentId === nodeId && x.action === 'buy').id;
        steps.push({
          code: 'c_calc_buy',
          badge: `Option 1 (BUY): Buy share on Day ${day} at $${prices[day]}. Subproblem: maxProfitFrom(${day + 1}, canBuy=0).`,
          vars: cs,
          state: curBruteState({
            activeNodeId: nodeId,
            currentDay: day,
            canBuy,
            callCounter: callCount,
            decision: 'buy',
            status: 'branching'
          })
        });

        const futureFromBuy = recBrute(day + 1, 0, nodeId, 'buy', buyNodeId, [...callStack, thisFrame]);
        const buyProfit = -prices[day] + futureFromBuy;

        // Child 1: Skip
        const skipNodeId = allNodes.find(x => x.parentId === nodeId && x.action === 'skip').id;
        steps.push({
          code: 'c_calc_skip',
          badge: `Option 2 (SKIP): Pass on Day ${day} (keep cash). Subproblem: maxProfitFrom(${day + 1}, canBuy=1).`,
          vars: cs,
          state: curBruteState({
            activeNodeId: nodeId,
            currentDay: day,
            canBuy,
            callCounter: callCount,
            decision: 'skip',
            status: 'branching'
          })
        });

        const futureFromSkip = recBrute(day + 1, 1, nodeId, 'skip', skipNodeId, [...callStack, thisFrame]);
        const skipProfit = futureFromSkip;

        const isBuyBetter = buyProfit > skipProfit;
        steps.push({
          code: 'c_check_buy_better',
          badge: `Compare: if (buy=$${buyProfit} > skip=$${skipProfit}) &rarr; ${isBuyBetter ? 'TRUE (Buying today yields higher net gain!)' : 'FALSE (Skipping today is better or equal)'}`,
          vars: [
            ...cs,
            frame('Decisions', [['buyProfit', '$' + buyProfit], ['skipProfit', '$' + skipProfit]])
          ],
          state: curBruteState({
            activeNodeId: nodeId,
            currentDay: day,
            canBuy,
            callCounter: callCount,
            status: 'evaluating'
          })
        });

        if (isBuyBetter) {
          resVal = buyProfit;
          steps.push({
            code: 'c_ret_buy',
            badge: `return buy=$${buyProfit}; Optimal choice for Day ${day} with cash is to BUY.`,
            vars: [...cs, frame('return', [['result', '$' + buyProfit]])],
            state: curBruteState({
              activeNodeId: nodeId,
              currentDay: day,
              profit: buyProfit,
              status: 'solved'
            })
          });
        } else {
          resVal = skipProfit;
          steps.push({
            code: 'c_ret_skip',
            badge: `return skip=$${skipProfit}; Optimal choice for Day ${day} with cash is to SKIP.`,
            vars: [...cs, frame('return', [['result', '$' + skipProfit]])],
            state: curBruteState({
              activeNodeId: nodeId,
              currentDay: day,
              profit: skipProfit,
              status: 'solved'
            })
          });
        }
      } else {
        // Child 0: Sell
        const sellNodeId = allNodes.find(x => x.parentId === nodeId && x.action === 'sell').id;
        steps.push({
          code: 'c_calc_sell',
          badge: `Option 1 (SELL): Sell share on Day ${day} at $${prices[day]}. Subproblem: maxProfitFrom(${day + 1}, canBuy=1).`,
          vars: cs,
          state: curBruteState({
            activeNodeId: nodeId,
            currentDay: day,
            canBuy,
            callCounter: callCount,
            decision: 'sell',
            status: 'branching'
          })
        });

        const futureFromSell = recBrute(day + 1, 1, nodeId, 'sell', sellNodeId, [...callStack, thisFrame]);
        const sellProfit = prices[day] + futureFromSell;

        // Child 1: Hold
        const holdNodeId = allNodes.find(x => x.parentId === nodeId && x.action === 'hold').id;
        steps.push({
          code: 'c_calc_hold',
          badge: `Option 2 (HOLD): Keep holding share past Day ${day}. Subproblem: maxProfitFrom(${day + 1}, canBuy=0).`,
          vars: cs,
          state: curBruteState({
            activeNodeId: nodeId,
            currentDay: day,
            canBuy,
            callCounter: callCount,
            decision: 'hold',
            status: 'branching'
          })
        });

        const futureFromHold = recBrute(day + 1, 0, nodeId, 'hold', holdNodeId, [...callStack, thisFrame]);
        const holdProfit = futureFromHold;

        const isSellBetter = sellProfit > holdProfit;
        steps.push({
          code: 'c_check_sell_better',
          badge: `Compare: if (sell=$${sellProfit} > hold=$${holdProfit}) &rarr; ${isSellBetter ? 'TRUE (Selling today yields higher net gain!)' : 'FALSE (Holding today is better or equal)'}`,
          vars: [
            ...cs,
            frame('Decisions', [['sellProfit', '$' + sellProfit], ['holdProfit', '$' + holdProfit]])
          ],
          state: curBruteState({
            activeNodeId: nodeId,
            currentDay: day,
            canBuy,
            callCounter: callCount,
            status: 'evaluating'
          })
        });

        if (isSellBetter) {
          resVal = sellProfit;
          steps.push({
            code: 'c_ret_sell',
            badge: `return sell=$${sellProfit}; Optimal choice for Day ${day} while holding is to SELL.`,
            vars: [...cs, frame('return', [['result', '$' + sellProfit]])],
            state: curBruteState({
              activeNodeId: nodeId,
              currentDay: day,
              profit: sellProfit,
              status: 'solved'
            })
          });
        } else {
          resVal = holdProfit;
          steps.push({
            code: 'c_ret_hold',
            badge: `return hold=$${holdProfit}; Optimal choice for Day ${day} while holding is to HOLD.`,
            vars: [...cs, frame('return', [['result', '$' + holdProfit]])],
            state: curBruteState({
              activeNodeId: nodeId,
              currentDay: day,
              profit: holdProfit,
              status: 'solved'
            })
          });
        }
      }

      nodeStateMap[nodeId].retVal = resVal;
      nodeStateMap[nodeId].state = 'solved';
      return resVal;
    }

    const finalAns = recBrute(0, 1, null, 'root', 0, []);

    // m_print_ans
    steps.push({
      code: 'm_print_ans',
      badge: `Output Result: System.out.println(${finalAns}); &mdash; Exhaustive recursive exploration found max profit = $${finalAns}!`,
      vars: [frame('main()', [['ans', '$' + finalAns]])],
      state: curBruteState({ currentDay: -1, profit: finalAns, status: 'finished' })
    });

    // m_done
    steps.push({
      code: 'm_done',
      badge: `Program execution finished. Optimal Maximum Profit = $${finalAns}.`,
      vars: [frame('main()', [['status', 'finished']])],
      state: curBruteState({ currentDay: -1, profit: finalAns, status: 'finished' })
    });

    return steps;
  }

  /* ------------------------------------------------------------------ */
  /* APPROACH 2: GREEDY (PEAK-VALLEY LOCAL ASCENT ACCUMULATION)         */
  /* ------------------------------------------------------------------ */
  else {
    function curGreedyState(extra = {}) {
      return {
        approach: 'greedy',
        n,
        prices,
        ...extra
      };
    }

    // Step 1: m_scanner
    steps.push({
      code: 'm_scanner',
      badge: 'Faculty Note: Scanner sc = new Scanner(System.in); \u2014 Initializing standard input stream.',
      vars: [frame('main()', [['n', '?']])],
      state: curGreedyState({ i: -1, maxProfit: 0, status: 'input', numsFilled: 0 })
    });

    // Step 2: m_read_n
    steps.push({
      code: 'm_read_n',
      badge: `Faculty Note: int n = sc.nextInt(); &rarr; Read trading days count n = ${n}.`,
      vars: [frame('main()', [['n', String(n)]])],
      state: curGreedyState({ i: -1, maxProfit: 0, status: 'input', numsFilled: 0 })
    });

    // Step 3: m_alloc_prices
    steps.push({
      code: 'm_alloc_prices',
      badge: `Faculty Note: int[] prices = new int[${n}]; &mdash; Allocated daily price array.`,
      vars: [frame('main()', [['n', String(n)]])],
      state: curGreedyState({ i: -1, maxProfit: 0, status: 'input', numsFilled: 0 })
    });

    // Step 4: m_for_input & m_read_price
    for (let idx = 0; idx < n; idx++) {
      steps.push({
        code: 'm_for_input',
        badge: `Reading Day ${idx} stock price (${idx + 1} of ${n}).`,
        vars: [frame('main()', [['i', String(idx)], ['n', String(n)]])],
        state: curGreedyState({ i: -1, maxProfit: 0, status: 'input', numsFilled: idx })
      });
      steps.push({
        code: 'm_read_price',
        badge: `prices[${idx}] = sc.nextInt(); \u2192 prices[${idx}] = $${prices[idx]}. Stock price for Day ${idx} recorded as $${prices[idx]}.`,
        vars: [frame('main()', [['i', String(idx)], ['prices[' + idx + ']', '$' + prices[idx]]])],
        state: curGreedyState({ i: -1, maxProfit: 0, status: 'input', numsFilled: idx + 1 })
      });
    }

    // Step 5: m_call_profit
    steps.push({
      code: 'm_call_profit',
      badge: `Calling maxProfit(prices, n=${n}). Optimal Greedy peak-valley algorithm begins!`,
      vars: [frame('main()', [['n', String(n)]])],
      state: curGreedyState({ i: -1, maxProfit: 0, status: 'call' })
    });

    // Step 6: c_entry
    steps.push({
      code: 'c_entry',
      badge: 'Entering maxProfit(prices, n). Greedy insight: Sum every positive daily ascent (prices[i] - prices[i-1] > 0).',
      vars: [frame('main()', [['n', String(n)]]), frame('maxProfit()', [['n', String(n)]])],
      state: curGreedyState({ i: -1, maxProfit: 0, status: 'entry' })
    });

    // Step 7: c_init_profit
    let maxProfit = 0;
    steps.push({
      code: 'c_init_profit',
      badge: 'int maxProfit = 0; Initialize total profit accumulator to $0.',
      vars: [frame('main()', [['n', String(n)]]), frame('maxProfit()', [['maxProfit', '$0']])],
      state: curGreedyState({ i: 0, maxProfit: 0, delta: 0, status: 'running' })
    });

    // Loop through days 1 to n - 1
    for (let i = 1; i < n; i++) {
      const prevPrice = prices[i - 1];
      const curPrice = prices[i];
      const delta = curPrice - prevPrice;
      const isAscent = delta > 0;

      // c_for_loop
      steps.push({
        code: 'c_for_loop',
        badge: `Scanning consecutive trading days: Day ${i - 1} ($${prevPrice}) &rarr; Day ${i} ($${curPrice}).`,
        vars: [
          frame('main()', [['n', String(n)]]),
          frame('maxProfit()', [
            ['i', String(i)],
            ['prices[i-1]', '$' + prevPrice],
            ['prices[i]', '$' + curPrice],
            ['maxProfit', '$' + maxProfit]
          ])
        ],
        state: curGreedyState({
          i,
          prevDay: i - 1,
          delta,
          maxProfit,
          arcFrom: i - 1,
          arcTo: i,
          status: 'scanning'
        })
      });

      // c_check_ascent
      steps.push({
        code: 'c_check_ascent',
        badge: `Evaluate ascent: if (prices[${i}]=$${curPrice} > prices[${i-1}]=$${prevPrice}) &rarr; ${isAscent ? 'TRUE (Price rose! Daily ascent delta = +$' + delta + ')' : 'FALSE (Price dropped or flat, delta = $' + delta + ' &rarr; No trade)'}`,
        vars: [
          frame('main()', [['n', String(n)]]),
          frame('maxProfit()', [
            ['i', String(i)],
            ['delta', (delta > 0 ? '+' : '') + '$' + delta],
            ['isAscent', isAscent ? 'true' : 'false'],
            ['maxProfit', '$' + maxProfit]
          ])
        ],
        state: curGreedyState({
          i,
          prevDay: i - 1,
          delta,
          maxProfit,
          arcFrom: i - 1,
          arcTo: i,
          status: isAscent ? 'ascent' : 'dip'
        })
      });

      if (isAscent) {
        maxProfit += delta;
        // c_add_profit
        steps.push({
          code: 'c_add_profit',
          badge: `maxProfit += prices[${i}] - prices[${i-1}]; &rarr; Buy at Day ${i-1} ($${prevPrice}) and sell at Day ${i} ($${curPrice})! Harvested +$${delta}. Total profit now = $${maxProfit}.`,
          vars: [
            frame('main()', [['n', String(n)]]),
            frame('maxProfit()', [
              ['harvested', '+$' + delta],
              ['newProfit', '$' + maxProfit]
            ])
          ],
          state: curGreedyState({
            i,
            prevDay: i - 1,
            delta,
            maxProfit,
            arcFrom: i - 1,
            arcTo: i,
            harvested: true,
            status: 'profit'
          })
        });
      }
    }

    // c_ret_profit
    steps.push({
      code: 'c_ret_profit',
      badge: `return maxProfit=$${maxProfit}; All daily opportunities evaluated. Optimal max profit returned.`,
      vars: [frame('main()', [['n', String(n)]]), frame('maxProfit()', [['return', '$' + maxProfit]])],
      state: curGreedyState({ i: n - 1, maxProfit, status: 'success' })
    });

    // m_print_ans
    steps.push({
      code: 'm_print_ans',
      badge: `Output Result: System.out.println(${maxProfit}); &mdash; Solved greedily in O(n) linear time & O(1) space!`,
      vars: [frame('main()', [['ans', '$' + maxProfit]])],
      state: curGreedyState({ i: -1, maxProfit, status: 'success' })
    });

    // m_done
    steps.push({
      code: 'm_done',
      badge: `Program execution finished. Maximum Achievable Profit = $${maxProfit}.`,
      vars: [frame('main()', [['status', 'finished']])],
      state: curGreedyState({ i: -1, maxProfit, status: 'success' })
    });

    return steps;
  }
}

/* ==================================================================== */
/* COMPONENT REACTIVE STATE                                             */
/* ==================================================================== */
const currentApproach = ref('brute');
const currentPrices = ref([...DEFAULT_PRICES]);
const inputPricesText = ref(DEFAULT_PRICES.join(', '));
const lang = ref('java');
const rightTab = ref('code');
const speed = ref(1000);
const playing = ref(false);
let playTimer = null;

const stepsData = reactive({ steps: [] });
const si = ref(0);

const steps = computed(() => stepsData.steps);
const s = computed(() => steps.value[si.value] || {});
const st = computed(() => s.value.state || {});

const codeLines = computed(() => {
  const app = currentApproach.value;
  const l = lang.value;
  return (CODES[app] && CODES[app][l]) || [];
});

const pseudocodeLines = computed(() => {
  return PSEUDOCODES[currentApproach.value] || [];
});

// Panel dimensions & resizers
const leftWidth = ref(52);
const vizHeight = ref(230);
const tableHeight = ref(85);

function applyApproach(appId) {
  if (currentApproach.value === appId) return;
  playing.value = false;
  currentApproach.value = appId;
  stepsData.steps = buildSteps(appId, currentPrices.value);
  si.value = 0;
}

function parseInput(text) {
  const parts = text.split(/[,\s]+/).map(x => parseInt(x.trim(), 10)).filter(x => !isNaN(x) && x >= 0);
  return parts.length >= 2 ? parts : [...DEFAULT_PRICES];
}

function applyInput() {
  playing.value = false;
  const parsed = parseInput(inputPricesText.value);
  currentPrices.value = parsed;
  inputPricesText.value = parsed.join(', ');
  stepsData.steps = buildSteps(currentApproach.value, parsed);
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

/* ==================================================================== */
/* PANEL RESIZERS                                                       */
/* ==================================================================== */
const mainRef = ref(null);
const leftColRef = ref(null);
const hResizerRef = ref(null);
const vizResizerRef = ref(null);
const tableResizerRef = ref(null);

function initHResizer() {
  const rsz = hResizerRef.value, main = mainRef.value;
  if (!rsz || !main) return;
  let dragging = false, startX = 0, startW = 0;
  const onDown = e => {
    dragging = true; startX = e.clientX; startW = leftColRef.value.offsetWidth;
    rsz.classList.add('drag'); document.body.style.userSelect = 'none';
  };
  const onMove = e => {
    if (!dragging) return;
    const mainW = main.offsetWidth;
    leftWidth.value = (Math.max(220, Math.min(mainW - 220, startW + e.clientX - startX)) / mainW) * 100;
  };
  const onUp = () => {
    if (!dragging) return;
    dragging = false; rsz.classList.remove('drag'); document.body.style.userSelect = '';
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
  if (!rsz) return;
  let dragging = false, startY = 0, startH = 0;
  const onDown = e => {
    dragging = true; startY = e.clientY; startH = valueRef.value;
    rsz.classList.add('drag'); document.body.style.userSelect = 'none'; e.preventDefault();
  };
  const onMove = e => {
    if (!dragging) return;
    valueRef.value = Math.max(minH, Math.min(maxH, startH + (e.clientY - startY)));
  };
  const onUp = () => {
    if (!dragging) return;
    dragging = false; rsz.classList.remove('drag'); document.body.style.userSelect = '';
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
  stepsData.steps = buildSteps(currentApproach.value, currentPrices.value);
  document.addEventListener('keydown', onKeydown);
  cleanupFns.push(initHResizer());
  cleanupFns.push(initVResizer(vizResizerRef, vizHeight, 140, 420));
  cleanupFns.push(initVResizer(tableResizerRef, tableHeight, 30, 140));
});

onUnmounted(() => {
  document.removeEventListener('keydown', onKeydown);
  clearTimeout(playTimer);
  cleanupFns.forEach(fn => fn && fn());
});
</script>

<template>
  <div class="slide-wrapper">
    <!-- Top Navbar -->
    <div class="navbar">
      <h2 class="navbar-title">{{ topic }} &mdash; {{ subTopic }}</h2>
      <img src="../../assets/logo.png" alt="Logo" />
    </div>

    <div class="slide-body">
      <div class="row-main">
        <div class="ll-root">
          <!-- Control Panel Toolbar -->
          <div class="ll-toolbar">
            <!-- Approach Selector Button Group (Brute Force first, Greedy second) -->
            <div class="ll-approach-group">
              <button
                v-for="app in APPROACHES"
                :key="app.id"
                class="ll-approach-btn"
                :class="{ active: currentApproach === app.id }"
                :title="app.desc"
                @click="applyApproach(app.id)"
              >
                {{ app.label }}
              </button>
            </div>

            <!-- Input Array prices -->
            <div class="ll-input-group">
              <label>prices =</label>
              <input
                type="text"
                v-model="inputPricesText"
                class="ll-text-input"
                placeholder="7, 1, 5, 3, 6, 4"
                @keyup.enter="applyInput"
                style="width: 140px;"
              />
            </div>

            <button class="ll-viz-btn" @click="applyInput">&#9654; Visualize</button>

            <!-- Playback Navigation Controls -->
            <div class="ll-nav-controls">
              <button class="ll-nav-btn" title="First step" @click="stepBy(-steps.length)">&#171;</button>
              <button class="ll-nav-btn" title="Previous step" @click="stepBy(-1)">&#8249; Prev</button>
              <button class="ll-play-btn" @click="togglePlay">
                {{ playing ? '&#9208; Pause' : '&#9654; Play' }}
              </button>
              <button class="ll-nav-btn" title="Next step" @click="stepBy(1)">Next &#8250;</button>
              <button class="ll-nav-btn" title="Last step" @click="stepBy(steps.length)">&#187;</button>
            </div>
          </div>

          <div class="ll-main" ref="mainRef">
            <!-- Left Visualization Column -->
            <div class="ll-left-col" ref="leftColRef" :style="{ width: leftWidth + '%' }">
              <!-- Top Banner: Array Display with Day Cards -->
              <div class="ll-house-banner">
                <div class="ll-house-title">
                  Daily Stock Quotes: <code>prices[0 ... {{ currentPrices.length - 1 }}]</code>
                </div>

                <div class="ll-dual-arrays-wrap">
                  <div class="ll-arr-row">
                    <span class="ll-arr-tag">prices[]:</span>
                    <div class="ll-arr-cells">
                      <div
                        v-for="(val, idx) in currentPrices"
                        :key="idx"
                        class="ll-arr-cell"
                        :class="{
                          'll-cell-active': st.i === idx || st.currentDay === idx,
                          'll-cell-prev': st.prevDay === idx,
                          'll-cell-profit': (st.i === idx || st.currentDay === idx) && st.delta > 0
                        }"
                      >
                        <span class="ll-cell-idx">Day {{ idx }}</span>
                        <span class="ll-cell-val">{{ (st.numsFilled !== undefined && idx >= st.numsFilled) ? '?' : ('$' + val) }}</span>
                      </div>
                    </div>
                  </div>
                </div>
              </div>

              <!-- Real-time Stats Chips Bar -->
              <div class="ll-ptrs">
                <div class="ll-ptr-chip">Trading Days: <b class="ll-c-blue">{{ st.n || currentPrices.length }}</b></div>
                <template v-if="currentApproach === 'greedy'">
                  <div class="ll-ptr-chip" v-if="st.i >= 1">
                    Comparing: <b class="ll-c-orange">Day {{ st.prevDay }} (${{ currentPrices[st.prevDay] }}) &rarr; Day {{ st.i }} (${{ currentPrices[st.i] }})</b>
                  </div>
                  <div class="ll-ptr-chip" v-if="st.delta !== undefined">
                    Daily Delta: <b :class="st.delta > 0 ? 'll-c-green' : (st.delta < 0 ? 'll-c-red' : 'll-c-muted')">
                      {{ (st.delta > 0 ? '+' : '') + '$' + st.delta }}
                    </b>
                  </div>
                  <div class="ll-ptr-chip">
                    Total Profit: <b class="ll-c-green">${{ st.maxProfit || 0 }}</b>
                  </div>
                  <div class="ll-ptr-chip" :class="{ 'll-chip-success': st.delta > 0 || st.status === 'success' }">
                    Status: <b :class="{ 'll-c-green': st.delta > 0 || st.status === 'success', 'll-c-red': st.delta < 0, 'll-c-blue': st.delta === 0 || st.status === 'running' }">
                      {{ st.status === 'profit' ? 'Ascent Harvested (+$' + st.delta + ')' : (st.status === 'ascent' ? 'Profitable Ascent Detected' : (st.status === 'dip' ? 'Price Dip (Skip / Hold Cash)' : (st.status === 'success' ? 'Optimal Search Complete' : 'Linear Day-by-Day Scan'))) }}
                    </b>
                  </div>
                </template>
                <template v-else>
                  <div class="ll-ptr-chip">Recursive Calls: <b class="ll-c-orange">{{ st.callCounter || 0 }}</b></div>
                  <div class="ll-ptr-chip" v-if="st.currentDay >= 0">
                    Day: <b class="ll-c-blue">Day {{ st.currentDay }} (${{ currentPrices[st.currentDay] }})</b>
                  </div>
                  <div class="ll-ptr-chip">
                    Portfolio State: <b :class="st.canBuy === 1 ? 'll-c-green' : 'll-c-purple'">{{ st.canBuy === 1 ? 'Cash Available (Can Buy)' : 'Holding Share (Can Sell)' }}</b>
                  </div>
                  <div class="ll-ptr-chip" v-if="st.profit !== undefined">
                    Subtree Profit: <b class="ll-c-green">${{ st.profit }}</b>
                  </div>
                </template>
              </div>

              <!-- Visualization Viewport Container -->
              <div class="ll-viz-wrap" :style="{ height: vizHeight + 'px' }">
                <!-- VIEW 1: GREEDY ASCENT TRACK & ARC ANIMATION -->
                <div v-if="currentApproach === 'greedy'" class="ll-timeline-container">
                  <div class="ll-section-caption">
                    <span>Greedy Local Ascent &amp; Peak-Valley Profit Track</span>
                    <span v-if="st.harvested" class="ll-calc-pill">&#10003; Trade Harvested: +${{ st.delta }}</span>
                    <span v-else-if="st.status === 'dip'" class="ll-calc-pill ll-pill-error">No Trade: Dip (${{ st.delta }})</span>
                    <span v-else-if="st.status === 'success'" class="ll-calc-pill">&#10003; Max Profit: ${{ st.maxProfit }}</span>
                    <span v-else class="ll-calc-pill">Scan: i = 1 to {{ currentPrices.length - 1 }}</span>
                  </div>

                  <div class="ll-jump-track-scroll">
                    <div class="ll-track-board" :style="{ width: Math.max(340, currentPrices.length * 68) + 'px' }">
                      <!-- 1. Arc Layer (Curved path showing daily ascent from i - 1 to i) -->
                      <div class="ll-arc-svg-wrap">
                        <svg class="ll-arc-svg" :style="{ width: Math.max(340, currentPrices.length * 68) + 'px', height: '42px' }">
                          <path
                            v-if="st.arcFrom !== undefined && st.arcTo !== undefined && st.arcTo > st.arcFrom"
                            :d="`M ${26 + st.arcFrom * 68} 38 Q ${26 + ((st.arcFrom + st.arcTo) / 2) * 68} ${st.delta > 0 ? 8 : 32} ${26 + st.arcTo * 68} 38`"
                            fill="none"
                            :stroke="st.delta > 0 ? '#10b981' : '#94a3b8'"
                            :stroke-width="st.delta > 0 ? 2.5 : 1.5"
                            :stroke-dasharray="st.delta > 0 ? 'none' : '4 2'"
                            class="ll-arc-jump"
                          />
                          <text
                            v-if="st.arcFrom !== undefined && st.arcTo !== undefined && st.arcTo > st.arcFrom"
                            :x="26 + ((st.arcFrom + st.arcTo) / 2) * 68"
                            :y="st.delta > 0 ? 12 : 26"
                            text-anchor="middle"
                            class="ll-arc-txt"
                            :style="{ fill: st.delta > 0 ? '#047857' : '#64748b' }"
                          >
                            {{ st.delta > 0 ? '+$' + st.delta + ' (Buy Day ' + st.arcFrom + ' &rarr; Sell Day ' + st.arcTo + ')' : 'Dip: $' + st.delta + ' (No Trade)' }}
                          </text>
                        </svg>
                      </div>

                      <!-- 2. Daily Price Pods Row -->
                      <div class="ll-stones-track" :style="{ width: Math.max(340, currentPrices.length * 68) + 'px' }">
                        <div
                          v-for="(val, idx) in currentPrices"
                          :key="'st-' + idx"
                          class="ll-stone-pod"
                          :class="{
                            'll-pod-active': st.i === idx,
                            'll-pod-reach': st.prevDay === idx,
                            'll-pod-profit': (st.i === idx || st.prevDay === idx) && st.delta > 0,
                            'll-pod-stranded': st.i === idx && st.delta < 0
                          }"
                        >
                          <!-- Top Marker (BUY / SELL / CURRENT) -->
                          <div class="ll-pod-pointer">
                            <span v-if="st.prevDay === idx && st.delta > 0" class="ll-pin ll-pin-buy">
                              BUY
                            </span>
                            <span v-else-if="st.i === idx && st.delta > 0" class="ll-pin ll-pin-sell">
                              SELL
                            </span>
                            <span v-else-if="st.i === idx" class="ll-pin ll-pin-active">
                              DAY {{ idx }}
                            </span>
                            <span v-else class="ll-pin ll-pin-space"></span>
                          </div>

                          <!-- Stone Card -->
                          <div class="ll-stone-card">
                            <div class="ll-stone-card-idx">Day [{{ idx }}]</div>
                            <div class="ll-stone-card-val">${{ val }}</div>
                            <div class="ll-stone-card-sub">
                              {{ idx === 0 ? 'base' : (currentPrices[idx] > currentPrices[idx - 1] ? '+$' + (currentPrices[idx] - currentPrices[idx - 1]) : '-$' + (currentPrices[idx - 1] - currentPrices[idx])) }}
                            </div>
                          </div>

                          <!-- Bottom Delta Pill -->
                          <div class="ll-pod-barrier">
                            <div
                              v-if="st.i === idx && st.delta !== undefined"
                              class="ll-barrier-tag"
                            >
                              <div class="ll-barrier-arrow" :class="{ 'll-arr-up': st.delta > 0, 'll-arr-down': st.delta <= 0 }">
                                {{ st.delta > 0 ? '&#9650;' : '&#9660;' }}
                              </div>
                              <div class="ll-barrier-lbl" :class="{ 'll-lbl-gain': st.delta > 0, 'll-lbl-dip': st.delta <= 0 }">
                                {{ (st.delta > 0 ? '+' : '') + '$' + st.delta }}
                              </div>
                            </div>
                          </div>
                        </div>
                      </div>

                      <!-- 3. Horizon Baseline Line below stones -->
                      <div class="ll-horizon-line-wrap" :style="{ width: Math.max(340, currentPrices.length * 68) + 'px' }">
                        <div class="ll-horizon-track">
                          <div
                            class="ll-horizon-active"
                            :style="{
                              width: st.i !== undefined && st.i >= 0
                                ? Math.min(100, (((st.i) + 0.5) / currentPrices.length) * 100) + '%'
                                : '0%'
                            }"
                          ></div>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>

                <!-- VIEW 2: BRUTE FORCE DECISION TREE -->
                <div v-else class="ll-tree-container">
                  <div class="ll-tree-scroll-area">
                    <svg
                      :viewBox="`0 0 ${st.treeWidth || 480} ${st.treeHeight || 240}`"
                      :style="{
                        width: '100%',
                        maxWidth: (st.treeWidth || 480) + 'px',
                        height: (st.treeHeight || 240) + 'px',
                        maxHeight: (st.treeHeight || 240) + 'px'
                      }"
                      class="ll-tree-svg"
                    >
                      <!-- Dynamic Edges -->
                      <g class="ll-tree-edges">
                        <line
                          v-for="(edge, idx) in st.treeEdges"
                          :key="idx"
                          :x1="edge.x1"
                          :y1="edge.y1"
                          :x2="edge.x2"
                          :y2="edge.y2"
                          class="ll-tree-edge"
                          :class="{ 'll-edge-pick': edge.isTrade, 'll-edge-skip': !edge.isTrade }"
                        />
                      </g>

                      <!-- Dynamic Nodes -->
                      <g class="ll-tree-nodes">
                        <g
                          v-for="node in st.treeNodes"
                          :key="node.id"
                          :transform="`translate(${node.x}, ${node.y})`"
                          class="ll-tree-node-group"
                        >
                          <rect
                            x="-20"
                            y="-11"
                            width="40"
                            height="22"
                            rx="4"
                            class="ll-node-rect"
                            :class="{
                              'll-node-active': node.id === st.activeNodeId,
                              'll-node-solved': node.state === 'solved',
                              'll-node-leaf': node.isLeaf
                            }"
                          />
                          <text x="0" y="-2.5" text-anchor="middle" class="ll-node-text-call">
                            d={{ node.day }} {{ node.canBuy ? 'Buy?' : 'Sell?' }}
                          </text>
                          <text
                            x="0"
                            y="6.5"
                            text-anchor="middle"
                            class="ll-node-text-val"
                            :class="{
                              'll-val-active': node.id === st.activeNodeId,
                              'll-val-solved': node.state === 'solved'
                            }"
                          >
                            {{ node.isLeaf ? '$0' : (node.retVal !== null ? '$' + node.retVal : (node.action === 'root' ? 'root' : node.action)) }}
                          </text>
                        </g>
                      </g>
                    </svg>
                  </div>
                </div>
              </div>

              <!-- Vertical Resizer for Viz Panel -->
              <div class="ll-vresizer" ref="vizResizerRef"></div>

              <!-- Dedicated Legend Strip (Exact match with ActivitySelection / JumpGame design) -->
              <div class="ll-legend">
                <template v-if="currentApproach === 'greedy'">
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-cur"></span>Active Day i</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-new"></span>Buy Day (i - 1)</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-purple"></span>Sell Day (i)</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-red"></span>Price Dip (No Trade)</span>
                </template>
                <template v-else>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-cur"></span>Active Call</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-new"></span>Subtree Solved</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-purple"></span>Leaf (day &ge; n)</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-red"></span>Trade Branch</span>
                </template>
              </div>

              <!-- Call Stack & Variable Environment Panel -->
              <div class="ll-table-area" :style="{ height: tableHeight + 'px' }">
                <div class="ll-table-title">Call Stack &amp; Environment Variables:</div>
                <div class="ll-stack-line" v-for="(fr, fIdx) in s.vars" :key="fIdx">
                  <span class="ll-fname">{{ fr.title }}</span>
                  <span class="ll-now">&rarr;</span>
                  <span v-for="(v, vIdx) in fr.rows" :key="vIdx" class="ll-frame">
                    {{ v[0] }} = <b>{{ v[1] }}</b><span v-if="vIdx < fr.rows.length - 1">, </span>
                  </span>
                </div>
              </div>

              <!-- Vertical Resizer for Call Stack Panel -->
              <div class="ll-vresizer" ref="tableResizerRef"></div>

              <!-- Current Execution Step Badge Description -->
              <div class="ll-badge-wrap">
                <div
                  class="ll-badge"
                  :class="{
                    'll-badge-error': s.badge && (s.badge.includes('Dip') || s.badge.includes('drop') || s.badge.includes('No trade')),
                    'll-badge-success': s.badge && (s.badge.includes('Optimal') || s.badge.includes('Harvested') || s.badge.includes('finished') || s.badge.includes('higher'))
                  }"
                >
                  {{ s.badge || 'Ready to visualize Best Time to Buy and Sell Stock II.' }}
                </div>
              </div>
            </div>

            <!-- Horizontal Resizer between Left and Right Columns -->
            <div class="ll-resizer" ref="hResizerRef"></div>

            <!-- Right Column: Code & Faculty Lecture Panel -->
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
                  </select>
                </div>

                <!-- Code Tab -->
                <div v-if="rightTab === 'code'" class="ll-code-scroll">
                  <pre class="ll-pre"><span v-for="(line, i) in codeLines" :key="i" class="ll-codeline" :class="{ 'll-hl': line[0] && line[0] === s.code }">{{ line[1] === '' ? ' ' : line[1] }}</span></pre>
                </div>

                <!-- Pseudocode Tab -->
                <div v-else-if="rightTab === 'pseudo'" class="ll-code-scroll">
                  <pre class="ll-pre"><span v-for="(line, i) in pseudocodeLines" :key="i" class="ll-codeline">{{ line }}</span></pre>
                </div>

                <!-- Time and Space Complexity Tab -->
                <div v-else class="ll-info-scroll">

                  <!-- ════════ BRUTE FORCE COMPLEXITY ════════ -->
                  <template v-if="currentApproach === 'brute'">
                    <h3 class="ll-cx-heading">Brute Force &mdash; Complexity Analysis</h3>
                    <p class="ll-cx-intro">
                      Exhaustively explores every possible Buy/Skip/Sell/Hold decision at each
                      trading day using recursion. At each state <code>(day, canBuy)</code>,
                      two branches are tried, generating an exponential decision tree.
                    </p>

                    <h4 class="ll-cx-sub">Step-by-step Breakdown</h4>
                    <table class="ll-complexity-table">
                      <thead>
                        <tr><th>Operation</th><th>Time</th><th>Space</th><th>Reason</th></tr>
                      </thead>
                      <tbody>
                        <tr>
                          <td>Read input prices[]</td>
                          <td class="ll-cx-good">O(n)</td>
                          <td class="ll-cx-good">O(n)</td>
                          <td>Single pass to fill the array</td>
                        </tr>
                        <tr>
                          <td>Recursive Buy/Skip/Sell/Hold branching</td>
                          <td class="ll-cx-bad">O(2<sup>n</sup>)</td>
                          <td class="ll-cx-mid">O(n)</td>
                          <td>Binary decision tree of depth n &rarr; 2<sup>n</sup> leaves; call stack depth = n</td>
                        </tr>
                        <tr>
                          <td>Base case check &amp; return</td>
                          <td class="ll-cx-good">O(1)</td>
                          <td class="ll-cx-good">O(1)</td>
                          <td>Simple comparison <code>day &ge; n</code> at each leaf</td>
                        </tr>
                      </tbody>
                    </table>

                    <h4 class="ll-cx-sub">Overall Complexity</h4>
                    <div class="ll-cx-summary-grid">
                      <div class="ll-cx-card ll-cx-card-bad">
                        <div class="ll-cx-card-label">Time</div>
                        <div class="ll-cx-card-val">O(2<sup>n</sup>)</div>
                        <div class="ll-cx-card-note">Exponential &mdash; infeasible for large n</div>
                      </div>
                      <div class="ll-cx-card ll-cx-card-mid">
                        <div class="ll-cx-card-label">Space</div>
                        <div class="ll-cx-card-val">O(n)</div>
                        <div class="ll-cx-card-note">Recursion call stack depth</div>
                      </div>
                      <div class="ll-cx-card ll-cx-card-good">
                        <div class="ll-cx-card-label">Optimal?</div>
                        <div class="ll-cx-card-val">YES</div>
                        <div class="ll-cx-card-note">Exhaustive &rarr; guaranteed correct</div>
                      </div>
                    </div>

                    <div class="ll-note">
                      <strong>When is Brute Force used?</strong> Only for very small inputs (n &le; 20).
                      For real trading problems with hundreds of days, the Greedy single-pass approach is the only practical choice.
                    </div>
                  </template>

                  <!-- ════════ GREEDY COMPLEXITY ════════ -->
                  <template v-else>
                    <h3 class="ll-cx-heading">Greedy Algorithm &mdash; Complexity Analysis</h3>
                    <p class="ll-cx-intro">
                      Accumulates profit by capturing every consecutive positive daily price
                      ascent. A multi-day holding from Day A to Day B telescopes into the exact
                      sum of unit-length daily deltas &mdash; so greedily harvesting every
                      <code>prices[i] &gt; prices[i-1]</code> day is globally optimal.
                    </p>

                    <h4 class="ll-cx-sub">Step-by-step Breakdown</h4>
                    <table class="ll-complexity-table">
                      <thead>
                        <tr><th>Operation</th><th>Time</th><th>Space</th><th>Reason</th></tr>
                      </thead>
                      <tbody>
                        <tr>
                          <td>Read input prices[]</td>
                          <td class="ll-cx-good">O(n)</td>
                          <td class="ll-cx-good">O(n)</td>
                          <td>Single pass to fill the array</td>
                        </tr>
                        <tr>
                          <td>Initialise <code>maxProfit = 0</code></td>
                          <td class="ll-cx-good">O(1)</td>
                          <td class="ll-cx-good">O(1)</td>
                          <td>Single integer accumulator</td>
                        </tr>
                        <tr>
                          <td>Greedy peak-valley forward scan</td>
                          <td class="ll-cx-good">O(n)</td>
                          <td class="ll-cx-good">O(1)</td>
                          <td>Single left-to-right pass; ascent check and accumulation are O(1) per step</td>
                        </tr>
                      </tbody>
                    </table>

                    <h4 class="ll-cx-sub">Overall Complexity</h4>
                    <div class="ll-cx-summary-grid">
                      <div class="ll-cx-card ll-cx-card-good">
                        <div class="ll-cx-card-label">Time</div>
                        <div class="ll-cx-card-val">O(n)</div>
                        <div class="ll-cx-card-note">Single forward scan &mdash; optimal</div>
                      </div>
                      <div class="ll-cx-card ll-cx-card-good">
                        <div class="ll-cx-card-label">Space</div>
                        <div class="ll-cx-card-val">O(1)</div>
                        <div class="ll-cx-card-note">Only a scalar <code>maxProfit</code> variable</div>
                      </div>
                      <div class="ll-cx-card ll-cx-card-good">
                        <div class="ll-cx-card-label">Optimal?</div>
                        <div class="ll-cx-card-val">YES</div>
                        <div class="ll-cx-card-note">Provable via telescoping sum identity</div>
                      </div>
                    </div>

                    <h4 class="ll-cx-sub">Approach Comparison</h4>
                    <table class="ll-complexity-table">
                      <thead>
                        <tr><th>Approach</th><th>Time</th><th>Space</th><th>Practical?</th></tr>
                      </thead>
                      <tbody>
                        <tr>
                          <td><strong>Brute Force</strong></td>
                          <td class="ll-cx-bad">O(2<sup>n</sup>)</td>
                          <td class="ll-cx-mid">O(n)</td>
                          <td class="ll-cx-bad">No &mdash; exponential blowup</td>
                        </tr>
                        <tr>
                          <td><strong>Dynamic Programming</strong></td>
                          <td class="ll-cx-good">O(n)</td>
                          <td class="ll-cx-good">O(1)</td>
                          <td class="ll-cx-mid">Yes &mdash; but adds unnecessary state tracking</td>
                        </tr>
                        <tr>
                          <td><strong>Greedy (Peak-Valley)</strong></td>
                          <td class="ll-cx-good">O(n)</td>
                          <td class="ll-cx-good">O(1)</td>
                          <td class="ll-cx-good">Yes &mdash; simplest and fastest</td>
                        </tr>
                      </tbody>
                    </table>

                    <div class="ll-note">
                      <strong>Key Insight:</strong> Unlike Stock I (one transaction only), Stock II
                      allows unlimited non-overlapping transactions. Because every multi-day
                      profit telescopes into a sum of positive unit-day deltas, locally greedy
                      choices lead directly to the globally optimal solution.
                    </div>
                  </template>

                </div>
              </div>
            </div>
          </div>

          <!-- Bottom Footer Toolbar -->
          <div class="ll-footer">
            Step {{ si + 1 }} / {{ steps.length }}
            <span class="ll-speed-wrap">
              Speed
              <input type="range" min="100" max="2000" step="100" v-model.number="speed" />
            </span>
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
  --coral: #F04D4D;
  --coral-dark: #d93e3e;
  --coral-light: #fff0f0;
  --bg: #f5f6fa;
  --surface: #ffffff;
  --surface2: #f1f4f9;
  --border: #e2e8f0;
  --border2: #cbd5e1;
  --text: #1e293b;
  --text2: #475569;
  --muted: #94a3b8;
  --blue: #3b82f6;
  --blue-light: #eff6ff;
  --green: #22c55e;
  --green-light: #f0fdf4;
  --orange: #f97316;
  --orange-light: #fff7ed;
  --purple: #9333ea;
  --purple-light: #f3e8ff;
  --red: #ef4444;
  --red-dark: #991b1b;
  --red-light: #fef2f2;
  --shadow-sm: 0 1px 3px rgba(0,0,0,.08), 0 1px 2px rgba(0,0,0,.04);
  --radius: 8px;
  --radius-sm: 6px;
  background: var(--bg);
  color: var(--text);
  font-family: 'Segoe UI', system-ui, sans-serif;
  font-size: 12.5px;
  height: 75vh;
  width: 100%;
  display: flex;
  flex-direction: column;
  overflow: hidden;
}

@keyframes ll-pop { 0% { transform: scale(0.85); opacity: 0; } 100% { transform: scale(1); opacity: 1; } }
@keyframes ll-pulse { 0%, 100% { transform: scale(1); } 50% { transform: scale(1.05); } }

.slide-wrapper {
  width: 107%;
  max-height: 100%;
  margin-top: -10px;
  margin-left: -30px;
  font-size: 0.8rem;
  font-weight: 400;
}

.slide-body {
  height: 100%;
  display: flex;
  flex-direction: column;
  border-radius: 4px;
}

.navbar {
  background-color: #ffffff;
  padding: 0 10px;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
  gap: 0.75rem;
  width: 94.7%;
  position: fixed;
}
.navbar > img { height: 30px; }
.navbar-title {
  font-size: 1.35rem;
  font-weight: 700;
  color: #ffffff;
  background-color: #ef5050;
  border-radius: 5px;
  padding: 2px 10px;
  width: 80%;
  margin: 0 0 0 -10px;
}

.row-main {
  width: 100%;
  height: 90%;
  margin-top: 36px;
  overflow: auto;
}

.ll-toolbar {
  display: flex;
  align-items: center;
  gap: 6px;
  padding: 5px 12px;
  background: var(--surface);
  border-bottom: 1px solid var(--border);
  box-shadow: var(--shadow-sm);
  flex-shrink: 0;
  flex-wrap: wrap;
  margin-top: 4px;
}

.ll-approach-group {
  display: flex;
  gap: 2px;
  background: var(--surface2);
  padding: 2px;
  border-radius: var(--radius-sm);
  border: 1px solid var(--border);
}

.ll-approach-btn {
  padding: 4px 8px;
  font-size: 11px;
  font-weight: 600;
  border: none;
  background: transparent;
  color: var(--text2);
  border-radius: 4px;
  cursor: pointer;
  transition: all .15s ease;
  white-space: nowrap;
}
.ll-approach-btn:hover { color: var(--coral); }
.ll-approach-btn.active {
  background: var(--coral);
  color: #fff;
  box-shadow: var(--shadow-sm);
}

.ll-input-group {
  display: flex;
  align-items: center;
  gap: 4px;
}
.ll-input-group label {
  font-size: 11.5px;
  color: var(--muted);
  font-weight: 700;
}

.ll-text-input {
  width: 135px;
  padding: 3px 6px;
  font-family: monospace;
  font-size: 11px;
  border: 1px solid var(--border2);
  border-radius: var(--radius-sm);
  background: var(--surface);
  color: var(--text);
}
.ll-text-input:focus {
  outline: none;
  border-color: var(--coral);
  box-shadow: 0 0 0 3px rgba(240,77,77,.1);
}

.ll-viz-btn {
  padding: 5px 12px;
  font-size: 11.5px;
  font-weight: 600;
  background: var(--coral);
  color: #fff;
  border: none;
  border-radius: var(--radius-sm);
  cursor: pointer;
  box-shadow: var(--shadow-sm);
  transition: filter .15s;
}
.ll-viz-btn:hover { filter: brightness(1.08); }

.ll-nav-controls {
  display: flex;
  align-items: center;
  gap: 4px;
  margin-left: auto;
  flex-shrink: 0;
  flex-wrap: wrap;
}

.ll-nav-btn {
  padding: 4px 9px;
  font-size: 11px;
  font-weight: 500;
  background: var(--surface2);
  border: 1px solid var(--border2);
  color: var(--text2);
  border-radius: var(--radius-sm);
  cursor: pointer;
  transition: all .15s ease;
  white-space: nowrap;
}
.ll-nav-btn:hover {
  background: var(--surface);
  border-color: var(--coral);
  color: var(--coral);
}

.ll-play-btn {
  padding: 4px 9px;
  font-size: 11px;
  font-weight: 600;
  background: var(--blue-light);
  border: 1px solid var(--blue);
  color: var(--blue);
  border-radius: var(--radius-sm);
  cursor: pointer;
  min-width: 68px;
  transition: all .15s ease;
}
.ll-play-btn:hover {
  background: var(--blue);
  color: #fff;
}

.ll-main {
  display: flex;
  flex: 1;
  overflow: hidden;
  position: relative;
}

.ll-left-col {
  display: flex;
  flex-direction: column;
  min-width: 220px;
  max-width: 75%;
  overflow: hidden;
}

.ll-house-banner {
  background: var(--surface);
  border-bottom: 1px solid var(--border);
  padding: 4px 10px 6px;
  flex-shrink: 0;
}

.ll-house-title {
  font-size: 10.5px;
  font-weight: 700;
  color: var(--text2);
  margin-bottom: 2px;
}

.ll-dual-arrays-wrap {
  display: flex;
  flex-direction: column;
  gap: 5px;
  margin-top: 2px;
}

.ll-arr-row {
  display: flex;
  align-items: center;
  gap: 6px;
}

.ll-arr-tag {
  font-size: 10px;
  font-family: monospace;
  font-weight: 800;
  color: var(--coral);
  width: 45px;
  flex-shrink: 0;
}

.ll-arr-cells {
  display: flex;
  gap: 7px;
}

.ll-arr-cell {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  background: var(--surface2);
  border: 1px solid var(--border2);
  border-radius: 5px;
  width: 44px;
  padding: 2px 3px;
  transition: all .2s ease;
}

.ll-cell-idx {
  font-size: 8px;
  color: var(--muted);
  font-family: monospace;
}

.ll-cell-val {
  font-size: 11px;
  font-weight: 700;
  color: var(--text);
  font-family: monospace;
}

.ll-cell-active {
  border-color: #f59e0b !important;
  background: #fef3c7 !important;
  transform: translateY(-2px);
  box-shadow: 0 0 0 2px rgba(245,158,11,.3);
}

.ll-cell-prev {
  border-color: #10b981 !important;
  background: #f0fdf4 !important;
}

.ll-cell-profit {
  border-color: #10b981 !important;
  background: #dcfce7 !important;
}

.ll-ptrs {
  display: flex;
  gap: 6px;
  padding: 3px 10px;
  min-height: 26px;
  flex-wrap: wrap;
  width: 100%;
  box-sizing: border-box;
}

.ll-ptr-chip {
  padding: 2px 7px;
  font-size: 10.5px;
  background: var(--surface2);
  border: 1px solid var(--border);
  border-radius: var(--radius-sm);
  box-shadow: var(--shadow-sm);
  white-space: nowrap;
  font-family: monospace;
}

.ll-chip-success { background: #f0fdf4 !important; border-color: #86efac !important; }

.ll-c-blue { color: var(--blue); }
.ll-c-orange { color: var(--orange); }
.ll-c-green { color: var(--green); }
.ll-c-purple { color: var(--purple); }
.ll-c-red { color: var(--red); }
.ll-c-muted { color: var(--muted); }

.ll-viz-wrap {
  flex-shrink: 0;
  position: relative;
  background: var(--surface);
  border-bottom: 1px solid var(--border);
  overflow: auto;
}

.ll-timeline-container {
  display: flex;
  flex-direction: column;
  width: 100%;
  height: 100%;
  min-height: 0;
}

.ll-section-caption {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 3px 10px;
  background: var(--surface2);
  border-top: 1px solid var(--border);
  border-bottom: 1px solid var(--border);
  font-size: 10.5px;
  font-weight: 700;
  color: var(--text2);
}

.ll-calc-pill {
  font-size: 10px;
  font-weight: 700;
  font-family: monospace;
  padding: 1px 6px;
  border-radius: 10px;
  background: #dcfce7;
  color: #15803d;
}
.ll-pill-error {
  background: #fee2e2 !important;
  color: #991b1b !important;
  border-radius: 10px;
  padding: 1px 6px;
  font-size: 10px;
  font-weight: 700;
  font-family: monospace;
}

.ll-jump-track-scroll {
  flex: 1;
  overflow: auto;
  padding: 6px 10px;
  display: flex;
  flex-direction: column;
}

.ll-track-board {
  display: flex;
  flex-direction: column !important;
  align-items: center;
  justify-content: flex-start;
  margin: 0 auto;
  padding: 4px 0 8px;
}

.ll-arc-svg-wrap {
  height: 42px;
  display: flex;
  justify-content: center;
  align-items: flex-end;
  width: 100%;
}

.ll-arc-svg {
  display: block;
  overflow: visible;
}

.ll-arc-txt {
  font-family: Consolas, 'Fira Code', monospace !important;
  font-size: 9.5px !important;
  font-weight: 700 !important;
  user-select: none !important;
}

.ll-stones-track {
  display: flex;
  flex-direction: row;
  justify-content: flex-start;
  align-items: flex-start;
  gap: 16px;
  padding: 0;
}

.ll-stone-pod {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 52px;
  flex-shrink: 0;
}

.ll-pod-pointer {
  height: 18px;
  display: flex;
  align-items: flex-end;
  justify-content: center;
  margin-bottom: 3px;
  width: 100%;
}

.ll-pin {
  font-family: Consolas, 'Fira Code', monospace;
  font-size: 8px;
  font-weight: 800;
  padding: 1px 4px;
  border-radius: 3px;
  letter-spacing: 0.3px;
  line-height: 1.1;
  text-align: center;
  white-space: nowrap;
}

.ll-pin-active {
  color: #b45309;
  background: #fef3c7;
  border: 1px solid #f59e0b;
}

.ll-pin-buy {
  color: #047857;
  background: #dcfce7;
  border: 1px solid #10b981;
}

.ll-pin-sell {
  color: #6b21a8;
  background: #f3e8ff;
  border: 1px solid #9333ea;
}

.ll-pin-space {
  visibility: hidden;
}

.ll-stone-card {
  width: 52px;
  height: 48px;
  background: var(--surface);
  border: 1.5px solid var(--border2);
  border-radius: 6px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  box-shadow: var(--shadow-sm);
  transition: all 0.2s ease;
  position: relative;
}

.ll-stone-card-idx {
  font-family: Consolas, monospace;
  font-size: 8.5px;
  color: var(--muted);
  font-weight: 600;
  line-height: 1;
  margin-bottom: 1px;
}

.ll-stone-card-val {
  font-family: Consolas, monospace;
  font-size: 14px;
  font-weight: 800;
  color: var(--text);
  line-height: 1.1;
}

.ll-stone-card-sub {
  font-family: Consolas, monospace;
  font-size: 7.5px;
  color: var(--muted);
  font-weight: 600;
  line-height: 1;
  margin-top: 1px;
}

.ll-pod-active .ll-stone-card {
  border-color: #f59e0b !important;
  background: #fffbeb !important;
  box-shadow: 0 0 0 3px rgba(245, 158, 11, 0.25) !important;
  transform: translateY(-2px);
}
.ll-pod-active .ll-stone-card-val {
  color: #b45309 !important;
}

.ll-pod-reach .ll-stone-card {
  border-color: #10b981 !important;
  background: #f0fdf4 !important;
}
.ll-pod-reach .ll-stone-card-val {
  color: #065f46 !important;
}

.ll-pod-profit .ll-stone-card {
  border-color: #10b981 !important;
  background: #ecfdf5 !important;
  box-shadow: 0 0 0 2px rgba(16, 185, 129, 0.25) !important;
}

.ll-pod-stranded .ll-stone-card {
  border-color: #ef4444 !important;
  background: #fef2f2 !important;
  box-shadow: 0 0 0 2px rgba(239, 68, 68, 0.2) !important;
}

.ll-pod-barrier {
  height: 22px;
  display: flex;
  justify-content: center;
  align-items: flex-start;
  margin-top: 3px;
  width: 100%;
}

.ll-barrier-tag {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.ll-barrier-arrow {
  font-size: 7.5px;
  line-height: 1;
}
.ll-arr-up { color: #10b981; }
.ll-arr-down { color: #ef4444; }

.ll-barrier-lbl {
  font-family: Consolas, monospace;
  font-size: 8px;
  font-weight: 700;
  color: #ffffff;
  border-radius: 3px;
  padding: 1px 4px;
  white-space: nowrap;
}
.ll-lbl-gain { background: #10b981; }
.ll-lbl-dip { background: #64748b; }

.ll-horizon-line-wrap {
  width: 100%;
  display: flex;
  justify-content: center;
  padding: 4px 0 0;
}

.ll-horizon-track {
  width: 100%;
  height: 4px;
  background: #e2e8f0;
  border-radius: 2px;
  overflow: hidden;
}

.ll-horizon-active {
  height: 100%;
  background: #10b981;
  transition: width 0.25s ease;
}

.ll-arc-jump {
  animation: ll-pop 0.3s ease;
}

/* Legend Strip */
.ll-legend {
  display: flex;
  flex-wrap: wrap;
  gap: 4px 10px;
  padding: 4px 10px;
  border-bottom: 1px solid var(--border);
  flex-shrink: 0;
  background: var(--surface2);
}

.ll-leg {
  display: flex;
  align-items: center;
  gap: 4px;
  font-size: 10px;
  color: var(--text2);
  font-weight: 500;
}

.ll-legdot {
  width: 8px;
  height: 8px;
  border-radius: 2px;
  display: inline-block;
}

.ll-legdot-cur { background: #fef3c7; border: 1.5px solid #f59e0b; }
.ll-legdot-new { background: #dcfce7; border: 1.5px solid #10b981; }
.ll-legdot-red { background: #fee2e2; border: 1.5px solid #ef4444; }
.ll-legdot-purple { background: #f3e8ff; border: 1.5px solid #9333ea; }

/* Tree view for Brute Force */
.ll-tree-container {
  display: flex;
  flex-direction: column;
  width: 100%;
  height: 100%;
}

.ll-tree-scroll-area {
  flex: 1;
  overflow: auto;
  padding: 4px 6px;
  display: flex;
  flex-direction: column;
  align-items: center;
  min-height: 0;
  width: 100%;
  box-sizing: border-box;
}

.ll-tree-svg {
  display: block;
  flex-shrink: 0;
  max-width: 100%;
  margin: 0 auto;
}

.ll-tree-edge {
  stroke: #cbd5e1;
  stroke-width: 1.5px;
}

.ll-edge-pick {
  stroke: #22c55e !important;
  stroke-width: 2px !important;
}

.ll-edge-skip {
  stroke: #94a3b8 !important;
  stroke-dasharray: 4,3 !important;
}

.ll-tree-node-group { cursor: default; }

.ll-node-rect {
  fill: #ffffff;
  stroke: #cbd5e1;
  stroke-width: 1.4px;
  filter: drop-shadow(0 1px 2px rgba(0,0,0,.05));
  transition: all .25s ease;
}

.ll-node-active {
  stroke: #f59e0b !important;
  stroke-width: 2.2px !important;
  fill: #fffbeb !important;
  filter: drop-shadow(0 0 5px rgba(245,158,11,.45)) !important;
}

.ll-node-solved {
  stroke: #10b981 !important;
  fill: #dcfce7 !important;
  stroke-width: 1.6px !important;
}

.ll-node-leaf {
  stroke: #9333ea !important;
  fill: #f3e8ff !important;
  stroke-width: 1.6px !important;
}

.ll-tree-svg text {
  font-family: 'Segoe UI', system-ui, sans-serif !important;
  user-select: none;
}

.ll-node-text-call {
  font-family: 'Consolas','Fira Code',monospace !important;
  font-size: 8px !important;
  font-weight: 700 !important;
  fill: #1e293b !important;
  dominant-baseline: central !important;
}

.ll-node-text-val {
  font-family: 'Consolas','Fira Code',monospace !important;
  font-size: 7.5px !important;
  font-weight: 700 !important;
  fill: #64748b !important;
  dominant-baseline: central !important;
}

.ll-val-active { fill: #b45309 !important; }
.ll-val-solved { fill: #047857 !important; }

/* Resizers */
.ll-resizer {
  width: 5px;
  cursor: col-resize;
  background: var(--border);
  transition: background .15s;
  flex-shrink: 0;
  position: relative;
  z-index: 20;
}
.ll-resizer:hover, .ll-resizer.drag { background: var(--coral); }

.ll-vresizer {
  height: 5px;
  cursor: row-resize;
  background: var(--border);
  transition: background .15s;
  flex-shrink: 0;
  position: relative;
  z-index: 20;
}
.ll-vresizer:hover, .ll-vresizer.drag { background: var(--coral); }

.ll-right-col {
  flex: 1;
  display: flex;
  flex-direction: column;
  min-width: 0;
  overflow: hidden;
}

.ll-table-area {
  padding: 4px 10px;
  background: var(--surface);
  border-bottom: 1px solid var(--border);
  flex-shrink: 0;
  overflow-y: auto;
  overflow-x: hidden;
  box-sizing: border-box;
  min-width: 0;
}

.ll-table-title {
  font-size: 9.5px;
  color: var(--muted);
  margin-bottom: 2px;
  font-style: italic;
}

.ll-stack-line {
  font-family: Consolas, monospace;
  font-size: 11px;
  line-height: 1.6;
}

.ll-fname { color: var(--text2); }
.ll-now { color: var(--orange); margin-left: 4px; font-size: 9.5px; }
.ll-frame {
  font-family: Consolas, monospace;
  font-size: 10.5px;
  color: var(--text2);
  white-space: nowrap;
  padding: 1px 0;
}

.ll-badge-wrap {
  padding: 4px 8px;
  background: var(--surface);
  border-bottom: 1px solid var(--border);
  display: flex;
  align-items: center;
  min-height: 30px;
  flex-shrink: 0;
}

.ll-badge {
  font-size: 10.5px;
  line-height: 1.35;
  padding: 3px 10px;
  border-radius: var(--radius-sm);
  background: var(--coral-light);
  color: var(--coral-dark);
  border-left: 3px solid var(--coral);
  word-break: break-word;
  font-weight: 500;
  display: inline-block;
}

.ll-badge-error {
  background: var(--red-light);
  color: var(--red-dark);
  border-left-color: var(--red);
  font-weight: 600;
}
.ll-badge-success {
  background: var(--green-light);
  color: #166534;
  border-left-color: var(--green);
  font-weight: 600;
}

.ll-code-panel {
  display: flex;
  flex-direction: column;
  height: 100%;
  overflow: hidden;
}

.ll-code-header {
  display: flex;
  align-items: center;
  gap: 6px;
  padding: 5px 12px;
  background: var(--surface);
  border-bottom: 1px solid var(--border);
  box-shadow: var(--shadow-sm);
  flex-shrink: 0;
  flex-wrap: wrap;
}

.ll-tabbar {
  display: flex;
  gap: 3px;
  flex-wrap: wrap;
}

.ll-tab-btn {
  padding: 4px 9px;
  font-size: 10.5px;
  font-weight: 600;
  border: 1px solid var(--border2);
  background: var(--surface2);
  color: var(--text2);
  border-radius: var(--radius-sm);
  cursor: pointer;
  transition: all .15s ease;
  white-space: nowrap;
}
.ll-tab-btn:hover { border-color: var(--coral); color: var(--coral); }
.ll-tab-btn.active {
  background: var(--coral);
  border-color: var(--coral);
  color: #fff;
}

.ll-lang-select {
  margin-left: auto;
  padding: 4px 24px 4px 8px;
  font-size: 11px;
  font-weight: 500;
  border: 1px solid var(--border2);
  border-radius: var(--radius-sm);
  background: var(--surface2);
  color: var(--text);
  cursor: pointer;
  appearance: none;
  background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='10' height='6' viewBox='0 0 10 6'%3E%3Cpath d='M0 0l5 6 5-6z' fill='%2394a3b8'/%3E%3C/svg%3E");
  background-repeat: no-repeat;
  background-position: right 8px center;
  min-width: 95px;
  transition: border-color .15s;
}
.ll-lang-select:focus {
  outline: none;
  border-color: var(--coral);
  box-shadow: 0 0 0 3px rgba(240,77,77,.1);
}

.ll-code-scroll {
  flex: 1;
  overflow: auto;
  background: #f8fafc;
  padding: 10px 14px;
  min-width: 0;
}

.ll-pre {
  margin: 0;
  font-family: 'Cascadia Code', 'Fira Code', 'Consolas', monospace;
  font-size: 11px;
  line-height: 1.5;
  color: var(--text);
  white-space: pre;
  padding-bottom: 100px;
}

.ll-codeline {
  display: block;
  padding: 0 14px;
  margin: 0 -14px;
}

.ll-hl {
  background: #dcfce7;
  color: #15803d;
  font-weight: 600;
  border-left: 3px solid var(--green);
  border-radius: 3px;
}

.ll-info-scroll {
  flex: 1;
  overflow: auto;
  padding: 12px 16px;
  background: var(--surface);
  color: var(--text2);
  font-size: 12px;
  line-height: 1.55;
}
.ll-info-scroll h3 { font-size: 13px; font-weight: 700; color: var(--text); margin: 0 0 6px; }
.ll-info-scroll h3:not(:first-child) { margin-top: 14px; }
.ll-info-scroll h4 { font-size: 12px; font-weight: 700; color: var(--text); margin: 10px 0 4px; }
.ll-info-scroll p { margin: 0 0 6px; }
.ll-info-scroll ul { margin: 0 0 10px 16px; padding: 0; }
.ll-info-scroll li { margin-bottom: 3px; }
.ll-info-scroll code {
  font-family: Consolas, monospace;
  font-size: 11px;
  background: var(--surface2);
  padding: 1px 4px;
  border-radius: 4px;
  border: 1px solid var(--border);
  color: var(--coral-dark);
}

.ll-complexity-table {
  width: 100%;
  border-collapse: collapse;
  margin-bottom: 10px;
  font-size: 11.5px;
}
.ll-complexity-table th, .ll-complexity-table td {
  border: 1px solid var(--border);
  padding: 6px 8px;
  text-align: left;
}
.ll-complexity-table th { background: var(--surface2); color: var(--text); font-weight: 700; }
.ll-complexity-table td:nth-child(2), .ll-complexity-table td:nth-child(3) { font-family: Consolas, monospace; }

.ll-math-box {
  background: var(--surface2);
  border: 1px solid var(--border2);
  border-radius: var(--radius-sm);
  padding: 6px 10px;
  font-family: Consolas, monospace;
  font-weight: 700;
  color: #1e293b;
  margin-bottom: 6px;
}

/* Complexity Tab Styles */
.ll-cx-heading {
  font-size: 13px;
  font-weight: 700;
  color: var(--text);
  margin: 0 0 6px;
}

.ll-cx-intro {
  font-size: 10.5px;
  color: var(--text2);
  margin: 0 0 10px;
  line-height: 1.55;
}

.ll-cx-sub {
  font-size: 11px;
  font-weight: 700;
  color: var(--text2);
  margin: 10px 0 4px;
  border-bottom: 1px solid var(--border);
  padding-bottom: 3px;
}

/* Colored complexity cells */
.ll-cx-good { color: #15803d; font-weight: 700; }
.ll-cx-mid  { color: #b45309; font-weight: 700; }
.ll-cx-bad  { color: #b91c1c; font-weight: 700; }

/* Summary card grid */
.ll-cx-summary-grid {
  display: flex;
  gap: 8px;
  flex-wrap: wrap;
  margin: 6px 0 10px;
}

.ll-cx-card {
  flex: 1;
  min-width: 90px;
  border-radius: var(--radius-sm);
  padding: 8px 10px;
  text-align: center;
  border: 1.5px solid var(--border);
}

.ll-cx-card-label {
  font-size: 9px;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: .05em;
  opacity: .7;
  margin-bottom: 4px;
}

.ll-cx-card-val {
  font-size: 13px;
  font-weight: 800;
  font-family: monospace;
  margin-bottom: 3px;
}

.ll-cx-card-note {
  font-size: 8.5px;
  opacity: .75;
  line-height: 1.3;
}

.ll-cx-card-good {
  background: #f0fdf4;
  border-color: #86efac;
  color: #15803d;
}

.ll-cx-card-mid {
  background: #fff7ed;
  border-color: #fed7aa;
  color: #c2410c;
}

.ll-cx-card-bad {
  background: #fef2f2;
  border-color: #fca5a5;
  color: #b91c1c;
}

.ll-note {
  background: #fefce8;
  border: 1px solid #fef08a;
  border-left: 3px solid #eab308;
  padding: 6px 10px;
  font-size: 10.5px;
  color: #854d0e;
  border-radius: 0 4px 4px 0;
  margin-top: 10px;
  margin-bottom: 120px;
}

.ll-footer {
  display: flex;
  align-items: center;
  padding: 3px 12px;
  background: var(--surface);
  border-top: 1px solid var(--border);
  font-size: 10.5px;
  color: var(--muted);
  flex-shrink: 0;
}

.ll-speed-wrap {
  display: flex;
  align-items: center;
  gap: 4px;
  margin-left: 14px;
}
.ll-speed-wrap input[type=range] {
  width: 80px;
  accent-color: var(--coral);
}
</style>