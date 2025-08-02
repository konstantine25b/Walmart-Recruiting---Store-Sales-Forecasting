# Walmart-Recruiting---Store-Sales-Forecasting

ჯგუფი: 
კონსტანინე ბახუტაშვილი,
ლუკა ლოდია.

Description
One challenge of modeling retail data is the need to make decisions based on limited history. If Christmas comes but once a year, so does the chance to see how strategic decisions impacted the bottom line.

Markdowns

In this recruiting competition, job-seekers are provided with historical sales data for 45 Walmart stores located in different regions. Each store contains many departments, and participants must project the sales for each department in each store. To add to the challenge, selected holiday markdown events are included in the dataset. These markdowns are known to affect sales, but it is challenging to predict which departments are affected and the extent of the impact.

Want to work in a great environment with some of the world's largest data sets? This is a chance to display your modeling mettle to the Walmart hiring teams.

This competition counts towards rankings & achievements.  If you wish to be considered for an interview at Walmart, check the box "Allow host to contact me" when you make your first entry. 

You must compete as an individual in recruiting competitions. You may only use the provided data to make your predictions.

Evaluation
This competition is evaluated on the weighted mean absolute error (WMAE):

ინსტრუქციიდან გამომდინარე მოგვიწევს მუშაობა ვოლმარტის ქომფეთიშენის დატაზე 45 სხვადასხვა მაღაზია გვექნება, სხვადასხვა დეპარტამენტებით. ამბობენ რომ ძალიან დიდ დატასეტია, რაც კარგია მე როგორც გავერკვიე თავისთავდ Time series-ში. ნუ ყველგან კაია მაგრამ 
კონკრეტულად Time Series-ში დიდი მნიშვნელობა აქვს ამას. ასევე შეფასებისთვის გვაქვს: WMAE, ნუ ზოგადად MAE-ზე ვიცი მაგრამ ალბათ ამ შემთხვევაში ეს იქნება უპირატესი. ალბათ იმიტომ რომ დეტარტამენტების ზომები სხვადასხვა იქნება და ამიტომ არის weighted.

მოკლედ ესეთი გეგმა გვაქ, შევქმნით ცალ ცალკე Readme-ებს. და მერე ნელ ნელა დავიწყებთ ცალ ცალკე ბრანჩებზე მუშაობას და შიგა და შიგ ჩვენ კვლევებს შევაერთებთ და დავმერჯავთ ხოლმე.

იდეაში შეგვლია დავიწყოთ. 

# დავიწყოთ


მოკლედ როგორც ვთქვი ისე დავიწყეთ მუშაობა. გავაკეთედ ორი ცალი REEAME kote და lodia. თითოში ნელ-ნელა მივყვებით და ჩვენ ჩვენ ექსპერიმენტებს ვწერთ. 

# მაგ ფაილებში წერია დიდი ვრცელი აღწერები თითოეული ექსპერიმენტის. აქ კიდე თითოეული ექსპერიმენტის შედარებით მოკლე აღწერებს დავწერ.

# experiment_1_k.ipynb

ამ ექსპერიმენტში, ჯერ გავერკვიეთ დატასეტის სტრუქტურაში. განვიხილეთ მთლიანად როგორ გამოიყურება ის.
შევხედეთ როგორი განსხვავებაა holiday და not-holiday-ებს შორის. მათი გაყიდვების პროცენტული სხვაობა ( 7%)

https://dagshub.com/konstantine25b/Walmart-Recruiting---Store-Sales-Forecasting.mlflow/#/experiments/0?searchFilter=&orderByKey=attributes.start_time&orderByAsc=false&startTime=ALL&lifecycleFilter=Active&modelVersionFilter=All+Runs&datasetsFilter=W10%3D

მანდ მხოლოდ გამოვიყენე train.csv სიმარტივისთვის. 
train.csv გაყოფილი მქონდა 3 ნაწილად.
არ მქონია განსაკუთრებული ფიჩერები მხოლოდ თარიღისგან მიღებული.
დავატრეინინგე xgboost.

📊 FINAL WMAE RESULTS:
   Competition Metric (WMAE): $4,110.79
   Regular MAE: $3,966.39
   Holiday weeks MAE: $4,619.24
   Normal weeks MAE: $3,916.50

ეს იყო საბოლოო შედეგი.

# experiment_2_future_engineering_k.ipynb

აქ შედარებით ვცადე სხვადასხვა დატასეტების ჩართვა და მათი გამოყენებით ფიჩერების გაკეთება.

year, month, day, dayOfweek,weekofyear,quarter, IsWeekend,IsMonthStart,IsMonthEnd,IsQuarterStart,IsQuarterEnd,
DaysFromStart, WeeksFromStart -ესენი როგორც გავარკვიე კაიაო linear trend-ის დასაჭერადო.
ამასთან ერთად დავამატე ფიჩერების და სტორის სვეტები და დავმერჯე ერთმანეთთან.

MarkDown2	MarkDown2	310322	73.611025	float64
MarkDown4	MarkDown4	286603	67.984676	float64
MarkDown3	MarkDown3	284479	67.480845	float64
MarkDown1	MarkDown1	270889	64.257181	float64
MarkDown5	MarkDown5	270138	64.079038	float64 

მხოლოდ ამათში აღმოვაჩინე რომ გვქონდა nan-ები დანარჩენი სუფთაა. ამიტომ რახან მეორე ექსპერიმენტია უბრალოდ გადავყარე ესენი.
ამის შემდეგ ავდექი და აუთლაიერები ამოვიღე. 
თითოეულში იყო აუთლაიერები და გავწმინდე.
 Outliers removed: 339 records (0.08%)

 საბოლოოდ გვქონდა 24 column .
 კორელაციებზეც შევამოწმე შემდეგ და მოვიღე 80% ის მქონეები.

 შემდეგ კატეგორიულები ავდექოთ და გადავეაკეთეთ one-hot ით ნუმერიკალებში და გამოგვივიდა 19 ქოლუმი.
 გავყავი 80.10.10

 ასევე ტრეინინგისთვის მხოლოდ ტრეინ და ვალ
 და ტესტისთვის ტრაინ + ვალ და ტესტ ცალკე იმიტომ რომ დროში გეპი არ გვქონოდა.
 ეს კიდე ბევრად უკეთესი შედეგი.
 FINAL TESTING RESULTS:
   Training (Train+Val): MAE=$2,896.51 | RMSE=$5,242.54 | WMAE=$3,045.81
   Testing  (Test only): MAE=$2,968.85 | RMSE=$4,959.53 | WMAE=$3,049.17


# experiment_3_k.ipynb

