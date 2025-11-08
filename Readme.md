That's a smart request. The **Keto Language Interpreter (KLI)** component actually integrates two distinct but related "commands" or input types.

Here is the full documentation for the Keto Language Interpreter component and its input commands, designed to help you quickly assess food compliance.

---

## Keto Language Interpreter (KLI) Command Documentation

The Keto Language Interpreter (KLI) is a unified metabolic processor that provides an immediate keto compliance rating based on the "language" (name) or "logic" (macros) of a food item.

It processes input via two primary command paths, the results of which are combined to provide a comprehensive metabolic analysis.

### 1. Lexical Query Command (Food Name)

This command is the **qualitative** input. It asks the KLI to check the food name against your pre-approved or denied **Keto Standard** database. This database is managed in the **Standard Editor** tab.

| Field | Input | Purpose |
| :--- | :--- | :--- |
| **Lexical Query (Food Name)** | A string (e.g., `spinach`, `rice`, `avocado`) | Checks the input against the shared database for a pre-assigned binary compliance code (`1` or `0`). |

**Interpretation Logic:**

| Result | Binary Code | Metabolic Implication |
| :--- | :--- | :--- |
| **Found (GO)** | **1** | The food is generally accepted as keto-compliant based on your established standard. |
| **Found (STOP)** | **0** | The food is generally considered non-keto and should be avoided. |
| **Not Found** | **?** | The name is unknown. The KLI defaults to relying *only* on the **Functional Query** (macro analysis). |

***

### 2. Functional Query Command (Macro Analysis)

This command is the **quantitative** input. It is the core **Net Carb calculation** that determines the *actual* metabolic load of the food item based on standard nutritional data. 

[Image of chemical structure of simple sugar vs fiber]


| Field | Input | Purpose |
| :--- | :--- | :--- |
| **Total Carbs (g)** | Numeric (e.g., `45`) | The total carbohydrate content in the specified serving. |
| **Fiber (g)** | Numeric (e.g., `5.5`) | The dietary fiber content, which is subtracted from Total Carbs. |
| **Sugar Alcohols (g)** | Numeric (e.g., `12`) | Sugar alcohols (like erythritol) are partially subtracted from Total Carbs. |

**Output Formula:**
The KLI instantly calculates the **Net Carbs** (the value that counts toward your daily limit) using the following formula:

$$\mathbf{\text{Net Carbs}} = \mathbf{\text{Total Carbs}} - \mathbf{\text{Fiber}} - \mathbf{\text{Sugar Alcohols}}$$

**Metabolic Rating Logic (Based on 20g Limit):**

| Net Carb Range | Metabolic Rating | KLI Status |
| :--- | :--- | :--- |
| **0g – 3g** | Excellent | **KETO GO** (High Compatibility) |
| **> 3g – 10g** | Moderate | **KETO CAUTION** (Requires Portion Control) |
| **> 10g** | High | **KETO STOP** (Likely Ketosis Interrupt) |

***

## KLI Integrated Output (The Combined Report)

The KLI output report on the screen combines both commands into a single, comprehensive interpretation:

1.  **If the food is found in the Standard (Lexical):** The report will first state the **Binary Code (1 or 0)** and its **Rationale**.
2.  **If macro data is provided (Functional):** The report will include the **Calculated Net Carbs** and a **Metabolic Interpretation** based on the Net Carb amount.
3.  **Conflict Resolution:** If the **Lexical Query** returns **STOP (0)** but the **Functional Query** returns a low Net Carb count, the KLI will present *both* analyses, allowing you to decide (e.g., maybe a certain keto bread is "STOP" in the standard but this specific brand has acceptable macros).

The KLI gives you the data you need to make fast, informed decisions.

Do you have a specific food item you want to analyze, or would you like to update an entry in the **Standard Editor**?

sample assembly program for checker:
```
bash

; File: K.asm
; Keto Assembly Program — Linux x86_64 example
; Author: Rushikesh Pundkar

section .data
    msg db "Keto Assembly Running Successfully!", 10   ; message + newline
    len equ $ - msg

section .text
    global _start

_start:
    ; write(msg)
    mov rax, 1          ; syscall: write
    mov rdi, 1          ; stdout
    mov rsi, msg        ; message address
    mov rdx, len        ; message length
    syscall

    ; exit(0)
    mov rax, 60         ; syscall: exit
    xor rdi, rdi        ; status 0
    syscall
```
save and run

```
bash
mkdir KetoRun
cd KetoRun
nano K.asm
```
run

```
bash
sudo apt update
sudo apt install nasm -y
nasm -f elf64 K.asm -o K.o
ld K.o -o keto.out
```

```
bash
./keto.out
```
