# CS_ExportToCSV
This repository contains the README file for a GH definition that runs a multi-zone thermal analysis using ClimateStudio and exports results directly to a CSV without the need to load the .csr on the Rhino UI. 

## Overview

This Grasshopper definition performs a thermal simulation using Climate Studio (EnergyPlus) and exports selected results to a CSV file.

The workflow is based on the multi-zone baseline with shading but can be adapted to different geometries, climates, and analysis scenarios.

The output CSV contains:
- one column per selected variable per selected zone  
- hourly values (8760 rows)

N.B. Contrarily to the usual .csv files exported by CS through Rhino, the output file has a single-line header with labels in the following format:
    "ZONE_NAME, Variable_Name [Unit]"      


## Required User Inputs

Each user must update the following parameters before running the simulation.

### 1. Output Directory

Define the folder where CSV files will be saved.

**Component:**
- Panel connected to `out_directory`

**Example:**
"C:\Users...\Project\csv_results"


### 2. Simulation Name

A Value List component is used to define the simulation identifier.

- This name is also used as the output CSV file name  
- Use consistent and descriptive naming for different scenarios  


### 3. Weather File (EPW)

The EPW file is also controlled via a Value List.

Make sure:
- the file path is valid on your machine  
- paths are updated if working across different computers  


### 4. Geometry Inputs

Update the Breps connected to:
- study rooms  
- corridors  
- offices  

These are located at the beginning of the Grasshopper definition.


## Workflow Instructions

### Step 1 — Select Output Variables

In the **Load CS Raw Results** component:

1. Click on **Outputs**  
2. Select:
   - zones  
   - variables (e.g. lighting energy, equipment, solar gains, etc.)

**Important:**  
If a variable is not available (e.g. ACH), enable it first in the **Run E+** component. It will then become available in the results component.


### Step 2 — Enable Results Loading

Set the Boolean Toggle connected to the results component to:
    True
This allows Grasshopper to read simulation outputs.


### Step 3 — Run Simulation

Run the **Run E+** component.

After completion:
- results are passed to the Python component  
- the CSV file is automatically generated  


### Step 4 — Check Output

The CSV file will be saved in:
    "[out_directory] / [simulation_name].csv"


## Notes and Best Practices

- Use clear and consistent naming for simulation scenarios  
- Avoid special characters in file names  
- Ensure all file paths are valid on your system  
- Do not modify the Python component unless necessary  


## Troubleshooting

### CSV file not generated

- Verify that the Boolean Toggle is set to `True`  
- Check that the output directory exists  
- Confirm that the simulation completed successfully  


### Missing variables

- Enable the variables in the **Run E+** component  
- Re-run the simulation  


### Empty CSV file

- Ensure that variables are selected in the **Load CS Raw Results** component  


## Implementation Notes

The Python component:
- converts Grasshopper DataTrees into a tabular format  
- maps each variable to a column  
- exports results as a CSV file  

No manual editing of the script is required for standard use.
