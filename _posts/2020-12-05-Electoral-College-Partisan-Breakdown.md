---
layout: post
title: Quote Me Dev Diary Part 2: Webscraping
---
## Who Does the Electoral College Disenfranchise?

With the election behind us once again the electoral college, that forever most silly feature of American presidential elections is bubbling up as a part of the conversation. Typically when the topic comes up it usually pits Democrats against Republicans, with Democrats favoring an overturn of the system and with the Republicans favoring the system in place. While it's pretty apparent that the electoral college does disadvantage Democrats who are primarily located in more urban states. I was curious to see if it in fact also disenfranchises Republicans as well. The strongest argument that I hear in favor for the electoral college is that it gives a bump to more rural populations who would otherwise never have their concerns met. And it's typically Republican voices that bring up this concern. Which I don't fault them for advocating for their consituents. But I'm not sure if addressing that concern on a state level really makes much sense. While our typical idea of a rural voter might be someone living in Kansas or other midwestern states. I think we underestimate how rural/Republican huge swaths of even very populous states can be. So I decided to pull some state level data on the population, number of registered voters, and their partisan composition and see if there is a difference in partisan composition in more populous states compared to the average composition. 

I wrote all of this in a jupyter notebook and you can pull this notebook which contains my code and the associated data [here](https://github.com/coreyclip/coreyclip.github.io/tree/master/jupyternotebooks)

#### Sources for the data 
* partisan breakdown of states where available: 
https://en.wikipedia.org/wiki/Political_party_strength_in_U.S._states

* registered voters by state: 
https://worldpopulationreview.com/state-rankings/number-of-registered-voters-by-state
worldpopulationreview further posts sources from invidual state agencies handling the number of registered voters by state

## Data Overview

Here's the top 5 rows of the dataset. I calculated out the number of registered Democrats, Republicans, and Independents per state myself from the original datasets out of the total number of registered voters in each state. 

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>totalRegistered</th>
      <th>Pop</th>
      <th>registeredPerc</th>
      <th>asOf</th>
      <th>DemPercentage</th>
      <th>RepubPercentage</th>
      <th>IndPercentage</th>
      <th>Democrats</th>
      <th>Republicans</th>
      <th>Independents</th>
    </tr>
    <tr>
      <th>State</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Alabama</th>
      <td>3708804</td>
      <td>4908620</td>
      <td>0.7556</td>
      <td>11/4/2020</td>
      <td>0.35</td>
      <td>0.52</td>
      <td>0.13</td>
      <td>1298081.0</td>
      <td>1928578.0</td>
      <td>482145.0</td>
    </tr>
    <tr>
      <th>Alaska</th>
      <td>597319</td>
      <td>734002</td>
      <td>0.8138</td>
      <td>11/3/2020</td>
      <td>0.13</td>
      <td>0.24</td>
      <td>0.63</td>
      <td>77651.0</td>
      <td>143357.0</td>
      <td>376311.0</td>
    </tr>
    <tr>
      <th>Arizona</th>
      <td>4281152</td>
      <td>7378490</td>
      <td>0.5802</td>
      <td>11/4/2020</td>
      <td>0.33</td>
      <td>0.35</td>
      <td>0.32</td>
      <td>1412780.0</td>
      <td>1498403.0</td>
      <td>1369969.0</td>
    </tr>
    <tr>
      <th>Arkansas</th>
      <td>1755775</td>
      <td>3039000</td>
      <td>0.5777</td>
      <td>6/3/2020</td>
      <td>0.35</td>
      <td>0.48</td>
      <td>0.17</td>
      <td>614521.0</td>
      <td>842772.0</td>
      <td>298482.0</td>
    </tr>
    <tr>
      <th>California</th>
      <td>22047448</td>
      <td>39937500</td>
      <td>0.5520</td>
      <td>10/19/2020</td>
      <td>0.45</td>
      <td>0.24</td>
      <td>0.31</td>
      <td>9921352.0</td>
      <td>5291388.0</td>
      <td>6834709.0</td>
    </tr>
  </tbody>
</table>
</div>



## Top 10 States By Number of Registered Voters 

First I opted to look at our dataset sorted by the number of registered Republicans. We can see that the up until recently solidly red state Texas contains the most Republicans but in second comes a "solidly" blue state California. What is worth noting is that three states traditionally thought of as swing states: Ohio, Florida, and Pennsylvania do come up the next three states with the most Republican voters. 

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>totalRegistered</th>
      <th>Pop</th>
      <th>registeredPerc</th>
      <th>asOf</th>
      <th>DemPercentage</th>
      <th>RepubPercentage</th>
      <th>IndPercentage</th>
      <th>Democrats</th>
      <th>Republicans</th>
      <th>Independents</th>
    </tr>
    <tr>
      <th>State</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Texas</th>
      <td>16211198</td>
      <td>29472300</td>
      <td>0.5500</td>
      <td>3/1/2020</td>
      <td>0.39</td>
      <td>0.42</td>
      <td>0.19</td>
      <td>6322367.0</td>
      <td>6808703.0</td>
      <td>3080128.0</td>
    </tr>
    <tr>
      <th>California</th>
      <td>22047448</td>
      <td>39937500</td>
      <td>0.5520</td>
      <td>10/19/2020</td>
      <td>0.45</td>
      <td>0.24</td>
      <td>0.31</td>
      <td>9921352.0</td>
      <td>5291388.0</td>
      <td>6834709.0</td>
    </tr>
    <tr>
      <th>Florida</th>
      <td>14065627</td>
      <td>21993000</td>
      <td>0.6396</td>
      <td>8/31/2020</td>
      <td>0.37</td>
      <td>0.35</td>
      <td>0.28</td>
      <td>5204282.0</td>
      <td>4922969.0</td>
      <td>3938376.0</td>
    </tr>
    <tr>
      <th>Ohio</th>
      <td>7774767</td>
      <td>11747700</td>
      <td>0.6618</td>
      <td>3/17/2020</td>
      <td>0.41</td>
      <td>0.45</td>
      <td>0.14</td>
      <td>3187654.0</td>
      <td>3498645.0</td>
      <td>1088467.0</td>
    </tr>
    <tr>
      <th>Pennsylvania</th>
      <td>9091371</td>
      <td>12820900</td>
      <td>0.7091</td>
      <td>11/2/2020</td>
      <td>0.48</td>
      <td>0.38</td>
      <td>0.14</td>
      <td>4363858.0</td>
      <td>3454721.0</td>
      <td>1272792.0</td>
    </tr>
    <tr>
      <th>Michigan</th>
      <td>8127040</td>
      <td>10045000</td>
      <td>0.8091</td>
      <td>11/3/2020</td>
      <td>0.45</td>
      <td>0.39</td>
      <td>0.16</td>
      <td>3657168.0</td>
      <td>3169546.0</td>
      <td>1300326.0</td>
    </tr>
    <tr>
      <th>Georgia</th>
      <td>7233584</td>
      <td>10736100</td>
      <td>0.6738</td>
      <td>11/1/2020</td>
      <td>0.43</td>
      <td>0.42</td>
      <td>0.15</td>
      <td>3110441.0</td>
      <td>3038105.0</td>
      <td>1085038.0</td>
    </tr>
    <tr>
      <th>New York</th>
      <td>13555547</td>
      <td>19440500</td>
      <td>0.6973</td>
      <td>11/1/2020</td>
      <td>0.51</td>
      <td>0.22</td>
      <td>0.27</td>
      <td>6913329.0</td>
      <td>2982220.0</td>
      <td>3659998.0</td>
    </tr>
    <tr>
      <th>Illinois</th>
      <td>8036534</td>
      <td>12659700</td>
      <td>0.6348</td>
      <td>3/17/2020</td>
      <td>0.50</td>
      <td>0.34</td>
      <td>0.16</td>
      <td>4018267.0</td>
      <td>2732422.0</td>
      <td>1285845.0</td>
    </tr>
    <tr>
      <th>Virginia</th>
      <td>5975696</td>
      <td>8626210</td>
      <td>0.6927</td>
      <td>11/1/2020</td>
      <td>0.46</td>
      <td>0.39</td>
      <td>0.15</td>
      <td>2748820.0</td>
      <td>2330521.0</td>
      <td>896354.0</td>
    </tr>
  </tbody>
</table>
</div>



## Top 10 States By Number of Registered Democratic Voters 

Looking at the states with the most Democrat voters. We can see that we basically have the same states but swap out Ohio for New York. 

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>State</th>
      <th>totalRegistered</th>
      <th>Pop</th>
      <th>registeredPerc</th>
      <th>asOf</th>
      <th>DemPercentage</th>
      <th>RepubPercentage</th>
      <th>IndPercentage</th>
      <th>Democrats</th>
      <th>Republicans</th>
      <th>Independents</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>California</td>
      <td>22047448</td>
      <td>39937500</td>
      <td>0.5520</td>
      <td>10/19/2020</td>
      <td>0.45</td>
      <td>0.24</td>
      <td>0.31</td>
      <td>9921352.0</td>
      <td>5291388.0</td>
      <td>6834709.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>New York</td>
      <td>13555547</td>
      <td>19440500</td>
      <td>0.6973</td>
      <td>11/1/2020</td>
      <td>0.51</td>
      <td>0.22</td>
      <td>0.27</td>
      <td>6913329.0</td>
      <td>2982220.0</td>
      <td>3659998.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Texas</td>
      <td>16211198</td>
      <td>29472300</td>
      <td>0.5500</td>
      <td>3/1/2020</td>
      <td>0.39</td>
      <td>0.42</td>
      <td>0.19</td>
      <td>6322367.0</td>
      <td>6808703.0</td>
      <td>3080128.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Florida</td>
      <td>14065627</td>
      <td>21993000</td>
      <td>0.6396</td>
      <td>8/31/2020</td>
      <td>0.37</td>
      <td>0.35</td>
      <td>0.28</td>
      <td>5204282.0</td>
      <td>4922969.0</td>
      <td>3938376.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Pennsylvania</td>
      <td>9091371</td>
      <td>12820900</td>
      <td>0.7091</td>
      <td>11/2/2020</td>
      <td>0.48</td>
      <td>0.38</td>
      <td>0.14</td>
      <td>4363858.0</td>
      <td>3454721.0</td>
      <td>1272792.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Illinois</td>
      <td>8036534</td>
      <td>12659700</td>
      <td>0.6348</td>
      <td>3/17/2020</td>
      <td>0.50</td>
      <td>0.34</td>
      <td>0.16</td>
      <td>4018267.0</td>
      <td>2732422.0</td>
      <td>1285845.0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Michigan</td>
      <td>8127040</td>
      <td>10045000</td>
      <td>0.8091</td>
      <td>11/3/2020</td>
      <td>0.45</td>
      <td>0.39</td>
      <td>0.16</td>
      <td>3657168.0</td>
      <td>3169546.0</td>
      <td>1300326.0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Ohio</td>
      <td>7774767</td>
      <td>11747700</td>
      <td>0.6618</td>
      <td>3/17/2020</td>
      <td>0.41</td>
      <td>0.45</td>
      <td>0.14</td>
      <td>3187654.0</td>
      <td>3498645.0</td>
      <td>1088467.0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Georgia</td>
      <td>7233584</td>
      <td>10736100</td>
      <td>0.6738</td>
      <td>11/1/2020</td>
      <td>0.43</td>
      <td>0.42</td>
      <td>0.15</td>
      <td>3110441.0</td>
      <td>3038105.0</td>
      <td>1085038.0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Virginia</td>
      <td>5975696</td>
      <td>8626210</td>
      <td>0.6927</td>
      <td>11/1/2020</td>
      <td>0.46</td>
      <td>0.39</td>
      <td>0.15</td>
      <td>2748820.0</td>
      <td>2330521.0</td>
      <td>896354.0</td>
    </tr>
  </tbody>
</table>
</div>



## Top 10 States By Number of Independent Voters 

Independents are gathered in mostly the same states as Republicans and Democrats, Though check out Massachusetts! One thing worth noting that certain states through a combination of voting laws and partisan party rules end up depressing political party membership, amongst other factors. Since this post is just focusing on partisan politics I'm not really going to try and account for Independents too much it's an area for further investigation on the topic I'm covering. 



<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>State</th>
      <th>totalRegistered</th>
      <th>Pop</th>
      <th>registeredPerc</th>
      <th>asOf</th>
      <th>DemPercentage</th>
      <th>RepubPercentage</th>
      <th>IndPercentage</th>
      <th>Democrats</th>
      <th>Republicans</th>
      <th>Independents</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>California</td>
      <td>22047448</td>
      <td>39937500</td>
      <td>0.5520</td>
      <td>10/19/2020</td>
      <td>0.45</td>
      <td>0.24</td>
      <td>0.31</td>
      <td>9921352.0</td>
      <td>5291388.0</td>
      <td>6834709.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Florida</td>
      <td>14065627</td>
      <td>21993000</td>
      <td>0.6396</td>
      <td>8/31/2020</td>
      <td>0.37</td>
      <td>0.35</td>
      <td>0.28</td>
      <td>5204282.0</td>
      <td>4922969.0</td>
      <td>3938376.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>New York</td>
      <td>13555547</td>
      <td>19440500</td>
      <td>0.6973</td>
      <td>11/1/2020</td>
      <td>0.51</td>
      <td>0.22</td>
      <td>0.27</td>
      <td>6913329.0</td>
      <td>2982220.0</td>
      <td>3659998.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Texas</td>
      <td>16211198</td>
      <td>29472300</td>
      <td>0.5500</td>
      <td>3/1/2020</td>
      <td>0.39</td>
      <td>0.42</td>
      <td>0.19</td>
      <td>6322367.0</td>
      <td>6808703.0</td>
      <td>3080128.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Massachusetts</td>
      <td>4812909</td>
      <td>6976600</td>
      <td>0.6899</td>
      <td>10/24/2020</td>
      <td>0.33</td>
      <td>0.10</td>
      <td>0.57</td>
      <td>1588260.0</td>
      <td>481291.0</td>
      <td>2743358.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>New Jersey</td>
      <td>6486299</td>
      <td>8936570</td>
      <td>0.7258</td>
      <td>11/2/2020</td>
      <td>0.38</td>
      <td>0.22</td>
      <td>0.40</td>
      <td>2464794.0</td>
      <td>1426986.0</td>
      <td>2594520.0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>North Carolina</td>
      <td>7361219</td>
      <td>10611900</td>
      <td>0.6937</td>
      <td>11/3/2020</td>
      <td>0.36</td>
      <td>0.30</td>
      <td>0.34</td>
      <td>2650039.0</td>
      <td>2208366.0</td>
      <td>2502814.0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Colorado</td>
      <td>4238513</td>
      <td>5845530</td>
      <td>0.7251</td>
      <td>11/1/2020</td>
      <td>0.30</td>
      <td>0.28</td>
      <td>0.42</td>
      <td>1271554.0</td>
      <td>1186784.0</td>
      <td>1780175.0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Arizona</td>
      <td>4281152</td>
      <td>7378490</td>
      <td>0.5802</td>
      <td>11/4/2020</td>
      <td>0.33</td>
      <td>0.35</td>
      <td>0.32</td>
      <td>1412780.0</td>
      <td>1498403.0</td>
      <td>1369969.0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Michigan</td>
      <td>8127040</td>
      <td>10045000</td>
      <td>0.8091</td>
      <td>11/3/2020</td>
      <td>0.45</td>
      <td>0.39</td>
      <td>0.16</td>
      <td>3657168.0</td>
      <td>3169546.0</td>
      <td>1300326.0</td>
    </tr>
  </tbody>
</table>
</div>



## Top 10 States By Number of Registered Voters 

Looking at the five most populous states in the US we can see that this is basically the same set of states with the most democratic voters and includes most of the states that have the most Republican and Independents. Of these really only Florida, Pennsylvania, and (Ohio or Georgia depending on your memory) are swing states. 

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>totalRegistered</th>
      <th>Pop</th>
      <th>registeredPerc</th>
      <th>asOf</th>
      <th>DemPercentage</th>
      <th>RepubPercentage</th>
      <th>IndPercentage</th>
      <th>Democrats</th>
      <th>Republicans</th>
      <th>Independents</th>
    </tr>
    <tr>
      <th>State</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>California</th>
      <td>22047448</td>
      <td>39937500</td>
      <td>0.5520</td>
      <td>10/19/2020</td>
      <td>0.45</td>
      <td>0.24</td>
      <td>0.31</td>
      <td>9921352.0</td>
      <td>5291388.0</td>
      <td>6834709.0</td>
    </tr>
    <tr>
      <th>Texas</th>
      <td>16211198</td>
      <td>29472300</td>
      <td>0.5500</td>
      <td>3/1/2020</td>
      <td>0.39</td>
      <td>0.42</td>
      <td>0.19</td>
      <td>6322367.0</td>
      <td>6808703.0</td>
      <td>3080128.0</td>
    </tr>
    <tr>
      <th>Florida</th>
      <td>14065627</td>
      <td>21993000</td>
      <td>0.6396</td>
      <td>8/31/2020</td>
      <td>0.37</td>
      <td>0.35</td>
      <td>0.28</td>
      <td>5204282.0</td>
      <td>4922969.0</td>
      <td>3938376.0</td>
    </tr>
    <tr>
      <th>New York</th>
      <td>13555547</td>
      <td>19440500</td>
      <td>0.6973</td>
      <td>11/1/2020</td>
      <td>0.51</td>
      <td>0.22</td>
      <td>0.27</td>
      <td>6913329.0</td>
      <td>2982220.0</td>
      <td>3659998.0</td>
    </tr>
    <tr>
      <th>Pennsylvania</th>
      <td>9091371</td>
      <td>12820900</td>
      <td>0.7091</td>
      <td>11/2/2020</td>
      <td>0.48</td>
      <td>0.38</td>
      <td>0.14</td>
      <td>4363858.0</td>
      <td>3454721.0</td>
      <td>1272792.0</td>
    </tr>
    <tr>
      <th>Michigan</th>
      <td>8127040</td>
      <td>10045000</td>
      <td>0.8091</td>
      <td>11/3/2020</td>
      <td>0.45</td>
      <td>0.39</td>
      <td>0.16</td>
      <td>3657168.0</td>
      <td>3169546.0</td>
      <td>1300326.0</td>
    </tr>
    <tr>
      <th>Illinois</th>
      <td>8036534</td>
      <td>12659700</td>
      <td>0.6348</td>
      <td>3/17/2020</td>
      <td>0.50</td>
      <td>0.34</td>
      <td>0.16</td>
      <td>4018267.0</td>
      <td>2732422.0</td>
      <td>1285845.0</td>
    </tr>
    <tr>
      <th>Ohio</th>
      <td>7774767</td>
      <td>11747700</td>
      <td>0.6618</td>
      <td>3/17/2020</td>
      <td>0.41</td>
      <td>0.45</td>
      <td>0.14</td>
      <td>3187654.0</td>
      <td>3498645.0</td>
      <td>1088467.0</td>
    </tr>
    <tr>
      <th>North Carolina</th>
      <td>7361219</td>
      <td>10611900</td>
      <td>0.6937</td>
      <td>11/3/2020</td>
      <td>0.36</td>
      <td>0.30</td>
      <td>0.34</td>
      <td>2650039.0</td>
      <td>2208366.0</td>
      <td>2502814.0</td>
    </tr>
    <tr>
      <th>Georgia</th>
      <td>7233584</td>
      <td>10736100</td>
      <td>0.6738</td>
      <td>11/1/2020</td>
      <td>0.43</td>
      <td>0.42</td>
      <td>0.15</td>
      <td>3110441.0</td>
      <td>3038105.0</td>
      <td>1085038.0</td>
    </tr>
  </tbody>
</table>
</div>



# The Electoral College disenfranchises people regardless of political affiliation

By looking at the partisan composition of the states with the most democrats or Republicans we can see that essentially by doing away with the electoral college and moving to a straight popular vote, it wouldn't mean that presidential candidates would be ignoring one political party or the other by campaigning in states where most voters live. A candidate looking to reach out to Republican voters or Democratic voters would do well to focus on Texas, California, and New York as these are plain where the bulk of either live. And by doing so they wouldn't be really disregarding the partisan composition of the nation as a whole. The partisan composition of the most democratic and republicans states doesn't really differ too much from the nationwide average, especially once we take into account that the states with the most democrats or republicans are for the most part the same.  

One thought I had looking at this data given our rather hideous political climate of today is how much potential do we waste with our undemocratic and anti-republican electoral college system? In effect the majority of democrats and republicans are basically taken as given in the actual election and instead our presidential politics is filtered through the lens of what matters to the voters in a couple of states. Unless your rather civically engaged this is most of the politics that really catches any of your attention. If instead we moved to popular vote the diversity of each political party would have to be taken into account by any candidate. We'd probably see more ideas out of the left and the right. This is purely annecdotal but coming from a resident of California, strong satisfaction with the priorities of either political party's candidate is very much the exception rather than the norm. Most feel dragged along with what has been given, and we loose out as a nation by not having their views informing our presidential campaigns. 


```python
limit = 10

tdf = pd.DataFrame(index=[f'Top {limit} States with the most Democrats', f'Top {limit} States with the most Republicans', 'Nationwide Average'], data=[],
             columns=['% Democrat', '% Republican', '% of Total Registered Voters'])
tdf['% Republican'] = [df.sort_values('Democrats', ascending=False).head(limit)['RepubPercentage'].mean().round(3) * 100,
                       df.sort_values('Republicans', ascending=False).head(limit)['RepubPercentage'].mean().round(3) * 100,
                       df['RepubPercentage'].mean().round(3) * 100
                      ]

tdf['% Democrat'] = [  df.sort_values('Democrats', ascending=False).head(limit)['DemPercentage'].mean().round(3) * 100,
                       df.sort_values('Democrats', ascending=False).head(limit)['RepubPercentage'].mean().round(3) * 100,
                       df['DemPercentage'].mean().round(2) * 100
                      ]
tdf['% of Total Registered Voters'] = [(df.sort_values('Democrats', ascending=False).head(limit)['totalRegistered'].sum() / df['totalRegistered'].sum() * 100).round(3),
                         (df.sort_values('Republicans', ascending=False).head(limit)['totalRegistered'].sum() / df['totalRegistered'].sum() * 100).round(3),
                         np.nan]

tdf
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>% Democrat</th>
      <th>% Republican</th>
      <th>% of Total Registered Voters</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Top 10 States with the most Democrats</th>
      <td>44.5</td>
      <td>36.0</td>
      <td>52.441</td>
    </tr>
    <tr>
      <th>Top 10 States with the most Republicans</th>
      <td>36.0</td>
      <td>36.0</td>
      <td>52.441</td>
    </tr>
    <tr>
      <th>Nationwide Average</th>
      <td>38.0</td>
      <td>37.1</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>

#### Graphs 
<p float="left">
    <img src="/images/most_democrats.png" width="100" />
    <img src="/images/most_republicans.png" width="100" />
    <img src="/images/most_independents.png" width="100" />
</p>



