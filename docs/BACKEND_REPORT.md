# RED DATA REDEMPTION
## Backend Development Report

**Version:** 1.0  
**Status:** Development complete; data collection ongoing

## 1. Why I Built My Own Backend

When I started Red Data Redemption, I wanted more than a questionnaire. I needed a system where I could store participants' answers in an organised way, calculate results consistently and manage data collection around the needs of the research. A ready-made survey service would have been quicker to set up, but I wanted to handle research sessions, Honour Score, Research ID and repeat-participation controls within the same system.

That is why I separated the survey interface from the data-handling system. The screens participants see present the questions and collect their answers. The backend checks incoming information, carries out the necessary operations and decides how records are written to the database. This allowed me to enforce the main research rules outside the browser rather than relying on the form alone.

## 2. Why Supabase and PostgreSQL?

I chose Supabase for the backend and PostgreSQL as the database behind it. Supabase brought a managed PostgreSQL database, API access and server-side tools such as Edge Functions into one environment. For a small research project, using these tools together made development and maintenance easier to manage than setting up several separate services.

PostgreSQL suited the kind of structured data I wanted to collect. I could keep sessions and answers in an organised table, define data types, check required fields and prevent certain invalid operations at database level. Its transaction support also helped me manage operations that needed to succeed together.

Choosing Supabase did not mean security was taken care of automatically. I still had to design access rules, server-side checks and database permissions. The benefit for me was having the tools to do that in one place.

## 3. How I Organised the Database

I treated starting and completing the survey as two stages of the same research session. In PostgreSQL, each session begins as an incomplete record in the `survey_sessions` table. When the survey is successfully submitted, the answers, completion time and result information are added to that record. This keeps the start and completion connected.

There is a reason for this distinction: starting the research is not the same as finishing it. I wanted to be able to distinguish abandoned sessions, completed responses and test activity. Recording the survey version also makes it easier to understand which questionnaire structure was used if the questions or flow change later.

Database design was not just about adding columns. I had to think about where information belonged, how records related to one another and how to reduce the chance of incomplete or inconsistent data entering the system. My aim was to avoid some of the problems that could otherwise appear during cleaning and analysis.

## 4. How Answers Reach the Database

When someone begins the survey, the frontend sends a request to the backend. Edge Functions sit between the browser and the database. They handle starting and submitting the survey through separate endpoints, check the structure of incoming requests and call the relevant PostgreSQL functions. I kept operations that need consistent database handling, such as creating and completing sessions, in RPC functions.

The same separation applies when someone finishes the questionnaire. The browser sends the selected answers, but it does not decide on its own whether they become a valid research record. Server-side checks and database rules work together, and the information needed for the result screen is returned through that process.

I chose this approach because frontend code can be inspected and changed by the person using it. A field being required on screen does not guarantee that every request reaching the server will contain a valid value. I wanted the rules for accepting research records to be checked outside the browser too.

## 5. Why Security Matters for This Research

I did not see security only as a way to prevent unauthorised access to the database. Access control and protection against misuse matter, of course. But Red Data Redemption also has another concern: how reliable the collected answers will be for research.

Participation is voluntary and the survey is available online. People join from different devices and networks, and I cannot verify each participant in person. That meant I had to consider automated submissions, bursts of requests and the possibility of someone completing the survey repeatedly.

For example, thousands of records do not necessarily mean thousands of independent participants. Repeated answers could make certain preferences appear more common than they really are. That could affect the distribution of Honour Scores, relationships between answers and the conclusions drawn from the data.

My goal is not simply to increase the response count. I want a collection process where I can understand the limits of the data, review unusual activity and explain which records I use in the analysis. That is why these controls are also part of the research method.

## 6. Why Might Someone Take the Survey More Than Once?

At the end of Red Data Redemption, participants receive an Honour Score and a personal result. That makes the experience a little different from a form that only collects information. Some players may wonder what result they would get with different choices. Others might want to change their answers or show the experience to a friend.

So I do not assume that repeat participation always comes from bad intentions. Several genuine participants may also share a home or an internet connection. The question for the research is how these situations might affect the dataset. Answers submitted by one person to try different outcomes should not necessarily be treated in the same way as answers from independent participants.

