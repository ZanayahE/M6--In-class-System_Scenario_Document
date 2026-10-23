# 01 Overview — Scenario 1: Gated Carpark

1. Scenario 1: A gated carpark with a fixed number of spaces (5 spaces total). The gate has to know when to open and when not to.

This system will be used by residents and visitors when they need to park. Residents will have a key card that they need to scan on the scanner on the gate which opens the gate. There will be a pressure plate on the floor so after the car rolls over it it will start a 5-second timer to close the gate to make sure the car gets in. Once in the parking area each car goes into one of the 5 parking spots with pressure plates which allows the system to know a car is there. There will also be a screen at the gate which tells you how many spots remain (from 5 down to 0). If there are 0 spots remaining the gate will not open even with a key card. The car will have to now wait till a parking spot opens and when it does the gate will then open (you would have had to use your key card).

Peer feedback
When a car leaves, the gate should automatically open without a key card because it knows an exit from an entry.

There should also be a clock on the system that lets it know if and when a curfew is set (22:00 to 06:00) and no cars are supposed to come in or go out except through an override, even when there are still spaces.

---

## LLM Instructions
- 01 Overview. All ten questions answered, with the numbers.
- 02 Frontend. Every part the visitor sees, each on its own; what each part shows, with the exact words; what the visitor can do with each part; the screen before anyone does anything; what changes after each action.
- 03 Backend. What runs out of sight, in order; each message, with who sends it and who receives it; each thing it remembers, what kind of value it is, what it starts as, and whether it is kept after the page closes; the line "Nobody logs in."
- 04 Flow chart. A start state; each action as a box and each question as a diamond; a yes and a no out of every diamond; a number in every question; a way back to the start.
- 05 Error states. For each thing that can go wrong: the exact message, where it shows, what the system does meanwhile, and how it gets back to normal. The edge cases too: two people at once, someone who never leaves, nothing for an hour.
- 06 Tech spec. HTML, CSS and JavaScript in three files; each library and the framework by name with its reason, or "none"; what is saved and where; a Do list and a Do not list.
- 07 Acceptance checks. Three to eight plain sentences, each one true or false, using your numbers, with at least one for something going wrong and one for an edge case. No code.

