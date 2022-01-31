# Regression Analysis of Hell Let Loose Game Data

This project is based upon the data collected to create a "Hell Let Loose Commander Stats Dashboard," the details of which can be found [in this post](https://www.reddit.com/r/HellLetLoose/comments/s9s0te/i_created_a_dashboard_to_track_my_command_stats/?utm_source=share&utm_medium=web2x&context=3 "HLL Dashboard Reddit Post" ). 

![HLL Commander Dashboard](https://github.com/antonyebrown/HellLetLoose_Stats/blob/main/images/cc_viz1.jpg)

Here, we were asked to look at which factors are the most important when predicting the number of "victory points" (and therefore Win or Loss result) gained during a match of Hell Let Loose. The prevailing assumption is that the number of nodes built and squad leader quality have the greatest effect on match outcome. Using regression analysis on data that we've collected, we will attempt to see if that assumption holds up.  

You can find more information about Hell Let Loose [here](https://www.hellletloose.com "HLL Main Site" ).

## Methodology

We collected the following data for 60 matches of Hell Let Loose where the player "moonface" held the Commander role:

* Game_ID, Date, Map, Mode, Side, Win: Self-explanatory

* Takeover: Whether or not moonface took over for an absent commander spot partway through the game

* Nodes: How many sets of nodes the team had for the majority of the game (this is a bit subjective)

* Squad Leader Quality: A subjective rating on a scale of 1-3 of how well the squad leaders performed. We tried to stick with giving 1 point for communication, 1 point for setting up garrisons as needed/requested, and 1 point for following attack/defend instructions

* Points: How many points we held at the end of the match

With the above data collected, we ran an ordinary least squares regresssion on the factors generally believed to have the greatest effect on the victory point results for a match. Because we are working with a relatively small dataset (<100 points), and the emphasis is on model fit as opposed to prediction, we will not utilize a test/training split. Additionally, will not use the factor "Takeover" as this is exclusive to commanding stats, and we want to generally apply our data to all matches.

In order to generate our linear model, we will use Recursive Feature Elimination (RFE), which is essentially an automated backwards-elimination process.

## Results

The first round of regression with all probable factors included generated the following:

![Full OLS](https://github.com/antonyebrown/HellLetLoose_Stats/blob/main/images/full_ols.jpg)

![Full ANOVA](https://github.com/antonyebrown/HellLetLoose_Stats/blob/main/images/full_anova.jpg)

We will use a semi-conservative p-value cutoff of 0.1 for determining significance. Right from the start, we can see that Squad Leader Quality and Mode are clearly significant. We can also see that Nodes, Map played (as a whole), and faction side are prime candidates for elimination from our model.

At each stage of elimination, we checked each factor for multicolinearity using VIF (Variance Inflation Factor) as our metric.

After running the RFE, we were left with six factors: Squad Leader Quality, Map_Omaha Beach, Map_St Marie Du Mont, Map_Stalingrad, Mode_Offensive, and Side_Soviet.

The regression results with only these factors are here, with the top 3 highlighted:

![Narrow OLS](https://github.com/antonyebrown/HellLetLoose_Stats/blob/main/images/narrow_ols.jpg)

While the R-Squared value of the narrow model is slightly worse than the full effect model, it greatly clarifies the importance of our remaining factors.

The modeling software did not see Seasonality as an important factor in any of the groups. This could be contributed to two issues: the lack of length of our data, as well as confounding between war participation, COVID participation and summer participation.

### Conclusion

**When it comes to victory points, our data suggets that nodes don't matter.** We were able to eliminate this factor fairly early in our analysis. Interestingly, this analysis period covers the Update 11 change to the Commander's ability "Encourage." Even with this nerf, nodes still are not a significant factor in the outcome of the game. **The Node mechanics in Hell Let Loose need to be changed in order to make this a value-added game feature.**

Based upon the sample data, **the top three factors that effect a match's outcome are, in order of importance: Squad Leader Quality, Game Mode, and whether or not you're playing Omaha Beach.** These conclusions are not particularly groundbreaking to anyone who is a long term veteran of the game, but I hope that this analysis has shed some light on the commonly held conceptions that most players have.

In the future, we should look to somehow incorporate tank effectiveness as a separate category in our analysis, since there is a good chance that it effects game outcome. We should also strive to delineate between Offensive sides (Defense vs Offense). Did I miss anything else? Let me know what you think should be included!

*Note: All of these findings are simply results from my specific sample. They could easily change with more data.*