ამის მერე ვიფიქრე ასეთი რამე:
future დატასეტის გარეშე მხოლოდ stores, და train რომ გამოვიყენო 
ოღონდ მივიღო რაც შეილება ბევრი და კარგი ფიჩერი დეიტებიდან რა შედეგს დავდებ. 
თან ლაგინგ ფიჩერებს გამოვიყენებ ანუ ძველი დატას გავლენა რომ ქონდეს რა ეგეთს.

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

დატა დავამუშავე როგორც მეორეში. ნუ ცოტა სხვანაირად მარა ნუ ექსპერიმენტშ ჩანს.

მივიღე საბოლოოდ : 71 ფიჩერი.
 🎯 KEY METRICS (Validation Set):
     WMAE (Walmart Competition Metric): $94.20
     MAE: $91.00
     RMSE: $191.05
     R²: 0.9998

რაც რადიკალურად ცუდი აღმოჩნდა.
იმიტომ რომ აშკარად რაღაც არასწორი იყო იმიტომ რომ პრაქტიკულკად ძალიან დიდი სიზუსტე აქვს რაც არარეალური არის. ამიტომ გავაკეთე შემდეგი ექსპერიმენტი.

# experiment_4_k.ipynb

აქ ვეცადე მესამეში დაშვებული შეცდომა გამომესწორებინა.
და აღმოვაჩინე რო ფრედიქშენში 
  1. Weekly_Sales_ewm_0.5               : 0.7531
    2. Weekly_Sales_ewm_0.3               : 0.2101

    ამათ აქცევდა ძალიან დიდ ყურადღებას.

    გამოვიდა რომ მთლიან დატასეტზე ითვლიდა ამათ და მაგაში იყო პრობლემა.

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

   აქ ნუ ეგ გავასწორე მარა მაინც ოვერფიტში მიდის. ამიტომ კვლავ რაღაც პრობლემები გვაქ.


ამის შემდეგ უკვე ლუკამ დაიწყო გატესტვა თავისი ექპერიმენტების 
# model_exp_RandomForest_1.ipynb

ჩვენი საწყისი მიდგომა გულისხმობს Random Forest რეგრესორის გამოყენებას.
თავდაპირველად იტვირთება train.csv, features.csv და stores.csv ფაილები და ერთიანდება train_merged DataFrame-ში. 
Date სვეტიდან ამოღებულია დროზე დამოკიდებული მახასიათებლები.
ხდება უარყოფითი გაყიდვების დამუშავება და გამოტოვებული მნიშვნელობების დამუშავება (MarkDowns).
Type (მაღაზიის ტიპი A, B, C) სვეტი გარდაიქმნება One-Hot Encoding-ის გამოყენებით.
train_megred დაიყო სამ-ნაწილად, 60-20-20.
გამოიყენება n_estimators=100, random_state=42, n_jobs=-1.
Evaluating on Validation Set... Validation WMAE: 3035.2597 Validation MAE: 2816.7839
Evaluating on Local Test Set... Local Test WMAE: 2564.8268 Local Test MAE: 2543.5857


# model_exp_RandomForest_2.ipynb

შედეგის გაუმჯობესების მიზნით მოვახდინე CPI-სა და Unemployment-ში გამოტოვებული მნიშვნელობების დამუშავება.მონაცემები ჯგუფდება Store-ის მიხედვით, შემდეგ გამოიყენება ffill() (წინა მნიშვნელობით შევსება) და bfill() (შემდეგი მნიშვნელობით შევსება), რაც ბევრად უკეთესია ვიდრე გლობალური მედიანით შევსება.

დროითი სერიების მოწინავე მახასიათებლების შემოტანა: 
Lag_Weekly_Sales - წინა კვირის გაყიდვები იმავე მაღაზიისა და დეპარტამენტისთვის
Lag52_Weekly_Sales - გაყიდვები წინა წლის იმავე კვირიდან
RollingMean_4W_Sales - ბოლო 4 კვირის გაყიდვების მოძრავი საშუალო
RollingStd_4W_Sales - ბოლო 4 კვირის გაყიდვების მოძრავი სტანდარტული გადახრა.

ვერსია 2.0-ის შედეგები (ახალი მახასიათებლებით და დახვეწილი წინასწარი დამუშავებით): ვალიდაციის WMAE: 1873.3626

ვალიდაციის MAE: 1692.2703

ლოკალური ტესტის WMAE: 1578.4943

ლოკალური ტესტის MAE: 1407.5672

ერთ შეხედვით ძალიან კარგი წინსვლაა, თითქმის 40$ გაუმჯობესებული შედეგი, მაგრამ როგორც ვიცი ქომფეთიშენის საუკეთესო შედეგი ამაზე მაღალი იყო, ამიტომ დიდი ალბათობით რაღაცა მეშლება.

# model_exp_RandomForest_3.ipynb

გაყოფის პრინციპი შენარჩუნებულია, მაგრამ დამატებულია Friday Alignment (გაყოფის თარიღები ზუსტდება უახლოეს პარასკევზე).  

ციკლური თარიღის მახასიათებლები: Month_sin და Month_cos. ეს ტრანსფორმაციები Random Forest-ს აძლევს უკეთეს საშუალებას, გაიგოს სეზონურობა და თვეების ციკლური ბუნება, რაც აუმჯობესებს პროგნოზირების სიზუსტეს

წინასწარი დამუშავების Pipeline: ColumnTransformer გამოიყენება კატეგორიული (Type სვეტის One-Hot Encoding) და რიცხვითი მახასიათებლების დასამუშავებლად.

RandomizedSearchCV გამოიყენება Random Forest-ის ოპტიმალური ჰიპერპარამეტრების მოსაძებნად. დროითი სერიების ვალიდაციისთვის გამოიყენება TimeSeriesSplit.

რატომღაც random_search.fit(X_train, y_train) ანდომებს ბევრ დროს, ნუ ძალიანაც არა მაგრამ RandomForest-სთვის შეუფერებლად ბევრს, ხოდა ჯერ ვერ მივხვდი რატომ.

Validation WMAE: 1805.7305 (Improved!)

Validation MAE: 1615.0387 (Improved!)

Local Test WMAE: 1490.7268 (Improved!)

Local Test MAE: 1307.3495 (Improved!)

ნუ აქაც ზედმეტად კარგი შედეგია და ეჭვებს აჩენს.


# experiment_5_future_engineering.ipynb

