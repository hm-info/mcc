---
title: Disabling a tool
lang: en
pdf: mcc-tool-disable-en.pdf
---

# Disabling a tool

<div class="document-tools" markdown="1">
[Türkçe](tool_disable_tr.md) · **[Download PDF](downloads/mcc-tool-disable-en.pdf)**
</div>

MCC generates the G code of a drawing **for every tool in the drawing**; it does not output only the selected tool. To skip the operations of one tool temporarily you do not need to delete or regenerate the G code: **disable the tool**, and the machine skips that tool's section at run time.

## How it looks in the G code

In the generated program every tool starts its own block. The first line of the block is a jump condition: if the tool's disable parameter is **not 0**, the NC skips all lines of that tool and continues with the next tool's block.

```text
N01000 @121 R154 K0 K2000     ; if R154 ≠ 0 jump to N02000 (tool 4)
...                           ; tool 4 operations
N02000 @121 R156 K0 K3000     ; if R156 ≠ 0 jump to N03000 (tool 6)
...                           ; tool 6 operations
N03000 M17
```

On B&R controlled machines the same logic is written as `IF R... <> 0` / `GOTO N...000` / `ENDIF`.

**Parameter number = 150 + tool number.**

| Tool | Parameter | | Tool | Parameter |
|---|---|---|---|---|
| T1 | R151 | | T11 | R161 |
| T2 | R152 | | T12 | R162 |
| T3 | R153 | | T13 | R163 |
| T4 | R154 | | T14 | R164 |
| T5 | R155 | | T15 | R165 |
| T6 | R156 | | T16 | R166 |
| T7 | R157 | | T17 | R167 |
| T8 | R158 | | T18 | R168 |
| T9 | R159 | | T19 | R169 |
| T10 | R160 | | T20 | R170 |

## Method 1 — Tool Disable box

On machines whose tool offset screen has a **Tool Disable** box (e.g. CN779, CN784, DCM019B):

1. Open the tool offset screen and select the tool.
2. Tick **Tool Disable** and save.
3. To use the tool again, clear the box and save.

## Method 2 — R parameter

On machines without the Tool Disable box (e.g. CN770) the tool is disabled with R parameters:

1. Open the **R Parameters** page from the service menu.
2. Write **1** to the tool's parameter (150 + tool number). Example: for tool 4, **R154 = 1**.
3. To use the tool again, write **0** to the same parameter.

R parameters are retained: the value is kept when the machine is switched off and on. The values are written to the NC every time a program is started.

## Notes

- **A change takes effect at the next program start.** Changing the value while a program is running does not affect the running program.
- The operations of a disabled tool are **not machined** on the part. Remember to set the parameter back to 0 / clear the box when the tool is back in use.
- The parameter has no effect for a tool that is not used in the drawing.
- Several tools can be disabled at the same time; each skips its own block.
