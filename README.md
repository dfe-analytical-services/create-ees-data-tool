# README
### David Sands
### 2021/03/10

## Contents
1. [Introduction](#tag1)
2. [How to download and install this code](#tag2)
2. [How to run the UI](#tag3)
3. [How to maintain the UI](#tag4)
4. [How does the UI work?](#tag5)
    1. [Summary](#tag6)
    2. [Detailed Explanation](#tag7)
    
# Introduction <a class="anchor" id = "tag1"></a>
    
This project makes a User Interface (UI) for use by teams in Data Insights and Statistics Division (DISD) who publish data on Explore Education Statistics. This user interface will take selections entered by an analyst, and returns data for use in:

 * An Explore Education Statistics Import File (EESIF); 
 * A PowerBI dashboard (PBI); or,
 * Ad-hoc analysis
 
# How to download and install this code <a class="anchor" id = "tag2"></a>

1. On the [Github page](https://github.com/dfe-analytical-services/create-ees-data-tool), you will see a green box near the top called `Code`. Click it, and look underneath the underlined _HTTPS_ text. There is a url there starting with _https://_. Copy that url.
2. Open RStudio on your computer. In the top right of the screen, click on `Project (None)`. In the menu that appears, click on `New Project...`
3. In the next menu, click on the third option `Version Control`. Then `Git`. You will see a menu called _Clone Git Repository_. In the `Repository URL:` cell, paste the url you copied from Github
4. Decide where to save the code using the `Browse..` button, and click `Create Project`. The code will download from Github to the folder location you gave in _Browse..._
5. After the code has downloaded, look in the bottem left _Console_ pane of RStudio where you will see this message:

> ### Bootstrapping renv 0.12.5 --------------------------------------------------
* Downloading renv 0.12.5 from CRAN archive ... OK
* Installing renv 0.12.5 ... Done!
* Successfully installed and loaded renv 0.12.5.
* Project '~/create-ees-data-tool' loaded. [renv 0.12.5]
* The project library is out of sync with the lockfile.
* Use `renv::restore()` to install packages recorded in the lockfile.

6. This means that you have to install R packages to the `renv` folder so that the UI can run. To do that, type `renv::restore` in the console and click enter on your keyboard.
7. You will see this message appear:

> The following package(s) will be updated:
CRAN =========
 - BH              [* -> 1.75.0-0]
 - DBI             [* -> 1.1.1]
  ....
  ....
 Do you want to proceed? [y/N]:

8. Type _y_ and click enter
9. After the code has finished downloading and installing the packages, reopen the `user-interface.Rmd` file and run it. If you don't know how to run it, read the section below.

    
# How to run the UI <a class="anchor" id = "tag3"></a>

To run the UI, follow these steps:

1. Open the R project `ees-user-interface.Rroj`. You need this because I've saved the R Package versions used in the UI within a sub-folder - `renv` - of the project. 
2. **Do not update R Packages after you've opened the R project**.
3. Make sure you're using `R version 4.0.3 (2020-10-10)` or later. I built and tested the UI in this version. If you don't, download it [here.](https://cran.r-project.org/bin/windows/base/) After it downloads, install it by double-clicking on the `R-4.0.3-win` file in your _Downloads_ folder. 
4. Open `user-interface.Rmd` in RStudio.
5. Click on *Run Document* at the top of that file, or press `Ctrl + Shift + K`
6. The UI will either appear in the *Viewer* pane on the bottom right-hand side of RStudio, or as a separate window.
7. Make sure your browser's default is not set to Internet Explorer. The UI will look awful if opened in Internet Explorer.
8. In the *Viewer* pane, click the *Arrow and Box* button to open the UI in your browser. If the UI opened in a separate window, click the *Open in Browser* button. You need to open the UI in your browser to download the data and metadata files. 
9. The UI will appear as a page in your internet browser. In sequential order, select the data you want to return:
    
    1. First, select the _SQL Table (ST)_ you want to return from in the _SQL Table_ drop down menu. 
    
    2. Now pick the years you want data from in the _Years_ menu. 
    
    3. Choose the geographical level of the data you want (e.g. at a National level of all of England, at a Regional Level, at Local Authority level etc) in the _Geographic_ level. You can only pick one. The default is set to National. Once you pick a different one, you need to select that geographic level's name and code as a Filter. E.g. if you select _Regional_, you need to select _region_name_ and _region_code_. 
    
    4. Specify the filters you want returned in the _Filters_ menu. __Always keep ST.year selected.__ The choice of filters will be determined by the ST you choose in `Step 1`. Ones with a `ST` prefix are from the Master Table. Ones with a different prefix (e.g. `dGeo`) come from a table you need to join on in `Step 2`. __If you select a Filter with a different prefix to ST., select the SQL table (with the prefix of the filter you choose) in Step 5 below.__
    
    5. Now select any tables you want to join onto your ST in the _Join Tables_ menu. If you only selected Filters with a `ST.` prefix in Step 4, leave it blank. To determine what Table you need to join, look at the prefixes of all the filters you selected in Step 4. Add the Tables that have this prefix in their Table Join. E.g. if you select `dGeo2.SCD2_region_SFR AS region_name`, look for a Table Join with `dGeo2`. In this case it's `INNER JOIN DWH_PL.DART.DIM_Provider dProv ON ST.Provider_SK = dProv.Provider_SK INNER JOIN DWH_PL.DART.DIM_Geography_V2 dGeo2 WITH (NOLOCK) ON dProv.Provider_PostCode_SK = dGeo2.Geography_SK and dgeo2.Flag_Latest=1`
  
    6. Specify these filters again in the _Group By Filters_ menu. You need to add these again because some variables use alias. Adding variables with an alias to a SQL query is not allowed.
    
    7. Decide on the Indicators you want to return (Starts, Achievements etc) in the _Indicators_ menu. __You need to select at least one.__
    
    8. If you need to add any where statements to the query, place it in the _Where_ text box. Start all your where statements with `AND`, followed by the statement
    
    9. Determine the Filters and Indicators you want sub-totals under the _Totals_ menu. __You need to select at least one filter AND indicator__ in each menu. __Always keep year selected as a filter to sub-total on.__
    
    10. If you want columns showing the percentage of each indicator you choose, click the _Percentages?_ button. If you selected the _Year on Year Percentage Changes?_ button below, clicking this one will unselect it.
    
    11. If, instead, you want to show the year-on-year percentage change for the indicators you choose, click the _Year on Year Percentage Changes?_ button. Selecting this will unselect the _Percentages?_ button if it were selected.
    
    12. Clarify the Rounding you want performed on the Tidy data by selecting it in the _Rounding?_ menu. The default is set to None. 

 10. Once you've selected all those parameters, click `Run your query`. A progress bar will appear in the bottom right of the screen. Wait until it finishes.
 11. When you clicked `Run your query`, your selections were passed to a SQL query, which started to run.  
 12. Once the progress bar is finished, `Tidy Data` will appear in the _Explore your data_ pane. Explore it here. 
 13. This data can be downloaded as a csv in the _Download your tidy data_ pane.
 14. The Metadata file you need for EES can be downloaded in the _Download your Metadata File_ pane. The names, column types, and labels for the filters and labels you select will automatically generate. But, the _indicator_grouping, indicator_dp, filter_hint_, and _filter_grouping_column_ columns will not be generated. _If they need completed, you need to fill them in yourself._ For more information about what should go in these columns, open a browser on the DfE network and go to [the Explore Education Statistics guidance.](https://rsconnect/rsc/stats-production-guidance/ud.html#mandatory_ees_metadata_columns)
 15. The raw data - `Non-Tidy Data` - the SQL query returns can be downloaded as a csv in the _Download your non-tidy data_ pane. This can be used for PowerBI dashboards and underlying datasets
 16. The raw query that returns this data can be copied and pasted in the _View your query_ pane.
 17. Whilst this same query can be downloaded as a text file in the _Download your query_ pane.
 
# How to maintain the UI <a class="anchor" id = "tag4"></a>

Update the UI by checking the below requirements:

  1. Updating the `inputs\filters-and-indicators.xlsx` file with the latest filters and indicators from your SQL Tables. I envisage this taking the most time and effort to maintain
  2. If the SQL Tables move location on the SQL server, updating the SQL connection to refer to the new location. This is found in the `{r setup, include=FALSE}` code chunk of the UI R Markdown
  3. Adding the latest academic years to the Year selections. These are found in the `### Year` section of the UI R Markdown.
  
# How does the UI work? <a class="anchor" id = "tag5"></a>

## Summary <a class="anchor" id = "tag6"></a>

The UI imports information from the `inputs\filters-and-indicators.xlsx` Excel file containing the names of SQL tables, as well as filters & indicators from those tables. It sends that information to various drop-down menus which the user selects from. These selections populate a SQL query. When the user is finished and clicks a `Run` button, this SQL query is run, returning what the UI calls _non-tidy data_. Additional operations are applied to this non-tidy data to make it tidy - like sub-totaling all filter groups and adding the extra four columns required by EES. 

At the end, the UI returns _tidy data_ for use in EES, _non-tidy data_ for PBI dashboards, underlying files and ad-hoc queries, and a corresponding _meta data file_ for EES.  
  
## Detailed Explanation <a class="anchor" id = "tag7"></a>

This UI is a Flexdashboard - an R Markdown dashboard - written in the _user-interface.Rmd_ file. It connects to the `inputs\filters-and-indicators.xlsx` Excel file and a SQL server you specify, asks the user for selections, sends those selections to a SQL query, runs it, manipulates the returned data, and outputs both tidy and non-tidy data. 

### Inputs

  1.  `filters <- readxl::read_xlsx("inputs/filters-and-indicators.xlsx", sheet = "filters")` - A sheet of the `inputs\filters-and-indicators.xlsx` Excel file with the info on the filters in each SQL Table (ST)
  2. `indicators <- readxl::read_xlsx("inputs/filters-and-indicators.xlsx", sheet = "indicators")` - Another sheet of the `inputs\filters-and-indicators.xlsx` Excel file with the info on the indicators in each ST
  3. `conn1 <- DBI::dbConnect(odbc(), .connection_string = 'driver={SQL Server};  server=<name-of-server>;database=<name-of-database>; trusted_connection=true')` - A connection to the SQL server that contains the ST. You need to insert your server and database names in here.
  
### Process

With information from the Excel imported and a connection created to SQL, the UI starts working. 

The `### Selection Filters` section takes the information from the Excel and presents it in `selectInput ` boxes - Flexdashboard term for a drop-down menu that appears when this file is Knitted. The values that are selected in these selectInputs are assigned to them using the form `input$<name-of-selectInput>`. We use these values as input for the code. 

The _year_ selectInput will need updating over the years. Some of these selectInputs update based on a selection in a previous one. E.g. _filter_. This uses an `observeEvent` function to pull the value from the _master_table_ selection, and changes the info _filter_ shows by an `updateSelectInput` function. This allows only filters and indicators from specific ST to be shown.

Near the end of this section a `checkboxInput` function appears. This is a tick-box. A user would click it if they want percentages added to the data that's returned. 

At the end, an `actionButton` appears called _run_. This feeds into `eventReactive` functions in the `Process` section to run the SQL query and data manipulation code. 

In this `Process` section, we start with the R chunk _Set input values_. This starts creating variables of the users selections via `renderText` functions - which take values stored in the inputs of `### Selection Filters` and converts them to text. Others deploy `reactive(as.vector())` functions. These convert input values to vector - they're needed as later data manipulation operations only worked on data as vectors. 

In _var_label_vector_, we use [Regular Expressions](https://en.wikipedia.org/wiki/Regular_expression) to detect instances of text that match geographical areas, and then exclude them from the selection. This is done to stop sub-totaling on geographical levels. 

At the bottom of this chunk, we have two functions. _prop_round_ is a modified round function that takes a value of 5 and rounds it up to 10, rather than R's default of taking a 5 and rounding down to 0. It's lifted from Lifted from Isabel Cottam's code from the Destination Measures team. _round_value_ is a function that returns a number based on what level of rounding a user choose in the inputs section. It's used as input to the _prop_round_ function.

Moving onto the _Create non-tidy data_ chunk; this returns raw unrounded from the ST. When the _run_ action button is pressed by a user, the _non_tidy_data_ function is activated. This uses if statements to see if a user has selected a final year, an in-year, or both selections from the inputs, and runs a SQL query populated with the other inputs a user choose. 

The _Create tidy data_ chunk is the most complicated one of the entire UI. It comprises of several data manipulation steps in one function - _tidy_data_ - that's activated when the _run_ button is pressed. 

  1. First, a progress bar appears that runs for 15 seconds, and then stays at 99% if a query takes longer than 15 seconds. 
  2. The _Create non-tidy data_ code is repeated to generate raw unrounded data to manipulate
  3. It sees how many indicators a user selected. This is used in later code to apply manipulations to, and only to, the indicator columns, as well as select only filter columns from the data. 
  4. It then checks if a user has selected percentages to be calculated or not. If yes, it detects if any rows have 'unknown' or 'Unknown' values in them in the filter columns, and removes these rows from the percentage calculations. It then calculates percentage values on the remaining data. 
  5. It cubes the data by sub-totaling each filter against the indicators. Any rows that have geographic sub-totals (like sub-totals for Regions, Local Authorities, PCONs etc) are removed as EES doesn't like sub-totals for geo levels. 
  6. It adds 4 additional columns required by EES: time_identifier, geographic_level, country_code, and country_name. 
  7. Then it renames any NAs generated by the cube's sub-totaling to 'Total'
  8. Runs an if statement to see if a user selected the rounding level _Sub-totals to the nearest ten, and totals to the nearest hundred_. If they have, it calls the _prop_round_ function to round the indicators so rows with 'Total' in them are rounded to the nearest 100, while all other rows are rounded to the nearest 10. If not, all indicator values are rounded to either the nearest 10, or nearest 100. 
  9. Any indicator values that are rounded down to zero get replaced by ~
  10. And finally returns the data.
  
### Outputs

The next few chunks return the various outputs of the UI. _Return the tidy data as a DataTable_ does what it says: it returns the data in a DataTable, a feature of R allowing interactive and sort-able tables in an HTML or Flexdashboard report. This is rendered in the UI by the `DT::dataTableOutput` function.

_Download your tidy data_ returns the tidy data you generated in a CSV file called `<current-data-YYYY-MM-DD>_tidy-data.csv`

_Download your Metadata File_ returns the metadata file for use alongside the tidy data as a CSV file. 

_Download your non-tidy data_ returns the non-tidy data you generated in a CSV file called `<current-data-YYYY-MM-DD>_non-tidy-data.csv`

_View your query_ shows the SQL query that the UI runs from a `renderPrint` function. 

And finally, _Download your query_ allows you to download the same query as a text file called `<current-data-YYYY-MM-DD>_query.txt`
