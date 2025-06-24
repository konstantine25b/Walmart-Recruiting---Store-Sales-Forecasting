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

კაი, რადგან ვნახე ზოგადად ჩვეულებრივი მარტივი ტრაინინგით სქორი, ახლა დავიწყოთ ნელ ნელა პრეპროცესინგი ამიტომ შევქენი ახალი ფაილი სადაც დავაკვირდები დათას და ვცდი სხვადასხვა მიდგომებს პრეპროცესინგისთის.

https://dagshub.com/konstantine25b/Walmart-Recruiting---Store-Sales-Forecasting.mlflow/#/experiments/1

აქაც ჩვეულებრივად ინიციალიზაცია გავუკეთე mlflow-ზე
 ჯერ დავმერჯე ყველა დატასეტი ერთმანეთთან.
ნუ დუპლიკატი სვეტი აღმოგვიჩნდა isholiday -ჯერ ეგ მოვაშორე. 

ჯერ დავამატე მარტივი ფიჩერები:
year, month, day, dayOfweek,weekofyear,quarter, IsWeekend,IsMonthStart,IsMonthEnd,IsQuarterStart,IsQuarterEnd

შემდეგ:

DaysFromStart, WeeksFromStart -ესენი როგორც გავარკვიე კაიაო linear trend-ის დასაჭერადო.

['Store', 'Dept', 'Date', 'Weekly_Sales', 'Type', 'Size', 'Temperature', 'Fuel_Price', 'MarkDown1', 'MarkDown2', 'MarkDown3', 'MarkDown4', 'MarkDown5', 'CPI', 'Unemployment', 'IsHoliday', 'Year', 'Month', 'Day', 'DayOfWeek', 'WeekOfYear', 'Quarter', 'IsWeekend', 'IsMonthStart', 'IsMonthEnd', 'IsQuarterStart', 'IsQuarterEnd', 'DaysFromStart', 'WeeksFromStart']

ესენი არის ჩვენი სვეტები ახლა.
ახლა გვინდა რომ nan -ები დავფიქსოთ და ასევე აუთლაიერებს მივედოთ.

ჯერ ამისთვის ვნახოთ როგორ გამოიყურება დატა.

ნუ როგორც ვნახე nan მხოლოდ markdown-ებში გვაქ რადგან 2011-იდან გვაქ მანდ დატა მხოლოდ ამიტო მანდ 
რაღაც უნდა მოვუფიქრო.

კაი ნუ ჯერ შევხედოთ და გვაქ ასეთი რამე:
MarkDown2	MarkDown2	310322	73.611025	float64
MarkDown4	MarkDown4	286603	67.984676	float64
MarkDown3	MarkDown3	284479	67.480845	float64
MarkDown1	MarkDown1	270889	64.257181	float64
MarkDown5	MarkDown5	270138	64.079038	float64

აქ ბერვი nan გვაქ.

გვაქ ნეგატიური სეილები ანუ წაგებაში რო მიხის ეგ:
Negative sales range: $-4,988.94 to $-0.02

72 -ე დეპარტამენტშ გვაქ ძალიან მაღალი სეილები, მაგრამ გავითვალისწინოთ რომ ერთ ერთ ჰოლიდეიზე ხდება ეგ ამიტომ აუთლაიერებში ვერ გავიყვანთ მაგას. ნუ თან გავითვალისწინოთ რომ სხვადასხვა ტიპის სტორებში სხვადასხვაა საშუალო სეილების დონე.

ამიტომ ასე ვფიქრობ რომ თუ ჰოლიდეი არაა და მაინც აუთლაიერია მაშინ მოვაშოროთ.

ყველასთვის ანუ ყველა სთორისთვის ვქნათ ასეთი რამ რო თუ ძააან დიდია ან ძაან პატარა მოვაშოროთ. 
ამისთვის ჯერ მოდი ვიპოვოთ ტრეშჰოლდები.
და მერე მოვაშოროთ.

📊 CALCULATING ACTUAL THRESHOLDS BY STORE TYPE
============================================================
(Using train_with_dates - the actual dataset we have)

