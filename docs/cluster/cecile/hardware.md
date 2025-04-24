# Cecile hardware

## Overview

Cecile comprises a head node, a storage node and 7 compute nodes amounting to 352 CPU cores and 1.792 TB of RAM.

### Head node

- **CPU:** 1x AMD Epyc 7513, 32core
- **RAM:** 512GB DDR4-3200 ECC REG (16x32 GB)
- **SSD:** 
    - 1x960GB Samsung PM9A3, M.2, NVMe, 1 DWPD (Reading 5000MB/s, Writing 1400MB/s)
    - 1x3,84TB Micron 7300 PRO, M.2, NVMe, 1 DWPD
- **Network card:** 1x10Gb Intel X550-T2

### Storage node

- **CPU:** 1x AMD Epyc 7302P, 16core
- **RAM:** 256GB DDR-3200 ECC Reg (16x16GB)
- **SSD:**
    - 8x7,86TB Micron 7300 Pro, 1DWPD
    - 2x960GB Intel S4510, 1,3 DWDP
- **Network card:** 1x10Gb Intel X550-T2

### Compute nodes

- **Nodes 4-5:**

    - **CPU:** 1x AMD Epyc 7513, 32core, inkl Kühler
    - **RAM:** 512MB DDR4-3200 ECC REG (16x32 GB)
    - **SSD:** 1x960GB Samsung PM9A3, M.2, NVMe, 1 DWPD (Reading 5000MB/s, Writing 1400MB/s)

### Backup