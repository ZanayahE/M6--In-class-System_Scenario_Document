# 04 Flow Chart

## Legend
- **Start State:** `( Start )`
- **Action (Box):** `[ Action ]`
- **Question (Diamond):** `< Question with a number? >` — every diamond has both a **YES** path and a **NO** path, and every path leads back to the **Start State**.

---

## Step-by-Step Flow Chart

**( START: Gate is CLOSED (0 = closed), Screen shows Remaining Spots out of 5, System waits for 1 car action )**
  |
  v
**< Question 1: Is the System Clock between 22:00 and 06:00 (1 = Curfew Active)? >**
  |-- **YES:** Go to **Question 2**
  |-- **NO:** Go to **Question 3**

---

**< Question 2: Did someone activate the 1 Curfew Override? >**
  |-- **NO:** `[ Action: Keep gate CLOSED (0), show "Curfew Active (22:00 - 06:00) — Entry and Exit Closed" on screen ]` ---> **Return to ( START )**
  |-- **YES:** `[ Action: Show "Override Active — Gate Unlocked" and allow this 1 car to proceed ]` ---> Go to **Question 3**

---

**< Question 3: Is 1 car leaving from inside (Exit = 1) instead of entering from outside (Entry = 0)? >**
  |-- **YES (Exiting):** `[ Action: Open gate automatically without a key card, free 1 parking spot pressure plate, and add 1 to Remaining Spots (up to 5) ]` ---> Go to **Question 6**
  |-- **NO (Entering):** Go to **Question 4**

---

**< Question 4: Did the entering car scan 1 valid Key Card at the gate scanner? >**
  |-- **NO:** `[ Action: Keep gate CLOSED (0), show "Please scan 1 key card to enter" on screen ]` ---> **Return to ( START )**
  |-- **YES:** Go to **Question 5**

---

**< Question 5: Are Remaining Spots greater than 0 (at least 1 of 5 spots open)? >**
  |-- **NO (0 of 5 spots open):** `[ Action: Keep gate CLOSED (0), save 1 car in waiting queue (Key Card remembered), and wait until 1 of the 5 spot pressure plates becomes empty ]` ---> `[ Action: As soon as Remaining Spots becomes 1 of 5, open gate automatically for the 1 waiting car ]` ---> Go to **Question 6**
  |-- **YES (1 to 5 spots open):** `[ Action: Open gate immediately for the 1 entering car ]` ---> Go to **Question 6**

---

**< Question 6: Did the 1 car roll over the 1 Gate Floor Pressure Plate within 60 seconds? >**
  |-- **NO:** `[ Action: After 60 seconds with 0 floor plate presses, close the gate for safety and reset scanner ]` ---> **Return to ( START )**
  |-- **YES:** `[ Action: Start the 5-second auto-close timer ]` ---> Go to **Question 7**

---

**< Question 7: Has the 5-second gate timer reached 0 seconds? >**
  |-- **NO:** `[ Action: Keep gate OPEN and count down 1 second at a time ]` ---> Repeat **Question 7**
  |-- **YES:** `[ Action: Close the gate (0 = closed). If the car entered, it presses 1 of the 5 parking spot pressure plates and subtracts 1 from Remaining Spots ]` ---> **Return to ( START )**

