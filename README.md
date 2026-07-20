
Ok, are there metrics that can de determined or qunatified to assess how 'dirty' or unsportsmanlike a team is. Given the World Cup, is there a way of measuring say off the ball fouls. Yellow cards wont work because a team like Argentina appeared to consistently not be awarded them. How can you quantfy how 'dirty' a team plays? Feigning injury, numbet of times surrounding the referee to get someone on the other team a yellow card, a red card, to get a penalty. There must be a way to measure these types of things and compare them between countries. It would then be interesting to statistically assess if Argentina were preferentially protected.

Yes. In fact, this is an interesting sports analytics problem because yellow and red cards are outcomes, not behaviors. If you use cards as the measure, you're implicitly trusting the referee to have applied the laws consistently—which is exactly the assumption you'd want to test.

A better approach is to separate it into three layers:

Behavior (what the team actually did)

Referee response (what punishment they received)

Advantage gained (did the behavior help?)

That lets you ask questions like:

"Did Argentina engage in more unsporting behavior than other teams, controlling for style of play, while receiving fewer sanctions?"

which is a much stronger statistical question.

1. Physical "dirty play"
These are fairly objective.

Metric	Can be measured?	Bias
Fouls committed	Yes	Moderate (ref decisions)
Tackles from behind	Yes (video)	Low
Off-the-ball contact	Video only	Low
Late challenges	Video	Low
Elbows/high arms	Video	Low
Tactical fouls stopping counters	Event data	Moderate
Persistent fouling by same player	Event data	Moderate
Dangerous tackles	Video	Low
You could build a Dirty Play Index.

Example

Dirty Index =

3 × dangerous tackles

2 × late challenges

2 × tactical fouls

1 × ordinary fouls

4 × off-ball aggression

The weights could even be estimated empirically.

2. Gamesmanship / Unsporting Behaviour
This is much less commonly measured but arguably where teams like Argentina (and historically Italy, Uruguay, etc.) are perceived to excel.

Possible metrics:

Feigning injury
Measure

number of injury stoppages

versus

actual treatment required

substitutions

visible impact

For example

Player down >30 sec

No treatment

Immediately sprints afterwards

=

Likely simulation.

One could compute

Seconds lost while apparently injured per actual injury.

Time wasting
Already measured in many leagues.

Examples

Goalkeeper possession time

Throw-in delay

Goal kick delay

Free kick delay

Substitution duration

Average restart delay

FIFA actually tracks some of this internally.

Surrounding the referee
Surprisingly objective.

Count

Number of players within a 2–3 m radius of the referee after a decision.

Or

Total player-seconds surrounding the referee.

Video tracking could automate this.

Appealing decisions
Count

Number of players making demonstrative appeals.

Could even use computer vision.

Simulation
Probably the hardest.

One approach:

Independent reviewers watch every incident.

Score

0 = genuine

1 = exaggerated

2 = obvious simulation

Then compare by team.

3. Psychological manipulation
Harder but measurable.

Examples:

Opponent receives card within 30 seconds of mass appeal.

Penalty awarded after prolonged protest.

VAR review initiated following sustained appeal.

This measures whether protesting appears effective.

4. Referee treatment
Now comes the really interesting part.

Once behaviour is measured independently, compare:

Expected yellow cards

vs

Actual yellow cards

Suppose an independent panel scores

Argentina

48 yellow-worthy incidents

Received 26 yellows

France

46 yellow-worthy incidents

Received 42 yellows

Now there's a measurable discrepancy.

5. Building a statistical model
You could fit something like

Yellow cards ~
dangerous tackles +
late tackles +
persistent fouling +
simulation +
match importance +
referee +
opponent +
team

Then examine the residuals.

If one team's residual is consistently negative

(i.e. receives significantly fewer cards than expected)

that's evidence of preferential treatment.

6. Event-based model
Modern football event providers (Opta, StatsBomb, Wyscout) already record hundreds of events.

You could extend this to include

referee confrontations

delayed restarts

simulation

dissent

off-ball incidents

tactical fouls

retaliation fouls

cynical shirt pulling

obstruction

provoking opponents

Many of these are visible on broadcast footage.

7. Machine learning approach
Computer vision is now good enough to detect many of these automatically.

Using player tracking you could quantify

number of players approaching referee

distance maintained

