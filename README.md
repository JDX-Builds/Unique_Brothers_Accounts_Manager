# Expense Tracker

A desktop application for recording daily income and expenses, built with **Python** and **Tkinter**. It captures cash, UPI, and credit income alongside itemized online/cash expenses, then generates a structured **Microsoft Word (.docx) report** for each day — including opening balance, sales summary, expense breakdown, and closing balance — along with a running cumulative summary across all entries.

## Features

- **Date-based entry** through an interactive calendar picker
- Tracking of **cash, UPI, and credit income** per day
- Support for multiple **itemized online/cash expense entries**, with an inline running preview
- Integrated **safe calculator** for quick calculations during data entry (AST-based expression evaluation — no use of `eval()`)
- Automated generation of a daily **Word report** (`C:\ub\<date>.docx`) containing:
  - Opening balance, carried forward from the previous day's report
  - Sales summary (cash / UPI / credit breakdown)
  - Expense summary (online vs. cash purchases)
  - Net and closing balance calculations
- A persistent **summary document** (`data.docx`) tracking cumulative gross sales, expenses, and profit across all recorded days
- Robust handling of edge cases: duplicate-date overwrite confirmation, missing prior-day data, and file-lock errors when the target document is open in Word

## Project Structure

```
.
├── main.py            # Entry point — wires up GUI, logic, and storage
├── gui.py              # Tkinter UI (Win1) — date picker, income/expense forms
├── logic.py            # Business logic — validation, expense merging, calculations
├── store.py            # Word document generation via python-docx
├── calc.py              # Standalone safe calculator (Toplevel window)
└── requirements.txt     # Python dependencies
```

## Requirements

- Python 3.9+
- Windows (output path is hardcoded to `C:\ub`)
- Microsoft Word not required to run, but the target `.docx` files should be closed while saving

Install dependencies:

```bash
pip install -r requirements.txt
```

Key dependencies:
- [`python-docx`](https://python-docx.readthedocs.io/) — Word document generation
- [`tkcalendar`](https://github.com/j4321/tkcalendar) — calendar date picker widget
- [`Pillow`](https://python-pillow.org/) — banner image handling
- `pyinstaller` — for building a standalone `.exe` (optional)

 In the app:
   - Click **Select Date** and choose a date from the calendar
   - Enter Cash / UPI / Credit income
   - Add expense sources one at a time using **Add**, or remove the last one with **Remove**
   - Toggle expense type with **Switch** (Online ↔ Cash)
   - Click **Store Data** to validate, calculate, and save the report as a `.docx` file
   - Use **Kill Entry** to clear all current entries, or the built-in **Calculator** for quick sums

Reports are saved to `C:\ub\<dd_mm_yyyy>.docx`, and a running summary is kept in `C:\ub\data.docx`.

## Notes

- The output directory (`C:\ub`) is currently hardcoded in `logic.py` and `store.py`. Update the `OUTPUT_DIR` constant in `store.py` (and the corresponding path in `logic.py`) if you'd like to change it.
- `gui.py` looks for an optional banner image (`img.jpg`, `img.png`, `banner.jpg`, etc.) in the script directory, home directory, or `C:\Codes\Projects\Ub`; if none is found, a text header is shown instead.
