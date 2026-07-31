# StudyFlow — Functional Vision and Business Rules

*Product idea, audience, features, flows and domain decisions.*

| **OBJECTIVE** | Guide the construction of a demonstrable product, used as a learning laboratory and professional portfolio. |
|--------------|----------------------------------------------------------------------------------------------------------------|

# 1. Product vision

StudyFlow is a study platform that combines content organization, flashcards, study sessions, and spaced review. The system records the user's performance and calculates when each content should be reviewed again, making the study continuous, measurable and adapted to the individual's history.

> Central principle
> The product is not just a collection of notes or cards. Each response must generate consequences: new scheduling, progress updates, history, goals and indicators.

# 2. Product objectives
- Allow the user to organize materials, topics, notes and review materials in a single environment.
- Create a daily review routine based on deadlines and performance.
- Show progress, difficulties and study consistency through history and indicators.
- Allow future evolution for mobile application, collaboration, offline use and shared content.
- Serve as a real product and, at the same time, as a context for technical learning.

# 3. Initial target audience

| **Profile** | **Main need** | **Probable use** |
|------------------------------|-----------------------------------------------|-----------------------------------------------------------------|
| Self-taught student | Organize content and maintain consistency | Technical subjects, languages, certifications and competitions |
| Developer in learning | Review concepts and prepare interviews | Programming, Architecture, and Problem Solving Flashcards |
| University | Consolidate subject content | Notes, questions, schedules and tests |
| Professional in certification | Monitor mastery of a syllabus | Goals, reviews and simulations |
| Study group — future | Share materials and track progress | Collaborative workspaces and learning paths |

# 4. Domain structure

| **Element** | **Product responsibility** |
|------------------|------------------------------------------------------------------------------------------------|
| User | It has identity, preferences, sessions, content and study history.                     |
| Workspace | Limit of organization and ownership of data. Initially personal; future collaborative. |
| Subject | It groups a broad area of ​​study, such as C#, English or Mathematics.                                |
| Topic | It divides a subject into smaller subjects and can form a hierarchy.                           |
| Note | Records textual content, links, examples and references.                                      |
| Deck | Groups flashcards with a review purpose.                                                  |
| Flashcard | Question and answer unit used in review.                                               |
| Study session | Represents an activity with a beginning, end, scope and summary.                                     |
| Review | Records an attempt made on a flashcard.                                               |
| Scheduling | Defines when the card should reappear and what learning state it is in.           |
| Goal | Represents a daily, weekly or subject objective.                                         |

# 5. Core User Journey

1. The user creates the account and configures basic study preferences.

2. Create a story and organize topics or decks.

3. Add notes and create flashcards manually.

4. The system assembles the daily queue with new cards and expired revisions.

5. The user starts a session and responds to the cards.

6. For each card, inform the result: I made a mistake, difficult, good or easy.

7. The system records the attempt and calculates the next review.

8. Upon completing the session, the user sees a summary.

9. Progress indicators, calendar and goals are updated.

# 6. Features by area

## 6.1 Account and profile
- Registration, login, email confirmation and access recovery.
- Language preferences, time zone, start of day and daily review limit.
- View and close active sessions or devices.
- Account export and deletion.

## 6.2 Subjects, topics and organization
- Create, edit, archive and restore materials.
- Organize topics in a tree and apply tags.
- Set priority, color, icon and weekly goal.
- Move content between subjects and topics.

## 6.3 Notes and materials
- Create notes in Markdown with code blocks and links.
- Save draft, maintain version history and restore a previous version.
- Create flashcards from selected excerpts.
- Attach images or files in a future evolution.

## 6.4 Flashcards
- Create basic question and answer cards.
- Add future types: blanks, multiple choice, typed answer and code.
- Archive, suspend or reset a card's progress.
- Duplicate, move and tag cards.
- Import and export decks.

## 6.5 Reviews and sessions
- Display daily queue composed of new, expired and priority cards.
- Allow sessions by subject, deck, duration or number of cards.
- Record response time and user evaluation.
- Pause, resume, cancel and end a session.
- Present a summary with successes, difficulties, time and upcoming revisions.

## 6.6 Goals and monitoring
- Create goals for minutes, cards or study days.
- Monitor sequence of active days.
- Display activity calendar, progress by subject and success rate.
- Identify the most forgotten cards and most difficult topics.
- Show expected load of revisions for the next few days.

## 6.7 Notifications
- Remember pending revisions and upcoming goals.
- Warn when the sequence is at risk.
- Send weekly summary and achievement notifications.
- Allow preferences by channel and time.

## 6.8 Collaboration — future evolution
- Create shared workspaces.
- Invite members with roles of owner, editor, teacher, student or reader.
- Share decks and materials.
- Allow comments, suggestions and copies of public content.

# 7. Essential business rules

