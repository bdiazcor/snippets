## Grouping and map values
data['pred'] = data['pred_proba'].map(lambda x: 1 if x > threshold else 0)

## NBConvert
import nbconvert
! jupyter nbconvert Classification_model.ipynb

## Nbconvert with Hide Inputs
! jupyter nbconvert --template=nbextensions --to=html my_notebook.ipynb

## Merge Dataframes
df_local16 = df_epi16_cl_d.merge(df_epi15_cl_d, how= 'left', 
                                    on=columns_to_merge,
                                    suffixes=('_16','_15'))



## Loops examples
for i in desc_act_fil.index.values:   
    desc_act_fil.loc[desc_act_fil.index == i,'id'] = df_local19_ft.loc[df_local19_ft.desc_epigrafe == i].id_epigrafe.unique()[0]
    

## Replace characters and list comprehension
df_locals_v5['coord_x_f'] = [c.replace(',', '.') for c in df_locals_v5['coordenada_x_local'].values]

## New variables
ids = sort(df_locals_v6.id_distrito_local.unique())   
total_local_act = df_locals_v6.groupby('id_distrito_local').count().id_local.values   
total_locales = df_local19_ft.groupby('id_distrito_local').count().id_local.values   
dict = {'id_distrito_local': ids, 'total_local_act': total_local_act, 'total_locales': total_locales}   
kpis_loc = pd.DataFrame(dict)   
kpis_loc   


## Plots   
plot = df_locals_v8['target'].value_counts().plot(kind='bar',
                                            title='Target distribution')

### Seaborn
import seaborn as sns   
plt.figure(figsize=(5,5))   
g = sns.countplot(x = 'desc_distrito_local', data = df_locals_v8)   
plt.show()   

#### Heatmap
plt.figure(figsize=(12,12))   
sns.heatmap(df_locals_v8_prep.corr(),annot = True, cmap ='coolwarm')  


# Columns transform, scaler and OneHot
df_locals_v9 = df_locals_v8.drop(['target','id_distrito_local'],axis=1)   
cat_feature=['desc_distrito_local','desc_tipo_agrup']   
num_feature=df_locals_v9.columns[df_locals_v9.dtypes.apply(lambda c: np.issubdtype(c,np.number))]   
num_feature   
X_cat = df_locals_v8[cat_feature]   
X_num = df_locals_v8[num_feature]   
y = df_locals_v8['target']   

X_cat = pd.get_dummies(X_cat)   

##### scaler = MinMaxScaler()   
scaler = StandardScaler()   
X_num_scaled = scaler.fit_transform(X_num)    
X_num_scaled = pd.DataFrame(X_num_scaled, columns=['total_locales', 'num_loc_d','total_local_act'])   
X_cat.columns = X_cat.columns.str.strip()   
X = pd.merge(X_num_scaled,X_cat,left_index=True,right_index=True)   
df_locals_v8_prep = X.copy()   
df_locals_v8_prep['target'] = y   
X.shape, df_locals_v8_prep.shape   

