# Filament Calibration Procedure: Creality Ender-3 V3 KE
**Software:** Creality Print (v5.x) / Orca Slicer

This guide provides a step-by-step process to perfectly tune settings for any specific filament. Please follow these steps in the order presented to ensure the best results.

---

## 1. Temperature Tower
*Goal: Find the temperature where layers adhere the strongest and the print looks cleanest.*

### Analysis & Calculation
1.  Print the tower (usually generates from high to low temperature).
2.  Evaluate visual quality (overhangs, bridges, stringing).
3.  **Strength Test:** Attempt to break the tower with your hands at the weakest looking points.
4.  **Selection:** Choose the lowest temperature that still offers **strong layer bonding** (does not snap easily) and maintains a glossy finish (for glossy materials).

### Where to enter in Creality Print?
* **Path:** Click "Edit" (pencil icon) next to Filament -> **Basic** (or Information) tab.
* **Parameter:** `Printing Temperature` (and `First Layer Temperature`).

---

## 2. Flow Rate
*Goal: Achieve a smooth top surface and dimensionally accurate walls.*

### Analysis & Calculation (Two-Pass / Two-Step Approach)

**Step 1: Coarse Adjustment**  
Print a flow rate test model ("chips," lines, or similar), each labelled with a flow modifier (e.g., -5, 0, +5)%.
1. Inspect each test chip and choose the one with the **smoothest top surface**—no ridges, burrs, or line gaps.
2. Note the modifier from that chip (e.g., +5).
3. **Formula:**  
   $$\text{NewFlow} = \text{OldFlow} \times \frac{100 + \text{Modifier}}{100}$$  
   *Example: Flow was 0.98, best chip is +5 → $0.98 \times 1.05 = 1.029$*

**Step 2: Fine-Tuning**  
Print another test model using the new flow rate from step 1.
1. Test several small flow rate percentage differences near your new value (e.g., +2, -2%, etc.).
2. Inspect the resulting prints for improved surface quality and dimensional accuracy—especially check for fit and appearance in intended prints.
3. Select the flow rate percentage difference (modifier) that yields the best results.
   - This step is for micro-adjustments: sometimes, small tweaks (+/- 1–2%) make a noticeable improvement for your specific filament and printer.
4. Enter the final value in your slicer.

> The two-step approach helps you home in on a good starting flow rate with a visual test, then optimize the final quality and fit with fine-tuning calibration.

### Where to enter in Creality Print?
* **Path:** Filament Edit -> **Extrusion** (or Filament) tab.
* **Parameter:** `Flow Ratio` (sometimes called `Extrusion Multiplier`).

---

## 3. Pressure Advance (PA)
*Goal: Sharp corners and consistent extrusion during speed changes.*

### Analysis & Calculation (Line Method)
1.  Find the line that is the most **uniform across its entire length**. Ignore lines that are thin at the ends or bloated in the middle.
2.  Read the PA value assigned to that line (usually printed next to it or calculated based on height if using a tower).
    * *For PA Line method in Orca/Creality:* The value is printed directly next to the line.

### Where to enter in Creality Print?
* **Path:** Filament Edit -> **Advanced** (or Filament Overrides) tab.
* **Parameter:** `Pressure Advance`.
    * *Important:* Ensure the "Enable Pressure Advance" box is checked.

---

## 4. Retraction
*Goal: Eliminate stringing (oozing).*

### Analysis & Calculation (Distance Test)
1.  Count the segments (notches) from the bottom up to the point where stringing **completely disappears**.
2.  **Formula:**
    $$Result = (SegmentCount \times TestStep) + SafetyMargin$$
    *Example: Stringing stops at the 6th segment, step is 0.1mm. $0.6mm + 0.1mm (margin) = 0.7mm$.*
3.  **Verification:** For Ender-3 V3 KE (Direct Drive), the result should not exceed 1.0 mm (max 1.2 mm for PETG is acceptable).

