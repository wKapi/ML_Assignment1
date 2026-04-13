## House Prices - Advanced Regression Techniques

### კონკურსის მიმოხილვა
House Prices - Advanced Regression Techniques კონკურსის მიზანია სახლების გაყიდვის ფასის პროგნოზი სხვადასხვა მახასიათებლის მიხედვით (მაგალითად: ფართობი, ოთახების რაოდენობა, მდებარეობა და ა.შ.).
შეფასება ხდება Root Mean Squared Log Error (RMSLE) მეტრიკით.

---
## რეპოზიტორიის სტრუქტურა

```
ML_Assignment1/
│
├── model_experiment.ipynb    ← Cleaning, Feature Engineering, Training, MLflow
├── model_inference.ipynb     ← საუკეთესო მოდელის ჩამოტვირთვა, პროგნოზი, submission
├── README.md
```

---

## ფაილების აღწერა

| ფაილი | აღწერა |
|---|---|
| `model_experiment.ipynb`| მონაცემთა დამუშავება, ექსპერიმენტები და მოდელების ტრენინგი |
| `model_inference.ipynb` | საუკეთესო მოდელის გამოყენება და submission გენერაცია |

---


### მონაცემთა გაწმენდა და დამუშავება (Cleaning & Preprocessing)

#### 1. NA მნიშვნელობების დამუშვება
* **რიცხვითი სვეტები (შევავსე მედიანით:** საშუალო არითმეტიკულის ნაცვლად მედიანა ავირჩიე, რადგან ის უფრო მდგრადია outliers-ების მიმართ.
* **კატეგორიული სვეტები გადავიყვანე რიცხვითში** გამოვიყენე get_dummies ფუნქცია. რადგან მოდელები ტექსტს ვერ ამუშავებენ 

#### 2. უსარგებლო სვეტების მოცილება
* **id სვეტი** ეს სვეტი მხოლოდ ინდივიდუალური იდენტიფიკატორია. მას არ გააჩნია სტატისტიკური კავშირი ფასის ალბათობასთან. მისი დატოვება მოდელს დააბნევდა ამიტომ ამოვიღე.

---

### Feature Engineering
შევქმენი ახალი featuree: TotalSF = TotalBsmtSF + 1stFlrSF + 2ndFlrSF ეს აერთიანებს სახლის საერთო ფართობს და აუმჯობესებს მოდელის ხარისხს.

---

### Feature Selection 
გამოყენებული მიდგომები:
- არასაჭირო სვეტების მოშორება (id);
- feature engineering-ის ტესტირება;
აღმოჩნდა რომ ახალი feature მნიშვნელოვნად აუმჯობესებს შედეგს

---

## ტრენინგი და ექსპერიმენტები

გამოვიყენე სამი ძირითადი ალგორითმი: linear regression, decision trees, Random forests
ყველა ექსპერიმენტი ავტვირთე MLflow-ში DagsHub-ზე. თითოეული მოდელისთვის დავლოგე მეტრიკები.

---

### 🔹 Logistic Regression

- mean-ის გამოყენებით rmse= 65000მდე დავიდა
- median-ის გამოყენებით მივიღეთ მცირე გაუმჯობესება.
შედეგები თითქმის იდენტურია. შედეგებიდან გამომდინარე ჩანს რომ არის ძალიან მარტივი მოდელი და არის underfitting-ის პრობლემა.

---

### 🔹 Decision Tree

შევამოწმე სიღრმეებზე 3,4,5,6,7,10:
- depth = 3 underfitting
- depth = 5-6 მივიღე საუკეთესო შედეგი
- depth = 7-10 overfitting
მიღებული შედეგებით დადასტურდა რომ დიდ სიღრმეზე არის overfitting-ის პრობლემა.

---

### 🔹 Random Forest
გავტესტე სხვადასხვა n_estimatorზე (50,100,200,400,500).
 n_estimators = 400
 RMSE ≈ 29,000
საუკეთესო შედეგი მივიღე რანდომ ფორესტით, როცა n_estimator=400ს.

---

## საუკეთესო მოდელი

საუკეთესო მოდელი შეირჩა სამივე მოდელის ექსპერიმენტის შედარებით.
შეირჩა Random forest რადგან:
-ამცირებს overfitting-ს
- უკეთ იჭერს nonlinear დამოკიდებულებებს
- აჩვენა ყველაზე დაბალი rmse (შერჩების მთავარი კრიტერიუმი)
---

## MLflow tracking

ყველა run დარეგისტრირებულია:
 (https://dagshub.com/mkapa22/ML_Assignment1.mlflow/#/experiments/1/runs?searchFilter=&orderByKey=attributes.start_time&orderByAsc=false&startTime=ALL&lifecycleFilter=Active&modelVersionFilter=All+Runs&datasetsFilter=W10%3D)

თითოეულ run-ში დაილოგა:
- პარამეტრები (model, depth, n_estimators)
- მეტრიკები (RMSE)
- დატრენინგებული მოდელის არტეფაქტები

---

