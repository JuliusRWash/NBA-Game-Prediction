# NBA-Game-Prediction
<!-- INTRODUCTION -->
## Introduction
I turned to my passion of NBA analytics to build a Recurrent Neural Network that could predict the outcomes of NBA games.
I was surprised by the results! It appears 538 has an accuracy of approximately 76% and my network currently performs just as well. I hope to improve on the model and methodology in the future.

## Data Aggregation and Preprocessing
I owe a huge thanks to this <a href="https://www.kaggle.com/datasets/nathanlauga/nba-games">dataset</a> publicly available on Kaggle. It contains NBA game and ranking data from 2014 onwards, which was the core of my dataset.

<!-- LOGIC-->
## Logic

When thinking about how an NBA team performs in a given game, there are a couple factors that I believe contribute the most:
<ul>
  <li>the team's current ranking</li>
  <li>how the team has performed recently</li>
  <li>the opponent's current ranking</li>
  <li>how the opponent has performed recently</li>
  <li>how the two teams fared against one another in the past</li>
</ul>
Maintaining historical data is performed using Long-Short Term Memory (LSTM) units in deep learning. For more information on LSTM's refer to this <a href="https://machinelearningmastery.com/gentle-introduction-long-short-term-memory-networks-experts/">link</a>.<br><br>

Hence, to predict the a game on a given day N between teams X and Y, I collected data about:
<ul>
<li>X's last 10 games</li>
<li>Y's last 10 games</li>
<li>X's rank on day N</li>
<li>Y's rank on day N</li>
<li>Last 3 matchups between X and Y</li>
</ul>
Game statistics included the following features:

  ```sh
  [WIN, IS_HOME, PTS, FG_PCT, FT_PCT, FG3_PCT, AST, REB]
  ```

Ranking statistics included the following features:
  ```sh
  [G, W_PCT, HOME_W_PCT, AWAY_W_PCT]
  ```
(Where G corresponds to game #)

Matchup statistics included the following features:
  ```sh
  [WIN, IS_HOME, PTS_home, FG_PCT_home, FT_PCT_home, FG3_PCT_home, AST_home, REB_home, PTS_away, FG_PCT_away, FT_PCT_away, FG3_PCT_away, AST_away, REB_away]
  ```

I also normalized these statistics between 0 and 1 to allow my model converge more quickly (if some values like points, assists, or rebounds are very different, the model could take longer).

My training data consisted of 22873 examples and validation data consisted of 2542 examples

<!-- MODEL DESIGN AND HYPERPARAMETERS -->
## Model Design and Hyperparameters

My model consisted of 3 LSTM's to maintain the history of each team AND to maintain the history of the matchups between the 2 teams.
I combined the outputs of these LSTMs with a simple linear layer.
Next, we also will make a sequential linear layer to handle the rankings. These linear layers had a cylindrical architecture. Within the linear layers I used the GELU activation function as I found it outperformed ReLU and TanH. I am yet to experiment with dropout within these layers to improve validation accuracy, but found my model was never overfitting.
Finally, we will combine everything through one fully connected layer.
The output of the fc layer will go into a sigmoid function since our output is binary.

<!-- HYPERPARAMETERS -->
## Hyperparameters

Loss function - since this was a binary classification problem I used BCELoss
Optimizer - I used Adam, but am yet to experiment much with alternatives (e.g. AdamW or SGD)
Schedulers - I'm yet to, but plan on adding schedulers to modify my static learning rate of 0.01
Regularization - I'm yet to encounter overfitting issues, so have not added any regularization techniques.

<!-- RESULTS -->
## Results

I trained my model for 100 epochs and achieved a training accuracy of 76% and validation accuracy of 78%. I plan on improving my model in the future and am open to suggestions on how to do so.
Conclusions

Deep learning methods have not been applied to NBA game prediction very much per my research, but they are a very valid way of predicting game outcomes. Particularly, using a RNN to combine the features of previous home games, previous away games, and the matchups between the two can be very useful in order to beyond simple rankings.

<!-- CONTACT -->
## Contact

Your Name - [@JuliusRWash](https://linkedin.com/in/j-r-washington) - juliusrwash@gmail.com