მოკლედ ამ ექსპერიმენტში იქიდან გამომდინარე რომ რადგან უკვე გავტესტე სხვადასხვა მიდგომები და ვნახე როგორია დატა. მოკლედ ვიზავ პრეპროცესინგის მთლიან ფაიფლაინს.
დატას გავყოფ ორად ტრეინად და ვალიდაციად, 80-20 ზე რადგან ტესტსეტზეც ეგრე ტრაინთან შედარებით 4 ჯერ პატარაა.
### ამის მერე უკვე ვაპირებ ასეთ პრეპროცესინგს. ნუ როგორც experiemt 2 ში ვქენი ეგრე ვიზავ მხოლოდ ტრეინსეტზე + დავამატებ მეოთხეში როგორც ვქენი მაგეებს ანუ დავმერჯავ მაგრამ markdown- ებს არ გამოვიყენებ.
ასევე გავწმინდავ დატას.

ასევე როგორც ვთქვი გვექნება: date_features, lag_features, rolling_features, store_dept_features.
ნუ ცალკე მაქ კიდე WalmartFeaturePipeline სადაც ვახდენ აუთლაიერების მოშორებასაც და შენდეგ FeatureEngineer.
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
https://dagshub.com/konstantine25b/Walmart-Recruiting---Store-Sales-Forecasting.mlflow/#/experiments/14?searchFilter=&orderByKey=attributes.start_time&orderByAsc=false&startTime=ALL&lifecycleFilter=Active&modelVersionFilter=All+Runs&datasetsFilter=W10%3D


# experiment_6_future_engineering.ipynb

აქ ვაპირებ რომ მეორე ექსპერიმენტი და მეოთხეში ნასწავლი რაღაცეები შევაერთო და მივიხო პრეპროცესინგის კარგი ვარიანტი.
Final columns: ['Store', 'Dept', 'Date', 'Weekly_Sales', 'Type', 'Size', 'Temperature', 'Fuel_Price', 'CPI', 'Unemployment', 'IsHoliday', 'Month', 'DayOfWeek', 'IsWeekend', 'IsMonthStart', 'IsMonthEnd', 'WeeksFromStart']
- ესენია ჩვენი მეორე ექსპერიმენტის საბოლოო ქოლუმები.
train_data, val_data, split_info = experiment_2_pipeline()

type-სთვის one hot encoderi გამომრჩა და ჩავამატე. ანუ გვაქ 19 col.
აი აქ არი ექსპერიმენტი.
https://dagshub.com/konstantine25b/Walmart-Recruiting---Store-Sales-Forecasting.mlflow/#/experiments/15?searchFilter=&orderByKey=attributes.start_time&orderByAsc=false&startTime=ALL&lifecycleFilter=Active&modelVersionFilter=All+Runs&datasetsFilter=W10%3D

'IsSuperBowlWeek', 'IsLaborDayWeek', 'IsMajorHoliday', 'Weekly_Sales_lag_2', 'Weekly_Sales_lag_8', 'Weekly_Sales_lag_4', 'IsChristmasWeek', 'IsBackToSchool', 'Weekly_Sales_lag_3', 'IsHolidayMonth', 'IsThanksgivingWeek', 'Weekly_Sales_lag_1', 'Weekly_Sales_lag_12'

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


# experiment_7_xgboost.ipynb

მოკლედ ვეჩალიჩე მაგრამ INTERNAL_ERROR: Response: {'error': 'unsupported endpoint, please contact support@dagshub.com'}
და ამის გამო ვერ ჩამოვტვირთე ჩვენი არტიფაქტი ამიტომ მომიწევს მე-7 ექსპერიმენტშ უბრაოგ გადავაკოპო კოდი.

https://dagshub.com/konstantine25b/Walmart-Recruiting---Store-Sales-Forecasting.mlflow/#/experiments/25?searchFilter=&orderByKey=attributes.start_time&orderByAsc=false&startTime=ALL&lifecycleFilter=Active&modelVersionFilter=All+Runs&datasetsFilter=W10%3D

მოკლედ ეს Weekly_Sales_lag ამათ აქვთ ყველაზე დიდი იმფორთანსი არ ვიცი კიდე ვცდი გაუმჯობესებას და ნუ თუ გამოვა კაია თუ არადა ამ ლაგებს დავანებებ თავს და მაგის გარეშე ვიჩალჩებ ჯობია. იმიტომ რომ მთელი დღე წაიღო მაგათზე წვალებამ.

კაი მოკლედ მეორე ექსპერიმენტს ვუყურებ და მანდ უკეთესი შედეგი გვაქ ბევრად ხოდა მანდ ასევე სხვა ფიჩერები მაქ ამიტომ მაქ ასეთი იდეა რო მანდ რა ფიჩერებიც მაქ განსხვავებულები ეგენი ჩავამატო და ისე გავტესტო.

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

აი ეს ფიჩერები გამომივიდა საბოლოო ჯამში.

['Store', 'Dept', 'Size', 'Temperature', 'Fuel_Price', 'CPI', 'Unemployment', 'IsHoliday', 'Month', 'DayOfWeek', 'IsWeekend', 'IsMonthStart', 'IsMonthEnd', 'WeeksFromStart', 'IsSuperBowlWeek', 'IsLaborDayWeek', 'IsThanksgivingWeek', 'IsChristmasWeek', 'IsMajorHoliday', 'IsHolidayMonth', 'IsBackToSchool', 'Type_Encoded', 'Type_A', 'Type_B', 'Type_C']


# model_exp_Prophet_1
მიდგომა: ცალკეული Prophet მოდელი ტრენინგდება Walmart-ის თითოეული უნიკალური (Store, Dept) კომბინაციისთვის, რაც უზრუნველყოფს პროგნოზირების დეტალურ დონეს.

ფიჩერები: გამოყენებულია წინასწარ დამუშავებული და გამდიდრებული მონაცემები, მათ შორის IsHoliday, Temperature, Fuel_Price, CPI, Unemployment, Type_Encoded (მაღაზიის ტიპის კოდირება) და Size, როგორც დამატებითი რეგრესორები.

მონაცემთა გაყოფა:

კომბინირებული ტრენინგი+ვალიდაცია: 2010-02-05 - 2012-07-13

ტესტირება (უცნობი): 2012-07-20 - 2012-10-26

სერიები, რომლებსაც აქვთ 50-ზე ნაკლები დაკვირვება, გამოტოვებულია. დანერგილია შეცდომების დამუშავება Prophet.fit() და Prophet.predict() ფუნქციებისთვის, რათა თავიდან იქნას აცილებული კრაშები პრობლემურ სერიებზე.

ტრენინგი 

MAE: $2,463.04

RMSE: $6,697.10

WMAE: $2,759.62

ტესტირება

MAE: $15,535.41

RMSE: $26,635.84

WMAE: $15,697.87

