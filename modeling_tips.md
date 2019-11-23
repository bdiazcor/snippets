# Interpretability

import eli5
from eli5.sklearn import PermutationImportance
perms = PermutationImportance(clf_rf2,random_state=1).fit(X_test,y_test)
eli5.show_weights(perms, feature_names=X_test.columns.tolist())


