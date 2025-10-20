
Formula 1 Race Outcome Prediction Model
Overview
This project develops a machine learning model to predict Formula 1 race outcomes using historical timing and qualifying data processed through the FastF1 Python library.
The model leverages driver statistics, track history, and recent performance trends to estimate the final race order for an upcoming Grand Prix.
The included example focuses on predicting the 2025 Singapore Grand Prix, where the model achieved strong alignment with actual results:
•	Actual podium: Verstappen / Norris / Leclerc
•	Predicted podium: Norris / Verstappen / Leclerc
________________________________________
Key Features
•	Retrieves and caches official Formula 1 data locally via FastF1
•	Generates driver-level features such as:
o	Starting grid position
o	Qualifying time
o	Average historical performance at the same circuit
o	Average finishing position over the previous five races
•	Trains an XGBoost ranking model to predict finishing order
•	Applies a Monte Carlo simulation to estimate podium probabilities
•	Outputs the predicted top three finishers with confidence percentages
________________________________________
Methodology
1. Data Collection
Historical race and qualifying data from the 2023–2024 seasons are collected using FastF1.
The library automatically caches timing data locally for offline access.
2. Feature Engineering
Each driver’s race entry is represented by:
•	GridPosition — official grid slot
•	QualifyingTime — fastest valid qualifying lap
•	TrackHistory — mean past finishing position at the same circuit
•	RecentForm — mean finishing position in the most recent five races
Missing values are handled using median imputation.
3. Model Training
An XGBRanker model is trained with a pairwise ranking objective (rank:pairwise) and evaluated using the normalized discounted cumulative gain metric (ndcg@5).
Training groups are organized by race to preserve intra-event ranking context.
4. Prediction Phase
For a target event (e.g., 2025 Singapore GP), the qualifying session is analyzed, features are generated, and each driver’s expected performance score is predicted.
Drivers are ranked by predicted score to form the forecasted finishing order.
5. Probability Estimation
A Monte Carlo simulation with 10,000 iterations adds Gaussian noise to the predicted scores to estimate the likelihood of each driver finishing on the podium.
________________________________________
Technologies Used
Category	Tools / Libraries
Data Access	FastF1
Data Processing	pandas, numpy
Modeling	XGBoost (XGBRanker)
Imputation	scikit-learn (SimpleImputer)
Visualization	matplotlib
Progress Tracking	tqdm
Caching	Local FastF1 directory (f1_US_GP/)

________________________________________
 Example Output
🏆 Predicted Top 3 for 2025 Singapore Grand Prix 🏆
🥇 P1: NOR with 99.4% chance of a podium finish.
🥈 P2: VER with 98.7% chance of a podium finish.
🥉 P3: LEC with 76.2% chance of a podium finish.
________________________________________
 Model Evaluation
The model demonstrated strong predictive performance:
•	Correctly identified all podium finishers
•	Minor positional deviation (Norris and Verstappen swapped)
•	Robust consistency across multiple Monte Carlo iterations
Potential future improvements include:
•	Incorporating weather, pit stop, and tire data
•	Integrating telemetry metrics for more granular performance indicators
•	Conducting hyperparameter optimization for further accuracy gains
________________________________________
Data Source
Race data is accessed exclusively through the FastF1 Python library, which provides structured access to official Formula 1 timing and results data.
FastF1 handles session retrieval, caching, and parsing without the need for external APIs or authentication keys.
Documentation: https://docs.fastf1.dev

