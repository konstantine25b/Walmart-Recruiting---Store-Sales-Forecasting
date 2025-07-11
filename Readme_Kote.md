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


# experiment_3_k.ipynb

კაი უბრალოდ მაინტერესებს future დატასეტის გარეშე მხოლოდ stores, და train რომ გამოვიყენო 
ოღონდ მივიღო რაც შეილება ბევრი და კარგი ფიჩერი დეიტებიდან რა შედეგს დავდებ. 
თან ლაგინგ ფიჩერებს გამოვიყენებ ანუ ძველი დატას გავლენა რომ ქონდეს რა ეგეთს.
ასევე ვფიქრობ ერთერთი ალგორითმის facebook prophet-ის გატესტვას.

ნუ prophet- კაი ალგორითმია ადრეც გამომიყენებია. ამ შემთხვევაში basic prophets- გამოვიყენებ და არა ნეირონულს. იდეაში როგორც ვნახე ნეირონული უფრო უკეთეს შედეგებს დებსო მარა ესეც პატერნებზე ამიტომ ჯერ ეს იყოს. 

https://dagshub.com/konstantine25b/Walmart-Recruiting---Store-Sales-Forecasting.mlflow/#/experiments/2

### Cyclical encodings, Time since reference
 იდეაში დავამატებ Cyclical encodings-ეებს როგორც ვნახე სინუსი და კოსინუსები რაღაც მხრივ შეილება კაი ფიჩერებუ გამოდგეს და ასევე, რამდენი დრო გავიდა დაწყებიდან ან მსგავსი ფიჩერებიც საინტერესოა გასატესტად.

ამასთან ერთად ვამატებ lagging ფიჩერებს ანუ, ისტორიული პატერნების გამოყენება რომ შეძლოს.  მაგალითად ესეთები
Weekly_Sales_lag_1, 2, 4, 8, 13, 52. ესენი ძველ მონაცემებზე წვდომაშ დაეხმარება.
Weekly_Sales_rolling_mean_4 ,12 ..  ესენი პროსტა ევერიჯებია.

ხო თავიდან ვიფიქრე წინა წლის იგივე დღის დატას გავითვალისწინებ თქო სეზონურობისთვის მარა მხოლოდ 2 წლის დატა გვაქ ამიტო აზრი არ აქ მაგას.

კაი გავუშვი და დავამატე ფიჩერები ეხა შევხედოთ და ვნახოთ როგორია.

ჰმმ მოდი ჯერ xgboost-ზე ვნახავ და მერე prophet-ზე 
ამჯერად მხოლოდ ვალიდაციის და ტარეინის გვექნება მაინც ბოლოს იგივე მიწევს ტესტზე და ამიტო აღარ გამოვიყენებ ეხა

Date Features: 65 features
  Examples: ['Weekly_Sales', 'Year', 'Month']

Holiday Features: 7 features
  Examples: ['IsHoliday', 'IsSuperBowlWeek', 'IsLaborDayWeek']

Lag Features: 6 features
  Examples: ['Weekly_Sales_lag_1', 'Weekly_Sales_lag_2', 'Weekly_Sales_lag_3']

Rolling Features: 16 features
  Examples: ['Weekly_Sales_rolling_mean_4', 'Weekly_Sales_rolling_std_4', 'Weekly_Sales_rolling_min_4']

EWM Features: 3 features
  Examples: ['Weekly_Sales_ewm_0.1', 'Weekly_Sales_ewm_0.3', 'Weekly_Sales_ewm_0.5']

ეხა შევამოწმებ თუ nan-ები მაქ. 

ნუ nan-ებიც გვაქ თავისთავად.

📊 Overall NaN Summary:
   Total cells: 31,617,750
   Total NaNs: 325,131
   NaN percentage: 1.03%

📋 Columns with NaNs: 14 out of 75

🔍 Detailed NaN Analysis by Column:
============================================================
                          Column  NaN_Count  NaN_Percentage Data_Type
Weekly_Sales_same_week_prev_year     159167       37.755770   float64
             Weekly_Sales_lag_12      38615        9.159807   float64
     Weekly_Sales_quarterly_diff      38615        9.159807   float64
              Weekly_Sales_lag_8      25966        6.159357   float64
              Weekly_Sales_lag_4      13134        3.115497   float64
       Weekly_Sales_monthly_diff      13134        3.115497   float64
              Weekly_Sales_lag_3       9889        2.345755   float64
              Weekly_Sales_lag_2       6625        1.571507   float64
              Weekly_Sales_lag_1       3331        0.790142   float64
      Weekly_Sales_rolling_std_4       3331        0.790142   float64
     Weekly_Sales_rolling_std_26       3331        0.790142   float64
     Weekly_Sales_rolling_std_12       3331        0.790142   float64
      Weekly_Sales_rolling_std_8       3331        0.790142   float64
         Weekly_Sales_trend_diff       3331        0.790142   float64

ნუ Weekly_Sales_same_week_prev_year ამას მოვაშორებ და დანარჩენებს დავფიქსავ და ვეცდები გავასწორო.

ნუ ალბათ ჯობია დასაწყისები თუა 0 ებით შევავსი იმიტორო მანამდე დატა არ არის. 

ვსიო nan-ებისგან გავასუფთავე Total NaNs remaining: 0

 Data Quality Report:
   ✅ Total NaNs: 0
   ✅ Infinite values: 0
   ✅ Negative sales: 1,285 (0.30%)
   ✅ Duplicate rows: 0
   ✅ Date issues: 0

როგორც მივხვდი negative sales არის ესეთუ რამე:
Customer bought $100 worth of items last week → +$100 sales
Customer returned $30 worth of items this week → -$30 sales
Net effect: Week 1: +$100, Week 2: -$30

კაი ეხა გავწმინდოთ outliereb-ისგან.

