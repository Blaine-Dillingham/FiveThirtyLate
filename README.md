# FiveThirtyLate

An avid reader of FiveThirtyEight, I sought to explore whether I could achieve comparable performance at predicting US elections by using machine learning to find previously undiscovered patterns, rather than manually weighting different factors in a probabilistic model as FiveThirtyEight does.

I trained several models to predict the percent of the vote won by the Republican [^1] in US Senate Elections given the ideology of each candidate, the partisan lean of the state they’re running in, the national partisan environment (think 2018’s “Blue Wave”), and national economic indicators. I assembled the training data from several different publicly available datasets of over 10 million entries. 

I trained decision trees, random forests, and neural networks on Senate elections in the years 2000-2010, and used the 2012 elections as a validation to experiment with the performance of different hyperparameters. I then built a test set of Senate elections in 2016-2020 to evaluate the final performance of these models. Using an ensemble of the best-performing random forest and neural network, I was able to predict unseen data points within 8%, on average, which is comparabe to the true margin of error on many polls.

<p align="center">
  <img width="700" src="https://user-images.githubusercontent.com/98286463/183139138-9c510cf5-3290-4477-be9e-b9bb5aa69abe.png">
</p>
                                                                                                                                
<p align="center">
  <img src="https://user-images.githubusercontent.com/98286463/183137918-4f5b7c14-2cb1-467b-97ab-9fe34c9d1c0f.png" width="700" />
  <img src="https://user-images.githubusercontent.com/98286463/183139690-76597e33-0199-4b29-81ae-4b84d69cec5e.png" width = "200" /> 
</p>                                                                                                                                

I hope to expand my model to US House elections before the midterms in November. I am also in the process of transitioning from using the fast.ai library to pure PyTorch to reduce the number of layers of abstraction in my code, as this will make for easier debugging. My next steps are to build a usable web application that constantly updates 

## Methodology

Ideology scores used are from Adam Bonica’s 2014 Paper “Mapping The Ideological Marketplace.” Rather than base ideology on votes once elected to Congress, which precludes having scores for any non-incumbents, Bonica’s “CF Score” is a metric based on the ideology of a candidate’s financial contributors. Using collaborative filtering, one can identify donors that tend to donate to candidates of certain ideologies, and infer the ideologies of candidates who receive money from the same donors.

In addition to the partisan lean of each state (how much more Republican does it vote than the nation as a whole in a weighted combination of the last two presidential elections), I included the states themselves as categorical variables in my models, after ensuring that they were neither polluting the analysis nor an insignificant factor. I hypothesize that this is because some states, particularly small ones, can be quite politically idiosyncratic.

When building my datasets, I exclude special elections, jungle primaries included in the source data, and Democrat vs Democrat or Republican vs Republican elections included in the source data that are the result of jungle primaries earlier in that year.

[^1]: I use two-party vote share, in which I divide the votes for the Republican by (votes for Repub + votes for Democrat)
