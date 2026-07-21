# Predicting Innovation Impact from Research Collaboration Networks

MSc coursework project (Advanced Analytics & Machine Learning, Queen's University Belfast). I ran a full applied ML workflow on a 35,308-record biomedical research dataset to test what actually drives citation impact: novelty, knowledge-network position, or author collaboration.

## Problem

Citation counts are extremely right-skewed (skewness 18.7 raw, 0.27 after log transform) and driven by overlapping, correlated signals. Novelty scores, topic breadth and collaboration-network position all move together, so the real question was which of these genuinely predicts impact once multicollinearity is accounted for. That called for a full model-comparison workflow rather than a single method.

## Approach

- Feature selection: LASSO regression (glmnet) with cross-validated lambda tuning to shrink 16 correlated predictors down to a stable set, reported at both lambda.min and lambda.1se.
- Non-linear modelling: pruned decision tree and random forest (rpart, ranger) to capture thresholds and interactions LASSO can't see, with permutation importance and partial dependence plots.
- Classification: LDA, linear SVM and radial SVM (MASS, e1071) to classify high vs low citation records on a near-balanced 51/49 split, evaluated on accuracy, balanced accuracy, precision, recall and F1.
- Model complexity control: bias-variance analysis across random forest mtry values, comparing training, CV and test error to justify the final model choice.
- Non-linearity check: polynomial regression (linear, quadratic, cubic) on team size, showing an inverted-U relationship between collaboration size and citation impact.
## Key results

| Stage | Best model | Result |
|---|---|---|
| Regression | Random Forest (mtry=2) | Test RMSE 1.40, beats pruned tree (RMSE 1.43) |
| Classification | Radial SVM | 67.8% accuracy, 68.8% recall on high-citation class |
| Non-linearity | Cubic polynomial (AuthorCount) | Adjusted R-squared 0.192 vs 0.154 linear |

Collaboration-network variables (Degree_Avg, Core_Number_Avg, AuthorCount, PageRank_Avg) consistently outranked novelty scores in predictive importance. Team visibility and network position matter more than idea novelty alone, and returns diminish (and eventually turn negative) for very large teams.

## Tools

R, glmnet, ranger, rpart, MASS, e1071, caret, ggplot2

## Files

- report.pdf: full write-up with all tables, plots and methodology
- analysis.Rmd: R Markdown source, fully reproducible (set.seed fixed throughout)
