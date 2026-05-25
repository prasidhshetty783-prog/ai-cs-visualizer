# Contributing to the Interactive AI & CS Visualizer

First — thank you. Every contribution, whether it's a new algorithm, a better analogy, or a typo fix, makes this resource more valuable for learners everywhere.

This guide explains everything you need to know to contribute effectively.

---

## Table of Contents

1. [Project Philosophy](#1-project-philosophy)
2. [Types of Contributions](#2-types-of-contributions)
3. [Understanding the Data Model](#3-understanding-the-data-model)
4. [Adding a New Algorithm (Node) to an Existing Topic](#4-adding-a-new-algorithm-node-to-an-existing-topic)
5. [Adding a New Topic Tab](#5-adding-a-new-topic-tab)
6. [Adding a New Section within a Topic](#6-adding-a-new-section-within-a-topic)
7. [Adding Compatibility Data Between Nodes](#7-adding-compatibility-data-between-nodes)
8. [UI and Engine Contributions](#8-ui-and-engine-contributions)
9. [Pull Request Guidelines](#9-pull-request-guidelines)
10. [Content Quality Standards](#10-content-quality-standards)

---

## 1. Project Philosophy

**Learning first.** Every piece of content must serve the learner — not the author. Before adding something, ask: *will someone who has never seen this concept understand it better after reading this?*

**Plain English over jargon.** Every node has a "plain English" section. Write it as if you're explaining to a smart friend who has never studied ML or CS. Use concrete analogies. Make it click.

**One file, zero dependencies.** The entire app is `preview.html`. No npm, no build step, no framework. Keep it that way. It must work by double-clicking the file.

**Accuracy matters.** Plain-English explanations should be accessible but not wrong. If a simplification sacrifices accuracy, add a technical note to clarify.

---

## 2. Types of Contributions

| Type | Difficulty | Impact |
|---|---|---|
| Fix a typo or improve an analogy | ⭐ Easy | Immediate clarity improvement |
| Add a new algorithm to an existing section | ⭐⭐ Medium | Expands learning for that topic |
| Add compatibility data between two nodes | ⭐⭐ Medium | Enriches the comparison tool |
| Add a new section within an existing topic | ⭐⭐⭐ Medium-Hard | Expands a whole topic area |
| Add an entirely new topic tab | ⭐⭐⭐⭐ Hard | Major new learning domain |
| UI/engine improvements | ⭐⭐⭐ Varies | Better experience for all users |

Start with what you know. If you're an expert in transformers, add the Deep Learning tab. If you're great at explaining things, improve existing analogies.

---

## 3. Understanding the Data Model

Every topic area (Supervised Learning, Reinforcement Learning, etc.) is a JavaScript object with this exact shape:

```js
const MY_TOPIC = {

  // ── Connection type colours (hex) for this topic's edge types
  cclr: {
    'extends_to':    '#3b82f6',
    'compare_to':    '#f59e0b',
    'builds_on':     '#10b981',
    // ...add your own types with unique colours
  },

  // ── Human-readable labels for those edge types
  clbl: {
    'extends_to': 'extends to',
    'compare_to': 'compare',
    'builds_on':  'builds on',
  },

  // ── Per-node visual metadata
  vmeta: {
    node_id: {
      dot:  '#3b82f6',   // dot colour on the node card
      tBg:  '#dbeafe',   // tag background colour
      tFg:  '#1d4ed8',   // tag text colour
      tags: ['#tag1', '#tag2', '#tag3']  // searchable keyword tags
    },
    // ...one entry per node id
  },

  // ── Section groups (the coloured band backgrounds)
  groups: [
    {
      label: '① Section Name',   // ① ② ③ prefix + display name
      ids:   ['node_a', 'node_b', 'node_c'],   // node ids in this section
      color: '#8b5cf6'           // band colour (also used for label text)
    },
    // ...one entry per section
  ],

  // ── Compatibility data (for the two-node comparison tool)
  compat: {
    'node_a|node_b': {
      ok:         true,     // are these compatible / do they work together?
      why:        'Explanation of WHY they are or aren\'t compatible.',
      analogy:    'A plain-English analogy for the relationship.',
      need:       [],       // if ok:false, list of bridge node ids needed
      needExplain:'How to bridge the gap (empty string if ok:true)'
    },
    // ...one entry per pair you want to explain
  },

  // ── The nodes themselves
  blocks: [
    {
      id:     'node_a',                      // unique snake_case id
      label:  'Human-Readable Name',         // shown on the card
      desc:   'One sentence. What it does.',  // shown on card body (keep under 100 chars)
      detail: 'Full technical explanation. Can use <strong>, <em>, <br>.',
      simple: '🎯 <strong>Plain English:</strong> Your analogy here. Make it vivid.',
      useCases: [
        'Real-world use case 1',
        'Real-world use case 2',
        'Real-world use case 3',
      ],
      tools: [
        'library_name.ClassName() — one-line note on when/why to use it',
        'another_tool — brief note',
      ],
      pos:   { x: 0, y: 0 },   // grid position: column (x) and row (y)
                                 // x steps by COL_W=260px, y steps by ROW_H=190px
      conns: [
        { t: 'target_node_id', c: 'connection_type' },
        // ...one entry per outgoing connection
      ]
    },
    // ...one entry per node
  ]
};
```

### Position Grid

Nodes are placed on a grid. `pos: {x: 0, y: 0}` is top-left.

- `x` increases to the right (each step = 260px)
- `y` increases downward (each step = 190px)
- Leave `y` gaps between sections (e.g., section 1 at `y:0`, section 2 at `y:2`) so group bands don't overlap

```
x=0      x=1      x=2      x=3
y=0 [A] ──► [B] ──► [C]
y=1
y=2 [D] ──► [E]
```

### Section Labels

The `groups[].label` string must start with a circled number (① ② ③ ④) followed by the section name. The display panel strips this prefix automatically:
- `'① Classification Problems'` → shown as "Classification Problems"

### GROUP_INFO

After adding a new section, you must also add an entry in the `GROUP_INFO` object (search for `const GROUP_INFO =` in `preview.html`). This powers the section info panel:

```js
'① Your Section Name': {
  icon: '🎯',       // emoji shown in the panel header
  what: 'What this section is (2-3 sentences).',
  why:  'Why these nodes belong together (used only for Atlassian tab, omit/leave empty for ML tabs).',
  analogy: 'A plain-English analogy for the whole section.',
  color: '#8b5cf6'
},
```

---

## 4. Adding a New Algorithm (Node) to an Existing Topic

**Example: Adding "Gradient Boosting" to Supervised Learning**

1. Open `preview.html`
2. Find `const ML_SL = {` (or whichever topic you're adding to)
3. Add your node to the `blocks` array:

```js
{
  id:     'gradient_boosting',
  label:  'Gradient Boosting',
  desc:   'Builds trees sequentially; each tree corrects the errors of the previous one.',
  detail: 'Gradient Boosting fits a new decision tree to the <em>residuals</em> (errors) of all previous trees. Learns slowly (low learning rate) but extremely accurately. Base of XGBoost, LightGBM, CatBoost.',
  simple: '🏗 <strong>Plain English:</strong> Imagine a team of builders fixing a house. Builder 1 does what they can. Builder 2 comes in and only fixes what Builder 1 missed. Builder 3 fixes what Builder 2 missed. The final house is much better than any single builder could manage.',
  useCases: [
    'Tabular competition winner (Kaggle leaderboards)',
    'Credit risk scoring',
    'Click-through rate prediction in ads',
  ],
  tools: [
    'sklearn.ensemble.GradientBoostingClassifier()',
    'xgboost.XGBClassifier() — faster, regularised version',
    'lightgbm.LGBMClassifier() — very fast on large datasets',
  ],
  pos:   { x: 5, y: 0 },  // place after XGBoost in Classification row
  conns: [{ t: 'xgboost', c: 'compare_to' }]
}
```

4. Add vmeta for your node:

```js
gradient_boosting: {dot:'#ef4444', tBg:'#fee2e2', tFg:'#b91c1c', tags:['#boosting','#sequential','#residuals']},
```

5. Add your node's id to the relevant group's `ids` array:

```js
{label:'① Classification Problems', ids:['linear_clf','logistic_regression','knn','svm','random_forest','xgboost','gradient_boosting'], color:'#8b5cf6'},
```

6. Open the file in your browser and verify the node appears, the panel opens correctly, and the connection renders.

---

## 5. Adding a New Topic Tab

This is the biggest contribution type — adding an entirely new learning domain (e.g., Deep Learning, Data Structures, System Design).

**Steps:**

### 5a. Create the view data object

Follow the full data model from Section 3. Place it near the other ML datasets (search for `const ML_SL =`).

```js
const MY_NEW_TOPIC = {
  cclr: { ... },
  clbl: { ... },
  vmeta: { ... },
  groups: [ ... ],
  compat: { ... },
  blocks: [ ... ]
};
```

### 5b. Add GROUP_INFO entries

For every section in your topic, add an entry to `GROUP_INFO` (search for `const GROUP_INFO =`).

### 5c. Add a hub card (if adding a sub-tab under ML)

If adding under the Machine Learning tab, add a hub card in the `#ml-hub` HTML section:

```html
<div class="hub-card" onclick="enterMLView('my_topic')">
  <div class="hub-icon">🧠</div>
  <h2>My Topic Name</h2>
  <p>One sentence describing what learners will explore here.</p>
  <div class="hub-tags">
    <span class="hub-tag" style="background:#ede9fe;color:#6d28d9">Subtopic A</span>
    <span class="hub-tag" style="background:#dbeafe;color:#1d4ed8">Subtopic B</span>
  </div>
</div>
```

### 5d. Register the view in ML_META

Add your topic to the `ML_META` object:

```js
const ML_META = {
  supervised:    { data: ML_SL, label: 'Supervised Learning',    sub: '...' },
  unsupervised:  { data: ML_UL, label: 'Unsupervised Learning',  sub: '...' },
  reinforcement: { data: ML_RL, label: 'Reinforcement Learning', sub: '...' },
  my_topic:      { data: MY_NEW_TOPIC, label: 'My Topic Name',   sub: 'Brief subtitle' }, // ← add this
};
```

### 5e. (Advanced) Add an entirely new top-level tab

If you're adding a brand new top-level tab (not under ML), you'll also need to:
- Add a `<button class="tab-btn">` to `#tab-bar`
- Add a new hub `<div>` (like `#ml-hub`) to the HTML
- Extend the `switchTab()` function to handle the new tab key

---

## 6. Adding a New Section within a Topic

1. Add your nodes to `blocks[]` with positions in a new row (e.g., `y: 4` if existing sections use `y: 0` and `y: 2`)
2. Add a new entry to `groups[]`:
   ```js
   {label:'③ My New Section', ids:['node_x','node_y'], color:'#10b981'}
   ```
3. Add a `GROUP_INFO` entry (see Section 3)
4. Optionally add connections between your new nodes and existing ones in `conns[]`

---

## 7. Adding Compatibility Data Between Nodes

The compatibility checker shows what happens when a user selects two non-directly-connected nodes. It's one of the most educational features.

Add entries to the `compat` object:

```js
'node_a|node_b': {
  ok: true,
  why: 'Why these two work together or complement each other.',
  analogy: 'A memorable real-world analogy.',
  need: [],
  needExplain: ''
},
'node_c|node_d': {
  ok: false,
  why: 'Why these two don\'t connect directly.',
  analogy: 'An analogy for the incompatibility.',
  need: ['bridge_node_id'],
  needExplain: 'How to use bridge_node to connect them. Concrete steps.'
},
```

**The key is `'id1|id2'` where `id1` comes before `id2` alphabetically** — the app checks both orderings so don't worry about direction.

**Quality bar for compat entries:**
- `why` should be technically accurate and specific (not "they're different things")
- `analogy` should be vivid and memorable
- If `ok: false`, `needExplain` should give concrete steps, not vague hints

---

## 8. UI and Engine Contributions

The rendering engine lives entirely in the `<script>` block of `preview.html`. Key areas for improvement:

- **Accessibility** — keyboard navigation between nodes, ARIA labels, screen reader support
- **Search** — filter/highlight nodes by tag or keyword
- **Mobile layout** — touch gestures for pan/zoom, responsive panel sizing
- **Export** — save the current canvas view as PNG or SVG
- **Performance** — optimise rendering for topics with 50+ nodes
- **Dark mode** — CSS variable theming

For engine changes, please include a brief explanation in your PR of what you changed and why, since the engine is tightly coupled.

---

## 9. Pull Request Guidelines

1. **Fork** the repo and create a branch: `git checkout -b add-deep-learning-tab`
2. Make your changes in `preview.html`
3. **Test locally** — open the file in at least one browser and verify:
   - All your new nodes render correctly
   - Detail panels open and display all fields
   - Group panels open with correct content
   - Connections render without overlapping
   - The file still works (pan, zoom, tab switching) after your change
4. **One PR per topic** — don't mix unrelated changes
5. Write a clear PR description:
   - What you added/changed
   - Why it matters for learners
   - Any design decisions you made

**PR title format:**
- `feat: Add deep learning tab (CNNs, RNNs, Transformers)`
- `fix: Correct DBSCAN analogy`
- `content: Add gradient boosting node to supervised learning`
- `ui: Add keyboard navigation between nodes`

---

## 10. Content Quality Standards

Every piece of content in this project must meet these standards:

### For `desc` (card body — one sentence)
- Under 100 characters
- States what the algorithm **does**, not what it is called
- No jargon without explanation

### For `detail` (technical panel section)
- Accurate to current research/practice
- Can use HTML tags (`<strong>`, `<em>`, `<br>`)
- Should give someone enough to have an intelligent conversation about this topic

### For `simple` (plain-English analogy)
- Must start with an emoji
- Must use `<strong>Plain English:</strong>` prefix
- The analogy must actually illuminate the mechanism — not just be loosely related
- If someone reads only this and nothing else, they should walk away with the right mental model

### For `useCases`
- Real, specific examples — not "used in industry" or "used in research"
- Name actual domains or products where possible
- 3–5 items is ideal

### For `tools`
- Real library/tool names with the actual class or function name
- A brief note on *when* to use this tool vs alternatives
- Keep it Python/standard unless the topic demands otherwise

### For `compat` entries
- Both `ok:true` and `ok:false` entries are valuable
- Every pair in an algorithm family should have a compat entry
- The `analogy` is the most memorable part — spend the most time on it

---

## Questions?

Open a GitHub Issue with the label `question`. We're here to help new contributors get their first PR merged.

Thank you for making this resource better for learners everywhere. 🌍
