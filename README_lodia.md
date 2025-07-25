ჩვენი საწყისი მიდგომა გულისხმობს Random Forest რეგრესორის გამოყენებას, რადგან ის არის მძლავრი, მოქნილი და კარგად უმკლავდება როგორც რიცხვით, ისე კატეგორიულ მახასიათებლებს.

კონკურსის ძირითადი მიზანია რაც შეიძლება ზუსტი პროგნოზების მიღება Weekly_Sales (ყოველკვირეული გაყიდვების) ველისთვის.

ჩვენი მოდელის შეფასება ხდება WMAE  მეტრიკის გამოყენებით.
ამ მეტრიკის მიხედვით, დღესასწაულების კვირებს 5 ჯერ მეტი წონა აქვს, ვიდრე არასადღესასწაულოს. 

მონაცემთა ნაკრები (Dataset)
პროექტი იყენებს Walmart-ის გაყიდვების კონკურსის მონაცემთა ნაკრებს, რომელიც შედგება 4 ცალკეული .csv ფაილისგან:
train.csv: მოიცავს ისტორიულ გაყიდვებს (Weekly_Sales) 2010-02-05-დან 2012-11-01-მდე, მაღაზიის და დეპარტამენტის ID-ს, თარიღს და IsHoliday დროშას.
test.csv: იდენტური სტრუქტურისაა train.csv-ის, მაგრამ Weekly_Sales ველის გარეშე. ეს არის ის მონაცემები, რომლებზეც პროგნოზირება გვჭირდება კონკურსისთვის. (შენიშვნა: ამჟამინდელ ვერსიაში ეს ფაილი არ გამოიყენება. სამაგიეროდ, train.csv იყოფა ლოკალურ სასწავლო, ვალიდაციისა და ტესტირების ნაწილებად.)
features.csv: შეიცავს დამატებით ინფორმაციას თარიღის, მაღაზიის, ტემპერატურის, საწვავის ფასის, CPI-ის, უმუშევრობის დონის და MarkDown მონაცემების (MarkDown1-5) შესახებ. MarkDown მონაცემებს ხშირად აქვს ბევრი გამოტოვებული მნიშვნელობა.
stores.csv: მოიცავს სტატიკურ ინფორმაციას თითოეული მაღაზიის შესახებ: მაღაზიის ტიპი (Type) და ზომა (Size).


RandomForest_Preprocessing_&_FeatureEngineering Run
ეს ეტაპი მოიცავს მონაცემთა მომზადებას და ახალი მახასიათებლების შექმნას Random Forest მოდელისთვის.
მონაცემთა ჩატვირთვა და გაერთიანება: თავდაპირველად იტვირთება train.csv, features.csv და stores.csv ფაილები და ერთიანდება train_merged DataFrame-ში.

თარიღის მახასიათებლების შექმნა: Date სვეტიდან ამოღებულია დროზე დამოკიდებული მახასიათებლები, როგორიცაა წელი (Year), თვე (Month), კვირა (Week), კვირის დღე (DayOfWeek), წლის დღე (DayOfYear), თვის დასაწყისი/ბოლო (IsMonthStart, IsMonthEnd) და დროის ინდექსი (TimeIdx).

უარყოფითი გაყიდვების დამუშავება: Weekly_Sales ველში არსებული უარყოფითი მნიშვნელობები (რომლებიც შეიძლება დაბრუნებებს ნიშნავდეს) იცვლება 0-ით.

გამოტოვებული მნიშვნელობების დამუშავება (MarkDowns): MarkDown1-5 სვეტებში არსებული NaN მნიშვნელობები ივსება 0-ით. ეს მიანიშნებს, რომ მოცემულ პერიოდში სარეკლამო აქცია არ ყოფილა აქტიური.

კატეგორიული მახასიათებლების კოდირება: Type (მაღაზიის ტიპი A, B, C) სვეტი გარდაიქმნება One-Hot Encoding-ის გამოყენებით. Store და Dept სვეტები ამ ეტაპზე განიხილება როგორც რიცხვითი ID-ები Random Forest-ისთვის.

მონაცემთა გაყოფა (ქრონოლოგიური): train_merged მონაცემთა ნაკრები ქრონოლოგიურად იყოფა სამ ნაწილად:
სასწავლო ნაკრები (Training Set): მონაცემების 60%.
ვალიდაციის ნაკრები (Validation Set): მონაცემების მომდევნო 20%.
ლოკალური ტესტირების ნაკრები (Local Test Set): მონაცემების ბოლო 20%. ეს გაყოფა უზრუნველყოფს, რომ ჩვენი შეფასება რეალისტური იყოს დროითი სერიების პროგნოზირებისთვის, რადგან მოდელი ყოველთვის ახდენს პროგნოზირებას მომავალი მონაცემებისთვის.


RandomForest_Training_&_Evaluation Run
ეს ეტაპი მოიცავს Random Forest მოდელის სწავლებას და მის შეფასებას შიდა ვალიდაციისა და ლოკალური ტესტირების ნაკრებებზე.
მოდელის ინსტანცირება: გამოიყენება sklearn.ensemble.RandomForestRegressor 100 ესტიმატორით (n_estimators=100). n_jobs=-1 უზრუნველყოფს ყველა ხელმისაწვდომი CPU ბირთვის გამოყენებას.
ტრენინგი: მოდელი წვრთნება სასწავლო ნაკრებზე (X_train, y_train).
პროგნოზირება: პროგნოზები კეთდება როგორც ვალიდაციის, ისე ლოკალური ტესტირების ნაკრებებზე. ყველა პროგნოზი, რომელიც უარყოფითია, იცვლება 0-ით, რადგან გაყიდვები არ შეიძლება იყოს უარყოფითი.
შეფასება: გამოითვლება WMAE და MAE ვალიდაციისა და ლოკალური ტესტირების ნაკრებებზე. IsHoliday დროშა გამოიყენება WMAE-ის გამოსათვლელად სწორი წონების მისაცემად.


მოდელის პირველი გაშვების შედეგები შიდა ვალიდაციისა და ლოკალური ტესტირების ნაკრებებზე:

Trainig WMAE: 3335.2397
Training MAE: 2916.1239

