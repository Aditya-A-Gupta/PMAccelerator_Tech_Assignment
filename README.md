# <a name="_1yok37gskr5m"></a> **Weather Forecasting Using Time Series Models**
-----
## <a name="_abe2pssfxkek"></a>**Introduction:**
1. ### <a name="_vlr1qqc7zgek"></a>Project Overview
Climate change and weather variability have made **temperature forecasting** a crucial field of study. This project aims to **analyze global weather data** and develop a reliable forecasting model using **time-series techniques**. The study covers:

- **Data Cleaning & Preprocessing** – Handling missing values, outlier detection, and data normalization.
- **Exploratory Data Analysis (EDA)** – Identifying key trends, correlations, and seasonal patterns.
- **Forecasting Model Implementation** – Using **ARIMA (AutoRegressive Integrated Moving Average)**.
- **Model Evaluation** – Assessing performance using **Mean Absolute Error (MAE) & Root Mean Squared Error (RMSE)**.
- **Insights & Conclusions**– Interpreting findings.
2. ### <a name="_u5a53x7w54ah"></a>Objective of the Study
The primary objective is to **predict temperature trends** using time-series forecasting techniques and analyze the impact of various weather parameters.
3. ### <a name="_ymhf13gxjduc"></a>Dataset Description
- **Total Records:** 50,085
- **Total Features:** 41
- **Time Period:** Continuous weather data
4. ### <a name="_1rlivbcmd193"></a>Key Variables in Dataset
- **Temperature (Celsius & Fahrenheit)** – Target variable for forecasting.
- **Precipitation (mm & inches)** – Measures rainfall at different locations.
- **Wind Speed (mph & kph)** – Affects temperature variations.
- **Humidity & Air Quality** – Indicators of climate conditions.
- **Timestamps** – Essential for time-series analysis.
-----
## <a name="_xlpkd41vqavc"></a>**Data Cleaning and Preprocessing:**
1. ### <a name="_s2rcd3sndoq"></a>Handling Missing Values:
- The dataset was **analyzed for missing values**, and **no missing data** was detected.
- Therefore, **no imputation was necessary** for data integrity.
2. ### <a name="_aybfdaup46q9"></a>Outlier Detection:
To ensure **data reliability**, outliers were identified using the **Interquartile Range (IQR) method**.

**Interquartile Range (IQR) Method**

- The IQR method detects outliers by calculating: 

  Q1 =25th Percentile          

  Q3=75th Percentile

  IQR=Q3-Q1

  Lower Bound=Q1-1.5\*IQR      

  Upper Bound=Q3+1.5\*IQR  

- Any value **outside this range** was flagged as an **outlier**.

3. ### <a name="_ftjsxxwvq8ch"></a>Outlier Handling:
   Two methods of Outlier Handling were tried out to determine which worked the best:

   C.1. Winsorization

✔  Winsorization is a technique that **limits extreme values by replacing them with the nearest boundary value** at a certain percentile (e.g., **1st and 99th percentiles**).
✔  This method **retains the total number of observations** while **reducing the impact of extreme values**.

🔹 **How Winsorization Works:**

- If a value is **below the 1st percentile**, it is **replaced** with the **1st percentile value**.
- If a value is **above the 99th percentile**, it is **replaced** with the **99th percentile value**.                          

C.2. Clipping(Capping)

✔ Clipping (also known as **capping**) involves **replacing all values beyond a fixed threshold with the threshold itself**.
✔ Unlike Winsorization, **clipping strictly removes all outliers** beyond the set boundary.

🔹 **How Clipping Works:**

- If a value is **less than a lower threshold**, it is **replaced with the lower threshold**.
- If a value is **greater than an upper threshold**, it is **replaced with the upper threshold**.            

