# Ion-Switching
## Abstract
Ion channels are pore-forming membrane proteins, present in animals and plants, that allow ions to pass through the channel pore. They encode learning and memory, help fight infections, enable pain signals, and stimulate muscle contraction. These ion channels can be recorded through patch clamp techniques and analyzed to infer certain properties of diseases.When Ion channels open they emit a signal with a certain amplitude.

The patch clamp technique is a laboratory technique in electrophysiology used to study ionic currents in individual isolated living cells, tissue sections, or patches of cell membrane.

 ## Problem Statement
Rapid automatic identification of the number of Ion channels open in a continous time series ion-signal data from the patch clamp that are processed through **Hidden Markov Process**.
 

# Data
Provided by the **The University of Liverpool’s Institute of Ageing and Chronic Disease.**
![alt text](https://encrypted-tbn0.gstatic.com/images?q=tbn%3AANd9GcRVMqVoqejHC87pK87eQ4TZa4yzej220r4wn9SmoX_PwGfli9mG&usqp=CAU)


# Preprocessing
This is a Hidden Markov Process because we don't know the initial probability of the open channels.
We can calculate the transition probabilties of the open channels and the emission probabilities of the signals and can analyse where our channels change with respect to the signals emitted. After some analysing its found that even the emiited channels is also an "Hidden" Hidden Markov model leading to a various number of hidden channels from the emitted ones.
Signals in the data is added with a certain amount of Gaussian Normal Distributions noise with a standard deviation of 0.001 - 0.005 .
It turns out that the signal has 50Hz noise because the AC power in United Kingdom was around 50Hz. So while experimenting the data these noise also got injected by the emitted ion currents. 
Data was proccessed with Kalman Filter that partially removes the 50Hz sinusoidal noise in the data.

# Evaluation
Here the evaluation method used was Macro-F1 Score to clearly know the sensitivity and specificity of the predictions

# Machine Learning Methods
## LightGBM
With a custom validation method trained the data with the preproccessed data and with feature engineering in a K-Fold  Regression like method (K=5)
![alt text](https://cdn.analyticsvidhya.com/wp-content/uploads/2017/06/11194227/depth.png)

## WaveNet Model (Deep Learning Method)
![alt text](https://lh3.googleusercontent.com/Zy5xK_i2F8sNH5tFtRa0SjbLp_CU7QwzS2iB5nf2ijIf_OYm-Q5D0SgoW9SmfbDF97tNEF7CmxaL-o6oLC8sGIrJ5HxWNk79dL1r7Rc=w1440)

Implemented the wavenet model in tensorflow and trained with signal only data in a K-Fold method.
Since this method extract various features through convolutions there is no need of feature engineering here.
This would take around 1 hour to finish training with Tesla P100 or Tesla P4 GPU.

# Ensembling method
Took the predictions from both the models and averaged them by rounding clipping the values

By doing from the above method we can get a cross-validation Macro-F1 Score of 0.941(+/- 0.0002). This shows that we can predict the number of ion channels open at a continious signal with 94.1% accuracy.
