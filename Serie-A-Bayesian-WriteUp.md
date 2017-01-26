

```python
# findatsciproj2017RM
Final Dat Sci 13 project 

The problem of modelling football data has become increasingly popular in the last few years and many different models have been proposed with the aim of estimating the characteristics that bring a team to lose or win a game, or to predict the score of a particular match. We propose a Bayesian hierarchical model to address both these aims and test its predic- tive strength on data about the Italian Serie A championship 1991-1992. To overcome the issue of overshrinkage produced by the Bayesian hierar- chical model, we specify a more complex mixture model that results in better fit to the observed data.


We ﬁrst consider the Italian Serie A for the season 1991-1992. The league is made by a total of T = 18 teams, playing each other twice in a season (one at home and one away). We indicate the number of goals scored by the home and by the away team in the g−th game of the season (g = 1,...,G = 306) as yg1 and yg2 respectively. The vector of observed counts y = (yg1,yg2) is modelled as independent Poisson:

                                          ygj | θgj ∼ Poisson(θgj),
where the parameters θ = (θg1,θg2) represent the scoring intensity in the g-th game for the team playing at home (j = 1) and away (j = 2), respectively. We model these parameters according to a formulation that has been used widely in the statistical literature (see Karlis & Ntzoufras 2003 and the reference therein), assuming a log-linear random eﬀect model:

                                logθg1 = home + atth(g) + defa(g) 
                                logθg2 = atta(g) + defh(g).

The parameter home represents the advantage for the team hosting the game and we assume that this eﬀect is constant for all the teams and throughout the season. In addition, the scoring intensity is determined jointly by the attack and defense ability of the two teams involved, represented by the parameters att and def , respectively. The nested indexes h(g),a(g) = 1,...,T identify the team that is playing at home (away) in the g-th game of the season. The data structure for the model is presented in Table 1 and consist of the name and code of the teams, and the number of goals scored for each game of the season. As is possible to see, the indexes h(g) and a(g) are uniquely associated with one of the 18 teams. For example, in Table 1 Sampdoria are always associated with the index 16, whether they play away, as for a(4), or at home, as for h(303).

g home team away team h(g) a(g) yg1 yg2 
1 Verona Roma 18 15 0 1 
2 Napoli Atalanta 13 2 1 0 
3 Lazio Parma 11 14 1 1 
4 Cagliari Sampdoria 4 16 3 2 
... ... ... ... ... ... ... 
303 Sampdoria Cremonese 16 5 2 2 
304 Roma Bari 15 3 2 0 
305 Inter Atalanta 9 2 0 0 
306 Torino Ascoli 17 1 5 2

Table 1: The data for the ‘Serie A’ 1991-1992

In line with the Bayesian approach, we have to specify some suitable prior distributions for all the random parameters in our model. The variable home is modelled as a ﬁxed eﬀect, assuming a standard ﬂat prior distribution (notice that we use here the typical notation to describe the Normal distribution in terms of the mean and the precision):
                                      
                                      home ∼ Normal(0,0.0001)

Conversely, for each t = 1,...,T, the team-speciﬁc eﬀects are modelled as exchangeable from a common distribution:

                                      attt ∼ Normal(µatt,τatt), deft ∼ Normal(µdef,τdef)

As suggested by various works, we need to impose some identiﬁability constraints on the team-speciﬁc parameters. In line with Karlis & Ntzoufras (2003), we use a sum-to-zero constraint, that is
T X t=1
attt = 0,
T X t=1
deft = 0.
However, we also assessed the performance of the model using a cornerconstraint instead, in which the team-speciﬁc eﬀect for only one team are set to 0, for instance att1 := 0 and def 1 := 0. Even if this latter method is slightly faster to run, the interpretation of these coeﬃcients is incremental with respect to the baseline identiﬁed by the team associated with an attacking and defending strength of 0 and therefore is less intuitive. Finally, the hyper-priors of the attack and defense eﬀects are modelled independently using again ﬂat prior distributions:
µatt ∼ Normal(0,0.0001), µdef ∼ Normal(0,0.0001), τatt ∼ Gamma(0.1,0.1), τdef ∼ Gamma(0.1,0.1).
A graphical representation of the model is depicted in Figure 1. The inherent hierarchical nature implies a form of correlation between the observable variables yg1 and yg2 by means of the unobservable hyper-parameters η = (µatt,µdef,τatt,τdef). In fact, the components of η represent a latent structure that we assume to be common for all the games played in a season and that determine the average scoring rate.

Each game contributes to the estimation of these parameters, which in turn generate the main effects that explain the variations in the parameters θ and therefore implying a form of correlation on the observed counts y.

3 Results
According to the Bayesian approach, the objective of our modelling is two-fold: first, we wish to estimate the value of the main effects that we used to explain the scoring rates. This task is accomplished by entering the evidence provided by the observed results (the vector y) and updating the prior distributions by means of the Bayes’s theorem using an MCMC-based procedure.

 teams            attack effect                          defense effect
______________________________________________________________________________________
             mean     2.5%     median    97.5%     mean     2.5%     median    97.5%
______________________________________________________________________________________
Ascoli       -0.2238  -0.5232  -0.2165   0.0595    0.4776   0.2344   0.4804    0.6987
Atalanta     -0.1288  -0.4050  -0.1232   0.1321    -0.0849  -0.3392  -0.0841   0.1743
Bari         -0.2199  -0.5098  -0.2213   0.0646    0.1719   -0.0823  0.1741    0.4168
Cagliari     -0.1468  -0.4246  -0.1453   0.1255    -0.0656  -0.3716  -0.0645   0.2109
Cremonese    -0.1468  -0.4915  -0.1983   0.0678    0.1915   0.0758   0.1894    0.4557
Fiorentina   -0.1974  -0.1397  0.1255    0.3451    0.0672   -0.1957  0.0656    0.3372
Foggia       0.1173   0.1077   0.3453    0.5811    0.3701   0.1207   0.3686    0.6186
Genoa        0.3464   -0.3108  -0.0464   0.2149    0.1700   -0.0811  0.1685    0.4382
Inter        -0.0435  -0.4963  -0.2046   0.0980    -0.2061  -0.5041  -0.2049   0.0576
Juventus     -0.2077  -0.1210  0.1205    0.3745    -0.3348  -0.6477  -0.3319   -0.0514
Lazio        0.1214   -0.1626  0.0826    0.3354    0.0722   -0.1991  0.0742    0.3145
Milan        0.5226   0.2765   0.5206    0.7466    -0.3349  -0.6788  -0.3300   -0.0280
Napoli       0.2982   0.0662   0.2956    0.5267    0.0668   -0.2125  0.0667    0.3283
Parma        -0.1208  -0.3975  -0.1200   0.1338    -0.2038  -0.5136  -0.2031   0.0859
Roma         -0.0224  -0.2999  -0.0182   0.2345    -0.1358  -0.4385  -0.1300   0.1253
Sampdoria    -0.0096  -0.2716  -0.0076   0.2436    -0.1333  -0.4484  -0.1317   0.1346
Torino       0.0824   -0.1821  0.0837    0.3408    -0.4141  -0.7886  -0.4043   -0.1181
Verona       -0.2532  -0.5601  -0.2459   0.0206    0.3259   0.1026   0.3254    0.5621

            mean     2.5%     median    97.5%
home        0.2124   0.1056   0.2128    0.3213
______________________________________________________________________________________

              Table 2: Estimation of the main effects for the loglinear model
              
Table 2 shows some summary statistics for the posterior distributions of the coefficients for the log-linear model describing the scoring intensity.

Similarly to what found in other works, the home effect is positive (the posterior mean and 95% CI are 0.2124 and [0.1056; 0.3213], respectively). AC Milan, the league winner in that year, have by far the highest propensity to score (as suggested by the posterior mean of 0.5226 for the effect att). The top three clubs (AC Milan, Juventus and Torino) performed better than any other club in terms of defence showing the lowest value for the parameter def , while Ascoli (who were actually relegated), Foggia and Verona showed the highest propensity to concede goals.

The second — and probably more interesting — objective of the model is that of prediction. We can use the results derived in the implied posterior distributions for the vector θ to predict a future occurrence of a similar (ex- changeable) game. In this case, we produced a vector of 1000 replications for the posterior predictive distribution of y that we used for the purpose of model checking.

Figure 2 shows the comparison between the observed results throughout the season (the black line) and the estimations provided by both the posterior pre- dictions from our model (in blue) and the bivariate Poisson model of Karlis & Ntzoufras (2003) (in red). As one can appreciate, for most of the teams (Atalanta, Foggia, Genoa, Inter, Juventus, AC Milan, Napoli, Parma, Roma, Sampdoria, Torino and Verona), the Bayesian hierarchical model seem to produce a better fit to the observed results. For a few teams, the red line is closer to the black one (Cagliari, Cremonese and, marginally, Lazio), while for Ascoli, Bari and Fiorentina the two estimations are equally poor (with the Bayesian hierarchical model generally overestimating the final result and the bivariate Poisson generally underestimating it). In general, it seems that our model per- forms better than the bivariate Poisson in terms of adapting to the observed dynamics throughout the season.


Figure 2: Posterior predictive validation of the model in comparison with Karlis and Ntzoufras (2003). Data are for the Italian Serie A 1991-92. For each team, the dark line represents the observed cumulative points through the season, while the blue and the red lines represent predictions for the Bayesian hierar- chical and the Bivariate Poisson model (Karlis & Ntzoufras 2003), respectively


4 Reducing overshrinkage caused by hierarchical model

One possible well known drawback of Bayesian hierarchical models is the phe- nomenon of overshrinkage, under which some of the extreme occurrences tend to be pulled towards the grand mean of the overall observations. In the ap- plication of a hierarchical model for the prediction of football results this can be particularly relevant, as presumably a few teams will have very good per- formances (and therefore compete for the final title or the top positions), while some other teams will have very poor performance (struggling for relegation).
The model of §2 assumes that all the attack and defense propensities be drawn by a common process, characterised by the common vector of hyper- parameters (μatt, τatt, μdef , τdef ); clearly, this might be not sufficient to capture the presence of different quality in the teams, therefore producing overshrinkage,with the effect of: a) penalising extremely good teams; and b) overestimate the performance of poor teams.

One possible way to avoid this problem is to introduce a more complicated structure for the parameters of the model, in order to allow for three different generating mechanism, one for the top teams, one for the mid-table teams, and one for the bottom-table teams. Also, in line with Berger (1984), shrinkage can be limited by modelling the attack and defense parameters using a non central t (nct) distribution on ν = 4 degrees of freedom instead of the normal of § 2.
Consequently, the model for the likelihood, and the prior specification for
the θgj and for the hyper-parameter home is unchanged, while the other hyper-
parameters are modelled as follows. First we define for each team t two latent
(unobservable) variables grpatt(t) and grpdef (t), which take on the values 1, 2 or
3 identifying the bottom-, mid- or top-table performances in terms of attack (de-
fense). These are given suitable categorical distributions, each depending on a
vectorofpriorprobabilitiesπatt =(πatt,πatt,πatt)andπdef =(πdef,πdef,πdef). 1t 2t 3t 1t 2t 3t
We specify minimally informative models for both πatt and πdef in terms of a Dirichlet distribution with parameters (1,1,1), but obvioulsy one can include (perhaps subjective) prior information on the vectors πatt and πdef to represent the prior chance that each team is in one of the three categories.
The attack and defense effects are then modelled for each team t as:

                         attt ∼nct μatt ,τatt ,ν , deft ∼nct μdef ,τdef ,ν 

In particular, since the values of grpatt(t) and grpdef(t) are unknown, this for- mulation essentially amounts to defining a mixture model on the attack and defense effects:

                          3                             3
                          
                  att =  ∑π att × nct (μ att,τ att,ν) , def = ∑π def × nct (μ def,τ def,ν) 
                         k=1                                  k=1
                         
                         
The location and scale of the nct distributions (as suggested, we use ν = 4) depend on the probability that each team actually belongs in any of the three categories of grpatt(t) and grpdef (t).
The model for the location and scale parameters of the nct distributions is specified as follows. If a team have poor performance, then they are likely to show low (negative) propensity to score, and high (positive) propensity to concede goals. This can be represented using suitable truncated Normal distri- butions, such as
                        μatt ∼ truncNormal(0, 0.001, −3, 0) 1
                        μdef ∼ truncNormal(0, 0.001, 0, 3)

For the top teams, we can immagine a symmetric situation, that is μatt ∼ truncNormal(0, 0.001, 0, 3)
                        μdef ∼ truncNormal(0, 0.001, −3, 0) 3
Finally, for the average teams we assume that the mean of the attack and defense effect have independent dispersed Normal distributions
                        μ att ∼ Normal(0, τ att ) μ def ∼ Normal(0, τ def )
                        
(that is, on average, the attack and defense effects are 0, but can take on both negative or positive values).
For all the groups k = 1, 2, 3, the precisions are modelled using independent minimally informative Gamma distributions
                        τatt ∼Gamma(0.01,0.01), τdef ∼Gamma(0.01,0.01)









```
