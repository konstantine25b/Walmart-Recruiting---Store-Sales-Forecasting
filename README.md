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


