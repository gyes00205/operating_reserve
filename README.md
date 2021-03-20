# Predict Operating Reserve(MW)
## Usage
* Because our data is operating reserve (MW) of 2021/01/01 ~ 2021/03/19, periods is 10
`python app.py --training "Your Training Data" --output submission.csv --periods 10`
## Data analysis
* Download [台灣電力公司_本年度每日尖峰備轉容量率]* (https://data.gov.tw/dataset/25850)
* The unit of operating reserve is 萬瓩，1 萬瓩 = 10 MW
![](https://i.imgur.com/s1ZwIlY.png)
* Convert 萬瓩 to MW
![](https://i.imgur.com/ov8vMn3.png)
* Plot Operating Reserve Graph
![](https://i.imgur.com/qDdKQI9.png)
## Model Training
* load data
```python
df_training = pd.read_csv(args.training)
```
* Use ARIMA to train model
```python
arima = auto_arima(df_training["operating_reserve(MW)"], seasonal=True)
```
* Predict
```python
result = arima.predict(n_periods=int(args.periods))
```
* Write csv
Only need the last 7 prediction
```python
result = {"date": [20210323, 20210324, 20210325, 20210326, 20210327, 20210328, 20210329],
          "operating_reserve(MW)": result[-7:]
         }
df_result = pd.DataFrame(result)
df_result.to_csv(args.output, index=0)
```


