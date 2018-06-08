

```python
import os
import pandas as pd
import json
import numpy as np
```


```python
with open("purchase_data.json") as datafile:
    data = json.load(datafile)
game_df = pd.DataFrame(data)

game_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Age</th>
      <th>Gender</th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
      <th>SN</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>38</td>
      <td>Male</td>
      <td>165</td>
      <td>Bone Crushing Silver Skewer</td>
      <td>3.37</td>
      <td>Aelalis34</td>
    </tr>
    <tr>
      <th>1</th>
      <td>21</td>
      <td>Male</td>
      <td>119</td>
      <td>Stormbringer, Dark Blade of Ending Misery</td>
      <td>2.32</td>
      <td>Eolo46</td>
    </tr>
    <tr>
      <th>2</th>
      <td>34</td>
      <td>Male</td>
      <td>174</td>
      <td>Primitive Blade</td>
      <td>2.46</td>
      <td>Assastnya25</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>1.36</td>
      <td>Pheusrical25</td>
    </tr>
    <tr>
      <th>4</th>
      <td>23</td>
      <td>Male</td>
      <td>63</td>
      <td>Stormfury Mace</td>
      <td>1.27</td>
      <td>Aela59</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Total Number of Players
playerct=game_df['SN'].unique()
len(playerct)
p3={'Total_Player':[573]}
pct_df=pd.DataFrame(p3,columns=['Total_Player'])

pct_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Total_Player</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>573</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Total Number of Unique Items
item=game_df['Item Name'].unique()
len(item)
u1={'Total_Unique':[179]}
unique_df=pd.DataFrame(u1,columns=['Total_Unique'])
```


```python
#Average Purchase Price
AvPrice=game_df['Price'].sum()
Total_Item=game_df['Item Name'].count()

Average=AvPrice/Total_Item
Average

a1={'Average_Price':[2.93]}
average_df=pd.DataFrame(a1,columns=['Average_Price'])
```


```python
#Total Number of Purchases
n1={'Total_Purchases':[780]}
totalpur_df=pd.DataFrame(n1,columns=['Total_Purchases'])
```


```python
#Total Revenue
r1={'Total_Revenue':[2286.33]}

revenue_df=pd.DataFrame(r1,columns=['Total_Revenue'])
```


```python
final_analysis=pd.concat([pct_df,unique_df,average_df,totalpur_df,revenue_df],axis=1)
final_analysis
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Total_Player</th>
      <th>Total_Unique</th>
      <th>Average_Price</th>
      <th>Total_Purchases</th>
      <th>Total_Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>573</td>
      <td>179</td>
      <td>2.93</td>
      <td>780</td>
      <td>2286.33</td>
    </tr>
  </tbody>
</table>
</div>




```python
x=len(game_df.loc[game_df['Gender']=='Male']['SN'].unique())
xper='{:.2f}%'.format(x/573*100)
```


```python
y=len(game_df.loc[game_df['Gender']=='Female']['SN'].unique())
yper='{:.2f}%'.format(y/573*100)
```


```python
z=len(game_df.loc[game_df['Gender']=='Other / Non-Disclosed']['SN'].unique())
zper='{:.2f}%'.format(z/573*100)
```


```python
xyz=game_df.pivot_table(game_df,'Gender')
xyz=xyz.drop(columns=['Age','Item ID','Price'])
xyz['Total Count']=y,x,z
xyz['Percentage']=yper,xper,zper
xyz
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Total Count</th>
      <th>Percentage</th>
    </tr>
    <tr>
      <th>Gender</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Female</th>
      <td>100</td>
      <td>17.45%</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>465</td>
      <td>81.15%</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>8</td>
      <td>1.40%</td>
    </tr>
  </tbody>
</table>
</div>




```python
genders_sum = game_df.pivot_table(game_df,'Gender')
mx=game_df.loc[game_df['Gender']=='Male']['Item ID'].count()
fx=game_df.loc[game_df['Gender']=='Female']['Item ID'].count()
ox=game_df.loc[game_df['Gender']=='Other / Non-Disclosed']['Item ID'].count()

ma=game_df.loc[game_df['Gender']=='Male']['Price'].mean()
fa=game_df.loc[game_df['Gender']=='Female']['Price'].mean()
oa=game_df.loc[game_df['Gender']=='Other / Non-Disclosed']['Price'].mean()

ms=game_df.loc[game_df['Gender']=='Male']['Price'].sum()
fs=game_df.loc[game_df['Gender']=='Female']['Price'].sum()
os=game_df.loc[game_df['Gender']=='Other / Non-Disclosed']['Price'].sum()

mn=ms/465
fn=fs/100
on=os/8

genders_sum=genders_sum.drop(columns=['Age','Price','Item ID'])
genders_sum['Purchase Count']=fx,mx,ox
genders_sum['Average Price']=fa,ma,oa
genders_sum['Total Purchase Value']=fs,ms,os
genders_sum['Normalized Totals']=fn,mn,on

genders_sum

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase Count</th>
      <th>Average Price</th>
      <th>Total Purchase Value</th>
      <th>Normalized Totals</th>
    </tr>
    <tr>
      <th>Gender</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Female</th>
      <td>136</td>
      <td>2.815515</td>
      <td>382.91</td>
      <td>3.829100</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>633</td>
      <td>2.950521</td>
      <td>1867.68</td>
      <td>4.016516</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>11</td>
      <td>3.249091</td>
      <td>35.74</td>
      <td>4.467500</td>
    </tr>
  </tbody>
</table>
</div>




```python
unique_demo=game_df.pivot_table(game_df,'SN')
```


```python
bins = [0,9,14,19,24,29,34,39,500]

