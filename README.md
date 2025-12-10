# Directed Graphlet-Based Network Comparison Distances

Software and Python scripts for computing distance measures between directed biological networks.

## Authors

- A. Sarajlic
- N. Malod-Dognin
- O. N. Yaveroglu
- N. Przulj

**Corresponding Author:** Prof. Natasa Przulj  
📧 natasa.przulj@mbzuai.ac.ae

## Overview

This repository provides tools for computing various directed network distance measures presented in the paper "Graphlet-based Characterization of Directed Networks". The toolkit enables quantitative comparison of directed network structures using directed graphlet-based methods.

**Paper:** ["Graphlet-based Characterization of Directed Networks"](https://doi.org/10.1038/srep35098)

## Directed Network Distance Measures

This toolkit computes six distance measures for directed networks:

1. **DGCD-13** - Directed Graphlet Correlation Distance using 2- to 3-node directed graphlet orbits
2. **DGCD-129** - Directed Graphlet Correlation Distance using 2- to 4-node directed graphlet orbits
3. **RDGF** - (Relative) Directed Graphlet Frequency Distribution Distance
4. **DGDDA** - Directed Graphlet Degree Distribution Agreement
5. **Directed Spectral Distance** - Using eigenvalues of directed network representations
6. **In- and Out-Degree Distribution Distances** - Comparing directed degree distributions

## Resources

### Tools Included

- **[C++ Directed Graphlet Counter](https://github.com/przulj-lab/directed-network-comparison/blob/main/Directed_Graphlet_Counter_v3.cpp)** - Efficient computation of directed graphlet degree vector (DGDV) signatures
- **[Python Network Comparison Script](https://github.com/przulj-lab/directed-network-comparison/blob/main/Directed_Distances_v2.py)** - Computing distances between directed networks

*Note: If download links don't work when clicked, copy and paste the URLs directly into your browser's address bar.*

## Requirements

- **C++ compiler** (for building the graphlet counter)
- **Python** (2.7 or 3.x)
- Networks in edge-list format
- Multi-processing support for parallel distance computation

## Installation

```bash
git clone https://github.com/przulj-lab/directed-network-comparison.git
cd directed-network-comparison

# Compile the C++ directed graphlet counter
g++ -O3 -o Directed_Graphlet_Counter_v3 Directed_Graphlet_Counter_v3.cpp
```

## Usage

### Step 1: Prepare Networks

All directed networks must be in **edge-list format**:
```
source1 target1
source2 target2
source3 target3
...
```

### Step 2: Compute Directed Graphlet Degree Vector Signatures

For each network, compute DGDV signatures using the C++ counter:

```bash
./Directed_Graphlet_Counter_v3 my_network
```

**Output:** Three files are produced:
1. `my_network.graphletcounts.txt` - Counts of directed graphlets in the network (for DRGF)
2. `my_network.signatures.txt` - Directed graphlet degree signatures (one per node, for DGCD)
3. `my_network.dictionary.txt` - Maps line numbers to node IDs

### Step 3: Compute Network Distances

Organize networks into cluster directories, then compute distances:

```bash
python Directed_Distances_v2.py clusters_folder test_mode process_count
```

**Parameters:**
- `clusters_folder` - Directory containing subdirectories for each network cluster
  - Each subdirectory should contain:
    - `.signatures.txt` files (directed graphlet signatures)
    - `.graphletcounts.txt` files (graphlet counts)
    - `.gw` files (LEDA format, required only for test_mode 3)
  - **Important:** File names must match exactly across file types
- `test_mode` - Distance measure to compute (see options below)
- `process_count` - Number of parallel processes (≥1, default: 4)

## Test Mode Options

| Test Mode | Distance Measure | Description |
|-----------|------------------|-------------|
| `1` | DGCD-13 | Directed GCD using 2-3 node orbits (default) |
| `2` | DGCD-129 | Directed GCD using 2-4 node orbits |
| `3` | RDGF | Relative Directed Graphlet Frequency distance |
| `4` | DGDDA | Directed Graphlet Degree Distribution Agreement |
| `5` | Spectral | Directed spectral distance |
| `6` | Degree | In- and Out-degree distribution distances |

## Example Workflow

### Complete Example

1. **Prepare networks in edge-list format**:
   ```bash
   clusters/
   ├── cluster1/
   │   ├── network1_edgelist.txt
   │   └── network2_edgelist.txt
   └── cluster2/
       └── network3_edgelist.txt
   ```

2. **Compute DGDV signatures for all networks**:
   ```bash
   ./Directed_Graphlet_Counter_v3 clusters/cluster1/network1_edgelist.txt
   ./Directed_Graphlet_Counter_v3 clusters/cluster1/network2_edgelist.txt
   ./Directed_Graphlet_Counter_v3 clusters/cluster2/network3_edgelist.txt
   ```
   
   This creates:
   ```bash
   clusters/
   ├── cluster1/
   │   ├── network1_edgelist.txt
   │   ├── network1_edgelist.signatures.txt
   │   ├── network1_edgelist.graphletcounts.txt
   │   ├── network1_edgelist.dictionary.txt
   │   ├── network2_edgelist.txt
   │   ├── network2_edgelist.signatures.txt
   │   ├── network2_edgelist.graphletcounts.txt
   │   └── network2_edgelist.dictionary.txt
   └── cluster2/
       ├── network3_edgelist.txt
       ├── network3_edgelist.signatures.txt
       ├── network3_edgelist.graphletcounts.txt
       └── network3_edgelist.dictionary.txt
   ```

3. **Compute DGCD-13 distance using 4 processes**:
   ```bash
   python Directed_Distances_v2.py clusters/ 1 4
   ```

4. **Compute other distance measures**:
   ```bash
   # DGCD-129 (2-4 node orbits)
   python Directed_Distances_v2.py clusters/ 2 4
   
   # RDGF distance
   python Directed_Distances_v2.py clusters/ 3 4
   
   # In/Out degree distributions
   python Directed_Distances_v2.py clusters/ 6 4
   ```

### Quick Start (Default Settings)

```bash
# Using default test_mode=1 (DGCD-13) and process_count=4
python Directed_Distances_v2.py clusters/
```

## Output

The script produces **pairwise distance matrices** showing distances between all networks within each cluster directory.

Results can be used for:
- Directed network classification
- Biological pathway analysis
- Social network comparison
- Web graph analysis
- Gene regulatory network studies

## File Structure

```
.
├── Directed_Graphlet_Counter_v3.cpp  # C++ source for DGDV computation
├── Directed_Graphlet_Counter_v3      # Compiled executable
├── Directed_Distances_v2.py          # Python distance computation script
├── clusters/                          # Network clusters directory
│   ├── cluster1/
│   │   ├── network1.txt
│   │   ├── network1.signatures.txt
│   │   ├── network1.graphletcounts.txt
│   │   └── network1.dictionary.txt
│   └── cluster2/
│       ├── network2.txt
│       ├── network2.signatures.txt
│       ├── network2.graphletcounts.txt
│       └── network2.dictionary.txt
└── README.md
```

## Implementation Details

**Implemented by:** Noel Malod-Dognin  
Based on scripts by Anida Sarajlic and Omer Nebil Yaveroglu

### Algorithm Steps

1. **Network Discovery**: Automatically identifies all network files in cluster subdirectories
2. **Signature Loading**: Reads directed graphlet degree signatures from `.signatures.txt` files
3. **Property Calculation**: Computes required network properties based on test mode
4. **Distance Computation**: Calculates pairwise distances using parallel processing
5. **Output Generation**: Produces distance matrices for network comparison

## Performance Notes

- **C++ Counter**: Optimized for fast directed graphlet enumeration
- **Parallel Processing**: Significantly speeds up distance calculations for large network sets
- **Memory Efficiency**: Processes networks in batches to handle large datasets
- Adjust `process_count` based on available CPU cores

## Related Tools

- **[Undirected Graphlet Distances](https://github.com/przulj-lab/network-comparison)** - For undirected network comparison
- **[GraphCrunch](https://github.com/przulj-lab/GraphCrunch/)** - Network analysis toolkit

## Troubleshooting

### Compilation Issues
Ensure you have a C++ compiler installed:
```bash
g++ --version
# or
clang++ --version
```

### Python Version Compatibility
If you encounter issues, try:
```bash
python3 Directed_Distances_v2.py clusters/ 1 4
```

### File Name Mismatches
Ensure that for each network `network_name`:
- `network_name.signatures.txt`
- `network_name.graphletcounts.txt`
- `network_name.dictionary.txt`

All have matching base names.

### Memory Issues
For very large networks:
- Use DGCD-13 instead of DGCD-129 (fewer orbits)
- Reduce the number of parallel processes
- Process networks in smaller batches

## Contact

For questions or issues, please contact Prof. Natasa Przulj at natasa.przulj@mbzuai.ac.ae