ნუ გამოვიყენებ ამისთვის სხვადასხვა მეთოდებს:
IQR  - Q1 = 25th percentile (first quartile)
Q3 = 75th percentile (third quartile)  
IQR = Q3 - Q1

Lower Bound = Q1 - (multiplier × IQR)
Upper Bound = Q3 + (multiplier × IQR)

Any value outside these bounds = outlier

 Z-Score Method - Z-Score = (Value - Mean) / Standard Deviation

If |Z-Score| > threshold (usually 2 or 3), it's an outlier

Percentile Method - Keep only values between 0.5th percentile and 99.5th percentile
Remove the most extreme 1% of data (0.5% from each tail)

კაი მაგრამ აქ ერთი რამე უნდა გავითვალისწინოთ. როცა holiday- მაშინ ბევრად მეტი გაყიდვაა ამიტო holiday-ებითვის ცალკე უნდა გავფილტროთ.
ანუ გავყოფთ ორად და ცალ ცალკე დავთვლი აუთლაიერებს.

(ჰმმ, აქ მომივიდა იდეა რომ ბოლოში რო დავიუწყებ დატასეტი თავიდანვე ორად გავყო და ჰოლიდეიებისთვის ცალკე დავდო ფრედიქშენი და არაჰოლიდეიებისთვის 
ცალკე მარა მამენტ წინა დღის დატას გათვალისწინებაც და ისტორიული დატას გათვალისწინებაც კაია და მაშინ ეგ აღარ გამოვა, ვნახოთ შეილება ეგრეც გავტესტო)

Holiday Outlier Detection Results:
   IQR bounds: $-55,254 to $78,521
   Percentile bounds: $-4 to $193,132
   Z-score threshold: 3.5
   Negative threshold: $-15,000
     iqr: 897 outliers (3.02%)
     percentile: 150 outliers (0.51%)
     z_score: 339 outliers (1.14%)
     extreme_negative: 0 outliers (0.00%)

   Non-Holiday Outlier Detection Results:
   IQR bounds: $-43,049 to $65,260
   Percentile bounds: $-3 to $150,350
   Z-score threshold: 3.0
   Negative threshold: $-8,000
     iqr: 16,762 outliers (4.28%)
     percentile: 1,958 outliers (0.50%)
     z_score: 8,516 outliers (2.17%)
     extreme_negative: 0 outliers (0.00%)

ამასობაში გაიფილტრა და შედარებითი აუთლაიერები მოვაშორეთ.

ოკეი ეხა გავსპლიტოთ 80/20 ზე ოღონდ როგორც ტაიმ სერიეს დატა და არა ჩვეულებრივი.
✅ Training data saved: 321,636 records
✅ Validation data saved: 82,275 records
ხო ეს 80/20 სპლიტი კაია იმიტომ რომ აქ არის 421 000 ცალი , ხოლო ტესტ სეტშის არის 115 000 ხოდა დაახლოებით 80/20 ზეა. 
ჯერ xgboost-ს დავატრეინინგებ და ვნახოთ აბა რა შედეგი გვაქ.
71 ფიჩერი კი გვაქ.

https://dagshub.com/konstantine25b/Walmart-Recruiting---Store-Sales-Forecasting.mlflow/#/experiments/2/runs/912678876c794318881e59358327994d

 🎯 KEY METRICS (Validation Set):
     WMAE (Walmart Competition Metric): $94.20
     MAE: $91.00
     RMSE: $191.05
     R²: 0.9998

ეუფ აქ რაღაც ძაან ნიტო მოხდა სისულელე შედეგი დადო რაღაც შემეშალა.
შემეშალა რო დატა სპლიტი თავის დროზე არ გავაკეთე. ეჰჰ ახლიდან ვიზავ ეხა.

კაი ნუ კიდე იქ შემეშალა რო ეს ფიჩერები რო დავამატე მთლიან დატაზე ვჩალიჩობდი მაგას და არა მხოლოს 80% ზე. ნუ ამიტომ ამის წაშლას და ახლიდან დაწყებას ჯობია ახალი ექსპერიმენტი გავაკეთო.
მარა მაინც კაია იმიტორო ეხა ვიცი
რომ თუ მთლიანად გავწმენდდი ძააააან მაგარი შედეგი მექნებოდა.

კაი დავიწყებ თავიდან ოღონდ ისე რო თავიდანვე მექნება გასპლიტული. და იქ გამოვიყენებ prophet-ს.


# experiment_4_k.ipynb

კაი ეხა მესამეში დაშვებული შეცდომა გამოვასწოროთ.
იდეაში იგივეს ვიზავ რასაც მესაამეში მარა შეცდომებს ვეცდები ავარიდო თავი.

ნუ ჩვეულებრივად იგივეს ვაკეთებ მაგრამ დასაწყისშივე ვყოფ დატას.
ასევე one hot encoding for type.

ამის მერე მხოლოდ ტრეინინგ დატაზე გავაკეთე აუთლაიერების ამოღება.

Training Outlier Detection Results:
   Total outliers: 14,179 (4.22%)
   Holiday outliers: 807
   Non-holiday outliers: 13,372

🚂 Training: 321,582 records (removed 14,179)
🔮 Validation: 82,260 records (removed 3,549)

189 სვეტი გაქ იმიტომ რომ

Type: 3 dummy variables (Type_A, Type_B, Type_C)
Store: 45 dummy variables (Store_1, Store_2, ..., Store_45)
Dept: 81 dummy variables (Dept_1, Dept_2, ..., Dept_81)

ხოო ხოდა ეხა დავფიქრდი და იდეაშ თითო სთორისთვის ცალ ცალკე რომ ავაწყო მოდელი და მერე გავაერთიანო ეგეც ლოგიკურად შეილება ჩაითვალოს. მარა კაი საჩალიჩოა და ცუდია წესით პრაქტიკაში. მერე ვიკითხავ მაინც შენთან.

