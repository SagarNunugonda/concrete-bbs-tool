# BBS Calculator - Footing Reinforcement

A Python-based Bill of Quantities (BBS) calculator for automatically processing footing reinforcement data and generating detailed quantity takeoffs.

## Overview

This tool processes a CSV file containing footing specifications and reinforcement details, then generates a comprehensive BBS (Bill of Quantities) output file with calculated reinforcement quantities, lengths, and weights.

## Features

- ✅ Reads footing specifications from CSV input files
- ✅ Parses reinforcement details in standard format (e.g., T12@125C/C)
- ✅ Calculates:
  - Number of bars required
  - Effective bar lengths
  - Total reinforcement length
  - Total reinforcement weight
- ✅ Handles multiple footings with different specifications
- ✅ Skips invalid entries automatically (REFER PLAN, NOT IN USE)
- ✅ Generates detailed BBS output in CSV format
- ✅ User-friendly command-line interface

## Requirements

- Python 3.14 or higher
- pandas
- numpy

## Installation

### 1. Install Python
Download and install Python 3.14+ from [python.org](https://www.python.org)

### 2. Clone/Download Repository
```bash
git clone https://github.com/yourusername/bbs-calculator.git
cd bbs-calculator
```

### 3. Install Dependencies
```bash
pip install pandas numpy
```

Or using requirements.txt (if available):
```bash
pip install -r requirements.txt
```

## Usage

Run the script from the terminal:

```bash
python process_bbs.py
```

### Interactive Prompts

The script will ask for:

1. **Input CSV file path** - Path to your footing schedule CSV file
   - Example: `footing_schedule.csv` or `C:\Users\Dell\civil\footing_schedule.csv`

2. **Output CSV file path** - Where to save the BBS results
   - Example: `bbs_output.csv` or `C:\Users\Dell\civil\bbs_output.csv`

3. **Pour name** (optional) - Name of the pour/project phase
   - Default: `Pour-01`
   - Press Enter to use default, or enter custom name (e.g., `Pour-02`)

### Example Execution

```
Enter the input CSV file path (e.g., footing_schedule.csv): footing_schedule.csv
Enter the output CSV file path (e.g., bbs_output.csv): bbs_output.csv
Enter the pour name (default: Pour-01): Pour-01

Process completed successfully!
BBS data successfully written to bbs_output.csv
```

## Input File Format

Your input CSV file should have the following columns:

| Column | Description | Format |
|--------|-------------|--------|
| FOOTING TYPE. | Footing identifier | F1, F2, CF1, RAFT-1, etc. |
| FOOTING SIZE(LENGTH) | Length in mm | Numeric or "REFER PLAN" |
| FOOTING SIZE(BREADTH) | Breadth in mm | Numeric or "REFER PLAN" |
| DEPTH(D) | Footing depth in mm | Numeric |
| BOTTOM REINFORCEMENT(SHORT BARS) | Short bars reinforcement | T12@125C/C or "-" |
| BOTTOM REINFORCEMENT(LONG BARS) | Long bars reinforcement | T12@125C/C or "-" |
| TOP REINFORCEMENT(SHORT BARS) | Top short bars | T12@125C/C or "-" |
| TOP REINFORCEMENT(LONG BARS) | Top long bars | T12@125C/C or "-" |
| REMARKS | Additional notes | Optional |

### Reinforcement Format

Reinforcement should be in the format: **T[Diameter]@[Spacing]C/C**

Examples:
- `T12@125C/C` - 12mm diameter bars at 125mm center-to-center spacing
- `T16@100C/C` - 16mm diameter bars at 100mm center-to-center spacing
- `T20@200C/C` - 20mm diameter bars at 200mm center-to-center spacing
- `-` - No reinforcement

## Output File Format

The generated BBS CSV file contains the following columns:

| Column | Description |
|--------|-------------|
| S.No | Serial number |
| Pour | Pour/Phase name |
| Footing Mark | Footing identifier |
| Member | Member type (Footing) |
| Bar Mark | Bar identification (e.g., F1-X (Bottom)) |
| Dia (mm) | Bar diameter in mm |
| Spacing (mm) | Center-to-center spacing in mm |
| Nos | Number of bars |
| Length of One Bar (m) | Effective length of one bar in meters |
| Shape | Bar shape (Straight, Bent, etc.) |
| Total Length (m) | Total length of all bars in meters |
| Weight (kg) | Total weight of reinforcement in kg |

## Calculations

### Key Formulas Used

1. **Number of Bars**: `(Length / Spacing) + 1`
2. **Effective Length**: `Footing Length - (2 × Cover)` (Default cover: 50mm)
3. **Unit Weight**: `(Diameter² / 162)` kg/m
4. **Total Weight**: `Unit Weight × Total Length`

### Default Parameters

- **Cover**: 50mm (0.05m) - Standard concrete cover
- **Unit Weight Formula**: Based on standard steel bar weight calculations

## Error Handling

The script includes robust error handling:

- **FileNotFoundError**: Input file not found - Check file path
- **Invalid Data**: Non-numeric values are skipped with a warning message
- **Missing Columns**: Will fail gracefully if required columns are missing
- **Invalid Reinforcement Format**: Rows with unparseable reinforcement are skipped

## Sample Input Data

```csv
FOOTING TYPE.,FOOTING SIZE(LENGTH),FOOTING SIZE(BREADTH),DEPTH(D),BOTTOM REINFORCEMENT(SHORT BARS),BOTTOM REINFORCEMENT(LONG BARS),TOP REINFORCEMENT(SHORT BARS),TOP REINFORCEMENT(LONG BARS),REMARKS
F1,REFER PLAN,REFER PLAN,750,T12@125C/C,T12@125C/C,-,-,
F2,2800,2350,600,T16@100C/C,T16@100C/C,-,-,
F4,1900,1450,500,T10@125C/C,T10@125C/C,-,-,
```

## Sample Output Data

```csv
S.No,Pour,Footing Mark,Member,Bar Mark,Dia (mm),Spacing (mm),Nos,Length of One Bar (m),Shape,Total Length (m),Weight (kg)
1,Pour-01,F2,Footing,F2-X (Bottom),16,100,30,2.7,Straight,81,154.88
2,Pour-01,F2,Footing,F2-Y (Bottom),16,100,24,2.25,Straight,54,103.68
```

## Troubleshooting

### "Import pandas could not be resolved"
- Solution: Make sure pandas is installed: `pip install pandas`

### "Input file not found"
- Solution: Provide the correct full file path to your CSV file

### Empty output file
- Solution: Check that your input file contains valid footing data (not marked as REFER PLAN or NOT IN USE)

### 0 bars calculated
- Solution: Verify your reinforcement format follows the pattern: `T[Diameter]@[Spacing]C/C`

## Project Structure

```
bbs-calculator/
├── process_bbs.py              # Main BBS calculation script
├── footing_schedule.csv         # Sample input file
├── bbs_output.csv              # Generated output file
├── README.md                   # This file
└── requirements.txt            # Python dependencies
```

## How It Works

1. **Read Input**: Loads the CSV file containing footing specifications
2. **Iterate**: Processes each footing row-by-row
3. **Validate**: Skips invalid entries (REFER PLAN, NOT IN USE)
4. **Parse**: Extracts diameter and spacing from reinforcement strings
5. **Calculate**: Computes number of bars, lengths, and weights
6. **Generate**: Creates comprehensive BBS output
7. **Export**: Saves results to output CSV file

## Limitations

- Currently supports straight reinforcement bars only
- Cover is set to a default 50mm (can be modified in code)
- Assumes standard steel bar weight formula
- Does not account for development length or lap lengths
- Limited to footing members

## Future Enhancements

- [ ] Support for bent bars and complex bar shapes
- [ ] Customizable concrete cover value
- [ ] Support for multiple materials and grades
- [ ] GUI interface
- [ ] Export to Excel format
- [ ] Integration with design calculation tools

## Contributing

Contributions are welcome! Please feel free to submit issues or pull requests.

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Author

**Civil Engineering Tools**
- Email: your.email@example.com
- GitHub: [@yourusername](https://github.com/yourusername)

## Changelog

### Version 1.0.0 (Current)
- Initial release
- Support for standard footing BBS calculation
- CSV input/output functionality
- Interactive command-line interface

## Support

For issues, questions, or suggestions, please open an issue on GitHub or contact the author.

---

**Last Updated**: January 30, 2026
#   c o n c r e t e - b b s - t o o l  
 