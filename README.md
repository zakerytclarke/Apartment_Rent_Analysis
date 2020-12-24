# Predicting Rent Prices in Albuquerque Metropolitan Area
## Project Goals
Contracted to analyze apartment metrics to build a model to predict rent prices and apartment occupancy from relevant features. Data will be used to competetively price apartment rent and predict occupancy for profit analysis. Resulted in a model with less than 1% error rate using ensemble machine learning methods.

## Data Cleaning
The first step in the data analysis was to examine the data. There were many missing or incorrect values, so we replaced invalid entires with either the mean or mode, depending on the specific feature. This was done to improve the quality of the dataset, without removing rows from an already sparse data source.

We also remove features such as the apartment complex name, sub market, etc as they do not have any relationship with our predictors.

## Data Visualization 
The data then needed to be visualized to examine given features and their relationship to our two output features- rent and occupancy. The graphs found below start telling the story of our data:


As you can see, the size of a unit and the number of bedrooms (and bathrooms) are positively correlated with the rent. This intuitively makes sense, as we expect large apartments with more amenities to be more expensive.

Interestingly enough, the number of units in an apartment complex is negatively correlated with occupancy. This is likely due to large apartment complexes having longer occupancy between renters.

We then use a correlation matrix to confirm our visual suspicions:
| Feature | Rent | Occupancy 
| --- | --- | --- |
| Units | 9% |	-22%
| Bedrooms | 40% | 8% |
| Size | 39% | -6% |
| Bathrooms |	34% |	-3% |
| ZIP	| 100% | 100% |
| Age |	100% |	100% |

The correlation matrix reflects what we learned from visualizing the data, but also brings up an alarming feature- both the zip code and age of the apartment complex are both highly correlated with rent and occupancy. Further inspection reveals that due to the sparse data, most zip codes and ages are unique, which explains the high correlation.

## Model Building and Analysis
The next step in our process is to build a model


<script>
  alert('hi');
</script>

## Query Model:

        <label>Units:
        <input id="query-Units-value" type="range" min="46" max="573" step="0.01" value="309.5" onchange="document.getElementById('query-Units').value=this.value;">
        <input type="number" id="query-Units" class="query-input" value="309.5" onchange="document.getElementById('query-Units-value').value=this.value;">
        </label><br>
        
        <label>#_Bedrooms:
        <input id="query-#_Bedrooms-value" type="range" min="1" max="4" step="0.01" value="2.5" onchange="document.getElementById('query-#_Bedrooms').value=this.value;">
        <input type="number" id="query-#_Bedrooms" class="query-input" value="2.5" onchange="document.getElementById('query-#_Bedrooms-value').value=this.value;">
        </label><br>
        
        <label>Size:
        <input id="query-Size-value" type="range" min="109" max="1751" step="0.01" value="930" onchange="document.getElementById('query-Size').value=this.value;">
        <input type="number" id="query-Size" class="query-input" value="930" onchange="document.getElementById('query-Size-value').value=this.value;">
        </label><br>
        
        <label>#_Bathrooms:
        <input id="query-#_Bathrooms-value" type="range" min="1" max="3" step="0.01" value="2" onchange="document.getElementById('query-#_Bathrooms').value=this.value;">
        <input type="number" id="query-#_Bathrooms" class="query-input" value="2" onchange="document.getElementById('query-#_Bathrooms-value').value=this.value;">
        </label><br>
