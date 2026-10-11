# RED DATA REDEMPTION
## Frontend Development Report

**Version:** 1.0  
**Status:** Development complete; data collection ongoing

## 1. Where I Started

When I started building the interface for Red Data Redemption, I didn't want to use a ready-made survey template. I wanted the screens participants would see to fit the atmosphere of Red Dead Redemption 2, the game at the centre of the research. It was an appealing idea from a design perspective, but putting it into practice meant making more decisions than I would have with a standard web form.

I first designed the interface in Figma. Then I turned that design into a working survey using HTML, CSS and JavaScript. Rather than repeating the research questions or data collection methods here, this report focuses on how I moved from design to implementation, what I built and how I dealt with the problems along the way.

## 2. From Figma to a Web Interface

One of my first decisions was to preserve the visual layout of the screens. The open book, aged paper textures and objects on the table were all parts of the same composition. I prepared the screens around a 1920 × 1080 reference layout and exported the larger visual areas as flattened images.

The advantage was that the design I had made in Figma could look much the same in the browser. The downside was that the details inside those flattened images were not separate HTML elements. I had to place interactive parts, such as buttons, question fields and answer options, over the images.

I therefore kept the visual layer separate from the interactive layer. HTML defines the screen structure and usable areas, CSS handles their position and appearance, and JavaScript controls which screen is shown and how the interface responds to the participant.

## 3. Screen Layout and Scaling

The design was based on a fixed reference resolution, but not every desktop screen is the same size. Elements that looked correctly positioned at first could shift when the browser window changed. Keeping the answer areas aligned with the text and frames in the background was especially important.

To deal with this, I used a scaling system based on the reference composition. I wanted the book pages and the interactive areas on top of them to scale together, rather than having individual elements move independently. Rechecking the layout when screen or window dimensions changed was part of that approach.

I originally designed the interface with desktop and laptop computers in mind. Later, I adjusted the scaling and positioning of interactive elements so people could also take part on phones and tablets. I tried to make the survey usable across different screen sizes while keeping the original 1920 × 1080 composition. Larger screens still make it easier to see the design details.

## 4. Rethinking Page Transitions

At first, I wanted the book's pages to turn as if they were physical pages. But most of the design consisted of flattened screen images. A convincing page-turn effect would have needed more than rotating an image. The front and back of each page, the content visible during the movement and the interactive areas would all have needed to be prepared for it.

Adding that kind of animation later would have meant rebuilding important parts of the design. So I chose another approach. The candlelight fades, the screen goes dark, the active page changes while it is hidden, and the new screen gradually lights up.

This is not the same as turning a page. But it works better with the existing design and lets the transition feel like part of the journal's atmosphere. One thing I learned during development was that keeping an original idea at all costs is not always the best choice. Sometimes it makes more sense to find a solution that fits the system you already have.

## 5. Managing the Survey Flow

Under the visual design, there is a real form flow with multiple screens. After the cover and welcome screens, participants move through the questions. Some answers determine whether later questions appear.

In JavaScript, I had to manage screen changes, selected answers and movement between previous and next pages together. Preserving answers when someone went back was especially important. If a participant returned to a question, they shouldn't find an empty screen as though they had never answered it.

For some questions, the answer options can appear in a different order. The important technical distinction is that the displayed order is not the same as the value that gets recorded. Moving an option on the screen should not change its underlying meaning. I therefore kept the displayed order separate from the original answer definitions. The research reason for this decision is explained in METHODOLOGY.md.

## 6. Interactive Areas and Form Controls

Although the background images carry much of the design, the parts participants use still need to work as real HTML controls. I placed selection areas, navigation buttons and text inputs over the visual layer.

Making a button visible was only part of the job. Its clickable area had to match what people saw on screen. A selected answer needed to be clear, and people shouldn't accidentally click empty areas. I adjusted positioning with CSS and managed selection states and screen behaviour with JavaScript. I also paid attention to making answer areas usable by touch on phones and tablets and keeping them accessible on smaller screens.

Checking that required questions have been answered is another part of the experience. But I didn't treat browser-side checks as the only protection for the research data. The interface helps guide participants through the survey; validating research records on the server is the backend's responsibility.

## 7. Preserving the Session

During a longer survey, someone might refresh the page or lose their connection for a moment. I wanted the interface to be able to preserve their progress.

I used browser-side storage to save the session state and restore it when appropriate. This means reopening the page does not always force someone to start the survey again. It does not replace the research session on the server; it simply makes the browser experience more resilient to interruptions.

I used a similar approach for the optional feedback area at the end of the survey. A participant's draft message can be kept locally, and the interface shows its submission status. Browser-side checks help prevent repeated submissions, but the server makes the final decision about whether feedback is accepted.

## 8. Sound and Small Interactions

I wanted the interface to feel consistent not only in how it looked, but also in how it responded. I added different sounds for opening screens, selecting answers, moving between pages and reaching the results. Background music is part of the experience too.

Once the sounds were in place, the challenge was not making everything loud enough. It was making sure they didn't drown each other out. I adjusted selection and transition effects separately and kept the background music quieter. I also gave participants control over whether sound is on or off.

I had to account for browser restrictions on automatic audio playback. In cases where sound could not start before the first user interaction, I adjusted the flow accordingly. I also added controls that take reduced-motion preferences into account. These details did not change the main design, but they made a difference to how it works in use.

## 9. Environmental Details

Some of the objects around the book do more than serve as decoration. I used JavaScript to make the pocket watch show the system time and to calculate the moon phase. Moving environmental elements and lighting effects also help the screen feel less like a static image.

These details have no direct connection to survey answers. Still, I had to find a balance: decorative features should not make questions harder to read or answer. That is why keeping the interactive form areas separate from the visual and animated layers mattered to me.

## 10. Results Screen and Feedback

When the survey is completed, the interface moves away from the book pages to a different results screen. Participants see their Honour Score, corresponding profile and Research ID. I prepared separate visuals and sounds for the results.

The frontend does not generate these results. Its job is to display the completed result returned by the server. Neither the score calculation nor Research ID generation takes place in the browser. This keeps the visual presentation separate from calculations that matter to the research.

I also added the optional feedback area to this final part of the flow. Participants can write a short message, see its status while it is being sent and receive confirmation when it succeeds. The database and validation side of this feature will be covered in BACKEND_REPORT.md.

## 11. Testing and Final Adjustments

During development, I didn't only check whether the screens looked good. I tested moving back and forth, preserving answers, conditional questions, screen transitions, restoring sessions and the full results flow.

On the visual side, I repeatedly adjusted alignment at different desktop window sizes, clickable areas, transition timing and sound levels. I also checked screen scaling, touch selections and the main survey flow on phones and tablets. With an interface built around flattened design images, even small positioning errors can become noticeable in use.

These tests do not mean every possible device and browser combination works perfectly. But they were necessary to get the main participation flow working from start to finish and fix the problems I found before releasing v1.0.

## 12. Looking Back

For me, frontend development in this project was about more than recreating Figma screens with HTML and CSS. I learned to work within the limits of the design, let go of some ideas, keep interactive areas aligned with the visual layer and solve the smaller problems that matter when a research form needs to work without interruptions.

The frontend's job is to make it easy for participants to take part in the research. How data is accepted, validated and stored belongs to a separate layer. That is why the next report will focus on the server and database side, which I have only touched on where necessary here.

