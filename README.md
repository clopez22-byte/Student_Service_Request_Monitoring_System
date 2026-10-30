# Student Service Request Monitoring System — Streamlit

## Project purpose
A decision-support application for monitoring student service requests and estimating potential SLA breaches. It reuses the midterm Section B Decision Tree approach.

## Included modules
- Home
- Dataset upload, schema validation, preview, null and duplicate summary
- Data Analysis with descriptive statistics and three interactive Plotly charts
- Prediction using a saved preprocessing + Decision Tree pipeline
- Decision-support dashboard and staff review queue
- System Design with actors, requirements, supplied use-case diagram, and analytical workflow

## Midterm model details
- Target: `SLA_Breached` (1 = breached, 0 = not breached)
- Training features: Service_Type, Requester_Type, Requested_Channel, Priority, Assigned_Office, Queue_Length, Required_Documents_Complete, SLA_Days, Submission_Weekday, Queue_Category
- Algorithm: `DecisionTreeClassifier(random_state=42, class_weight="balanced")`
- Preprocessing: `OneHotEncoder(handle_unknown="ignore")` for categorical variables; numeric variables passed through
- Split: 80% train / 20% test, random_state 42
- Eligible labels: completed requests with known `Processing_Days`, `SLA_Days`, valid submission date, and queue length
- Leakage precaution: `Processing_Days`, `Status`, `Reopened`, and `SLA_Breached` are not prediction inputs. `Processing_Days` is used only to create the historical target for completed records.

## Evaluation results generated from the supplied CSV
- Eligible completed records: 270
- Train/test records: 216 / 54
- Accuracy: 0.7037
- Precision: 0.5455
- Recall: 0.6667
- F1-score: 0.6000
- Confusion matrix: [[26, 10], [6, 12]]

## Important limitations
The model is trained from historical completed requests because only those records have a known SLA outcome. Predictions for new/open requests are advisory and may not generalize well. The supplied UML image is included as the System Design diagram. Use synthetic or authorized data only. Do not use predictions to automatically deny student services.
