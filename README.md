# Predicting_UK_Train_Delays_in_Real_Time_and_Classifying_Route_Risk


#Problem Statement
Predict arrival delays and classify route risk levels in real time from live UK rail network updates, leveraging the Open Rail Data STOMP feed and integrating with AWS Kinesis + Lambda + ML models.

🔗 Data Sources
Open Rail Data STOMP Feed (via the Push Port):

Live train movement messages (e.g., Train Activation, Train Movement, Train Cancellation).

Can be parsed using libraries like stomp.py.

Reference: Open Rail Data

Static Timetable Data:

Use for training baseline models and comparing predicted vs actual arrival times.

##Achitecture Overview

⚙️ AWS Components
Component	Role
Kinesis Data Stream	Ingests real-time STOMP messages from listener client
EC2 / Docker Client	Runs the Python STOMP listener (stomp.py) → pushes to Kinesis
Lambda	Triggers on new messages, extracts features, invokes ML model
SageMaker Endpoint	Hosts trained ML model (e.g., Random Forest)
S3 / DynamoDB	Stores predictions, feature logs, metadata
MLflow	Tracks experiments, RMSE, feature importance, artifacts

🧠 ML Tasks
1. Task A: Train Delay Regression
Model: RandomForestRegressor, LinearRegression

Target: Arrival delay (minutes)

Features:

Train ID

Departure station

Arrival station

Time of day

Day of week

Weather (optional)

Current status: delayed, running, cancelled

Current and prior movement updates

Distance remaining

2. Task B: Route Risk Classification
Model: DecisionTreeClassifier or RandomForestClassifier

Target: Route risk level (Low / Medium / High) based on:

Route congestion

Historical delay probability

Weather

Number of active disruptions on the route

✅ MLOps Features
Feature	Tool
Model Tracking	MLflow or SageMaker Experiments
Model Registry	MLflow or SageMaker Registry
CI/CD for Model Updates	GitHub Actions + SageMaker Pipelines
Real-Time Inference	Kinesis + Lambda + SageMaker Endpoint
Batch Retraining	Step Functions or Lambda on schedule
Drift Detection	Use CloudWatch metrics + feature stats
Visualization	Use Quicksight or Grafana (via Timestream)

🧪 Bonus: Validation Ideas
Simulate test scenarios with synthetic delays or route disruptions.

Build dashboards that show:

Live prediction of next train arrival delays.

Map view of "risk levels" by route in real-time.

Time-series of actual vs predicted delays.

