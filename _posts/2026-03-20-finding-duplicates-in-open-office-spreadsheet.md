---
title: Simple Way to Find Duplicates in OpenOffice Calc spreadsheets
---

### How to Highlight Duplicate Entries in an OpenOffice Calc Column

If you want to quickly spot duplicate entries in a column in **Apache OpenOffice Calc v. 4.1.16**, you can use **Conditional Formatting** with a custom formula. Here's how:

1. **Select the range of cells** you want to check, for example `B2:B200`. **Important:** in this version of Calc, select the cells **from the bottom up** (B200 first, then B2). Selecting from the top down may prevent the formula from working correctly.
2. Go to **Format → Conditional Formatting → Condition**.
3. Set **Condition 1** to **Formula is:**.
4. Enter the following formula:
```bash
AND(LEN(B2)>0;COUNTIF($B$2:$B$200;B2)>1)
```

This formula checks if a cell in the range is not empty and appears more than once.

5. Choose a **cell style** to apply when duplicates are found. You can use a predefined style or create a new one with custom colors, fonts, or borders.
6. Click **OK** to apply the formatting.

Now, any duplicate entries in your column will be automatically highlighted using the style you selected, making duplicates easy to spot.