მიუხედავად დეტალური მოდელირებისა და ფიჩერების ინჟინერიისა, Prophet მოდელის შესრულება სატესტო ნაკრებზე ძალიან ცუდია. Out-of-sample WMAE მნიშვნელოვნად მაღალია (დაახლოებით $15,697.87), რაც მიუთითებს იმაზე, რომ მოდელი კარგად ვერ ახდენს განზოგადებას უცნობ მონაცემებზე.



## Prophet მოდელის ექსპერიმენტი (Exp. 2: გაუმჯობესებული)

მიდგომა: ცალკეული Prophet მოდელი ტრენინგდება Walmart-ის თითოეული უნიკალური (Store, Dept) კომბინაციისთვის.

გაფართოებული თარიღის ფიჩერები: დაემატა კვარტალური, თვის/კვარტლის დასაწყისის/დასასრულის და "დღეები/კვირები დაწყებიდან" ფიჩერები.

დღესასწაულის დეტალური ფიჩერები: შეიქმნა უფრო მკაფიო ინდიკატორები ძირითადი დღესასწაულებისა და სადღესასწაულო პერიოდებისთვის (IsMajorHoliday, IsHolidayMonth, IsBackToSchool).

Lag ფიჩერები: აშკარად გამორთულია ოვერფიტინგის თავიდან ასაცილებლად.

ჭარბი ფიჩერების ამოღება: გარკვეული ფიჩერები (Year, Quarter, Day და სხვ.) ამოღებულია კოლინეარობის ან ზედმეტობის თავიდან ასაცილებლად.

მონაცემთა გაყოფა:

ტრენინგი: 2010-02-05 - 2012-04-13 (337,256 ჩანაწერი)

ვალიდაცია: 2012-04-13 - 2012-10-26 (84,314 ჩანაწერი)

მოდელირება:

სერიების რაოდენობა: 3082 უნიკალური (Store, Dept) კომბინაცია.

გატრენინგებული სერიები: 2615 (საჭირო მინიმუმ 50 დაკვირვება).

გამოტოვებული სერიები: 467 (ძალიან მოკლე სერიები).

# შედეგები
ტრენინგი: WMAE: 1439.43

RMSE: 2831.60

MAE: 1388.21

R²: 0.9246

ვალიდაცია: WMAE: 6178.43 ⭐

RMSE: 19636.79

MAE: 6141.23

R²: -1.1195

ესეც ცუდია ნამდვილად, მარა წინას ჯობია!


# model_exp_LightGBM_1.

მიდგომა: ერთი გლობალური მოდელი, რომელიც სწავლობს შაბლონებს ყველა (Store, Dept) კომბინაციიდან. Store და Dept ID-ები ჩართულია როგორც კატეგორიული ფიჩერები, რაც მოდელს საშუალებას აძლევს ისწავლოს თითოეული მაღაზიისა და განყოფილების სპეციფიკური მახასიათებლები.

წინასწარი დამუშავება: გამოიყენება იგივე WalmartPreprocessingPipeline, რაც Prophet ექსპერიმენტში, რაც უზრუნველყოფს ფიჩერების ინჟინერიისა და მონაცემთა გაწმენდის თანმიმდევრულობას. ეს მოიცავს თარიღის/დღესასწაულის ფიჩერების ინჟინერიას, მაღაზიის ტიპის კატეგორიულ დაშიფვრას (One-Hot და Label), და MarkDown ფიჩერების ამოღებას. Lag ფიჩერები კვლავ გამორთულია.

მონაცემთა გაყოფა: იდენტურია წინა ექსპერიმენტისა, დროითი გაყოფის გამოყენებით:

ტრენინგი: 2010-02-05 - 2012-04-13

ვალიდაცია: 2012-04-13 - 2012-10-26

ფიჩერები: LightGBM იყენებს ფიჩერების უფრო ფართო სპექტრს:

კატეგორიული: Store, Dept, Type_A, Type_B, Type_C, Type_Encoded, Month, DayOfWeek, WeekOfYear, IsHoliday, IsWeekend, IsMonthStart, IsMonthEnd, IsSuperBowlWeek, IsLaborDayWeek, IsThanksgivingWeek, IsChristmasWeek, IsMajorHoliday, IsHolidayMonth, IsBackToSchool.

რიცხვითი: Size, Temperature, Fuel_Price, CPI, Unemployment, DaysFromStart, WeeksFromStart.

LightGBM პარამეტრები: დაყენებულია MAE ობიექტურ ფუნქციაზე (regression_l1), რაც უფრო მდგრადია აუტლაიერების მიმართ. გამოყენებულია early_stopping_round (50 რაუნდი) ოვერფიტინგის თავიდან ასაცილებლად.

შედეგები
ტრენინგის 

WMAE: 1554.33

RMSE: 3251.47

MAE: 1464.41

R²: 0.9481

ვალიდაციის

WMAE: 1663.04 ⭐ (კონკურსის მთავარი მეტრიკა)

RMSE: 3359.20

MAE: 1654.74

R²: 0.9449

რა თქმა უნდა! შესანიშნავი შედეგებია. დაბალი WMAE-ით და მაღალი R²-ით, მინიმალური ოვერფიტინგით.

# feature_importance.ipynb

ეს ანალიზი მიზნად ისახავს Weekly_Sales-თან (კვირის გაყიდვები) ფიჩერების დამოკიდებულების სიძლიერის შეფასებას, კორელაციის მონაცემების გამოყენებით. 
კორელაციის ანალიზის თანახმად, გაყიდვებზე ყველაზე დიდ გავლენას მაღაზიის მახასიათებლები ახდენს, ხოლო ეკონომიკური და დროითი ფაქტორების გავლენა უმნიშვნელოა.

✅ Important Features (Absolute Correlation >= 0.06):
   - Size: 0.3282
   - Type_Encoded: 0.2930
   - Type_A: 0.2798
   - Type_C: 0.1804
   - Type_B: 0.1800
   - Store: 0.0875

❌ Less Important Features (Absolute Correlation < 0.06):
   - MarkDown5: 0.0583
   - MarkDown1: 0.0541
   - MarkDown4: 0.0394
   - Unemployment: 0.0376
   - Dept: 0.0366
   - MarkDown2: 0.0247
   - MarkDown3: 0.0234
   - Month: 0.0229
   - WeekOfYear: 0.0225
   - IsThanksgivingWeek: 0.0156
   - IsMajorHoliday: 0.0121
   - IsChristmasWeek: 0.0116
   - Fuel_Price: 0.0073
   - IsMonthEnd: 0.0054
   - IsHoliday: 0.0052
   - DaysFromStart: 0.0039
   - WeeksFromStart: 0.0039
   - IsHolidayMonth: 0.0032
   - IsBackToSchool: 0.0029
   - Temperature: 0.0025
   - IsLaborDayWeek: 0.0013
   - CPI: 0.0010
   - IsSuperBowlWeek: 0.0005
   - IsMonthStart: 0.0003

