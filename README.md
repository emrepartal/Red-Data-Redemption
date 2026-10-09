# RED DATA REDEMPTION
### An Independent Quantitative Study of Player Behaviour in Red Dead Redemption 2

**Version:** 1.0  
**Status:** Active data collection  
**Research type:** Independent quantitative research  
**Focus:** Player behaviour, decision-making, gameplay preferences and data analysis

## About the Project

Red Data Redemption is an independent quantitative research project I developed to study the behaviour, preferences and decisions of Red Dead Redemption 2 players.

I want to understand not just how people play the game, but also how they make decisions in different situations, which parts of the game matter most to them, and how they engage with its world.

The study covers player backgrounds, gameplay habits, Honour-related choices, combat behaviour, engagement with the story, exploration, community preferences and Red Dead Online experiences.

To explore these topics, I put together a structured survey with 27 questions.

But I did not want to build just another questionnaire. I wanted the interface itself to feel connected to the subject of the research.

Instead of using a standard survey platform, I designed an interactive research journal inspired by the atmosphere of Red Dead Redemption 2.

A leather-bound book, aged paper, candlelight, mechanical sounds and small environmental details all became part of the experience.

The result is a system that gives players a themed way to take part while collecting structured data that I can analyse later.

## Research Objectives

The main aim of this study is to examine the preferences and behaviour of Red Dead Redemption 2 players using quantitative methods.

I am particularly interested in the relationships between the way people play and the decisions they tend to make.

For example, do players who care more about the story make different Honour-related choices? Is time spent in the game associated with combat style or exploration habits? Where do players show similar preferences for particular characters, locations or experiences?

These are some of the questions I want to investigate.

Using the collected data, I plan to describe overall player tendencies, compare different groups and examine possible relationships between variables.

The goal is not to label any particular way of playing as right or wrong. It is to understand how different experiences and preferences are distributed among players.

## Research Design

The study uses an online survey based on voluntary participation.

It contains 27 questions, organised into sections covering different aspects of the game.

The survey begins with basic player information and moves through gameplay preferences, Honour decisions, combat approaches, story experiences and community preferences.

Questions about Red Dead Online appear depending on a participant's earlier answers.

In some sections, the order of answer options is randomised. I included this to reduce the possible influence of where an option appears on the screen.

When responses are stored, the system uses the original answer values rather than their displayed positions.

I also considered data quality when designing the research, not just the collection process.

The system records session duration and uses repeat-participation checks and selected quality flags.

A flagged response is not automatically treated as invalid. I plan to review these records separately during analysis.

A fuller explanation of the research design, variables, data-quality approach and planned analyses will be available in **METHODOLOGY.md**.

## Honour Score System

The Honour system in Red Dead Redemption 2 is an important part of this research.

I therefore developed a separate Honour Score based on four questions in the survey.

Answers to specific situations receive predefined points, producing a score from 0 to 100.

The score is grouped into three Honour profiles:

| Honour Score | Profile |
|---|---|
| 0–39 | Low Honour |
| 40–60 | Neutral Honour |
| 61–100 | High Honour |

The score is calculated on the server, not in the participant's browser.

This keeps the calculation tied to one consistent set of rules.

The Honour Score is not a direct measurement of a player's in-game Honour level. It is a rule-based indicator I created from selected survey answers for this research.

After completing the survey, participants can see their Honour Score and profile.

This gives me an additional research variable and gives participants a result connected to their answers.

## Research ID System

A unique Research ID is generated for each successfully completed survey.

It is created on the server when the survey is completed and stored with the corresponding record.

The Research ID does not represent a participant's real-world identity. It is an identifier used to distinguish research records within the system.

Participants can see their Research ID on the results screen.

## Participant Feedback

I also added an optional feedback form at the end of the survey.

After completing the questions, participants can leave a short message about their experience, report a problem or share a suggestion.

Feedback is kept separate from the main survey answers. Messages are stored in a dedicated database table and linked to the relevant research session.

The system checks the message before saving it and allows one feedback submission per participant.

I added this because the structured questions cannot cover every part of someone's experience with the survey. Feedback gives participants a way to point out issues or suggest improvements that I might otherwise miss.

## Design and User Experience

I designed the visual interface for Red Data Redemption in Figma.

From the beginning, I wanted the research interface to feel at home in the world of Red Dead Redemption 2.

Instead of a conventional form layout, I used an old book opened on a wooden table.

The interface includes a leather-bound book, paper textures, candlelight, a pocket watch, a revolver-themed progress indicator, sound effects and other decorative elements.

The design uses a 1920 × 1080 reference resolution.

Screens are scaled around that reference layout to preserve the visual composition.

Early in development, I planned to add a realistic page-turning animation. But the pages had been prepared as flattened images, which made a physical page-turn effect difficult to implement.

Rather than rebuild the interface from scratch, I developed a different transition system.

The candlelight gradually fades, the screen goes dark, the page changes, and the new page is lit again.

