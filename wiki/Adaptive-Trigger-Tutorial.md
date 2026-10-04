# Adaptive Trigger Tutorial (Blueprint)

This tutorial shows how to set up and control the DualSense adaptive triggers (R2/L2) using Blueprints, driven by an Enhanced Input action.

## 1. Get the Local Player ID (Event BeginPlay)

All trigger nodes need a **Controller Id**. Retrieve it once at `Event BeginPlay` and store it in a variable (`Local Player Id`).

1. `Event BeginPlay` → **Get Controller** (Target is Pawn, `self`).
2. Connect `Return Value` to **Cast To PlayerController** (Object).
3. Connect `As Player Controller` to **Get Local Player Controller ID** (Player).
4. Use a **SET** node to store the result in `Local Player Id`.
5. Call **Stop Trigger (New)** with `Controller Id = Local Player Id` and `Hand = Any Hand`. This makes sure no effect is active when the game starts.
6. Call your own `On Change Trigger Event` (a custom event, e.g. one that refreshes the UI/output).

> In the demo, this is the **Trigger Tutorial** comment group. The same chain can end with **Update Output** instead of the custom event.

## 2. Hook the effect to the input action

In the **Player Fire Input** group, use the `EnhancedInputAction IA_Shoot` node:

| Execution pin | Action |
|---|---|
| **Started** | (Optional) `Print String` ("Start Trigger") → custom **Start Trigger Event** |
| **Triggered** | Custom **On Change Trigger Event** (fires every frame while the button is held) |
| **Completed** | (Optional) `Print String` ("Stop Trigger") → custom **Stop Trigger Event** |

- **Start Trigger Event** applies the adaptive trigger effect (e.g. Custom Trigger or Automatic Gun).
- **Stop Trigger Event** calls **Stop Trigger (New)** with the stored `Local Player Id` and the desired `Hand`, returning the trigger to normal.
- `Print String` nodes are *Development Only* and are used just for debugging.

## 3. Alternative: Automatic Gun (0x26)

Instead of the **Custom Trigger**, you can use the **Automatic Gun (0x26)** node, which is more intuitive thanks to its pre-configured positions.

| Pin | Description | Example |
|---|---|---|
| **Controller Id** | Connect `Local Player Id` | – |
| **Start Zone** | Where the effect begins | `End (75-100%)` |
| **Behavior** | How the effect acts | `Sustained (Extend to End)` |
| **Recoil** | Recoil strength | `Heavy Recoil` |
| **Frequency** | Vibration frequency (connect a `Frequency` variable) | – |
| **Hand** | Which trigger to affect (connect a `Hand` variable) | `Left` / `Right` / `Any Hand` |

## 4. Stopping the effect

Always call **Stop Trigger (New)** when the effect should end (e.g. on `Completed`, when the weapon changes, or on `BeginPlay`). Use `Any Hand` to reset both triggers.

## Summary

1. Get and store the `Local Player Id` on `BeginPlay`.
2. Start the effect on `Started` of your input action.
3. Stop it on `Completed` with **Stop Trigger (New)**.
4. Use **Automatic Gun (0x26)** for a quick, pre-configured weapon feel.
