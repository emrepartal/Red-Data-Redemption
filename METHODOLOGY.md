# RED DATA REDEMPTION
## Research Methodology

**Version:** 1.0  
**Status:** Data collection in progress

## 1. Research Objectives

I developed Red Data Redemption to study the habits, preferences, and decisions of Red Dead Redemption 2 players using quantitative methods. I want to understand not only which activities players prefer, but also how different gameplay experiences and ways of making decisions might relate to one another.

The study covers players' experience levels, playstyles, Honour-related decisions, approaches to combat, attitudes toward the story and game world, community preferences, and experiences with Red Dead Online. For example, I plan to examine whether time spent playing is associated with certain gameplay preferences, and whether different groups of players respond similarly to the Honour questions.

I'm not trying to present one way of playing as better than another. My aim is to examine how different preferences are distributed and what relationships may exist between them, based on participants' answers.

## 2. Research Approach

I designed the study as a voluntary, online, cross-sectional survey. Participants answer questions based on their own experiences with the game. The survey does not directly measure gameplay records; it collects players' reported experiences and preferences. That distinction will matter when interpreting the findings.

The survey contains 27 questions, organised into sections that cover different aspects of player behaviour rather than focusing on a single topic. Responses are collected through structured answer options so that variables can be compared during analysis.

This is not an experimental study. Participants are not randomly assigned to experimental groups, and I do not intervene in how they play. I therefore won't interpret any associations in the data as proof of cause and effect.

## 3. Survey Structure

The survey covers the following areas:

- **Player profile:** Age group, country, gaming platform, playtime, and story completion status.
- **Playstyle preferences:** How players spend their time in the game and which experiences they prefer.
- **Honour decisions:** Choices made in specific scenarios.
- **Combat and in-game decisions:** Approaches taken in different situations.
- **Story and game world:** Preferences related to the story, characters, atmosphere, and exploration.
- **Community and online experience:** Community-related preferences and, where applicable, questions about Red Dead Online.

These categories summarise the survey's main topics. The exact wording of each question and its answer options will also need to be considered when interpreting the variables.

### 3.1. Conditional Questions

Not every participant sees exactly the same questions. The final questions about Red Dead Online are shown according to an earlier answer. This prevents people who have not played the online mode from being asked questions that do not apply to them.

The database has separate fields for these conditional responses. When the relevant condition is not met, those fields remain empty. During analysis, I won't treat these entries in the same way as a question that a participant saw but left unanswered.

### 3.2. Answer Order

For some questions, the order of the answer options is randomised on screen. I introduced this to reduce possible order effects caused by presenting options in the same position to every participant.

Although the displayed order can change, the recorded values remain tied to the original answer definitions. This means that showing the same option in a different position does not create a different category in the dataset.

I don't assume that randomising answer order removes every form of response bias.

## 4. Reaching Participants and the Survey Experience

The study is open to Red Dead Redemption 2 players who choose to take part. I plan to reach participants through gaming communities and suitable channels for sharing the survey. This is not a probability sampling method, so I cannot assume that participants statistically represent the entire player base.

I put considerable effort into the frontend design for reasons beyond making the survey look different. I wanted to catch players' interest, help the research reach more people, and give participants a reason to complete the questionnaire. But increasing participation was never enough on its own. My goal was not simply to collect a large number of responses; it was to build a reliable dataset that I could actually analyse. That is why I included data quality checks alongside the work on the participant experience.

I designed the interface as an interactive journal inspired by the atmosphere of the game. I hope it makes the survey more appealing, but I have not yet measured whether the design affects participation rates or data quality.

In the current version, the survey is available on desktop and laptop computers. Participation on phones and tablets is not supported. I made this decision to preserve the visual layout and keep the interactive elements consistent within the existing design. I recognise that this may limit who can participate, and I will take that into account when interpreting the results.

## 5. Data Collection and Record Management

I built the data collection system using Supabase and PostgreSQL. When someone starts the survey, a session record is created on the server. It stores the start time and the information needed to manage the research session.

When the survey is completed, the answers are saved to that session. The completion time and total duration are calculated using server and database timestamps, rather than relying only on the participant's device clock.

