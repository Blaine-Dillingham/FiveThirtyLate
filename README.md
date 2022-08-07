# FiveThirtyLate

An avid reader of the elections forecasting website FiveThirtyEight, I sought to explore whether I could achieve comparable performance at predicting US elections by using machine learning to find previously undiscovered patterns, rather than manually weighting different factors in a probabilistic model as FiveThirtyEight does.

I trained several models to predict the percent of the vote won by the Republican [^1] in US Senate Elections given the ideology of each candidate, the partisan lean of the state they’re running in, the national partisan environment (think 2018’s “Blue Wave”), and national economic indicators. I assembled the training data from several different publicly available datasets of over 10 million entries. 

<p align="center">
  <img width="700" src="https://user-images.githubusercontent.com/98286463/183139138-9c510cf5-3290-4477-be9e-b9bb5aa69abe.png">
</p>

I trained decision trees, random forests, and neural networks on Senate elections in the years 2000-2010, and used the 2012 elections as a validation to experiment with the performance of different hyperparameters. I then built a test set of Senate elections in 2016-2020 to evaluate the final performance of these models. Using an ensemble of the best-performing random forest and neural network, I was able to predict unseen data points within 8%, on average, which is comparabe to the true margin of error on many polls.
                                                                                                                                
<p align="center">
  <img src="https://user-images.githubusercontent.com/98286463/183137918-4f5b7c14-2cb1-467b-97ab-9fe34c9d1c0f.png" width="700" />
  <img src="https://user-images.githubusercontent.com/98286463/183139690-76597e33-0199-4b29-81ae-4b84d69cec5e.png" width = "200" /> 
</p>                                                                                                                                

I hope to expand my model to US House elections before the midterms in November. I am also in the process of transitioning from using the fast.ai library to pure PyTorch to reduce the number of layers of abstraction in my code, as this will make for easier debugging. My next step is to build a usable web application that  updates live with new polls released.

## Findings

The partisan lean [^2] of the state in which each election took place appears to be by far the most important factor in my model's predictions, with the ideologies of each candidate coming in a distant 2nd and 3rd place. 

<p align="center">
  <img width="700" src="https://user-images.githubusercontent.com/98286463/183303381-67197ace-3ec7-4448-977c-3c4b11d477ec.png">
</p>

Worth noting about ideology is the immense shift in the distribution of candidate CF Scores between training, validation, and test sets. The graphs below are, from left to right, the CF score of Democratic candidates in the training, validation, and test sets. This corresponds to the year ranges 2000-2010, 2012, and 2016-2020, respectively[^3].

<p align="center">
  <img src="https://user-images.githubusercontent.com/98286463/183304232-9ba05bd6-0f47-4383-b82f-95aa5d0a7909.png" width="300" />
  <img src="https://user-images.githubusercontent.com/98286463/183304269-45c59e94-8fb7-4489-9df5-0b6627f908b5.png" width = "300" />
  <img src="https://user-images.githubusercontent.com/98286463/183304284-7d9185e4-3d51-4ee4-867b-6d493372a824.png" width = "300" /> 
</p>

And the Republican ideology distribution:

<p align="center">
  <img src="https://user-images.githubusercontent.com/98286463/183304357-d59bd55f-30bc-4377-a9ea-7dd42ca33134.png" width="300" />
  <img src="https://user-images.githubusercontent.com/98286463/183304366-1308333a-28dd-4408-b21c-46ccaf77cc83.png" width = "300" />
  <img src="https://user-images.githubusercontent.com/98286463/183304382-7d6f718b-549b-4f86-836c-b006a88167b0.png" width = "300" /> 
</p>

This distributional shift both explains some of the challenge my model has in generalizing as well as speaks to its strong ability to find fundamental relationships between candidate ideology and performance.

## Methodology Notes

Ideology scores used are from Adam Bonica’s 2014 Paper “Mapping The Ideological Marketplace.” Rather than base ideology on votes once elected to Congress, which precludes having scores for any non-incumbents, Bonica’s “CF Score” is a metric based on the ideology of a candidate’s financial contributors. Using collaborative filtering, one can identify donors that tend to donate to candidates of certain ideologies, and infer the ideologies of candidates who receive money from the same donors.

In addition to the partisan lean [^2] of each state, I included the states themselves as categorical variables in my models, after ensuring that they were neither polluting the analysis nor an insignificant factor. I hypothesize that this is because some states, particularly small ones, can be quite politically idiosyncratic.

When building my datasets, I exclude special elections, jungle primaries included in the source data, and Democrat vs Democrat or Republican vs Republican elections included in the source data that are the result of jungle primaries earlier in that year.

[^1]: I use two-party vote share, in which I divide the votes for the Republican by (votes for Repub + votes for Democrat)
[^2]: how much more Republican does the state vote than the nation as a whole, in a weighted combination of the last two presidential elections
[^3]: the 3rd graph for each party graph exaggerates the number of candidates having the mode of ideologies because missing values are filled by using the mean ideology of that party in the 2016-2020 period
