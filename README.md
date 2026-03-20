# US Election Visualization Engine

[![Java](https://img.shields.io/badge/Java-11+-ED8B00?logo=openjdk&logoColor=white)](https://openjdk.org/)
[![Maven](https://img.shields.io/badge/Maven-3.6+-C71A36?logo=apache-maven&logoColor=white)](https://maven.apache.org/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

**Interactive choropleth visualization of US House & Senate election data** — built with custom linked structures, Weighted Quick Union (Union-Find), and Flood Fill for real-time state hit-testing on a map canvas.

---

## Overview

This engine parses historical House and Senate election CSV data and renders an interactive US map with four visualization modes: total votes by state, year-over-year vote change, Senate seat winners, and House seat winners. Users click states to drill down into candidates, vote counts, and party affiliations.

The system is designed around core data-structure primitives: **singly linked lists** for years and elections, **circular linked lists** for states within a year, and a **Weighted Quick Union** over map pixels for O(α(n)) state identification on click.

---

## Tech Stack

| Component | Technology |
|-----------|------------|
| Language | Java 11+ |
| GUI | Java Swing (`javax.swing`, `java.awt`) |
| Build | Maven |
| Testing | JUnit 4 |
| Data Format | CSV (race_id, state, office, cycle, candidate, party, votes, winner) |

---

## Key Features

- **Nested linked data model**  
  Years → States (CLL) → Elections (SLL) for efficient traversal by cycle and geography.

- **Weighted Quick Union (WQU)**  
  Pixel-level Union-Find over the map image to cluster state regions and support O(α(n)) click-to-state lookup.

- **Flood Fill**  
  BFS-based fill from seed coordinates to assign pixels to states and build the WQU structure.

- **Four map modes**
  - Total Votes (white → green gradient)
  - Change in Votes (red/green by year-over-year delta)
  - Senate Elections (party colors by winning candidate)
  - House Elections (party colors by winning candidate)

- **Interactive state panel**  
  Click a state to view total/average votes, vote change, and per-race candidate breakdown.

---

## Quick Start

### Prerequisites

- **Java 11+**
- **Maven 3.6+**
- **US_Outline.jpg** in the project root (US outline map image; dimensions: 500×407)

### Build & Run

```bash
# Clone and enter project
git clone https://github.com/YOUR_USERNAME/us-election-visualization-engine.git
cd us-election-visualization-engine

# Build
mvn clean compile

# Run GUI
mvn exec:java -Dexec.mainClass="election.Driver"
```

Or run from your IDE by executing `election.Driver` as the main class. Ensure `house.csv`, `senate.csv`, and `US_Outline.jpg` are in the working directory.

### Run Tests

```bash
mvn test
```

---

## Project Structure

```
├── src/election/
│   ├── Driver.java           # Main Swing GUI & event handling
│   ├── ElectionAnalysis.java # CSV parsing, data queries, linked-structure ops
│   ├── MapVisualizer.java    # Map rendering, WQU, Flood Fill, hit-testing
│   ├── YearNode.java         # SLL node (year → states)
│   ├── StateNode.java        # CLL node (state → elections)
│   ├── ElectionNode.java     # SLL node (race, candidates, votes, parties)
│   ├── StdIn.java            # Princeton StdIn (file/stdin)
│   └── StdOut.java           # Princeton StdOut
├── test/
│   └── ElectionAnalysisTest.java
├── house.csv                 # House election records
├── senate.csv                # Senate election records
├── US_Outline.jpg            # US map image (required)
└── pom.xml
```

---

## Results & Evaluation

| Test | Description |
|------|-------------|
| `testReadYears` | Validates SLL of years from CSV |
| `testReadStates` | Validates CLL of states per year |

The visualization supports ~17K+ House and ~2K+ Senate records across multiple election cycles with smooth interaction and color-coded maps.

---

## Acknowledgments

- Map visualization concept inspired by [Rising Tides](https://github.com/nifty) (Vian Miranda) and [NIFTY](https://nifty.stanford.edu/) (Keith Schwarz).
- Original data processing framework by Colin Sullivan, Sumedh Sinha (Rutgers).

---

## License

MIT
