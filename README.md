# cv-peak-analyzer

Batch analysis of cyclic voltammetry data

Parses both data recorded from Digi-Ivy and Admmiral Instruments squidstat equipment.

## What it does

- Parses two file formats: DY20 files from Digi-Ivy instruments and CSVs with `Working Electrode (V)` and `Current (A)` columns
- Splits each scan into forward and reverse sweeps at the turning point
- Finds the most prominent anodic and cathodic peaks using prominence-filtered detection, ignoring capacitive startup spikes at scan boundaries
- Fits a linear tangent to the flat pre-peak baseline on each sweep
- Measures peak heights as the vertical distance from the tangent to the peak
- Saves annotated plots and a summary CSV with peak positions and heights for every experiment

## Requirements

- Python 3.8+
- pandas
- numpy
- matplotlib
- scipy

Ideally, you should run this using Jupyter Notebook (Anaconda Distribution version).

## Folder layout

```
cv-peak-analyzer/
├── cv-peak-analyzer.ipynb  # main analysis notebook
├── lab_input/              # drop your DY20 and CSV files here
├── csv_output/             # cleaned CSVs written here (timestamped subfolders)
├── plot_output/            # annotated plots written here (timestamped subfolders)
└── peak_results.csv        # summary table across all experiments
```

## Usage

1. Place DY20 or CSV files in the `lab_input/` folder
2. Open `cv-peak-analyzer.ipynb` in Jupyter
3. Run cells 1-6 once to define the parsing, analysis, and plotting functions
4. Run cell 7 to process every file in `lab_input/`
5. Run cell 8 to view the summary table

Each run creates a new timestamped folder inside `csv_output/` and `plot_output/` so results from separate runs stay separated.

## Output

### Per-experiment plot

Each plot shows:
- Red: the CV curve
- Orange: tangent lines fit to the flat baseline of each sweep
- Blue vertical bars: the measured peak heights
- Gray labels: peak current values (`i_pa`, `i_pc`)
- Blue labels: peak heights (`h_a`, `h_c`)
- Green horizontal line: zero current reference

### Summary table (`peak_results.csv`)

One row per experiment with:
- File name
- Anodic peak current and potential
- Cathodic peak current and potential
- Anodic and cathodic peak heights

## Limitations

The analyzer works well on CVs with clean reversible redox peaks. For experiments with no real peaks (featureless baselines), truncated peaks (instrument compliance saturation), or asymmetric one-directional reactions, the automatic peak detection may still pick points but the measured heights will not be chemically meaningful. Always verify results by inspecting the generated plots.

## File formats

### DY20

Text files with a metadata header followed by a data block between `Data start from here:` and `End of data` markers. Each data row has potential (V) and current (A) separated by tabs or whitespace.

### CSV

Standard CSV with a header row. The parser reads the `Working Electrode (V)` and `Current (A)` columns and ignores everything else.

## License

MIT
