# Internship Placement Selection System

This project is an automated system designed to assign Pharmacy students at **Chulalongkorn University** to internship sites (drugstores/hospitals). It allocates placements based on student preferences (ranked choices of shift and site), shift availability, and specific site constraints such as seat capacity and gender requirements.

## Features

*   **Ranked-Choice Allocation**: Processes student preferences from Choice 1 to Choice `RANK`. Each choice specifies both a desired shift and a desired site.
*   **Shift Management**: Handles multiple internship shifts (e.g., Shift 1, Shift 2).
*   **Constraint Enforcement**:
    *   **Seat Capacity**: Ensures sites do not exceed their maximum number of students per shift.
    *   **Gender Requirements**: Respects site-specific requirements (Male only, Female only, or Both).
*   **Fair Tie-Breaking**: Uses a random lottery system for oversubscribed sites within the same choice index and shift.
*   **Validation & Verification**: Includes comprehensive checks for data integrity, capacity violations, duplicate site choices, and logic errors.
*   **Analysis**: Generates summary statistics and visualizations of the allocation results (assignments by choice index, by shift, and top sites).

## Prerequisites

*   Python 3.x
*   Jupyter Notebook or JupyterLab
*   Required Python libraries:
    *   `pandas`
    *   `numpy`
    *   `matplotlib`

## Input Files

The system expects two CSV files in the project directory. Column names are case-insensitive and space-tolerant.

### 1. Student Selection File (`student_selections_new.csv`)
Contains student details and their ranked choices.
*   **Columns**: `student_name`, `student_id`, `sex`, `shift 1`, `rank 1`, `shift 2`, `rank 2`, ..., `shift {RANK}`, `rank {RANK}`
*   **Values**:
    *   `sex`: `Male`, `Female`, or `Both`
    *   `shift i`: Integer representing the chosen shift (e.g., `1` to `SHIFT_ORDER`)
    *   `rank i`: The code of the internship site for choice `i`.

### 2. Drugstore/Site File (`drugstore_path.csv`)
Contains details about internship sites, capacities, and requirements.
*   **Columns**: `code`, `branch`, `sex_require1`, `seat1`, ..., `sex_require{SHIFT_ORDER}`, `seat{SHIFT_ORDER}`
*   **Values**:
    *   `sex_require{j}`: `Male`, `Female`, `Both`
    *   `seat{j}`: Integer (number of available seats for shift `j`).

## Usage

1.  **Prepare Data**: Ensure your student and drugstore CSV files are formatted correctly and placed in the project folder.
2.  **Configure**: Open `test_internshipPlacementSelectionSystem.ipynb`. In **Cell 1 - Configuration**, you can adjust:
    *   `RANK`: Number of ranked choices each student submits (default: 5).
    *   `SHIFT_ORDER`: Number of available shifts (default: 2).
    *   `student_path`: Path to the Students CSV.
    *   `drugstore_path`: Path to the Drugstores CSV.
    *   `output_path`: Directory to save the results.
    *   `random_seed`: Set a seed for reproducible results (optional).
3.  **Run**: Execute the notebook cells sequentially from top to bottom.
4.  **Results**:
    *   The system will generate an output CSV in the `output/` directory named with the current timestamp (e.g., `YYYYMMDD_HHMMSS_output.csv`).
    *   A summary of remaining seats is also saved as `YYYYMMDD_HHMMSS_remaining_seats.csv`.

## Allocation Logic

1.  **Normalization**: All inputs are normalized (case-insensitive, space trimming).
2.  **Deduplication**: Duplicate site codes within a single student's rank list are removed (keeping the first occurrence).
3.  **Allocation Loop**:
    *   Iterate through **Choice Index** (1 to `RANK`).
    *   For each choice, read the student's `(shift i, rank i)` pair.
    *   Identify eligible candidates for each `(site, shift)` group who have not yet been assigned.
    *   Check constraints (Sex, Shift, Remaining Seats).
    *   **Lottery**: If candidates > available seats, a random selection is performed.
4.  **Unassigned Students**: Students not matched after all choices are processed are marked as "Not selected".

## Output

The output file contains the original student data (`student_name` and `student_id`) plus:
*   `rank_result`: The choice index they were assigned (1 to `RANK`, or 0 if not selected).
*   `shift_result`: The shift they were assigned to (or 0 if not selected).
*   `result`: The code of the assigned site (or "Not selected").
*   `branch`: The branch name of the assigned site (empty if not selected).

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---
*Developed for Chulalongkorn University Pharmacy Internship Placement.*