Training Metrics:
   WMAE: $52.89
   MAE: $52.34
   RMSE: $77.13
   R²: 1.0000
   Holiday MAE: $54.58
   Non-Holiday MAE: $52.15

🔮 Validation Metrics:
   WMAE: $93.04
   MAE: $90.73
   RMSE: $196.31
   R²: 0.9998
   Holiday MAE: $109.52
   Non-Holiday MAE: $90.05

🎯 Top 15 Feature Importance:
    1. Weekly_Sales_ewm_0.5               : 0.7531
    2. Weekly_Sales_ewm_0.3               : 0.2101
    3. Weekly_Sales_rolling_min_4         : 0.0115
    4. Weekly_Sales_trend_diff            : 0.0046
    5. Weekly_Sales_rolling_max_4         : 0.0034
    6. Weekly_Sales_rolling_std_4         : 0.0025
    7. Weekly_Sales_monthly_diff          : 0.0022
    8. IsHoliday                          : 0.0021
    9. Dept_7                             : 0.0019
   10. IsMajorHoliday                     : 0.0015
   11. Weekly_Sales_lag_1                 : 0.0007
   12. Weekly_Sales_rolling_max_8         : 0.0005
   13. Weekly_Sales_rolling_mean_4        : 0.0004
   14. IsChristmasWeek                    : 0.0004
   15. Weekly_Sales_lag_2                 : 0.0004

📈 Model Performance Analysis:
   Training vs Validation WMAE: $52.89 vs $93.04
   Overfitting check: -43.2% difference
   Holiday vs Non-Holiday performance:
     Holiday MAE: $109.52 (2879 records)
     Non-Holiday MAE: $90.05 (79381 records)


ვააა კიდე 94 $ იო კაი რაღაც აშკარა პრობლემაა მარა ვერ ვხვდები რა


Weekly_Sales_ewm_0.5               : 0.7531
Weekly_Sales_ewm_0.3               : 0.2101

ესენი ძალიან მაღალი იმფორთანსითა არიან, და ლოგიკურად აქ უნდა იყოს პრობლემა.

ააა ვსიო ვიპოვე

# Exponential weighted features
alphas = [0.1, 0.3, 0.5]
for alpha in alphas:
    df[f'{target_col}_ewm_{alpha}'] = (
        df.groupby(['Store', 'Dept'])[target_col]
        .ewm(alpha=alpha)
        .mean()
        .reset_index(level=[0,1], drop=True)
    )

ეს მთლიანზე ითვლის და იასნია ესაა პრობლემა.

კაი ნუ აღარ მინდა მე5-ს შექმნა ამიტომ ახლიდან დავრანავ პროსტა და დავფიქსე ეგ პრობლემა.

კაი ვნახოთ აბა რა იქნება იმედია დაიფიქსა.
ნუ ამოვიღე ეგ lagging ზედმეტად კომპლექსური იყო და დატა ლიქიჯს იწვევდა.

კაი ანუ ეხა ესე გამოგვივიდა რო დაგვრჩა ასეთი ფიჩერები:

 Date Features: 30 features
     Examples: ['Weekly_Sales_lag_8', 'IsChristmasWeek', 'Weekly_Sales_lag_4']
   Holiday Features: 7 features
     Examples: ['IsChristmasWeek', 'IsLaborDayWeek', 'IsHoliday']
   Lag Features: 5 features
     Examples: ['Weekly_Sales_lag_8', 'Weekly_Sales_lag_4', 'Weekly_Sales_lag_12']
   Rolling Features: 0 features
   EWM Features: 0 features
   Diff Features: 2 features
     Examples: ['Weekly_Sales_diff_4', 'Weekly_Sales_diff_1']


🚂 Training: (335761, 166)
🔮 Validation: (85809, 166)

ხო ეხა წავიდა ტრეინინგი:
მაინც არასწორია
1. Weekly_Sales_lag_1                 : 0.6252
2. Weekly_Sales_lag_4                 : 0.0314
3. Weekly_Sales_diff_1                : 0.0231

WMAE: $233.41
MAE: $245.95

ნუ ალბათ კიდე სადმე მაქ ლიქიჯი ვნახოთ აბა.

კაი გავასწორე კიდე და აი ასთია შედეგი:

🚂 Training Metrics:
   WMAE: $1,338.89
   MAE: $1,267.53
   RMSE: $2,495.64
   R²: 0.9868

🔮 Validation Metrics:
   WMAE: $2,519.10
   MAE: $2,544.79
   RMSE: $5,669.32
   R²: 0.9333

📈 Overfitting Analysis:
   WMAE difference: 46.9%
   ⚠️ Moderate overfitting detected

🎯 Top 10 Feature Importance:
    1. Weekly_Sales_lag_1       : 0.6347
    2. Weekly_Sales_lag_2       : 0.2868
    3. Weekly_Sales_lag_4       : 0.0345
    4. Month                    : 0.0072
    5. DaysFromStart            : 0.0056
    6. Dept                     : 0.0054
    7. Type_A                   : 0.0048
    8. Type_B                   : 0.0034
    9. Size                     : 0.0028

ნუ ეხა რეალურს გავს მაგრამ ოვერფიტში ვარ აშკარად 1000$ მაქ განსხვავება.

კაი ეხა მოდი ვცდი facebook prophet-ს აქვე xgboost-ის შემდეგ.

კაი ამას უფრო მეტი დრო მოუნდს ვიდრე xgboost-ს

ა ხო საინტერესოა და რპოგორც ანს აქ ტითოეული დეპარტამენტი- store კომბინაციისთვის გვიწევს დატრეინინგება ანუ ის ესამფშენი რო ვთქვი
ადრე რო ეგრე შეილება კაი იყოს რო ცალ ცალკე განვიხილოო და მერე შევაერთო თქო აქ ეგ მიდგომა 
გვიწევს ისედაც. ამიტო მინიმუმ ეგ ლოგიკა მაინც აღმოვაჩიე რომ სწორია.

