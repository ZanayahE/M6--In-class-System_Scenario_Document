# 02 Frontend

## System Numbers Used
- **Total Parking Spaces:** 5 spaces (Spot 1 to Spot 5)
- **Gate Auto-Close Timer:** 5 seconds after rolling over the gate floor pressure plate
- **Curfew Hours:** 22:00 to 06:00

---

## Every Part the Visitor Sees (Each on Its Own)

### 1. Gate Availability Screen
- **What it shows (exact words):**
  - Default when empty: `"Spots remaining: 5 of 5"`
  - When spots are taken: `"Spots remaining: [X] of 5"` (where `[X]` is `4`, `3`, `2`, or `1`)
  - When full: `"Spots remaining: 0 of 5 — Carpark Full. Waiting for an open spot."`
  - During curfew: `"Curfew Active (22:00 - 06:00) — Entry and Exit Closed"`
  - During override: `"Override Active — Gate Unlocked"`
- **What the visitor can do with it:** Read-only display; the visitor looks at it to see how many parking spots are open or if curfew is active.

### 2. Gate Status Banner & Barrier Arm
- **What it shows (exact words):**
  - When closed: `"Gate Status: CLOSED"`
  - When open for entry: `"Gate Status: OPEN — Please drive across the floor pressure plate"`
  - When timer is counting down: `"Gate Status: OPEN — Closing in 5 seconds..."` (counting down `5`, `4`, `3`, `2`, `1`)
  - When open for exit: `"Gate Status: OPEN FOR EXIT — No key card needed"`
  - When waiting for a spot: `"Key Card Accepted — Waiting in queue for 1 open spot"`
- **What the visitor can do with it:** Read-only status display showing whether the gate is currently open or closed.

### 3. Key Card Scanner Button
- **What it shows (exact words):** A button labeled `"Scan Key Card"` and a status label underneath showing `"Scanner: Ready for Key Card"`.
- **What the visitor can do with it:** Click `"Scan Key Card"` to request entry into the carpark.

### 4. Gate Floor Pressure Plate Button
- **What it shows (exact words):** A button labeled `"Roll Over Gate Pressure Plate"` and a status label showing `"Gate Pressure Plate: Not Pressed"`.
- **What the visitor can do with it:** Click `"Roll Over Gate Pressure Plate"` after the gate opens to drive into the carpark and start the 5-second auto-close timer.

### 5. Five Parking Spot Pressure Plates (Spot 1 to Spot 5)
- **What it shows (exact words):** Five separate spot cards labeled `"Spot 1: Empty"`, `"Spot 2: Empty"`, `"Spot 3: Empty"`, `"Spot 4: Empty"`, and `"Spot 5: Empty"`. Each card has a button labeled `"Park / Leave Spot"`.
- **What the visitor can do with it:** Click `"Park / Leave Spot"` on an empty spot to park a car on that spot's pressure plate (changing it to `"Occupied"`), or click it on an occupied spot to leave that spot and trigger an exit at the gate.

### 6. Exit Approach Sensor Button
- **What it shows (exact words):** A button labeled `"Approach Gate to Exit"` and a label showing `"Exit Sensor: No Car Leaving"`.
- **What the visitor can do with it:** Click `"Approach Gate to Exit"` when leaving a parking spot so the gate recognizes an exit rather than an entry and opens automatically without a key card.

### 7. System Clock & Curfew Control Panel
- **What it shows (exact words):**
  - Time display: `"System Clock: 12:00 (Daytime — Open)"` or `"System Clock: 23:00 (Curfew Active: 22:00 to 06:00)"`
  - A toggle button labeled `"Toggle Curfew (22:00 - 06:00)"`
- **What the visitor can do with it:** Click `"Toggle Curfew (22:00 - 06:00)"` to switch the system clock between normal daytime hours and curfew hours.

### 8. Curfew Override Button
- **What it shows (exact words):** A button labeled `"Use Curfew Override"` and a status label showing `"Override: OFF"`.
- **What the visitor can do with it:** Click `"Use Curfew Override"` during curfew so a car can still enter or exit even when curfew is active.

---