For that reason, I approached repeat participation not only as something to prevent, but also as something to consider when assessing data quality.

## 7. How I Handle Repeat Participation and Unusual Requests

I use two different technical approaches to assess repeat participation. The first is a participant token created in the browser and retained across later visits. It helps the system check whether a completed response already exists for the same browser context. I do not treat it as a person's real identity or a permanent device identifier. Browser data can be cleared, and people can use different browsers.

The second approach works at network level. The server uses HMAC-SHA256 to derive an identifier from network information, using a secret key kept on the server. Instead of writing raw IP addresses to the research table, I store this derived identifier. It lets me assess network-related repetition without keeping raw IP addresses in the research records.

This does not make the network identifier completely anonymous in a technical sense. It is a pseudonymous identifier that can help link some records associated with the same network. And because different people may share an internet connection, I do not automatically treat responses from one network as coming from one person. A record marked for quality review is not automatically deleted or considered invalid for that reason alone.

There are also controls for bursts of requests and repeated participation over longer periods. I use PostgreSQL transactions and, where needed, concurrency locks to keep those checks consistent when requests arrive at the same time. The aim is to prevent simultaneous operations from making separate decisions that undermine the recording rules.

I explain the approach and why I use it here, but not the participation limits, time windows, evaluation thresholds or exact order of the checks. Those details are not necessary to understand the method and could make the live system easier to test for weaknesses.

## 8. Data Validation and Record Integrity

I did not leave validation to a single point. The frontend helps participants answer the questions correctly. The backend checks the structure of incoming requests and the required information. The database adds another layer of consistency through data types, required fields and other rules.

This matters especially when a request is incomplete or unexpected. I did not want to assume that a form working properly in the browser would always send proper data. An invalid record accepted without warning could cause problems that are harder to notice during analysis.

Keeping related operations consistent was just as important. I separated the responsibilities of the server and database to reduce the chance of sessions, answers and results ending up in conflicting states. Handling errors in a controlled way and returning appropriate responses to the browser are part of that process too.

## 9. Honour Score and Research ID

Honour Score is one of the results participants see at the end of the survey. I did not want to calculate it in the browser and accept that value as final. Instead, PostgreSQL applies the same scoring rules when the survey is completed. This means the result shown on screen and the result stored for research come from the same server-side calculation.

The algorithm uses four questions from the Honour section (Q10–Q13). Each answer option has a predefined point value, and the four scores add up to an Honour Score between 0 and 100. For example, choices that favour helping others or taking responsibility receive more points, while choices based on personal gain or harmful behaviour receive fewer. The total is then assigned to one of three profiles: Low, Neutral or High Honour.

This is not an exact copy of the game's own Honour algorithm. It is a scoring method I designed for the research theme to classify particular answers consistently. It is not meant to make a definitive judgement about a player's character. From a technical perspective, what matters is that the same scoring table is applied to every completed record and the database does not rely on a ready-made score sent by the browser.

Research ID is also created on the server when a survey is successfully completed. Simply starting the survey does not produce a result ID. If the same completed session is submitted again, the system returns the existing result rather than creating another one. This helps prevent one session from accidentally producing two different Research IDs.

Research ID does not reveal a participant's real identity; it identifies a completed research record. Duration is handled in a similar way: instead of trusting a time reported by the browser, the system uses the recorded start and completion times on the server and database side.

## 10. Feedback and Supporting Records

I do not collect survey answers and participant feedback for the same purpose. Answers form the basis of the quantitative analysis, while feedback helps me understand people's experience and any problems they encountered. I therefore handle feedback separately from the research answers.

This keeps the dataset more organised and allows each type of record to have checks suited to its purpose. Feedback submissions also need to have the expected structure, and unwanted repetition needs to be limited. But I do not treat feedback as having the same analytical role as survey responses.

## 11. Access Controls, Privacy and Layers of Protection

I did not rely on a single security measure. On the Supabase and PostgreSQL side, I used access permissions and Row Level Security (RLS) rules. I separated configuration that can be exposed to the browser from privileged access that must stay on the server. As a result, inspecting or modifying the frontend code does not by itself grant permission to create or change protected research records directly.

