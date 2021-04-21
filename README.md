# NYC Housing Sales Visualization

I used Jupyter notebook for this project. Run the `ipynb` file locally or open the `html` file to see the interactive map. \
To install Jupyter notebook, one can do so with [conda or pip](https://jupyter.org/install), but I found it neat to use the package management tool [Anaconda](https://docs.anaconda.com/anaconda/install/).


## Overview
This is mainly a data cleaning and data visualization project. I combined and cleaned data from 4 different sources to plot an interactive map of New York City showing the housing sales prices. 

The goal is to use appropriate keys to map the housing sales data to the `geojson` map. The key used in the `geojson` file is the NYC NTA (Neighborhood Tabulation Area) code, or NTA name. Here is how I mapped the data:
- Construct bbl (borough-block-lot) number, which uniquely identifiesthe location of a property in NYC, using the `Borough` and `Block` coloumns in the housing sales dataset. Note that I don't include the lot number, because properties in the same block would belong to the same NTA, so we don't need this precision. For example, a property in Borough **1** and Block **377** has a borough-block number of **100377**. Unfortunately, a conversion between bbl numbers and NTA was not available. What was available were the conversion between bbl and 2010 census tract, and the conversion between 2010 census tract and NTA. This was how I mapped the data, as further explained below.
- Map borough-block number to (borough code, 2010 census tract), because the census tract is not unique throughout different boroughs in NYC. The census tract is always 6 digits in the next dataset, so the census tract here needed some cleaning to ensure consistent formatting. For example, **838** becomes **083800**, **1502** becomes **150200**, **798.02** becomes **079802**, and so on.
- Map (borough code, 2010 census tract) to NTA code.

I dealt with some duplicate entries. There might be multiple properties sold on the same day, and the sale price listed would be the total price for all properties sold, rather than the individual price. Thus, I replaced these prices with the average sale price. That is, if there are **4** duplicated entries, the sale prices are divided by **4**. I also remove the NTAs if there were too few sales, making the median not reliable.

Finally, I visualized the median sale prices of each NTA using a heatmap on a NYC map. This map is interactive. One can use the cursor to get more information (NTA name, NTA code, median sale price) of a particular NTA.

## Takeaways
Here is a sample of the visualization.
![Alt text](sample_map.png?raw=true "Title")

We see that the most expensive neighborhoods concentrate in Manhattan and then Brooklyn. 
The three most expensive neighborhoods in Manhattan are: 
- SoHo-TriBeCa-Civic Center-Little Italy (**2.7 m**)
- Midtown-Midtown South (**1.9 m**)
- Lincoln Square (**1.9 m**) 
 
The three most expensive neighborhoods in Brooklyn are:  
- Carroll Gardens-Columbia Street-Red Hook (**1.5 m**)
- Prospect Heights (**1.2 m**)
- Brooklyn Heights-Cobble Hill (**1.2 m**)
