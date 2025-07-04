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
მოდელის ინსტანცირება: გამოიყენება sklearn.ensemble.RandomForestRegressor 100 ესტიმატორით (n_estimators=100) და random_state=42 რეპროდუცირებადობისთვის. n_jobs=-1 უზრუნველყოფს ყველა ხელმისაწვდომი CPU ბირთვის გამოყენებას.
ტრენინგი: მოდელი წვრთნება სასწავლო ნაკრებზე (X_train, y_train).
პროგნოზირება: პროგნოზები კეთდება როგორც ვალიდაციის, ისე ლოკალური ტესტირების ნაკრებებზე. ყველა პროგნოზი, რომელიც უარყოფითია, იცვლება 0-ით, რადგან გაყიდვები არ შეიძლება იყოს უარყოფითი.
შეფასება: გამოითვლება WMAE და MAE ვალიდაციისა და ლოკალური ტესტირების ნაკრებებზე. IsHoliday დროშა გამოიყენება WMAE-ის გამოსათვლელად სწორი წონების მისაცემად.


მოდელის პირველი გაშვების შედეგები შიდა ვალიდაციისა და ლოკალური ტესტირების ნაკრებებზე:
Evaluating on Validation Set...
Validation WMAE: 3035.2597
Validation MAE: 2816.7839

Evaluating on Local Test Set...
Local Test WMAE: 2564.8268
Local Test MAE: 2543.5857


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

Validation WMAE: 1805.7305 (Improved!)

Validation MAE: 1615.0387 (Improved!)

Local Test WMAE: 1490.7268 (Improved!)

Local Test MAE: 1307.3495 (Improved!)