Validation WMAE:  2543.5857
Validation MAE:  2564.8268

Evaluating on Local Test Set...
Local Test WMAE: 3035.2597
Local Test MAE: 2816.7839


აღსანიშნავია, რომ ლოკალური ტესტირების ნაკრებზე მიღებული WMAE უკეთესია (დაბალია), ვიდრე ვალიდაციის ნაკრებზე მიღებული. ეს შეიძლება გამოწვეული იყოს მონაცემთა გაყოფის სპეციფიკით ან იმით, რომ ლოკალური ტესტირების პერიოდი (თუმცა უფრო გვიანია) შესაძლოა ნაკლებად რთულ დღესასწაულებს ან ანომალიებს შეიცავს, ვიდრე ვალიდაციის პერიოდი. ეს არის კარგი საწყისი წერტილი შემდგომი გაუმჯობესებისთვის.

——————————————————————————————————————-
მეორე ექსპეტიმენტი RandomForest-ზე, გაუმჯობესება!

CPI-სა და Unemployment-ში გამოტოვებული მნიშვნელობების დახვეწილი დამუშავება:
ცვლილება: NaN (გამოტოვებული) მნიშვნელობები ამ სვეტებში ახლა ივსება დახვეწილი სტრატეგიით: მონაცემები ჯგუფდება Store-ის მიხედვით, შემდეგ გამოიყენება ffill() (წინა მნიშვნელობით შევსება) და bfill() (შემდეგი მნიშვნელობით შევსება). ნებისმიერი დარჩენილი NaN უსაფრთხოების მიზნით ივსება გლობალური მედიანით.
გაუმჯობესება: ეს მიდგომა იყენებს მაკროეკონომიკური ინდიკატორების დროით სერიულ ბუნებას, ავსებს ხარვეზებს მაღაზიის რეგიონისთვის ცნობილი უახლოესი  მნიშვნელობით, რაც უფრო ზუსტია, ვიდრე უბრალო გლობალური მედიანით შევსება.

დროითი სერიების მოწინავე მახასიათებლების შემოტანა:
ცვლილება: დაემატა რამდენიმე ახალი მახასიათებელი, რომლებიც უშუალოდ ასახავს გაყიდვების ტენდენციებსა და სეზონურობას.
Lag_Weekly_Sales: წინა კვირის გაყიდვები იმავე მაღაზიისა და დეპარტამენტისთვის.
Lag52_Weekly_Sales: გაყიდვები წინა წლის იმავე კვირიდან (52 კვირით ადრე) იმავე მაღაზიისა და დეპარტამენტისთვის, რაც წლიურ სეზონურობას ითვალისწინებს.
RollingMean_4W_Sales: ბოლო 4 კვირის გაყიდვების მოძრავი საშუალო (გადატანილია ერთი კვირით წინ მონაცემთა გაჟონვის თავიდან ასაცილებლად).
RollingStd_4W_Sales: ბოლო 4 კვირის გაყიდვების მოძრავი სტანდარტული გადახრა (ასევე გადატანილი).
გაუმჯობესება: ეს მახასიათებლები Random Forest მოდელს აწვდის ძლიერ ინფორმაციას ტრენდების აღბეჭდვისთვის, სეზონურობის ნაწილობრივი აღბეჭდვისთვის, რაც ფუნდამენტურია ზუსტი დროითი სერიების პროგნოზირებისთვის. ლაგირებით წარმოქმნილი გამოტოვებული მნიშვნელობები შეივსო 0-ით.

ვერსია 2.0-ის შედეგები (ახალი მახასიათებლებით და დახვეწილი წინასწარი დამუშავებით):


Training WMAE: 2235.2397
Trainig MAE: 2113.1229

ვალიდაციის WMAE: 1873.3626
ვალიდაციის MAE: 1692.2703

ლოკალური ტესტის WMAE: 1578.4943
ლოკალური ტესტის MAE: 1407.5672

რაოდენობრივი გაუმჯობესება:

ვალიდაციის WMAE გაუმჯობესდა დაახლოებით 38.3%-ით (3035-დან 1873-მდე).\

ლოკალური ტესტის WMAE გაუმჯობესდა დაახლოებით 38.5%-ით (2565-დან 1578-მდე).

ჯერჯერობით წინსვლა გვაქ შეგვიძლია ვთქვათ

------------------------------------------------
მესამე ექსპეტიმენტი RandomForest-ზე, გაუმჯობესება!

გაყოფის პრინციპი შენარჩუნებულია, მაგრამ დამატებულია "პარასკევის გასწორება" (Friday Alignment). ეს ნიშნავს, რომ გაყოფის თარიღები ზუსტდება უახლოეს პარასკევზე. ეს კრიტიკულად მნიშვნელოვანია ყოველკვირეული გაყიდვების მონაცემებისთვის, რათა ვალიდაციის და ტესტის ნაკრებებმა ზუსტად დაიწყოს ახალი სრული კვირა, რაც ხელს უშლის მონაცემთა გაჟონვას და უზრუნველყოფს უფრო რეალისტურ შეფასებას.

ციკლური თარიღის მახასიათებლები: დამატებულია ახალი მახასიათებლები: Month_sin და Month_cos. ეს ტრანსფორმაციები Random Forest-ს აძლევს უკეთეს საშუალებას, გაიგოს სეზონურობა და თვეების ციკლური ბუნება, რაც ხშირად აუმჯობესებს პროგნოზირების სიზუსტეს.

Weekly_Sales-ში უარყოფითი მნიშვნელობები კვლავ ნულდება.

MarkDown სვეტებში NaN მნიშვნელობები კვლავ ნულდება.

CPI და Unemployment სვეტებში NaN მნიშვნელობები ივსება ffill() და bfill()-ით მაღაზიის მიხედვით, შემდეგ კი დარჩენილი NaN-ები მედიანით.

წინასწარი დამუშავების Pipeline: ColumnTransformer გამოიყენება კატეგორიული (Type სვეტის One-Hot Encoding) და რიცხვითი მახასიათებლების დასამუშავებლად.