1. ### <a name="_bwknredsp1jm"></a>Skewness Analysis:
- The **skewness of all columns** was calculated to determine **the distribution of data in each feature**.
- **Observation:** Most columns exhibited **high skewness**, meaning they were **not normally distributed**.
- **Impact on Outlier Handling:** Since the data was skewed, traditional outlier removal techniques (like Z-score-based removal) **could have caused excessive data loss**.
- Since, it was observed that most of the columns were highly skewed and so, winsorization method was applied to handle the extreme outliers. This was because the clipping method worked best with data which is normally distributed whereas winsorization worked best with skewed data.
1. ### <a name="_7tjghx7up6mw"></a>Data Normalization (Scaling):
After handling outliers, the dataset was **scaled** using **Min-Max Scaling**:

- Since different variables had **different ranges** (e.g., temperature in Celsius, air quality in arbitrary index values), **Min-Max Scaling** was applied: 

  `                           `Xscaled =X-XminXmax-Xmin​​

-----
1. ## <a name="_y10i8djdloed"></a>**Exploratory Data Analysis (EDA):**
1. ### <a name="_38i6cy7x0ibq"></a>Plotting Graphs of all Features to obtain trends:
- **Temperature:** Normally distributed with some skewness in extreme values.
- **Wind Speed & Gusts:** Right-skewed, meaning most locations have low wind speeds, but extreme wind speeds exist.
- **Humidity & Cloud Cover:** Bimodal, with peaks at low and high values, indicating distinct dry and humid regions.
- **UV Index:** Skewed towards lower values, likely due to locations experiencing varied sunlight exposure.
- **PM2.5, PM10, CO, NO2, SO2:** Skewed right, with a majority of locations having low pollution, but some highly polluted regions.
- **EPA and DEFRA Indexes:** Mostly concentrated in lower values (indicating acceptable air quality), but some spikes in unhealthy levels.
- **Moon Illumination:** Nearly uniform, as expected from cyclic moon phases.
1. ### <a name="_ttno7ckyxtz8"></a>Correlation Matrix:
- **Highly Correlated:**
- **Wind Speed & Gusts** (~0.95) → Strong winds lead to strong gusts.
- **Humidity & Cloud Cover** (~0.56) → High humidity generally leads to cloud formation.
- **Air Pollution Components(CO, NO2, PM2.5, PM10) and Air Quality Indices**(~0.5-0.76)**:** Presence of Air pollutants lead to increase in air quality indices as expected.
- **Inverse Correlations:**
- **Temperature & Humidity** (~-0.26) → Higher temperatures tend to reduce humidity.
- **Cloud Cover & Visibility** (~-0.1) → More cloud cover often reduces clear visibility.
- **Weak/No Correlation:**
- **Air Pollution Components (PM2.5, PM10, CO, NO2, SO2)** (~0.2-0.7) → It would be expected that pollutants of one kind would lead to an increase in another but that isn’t the case statistically. 
- **Visibility & Air Pollution (PM2.5, PM10, CO, etc.)** (~-0.1-0) → It would be expected that pollutants would lead to decrease in visibility however statistically, visibility doesn’t depend much on pollution.
- **Moon Phases & Weather**(~0)**:** As expected, moon phases do not influence weather.
- **Wind Direction & All other factors**(~0)**:** No significant relationship.
1. ### <a name="_agbha2wv2nxd"></a>Temperature Trends over time:
- The plot reveals a **seasonal pattern**, where temperature values **rise and fall periodically**, indicating **recurring trends**. 
- The temperature was high in **June-September** likely due to the summer **season** in most of the parts of Earth whereas the temperatures were low between **September and February** likely due to the winter **season**.
- The data suggests the presence of **long-term trends**, which may indicate gradual climate shifts or yearly variations.
- Short-term **fluctuations** were also observed, likely due to **daily weather changes, atmospheric conditions, or local environmental factors**.

1. ### <a name="_wpognvn4uwjz"></a>Precipitation Trends over time:
- Unlike temperature, **precipitation does not follow a smooth trend** and instead appears **sporadic**, with frequent **zero or very low values** and occasional spikes indicating heavy rainfall events. This aligns with the **skewed precipitation distribution observed in the histogram**, where most values were close to zero.
- The precipitation trend shows **sudden, sharp increases** on specific days, likely due to **localized storms, seasonal monsoons, or atmospheric disturbances**.
- Precipitation **does not exhibit a clear long-term trend**. Since **precipitation does not follow a smooth trend**, traditional forecasting models like **ARIMA may struggle** with accuracy.


