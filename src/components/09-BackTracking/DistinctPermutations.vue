<script setup>
import { ref, reactive, computed, onMounted, onBeforeUnmount, watch, nextTick } from 'vue';

defineProps({
  topic: { type: String, default: 'Backtracking Algorithms' },
  subTopic: { type: String, default: 'Distinct Permutations of a String' }
});

// ─── Multi-Language Code Definitions ─────────────────────────────────────────
const CODES = {
  java: [
    ['',               'import java.util.Scanner;'],
    ['',               'import java.util.List;'],
    ['',               'import java.util.ArrayList;'],
    ['',               'import java.util.Arrays;'],
    ['',               ''],
    ['',               'public class Main {'],
    ['c_entry',        '    static List<String> findPermutations(String s) {'],
    ['c_init_result',  '        List<String> result = new ArrayList<>();'],
    ['c_to_char_arr',  '        char[] chars = s.toCharArray();'],
    ['c_sort_chars',   '        Arrays.sort(chars);'],
    ['c_init_visited', '        boolean[] visited = new boolean[chars.length];'],
    ['c_init_current', '        StringBuilder current = new StringBuilder();'],
    ['c_call_backtrack','        backtrack(chars, visited, current, result);'],
    ['c_return_res',   '        return result;'],
    ['',               '    }'],
    ['',               ''],
    ['c_bt_entry',     '    static void backtrack(char[] chars, boolean[] visited,'],
    ['',               '                          StringBuilder current, List<String> result) {'],
    ['c_base_check',   '        if (current.length() == chars.length) {'],
    ['c_save_sol',     '            result.add(current.toString());'],
    ['c_base_ret',     '            return;'],
    ['',               '        }'],
    ['c_loop_chars',   '        for (int i = 0; i < chars.length; i++) {'],
    ['c_chk_visited',  '            if (visited[i]) {'],
    ['c_cont_visited', '                continue;'],
    ['',               '            }'],
    ['c_chk_dup',      '            if (i > 0 && chars[i] == chars[i-1] && !visited[i-1]) {'],
    ['c_cont_dup',     '                continue;'],
    ['',               '            }'],
    ['c_mark_visited', '            visited[i] = true;'],
    ['c_append_char',  '            current.append(chars[i]);'],
    ['c_recurse',      '            backtrack(chars, visited, current, result);'],
    ['c_backtrack_pop','            current.deleteCharAt(current.length() - 1);'],
    ['c_unmark_visited','            visited[i] = false;'],
    ['',               '        }'],
    ['',               '    }'],
    ['',               ''],
    ['',               '    public static void main(String[] args) {'],
    ['m_scanner',      '        Scanner sc = new Scanner(System.in);'],
    ['m_read_str',     '        String s = sc.next();'],
    ['m_call_solver',  '        List<String> result = findPermutations(s);'],
    ['m_print_count',  '        System.out.println(result.size());'],
    ['m_loop_print',   '        for (int i = 0; i < result.size(); i++) {'],
    ['m_print_elem',   '            System.out.println(result.get(i));'],
    ['',               '        }'],
    ['m_done',         '    }'],
    ['',               '}']
  ],
  cpp: [
    ['',               '#include <iostream>'],
    ['',               '#include <vector>'],
    ['',               '#include <string>'],
    ['',               '#include <algorithm>'],
    ['',               'using namespace std;'],
    ['',               ''],
    ['c_bt_entry',     'void backtrack(string& chars, vector<bool>& visited,'],
    ['',               '               string& current, vector<string>& result) {'],
    ['c_base_check',   '    if (current.size() == chars.size()) {'],
    ['c_save_sol',     '        result.push_back(current);'],
    ['c_base_ret',     '        return;'],
    ['',               '    }'],
    ['c_loop_chars',   '    for (int i = 0; i < (int)chars.size(); i++) {'],
    ['c_chk_visited',  '        if (visited[i]) continue;'],
    ['c_cont_visited', '        // skip visited'],
    ['c_chk_dup',      '        if (i > 0 && chars[i] == chars[i-1] && !visited[i-1]) continue;'],
    ['c_cont_dup',     '        // skip duplicate'],
    ['c_mark_visited', '        visited[i] = true;'],
    ['c_append_char',  '        current += chars[i];'],
    ['c_recurse',      '        backtrack(chars, visited, current, result);'],
    ['c_backtrack_pop','        current.pop_back();'],
    ['c_unmark_visited','        visited[i] = false;'],
    ['',               '    }'],
    ['',               '}'],
    ['',               ''],
    ['c_entry',        'vector<string> findPermutations(string s) {'],
    ['c_init_result',  '    vector<string> result;'],
    ['c_to_char_arr',  '    // s is already char array'],
    ['c_sort_chars',   '    sort(s.begin(), s.end());'],
    ['c_init_visited', '    vector<bool> visited(s.size(), false);'],
    ['c_init_current', '    string current = "";'],
    ['c_call_backtrack','    backtrack(s, visited, current, result);'],
    ['c_return_res',   '    return result;'],
    ['',               '}'],
    ['',               ''],
    ['',               'int main() {'],
    ['m_scanner',      '    string s;'],
    ['m_read_str',     '    cin >> s;'],
    ['m_call_solver',  '    vector<string> result = findPermutations(s);'],
    ['m_print_count',  '    cout << result.size() << "\\n";'],
    ['m_loop_print',   '    for (auto& p : result)'],
    ['m_print_elem',   '        cout << p << "\\n";'],
    ['m_done',         '    return 0;'],
    ['',               '}']
  ],
  python: [
    ['',               'import sys'],
    ['',               ''],
    ['c_bt_entry',     'def backtrack(chars, visited, current, result):'],
    ['c_base_check',   '    if len(current) == len(chars):'],
    ['c_save_sol',     '        result.append("".join(current))'],
    ['c_base_ret',     '        return'],
    ['c_loop_chars',   '    for i in range(len(chars)):'],
    ['c_chk_visited',  '        if visited[i]:'],
    ['c_cont_visited', '            continue'],
    ['c_chk_dup',      '        if i > 0 and chars[i] == chars[i-1] and not visited[i-1]:'],
    ['c_cont_dup',     '            continue'],
    ['c_mark_visited', '        visited[i] = True'],
    ['c_append_char',  '        current.append(chars[i])'],
    ['c_recurse',      '        backtrack(chars, visited, current, result)'],
    ['c_backtrack_pop','        current.pop()'],
    ['c_unmark_visited','        visited[i] = False'],
    ['',               ''],
    ['c_entry',        'def findPermutations(s):'],
    ['c_init_result',  '    result = []'],
    ['c_to_char_arr',  '    chars = list(s)'],
    ['c_sort_chars',   '    chars.sort()'],
    ['c_init_visited', '    visited = [False] * len(chars)'],
    ['c_init_current', '    current = []'],
    ['c_call_backtrack','    backtrack(chars, visited, current, result)'],
    ['c_return_res',   '    return result'],
    ['',               ''],
    ['',               'def main():'],
    ['m_scanner',      '    data = sys.stdin.read().split()'],
    ['m_read_str',     '    s = data[0]'],
    ['m_call_solver',  '    result = findPermutations(s)'],
    ['m_print_count',  '    print(len(result))'],
    ['m_loop_print',   '    for p in result:'],
    ['m_print_elem',   '        print(p)'],
    ['m_done',         ''],
    ['',               'if __name__ == "__main__":'],
    ['',               '    main()']
  ],
  javascript: [
    ['',               '// Node.js solution'],
    ['',               ''],
    ['c_bt_entry',     'function backtrack(chars, visited, current, result) {'],
    ['c_base_check',   '    if (current.length === chars.length) {'],
    ['c_save_sol',     '        result.push(current.join(""));'],
    ['c_base_ret',     '        return;'],
    ['',               '    }'],
    ['c_loop_chars',   '    for (let i = 0; i < chars.length; i++) {'],
    ['c_chk_visited',  '        if (visited[i]) continue;'],
    ['c_cont_visited', '        // skip visited'],
    ['c_chk_dup',      '        if (i > 0 && chars[i] === chars[i-1] && !visited[i-1]) continue;'],
    ['c_cont_dup',     '        // skip duplicate'],
    ['c_mark_visited', '        visited[i] = true;'],
    ['c_append_char',  '        current.push(chars[i]);'],
    ['c_recurse',      '        backtrack(chars, visited, current, result);'],
    ['c_backtrack_pop','        current.pop();'],
    ['c_unmark_visited','        visited[i] = false;'],
    ['',               '    }'],
    ['',               '}'],
    ['',               ''],
    ['c_entry',        'function findPermutations(s) {'],
    ['c_init_result',  '    const result = [];'],
    ['c_to_char_arr',  '    const chars = s.split("");'],
    ['c_sort_chars',   '    chars.sort();'],
    ['c_init_visited', '    const visited = new Array(chars.length).fill(false);'],
    ['c_init_current', '    const current = [];'],
    ['c_call_backtrack','    backtrack(chars, visited, current, result);'],
    ['c_return_res',   '    return result;'],
    ['',               '}'],
    ['',               ''],
    ['',               'function main() {'],
    ['m_scanner',      '    // read tokens from stdin'],
    ['m_read_str',     '    const s = tokens[0];'],
    ['m_call_solver',  '    const result = findPermutations(s);'],
    ['m_print_count',  '    console.log(result.length);'],
    ['m_loop_print',   '    for (const p of result)'],
    ['m_print_elem',   '        console.log(p);'],
    ['m_done',         '}'],
    ['',               'main();']
  ],
  c: [
    ['',               '#include <stdio.h>'],
    ['',               '#include <string.h>'],
    ['',               '#include <stdlib.h>'],
    ['',               ''],
    ['',               '#define MAXN 8'],
    ['',               'char chars[MAXN];'],
    ['',               'int visited[MAXN];'],
    ['',               'char current[MAXN+1];'],
    ['',               'char result[5040][MAXN+1];'],
    ['',               'int resCount = 0;'],
    ['',               'int n;'],
    ['',               ''],
    ['',               'int cmp(const void* a, const void* b) { return *(char*)a - *(char*)b; }'],
    ['',               ''],
    ['c_bt_entry',     'void backtrack(int depth) {'],
    ['c_base_check',   '    if (depth == n) {'],
    ['c_save_sol',     '        strcpy(result[resCount++], current);'],
    ['c_base_ret',     '        return;'],
    ['',               '    }'],
    ['c_loop_chars',   '    for (int i = 0; i < n; i++) {'],
    ['c_chk_visited',  '        if (visited[i]) continue;'],
    ['c_cont_visited', '        // skip visited'],
    ['c_chk_dup',      '        if (i > 0 && chars[i] == chars[i-1] && !visited[i-1]) continue;'],
    ['c_cont_dup',     '        // skip duplicate'],
    ['c_mark_visited', '        visited[i] = 1;'],
    ['c_append_char',  '        current[depth] = chars[i];'],
    ['c_recurse',      '        backtrack(depth + 1);'],
    ['c_backtrack_pop','        current[depth] = 0;'],
    ['c_unmark_visited','        visited[i] = 0;'],
    ['',               '    }'],
    ['',               '}'],
    ['',               ''],
    ['c_entry',        'void findPermutations() {'],
    ['c_init_result',  '    resCount = 0;'],
    ['c_to_char_arr',  '    // chars already read'],
    ['c_sort_chars',   '    qsort(chars, n, sizeof(char), cmp);'],
    ['c_init_visited', '    memset(visited, 0, sizeof(visited));'],
    ['c_init_current', '    memset(current, 0, sizeof(current));'],
    ['c_call_backtrack','    backtrack(0);'],
    ['c_return_res',   '}'],
    ['',               ''],
    ['',               'int main() {'],
    ['m_scanner',      '    char s[MAXN+1];'],
    ['m_read_str',     '    scanf("%s", s); n = strlen(s);'],
    ['',               '    for (int i = 0; i < n; i++) chars[i] = s[i];'],
    ['m_call_solver',  '    findPermutations();'],
    ['m_print_count',  '    printf("%d\\n", resCount);'],
    ['m_loop_print',   '    for (int i = 0; i < resCount; i++)'],
    ['m_print_elem',   '        printf("%s\\n", result[i]);'],
    ['m_done',         '    return 0;'],
    ['',               '}']
  ]
};

