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
 # OBJECTIVE: Use information from the dataset to answer the following question
Q1) แนวโน้ม รายได้ และ รายจ่าย เฉลี่ยต่อครัวเรือนเป็นอย่างไร คนไทยมีเงินคงเหลือเพิ่มขึ้นหรือไม่  
Q2) คนไทยหมดค่าใช้จ่ายไปกับอะไรมากที่สุด ในแต่ละเดือน  
Q3) ภาคไหนใดในประเทศมีเงินคงเหลือ มากที่สุด และภาคใดน้อยที่สุด  
Q4) คนประกอบอาชีพอะไรมีเงินคงเหลือ มากที่สุด และอาชีพใดน้อยที่สุด  
Q5) ใครแนวโน้มที่จะมีเงินเหลือน้อยที่สุด และมีช่วงไหนหรือไม่ ที่ไม่มีเงินคงเหลือเลย

# DATA COLLECTION
![Capture](https://user-images.githubusercontent.com/101727971/195891361-aba3a898-005d-47c5-a2eb-4e54b420992d.JPG)

````
data_income = pd.read_csv('Income.csv')
data_expense = pd.read_csv('Expense.csv')
````

# DATA CLEANSING
* ข้อมูลต้นฉบับมีข้อมูลเชิง aggregate(รวม) อยู่ในหลาย column เช่น region มี ทั่วประเทศ หรือ source_income มี รายได้ทั้งสิ้น(รวม) ซึ่งข้อมูลเหล่านี้ในต้นฉบับเป็นค่าคงที่ ถ้ามีการปรับแต่งข้อมูลในตาราง ข้อมูลรวมเหล่านี้จะไม่สามารถใช้ได้ เลยจะขอตัดออก และทําข้อมูลรวมขึ้นมาเองด้วยใช้ aggregate function

# Cleansing Income Table

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


# Cleansing Expense Table

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

* เนื่องจากข้อมูลรายได้ (Income table) ของ soc_eco_class ที่เป็น 'ผู้ทำการประมง ป่าไม้ ล่าสัตว์ หาของป่า และบริการทางการเกษตร' ที่ region เป็น 'กรุงเทพ นนทบุรี ปทุมธานี และสมุทรปราการ' มีข้อมูลในตารางที่เป็น NaN และ 0(ปรับแก้ไปแล้ว) จึงมาสํารวจในฝั้งของ Expense บ้าง พบว่าจะมีปัญหาคล้ายกัน โดยในปี 2554 และ 2560 มีค่าใช้จ่ายเป็น 0 ซึ่งอาจจะเกิดจากไม่ได้ทําการเก็บข้อมูลในปีเหล่านี้


````
df_x_adjusted = df_x[(df_x['region'] == 'กทม. นนทบุรี ปทุมธานี และสมุทรปราการ') & (df_x['soc_eco_class'] =='ผู้ทำการประมง ป่าไม้ ล่าสัตว์ หาของป่า และบริการทางการเกษตร')]
group_x_adjusted = df_x_adjusted.groupby(['year']).agg('sum')
group_x_adjusted.reset_index()
````

![image](https://user-images.githubusercontent.com/101727971/195996827-b3614674-7b55-4088-a60b-1e14ad66ca68.png)

* ทําการหาค่าเฉลี่ยของค่าใช้จ่ายในแต่ละประเภทในปีที่ 2556, 2558, 2562 ซึ่งข้อมูลไม่ได้เป็น 0 เพื่อไป replace value ในปี 2554 และ 2560 ซึ่งค่าใช้จ่ายเป็น 0 และทําการปรับค่าใช้จากรูปแบบ dataframe เป็น list ง่ายต่อการทํา replace value


````
df_x_adjusted = group2_x_adjusted.drop(group2_x_adjusted[(group2_x_adjusted['year'] == 2554) | 
                (group2_x_adjusted['year'] == 2560)].index)
df_x_adjusted = df_x_adjusted.groupby(['type_expenditur']).agg('mean')
df_x_adjusted = df_x_adjusted.drop(columns =['year'])
df_x_adjusted = df_x_adjusted.reset_index()
list_x_adjusted = df_x_adjusted.value.tolist()
display(df_x_adjusted)
print(list_x_adjusted)
````

![image](https://user-images.githubusercontent.com/101727971/196022352-b16b8501-1af3-4a5c-8fe3-8dcde2580a45.png)


* ทําการ replace ข้อมูลเฉลี่ยค่าใช้จ่ายของตัวเรือนแต่ละประเภทที่ข้อมูลเป็น 0 ในตารางหลัก ด้วยข้อมูลค่าใช้จ่ายเฉลี่ยที่หามาได้ โดยทําการ replace ทั้ง 10 ประเภทที่ข้อมูลไม่เป็น 0 (ตัวอย่างการทํา 2 ครั้งด้านล่าง)

````
df_x[((df_x['year'] == 2554)|(df_x['year'] == 2560)) & (df_x['region'] == 'กทม. นนทบุรี ปทุมธานี และสมุทรปราการ') 
            & (df_x['soc_eco_class'] =='ผู้ทำการประมง ป่าไม้ ล่าสัตว์ หาของป่า และบริการทางการเกษตร')
            & (df_x['type_expenditur'] == 'ค่าใช้จ่ายที่ไม่เกี่ยวกับการอุปโภคบริโภค')]
````
![image](https://user-images.githubusercontent.com/101727971/196022521-59695df3-cf47-4d7c-a47a-28b87b2b14e9.png)

````
df_x.loc[656,'value'] = list_x_adjusted[0]
df_x.loc[1088,'value'] = list_x_adjusted[0]
````

````
df_x[((df_x['year'] == 2554)|(df_x['year'] == 2560)) & (df_x['region'] == 'กทม. นนทบุรี ปทุมธานี และสมุทรปราการ') 
            & (df_x['soc_eco_class'] =='ผู้ทำการประมง ป่าไม้ ล่าสัตว์ หาของป่า และบริการทางการเกษตร')
            & (df_x['type_expenditur'] == 'ค่าใช้จ่ายเพื่อการอุปโภคบริโภค (การบันเทิง การอ่านและกิจกรรมทางศาสนา)')] 
````

![image](https://user-images.githubusercontent.com/101727971/196022624-08038cb5-be3d-47a7-81c0-7cfad1b2fc96.png)

````
df_x.loc[1198,'value'] = list_x_adjusted[2]
df_x.loc[1645,'value'] = list_x_adjusted[2]
````

* สํารวจ Dataset ที่ปรับแก้อีกรอบ พบว่าข้อมูลน่าจะไม่มีปัญหา สามารถนําไปใข้งานได้

![image](https://user-images.githubusercontent.com/101727971/195929027-69850db0-056d-4e5c-82f5-d911c9ebe6a2.png)


# DATA ANALYSIS (GROUP, TRANSFORM, PLOT)

* ทําการวิเคราะห์ข้อมูลในแง่มุมต่างๆ เพื่อสามารถที่จะสอบโจทย์ของปัญหาที่อยากรู้ใน Dataset ชุดนี้

# Time Series of Income and Expense

* ทําการ grouping ข้อมูล รายได้ และ รายจ่าย ให้เป็นรูปแบบรายปี และทําการ Merge ข้อมูลเข้าด้วยกัน และหาสัดส่วนรายจ่ายต่อรายได้โดยนําข้อมูลค่าใช้จ่ายมาหารข้อมูลรายได้
````
group_in_yr = df_in.groupby(['year','source_income'])
df_in_gyr = group_in_yr['value'].mean().reset_index()
df_in_gyr = df_in_gyr.groupby(['year'])['value'].sum().reset_index()

group_x_yr = df_x.groupby(['year','type_expenditur'])
df_x_gyr = group_x_yr['value'].mean().reset_index()
df_x_gyr = df_x_gyr.groupby(['year'])['value'].sum().reset_index()

df_merge_yr = pd.merge(df_in_gyr,df_x_gyr, left_on='year', right_on='year', how='left' )
df_merge_yr = df_merge_yr.astype({'value_x': int, 'value_y': int})
df_merge_yr['Ratio Expense/Income'] = round(df_merge_yr['value_y'] / df_merge_yr['value_x'],2)
df_merge_yr.rename(columns = {'value_x':'Avg. Monthly Income','value_y':'Avg. Monthly Expense'},inplace=True)
display(df_merge_yr)
````

![image](https://user-images.githubusercontent.com/101727971/196023442-3722b0e5-856e-4b06-b4e1-fafaf88e257c.png)


* ปรับข้อมูลจากแบบ wide ให้เป็นแบบ long และทําการ Plot Graph ข้อมูลรายได้ และ ค่าใช้จ่ายรายปี

````
line_plot_yr = df_merge_yr.melt( id_vars=['year'],value_vars=['Avg. Monthly Income','Avg. Monthly Expense'] ) 
sns.set(font_scale= 2)
fig,(ax1,ax2) = plt.subplots(2,sharex=False,sharey=False,figsize=(15, 10),dpi=300,gridspec_kw={'height_ratios': [2, 1]})
fig.suptitle('YEARLY INCOME AND EXPENSE')
fig.subplots_adjust(hspace=0.3)

ax1 = sns.lineplot(data= line_plot_yr, x='year', y='value', hue='variable',marker='o',linewidth=6,markersize=12,ax=ax1)
ax1.legend_.set_title(None)
ax1.set_ylim(bottom=0, top=30000)
ax1.set(ylabel=None,xlabel=None,title='Avg. Monthly Income and Expense')
ax1.xaxis.set_major_locator(ticker.MultipleLocator(2))
ax1.legend(frameon=True,loc=4,edgecolor = 'grey',facecolor = 'white')

ax2 = sns.barplot(data= df_merge_yr, x='year', y='Ratio Expense/Income',color='#CD5C5C',ax=ax2)
ax2.set(ylabel=None,xlabel=None,title = 'Expense to Income Ratio')
ax2.bar_label(ax2.containers[0])
ax2.set_ylim(bottom=0, top=1.5)
````

![image](https://user-images.githubusercontent.com/101727971/196025501-d4325a4d-1b0f-4a85-8797-ba4b610b1ccb.png)


Q1) แนวโน้ม รายได้ และ รายจ่าย เฉลี่ยต่อครัวเรือนเป็นอย่างไร คนไทยมีเงินคงเหลือเพิ่มขึ้นหรือไม่ 
* จากข้อมูลพบว่า เมื่อเทียบกับปี 2554 แม้รายได้เฉลี่ยต่อครัวเรือนจะเพิ่มสูงขึ้น แต่รายจ่ายก็เพิ่มสูงขึ้นเช่นกัน อีกทั้งแนวโน้มอันตราที่เพิ่มขึ้นยังสูงกว่ารายได้อีกด้วย ทําได้ สัดส่วนของรายได้ต่อรายจ่าย จากที่เดิมเคยอยู่ประมาณ 74% สูงขึ้นเป็นเกือบ 80% ในปีหลังๆ สรุปก็คือ คนไทยมีแนวโน้มที่จะมีเงินเหลือในแต่ละเดือนน้อยลงเรื่อยๆ

# Income and Expense by Categories

* Group ข้อมูลรายได้ตามประเภท(Categories) และ Sort จากมากไปน้อย โดยใช้ข้อมูลเฉลี่ยของปี 2560 และ 2562

````
group_in_type = df_in2.groupby('source_income')
df_in2_type = group_in_type.agg('mean')
df_in2_type.drop('year',inplace=True,axis=1)
df_in2_type = df_in2_type.astype({'value': int}).reset_index()
df_in2_type.sort_values(by=['value'],inplace=True,ascending=False)
df_in2_type = df_in2_type.reset_index(drop=True)
display(df_in2_type)
````
![image](https://user-images.githubusercontent.com/101727971/196026126-06cb4361-e4e1-4cfa-94f0-f1ab0a8d6545.png)


* Group ข้อมูลประเภทของรายจ่ายตามประเภท(Categories) จากนั้นปรับข้อมูลรายจ่ายให้ตัวเลขเป็นแบบติดลบ และทําการ Sort โดยใช้ข้อมูลเฉลี่ยของปี 2560 และ 2562

````
group_x_type = df_x2.groupby(['region','type_expenditur','soc_eco_class'])
df_x2_type = group_x_type.agg('mean')
group_x_type = df_x2.groupby(['type_expenditur','soc_eco_class'])
df_x2_type = group_x_type.agg('mean')
group_x_type = df_x2.groupby(['type_expenditur'])
df_x2_type = group_x_type.agg('mean')

df_x2_type.drop('year',inplace=True,axis=1)
df_x2_type = df_x2_type.astype({'value': int}).reset_index()
df_x2_type['value'] = -df_x2_type['value'].abs()
df_x2_type.sort_values(by=['value'],inplace=True,ascending=False)
df_x2_type = df_x2_type.reset_index(drop=True)
display(df_x2_type)
````

![image](https://user-images.githubusercontent.com/101727971/196026296-1674146f-12f4-44a9-bae8-1f4bafc1f9e2.png)


* เพื่อที่จะจัดทํา Graph ตามที่รูปแบบที่ต้องเลยใช้วิธี  
  - จัดทําเป็น 2 Graph คือ Graph รายได้ และ Graph รายจ่าย จากนั้น ตัดพวก label ออกและทํา Scale ให้เท่่ากับกัน สุดท้ายจัดระยะห่างระหว่าง 2 Graph ให็เป็น 0 (hspace = 0) และปรับสัดส่วนของทั้ง 2 Graph ให้เหมาะสม
  - เนื่องไม่สามารถจากจะจัด label ของประเภทค่าใช้จ่ายให้อยู่ไกล้ bar chart แบบที่มีทั้งบวก (label อยู่ซ้าย) และลบ (label อยู่ขวา) โดยใช้คํานั่งจาก function ประจําพวก label ได้จึงใช้การใส่ text เข้าไปแทนโดยเริ่มจากหา position ที่ถูกต้องและทําการ for loop เพื่อใส่ label ที่มีระยะห่างเท่าๆกันได้


````
sns.set(font='Tahoma',font_scale =2,rc={'axes.facecolor':'#EAEAF2'})
fig,(ax1,ax2) = plt.subplots(2,sharex=False,sharey=False,figsize=(15, 15),dpi=300,gridspec_kw={'height_ratios': [5, 12]})
fig.suptitle('INCOME AND EXPENSE CATEGORIES',size=40)
fig.subplots_adjust(hspace=0,left=-0.3,right=1.3,top=13.8/15)

sns.barplot(data=df_in2_type, x = 'value', y = 'source_income',ax=ax1,color='#4C72B0',facecolor=None)
ax1.set(xlim=(-10000,20000),ylabel=None,xlabel=None)
ax1.set_xticklabels([])
ax1.set_yticklabels([])
ax1.bar_label(ax1.containers[0],labels=[f'{x:,.0f}' for x in ax1.containers[0].datavalues],color='#4C72B0',size=30)

sns.barplot(data=df_x2_type, x ='value', y = 'type_expenditur',ax=ax2,color='#CD5C5C')
sns.despine(fig=None, ax=ax2, top=True, right=True, left=True, bottom=True)
ax2.set(xlim=(-10000,20000),ylabel=None,xlabel=None)
ax2.set_yticklabels([])
ax2.set_xticklabels([])
ax2.bar_label(ax2.containers[0],labels=[f'{x:,.0f}' for x in ax2.containers[0].datavalues],color='#CD5C5C',size=30)

y_tick_in = df_in2_type['source_income'].tolist()
y_tick_x = df_x2_type['type_expenditur'].tolist()
pos_in = 0.1
pos_x = 0.1

for i in y_tick_in:
    ax1.text(0,pos_in,i,ha='right',size=25,color='#4C72B0')
    pos_in = pos_in+1
for i in y_tick_x:
    ax2.text(0,pos_x,i,ha='left',size=25,color='#CD5C5C')
    pos_x = pos_x+1  
````  


![image](https://user-images.githubusercontent.com/101727971/196027058-66608dd7-504a-4778-ae78-ae957e4fc67d.png)


Q2) คนไทยหมดค่าใช้จ่ายไปกับอะไรมากที่สุด ในแต่ละเดือน  
- จากข้อมูลพบว่า รายได้หลักของครัวเรือนคือ รายได้ประจําที่เป็นรายได้จากการทํางาน และ จริงๆแล้วคนไทยไม่ได้ฟุ่มเฟือยเลย โดยมีค่าใช้จ่ายๆหลักๆเพื่อการอุปโภคบริโภค ในส่วนของ อาหารและเครื่องเดื่ม และ การเดินทางกับการสื่อสาร

# Income and Expense by Region

* ทําการ Grouping ของมูลในส่วนของรายได้ และ รายจ่ายในรูปแบบ region โดยจะใช้ข้อมูลเฉลี่ยของปี 2560 และ 2562 เนื่องจากเป็นข้อมูลกี่ไกล้เคียงปัจจุปัน 
* พบว่าชื่อจังหวัดในตาราง Income และ Expense ไม่เหมือนกัน โดยในตาราง Income เป็น "กรุงเทพ นนทบุรี ปทุมธานี และสมุทรปราการ" แต่ ตาราง Expense เป็น "กทม. นนทบุรี ปทุมธานี และสมุทรปราการ" เลยทําการปรับแก้ชื่อให้เหมือนกัน
* จากนั้น Merge ข้อมูล และ นํารายได้มาหักลบกับรายจ่ายเพื่อหา Saving (เงินคงเหลือเฉลี่ยต่อเดือน)

````
drop_year = [2554,2556,2558]
df_in2 = df_in
for yr in drop_year:
    df_in2 = df_in2.drop(df_in.index[df_in['year'] == yr])
group_in_reg = df_in2.groupby(['year','region','source_income'])
agg_in_reg = group_in_reg.agg('mean')
group_in_reg = agg_in_reg.groupby(['year','region'])
agg_in_reg = group_in_reg.agg('sum')
group_in_reg = agg_in_reg.groupby(['region'])
df_in2_reg = group_in_reg.agg('mean').reset_index()
````

````
df_x2 = df_x
for yr in drop_year:
    df_x2 = df_x2.drop(df_x.index[df_x['year'] == yr])
group_x_reg = df_x2.groupby(['year','region','type_expenditur'])
agg_x_reg = group_x_reg.agg('mean')
group_x_reg = agg_x_reg.groupby(['year','region'])
agg_x_reg = group_x_reg.agg('sum')
group_x_reg = agg_x_reg.groupby(['region'])
df_x2_reg = group_x_reg.agg('mean').reset_index()
````

````
df_in2_reg_ = df_in2_reg.replace('กรุงเทพ นนทบุรี ปทุมธานี และสมุทรปราการ','กทม. นนทบุรี ปทุมธานี และสมุทรปราการ')
df_merge_reg = pd.merge(df_in2_reg_,df_x2_reg, left_on ='region', right_on ='region', how ='left' )
df_merge_reg = df_merge_reg.astype({'value_x': int, 'value_y': int})
df_merge_reg['Avg. Monthly Saving'] = df_merge_reg['value_x'] - df_merge_reg['value_y']
df_merge_reg.rename(columns = {'value_x':'Avg. Monthly Income','value_y':'Avg. Monthly Expense'},inplace=True)
df_merge_reg = df_merge_reg.replace('กทม. นนทบุรี ปทุมธานี และสมุทรปราการ','กทม. นน. ปทุม. สุมุทรปราการ') 
display(df_merge_reg)
````

* ปรับตารางจาก Wide เป็น long แล้วทําการ Plot graph ข้อมูล รายได้ รายจ่าย และเงินคงเหลือ แยกตาม Region

````
bar_plot_reg = df_merge_reg.melt( id_vars=['region'],value_vars=['Avg. Monthly Income','Avg. Monthly Expense','Avg. Monthly Saving'] )   

sns.set(font='Tahoma',font_scale =1.5)
fig,ax = plt.subplots(figsize=(15, 10),dpi=300)
fig.suptitle('INCOME AND EXPENSE BY REGION',size=30)
fig.subplots_adjust(top=9.2/10)

sns.barplot(data=bar_plot_reg, x='region', y='value', hue='variable',ax=ax)
ax.set( ylabel= None, xlabel= None)
ax.legend_.set_title(None)
ax.legend(frameon=True,loc=1,edgecolor = 'grey',facecolor = 'white')
````

![image](https://user-images.githubusercontent.com/101727971/196025791-15d7521d-650e-4d99-a00b-1e10ca3711d5.png)


Q3) ภาคไหนใดในประเทศมีเงินคงเหลือ มากที่สุด และภาคใดน้อยที่สุด  
* ใน Zone กรุงเทพและจังหวัดไกล้เคียงมีรายได้เฉลี่ยสูงที่สุด และภาคเหนือมีรายได้เฉลี่ยน้อยที่สุด แต่อย่างไรก็ตาม Zone กรุงเทพก็มีค่าใช้จ่ายสูงที่สุด และภาคเหนือน้อยที่สุด เช่นกัน
* โดยถ้ามองเป็นเงินคงเหลือจริงๆ จะพบว่า ภาคใต้ มีเงินคงเหลือเฉลี่ยในแต่ละเดือนสูงที่สุด

# Income and Expense by class

* Group ข้อมูล Income และ Expense ตาม social class (กลุ่มอาชีพ) 
* คํานวณหา เงินคงเหลือ ของแต่ละกลุ่มอาชีพ
* ปรับแต่งชื่อตามความเหมาะสม

````
group_in_class = df_in2.groupby(['soc_eco_class','region','source_income'])
df_in2_class = group_in_class.agg('mean').reset_index()
group_in_class = df_in2_class.groupby(['soc_eco_class','source_income'])
df_in2_class = group_in_class.agg('mean').reset_index()
group_in_class = df_in2_class.groupby(['soc_eco_class'])
df_in2_class = group_in_class.agg('sum').reset_index()
df_in2_class.drop('year',inplace=True,axis=1)
df_in2_class = df_in2_class.astype({'value': int}).reset_index(drop=True)
df_in2_class
````

````
group_x_class = df_x2.groupby(['soc_eco_class','region','type_expenditur'])
df_x2_class = group_x_class.agg('mean').reset_index()
group_x_class = df_x2_class.groupby(['soc_eco_class','type_expenditur'])
df_x2_class = group_x_class.agg('mean').reset_index()
group_x_class = df_x2_class.groupby(['soc_eco_class'])
df_x2_class = group_x_class.agg('sum').reset_index()
df_x2_class.drop('year',inplace=True,axis=1)
df_x2_class = df_x2_class.astype({'value': int}).reset_index(drop=True)
df_x2_class
````

````
df_merge_class = pd.merge(df_in2_class,df_x2_class, left_on ='soc_eco_class', right_on ='soc_eco_class', how ='left' )
df_merge_class.rename(columns = {'value_x':'Avg. Monthly Income','value_y':'Avg. Monthly Expense'},inplace=True)
df_merge_class['Avg. Monthly Saving'] = df_merge_class['Avg. Monthly Income'] - df_merge_class['Avg. Monthly Expense']
df_merge_class.sort_values(by=['Avg. Monthly Saving'],inplace=True,ascending=False)
df_merge_class.rename(columns = {'ผู้ถือครองทำการเกษตร การปลูกพืช การเลี้ยงสัตว์ และการเพาะเลี้ยงสัตวน้ำ ซึ่งที่ดินส่วนใหญ่เป็นของตนเอง':
                                'ผู้ถือครองทำการเกษตร ปลูกพืช เลี้ยงสัตว์ ที่ดินของตนเอง',
                                'ผู้ถือครองทำการเกษตร การปลูกพืช การเลี้ยงสัตว์ และการเพาะเลี้ยงสัตวน้ำ ซึ่งที่ดินส่วนใหญ่เช่าผู้อื่น/ที่สาธารณะ/อื่น ๆ':
                                'ผู้ถือครองทำการเกษตร ปลูกพืช เลี้ยงสัตว์ ที่ดินเช่า'},inplace=True)
df_merge_class['soc_eco_class'] = df_merge_class['soc_eco_class'].str.replace('ผู้ถือครองทำการเกษตร การปลูกพืช การเลี้ยงสัตว์ และการเพาะเลี้ยงสัตวน้ำ ซึ่งที่ดินส่วนใหญ่เป็นของตนเอง','ผู้ถือครองทำการเกษตร ปลูกพืช เลี้ยงสัตว์ ที่ดินของตนเอง')
df_merge_class['soc_eco_class'] = df_merge_class['soc_eco_class'].str.replace('ผู้ถือครองทำการเกษตร การปลูกพืช การเลี้ยงสัตว์ และการเพาะเลี้ยงสัตวน้ำ ซึ่งที่ดินส่วนใหญ่เช่าผู้อื่น/ที่สาธารณะ/อื่น ๆ','ผู้ถือครองทำการเกษตร ปลูกพืช เลี้ยงสัตว์ ที่ดินเช่า')
df_merge_class = df_merge_class.reset_index(drop=True)
display(df_merge_class)
````

![image](https://user-images.githubusercontent.com/101727971/196027854-0a386ff0-f2ba-4d60-b359-d627f16a35ba.png)


* ปรับตารางจากแบบ wide เป็น long และทําการ plot graph

````
bar_plot_class = df_merge_class.melt( id_vars=['soc_eco_class'],value_vars=['Avg. Monthly Income','Avg. Monthly Expense','Avg. Monthly Saving'] )
sns.set(font='Tahoma',font_scale =1.5)
fig,ax = plt.subplots(figsize=(15, 10),dpi=300)
fig.suptitle('INCOME AND EXPENSE BY CLASS',size=30)
fig.subplots_adjust(top=9.2/10)
sns.barplot(data=bar_plot_class, x='value', y='soc_eco_class', hue='variable',ax=ax)
ax.set( ylabel= None, xlabel= None)
ax.legend_.set_title(None)
ax.legend(frameon=True,loc=4,edgecolor = 'grey',facecolor = 'white')
````

![image](https://user-images.githubusercontent.com/101727971/196027990-eef7cbb9-bd95-41fb-a496-be86d0cba66a.png)


Q4) คนประกอบอาชีพอะไรมีเงินคงเหลือ มากที่สุด และอาชีพใดน้อยที่สุด  
- กลุ่ม อาชีพ ผู้ปฎิบัตงานวิชาชีพ นักวิชาการ และนักบริหาร มีรายได้สูงสุด ถึงไม้จะมีรายใช้สูงสุดด้วย แต่ก็ยังคงมีเงินเหลือในแต่ละเดือนสูงสุด
- กลุ่ม อาขีพ คนงานเกษตร มีรายได้ และเงินคงเหลือน้อยทึ่สุด โดยเฉลี่ยเหลือไม่ถึง 1500 บาท/เดือน จากข้อมูลเฉลี่ยของปี 2560 และ 2562

# Trend เงินคงเหลือของ คนงานเกษตร ในแต่ละปี แต่ละภาค
ตรวจสอบเพิ่มเติมว่า คนงานเกษตร ที่อยู่ภาคใดและช่วงไหนมีเงินคงเหลือน้อยที่สุด

* Group ข้อมูลยอดรวม Income และ Expense ของคนงานเกษตรในแต่ละภาค แต่ละปี
* ปรับแก้ชื่อภาคที่สะกดไม่เหมือนกัน ให้เหมือนกัน
* Merge ข้อมูลตาราง Income และ Expense จากนั้นใช้ในการหา Monthly Saving(เงินคงเหลือ)
* จากนั้นทําการ Plot Graph

````
df_in_argi = df_in[(df_in['soc_eco_class'] == 'คนงานเกษตร')]
group_in_argi = df_in_argi.groupby(['year','region','soc_eco_class','source_income'])
df_in_argi = group_in_argi.agg('mean').reset_index()
group_in_argi = df_in_argi.groupby(['year','region','soc_eco_class'])
df_in_argi = group_in_argi.agg('sum').reset_index()
df_in_argi = df_in_argi.drop(columns=['soc_eco_class'])
df_in_argi = df_in_argi.replace('กรุงเทพ นนทบุรี ปทุมธานี และสมุทรปราการ','กทม. นนทบุรี ปทุมธานี และสมุทรปราการ') 
````

````
df_x_argi = df_x[(df_x['soc_eco_class'] == 'คนงานเกษตร')]
group_x_argi = df_x_argi.groupby(['year','region','soc_eco_class','type_expenditur'])
df_x_argi = group_x_argi.agg('mean').reset_index()
group_x_argi = df_x_argi.groupby(['year','region','soc_eco_class'])
df_x_argi = group_x_argi.agg('sum').reset_index()
df_x_argi = df_x_argi.drop(columns=['soc_eco_class'])
````

````
df_merge_argi = pd.merge(df_in_argi,df_x_argi, left_on =['year','region'], right_on =['year','region'], how ='left' )
df_merge_argi.rename(columns = {'value_x':'Avg. Monthly Income','value_y':'Avg. Monthly Expense'},inplace=True)
df_merge_argi['Avg. Monthly Saving'] = df_merge_argi['Avg. Monthly Income'] - df_merge_argi['Avg. Monthly Expense']
df_merge_argi
````

````
df_plot_low = df_merge_argi.drop(columns = ['Avg. Monthly Income','Avg. Monthly Expense'])
pivot = df_plot_low.pivot_table(index= 'region',columns='year',values = 'Avg. Monthly Saving')
fig,ax = plt.subplots(figsize=(15, 10),dpi=300)
fig.suptitle('คนงานเกษตร: Monthly Saving',ha='right')
ax = sns.heatmap(pivot,linewidths=1,annot=True,fmt='.0f',cbar_kws ={"shrink":.8,'extend':'both'})
````

![image](https://user-images.githubusercontent.com/101727971/196038676-7651286f-1578-4703-8ec9-561b5e30cdfb.png)


Q5) ใครแนวโน้มที่จะมีเงินเหลือน้อยที่สุด และมีช่วงไหนหรือไม่ ที่ไม่มีเงินคงเหลือเลย 
- จากข้อมูลพบว่า คนงานเกษตรในภาคใต้ในช่วงปี 2558 และ 2560 ไม่มีเงินเหลือเก็บเลย
- ในปีข้อมูลล่าสุดปี 2562 คนงานเกษตรใน zone กรุงเทพและจังหวัดไกล้เคียงก็ ไม่มีเงินเหลือเก็บเลยเช่นกัน

# Challenge
	
* การใช้เวลาการเข้าใจข้อมูล และการหาจัดที่ไม่ถูกต้องของข้อมูล
* การทํา Data cleansing ของมูลบางรายการทําได้ยาก ส่วนหนึ่งอาจจะมาจากการที่ไม่รู้การเขียน code ที่เหมาะสม ที่สามารถลดเวลาการเขียนลงได้
* ต้องมีการ Grouping ข้อมูลหลายด้าน หลายแบบ ซึ่งใช้เวลานาน
* การจัดทํา Visualization graph ใช้เวลานานมาก เนื่องจากประกบการณ์ในการทํายังน้อยอยู่ ต้องไป search หาข้อมูลเพิ่มเติมตลอดเวลา
