---
title: mysql example RevenueForecast forecast timeseries (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'RevenueForecast forecast timeseries'


Modules used in program: 
* `import numpy as np`
* `import matplotlib.pyplot as plt`
* `import statsmodels.api as sm`
* `import logging`
* `import pandas as pd`

## python RevenueForecast forecast timeseries

Python mysql example: RevenueForecast forecast timeseries

```python
import pandas as pd
import logging
import statsmodels.api as sm
import matplotlib.pyplot as plt
import numpy as np
from statsmodels.tsa.api import Holt
from statsmodels.tsa.holtwinters import ExponentialSmoothing


DEBUG = True

class TimeSeries:
    def __init__(self):
        pass

    def read_data(self, filename):
        df = pd.read_csv(filename)
        return df

    def forecast(self, filename):
        logging.info('1. reading data.')

        df = self.read_data(filename)

        logging.info("2. clean the headers.")
        indexes = ['Region', 'Master_Account_Id', 'Product']
        date_columns = df.columns[7:31].tolist()
        date_columns = [col.replace('Rev_', '') for col in date_columns]
        df.columns = df.columns[0:7].tolist() + date_columns

        logging.info("3. handle the trainning data.")
        train_cols = df.columns[7:28].tolist()
        train_cols = indexes + train_cols
        train = df[train_cols]
        train.index = train[indexes]
        transposed_train = train.transpose()[3:]
        transposed_train.index = pd.to_datetime(transposed_train.index, format='%Y_%m')

        logging.info("4. handle the test data.")
        test_cols = indexes + df.columns.values[28:31].tolist()
        test = df[test_cols]
        test.index = test[indexes]
        transposed_test = test.transpose()[3:]
        transposed_test.index = pd.to_datetime(transposed_test.index, format='%Y_%m')

        # forecast dataset.
        y_hat_avg = transposed_test.copy()
        foreast_period = len(y_hat_avg)

        logging.info("4. Iterate all the columns of the dataset to forecast each customer's revenue.")

        transposed_train = transposed_train.astype(np.float64)
        transposed_test = transposed_test.astype(np.float64)

        i = 0
        N = 5
        for col in transposed_train.columns:
            i += 1
            if i > N:
                break
            train_records = transposed_train.loc[:, [col]]
            test_records = transposed_test.loc[:, [col]]
            col_name = '_'.join(map(str, col))

            logging.info("\nForecasting %s." % col_name)

            if DEBUG:
                # check the trend, seasonality
                sm.tsa.seasonal_decompose(train_records[col]).plot()
                result = sm.tsa.stattools.adfuller(train_records[col])
                plt.savefig("%s_seasonal_decompose.png" % col_name)
                plt.close()

            logging.info("5.1 Forecast with Holt linear method.")
            fit1 = Holt(np.asarray(train_records[col])).fit(smoothing_level=0.3, smoothing_slope=0.1)
            y_hat_avg['Holt_linear'] = fit1.forecast(foreast_period)


            logging.info("5.2 Forecast with Holt Winter method.")
            fit2 = ExponentialSmoothing(np.asarray(train_records[col]), seasonal_periods=7,
                                        trend='add', seasonal='add', ).fit()
            y_hat_avg['Holt_Winter'] = fit2.forecast(len(transposed_test))

            # draw the forecast picture with train and test data.
            if DEBUG:
                plt.figure(figsize=(16, 8))
                plt.plot(train_records[col], label='Train')
                plt.plot(test_records[col], label='Test')
                plt.plot(y_hat_avg['Holt_linear'], label='Holt_linear')
                plt.plot(y_hat_avg['Holt_Winter'], label='Holt_Winter')
                plt.legend(loc='best')
                plt.savefig("%s_forecast.png" % col_name)
                plt.close()


if __name__ == '__main__':
    filename = ''
    ts = TimeSeries()
    ts.forecast(filename)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