This let me keep the existing design while creating a transition that suited the journal's atmosphere.

Details such as a pocket watch showing the actual system time, a calculated moon phase and moving environmental elements are also part of the interface.

None of these features is necessary for collecting survey responses. I added them because I wanted the experience to feel like more than a form with a themed background.

The current version of the survey is designed for desktop and laptop computers. Participation on phones and tablets is not supported.

The design process, implementation choices, animations, screen management and technical problems will be covered in detail in **FRONTEND_REPORT.md**.

## Technical Infrastructure

I built the frontend using HTML, CSS and JavaScript.

The interface handles screen navigation, answer validation, conditional questions, audio, animations and the preservation of a participant's session state.

For data collection, I use Supabase and PostgreSQL.

The frontend communicates with the server through Supabase Edge Functions to start research sessions and submit completed surveys.

Research-critical operations are handled on the server and in the database.

These include session creation, data validation, survey completion, Honour Score calculation, Research ID generation and data-quality checks.

I chose this approach because I did not want the integrity of the research data to depend entirely on what happens in the browser.

The database also uses constraints and access controls to help keep records consistent.

The current technical system has three main parts:

**Frontend:** The interface participants use to take part in the research.

**Edge Functions:** The layer that receives requests from the browser and carries out the necessary server-side operations.

**PostgreSQL:** The database where research records are stored, validated and, in some cases, calculated.

The backend development process will be described in **BACKEND_REPORT.md**, while **ARCHITECTURE.md** will explain how the system works as a whole.

## Data Quality and Participation Controls

Data quality matters in online research based on voluntary participation.

For that reason, I developed several checks to assess participation and completed responses.

The system uses a persistent participant token to identify browser sessions and check for repeated completed submissions from the same browser context.

It also applies network-level participation controls.

These checks are intended to limit unusually concentrated participation over short periods and help assess possible repeat participation.

Survey duration is calculated using server-side timestamps.

Responses completed unusually quickly, along with certain repeat-participation cases, can be flagged for data-quality review.

A flag does not automatically mean that a response is excluded from the study.

Different people may share the same network, and that needs to be taken into account.

I therefore plan to assess flagged records separately during analysis.

Sensitive implementation details of the security and abuse-prevention controls are not included in this public documentation repository.

## Data Privacy

Participation is voluntary, and the study is intended for adults.

The survey does not ask participants for their names, email addresses or other direct identifying details.

The main information collected consists of answers to the research questions.

Limited technical information is also processed for security, session management and data-quality checks.

This information is used to run the study and help protect the integrity of the data.

I plan to share the findings through aggregated statistics and analyses without identifying individual participants.

## Planned Data Analysis

The technical development of Red Data Redemption is complete, and the project is currently collecting data.

Once enough research data has been collected, I plan to move on to statistical analysis.

I will begin by examining the dataset's structure, missing values, response distributions and data-quality flags.

Depending on the research questions, I then plan to consider the following methods:

- Descriptive statistics and frequency distributions
- Comparisons between player groups
- Examination of relationships between variables
- Correlation analysis
- Regression models
- Player segmentation and clustering methods
- Data visualisation and reporting of research findings

The methods I ultimately use will depend on the structure of the collected data and the questions being investigated.

There is an important distinction here: these analyses have not yet been completed. They are part of my plan for the next stage of the research.

When the research reaches that stage, I aim to evaluate and share the findings using appropriate statistical methods.

## Current Project Status

The design, frontend development, backend infrastructure and production deployment of Red Data Redemption v1.0 are complete.

The survey is running, and data collection is ongoing.

The current version includes the 27-question survey, conditional question flow, Honour Score calculation, Research ID generation, session management, data validation and participation controls.

The next main stage is to collect and analyse the research data.

I therefore see the project not as a completed statistical study, but as an independent quantitative research project with a working data-collection system and an ongoing research process.

## Documentation

I created this GitHub repository to document the research methods, design process and technical architecture behind the project.

The repository contains the following documents:

| Document | Contents |
|---|---|
| `README.md` | Project overview, objectives and current status |
| `METHODOLOGY.md` | Research design, data collection approach and planned analyses |
| `docs/FRONTEND_REPORT.md` | Interface design, development process and technical decisions |
| `docs/BACKEND_REPORT.md` | Server infrastructure, database and data validation |
| `docs/ARCHITECTURE.md` | System components and how they work together |

This repository is not intended for sharing source code.

Frontend and backend source files, SQL functions and security-sensitive configuration details are not included.

The purpose of the documentation is to explain how I developed the project, which methods I used and why I made particular decisions.

## Independent Project Notice

Red Data Redemption is an independent research project that I developed myself.

It has no official connection to Red Dead Redemption 2, Rockstar Games or Take-Two Interactive.

The project is not sponsored or endorsed by those companies.

The thematic illustrations used in the research interface are not gameplay screenshots or assets extracted directly from the game.

