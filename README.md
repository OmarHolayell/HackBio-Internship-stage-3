### HackBio-Internship-stage-3
## Cholera Data Insights (R Shiny web application)
# Web Application Link: [Cholera Data Insights](https://choleradatainsights.shinyapps.io/solve/)

## Loading required libraries:
```r
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
```
# UI (User Interface) part of the app
ui <- fluidPage(
  theme = shinytheme("cerulean"),   # Use the 'cerulean' theme for a clean blue look
  
  # Header section with title, centered and styled with background color and padding
  tags$div(
    style = "text-align: center; background-color: lightblue; padding: 20px; border-radius: 10px;",
    h2("Cholera Data Insights")     # Display app title
  ),
  
  # Navigation bar with two main tabs: Home and Data
  navbarPage(
    "",
    
    # 'Home' tab contains a sidebar to select a country and a map displaying data
    tabPanel("Home",
             sidebarPanel(
               # Dropdown to select a country or view data for all countries
               selectInput("selectCountry", "Choose a country:", choices = c("All Countries", unique(my_data_df$Countries)))
             ),
             mainPanel(
               leafletOutput("world_map")   # Display a world map with outbreak data
             )
    ),
    
    # 'Data' tab has sub-tabs for visualizing statistics, cases, fatalities, and fatality rate
    tabPanel("Data",
             sidebarPanel(
               # Dropdown to select a country and a year range for data filtering
               selectInput("selectCountryData", "Choose a country:", choices = c("All Countries", unique(my_data_df$Countries))),
               tags$h3("Select Year Range:"),   # Year range selection
               selectInput("selectYearRange", "Choose a year range:", choices = c("All Years", "1949-1959", "1960-1969", "1970-1979", "1980-1989", "1990-1999", "2000-2009", "2010-2016"))
             ),
             mainPanel(
               # Sub-tabs for different data visualizations and a download report button
               tabsetPanel(
                 tabPanel("Statistics", plotOutput("outbreak_plot", height = "250px", width = "90%")),    # Plot for top outbreaks
                 tabPanel("Number of Cases", plotOutput("Number_of_Cases")),                              # Plot for number of cases
                 tabPanel("Number of Fatalities", plotOutput("Number_of_Fatalities")),                    # Plot for number of fatalities
                 tabPanel("Fatality Rate", plotOutput("Fatality_Rate")),                                  # Plot for fatality rate
                 tabPanel("Download Report", downloadButton("download_report", "Download Country Data Report"))  # Button to download report
               )
             )
    )
  )
)


