
---
date: 2026-09-18

source: "https://youtu.be/I3LBYMXBhus"

---

# Seven Second Subnetting (Fast IPv4 Calculation)

A method for quickly calculating a range [[IPv4]] without manually converting to binary. Allows you to determine in seconds: **Network ID**, **Broadcast** and **First/Last Usable IP**.

---

## Reference Chart

|           CIDR Prefix           | Subnet Mask (Dec) | Subnets / Multiplier | Total Addresses per Subnet |
| :-----------------------------: | :---------------: | :------------------: | :------------------------: |
| `/1` \| `/9` \| `/17` \| `/25`  |      `.128`       |          2           |          **128**           |
| `/2` \| `/10` \| `/18` \| `/26` |      `.192`       |          4           |           **64**           |
| `/3` \| `/11` \| `/19` \| `/27` |      `.224`       |          8           |           **32**           |
| `/4` \| `/12` \| `/20` \| `/28` |      `.240`       |          16          |           **16**           |
| `/5` \| `/13` \| `/21` \| `/29` |      `.248`       |          32          |           **8**            |
| `/6` \| `/14` \| `/22` \| `/30` |      `.252`       |          64          |           **4**            |
| `/7` \| `/15` \| `/23` \| `/31` |      `.254`       |         128          |           **2**            |
| `/8` \| `/16` \| `/24` \| `/32` |      `.255`       |         256          |           **1**            |

---

## 4-Step Subnetting Algorithm

1. **Convert CIDR to Dec**
2. **Find Network Address**
3. **Find Broadcast Address**
4. **Determine Usable Range**
    * **Firest IP:** `Network Address +1`
	* **Last IP:** `Broadcast Address -1`

---
## Exaples

## First

* **IP:** `165.245.12.88/26`
* **Mask:** `/26`, `255.255.255.192`
* **Block size:** `64` (Blocks: 0 - 63, 64 - 127, 128 - 191, 192 - 255) 
* x.x.x.`88` in `64 - 127`
	* **Network address:** `165.245.12.64`
	* **Broadcast Address:** `165.245.12.127`
	* **First Usable IP:** `165.245.12.65`
	* **Last Usable IP:** `165.245.12.126`

## Second 

* **IP:** `165.245.12.188/20`
* **Mask:** `/20`, `255.255.240.0`
* **Block size:** `16` (Blocks: 0 - 15, 16 - 31, 32 - 47, 48 - 63, 64 - 79, 80 - 95, 96 - 111, 112 - 127, 128 - 143, 144 - 159, 160 - 175, 176 - 191, 192 - 207, 208 - 223, 224 - 239, 240 - 255) 
* x.x.`12`.x in `0 - 15`
	* **Network address:** `165.245.0.0`
	* **Broadcast Address:** `165.245.15.255`
	* **First Usable IP:** `165.245.0.1`
	* **Last Usable IP:** `165.245.15.254`

---

## Terminal Verification

Use the utility `ipcalc`:
* `ipcalc 165.245.12.88/26`
