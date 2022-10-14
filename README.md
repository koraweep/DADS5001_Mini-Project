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
* ข้อมูลมากจาก Website สํานักงานสถิติแห่งชาติในหมวดสถิติรายได้และค่าใช้จ่ายขอครัวเรือน

![image](https://user-images.githubusercontent.com/101727971/195892753-27d5b650-b957-4277-8d26-d093e5367823.png)

# DATA CLEANING
* ข้อมูลต้นฉบับมีข้อมูลเชิง aggregate(รวม) อยู่ในหลาย column เช่น region มี ทั่วประเทศ หรือ source_income มี รายได้ทั้งสิ้น(รวม) ซึ่งข้อมูลเหล่านี้ในต้นฉบับเป็นค่าคงที่ ถ้ามีการปรับแต่งข้อมูลในตาราง ข้อมูลรวมเหล่านี้จะไม่สามารถใช้ได้ เลยจะขอตัดออก และทําข้อมูลรวมขึ้นมาเองด้วยใช้ aggregate function

# Cleaning Income Table
* ทําการสํารวจข้อมูลในตาราง Income Dataset

![image](https://user-images.githubusercontent.com/101727971/195910820-7a9b1dcb-da89-4216-8202-2f33bcc5d6e6.png)


![image](https://user-images.githubusercontent.com/101727971/195910451-433954ad-b605-4b1e-a455-ca4a32130546.png)

* ทําการตัดข้อมูลรวมออกในแต่ละ Columns ของ dataset income

![image](https://user-images.githubusercontent.com/101727971/195911729-f9c99c38-e99d-4898-b6d2-f568b3d40f3f.png)

* ข้อมูลใน Dataset มี NaN อยู่หลักๆมาจาก Column ที่ region เป็น กรุงเทพและจังหวัดไกล้เคียง และ soc_eco_class เป็น 'ผู้ทําการประมง ป่าไม้า...' ซึ่งทําการสํารวจข้อมูลเพิ่มเติมพบว่านอกจากข้อมูล NaN แล้วยังมีข้อมูลที่เป็น 0 อยู่หลายรายการซึ่งคาดว่าอาจจะไม่ได้ทําการเก็บข้อมูลเหล่านี้มา โดยเฉพาะรายได้ประจําจากการทํางานที่ไม่น่าเป็น 0

![image](https://user-images.githubusercontent.com/101727971/195913440-73ae06ff-5739-4223-9f95-250cccb462c9.png)

* ปรับข้้อมูล NaN ให้เป็น 0 จากนั้น replace value ข้อมูล'รายได้ประจําจากการทํางาน' ของกลุ่ม 'ผู้ทํางานประมง ป่าไม้...' ในปี 2554,2560,2562 ที่เป็น 0 ด้วยค่าเฉลี่ยของรายได้ในปี 2556 กับ 2558

![image](https://user-images.githubusercontent.com/101727971/195917802-8e269dc6-658a-4c06-8631-4eac5efaa273.png)

![image](https://user-images.githubusercontent.com/101727971/195921843-8bc92a6f-dac3-48be-a560-d4b22507de07.png)

![image](https://user-images.githubusercontent.com/101727971/195922279-e3b829ee-d990-4525-9e56-9623ba2c3ce3.png)

* สํารวจข้อมูลอีกครั้งพบว่า Dataset มีจํานวนเท่ากันแล้วโดยไม่มี  NaN 

![image](https://user-images.githubusercontent.com/101727971/195925035-bcda9cf3-e2ae-40bf-8657-2ea896d2da1f.png)

# Cleaning Expense Table

* ทําการสํารวจข้อมูลในตาราง Expense Table

![image](https://user-images.githubusercontent.com/101727971/195926040-b6debbcc-3747-4f4c-a00d-5a05fd4962a6.png)

![image](https://user-images.githubusercontent.com/101727971/195926317-7769e047-8e67-4739-912b-8c489b7a8a5d.png)

* ทําการตัดข้อมูลรวมออกในแต่ละ Columns ของ dataset expense

![image](https://user-images.githubusercontent.com/101727971/195926800-c6cfef97-f0e5-49ce-a00a-7643660a3ad9.png)

* เนื่องจากข้อมูลรายได้จาก ผู้ทําการประมงในกรุงเทพและจังหวัดไกล้เคียงมีปัญหา(ปรับแก้ไปแล้ว) จึงมาสํารวจในฝั้งของ Expense บ้าง พบว่าจะเป็นคล้ายกันโดยมีบางปีที่อาจจะไม่ได้ทําการสํารวจทําให้ค่าใช้จ่ายเป็น 0

![image](https://user-images.githubusercontent.com/101727971/195927294-92d24b20-0334-4492-8922-29f2e74b0983.png)

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

