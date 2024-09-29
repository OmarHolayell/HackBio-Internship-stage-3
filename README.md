# HackBio-Internship-stage-3
# Cholera Data Insights (R Shiny web application)
# Web Application Link: [Cholera Data Insights](https://choleradatainsights.shinyapps.io/solve/)
# Loading required libraries:
library(shinythemes)     # For applying themes to the Shiny app

library(leaflet)         # For creating interactive maps

library(ggplot2)         # For creating charts and plots

library(dplyr)           # For data manipulation

library(readxl)          # For reading Excel files

 library(DT)              # For creating interactive data tables


# Load the dataset
# The Excel file 'cholera - geoT.xlsx' is assumed to be in the app folder. The data is loaded into 'my_data' and converted to a data frame 'my_data_df'.
my_data <- read_excel("cholera - geoT.xlsx")
my_data_df <- as.data.frame(my_data)

# Summarizing the dataset by country
# This section groups the data by 'Countries', and for each country calculates:
# - TotalCases: Sum of cholera cases
# - TotalFatalities: Sum of fatalities
# - Average Longitude and Latitude (for mapping purposes)
 country_summary <- my_data_df %>%
 group_by(Countries) %>%
 summarise(
TotalCases = sum(Cases, na.rm = TRUE),
TotalFatalities = sum(Fatalities, na.rm = TRUE),
Longitude = mean(Longitude, na.rm = TRUE),
Latitude = mean(Latitude, na.rm = TRUE)
)
