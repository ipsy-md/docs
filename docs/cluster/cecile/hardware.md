# Cecile hardware

## Overview

Cecile comprises a head node, a storage node and 7 compute nodes amounting to 352 CPU cores and 3,3 TB of RAM.

### Head node

- **CPU:** 1x32 core AMD Epyc 7513
- **RAM:** 512GB DDR4-3200 ECC REG (16x32GB)
- **Network card:** 1x10Gb Intel X550-T2

### Storage node

- **CPU:** 1x16 core AMD Epyc 7302P
- **RAM:** 256GB DDR-3200 ECC Reg (16x16GB)
- **Network card:** 1x10Gb Intel X550-T2
- **SSD:**
    - 8x7.86TB Micron 7300 Pro
- **HDD:**
    - 12x6TB WD Ultrastar DC Hc310
    - not installed yet: 20x6TB Toshiba MG06SCA600E (some temporarily used in Medusa)

### Compute nodes

- **Nodes 1-2:**

    - **CPU:** 2x32 core AMD Epyc 7513
    - **RAM:** 512MB DDR4-3200 ECC REG (16x32GB)
    - **Network:** 2x10Gib/s

- **Node 3:**

    - **CPU:** 2x10-core 2.3 GHz Intel Xeon E5-2650v3
    - **RAM:** 96GB DDR3 RAM
    - **Network:** 2x10Gib/s

- **Node 4:**

    - **CPU:** 4x8 core 2.6 GHz AMD Opteron 6282 SE
    - **RAM:** 384GB RAM (48x8GB DDR3 ECC)
    - **Network:** 2x10Gib/s

- **Node 5:**

    - **CPU:** 4x4-core AMD Opteron 6320
    - **RAM:** 512GB DDR3 RAM
    - **Network:** 2x10Gib/s

### Backup

Proxmox system, ~100TB of usable space, provided by the URZ.
