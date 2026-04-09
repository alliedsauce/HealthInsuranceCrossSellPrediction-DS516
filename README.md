# Health Insurance Cross Sell Prediction


## ภาพรวมโครงการ (Project Overview)
โครงการนี้มีวัตถุประสงค์เพื่อดำเนินการวิเคราะห์ข้อมูลเชิงสำรวจ (Exploratory Data Analysis: EDA) บนชุดข้อมูลลูกค้าประกันภัย เพื่อทำความเข้าใจลักษณะการกระจายของข้อมูล ความสัมพันธ์ระหว่างตัวแปร และปัจจัยที่ส่งผลต่อการตอบสนองของลูกค้า (Response)
โดยผลลัพธ์จากการทำ EDA จะถูกนำไปใช้ในการคัดเลือกตัวแปร (Feature Selection) และออกแบบตัวแปรใหม่ (Feature Engineering) เพื่อเตรียมความพร้อมสำหรับการพัฒนาแบบจำลองการเรียนรู้ของเครื่องประเภท Ensemble ซึ่งมุ่งเน้นการเพิ่มประสิทธิภาพ
ในการพยากรณ์และลดความเอนเอียงของโมเดล

---

## นิยามปัญหา (Problem Statement)
แม้ว่าชุดข้อมูลจะมีตัวแปรที่หลากหลายซึ่งครอบคลุมลักษณะของลูกค้าและยานพาหนะ แต่ยังขาดความเข้าใจอย่างเป็นระบบเกี่ยวกับความสัมพันธ์ระหว่างตัวแปรอิสระกับตัวแปรเป้าหมาย (`Response`)
ปัญหาที่พบในข้อมูลชุดนี้ ได้แก่:
- ตัวแปรเป้าหมายมีลักษณะไม่สมดุล (Class Imbalance)
- ตัวแปรบางส่วนเป็นข้อมูลเชิงหมวดหมู่ที่ถูกเข้ารหัสเป็นตัวเลข ซึ่งอาจนำไปสู่การตีความที่คลาดเคลื่อน
- ยังไม่สามารถระบุได้อย่างชัดเจนว่าตัวแปรใดมีอิทธิพลต่อการตอบสนองของลูกค้า
- การเลือกตัวแปรแบบสุ่มอาจส่งผลต่อประสิทธิภาพของแบบจำลองในขั้นตอนถัดไป

---

## วัตถุประสงค์ของโครงการ (Objectives)
1. เพื่อสำรวจโครงสร้างและลักษณะการกระจายของข้อมูลในแต่ละตัวแปร
2. เพื่อวิเคราะห์ความสัมพันธ์ระหว่างตัวแปรอิสระกับตัวแปรเป้าหมาย (Response)
3. เพื่อค้นหาตัวแปรที่มีความสำคัญต่อการพยากรณ์
4. เพื่อสนับสนุนการคัดเลือกตัวแปร (Feature Selection) อย่างมีเหตุผลเชิงข้อมูล
5. เพื่อเตรียมชุดข้อมูลสำหรับการสร้างแบบจำลอง Machine Learning ประเภท Ensemble เช่น  
   - Random Forest  
   - Gradient Boosting  
   - XGBoost  

---