🏪 STORE TYPE A
Sample size: 215,478 records
----------------------------------------
 80.0th percentile: $ 32,521.16 | 43,096 records above (20.00%)
 85.0th percentile: $ 40,311.11 | 32,322 records above (15.00%)
 90.0th percentile: $ 53,172.67 | 21,548 records above (10.00%)
 95.0th percentile: $ 74,285.73 | 10,774 records above (5.00%)
 97.5th percentile: $ 94,116.95 |  5,387 records above (2.50%)
 99.0th percentile: $126,175.36 |  2,155 records above (1.00%)
 99.5th percentile: $149,165.60 |  1,078 records above (0.50%)
 99.9th percentile: $184,007.45 |    216 records above (0.10%)

📅 Holiday vs Normal Sales for Store Type A:
Normal Sales   - Max: $356,867.25 | 99.5th: $148,205.09
Holiday Sales  - Max: $474,330.10 | 99.5th: $168,918.87

🏪 STORE TYPE B
Sample size: 163,495 records
----------------------------------------
 80.0th percentile: $ 19,089.04 | 32,699 records above (20.00%)
 85.0th percentile: $ 24,038.83 | 24,525 records above (15.00%)
 90.0th percentile: $ 32,101.40 | 16,350 records above (10.00%)
 95.0th percentile: $ 46,279.64 |  8,175 records above (5.00%)
 97.5th percentile: $ 58,587.54 |  4,088 records above (2.50%)
 99.0th percentile: $ 77,292.35 |  1,635 records above (1.00%)
 99.5th percentile: $ 93,515.75 |    818 records above (0.50%)
 99.9th percentile: $129,147.93 |    164 records above (0.10%)

📅 Holiday vs Normal Sales for Store Type B:
Normal Sales   - Max: $406,988.63 | 99.5th: $ 92,397.70
Holiday Sales  - Max: $693,099.36 | 99.5th: $113,205.04

🏪 STORE TYPE C
Sample size: 42,597 records
----------------------------------------
 80.0th percentile: $ 16,314.79 |  8,520 records above (20.00%)
 85.0th percentile: $ 20,140.12 |  6,390 records above (15.00%)
 90.0th percentile: $ 31,192.28 |  4,260 records above (10.00%)
 95.0th percentile: $ 46,833.02 |  2,130 records above (5.00%)
 97.5th percentile: $ 59,351.24 |  1,065 records above (2.50%)
 99.0th percentile: $ 72,103.37 |    426 records above (1.00%)
 99.5th percentile: $ 81,200.31 |    213 records above (0.50%)
 99.9th percentile: $ 95,654.76 |     43 records above (0.10%)

📅 Holiday vs Normal Sales for Store Type C:
Normal Sales   - Max: $112,152.35 | 99.5th: $ 80,865.55
Holiday Sales  - Max: $110,379.12 | 99.5th: $ 88,838.55



ხოოო საინტერესოდ გამოიყურება ეს ტრეშჰოლდების გატესტვით ამიტომ როგორც შევხედოთ სხვადასხვა ტიპის სტორში სხვადასხვა გვინდა.

შესაბამისად ავიღებ რეკომენდირებულ თრეშჰოლდებს და გავფილტრავ დატას.
📊 BEFORE outlier removal: 421,570 records

🏪 Store Type A:
   Normal day outliers:  206 records
   Holiday outliers:     0 records
   Total outliers:       206 records (0.10%)

🏪 Store Type B:
   Normal day outliers:  118 records
   Holiday outliers:     0 records
   Total outliers:       118 records (0.07%)

🏪 Store Type C:
   Normal day outliers:  15 records
   Holiday outliers:     0 records
   Total outliers:       15 records (0.04%)

📊 AFTER outlier removal: 421,231 records
🗑️ Outliers removed: 339 records (0.08%)


ახლა უკვე შეგვიძლია nan-ებს მივხედოთ მარა
რახან ეხა კაი ბევრი დატა გვაქ მოდი ჯერ საერთოდ მოვაშოროთ markdown-ები და nan-ები აღარ გვექნება და ესე შევხედოთ და გავტესტოთ.

ეს იმიტომ რო 60% missing დატაა და მისი შევსება კაი საჩალიჩო იქნება და მოდი ჯერ არ გვინდა.

Data columns (total 24 columns):
 #   Column          Non-Null Count   Dtype         