age_group=['<10','10-14','15-19','20-24','25-29','30-34','35-39','40+']

unique_demo["Age Bucket"] = pd.cut(unique_demo["Age"], bins, labels=age_group)

unique_demo.head()

age_series=unique_demo['Age Bucket'].value_counts()

age_bucket=age_series.to_frame(name=None)

age_bucket['Total Percentage']=np.round((age_bucket['Age Bucket']/573*100), decimals=2)
```


```python
#Reindex
age_bucket=age_bucket.reindex(index=['<10','10-14','15-19','20-24','25-29','30-34','35-39','40+'])

#Total Purchase Count
uten=game_df.loc[game_df['Age']<10]['Item ID'].count()
ten=game_df.loc[(game_df['Age']>=10) & (game_df['Age']<15)]['Item ID'].count()
fifteen=game_df.loc[(game_df['Age']>=15) & (game_df['Age']<20)]['Item ID'].count()
twenty=game_df.loc[(game_df['Age']>=20) & (game_df['Age']<25)]['Item ID'].count()
twentyfive=game_df.loc[(game_df['Age']>=25) & (game_df['Age']<30)]['Item ID'].count()
thirty=game_df.loc[(game_df['Age']>=30) & (game_df['Age']<35)]['Item ID'].count()
thirtyfive=game_df.loc[(game_df['Age']>=35) & (game_df['Age']<40)]['Item ID'].count()
forty=game_df.loc[game_df['Age']>=40]['Item ID'].count()

#Average Purchase Price
auten=game_df.loc[game_df['Age']<10]['Price'].mean()
aten=game_df.loc[(game_df['Age']>=10) & (game_df['Age']<15)]['Price'].mean()
afifteen=game_df.loc[(game_df['Age']>=15) & (game_df['Age']<20)]['Price'].mean()
atwenty=game_df.loc[(game_df['Age']>=20) & (game_df['Age']<25)]['Price'].mean()
atwentyfive=game_df.loc[(game_df['Age']>=25) & (game_df['Age']<30)]['Price'].mean()
athirty=game_df.loc[(game_df['Age']>=30) & (game_df['Age']<35)]['Price'].mean()
athirtyfive=game_df.loc[(game_df['Age']>=35) & (game_df['Age']<40)]['Price'].mean()
aforty=game_df.loc[game_df['Age']>=40]['Price'].count()

#Total Purchase Price
tuten=game_df.loc[game_df['Age']<10]['Item ID'].mean()
tten=game_df.loc[(game_df['Age']>=10) & (game_df['Age']<15)]['Price'].sum()
tfifteen=game_df.loc[(game_df['Age']>=15) & (game_df['Age']<20)]['Price'].sum()
ttwenty=game_df.loc[(game_df['Age']>=20) & (game_df['Age']<25)]['Price'].sum()
ttwentyfive=game_df.loc[(game_df['Age']>=25) & (game_df['Age']<30)]['Price'].sum()
tthirty=game_df.loc[(game_df['Age']>=30) & (game_df['Age']<35)]['Price'].sum()
tthirtyfive=game_df.loc[(game_df['Age']>=35) & (game_df['Age']<40)]['Price'].sum()
tforty=game_df.loc[game_df['Age']>=40]['Item ID'].count()

#Normalized Totals
nuten=tuten/19
nten=tten/23
nfifteen=tfifteen/100
ntwenty=ttwenty/259
ntwentyfive=ttwentyfive/87
nthirty=tthirty/47
nthirtyfive=tthirtyfive/27
nforty=tforty/11
#Adjustments
age_bucket=age_bucket.drop(columns=['Age Bucket','Total Percentage'])

#New Columns
age_bucket['Purchase Count']=uten,ten,fifteen,twenty,twentyfive,thirty,thirtyfive,forty
age_bucket['Average Purchase Price']=auten,aten,afifteen,atwenty,atwentyfive,athirty,athirtyfive,aforty
age_bucket['Total Purchase Price']=tuten,tten,tfifteen,ttwenty,ttwentyfive,tthirty,tthirtyfive,tforty
age_bucket['Normalized Totals']=nuten,nten,nfifteen,ntwenty,ntwentyfive,nthirty,nthirtyfive,nforty

age_bucket

