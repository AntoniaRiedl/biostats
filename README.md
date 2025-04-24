# biostats
Libraries Used

    Shiny: This is a framework used to build web applications in R. It's designed to let you create interactive, web-based interfaces for your R code.

    readxl: This allows the app to read data from Excel files.

    tidyverse: A collection of R packages for data manipulation and visualization, such as dplyr, ggplot2, etc.

    drc: This package is used for dose-response curve fitting, which is useful for fitting a standard curve in ELISA.

    writexl: This package helps with writing data to Excel files.

    scales: This package is used for scaling and formatting data for presentation, such as for adjusting axis labels or coloring.

User Interface (UI)

This part defines the layout and elements that the user interacts with.

    File Input: The user uploads their 96-well ELISA data from an Excel file (fileInput("file", ...)).

    Checkbox: Option to indicate if the uploaded Excel file contains column headers (checkboxInput("has_header", ...)).

    Well Selection: Users can select wells on the ELISA plate for blank controls, standard controls, and unknown samples. The selectInput controls are dynamically created for each sample added.

    Text Input: Users enter the concentrations of the standard solutions in a comma-separated format.

    Action Buttons: Two buttons allow users to add more sample groups (add_sample) and trigger the analysis (analyze).

    Download Button: After analysis, users can download the results as a PDF file.

Server Logic (the server function)

The server part of the app defines what happens when users interact with the app. Here’s a breakdown of key parts:

    Handling Dynamic Sample Groups:

        Adding Samples: Every time the user clicks "Add Sample," a new sample group is created and added to the list of sample groups. Each sample group can have a name and a set of wells selected.

    Data Upload and Preprocessing:

        Reading Excel Data: The plate_data reactive expression loads the Excel file and processes it into a "long" format (i.e., rows of data for each well).

        Data Transformation: The code cleans the data, converts numeric values, and reshapes it into a "long" format with columns for the row, column, and optical density (OD) values of each well.

    Well Selection UI:

        Based on the uploaded data, the app dynamically generates well selection inputs. It allows users to select wells for blank controls, standard solutions, and samples.

    Analysis Process:

        Standard Curve Fitting: When the user clicks "Analyze":

            The code checks that the number of standard wells and concentrations match.

            It calculates the corrected OD values by subtracting the blank well average OD from the standard and sample OD values.

            Then, it fits two models to the data:

                A linear model (linear regression between concentration and OD).

                A logistic model (4PL: four-parameter logistic curve fitting).

        The best-fitting model is chosen based on the R² value (a measure of how well the model fits the data).

    Results and Visualization:

        Plotting:

            The app generates two plots:

                Standard Curve Plot: Shows the standard curve with data points for standards, the fitted model, and predicted concentrations for samples.

                Debugging Plot: A bar plot that shows the corrected OD values for each well, allowing you to visually check the data.

        Results Table: Displays the mean OD, standard deviation, and predicted concentrations for each sample.

        Error Handling: Any errors during the process (e.g., file issues, mismatched concentrations) are displayed in the error output.

    Model Selection:

        The app compares the R² values for the linear model and the logistic model (4PL). It chooses the model with the better fit to the data. If the logistic model fails, the linear model is selected.

    Download Results:

        The app allows the user to download the results, including the standard curve plot and a table of results, as a PDF.