გამოიყო ფიჩერების მნიშვნელობის დონეები.


ეს არის ჩვენი მიღებული შედეგი მარქდაუნის გარეშე.  (xgboost)    
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

ეს არის მარქდაუნით მიღებული შედეგი.
 TRAINING METRICS:
   WMAE: 1854.59
   RMSE: 2842.06
   MAE: 1827.66
   R²: 0.9262

📊 VALIDATION METRICS:
   WMAE: 5983.96 ⭐
   RMSE: 14582.25
   MAE: 5932.02
   R²: 0.5585

   დიდად შედეგი არ შეცვლილა და რაც შეიცვალა უარესობისკენ, ამიტომ მარქდაუნს არ ვიყენებთ


# experiment_9_prophet

ავიღებ ისევ მე-7 ექსპერიმენტში მიღებულფიჩერებს და დატას დავამუშავებ ეგრე 

['Store', 'Dept', 'Size', 'Temperature', 'Fuel_Price', 'CPI', 'Unemployment', 'IsHoliday', 'Month', 'DayOfWeek', 'IsWeekend', 'IsMonthStart', 'IsMonthEnd', 'WeeksFromStart', 'IsSuperBowlWeek', 'IsLaborDayWeek', 'IsThanksgivingWeek', 'IsChristmasWeek', 'IsMajorHoliday', 'IsHolidayMonth', 'IsBackToSchool', 'Type_Encoded', 'Type_A', 'Type_B', 'Type_C']

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

ახლა გავტესტოთ timesfm - ნუ ეს ტრანსფორმეტ based მოდელია მაგრამ მაინტერესებს ეხა.

გავტესტე მარტივი ვარიანტი 

https://dagshub.com/konstantine25b/Walmart-Recruiting---Store-Sales-Forecasting.mlflow/#/experiments/44
📊 Validation Metrics:
   WMAE (Competition Metric): $1,985.27
   MAE: $1,976.18
   RMSE: $4,068.65
   R²: 0.9131


ამის მერე ვცადე უფრო კომპლექსურის გატესტვა


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

კაი როგორცჩანს ალბათ კომპლექსური ოვერფიტში წავიდია და სწორედ ამიტომ აჯობა მარტივმა მოდელმა.





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


# arima მოდელი

მოკლედ აღმოვაჩინეთ რომ future enginering კაი გვქონდა. მაგრამ აღმოვაჩინეთ რომ თითოეულ მოდელს სხვადასხვანაირი
ტრენინგი და დატასეტის დამუშავება უნდა.

ამიტომ ჩავუჯექი ეხა სათითაოდ მოდელებს ჯერ ვიწყებ arima-თი.
ვუყურე arima-ზე ვიდეოს და ნუ ყველაზე მარტივი time series ალგორითმია ალბათ.

AutoRegressive Integrated Moving Average
AR (AutoRegressive) – "memory of the past" today’s value depends on yesterday’s value.
I (Integrated) – "trend remover" If your data has a trend, we subtract the previous value to get the difference.
MA (Moving Average) – "shock smoother" Captures shocks/noise that impact future values.

კაი ნუ ეხა სეზონურობას არ გამოვიყენებ, და თავიდან ბოლომდე სტატისტიკური მოდელი იქნება იმიტომ რომ არიმაა. 
მხოლოდ date აქვს ინფუთად ამიტომ feature engineering-ს დიდი აზრი არც აქვს.

თითოეული store_dept კომბოზე ცალ ცალკე მოდელი გვაქ.
Total possible combinations: 3,248 (45 stores × ~72 departments average)
აი ეს არის შედეგი:

https://dagshub.com/konstantine25b/Walmart-Recruiting---Store-Sales-Forecasting.mlflow/#/experiments/52

📊 Training Metrics:
   Training WMAE: $4,956.27

📊 Validation Metrics:
   WMAE (Competition Metric): $5,611.67
   MAE: $5,776.46
   RMSE: $4,574.46
   R²: 0.7273

# Sarima იგივე arima + სეზონურობა.

Sarima ჯობია arima-ს იმით რომ აქვს სეზონურობაც რაც გუსლისმობს რომ სეზონური პატერნების
ამოცნობა შეუძლია.
Sarima იღებს 7 პარამეტრს:
(p, d, q) – non-seasonal ARIMA terms:
p: autoregression (AR)
d: differencing (trend removal)
q: moving average (MA)

(P, D, Q, s) – seasonal parts:
P: seasonal autoregression
D: seasonal differencing
Q: seasonal moving average
s: length of the seasonal cycle (e.g., 12 for monthly, 7 for weekly)

ანალოგიურად როგორც arima ესეც მხოლოდ date columns-ს მიიღებს და მხოლოდ მასზე ტრეინინგდება ანუ არ ჭირდება სხვა ქოლუმნები.

იგივენაირი ტრეინინგით როგორც arima შეგვიძლია გავუშვათ ეს მოდელიც.
SARIMA(1,1,1)x(0,1,0,52)

📊 Training Metrics:
   Training WMAE: $2,686.54

📊 Validation Metrics:
   WMAE (Competition Metric): $3,219.86
   MAE: $3,173.68
   RMSE: $5,867.32
   R²: 0.6853

https://dagshub.com/konstantine25b/Walmart-Recruiting---Store-Sales-Forecasting.mlflow/#/experiments/58

# Experiment Sarimax

Sarimax= sarima + external regressors 

შესაბამისად აქ შეგვიძლია სხვა ფიჩერების გამოყენებაც რაც მიღებული გვაქვს.

იგივე ნაირად ვატრეინინგებ რაც sarima, მაგრამ მანდ რო გვაქ სხვადასხვა ადრე გამოყვანილი ფიჩერების მაგეებსაც ვატან ინფუთად. შესაბამისად უფრო კომპლექსური მოდელი გამოდის.

📊 Validation Metrics:
   WMAE (Competition Metric): $3,591.14
   MAE: $3,467.08
   RMSE: $6,590.84
   R²: 0.7935

ნუ ესენია სტატისტიკური მოდელები.