RandomizedSearchCV გამოიყენება Random Forest-ის ოპტიმალური ჰიპერპარამეტრების მოსაძებნად. დროითი სერიების ვალიდაციისთვის გამოიყენება TimeSeriesSplit.

უბრალოდ ის არი რო, random_search.fit(X_train, y_train) ამას ანდომებს უაზროდ ბევრ დროს რა, ხოდა ჯერ ვერ მივხვდი რატომ.

ნუ დაამთავრა და შედეგი აჩვენა ასეთი:

Training WMAE: 2246.1327
Trainig MAE: 1913.5343

Validation WMAE: 1805.7305 (Improved!)
Validation MAE: 1615.0387 (Improved!)

Local Test WMAE: 1490.7268 (Improved!)
Local Test MAE: 1307.3495 (Improved!)



---------------------------------------------------
მეოთხე ექსპერიმენტი

მონაცემთა მომზადების პროცესი

მონაცემთა მომზადება შესრულდა მკაცრი ნაბიჯ-ნაბიჯ პრინციპით, რათა უზრუნველყოფილიყო მონაცემთა მაღალი ხარისხი და მოდელის ეფექტურობა.

1. მონაცემთა გაერთიანება და გაწმენდა

train.csv, stores.csv და features.csv მონაცემთა ნაკრებები გაერთიანდა ერთიან DataFrame-ში.
მონაცემთა გაწმენდა მოიცავდა დუბლირებული IsHoliday კოლონების დამუშავებას.

2. დროითი მახასიათებლების ინჟინერია

მონაცემთა ნაკრებს დაემატა ახალი, დროითი მახასიათებლები:
თარიღის კომპონენტები: Year, Month, Day, DayOfWeek, WeekOfYear, Quarter.
დროითი დროშები: IsWeekend, IsMonthStart, IsMonthEnd, IsQuarterStart, IsQuarterEnd.
დროითი პროგრესი: DaysFromStart, WeeksFromStart.
ეს მახასიათებლები აუმჯობესებს მოდელის უნარს, ამოიცნოს დროითი ტენდენციები და სეზონურობა.

3. მონაცემთა ანალიზი და ვიზუალიზაცია

ჩატარდა მონაცემთა სიღრმისეული ანალიზი:
უარყოფითი გაყიდვები: გამოვლინდა და გაანალიზდა უარყოფითი გაყიდვების მქონე ჩანაწერები.
გაყიდვების განაწილება: შესწავლილია გაყიდვების განაწილება (ჰისტოგრამები) და გაყიდვები მაღაზიის ტიპის მიხედვით.
დროითი ტენდენციები: ვიზუალიზებულია საშუალო ყოველთვიური გაყიდვები დროთა განმავლობაში.

4. აუტლაიერების (Outlier) დამუშავება

გამოვლინდა და გაანალიზდა აუტლაიერები. მათ დასამუშავებლად გამოყენებული იქნა ბიზნეს-კონტექსტზე მორგებული ზღვრები, რომლებიც განსხვავდება მაღაზიის ტიპისა (A, B, C) და დღესასწაულის სტატუსის მიხედვით (IsHoliday).
A ტიპის მაღაზიები: ზღვარი დაწესდა ნორმალური და სადღესასწაულო გაყიდვებისთვის.
B და C ტიპის მაღაზიები: ზღვრები დაწესდა შესაბამისი ზომისა და გაყიდვების პოტენციალის გათვალისწინებით.
მონაცემთა ნაკრებიდან ამოღებულია დაახლოებით 16,000-ზე მეტი აუტლაიერი.

5. ჭარბი (Redundant) მახასიათებლების მოშორება

გაანალიზდა და მოშორდა ჭარბი ან მაღალი კორელაციის მქონე მახასიათებლები (მაგ., DaysFromStart, WeekOfYear, Quarter, Year, IsQuarterStart/End, Day), რათა შენარჩუნებულიყო მხოლოდ ყველაზე ინფორმაციული და ინტერპრეტაციული მახასიათებლები.

6. კატეგორიული ცვლადების კოდირება

Type კოლონა (რომელიც შეიცავს A, B, C კატეგორიებს) გადაკოდირდა რიცხვით ფორმატში Label Encoding-ის გამოყენებით. ეს ეტაპი სასიცოცხლოდ მნიშვნელოვანია Random Forest-ის მოდელის გაწვრთნისთვის.
დროითი გაყოფა (Time Series Split)

პროგნოზირების მოდელის სიზუსტის შესამოწმებლად რეალურ სამყაროში, გამოყენებულია დროითი გაყოფის სტრატეგია:
მთლიანი მონაცემების დაყოფა: მონაცემები დაიყო ქრონოლოგიური პრინციპით 80% (ტრეინინგი), 10% (ვალიდაცია) და 10% (ტესტირება).
თარიღის გაყოფის წერტილები:
ტრეინინგი: 2010−02−05 - 2011−04−01
ვალიდაცია: 2011−04−08 - 2011−05−27
ტესტირება: 2011−06−03 - 2012−10−26



დროითი გაყოფა (Time Series Split)

პროგნოზირების მოდელის სიზუსტის შესამოწმებლად რეალურ სამყაროში, გამოყენებულია დროითი გაყოფის სტრატეგია:
მთლიანი მონაცემების დაყოფა: მონაცემები დაიყო ქრონოლოგიური პრინციპით 80% (ტრეინინგი), 10% (ვალიდაცია) და 10% (ტესტირება).
თარიღის გაყოფის წერტილები:
ტრეინინგი: 2010−02−05 - 2011−04−01
ვალიდაცია: 2011−04−08 - 2011−05−27
ტესტირება: 2011−06−03 - 2012−10−26
შედეგები: ეს მეთოდი უზრუნველყოფს, რომ მოდელი წვრთნის წარსულ მონაცემებზე და აკეთებს პროგნოზს მომავალ პერიოდებზე, რაც ასახავს რეალურ პროგნოზირების სცენარს.


მოდელის პარამეტრები:

n_estimators: 200 (ხეების რაოდენობა)
max_depth: 15 (ხეების მაქსიმალური სიღრმე)
min_samples_split: 5
min_samples_leaf: 3
n_jobs: -1 (ყველა CPU ბირთვის გამოყენება)