Edge Functions provide another layer where incoming requests are checked. reCAPTCHA helps assess automated or potentially abusive requests. Request validation, participation-frequency controls and keeping sensitive configuration on the server all support this approach. None of these tools removes every risk on its own.

I took a similar approach to privacy. I wanted to avoid storing information that was not needed for the checks I was carrying out. For network-level controls, I use technical identifiers derived on the server with HMAC-SHA256 instead of saving raw IP addresses in the research database. These identifiers are used to assess participation patterns and data quality, not to find out who participants are. This lets me apply the controls without collecting more personal information than I need for that purpose.

It is also important to distinguish the research database from the technical logs maintained by infrastructure providers. Not storing raw IP addresses in the research table does not mean that network service providers never process IP information in their operational systems. I would rather describe the measures I actually use than make absolute claims I cannot verify.

I did not add these measures because I considered Red Data Redemption an unusually high-risk project. I believe even a small research project should handle access permissions properly, treat data carefully and avoid unnecessary risks.

## 12. Why I Am Not Making All the Source Code Public

When sharing Red Data Redemption on GitHub, I want to show not only the finished interface but also the decisions I made while building it. Even so, I am not publishing all the backend code currently running in production. This is not about making the project seem bigger than it is or presenting it as a special security undertaking. I simply wanted to follow the basic development practices that should be considered when building an application that collects data online.

Keeping secret keys out of public source code, limiting database access, checking browser-submitted data on the server and avoiding unnecessary personal information are not measures unique to this research. I did not want to overlook those principles in my own project. Since data collection is still underway, I think it makes sense not to publish code that reveals the live system's exact request-acceptance rules or protection checks at this stage.

That does not mean I am hiding how the project works. This report explains the technologies I use, the data flow, the scoring approach and the reasons behind my decisions. I am only leaving out sensitive configuration and implementation details that could make the active system easier to misuse.

Once data collection is complete, I may reconsider sharing some of the source code or technical components. I would first need to check whether doing so is appropriate from a security and privacy perspective. For now, I am not making a firm promise to release them.

## 13. Testing and What I Learned During Development

While building the backend, I tested more than whether a successful submission worked. I checked different stages, including starting a session, submitting answers, completing the survey, producing results and handling unexpected requests. I also made sure the conditional question flow was reflected in the stored data. For example, when someone answers that they do not play Red Dead Online in Q25, Q26 and Q27 are also left empty by the server-side process.

Repeat-participation scenarios, HMAC network identifiers, requests arriving close together and concurrent operations were particularly important. Some problems do not appear during normal use but can emerge when several requests arrive at once. These tests helped me see why the rules needed to work consistently in the actual request flow, not just make sense on paper.

Development also made the division between frontend and backend responsibilities clearer. Something looking correct on screen does not prove it was stored correctly. So I tried to test the full path wherever possible: did an action started by the participant become the database record I expected, how did the system behave when something went wrong, and was the correct result returned?

## 14. When Data Collection Is Complete

I do not see the backend only as something that keeps the survey running. Once data collection ends, I will also need to assess which records are suitable for analysis, separate test records, review quality signals and export the data. Those are part of what I built the system to support.

Keeping the raw records and preparing separate datasets for analysis matters to me. I want to be able to make cleaning and review decisions without overwriting the original records. Repeat-participation signals will also need careful interpretation at that stage; a flag alone should not be treated as a final judgement.

## 15. Looking Back

At first, it was easy to think of the backend as the part that saves survey answers. As development continued, I realised the work was about more than storage. Keeping sessions consistent, handling invalid requests, considering the effects of repeat participation and managing access permissions all became connected concerns.

The process showed me the difference between a working form and a dependable data-collection system. That is what guided my backend decisions in Red Data Redemption: making the experience as smooth as possible for participants while keeping the records behind it organised, manageable and useful for analysis.

This report explains the approach I used and the reasons for my decisions. It does not publish the active system's security thresholds or production source code. When data collection is finished, I will separately consider the research results and any technical material that may be appropriate to share.