---  ------          --------------   -----         
 0   Store           421231 non-null  int64         
 1   Dept            421231 non-null  int64         
 2   Date            421231 non-null  datetime64[ns]
 3   Weekly_Sales    421231 non-null  float64       
 4   Type            421231 non-null  object        
 5   Size            421231 non-null  int64         
 6   Temperature     421231 non-null  float64       
 7   Fuel_Price      421231 non-null  float64       
 8   CPI             421231 non-null  float64       
 9   Unemployment    421231 non-null  float64       
 10  IsHoliday       421231 non-null  bool          
 11  Year            421231 non-null  int32         
 12  Month           421231 non-null  int32         
 13  Day             421231 non-null  int32         
 14  DayOfWeek       421231 non-null  int32         
 15  WeekOfYear      421231 non-null  UInt32        
 16  Quarter         421231 non-null  int32         
 17  IsWeekend       421231 non-null  int64         
 18  IsMonthStart    421231 non-null  int64         
 19  IsMonthEnd      421231 non-null  int64         
 20  IsQuarterStart  421231 non-null  int64         
 21  IsQuarterEnd    421231 non-null  int64         
 22  DaysFromStart   421231 non-null  int64         
 23  WeeksFromStart  421231 non-null  int64  

 კაი 24 column გვაქ მემგონი კაია ჯერ.

 კაი ეხა კატეგორიულებს მივხედოთ და მაღალი კორელაციის მქონეები მოვაშოროთ.

 1️⃣ CATEGORICAL VARIABLES:
   Categorical columns: ['Type']
   Store Type distribution: {'A': 215272, 'B': 163377, 'C': 42582}

2️⃣ NUMERICAL VARIABLES SCALING:
   Store       : Range [    1.00,    45.00] | Mean:    22.21 | Std:    12.79
   Dept        : Range [    1.00,    99.00] | Mean:    44.24 | Std:    30.48
   Size        : Range [34875.00, 219622.00] | Mean: 136706.14 | Std: 60979.97
   Temperature : Range [   -2.06,   100.14] | Mean:    60.10 | Std:    18.45
   Fuel_Price  : Range [    2.47,     4.47] | Mean:     3.36 | Std:     0.46
   CPI         : Range [  126.06,   227.23] | Mean:   171.21 | Std:    39.16
   Unemployment: Range [    3.88,    14.31] | Mean:     7.96 | Std:     1.86

3️⃣ HIGH CORRELATION CHECK:
   High correlations found (>0.8):
   - Year ↔ DaysFromStart: 0.942
   - Year ↔ WeeksFromStart: 0.942
   - Month ↔ WeekOfYear: 0.996
   - Month ↔ Quarter: 0.967
   - WeekOfYear ↔ Quarter: 0.964
   - IsMonthStart ↔ IsQuarterStart: 0.863
   - DaysFromStart ↔ WeeksFromStart: 1.000


ნუ ამოვიღოთ მაღალი კორელაციის მქონეები: 
DaysFromStart, WeekOfYear, Quarter, Year,
Day, IsQuarterStart, IsQuarterEnd

REMAINING FEATURES (17 total) 
ახლა კატეგორიულიც დავამატოთ და ეგაა.One-Hot Encoding რადგან 3 ცალი გვაქ A, b,c.

  A → 0 (215,272 records)
   B → 1 (163,377 records)
   C → 2 (42,582 records)

   New columns: ['Store', 'Dept', 'Date', 'Weekly_Sales', 'Size', 'Temperature', 'Fuel_Price', 'CPI', 'Unemployment', 'IsHoliday', 'Month', 'DayOfWeek', 'IsWeekend', 'IsMonthStart', 'IsMonthEnd', 'WeeksFromStart', 'Type_A', 'Type_B', 'Type_C']

   19 ქოლუმი გვაქ ეხა.

### კაი ახლა გადავედით დატის გაყოფაზე.

რადგან time series არის აქ უფრო კომპლექსურად ხდება ყველაფერი.
ამიტომ დატის გაყოფაც რთულია შედარებით.
ესე პირდაპირ 80/10/10, გაყოფა არასწორია რადგან ქრონოლოგიას აქვს მნიშვნელობა.

