<script setup>
import { ref, reactive, computed, onMounted, onBeforeUnmount, watch } from 'vue';

defineProps({
  topic: { type: String, default: 'Directed Weighted Graph' },
  subTopic: { type: String, default: '2D Adjacency Matrix Representation' }
});

const CODES = {
  java: [
    ['', 'class DirectedWeightedGraphMatrix {'],
    ['c_v_decl', '    int V;'],
    ['c_adj_decl', '    int[][] adj;'],
    ['', ''],
    ['c_ctor_entry', '    DirectedWeightedGraphMatrix(int vertices) {'],
    ['c_v_assign', '        this.V = vertices;'],
    ['c_alloc_matrix', '        this.adj = new int[V][V];'],
    ['c_init_loop_i', '        for (int i = 0; i < V; i++) {'],
    ['c_init_loop_j', '            for (int j = 0; j < V; j++) {'],
    ['c_init_zero', '                adj[i][j] = 0;'],
    ['', '            }'],
    ['', '        }'],
    ['c_ctor_done', '    }'],
    ['', ''],
    ['c_add_edges_entry', '    void addEdges(int[][] edges) {'],
    ['c_edge_loop_k', '        for (int k = 0; k < edges.length; k++) {'],
    ['c_extract_u', '            int u = edges[k][0];'],
    ['c_extract_v', '            int v = edges[k][1];'],
    ['c_extract_w', '            int weight = edges[k][2];'],
    ['c_add_check', '            if (u >= 0 && u < V && v >= 0 && v < V) {'],
    ['c_add_set', '                adj[u][v] = weight;'],
    ['', '            }'],
    ['', '        }'],
    ['c_add_edges_done', '    }'],
    ['', ''],
    ['c_get_entry', '    int getEdge(int u, int v) {'],
    ['c_get_check', '        if (u >= 0 && u < V && v >= 0 && v < V) {'],
    ['c_get_return', '            return adj[u][v];'],
    ['', '        }'],
    ['c_get_default', '        return 0;'],
    ['c_get_done', '    }'],
    ['', '}']
  ],
  c: [
    ['c_v_decl', '#define MAX_V 10'],
    ['c_adj_decl', 'typedef struct { int V; int adj[MAX_V][MAX_V]; } Graph;'],
    ['', ''],
    ['c_ctor_entry', 'void initGraph(Graph* g, int vertices) {'],
    ['c_v_assign', '    g->V = vertices;'],
    ['c_init_loop_i', '    for (int i = 0; i < g->V; i++) {'],
    ['c_init_loop_j', '        for (int j = 0; j < g->V; j++) {'],
    ['c_init_zero', '            g->adj[i][j] = 0;'],
    ['', '        }'],
    ['', '    }'],
    ['c_ctor_done', '}'],
    ['', ''],
    ['c_add_edges_entry', 'void addEdges(Graph* g, int edges[][3], int numEdges) {'],
    ['c_edge_loop_k', '    for (int k = 0; k < numEdges; k++) {'],
    ['c_extract_u', '        int u = edges[k][0];'],
    ['c_extract_v', '        int v = edges[k][1];'],
    ['c_extract_w', '        int weight = edges[k][2];'],
    ['c_add_check', '        if (u >= 0 && u < g->V && v >= 0 && v < g->V) {'],
    ['c_add_set', '            g->adj[u][v] = weight;'],
    ['', '        }'],
    ['', '    }'],
    ['c_add_edges_done', '}'],
    ['', ''],
    ['c_get_entry', 'int getEdge(Graph* g, int u, int v) {'],
    ['c_get_check', '    if (u >= 0 && u < g->V && v >= 0 && v < g->V) {'],
    ['c_get_return', '        return g->adj[u][v];'],
    ['', '    }'],
    ['c_get_default', '    return 0;'],
    ['c_get_done', '}']
  ],
  cpp: [
    ['', 'class DirectedWeightedGraphMatrix {'],
    ['c_v_decl', '    int V;'],
    ['c_adj_decl', '    vector<vector<int>> adj;'],
    ['', 'public:'],
    ['c_ctor_entry', '    DirectedWeightedGraphMatrix(int vertices) {'],
    ['c_v_assign', '        this->V = vertices;'],
    ['c_alloc_matrix', '        adj.assign(V, vector<int>(V, 0));'],
    ['c_init_loop_i', '        for (int i = 0; i < V; i++) {'],
    ['c_init_loop_j', '            for (int j = 0; j < V; j++) {'],
    ['c_init_zero', '                adj[i][j] = 0;'],
    ['', '            }'],
    ['', '        }'],
    ['c_ctor_done', '    }'],
    ['', ''],
    ['c_add_edges_entry', '    void addEdges(const vector<vector<int>>& edges) {'],
    ['c_edge_loop_k', '        for (size_t k = 0; k < edges.size(); k++) {'],
    ['c_extract_u', '            int u = edges[k][0];'],
    ['c_extract_v', '            int v = edges[k][1];'],
    ['c_extract_w', '            int weight = edges[k][2];'],
    ['c_add_check', '            if (u >= 0 && u < V && v >= 0 && v < V) {'],
    ['c_add_set', '                adj[u][v] = weight;'],
    ['', '            }'],
    ['', '        }'],
    ['c_add_edges_done', '    }'],
    ['', ''],
    ['c_get_entry', '    int getEdge(int u, int v) {'],
    ['c_get_check', '        if (u >= 0 && u < V && v >= 0 && v < V) {'],
    ['c_get_return', '            return adj[u][v];'],
    ['', '        }'],
    ['c_get_default', '        return 0;'],
    ['c_get_done', '    }'],
    ['', '};']
  ],
  python: [
    ['', 'class DirectedWeightedGraphMatrix:'],
    ['c_ctor_entry', '    def __init__(self, vertices: int):'],
    ['c_v_assign', '        self.V = vertices'],
    ['c_alloc_matrix', '        self.adj = [[0] * vertices for _ in range(vertices)]'],
    ['c_init_loop_i', '        for i in range(self.V):'],
    ['c_init_loop_j', '            for j in range(self.V):'],
    ['c_init_zero', '                self.adj[i][j] = 0'],
    ['c_ctor_done', ''],
    ['c_add_edges_entry', '    def add_edges(self, edges: list):'],
    ['c_edge_loop_k', '        for k in range(len(edges)):'],
    ['c_extract_u', '            u = edges[k][0]'],
    ['c_extract_v', '            v = edges[k][1]'],
    ['c_extract_w', '            weight = edges[k][2]'],
    ['c_add_check', '            if 0 <= u < self.V and 0 <= v < self.V:'],
    ['c_add_set', '                self.adj[u][v] = weight'],
    ['c_add_edges_done', ''],
    ['c_get_entry', '    def get_edge(self, u: int, v: int) -> int:'],
    ['c_get_check', '        if 0 <= u < self.V and 0 <= v < self.V:'],
    ['c_get_return', '            return self.adj[u][v]'],
    ['c_get_default', '        return 0'],
    ['c_get_done', '']
  ],
  javascript: [
    ['', 'class DirectedWeightedGraphMatrix {'],
    ['c_ctor_entry', '  constructor(vertices) {'],
    ['c_v_assign', '    this.V = vertices;'],
    ['c_alloc_matrix', '    this.adj = Array.from({ length: vertices }, () => new Array(vertices).fill(0));'],
    ['c_init_loop_i', '    for (let i = 0; i < this.V; i++) {'],
    ['c_init_loop_j', '      for (let j = 0; j < this.V; j++) {'],
    ['c_init_zero', '        this.adj[i][j] = 0;'],
    ['', '      }'],
    ['', '    }'],
    ['c_ctor_done', '  }'],
    ['', ''],
    ['c_add_edges_entry', '  addEdges(edges) {'],
    ['c_edge_loop_k', '    for (let k = 0; k < edges.length; k++) {'],
    ['c_extract_u', '      let u = edges[k][0];'],
    ['c_extract_v', '      let v = edges[k][1];'],
    ['c_extract_w', '      let weight = edges[k][2];'],
    ['c_add_check', '      if (u >= 0 && u < this.V && v >= 0 && v < this.V) {'],
    ['c_add_set', '        this.adj[u][v] = weight;'],
    ['', '      }'],
    ['', '    }'],
    ['c_add_edges_done', '  }'],
    ['', ''],
    ['c_get_entry', '  getEdge(u, v) {'],
    ['c_get_check', '    if (u >= 0 && u < this.V && v >= 0 && v < this.V) {'],
    ['c_get_return', '      return this.adj[u][v];'],
    ['', '    }'],
    ['c_get_default', '    return 0;'],
    ['c_get_done', '  }'],
    ['', '}']
  ]
};

