# RED DATA REDEMPTION
## Data Analysis Plan

**Version:** 1.0  
**Status:** Data collection ongoing; analysis not yet completed

## 1. What I Want to Learn

I started Red Data Redemption because I wanted to look beyond individual opinions about Red Dead Redemption 2 and examine patterns across players' experiences. The survey and its data collection system are the first part of that work. The next step is to prepare the responses, examine how players differ, and see which preferences tend to appear together.

This document sets out the questions I want to investigate, the methods I plan to use, and how I will approach the results. It is a plan, not a report of completed findings. I will update it as the analysis develops.

## 2. How I Chose the Survey Questions

I built the questionnaire around several parts of the player experience: time spent with the game, preferred activities, exploration, combat, story engagement, choices in different situations, the game world, community interests, and Red Dead Online.

I wanted questions that players could answer from their own experience, rather than questions that required them to remember precise gameplay statistics. I also needed responses that could be compared across participants. That is why most questions use structured answer options instead of open-ended responses.

The questions were not chosen to produce a single ranking of players. I wanted to examine several sides of the experience and then explore how they relate. For example, time spent playing may be associated with preferred activities, while story completion may be associated with how players engage with the world. These are questions to test, not conclusions I have already reached.

The survey records what participants say about their experience. It does not collect their actual gameplay logs, and I will keep that distinction in mind throughout the analysis.

## 3. Questions I Plan to Investigate

I will start by describing the participants and the distribution of their answers. After that, I want to focus on a smaller set of comparisons and relationships:

- **Experience and playstyle:** Do players in different playtime groups show different preferences for exploration, combat, or other activities?
- **Story progress and engagement:** Are players who have completed the story different from those who have not in the activities or aspects of the game they value?
- **Platform and preferences:** Are there noticeable differences in reported playstyle across gaming platforms, after taking other relevant characteristics into account where possible?
- **Choices and playstyle:** Do answers to the survey's decision-based questions vary with players' reported preferences or experience?
- **Community and online play:** How do community interests and Red Dead Online experiences vary among eligible respondents?

These questions provide a starting point. I will check the actual response counts before deciding which comparisons are practical. Some categories may need to be combined for a clear, defensible reason; others may have too few observations for a reliable comparison. I will document those decisions rather than quietly changing the questions to fit the data.

## 4. Preparing the Dataset

Survey responses are stored in PostgreSQL through Supabase. I plan to extract the fields needed for analysis with SQL, preserve the original records, and work from a separate dataset for cleaning and statistical work.

First, I will distinguish completed sessions from incomplete ones and remove test records from the main research sample. I will check response categories, unexpected values, missing fields, and consistency across related questions.

Some missing answers are expected. For example, participants who do not play Red Dead Online are not shown the follow-up questions. Those empty fields are part of the questionnaire design, not evidence that a participant skipped a question. I will identify them separately from other missing values.

I will also review the available participation-quality indicators, including repeat-participation flags and completion times. A flag will not automatically make a response invalid. Shared connections and different answering speeds have reasonable explanations, so any exclusions will need a clear basis. Where useful, I will repeat key analyses with and without questionable records to see whether the results change.

Before running tests, I will prepare a variable guide covering the meaning of each field, its answer categories, any recoding, and how missing values are handled. This will help keep the analysis consistent as the dataset grows.

## 5. Tools I Plan to Use

**SQL** will be used to retrieve and check the records held in PostgreSQL. **Python** will be my main environment for cleaning the data and carrying out statistical analyses. I expect to use **pandas** and **NumPy** for data preparation, **SciPy** for statistical tests, and **statsmodels** for regression models and related diagnostics. **Jupyter Notebook** will help me keep the steps and explanations together. If I explore player segmentation, **scikit-learn** is an option.

For presenting the findings, I plan to use **Power BI**. I want to build a report where readers can examine response distributions and compare player groups without having to work through raw tables. Python may still be useful for diagnostic plots during analysis, but Power BI is my planned tool for the final visual presentation.

This is the intended toolkit, not a list of software I have already used for completed analyses. I will only add tools when they serve a clear purpose.

## 6. The Analyses I Plan to Carry Out

### 6.1. Descriptive analysis

I will begin with frequency distributions, percentages, and cross-tabulations. These will show the makeup of the sample, which answers are common, and where categories have very few responses. For ordered responses, I may also report medians and other summaries that respect their ranking.

I will use these results to understand the dataset before choosing more demanding methods. They will also form the basis of the Power BI report.

### 6.2. Group comparisons and association tests