1. ### <a name="_98sjjvslok84"></a>Data Visualizations:
- Distribution of Temperature:

The dataset is left-skewed but not much. This indicates that the dataset doesn’t exhibit very extreme variations in data.

- Distribution of Precipitation:

The precipitation data was **highly skewed**, meaning most recorded values were **low or near zero**, while a few extreme values represented heavy rainfall events. This suggests that **rainfall is not evenly distributed**, with dry periods being far more common than high-rainfall periods.

- Temperature vs Precipitation:

A scatter plot was used to visualize the relationship between **temperature and precipitation**. The plot **showed no strong correlation**, meaning that **higher temperatures do not necessarily lead to increased precipitation** or vice versa. This means that there are several other factors that affect precipitation in an area.

-----
1. ## <a name="_2nx725dvg3g"></a>**Forecasting Model:**
1. ### <a name="_84s7liwzzbjm"></a>Model used and why?
In our case, the ARIMA (AutoRegressive Integrated Moving Average) model was used to predict future temperatures. It is a widely used statistical approach for **time-series forecasting**, particularly for **predicting numerical values over time**.
1. ## <a name="_w66obk2o9ik1"></a>Model Implementation:
- To train the ARIMA model, the dataset was split into:
- **Training Data:** All the data before **January 23, 2025** were used to train the model on historical temperature patterns.
- **Test Data:** Data from **January 23 - January 29, 2025** were used to evaluate how well the model performs on unseen future data.
- Model was configured with the following **parameters (p, d, q):**
- **p (AutoRegressive Component) = 5** → The model uses the last **5 temperature values** to predict the next value.
- **d (Differencing) = 1** → The data was **differenced once** to remove trends and make it stationary.
- **q (Moving Average Component) = 2** → The model incorporates information from **the last 2 forecast errors** to adjust predictions.

These parameters were chosen based on **analysis of the autocorrelation function (ACF) and partial autocorrelation function (PACF) plots**.
1. ### <a name="_qulbj48sm2as"></a>Output prediction:
The **ARIMA model was used to generate temperature forecasts** for the test period **(January 23 - January 29, 2025)**.

These predictions were later evaluated against **actual recorded temperatures** using **Mean Absolute Error (MAE) and Root Mean Squared Error (RMSE)** to assess the model’s accuracy. 

-----
1. ## <a name="_xn36lhwsms55"></a>**Model Evaluation:**
Comparison of **actual vs predicted** temperatures were done for **January 23** till **January 29.** These values were then used to calculate the error metrics.
1. ### <a name="_zhmnxzmswx79"></a>Performance Metrics used:
- **Mean Absolute Error (MAE)**
- **Root Mean Squared Error (RMSE)**
1. ### <a name="_dzmop2ufuidd"></a>Model Evaluation:
The error metrics were calculated:

|**MAE (Lower is better)**|**RMSE (Lower is better)**|
| :-: | :-: |
|0\.9678221548340323|1\.002382024281664|

**ARIMA Model provided a reasonable short-term forecast.** The error metrics are quite decent and it can be said that the model worked well.
1. ## <a name="_mmrwag65y9eo"></a>**Insights and Observations:**
1. ### <a name="_pqrqc78y77q3"></a>Key Observations:
- **ARIMA** was effective for **short-term temperature predictions.**
- **No strong relationship** was found between **temperature** & **precipitation.** A **scatter plot** analysis of **temperature** vs. **precipitation** revealed that higher temperatures do not necessarily lead to increased precipitation.
- **Seasonality** was detected, meaning advanced seasonal models could improve forecasting accuracy. The **temperature trend analysis and ACF/PACF plots** showed a **repeating pattern over specific intervals**, indicating the presence of **seasonality** in the data.