### Where to enter in Creality Print?
* **Path:** Filament Edit -> **Setting Overrides** tab (Recommended) OR **Extruder** tab.
* **Parameters:**
    * `Retraction Length`: Your calculated result.
    * `Retraction Speed`: Typically 35-45 mm/s (PLA/PETG) or 20-30 mm/s (TPU).

---

## 5. Max Volumetric Speed
*Goal: Determine the physical speed limit of your hotend.*

### Analysis & Calculation
1.  Find the height (in mm) where the print starts to fail (becomes matte, layers delaminate, or holes appear).
2.  If the test doesn't have a scale, measure the height with calipers and convert based on the generator settings, or simply read the value if printed on the model.
3.  **Safety Correction:**
    $$ValueToEnter = TestResult \times 0.90$$
    *We deduct 10% to avoid straining the extruder during real prints.*

### Where to enter in Creality Print?
* **Path:** Filament Edit -> **Advanced** tab (or the first Basic/Filament tab at the very bottom).
* **Parameter:** `Max volumetric speed`.
    * *This is the most critical "safety fuse" for high-speed printing!*

---

## 6. Dimensional Tolerance (Optional)
*Goal: Fitting for Print-in-Place parts.*

### Analysis
If parts with a 0.3mm gap are fused together:

### Where to fix in Creality Print?
* **Path:** Print Settings (Process) -> **Quality** or **Precision** tab.
* **Parameter:** `X-Y Hole Compensation` (For holes - increase, e.g., 0.1).
* **Parameter:** `X-Y Contour Compensation` (For outer contours - decrease, e.g., -0.1).

---

## Settings Cheat Sheet (Ender-3 V3 KE / Direct Drive)

The values below are **starting points**. Every filament brand may require slight adjustments.

| Parameter               | PLA / Hyper PLA      | PET-G             | TPU (Flexible)    | ASA / ABS         |
| :---                    | :---                 | :---              | :---              | :---              |
| **Nozzle Temp**         | 210°C - 230°C        | 230°C - 250°C     | 210°C - 230°C     | 240°C - 260°C     |
| **Bed Temp**            | 60°C                 | 70°C - 80°C       | 0°C - 50°C        | 100°C - 110°C     |
| **Fan Cooling**         | 100%                 | 30% - 50%         | 50% - 100%        | 0% - 15% (Off)    |
| **Retraction Length**   | 0.5 - 0.8 mm         | 0.6 - 1.0 mm      | **0.0 - 0.4 mm**  | 0.4 - 0.8 mm      |
| **Retraction Speed**    | 40 - 45 mm/s         | 30 - 40 mm/s      | **20 - 30 mm/s**  | 40 mm/s           |
| **Pressure Advance**    | ~0.04 (**safe:** 0.03–0.06) | ~0.06 (**safe:** 0.05–0.08) | Off or Test (**safe:** 0.00–0.03) | ~0.04 (**safe:** 0.03–0.07) |
| **Max Vol. Speed**      | 15 - 24 mm³/s        | 10 - 15 mm³/s     | 2 - 5 mm³/s       | 12 - 18 mm³/s     |
| **Z-Offset**            | Standard             | Standard          | Standard (or +0.05) | Standard        |
| **Special Notes**       | Open enclosure       | Prone to stringing| **Print Slow!**   | Requires Ventilation |

> **Safe Pressure Advance range** means values for smooth extrusion and avoiding motor knocking/jamming.
>
> **Important for TPU:** On the Ender-3 V3 KE, print TPU very slowly (Set Max Volumetric Speed to 3-4 mm³/s). Printing too fast or using high retraction will cause the flexible filament to wrap around the extruder gears (clog).
>
> **Important for ASA/ABS:** These materials shrink when cooling. Turn off the cooling fan, use a wide brim on the build plate, and (if possible) shield the printer from drafts, even with a cardboard box enclosure.
