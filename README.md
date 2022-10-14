# DADS5001_Mini-Project
# สำรวจกระเป๋าตังคนไทย รายได้ และ รายจ่าย เฉลี่ยของครัวเรือน
ภาคครัวเรือนถือว่าเป็นองค์ประกอบที่สำคัญของระบบเศรษฐกิจ กิจกรรมทางเศรษฐกิจต่าง ๆ ของครัวเรือนทั้งการประกอบอาชีพ การบริโภค การออม การใช้จ่าย รวมไปถึงภูมิประเทศ ถิ่นที่อยู่อาศัย ล้วนเป็นปัจจัยให้เกิดความแตกต่างในรายได้ และรายจ่ายครัวเรือน
# Author
* Korawee Peerasantikul
* ID: 6420422007
* Subject: DADS5001
# Objective: 
1) Do Thai people have earn enough money for living      
2) What do  they spending most of their money for     
3) Income, Expense, and Saving of each Region and Social Class in Thailand
4) Finding Lowest monthly saving group in Thailand
# Dataset Information
**File Name:**   space_missions.csv\
**Source:**   (https://catalog.nso.go.th/dataset/?res_format=CSV&groups=os-08) \
**Role and Collumn:**    
* รายได้เฉลี่ยต่อครัวเรือน (4,600 rows)
* 7 Collumn
  *  year           
  *  region         
  *  source_income   
  *  soc_eco_class  
  *  value        
  *  unit           
  *  source
* รายจ่ายเฉลี่ยต่อครัวเรือน (2,600 rows)
* 7 Collumn
  *  year     
  *  region         
  *  type_expenditur  
  *  soc_eco_class    
  *  value            
  *  unit          
  *  source
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