ტრენინგი:
WMAE $2,050.71
MAE $1,874.79

ვალიდაცია:
WMAE $2,697.34
MAE $2,697.34

🎯 FINAL TESTING RESULTS (Random Forest):

Training (Train+Val): MAE=$1,830.35 | RMSE=$3,825.62 | WMAE=$1,997.00

Testing (Test only): MAE=$2,143.21 | RMSE=$4,037.71 | WMAE=$2,195.66

--------------------------------------------------
# model_exp_Prophet_1

მოდელი გატრენინგდა ისტორიული ტრენინგისა და ვალიდაციის მონაცემების კომბინირებულ ნაკრებზე, შემდეგ კი შეფასდა ცალკე, უცნობ სატესტო მონაცემთა ნაკრებზე.

train_ts და val_ts მონაცემთა ნაკრებები გაერთიანებულია უფრო დიდი combined_train_val_df-ის შესაქმნელად, რომელიც გამოიყენება მოდელის საბოლოო ტრენინგისთვის.

სვეტების სახელები (Date-დან ds-ზე, Weekly_Sales-დან y-ზე) მორგებულია Prophet-ის მოთხოვნებთან.

test_ts მონაცემთა ნაკრები მზადდება პროგნოზირებისთვის.

სკრიპტი იმეორებს combined_train_val_df-ში არსებულ თითოეულ უნიკალურ (Store, Dept) კომბინაციას.

თითოეული სერიისთვის, Prophet მოდელი ინიციალიზდება კონკრეტული კონფიგურაციებით (წლიური/ყოველკვირეული სეზონურობა, მულტიპლიკაციური სეზონურობის რეჟიმი, ხაზოვანი ზრდა).

ყველა შესაბამისი წინასწარ დამუშავებული ფუნქცია (მაგ., IsHoliday, Temperature, Fuel_Price, CPI, Unemployment, Type_Encoded, Size) დამატებულია, როგორც დამატებითი რეგრესორები Prophet მოდელში.

დიაგნოსტიკური ანალიზები:

სადღესასწაულო კვირის ანალიზი: გვაწვდის სადღესასწაულო კვირების რაოდენობასა და პროცენტებს როგორც ტრენინგის, ასევე ტესტირების მონაცემთა ნაკრებებში.

თარიღის დაფარვა: ამოწმებს ტრენინგისა და ტესტირების პერიოდების თარიღების დიაპაზონებს და უწყვეტობას.

პროგნოზების დეტალური ანალიზი (ტესტის ნაკრები):

იქმნება predictions_comparison DataFrame, რომელიც აერთიანებს რეალურ გაყიდვებს, პროგნოზირებულ გაყიდვებს და გამოთვლილ შეცდომებს სატესტო ნაკრებისთვის.



# შედეგები

საბოლოო ტესტირების ფაზა ავლენს მოდელის მუშაობის მნიშვნელოვან ცვლილებას ვალიდაციის ფაზასთან შედარებით:
rophet მოდელის აგრეგირებული შედეგები (საბოლოო ტესტირების ფაზა):

ტრენინგი (Train+Val In-Sample):

MAE: $2,463.04

RMSE: $6,697.10

WMAE: $2,759.62

ტესტირება (Test Out-of-Sample):

MAE: $15,535.41

RMSE: $26,635.84

WMAE: $15,697.87

ტრენინგი: 2010-02-05-დან 2012-07-13-მდე

ტესტირება: 2012-07-20-დან 2012-10-26-მდე

დროის სერიების უწყვეტი დაყენება დადასტურებულია.

ძააააააალიან ცუდი შედეგია. ძააააალიან.



-------------------------------------------------------
# model_exp_prophet_2


მონაცემთა წინასწარი დამუშავების პაიპლაინი:

მონაცემთა ჩატვირთვა და გაერთიანება: აერთიანებს train, stores და features მონაცემთა ნაკრებებს Store-სა და Date-ის საფუძველზე.

_merge_data (მონაცემთა გაერთიანება): აერთიანებს ყველა მონაცემთა წყაროს (train, features, stores) ერთიან DataFrame-ში Store-სა და Date-ის საფუძველზე.
_clean_data (მონაცემთა გაწმენდა): აგვარებს მონაცემთა შერწყმის შედეგად წარმოქმნილ IsHoliday სვეტის დუბლირებას. ის აერთიანებს IsHoliday_x და IsHoliday_y სვეტებს ერთ ბინარულ IsHoliday სვეტად.

ფიჩერების ინჟინერია (Feature Engineering)

ეს ეტაპი გადამწყვეტია ნედლი მონაცემებიდან ღირებული ინფორმაციის ამოსაღებად:
_feature_engineer_dates (დროითი ფიჩერები): Date სვეტიდან ამოიღებს დეტალურ დროით ფიჩერებს:
სეზონურობა: Month (თვე), DayOfWeek (კვირის დღე), WeekOfYear (წლის კვირა).
კალენდარული მოვლენები: IsWeekend (შაბათ-კვირა), IsMonthStart (თვის დასაწყისი), IsMonthEnd (თვის დასასრული).
ტრენდი: DaysFromStart და WeeksFromStart აფიქსირებს დროის გასვლას მონაცემთა ნაკრების დაწყებიდან, რაც მნიშვნელოვანია გრძელვადიანი ტრენდების დასაფიქსირებლად.
_feature_engineer_holidays (სადღესასწაულო ფიჩერები): ქმნის ბინარულ ინდიკატორებს ძირითადი ამერიკული დღესასწაულებისთვის (Super Bowl, Labor Day, Thanksgiving, Christmas) და ასევე უფრო ფართო სადღესასწაულო პერიოდებისთვის (IsMajorHoliday, IsHolidayMonth, IsBackToSchool).
_feature_engineer_store_type (მაღაზიის ტიპის დაშიფვრა): Type სვეტს (A, B, C) აკონვერტირებს ორი მეთოდით:
Type_Encoded (Label Encoding)
Type_A, Type_B, Type_C (One-Hot Encoding)

მონაცემთა გაფილტვრა და დამუშავება

