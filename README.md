# Predicting Rent Prices in Albuquerque Metropolitan Area
## Project Goals
Contracted to analyze apartment metrics to build a model to predict rent prices and apartment occupancy from relevant features. Data will be used to competetively price apartment rent and predict occupancy for profit analysis. Resulted in a model with less than 5% error rate using ensemble machine learning methods.

## Data Cleaning
The first step in the data analysis was to examine the data. There were many missing or incorrect values, so we replaced invalid entires with either the mean or mode, depending on the specific feature. This was done to improve the quality of the dataset, without removing rows from an already sparse data source.

We also remove features such as the apartment complex name, sub market, etc as they do not have any relationship with our predictors.

Here are the statistics for each feature:

| Feature	| Mean | Median | Mode | Minimum | Maximum | Standard Deviation |
| --- | --- | --- | --- | --- | --- | --- |
| Units | 209.2893 | 77 | 240 | 46 | 573 |	114.4006 |
| Bedrooms | 1.6309 | 2 | 2 | 1 | 4 | 0.7115 |
| Size | 854.8595 | 840 | 800 | 109 |1751 | 271.1651 |
| Bathrooms | 1.3795 |	1 |	1 |	1 |	3 | 0.4931 |
| Rent | $601.04 | $715 | $599 | $310 | $2,437 | $152.82 |
| Occupancy | 89.3106% | 100% | 100 | 36% | 100% | 9.0944% |
| Zip Code | 67671.6736 | 87111 | null | 87048 | 87123 | 17132.2395 |
| Age | 1820.5799 | 1965 | null | 2 | 2011 | 196.1572 |


## Data Visualization & Analysis
The data then needed to be visualized to examine given features and their relationship to our two output features- rent and occupancy. The graphs found below start telling the story of our data:

<img src="https://zclarke.dev/Apartment_Rent_Analysis/bedrooms_vs_rent.png" style="width:40%; display:inline-block; text-align:center; margin-left:20vw;"/>

<img src="https://zclarke.dev/Apartment_Rent_Analysis/size_vs_rent.png" style="width:40%; display:inline-block;  text-align:center; margin-left:20vw;"/>

<img src="https://zclarke.dev/Apartment_Rent_Analysis/units_vs_occupancy.png" style="width:40%; display:inline-block;  text-align:center; margin-left:20vw;" />

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

The correlation matrix reflects what we learned from visualizing the data, but also brings up an alarming feature- both the zip code and age of the apartment complex are both highly correlated with rent and occupancy. Further inspection reveals that due to the sparse data, most zip codes and ages are unique, which explains the high correlation. We will remove these features, as it is unlikely they will generalize well to new sample data.

## Model Building and Analysis
The next step in our process is to build a model. We use a simple feedforward network with a small number of hidden layers. Data is normalized to prevent training errors. Initial results were promising, but using a gradient boosting technique and hyperparameter tuning lowered the error of our model. Evaluation on the test dataset revealed the following errors:

| Feature | Mean Percent Absolute Error |
| --- | --- |
| Rent | 4.87%  |
| Occupancy | 5.07% |


## Query Model:
<div id="query-inputs">
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
        
<br>
<button onclick="eval();">Run Query</button>
<br><br /><br />
<b>
<p id="rent-value">Rent: </p>
</b>
<b>
<p id="occupancy-value">Occupancy: </p>
</b>
</div>



<script>
function eval(){
  var res=eval_network([
    Number(document.getElementById("query-Units-value").value)/573,
    Number(document.getElementById("query-#_Bedrooms-value").value)/4,
    Number(document.getElementById("query-Size-value").value)/1751,
    Number(document.getElementById("query-#_Bathrooms-value").value)/3,
    87123/87123,
    2011/2011
  ]);
  document.getElementById("rent-value").innerHTML="Rent: $"+(res[0]*999).toFixed(2)+" +/-$7.36";
  document.getElementById("occupancy-value").innerHTML="Occupancy: "+(res[0]*110).toFixed(2)+"% +/-4.14%";
}


function eval_network(input) {
return [1/(1+1/Math.exp(-1.1167824268341064+0.6776214838027954*(1/(1+1/Math.exp(-0.16549286246299744-0.13924993574619293*(input['0'])+0.2398795783519745*(input['1'])+0.7668423652648926*(input['2'])+0.48436564207077026*(input['3'])-0.14080971479415894*(input['4'])-0.2665686309337616*(input['5']))))+0.9796710014343262*(1/(1+1/Math.exp(-0.3486810028553009-0.11443651467561722*(input['0'])+0.4584387242794037*(input['1'])+0.9023691415786743*(input['2'])+0.600862979888916*(input['3'])-0.10812544822692871*(input['4'])-0.4033713638782501*(input['5']))))+2.587534189224243*(1/(1+1/Math.exp(-0.6857099533081055+0.2552834153175354*(input['0'])+1.1110868453979492*(input['1'])+2.0022220611572266*(input['2'])+1.0924711227416992*(input['3'])-0.7244277596473694*(input['4'])-0.7303528785705566*(input['5'])))))),1/(1+1/Math.exp(1.8970402479171753+0.8099343776702881*(1/(1+1/Math.exp(-0.16549286246299744-0.13924993574619293*(input['0'])+0.2398795783519745*(input['1'])+0.7668423652648926*(input['2'])+0.48436564207077026*(input['3'])-0.14080971479415894*(input['4'])-0.2665686309337616*(input['5']))))+0.7818050384521484*(1/(1+1/Math.exp(-0.3486810028553009-0.11443651467561722*(input['0'])+0.4584387242794037*(input['1'])+0.9023691415786743*(input['2'])+0.600862979888916*(input['3'])-0.10812544822692871*(input['4'])-0.4033713638782501*(input['5']))))+0.4910190999507904*(1/(1+1/Math.exp(-0.6857099533081055+0.2552834153175354*(input['0'])+1.1110868453979492*(input['1'])+2.0022220611572266*(input['2'])+1.0924711227416992*(input['3'])-0.7244277596473694*(input['4'])-0.7303528785705566*(input['5']))))))]
}

eval();
</script>

## Results and Future Work:
The generated model has great accuracy for predicting rent prices, with an acceptable error rate. The main issue with the current model centers around the lack of market data. The model could be improved with more data points and extended to other areas and markets. More data would likely make location information more relevant in the model, which we know to be correlated with price.

It would also be interesting to include a single image of the proeprty in the pipeline, to try and quantify the aesthetic value of the property. This could be trained using the same network with a convolutional neural network used to downsample features of the image into an input vector.