const PSEUDOCODE = [
  'class DirectedWeightedGraphMatrix:',
  '    constructor(V):',
  '        this.V = V',
  '        this.adj = 2D Array of size V x V initialized to 0',
  '    function addEdges(edges):',
  '        for k in 0..edges.length-1:',
  '            u = edges[k][0], v = edges[k][1], weight = edges[k][2]',
  '            if u in [0..V-1] and v in [0..V-1]:',
  '                adj[u][v] = weight'
];

function frame(title, rows) { return { title, rows }; }

function buildSteps(numV, edgeList) {
  const steps = [];
  const rawV = parseInt(numV);
  const V = isNaN(rawV) || rawV < 0 ? 0 : Math.min(10, rawV);
  const fnCtor = 'DirectedWeightedGraphMatrix(vertices)';
  const fnAddEdges = 'addEdges(edges)';

  if (V === 0) {
    steps.push({
      badge: 'Number of vertices is 0. No vertices, matrix, or graph created.',
      code: '',
      vars: [frame('main()', []), frame(fnCtor, [['vertices', '0']])],
      V: 0, matrix: [], activeU: -1, activeV: -1, activeW: -1, activeK: -1, curI: -1, curJ: -1, edges: []
    });
    return steps;
  }

  const matrix = Array.from({ length: V }, () => new Array(V).fill(0));

  // 1. Constructor Entry
  steps.push({
    badge: `DirectedWeightedGraphMatrix constructor called with vertices = ${V}`,
    code: 'c_ctor_entry',
    vars: [frame('main()', []), frame(fnCtor, [['vertices', String(V)]])],
    V, matrix: matrix.map(row => [...row]), activeU: -1, activeV: -1, activeW: -1, activeK: -1, curI: -1, curJ: -1, edges: []
  });

  // 2. Assign V
  steps.push({
    badge: `this.V = ${V}; → Stored vertex count V = ${V}`,
    code: 'c_v_assign',
    vars: [frame('main()', []), frame(fnCtor, [['V', String(V)]])],
    V, matrix: matrix.map(row => [...row]), activeU: -1, activeV: -1, activeW: -1, activeK: -1, curI: -1, curJ: -1, edges: []
  });

  // 3. Allocate 2D Array
  steps.push({
    badge: `this.adj = new int[${V}][${V}]; → Allocated 2D Adjacency Matrix of size ${V}x${V}`,
    code: 'c_alloc_matrix',
    vars: [frame('main()', []), frame(fnCtor, [['V', String(V)], ['adj', `${V}x${V} 2D Array`]])],
    V, matrix: matrix.map(row => [...row]), activeU: -1, activeV: -1, activeW: -1, activeK: -1, curI: -1, curJ: -1, edges: []
  });

  // 4. Initialize Matrix with 0 line by line
  for (let i = 0; i < V; i++) {
    steps.push({
      badge: `for (int i = ${i}; i < V (${V}); i++) → Initializing row ${i}`,
      code: 'c_init_loop_i',
      vars: [frame('main()', []), frame(fnCtor, [['i', String(i)], ['V', String(V)]])],
      V, matrix: matrix.map(row => [...row]), activeU: -1, activeV: -1, activeW: -1, activeK: -1, curI: i, curJ: -1, edges: []
    });

    for (let j = 0; j < V; j++) {
      steps.push({
        badge: `for (int j = ${j}; j < V (${V}); j++) → Initializing column ${j}`,
        code: 'c_init_loop_j',
        vars: [frame('main()', []), frame(fnCtor, [['i', String(i)], ['j', String(j)]])],
        V, matrix: matrix.map(row => [...row]), activeU: -1, activeV: -1, activeW: -1, activeK: -1, curI: i, curJ: j, edges: []
      });

      matrix[i][j] = 0;
      steps.push({
        badge: `adj[${i}][${j}] = 0; → Set distance cell [${i}][${j}] to 0`,
        code: 'c_init_zero',
        vars: [frame('main()', []), frame(fnCtor, [['i', String(i)], ['j', String(j)], [`adj[${i}][${j}]`, '0']])],
        V, matrix: matrix.map(row => [...row]), activeU: -1, activeV: -1, activeW: -1, activeK: -1, curI: i, curJ: j, edges: []
      });
    }
  }

  steps.push({
    badge: `Constructor Complete! 2D Adjacency Matrix (${V}x${V}) initialized to 0.`,
    code: 'c_ctor_done',
    vars: [frame('main()', []), frame(fnCtor, [['V', String(V)]])],
    V, matrix: matrix.map(row => [...row]), activeU: -1, activeV: -1, activeW: -1, activeK: -1, curI: -1, curJ: -1, edges: []
  });

  // 5. Sequential Loop for Input Edge List
  const processedEdges = [];
  steps.push({
    badge: `addEdges(edges) called with ${edgeList.length} edges`,
    code: 'c_add_edges_entry',
    vars: [frame('main()', []), frame(fnAddEdges, [['edges.length', String(edgeList.length)]])],
    V, matrix: matrix.map(row => [...row]), activeU: -1, activeV: -1, activeW: -1, activeK: -1, curI: -1, curJ: -1, edges: []
  });

  for (let k = 0; k < edgeList.length; k++) {
    const edge = edgeList[k];
    const u = edge[0], v = edge[1], weight = edge[2];

    steps.push({
      badge: `for (int k = ${k}; k < edges.length (${edgeList.length}); k++) → Edge ${k + 1} of ${edgeList.length}`,
      code: 'c_edge_loop_k',
      vars: [frame('main()', []), frame(fnAddEdges, [['k', String(k)], ['edges.length', String(edgeList.length)]])],
      V, matrix: matrix.map(row => [...row]), activeU: -1, activeV: -1, activeW: -1, activeK: k, curI: -1, curJ: -1, edges: [...processedEdges]
    });

    steps.push({
      badge: `int u = edges[${k}][0]; → Source vertex u = ${u}`,
      code: 'c_extract_u',
      vars: [frame('main()', []), frame(fnAddEdges, [['k', String(k)], ['u', String(u)]])],
      V, matrix: matrix.map(row => [...row]), activeU: u, activeV: -1, activeW: -1, activeK: k, curI: -1, curJ: -1, edges: [...processedEdges]
    });

    steps.push({
      badge: `int v = edges[${k}][1]; → Target vertex v = ${v}`,
      code: 'c_extract_v',
      vars: [frame('main()', []), frame(fnAddEdges, [['k', String(k)], ['u', String(u)], ['v', String(v)]])],
      V, matrix: matrix.map(row => [...row]), activeU: u, activeV: v, activeW: -1, activeK: k, curI: -1, curJ: -1, edges: [...processedEdges]
    });

    steps.push({
      badge: `int weight = edges[${k}][2]; → Weight = ${weight}`,
      code: 'c_extract_w',
      vars: [frame('main()', []), frame(fnAddEdges, [['k', String(k)], ['u', String(u)], ['v', String(v)], ['weight', String(weight)]])],
      V, matrix: matrix.map(row => [...row]), activeU: u, activeV: v, activeW: weight, activeK: k, curI: -1, curJ: -1, edges: [...processedEdges]
    });

    const isValid = u >= 0 && u < V && v >= 0 && v < V;
    steps.push({
      badge: `if (u >= 0 && u < V && v >= 0 && v < V) → (${u} >= 0 && ${u} < ${V} && ${v} >= 0 && ${v} < ${V}) → ${isValid ? 'TRUE' : 'FALSE'}`,
      code: 'c_add_check',
      vars: [frame('main()', []), frame(fnAddEdges, [['u', String(u)], ['v', String(v)], ['valid', String(isValid)]])],
      V, matrix: matrix.map(row => [...row]), activeU: u, activeV: v, activeW: weight, activeK: k, curI: -1, curJ: -1, edges: [...processedEdges]
    });

    if (isValid) {
      matrix[u][v] = weight;
      const existingIdx = processedEdges.findIndex(e => e.u === u && e.v === v);
      if (existingIdx >= 0) processedEdges[existingIdx].weight = weight;
      else processedEdges.push({ u, v, weight });

      steps.push({
        badge: `adj[${u}][${v}] = ${weight}; → Updated matrix cell [${u}][${v}] to ${weight} & created edge (${u} → ${v})`,
        code: 'c_add_set',
        vars: [frame('main()', []), frame(fnAddEdges, [['u', String(u)], ['v', String(v)], [`adj[${u}][${v}]`, String(weight)]])],
        V, matrix: matrix.map(row => [...row]), activeU: u, activeV: v, activeW: weight, activeK: k, curI: u, curJ: v, edges: [...processedEdges]
      });
    }
  }

  steps.push({
    badge: `addEdges completed! All ${edgeList.length} edges processed sequentially.`,
    code: 'c_add_edges_done',
    vars: [frame('main()', []), frame('DirectedWeightedGraphMatrix', [['V', String(V)], ['totalEdges', String(processedEdges.length)]])],
    V, matrix: matrix.map(row => [...row]), activeU: -1, activeV: -1, activeW: -1, activeK: -1, curI: -1, curJ: -1, edges: [...processedEdges]
  });

  return steps;
}

