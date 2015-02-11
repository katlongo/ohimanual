## File System for Assessment Repositories

This section is an orientation to the files within your assessment repository. The file system structure is the same whether you view your assessment repository online or after downloading or cloning to your computer (see Section 5).

Throughout this example, we will use Ecuador’s assessment repository as an example, available at https://github.com/OHI-Science/ecu.


### Assessments and scenarios

Your **assessment repository** contains a **scenario folder**, which by default is named ** *subcountry2014* **. This scenario folder contains all the files needed to calculate scores, and they are described in detail below.

The scenario folder is named *subcountry2014* because it contains data for your country used in the 2014 global assessment. These data in most cases were attributed equally to all regions within your study area (for example, data used for Ecuador in the global assessment was attributed to all coastal states in the files within *subcountry2014*).

You will be able to rename your scenario folder to better reflect the spatial and temporal scale of your scenario after you have set up your GitHub account. We recommend that the name defines the scale of the regions and the year. Eventually, you will likely have multiple scenario folders that contain data for subsequent years or modifications to explore policy alternatives.


![](https://docs.google.com/drawings/d/1eHViTehnAuxSDw1fYI54C3X5YgBktGtaVt71R3OXYeE/pub?w=960&h=720)

In the above figure, **Ecuador (ecu)** is the **assessment repository** and ** *subcountry2014* ** is the **scenario folder**. Note that files with names preceded by a ‘.’ do not appear when not viewing from github.com; this is because these files are specific to GitHub.

![](./fig/ohiglobal_file_location.png)

Within the *subcountry2014* folder area all the inputs required by the Toolbox. Each one is described in detail below.

![](./fig/scenario_folder_overview.png)


### *layers.csv*
`layers.csv` is the registry that manages all data required for the assessment. All relevant data are prepared as a ‘data layer’ and registered in this file. The Toolbox will rely on information from this file to use the data and display information on the WebApp.

![](./fig/layers_csv_registry.png)

When you open `layers.csv`, you’ll see that each row of information represents a specific data layer that has been prepared for the Toolbox. The first columns (*targets, layer, name, description, fld_value, units, filename*) contain information that will be updated by your team as you incorporate your own data and edits; all other columns are generated later by the Toolbox as it confirms data formatting and content. The first columns have the following information:

* **targets** indicates which goal or dimension uses the data layer. Goals are indicated with two-letter codes and sub-goals are indicated with three-letter codes, with pressures, resilience, and spatial layers indicated separately.
 + Food Provision (FP): Fisheries (FIS) and Mariculture (MAR)
  + Artisanal Fishing Opportunity (AO)
  + Natural Products (NP)
  + Coastal Protection (CP)
  + Carbon Storage (CS)
  + Livelihoods and Economies (LE): Livelihoods (LIV) and Economies (ECO)
  + Tourism and Recreation (TR)
  + Sense of Place: Lasting Special Places (LSP) and Iconic Species (ICO)
  + Clean Waters (CW)
  + Biodiversity (BD): Habitats (HAB) and Species (SPP)  

* **layer** is the identifying name of the data layer, which will be used in R scripts like `functions.R` and *.csv* files like `pressures_matrix.csv` and `resilience_matrix.csv`. This is also displayed on the WebApp under the drop-down menu when the variable type is ‘input layer’.
* **name** is a longer title of the data layer; this is displayed on the WebApp under the drop-down menu when the variable type is ‘input layer’.
* **description** is further description of the data layer; this is also displayed on the WebApp under the drop-down menu when the variable type is ‘input layer’.
* **fld_value** indicates the units along with the units column.
* **units** unit of measure in which the data are reported
* **filename** is the *.csv* filename that holds the data layer information, and is located in the folder `subcountry2014/layers`.


### *layers* folder
The `layers` folder contains every data layer as an individual *.csv* file. The names of the *.csv* files within the layers folder correspond to those listed in the *filename* column of the `layers.csv` file described above. All *.csv* files can be read with text editors or with Microsoft Excel or similar software.

![](./fig/layers_folder_location.png)

Note that each *.csv* file within the `layers` folder has a specific format that the Toolbox expects and requires. Comma separated value files (*.csv* files) can be opened with text editor software, or will open by default by Microsoft Excel or similar software.  

Now, open the `layers/alien_species.csv` file: note the unique region identifier (*rgn_id*) with a single associated *score* or *value*, and that the data are presented in ‘long format’ with minimal columns. See the section on *Formatting Data for the Toolbox* for further details and instructions. Scores can be viewed through the App using the ‘Input Layer’ pulldown menu.


### *conf* folder
The `conf` (configuration) folder includes R functions (*config.R* and *functions.R*) and *.csv* files containing information that will be accessed by the R functions (*goals.csv*, *pressures_matrix.R*, *resilience_matrix.csv*, and *resilience_weights.csv*).

![](./fig/layers_folder_location_conf.png)

#### *config.r*
`config.r` is an R script that configures labeling and constants appropriately.

#### *functions.r*
`functions.r` contains functions for each goal and sub-goal model, which calculate the status and trend using data layers identified as ‘layers’ in `layers.csv`. When you modify or develop new goal models, you will modify functions.r.

#### *goals.csv*
`goals.csv` is a list of goals and sub-goals and their weights used to calculate the final score for each goal. Other information includes the goal description that is also presented in the WebApp. `goals.csv` also indicates the arguments passed to `functions.R`. These are indicated by two columns: *preindex_function* (functions for all goals that do not have sub-goals, and functions for all sub-goals) and *postindex_function* (functions for goals with sub-goals).  

#### *pressures_matrix.csv*
`pressures_matrix.csv` defines the different types of ocean pressures and the goals they affect.

Each column in the pressures matrix identifies a data layer that is also registered in `layers.csv`: and has a prefix (for example: `po_` for the pollution category).  The pressure data layers are also required to have a value for every region in the study area, with the region scores ranging from 0-1.

#### *resilience_matrix.csv*
`resilience_matrix.csv` defines the different types of resilience with the goals that they affect.

Like the pressures matrix, the resilience matrix also has weights depending on the level of protection. However, these weights are in a separate file: `resilience_weights.csv`.

Each column in the resilience matrix is a data layer that is also registered in `layers.csv`. Resilience layers, like the pressure layers, are also requried to have a value for every region in the study area. Resilience layers each have a score between 0-1.

#### *resilience_weights.csv*
`resilience_weights.csv` describes the weight of various resilience layers, which in Halpern et al. 2012 (Nature) were determined based on scientific literature and expert opinion.

### spatial folder
The spatial folder contains a single file, *regions_gcs.js*. This is a spatial file in the JSON format; it spatially identifies the study area and regions for the assessment. If you plan to modify your study area or regions, you will need to upload a *.js* file with appropriate offshore boundaries. You will need a GIS analyst to do this: see http://ohi-science.org/pages/create_regions.html for some instruction.

### launch_app_code.R
The App can be launched on your computer so that you can visualize any edits you make while you are offline. To do this, you will run the code in launch_app_code.R. Make sure you are in the *subcountry2014* directory at that time: `setwd(~/github/ecu/subcountry2014)`


### layers-empty_swapping-global-mean.csv
This file contains a list of data layers that were used in the global assessment, but no data were available for your country. Without these data for your country, global averages are included in your `subcountry2014` scenario folder so the Toolbox can calculate scores until you replace these data with appropriate data for your study area. This file is not used anywhere by the Toolbox but is a registry of data layers that should prioritized to be replaced with your own local data.

### *calculate_scores.r*
`calculate_scores.r` is a script that tells the Toolbox to calculate scores using the *.csv* files in the *layers* folder that are registered in *layers.csv* and the configurations identified in *config.r*. Scores will be saved in *scores.csv*.

### scores.csv
`scores.csv` contains the calculated scores for the assessment. Currently, these scores were calculated using data for your country from the global 2014 assessment. Scores are reported for each dimension (future, pressures, resilience, score, status, trend) for each region in the study area (with region identifier), and are presented in ‘long’ format. Scores can be viewed through the App using the ‘Output Score’ pulldown menu. 


### Relaunching the Toolbox
After the initial Toolbox setup, further launches of the Toolbox Application can be done without the software program R. Instead, PC users can double-click the `launchApp.bat` file and Mac users can double-click the `launchApp.command` file.