_remove_outliers (აუტლაიერების ამოღება): ამ მეთოდს აქვს წინასწარ განსაზღვრული ბარიერები (weekly_sales_outlier_thresholds) თითოეული მაღაზიის ტიპისთვის (A, B, C). ის აშორებს Weekly_Sales-ის იმ მნიშვნელობებს, რომლებიც ამ ბარიერებს სცილდება.
_remove_markdowns (MarkDown-ების ამოღება): პარამეტრის remove_markdowns=True-ის შემთხვევაში, ეს მეთოდი აშორებს MarkDown1-დან MarkDown5-მდე სვეტებს, რაც მნიშვნელოვანია თუ მოდელის ტრენინგი არ საჭიროებს ამ ტიპის სარეკლამო მონაცემებს.
_remove_lag_features (Lag ფიჩერების ამოღება): მიუხედავად იმისა, რომ ამ კოდში lag ფიჩერები არ არის შექმნილი, ეს მეთოდი უზრუნველყოფს მათ ამოღებას თუკი enable_lag_features=False.
_handle_missing_values (გამოტოვებული მნიშვნელობების შევსება): ავსებს დარჩენილ ყველა რიცხვით სვეტში გამოტოვებულ მნიშვნელობებს ნულით.
_remove_redundant_features (ჭარბი ფიჩერების ამოღება): აშორებს ისეთ სვეტებს, როგორიცაა Year და Day, რომლებიც ზედმეტია სხვა დროითი ფიჩერების არსებობის პირობებში.


დროის დატის გაყოფა: 
გაყოფის კოეფიციენტი: დაახლოებით 80% ტრენინგისთვის და 20% ვალიდაციისთვის.

გაყოფის თარიღი: 2012-04-13 00:00:00

ტრენინგის მონაცემები: 337,256 ჩანაწერი, 2010-02-05 00:00:00-დან 2012-04-13 00:00:00-მდე.

ვალიდაციის მონაცემები: 84,314 ჩანაწერი, 2012-04-13 00:00:00-დან 2012-10-26 00:00:00-მდე.

სერიების დონის Prophet მოდელირება
Prophet მოდელები ინდივიდუალურად გატრენინგდა ტრენინგის მონაცემებში აღმოჩენილი თითოეული უნიკალური მაღაზია-განყოფილების კომბინაციისთვის. ეს მიდგომა იძლევა მაღალპერსონალიზებული პროგნოზების საშუალებას, რომლებიც მორგებულია თითოეული გაყიდვების სერიის სპეციფიკურ ნიმუშებზე.

იდენტიფიცირებული სერიების ჯამური რაოდენობა: 3082 უნიკალური მაღაზია-განყოფილების კომბინაცია.

მინიმალური დაკვირვებები: სერიას სჭირდებოდა მინიმუმ 50 დაკვირვება, რათა Prophet-ის მიერ გატრენინგებულიყო. ნაკლები დაკვირვების მქონე სერიები გამოტოვებული იყო, ხოლო მათი ვალიდაციის გაყიდვები პროგნოზირებულ იქნა როგორც 0.

იდენტიფიცირებული სერიების ჯამური რაოდენობა 3082

გატრენინგებული სერიები 2615

გამოტოვებული სერიები (ძალიან მოკლე) 467

ვერ გატრენინგდა 0

ვერ განახორციელა პროგნოზი (ვალიდაცია) 0

ტრენინგი:
WMAE: 1439.43

RMSE: 2831.60

MAE: 1388.21

R²: 0.9246

ვალიდაცია:
WMAE: 6178.43 ⭐

RMSE: 19636.79

MAE: 6141.23

R²: -1.1195

მაღალი ოვერფიტინგი.
უარყოფითი R² ნიშნავს, რომ მოდელის პროგნოზები უარესია, ვიდრე უბრალოდ ვალიდაციის მონაცემების საშუალოს პროგნოზირება.
--------------------------------------------------------------
## model_exp_LightGBM_1.

იგივე WalmartPreprocessingPipeline იქნა გამოყენებული, რაც Prophet ექსპერიმენტში. ეს უზრუნველყოფს თანმიმდევრულობას ფიჩერების ინჟინერიასა და მონაცემთა გაწმენდაში. ნაბიჯები მოიცავს:

მონაცემთა ჩატვირთვა და გაერთიანება

მონაცემთა გაწმენდა (duplicate IsHoliday სვეტების მოგვარება)

თარიღის ფიჩერების ინჟინერია (Year, Month, Day, DayOfWeek, WeekOfYear, Quarter, IsWeekend, IsMonthStart/End, Days/WeeksFromStart)

დღესასწაულის ფიჩერების ინჟინერია (IsSuperBowlWeek, IsLaborDayWeek, IsThanksgivingWeek, IsChristmasWeek, IsMajorHoliday, IsHolidayMonth, IsBackToSchool)

კატეგორიული დაშიფვრა: Type სვეტი დაშიფრულია One-hot (Type_A, Type_B, Type_C) და Label (Type_Encoded) მეთოდებით. LightGBM-ისთვის, Type_Encoded და One-Hot კოდირებული სვეტები გამოყენებული იქნება როგორც ცალკეული ფიჩერები. Store და Dept სვეტები ასევე იქნება გამოყენებული როგორც კატეგორიული ფიჩერები.

Lag ფიჩერები (გამორთულია): რჩება გამორთული ოვერფიტინგის თავიდან ასაცილებლად.

მარკდაუნის ფიჩერების ამოღება: ყველა MarkDown სვეტი ამოღებულია.

ჭარბი ფიჩერების ამოღება: ზოგიერთი დროითი ფიჩერი, რომელიც შესაძლოა ჭარბი იყოს.

# დროითი მონაცემების დაყოფა

მონაცემთა ნაკრები დაიყო ტრენინგისა და ვალიდაციის ნაკრებებად ზუსტად ისე, როგორც წინა ექსპერიმენტში, დროითი გაყოფის გამოყენებით:

გაყოფის კოეფიციენტი: დაახლოებით 80% ტრენინგისთვის, 20% ვალიდაციისთვის.

გაყოფის თარიღი: 2012-04-13 00:00:00

ტრენინგის მონაცემები: 2010-02-05 00:00:00-დან 2012-04-13 00:00:00-მდე.

