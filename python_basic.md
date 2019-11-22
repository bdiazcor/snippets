# Grouping and map values
data['pred'] = data['pred_proba'].map(lambda x: 1 if x > threshold else 0)