#Don't judge me. I fell into a spiral of code.
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Price</th>
      <th>Normalized Totals</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>28</td>
      <td>2.980714</td>
      <td>82.321429</td>
      <td>4.332707</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>35</td>
      <td>2.770000</td>
      <td>96.950000</td>
      <td>4.215217</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>133</td>
      <td>2.905414</td>
      <td>386.420000</td>
      <td>3.864200</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>336</td>
      <td>2.913006</td>
      <td>978.770000</td>
      <td>3.779035</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>125</td>
      <td>2.962640</td>
      <td>370.330000</td>
      <td>4.256667</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>64</td>
      <td>3.082031</td>
      <td>197.250000</td>
      <td>4.196809</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>42</td>
      <td>2.842857</td>
      <td>119.400000</td>
      <td>4.422222</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>17</td>
      <td>17.000000</td>
      <td>17.000000</td>
      <td>1.545455</td>
    </tr>
  </tbody>
</table>
</div>




```python
spenders=np.round(game_df.pivot_table(game_df,'SN',aggfunc=sum),decimals=2)
spenders=spenders.sort_values(by=['Price'],ascending=False)
spenders=spenders.drop(columns=['Age','Item ID'])
spenders=spenders.rename(columns={'Price':'Total Purchase Value'})
```


```python
spender=game_df.groupby('SN')
spender=spender['Price'].mean()
spender=np.round(spender.to_frame(name=None),decimals=2)
```


```python
spender2=game_df.groupby('SN')
spender2=spender2['SN'].count()
spender2=np.round(spender2.to_frame(name=None),decimals=2)
```


```python
final=pd.concat([spenders,spender,spender2],axis=1)
final=final.sort_values(by=['Total Purchase Value'],ascending=False)
real_final=final.head(5)
real_final=real_final.rename(columns={'Price':'Average Purchase Value','SN':'Total Purchases'})

real_final
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Total Purchase Value</th>
      <th>Average Purchase Value</th>
      <th>Total Purchases</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Undirrala66</th>
      <td>17.06</td>
      <td>3.41</td>
      <td>5</td>
    </tr>
    <tr>
      <th>Saedue76</th>
      <td>13.56</td>
      <td>3.39</td>
      <td>4</td>
    </tr>
    <tr>
      <th>Mindimnya67</th>
      <td>12.74</td>
      <td>3.18</td>
      <td>4</td>
    </tr>
    <tr>
      <th>Haellysu29</th>
      <td>12.73</td>
      <td>4.24</td>
      <td>3</td>
    </tr>
    <tr>
      <th>Eoda93</th>
      <td>11.58</td>
      <td>3.86</td>
      <td>3</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Most Popular Items
pop_item=(game_df.pivot_table(game_df,'Item Name'))

countpop=game_df['Item Name'].value_counts()

pop_item['Item Count']=countpop

next1=pop_item.sort_values(['Item Count'],ascending=False)
next1=next1.drop(columns=['Age'])

next1['Total']=next1['Price']*next1['Item Count']

next2=next1.head()

next2
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Item ID</th>
      <th>Price</th>
      <th>Item Count</th>
      <th>Total</th>
    </tr>
    <tr>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Final Critic</th>
      <td>95.857143</td>
      <td>2.757143</td>
      <td>14</td>
      <td>38.60</td>
    </tr>
    <tr>
      <th>Arcane Gem</th>
      <td>84.000000</td>
      <td>2.230000</td>
      <td>11</td>
      <td>24.53</td>
    </tr>
    <tr>
      <th>Betrayal, Whisper of Grieving Widows</th>
      <td>39.000000</td>
      <td>2.350000</td>
      <td>11</td>
      <td>25.85</td>
    </tr>
    <tr>
      <th>Stormcaller</th>
      <td>105.000000</td>
      <td>3.465000</td>
      <td>10</td>
      <td>34.65</td>
    </tr>
    <tr>
      <th>Woeful Adamantite Claymore</th>
      <td>175.000000</td>
      <td>1.240000</td>
      <td>9</td>
      <td>11.16</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Most Profitable Items

prof_item=(game_df.pivot_table(game_df,'Item Name'))
prof1=game_df['Item Name'].value_counts()
prof_item['Count']=prof1
prof_item['Total']=prof_item['Price']*prof_item['Count']
prof_item=prof_item.sort_values(['Total'],ascending=False)

prof_item=prof_item.drop(columns=['Age'])

prof_final=prof_item.head()

prof_final
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Item ID</th>
      <th>Price</th>
      <th>Count</th>
      <th>Total</th>
    </tr>
    <tr>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Final Critic</th>
      <td>95.857143</td>
      <td>2.757143</td>
      <td>14</td>
      <td>38.60</td>
    </tr>
    <tr>
      <th>Retribution Axe</th>
      <td>34.000000</td>
      <td>4.140000</td>
      <td>9</td>
      <td>37.26</td>
    </tr>
    <tr>
      <th>Stormcaller</th>
      <td>105.000000</td>
      <td>3.465000</td>
      <td>10</td>
      <td>34.65</td>
    </tr>
    <tr>
      <th>Spectral Diamond Doomblade</th>
      <td>115.000000</td>
      <td>4.250000</td>
      <td>7</td>
      <td>29.75</td>
    </tr>
    <tr>
      <th>Orenmir</th>
      <td>32.000000</td>
      <td>4.950000</td>
      <td>6</td>
      <td>29.70</td>
    </tr>
  </tbody>
</table>
</div>