const PSEUDOCODE = [
  'function findPermutations(s):',
  '    chars = toCharArray(s)',
  '    sort(chars)                     // lexicographic order & group duplicates',
  '    visited = [false] * n',
  '    current = []',
  '    result = []',
  '    backtrack(chars, visited, current, result)',
  '    return result',
  '',
  'function backtrack(chars, visited, current, result):',
  '    if len(current) == len(chars):  // Base case: full permutation built',
  '        result.add(copy of current)',
  '        return',
  '    for i from 0 to n-1:',
  '        if visited[i]: continue     // Already used in current branch',
  '        if i > 0 and chars[i] == chars[i-1] and not visited[i-1]:',
  '            continue                // Skip duplicate at same recursion level',
  '        visited[i] = true',
  '        current.push(chars[i])     // [+ PUSH] Choose',
  '        backtrack(chars, visited, current, result)',
  '        current.pop()              // [- POP] Unchoose (backtrack)',
  '        visited[i] = false'
];

// ─── Step Builder ─────────────────────────────────────────────────────────────
function buildSteps(inputStr) {
  const steps = [];
  const n = inputStr.length;
  const sortedChars = inputStr.split('').sort();
  const result = [];
  const visited = new Array(n).fill(false);
  const current = [];
  const callStack = [];

  function frm(title, rows) { return { title, rows }; }

  function stackSnap(extra = []) {
    const list = [frm('main()', [['s', `"${inputStr}"`]])];
    if (n > 0) list.push(frm('findPermutations()', [['n', String(n)]]));
    for (const f of callStack) list.push(frm(f.name, f.args));
    if (extra.length && list.length > 0) {
      const top = list[list.length - 1];
      top.rows = [...top.rows, ...extra];
    }
    return list;
  }

  function snap(extra = { activeI: -1, pruneStatus: null }) {
    return {
      charArr: [...sortedChars],
      visited: [...visited],
      current: [...current],
      result: [...result],
      activeI: extra.activeI !== undefined ? extra.activeI : -1,
      pruneStatus: extra.pruneStatus || null,
      n
    };
  }

  // ── Phase 1: main() ──────────────────────────────────────────────────────
  steps.push({ phase: 'input', code: 'm_scanner', badge: `Scanner sc = new Scanner(System.in); → Initializing input reader.`, vars: [frm('main()', [['s', '?']])], data: snap(), solCount: 0, activeI: -1, depth: 0 });
  steps.push({ phase: 'input', code: 'm_read_str', badge: `String s = sc.next(); → Read input string s = "${inputStr}".`, vars: [frm('main()', [['s', `"${inputStr}"`]])], data: snap(), solCount: 0, activeI: -1, depth: 0 });
  steps.push({ phase: 'input', code: 'm_call_solver', badge: `findPermutations("${inputStr}"); → Handing off to solver. Entering findPermutations().`, vars: [frm('main()', [['s', `"${inputStr}"`]])], data: snap(), solCount: 0, activeI: -1, depth: 0 });

  // ── Phase 2: findPermutations() init ──────────────────────────────────────
  steps.push({ phase: 'init', code: 'c_entry', badge: `static List<String> findPermutations("${inputStr}") → Function entry. Initializing data structures.`, vars: [frm('main()', [['s', `"${inputStr}"`]]), frm('findPermutations()', [['n', String(n)]])], data: snap(), solCount: 0, activeI: -1, depth: 0 });
  steps.push({ phase: 'init', code: 'c_init_result', badge: `List<String> result = new ArrayList<>(); → Output list [result] initialized. Will collect all unique permutations.`, vars: stackSnap(), data: snap(), solCount: 0, activeI: -1, depth: 0 });
  steps.push({ phase: 'init', code: 'c_to_char_arr', badge: `char[] chars = s.toCharArray(); → Converted "${inputStr}" to char array [${inputStr.split('').map(c => `'${c}'`).join(', ')}]. Length = ${n}.`, vars: stackSnap(), data: { ...snap(), charArr: inputStr.split('') }, solCount: 0, activeI: -1, depth: 0, unsorted: true });
  steps.push({ phase: 'init', code: 'c_sort_chars', badge: `Arrays.sort(chars); → Sorted to [${sortedChars.map(c => `'${c}'`).join(', ')}]. Lexicographic order groups duplicate chars adjacently for pruning.`, vars: stackSnap(), data: snap(), solCount: 0, activeI: -1, depth: 0 });
  steps.push({ phase: 'init', code: 'c_init_visited', badge: `boolean[] visited = new boolean[${n}]; → All ${n} flags initialized to [false]. Tracks which chars are used in current branch.`, vars: stackSnap(), data: snap(), solCount: 0, activeI: -1, depth: 0 });
  steps.push({ phase: 'init', code: 'c_init_current', badge: `StringBuilder current = new StringBuilder(); → Empty path buffer initialized. Will build permutations character by character.`, vars: stackSnap(), data: snap(), solCount: 0, activeI: -1, depth: 0 });
  steps.push({ phase: 'init', code: 'c_call_backtrack', badge: `backtrack(chars, visited, current, result); → Launching recursive backtracking from depth 0.`, vars: stackSnap(), data: snap(), solCount: 0, activeI: -1, depth: 0 });

  // ── Phase 3: Recursive backtracking ────────────────────────────────────────
  function simulate(depth) {
    callStack.push({ name: `backtrack(depth=${depth})`, args: [['depth', String(depth)], ['current.len', String(current.length)]] });

    steps.push({ phase: 'solver', code: 'c_bt_entry', badge: `backtrack(depth=${depth}) → Entering backtrack. current = "${current.join('')}", depth = ${depth} / ${n}.`, vars: stackSnap(), data: snap(), solCount: result.length, activeI: -1, depth });

    const isBase = current.length === n;
    steps.push({ phase: 'solver', code: 'c_base_check', badge: `if (current.length() == chars.length): ${current.length} == ${n} → ${isBase ? 'TRUE! Full permutation built.' : 'FALSE. Continue building.'}`, vars: stackSnap(), data: snap(), solCount: result.length, activeI: -1, depth });

    if (isBase) {
      const perm = current.join('');
      result.push(perm);
      steps.push({ phase: 'solver', code: 'c_save_sol', badge: `[MATCH] result.add("${perm}"); → Unique permutation #${result.length} saved: "${perm}".`, vars: stackSnap([['[MATCH]', `"${perm}"`]]), data: snap(), solCount: result.length, activeI: -1, depth, justFound: perm });
      steps.push({ phase: 'solver', code: 'c_base_ret', badge: `return; → Returning from base case at depth ${depth}. Backtracking to depth ${depth - 1}.`, vars: stackSnap(), data: snap(), solCount: result.length, activeI: -1, depth });
      callStack.pop();
      return;
    }

    steps.push({ phase: 'solver', code: 'c_loop_chars', badge: `for (int i = 0; i < ${n}; i++) → Starting loop over ${n} characters at depth ${depth}.`, vars: stackSnap(), data: snap(), solCount: result.length, activeI: -1, depth });

    for (let i = 0; i < n; i++) {
      steps.push({ phase: 'solver', code: 'c_loop_chars', badge: `for (int i = ${i}; ...) → Checking candidate chars[${i}] = '${sortedChars[i]}' at depth ${depth}.`, vars: stackSnap([['i', String(i)]]), data: snap({ activeI: i }), solCount: result.length, activeI: i, depth });

      // Check visited
      steps.push({ phase: 'solver', code: 'c_chk_visited', badge: `if (visited[${i}]) → visited[${i}] = ${visited[i]} → ${visited[i] ? '[SKIPPED: ALREADY USED] chars[' + i + '] is locked in current branch.' : 'Not visited. Checking duplicate rule.'}`, vars: stackSnap([['i', String(i)], ['visited[i]', String(visited[i])]]), data: snap({ activeI: i, pruneStatus: visited[i] ? 'visited' : null }), solCount: result.length, activeI: i, depth });
      if (visited[i]) {
        steps.push({ phase: 'solver', code: 'c_cont_visited', badge: `continue; → Skipped chars[${i}]='${sortedChars[i]}'. [SKIPPED: ALREADY USED] — this char is already on the current path.`, vars: stackSnap([['i', String(i)]]), data: snap({ activeI: i, pruneStatus: 'visited' }), solCount: result.length, activeI: i, depth });
        continue;
      }

      // Check duplicate
      const isDup = i > 0 && sortedChars[i] === sortedChars[i - 1] && !visited[i - 1];
      steps.push({ phase: 'solver', code: 'c_chk_dup', badge: `if (i>0 && chars[${i}]=='${sortedChars[i]}'==chars[${i-1}]=='${sortedChars[Math.max(0,i-1)]}' && !visited[${i-1}]) → ${i > 0 ? `${isDup ? '[PRUNED: DUPLICATE AT SAME LEVEL] Same char as previous unvisited sibling.' : '[VALID CANDIDATE] Duplicate check passed.'}` : '[VALID CANDIDATE] i=0, no previous to compare.'}`, vars: stackSnap([['i', String(i)], ['isDup', String(isDup)]]), data: snap({ activeI: i, pruneStatus: isDup ? 'duplicate' : 'valid' }), solCount: result.length, activeI: i, depth });
      if (isDup) {
        steps.push({ phase: 'solver', code: 'c_cont_dup', badge: `continue; → [PRUNED: DUPLICATE AT SAME LEVEL] Skipped chars[${i}]='${sortedChars[i]}'. prev sibling '${sortedChars[i-1]}' not yet visited — would create identical subtree.`, vars: stackSnap([['i', String(i)]]), data: snap({ activeI: i, pruneStatus: 'duplicate' }), solCount: result.length, activeI: i, depth });
        continue;
      }

      // Mark visited
      visited[i] = true;
      steps.push({ phase: 'solver', code: 'c_mark_visited', badge: `visited[${i}] = true; → Locked chars[${i}]='${sortedChars[i]}'. visited[${i}] flips [F] → [T].`, vars: stackSnap([['i', String(i)], ['visited[i]', 'true']]), data: snap({ activeI: i, pruneStatus: 'valid' }), solCount: result.length, activeI: i, depth });

      // Append char
      current.push(sortedChars[i]);
      steps.push({ phase: 'solver', code: 'c_append_char', badge: `current.append('${sortedChars[i]}'); [+ PUSH] → Appended '${sortedChars[i]}' to buffer. current = "${current.join('')}". length = ${current.length}.`, vars: stackSnap([['+ PUSH', `'${sortedChars[i]}'`]]), data: snap({ activeI: i, pruneStatus: 'valid' }), solCount: result.length, activeI: i, depth, justPushed: i });

      // Recurse
      steps.push({ phase: 'solver', code: 'c_recurse', badge: `backtrack(chars, visited, "${current.join('')}", result); → Recursing to depth ${depth + 1}.`, vars: stackSnap([['i', String(i)]]), data: snap({ activeI: i, pruneStatus: 'valid' }), solCount: result.length, activeI: i, depth });
      simulate(depth + 1);

      // Pop
      const popped = current.pop();
      steps.push({ phase: 'solver', code: 'c_backtrack_pop', badge: `current.deleteCharAt(...); [- POP] → Backtracked. Removed '${popped}'. current = "${current.join('')}". length = ${current.length}.`, vars: stackSnap([['- POP', `'${popped}'`]]), data: snap({ activeI: i }), solCount: result.length, activeI: i, depth, justPopped: i });

      // Unmark
      visited[i] = false;
      steps.push({ phase: 'solver', code: 'c_unmark_visited', badge: `visited[${i}] = false; → Unlocked chars[${i}]='${sortedChars[i]}'. visited[${i}] flips [T] → [F]. Ready for next sibling.`, vars: stackSnap([['i', String(i)], ['visited[i]', 'false']]), data: snap({ activeI: i }), solCount: result.length, activeI: i, depth });
    }

    callStack.pop();
  }

  simulate(0);

  // Return from findPermutations
  steps.push({ phase: 'solver', code: 'c_return_res', badge: `return result; → Backtracking complete. Returning ${result.length} unique permutation(s) to main().`, vars: [frm('main()', [['s', `"${inputStr}"`]]), frm('findPermutations()', [['result.size', String(result.length)]])], data: snap(), solCount: result.length, activeI: -1, depth: 0 });

  // Done phase
  steps.push({ phase: 'done', code: 'm_print_count', badge: `System.out.println(result.size()); → Printing total unique permutations: ${result.length}.`, vars: [frm('main()', [['result.size()', String(result.length)]])], data: snap(), solCount: result.length, activeI: -1, depth: 0 });

  for (let pi = 0; pi < result.length; pi++) {
    steps.push({ phase: 'done', code: 'm_loop_print', badge: `for (int i = ${pi}; ...) → Printing permutation #${pi + 1}: "${result[pi]}".`, vars: [frm('main()', [['i', String(pi)]])], data: snap(), solCount: result.length, activeI: -1, depth: 0, selectedPerm: pi });
    steps.push({ phase: 'done', code: 'm_print_elem', badge: `System.out.println(result.get(${pi})); → Output: "${result[pi]}"`, vars: [frm('main()', [['i', String(pi)], ['output', `"${result[pi]}"`]])], data: snap(), solCount: result.length, activeI: -1, depth: 0, selectedPerm: pi });
  }

  steps.push({ phase: 'done', code: 'm_done', badge: `Program execution complete. Found ${result.length} unique permutation(s) of "${inputStr}".`, vars: [frm('main()', [['status', 'finished'], ['total', String(result.length)]])], data: snap(), solCount: result.length, activeI: -1, depth: 0 });

  return steps;
}