## รายละเอียดชุดข้อมูล (Dataset Description)
- ชุดข้อมูลเป็นข้อมูลระดับลูกค้า โดย 1 แถวแทนลูกค้า 1 ราย และแต่ละคอลัมน์แทนคุณลักษณะของลูกค้า
- จำนวนข้อมูล: 381,109 แถว
- จำนวนตัวแปร: 12 ตัวแปร
- Data Sources [www.kaggle.com](https://www.kaggle.com/datasets/anmolkumar/health-insurance-cross-sell-prediction/code)

---

## Data Dictionary
| ชื่อตัวแปร | ประเภทข้อมูล | คำอธิบาย |
|----------|------------|----------|
| `id` | Integer | รหัสประจำตัวลูกค้า (Unique ID) |
| `Gender` | Categorical | เพศของลูกค้า (Male, Female) |
| `Age` | Numeric | อายุของลูกค้า (ปี) |
| `Driving_License` | Binary | สถานะการมีใบขับขี่ (1 = มีใบขับขี่, 0 = ไม่มีใบขับขี่) |
| `Region_Code` | Categorical (Encoded) | รหัสระบุภูมิภาคของลูกค้า (ถูกเข้ารหัส) |
| `Previously_Insured` | Binary | สถานะการเคยมีประกันรถยนต์มาก่อน (1 = เคยมี, 0 = ไม่เคยมี) |
| `Vehicle_Age` | Categorical | อายุของรถยนต์ (< 1 Year, 1-2 Year, > 2 Years) |
| `Vehicle_Damage` | Categorical | ประวัติรถเคยเกิดความเสียหาย (Yes = เคยเกิดความเสียหาย, No = ไม่เคย) |
| `Annual_Premium` | Numeric | จำนวนเงินค่าเบี้ยประกันที่ลูกค้าต้องชำระต่อปี |
| `Policy_Sales_Channel` | Categorical (Encoded) | รหัสช่องทางการขายแบบไม่ระบุตัวตน เช่น ตัวแทนขาย, โทรศัพท์, จดหมาย, พบลูกค้าโดยตรง |
| `Vintage` | Numeric | จำนวนวันตั้งแต่ลูกค้าเริ่มมีความสัมพันธ์กับบริษัท |
| `Response` | Binary (Target) | ตัวแปรเป้าหมาย แสดงความสนใจของลูกค้า (1 = สนใจ / ตอบรับ, 0 = ไม่สนใจ) |

---

## เครื่องมือและไลบรารี (Tools & Libraries)
- **Python**: ภาษาหลักสำหรับการวิเคราะห์ข้อมูลและการสร้างโมเดล
- **Pandas / NumPy**: การจัดการและประมวลผลข้อมูล
- **Matplotlib / Seaborn**: การสร้างกราฟและการแสดงผลข้อมูล
- **Scikit-learn**: เครื่องมือสำหรับ preprocessing, feature selection และ ensemble models
- **XGBoost**: Ensemble learning library สำหรับ Gradient Boosting

---

## Exploratory Data Analysis (EDA)
- ชุดข้อมูลที่ใช้ในการวิเคราะห์ประกอบด้วยข้อมูลลูกค้า 381,109 ราย โดยมีตัวแปรทั้งหมด 12 ตัวแปร 
- ข้อมูลทั้งหมดไม่มีค่า missing values 
- ตัวแปรเป้าหมาย (Response) มี Class Imbalance โดย
  - กลุ่มที่ไม่ตอบรับ (Response = 0) มีสัดส่วน 87.74% 
  - กลุ่มที่ตอบรับ (Response = 1) มีเพียง 12.26%
- ตัวแปรในชุดข้อมูลสามารถแบ่งออกเป็น 3 ประเภทหลัก ได้แก่
  - ตัวแปรเชิงหมวดหมู่ (Categorical Variables) Gender/Vehicle_Age/Vehicle_Damage
  - ตัวแปรแบบไบนารี (Binary Variables) Previously_Insured/Driving_License
  - ตัวแปรเชิงตัวเลข (Numerical Variables) Age/Annual_Premium/Vintage
  - ตัวแปรรหัสเชิงระเบียน (Categorical (Encoded)) Region_Code/Policy_Sales_Channel

![EDA-Distribution](Material/EDA-distribution.png)

---

### กลุ่มที่ 1: Categorical (เชิงคุณลักษณะ) vs Response
จากการวิเคราะห์ความสัมพันธ์แบบสองตัวแปร ระหว่างตัวแปรเชิงหมวดหมู่กับตัวแปรเป้าหมาย (Response) โดยพิจารณาอัตราการตอบรับ (Response Rate) พบประเด็นสำคัญดังนี้

![EDA-Categorical](Material/EDA-categorical.png)

**Gender**
- พบอัตราการตอบรับแตกต่างกันเล็กน้อยระหว่างแต่ละค่า
- อย่างไรก็ตาม ความแตกต่างไม่ชัดเจน และเมื่อพิจารณาร่วมกับตัวแปรอื่น พบว่าให้สัญญาณเชิงทำนายค่อนข้างจำกัด

**Vehicle_Age**
- ลูกค้าที่มีรถอายุมากกว่ามีอัตราการตอบรับสูงกว่าอย่างชัดเจน
- สะท้อนว่าลูกค้าที่ใช้รถมานานมีแรงจูงใจในการทำประกันรถมากกว่า

**Vehicle_Damage**
- ลูกค้าที่มีประวัติรถเสียหายมีอัตราการตอบรับสูงกว่ามากเมื่อเทียบกับลูกค้าที่ไม่เคยมีความเสียหาย
- ตัวแปรนี้แสดงความสัมพันธ์เชิงธุรกิจกับการรับรู้ความเสี่ยงและความต้องการทำประกันรถ

---

### กลุ่มที่ 2: Binary (เชิงพฤติกรรมลูกค้า) vs Response
จากการวิเคราะห์ความสัมพันธ์แบบสองตัวแปร ระหว่างตัวแปรเชิงหมวดหมู่กับตัวแปรเป้าหมาย (Response) โดยพิจารณาอัตราการตอบรับ (Response Rate) พบประเด็นสำคัญดังนี้

![EDA-Categorical](Material/EDA-Binary.png)

**Previously_Insured**
- ลูกค้าที่ไม่เคยมีประกันรถมีอัตราการตอบรับสูงอย่างชัดเจน
- ลูกค้าที่มีประกันรถอยู่แล้วแทบไม่แสดงความสนใจซื้อเพิ่มเติม
- เป็นตัวแปรเชิงพฤติกรรมที่มีอิทธิพลต่อ Response สูงที่สุด

**Driving_License**
- ลูกค้าที่มีใบขับขี่มีอัตราการตอบรับสูงกว่ากลุ่มที่ไม่มีใบขับขี่
- อย่างไรก็ตาม ความแตกต่างไม่เด่นชัดมากเมื่อเทียบกับ Previously_Insured
- ให้สัญญาณเชิงทำนายระดับรอง

---

### กลุ่มที่ 3: Numerical Variables vs Response
จากการวิเคราะห์เชิงสองตัวแปร โดยพิจารณาความสัมพันธ์ระหว่างตัวแปรเชิงตัวเลข (Numerical Variables) กับตัวแปรเป้าหมาย (Response) ผ่านการเปรียบเทียบการกระจายของข้อมูลด้วย
boxplot พบว่าตัวแปรแต่ละตัวแสดงรูปแบบความสัมพันธ์กับการตอบสนองของลูกค้าที่แตกต่างกัน ดังนี้

![EDA-Categorical](Material/EDA-Numerical.png)

### กลุ่มที่ 3: Numerical Variables vs Response

**Age**
- กลุ่มลูกค้าที่สนใจซื้อมีค่าอายุมัธยฐานสูงกว่ากลุ่มไม่สนใจเล็กน้อย
- แสดงแนวโน้มว่าลูกค้าวัยกลางมีความสนใจมากกว่าวัยต่ำมาก

**Annual_Premium**
- การกระจายของข้อมูลมีความเบ้ขวาและมี outliers จำนวนมาก
- กลุ่มที่สนใจมีค่าเบี้ยกระจุกในช่วงระดับกลางถึงสูง
- ตัวแปรนี้เหมาะสำหรับใช้ร่วมกับการ transform หรือการวิเคราะห์ร่วมกับตัวแปรอื่น

**Vintage**
- ไม่พบความแตกต่างที่ชัดเจนระหว่างกลุ่ม Response
- เป็นตัวแปรเสริมที่อาจช่วยเพิ่มบริบทเชิงพฤติกรรม แต่ไม่ใช่ตัวแปรหลัก

---

### กลุ่มที่ 4: Categorical (Encoded) vs Response
อัตราการตอบสนอง (Response Rate %) ของลูกค้า โดยจัดเรียงจาก ค่าสูงไปต่ำ สำหรับตัวแปรที่มีลักษณะเป็น high-cardinality categorical variables ได้แก่ Region_Code และ Policy_Sales_Channel ซึ่งมีจำนวนกลุ่มย่อยจำนวนมาก 
การแสดงผลในลักษณะนี้มีวัตถุประสงค์เพื่อให้เห็นภาพรวมของความแตกต่างเชิงรูปแบบ (pattern) มากกว่าการตีความเชิงลำดับหรือเชิงสาเหตุ

![EDA-Categorical](Material/EDA-others.png)

### กลุ่มที่ 4: Categorical (Encoded) Variables vs Response

**Region_Code**
- พบความแตกต่างของอัตราการตอบรับระหว่างบาง Region
- อย่างไรก็ตาม ไม่ปรากฏ pattern เชิงธุรกิจที่สอดคล้องชัดเจน
- เหมาะสำหรับใช้เป็นตัวแปรเสริมในการเพิ่มความสามารถในการแยกกลุ่มของโมเดล

**Policy_Sales_Channel**
- ช่องทางการขายบางประเภทมีอัตราการตอบรับสูงกว่าช่องทางอื่น
- แสดงอิทธิพลเชิงพฤติกรรมและวิธีการเข้าถึงลูกค้า
- เป็นตัวแปรที่ช่วยเพิ่มความสามารถในการทำนายของโมเดล

---

## Feature Selection Prioritization
จากผลการวิเคราะห์ EDA และการทดลอง Feature Selection แบบตัดตัวแปรตามลำดับความสำคัญ
สามารถจัดกลุ่มตัวแปรได้ดังนี้

### High-Priority Features (ตัวแปรสำคัญมาก)
ตัวแปรกลุ่มนี้ให้สัญญาณแรงและมีผลต่อประสิทธิภาพของโมเดลโดยตรง

- Previously_Insured  
- Vehicle_Damage  
- Vehicle_Age  

### Medium-Priority Features (ช่วยเพิ่มความคมของโมเดล)
ตัวแปรกลุ่มนี้ช่วยเพิ่มความสามารถในการแยกคลาสและลด False Positive

- Policy_Sales_Channel  
- Region_Code  
- Annual_Premium  
- Vintage  
- (Age – ใช้เป็นตัวแปรเสริมได้ ขึ้นกับโมเดล)

### Low-Priority / Optional Features
ตัวแปรที่ตัดออกแล้วไม่ส่งผลต่อประสิทธิภาพของโมเดลอย่างมีนัยสำคัญ

- Gender  
- Driving_License  
- id (เป็นเพียงตัวระบุข้อมูล)

**Recommended Feature Set**
- Previously_Insured  
- Vehicle_Damage  
- Vehicle_Age  
- Policy_Sales_Channel  
- Region_Code  
- Annual_Premium  
- Vintage  
- Age

--- 

## Modeling Methodology
### 🖥️ Model Type
 - **Supervised Learning:** ประเภท Binary Classification
 - **Target Label:** กลุ่มที่ไม่ตอบรับ (0) และ กลุ่มที่ตอบรับ (Response = 1)
 - **Features:** Previously_Insured, Vehicle_Damage, Vehicle_Age, Policy_Sales_Channel, Region_Code, Age, Annual_Premium, Vintage
 - **Optional Features:** Driving_License, Gender

### ⚙️ Chosen Model
 - **XGBoost**
 - **Random Forest**

---

### Pipeline

### 🧮 Encoding

```python
df = df.set_index('id')
df = pd.get_dummies(df, columns=['Gender'], drop_first=True, dtype=int)
df['Vehicle_Age'] = df['Vehicle_Age'].map({'< 1 Year': 0, '1-2 Year': 1, '> 2 Years': 2})
df['Vehicle_Damage'] = df['Vehicle_Damage'].map({'Yes': 1, 'No': 0})
df.head()
```
---

### ✂️ Train/Test Split

```python
X = df.drop(columns=['Response'])
y = df['Response']

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42, stratify=y
)
print(f"Train: {X_train.shape[0]:,} | Test: {X_test.shape[0]:,}")
```
**Train:** 304,887
**Test:** 76,222

---

### Class imbalance handling

```python
# Random Undersampling
rus = RandomUnderSampler(random_state=42)
X_train_rus, y_train_rus = rus.fit_resample(X_train, y_train)
print(f"Random Under: {len(X_train):,} → {len(X_train_rus):,} | {y_train_rus.value_counts().to_dict()}")

# Tomek Links
tl = TomekLinks()
X_train_tl, y_train_tl = tl.fit_resample(X_train, y_train)
print(f"Tomek Links:  {len(X_train):,} → {len(X_train_tl):,} | {y_train_tl.value_counts().to_dict()}")
```
**Random Under**
- **Train:** 304,887 --> 37,368
- **Test:** 76,222 --> 37,368

**Tomek Links**
- **Train:** 304,887 --> 251,460
- **Test:** 76,222 --> 37,368

---














