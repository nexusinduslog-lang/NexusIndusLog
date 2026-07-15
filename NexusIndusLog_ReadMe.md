#  NexusInduslog Readme

## Objectives

The purpose of this repository is to provide the necessary information for using **NexusIndusLog**.

This software tool enables connections to various industrial devices using the **Modbus/TCP** or **OPC UA** protocols. It allows for:
*   **Logging, visualization, and export** of data from these devices.
*   **Synchronized data logging** with the PLC's execution task, achieved through the use of dedicated function blocks within the PLC.

---

## Protocols

### 1. Modbus/TCP

The software can access two memory ranges: **Holding Registers** and **Coils**. The following data types are supported:

| Type | Description |
| :--- | :--- |
| **BOOL** | Boolean |
| **INT** | 16-bit signed integer |
| **UINT** | 16-bit unsigned integer |
| **DINT** | 32-bit signed double integer |
| **UDINT** | 32-bit unsigned double integer |
| **REAL** | 32-bit IEEE 754 floating-point number |

> 🔗 **Tutorials & Implementation:** [To be defined]

---

### 2. OPC UA Client

You can define variables in two ways:
*   **Manually** by entering the node paths.
*   **Automatically** by connecting to the device and scanning for available variables.

> ⚠️ **Security Note:** Currently, only unencrypted clients without authentication are supported.

---

### 3. Probe over Modbus/TCP

By using a specific function block (FB) within the PLC, you can read variables defined in your PLC application with **cycle-level precision**. 

#### Function Block Types

There are two main types of function blocks:
1.  **`Probe_data_1ms` / `Probe_data_5ms`**: These functions format and package data synchronized with the PLC clock (1ms or 5ms minimum cycle time).
2.  **`Values_to_probe_x`**: These helper functions prepare and group your application variables before passing them to the main `Probe_data...` block.

#### Available Data Preparation Blocks (`Values_to_probe_x`)

*   **`Values_to_probe_1`** — Logs variables in this specific order:
    *   1x `WORD`
    *   1x `INT`
    *   1x `DINT`
    *   4x `REAL`
*   **`Values_to_probe_2`** — Logs:
    *   6x `REAL`
*   **`Values_to_probe_3`** — Logs:
    *   12x `WORD`
*   **`Values_to_probe_4`** — Logs:
    *   2x `REAL`
*   **`Values_to_probe_5`** — Logs:
    *   2x `DINT`

#### Compatibility & Association Matrix

Depending on the cycle time you target, you must pair the preparation blocks with the correct probe block:

| PLC Cycle Time Target | Main Probe Function | Compatible Preparation Blocks |
| :--- | :--- | :--- |
| **1 ms cycle** | `Probe_data_1ms` | `Values_to_probe_4` <br> `Values_to_probe_5` |
| **5 ms cycle** | `Probe_data_5ms` | `Values_to_probe_1` <br> `Values_to_probe_2` <br> `Values_to_probe_3` |

> 🔗 **Demonstration Tutorials:** [Link to be defined]