მოკლედ ეს კაი კომპლექსურია ამიტომ დეპარტამენტების მიხედვით გავსპლიტავ დატას და თითოში კვირების მიხედვით მოვუყრი თავს ასეთი პრინციპით:

ALL Store-Dept combinations:
Train: [Week1 → Week114]  (80% of weeks)
Val:   [Week115 → Week128] (10% of weeks)  
Test:  [Week129 → Week143] (10% of weeks)

აქ ამითი შევინახავთ ქრონოლოგიას.
DATE CUTOFFS:
   Train: 2010-02-05 00:00:00 to 2012-04-06 00:00:00
   Val:   2012-04-13 00:00:00 to 2012-07-13 00:00:00
   Test:  2012-07-20 00:00:00 to 2012-10-26 00:00:00

ტრეინინგისთვის მხოლოდ train და val.

მაგრამ ტესტისთვის უფრო კომპლექსური ისაა რო ეგრევე test-ზე თუ გავტესტეთ დატა ლიქიჯი გვექნება და ამიუტომ გვინდა რომ train+ val და test გვქონდეს.

კაი ეხა ტრეინინგისთვის ავიღოთ xgboost ისევე და ესე  n_estimators=200,           # More trees
            max_depth=8,                # Deeper trees  
            learning_rate=0.05,         # Lower learning rate
            subsample=0.8,


IMPROVED RESULTS:
   Train: MAE=$2,946.87 | RMSE=$5,377.46 | WMAE=$3,107.74
   Val:   MAE=$3,943.09 | RMSE=$6,816.74 | WMAE=$3,943.09
 
კაი ნუ უკეთესი შედეგი გვაქ ტრეინინგისას ვიდრე პირველ ექსპერიმენტში., ახლა პირდაპირ ტესტინგი ვნახოთ ანუ train + val და test.

TOP 10 FEATURES (Improved Model):
    1. Dept           : 0.3925
    2. Type_Encoded   : 0.1816
    3. Size           : 0.1381
    4. IsHoliday      : 0.0686
    5. Store          : 0.0634
    6. Month          : 0.0496
    7. CPI            : 0.0274
    8. IsMonthEnd     : 0.0261
    9. Unemployment   : 0.0220
   10. WeeksFromStart : 0.0163



ახლა იგივე ნაირად მოგვიწევს დატრეინინგება და მერე ტესტსეტყზე გაშვება, ნუ ამის მიზეზი არის ის რომ დატა ლიქიჯი გვაქ და არ გვინდა ეგ. 

 Original Train: 335,453 records
   Original Val:   41,378 records
   Combined Train: 376,831 records
   Test Set:       44,400 records

FINAL TESTING RESULTS:
   Training (Train+Val): MAE=$2,896.51 | RMSE=$5,242.54 | WMAE=$3,045.81
   Testing  (Test only): MAE=$2,968.85 | RMSE=$4,959.53 | WMAE=$3,049.17

TOP 10 MOST IMPORTANT FEATURES:
    1. Dept           : 0.4108
    2. Type_Encoded   : 0.1807
    3. Size           : 0.1415
    4. Store          : 0.0657
    5. IsHoliday      : 0.0630
    6. Month          : 0.0455
    7. CPI            : 0.0269
    8. Unemployment   : 0.0210
    9. IsMonthEnd     : 0.0187
   10. WeeksFromStart : 0.0124

ნუ ბევრად გაუმჯობესდა დაჟე ვიდრე ტრეინინგში იყო ალბათ იმიტომ რომ უფრო მეტი დატა გვაქ, ქრონოლოგიურადაც უკეთესია.

ასევე აქ როგორც ვუყურებ ძალიან კარგი ჯენერალიზაცია გვაქ. 

Training (Train+Val): MAE=$2,896.51
Testing  (Test only): MAE=$2,968.85
ასევე ბევრად უკეთსეი შედეგი გვაქ ვიდრე პირველ ექსპერმიენტში.

Experiment 1 Test WMAE: $4,163.80
Final Test WMAE:        $3,049.17

ჩემი აზრით კარგია ჯერ ჯერობით კაი შედეგია.

ნუ აქაც პრობლემა ისაა რო გითჰუბზე არ აჩვენებს თორე ისე რო დაფულო მუშაობს და განახებს notebook-ს.
