## Grouping and map values
data['pred'] = data['pred_proba'].map(lambda x: 1 if x > threshold else 0)

## NBConvert
import nbconvert
! jupyter nbconvert Classification_model.ipynb

! jupyter nbconvert --template=nbextensions --to=html my_notebook.ipynb
