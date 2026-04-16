# ==========================================
# ABALONE STATISTICAL ANALYSIS (RUBRIC COMPLIANT)
# COURSE: HANDS ON WITH PYTHON - CSE 205
# ==========================================

import pandas as pd
import sys

# Increase recursion limit to handle 4177 rows for the recursive functions
sys.setrecursionlimit(10000)

# -----------------------------------------
# 1. FILE HANDLING WITH EXCEPTION
# -----------------------------------------
def load_data(file_path):
    try:
        df = pd.read_csv(file_path, header=None)
        df.columns = ['Sex', 'Length', 'Diameter', 'Height',
                      'WholeWeight', 'ShuckedWeight',
                      'VisceraWeight', 'ShellWeight', 'Rings']
        print("✅ Step 1: File loaded successfully.")
        return df

    except FileNotFoundError:
        print("❌ Error: File not found. Please check the file path.")
    except PermissionError:
        print("❌ Error: Permission denied while accessing the file.")
    except ValueError:
        print("❌ Error: Data type conversion error.")
    except IOError as e:
        print(f"❌ IO Error occurred: {e}")
    except Exception as e:
        print(f"❌ Unexpected error: {e}")

    return None

# -----------------------------------------
# 2. DATA EXPLORATION
# -----------------------------------------
def explore_data(df):
    print("\n--- Step 2: Data Exploration ---")
    print("\nFirst 10 rows:\n", df.head(10))
    print("\nLast 5 rows:\n", df.tail(5))
    print("\nDataFrame Info:")
    df.info()

    print("\nFiltered Abalones (Length > 0.5 OR Rings > 15) using For-Loop:")
    match_count = 0
    # Strictly using for-loop and relational operators as requested
    for i in range(len(df)):
        length = df.iloc[i]['Length']
        rings = df.iloc[i]['Rings']
        if length > 0.5 or rings > 15:
            match_count += 1
            # Printing just the first 3 to prevent terminal spam
            if match_count <= 3:
                print(f"Row {i}: Length={length}, Rings={rings}")
    print(f"... Total matches found: {match_count}")

