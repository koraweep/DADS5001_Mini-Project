# DADS5001_Mini-Project
# สำรวจกระเป๋าตังคนไทย รายได้ และ รายจ่าย เฉลี่ยของครัวเรือน
ภาคครัวเรือนถือว่าเป็นองค์ประกอบที่สำคัญของระบบเศรษฐกิจ กิจกรรมทางเศรษฐกิจต่าง ๆ ของครัวเรือนทั้งการประกอบอาชีพ การบริโภค การออม การใช้จ่าย รวมไปถึงภูมิประเทศ ถิ่นที่อยู่อาศัย ล้วนเป็นปัจจัยให้เกิดความแตกต่างในรายได้ และรายจ่ายครัวเรือน
# Author
* Korawee Peerasantikul
* ID: 6420422007
* Subject: DADS5001
# Dataset Information
**File Name:**   1) Income.csv  2) Expense.csv\
**Source:**   (https://catalog.nso.go.th/dataset/?res_format=CSV&groups=os-08) \
**Role and Collumn:**    
* รายได้เฉลี่ยต่อครัวเรือน (4,600 rows)
* 7 Column
  *  year           
  *  region         
  *  source_income   
  *  soc_eco_class  
  *  value        
  *  unit           
  *  source
* รายจ่ายเฉลี่ยต่อครัวเรือน (2,600 rows)
* 7 Column
  *  year     
  *  region         
  *  type_expenditur  
  *  soc_eco_class    
  *  value            
  *  unit          
  *  source
 # Question that need answer from Dataset 
* แนวโน้ม รายได้ และ รายจ่าย เฉลี่ยต่อครัวเรือนเป็นอย่างไร คนไทยมีเงินคงเหลือเพิ่มขึ้นหรือไม่
* คนไทยหมดค่าใช้จ่ายไปกับอะไรมากที่สุด ในแต่ละเดือน
* ภาคไหนใดในประเทศมีเงินคงเหลือ มากที่สุด และภาคใดน้อยที่สุด
* คนประกอบอาชีพอะไรมีเงินคงเหลือ มากที่สุด และอาชีพใดน้อยที่สุด
* ใครมีเงินเหลือน้อยที่สุด

# DATA COLLECTION
![Capture](https://user-images.githubusercontent.com/101727971/195891361-aba3a898-005d-47c5-a2eb-4e54b420992d.JPG)

````
data_income = pd.read_csv('Income.csv')
data_expense = pd.read_csv('Expense.csv')
````

# DATA CLEANING
* ข้อมูลต้นฉบับมีข้อมูลเชิง aggregate(รวม) อยู่ในหลาย column เช่น region มี ทั่วประเทศ หรือ source_income มี รายได้ทั้งสิ้น(รวม) ซึ่งข้อมูลเหล่านี้ในต้นฉบับเป็นค่าคงที่ ถ้ามีการปรับแต่งข้อมูลในตาราง ข้อมูลรวมเหล่านี้จะไม่สามารถใช้ได้ เลยจะขอตัดออก และทําข้อมูลรวมขึ้นมาเองด้วยใช้ aggregate function

# Cleaning Income Table

* ทําการสํารวจข้อมูลในตาราง Income Dataset พบว่าข้อมูลนตาราง value มีจํานวนไม่เท่ากับกลุ่มอื่นๆ

````
df_income = data_income
df_income.info()
df_income.head(3)
````
![image](https://user-images.githubusercontent.com/101727971/195992627-d02d079e-9809-4e49-a169-5b64a486b955.png)


* ทําการสํารวจข้อมูลในแต่ละ Columns ว่ามีข้อมูลที่เป็น unique อะไรบ้าง

````
income_column = df_income.columns.values.tolist()
for col in income_column:
    print(f'{col.upper()}: {df_income[col].unique()}')
````
![image](https://user-images.githubusercontent.com/101727971/195992707-b181ccf6-5372-4667-893c-2153489000d4.png)


* Drop colume 'source' ซึ่งคิดว่าไม่น่าจะได้ใช้งานออก และตัดข้อมูล 'รวม' ที่อยู่ในแต่ละ Columns ของ dataset income ออก ยกเว้น 'รายได้ประจําที่ไม่เป็นตัวเงิน (รวม) 'เนื่องจากในตารางข้อมูลจริงๆแล้วไม่ได้เป็นข้อมูลเชิง aggregate 

````
df_in = df_income.replace('รายได้ประจำที่ไม่เป็นตัวเงิน (รวม)','รายได้ประจำที่ไม่เป็นตัวเงิน')
drop_string_in = ['ทั่วประเทศ','รวม']
df_in = df_in[ ~df_in.region.str.contains('|'.join(drop_string_in)) ]
df_in = df_in[ ~df_in.source_income.str.contains('|'.join(drop_string_in)) ]
df_in = df_in[ ~df_in.soc_eco_class.str.contains('|'.join(drop_string_in)) ]
````

* ข้อมูลใน Dataset มี NaN อยู่หลักๆมาจาก Column ที่ region เป็น 'กรุงเทพ นนทบุรี ปทุมธานี และสมุทรปราการ' และ soc_eco_class เป็น 'ผู้ทำการประมง ป่าไม้ ล่าสัตว์ หาของป่า และบริการทางการเกษตร' ซึ่งทําการสํารวจข้อมูลเพิ่มเติมพบว่านอกจากข้อมูล NaN แล้วยังมีข้อมูลที่เป็น 0 อยู่หลายรายการซึ่งคาดว่าอาจจะไม่ได้ทําการเก็บข้อมูลเหล่านี้มา โดยเฉพาะ 'รายได้ประจำที่เป็นตัวเงิน (รายได้จากการทำงาน)' ที่ไม่ควรที่จะเป็น 0

````
df_in[df_in.isna().any(axis=1)]
````

![image](https://user-images.githubusercontent.com/101727971/195994420-f16e7590-f37f-4709-8a53-141c7ed3ffcd.png)


````
df_adjusted = df_in.loc[(df_in['soc_eco_class'] == 'ผู้ทำการประมง ป่าไม้ ล่าสัตว์ หาของป่า และบริการทางการเกษตร') & 
          ((df_in['value'] == 0) | (df_in['value'].isnull()))]
df_adjusted
````
![image](https://user-images.githubusercontent.com/101727971/195994645-3bd30961-1c77-4f4b-8a89-8ce3370f9286.png)


* ปรับข้้อมูล NaN ให้เป็น 0 จากนั้น replace value ข้อมูล'รายได้ประจำที่เป็นตัวเงิน (รายได้จากการทำงาน)' ของกลุ่ม 'ผู้ทํางานประมง ป่าไม้...' ในปี 2554,2560,2562 ที่เป็น 0 ด้วยค่าเฉลี่ยของรายได้ในปี 2556 กับ 2558

````
df_in_adjusted = df_in_adjusted.fillna(0)
df_in_adjusted.head(5)
df_adjusted
````
![image](https://user-images.githubusercontent.com/101727971/195995154-e636455c-bd75-4e2e-b6f5-01e70e165cdc.png)


````
df_in_adjusted2 = df_in_adjusted[df_in_adjusted['value'] != 0]
group_adjusted = df_in_adjusted2.groupby(['source_income'])
df_in_adjusted_value = group_adjusted.agg('mean')
df_in_adjusted_value.drop(['year'], axis=1,inplace=True)
df_in_adjusted_value.reset_index()
````
![image](https://user-images.githubusercontent.com/101727971/195995231-697fb5d9-62ca-423a-967e-75138f3711de.png)


````
df_in.loc[(df_in['soc_eco_class'] == 'ผู้ทำการประมง ป่าไม้ ล่าสัตว์ หาของป่า และบริการทางการเกษตร') & 
          (df_in['source_income'] == 'รายได้ประจำที่เป็นตัวเงิน (รายได้จากการทำงาน)') &
          ((df_in['value'] == 0) | (df_in['value'].isnull()))]
````

![image](https://user-images.githubusercontent.com/101727971/195995380-4ab4902c-6fca-4b11-98c1-bb1fdc0b3808.png)


````
replace_value = df_in_adjusted_value['value'].tolist()
df_in.loc[608,'value'] = replace_value[0]
df_in.loc[641,'value'] = replace_value[0]
df_in.loc[652,'value'] = replace_value[0]
df_in.fillna(0,inplace=True)
````

* สํารวจข้อมูลอีกครั้งพบว่า Dataset มีจํานวนเท่ากันแล้วโดยไม่มี  NaN 

![image](https://user-images.githubusercontent.com/101727971/195995493-bc34eabd-eb60-4043-9824-c8a0d8ab2299.png)


# Cleaning Expense Table

* ทําการสํารวจข้อมูลในตาราง Expense Table

````
df_expense = data_expense
df_expense.info()
df_expense.head(3)
````
![image](https://user-images.githubusercontent.com/101727971/195995885-78da3e3e-5e0c-4c5d-93a5-7091ae2febef.png)
![image](https://user-images.githubusercontent.com/101727971/195995928-3395ab50-b6c4-4729-be04-ffb1d90b758b.png)


````
expense_column = df_expense.columns.values.tolist()
for col in expense_column:
    print(f'{col.upper()}: {df_expense[col].unique()}')
````

![image](https://user-images.githubusercontent.com/101727971/195996043-e7d3f203-426a-4f46-af69-1f7bca6ba518.png)


* Drop colume 'source' ซึ่งคิดว่าไม่น่าจะได้ใช้งานออก และตัดข้อมูล 'รวม' ที่อยู่ในแต่ละ Columns ของ dataset expense ออก ยกเว้น 'ค่าใช้จ่ายที่ไม่เกี่ยวกับการอุปโภคบริโภค (รวม)' เนื่องจากในตารางข้อมูลจริงๆแล้วไม่ได้เป็นข้อมูลเชิง aggregate

````
df_x = df_expense.drop(columns =['source'])
df_x = df_x.replace('ค่าใช้จ่ายที่ไม่เกี่ยวกับการอุปโภคบริโภค (รวม)','ค่าใช้จ่ายที่ไม่เกี่ยวกับการอุปโภคบริโภค')
drop_string = ['ทั่วประเทศ','รวม']
df_x = df_x[ ~df_x.region.str.contains('|'.join(drop_string)) ]
df_x = df_x[ ~df_x.type_expenditur.str.contains('|'.join(drop_string)) ]
df_x = df_x[ ~df_x.soc_eco_class.str.contains('|'.join(drop_string)) ]
````

* เนื่องจากข้อมูลรายได้ (Income table) ของ soc_eco_class ที่เป็น 'ผู้ทำการประมง ป่าไม้ ล่าสัตว์ หาของป่า และบริการทางการเกษตร' ที่ region เป็น 'กรุงเทพ นนทบุรี ปทุมธานี และสมุทรปราการ' มีข้อมูลในตารางที่เป็น NaN และ 0(ปรับแก้ไปแล้ว) จึงมาสํารวจในฝั้งของ Expense บ้าง พบว่าจะมีปัญหาคล้ายกัน โดยมีบางปีที่อาจจะไม่ได้ทําการสํารวจซึ่งมีค่าใช้จ่ายเป็น 0


````
df_x_adjusted = df_x[(df_x['region'] == 'กทม. นนทบุรี ปทุมธานี และสมุทรปราการ') & (df_x['soc_eco_class'] =='ผู้ทำการประมง ป่าไม้ ล่าสัตว์ หาของป่า และบริการทางการเกษตร')]
group_x_adjusted = df_x_adjusted.groupby(['year']).agg('sum')
group_x_adjusted.reset_index()
````

![image](https://user-images.githubusercontent.com/101727971/195996827-b3614674-7b55-4088-a60b-1e14ad66ca68.png)

* ทําการหาค่าเฉลี่ยของแต่ละประเภท เพื่อไปใส่ในข้อมูลปีที่ค่าใช้จ่ายสําหรับผู้ทําการ ประมง.. ในกรุงเทพและจังหวัดไกล้เคียงที่เดิมเป็น 0

![image](https://user-images.githubusercontent.com/101727971/195927790-775f201d-e0d1-428f-ada2-ea721fc50747.png)

* ทําการ replace ข้อมูลเฉลี่ยค่าใช้จ่ายของตัวเรือนแต่ละประเภท ด้วยข้อมูลค่าใช้จ่ายเฉลี่ยที่หามาได้ 

![image](https://user-images.githubusercontent.com/101727971/195928625-bd82e6f9-6fe0-4075-9e3c-0c6951f412a8.png)

* สํารวจ Dataset ที่ปรับแก้อีกรอบ พบว่าข้อมูลน่าจะไม่มีปัญหา

![image](https://user-images.githubusercontent.com/101727971/195929027-69850db0-056d-4e5c-82f5-d911c9ebe6a2.png)


# DATA ANALYSIS (GROUP,TRANSFORM,PLOT)
**1. Time Series of Income and Expense**
````group_in_yr = df_in.groupby(['year','source_income'])
df_in_gyr = group_in_yr['value'].mean().reset_index()
df_in_gyr = df_in_gyr.groupby(['year'])['value'].sum().reset_index()
df_in_gyr.rename(columns = {'value':'Avg. Monthly Income'})
````
![Capture](https://user-images.githubusercontent.com/101727971/195875574-9217a377-91e8-4b60-98ae-32dbc2f894f2.JPG)
````group_x_yr = df_x.groupby(['year','type_expenditur'])
df_x_gyr = group_x_yr['value'].mean().reset_index()
df_x_gyr = df_x_gyr.groupby(['year'])['value'].sum().reset_index()
df_x_gyr.rename(columns = {'value':'Avg. Monthly Expense'})
````
````df_merge_yr = pd.merge(df_in_gyr,df_x_gyr, left_on='year', right_on='year', how='left' )
df_merge_yr = df_merge_yr.astype({'value_x': int, 'value_y': int})
df_merge_yr['Ratio Expense/Income'] = round(df_merge_yr['value_y'] / df_merge_yr['value_x'],2)
df_merge_yr.rename(columns = {'value_x':'Avg. Monthly Income','value_y':'Avg. Monthly Expense'},inplace=True)

display(df_merge_yr)
````
````# Preparing Data for plot (from wide to long)
line_plot_yr = df_merge_yr.melt( id_vars=['year'],value_vars=['Avg. Monthly Income','Avg. Monthly Expense'] )          
display(line_plot_yr)
display(df_merge_yr)
````