# model_exp_FX_Prophet.ipynb
ჩვენი მიზანია გამოვასწოროთ წინაზე დაშვებული შეცდომები.
Prophet-ისთვის დროითი კომპონენტები უკვე შიდა მექანიზმებშია ჩაშენებული და ის თავად იყენებს მათ სეზონურობის, ტრენდისა და დღესასწაულების დასადგენად.
შევქმენით holidays_df DataFrame, რომელიც შეიცავს Walmart-ისთვის რელევანტურ ძირითად ამერიკულ დღესასწაულებს მათი თარიღებით
ჩვენ დავამატეთ ლოგიკა, რომელიც უზრუნველყოფს, რომ Weekly_Sales (y) მნიშვნელობები არ იყოს უარყოფითი.
მონაცემები იყოფა ტრენინგისა და ვალიდაციის ნაკრებებად მკაცრად დროის მიხედვით (80/20 თანაფარდობით).
Date სვეტები გარდაიქმნება datetime ტიპად.
Prophet-ისთვის საჭირო ფორმატირება (transform):სვეტები გადაირქმევა Date-დან ds-ზე და Weekly_Sales-დან y-ზე, რაც Prophet-ის სტანდარტული მოთხოვნაა.
ჩვენ ვწვრთნით ცალკე Prophet მოდელს თითოეული უნიკალური (Store, Dept) კომბინაციისთვის.
Prophet-ის პარამეტრები: yearly_seasonality=True და weekly_seasonality=True ააქტიურებს Prophet-ის ჩაშენებულ ფუნქციებს ყოველწლიური და ყოველკვირეული სეზონურობის ავტომატურად ამოსაცნობად. holidays=holidays_df აერთიანებს ჩვენს მიერ განსაზღვრულ დღესასწაულებს მოდელის პროცესში.

შედეგები:
Validation Metrics: WMAE (Competition Metric): $1,669.21 MAE: $1,626.37 RMSE: $3,725.00

Holiday Breakdown: Holiday MAE: $1,979.10 (2966 samples) Non-Holiday MAE: $1,613.74 (82843 samples)

პროექტის განახლებულმა ვერსიამ მნიშვნელოვნად გააუმჯობესა Prophet მოდელის შესრულება.



# Experiment N-Beats
იგივე Neural Basis Expansion Analysis for Time Series

კაი როგორც გავარკვიე ორნაირი მოდელი არსებობს.
default-ად შეუძლია მხოლოდ date column-ის მიღება
ხოლო მეორეს აქვს ექსთენშენი სადაც Multivariate or Exogenous Input-ებიც შეიძლება ანუ ბევრი ქოლუმის ერთად გადაცემა.

ჯერ გავტესტოთ პირველი ანუ მხოლოდ date რომ აქვს.

ჰიპერპაამეტრებს ორივეგან იგვეს გამოვიყენებ:
config = {
        'h': 53,               
        'input_size': 52, 
        'max_steps': 5000,     
        'val_size': 53,       
        'batch_size': 256,   
        'learning_rate': 1e-3, 
        'random_seed': 42,
        'optimizer': 'AdamW',
        'shared_weights': True
    }

[ Input Block ] → [ Block 1 ] → [ Block 2 ] → ... → [ Final Forecast ]
ასევე როგორც ვნახე აქ გვაქ რესიდუალ ქონექშენებიც.

WMAE: 3767.47, MAE: 3388.35, RMSE: 8433.92, R²: 0.8702

აი ეს არის ვალიდაციის შედეგი.

# Experiment NBEATSx ახლა კიდე გავტესტოთ ვერსია სადაც nbeats-ში შეგიძლია ფიჩერების გატანებაც. Extended N-BEATS
ანუ ვიყენებ ეხა NBEATSx

ანუ ეხა გადავეცი ის ფიჩერებიც რაც მანმდე დამუშავებული მაქვს. ესენი ანუ.
['IsHoliday', 'Temperature', 'Fuel_Price', 'CPI', 'Unemployment', 'Size', 'Month', 'DayOfWeek', 'WeekOfYear', 'IsWeekend', 'IsMonthStart', 'IsMonthEnd', 'IsSuperBowlWeek', 'IsLaborDayWeek', 'IsThanksgivingWeek', 'IsChristmasWeek', 'IsMajorHolidayDerived', 'IsHolidayMonth', 'IsBackToSchool', 'Type_A', 'Type_B', 'Type_C', 'Type_Encoded']

სქორი კი ეს არის: WMAE: 2117.76, MAE: 2025.81, RMSE: 4381.23, R²: 0.8364

ანუ უკეთესია ვიდრე მხოლოდ date-ებზე რო ეყრდნობა ეგეთი nbeats.

# Experiment PatchTST
 Patch-based Time Series Transformer

მოკლედ ესეც არის time series მოდელი, იყენებს ტრანსფორმერებს,
მაგრამ სხვანაირად 
1. Splits the time series into patches 
2. Uses 1D patches of historical values to learn patterns more efficiently
Time series → patches → transformer → forecast

ასევე კარგიაო long term ფორკასტინგისთვის რაც იდეაში ჩვენ გვინდა.
ასევე ინფითად ჩვეულებრივად სხვადასხვა ფიჩერების მიღება შეუძია

https://wandb.ai/konstantine25b-free-university-of-tbilisi-/walmart-patchtst-forecasting-NO-EXOG-manual-log/runs/sje09f04/overview

config = {
        'h': 53,
        'input_size': 52,
        'max_steps': 5000,
        'val_size': 53,
        'batch_size': 256,
        'learning_rate': 1e-3,
        'random_seed': 42,
        'optimizer': 'AdamW',
        'patch_len': 16,
        'stride': 8,
        'revin': True,
        'n_heads': 16,
        'encoder_layers': 3,
        'hidden_size': 128,
        'linear_hidden_size': 256,
        'dropout': 0.2,
        'fc_dropout': 0.2,
        'head_dropout': 0.0,
        'attn_dropout': 0.0
    } 
აქ განსხვავებული სხვებისგან არის :
patch_len - რამდენ patch-ად დაიყოფა,
stride - ეს ნიშნავს იგივე სლაიდინგ ვინდოვს პრინციპით 
input_size = 52, patch_len = 16, and stride = 8, you get:
Patches: [(0-15), (8-23), (16-31), ..., (36-51)]

encoder_layers - ტრანსფორმერების რაოდენობა.

აი შედეგი:
WMAE: 4311.47, MAE: 4228.89, RMSE: 9884.75, R²: 0.8218


# model_exp_TFT_FX.ipynb
ეს მოდელი გამოირჩევა შერჩევითი სწავლის მექანიზმით (Gating Mechanism) და ყურადღების მექანიზმით (Attention Mechanism).
ამ მოდელს პროფეტისგან განსხვავებით შეუძლია გამოიყენოს სტატიკური მახასიათებლები, მომავალში ცნობილი მახასიათებლები და მომავალში უცნობი მახასიათებლები.
NeuralForecast ბიბლიოთეკა იყენებს ამ unique_id-ს, რათა დაადგინოს თითოეული ინდივიდუალური დროითი სერია (მაგ. "მაღაზია 1, განყოფილება 1"). მოდელი ერთდროულად სწავლობს ყველა ამ სერიიდან, რაც აუმჯობესებს პროგნოზების სიზუსტეს.
Date სვეტები გარდაიქმნება datetime ფორმატში.
MarkDown სვეტებში NaN მნიშვნელობები ივსება ნულებით.
ჩვენ შევქმენით ყველა შესაძლო (Store, Dept, Date) კომბინაცია თითოეული სერიის მინიმალურ და მაქსიმალურ თარიღებს შორის.
80% ტრეინინგი - 20% ვალიდაცია