// ─── Reactive State ───────────────────────────────────────────────────────────
const DEFAULT_STR = 'ABA';
const inputStr = ref(DEFAULT_STR);
const lang = ref('java');
const speed = ref(650);
const si = ref(0);
const playing = ref(false);
const vizHeight = ref(350);
const tableHeight = ref(60);
const leftWidth = ref(52);
const rightTab = ref('code');
const selectedPermIdx = ref(-1);

const stepsData = reactive({ steps: buildSteps(DEFAULT_STR) });
const steps = computed(() => stepsData.steps);
const s = computed(() => steps.value[Math.max(0, Math.min(si.value, steps.value.length - 1))] || {});
const codeLines = computed(() => CODES[lang.value] || []);

let playTimer = null;

function onChipsWheel(e) { if (e.currentTarget) e.currentTarget.scrollLeft += e.deltaY; }

function applyInput() {
  const str = inputStr.value.trim().toUpperCase();
  if (!str || str.length < 1 || str.length > 6) {
    alert('Please enter a string of 1-6 letters (A-Z).');
    inputStr.value = DEFAULT_STR;
    return;
  }
  if (!/^[A-Z]+$/.test(str)) {
    alert('Only uppercase letters A-Z are allowed.');
    return;
  }
  inputStr.value = str;
  playing.value = false;
  selectedPermIdx.value = -1;
  stepsData.steps = buildSteps(str);
  si.value = 0;
  if (typeof window !== 'undefined') window.scrollTo(0, 0);
}

function loadPreset(p) {
  if (p === 'aba') inputStr.value = 'ABA';
  else if (p === 'abc') inputStr.value = 'ABC';
  else if (p === 'aaa') inputStr.value = 'AAA';
  else if (p === 'aabc') inputStr.value = 'AABC';
  applyInput();
}