## 7.1 Ownership and access
- All content must belong to a workspace.
- A user can only access a workspace of which they are a member.
- The active workspace should never be accepted just because it was sent by the client; the association must be validated.
- Archived content does not participate in new sessions, but remains available in the history.
- Important deletions should prefer archiving or recycle bin to avoid accidental loss.

## 7.2 Flashcards and Scheduling
- An active flashcard must have enough content to be presented and evaluated.
- A new card only enters the queue if it respects the configured daily limit.
- Each response generates an immutable review in history.
- The next revision is calculated from the result, previous state and algorithm version.
- Changing the text on a card should not automatically erase its history.
- Resetting progress must be an explicit and auditable action.
- Suspended cards do not enter the queue until they are reactivated.

## 7.3 Study sessions
- A session can be created, in progress, paused, completed, canceled or expired.
- A completed session cannot return to the ongoing state.
- A response sent more than once for the same operation must not generate two revisions.
- The session summary must only reflect actually recorded responses.
- The user can end the session before the planned total, maintaining the progress already made.

## 7.4 Dates, time zone and sequence
- The definition of “study day” must consider the time zone and user preference.
- The sequence must use a clear rule about what counts as valid activity.
- Offline activities synchronized later must retain the actual moment in which they occurred.
- Time zone changes must not silently modify the already consolidated history.
- Daily and weekly goals must state when they start and end.

## 7.5 History and integrity
- Completed revisions should not be overwritten; corrections need to generate a new record or adjustment action.
- Relevant changes to notes, cards and goals must maintain authorship, date and version.
- Calculated results can be reconstructed from activity logs whenever possible.
- Duplicate operations coming from unstable connection should be safely recognized and ignored.

# 8. Review algorithm: functional evolution

| **Step** | **Behavior** | **Product Learning Objective** |
|---------------------|----------------------------------------------------------------------|--------------------------------------------------------------|
| Initial version | Fixed intervals according to user assessment.                    | Validate the review experience without excessive complexity. |
| Progressive version | New range depends on previous range and recent history. | Make behavior more adaptive.                      |
| Known algorithm | Adopt SM-2 or another documented reference.                         | Compare results with an established approach.           |
| Adaptive version | Consider response time, lapses, confidence and individual standards. | Experiment with personalization and data analysis.              |
| Experiments | Allow groups using different versions.                           | Assess retention, load and satisfaction.                        |

> Evolution rule
> Each schedule must record which version of the algorithm was used. This allows you to explain old results and test new strategies without silently changing history.

# 9. Indicators and reports

| **Indicator** | **What answers** |
|-------------------------|------------------------------------------------------|
| Pending reviews | What needs to be studied now?                    |
| Hit rate | How did you perform in a period or subject?      |
| Retention after break | Does the knowledge remain after days or weeks?       |
| Cards with the most lapses | What content is repeatedly forgotten?        |
| Average response time | Where is there slowness or insecurity?                 |
| Future load | How many reviews should occur in the next few days?    |
| Progress by topic | Which areas are new, learning or mature? |
| Consistency | How often does the user maintain the routine?        |
| Confidence versus success | Does the user know or did they just get it right by chance?          |

# 10. Differentiating features
- Interview mode, with technical questions, timer, written response and self-assessment.
- Code flashcards with syntax highlighting and response analysis.
- Learning diary that turns reflections into new cards.
- Confidence-based review, separating “I got it right” from “I got it right out of doubt”.
- Study plan by objective and deadline.
- Focus mode with timer and session summary.
- Forecast of review overload and suggestion for adjusting the daily limit.
- Import of decks from known formats and export to open formats.
- Assisted generation of cards from notes, always requiring human review.

# 11. Recommended Scope of First Usable Product

10. Registration, login and basic preferences.

11. Materials and decks.

12. Basic Question and Answer Flashcard.

13. Queue of new cards and revisions of the day.

14. Assessment at four levels.

15. Initial interval algorithm.

16. History per card.

17. Study session and summary.

18. Simple dashboard with activity and pending issues.

> First version success criteria
> A user must be able to register content today, review it in the following days and clearly realize that the system is adapting the agenda to their performance.

# 12. Items consciously outside the initial scope
- Real-time collaboration.
- Complex note editor similar to full productivity tools.
- Artificial intelligence as a central requirement.
- Execution of code sent by the user.
- Extensive gamification and global rankings.
- Decking marketplace.
- Mobile application before there was a stable web flow.
- Multiple advanced flashcard types on first delivery.

# 13. Product evolution vision

| **Moment** | **Functional focus** |
|----------------|--------------------------------------------------------------|
| Core | Organization, cards, reviews, sessions and history.         |
| Follow-up | Goals, calendar, indicators and notifications.               |
| Content | Notes, search, import, attachments and bulk creation.         |
| Mobility | Mobile app, push notifications and offline use.          |
| Collaboration | Workspaces, sharing, classes and public content.     |
| Intelligence | Suggestions, experiments and individual adaptation of the algorithm. |