Feature engeneering: MultiIndexKeeper: ეს ტრანსფორმატორი აყენებს DataFrame-ის ინდექსს ['Date', 'Store', 'Dept'] MultiIndex-ად. ეს აუცილებელია, რადგან NeuralForecast მუშაობს MultiIndex-იან DataFrame-ებთან
week: კვირის ნომერი წლის დასაწყისიდან (ან პერიოდის დასაწყისიდან).
sin_13, cos_13: 13-კვირიანი (დაახლოებით კვარტალური) სეზონურობის დასაფიქსირებლად.
sin_23, cos_23: 23-კვირიანი სეზონურობის დასაფიქსირებლად.

ინფუთები:
unique_id: სერიის იდენტიფიკატორი.
ds: თარიღი/timestamp.
y: სამიზნე ცვლადი (Weekly_Sales).

ვალიდაციაზე ჩვენი შედეგები: 
WMAE (Competition Metric): $1,421.02
MAE: $1,585.18
RMSE: $3,415.59
Holiday MAE: $868.07 (6,617 samples)
Non-Holiday MAE: $1,642.66 (82,540 samples)
Holiday MAE ($868.07) მნიშვნელოვნად დაბალია Non-Holiday MAE-ზე ($1,642.66).


# model_exp_DLinear_FX

model_exp_DLinear_FX

DLinear მოდელის მიმოხილვა: როგორ მუშაობს და რატომ არის კარგი/ცუდი ამ ამოცანისთვის? DLinear (Decomposition Linear) არის Deep Learning არქიტექტურა, რომელიც ეფუძნება დროითი სერიების დეკომპოზიციას. მისი მთავარი იდეაა დროითი სერიის დაყოფა ორ ძირითად, დამოუკიდებელ კომპონენტად: ტრენდული (Trend): მონაცემების გრძელვადიანი, საერთო მიმართულება ან ძირითადი დონე. სეზონური (Seasonal): განმეორებადი, მოკლევადიანი შაბლონები, რომლებიც კონკრეტულ პერიოდებში (მაგ., კვირა, თვე, წელი) ვლინდება. ამ კომპონენტების გამოყოფის შემდეგ, თითოეული მათგანი დამოუკიდებლად მოდელირდება ხაზოვანი ფენების (Linear Layers) გამოყენებით. ეს მიდგომა მიზნად ისახავს რთული და არაწრფივი დამოკიდებულებების გამარტივებას, რაც პროგნოზირების სიზუსტის გაუმჯობესებას უწყობს ხელს

ტრადიციული "თითო-სერიაზე-ერთი-მოდელი" მიდგომებისგან განსხვავებით, DLinear-ს შეუძლია ყველა სერიის საერთო შაბლონების სწავლა ერთდროულად. ეს მას გაცილებით ეფექტურს ხდის დიდი მონაცემთა ნაკრებებისთვის.

ცვლადები როგორიცაა Temperature, Fuel_Price, CPI, Unemployment დიდად არ გამოგვადგება ამ მოდელში. გამოვიყენოთ neural forecast, სადაც მოდელი იღებს unique_id, ds (Date) და y (Weekly_Sales) სვეტებს.

მიუხედავად იმისა, რომ neuralforecast-ის DLinear-ის ძირითადი ვერსია მხოლოდ y-ზე დაყრდნობით პროგნოზირებს, მისი ოპტიმიზებული არქიტექტურა უკეთ აითვისებს y-ში არსებულ ტრენდებსა და სეზონურობას.

Validation WMAE: 1564.401




# inference

კარგი ბევრი მოდელი ვნახეთ და მოდი ავარჩიოთ საუკეთესო:

## მოდელების შედარება (Validation WMAE):

###  საუკეთესო მოდელები:

1. **TFT (Temporal Fusion Transformer)** - $1,421.02 WMAE
  - ნეირალური ქსელი (Transformer-based)

2. **Prophet** - $1,669.21 WMAE
   - სტატისტიკური მოდელი
   - ჰოლიდეიებზე კარგია (Holiday MAE: $1,979 vs Non-Holiday: $1,614)


###  საშუალო მოდელები:

 **NBEATSx** - $2,117.76 WMAE
   - ნეირალური ქსელი + ფიჩერები
   - უკეთესია ჩვეულებრივ N-BEATS-ზე

 **SARIMAX** - $3,591.14 WMAE
   - SARIMA + external regressors

 **PatchTST** - $4,311.47 WMAE
   - Transformer time series მოდელი

### მოდი გავაკეთოთ ერთი ცალი ჰიბრიდული მოდელუ TFT + Prophet Hybrid Ensemble

- TFT: საუკეთესო იყო ($1,421) თან ტრანსფორმერტებს იყტენებს და ასევე attention მექანიზმი აქ
- Prophet: კარგი იყო ჰოლიდეიებში. თან სტატისტიკურია და ჰოლიდეიებში კარგად შეუძლია დაპროგნოზირება
- Adaptive weighting holiday:  80% Prophet + 20% TFT
- ჩვეულებრივ დღეებზე: 55% TFT + 45% Prophet

ვნახოთ რა შედეგს დადებს. 

# Experimetn Hybrid TFT + Prophet

📊 Validation Metrics:
   WMAE (Competition Metric): $5,146.98
   MAE: $5,338.29
   RMSE: $9,717.69

აი ეს ფრედიქშენი დამიბრუნე. ჰმმ რაღაც არასწორად ვქენი
ამიტო მოდი თითოეულის ფრედიქშენს დავაბრუნებინებ შემდეგ :

📊 Individual Model Evaluation:
   🤖 TFT Model:
      WMAE: $1,420.70
      MAE: $1,595.00
      RMSE: $3,419.24
   📈 Prophet Model:
      WMAE: $13,383.78
      MAE: $15,188.69
      RMSE: $26,528.17

============================================================
🎯 EXPERIMENT HYBRID RESULTS SUMMARY
============================================================

📊 Validation Metrics:
   WMAE (Competition Metric): $5,146.98
   MAE: $5,338.29
   RMSE: $9,717.69


ხო როგორც ჩანს tft სწორად მუშაობს მაგრამ prophet-ში რაღაც დაერორდა.
ამიტო დავფიქსო უნდა.