const numVInput = ref(4);
const edgesInputStr = ref('[[0,1,5],[0,2,4],[1,2,3]]');

function parseEdges(str) {
  if (!str || !str.trim()) return [];
  try {
    const parsed = JSON.parse(str);
    if (Array.isArray(parsed)) {
      return parsed
        .filter(e => Array.isArray(e) && e.length >= 3)
        .map(e => [parseInt(e[0]), parseInt(e[1]), parseInt(e[2])]);
    }
  } catch (err) {
    const matches = (str || '').match(/\[\s*\d+\s*,\s*\d+\s*,\s*\d+\s*\]/g);
    if (matches) {
      return matches.map(m => {
        try {
          const arr = JSON.parse(m);
          return [parseInt(arr[0]), parseInt(arr[1]), parseInt(arr[2])];
        } catch (e) { return null; }
      }).filter(Boolean);
    }
  }
  return [];
}

const defaultEdgeList = [[0, 1, 5], [0, 2, 4], [1, 2, 3]];

const hoveredEdge = ref(null);

function isEdgeHovered(u, v) {
  return hoveredEdge.value && hoveredEdge.value.u === u && hoveredEdge.value.v === v;
}
function isNodeHoveredSource(nodeId) {
  return hoveredEdge.value && hoveredEdge.value.u === nodeId;
}
function isNodeHoveredTarget(nodeId) {
  return hoveredEdge.value && hoveredEdge.value.v === nodeId;
}

