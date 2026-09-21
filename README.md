# ai-medical-robotics-exercises
from sklearn.datasets import load_breast_cancer
import pandas as pd

data = load_breast_cancer()
df = pd.DataFrame(data.data, columns=data.feature_names)
df['Diagnosis'] = pd.Series(data.target).map({0: 'Malignant', 1: 'Benign'})

# we use those 4 features in the exercise
cols = ['mean radius', 'mean texture', 'mean perimeter', 'mean area']
df[cols + ['Diagnosis']].head()