ვალიდაციის მონაცემები: 2012-04-13 00:00:00-დან 2012-10-26 00:00:00-მდე.

#  LightGBM მოდელი
Prophet-ისგან განსხვავებით, რომელიც მოდელებს ინდივიდუალურად ატრენინგებს თითოეული სერიისთვის, LightGBM-ის მიდგომა გულისხმობს ერთი გლობალური მოდელის ტრენინგს, რომელიც სწავლობს ყველა Store-Department კომბინაციის მონაცემებს ერთდროულად. Store და Dept ID-ები შეყვანილია როგორც კატეგორიული ფიჩერები, რაც მოდელს საშუალებას აძლევს ისწავლოს თითოეული მაღაზიისა და განყოფილების სპეციფიკური ნიმუშები.

{
    'objective': 'regression_l1', # MAE ობიექტური ფუნქცია, უფრო მდგრადია აუტლაიერების მიმართ
    'metric': 'mae',
    'n_estimators': 1000,
    'learning_rate': 0.05,
    'feature_fraction': 0.8,
    'bagging_fraction': 0.8,
    'bagging_freq': 1,
    'lambda_l1': 0.1,
    'lambda_l2': 0.1,
    'num_leaves': 31,
    'verbose': -1, 
    'n_jobs': -1,
    'seed': 42,
    'boosting_type': 'gbdt',
    'early_stopping_round': 50 # შეაჩერებს ტრენინგს, თუ ვალიდაციის მეტრიკა 50 რაუნდის განმავლობაში არ გაუმჯობესდება
}

ფიჩერების შერჩევა
Prophet-ისგან განსხვავებით, LightGBM-ს შეუძლია უშუალოდ გამოიყენოს მეტი ფიჩერი. Date და Weekly_Sales სვეტების გარდა, LightGBM მოდელი იყენებს ყველა სხვა სვეტს, როგორც საპროგნოზო ფიჩერებს:

კატეგორიული ფიჩერები: Store, Dept, Type_A, Type_B, Type_C, Type_Encoded, Month, DayOfWeek, WeekOfYear, IsHoliday, IsWeekend, IsMonthStart, IsMonthEnd, IsSuperBowlWeek, IsLaborDayWeek, IsThanksgivingWeek, IsChristmasWeek, IsMajorHoliday, IsHolidayMonth, IsBackToSchool.

რიცხვითი ფიჩერები: Size, Temperature, Fuel_Price, CPI, Unemployment, DaysFromStart, WeeksFromStart.

# შედეგები

აგრირებული ტრენინგის მეტრიკები (In-Sample)

WMAE: 1554.33

RMSE: 3251.47

MAE: 1464.41

R²: 0.9481

აგრირებული ვალიდაციის მეტრიკები (Out-of-Sample)

WMAE: 1663.04 ⭐ (კონკურსის მთავარი მეტრიკა)

RMSE: 3359.20

MAE: 1654.74

R²: 0.9449

რა თქმა უნდა! შესანიშნავი შედეგებია. LightGBM მოდელმა აჩვენა ძალიან კარგი შესრულება, დაბალი WMAE-ით და მაღალი R²-ით, მინიმალური ოვერფიტინგით.



# model_exp_FX_Prophet.ipynb

ჩვენი მიზანია გამოვასწოროთ წინაზე დაშვებული შეცდომა.
Prophet მოდელი დროითი სერიების პრობლემას ფუნდამენტურად განსხვავებულად უდგება, ვიდრე, მაგალითად, Random Forest. Random Forest-ისთვის ნებისმიერი თარიღთან დაკავშირებული მახასიათებელი (თვე, კვირის დღე, წელი და ა.შ.) ცალკე "ფიჩერია". Prophet-ისთვის კი ეს დროითი კომპონენტები უკვე შიდა მექანიზმებშია ჩაშენებული და ის თავად იყენებს მათ სეზონურობის, ტრენდისა და დღესასწაულების დასადგენად.

ძირითადი პრობლემები, რომლებიც გვქონდა და როგორ გადავწყვიტეთ:

დღესასწაულების არაადეკვატური დამუშავება: დღესასწაულები, როგორიცაა სუპერ ბოული, შრომის დღე, მადლიერების დღე და შობა, მნიშვნელოვნად მოქმედებს Walmart-ის გაყიდვებზე. წინა მიდგომა სრულად არ ითვალისწინებდა მათ ზეგავლენას.
გადაწყვეტა: Prophet-ს აქვს ჩაშენებული დღესასწაულების დამუშავების ფუნქცია. ჩვენ შევქმენით holidays_df DataFrame, რომელიც შეიცავს Walmart-ისთვის რელევანტურ ძირითად ამერიკულ დღესასწაულებს მათი თარიღებით. ეს Prophet-ს საშუალებას აძლევს, ცალ-ცალკე ისწავლოს თითოეული დღესასწაულის გავლენა გაყიდვებზე, რაც ბევრად უფრო ეფექტურია, ვიდრე უბრალოდ IsHoliday სვეტის, როგორც ზოგადი რეგრესორის, გამოყენება.

მონაცემებში არსებული არარეალურად მაღალი ან დაბალი გაყიდვების მაჩვენებლები ამახინჯებდა მოდელის სწავლის პროცესს და ამცირებდა სიზუსტეს.
გადაწყვეტა: ჩვენ დავამატეთ ლოგიკა, რომელიც უზრუნველყოფს, რომ Weekly_Sales (y) მნიშვნელობები არ იყოს უარყოფითი. გაყიდვები ვერასდროს იქნება უარყოფითი, ამიტომ ნებისმიერი ასეთი მნიშვნელობა გამოსწორდა და გახდა 0.

დროითი გაყოფის ნაკლოვანებები: დროითი სერიების სწორი გაყოფა (ტრენინგისა და ვალიდაციის ნაკრებებად) აუცილებელია მოდელის განზოგადების შესაძლებლობის შესაფასებლად და მონაცემთა გაჟონვის (data leakage) თავიდან ასაცილებლად.
გადაწყვეტა: მონაცემები იყოფა ტრენინგისა და ვალიდაციის ნაკრებებად მკაცრად დროის მიხედვით (80/20 თანაფარდობით). ეს უზრუნველყოფს, რომ მოდელი შეფასდეს მხოლოდ იმ მონაცემებზე, რომლებიც ტრენინგის დროს არ უნახავს.