ნუ რაღაცა პრობელმა ქონდა 20 წუთზე დიდი ხანი იყო და ამიტო ჯობია სხვა დროს დავატრეინინგო როცა სწორად მექნება გაკეთებული. 


# experiment_5_future_engineering.ipynb

ამ ექსპერიმენტში, რადგან უკვე გავტესტე სხვადასხვა მიდგომები და ვნახე როგორია დატა. მოკლედ ვიზავ პრეპროცესინგის მთლიან ფაიფლაინს.
დატას გავყოფ ორად ტრეინად და ვალიდაციად, 80-20 ზე რადგან ტესტსეტზეც ეგრე ტრაინთან შედარებით 4 ჯერ პატარაა.
### ამის მერე უკვე ვაპირებ ასეთ პრეპროცესინგს. ნუ როგორც experiemt 2 ში ვქენი ეგრე ვიზავ მხოლოდ ტრეინსეტზე + დავამატებ მეოთხეში როგორც ვქენი მაგეებს ანუ დავმერჯავ მაგრამ markdown- ებს არ გამოვიყენებ.
ასევე გავწმინდავ დატას.


როგორც ვთქვი ჯერ დავალოადე დატა, შემდეგ ამოვიღე 'MarkDown1', 'MarkDown2', 'MarkDown3', 'MarkDown4', 'MarkDown5',
შემდეგ დავმერჯე Final merged dataset: (421570, 11)

შემდეგ გავყავი დატა. ოღონდ პარასკევი ავიღე დაყოფის წერტილად და ისე.
📅 Split date: 2012-04-13 00:00:00
🚂 Training: 335,761 records (79.6%)
🔮 Validation: 85,809 records (20.4%)
📊 Training date range: 2010-02-05 00:00:00 to 2012-04-06 00:00:00
📊 Validation date range: 2012-04-13 00:00:00 to 2012-10-26 00:00:00

შემდეგ უკვე ვზრუნავთ აუთლაიერებზე ოღონდ მხოლოდ ტრეინ სეტიდან თავისთავად.

მარა მგონი ჯობდა პირდაპირ დიდ ფაიფლაინში მქონოდა ეს ამიტომ ახლიდან ვიზავ ამას დიდ ფაიფლაინში- WalmartFeatureEngineer.

ასევე როგორც ვთქვი გვექნება: date_features, lag_features, rolling_features, store_dept_features.
ნუ ცალკე მაქ კიდე WalmartFeaturePipeline სადაც ვახდენ აუთლაიერების მოშორებასაც და შენდეგ FeatureEngineer.

მაქ ასევე calculate_wmae და evaluate_model_performance ფუნქციები.

https://dagshub.com/konstantine25b/Walmart-Recruiting---Store-Sales-Forecasting.mlflow/#/experiments/4?searchFilter=&orderByKey=attributes.start_time&orderByAsc=false&startTime=ALL&lifecycleFilter=Active&modelVersionFilter=All+Runs&datasetsFilter=W10%3D
- ეს ნელ-ნელა

და როცა მთლიან ფაიფლაინზე გადავედი ეს: 
https://dagshub.com/konstantine25b/Walmart-Recruiting---Store-Sales-Forecasting.mlflow/#/experiments/14?searchFilter=&orderByKey=attributes.start_time&orderByAsc=false&startTime=ALL&lifecycleFilter=Active&modelVersionFilter=All+Runs&datasetsFilter=W10%3D

მოკლედ, კვლავ აღმოჩნდა ოვერფიტიც და მოკლედ უნდა შევცვალო მიდგონა.
🚂 TRAINING PERFORMANCE:
   WMAE: $72.59
   MAE:  $72.65
   R²:   1.0000

🔮 VALIDATION PERFORMANCE:
   WMAE: $134.98
   MAE:  $132.71
   R²:   0.9992


ვიფიქრე რომ ეგრევე გავაერთიაბნებ თქომეორე ექსპერიმენტს და მეოთხეს თქო მაგრამ ძალიან დიდი დრო დავხარჯე ტყუილად ავაგე რაღაც უზარმაზარი ფაიფლაინი რომელიც საბოლოოდ ისე მოხდა რომ სწორად ვერ მუშაობს. ამიტომ ვცვლი მიდგომას და step-bystep ვიზავ.

ნუ ექსპერიმენტი ყოველი შემთხვევისთვის მაინც დავასეივე.

# experiment_6_future_engineering.ipynb

აქ ვაპირებ რომ მეორე ექსპერიმენტი და მეოთხეში ნასწავლი რაღაცეები შევაერთო და მივიხო პრეპროცესინგის კარგი ვარიანტი.

მოდი ჯერ პირდაპირ გავაკეთოთ ფუნქცია რომელიც დატას დაამუშავებს ზუსტად ისე როგორც მეორე ექსპერიმენტში. ჯერ გაყოფს 80-20ზე და შემდეგ დაამუშავებს.

Final columns: ['Store', 'Dept', 'Date', 'Weekly_Sales', 'Type', 'Size', 'Temperature', 'Fuel_Price', 'CPI', 'Unemployment', 'IsHoliday', 'Month', 'DayOfWeek', 'IsWeekend', 'IsMonthStart', 'IsMonthEnd', 'WeeksFromStart']
- ესენია ჩვენი მეორე ექსპერიმენტის საბოლოო ქოლუმები.
train_data, val_data, split_info = experiment_2_pipeline()

ახლანდელ დატას გავუკეთე ანალიზი და გვაქ ასეთი:
გამოგვივივიდა 16 Features.

📊 STARTING DATA ANALYSIS
==================================================
📋 BASIC DATASET INFORMATION
Training shape: (335453, 17)
Validation shape: (85809, 17)
Split date: 2012-04-13 00:00:00

🎯 TARGET VARIABLE ANALYSIS (Weekly_Sales)
   Mean: $15,873.74
   Median: $7,636.72
   Std: $22,262.57
   Range: $-4,988.94 to $693,099.36
   Skewness: 3.178
   Kurtosis: 22.166

