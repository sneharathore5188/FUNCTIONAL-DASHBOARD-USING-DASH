# Functional Dashboard Using Dash

## Overview
This project implements an interactive dashboard using Dash and Plotly to visualize the Iris dataset. The dashboard includes dropdown-based filtering and dynamic visualizations, such as scatter plots, histograms, and pie charts.

## Features
- Dropdown menu to select species
- Scatter plot of Sepal Length vs. Sepal Width
- Histogram of Petal Length distribution
- Pie chart showing the distribution of species

## Installation
To run this dashboard, install the required dependencies using:
```bash
pip install dash plotly pandas
```

## Dataset
The dashboard uses the **Iris dataset**, which is loaded from an external source:
```python
url = "https://raw.githubusercontent.com/mwaskom/seaborn-data/master/iris.csv"
df = pd.read_csv(url)
```

## Project Structure
- **Initialize Dash App**: Creates a Dash instance.
- **Layout Definition**: Defines the structure of the dashboard using Dash components.
- **Callbacks**: Updates graphs dynamically based on the selected species.
- **Running the App**: The script runs a Dash server to display the interactive dashboard.

## Code Overview
### 1. Import Required Libraries
```python
import dash
from dash import dcc, html
from dash.dependencies import Input, Output
import pandas as pd
import plotly.express as px
```

### 2. Load Dataset
```python
url = "https://raw.githubusercontent.com/mwaskom/seaborn-data/master/iris.csv"
df = pd.read_csv(url)
```

### 3. Initialize Dash App
```python
app = dash.Dash(__name__)
```

### 4. Define Layout
```python
app.layout = html.Div([
    html.H1("Iris Dataset Interactive Dashboard", style={'textAlign': 'center'}),
    
    dcc.Dropdown(
        id="species-dropdown",
        options=[{'label': species, 'value': species} for species in df['species'].unique()],
        value=df['species'].unique()[0],
        clearable=False,
        style={"width": "50%", "margin": "auto"}
    ),
    
    dcc.Graph(id="scatter-plot"),
    dcc.Graph(id="histogram"),
    dcc.Graph(id="pie-chart")
])
```

### 5. Define Callbacks
```python
@app.callback(
    [Output("scatter-plot", "figure"),
     Output("histogram", "figure"),
     Output("pie-chart", "figure")],
    [Input("species-dropdown", "value")]
)
def update_graphs(selected_species):
    filtered_df = df[df['species'] == selected_species]
    
    scatter_fig = px.scatter(filtered_df, x="sepal_length", y="sepal_width",
                             color="species", title=f"Sepal Length vs Sepal Width - {selected_species}")
    
    hist_fig = px.histogram(filtered_df, x="petal_length", nbins=20,
                            title=f"Petal Length Distribution - {selected_species}", color="species")
    
    pie_fig = px.pie(df, names="species", title="Overall Species Distribution")
    
    return scatter_fig, hist_fig, pie_fig
```

### 6. Run the Application
```python
if __name__ == "__main__":
    app.run_server(debug=True)
```

## Running the Dashboard
To start the dashboard, run the script:
```bash
python app.py
```
Then, open your browser and navigate to **http://127.0.0.1:8050/**.

## Output
The dashboard provides:
- **Scatter Plot**: Visualizes the relationship between Sepal Length and Sepal Width for the selected species.
- **Histogram**: Displays the distribution of Petal Length for the selected species.
- **Pie Chart**: Shows the overall distribution of species in the dataset.


