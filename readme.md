Welcome to my Reach Mahjong Bot!

Reach Mahjong is a 4-player rummy-style tile game where players are all trying to complete their hands (to earn points), but also trying to avoid dealing into their opponents' hands (losing points.)

===
Edit (10/06/2017): Just a brief couple things:

1) This bot is not generalized to play on all screen resolutions yet.  That'll be done a bit later.  I'm more concerned just about getting it to work very well on my own setup.

2) Lots of ideas are not fully implemented yet.

3) Below, it says we have not integrated the offense, ML model, and defense, but I do have all three parts described below working together now partially.
===

The bot (although the defense portion is still unfinished) will be broken into the following parts:

1) /An offense algorithm/ that tries to reach a complete hand as fast as possible, while also taking at most a second or two to output a decision.  There are many algorithms in the code, but only one is being used right now.

2) /A machine learning model/ that predicts whether someone is close to a hand or not (so that the bot can know whether it should be playing defensive moves in the case that someone doesn't announce to us that they are about to win their hand. (This sort of announcement is actually a part of the game and it is called "reach" -- there is often incentive, despite it giving away information, to make this play.)

3) /A defensive algorithm/ that, when we think the opponent should be feared, outputs a defensive move, again in a short time.

Right now only the offense algorithm has been inserted into the bot that can currently play a full game on tenhou.net/0 against human players, but soon I will update it.  Here is the order that new things will happen:

-First I will finish the defensive portion, so that when people reach (tell us they are 1 tile away from completing their hand), we can be defensive adequately.

-Second, I will integrate all three of the components, Offensive, Predictive, and Defensive, so that it can be defensive in more situations.

-Third, I will refine the offense and defense algorithms to be more efficient and to output a list of suggestions, so that in the case there is useful overlap between the offense suggestions and defense suggestions, we can pick the aggressive move that is sufficiently safe.

-Fourth, I will revisit the predictive model, attempting to draw a better line in the sand.  Currently I can differentiate 12% better than the baseline of 66% whether someone is 2 away from a complete hand or closer, and 3 away from a complete hand or further.  What I'd like is to predict whether someone is 1 away from a hand, or further away.  To do this, I may need to add a feature to the bot where it can record how long someone is taking to make each of their moves.  I feel that this information is extremely valuable, as from my own experience, it seems that lower-rated players will take a long time when they are progressing toward their hand, and a generally shorter time when they are 1 tile away from a complete hand.  They will throw away their unwanted tile almost instantly.

-After this, I'm not sure!  That's a lot for now!

Thanks for reading!



============================

LATEST UPDATE: 09/18/2017:

New notebook mainAutomation.ipynb added.  It only works on 1366x768 for now, but this clicks around and plays a 4-player reach mahjong game on tenhou.net/0 in either mainLobby or against CPUs to debug and add stuff.  Right now it can play a whole game with decent offense, but no predictive model or defense has been incorporated yet.  Next steps are to make the defense algorithm so that all 3 important components (attack, predict if enemy is dangerous, defense), and have them all communicate so that a better final decision can be made.  It should be noted that in the case the enemy just reaches (revealing that he needs only one final tile) we will probably just abandon our hand if we're not close to our own.

tldr, lots of work to do, early phase.

==============
gameData folder removed, because it is too large.
goodPlz folder's t2.csv removed, because it is also too large.

Notes on contents:

mouseMovement.ipynb -- not important and not really done anything with this yet.  This is not data science and is just going to be about moving the mouse, clicking, etc, and should probably be a .py file sooner rather than later when I do get going with it (so that it can be imported easily.)

Parse complete mahjong game.ipynb -- This is how I get the data from the gameData folder which normally contains like 12,000 mahjong games played by someone who eventually cracked into the very very top rank you can get on tenhou.net.  It has a bug right now and a few rows sometimes get messed up in the dataset, but it is mostly good and in the original EDA I did not have this problem, and all the conclusions I arrived at were fine.  The problem has something to do with the fact that later games have different conventions associated with how the replay information is encoded. Hopefully later today I can identify what's messing up some of the rows.

shanten calc (and early EDA and models).ipynb -- lots of stuff in here, and honestly I may have messed it up because I was trying to fix that bug I'm talking about above. It might be because of the new replays and it might be because of a function in this file.  Not sure / have to fix.  That all said, where I'm currently at is I need to fix this bug, and then I'm going to train a model to predict what discard someone should make given their hand and other factors (for the aggressive portion of the bot.)  I also have a model in there that can predict (roughly 12% above baseline) whether someone is EITHER 2 away or 1 away from a complete hand.  This is /without/ that much tuning, and what I want to do is use a LogisticRegression or something like this because I need a predict proba in order to have the output of that portion of the bot not be entirely binary, so that in the future I might be able to say things like "Well, we kinda think that player might have a hand, but we're so close and ours is so good! Let's just go for it." versus "That player HAS a hand FOR SURE, BE CAREFUL."  And then we probably activate defense mode (not yet made.)   ---- Oh also there's functions in here for deterministic offense, for calculating how far away a known hand is from completion, for cleaning up the data, feature engineering, etc.

shantenOnly.ipynb -- this file has just the deterministic functions, a low-lag notebook where I can just think about writing deterministic functions should I need them while being able to quickly look at old ones, etc, idk.
