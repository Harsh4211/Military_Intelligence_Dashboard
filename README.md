# 🛡 AI-Based Military Intelligence Dashboard

A comprehensive Streamlit-based intelligence analysis platform that leverages machine learning and the Global Terrorism Database (GTD) to provide real-time threat assessment, predictive analytics, and strategic intelligence insights.

## 📋 Table of Contents
- [Features](#features)
- [Project Structure](#project-structure)
- [Installation](#installation)
- [Setup & Configuration](#setup--configuration)
- [How to Run](#how-to-run)
- [Dashboard Modules](#dashboard-modules)
- [Adding More Data to the Model](#adding-more-data-to-the-model)
- [Technologies Used](#technologies-used)
- [Dataset](#dataset)

## ✨ Features

### 🏠 Home
- Welcome page with navigation guide
- Overview of all available dashboard modules

### 🌍 Global Threat Map
- Interactive global terrorism incident visualization
- Geographic distribution of attacks
- Real-time threat mapping

### 🌎 Country Analysis
- Detailed country-specific intelligence reports
- Incident statistics (fatalities, injuries, attack count)
- Top terrorist organizations operating in selected country
- Weapon type analysis
- Attack type distribution
- Historical trend analysis
- Interactive incident location map
- Data export functionality

### 🤖 Attack Prediction
- Machine learning-powered attack type prediction
- Input incident parameters (country, region, weapon type, target type, etc.)
- Real-time predictions with confidence scores
- Trained on Random Forest Classifier

### 🚨 Threat Level Prediction
- Dynamic threat assessment based on historical data
- Risk categorization (Low, Medium, High)
- Incident severity analysis

### 📈 Forecasting
- Time-series forecasting using Linear Regression
- Multi-year attack trend predictions
- Growth analysis and risk assessment
- Downloadable forecast reports

### 🧠 AI Intelligence Report
- Comprehensive AI-generated intelligence summaries
- Executive summary generation
- Strategic recommendations
- Top high-risk countries analysis
- Most active terrorist groups identification
- Report export to text file

### 📊 Data Explorer
- Advanced filtering and search capabilities
- Multi-criteria dataset filtering (year, country, region, attack type, weapon type, group)
- Interactive visualizations (charts, pie charts, bar graphs)
- Missing value analysis
- Dataset statistics and metadata
- Download filtered data as CSV

### ⚙️ Settings
- User preferences and configuration
- Model parameters adjustment
- Data refresh options

## 📁 Project Structure

```
Military_Intelligence_Dashboard/
├── app.py                          # Main Streamlit application entry point
├── train_attack_model.py           # Model training script for attack prediction
├── requirements.txt                # Python dependencies
├── .gitignore                      # Git ignore configuration
├── README.md                       # Project documentation
│
├── pages/                          # Streamlit multi-page application
│   ├── 1_🏠 Home.py               # Home page
│   ├── 2_🌍 Global_Threat_Map.py  # Global threat visualization
│   ├── 3_🌎Country_Analysis.py    # Country-specific analysis
│   ├── 4_🤖 Attack_Prediction.py  # ML-based attack prediction
│   ├── 5_🚨Threat_Level.py        # Threat level assessment
│   ├── 6_📈Forecasting.py         # Time-series forecasting
│   ├── 7_🧠 AI_Intelligence.py    # AI intelligence reports
│   ├── 8_📊Data_Explorer.py       # Advanced data exploration
│   └── 9_⚙ Setting.py             # Application settings
│
├── utils/                          # Utility modules
│   └── data_loader.py             # Data loading and caching functions
│
├── data/                           # Data directory
│   └── globalterrorism.csv        # Global Terrorism Database (to be added)
│
└── models/                         # Trained model artifacts
    ├── attack_prediction_model.pkl # Trained Random Forest model
    ├── target_encoder.pkl          # Target variable encoder
    └── feature_encoders.pkl        # Feature encoders
```

## 🛠 Installation

### Prerequisites
- Python 3.8 or higher
- pip (Python package manager)

### Step 1: Clone the Repository
```bash
git clone https://github.com/Harsh4211/Military_Intelligence_Dashboard.git
cd Military_Intelligence_Dashboard
```

### Step 2: Create a Virtual Environment (Optional but Recommended)
```bash
# On Windows
python -m venv venv
venv\Scripts\activate

# On macOS/Linux
python3 -m venv venv
source venv/bin/activate
```

### Step 3: Install Dependencies
```bash
pip install -r requirements.txt
```

## 📊 Setup & Configuration

### Step 1: Download the Global Terrorism Database
1. Visit: [https://www.start.umd.edu/gtd/](https://www.start.umd.edu/gtd/)
2. Download the full GTD dataset (CSV format)
3. Place the file in the `data/` directory with the filename: `globalterrorism.csv`

### Step 2: Train the Attack Prediction Model
Before running the dashboard, you need to train the ML model:

```bash
python train_attack_model.py
```

This will:
- Load the GTD dataset
- Preprocess and encode features
- Train a Random Forest classifier
- Save model artifacts to the `models/` directory:
  - `attack_prediction_model.pkl`
  - `target_encoder.pkl`
  - `feature_encoders.pkl`

## 🚀 How to Run

### Start the Streamlit Dashboard
```bash
streamlit run app.py
```

The dashboard will open in your default web browser at `http://localhost:8501`

### Dashboard Navigation
- Use the **left sidebar** to navigate between different modules
- Each page has specific filtering and analysis capabilities
- Download buttons are available throughout for data export

## 📱 Dashboard Modules

| Module | Purpose | Key Features |
|--------|---------|--------------|
| 🏠 Home | Entry point | Navigation guide, module overview |
| 🌍 Global Map | World overview | Global incident distribution |
| 🌎 Country | Country analysis | Detailed country-specific reports |
| 🤖 Prediction | ML inference | Attack type prediction with confidence |
| 🚨 Threat Level | Risk assessment | Threat categorization (Low/Med/High) |
| 📈 Forecasting | Future trends | Multi-year attack predictions |
| 🧠 AI Report | Intelligence | Executive summaries & recommendations |
| 📊 Explorer | Data discovery | Advanced filtering & visualization |
| ⚙️ Settings | Configuration | User preferences & parameters |

## 📈 Adding More Data to the Model

### Method 1: Update Existing CSV Dataset

#### Step 1: Prepare Your New Data
- Ensure your data follows the GTD schema structure
- Required columns: `country_txt`, `region_txt`, `weaptype1_txt`, `targtype1_txt`, `gname`, `attacktype1_txt`, `success`, `suicide`, `nkill`, `nwound`, `iyear`
- Data format: CSV with UTF-8 or Latin-1 encoding

#### Step 2: Merge Data with Existing Dataset
```python
import pandas as pd

# Load existing GTD dataset
existing_df = pd.read_csv('data/globalterrorism.csv', encoding='latin1', low_memory=False)

# Load new data
new_data = pd.read_csv('data/new_incidents.csv', encoding='utf-8')

# Merge datasets
updated_df = pd.concat([existing_df, new_data], ignore_index=True)

# Remove duplicates if any
updated_df = updated_df.drop_duplicates(subset=['eventid'], keep='first')

# Save updated dataset
updated_df.to_csv('data/globalterrorism.csv', index=False, encoding='latin1')

print(f"Dataset updated! Total incidents: {len(updated_df)}")
```

#### Step 3: Retrain the Model
```bash
python train_attack_model.py
```

### Method 2: Programmatic Data Addition

Create a script `add_data.py`:

```python
import pandas as pd
import numpy as np

def add_new_incidents(csv_file_path):
    """Add new incidents to the GTD dataset"""
    
    # Load existing data
    df = pd.read_csv('data/globalterrorism.csv', encoding='latin1', low_memory=False)
    
    # Load new incidents
    new_incidents = pd.read_csv(csv_file_path, encoding='utf-8')
    
    # Validate required columns
    required_cols = ['country_txt', 'region_txt', 'weaptype1_txt', 
                     'targtype1_txt', 'gname', 'attacktype1_txt', 
                     'success', 'suicide', 'nkill', 'nwound', 'iyear']
    
    missing_cols = [col for col in required_cols if col not in new_incidents.columns]
    if missing_cols:
        print(f"Error: Missing columns: {missing_cols}")
        return False
    
    # Combine datasets
    combined_df = pd.concat([df, new_incidents], ignore_index=True)
    
    # Remove duplicates based on event ID (if available)
    if 'eventid' in combined_df.columns:
        combined_df = combined_df.drop_duplicates(subset=['eventid'], keep='first')
    
    # Save updated dataset
    combined_df.to_csv('data/globalterrorism.csv', index=False, encoding='latin1')
    
    print(f"✅ Successfully added {len(new_incidents)} new incidents")
    print(f"📊 Total incidents in dataset: {len(combined_df)}")
    
    return True

# Usage
if __name__ == "__main__":
    add_new_incidents('data/new_incidents.csv')
```

Run the script:
```bash
python add_data.py
```

### Method 3: Real-Time Data Integration (Advanced)

Create `data_integration.py` for automated data updates:

```python
import pandas as pd
import requests
from datetime import datetime

def fetch_and_integrate_data():
    """Fetch new data from external sources and integrate"""
    
    # Load existing dataset
    df = pd.read_csv('data/globalterrorism.csv', encoding='latin1', low_memory=False)
    
    # Your logic to fetch new data from API, web scraping, etc.
    # Example: new_data = requests.get('your_api_endpoint').json()
    
    # Convert to DataFrame and merge
    # new_df = pd.DataFrame(new_data)
    # updated_df = pd.concat([df, new_df], ignore_index=True)
    
    # Remove duplicates
    # updated_df = updated_df.drop_duplicates()
    
    # Save
    # updated_df.to_csv('data/globalterrorism.csv', index=False, encoding='latin1')
    
    print(f"✅ Data updated at {datetime.now()}")

if __name__ == "__main__":
    fetch_and_integrate_data()
```

### After Adding Data:

1. **Retrain the model:**
   ```bash
   python train_attack_model.py
   ```

2. **Restart the dashboard:**
   ```bash
   streamlit run app.py
   ```

3. **Verify updates:**
   - Check 📊 Data Explorer for new records
   - Validate predictions in 🤖 Attack Prediction module
   - Review updated trends in 📈 Forecasting

## 📦 Technologies Used

| Technology | Purpose |
|------------|---------|
| **Streamlit** | Web framework for interactive dashboard |
| **Pandas** | Data manipulation and analysis |
| **NumPy** | Numerical computations |
| **Scikit-learn** | Machine learning (Random Forest, Linear Regression) |
| **Plotly** | Interactive data visualizations |
| **Joblib** | Model serialization and persistence |

## 📊 Dataset

### Global Terrorism Database (GTD)
- **Source:** https://www.start.umd.edu/gtd/
- **Coverage:** 1970-present
- **Records:** 200,000+ incidents
- **Variables:** 135+ data fields per incident
- **Encoding:** Latin-1

### Key Data Fields Used:
- `iyear` - Incident year
- `country_txt` - Country name
- `region_txt` - Geographic region
- `city` - City of incident
- `latitude`, `longitude` - Geographic coordinates
- `attacktype1_txt` - Type of attack
- `weaptype1_txt` - Weapon type
- `targtype1_txt` - Target type
- `gname` - Terrorist group name
- `nkill` - Number of fatalities
- `nwound` - Number of injured
- `success` - Attack success indicator
- `suicide` - Suicide attack indicator

## 📝 License

This project is open-source and available for educational and research purposes.

## 🤝 Contributing

To contribute to this project:
1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Submit a pull request

## 📧 Support

For issues, questions, or suggestions, please open an issue on GitHub.

---

**Last Updated:** July 2026  
**Maintainer:** Harsh4211
