# Internship Placement Selection System

This project is an automated system designed to assign Pharmacy students at **Chulalongkorn University** to internship sites (drugstores/hospitals). It allocates placements based on student preferences (ranked choices), shift availability, and specific site constraints such as seat capacity and gender requirements.

## Features

*   **Ranked-Choice Allocation**: Processes student preferences from Rank 1 to Rank X.
*   **Shift Management**: Handles multiple internship shifts (e.g., Shift 1, Shift 2).
*   **Constraint Enforcement**:
    *   **Seat Capacity**: Ensures sites do not exceed their maximum number of students per shift.
    *   **Gender Requirements**: Respects site-specific requirements (Male only, Female only, or Both).
*   **Fair Tie-Breaking**: Uses a random lottery system for oversubscribed sites within the same rank and shift.
*   **Validation & Verification**: Includes comprehensive checks for data integrity, capacity violations, and logic errors.
*   **Analysis**: Generates summary statistics and visualizations of the allocation results.

## Prerequisites

*   Python 3.x
*   Jupyter Notebook
*   Required Python libraries:
    *   `pandas`
    *   `numpy`
    *   `matplotlib`

## Input Files

The system expects two CSV files in the project directory:

### 1. Student Selection File (`student_selection.csv`)
Contains student details and their ranked choices.
*   **Columns**: `student_name`, `student_id`, `sex`, `shift`, `rank 1`, `rank 2`, ..., `rank x`
*   **Values**:
    *   `sex`: `Male`, `Female`
    *   `shift`: Integer (e.g., `1`, `2`)
    *   `rank n`: The code of the internship site.

### 2. Drugstore/Site File (`drugstore_path.csv`)
Contains details about internship sites, capacities, and requirements.
*   **Columns**: `code`, `branch`, `sex_require1`, `seat1`, ..., `sex_requireN`, `seatN` (where N is the number of shifts).
*   **Values**:
    *   `sex_require{i}`: `Male`, `Female`, `Both`
    *   `seat{i}`: Integer (number of available seats for shift `i`).

## Usage

1.  **Prepare Data**: Ensure `student_selection.csv` and `drugstore_path.csv` are formatted correctly and placed in the project folder.
2.  **Configure**: Open `internshipPlacementSelectionSystem.ipynb`. In **Cell 1**, you can adjust:
    *   `x`: Number of ranks per student.
    *   `SHIFT_ORDER`: Number of shifts to process.
    *   `random_seed`: Set a seed for reproducible results (optional).
3.  **Run**: Execute the notebook cells from top to bottom.
4.  **Results**:
    *   The system will generate an output CSV in the `output/` directory named with the current timestamp (e.g., `YYYYMMDD_HHMMSS_output.csv`).
    *   A summary of remaining seats is also saved as `YYYYMMDD_HHMMSS_remaining_seats.csv`.

## Allocation Logic

1.  **Normalization**: All inputs are normalized (case-insensitive, space trimming).
2.  **Deduplication**: Duplicate site codes within a single student's rank list are removed (keeping the highest rank).
3.  **Allocation Loop**:
    *   Iterate through **Shifts** (1 to N).
    *   Iterate through **Ranks** (1 to X).
    *   Identify eligible candidates for each site who have not yet been assigned.
    *   Check constraints (Sex, Shift, Remaining Seats).
    *   **Lottery**: If candidates > available seats, a random selection is performed.
4.  **Unassigned Students**: Students not matched after all ranks are processed are marked as "Not selected".

## Output

The output file contains the original student data plus:
*   `rank_result`: The rank number they were assigned (0 if not selected).
*   `result`: The code of the assigned site (or "Not selected").

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---
*Developed for Chulalongkorn University Pharmacy Internship Placement.*