const lang = ref('java');
const speed = ref(650);
const si = ref(0);
const playing = ref(false);
const vizHeight = ref(300);
const tableHeight = ref(60);
const leftWidth = ref(55);
const rightTab = ref('code');
const showGraphModal = ref(false);

const stepsData = reactive({ steps: buildSteps(4, defaultEdgeList) });
const steps = computed(() => stepsData.steps);
const s = computed(() => steps.value[Math.max(0, Math.min(si.value, steps.value.length - 1))] || {});
const codeLines = computed(() => CODES[lang.value] || []);

const modalNodePositions = computed(() => {
  const V = s.value.V ?? 0;
  if (V <= 0) return [];
  const positions = [];
  const cx = 360, cy = 230;
  const r = V <= 4 ? 145 : (V <= 6 ? 168 : 185);
  for (let i = 0; i < V; i++) {
    const angle = (2 * Math.PI * i) / V - Math.PI / 2;
    positions.push({
      id: i,
      x: cx + r * Math.cos(angle),
      y: cy + r * Math.sin(angle)
    });
  }
  return positions;
});

function getEdgeGeometry(edge, positions, allEdges, curvature = 32) {
  const uNode = positions[edge.u];
  const vNode = positions[edge.v];
  if (!uNode || !vNode) return { pathD: '', labelX: 0, labelY: 0, isBi: false, isLoop: false };

  // Handle Self-Loop (u === v)
  if (edge.u === edge.v) {
    const x0 = uNode.x, y0 = uNode.y;
    const gCx = 360, gCy = 230;
    let dx = x0 - gCx, dy = y0 - gCy;
    let dist = Math.hypot(dx, dy);
    if (dist < 1e-3) { dx = 0; dy = -1; dist = 1; }
    const dirX = dx / dist, dirY = dy / dist;
    const px = -dirY, py = dirX;

    const nodeR = 20;
    const loopOffset = 38;

    const startX = x0 + dirX * nodeR - px * 10;
    const startY = y0 + dirY * nodeR - py * 10;
    const endX = x0 + dirX * nodeR + px * 10;
    const endY = y0 + dirY * nodeR + py * 10;

    const c1X = startX + dirX * loopOffset - px * 14;
    const c1Y = startY + dirY * loopOffset - py * 14;
    const c2X = endX + dirX * loopOffset + px * 14;
    const c2Y = endY + dirY * loopOffset + py * 14;

    const pathD = `M ${startX} ${startY} C ${c1X} ${c1Y}, ${c2X} ${c2Y}, ${endX} ${endY}`;
    const labelX = x0 + dirX * (nodeR + loopOffset + 12);
    const labelY = y0 + dirY * (nodeR + loopOffset + 12);

    return { pathD, labelX, labelY, isBi: false, isLoop: true };
  }

  // Normal edge (u !== v)
  const x1 = uNode.x, y1 = uNode.y;
  const x2 = vNode.x, y2 = vNode.y;
  const dx = x2 - x1, dy = y2 - y1;
  const dist = Math.hypot(dx, dy) || 1;

  const ux = dx / dist, uy = dy / dist;
  const nx = -uy, ny = ux;

  const isBi = allEdges.some(e => e.u === edge.v && e.v === edge.u);

  const h = isBi ? curvature : 0;
  const mx = (x1 + x2) / 2;
  const my = (y1 + y2) / 2;
  const cx = mx + h * nx;
  const cy = my + h * ny;

  const nodeR = 20;
  const startX = x1 + ux * nodeR + (isBi ? nx * 6 : 0);
  const startY = y1 + uy * nodeR + (isBi ? ny * 6 : 0);
  const endX = x2 - ux * nodeR + (isBi ? nx * 6 : 0);
  const endY = y2 - uy * nodeR + (isBi ? ny * 6 : 0);

  const pathD = isBi
    ? `M ${startX} ${startY} Q ${cx} ${cy} ${endX} ${endY}`
    : `M ${startX} ${startY} L ${endX} ${endY}`;

  const labelX = isBi ? 0.25 * startX + 0.5 * cx + 0.25 * endX : mx;
  const labelY = isBi ? 0.25 * startY + 0.5 * cy + 0.25 * endY : my;

  return { pathD, labelX, labelY, isBi, isLoop: false };
}

function getEdgeGeo(edge) {
  return getEdgeGeometry(edge, modalNodePositions.value, s.value.edges || [], 32);
}

let playTimer = null;

function applySetup() {
  const rawV = parseInt(numVInput.value);
  const vCount = isNaN(rawV) || rawV < 0 ? 0 : Math.min(10, rawV);
  const parsedEdges = parseEdges(edgesInputStr.value);
  playing.value = false;
  stepsData.steps = buildSteps(vCount, parsedEdges);
  si.value = 0;
}

function handleAddEdge() {
  const u = parseInt(inputU.value);
  const v = parseInt(inputV.value);
  const w = parseInt(inputW.value);
  const V = s.value.V ?? 0;

  if (V <= 0) {
    alert('Cannot add edge to a graph with 0 vertices.');
    return;
  }

  if (isNaN(u) || u < 0 || u >= V || isNaN(v) || v < 0 || v >= V) {
    alert(`Source (u) and Destination (v) must be between 0 and ${V - 1}.`);
    return;
  }
  if (isNaN(w) || w <= 0) {
    alert('Weight must be a positive integer.');
    return;
  }

  playing.value = false;
  const currentEdges = s.value.edges ? [...s.value.edges] : [];
  const existingIdx = currentEdges.findIndex(e => e.u === u && e.v === v);
  if (existingIdx >= 0) currentEdges[existingIdx].weight = w;
  else currentEdges.push({ u, v, weight: w });

  stepsData.steps = buildSteps(V, currentEdges);
  si.value = stepsData.steps.length - 4; // Jump to adding this edge
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
  cleanupFns.push(initVResizer(vizResizerRef, vizHeight, 180, 500));
  cleanupFns.push(initVResizer(tableResizerRef, tableHeight, 50, 200));
});

