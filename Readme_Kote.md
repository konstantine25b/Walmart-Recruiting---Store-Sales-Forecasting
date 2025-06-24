დავიწყოთ მუშაობა თავიდან ჯერ როგორც ყოველთვის ვეცდები გავერკვიო დატაში.

კოლაბში ვიმუშავებ როგორც ვნახეთ ბევრად უკეთესია კეგლში მუშაობაზე.

მინდა დავამატო რომ სანამ დავიწყებდი მანამდე რაღაც ვიდეოები ვნახე time series ალგორითმებზე და რაღაც მიდგომები გავიაზრე.

ჯერ განვიხილოთ რაც წერია:
წერია რომ ყურადღება ექცევა holidays weeks და 5ჯერ მეტად ითვლებაო. ალბათ ამიტო გვაქ WMAE.

გვაქ stores.csv სადაც არის 45 store-ზე ინფორმაციები
 id, Type(A, B, C) , size 

გვაქ train.csv  2010-02-05 to 2012-11-01
Store, Department, Date, Weekly_Sales, IsHoliday

კიდევ test.csv
Store, Department, Date, Weekly_Sales, IsHoliday

და ეს განსხვავებული რამეა: features.csv
CPI, unemployment, fuel price, temperature, MarkDown1-5, sparse data.

ესენი კიდე მთავარი 5x დღეები:

Super Bowl: 12-Feb-10, 11-Feb-11, 10-Feb-12, 8-Feb-13
Labor Day: 10-Sep-10, 9-Sep-11, 7-Sep-12, 6-Sep-13
Thanksgiving: 26-Nov-10, 25-Nov-11, 23-Nov-12, 29-Nov-13
Christmas: 31-Dec-10, 30-Dec-11, 28-Dec-12, 27-Dec-13



კაი ახლა უკვე დავიწყოთ:

# experiment_1_k.ipynb
აქ ჯერ დავაუნზიპებ colab-ში ფაილებს და ვნახოთ რა როგორ გამოიყურება:

📊 DATASET OVERVIEW
==================================================
STORES:
  Shape: (45, 3)
  Columns: ['Store', 'Type', 'Size']
  Memory: 0.00 MB

TRAIN:
  Shape: (421570, 5)
  Columns: ['Store', 'Dept', 'Date', 'Weekly_Sales', 'IsHoliday']
  Memory: 36.99 MB

TEST:
  Shape: (115064, 4)
  Columns: ['Store', 'Dept', 'Date', 'IsHoliday']
  Memory: 9.22 MB

FEATURES:
  Shape: (8190, 12)
  Columns: ['Store', 'Date', 'Temperature', 'Fuel_Price', 'MarkDown1', 'MarkDown2', 'MarkDown3', 'MarkDown4', 'MarkDown5', 'CPI', 'Unemployment', 'IsHoliday']
  Memory: 1.16 MB


ნუ ყველაფერი ჩვეულებრივი მაგრამ features დატასეტი ცოტა კომპლექსურია ჯერ არ ვიცი როგორ გამოვიყენო მარა ვნახოთ.

განვიხილოთ ცალ ცალკე ყველა დატასეტი:

🏪 STORES DATASET
==============================
Number of stores: 45
Store types: {'A': 22, 'B': 17, 'C': 6}
Size range: 34,875 - 219,622

ხო ნუ შედარებით ცოდა C ტიპის გვაქ.

სთორებში ეს ტიპები სიდეიდეების მიხედვითაა ანუ ყველაზე დიდია A მერე B და ბოლოს C. 

📈 TRAINING DATASET
==============================
Date range: 2010-02-05 00:00:00 to 2012-10-26 00:00:00
Stores: 45
Departments: 81
Total records: 421,570
Holiday weeks: 29,661
Sales range: $-4,988.94 to $693,099.36

421,570 - ნამდვილად დიდი დატასეტია.


💰 SALES ANALYSIS
=========================
Mean weekly sales: $15,981.26
Median weekly sales: $7,612.03

Holiday vs Non-Holiday Sales:
mean	median	count
IsHoliday			
False	15901.445069	7589.95	391909
True	17035.823187	7947.74	29661




Top 10 departments by average sales:
Weekly_Sales
Dept	
92	75204.870531
95	69824.423080
38	61090.619568
72	50566.515417
65	45441.706224
90	45232.084488
40	44900.702727
2	43607.020113
91	33687.910758
94	33405.883963


როგორც ჩანს holiday-ს დროს 7% ით მეტი sale გვაქ.