🏪 STORE ANALYSIS
Store Type Statistics:
     Weekly_Sales                         Size             Store    Dept
             mean       std   count       mean       std nunique nunique
Type                                                                    
A        19950.28  25866.49  171570  182250.42  41479.05      22      81
B        12153.91  16852.17  130169  101822.50  30924.44      17      80
C         9490.49  15851.93   33714   40543.11   1198.38       6      66

📅 TEMPORAL ANALYSIS
Temporal Patterns:
   Best month: 12
   Worst month: 1
   Weekend boost: N/A (insufficient data)

🔗 FEATURE CORRELATION ANALYSIS
Strongest correlations with Weekly_Sales:
   Size: 0.246
   Month: 0.030
   Unemployment: 0.024
   CPI: 0.021
   IsMonthEnd: 0.011
   IsMonthStart: 0.006
   Fuel_Price: 0.004

❓ MISSING VALUES ANALYSIS
Missing values summary:
Empty DataFrame
Columns: [Training, Validation, Train_%, Val_%]
Index: []

✅ DATA QUALITY SUMMARY
   📊 Training samples: 335,453
   📊 Validation samples: 85,809
   🔗 Features: 16 (excluding target)
   ❓ Missing values: 0 in training, 0 in validation
   🎯 Target range: $-4,988.94 to $693,099.36
   🏪 Store types: 3 (A, B, C)
   📅 Time span: 791 days

📊 DATA ANALYSIS COMPLETED!
🔗 All plots and metrics logged to MLflow
==================================================

type-სთვის one hot encoderi გამომრჩა და ჩავამატე. ანუ გვაქ 19 col.

აი აქ არი ექსპერიმენტი.
https://dagshub.com/konstantine25b/Walmart-Recruiting---Store-Sales-Forecasting.mlflow/#/experiments/15?searchFilter=&orderByKey=attributes.start_time&orderByAsc=false&startTime=ALL&lifecycleFilter=Active&modelVersionFilter=All+Runs&datasetsFilter=W10%3D

ახლა კიდევ როგორც მეოთხე ექსპერიმენტში დავამატებ ლაგინგ და ჰოლიდეი ფიჩერებს. არ ვამატებ როლინგ და ewm იმიტომ რომ ეგენი რაღაც ცუდად მოქმედებენ და მირჩევნია სანამ უკეთესად გავერკვევი ესენი მქონდეს მხოლოდ.

'IsSuperBowlWeek', 'IsLaborDayWeek', 'IsMajorHoliday', 'Weekly_Sales_lag_2', 'Weekly_Sales_lag_8', 'Weekly_Sales_lag_4', 'IsChristmasWeek', 'IsBackToSchool', 'Weekly_Sales_lag_3', 'IsHolidayMonth', 'IsThanksgivingWeek', 'Weekly_Sales_lag_1', 'Weekly_Sales_lag_12'

კაი კი ახლა ავაწყოთ საბოლოო ფაიფლაინი პრეპროცესინგის.
ვააა ლაგინგ feature-ებს მაინც ისე weekly_sale-ების გამოყენებთ აწყობდა. ამიტომ ეგ იყო მთავარი მინუსი.
გავასწორე lag ფიჩერებისთვის მხოლოდ training data-ს ვიყენებ. ამითი მასკინგს ვაკეთებ და ისე ვაკეთებ.
['IsSuperBowlWeek',
  'Temperature',
  'IsLaborDayWeek',
  'WeeksFromStart',
  'Dept',
  'IsMajorHoliday',
  'Weekly_Sales_lag_2',
  'Weekly_Sales_lag_8',
  'Type_B',
  'Weekly_Sales_lag_4',
  'IsChristmasWeek',
  'Type_A',
  'IsMonthEnd',
  'IsWeekend',
  'Type_C',
  'IsBackToSchool',
  'Month',
  'Weekly_Sales_lag_3',
  'Store',
  'IsHolidayMonth',
  'IsThanksgivingWeek',
  'Unemployment',
  'IsHoliday',
  'Fuel_Price',
  'CPI',
  'Size',
  'Weekly_Sales_lag_1',
  'DayOfWeek',
  'Weekly_Sales_lag_12',
  'IsMonthStart']

ეხა მოდი უკვე შექმნილი ფაიფლაინი გავტესტოთ ამიტომ შევქმნათ მე-7 ექსპერიმენტი.

# experiment_7_xgboost.ipynb

მოკლედ ვეჩალიჩე მაგრამ INTERNAL_ERROR: Response: {'error': 'unsupported endpoint, please contact support@dagshub.com'}
და ამის გამო ვერ ჩამოვტვირთე ჩვენი არტიფაქტი ამიტომ მომიწევს მე-7 ექსპერიმენტშ უბრაოგ გადავაკოპო კოდი.


https://dagshub.com/konstantine25b/Walmart-Recruiting---Store-Sales-Forecasting.mlflow/#/experiments/25?searchFilter=&orderByKey=attributes.start_time&orderByAsc=false&startTime=ALL&lifecycleFilter=Active&modelVersionFilter=All+Runs&datasetsFilter=W10%3D

მოკლედ აქ აღმოვაჩინე რომ ტესტ სეტში ამატებდა type ქოლუმს სადაც nan-ები ეწერა ამიტო ეგ  დავფიქსე.
ამის შემდეგ გავუშვი დაპრეპროცესებულ დატაზე xgboost.

მოკლედ რაღაც პრობლემები აქვს ამ კლასს, ხოდა ამიტომ ნელ ნლე ვცდილობ გავასწორო
მოკლედ ყვლეა ჯერზე ახალი ექსპერიმენტი რო არ ვაკეთო ვშლი და თავიდან ნელ ნელა ვასწორებ
1. 📊 TRAINING METRICS:
   WMAE: 1503.79
   RMSE: 1493.15
   MAE: 872.81
   R²: 0.9796