function stepBy(d) { selectedPermIdx.value = -1; si.value = Math.max(0, Math.min(steps.value.length - 1, si.value + d)); }

function togglePlay() {
  selectedPermIdx.value = -1;
  const next = !playing.value;
  if (next && si.value >= steps.value.length - 1) si.value = 0;
  playing.value = next;
}

function tick() {
  clearTimeout(playTimer);
  if (!playing.value) return;
  if (si.value >= steps.value.length - 1) { playing.value = false; return; }
  playTimer = setTimeout(() => { si.value = Math.min(steps.value.length - 1, si.value + 1); tick(); }, 2100 - speed.value);
}

watch(playing, v => { if (v) tick(); else clearTimeout(playTimer); });

const codeScrollRef = ref(null);

function scrollActiveCodeLine() {
  nextTick(() => {
    const container = codeScrollRef.value;
    if (!container) return;
    const activeEl = container.querySelector('.ll-hl');
    if (!activeEl) return;
    const contRect = container.getBoundingClientRect();
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
watch(rightTab, v => { if (v === 'code') scrollActiveCodeLine(); });

function onKeydown(e) {
  const tag = e.target.tagName;
  if (tag === 'INPUT' || tag === 'SELECT' || tag === 'TEXTAREA') return;
  if (e.key === 'ArrowRight') stepBy(1);
  if (e.key === 'ArrowLeft') stepBy(-1);
  if (e.key === ' ') { e.preventDefault(); togglePlay(); }
}

function viewPerm(idx) { selectedPermIdx.value = idx; playing.value = false; }

// Computed display values
const displayData = computed(() => s.value.data || { charArr: [], visited: [], current: [], result: [], activeI: -1, pruneStatus: null, n: 0 });
const displayCurrent = computed(() => {
  if (selectedPermIdx.value >= 0 && s.value.data && s.value.data.result && s.value.data.result[selectedPermIdx.value]) {
    return s.value.data.result[selectedPermIdx.value].split('');
  }
  return displayData.value.current || [];
});
const displayResult = computed(() => displayData.value.result || []);
const displayVisited = computed(() => displayData.value.visited || []);
const displayCharArr = computed(() => displayData.value.charArr || []);
const displayN = computed(() => displayData.value.n || inputStr.value.length);

const pruneStatusLabel = computed(() => {
  const st = displayData.value.pruneStatus;
  const i = s.value.activeI;
  if (i === undefined || i < 0) return null;
  return st;
});

// Memory metrics
const memChars = computed(() => `${displayN.value} x 2 B = ${displayN.value * 2} B`);
const memVisited = computed(() => `${displayN.value} x 1 B = ${displayN.value} B`);
const memBuffer = computed(() => `${displayCurrent.value.length} / ${displayN.value}`);
const memResults = computed(() => `${s.value.solCount || 0} unique permutations`);
const stackDepthStr = computed(() => `${s.value.depth || 0} / ${displayN.value} (max)`);

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
  rsz.addEventListener('mousedown', onDown); document.addEventListener('mousemove', onMove); document.addEventListener('mouseup', onUp);
  return () => { rsz.removeEventListener('mousedown', onDown); document.removeEventListener('mousemove', onMove); document.removeEventListener('mouseup', onUp); };
}

function initVResizer(elRef, valueRef, minH, maxH) {
  const rsz = elRef.value;
  if (!rsz) return;
  let dragging = false, startY = 0, startH = 0;
  const onDown = e => { dragging = true; startY = e.clientY; startH = valueRef.value; rsz.classList.add('drag'); document.body.style.userSelect = 'none'; e.preventDefault(); };
  const onMove = e => { if (!dragging) return; valueRef.value = Math.max(minH, Math.min(maxH, startH + (e.clientY - startY))); };
  const onUp = () => { if (!dragging) return; dragging = false; rsz.classList.remove('drag'); document.body.style.userSelect = ''; };
  rsz.addEventListener('mousedown', onDown); document.addEventListener('mousemove', onMove); document.addEventListener('mouseup', onUp);
  return () => { rsz.removeEventListener('mousedown', onDown); document.removeEventListener('mousemove', onMove); document.removeEventListener('mouseup', onUp); };
}

let cleanupFns = [];
onMounted(() => {
  document.addEventListener('keydown', onKeydown);
  cleanupFns.push(initHResizer());
  cleanupFns.push(initVResizer(vizResizerRef, vizHeight, 200, 650));
  cleanupFns.push(initVResizer(tableResizerRef, tableHeight, 50, 200));
  if (typeof window !== 'undefined') window.scrollTo(0, 0);
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
          <!-- Control Panel Toolbar -->
          <div class="ll-toolbar">
            <div class="ll-input-group">
              <label>String s:</label>
              <input type="text" maxlength="6" v-model="inputStr" class="ll-text-input dp-str-input" @keyup.enter="applyInput" placeholder="e.g. ABA" />
            </div>
            <div class="ll-preset-group">
              <button class="ll-preset-btn" @click="loadPreset('aba')" title="ABA — 3 unique perms">ABA</button>
              <button class="ll-preset-btn" @click="loadPreset('abc')" title="ABC — 6 unique perms">ABC</button>
              <button class="ll-preset-btn" @click="loadPreset('aaa')" title="AAA — 1 unique perm">AAA</button>
              <button class="ll-preset-btn" @click="loadPreset('aabc')" title="AABC — 12 unique perms">AABC</button>
            </div>
            <button class="ll-viz-btn" @click="applyInput">&#9654; Visualize</button>
            <div class="ll-nav-controls">
              <button class="ll-nav-btn" title="First step" @click="stepBy(-steps.length)">&#171;</button>
              <button class="ll-nav-btn" title="Previous step" @click="stepBy(-1)">&#8249; Prev</button>
              <button class="ll-play-btn" @click="togglePlay">{{ playing ? '\u23F8 Pause' : '\u25B6 Play' }}</button>
              <button class="ll-nav-btn" title="Next step" @click="stepBy(1)">Next &#8250;</button>
              <button class="ll-nav-btn" title="Last step" @click="stepBy(steps.length)">&#187;</button>
            </div>
          </div>

          <div class="ll-main" ref="mainRef">
            <!-- Left Visualization Column -->
            <div class="ll-left-col" ref="leftColRef" :style="{ width: leftWidth + '%' }">
              <div class="ll-viz-wrap" :style="{ height: vizHeight + 'px' }">
                <div class="ll-perm-area">
                  <!-- Real-time Stats Chips -->
                  <div class="ll-ptrs ll-ptrs-compact" @wheel.passive="onChipsWheel">
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">n</span><b class="ll-c-blue">{{ displayN }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">i</span><b class="ll-c-purple">{{ s.activeI !== undefined && s.activeI >= 0 ? s.activeI : '-' }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">depth</span><b class="ll-c-orange">{{ s.depth !== undefined ? s.depth : 0 }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">phase</span><b class="ll-c-orange">{{ s.phase || 'input' }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">buffer.len</span><b class="ll-c-purple">{{ displayCurrent.length }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">results</span><b class="ll-c-green">{{ s.solCount || 0 }}</b></span>
                  </div>

                  <!-- BOARD CONTAINER -->
                  <div class="ll-board-container">

                    <!-- Tier 1: char[] chars -->
                    <div class="gp-tier-title">Tier 1 &mdash; Sorted Char Array <code>char[] chars</code></div>
                    <div class="ll-board-frame dp-chars-frame">
                      <div class="dp-char-row">
                        <div
                          v-for="(ch, idx) in displayCharArr"
                          :key="'ch-' + idx"
                          class="dp-char-cell"
                          :class="{
                            'dp-char-active': idx === s.activeI,
                            'dp-char-visited': displayVisited[idx],
                            'dp-char-just-pushed': s.justPushed === idx,
                            'dp-char-prune': idx === s.activeI && (s.data && s.data.pruneStatus === 'visited' || s.data && s.data.pruneStatus === 'duplicate'),
                            'dp-char-valid': idx === s.activeI && s.data && s.data.pruneStatus === 'valid'
                          }"
                          :title="`chars[${idx}] = '${ch}'`"
                        >
                          <span class="dp-char-idx">[{{ idx }}]</span>
                          <span class="dp-char-val">'{{ ch }}'</span>
                          <span v-if="displayVisited[idx]" class="dp-char-lock">[T]</span>
                        </div>
                        <div v-if="displayCharArr.length === 0" class="dp-empty-msg">Awaiting input...</div>
                      </div>
                      <div v-if="s.unsorted && displayCharArr.length > 0" class="dp-sort-note">Unsorted — Arrays.sort() not yet called</div>
                    </div>

                    <!-- Tier 2: boolean[] visited -->
                    <div class="gp-tier-title">Tier 2 &mdash; Visited Flags <code>boolean[] visited</code></div>
                    <div class="ll-board-frame dp-visited-frame">
                      <div class="dp-char-row">
                        <div
                          v-for="(flag, idx) in displayVisited"
                          :key="'vis-' + idx"
                          class="dp-vis-cell"
                          :class="{
                            'dp-vis-true': flag,
                            'dp-vis-false': !flag,
                            'dp-vis-active': idx === s.activeI
                          }"
                          :title="`visited[${idx}] = ${flag}`"
                        >
                          <span class="dp-vis-idx">[{{ idx }}]</span>
                          <span class="dp-vis-val">{{ flag ? '[T]' : '[F]' }}</span>
                        </div>
                        <div v-if="displayVisited.length === 0" class="dp-empty-msg">Not allocated yet.</div>
                      </div>
                    </div>

                    <!-- Tier 3: Duplicate Pruning Inspector -->
                    <div class="gp-tier-title">Tier 3 &mdash; Duplicate Pruning Inspector</div>
                    <div class="dp-prune-panel" :class="{
                      'dp-prune-valid': pruneStatusLabel === 'valid',
                      'dp-prune-visited': pruneStatusLabel === 'visited',
                      'dp-prune-dup': pruneStatusLabel === 'duplicate'
                    }">
                      <template v-if="pruneStatusLabel === 'visited'">
                        <span class="dp-prune-icon">!</span>
                        <div class="dp-prune-content">
                          <div class="dp-prune-title">[SKIPPED: ALREADY USED]</div>
                          <div class="dp-prune-detail">visited[{{ s.activeI }}] = true &mdash; chars[{{ s.activeI }}] = '{{ displayCharArr[s.activeI] }}' is locked in the current branch.</div>
                        </div>
                      </template>
                      <template v-else-if="pruneStatusLabel === 'duplicate'">
                        <span class="dp-prune-icon">X</span>
                        <div class="dp-prune-content">
                          <div class="dp-prune-title">[PRUNED: DUPLICATE AT SAME LEVEL]</div>
                          <div class="dp-prune-detail">chars[{{ s.activeI }}]='{{ displayCharArr[s.activeI] }}' == chars[{{ Math.max(0, s.activeI - 1) }}]='{{ displayCharArr[Math.max(0, (s.activeI||1) - 1)] }}' and visited[{{ Math.max(0, (s.activeI||1) - 1) }}]=[F] &mdash; would generate duplicate subtree.</div>
                        </div>
                      </template>
                      <template v-else-if="pruneStatusLabel === 'valid'">
                        <span class="dp-prune-icon">V</span>
                        <div class="dp-prune-content">
                          <div class="dp-prune-title">[VALID CANDIDATE]</div>
                          <div class="dp-prune-detail">chars[{{ s.activeI }}]='{{ displayCharArr[s.activeI] }}' passes all checks. Proceeding to choose this character.</div>
                        </div>
                      </template>
                      <template v-else>
                        <span class="dp-prune-icon">-</span>
                        <div class="dp-prune-content">
                          <div class="dp-prune-title">Waiting for loop iteration</div>
                          <div class="dp-prune-detail">Condition: if (i&gt;0 &amp;&amp; chars[i]==chars[i-1] &amp;&amp; !visited[i-1]) → skip duplicate sibling.</div>
                        </div>
                      </template>
                    </div>

                    <!-- Tier 4: StringBuilder current (buffer tray) -->
                    <div class="gp-tier-title">Tier 4 &mdash; Path Buffer <code>StringBuilder current</code></div>
                    <div class="gp-buf-panel">
                      <div class="gp-buf-meta">
                        <span>length = <b class="ll-c-purple">{{ displayCurrent.length }}</b></span>
                        <span style="margin-left:8px">target = <b class="ll-c-blue">{{ displayN }}</b></span>
                      </div>
                      <div class="gp-buf-slots">
                        <template v-for="idx in displayN" :key="'buf-' + idx">
                          <div
                            class="gp-buf-slot"
                            :class="{
                              'gp-buf-filled': idx - 1 < displayCurrent.length,
                              'gp-buf-empty': idx - 1 >= displayCurrent.length,
                              'gp-buf-just-pushed': s.code === 'c_append_char' && idx - 1 === displayCurrent.length - 1,
                              'gp-buf-just-popped': s.code === 'c_backtrack_pop' && idx - 1 === displayCurrent.length,
                              'dp-buf-found': s.justFound && idx - 1 < displayCurrent.length
                            }"
                          >
                            <span class="gp-buf-idx">[{{ idx - 1 }}]</span>
                            <span class="gp-buf-val">{{ idx - 1 < displayCurrent.length ? `'${displayCurrent[idx - 1]}'` : '\u00a0' }}</span>
                            <span v-if="s.code === 'c_append_char' && idx - 1 === displayCurrent.length - 1" class="gp-push-badge">+ PUSH</span>
                            <span v-if="s.code === 'c_backtrack_pop' && idx - 1 === displayCurrent.length" class="gp-pop-badge">- POP</span>
                          </div>
                        </template>
                        <div v-if="displayN === 0" class="gp-buf-empty-msg">&lang; empty &rang;</div>
                      </div>
                      <div v-if="displayCurrent.length === displayN && displayN > 0" class="dp-buf-complete">
                        "{{ displayCurrent.join('') }}" &mdash; [LEXICOGRAPHIC] Full permutation built.
                      </div>
                    </div>

                    <!-- Tier 5: List<String> result -->
                    <div class="gp-tier-title">Tier 5 &mdash; Collected Permutations <code>List&lt;String&gt; result</code></div>
                    <div class="gp-solutions-panel">
                      <div v-if="displayResult.length === 0" class="gp-no-solutions">No permutations collected yet.</div>
                      <template v-else>
                        <button
                          v-for="(perm, idx) in displayResult"
                          :key="'perm-' + idx"
                          class="gp-sol-btn dp-perm-btn"
                          :class="{ active: selectedPermIdx === idx || s.justFound === perm }"
                          @click="viewPerm(idx)"
                          :title="`Permutation #${idx + 1}: &quot;${perm}&quot;`"
                        >
                          Perm #{{ idx + 1 }}: "{{ perm }}"
                        </button>
                      </template>
                    </div>

                    <!-- Tier 6: Memory Metrics Readout -->
                    <!-- <div class="gp-tier-title">Tier 6 &mdash; Memory Metrics</div>
                    <div class="gp-mem-bar">
                      <span class="gp-mem-item"><span class="gp-mem-label">Character Array:</span> <code>{{ memChars }}</code></span>
                      <span class="gp-mem-sep">|</span>
                      <span class="gp-mem-item"><span class="gp-mem-label">Visited Flags:</span> <code>{{ memVisited }}</code></span>
                      <span class="gp-mem-sep">|</span>
                      <span class="gp-mem-item"><span class="gp-mem-label">Buffer:</span> <code>{{ memBuffer }}</code></span>
                      <span class="gp-mem-sep">|</span>
                      <span class="gp-mem-item"><span class="gp-mem-label">Stack Depth:</span> <code>{{ stackDepthStr }}</code></span>
                      <span class="gp-mem-sep">|</span>
                      <span class="gp-mem-item"><span class="gp-mem-label">Perms Found:</span> <code>{{ memResults }}</code></span>
                    </div> -->

                  </div>
                  <!-- END ll-board-container -->
                </div>
              </div>

              <!-- Vertical Resizer -->
              <div class="ll-vresizer" ref="vizResizerRef"></div>

              <!-- Color Legend -->
              <div class="ll-legend">
                <span class="ll-leg"><span class="ll-legdot dp-legdot-visited"></span>VISITED</span>
                <span class="ll-leg"><span class="ll-legdot dp-legdot-valid"></span>VALID</span>
                <span class="ll-leg"><span class="ll-legdot dp-legdot-dup"></span>DUPLICATE PRUNED</span>
                <span class="ll-leg"><span class="ll-legdot dp-legdot-match"></span>MATCH</span>
              </div>

              <!-- Recursion Call Stack Frame Area -->
              <div class="ll-table-area" :style="{ height: tableHeight + 'px' }">
                <div class="ll-table-title">Recursion Call Stack &mdash; innermost = current depth</div>
                <div class="ll-stack-line">
                  <template v-if="s.vars && s.vars.length">
                    <div v-for="(f, depth) in s.vars" :key="depth" class="ll-frame" :class="{ 'll-frame-cur': depth === s.vars.length - 1 }" :style="{ marginLeft: depth * 14 + 'px' }">
                      {{ f.title }}(<span v-for="(r, idx) in f.rows" :key="idx"><span v-if="idx > 0">, </span><span class="ll-fname">{{ r[0] }}</span>=<span :class="depth === s.vars.length - 1 ? 'll-c-orange' : 'll-c-blue'" style="font-weight:700">{{ r[1] }}</span></span>)<span v-if="depth === s.vars.length - 1" class="ll-now"> &#9668; active</span>
                    </div>
                  </template>
                  <template v-else>&mdash;</template>
                </div>
              </div>

              <!-- Vertical Resizer -->
              <div class="ll-vresizer" ref="tableResizerRef"></div>

              <!-- Step Badge Banner -->
              <div class="ll-badge-wrap">
                <div class="ll-badge" :class="{
                  'll-badge-error': s.badge && (s.badge.includes('[PRUNED') || s.badge.includes('[SKIPPED') || s.badge.includes('- POP') || s.badge.includes('FALSE')),
                  'll-badge-success': s.badge && (s.badge.includes('[MATCH]') || s.badge.includes('complete') || s.badge.includes('TRUE') || s.badge.includes('[VALID CANDIDATE]') || s.badge.includes('[LEXICOGRAPHIC]'))
                }">
                  {{ s.badge || 'Ready to run Distinct Permutations backtracking algorithm.' }}
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

                <div v-if="rightTab === 'code'" class="ll-code-scroll" ref="codeScrollRef">
                  <pre class="ll-pre"><span v-for="(line, idx) in codeLines" :key="idx" class="ll-codeline" :class="{ 'll-hl': line[0] && line[0] === s.code }">{{ line[1] === '' ? ' ' : line[1] }}</span></pre>
                </div>

                <div v-else-if="rightTab === 'pseudo'" class="ll-code-scroll">
                  <pre class="ll-pre"><span v-for="(line, idx) in PSEUDOCODE" :key="idx" class="ll-codeline">{{ line }}</span></pre>
                </div>

                <div v-else class="ll-info-scroll">
                  <h3 class="ll-cx-heading">Distinct Permutations of a String &mdash; Complexity Analysis</h3>
                  <p class="ll-cx-intro">
                    Generates all unique permutations of a string of length <code>N</code> using backtracking with sorting
                    and a <code>visited[]</code> array. Duplicates are pruned at the same recursion level by checking
                    <code>chars[i] == chars[i-1] &amp;&amp; !visited[i-1]</code>.
                    The output is produced in lexicographic order because the input is sorted first.
                  </p>

                  <h4 class="ll-cx-sub">Step-by-step Breakdown</h4>
                  <table class="ll-complexity-table">
                    <thead><tr><th>Operation</th><th>Time</th><th>Space</th><th>Reason</th></tr></thead>
                    <tbody>
                      <tr><td>Sort chars[]</td><td class="ll-cx-good">O(N log N)</td><td class="ll-cx-good">O(1)</td><td>In-place sort; enables adjacent duplicate detection</td></tr>
                      <tr><td>Backtracking DFS nodes</td><td class="ll-cx-mid">O(N!)</td><td class="ll-cx-good">O(N)</td><td>At most N! leaf nodes; recursion stack depth = N</td></tr>
                      <tr><td>Duplicate pruning</td><td class="ll-cx-good">Reduces by factor k!</td><td class="ll-cx-good">O(1)</td><td>For k identical chars, k! duplicate subtrees are pruned</td></tr>
                      <tr><td>Storing results</td><td class="ll-cx-mid">O(N!/k1!k2!...)</td><td class="ll-cx-mid">O(N x P)</td><td>P = number of unique perms; each stored as N-length string</td></tr>
                    </tbody>
                  </table>

                  <h4 class="ll-cx-sub">Overall Complexity</h4>
                  <div class="ll-cx-summary-grid">
                    <div class="ll-cx-card ll-cx-card-mid">
                      <div class="ll-cx-card-label">Time</div>
                      <div class="ll-cx-card-val">O(N!)</div>
                      <div class="ll-cx-card-note">Upper bound; pruning reduces by duplicate factor</div>
                    </div>
                    <div class="ll-cx-card ll-cx-card-good">
                      <div class="ll-cx-card-label">Aux Space</div>
                      <div class="ll-cx-card-val">O(N)</div>
                      <div class="ll-cx-card-note">visited[] array + recursion stack depth N</div>
                    </div>
                    <div class="ll-cx-card ll-cx-card-mid">
                      <div class="ll-cx-card-label">Unique Perms</div>
                      <div class="ll-cx-card-val">N!/k!</div>
                      <div class="ll-cx-card-note">Divided by factorial of each duplicate group</div>
                    </div>
                  </div>

                  <h4 class="ll-cx-sub">Preset Examples</h4>
                  <table class="ll-complexity-table">
                    <thead><tr><th>Input</th><th>Unique Perms</th><th>Formula</th></tr></thead>
                    <tbody>
                      <tr><td><code>"ABC"</code></td><td class="ll-cx-good">6</td><td>3! / 1 = 6</td></tr>
                      <tr><td><code>"ABA"</code></td><td class="ll-cx-mid">3</td><td>3! / 2! = 3</td></tr>
                      <tr><td><code>"AAA"</code></td><td class="ll-cx-good">1</td><td>3! / 3! = 1</td></tr>
                      <tr><td><code>"AABC"</code></td><td class="ll-cx-mid">12</td><td>4! / 2! = 12</td></tr>
                    </tbody>
                  </table>

                  <div class="ll-note">
                    <strong>Pruning Key Insight:</strong> After sorting, identical characters are adjacent.
                    The condition <code>chars[i] == chars[i-1] &amp;&amp; !visited[i-1]</code> detects that we would
                    place the same character at the same position as a previously tried sibling that was
                    already unvisited at this level — producing an identical subtree. Skipping it is safe and
                    ensures each unique permutation is generated exactly once, in lexicographic order.
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
  --amber: #f59e0b; --amber-light: #fffbeb;
  --shadow-sm: 0 1px 3px rgba(0,0,0,.08), 0 1px 2px rgba(0,0,0,.04);
  --radius: 8px; --radius-sm: 6px;
  background: var(--bg); color: var(--text);
  font-family: 'Segoe UI', system-ui, sans-serif; font-size: 12.5px;
  display: flex; flex-direction: column; height: 74vh; overflow: hidden; width: 100%;
}