Next, I will compare response patterns across groups such as playtime, platform, and story completion. For categorical variables, I plan to use contingency tables and, where assumptions allow, **chi-square tests of independence**. If expected cell counts are too small, I will consider an appropriate exact test or a different way of presenting the comparison.

A p-value alone will not tell me how important a difference is. I also plan to report measures such as **Cramér's V** for categorical associations, alongside the actual percentages and sample sizes. For ordered variables, I will consider methods that use the order of the categories without assuming equal spacing between them.

### 6.3. Ordinal relationships

Several survey responses have a natural order, such as playtime groups. Where the question calls for it, I may use **Spearman's rank correlation** or suitable rank-based group comparisons. These methods can be more appropriate than treating ordered categories as precise numerical measurements.

I will choose them based on the variables involved, rather than applying the same test to every pair of questions.

### 6.4. Regression models

Some differences may reflect more than one player characteristic. For selected research questions, I plan to use regression to examine several variables together.

If the outcome has two categories, **binary logistic regression** may be appropriate. For outcomes with several unordered categories, I will consider **multinomial logistic regression**. If the outcome categories have a meaningful order, **ordinal logistic regression** may be a better fit, provided its assumptions are reasonable.

I will check category sizes, sparse combinations, model fit, and relevant assumptions before interpreting any model. Where possible, I will present results as **odds ratios with confidence intervals**, not just coefficients or significance levels. The goal is to understand associations after accounting for other measured characteristics, not to claim that one preference causes another.

### 6.5. Exploring player segments

If the data supports it, I may explore whether combinations of answers reveal groups of players with similar preferences. Because many survey variables are categorical, I will not assume that ordinary numerical clustering methods are automatically suitable. I will consider methods designed for categorical or mixed data, such as **k-modes or k-prototypes**, depending on the variables included.

Any segments would be exploratory. I would check whether they are stable and interpretable before presenting them, and I would not treat them as fixed types of RDR2 players.

### 6.6. Checks on the results

I expect to make many comparisons across the questionnaire, so I will distinguish the main research questions from exploratory findings. Where I run a large set of related statistical tests, I will consider a **multiple-testing adjustment**, such as controlling the false discovery rate, to reduce the risk of overinterpreting chance results.

I will also look at how sensitive important findings are to data-cleaning choices, small categories, and the inclusion of flagged records. If a result changes substantially under reasonable alternatives, that will be part of the report.

Not every method described here will necessarily be used. The final selection will depend on the number and quality of responses and on what each research question actually requires.

## 7. Presenting the Findings in Power BI

I want the final report to be useful both as a research summary and as a way to explore the data. My current plan is to organise it around a few clear views: the participant profile, gameplay preferences, comparisons between groups, and the main statistical findings.

Power BI will allow me to use filters for relevant player characteristics and show how response distributions change across groups. I will make the number of respondents visible wherever percentages are reported, especially when filters leave only a small group.

The dashboard will not replace the statistical analysis. Tests, models, assumptions, and limitations will still need written explanations. The visual report should make the findings easier to follow, not make them appear more certain than they are.

## 8. Interpreting the Results

I will look at the size and direction of relationships, not only whether a test returns a statistically significant result. I also want to report uncertainty and discuss findings that do not match my initial expectations.

Participation is voluntary and the sample is not randomly selected. Even a large dataset will not automatically represent all Red Dead Redemption 2 players. The answers are self-reported, and the study observes players at one point in time. For these reasons, I will describe patterns within the collected sample and avoid causal claims or unsupported generalisations.

The purpose is not to assign every player to a neat category. It is to see what the responses can tell us, and to be clear about what they cannot.

## 9. Documenting and Sharing the Work

I plan to document the cleaning steps, variable definitions, exclusions, statistical methods, and changes made along the way. Where appropriate, I will publish analysis notebooks or scripts in the GitHub repository so that the work behind the findings is visible.

I will not publish the raw participant database, sensitive identifiers, or production security details. Public findings will use aggregated results, with additional care for small groups that could create privacy risks. Any demonstration data will be clearly labelled so that it cannot be mistaken for real survey responses.

## 10. Current Status

The Red Data Redemption v1.0 survey and data collection system are running, and data collection is still underway. The analyses described in this document are planned work; no statistical findings are being reported here yet.

Once the available sample is suitable for analysis, I will prepare the dataset, review the first distributions, and begin with the priority research questions. As that work progresses, I will replace the plans in this document with the methods actually used and the results obtained.

For me, the value of this project lies in carrying the research through the whole process: from choosing questions and collecting responses to testing relationships, explaining results, and acknowledging their limits.

