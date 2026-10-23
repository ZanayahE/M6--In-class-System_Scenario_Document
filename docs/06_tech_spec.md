# 06 Tech Spec

## Three Files (HTML, CSS, and JavaScript)

1. **`index.html` (Structure)**
   - Contains the Gate Availability Screen (`5` spots total), the Gate Status Banner, the Key Card Scanner button, the Gate Floor Pressure Plate button, the `5` Parking Spot Pressure Plate cards (Spot 1 to Spot 5), the Exit Approach Sensor button, the System Clock / Curfew toggle (`22:00` to `06:00`), and the Curfew Override button.
2. **`styles.css` (Appearance)**
   - Styles the gate screen, open/closed gate banner colors, the `5`-spot parking grid (showing empty vs. occupied pressure plates), and curfew warning states.
3. **`script.js` (Behaviour)**
   - Runs the spot counter (`0` to `5`), the key card scanner check, the waiting car queue when `0` spots remain, the `5`-second gate floor pressure plate timer, the automatic exit sensor, the `22:00`–`06:00` curfew clock check, the curfew override, and the `60`-minute inactivity standby check.

---

## Libraries and Frameworks
- **Libraries:** none
- **Frameworks:** none
- **Reason:** Plain HTML, CSS, and vanilla JavaScript handle all `5` pressure plates, the `5`-second countdown timer, and the gate screen updates directly in the browser without needing external dependencies.

---

## What Is Saved and Where
- **Where it is saved:** Browser `localStorage` on the user's computer (under the key `carparkSystemState`).
- **What is saved:**
  1. `spotPlates`: Which of the `5` parking spot pressure plates are currently occupied (`true` or `false` for each of the `5` spots).
  2. `remainingSpots`: The number of available spots (`0` to `5`).
  3. `hasWaitingCarScannedCard`: Whether `1` car has already scanned a key card and is waiting for a spot to open (`true` or `false`).
  4. `isCurfewActive`: Whether the system clock is currently in curfew mode between `22:00` and `06:00` (`true` or `false`).
- **What is NOT saved after closing the page:** The gate open state (`isGateOpen`), the `5`-second gate countdown timer, and the temporary curfew override always reset to closed/off when the page reloads so the gate never gets stuck open.

---

## Do List and Do Not List

### Do
- **Do** keep HTML, CSS, and JavaScript in three separate files (`index.html`, `styles.css`, and `script.js`).
- **Do** display the exact remaining spots out of `5` on the gate screen at all times.
- **Do** start a `5`-second timer to close the gate only after the car rolls over the gate floor pressure plate.
- **Do** remember when a car scans its key card while `0` spots remain, and open the gate automatically as soon as `1` spot opens up.
- **Do** open the gate automatically without a key card when a car exits from inside (when curfew is not active).
- **Do** block all normal entries and exits during curfew (`22:00` to `06:00`) unless the curfew override is activated.

### Do Not
- **Do not** add any login screen, user accounts, passwords, or database servers.
- **Do not** open the gate for an entering car when `0` of `5` spots remain.
- **Do not** close the gate immediately upon scanning a key card before the car rolls over the floor pressure plate and finishes the `5`-second timer.
- **Do not** require a key card for a car that is exiting the carpark under normal daytime hours.
- **Do not** allow cars to enter or exit during curfew (`22:00` to `06:00`) without triggering the override, even if all `5` spots are empty.
- **Do not** use external JavaScript frameworks or CSS libraries.

