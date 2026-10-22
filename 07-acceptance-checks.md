# 07 Acceptance Checks

1. **True or False:** Before anyone does anything, the gate is closed and the gate screen shows `"Spots remaining: 5 of 5"`.
2. **True or False:** When a resident scans `1` key card while `5` spots are available and curfew is off, the gate opens, and rolling over the gate floor pressure plate starts a `5`-second timer that closes the gate automatically.
3. **True or False:** When `3` cars are parked on the parking spot pressure plates, the screen at the gate shows `"Spots remaining: 2 of 5"`.
4. **True or False (Something Going Wrong — Full Carpark):** When `0` of `5` spots remain and a car scans its key card, the gate stays closed until `1` parked car leaves its spot, at which point the gate opens automatically for the waiting car without needing a second scan.
5. **True or False:** When `1` car leaves a parking spot from the inside during normal hours, the gate recognizes an exit and opens automatically without requiring a key card.
6. **True or False (Something Going Wrong — Curfew Lockout):** When the system clock is set to curfew (`22:00` to `06:00`), no cars can enter or exit even when `5` spots are open, unless someone clicks the curfew override.
7. **True or False (Edge Case — Two Cars at Once):** If `2` cars try to scan their key cards at the exact same time, the system lets only `1` car through and blocks the second key card scan until the `5`-second floor pressure plate timer closes the gate.
8. **True or False (Edge Case — Nothing for an Hour):** If nothing happens for `60` minutes (`1` hour), the system enters standby mode while keeping the gate closed and remembering the exact number of occupied spots out of `5`.

