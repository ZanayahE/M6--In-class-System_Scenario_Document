# 05 Error States

## Things That Can Go Wrong

### 1. Car Tries to Enter When 0 of 5 Spots Remain (Carpark Full)
- **Exact Message:** `"Spots remaining: 0 of 5 — Carpark Full. Key Card saved; please wait until 1 spot opens."`
- **Where It Shows:** Gate Availability Screen and Gate Status Banner.
- **What the System Does Meanwhile:** Keeps the gate closed, remembers that `1` waiting car has already scanned its key card, and watches the `5` parking spot pressure plates.
- **How It Gets Back to Normal:** As soon as `1` parked car leaves its spot pressure plate (making remaining spots `1 of 5`), the gate opens automatically for the waiting car, the car rolls over the gate floor pressure plate, and the gate closes after `5` seconds.

### 2. Car Tries to Enter or Exit During Curfew (22:00 to 06:00) Without Override
- **Exact Message:** `"Curfew Active (22:00 - 06:00) — No cars may enter or exit without an override."`
- **Where It Shows:** Gate Availability Screen and Gate Status Banner.
- **What the System Does Meanwhile:** Locks the gate in the closed state (`CLOSED`) and ignores standard key card scans and standard exit sensor triggers, even if `1` to `5` parking spots are open.
- **How It Gets Back to Normal:** Either a staff member/resident clicks `"Use Curfew Override"` to let `1` car through, or the System Clock passes `06:00` (curfew toggled off), returning the screen to `"Spots remaining: [X] of 5"`.

### 3. Visitor Tries to Drive In or Roll Over Gate Pressure Plate Before Scanning a Key Card
- **Exact Message:** `"Access Denied — Please scan your key card first."`
- **Where It Shows:** Gate Status Banner.
- **What the System Does Meanwhile:** Keeps the gate closed and does not start the `5`-second timer.
- **How It Gets Back to Normal:** The visitor clicks `"Scan Key Card"`, which clears the warning and opens the gate (as long as at least `1` of `5` spots is open and curfew is off).

### 4. Gate Opens, But the Car Does Not Roll Over the Gate Floor Pressure Plate
- **Exact Message:** `"Gate Warning — No car detected on floor pressure plate after 15 seconds. Closing gate."`
- **Where It Shows:** Gate Status Banner.
- **What the System Does Meanwhile:** Holds the gate open for up to `15` seconds waiting for the floor pressure plate to be pressed.
- **How It Gets Back to Normal:** After `15` seconds with no floor plate press, the system closes the gate automatically and resets the scanner to `"Scanner: Ready for Key Card"` without subtracting a parking spot.

---

## Required Edge Cases

### 5. Edge Case: Two People at Once
- **Scenario A (Two cars scan key cards at the gate at the same time):**
  - **Exact Message:** `"1 car is already using the gate — Please wait 5 seconds for the gate to close."`
  - **Where It Shows:** Gate Status Banner.
  - **What the System Does Meanwhile:** Processes the first car only, ignores the second key card scan while the gate is open or counting down its `5`-second timer, and prevents `2` cars from squeezing in on `1` scan.
  - **How It Gets Back to Normal:** Once the first car rolls over the gate floor pressure plate and the `5`-second timer closes the gate, the scanner resets so the second car can scan its key card.
- **Scenario B (One car is waiting at a full carpark with 0 of 5 spots, and another car leaves at the exact same moment):**
  - **Exact Message:** `"Exiting car clearing gate first — Waiting car will enter in 5 seconds."`
  - **Where It Shows:** Gate Status Banner.
  - **What the System Does Meanwhile:** Lets the exiting car leave first so the `2` cars do not collide in the gateway, increases remaining spots from `0` to `1`, and holds the waiting car's key card in memory.
  - **How It Gets Back to Normal:** Right after the exiting car finishes its `5`-second gate exit, the gate automatically re-opens for the waiting car.

### 6. Edge Case: Someone Who Never Leaves
- **What Happens:** A car parks on `1` of the `5` parking spot pressure plates and stays there indefinitely without ever leaving.
- **Exact Message:** `"Spot [X]: Occupied (Long-Term Parked — Plate Still Pressed)"`
- **Where It Shows:** On that specific Parking Spot card (Spot `1` to `5`) and reflected in the Gate Availability Screen (`"Spots remaining: 4 of 5"`).
- **What the System Does Meanwhile:** Because the spot uses a physical pressure plate, the system keeps counting that `1` spot as occupied for as long as the car's weight is on the plate (even across page reloads using saved browser data), reducing available capacity for other visitors until only the remaining `4` spots rotate.
- **How It Gets Back to Normal:** The spot remains occupied until someone clicks `"Park / Leave Spot"` on that spot (or uses a reset control) to move the car off the pressure plate and restore `1` available spot.

### 7. Edge Case: Nothing for an Hour (60 Minutes of Inactivity)
- **What Happens:** No one scans a key card, parks, exits, or changes a control for `60` minutes (`1` hour).
- **Exact Message:** `"Standby Mode (No activity for 60 minutes) — Spots remaining: [X] of 5. Scan Key Card or Approach Gate to wake."`
- **Where It Shows:** Gate Availability Screen.
- **What the System Does Meanwhile:** Keeps the gate safely `CLOSED`, keeps the exact count of occupied parking spot pressure plates in memory, and continues checking the system clock in case the `22:00` curfew starts during the idle hour.
- **How It Gets Back to Normal:** The instant any visitor clicks `"Scan Key Card"`, `"Approach Gate to Exit"`, or any spot pressure plate, the idle timer resets to `0` minutes and the screen returns to active mode immediately.