Each successfully completed survey receives a unique Research ID. This identifier distinguishes a research record within the system; it does not represent the participant's real identity. Participants can see their Research ID on the results screen.

The browser also includes mechanisms to preserve the survey session. These help make the experience more resilient to interruptions. However, I don't rely on frontend checks alone: important validation and record-handling operations take place on the server and in the database.

## 6. Variables and Measurement

During analysis, responses will be treated as categorical variables or, where appropriate, ordinal variables. Age group, platform, playtime, story completion status, and answers to individual questions may all be useful for comparisons.

Before analysing the data, I will review the variables individually. In particular, I will consider whether categories have a meaningful order, whether some groups have very few responses, and how to handle empty fields created by conditional questions.

I won't assume that every question has the same measurement level or that the distance between answer categories is equal. The statistical methods will depend on the structure of each variable.

### 6.1. Honour Score

I calculate a rule-based score from four Honour questions (Q10–Q13). Each answer has a predefined point value, producing a total score between 0 and 100.

The score is divided into three profiles:

| Score | Profile |
|---|---|
| 0–39 | Low Honour |
| 40–60 | Neutral Honour |
| 61–100 | High Honour |

The calculation takes place in the database, so the same rules are applied regardless of what happens in the participant's browser.

The Honour Score is not a measurement of the game's actual Honour meter. It is an indicator created for this study from answers to four scenarios. I don't present it as a personality test, a moral judgement, or a validated psychometric scale.

I may examine the score's distribution and its possible relationships with other variables. When reporting findings, I will make clear how the score was constructed and which questions it uses.

## 7. Response Quality and Repeat Participation

In a voluntary online survey, the total number of responses is not enough to judge the quality of the dataset. Multiple submissions by the same person, unusually fast completions, or automated submissions can affect how the data should be interpreted.

For that reason, I added several checks to the collection process.

### 7.1. Session and Participant-Level Checks

The system links survey sessions to a participant token. It includes checks designed to prevent multiple completed responses from being created with the same token. Database-level concurrency controls also help prevent simultaneous requests from producing inconsistent records.

### 7.2. Network-Level Checks

I also use network-level checks for repeated or unusually frequent participation. Their purpose is to limit bursts of submissions over short periods and help assess the possibility of repeated participation over time.

I know that people sharing a network are not necessarily the same participant. Different players may use the same connection in a household, student accommodation, or another shared setting. A network-level flag therefore won't be treated as proof that a response is false or invalid.

I don't publish specific security thresholds or implementation details that could make these protections easier to bypass.

### 7.3. Completion Time

Total survey duration is calculated from the recorded start and completion times. Responses completed unusually quickly can be flagged for quality review.

A short completion time does not always mean someone answered carelessly. Equally, a long completion time is not proof of a high-quality response. I treat duration as one indicator to consider alongside other information.

### 7.4. Reviewing Flagged Records

Quality flags do not automatically delete or invalidate responses. I plan to retain the records during data collection and review them during the analysis stage.

Before analysis, I want to document which records are considered, why any exclusions are made, and how those decisions affect the results. Where appropriate, I may also compare analyses with and without flagged responses.

The aim is neither to keep as many records as possible nor to exclude as many as possible. It is to build a consistent dataset with decisions that can be explained.

## 8. Optional Participant Feedback

I also added an optional feedback field at the end of the survey, where participants can leave a short message.

This field is separate from the 27 structured research questions. Participants can share their thoughts on the survey experience, report a problem, or suggest an improvement.

Messages are stored in a separate database table and linked to the relevant research session. The system validates messages before saving them and limits repeated feedback from the same participant.

I don't treat these messages as the same type of data as the survey's quantitative answers. Their main purpose is to help me understand the participant experience and identify potential problems. If feedback leads to changes, I will need to consider those changes in relation to the survey version and the data collection process.

## 9. Data Preparation Plan

Once data collection is complete, I won't go straight into building statistical models. I plan to examine the dataset first and prepare it for analysis.

I will look at the following areas:

1. **Record integrity:** Separating completed and incomplete sessions and checking essential record fields.
2. **Missing values:** Distinguishing genuinely unanswered questions from fields left empty because of conditional survey logic.
3. **Category consistency:** Checking whether responses match the expected answer categories.
4. **Repeat participation and quality flags:** Reviewing session-, network-, and duration-based indicators together.
5. **Distributions:** Identifying categories with very few observations and unusual response patterns.
6. **Analysis dataset:** Documenting necessary transformations, recoding decisions, and any exclusions.

Database validation helps keep records consistent, but I don't assume it solves every measurement problem. The meaning of the research questions will also guide how I prepare the data.

## 10. Planned Statistical Analyses

At the time of writing, the study is still collecting responses. The methods below are options I plan to consider after data collection; they are not analyses I have already completed.

### 10.1. Descriptive Statistics

I plan to start by examining participant profiles and response distributions. Frequencies, percentages, and summary measures suited to each variable will help establish an overall picture of the dataset.

### 10.2. Group Comparisons

I may compare responses across groups defined by playtime, platform, story completion status, or other relevant characteristics. I will choose comparison methods based on the types of variables and the number of observations in each group.

### 10.3. Relationships Between Variables

I want to examine whether meaningful patterns exist between player preferences. Depending on the variables, I may use cross-tabulations, suitable measures of association, and correlation analysis where appropriate.

### 10.4. Regression Models

Some research questions may benefit from examining several variables together. In those cases, I plan to consider regression models suited to the type of outcome variable. I will pay attention to model assumptions, the sample structure, and the limits of interpretation.

### 10.5. Player Groups and Clustering

If the dataset is large enough and the variables are suitable, I may explore clustering methods to identify groups of players with similar response patterns. I won't treat any resulting clusters as fixed or naturally occurring player types. They will be analytical groupings shaped by the selected variables and methods.

### 10.6. Visualisation and Reporting

I plan to present the findings through tables and clear visualisations. Alongside statistical results, I will report the sample's characteristics, the methods used, data quality decisions, and the study's limitations.

## 11. Research Limitations

There are several limitations I want to acknowledge from the outset.

**Voluntary sample:** Participants choose whether to take part. I therefore cannot directly generalise the findings to all Red Dead Redemption 2 players.

**Self-reported data:** Answers reflect players' own accounts. Memory errors, personal interpretations, and response tendencies may affect the results.

**Cross-sectional design:** Data is collected over a defined period. Associations between variables do not, by themselves, establish causation.

**Themed interface:** The game-inspired design is intended to encourage participation, but it may appeal more to some groups of players than others. This could influence who takes part.

**Device restrictions:** The lack of phone and tablet support may exclude players who would otherwise participate using only a mobile device.

**Scope of the Honour Score:** The score is based on just four scenarios. It does not represent every decision a player makes or their actual Honour level in the game.

**Detecting repeat participation:** Technical checks can help reduce the risk of repeat submissions, but they cannot identify every case with certainty. Shared networks require particular care when interpreting flags.

I see these limitations not as a footnote to add at the end, but as factors that should influence the decisions I make during analysis.

## 12. Privacy and Use of Research Data

Participation is voluntary, and the study is intended for adults. The survey does not ask for direct identifying information such as names or email addresses.

The main research data consists of answers to the survey questions. Limited technical information is also processed for session management, security, and data quality. I plan to share findings through aggregated statistics and analyses that do not identify individual participants.

I don't publish actual participant records, the raw database, secret keys, or sensitive details of security controls in this GitHub repository. The repository is intended to document the research methods and development process.

## 13. Current Status and Next Steps

The research interface and data collection system for Red Data Redemption v1.0 are operational. The current version includes the 27-question survey, conditional question flow, server-side Honour Score calculation, Research ID generation, participant feedback, and data quality checks.

Data collection is ongoing. The statistical analyses and findings have not yet been completed.

My next step will be to assess whether the collected data is sufficient and suitable for analysis. I then plan to apply methods that fit the research questions and report the results together with their limitations.

For me, the success of this project won't be measured only by how many people complete the survey. What matters most is building a reliable dataset that can be analysed, with clear explanations of how it was collected and how decisions about the data were made.
