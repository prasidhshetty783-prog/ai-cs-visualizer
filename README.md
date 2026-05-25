# 🌐 Interactive AI & CS Visualizer

> **Learn anything in AI and Computer Science — visually, interactively, and for free.**

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Contributions Welcome](https://img.shields.io/badge/contributions-welcome-brightgreen.svg)](CONTRIBUTING.md)
[![Open Source](https://img.shields.io/badge/Open%20Source-%E2%9D%A4-red.svg)]()

---

## What Is This?

This is a single-file, zero-dependency interactive visualizer that lets you **explore complex AI and Computer Science concepts as connected, living diagrams** — not boring text or static images.

Click a node to understand what it does. Click two nodes to see if they're compatible, why they are or aren't, and what you'd need to connect them. Click a section band to understand *why* those things belong together. Pan, zoom, and explore at your own pace.

**Currently covers:**
- ⬡ **API Gateway & Edge Fleet Architecture** — A real-world production system: how Atlassian routes every customer request through 2,000+ Envoy proxies globally, with automatic infrastructure provisioning and a live configuration control plane
- 🤖 **Machine Learning** — Three complete learning paradigms:
  - 📊 Supervised Learning (Classification + Regression, 9 algorithms)
  - 🔍 Unsupervised Learning (Partitional + Hierarchical Clustering, 6 algorithms)
  - 🎮 Reinforcement Learning (Value-Based + Policy-Based methods, 4 algorithms)

**The vision:** Grow this into the world's most comprehensive interactive learning resource for AI and CS — covering deep learning, system design, data structures, algorithms, distributed systems, and beyond. Built by the community, for the community.

---

## ✨ Features

- **Zero setup** — one HTML file, open in any browser, no server needed
- **Interactive detail panels** — plain-English analogies, technical depth, real-world use cases, and recommended tools for every node
- **Compatibility checker** — select any two nodes to see whether they connect, why or why not, and a real-world analogy explaining the relationship
- **Section info panels** — click any group band to understand what that section is and why those elements belong together
- **Smart panel positioning** — detail panels reposition themselves during zoom and pan, never going off-screen
- **Multi-view architecture** — tab-based navigation between completely different topic areas with a shared rendering engine
- **Connection type legend** — colour-coded edge types update per view

---

## 🚀 Getting Started

### Run it instantly

1. Download `index.html`
2. Open it in Chrome, Firefox, Safari, or Edge
3. That's it — no npm, no Python, no server

### Clone and explore

```bash
git clone https://github.com/YOUR_USERNAME/ai-cs-visualizer.git
cd ai-cs-visualizer
open index.html   # macOS
# or: start index.html  (Windows)
# or: xdg-open index.html  (Linux)
```

---

## 🗺 Project Structure

```
ai-cs-visualizer/
├── index.html          ← The entire app — one self-contained file
├── README.md             ← You are here
├── CONTRIBUTING.md       ← How to add new topics and algorithms
└── LICENSE               ← MIT
```

The entire application — HTML, CSS, JavaScript, all data — lives in `index.html`. This is intentional: zero-dependency, zero-build-step, runs anywhere.

---

## 🧠 How It Works (Quick Technical Overview)

The app is a hand-crafted SVG rendering engine with:

- **SVG canvas** with pan and zoom (mouse wheel + drag)
- **Orthogonal edge routing** between nodes
- **Smart modal positioning** — 4-position fallback algorithm that reads actual rendered panel height
- **Multi-view data engine** — global arrays swapped in-place so a single rendering pipeline serves all topic areas
- **Per-node data model**: plain description, technical detail, plain-English analogy, use cases, recommended tools, connections

See [CONTRIBUTING.md](CONTRIBUTING.md) for a full guide on adding new topics and nodes.

---

## 🗺 Roadmap

The community can expand this to cover any topic. Suggested areas:

| Area | Status |
|---|---|
| API Gateway & Edge Architecture | ✅ Complete |
| Supervised Learning | ✅ Complete |
| Unsupervised Learning | ✅ Complete |
| Reinforcement Learning | ✅ Complete |
| Deep Learning (CNNs, RNNs, Transformers) | 🔜 Planned |
| Large Language Models & RLHF | 🔜 Planned |
| Data Structures (Trees, Graphs, Heaps) | 🔜 Planned |
| Sorting & Search Algorithms | 🔜 Planned |
| System Design Patterns | 🔜 Planned |
| Distributed Systems | 🔜 Planned |
| Database Internals | 🔜 Planned |
| AI Integration for recommending video links | 🔜 Planned |
| AI integration to create animated visualisation | 🔜 Planned |
| *Your idea here* | 💡 Open |

---

## 🤝 Contributing

We welcome contributions of all kinds — new topic areas, new algorithms within existing topics, better analogies, typo fixes, UI improvements, and accessibility enhancements.

**See [CONTRIBUTING.md](CONTRIBUTING.md) for the full guide.**

The short version:
1. Fork this repo
2. Add your content following the data model in CONTRIBUTING.md
3. Test it locally (just open the file)
4. Open a pull request

---

## 📄 License

MIT — free to use, share, modify, and build on. See [LICENSE](LICENSE).

---

## 🙏 Acknowledgements

- The Atlassian Edge Platform architecture is based on the public talk by **Vasilios Syrakis** on Atlassian's internal edge routing infrastructure
- Built with vanilla HTML, CSS, and JavaScript — no frameworks, no dependencies
- Inspired by the idea that the best way to understand a complex system is to *see* it

---

*If this helped you learn something, give it a ⭐ and share it with someone who's studying AI or CS.*