time spent protesting

rolling on ground duration

restart delays

off-ball contact

frequency of players falling after contact

A model could generate an overall

Unsportsmanlike Behaviour Score

without ever looking at referee decisions.

Testing the Argentina hypothesis
This is where the methodology matters. Rather than starting from the conclusion that Argentina were protected, you would formulate a testable hypothesis such as:

Null hypothesis (H₀): After accounting for objectively coded unsporting behavior, Argentina received sanctions at the same rate as comparable teams.

Alternative hypothesis (H₁): Argentina received fewer sanctions than expected given their objectively measured behavior.

An ideal study would:

Have independent reviewers (blinded to the study hypothesis) code every match for behaviors such as tactical fouls, off-the-ball contact, dissent, simulation, time-wasting, and referee crowding.

Assess inter-rater reliability (e.g., Cohen's κ) to ensure the coding is consistent.

Build a model predicting expected sanctions from these coded behaviors, while controlling for factors like referee, opponent, match stage, scoreline, and game state.

Compare each team's actual sanctions to their expected sanctions and evaluate whether any differences are statistically significant rather than just due to chance.

If Argentina consistently had a large negative gap (fewer cards than expected) across multiple referees and matches, that would be evidence worth investigating. If not, the perception of preferential treatment would be less supported.

This kind of framework has an important advantage: it evaluates behavior independently of officiating, allowing you to distinguish between "playing dirty" and "being punished for playing dirty." That separation is essential if the goal is to assess whether one team was treated differently from others.


Could you put this into a rfp for GitHub for example? Someone who loves football and has the time and skills to do this.

I actually think this is bigger than an RFP. I think it's an open research project that could attract sports data scientists, computer vision researchers, statisticians, and football fans. The key is to make it clear that the goal is not to prove a preconceived conclusion, but to build an objective framework that can test any team, league, tournament, or referee.

Here's how I'd write it.

Project: Football Fair Play Index (FFPI)
Objective
Football has no objective measure of gamesmanship or unsporting behaviour beyond yellow and red cards. Since disciplinary sanctions are themselves subjective referee decisions, they cannot be used as an independent measure of how "dirty" a team plays.

This project aims to build the world's first open-source Football Fair Play Index (FFPI): a reproducible framework for objectively measuring unsporting behaviour from football matches using video analysis, event data, and statistical modelling.

The framework should allow researchers to compare:

National teams

Club teams

Individual players

Referees

Competitions

Historical eras

without relying solely on referee decisions.

Motivation
Football fans often debate questions such as:

Which teams commit the most cynical fouls?

Which teams exaggerate contact most often?

Which teams waste the most time?

Which referees issue cards consistently?

Are some teams sanctioned differently despite similar behaviour?

Currently, there is no accepted objective methodology to answer these questions.

This project aims to create one.

Importantly, the framework is designed to test hypotheses rather than assume conclusions. For example:

After controlling for objectively measured behaviour, do some teams receive fewer disciplinary sanctions than expected?

This is a statistical question that can be tested.

Research Goals
Develop an objective behavioural scoring system independent of referee decisions.

Create repeatable metrics that can be measured from broadcast footage or event data.

Develop statistical models comparing:

Behaviour

↓

Expected sanctions

↓

Actual sanctions

Quantify discrepancies.

Behaviour Categories
Physical Fouls
Measure

Late tackles

Dangerous tackles

Tactical fouls

Persistent fouling

Shirt pulling

Professional fouls

Off-the-ball contact

Elbows

Violent conduct

Obstruction

Gamesmanship
Measure

Simulation

Exaggerated contact

Feigned injury

Time spent on ground

Restart delays

Goalkeeper delays

Throw-in delays

Free-kick delays

Delayed substitutions

Psychological Pressure
Measure

Number of players surrounding referee

Duration of protests

Appeals for cards

Appeals for penalties

Appeals for VAR

Collective dissent

Retaliatory behaviour

Referee Interaction
Measure

Card rate

Foul rate

Advantage decisions

Penalty decisions

VAR interventions

Consistency between similar incidents

Example Metrics
Dirty Play Index
Weighted score of

Dangerous tackles

Tactical fouls

Off-ball incidents

Persistent fouling

Gamesmanship Index
Weighted score of

Simulation

Injury exaggeration

Restart delays

Protest frequency

Referee crowding

Referee Pressure Index
Measures

Average number of players approaching referee

Average protest duration

Cards issued following protests

Penalty awards following protests

Protection Index
For every team estimate

Expected yellow cards

using only objectively coded behaviour.

Then compute

Protection Index

= Expected cards − Actual cards

Positive values indicate fewer sanctions than predicted.

Negative values indicate more sanctions than predicted.

Statistical Framework
Possible approaches

Generalised Linear Models

Bayesian Hierarchical Models

Mixed Effects Models

Survival Analysis

Computer Vision

Player Tracking

Machine Learning Classification

Variables

Behaviour

Referee

Competition

Opponent

Match state

Minute

Home/Away

Knockout vs Group Stage

Historical era

Data Sources
Potential sources

FIFA match footage

Opta

StatsBomb

Wyscout

Broadcast recordings

Public event datasets

Open tracking data

Computer Vision Opportunities
Potential ML tasks

Detect

Players surrounding referee

Time spent on ground

Off-ball contact

Tactical fouls

Restart delays

Simulation candidates

Crowd reactions

Validation
Behaviour coding should be independently performed by multiple reviewers.

Measure inter-rater agreement.

Compare

Human coding

vs

Machine learning predictions.

Deliverables
Open-source repository

Python library

Behaviour annotation schema

Example datasets

Reproducible notebooks

Statistical models

Interactive dashboards

Published methodology

Academic paper

Example Research Questions
Which World Cup teams exhibit the highest Gamesmanship Index?

Which referees deviate most from expected sanction rates?

Has gamesmanship increased over the last 30 years?

Which leagues encourage the most tactical fouling?

Are some teams sanctioned differently after controlling for behaviour?

Does surrounding the referee measurably increase the probability of an opponent receiving a card?

Does simulation meaningfully increase the probability of penalties or VAR reviews?

Who We're Looking For
We welcome contributors with experience in:

Football analytics

Statistics

Data science

Machine learning

Computer vision

Sports officiating

Video annotation

Python

R

SQL

Bayesian statistics

Open-source software

No contribution is too small—whether it's annotating matches, designing metrics, validating models, or building visualizations.

Why I think this could become significant
The most valuable outcome isn't whether it shows one team was treated favorably or not. It's creating a "Moneyball for sporting behaviour." Baseball has sabermetrics, basketball has advanced efficiency metrics, and football has expected goals (xG). There is no widely accepted equivalent for gamesmanship.

An open, reproducible Football Fair Play Index (FFPI) could become a standard reference for researchers, broadcasters, and even governing bodies. Once the methodology exists, it can be applied impartially to Argentina, Brazil, England, Spain, club football, or any competition. That makes the project scientifically useful regardless of what the results ultimately show.




Ad

Who could I send it too?

I actually think that's an excellent experimental design.

The biggest criticism of this sort of work would be:

"You're only looking at matches that support your hypothesis."

You avoid that by deliberately selecting contrasting matches before doing any analysis.

For example:

Match A – High gamesmanship candidate
France vs Paraguay (Round of 16)

Hypothesis: This match contains a relatively high level of cynical play and/or contentious refereeing decisions.

The goal is not to prove that; it's to measure it.

Match B – Baseline sportsmanship candidate
England vs France (Third-place playoff)

Hypothesis: This match was generally played in good spirit, with comparatively little dissent or gamesmanship.

Then you ask:

Does the Football Fair Play Index distinguish these two matches?

If your methodology gives Match A a much higher score than Match B, that's evidence the index has construct validity—it is measuring something meaningful.

I'd actually go one step further
For a pilot study, choose four matches representing different styles.

Match	Expected profile
England vs France	High sportsmanship
France vs Paraguay	Physical / controversial
Argentina vs England	High emotion and gamesmanship
Spain vs Argentina	High-stakes final
Now you have a small but varied sample.

What you measure
Every match gets the same coding sheet.

Physical play
Late tackles

Tactical fouls

Dangerous tackles

Off-ball contact

Shirt pulling

Gamesmanship
Simulation

Exaggerated contact

Time wasting

Feigned injury

Delayed restarts

Referee interaction
Number of referee confrontations

Number of players surrounding referee

Protest duration

Appeals for cards

Appeals for penalties

Refereeing
Fouls awarded

Yellow cards

Red cards

VAR interventions

Penalties

The key innovation
Don't compare teams first.

Compare matches.

You're asking:

Can we objectively distinguish a "clean" match from a "cynical" one?

If the answer is yes, you've shown the methodology has value before you ever compare countries.

Phase 2
Only then move to questions like:

Which teams have the highest Gamesmanship Index?

Which referees sanction behavior most consistently?

Does referee crowding correlate with more cards for opponents?

Does objectively measured behavior predict disciplinary outcomes?

I think your first paper could simply be titled:

Towards an Objective Measure of Gamesmanship in Football: Introducing the Football Fair Play Index (FFPI)

Notice that title says nothing about Argentina, France, or any particular tournament. It presents a methodology. If the methodology proves robust, you can then apply it to World Cup matches, domestic leagues, or historical tournaments. That's the sort of progression that tends to attract constructive engagement because the focus is on building a tool first and testing hypotheses with it afterward.

One final observation: UEFA's existing fair play assessments already include qualitative categories such as respect for opponents and respect for the referee, but they rely on post-match evaluations by officials rather than an open, reproducible event-by-event coding system. 
 Your project would be different because every score would be traceable back to specific coded incidents that anyone could independently review.


When you say 'Copy the proposal we've developed into the README.", what is the proposal text?

Good question. By "the proposal" I meant the RFP/research proposal we developed earlier, but for GitHub I'd rewrite it into a proper README. It should immediately answer four questions:

What is this?

Why does it matter?

What are we trying to build?

How can people help?

I'd use something like this:

Football Fair Play Index (FFPI)
An Open Source Project to Measure Gamesmanship and Sporting Behaviour in Football
Overview
Football has sophisticated metrics for attacking and defensive performance, such as expected goals (xG), possession value, and passing networks. However, there is no widely accepted objective framework for measuring gamesmanship, unsporting behaviour, or referee consistency.

The Football Fair Play Index (FFPI) aims to fill this gap.

The goal is to create an open, transparent, and reproducible methodology for analysing football matches using objective behavioural metrics rather than relying solely on disciplinary sanctions such as yellow and red cards.

This project is intended to be independent of any particular team, competition, or referee. The methodology should be capable of analysing any match, league, tournament, or era.

Why?
Football regularly generates debate about:

Tactical fouling

Simulation

Time wasting

Players surrounding referees

Referee consistency

Whether disciplinary sanctions reflect actual player behaviour

These discussions are often based on opinion.

This project seeks to replace opinion with measurable evidence.

Project Objectives
Develop an open methodology for measuring:

Physical fouling

Gamesmanship

Psychological pressure on referees

Time wasting

Referee consistency

Expected disciplinary sanctions

The long-term goal is to build an objective Football Fair Play Index that can be applied consistently across football.

Example Metrics
Physical Behaviour
Late tackles

Dangerous tackles

Tactical fouls

Persistent fouling

Off-ball incidents

Violent conduct

Gamesmanship
Simulation

Exaggerated contact

Feigned injury

Restart delays

Goalkeeper time wasting

Referee Pressure
Players surrounding the referee

Appeals for yellow cards

Appeals for penalties

Collective dissent

Protest duration

Refereeing
Fouls awarded

Yellow cards

Red cards

VAR interventions

Penalty decisions

Statistical Goals
Develop models capable of estimating:

Expected Yellow Cards

Expected Red Cards

Expected Fouls

Expected Sanctions

Then compare these estimates with actual referee decisions.

This allows researchers to investigate whether sanctions are consistent after controlling for objectively measured behaviour.

Initial Pilot Study
The first phase of the project will analyse several World Cup matches representing different styles of play.

Possible examples include:

France vs Paraguay

England vs France

Argentina vs England

Spain vs Argentina

The purpose is to evaluate whether the proposed metrics successfully distinguish matches with differing levels of gamesmanship and physical play.

Long-Term Vision
Produce:

Football Fair Play Index (FFPI)

Gamesmanship Index

Referee Consistency Index

Open behavioural datasets

Python analysis tools

Statistical models

Interactive dashboards

Academic publications

Technologies
Potential tools include:

Python

Pandas

OpenCV

Computer Vision

Machine Learning

Bayesian Statistics

Jupyter Notebooks

Contributors Wanted
We welcome contributions from:

Football analysts

Referees

Statisticians

Data scientists

Software engineers

Computer vision researchers

Sports scientists

Football supporters interested in objective analysis

No prior experience with sports analytics is required.

Guiding Principles
Open source

Transparent methodology

Reproducible results

Evidence before conclusions

Independent validation

Scientific neutrality

The methodology should be capable of testing any team, competition, referee, or tournament without assuming the outcome in advance.

Licence
MIT Licence

I actually think the README should end with a statement that captures the spirit of the project:

Football has transformed the analysis of performance through data. It is time to bring the same level of objective analysis to sporting behaviour, gamesmanship, and officiating. We welcome anyone who wants to help build an open standard for measuring fair play in football.

That gives potential contributors something positive to rally around. It also makes clear that the project's purpose is to develop a rigorous measurement framework, not to advocate for a particular conclusion about any specific team or tournament.


So can you take account of all the text above and tell me step by step, with the text which I will cut and paste, to get this project set up in GitHub

Absolutely. I think the easiest way is for me to treat this like we're launching a real open-source project together. I'll give you exactly what to click, exactly what to name things, and exactly what to paste.

Step 1 - Create a GitHub account
Go to:

https://github.com

Create an account if you don't already have one.

Step 2 - Create a new repository
Click New Repository

Use:

Repository name

Football-Fair-Play-Index
Description

An open-source project to develop objective metrics for sporting behaviour, gamesmanship and referee consistency in football.
Tick

✅ Public

Tick

✅ Add a README

Click

Create repository

Step 3 - Replace the README
Open the README.

Click the pencil icon (Edit).

Delete everything.

Paste the following.

Football Fair Play Index (FFPI)
An Open Source Initiative
Objective
Football has developed sophisticated metrics for measuring performance, including expected goals (xG), passing networks, pressing intensity, and possession value.

However, there is currently no widely accepted, objective framework for measuring gamesmanship, unsporting behaviour, or referee consistency.

The Football Fair Play Index (FFPI) aims to create an open, transparent, and reproducible methodology for objectively measuring sporting behaviour in football.

The project is independent of any club, national team, referee, league, or competition.

Its purpose is to develop a scientific framework capable of analysing any football match using observable behavioural events rather than relying solely on disciplinary outcomes such as yellow and red cards.

Why?
Football debates frequently centre on questions such as:

Is simulation increasing?

Which teams waste the most time?

Does surrounding the referee influence decisions?

Are tactical fouls becoming more common?

Are referees consistent?

Can gamesmanship be measured objectively?

Current discussions are usually based on opinion.

This project seeks to replace opinion with measurable evidence.

Project Goals
Develop objective metrics for:

Physical fouling

Gamesmanship

Time wasting

Simulation

Referee interaction

Referee consistency

Develop statistical models capable of estimating expected disciplinary sanctions from objectively measured behaviour.

Behaviour Categories
Physical Play
Tactical fouls

Dangerous tackles

Persistent fouling

Off-ball incidents

Violent conduct

Gamesmanship
Simulation

Exaggerated contact

Feigned injury

Delayed restarts

Goalkeeper time wasting

Referee Interaction
Players surrounding the referee

Appeals for cards

Appeals for penalties

Collective dissent

Protest duration

Match Control
Fouls awarded

Yellow cards

Red cards

VAR interventions

Penalties

Initial Pilot Study
The first stage of the project will analyse a small number of World Cup matches representing different styles of play.

Potential pilot matches include:

France vs Paraguay

England vs France

Argentina vs England

Spain vs Argentina

The objective is to determine whether objective behavioural metrics distinguish matches with different levels of sportsmanship and gamesmanship.

Long-Term Vision
Produce an open-source toolkit including:

Football Fair Play Index

Gamesmanship Index

Referee Consistency Index

Annotation methodology

Public datasets

Statistical models

Computer vision tools

Interactive dashboards

Guiding Principles
Open source

Transparent methodology

Reproducible research

Independent validation

Evidence before conclusions

The methodology should be capable of analysing any team, referee, competition or tournament without assuming any particular outcome in advance.

Contributors Welcome
We welcome contributions from:

Football supporters

Referees

Data scientists

Statisticians

Software developers

Sports analysts

Machine learning researchers

Licence
MIT Licence