📊 VALIDATION METRICS:
   WMAE: 16774.31 ⭐
   RMSE: 11650.46
   MAE: 4457.34
   R²: 0.7182

2. 📊 TRAINING METRICS:
   WMAE: 1223.01
   RMSE: 2129.43
   MAE: 1128.36
   R²: 0.9585

📊 VALIDATION METRICS:
   WMAE: 4593.32 ⭐
   RMSE: 12112.20
   MAE: 4505.92
   R²: 0.6954

ნელ-ნელა ვასწორებ თან კოდს და უმჯობესდება მაგრამ მაინც ოვერფიტში ვართ.
🔝 TOP 10 MOST IMPORTANT FEATURES:
   25. Weekly_Sales_lag_1        0.3655
   26. Weekly_Sales_lag_2        0.2280
   27. Weekly_Sales_lag_3        0.1263
   29. Weekly_Sales_lag_8        0.0997
   28. Weekly_Sales_lag_4        0.0551
   22. Type_A                    0.0308
   23. Type_B                    0.0262
   18. IsChristmasWeek           0.0139
   30. Weekly_Sales_lag_12       0.0068
    9. Month                     0.0063

მოკლედ ეს Weekly_Sales_lag ამათ აქვთ ყველაზე დიდი იმფორთანსი არ ვიცი კიდე ვცდი გაუმჯობესებას და ნუ თუ გამოვა კაია თუ არადა ამ ლაგებს დავანებებ თავს და მაგის გარეშე ვიჩალჩებ ჯობია. იმიტომ რომ მთელი დღე წაიღო მაგათზე წვალებამ.

მოკლედ ვეწვალე და ეს lag ფიჩერები არის ძალიან კომპლექსური და მათი იმპლემენტაციამ ისეთი პრობლემები გამოიწვია რო ძალიან დიდი დრო წაიღო. ამიტომ მოდიმაგათ გარეშე გავაკეთოთ. ანუ არ გვინდა ეს ლაგგინგ ფიჩერები.

'n_estimators': 300,    
        'max_depth': 4,           
        'learning_rate': 0.05,   
        'subsample': 0.7,         
        'colsample_bytree': 0.7,  
        'min_child_weight': 3,    
        'gamma': 0.1,             
        'reg_alpha': 0.1,      
        'reg_lambda': 1.0,     
        'random_state': 42,

📊 TRAINING METRICS:
   WMAE: 3306.82
   RMSE: 4794.32
   MAE: 3256.20
   R²: 0.7899

📊 VALIDATION METRICS:
   WMAE: 7479.42 ⭐
   RMSE: 15933.22
   MAE: 7427.52
   R²: 0.4729

რო მოვაშორე ლაგინგ ფიჩერები ბევრად გაუმჯობესდა შედეგი ნუ საშინელი შედეგია მაგრამ იმას ჯობია.

