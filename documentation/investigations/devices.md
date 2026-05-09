# 🔧 Identifying Chipsets & Required Drivers on a Linux‑Based Home Router

(From Microsoft COpilot)

Running Linux on a home router (OpenWrt, Debian, Alpine, etc.) often requires identifying the underlying chipsets so you can load the correct drivers and firmware. This guide lists the most effective Linux tools for discovering hardware on embedded router platforms.

---

## 🧩 1. Hardware Enumeration Tools (PCI, USB, SoC Buses)

These tools reveal *what hardware exists*, even if no driver is loaded.

### **lspci**
Useful for routers with PCIe Wi‑Fi chips (Atheros, Broadcom, MediaTek).

```
lspci -nn
```

### **lsusb**
For routers with USB Wi‑Fi, LTE modems, or storage controllers.

```
lsusb -v
```

### **lsdev**
Summarises devices from `/proc` (IRQ, DMA, I/O ports). Helpful on embedded systems with minimal PCI/USB.

---

## 🧩 2. Kernel Device & Driver Introspection

These tools show *what the kernel detects* and which drivers are bound.

### **dmesg**
The single most useful tool on embedded Linux. Reveals chipset detection, firmware loading, and driver failures.

```
dmesg | grep -i -e wifi -e ath -e mt76 -e brcm -e firmware
```

### **lsmod**
Lists loaded kernel modules (drivers).

```
lsmod
```

### **modinfo**
Shows which devices a driver supports (via modalias).

```
modinfo mt76
```

### **udevadm**
Displays device attributes and modalias strings used for driver matching.

```
udevadm info --export-db | grep -i modalias
```

---

## 🧩 3. SoC‑Specific Tools (ARM/MIPS Routers)

Most routers use SoCs from Qualcomm Atheros, MediaTek, Broadcom, or Realtek. These often hide internal buses, so SoC‑aware tools are essential.

### **/proc/cpuinfo**
Identifies the SoC family (e.g., *Atheros AR9344*, *MediaTek MT7621*).

```
cat /proc/cpuinfo
```

### **/proc/device-tree/**
Device tree nodes reveal Wi‑Fi, Ethernet PHYs, switches, GPIOs.

```
strings /proc/device-tree/compatible
```

### **ethtool**
Identifies Ethernet PHYs, switch chips, and offload capabilities.

```
ethtool -i eth0
```

---

## 🧩 4. OpenWrt‑Specific Tools (If Applicable)

OpenWrt includes excellent introspection utilities.

### **opkg list-installed | grep -i firmware**
Shows installed firmware packages.

### **logread**
Equivalent to `dmesg` but includes system logs.

### **ubus**
Queries system components (network, wireless, board info).

```
ubus call system board
```

---

## 🧩 5. Firmware & Board Data Inspection

Many Wi‑Fi chips require board‑specific calibration data.

### **strings /lib/firmware/**
Helps identify available or missing firmware blobs.

### **hexdump /dev/mtdX**
Used to inspect calibration partitions (ath9k, ath10k, mt76).

---

## 🧩 6. Modalias‑Based Driver Matching

Linux exposes a modalias string for each device. Feeding it to `modprobe` resolves the correct driver.

```
cat /sys/class/net/wlan0/device/modalias
modprobe --resolve-alias <modalias>
```

This is extremely effective when the chipset is unknown.

---

## 🧩 7. Network‑Specific Tools

### **iw / iwinfo**
Shows Wi‑Fi chipset, driver, and capabilities.

```
iw phy
iwinfo
```

### **ip link / ip addr**
Shows network interfaces and their driver bindings.

---

## 🧩 8. Switch / PHY Identification

Routers often include integrated switches.

### **swconfig** (OpenWrt)
Identifies switch chips (Atheros, Realtek, MediaTek).

```
swconfig list
```

### **mdio-tool**
Reads PHY registers to identify Ethernet PHYs.

---

## ⭐ Shortlist: The Most Useful Tools

If you only install a few tools, these provide the highest value:

1. **dmesg** — chipset detection, firmware loading
2. **lspci / lsusb** — PCI/USB enumeration
3. **udevadm info** — modalias → driver mapping
4. **ethtool** — Ethernet chipset info
5. **iw / iwinfo** — Wi‑Fi chipset info
6. **/proc/device-tree/** — SoC‑level hardware description

These will identify 95% of chipsets on a Linux‑based router.

---

If you'd like, I can also produce a **single self‑contained hardware‑identification script** that runs all these checks and outputs a clean summary.