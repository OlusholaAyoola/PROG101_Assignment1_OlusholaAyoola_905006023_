# FloodWaste Early Warning System

An interactive Flowgorithm flowchart system designed for urban waste clogging, flood, and landslide early warning, contact registration, alert dispatch, offline outbox queuing, and assessment history tracking[cite: 6].

## Overview

The **FloodWaste Early Warning System** (`FloodWaste - Shola (Complete)`) models community-level hazard monitoring and alert workflows using modular functions and state arrays in Flowgorithm[cite: 6]. Users can register contacts, input environmental and drainage readings, evaluate flood/landslide risk levels, trigger warnings or clearance notices, queue messages when cellular/SMS service is unavailable, and inspect past assessments and queued alerts[cite: 6].

## System Architecture & Modules

* **`Main`** — Initializes state arrays and counters (`contacts`, `alertHistory`, `outbox`, `contactCount`, `historyCount`, `outboxCount`), runs the main menu loop, validates user inputs, records assessment history, and coordinates function calls[cite: 6].
* **`AssessRisk`** — Evaluates 24-hour rainfall (`total24`), river level (`riverlevel`), drainage waste blockage percentage (`wasteBlockage`), and steep slope proximity (`steepSlope`) to return a risk tier (`RED`, `ORANGE`, `YELLOW`, or `GREEN`)[cite: 6].
* **`RegisterContact`** — Adds resident or responder contact details into the `contacts` array (up to 100 entries) while rejecting empty inputs[cite: 6].
* **`SendAlerts`** — Builds formatted alert messages (including landslide warnings when active), checks cellular/SMS availability, and either sends alerts immediately or queues them in `outbox`[cite: 6].
* **`ShowAlertHistory`** — Displays recorded risk assessments from `alertHistory` alongside any messages waiting in the offline `outbox`[cite: 6].

### Additional Planned / Extension Modules

* **`FlushOutbox`** — Syncs and clears queued alerts from `outbox` once cellular/SMS connectivity is restored.
* **`DispatchClearanceTeam`** — Assigns and logs a drainage clearance crew when waste blockage reaches critical thresholds (`>= 70%`)[cite: 6].

## Risk & Warning Logic

* **Risk Levels (`AssessRisk`)**[cite: 6]:
  * **`RED`**: `total24 >= 150` OR `riverlevel >= 250` OR (`wasteBlockage >= 80` AND `total24 >= 80`)[cite: 6]
  * **`ORANGE`**: `total24 >= 100` OR `riverlevel >= 200` OR (`wasteBlockage >= 60` AND `total24 >= 50`) OR (`steepSlope == True` AND `total24 >= 75`)[cite: 6]
  * **`YELLOW`**: `total24 >= 50` OR `riverlevel >= 150` OR `wasteBlockage >= 50`[cite: 6]
  * **`GREEN`**: All lower/routine conditions[cite: 6]
* **Landslide Warning**: Triggered in `Main` when `steepSlope == True` AND (`total24 >= 50` OR `risk == "ORANGE"` OR `risk == "RED"`)[cite: 6].
* **Severe Waste Clogging Notice**: Triggered in `Main` when `wasteBlockage >= 70`[cite: 6].

## Getting Started

1. Install **Flowgorithm** (version 4.2 or compatible)[cite: 6].
2. Open `FloodWaste_Shola_v2.fprg` in Flowgorithm[cite: 6].
3. Click **Run** to launch the console menu[cite: 5, 6]:
   * `1` — Enter readings and assess risk[cite: 6]
   * `2` — Register contact[cite: 6]
   * `3` — View alert history[cite: 6]
   * `4` — Exit[cite: 6]

## Example Test Scenarios

### Scenario A: Compound Emergency — Severe Clogging, Landslide & Red Flood Alert (Online)
* **Inputs:** menu choice = `2`, contact = `"Aminata Kamara - +23276001122"`, menu choice = `1`, `communityName` = `"Kroo Bay"`, `total24` = `165`, `riverlevel` = `260`, `wasteBlockage` = `85`, `steepSlope` = `True`, `networkAvailable` = `True`[cite: 6]
* **Output:**
  “Contact registered successfully! Total contacts: 1
  Running Assessment...
  Assessment Complete -> Risk Level: RED
  WARNING: Landslide risk detected due to steep slope and heavy rainfall!
  ACTION REQUIRED: Severe waste clogging (85%). Dispatch drainage clearance team!
  [SENT to Aminata Kamara - +23276001122]: RED flood alert for Kroo Bay (Drain blockage: 85%). Landslide danger: leave steep slopes. 
  Alerts sent: 1 | Queued: 0”[cite: 6]

### Scenario B: Network Outage & Offline Queue Recovery (Moderate Risk)
* **Inputs:** menu choice = `1`, `communityName` = `"Lumley"`, `total24` = `65`, `riverlevel` = `140`, `wasteBlockage` = `55`, `steepSlope` = `False`, `networkAvailable` = `False`, menu choice = `3`[cite: 6]
* **Output:**
  “Assessment Complete -> Risk Level: YELLOW
  [QUEUED to outbox for Aminata Kamara - +23276001122]: YELLOW flood alert for Lumley (Drain blockage: 55%). 
  Alerts sent: 0 | Queued: 1”[cite: 6]