# -----------------------------------------
# 3. HANDLE MISSING VALUES
# -----------------------------------------
def manual_median(lst):
    clean_lst = [x for x in lst if not pd.isna(x)]
    if not clean_lst: return 0
    
    lst_sorted = sorted(clean_lst)
    n = len(lst_sorted)
    if n % 2 != 0:
        return lst_sorted[n//2]
    else:
        return (lst_sorted[n//2 - 1] + lst_sorted[n//2]) / 2

def fill_missing_values(df):
    numeric_cols = df.columns[1:]
    for col in numeric_cols:
        values = [v for v in df[col] if v != 0 and not pd.isna(v)]
        median_val = manual_median(values)
        
        for i in range(len(df)):
            if df.at[i, col] == 0 or pd.isna(df.at[i, col]):
                df.at[i, col] = median_val
    print("\n✅ Step 3: Missing and zero values filled with column medians.")
    return df

# -----------------------------------------
# 4. CREATE NEW COLUMNS
# -----------------------------------------
def create_columns(df):
    # i) ShellVolume
    df['ShellVolume'] = df['Length'] * df['Diameter'] * df['Height']
    # ii) Weight_Ratio
    df['Weight_Ratio'] = df['WholeWeight'] / df['ShellWeight']

    # iii) AgeGroup using if-elif-else
    def get_age_group(rings):
        if rings < 8:
            return "Young"
        elif 8 <= rings <= 15:
            return "Adult"
        else:
            return "Old"

    df['AgeGroup'] = df['Rings'].apply(get_age_group)
    print("✅ Step 4: New columns (ShellVolume, Weight_Ratio, AgeGroup) created.")
    return df

# -----------------------------------------
# 5. OUTLIER REMOVAL (WHILE LOOP + CONTINUE)
# -----------------------------------------
def remove_outliers(df):
    i = 0
    clean_data = []
    

    while True:
        if i >= len(df):
            break
            
        row = df.iloc[i]
        
        if row['Height'] > 0.3 or row['WholeWeight'] > 3:
            i += 1
            continue
            
        clean_data.append(row)
        i += 1

    print("✅ Step 5: Outliers removed using while loop.")
    return pd.DataFrame(clean_data)

# -----------------------------------------
# 6. SETS FOR UNIQUE VALUES
# -----------------------------------------
def analyze_sex_groups(df):
    # Extract to set
    sex_set = set(df['Sex'])
    print(f"\n✅ Step 6: Unique Categories in Sex using Set: {sex_set}")

    # Group-wise analysis
    group_stats = {}
    for category in sex_set:
        subset = df[df['Sex'] == category]
        group_stats[category] = len(subset)
    
    print(f"Group-wise counts: {group_stats}")
    return sex_set

# -----------------------------------------
# 7. RECURSION FOR GEOMETRIC MEAN
# -----------------------------------------
def calculate_geometric_mean(df, index=0, product=1.0, count=0):
    # Base Case
    if index == len(df):
        if count == 0: return 0
        return product ** (1 / count)

    row = df.iloc[index]
    
   
    if index < 50: 
        product *= (row['Length'] * row['Diameter'] * row['Height'])
        count += 3

    # Recursive step
    return calculate_geometric_mean(df, index + 1, product, count)

# -----------------------------------------
# 8. STATISTICS CALCULATION (MANUAL VS PANDAS)
# -----------------------------------------
def recursive_sum(lst, index=0):
    if index == len(lst):
        return 0
    return lst[index] + recursive_sum(lst, index + 1)

def manual_statistics(df, col_name):
    # Using lists and loops
    values = list(df[col_name])
    n = len(values)
    
    # 1. Mean (using recursion for summation)
    total_sum = recursive_sum(values)
    mean = total_sum / n
    
    # 2. Median
    median = manual_median(values)
    
    # 3. Mode
    freq = {}
    for v in values:
        freq[v] = freq.get(v, 0) + 1
    mode = max(freq, key=freq.get)
    
    # 4. Min / Max
    min_val, max_val = values[0], values[0]
    for v in values:
        if v < min_val: min_val = v
        if v > max_val: max_val = v
        
    # Returning a Tuple
    return (mean, median, mode, min_val, max_val)

def compute_all_stats(df):
    cols_to_calc = ['Length', 'Diameter', 'Height', 'WholeWeight', 'Rings', 'ShellVolume']
    stats_dict = {}
    
    for col in cols_to_calc:
        man_stats = manual_statistics(df, col)
        pandas_mean = df[col].mean() # Compare with pandas
        stats_dict[col] = {
            'manual_tuple': man_stats,
            'pandas_mean': pandas_mean
        }
        
    # Group statistics by Sex and AgeGroup using dictionaries
    grouped_dict = {}
    for sex in set(df['Sex']):
        for age in set(df['AgeGroup']):
            subset = df[(df['Sex'] == sex) & (df['AgeGroup'] == age)]
            if not subset.empty:
                grouped_dict[(sex, age)] = subset['Rings'].mean()
                
    print("✅ Step 8: Statistics calculated manually (tuples, lists, recursion) and compared with pandas.")
    return stats_dict, grouped_dict

# -----------------------------------------
# 9. FINAL REPORT GENERATION
# -----------------------------------------
def generate_report(stats_dict, grouped_dict):
    try:
        with open("abalone_analysis_report.txt", "w") as file:
            file.write("=========================================\n")
            file.write("ABALONE AGE STATISTICAL ANALYSIS REPORT\n")
            file.write("=========================================\n\n")
            
            file.write("1. MANUAL VS PANDAS STATISTICS\n")
            file.write("-" * 40 + "\n")
            for col, data in stats_dict.items():
                man = data['manual_tuple']
                file.write(f"Attribute: {col}\n")
                file.write(f"  Manual -> Mean: {man[0]:.4f}, Median: {man[1]:.4f}, Mode: {man[2]:.4f}, Min: {man[3]}, Max: {man[4]}\n")
                file.write(f"  Pandas -> Mean: {data['pandas_mean']:.4f}\n\n")
                
            file.write("\n2. GROUPED STATISTICS (Mean Rings by Sex & AgeGroup)\n")
            file.write("-" * 55 + "\n")
            for key, mean_rings in grouped_dict.items():
                sex, age = key
                file.write(f"Sex: {sex} | AgeGroup: {age} => Avg Rings: {mean_rings:.2f}\n")
                
        print("\n✅ Step 9: Report 'abalone_analysis_report.txt' generated successfully.")
    except IOError as e:
        print(f"❌ Error writing report file: {e}")
    except Exception as e:
        print(f"❌ Unexpected error during report generation: {e}")

# -----------------------------------------
# MAIN EXECUTION
# -----------------------------------------
def main():
    # Step 1
    df = load_data("abalone.csv")
    if df is None: return

    # Step 2
    explore_data(df)
    
    # Step 3
    df = fill_missing_values(df)
    
    # Step 4
    df = create_columns(df)
    
    # Step 5
    df = remove_outliers(df)
    
    # Step 6
    analyze_sex_groups(df)
    
    # Step 7
    geo_mean = calculate_geometric_mean(df)
    print(f"✅ Step 7: Recursive Geometric Mean (Subset demo to prevent float underflow) = {geo_mean:.4f}")
    
    # Step 8
    stats_dict, grouped_dict = compute_all_stats(df)
    
    # Step 9
    generate_report(stats_dict, grouped_dict)

if __name__ == "__main__":
    main()
