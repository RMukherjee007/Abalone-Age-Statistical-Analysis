# 🐚 Abalone Age Statistical Analysis

A comprehensive statistical analysis project built in Python to analyze the **Abalone dataset** using both **manual algorithms and pandas**, following strict academic constraints and rubric requirements.

---

## 📌 Project Overview

This project performs **end-to-end data analysis** on the Abalone dataset, including:

* Data loading with exception handling
* Data exploration using loops
* Handling missing values manually
* Feature engineering
* Outlier removal using control structures
* Statistical analysis (manual vs pandas)
* Recursive computations
* Report generation

The project strictly follows **Hands-On Python (CSE 205)** guidelines and demonstrates strong fundamentals in Python programming and data analysis.

---

## ⚙️ Tech Stack

* **Language:** Python
* **Libraries:** pandas
* **Concepts Used:**

  * Recursion
  * File handling
  * Exception handling
  * Loops (for, while)
  * Conditional logic
  * Lists, sets, dictionaries
  * Manual statistical computation

---

## 📊 Features Implemented

### 1. File Handling & Exception Handling

* Robust CSV loading with multiple exception cases:

  * File not found
  * Permission issues
  * Data errors

---

### 2. Data Exploration

* Displays:

  * First 10 rows
  * Last 5 rows
  * DataFrame info
* Custom filtering using **for-loop and relational operators**

---

### 3. Missing Value Handling (Manual)

* Custom **median calculation function**
* Replaces:

  * Missing values
  * Zero values
* Does NOT rely on pandas built-in median

---

### 4. Feature Engineering

New columns created:

* `ShellVolume = Length × Diameter × Height`
* `Weight_Ratio = WholeWeight / ShellWeight`
* `AgeGroup`:

  * Young (< 8 rings)
  * Adult (8–15 rings)
  * Old (> 15 rings)

---

### 5. Outlier Removal

* Implemented using:

  * `while` loop
  * `continue` statement
* Removes:

  * Height > 0.3
  * WholeWeight > 3

---

### 6. Set-Based Analysis

* Extracts unique categories from `Sex`
* Performs group-wise counting

---

### 7. Recursive Computation

* Recursive calculation of **Geometric Mean**
* Includes safe handling to prevent overflow

---

### 8. Statistical Analysis (Manual vs Pandas)

For each attribute:

* Mean (via recursion)
* Median (manual)
* Mode
* Min / Max

Comparison with pandas mean included.

Also includes:

* Grouped statistics by **Sex & AgeGroup**

---

### 9. Report Generation

* Automatically generates:

  `abalone_analysis_report.txt`

* Contains:

  * Manual vs pandas comparison
  * Grouped statistics

---

## 📂 Project Structure

```
Abalone-Age-Statistical-Analysis/
│── abalone.csv
│── main.py
│── abalone_analysis_report.txt
│── README.md
```

---

## ▶️ How to Run

1. Clone the repository:

```bash
git clone https://github.com/RMukherjee007/Abalone-Age-Statistical-Analysis.git
cd Abalone-Age-Statistical-Analysis
```

2. Install dependencies:

```bash
pip install pandas
```

3. Run the program:

```bash
python main.py
```

---

## 📈 Key Highlights

* Implements **manual statistics without relying on libraries**
* Uses **recursion in real-world computation**
* Demonstrates **strong control over core Python concepts**
* Clean modular design with multiple functions
* Fully aligned with academic rubric requirements

---

## 🚀 Future Improvements

* Add data visualizations (matplotlib / seaborn)
* Build a simple web dashboard
* Integrate machine learning for age prediction
* Optimize recursive functions

---

## 👨‍💻 Author

**Raghav Mukherjee**

---

## 📜 License

This project is for academic and learning purposes.

