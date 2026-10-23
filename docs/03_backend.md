# 03 Backend

Nobody logs in.

---

## What Runs Out of Sight, In Order

1. **System Startup Check:** When the page loads, the system sets the total capacity to `5` spots, sets all `5` parking spot pressure plates to empty, sets the remaining spot counter to `5`, sets the gate to closed, sets the system clock to `12:00` (curfew off), and sets the override to off.
2. **Curfew & Override Check:** Every time a car tries to enter or exit, the system checks the system clock first. If curfew is active (`22:00` to `06:00`) and override is off, the system blocks the gate from opening and sends a curfew error message to the screen. If override is on, the system allows the gate to open.
3. **Direction Check (Entry vs. Exit):** If a car approaches the gate from the inside (`exit`), the system skips the key card requirement and opens the gate automatically. If a car approaches from the outside (`entry`), the system requires a key card scan.
4. **Available Space & Queue Check:** When a key card is scanned for entry, the system checks if remaining spots are greater than `0`.
   - If remaining spots are `1` to `5`, the system opens the gate immediately.
   - If remaining spots are `0`, the system keeps the gate closed and saves that `1` car with a scanned key card is waiting at the gate.
5. **Gate Floor Pressure Plate & 5-Second Auto-Close Timer:** Once the gate is open and the car rolls over the gate floor pressure plate, the system starts a `5`-second countdown timer. When the timer reaches `0` seconds, the system closes the gate automatically.
6. **Parking Spot Pressure Plate Recalculation:** Whenever a car parks on or leaves one of the `5` spot pressure plates, the system recounts how many plates are pressed, subtracts that number from `5` to get `remainingSpots`, and updates the gate screen.
7. **Automatic Release for Waiting Car:** If `remainingSpots` changes from `0` to `1` while a car that already scanned its key card is waiting at the gate, the system automatically opens the gate for that waiting car and clears the waiting state.

---

## Each Message (Who Sends It and Who Receives It)

1. **Key Card Scan Message**
   - **Who sends it:** Key Card Scanner (Frontend button)
   - **Who receives it:** Gate Controller (Backend logic)
   - **What it says:** A resident or visitor has scanned a key card to request entry.

2. **Exit Approach Message**
   - **Who sends it:** Exit Direction Sensor (Frontend button)
   - **Who receives it:** Gate Controller (Backend logic)
   - **What it says:** A car is leaving from the inside and needs the gate to open without a key card.

3. **Gate Floor Pressure Plate Trigger Message**
   - **Who sends it:** Gate Floor Pressure Plate (Frontend button)
   - **Who receives it:** Gate Auto-Close Timer (Backend logic)
   - **What it says:** The car has rolled over the floor plate at the gate; start the `5`-second countdown to close the gate.

4. **Timer Expired Message**
   - **Who sends it:** Gate Auto-Close Timer (Backend logic)
   - **Who receives it:** Gate Controller and Gate Status Banner (Frontend display)
   - **What it says:** `5` seconds have passed; close the gate now.

5. **Spot Pressure Plate Change Message**
   - **Who sends it:** Parking Spot Pressure Plate `1`, `2`, `3`, `4`, or `5` (Frontend spot button)
   - **Who receives it:** Spot Counter (Backend logic)
   - **What it says:** Spot `[1-5]` changed to occupied or empty; recalculate remaining spots out of `5`.

6. **Remaining Spots Update Message**
   - **Who sends it:** Spot Counter (Backend logic)
   - **Who receives it:** Gate Availability Screen (Frontend display) and Waiting Queue Checker (Backend logic)
   - **What it says:** The new number of open spots (`0` to `5`) and whether a waiting car can now enter.

7. **Curfew Status Message**
   - **Who sends it:** System Clock (Frontend toggle / Backend clock state)
   - **Who receives it:** Gate Controller (Backend logic) and Gate Availability Screen (Frontend display)
   - **What it says:** Curfew (`22:00` to `06:00`) is now active or inactive.

8. **Override Activation Message**
   - **Who sends it:** Curfew Override Control (Frontend button)
   - **Who receives it:** Gate Controller (Backend logic)
   - **What it says:** Override has been triggered so a car can enter or exit during curfew.

---

## Everything the System Remembers

| Thing It Remembers | Kind of Value | What It Starts As | Kept After the Page Closes? |
| :--- | :--- | :--- | :--- |
| `totalSpaces` (Fixed number of parking spaces) | Number | `5` | No (resets to `5` on page load) |
| `remainingSpots` (Open spaces left at the carpark) | Number | `5` | Yes (saved in browser `localStorage`) |
| `spotPlates` (Status of the 5 parking spot pressure plates) | List of 5 true/false values | `[false, false, false, false, false]` (all 5 empty) | Yes (saved in browser `localStorage`) |
| `isGateOpen` (Whether the gate arm is open or closed) | True/False (Boolean) | `false` (closed) | No (always starts closed for safety) |
| `gateTimerSeconds` (Countdown after car rolls over gate plate) | Number | `0` (counts down from `5` to `0` when triggered) | No |
| `hasWaitingCarScannedCard` (Whether a car scanned a card while full) | True/False (Boolean) | `false` | Yes (saved in browser `localStorage`) |
| `isCurfewActive` (Whether system clock is between 22:00 and 06:00) | True/False (Boolean) | `false` (daytime `12:00`) | Yes (saved in browser `localStorage`) |
| `isOverrideActive` (Whether curfew override is currently turned on) | True/False (Boolean) | `false` | No (resets to `false` after use or page close) |
| `lastActivityMinutes` (Minutes since last action, for 60-minute idle check) | Number | `0` | No |

