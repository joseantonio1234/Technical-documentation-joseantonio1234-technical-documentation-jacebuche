## 💡 Technical Documentation: No Display – GPU Test & Troubleshooting

**Author:** Jose Antonio Acebuche  
**Date:** *06/28/2025*  
**Category:** Desktop Troubleshooting / Hardware Diagnosis

---

### 🌟 Purpose
To diagnose and troubleshoot a PC unit experiencing **no video display output**, with a focus on identifying GPU (Graphics Processing Unit) failure or related causes.

---

### 🔍 Symptoms
- No signal on monitor  
- Monitor stays black or goes to standby  
- No POST or BIOS screen  
- Fans may spin, but system doesn’t boot visually

---

### ✅ Pre-Check
- Confirm the **monitor is powered** and working (test with another system)  
- Confirm **HDMI/VGA/DP cable** is connected properly and not damaged  
- Try **another monitor** and **cable** to rule out external faults

---

### 🛠️ Troubleshooting Procedure

#### 1. Check GPU Seating
- Power off the PC, unplug power cable  
- Open the side panel and **re-seat** the GPU  
- Ensure it **clicks securely into the PCIe slot**  
- Reconnect the power cables (if using 6/8-pin connectors)

#### 2. Test Without GPU (for systems with onboard graphics)
- Remove the GPU  
- Connect the monitor to the **motherboard video output**  
- Boot the PC  
- If display works, issue may be with the GPU

#### 3. Test GPU on Another System
- Install the suspected GPU on a known working system  
- If still no display → possible **GPU failure**  
- If display works → issue may be on motherboard or PSU

#### 4. Clear CMOS / BIOS Reset
- Remove CMOS battery for 1–2 minutes  
- Reinsert and boot system  
- This resets display preferences and possible GPU config issues

#### 5. Check PSU Wattage
- Ensure PSU provides enough power for GPU  
- A **failing or low-wattage PSU** can cause boot/no display symptoms

---

### ⚠️ Common Findings
| Issue                         | Root Cause                  | Resolution                         |
|------------------------------|-----------------------------|------------------------------------|
| No display even with GPU reseated | Faulty GPU | Replace GPU |
| Works with onboard video     | GPU failure or power issue  | Replace GPU or check PSU |
| No display with either output | Motherboard or PSU fault    | Further hardware testing |

---

### 🥺 Optional Tests
- Try a **different PCIe slot**  
- Use a **POST card** or **speaker** for beep code error messages  
- Use **multimeter** to test power output on PCIe connectors

---

### 📌 Summary
A "No Display" issue may originate from:
- Loose or damaged GPU  
- Faulty display cables or monitor  
- Dead GPU or motherboard  
- PSU failing to deliver power

Systematic testing — from cable checks to component swapping — helps isolate the failing part quickly and accurately.