@keyframes ll-pop { 0% { transform: scale(0.6); opacity: 0; } 70% { transform: scale(1.15); opacity: 1; } 100% { transform: scale(1); opacity: 1; } }
@keyframes dp-flash-found { 0% { background: #fef9c3; } 50% { background: #22c55e; } 100% { background: #dcfce7; } }
@keyframes dp-pulse-pop { 0% { opacity: 1; } 50% { opacity: 0.4; background: #fee2e2; } 100% { opacity: 1; } }
@keyframes dp-flash-prune { 0% { background: #fff7ed; } 50% { background: #fef3c7; } 100% { background: #fff7ed; } }

.slide-wrapper { margin-top: -10px; margin-left: -30px; width: 107%; max-height: 100%; font-size: 0.8rem; font-weight: 400; }
.slide-body { display: flex; flex-direction: column; border-radius: 4px; height: 100%; }
.navbar { display: flex; flex-direction: row; justify-content: space-between; align-items: center; gap: 0.75rem; padding: 0 10px; background-color: #ffffff; position: fixed; width: 94.7%; z-index: 50; }
.navbar > img { height: 30px; }
.navbar-title { margin: 0; font-size: 1.35rem; font-weight: 700; background-color: #ef5050; color: #ffffff; width: 80%; padding: 2px 10px; margin-left: -10px; border-radius: 5px; }
.row-main { width: 100%; height: 90%; margin-top: 36px; overflow-x: auto; overflow-y: hidden; }

.ll-toolbar { margin-top: 4px; display: flex; align-items: center; gap: 6px; padding: 6.5px 12px; background: var(--surface); border-bottom: 1px solid var(--border); flex-shrink: 0; flex-wrap: wrap; box-shadow: var(--shadow-sm); }
.ll-input-group { display: flex; align-items: center; gap: 4px; }
.ll-input-group label { font-size: 11px; color: var(--muted); font-weight: 700; }
.ll-text-input { background: var(--surface); border: 1px solid var(--border2); color: var(--text); border-radius: var(--radius-sm); padding: 3px 6px; font-size: 11.5px; font-family: monospace; }
.ll-text-input:focus { outline: none; border-color: var(--coral); box-shadow: 0 0 0 3px rgba(240,77,77,.1); }
.dp-str-input { width: 80px; font-size: 14px; font-weight: 800; text-transform: uppercase; text-align: center; letter-spacing: 0.1em; font-family: 'Cascadia Code', 'Fira Code', monospace; }
.ll-preset-group { display: flex; gap: 3px; }
.ll-preset-btn { background: var(--surface2); border: 1px solid var(--border2); color: var(--text2); padding: 3px 6px; border-radius: 4px; font-size: 10.5px; cursor: pointer; transition: all .12s; font-family: monospace; font-weight: 700; }
.ll-preset-btn:hover { background: var(--coral-light); border-color: var(--coral); color: var(--coral-dark); }
.ll-viz-btn { background: var(--coral); color: #fff; border: none; padding: 5px 12px; border-radius: var(--radius-sm); cursor: pointer; font-size: 11.5px; font-weight: 600; box-shadow: var(--shadow-sm); transition: filter .15s; }
.ll-viz-btn:hover { filter: brightness(1.08); }
.ll-nav-controls { display: flex; margin-left: auto; align-items: center; gap: 4px; flex-shrink: 0; flex-wrap: wrap; }
.ll-nav-btn { background: var(--surface2); border: 1px solid var(--border2); color: var(--text2); padding: 4px 9px; border-radius: var(--radius-sm); cursor: pointer; font-size: 11px; font-weight: 500; transition: all .15s; white-space: nowrap; }
.ll-nav-btn:hover { background: var(--surface); border-color: var(--coral); color: var(--coral); }
.ll-play-btn { background: var(--blue-light); border: 1px solid var(--blue); color: var(--blue); min-width: 68px; font-weight: 600; padding: 4px 9px; border-radius: var(--radius-sm); cursor: pointer; font-size: 11px; transition: all .15s; }
.ll-play-btn:hover { background: var(--blue); color: #fff; }

.ll-main { display: flex; flex: 1; overflow: hidden; position: relative; }
.ll-left-col { display: flex; flex-direction: column; overflow: hidden; min-width: 220px; max-width: 75%; }
.ll-resizer { width: 5px; cursor: col-resize; background: var(--border); flex-shrink: 0; transition: background .15s; position: relative; z-index: 20; }
.ll-resizer:hover, .ll-resizer.drag { background: var(--coral); }
.ll-right-col { display: flex; flex-direction: column; flex: 1; overflow: hidden; min-width: 0; height: 100%; }

.ll-viz-wrap { flex-shrink: 0; background: var(--surface); border-bottom: 1px solid var(--border); position: relative; overflow-x: auto; overflow-y: auto; }
.ll-perm-area { display: flex; flex-direction: column; align-items: stretch; min-height: 100%; width: 100%; min-width: 0; box-sizing: border-box; }
.ll-ptrs { display: flex; gap: 8px; flex-wrap: wrap; padding: 4px 14px; min-height: 28px; width: 100%; box-sizing: border-box; min-width: 0; align-items: center; }
.ll-ptrs-compact { flex-wrap: nowrap; gap: 6px; padding: 3px 14px 5px; min-height: 0; overflow-x: auto; overflow-y: hidden; }
.ll-ptr-chip-inline { display: inline-flex; align-items: center; gap: 4px; background: var(--surface2); border: 1px solid var(--border); border-radius: 6px; padding: 2px 8px; font-size: 11px; font-family: monospace; white-space: nowrap; flex-shrink: 0; line-height: 1.4; }
.ll-chip-label { color: var(--muted); font-weight: 500; margin-right: 2px; }
.ll-c-blue { color: var(--blue); } .ll-c-orange { color: var(--orange); } .ll-c-green { color: var(--green); } .ll-c-purple { color: var(--purple); }

/* Board Container */
.ll-board-container { display: flex; flex-direction: column; align-items: flex-start; padding: 6px 14px 10px; gap: 5px; min-width: 0; }
.gp-tier-title { font-size: 10px; font-weight: 700; text-transform: uppercase; letter-spacing: 0.04em; color: var(--muted); margin-top: 4px; margin-bottom: 2px; font-family: 'Consolas', monospace; border-left: 3px solid var(--coral); padding-left: 6px; }

.ll-board-frame { display: flex; flex-direction: column; background: #f8fafc; border: 1px solid var(--border); border-radius: var(--radius); padding: 8px 12px; box-shadow: var(--shadow-sm); }

/* Tier 1 & 2: Char / Visited cells */
.dp-chars-frame { gap: 6px; }
.dp-visited-frame { gap: 4px; }
.dp-char-row { display: flex; gap: 4px; flex-wrap: wrap; align-items: center; }
.dp-char-cell {
  display: flex; flex-direction: column; align-items: center; justify-content: center;
  width: 48px; min-height: 52px; border-radius: 6px; border: 1.5px solid var(--border2);
  background: var(--surface); position: relative; transition: all .18s; cursor: default;
  font-family: monospace; gap: 2px;
}
.dp-char-idx { font-size: 9px; color: var(--muted); font-weight: 700; }
.dp-char-val { font-size: 18px; font-weight: 800; color: var(--text); line-height: 1; }
.dp-char-lock { font-size: 8px; font-weight: 800; font-family: monospace; background: #fee2e2; color: #b91c1c; border: 1px solid #fca5a5; padding: 1px 4px; border-radius: 3px; }
.dp-char-active { background: #fffbeb !important; border: 2px solid var(--orange) !important; transform: scale(1.08); z-index: 10; box-shadow: 0 0 10px rgba(249,115,22,.4); }
.dp-char-visited { background: #fef2f2 !important; border: 1.5px solid #fca5a5 !important; opacity: 0.75; }
.dp-char-prune { background: #fff7ed !important; border: 2px dashed var(--amber) !important; animation: dp-flash-prune 0.5s ease; }
.dp-char-valid { background: #f0fdf4 !important; border: 2px solid var(--green) !important; }
.dp-char-just-pushed { background: #dcfce7 !important; border: 2px solid #10b981 !important; animation: ll-pop 0.25s ease; }
.dp-sort-note { font-size: 9px; color: var(--amber); font-style: italic; font-family: monospace; margin-top: 4px; padding: 2px 6px; background: #fffbeb; border: 1px solid #fcd34d; border-radius: 4px; }

.dp-vis-cell {
  display: flex; flex-direction: column; align-items: center; justify-content: center;
  width: 48px; min-height: 40px; border-radius: 6px; border: 1.5px solid var(--border2);
  background: var(--surface); font-family: monospace; transition: all .18s; cursor: default;
}
.dp-vis-idx { font-size: 9px; color: var(--muted); font-weight: 700; }
.dp-vis-val { font-size: 13px; font-weight: 800; }
.dp-vis-true { background: #fef2f2 !important; border-color: #fca5a5 !important; }
.dp-vis-true .dp-vis-val { color: #b91c1c; }
.dp-vis-false { background: #f0fdf4 !important; border-color: #86efac !important; }
.dp-vis-false .dp-vis-val { color: #15803d; }
.dp-vis-active { transform: scale(1.08); z-index: 5; box-shadow: 0 0 8px rgba(249,115,22,.3); border-color: var(--orange) !important; }

/* Tier 3: Pruning Inspector */
.dp-prune-panel {
  display: flex; align-items: center; gap: 10px; padding: 8px 12px;
  border-radius: var(--radius-sm); border: 1.5px solid var(--border);
  background: var(--surface2); min-width: 320px; transition: all .2s;
}
.dp-prune-valid { border-color: var(--green) !important; background: #f0fdf4 !important; }
.dp-prune-visited { border-color: var(--red) !important; background: var(--red-light) !important; }
.dp-prune-dup { border-color: var(--amber) !important; background: var(--amber-light) !important; }
.dp-prune-icon { width: 24px; height: 24px; border-radius: 50%; display: flex; align-items: center; justify-content: center; font-size: 12px; font-weight: 900; font-family: monospace; flex-shrink: 0; border: 2px solid currentColor; color: var(--muted); background: var(--surface); }
.dp-prune-valid .dp-prune-icon { color: #15803d; background: #dcfce7; }
.dp-prune-visited .dp-prune-icon { color: #b91c1c; background: #fee2e2; }
.dp-prune-dup .dp-prune-icon { color: #b45309; background: #fef3c7; }
.dp-prune-content { display: flex; flex-direction: column; gap: 2px; min-width: 0; }
.dp-prune-title { font-size: 11px; font-weight: 800; font-family: monospace; }
.dp-prune-valid .dp-prune-title { color: #15803d; }
.dp-prune-visited .dp-prune-title { color: #b91c1c; }
.dp-prune-dup .dp-prune-title { color: #b45309; }
.dp-prune-detail { font-size: 10px; color: var(--text2); font-family: monospace; line-height: 1.4; }

/* Tier 4: Buffer (reuse gp-buf-* classes + dp additions) */
.gp-buf-panel { display: flex; flex-direction: column; gap: 4px; background: var(--surface2); border: 1px solid var(--border); border-radius: var(--radius-sm); padding: 6px 10px; min-width: 200px; }
.gp-buf-meta { font-size: 10.5px; font-family: monospace; color: var(--text2); }
.gp-buf-slots { display: flex; gap: 3px; flex-wrap: wrap; align-items: center; }
.gp-buf-slot { display: flex; flex-direction: column; align-items: center; justify-content: center; width: 40px; min-height: 44px; border-radius: 4px; border: 1.5px solid var(--border); background: var(--surface); font-family: monospace; position: relative; transition: all .15s; }
.gp-buf-filled { background: #eff6ff; border-color: #93c5fd; }
.gp-buf-empty { background: #f8fafc; border-color: var(--border); opacity: 0.55; }
.gp-buf-just-pushed { background: #dcfce7 !important; border-color: #10b981 !important; animation: ll-pop 0.25s ease; }
.gp-buf-just-popped { background: #fee2e2 !important; border-color: var(--red) !important; animation: dp-pulse-pop 0.3s ease; }
.dp-buf-found { background: #fef9c3 !important; border-color: #f59e0b !important; }
.gp-buf-idx { font-size: 8px; color: var(--muted); font-weight: 700; margin-bottom: 2px; }
.gp-buf-val { font-size: 14px; font-weight: 800; color: var(--text); }
.gp-push-badge { position: absolute; top: -9px; left: 50%; transform: translateX(-50%); font-size: 8px; font-weight: 800; font-family: monospace; background: #dcfce7; color: #15803d; border: 1px solid #86efac; padding: 1px 4px; border-radius: 3px; white-space: nowrap; }
.gp-pop-badge { position: absolute; top: -9px; left: 50%; transform: translateX(-50%); font-size: 8px; font-weight: 800; font-family: monospace; background: #fee2e2; color: #b91c1c; border: 1px solid #fca5a5; padding: 1px 4px; border-radius: 3px; white-space: nowrap; }
.gp-buf-empty-msg { color: var(--muted); font-style: italic; font-size: 11px; }
.dp-buf-complete { margin-top: 4px; font-size: 11px; font-weight: 700; font-family: monospace; color: #15803d; background: #dcfce7; border: 1px solid #86efac; padding: 3px 8px; border-radius: 4px; animation: dp-flash-found 0.5s ease; }

/* Tier 5: Result badges */
.gp-solutions-panel { display: flex; flex-wrap: wrap; gap: 4px; max-width: 500px; background: #f8fafc; padding: 6px 10px; border-radius: var(--radius-sm); border: 1px solid var(--border); }
.gp-no-solutions { color: var(--muted); font-style: italic; font-size: 11px; }
.gp-sol-btn { padding: 3px 8px; border-radius: 4px; border: 1px solid var(--border2); background: var(--surface); color: var(--text2); font-size: 10.5px; font-family: monospace; cursor: pointer; transition: all .15s ease; }
.gp-sol-btn:hover { border-color: #10b981; color: #059669; }
.gp-sol-btn.active { background: #dcfce7; border-color: #10b981; color: #15803d; font-weight: 700; }
.dp-perm-btn.active { animation: dp-flash-found 0.5s ease; }

/* Tier 6: Memory bar */
.gp-mem-bar { display: flex; flex-wrap: wrap; gap: 4px 10px; background: #1e293b; color: #94a3b8; font-family: monospace; font-size: 10px; padding: 5px 10px; border-radius: var(--radius-sm); align-items: center; }
.gp-mem-item { display: flex; align-items: center; gap: 4px; }
.gp-mem-label { color: #64748b; font-weight: 700; }
.gp-mem-bar code { color: #38bdf8; font-size: 10px; }
.gp-mem-sep { color: #334155; font-size: 12px; }

.dp-empty-msg { color: var(--muted); font-style: italic; font-size: 11px; }

.ll-vresizer { height: 5px; cursor: row-resize; background: var(--border); flex-shrink: 0; transition: background .15s; position: relative; z-index: 20; }
.ll-vresizer:hover, .ll-vresizer.drag { background: var(--coral); }
.ll-legend { display: flex; flex-wrap: wrap; gap: 6px 14px; padding: 6px 12px; border-bottom: 1px solid var(--border); flex-shrink: 0; background: var(--surface2); }
.ll-leg { display: flex; align-items: center; gap: 5px; font-size: 11px; color: var(--text2); font-weight: 500; }
.ll-legdot { width: 11px; height: 11px; border-radius: 3px; flex-shrink: 0; display: inline-block; }
.dp-legdot-active { background: #fffbeb; border: 1.5px solid var(--orange); }
.dp-legdot-visited { background: #fef2f2; border: 1.5px solid #fca5a5; }
.dp-legdot-valid { background: #f0fdf4; border: 1.5px solid var(--green); }
.dp-legdot-dup { background: #fff7ed; border: 1.5px dashed var(--amber); }
.dp-legdot-match { background: #dcfce7; border: 1.5px solid #10b981; }

.ll-table-area { flex-shrink: 0; padding: 8px 14px; border-bottom: 1px solid var(--border); overflow-x: hidden; overflow-y: auto; background: var(--surface); min-width: 0; box-sizing: border-box; }
.ll-table-title { font-size: 10px; color: var(--muted); margin-bottom: 4px; font-style: italic; }
.ll-stack-line { font-family: 'Consolas', monospace; font-size: 12px; line-height: 1.8; }
.ll-frame { font-family: 'Consolas', monospace; font-size: 11.5px; color: var(--text2); padding: 1px 0; white-space: nowrap; }
.ll-frame-cur { color: var(--orange); background: var(--orange-light); border-radius: 4px; padding: 1px 5px; }
.ll-fname { color: var(--text2); }
.ll-now { color: var(--orange); font-size: 10px; margin-left: 6px; }

.ll-badge-wrap { padding: 6px 10px; border-bottom: 1px solid var(--border); flex-shrink: 0; min-height: 36px; display: flex; align-items: center; background: var(--surface); }
.ll-badge { display: inline-block; padding: 4px 12px; border-radius: var(--radius-sm); border-left: 3px solid var(--coral); background: var(--coral-light); font-size: 11px; color: var(--coral-dark); line-height: 1.4; word-break: break-word; font-weight: 500; }
.ll-badge-error { border-left-color: var(--amber) !important; background: var(--amber-light) !important; color: #b45309 !important; }
.ll-badge-success { border-left-color: var(--green) !important; background: var(--green-light) !important; color: #15803d !important; }

.ll-code-panel { display: flex; flex-direction: column; height: 100%; overflow: hidden; }
.ll-code-header { display: flex; align-items: center; gap: 6px; padding: 5px 12px; background: var(--surface); border-bottom: 1px solid var(--border); box-shadow: var(--shadow-sm); flex-shrink: 0; flex-wrap: wrap; }
.ll-tabbar { display: flex; gap: 3px; flex-wrap: wrap; }
.ll-tab-btn { padding: 4px 9px; font-size: 10.5px; font-weight: 600; border: 1px solid var(--border2); background: var(--surface2); color: var(--text2); border-radius: var(--radius-sm); cursor: pointer; transition: all .15s ease; white-space: nowrap; }
.ll-tab-btn:hover { border-color: var(--coral); color: var(--coral); }
.ll-tab-btn.active { background: var(--coral); border-color: var(--coral); color: #fff; }
.ll-lang-select { margin-left: auto; padding: 4px 24px 4px 8px; font-size: 11px; font-weight: 500; border: 1px solid var(--border2); border-radius: var(--radius-sm); background: var(--surface2); color: var(--text); cursor: pointer; appearance: none; background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='10' height='6' viewBox='0 0 10 6'%3E%3Cpath d='M0 0l5 6 5-6z' fill='%2394a3b8'/%3E%3C/svg%3E"); background-repeat: no-repeat; background-position: right 8px center; min-width: 95px; transition: border-color .15s; }
.ll-lang-select:focus { outline: none; border-color: var(--coral); box-shadow: 0 0 0 3px rgba(240,77,77,.1); }
.ll-code-scroll { flex: 1; overflow: auto; background: #f8fafc; padding: 10px 14px; min-width: 0; }
.ll-pre { margin: 0; font-family: 'Cascadia Code', 'Fira Code', 'Consolas', monospace; font-size: 11px; line-height: 1.5; color: var(--text); white-space: pre; padding-bottom: 150px; }
.ll-codeline { display: block; padding: 0 14px; margin: 0 -14px; }
.ll-hl { background: #dcfce7; color: #15803d; font-weight: 600; border-left: 3px solid var(--green); border-radius: 3px; }

.ll-info-scroll { flex: 1; overflow: auto; padding: 12px 16px; background: var(--surface); color: var(--text2); font-size: 12px; line-height: 1.55; }
.ll-info-scroll h3 { font-size: 13px; font-weight: 700; color: var(--text); margin: 0 0 6px; }
.ll-info-scroll h4 { font-size: 12px; font-weight: 700; color: var(--text); margin: 10px 0 4px; }
.ll-info-scroll p { margin: 0 0 6px; }
.ll-info-scroll code { background: var(--surface2); padding: 1px 4px; border-radius: 3px; font-family: 'Cascadia Code', monospace; font-size: 11px; color: var(--coral-dark); }
.ll-cx-heading { font-size: 13px; font-weight: 700; color: var(--text); margin: 0 0 6px; }
.ll-cx-intro { font-size: 10.5px; color: var(--text2); margin: 0 0 10px; line-height: 1.55; }
.ll-cx-sub { font-size: 11px; font-weight: 700; color: var(--text2); margin: 10px 0 4px; border-bottom: 1px solid var(--border); padding-bottom: 3px; }
.ll-complexity-table { width: 100%; border-collapse: collapse; font-size: 10.5px; margin: 8px 0; }
.ll-complexity-table th, .ll-complexity-table td { border: 1px solid var(--border); padding: 4px 8px; text-align: left; }
.ll-complexity-table th { background: var(--surface2); font-weight: 700; color: var(--text2); }
.ll-cx-good { color: #15803d; font-weight: 700; } .ll-cx-mid { color: #b45309; font-weight: 700; }
.ll-cx-summary-grid { display: flex; gap: 8px; flex-wrap: wrap; margin: 6px 0 10px; }
.ll-cx-card { flex: 1; min-width: 90px; border-radius: var(--radius-sm); padding: 8px 10px; text-align: center; border: 1.5px solid var(--border); }
.ll-cx-card-good { background: #f0fdf4; border-color: #86efac; color: #15803d; }
.ll-cx-card-mid { background: #fff7ed; border-color: #fed7aa; color: #c2410c; }
.ll-cx-card-label { font-size: 9px; font-weight: 700; text-transform: uppercase; letter-spacing: .05em; opacity: .7; margin-bottom: 4px; }
.ll-cx-card-val { font-size: 13px; font-weight: 800; font-family: monospace; margin-bottom: 3px; }
.ll-cx-card-note { font-size: 8.5px; opacity: .75; line-height: 1.3; }
.ll-note { background: #fefce8; border: 1px solid #fef08a; border-left: 3px solid #eab308; padding: 6px 10px; font-size: 10.5px; color: #854d0e; border-radius: 0 4px 4px 0; margin-top: 10px; margin-bottom: 120px; }

.ll-footer { display: flex; align-items: center; justify-content: space-between; padding: 4px 12px; background: var(--surface); border-top: 1px solid var(--border); font-size: 11px; color: var(--muted); font-weight: 600; flex-shrink: 0; }
.ll-speed-wrap { display: flex; align-items: center; gap: 6px; }
.ll-speed-wrap input[type="range"] { width: 80px; accent-color: var(--coral); }
</style>