რა შევცვალეთ კოდში?
ჩვენ განვახორციელეთ შემდეგი გაუმჯობესებები, რათა სრულად გამოგვეყენებინა Prophet-ის შესაძლებლობები:

WalmartProphetPreprocessingPipeline (მონაცემთა წინასწარი დამუშავება):

მონაცემთა ჩატვირთვა და შერწყმა: მონაცემები (train.csv, features.csv, stores.csv) საგულდაგულოდ იტვირთება და ერთიანდება Store, Date, IsHoliday და Store სვეტების მიხედვით. Date სვეტები გარდაიქმნება datetime ტიპად.

დროითი გაყოფა (create_temporal_split): მონაცემები იყოფა 80% ტრენინგად და 20% ვალიდაციად, უნიკალური თარიღების საფუძველზე, რათა თავიდან იქნას აცილებული მომავლის მონაცემების "დაბინძურება" ტრენინგის ნაკრებში.

Prophet-ისთვის საჭირო ფორმატირება (transform):სვეტები გადაირქმევა Date-დან ds-ზე და Weekly_Sales-დან y-ზე, რაც Prophet-ის სტანდარტული მოთხოვნაა. y (Weekly_Sales) მნიშვნელობები გარდაიქმნება ისე, რომ არ იყოს უარყოფითი, რადგან გაყიდვები ვერ იქნება 0-ზე ნაკლები.

დღესასწაულების დამუშავება (fit):
შევქმენით self.holidays_df DataFrame, რომელიც მოიცავს ძირითად ამერიკულ დღესასწაულებს (სუპერ ბოული, შრომის დღე, მადლიერების დღე, შობა), რომლებსაც Prophet გამოიყენებს გაყიდვებზე მათი გავლენის შესასწავლად. ეს გადამწყვეტია სადღესასასწაულო პერიოდებში სიზუსტის გასაუმჯობესებლად.

მოდელი თითოეული სერიისთვის: ჩვენ ვწვრთნით ცალკე Prophet მოდელს თითოეული უნიკალური (Store, Dept) კომბინაციისთვის. ეს ნიშნავს, რომ თუ გვაქვს 3,313 უნიკალური კომბინაცია, გვეყოლება 2,966 წარმატებით გაწვრთნილი მოდელი (ზოგისთვის მონაცემები არასაკმარისია). ეს მიდგომა საშუალებას აძლევს მოდელს, ისწავლოს თითოეული კონკრეტული სერიის უნიკალური ტენდენციები, სეზონურობა და დღესასწაულების ეფექტები.

Prophet-ის პარამეტრები: yearly_seasonality=True და weekly_seasonality=True ააქტიურებს Prophet-ის ჩაშენებულ ფუნქციებს ყოველწლიური და ყოველკვირეული სეზონურობის ავტომატურად ამოსაცნობად. holidays=holidays_df აერთიანებს ჩვენს მიერ განსაზღვრულ დღესასწაულებს მოდელის პროცესში.

პროექტის განახლებულმა ვერსიამ მნიშვნელოვნად გააუმჯობესა Prophet მოდელის შესრულება.

🎯 EXPERIMENT PROPHET RESULTS SUMMARY
============================================================
📊 Training Metrics:
   Training WMAE: $1,166.70

📊 Validation Metrics:
   WMAE (Competition Metric): $1,669.21
   MAE: $1,626.37
   RMSE: $3,725.00

📊 Holiday Breakdown:
   Holiday MAE: $1,979.10 (2966 samples)
   Non-Holiday MAE: $1,613.74 (82843 samples)

📊 Model Statistics:
   Successful models trained: 2,966
   Store-Dept combinations: 3,313
   No training errors calculated
------------------------------------------------------------
# model_exp_TFT_FX.ipynb

Temporal Fusion Transformer არის ღრმა სწავლის მოდელი, რომელიც აგებულია Transformer არქიტექტურის საფუძველზე, მაგრამ ადაპტირებულია დროითი სერიების მონაცემებისთვის. 

ეს მოდელი გამოირჩევა შერჩევითი სწავლის მექანიზმით (Gating Mechanism) და ყურადღების მექანიზმით (Attention Mechanism).

ამ მოდელს პროფეტისგან განსხვავებით შეუძლია გამოიყენოს სტატიკური მახასიათებლები, მომავალში ცნობილი მახასიათებლები და მომავალში უცნობი მახასიათებლები.

მრავალსერიული პროგნოზირება (Multi-Series Forecasting): TFT-ს შეუძლია ერთდროულად ისწავლოს და გააკეთოს პროგნოზი მრავალი დაკავშირებული დროითი სერიისთვის. ჩვენს კოდში: unique_id სვეტის შექმნა df["unique_id"] = df["Store"].astype(str) + "_" + df["Dept"].astype(str) არის ამის მთავარი გამოხატულება. NeuralForecast ბიბლიოთეკა იყენებს ამ unique_id-ს, რათა დაადგინოს თითოეული ინდივიდუალური დროითი სერია (მაგ. "მაღაზია 1, განყოფილება 1"). მოდელი ერთდროულად სწავლობს ყველა ამ სერიიდან, რაც აუმჯობესებს პროგნოზების სიზუსტეს.

ახლა უშუალოდ რა გავაკეთე:
თარიღის ფორმატირება: Date სვეტები გარდაიქმნება datetime ფორმატში, რაც აუცილებელია დროითი სერიების ანალიზისა და შემდგომი ფიჩერ ინჟინერიისთვის.

მონაცემთა გაერთიანება (Merge): სამივე DataFrame ერთიანდება Store და Date სვეტების მიხედვით. ეს ქმნის ერთიან მონაცემთა ბაზას, სადაც თითოეული ჩანაწერი შეიცავს გაყიდვებს, მაღაზიის/განყოფილების იდენტიფიკატორებს და ყველა შესაბამის მახასიათებელს მოცემული თარიღისთვის.