onBeforeUnmount(() => {
  document.removeEventListener('keydown', onKeydown);
  clearTimeout(playTimer);
  cleanupFns.forEach(fn => fn && fn());
});

// Vertex Node Positions in Circular Layout
const nodePositions = computed(() => {
  const V = s.value.V ?? 0;
  if (V <= 0) return [];
  const positions = [];
  const cx = 150, cy = 130, r = 85;
  for (let i = 0; i < V; i++) {
    const angle = (2 * Math.PI * i) / V - Math.PI / 2;
    positions.push({
      id: i,
      x: cx + r * Math.cos(angle),
      y: cy + r * Math.sin(angle)
    });
  }
  return positions;
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
            <label>Vertices</label>
            <input type="number" v-model.number="numVInput" min="0" max="10" style="width: 45px;" class="ll-text-input" @change="applySetup" @keyup.enter="applySetup" />

            <span class="ll-divider">|</span>

            <label>Edges</label>
            <input type="text" v-model="edgesInputStr" style="width: 220px;" class="ll-text-input" placeholder="e.g. [[0,1,5],[0,2,4],[1,2,3]]" @keyup.enter="applySetup" />
            <button class="ll-viz-btn" @click="applySetup" title="Visualize Edge List Loop">&#9654;</button>

            <button class="ll-graph-modal-btn" @click="showGraphModal = !showGraphModal">
              {{ showGraphModal ? 'Hide' : 'Show' }}
            </button>

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
                  <!-- 2D Adjacency Matrix Grid Display Only -->
                  <div class="ll-matrix-area">
                    <div class="ll-matrix-card">
                      <div class="ll-card-title">2D Adjacency Matrix <code>adj[u][v]</code>:</div>
                      <div v-if="(s.V ?? 0) === 0" class="ll-empty-matrix-msg">
                        No matrix to display (Vertices = 0). Enter a vertex count &gt; 0 to initialize graph.
                      </div>
                      <div v-else class="ll-matrix-grid-wrap">
                        <table class="ll-matrix-table">
                          <thead>
                            <tr>
                              <th class="ll-corner-th">u \ v</th>
                              <th
                                v-for="vIdx in s.V"
                                :key="'ch-' + (vIdx - 1)"
                                :class="{ 'll-th-active-v': (vIdx - 1) === s.activeV || (vIdx - 1) === s.curJ }"
                              >
                                <div class="ll-ptr-tag-wrap">
                                  <span v-if="(vIdx - 1) === s.activeV" class="ll-ptr-lbl ll-lbl-purple">v</span>
                                  <span v-if="(vIdx - 1) === s.curJ && (vIdx - 1) !== s.activeV" class="ll-ptr-lbl ll-lbl-green">j</span>
                                </div>
                                {{ vIdx - 1 }}
                              </th>
                            </tr>
                          </thead>
                          <tbody>
                            <tr v-for="uIdx in s.V" :key="'row-' + (uIdx - 1)">
                              <th :class="{ 'll-th-active-u': (uIdx - 1) === s.activeU || (uIdx - 1) === s.curI }">
                                <div class="ll-ptr-tag-wrap" style="justify-content: flex-start; padding-left: 2px;">
                                  <span v-if="(uIdx - 1) === s.activeU" class="ll-ptr-lbl ll-lbl-blue">u</span>
                                  <span v-if="(uIdx - 1) === s.curI && (uIdx - 1) !== s.activeU" class="ll-ptr-lbl ll-lbl-green">i</span>
                                </div>
                                {{ uIdx - 1 }}
                              </th>
                              <td
                                v-for="vIdx in s.V"
                                :key="'cell-' + (uIdx - 1) + '-' + (vIdx - 1)"
                                class="ll-matrix-cell"
                                :class="{
                                  'll-cell-active': (uIdx - 1) === s.activeU && (vIdx - 1) === s.activeV,
                                  'll-cell-cur': (uIdx - 1) === s.curI && (vIdx - 1) === s.curJ,
                                  'll-cell-hovered': isEdgeHovered(uIdx - 1, vIdx - 1),
                                  'll-cell-has-edge': s.matrix && s.matrix[uIdx - 1] && s.matrix[uIdx - 1][vIdx - 1] > 0
                                }"
                                @mouseenter="hoveredEdge = { u: uIdx - 1, v: vIdx - 1 }"
                                @mouseleave="hoveredEdge = null"
                              >
                                {{ s.matrix && s.matrix[uIdx - 1] ? s.matrix[uIdx - 1][vIdx - 1] : 0 }}
                              </td>
                            </tr>
                          </tbody>
                        </table>
                      </div>
                    </div>
                  </div>
                </div>
              </div>

              <div class="ll-vresizer" ref="vizResizerRef"></div>

              <!-- Color Legend -->
              <div class="ll-legend">
                <span class="ll-leg"><span class="ll-legdot ll-legdot-existing"></span>No Edge (0)</span>
                <span class="ll-leg"><span class="ll-legdot ll-legdot-has-edge"></span>Edge Exists (Weight w)</span>
                <span class="ll-leg"><span class="ll-legdot ll-legdot-u"></span>Source Vertex (u)</span>
                <span class="ll-leg"><span class="ll-legdot ll-legdot-v"></span>Destination Vertex (v)</span>
                <span class="ll-leg"><span class="ll-legdot ll-legdot-active"></span>Active Cell adj[u][v]</span>
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
                <div class="ll-badge">{{ s.badge }}</div>
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
                    <thead><tr><th>Operation</th><th>Time</th><th>Why</th></tr></thead>
                    <tbody>
                      <tr><td>Matrix Allocation</td><td>O(V&sup2;)</td><td>Initializes V x V matrix cells to 0.</td></tr>
                      <tr><td>Add Edge <code>addEdge(u, v, w)</code></td><td>O(1)</td><td>Direct indexing access <code>adj[u][v] = w</code>.</td></tr>
                      <tr><td>Query Edge <code>getEdge(u, v)</code></td><td>O(1)</td><td>Direct array lookup <code>return adj[u][v]</code>.</td></tr>
                      <tr><td>Space Complexity</td><td>O(V&sup2;)</td><td>Stores a dense V x V array in memory.</td></tr>
                    </tbody>
                  </table>
                  <p class="ll-note">
                    Key feature: <b>2D Adjacency Matrix</b> allows instant <code>O(1)</code> lookup for any directed edge <code>u &rarr; v</code>, but uses <code>O(V&sup2;)</code> space regardless of edge density.
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

    <!-- Floating Directed Weighted Graph Modal Dialog Container -->
    <div v-if="showGraphModal" class="graph-modal-backdrop" @click.self="showGraphModal = false">
      <div class="graph-modal-card">
        <div class="graph-modal-header">
          <div class="graph-modal-title">
            <span>Directed Weighted Graph View</span>
            <span class="graph-subtitle">(Synchronized with 2D Adjacency Matrix)</span>
          </div>
          <button class="graph-close-btn" @click="showGraphModal = false" title="Close modal">&times;</button>
        </div>

        <div class="graph-modal-body">
          <div v-if="(s.V ?? 0) === 0" class="ll-empty-graph-msg">
            No vertices or edges to display (Vertices = 0).
          </div>
          <svg v-else class="graph-modal-svg" viewBox="0 0 720 460">
            <defs>
              <marker id="modal-arrowhead" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="7" markerHeight="7" orient="auto-start-reverse">
                <path d="M 0 0 L 10 5 L 0 10 z" fill="#64748b" />
              </marker>
              <marker id="modal-arrowhead-active" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="8" markerHeight="8" orient="auto-start-reverse">
                <path d="M 0 0 L 10 5 L 0 10 z" fill="#f97316" />
              </marker>
              <marker id="modal-arrowhead-hover" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="8" markerHeight="8" orient="auto-start-reverse">
                <path d="M 0 0 L 10 5 L 0 10 z" fill="#3b82f6" />
              </marker>
            </defs>

            <!-- Directed Edges with Interactive Hover Targets -->
            <g
              v-for="edge in s.edges"
              :key="'me-' + edge.u + '-' + edge.v"
              @mouseenter="hoveredEdge = { u: edge.u, v: edge.v }"
              @mouseleave="hoveredEdge = null"
              style="cursor: pointer;"
            >
              <!-- Invisible Thick Hit Target Path for Smooth Hovering -->
              <path
                :d="getEdgeGeo(edge).pathD"
                fill="none"
                stroke="transparent"
                stroke-width="18"
                stroke-linecap="round"
              />

              <!-- Visible Directed Edge Line -->
              <path
                :d="getEdgeGeo(edge).pathD"
                fill="none"
                class="ll-edge-line"
                :class="{
                  'll-edge-active': edge.u === s.activeU && edge.v === s.activeV,
                  'll-edge-hovered': isEdgeHovered(edge.u, edge.v)
                }"
                :marker-end="isEdgeHovered(edge.u, edge.v)
                  ? 'url(#modal-arrowhead-hover)'
                  : ((edge.u === s.activeU && edge.v === s.activeV) ? 'url(#modal-arrowhead-active)' : 'url(#modal-arrowhead)')"
              />

              <!-- Weight Label Badge -->
              <g v-if="getEdgeGeo(edge)">
                <circle
                  :cx="getEdgeGeo(edge).labelX"
                  :cy="getEdgeGeo(edge).labelY"
                  r="13"
                  class="ll-weight-bg"
                  :class="{
                    'll-weight-bg-active': edge.u === s.activeU && edge.v === s.activeV,
                    'll-weight-bg-hovered': isEdgeHovered(edge.u, edge.v)
                  }"
                />
                <text
                  :x="getEdgeGeo(edge).labelX"
                  :y="getEdgeGeo(edge).labelY + 4"
                  class="ll-weight-text"
                  :class="{ 'll-weight-text-hovered': isEdgeHovered(edge.u, edge.v) }"
                >
                  {{ edge.weight }}
                </text>
              </g>
            </g>

            <!-- Graph Vertex Nodes -->
            <g v-for="node in modalNodePositions" :key="'mn-' + node.id">
              <!-- Pointer Labels above Nodes (u/v during animation or src/tgt on edge hover) -->
              <g v-if="node.id === s.activeU || node.id === s.activeV || isNodeHoveredSource(node.id) || isNodeHoveredTarget(node.id)">
                <text
                  :x="node.x"
                  :y="node.y - 28"
                  class="ll-svg-ptr"
                  :class="isNodeHoveredSource(node.id) ? 'll-svg-ptr-hover-src' : (isNodeHoveredTarget(node.id) ? 'll-svg-ptr-hover-tgt' : (node.id === s.activeU ? 'll-svg-ptr-blue' : 'll-svg-ptr-purple'))"
                >
                  {{ isNodeHoveredSource(node.id) ? 'src' : (isNodeHoveredTarget(node.id) ? 'tgt' : (node.id === s.activeU ? 'u' : 'v')) }}
                </text>
                <text
                  :x="node.x"
                  :y="node.y - 17"
                  class="ll-svg-ptr"
                  :class="isNodeHoveredSource(node.id) ? 'll-svg-ptr-hover-src' : (isNodeHoveredTarget(node.id) ? 'll-svg-ptr-hover-tgt' : (node.id === s.activeU ? 'll-svg-ptr-blue' : 'll-svg-ptr-purple'))"
                >
                  &darr;
                </text>
              </g>

              <circle
                :cx="node.x"
                :cy="node.y"
                r="20"
                class="ll-node-circle"
                :class="{
                  'll-node-u': node.id === s.activeU,
                  'll-node-v': node.id === s.activeV,
                  'll-node-hover-src': isNodeHoveredSource(node.id),
                  'll-node-hover-tgt': isNodeHoveredTarget(node.id)
                }"
              />
              <text
                :x="node.x"
                :y="node.y + 5"
                class="ll-node-text"
                :class="{
                  'll-node-text-hover-src': isNodeHoveredSource(node.id),
                  'll-node-text-hover-tgt': isNodeHoveredTarget(node.id)
                }"
                style="font-size: 15px;"
              >
                {{ node.id }}
              </text>
            </g>
          </svg>
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
.ll-text-input { background: var(--surface); border: 1px solid var(--border2); color: var(--text); border-radius: var(--radius-sm); padding: 4px 8px; font-size: 12px; font-family: monospace; transition: border-color .15s; text-align: center; }
.ll-text-input:focus { outline: none; border-color: var(--coral); box-shadow: 0 0 0 3px rgba(240,77,77,.1); }
.ll-divider { color: var(--border2); font-weight: 300; margin: 0 4px; }
.ll-viz-btn { background: var(--coral); color: #fff; border: none; padding: 5px 14px; border-radius: var(--radius-sm); cursor: pointer; font-size: 12px; font-weight: 600; box-shadow: var(--shadow-sm); transition: filter .15s; }
.ll-viz-btn:hover { filter: brightness(1.08); }
.ll-add-btn { background: var(--blue); color: #fff; border: none; padding: 5px 12px; border-radius: var(--radius-sm); cursor: pointer; font-size: 12px; font-weight: 600; box-shadow: var(--shadow-sm); transition: filter .15s; }
.ll-add-btn:hover { filter: brightness(1.08); }
.ll-graph-modal-btn { background: var(--purple); color: #fff; border: none; padding: 5px 12px; border-radius: var(--radius-sm); cursor: pointer; font-size: 12px; font-weight: 600; box-shadow: var(--shadow-sm); transition: filter .15s; }
.ll-graph-modal-btn:hover { filter: brightness(1.08); }

.graph-modal-backdrop {
  position: fixed;
  top: 0; left: 0; right: 0; bottom: 0;
  background: rgba(15, 23, 42, 0.55);
  backdrop-filter: blur(3px);
  z-index: 9999;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 20px;
}
.graph-modal-card {
  background: #ffffff;
  border-radius: 12px;
  box-shadow: 0 20px 25px -5px rgba(0,0,0,0.15), 0 10px 10px -5px rgba(0,0,0,0.04);
  width: 760px;
  max-width: 95vw;
  overflow: hidden;
  border: 1px solid var(--border);
  animation: ll-pop 0.25s ease-out;
  height: 90%;
}
.graph-modal-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 12px 18px;
  background: #f8fafc;
  border-bottom: 1px solid var(--border);
}
.graph-modal-title {
  font-size: 14px;
  font-weight: 700;
  color: #1e293b;
  display: flex;
  align-items: center;
  gap: 8px;
}
.graph-subtitle {
  font-size: 11px;
  font-weight: 500;
  color: #64748b;
}
.graph-close-btn {
  background: none;
  border: none;
  font-size: 22px;
  color: #64748b;
  cursor: pointer;
  line-height: 1;
  padding: 0 4px;
  border-radius: 4px;
  transition: color 0.15s;
}
.graph-close-btn:hover { color: #ef4444; }
.graph-modal-body {
  padding: 16px;
  display: flex;
  justify-content: center;
  background: #ffffff;
  height: 90%;
}
.graph-modal-svg {
  width: 100%;
  max-height: 480px;
}
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
.ll-ptrs { display: flex; gap: 8px; flex-wrap: wrap; padding: 8px 16px 4px; min-height: 34px; width: 100%; box-sizing: border-box; min-width: 0; }
.ll-ptr-chip { background: var(--surface2); border: 1px solid var(--border); border-radius: var(--radius-sm); padding: 3px 10px; font-size: 12px; font-family: monospace; box-shadow: var(--shadow-sm); white-space: nowrap; flex-shrink: 0; }
.ll-c-blue { color: var(--blue); } .ll-c-orange { color: var(--orange); } .ll-c-green { color: var(--green); } .ll-c-purple { color: var(--purple); } .ll-c-red { color: var(--red); }

/* Matrix Layout */
.ll-matrix-area { display: flex; padding: 10px 16px; width: 100%; min-width: 0; box-sizing: border-box; }
.ll-matrix-card { width: 100%; background: transparent; border: none; padding: 0; min-width: 0; }
.ll-card-title { font-size: 12px; font-weight: 700; color: var(--text2); font-family: monospace; margin-bottom: 8px; }
.ll-empty-matrix-msg { padding: 24px 16px; text-align: center; color: var(--muted); font-size: 12px; font-weight: 600; border: 1px dashed var(--border2); border-radius: var(--radius-sm); background: var(--surface2); }
.ll-empty-graph-msg { display: flex; align-items: center; justify-content: center; height: 100%; width: 100%; color: #64748b; font-size: 14px; font-weight: 600; text-align: center; }
.ll-graph-svg { width: 100%; height: 220px; background: #fafafa; border-radius: var(--radius-sm); border: 1px solid var(--border); }

/* SVG Graph Elements */
.ll-edge-line { stroke: #94a3b8; stroke-width: 2.5; transition: all 0.25s ease; }
.ll-edge-active { stroke: #f97316; stroke-width: 4; stroke-dasharray: 6 3; animation: ll-dash 1s linear infinite; }
@keyframes ll-dash { to { stroke-dashoffset: -18; } }

/* Hover Highlights for Graph Edges & Nodes */
/* Edge & Weight Badge (Blue) */
.ll-edge-line.ll-edge-hovered {
  stroke: #3b82f6 !important;
  stroke-width: 4px !important;
  filter: drop-shadow(0 0 6px rgba(59, 130, 246, 0.6));
  transition: all 0.15s ease;
}
.ll-weight-bg { fill: #ffffff; stroke: #94a3b8; stroke-width: 1.5; }
.ll-weight-bg-active { fill: #fff7ed; stroke: #f97316; stroke-width: 2.5; }
.ll-weight-bg-hovered {
  fill: #eff6ff !important;
  stroke: #3b82f6 !important;
  stroke-width: 2.5px !important;
  transition: all 0.15s ease;
}
.ll-weight-text { font-size: 11px; font-weight: 800; font-family: monospace; text-anchor: middle; fill: #1e293b; }
.ll-weight-text-hovered {
  fill: #1d4ed8 !important;
  font-weight: 900 !important;
}

/* Source Node (Orange) */
.ll-node-circle { fill: #eff6ff; stroke: #3b82f6; stroke-width: 2.5; transition: all 0.25s ease; }
.ll-node-u { fill: #dbeafe !important; stroke: #3b82f6 !important; stroke-width: 3.5 !important; }
.ll-node-v { fill: #f3e8ff !important; stroke: #9333ea !important; stroke-width: 3.5 !important; }
.ll-node-hover-src {
  fill: #fff7ed !important;
  stroke: #f97316 !important;
  stroke-width: 3.5px !important;
  filter: drop-shadow(0 0 8px rgba(249, 115, 22, 0.55));
  transition: all 0.15s ease;
}
.ll-node-text { font-size: 13px; font-weight: 800; font-family: monospace; text-anchor: middle; fill: #1e293b; }
.ll-node-text-hover-src {
  fill: #c2410c !important;
  font-weight: 900 !important;
}
.ll-svg-ptr-hover-src {
  fill: #ea580c !important;
  font-weight: 900 !important;
}

/* Destination Node (Light Green) */
.ll-node-hover-tgt {
  fill: #f0fdf4 !important;
  stroke: #22c55e !important;
  stroke-width: 3.5px !important;
  filter: drop-shadow(0 0 8px rgba(34, 197, 94, 0.55));
  transition: all 0.15s ease;
}
.ll-node-text-hover-tgt {
  fill: #15803d !important;
  font-weight: 900 !important;
}
.ll-svg-ptr-hover-tgt {
  fill: #16a34a !important;
  font-weight: 900 !important;
}

.ll-svg-ptr { font-size: 13px; font-weight: 800; font-family: monospace; text-anchor: middle; }
.ll-svg-ptr-blue { fill: #3b82f6; }
.ll-svg-ptr-purple { fill: #9333ea; }

/* 2D Matrix Table */
.ll-matrix-grid-wrap { overflow-x: auto; width: 100%; }
.ll-matrix-table { border-collapse: collapse; margin-top: 4px; font-family: monospace; table-layout: fixed; }
.ll-corner-th { background: var(--surface2); color: var(--muted); font-size: 11px; padding: 6px 4px; border: 1px solid var(--border); width: 68px; min-width: 68px; max-width: 68px; text-align: center; }
.ll-matrix-table th { background: var(--surface2); color: var(--text); font-size: 12px; font-weight: 700; padding: 4px 6px; border: 1px solid var(--border); text-align: center; width: 44px; min-width: 44px; }
.ll-th-active-u { background: #dbeafe !important; color: #1e40af !important; }
.ll-th-active-v { background: #f3e8ff !important; color: #5b21b6 !important; }

.ll-matrix-cell { width: 44px; min-width: 44px; height: 38px; text-align: center; border: 1px solid var(--border); font-size: 14px; font-weight: 700; background: #ffffff; color: var(--muted); transition: all 0.2s ease; }
.ll-cell-has-edge { background: #eff6ff; color: #1e293b; font-weight: 800; border-color: #93c5fd; }
.ll-cell-active { background: #fff7ed !important; border: 2px solid #f97316 !important; color: #c2410c !important; font-size: 16px !important; font-weight: 900 !important; transform: scale(1.04); }
.ll-cell-cur { background: #dcfce7 !important; border-color: #22c55e !important; color: #15803d !important; }
.ll-cell-hovered {
  background: #eff6ff !important;
  border: 2px solid #3b82f6 !important;
  color: #1d4ed8 !important;
  font-weight: 900 !important;
  transform: scale(1.06);
}

/* Pointer Tag Styling */
.ll-ptr-tag-wrap { height: 18px; display: flex; align-items: flex-end; justify-content: center; gap: 4px; margin-bottom: 1px; }
.ll-ptr-lbl { font-size: 11px; font-weight: 800; font-family: 'Consolas', monospace; display: inline-flex; flex-direction: column; align-items: center; line-height: 1; gap: 0px; white-space: nowrap; }
.ll-ptr-lbl::after { content: '↓'; font-size: 9px; font-weight: 900; line-height: 1; }
.ll-lbl-blue { color: #3b82f6; }
.ll-lbl-orange { color: #f97316; }
.ll-lbl-purple { color: #9333ea; }
.ll-lbl-green { color: #10b981; }

.ll-vresizer { height: 5px; cursor: row-resize; background: var(--border); flex-shrink: 0; transition: background .15s; position: relative; z-index: 20; }
.ll-vresizer:hover, .ll-vresizer.drag { background: var(--coral); }
.ll-legend { display: flex; flex-wrap: wrap; gap: 6px 14px; padding: 6px 12px; border-bottom: 1px solid var(--border); flex-shrink: 0; background: var(--surface2); }
.ll-leg { display: flex; align-items: center; gap: 5px; font-size: 11px; color: var(--text2); font-weight: 500; }
.ll-legdot { width: 11px; height: 11px; border-radius: 3px; flex-shrink: 0; display: inline-block; }
.ll-legdot-existing { background: #ffffff; border: 1.5px solid var(--border); }
.ll-legdot-has-edge { background: #eff6ff; border: 1.5px solid #93c5fd; }
.ll-legdot-u { background: #dbeafe; border: 1.5px solid #3b82f6; }
.ll-legdot-v { background: #f3e8ff; border: 1.5px solid #9333ea; }
.ll-legdot-active { background: #fff7ed; border: 1.5px solid #f97316; }

.ll-table-area { flex-shrink: 0; padding: 8px 14px; border-bottom: 1px solid var(--border); overflow-x: hidden; overflow-y: auto; background: var(--surface); min-width: 0; box-sizing: border-box; }
.ll-table-title { font-size: 10px; color: var(--muted); margin-bottom: 4px; font-style: italic; }
.ll-stack-line { font-family: 'Consolas', monospace; font-size: 12px; line-height: 1.8; }
.ll-frame { font-family: 'Consolas', monospace; font-size: 11.5px; color: var(--text2); padding: 1px 0; white-space: nowrap; }
.ll-frame-cur { color: var(--orange); background: var(--orange-light); border-radius: 4px; padding: 1px 5px; }
.ll-fname { color: var(--text2); }
.ll-now { color: var(--orange); font-size: 10px; margin-left: 6px; }
.ll-badge-wrap { padding: 6px 10px; border-bottom: 1px solid var(--border); flex-shrink: 0; min-height: 36px; display: flex; align-items: center; background: var(--surface); }
.ll-badge { display: inline-block; padding: 4px 12px; border-radius: var(--radius-sm); border-left: 3px solid var(--coral); background: var(--coral-light); font-size: 11px; color: var(--coral-dark); line-height: 1.4; word-break: break-word; font-weight: 500; }
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
