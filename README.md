#  Group 01 Forecasting Flight Delay Final Project
### Dylan DiBello, Chuqin Wu, Frank Gonzalez, Sung Beom Park, Siyu Xue (Melanie)


# How To Run

Experienced Cluster Issues which would sometimes cause our data to be unaccessible and caused our models to throw data type errors. We are unsure of the exact reason behind this, but if you experience issues we found success when restarting the cluster occassionaly. 

The “DSC202-402 Forecasting Flight Delay Final Project” notebook is the top level notebook which is used to specify training dates for the models as well as to show that each of our individual notebooks can run properly. There is a cell to test each notebook that we have created for this project: “DIA”, “EDA”,”ETL”,”ML_ARR”,”ML_DEP”,”Monitoring”. Each notebook will return a success when complete and the message “Passed” is shown as the result of the cell as proof that the notebook ran successfully. 


To interface with our application as a user, run the DIA notebook and then set the view from “Standard” to “DIA Dashboard.” Next, press the “Present Dashboard” button on the right hand of the screen to view the final dashboard. The dashboard allows the user to select an airport and time entry. The dashboard shows the next 24 hours of departures and arrivals for the selected airport and time entry. Each flight number is shown along with the scheduled arrival/departure time, our models delay estimate, and then the updated estimated arrival/departure time. If the user changes the time entry or airport code the dashboard will update to display the requested information. 




## ETL


Frank Gonzalez


For the initial stages of the project we obtained two bronze level data instances of weather and airline data. We subset the data to include the specified airports of interest. We created two additional instances of weather tables to also provide arrival/destination weather information. Additional feature engineering was applied to create timestamp values based on other airline features and transform base weather features. Cleaned bronze airline,weather1,weather2, tables were joined to create departure and arrival weather features.


## MLOps


Melanie Xue / Sung Beom Park


* Input: airport_code, traing_start_date, training_end_date
* Output: staging models for predicting arrival delay and departure delay
* Prediction description: arrival delay -> the arrival delay time of all flights coming into the desired airport. Departure delay -> the departure delay time of all flights coming out of the desired airport. 
* Consists of ML_ARR & ML_DEP:
   * ML_ARR: Used month, distance, day_of_week, dep_delay, wnd_deg, wnd_mps, vis_km, temp_f as features and arr_delay as the predictor, as they provided the most optimal results. Originally, arr_delay was also included as one of the features as it gave extremely high accuracy, but it was shortly realized that it does not make sense to include a predictor as its features, thus it was removed. The model gave moderate performance of about 10.0 as its best loss. 
Gradient Boost (xgboost) was used as the machine learning algorithm for this problem. After many iterations, parameters (max_depth, log_learning_rate, log_reg_alpha, log_reg_lambda, log_gamma, log_min_child_weight) were adjusted to perform well. After that, the model was registered with the registry as “group01_model_ARR.”
* ML_DEP: Used month, distance, day_of_week, arr_delay, wnd_deg, wnd_mps, vis_km, temp_f as features and dep_delay as the predictor. Again, dep_delay was also included as one of the features, but it did not make sense. This model performed better than the ML_ARR as its best loss was 8.38. 
                Gradient Boost was used as well, and the model was registered. 


## ML Monitoring Dashboard


Chuqin Wu


The monitoring notebook uses the input training date and inference date from the widgets and creates the corresponding training set and testing set. The monitor process checks data anomalies and schema mismatches using the tensorflow library. And then it checks the model consistency by plotting the residual error for both training set and testing set. The qualified models are promoted to production in the end.


## DIA


Dylan DiBello


The DIA notebook loads in the arrival and departure production models that have been trained on the dates specified by the top level notebook. 24 hours of arrivals and departures for the specified airport are then retrieved from the silver data store and passed to the model for delay estimation. The delay estimates and flight information are then stored in our gold data store. This gold data is then accessed and displayed in the DIA Dashboard. Any update to airport code or time entry on the dashboard will update the data stored in the gold data store and then be displayed on the DIA Dashboard.
