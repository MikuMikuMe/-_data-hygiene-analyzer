# ### data-hygiene-analyzer

Creating a comprehensive Python program, `data_hygiene_analyzer`, involves crafting a tool to detect and optimize data quality issues in datasets. Here, I'll outline the code for such a tool with comments and error handling, focusing on basic data quality issues like missing values, duplicates, and data type inconsistencies.

```python
import pandas as pd
import numpy as np

class DataHygieneAnalyzer:
    def __init__(self, dataframe):
        """
        Initializes the DataHygieneAnalyzer with a pandas DataFrame.

        :param dataframe: pd.DataFrame - The data to be analyzed.
        """
        if not isinstance(dataframe, pd.DataFrame):
            raise ValueError("Input must be a pandas DataFrame.")
        self.df = dataframe

    def check_missing_values(self):
        """
        Checks for missing values in the DataFrame.

        :return: pd.Series - Number of missing values per column.
        """
        try:
            missing_values = self.df.isnull().sum()
            return missing_values
        except Exception as e:
            print(f"Error checking missing values: {e}")
            return None

    def remove_duplicates(self):
        """
        Removes duplicate rows from the DataFrame and returns the cleaned DataFrame.

        :return: pd.DataFrame - DataFrame with duplicates removed.
        """
        try:
            df_cleaned = self.df.drop_duplicates()
            return df_cleaned
        except Exception as e:
            print(f"Error removing duplicates: {e}")
            return None

    def analyze_data_types(self):
        """
        Analyzes the data types of each column in the DataFrame.

        :return: pd.DataFrame - Data type and basic stats for each column.
        """
        try:
            data_info = pd.DataFrame({'DataType': self.df.dtypes,
                                      'NumUniqueValues': self.df.nunique(),
                                      'NumNulls': self.df.isnull().sum()})
            return data_info
        except Exception as e:
            print(f"Error analyzing data types: {e}")
            return None

def load_data(file_path):
    """
    Loads data from a CSV file into a DataFrame.

    :param file_path: str - Path to the CSV file.
    :return: pd.DataFrame - Loaded DataFrame.
    """
    try:
        df = pd.read_csv(file_path)
        print(f"Data loaded successfully from {file_path}")
        return df
    except FileNotFoundError:
        print(f"File not found: {file_path}")
        return None
    except pd.errors.EmptyDataError:
        print("No data found at the specified path.")
        return None
    except Exception as e:
        print(f"Error loading data: {e}")
        return None

def main():
    # Example usage
    file_path = 'data.csv'  # Specify your data path
    data = load_data(file_path)
    
    if data is not None:
        analyzer = DataHygieneAnalyzer(data)
        
        # Check for missing values
        missing_values = analyzer.check_missing_values()
        print("\nMissing Values per Column:")
        if missing_values is not None:
            print(missing_values)

        # Remove duplicates
        data_cleaned = analyzer.remove_duplicates()
        print("\nNumber of rows after removing duplicates:", len(data_cleaned))

        # Analyze data types
        data_info = analyzer.analyze_data_types()
        print("\nData Type Analysis:")
        if data_info is not None:
            print(data_info)

if __name__ == "__main__":
    main()
```

### Code Explanation
- **DataHygieneAnalyzer Class**: 
  - It encapsulates functions to evaluate missing values, detect duplicates, and analyze data types.
  - It throws an error if initialized with a non-DataFrame object.

- **Error Handling**: 
  - Each method in `DataHygieneAnalyzer` contains try-except blocks to capture and print exceptions, ensuring any errors during processing are visible to the user.

- **load_data Function**: 
  - Loads data from a CSV file with error handling for file not found, empty data, and other exceptions.

- **main Function**: 
  - Demonstrates how to use the class and its methods, showcasing the analysis of a dataset loaded from a CSV file.

This implementation provides a fundamental approach to identifying common data quality issues and is adaptable for your specific needs by extending its methods to include more detailed checks and processing steps.