MarkDown სვეტების შევსება: MarkDown სვეტებში NaN მნიშვნელობები ივსება ნულებით (fillna(0)), ვინაიდან NaN აქ სავარაუდოდ ნიშნავს, რომ მოცემულ პერიოდში MarkDown არ ყოფილა.

ნეგატიური გაყიდვების ამოღება: მონაცემთა ნაკრებიდან ამოღებულია ის ჩანაწერები, სადაც Weekly_Sales უარყოფითია, ვინაიდან გაყიდვები არ შეიძლება იყოს უარყოფითი.

დროითი უწყვეტობის უზრუნველყოფა: ეს არის მნიშვნელოვანი ახალი ნაბიჯი. ჩვენ შევქმენით ყველა შესაძლო (Store, Dept, Date) კომბინაცია თითოეული სერიის მინიმალურ და მაქსიმალურ თარიღებს შორის. 

მონაცემთა დახარისხება: საბოლოო train_full DataFrame სორტირდება Date, Store და Dept სვეტების მიხედვით. ეს უმნიშვნელოვანესია დროითი სერიების მოდელებისთვის, რადგან უზრუნველყოფს მონაცემთა სწორ ქრონოლოგიურ თანმიმდევრობას.

80% ტრეინინგი - 20% ვალიდაცია

Feature engeneering:
MultiIndexKeeper: ეს ტრანსფორმატორი აყენებს DataFrame-ის ინდექსს ['Date', 'Store', 'Dept'] MultiIndex-ად. ეს აუცილებელია, რადგან NeuralForecast მუშაობს MultiIndex-იან DataFrame-ებთან. 
DateFeatureCreator (ფიჩერ ინჟინერია): ეს არის ჩვენი ფიჩერ ინჟინერიის ნაწილი. ის იღებს Date სვეტს და ქმნის რამდენიმე ახალ დროით მახასიათებელს:

week: კვირის ნომერი წლის დასაწყისიდან (ან პერიოდის დასაწყისიდან).

sin_13, cos_13:  13-კვირიანი (დაახლოებით კვარტალური) სეზონურობის დასაფიქსირებლად.

sin_23, cos_23: 23-კვირიანი სეზონურობის დასაფიქსირებლად.

ესენი არის "ცნობილი მომავალი მახასიათებლები".

ინფუთები:

unique_id: სერიის იდენტიფიკატორი.

ds: თარიღი/timestamp.

y: სამიზნე ცვლადი (Weekly_Sales).

ყველა სხვა სვეტი: (როგორიცაა Temperature, Fuel_Price, CPI, Unemployment, MarkDownX სვეტები, Type და IsHoliday One-Hot encoded ვერსიები, sin/cos დროითი ფიჩერები) ავტომატურად განიხილება NeuralForecast-ის მიერ, როგორც მახასიათებლები. მოდელი სწავლობს ამ მახასიათებლებს გამოყენებას y-ის პროგნოზირებისთვის.

predict(X): ეს მეთოდი გამოიყენება ახალი პროგნოზების შესაქმნელად

ვალიდაციაზე ჩვენი შედეგები:
WMAE (Competition Metric): $1,421.02

MAE: $1,585.18

RMSE: $3,415.59

Holiday MAE: $868.07 (6,617 samples)

Non-Holiday MAE: $1,642.66 (82,540 samples)

Holiday MAE ($868.07) მნიშვნელოვნად დაბალია Non-Holiday MAE-ზე ($1,642.66).

რატომ? ეს შეიძლება რამდენიმე მიზეზით იყოს განპირობებული:

პროგნოზირებადი ქცევა: დღესასწაულების გაყიდვები შეიძლება იყოს უფრო "პროგნოზირებადი" გარკვეულწილად, რადგან ისინი ხშირად მიჰყვებიან მსგავს ნიმუშებს წლიდან წლამდე (მაგალითად, შავი პარასკევი, შობა). მოდელს შესაძლოა უკეთესი სეზონური ინფორმაცია ჰქონდეს დღესასწაულებისთვის.

--------------------------------------------
model_exp_DLinear_FX

DLinear მოდელის მიმოხილვა: როგორ მუშაობს და რატომ არის კარგი/ცუდი ამ ამოცანისთვის? DLinear (Decomposition Linear) არის Deep Learning არქიტექტურა, რომელიც ეფუძნება დროითი სერიების დეკომპოზიციას. მისი მთავარი იდეაა დროითი სერიის დაყოფა ორ ძირითად, დამოუკიდებელ კომპონენტად: ტრენდული (Trend): მონაცემების გრძელვადიანი, საერთო მიმართულება ან ძირითადი დონე. სეზონური (Seasonal): განმეორებადი, მოკლევადიანი შაბლონები, რომლებიც კონკრეტულ პერიოდებში (მაგ., კვირა, თვე, წელი) ვლინდება. ამ კომპონენტების გამოყოფის შემდეგ, თითოეული მათგანი დამოუკიდებლად მოდელირდება ხაზოვანი ფენების (Linear Layers) გამოყენებით. ეს მიდგომა მიზნად ისახავს რთული და არაწრფივი დამოკიდებულებების გამარტივებას, რაც პროგნოზირების სიზუსტის გაუმჯობესებას უწყობს ხელს

ტრადიციული "თითო-სერიაზე-ერთი-მოდელი" მიდგომებისგან განსხვავებით, DLinear-ს შეუძლია ყველა სერიის საერთო შაბლონების სწავლა ერთდროულად. ეს მას გაცილებით ეფექტურს ხდის დიდი მონაცემთა ნაკრებებისთვის.

ცვლადები როგორიცაა Temperature, Fuel_Price, CPI, Unemployment დიდად არ გამოგვადგება ამ მოდელში, რადგან ვაპირებ გამოვიყენო neural forecast, სადაც მოდელი იღებს unique_id, ds (Date) და y (Weekly_Sales) სვეტებს.

მიუხედავად იმისა, რომ neuralforecast-ის DLinear-ის ძირითადი ვერსია მხოლოდ y-ზე დაყრდნობით პროგნოზირებს, მისი ოპტიმიზებული არქიტექტურა უკეთ აითვისებს y-ში არსებულ ტრენდებსა და სეზონურობას.

Validation WMAE: 1564.401