Training XGBoost model...
   📋 Parameters: {'n_estimators': 100, 'max_depth': 3, 'learning_rate': 0.03, 'subsample': 0.6, 'colsample_bytree': 0.6, 'min_child_weight': 5, 'gamma': 0.2, 'reg_alpha': 0.2, 'reg_lambda': 2.0, 

📊 TRAINING METRICS:
   WMAE: 7470.71
   RMSE: 9719.44
   MAE: 7428.61
   R²: 0.1365

📊 VALIDATION METRICS:
   WMAE: 13045.05 ⭐
   RMSE: 22284.83
   MAE: 12972.71
   R²: -0.0310

   გავაუარესე:(

Parameters: {'n_estimators': 200, 'max_depth': 8, 'learning_rate': 0.05, 'subsample': 0.8, 'colsample_bytree': 0.8, 'colsample_bylevel': 0.8, 'min_child_weight': 3, 'gamma': 0.1, 'reg_alpha': 0.1, 'reg_lambda': 1.0,

📊 TRAINING METRICS:
   WMAE: 1824.73
   RMSE: 2826.92
   MAE: 1799.88
   R²: 0.9269

📊 VALIDATION METRICS:
   WMAE: 5898.46 ⭐
   RMSE: 14480.74
   MAE: 5845.00
   R²: 0.5647

ესე უკეთესია მარა კვლავ ოვერფიტში ვარ.

კაი მოკლედ მეორე ექსპერიმენტს ვუყურებ და მანდ უკეთესი შედეგი გვაქ ბევრად ხოდა მანდ ასევე სხვა ფიჩერები მაქ ამიტომ მაქ ასეთი იდეა რო მანდ რა ფიჩერებიც მაქ განსხვავებულები ეგენი ჩავამატო და ისე გავტესტო.
https://dagshub.com/konstantine25b/Walmart-Recruiting---Store-Sales-Forecasting.mlflow/#/experiments/33

📊 TRAINING METRICS:
   WMAE: 1822.30
   RMSE: 2806.07
   MAE: 1790.54
   R²: 0.9280

📊 VALIDATION METRICS:
   WMAE: 5971.66 ⭐
   RMSE: 14599.85
   MAE: 5918.78
   R²: 0.5575


['Store', 'Dept', 'Size', 'Temperature', 'Fuel_Price', 'CPI', 'Unemployment', 'IsHoliday', 'Month', 'DayOfWeek', 'IsWeekend', 'IsMonthStart', 'IsMonthEnd', 'WeeksFromStart', 'IsSuperBowlWeek', 'IsLaborDayWeek', 'IsThanksgivingWeek', 'IsChristmasWeek', 'IsMajorHoliday', 'IsHolidayMonth', 'IsBackToSchool', 'Type_Encoded', 'Type_A', 'Type_B', 'Type_C']

აი ეს ფიჩერები გამომივიდა საბოლოო ჯამში.
ნუ ამ ექსპერიმენტისთვის საკმარისია.


# experiment_8_k.ipynb

მოკლედ ბევრი გზა განვიხილე მაგრამ რაღაც არ მაკმაყოფილებს მინდა რომ ერთხელ კიდევ გავიაზრო დატა და აღმოვაჩინო კანონზომიერებები.

ამ ექსპერიმენტში მივყვები ყველაფერს და გამოვიყენებ markdown-ებსად და lagging ფიცერებსაც. ჯერ lightgbm-ს დავატრეინინგებ:

 MODEL EVALUATION
==================================================
LightGBM WMAE: 2,418.70

ამის მერე xgboost

🔍 MODEL COMPARISON:
XGBoost WMAE: 2,491.51
LightGBM WMAE: 2,418.70
🏆 LightGBM wins by +2.9%

ნუ მეტი არაფერი გავტესტე უბრალოდ რაღაცეები აქ.


# experiment_9_prophet

მოკლედ ეხა უკვე დავიწყებ გატესტვას სხვადასხვა მოდელებით.

საბოლოოდ ავიღებ ისევ მე-7 ექსპერიმენტში მიღებულფიჩერებს და დატას დავამუშავებ ეგრე 

['Store', 'Dept', 'Size', 'Temperature', 'Fuel_Price', 'CPI', 'Unemployment', 'IsHoliday', 'Month', 'DayOfWeek', 'IsWeekend', 'IsMonthStart', 'IsMonthEnd', 'WeeksFromStart', 'IsSuperBowlWeek', 'IsLaborDayWeek', 'IsThanksgivingWeek', 'IsChristmasWeek', 'IsMajorHoliday', 'IsHolidayMonth', 'IsBackToSchool', 'Type_Encoded', 'Type_A', 'Type_B', 'Type_C']

https://dagshub.com/konstantine25b/Walmart-Recruiting---Store-Sales-Forecasting.mlflow/#/experiments/35?searchFilter=&orderByKey=attributes.start_time&orderByAsc=false&startTime=ALL&lifecycleFilter=Active&modelVersionFilter=All+Runs&datasetsFilter=W10%3D

ამ ექსპერიმენტშ ვოყენებ prophet-ს. ვნახოთ რა იქნება შედეგი:

დიდი დრო კი მიაქვს ტრეინინგს 25 წუთი 

https://dagshub.com/konstantine25b/Walmart-Recruiting---Store-Sales-Forecasting.mlflow/#/experiments/36

კაია ეს კაი მოდელი გამოვიდა:

🎯 Successful models: 3167
   ❌ Failed models: 146

📊 Validation Metrics:
   WMAE (Competition Metric): $1,871.08
   MAE: $1,819.20
   RMSE: $3,786.26
   R²: 0.9702


# experiment_10_ARIMA

ეხა უკვე იგივე ნაირი ფიჩერ ინჯინეერინგით გავტესტოთ არიმას მოდელი.

https://dagshub.com/konstantine25b/Walmart-Recruiting---Store-Sales-Forecasting.mlflow/#/experiments/37?searchFilter=&orderByKey=attributes.start_time&orderByAsc=false&startTime=ALL&lifecycleFilter=Active&modelVersionFilter=All+Runs&datasetsFilter=W10%3D

ამას 19 წუთი დაჭირდა მარა გავიდა ბოლოში როგორც იქნა

📊 Validation Metrics:
   WMAE (Competition Metric): $2,589.86
   MAE: $2,546.16
   RMSE: $5,267.98
   R²: 0.9424

ესენიც მშვენივრად გამოიყურება კარგი ფრედიქშენებია


# Experiment_11_sarimax.ipynb

კაი ახლა გავაკეთოთ სარიმაქსით რომელიც არიმას უმატებს სეზონურობას და წესით უკეთესი უნდა იყოს ვიდრე არიმას მოდელი.

ხო აქ sarimax გვაქ და არა sarima , ანუ დაგვემატა eXogenous variables (marketing spend, holidays, weather) ანუ უფრო კარგია როცა 
ჰოლიდეი ან აი ისეთი დატასეტი გვაქ სადაც დღე როგორია მაგას აქვს მნიშვნელობა. 

📊 Training shape: (292063, 25)
   📊 Validation shape: (84314, 25)
   🎯 Features: 25

პრეპროცესინგის შემდეგ დატა ასე გამოიუყურება ხოლმე.

https://dagshub.com/konstantine25b/Walmart-Recruiting---Store-Sales-Forecasting.mlflow/#/experiments/43?searchFilter=&orderByKey=attributes.start_time&orderByAsc=false&startTime=ALL&lifecycleFilter=Active&modelVersionFilter=All+Runs&datasetsFilter=W10%3D

რამდენიმე ვარიანტი გავტესტე მაგრამ ძალიან დიდი დრო მიქონდა ამიტო გავამარტივე და საბოლოოდ ეს შედეგია:
📊 Validation Metrics:
   WMAE (Competition Metric): $3,227.31
   MAE: $3,178.40
   RMSE: $7,723.57
   R²: 0.8762

ნუ არიმაზე უარესია იმიტომ რომ ბევრად მალე დატრეინინგდა იმიტომ რომ შედარებით რთული დროში ძალიან იწელებოდა.


# Experiment 12 timesfm

კაი მოდი კიდევ ერთი გავტესტოთ timesfm - ნუე ეს ტრანსფორმეტ based მოდელია მაგრამ მაინტერესებს ეხა.
როგორც გავერკვიე გუგლის გაკეთებულია და ძალიან დიდ თაიმ სერიეს დატაზეა დატრეინინგებული ეს მოდელი.

პრე ტრეინდ რადგან არის შეიძლება უკეთესი შედეგი დადოს ვიდრე ახლანდელი ალგორითმები დებენ.

https://dagshub.com/konstantine25b/Walmart-Recruiting---Store-Sales-Forecasting.mlflow/#/experiments/44

📊 Validation Metrics:
   WMAE (Competition Metric): $1,985.27
   MAE: $1,976.18
   RMSE: $4,068.65
   R²: 0.9131

ეს იყო მარტივი ვარიანტის შედეგი 

უფრო გავაკომპლექსურე და ეს არის უფრო კომპლექსურის შედეგი

მოდი ისე ვიზავ რომ ამავე notebook-ში იყოს კომპლექსური მოდელიც ბარემ.
მოკლედ უკეთესი რომ გამხდარიყო ვქენი შემდეგი რამ:

ვიყენებ 3 მოდელს , ლონგ, ჩვეულებრივ და შორთს რომლებიც გულისხმობს თუ რამხელა კონტექსტის ზომა აქვსთ :

# Standard model (balanced)
context_len=128, horizon_len=64

# Long context model (for complex patterns) 
context_len=256, horizon_len=32

# Short horizon model (immediate predictions)
context_len=64, horizon_len=16

ამასთან ერთად ვატრეინინგებ ამ სამივე მოდელს და შემდეგ ენსემბლ პრინციპით ვაჯამებ
model_weights = {
    'standard': 1.0,     
    'long_context': 0.9,  
    'short_horizon': 0.8
}
აქვთ თავისი წონებიც.

https://dagshub.com/konstantine25b/Walmart-Recruiting---Store-Sales-Forecasting.mlflow/#/experiments/44

📊 Validation Metrics:
   WMAE (Competition Metric): $2,535.07
   MAE: $2,480.96
   RMSE: $4,173.24
   R²: 0.7989


# Experiment 13 N-Beats

კაი გადავედით deep learning-ის სხვადასხვა ალგორითმის გატესტვაზე. 
ნუ წერია რო ვენიშინგ გრადიენტებს ერიდება და ასევე მარტივიაო დატრეინინგებაო.

ვნახოთ აბა რა შედეგს დადებს.

🚀 Training N-BEATS model...
   Epoch   0: Train Loss = 0.822043, Val Loss = 0.714339
   Epoch  20: Train Loss = 0.047840, Val Loss = 0.678586
   Epoch  40: Train Loss = 0.058264, Val Loss = 0.681264
   Epoch  60: Train Loss = 0.007083, Val Loss = 0.662860
   Epoch  80: Train Loss = 0.000364, Val Loss = 0.655802

✅ Evaluation complete!
   🎯 WMAE: $1,399.42
   📊 MAE: $1,369.29
   📊 RMSE: $2,581.87
   📊 R²: 0.9240

ეს კიდე იყო შედეგი, ნუ კაი შედეგია ჩემი აზრით. მაგრამ მაინც ოვერფიტშ წავიდა.

https://wandb.ai/konstantine25b-free-university-of-tbilisi-/walmart-nbeats-forecasting/runs/wdkyvyvq



# Experiment 14 DLearner

კაი ეხა გავტესტოთ სხვა ალგორითმი DLearner

როგორც მოვიძიე ეს მოდელი კარგია long-horizon univariate forecasting-ისთვის. 
აქ nbeats-სთან შედარებით სეზონურობას ვიყენებთ უკეთესად ასევე წერია რომ ესეც მარტივად დასატრეინინგებელი მოდელია საკმაოდ.

https://wandb.ai/konstantine25b-free-university-of-tbilisi-/walmart-dlinear-forecasting

📈 Training Results:
   🎯 Train WMAE: $1,100.11
   📊 Train MAE: $1,096.35
   📊 Train R²: 0.9604

📉 Validation Results:
   🎯 Val WMAE: $1,424.41
   📊 Val MAE: $1,378.75
   📊 Val R²: 0.9275

ამანაც საკამოდ კარგად იმუშავა. ბევრად კაი შედეგია საინტერესოა იქ რეალურ ტესტსეტზე რა იქნება. არ ვართ ოვერფიტში. ანუ ოდნავ ვართ მაგრამ კაია სხვა მოდელებთან შედარებით.


# Experiment 15 PatchTST

კაი ეხა გავტესტოთ სხვა მოდელი PatchTST

Divides the time series into non-overlapping patches (e.g., chunks of 16 or 32 timesteps)

Flattens + projects those patches into embeddings

Feeds these embeddings into a standard Transformer encoder

ნუ ეს ტრანსფორმერ მოდელია რაც საკმაოდ საინტერესოა გასატესტად.
წერია რომ შედარებით რთული გასატესტიაო.

https://wandb.ai/konstantine25b-free-university-of-tbilisi-/walmart-patchtst-forecasting

📈 Training Results:
   🎯 Train WMAE: $139.22
   📊 Train MAE: $145.96
   📊 Train R²: 0.9992

📉 Validation Results:
   🎯 Val WMAE: $1,468.55
   📊 Val MAE: $1,460.37
   📊 Val R²: 0.9060

აქ ძალიან ოვერფიტში ვართ შესაბამისად არ გამოდგა ეს მოდელი ჩვენი დატასეტისთვის კარგი.

# Experimetn 16 Temporal Fusion Transformer

Temporal Fusion Transformer  - google მა გააკეტა 2020-შიო. sequence-to-sequence მოდელიაო.

ფიჩერების იმფორთანსის გაგება შეუძლიაო კარგად. ამათი ჯამია LSTM + Attention + Feature Select. 

შესაბამისად შესაძლებელია საკმაოდ კარგი იყოს მაგრამ ჩემი აზრით timesfm-ს ვერ აჯობებს. 

https://wandb.ai/konstantine25b-free-university-of-tbilisi-/walmart-tft-forecasting

📈 Training Results:
   🎯 Train WMAE: $1,483.92
   📊 Train MAE: $1,433.58
   📊 Train R²: 0.9235

📉 Validation Results:
   🎯 Val WMAE: $1,536.30
   📊 Val MAE: $1,487.36
   📊 Val R²: 0.9000

ხოო აქ ოვერფიტში ნამდვილა არ არის და საკმაოდ მომეწონა ეს შედეგი.