📊 Individual Model Evaluation:
   🤖 TFT Model:
      WMAE: $1,399.54
      MAE: $1,573.87
      RMSE: $3,365.31
   📈 Prophet Model:
      WMAE: $19,600.19
      MAE: $19,884.70
      RMSE: $31,239.94

============================================================
🎯 EXPERIMENT HYBRID RESULTS SUMMARY
============================================================

📊 Validation Metrics:
   WMAE (Competition Metric): $7,120.19
   MAE: $7,187.35
   RMSE: $11,468.45

კიდე არასწორია ვაა კიდე ვეჩალიჩები აბა.


მოკლედ ერთ დატასეტზე დავაყენე საბოლოოდ რადგან 89000 ქონდა tft და 85000 prophets
ხოდა მაგან 19000 მდე აწია tft, ხოლო ამის შემდეგ ვცადე 
89000-ები ორივე და ამან prophet გააუარესა 31000 მდე მოკლედ როგორც აღმოჩნდა დატას სპლიტს დიდი მნიშვნელობა ქონია

ხო tft-ზე ამის მიზეზი იყო რომ შექმნა გეპები რომლებიც 0 - ებით შეავსო და მერე ისწავლა რო 0 კაია და ამიტო საგრძნობლად გაუარესდა.

ამიტომ გადავწყვიტე რომ ცალ ცალკე განმეხილა და საბოლოოდ ჯერ გავაკეთე inference tft 
და შემდეგ inference prophet.
inference tft -ის შედეგი იყო: private - 3554.98537 public - 3402.89439
inference prophet  -ის შედეგი იყო: provate - 3034.13754 public - 2923.90240
და ბოლოს როგორც ვთქვი

- Adaptive weighting holiday:  80% Prophet + 20% TFT
- ჩვეულებრივ დღეებზე: 55% TFT + 45% Prophet
ამისთვის ავდექი და ეს ფრედიქშენები ზუსტად ესე სევკარი csv ფაილში
private - 2780.48818
public - 2678.12363

რაც საკმაოდ კარგი score არის.


მონაცემთა დამუშავება:
train.csv, features.csv და stores.csv ფაილები იტვირთება და ერთიანდება Store, Date და IsHoliday სვეტების მიხედვით.
Date სვეტი გარდაიქმნება datetime ობიექტებად.
MarkDown1 - MarkDown5 სვეტებში არსებული NaN მნიშვნელობები ივსება 0-ით
ყოველი (Store, Dept) წყვილისთვის, მონაცემებში არსებული უმოკლესი და უგრძესი თარიღების მიხედვით იქმნება თარიღების სრული დიაპაზონი (კვირა-პარასკევის სიხშირით). თუ ამ დიაპაზონში რომელიმე თარიღი აკლია ორიგინალ მონაცემებში, ის ემატება და მისი Weekly_Sales ივსება 0-ით. ეს აუცილებელია დროითი სერიების მოდელებისთვის, რომლებიც უწყვეტ მონაცემებს ელიან.
Temperature, Fuel_Price, CPI, Unemployment სვეტებში დარჩენილი NaN მნიშვნელობები ივსება ჯერ (Store, Dept) ჯგუფის საშუალო მნიშვნელობით, შემდეგ კი (თუ მაინც რჩება NaN) საერთო საშუალო მნიშვნელობით.
Weekly_Sales სვეტში ნებისმიერი უარყოფითი მნიშვნელობა იცვლება 0-ით.
საბოლოო მონაცემები სორტირდება Date, Store და Dept სვეტების მიხედვით.

prophet part:
Prophet-ი მოითხოვს მონაცემებს ds (datetime) და y (რიცხვითი) სვეტებით. ამიტომ, Date სვეტი გადაერქმევა ds-ად, ხოლო Weekly_Sales - y-ად.
Prophet ავტომატურად ამოდელირებს წლიურ და ყოველკვირეულ სეზონურობას. ის იყენებს Fourier series-ს სეზონური ეფექტების დასაფიქსირებლად.
Prophet ამოდელირებს ტრენდს წრფივი ან ლოგისტიკური ტრენდის გამოყენებით.
დღესასწაულები: Prophet-ს აქვს კონკრეტული დღესასწაულების (სუპერ ბოული, მადლიერების დღე...) ჩართვის საშუალება. ამ დღესასწაულებს შეუძლიათ მნიშვნელოვნად შეცვალონ გაყიდვები და Prophet-ი მათ ცალკეული ორობითი რეგრესორების სახით ამატებს. IsHoliday აქ გადამწყვეტ როლს თამაშობს.
Prophet-ს შეუძლია გამოიყენოს დამატებითი რიცხვითი რეგრესორები, რომლებიც გავლენას ახდენენ y მნიშვნელობაზე, მაგრამ არ არიან დროის ფუნქციები (Temperature, Fuel_Price, CPI, Unemployment, MarkDown სვეტები). 

Temporal Fusion Transformer part:
MultiIndexKeeper: ეს არის ტრანსფორმატორი, რომელიც ინახავს ინდექსის სვეტებს (Date, Store, Dept) მონაცემთა დამუშავების ეტაპების განმავლობაში. TFT-ს სჭირდება თითოეული (Store, Dept) წყვილისთვის ცალკე დროითი სერიის იდენტიფიცირება.
DateFeatureCreator: ეს არის ტრანსფორმატორი, რომელიც Date სვეტიდან გამოიმუშავებს დამატებით დროით მახასიათებლებს (year, month, dayofweek, dayofyear, weekofyear, quarter, is_month_start, is_month_end, is_quarter_start, is_quarter_end, is_year_start, is_year_end). ეს სეზონურობისა და ციკლურობის დაჭერაში ეხმარება TFT-ს.
preprocessor: რიცხვითი სვეტები: Temperature, Fuel_Price, CPI, Unemployment, MarkDown1-5. მათთვის ხდება გამოტოვებული მნიშვნელობების საშუალოთი შევსება.
კატეგორიული სვეტები: Type, IsHoliday. მათთვის ხდება გამოტოვებული მნიშვნელობების ყველაზე ხშირი მნიშვნელობით შევსება.
Store, Dept გადადის შემდეგ ეტაპზე ცვლილებების გარეშე.
TFTRegressor: 
input_chunk_length: განსაზღვრავს რამდენი წარსული დროითი ნაბიჯი უნდა იქნას გამოყენებული პროგნოზირებისთვის.
output_chunk_length: განსაზღვრავს რამდენი მომავალი დროითი ნაბიჯის პროგნოზირება უნდა მოხდეს.
Epochs, Batch Size, Random Seed: კონფიგურაციები ტრენინგის პროცესისთვის.