## The Screen Before Anyone Does Anything
Before anyone clicks anything, the screen displays:
- **Gate Availability Screen:** `"Spots remaining: 5 of 5"`
- **Gate Status Banner:** `"Gate Status: CLOSED"`
- **Key Card Scanner:** `"Scanner: Ready for Key Card"` (with `"Scan Key Card"` button enabled)
- **Gate Floor Pressure Plate:** `"Gate Pressure Plate: Not Pressed"`
- **Parking Spots 1 to 5:** All 5 spots show `"Spot 1: Empty"`, `"Spot 2: Empty"`, `"Spot 3: Empty"`, `"Spot 4: Empty"`, `"Spot 5: Empty"`
- **Exit Sensor:** `"Exit Sensor: No Car Leaving"`
- **System Clock:** `"System Clock: 12:00 (Daytime — Open)"`
- **Curfew Override:** `"Override: OFF"`

---

## What Changes After Each Action

1. **Visitor clicks `"Scan Key Card"` when spots are available (`1` to `5` spots remaining) and curfew is OFF:**
   - Scanner label changes to `"Scanner: Key Card Accepted"`.
   - Gate Status Banner changes from `"Gate Status: CLOSED"` to `"Gate Status: OPEN — Please drive across the floor pressure plate"`.

2. **Visitor clicks `"Roll Over Gate Pressure Plate"` while the gate is open:**
   - Gate Pressure Plate label changes to `"Gate Pressure Plate: Car Detected — Starting 5s Timer"`.
   - Gate Status Banner counts down from `"Gate Status: OPEN — Closing in 5 seconds..."` to `1` second, then returns to `"Gate Status: CLOSED"`.

3. **Visitor clicks `"Park / Leave Spot"` on an empty spot (for example, Spot 1):**
   - Spot 1 card changes from `"Spot 1: Empty"` to `"Spot 1: Occupied (Pressure Plate Pressed)"`.
   - Gate Availability Screen decreases by 1 (for example, from `"Spots remaining: 5 of 5"` to `"Spots remaining: 4 of 5"`).

4. **Visitor clicks `"Scan Key Card"` when `0` of `5` spots remain:**
   - Gate stays closed (`"Gate Status: CLOSED"`).
   - Gate Availability Screen shows `"Spots remaining: 0 of 5 — Carpark Full. Waiting for an open spot."`
   - Gate Status Banner changes to `"Key Card Accepted — Waiting in queue for 1 open spot"`.

5. **A parked car leaves a spot when a waiting car is queued at the gate:**
   - That spot changes from `"Occupied"` to `"Empty"`.
   - Gate Availability Screen updates from `"Spots remaining: 0 of 5"` to `"Spots remaining: 1 of 5"`.
   - Because the waiting car already scanned its key card, the Gate Status Banner automatically changes to `"Gate Status: OPEN — Waiting car may now enter"`.

6. **Visitor leaves a parking spot and clicks `"Approach Gate to Exit"` (when curfew is OFF):**
   - Exit Sensor label changes to `"Exit Sensor: Car Leaving Detected"`.
   - Gate Status Banner changes to `"Gate Status: OPEN FOR EXIT — No key card needed"`.
   - After the car rolls over the gate pressure plate and 5 seconds pass, the gate closes and `"Exit Sensor: No Car Leaving"` is restored.

7. **Visitor clicks `"Toggle Curfew (22:00 - 06:00)"` to turn curfew ON:**
   - System Clock changes to `"System Clock: 23:00 (Curfew Active: 22:00 to 06:00)"`.
   - Gate Availability Screen changes to `"Curfew Active (22:00 - 06:00) — Entry and Exit Closed"`.
   - Gate Status Banner shows `"Gate Status: CLOSED"`. Scanning a key card or approaching to exit will not open the gate.

8. **Visitor clicks `"Use Curfew Override"` while curfew is ON:**
   - Override status label changes from `"Override: OFF"` to `"Override: ON"`.
   - Gate Availability Screen changes to `"Override Active — Gate Unlocked"`.
   - Gate Status Banner changes to `"Gate Status: OPEN — Override Used"` so the car can enter or exit, then closes after the 5-second floor pressure plate timer finishes.

