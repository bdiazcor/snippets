# Interpretability

import eli5   
from eli5.sklearn import PermutationImportance   
perms = PermutationImportance(clf_rf2,random_state=1).fit(X_test,y_test)   
eli5.show_weights(perms, feature_names=X_test.columns.tolist())   


# Select the best encoder

encoder_list = [ce.backward_difference.BackwardDifferenceEncoder,   
                  ce.basen.BaseNEncoder,   
                  ce.binary.BinaryEncoder,   
                  ce.cat_boost.CatBoostEncoder,   
                  ce.hashing.HashingEncoder,   
                  ce.helmert.HelmertEncoder,  
                  ce.james_stein.JamesSteinEncoder,   
                  ce.one_hot.OneHotEncoder,  
                  ce.leave_one_out.LeaveOneOutEncoder,   
                  ce.m_estimate.MEstimateEncoder,   
                  ce.ordinal.OrdinalEncoder,   
                  ce.polynomial.PolynomialEncoder,   
                  ce.sum_coding.SumEncoder,   
                  ce.target_encoder.TargetEncoder,   
                  ce.woe.WOEEncoder   
                 ]   

for encoder in encoder_list:

    numeric_transformer = RobustScaler()
    categorical_transformer = encoder()
    
    preprocessor = ColumnTransformer(
        transformers=[
            ('num', numeric_transformer, num_feature),
            ('cat', categorical_transformer, cat_feature)])
    
    pipe = Pipeline(steps=[('preprocessor', preprocessor),
                          ('classifier', RandomForestClassifier(n_estimators=800,
                                                                min_samples_leaf=20,
                                                                max_depth=3,
                                                                class_weight='balanced',
                                                                random_state=42))])
    #pipe = Pipeline(steps=[('preprocessor', preprocessor),
    #                       ('classifier', RandomForestClassifier(n_estimators=800))])
                                                                 
    model = pipe.fit(X_train, y_train)
    
    y_pred = model.predict(X_test)
    print(encoder)
    print(f1_score(y_test, y_pred))
