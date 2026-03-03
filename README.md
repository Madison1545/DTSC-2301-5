# DTSC-2301-5
# Import Packages
import matplotlib.pyplot as plt
import numpy as np
import pandas as pd
import seaborn as sns
from sklearn.preprocessing import MinMaxScaler, StandardScaler, RobustScaler

# %%
# Load Dataset
pat_data = pd.read_csv(r"C:\\Users\\MRyan\\Downloads\\WNBA Senior Year Stats.csv")
pat_data_1 = pd.read_csv(r"C:\\Users\\MRyan\\Downloads\\WNBA Rookie Stats.csv")
pat_data_2 = pd.read_csv(r"C:\Users\MRyan\OneDrive\Desktop\DTSC 2301\WNBA Stats 2020-2025")


# %% [markdown]
# Look at each dataset

# %%
pat_data

# %%
pat_data_1

# %%
pat_data_2.tail()

# %%
# Get information about datatypes for each dataset
pat_data.info()

# %%
pat_data_1.info()

# %%
pat_data_2.info()

# %%
def get_unique_vals(col):
    return pat_data[col].unique().tolist()

# %%
# Identify categorical columns:
cat_cols = pat_data.select_dtypes(include=['object']).columns.tolist()

# %%
# Create a for loop that prints unique values in each categorical column in form "Column_name : [list of unique values]"
for cat in cat_cols:
    print(f"{cat} : {get_unique_vals(cat)}")

# %% [markdown]
# Creating Visuals

# %%
# columns to stack
stack_cols = ['MIN', 'PTS', 'REB', 'AST', 'STL', 'BLK', 'TO', 'FG%', 'FT%', '3P%']

# ensure numeric
for c in stack_cols:
    pat_data[c] = pd.to_numeric(pat_data[c], errors='coerce')

# Plot
plot_df = pat_data.set_index('Name')[stack_cols]
ax = plot_df.plot(kind='bar', stacked=True, figsize=(12,6))
ax.set_xlabel('Player')
ax.set_ylabel('Count')
ax.set_title('Senior Year College Stats of WNBA Players')
plt.xticks(rotation=45, ha='right')
plt.tight_layout()
plt.show()

# %%
stack_cols2 = ['GP','MIN', 'PTS', 'OR', 'DR', 'REB', 'AST', 'STL', 'BLK', 'TO', 'PF', 'AST/TO']

# ensure numeric
for c in stack_cols2:
    pat_data_1[c] = pd.to_numeric(pat_data_1[c], errors='coerce')

# Plot
plot_df = pat_data_1.set_index('Name')[stack_cols2]
ax = plot_df.plot(kind='bar', stacked=True, figsize=(12,6))
ax.set_xlabel('Player')
ax.set_ylabel('Count')
ax.set_title('Rookie Year Stats of WNBA Players')
plt.xticks(rotation=45, ha='right')
plt.tight_layout()
plt.show()

# %%
sns.histplot(x="athlete_display_name", data=pat_data_2, bins=10)

# %% [markdown]
# Cleaning the data

# %%
pat_data_2.isnull().sum()

# %%
usecols = ["athlete_display_name","minutes","field_goals_made","field_goals_attempted","three_point_field_goals_made","three_point_field_goals_attempted","free_throws_made","free_throws_attempted","offensive_rebounds","defensive_rebounds","rebounds","assists","steals","blocks","turnovers","fouls","plus_minus","points"]
df = pd.read_csv(r"C:\Users\MRyan\OneDrive\Desktop\DTSC 2301\WNBA Stats 2020-2025", usecols=usecols)

# %%
df.head()

# %%
# drop duplicate rows based on the player‑name column
df = df.drop_duplicates(subset=["athlete_display_name"], keep="first")

# %%
# keep only the six specified players in `df`
selected = [
    "Nneka Ogwumike", "Caitlin Clark", "Angel Reese",
    "Chelsea Gray", "Sabrina Ionescu", "A'ja Wilson"
]
df = df[df["athlete_display_name"].isin(selected)]
df.head()

# %%
df

# %%
stack_cols3 = ['minutes','field_goals_made','field_goals_attempted','three_point_field_goals_made','three_point_field_goals_attempted','free_throws_made','free_throws_attempted','offensive_rebounds','defensive_rebounds','rebounds','assists','steals','blocks','turnovers','fouls','plus_minus','points']

# ensure numeric
for c in stack_cols3:
    pat_data_1[c] = pd.to_numeric(pat_data_2[c], errors='coerce')

# Plot
plot_df = pat_data_1.set_index('Name')[stack_cols3]
ax = plot_df.plot(kind='bar', stacked=True, figsize=(12,6))
ax.set_xlabel('Player')
ax.set_ylabel('Count')
ax.set_title('2025 Stats of WNBA Players')
plt.xticks(rotation=45, ha='right')
plt.tight_layout()
plt.show()