🌡️ FEATURES DATASET
==============================
Date range: 2010-02-05 00:00:00 to 2013-07-26 00:00:00
Stores covered: 45
Store	Date	Temperature	Fuel_Price	MarkDown1	MarkDown2	MarkDown3	MarkDown4	MarkDown5	CPI	Unemployment	IsHoliday
0	1	2010-02-05	42.31	2.572	NaN	NaN	NaN	NaN	NaN	211.096358	8.106	False
1	1	2010-02-12	38.51	2.548	NaN	NaN	NaN	NaN	NaN	211.242170	8.106	True
2	1	2010-02-19	39.93	2.514	NaN	NaN	NaN	NaN	NaN	211.289143	8.106	False
3	1	2010-02-26	46.63	2.561	NaN	NaN	NaN	NaN	NaN	211.319643	8.106	False
4	1	2010-03-05	46.50	2.625	NaN	NaN	NaN	NaN	NaN	211.350143	8.106	False



Missing values:
Missing Count	Missing %
MarkDown1	4158	50.769231
MarkDown2	5269	64.334554
MarkDown3	4577	55.885226
MarkDown4	4726	57.704518
MarkDown5	4140	50.549451
CPI	585	7.142857
Unemployment	585	7.142857

აქ თითოეული store-სთვის სხვადასხვა გამოყვანილი Feature-ბი გვაქ მარა ასევე გვაქვს ბევრი NAN.

ხო markdown-ში ეწერა რომ მხოლოდ 2011 -ის შემდეგ გვაქო და ასევე ყველაში არაო:

🔖 MARKDOWN DATA AVAILABILITY
===================================
MarkDown1: 4,032 records (49.2%)
MarkDown2: 2,921 records (35.7%)
MarkDown3: 3,613 records (44.1%)
MarkDown4: 3,464 records (42.3%)
MarkDown5: 4,050 records (49.5%)

First markdown data: 2011-11-11 00:00:00
As stated: Should be after Nov 2011 ✓

გავაკეთე ფლოთები და რასაც ვხედავ სეზონურობის პატერნიც იგრძნობა - იანვარში პიკშია ორივე წელს გაყიდვები.

### კაი დავიწყოთ დატრეინინგება რაც არის ტრეინინგ სეტი მხოლოდ იმაზე ჯერ არ გვინდა სხვა არაფერი. 
ჯერ გამოვიყენებ xgboost-ს.
თავისთავად დავლოგავ mlflow-ზე. 
ჯერ მაინტერესებს უბრალოდ უმარტივესი რა სქორს გვაძლევს.

ჯერ ინიციალიზაცია dagshub, და mlflow-ზე. 

https://dagshub.com/konstantine25b/Walmart-Recruiting---Store-Sales-Forecasting.mlflow/

ჯეტ მხოლოდ train.csv-ს გამოვიყენებ.
https://dagshub.com/konstantine25b/Walmart-Recruiting---Store-Sales-Forecasting.mlflow/#/experiments/0?searchFilter=&orderByKey=attributes.start_time&orderByAsc=false&startTime=ALL&lifecycleFilter=Active&modelVersionFilter=All+Runs&datasetsFilter=W10%3D

ხო ნუ BasicPreprocessor ეს უმარტივესი პრეპროცესორი.

ხოლო რაც შეეხება დატას:
trains გავყოფ 3-ად train + val +test . 60, 20 , 20
ნუ store-ების ინფო გვინდა თავისთავად უეჭველი ამიტო მხოლოდ ამას დავმერჯავ. და ასევე type_encoder-ს გამოვიყენებ რომ რიცვვებით გვქონდეს ესენი 0,1,2

ასევე holiday თუ არის 1 თუ არა 0.

კიდევ როგორც ვიდეოში ვნახე აქაც გამოვიყვან date-იდან ფიჩერებს : Year,Month , Week, DayOfYear

მისინგ ვალიუებში ჯერ პროსტა median (numerical) და mode (categorical) ჩავანაცვლებ.

რაც შეეხება ტრაინინგ xgboost: 100 ხე, 6 მაქსიმალური სიღრმე, 0.1 rate. 

ნუ შესამოწმებლად WMAE.
ამისთვის გავაკეთე მაგისი კლასი.
https://dagshub.com/konstantine25b/Walmart-Recruiting---Store-Sales-Forecasting.mlflow/#/experiments/0/runs/0f1276d7c85f426cb849c646a6d72ab5

📊 FINAL WMAE RESULTS:
   Competition Metric (WMAE): $4,110.79
   Regular MAE: $3,966.39
   Holiday weeks MAE: $4,619.24
   Normal weeks MAE: $3,916.50


ნუ ეს არის score , ლიდერბორდებში საუკეთესო შედეგი 2300 -ია. 

ჩვენ კიდე 4,110. 

კაი ნუ ჯერ ეს იყოს მხოლოდ პირველი ექსპერიმენტისთვის.

დავფუშე github-ზე მარა რაღაც ვიჯეტები არ მოსწონს და არ ანახებს ამიტო თუ ნოთბუქში ჩახედვა გინდათ დაფულვა მოგიწევთ.
ადრე ვნახე მსგავსი პრობლემა მაგრამ აქ ნეირონული ქსელისნაირად ეპოქების ავსება არ არის და არც აწითლებს ლოკალურად და ვერ ვხვდები ეხა რატო აქ პრობლემა.

# experiment_2_future_engineering_k.ipynb