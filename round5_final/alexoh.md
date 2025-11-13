# Final Project Proposal: Kinnect

**Team Name**: Team 2  
**Submission Date**: 11/13/25  
**GitHub Organization**: https://github.com/nets-2130-Kinnect

---

## Team Information

### Team Members

| Name | PennKey | Primary Role(s) | Secondary Skills |
|------|---------|----------------|------------------|
| Alexandra Oh | alexoh | Backend & Database (Firebase/MongoDB) | Node.js, Database Schema Design |
| Caroline Cummings | carfc |Frontend Dev (Vite + React) & UI/UX | CSS, Data Visualization |
| Emily Lo | emlo | Backend Logic & QC Module | Python, Data Validation Logic |
| Shivi Jain | shivij | Aggregation Module & Data Analysis | Database Querying (NoSQL/SQL), Excel |

### Team Skills Inventory

**Skills we have:**
- Frontend development: Caroline, Emily
- Backend Development (Node.js, Python): Alex, Shivi
- Database Design (NoSQL/SQL concepts): Alex, Shivi 
- Data Analysis & Visualization: Emily, Shivi
- QC / Validation Logic: Emily 

**Skills we need to learn/acquire:**
- React State Management & Hooks: [Why we need it: To build an interactive frontend for logging and viewing leaderboards.] - Caroline will learn it
- NoSQL Querying (Firestore/MongoDB): [Why we need it: To build the aggregation module for the leaderboard.] - Alex will learn it

**External resources we might need:**
- Vercel / Netlify / Heroku: [Hosting for our React web app] - [Status: Acquired (Free tier), Cost: $0]
- MongoDB Atlas: [Alternative database if we don't use Firestore] - [Status: Acquired (Free tier), Cost: $0]

### Team Availability for TA Meetings

**Week of [Date]:**

_List all time slots when the ENTIRE team can meet with a TA. Use Eastern Time. Format: Day, Time-Time_

- Monday: 12-1:45pm
- Tuesday: Not available
- Wednesday: 12-1:45pm
- Thursday: Not available
- Friday: 10am-5pm

**Preferred meeting duration**: 30 min

**Meeting format preference**: Zoom/Google Meet

**Primary contact for scheduling**: alexoh@seas.upenn.edu

---

## Project Overview

### Project Connection to Round 4

**Round 4 Decision**: STAYING

**Original idea from**: [Round 1/2/3 author(s) - names/pennkeys]

**How the idea evolved**: 
_Briefly describe how your project changed from its original conception through Rounds 1-4 to this final proposal

Our project has become much more feasible. We are forgoing a mobile app and all 3rd-party API integrations (HealthKit, Strava, etc.) and instead building our platform as a web application. Kinnect will now rely on manual user logging of daily activity. This shift allows us to focus entirely on the core crowdsourcing loop: using intrinsic motivation, gamification (streaks), and social accountability (team leaderboards) to encourage consistent participation.

### Problem Statement

_Refined from your Round 4 decision_
Staying consistent with daily fitness is difficult because most apps focus on intense, individual performance rather than simple social accountability. This lack of a "team" feeling causes many people to abandon their health habits. Kinnect solves this by creating a simple, low-stakes, web-based challenge that uses team competition to make fitness fun and consistent.

### One-Sentence Pitch
Kinnect is a web-based challenge that uses team leaderboards and manual activity logging to make daily fitness a fun, social, and consistent habit.


### Target Users

**End Users**: Penn students participating in Kinnect who are looking for a simple, fun, and social way to stay motivated.

**Crowd Workers**: The end-users themselves. Their manually-logged activity data is the contribution that powers the team leaderboards.

**Scale**:  A minimum of 30-50 real users for our 4-week demo. This is needed to create at least 3-4 active teams and generate enough data for a meaningful, competitive leaderboard.

### Project Type

- [ ] Human computation algorithm
- [ ] Social science experiment with the crowd
- [ ] Tool for crowdsourcing (requesters or workers)
- [X ] Business idea using crowdsourcing
- [ ] Other: [specify]

---

## System Architecture

### Flow Diagram

_Required: Include a visual flow diagram showing the major components/stages of your project_

**Flow diagram location**: [e.g., `docs/flow-diagram.pdf` or embed image here]

Your flow diagram MUST clearly show:
- [ ] Where/when the crowd touches the data
- [ ] Your quality control module
- [ ] Your aggregation module
- [ ] Data flow between components
- [ ] What happens before crowd involvement
- [ ] What happens after crowd contribution


**If you haven't created it yet**: Describe in words the major components and their relationships:

User Authentication (Before Crowd): A new user signs up or an existing user logs into the web app.

Crowd Task: Manual Activity Logging - The user navigates to the "Log Activity" form and submits their data (e.g., "Run," "3 miles"). This is the primary crowd contribution.

Data Ingestion & QC Module: The raw activity data is sent to our backend. It is immediately validated against a set of simple hard-limit rules (e.g., IF distance > 50 miles THEN reject). This is the quality control step.

Database Storage: If the activity passes QC, it's saved to the activities collection in our database. If it fails, it's rejected, and an error is sent to the user.

Aggregation Module: A backend process sums the validated activity points for all users and groups them by team. This SUM(...) GROUP BY team function is the aggregation.

Final Output: Leaderboard: The aggregated, sorted team scores are fetched from the backend and displayed to all users on the "Leaderboard" page, completing the feedback loop.


### Major System Components

_List all major components with point values (1-4) indicating implementation complexity. Total should be 15-20 points._

| Component | Description | Points | Owner(s) | Dependencies |
|-----------|-------------|--------|----------|--------------|
| Database Schema and Setup | Defines and sets up the MongoDB collections for `users`, `activities`, and `teams` | 3 | Alex | N/A |
| User Auth & Team Assignment | Handles user signup/login. Includes backend logic for a user to join a team. | 4 | Alex, Caroline | Database Schema |
| Frontend: UI/UX (React) | Builds all user-facing pages: Login, Signup, Activity Logging Form, Leaderboard. | 4 | Caroline | N/A (can build with mock data) |
| Backend: Activity Logging & QC | API endpoint for activity form. Runs data through simple QC hard-limit rules. | 4 | Emily | Database Schema |
| Backend: Aggregation Module | Backend query/logic that calculates total scores for each team and user. | 3 | Shivi | Database Schema |
**Total Points**: 18

**Point allocation rationale**: 
_If teaching staff questions your point distribution, explain your reasoning here_

The total of 18 points reflects a substantial but achievable 4-week project. The highest point values are given to User Auth/Team Assignment and the Frontend UI as they are complex, state-heavy, and critical for any user interaction. The Activity Logging & QC also gets a 4 because it's the core link between the user and the database and contains our main quality control logic. The database setup and aggregation logic are foundational but slightly less complex. Finally, recruitment/analysis is a 2-point task as it's less about code complexity and more about executing the experiment.

### Detailed Workflow

_Step-by-step description of how your system works from start to finish_

1. **User signs up and is assigned to a team**:  
   The user creates an account through Firebase Authentication. The backend checks MongoDB for an existing profile; if none exists, it creates a new `user` document and assigns the user to a default city/team.

2. **User views home screen and leaderboards**:  
   The React frontend requests the user’s current stats and team leaderboard data from the backend and displays it on the home/leaderboard screens.

3. **User logs an activity (crowd contribution)**:  
   The user enters an activity manually or confirms an auto-synced workout. This data is submitted from the frontend to the backend activity logging API.

4. **Backend runs quality control on the submitted activity**:  
   The backend validates the data using QC rules (e.g., sanity checks on duration, distance, unrealistic values) and determines whether to accept or reject the activity.

5. **Valid activities are stored in the database and user stats are updated**:  
   Accepted activities are written to the `activities` collection in MongoDB. User-related fields (e.g., streaks, last active date) are updated in the `users` collection.

6. **Aggregation module computes team and user scores**:  
   When a user loads the "Leaderboard" page, the frontend requests data from the backend. The **Aggregation Module** (a real-time MongoDB Aggregation Pipeline query) runs, summing all valid points and grouping them by team.

7. **Updated results are shown to the user and reinforce engagement**:  
   The backend sends the sorted leaderboard (a JSON array) to the frontend. The user sees their team's new position, completing the fast-feedback loop and motivating them to log more.


### Human vs. Automated Tasks

| Task | Performed By | Justification |
|:---|:---|:---|
| **Activity Logging** | Human | This is the core crowdsourced task. It requires personal, real-world knowledge that a machine cannot have (i.e., "Did I *actually* go for a 3-mile run?"). |
| **Activity Validation (QC)** | Automated | This is a simple, rule-based check. A machine can instantly and objectively check data against hard limits (e.g., `IF miles > 50 THEN reject`). |
| **Data Aggregation** | Automated | This is a pure mathematical task (summing points, grouping by team). It is perfectly suited for a database query and ensures a real-time leaderboard. |
| **Team Selection** | Human | This is a personal, social choice. Allowing the user to choose their team is critical for establishing the social accountability that motivates participation. |


---

## Quality Control Module

### QC Strategy Overview

Our QC strategy is based on deterrence and simplicity. Since our project relies on manual logging, we are operating on a model of good-faith participation. Therefore, our QC's primary goal is not to catch every minor exaggeration but to prevent obvious, game-breaking fraud that would demotivate honest users and destroy the leaderboard's integrity. We cannot verify if a "3-mile run" was actually 3 miles, but we can reject a "100-mile run."

This approach is appropriate because it's technically feasible within our 4-week timeline and it aligns with our core loop. The goal is to keep the challenge "fair enough" to be fun and motivating. We will use a primary automated filter for impossible entries, supported by a secondary, manual admin check for suspicious patterns.

### Specific QC Mechanisms

**Primary mechanism**: [e.g., Gold standard questions, Majority voting, Expert review]

**Implementation details**:
- Input format: A JSON object from the frontend API call, e.g., { "userId": "...", "activityType": "run", "value": 50, "unit": "miles" }
- Processing: The backend QC module checks the value against a predefined dictionary of hard-coded limits (e.g., limits = { 'run': 50, 'walk': 75, 'cycle': 150 }).
- Output format: If valid, the data is passed to the database module. If invalid, an error is returned to the frontend, e.g., { "error": "Activity value exceeds the 50-mile limit for 'run'." }
- Threshold for acceptance: The submitted value must be less than or equal to the hard-coded limit for that activityType.

**Additional mechanisms**:
- [ ] Gold standard questions
  - _How many? How distributed? Pass/fail criteria?_
- [ ] Majority voting across multiple workers
  - _How many workers per task? Tie-breaking?_
- [ ] Attention checks
  - _What types? How often?_
- [ ] Reputation system
  - _How do workers build reputation?_
- [X ] Statistical outlier detection
  - As described above. We will define simple, common-sense hard limits for each activity type (e.g., max 50 miles/day running, max 6 hours/day activity). Any submission exceeding these limits is automatically rejected.
- [X ] Other: As admins, we (the team) will manually scan the leaderboard 1-2 times per week. We will look for suspicious patterns that the hard-limit rule might miss (e.g., a user logging exactly "49 miles" every single day) and manually remove the fraudulent data.

### QC Module Code Plan

**Location in repo**: [e.g., `src/qc/quality_control.py`]

**Key functions/classes**:
1. [Function/class name]: [Purpose]
2. [Function/class name]: [Purpose]
3. [Function/class name]: [Purpose]

**Input data format**: 
```
[Describe or show example of input data structure]
```

**Output data format**:
```
[Describe or show example of output data structure]
```

**Sample scenario**:
_Walk through a concrete example of your QC module in action_

[Example: "Worker A labels image as 'cat', Worker B labels as 'dog', Worker C labels as 'cat'. QC module applies majority voting, outputs 'cat' with confidence 0.67, flags disagreement for potential review."]

---

## Aggregation Module

### Aggregation Strategy Overview

Our aggregation strategy is a **simple, real-time summation**. The goal is to provide immediate feedback to the user and maintain a live, competitive leaderboard. We are not averaging votes or finding a consensus; we are collecting discrete contributions (users activities) and summing them to create a total score for each team.

To do this, we will first convert all diverse activities into a single, comparable unit: **"points."** For example, `1 mile run = 10 points`, `1 mile walk = 5 points`, and `30 min lift = 10 points`. This point calculation will happen *when the activity is logged* (as part of the QC/Logging module).

This strategy means our Aggregation Module's only job is to efficiently sum the pre-calculated "points" for all users within a team and rank the teams. This makes the leaderboard fast, scalable, and easy to understand.

### Aggregation Method

**Primary method**: **Database Query (Summation with Grouping)**

**Implementation details**:
- **Input format**: The aggregation module receives a request (e.g., from the frontend). It reads from the entire collection of *valid* `activity` documents in our MongoDB database.
- **Processing**: We will use a **MongoDB Aggregation Pipeline**. The pipeline will:
    1.  `$match`: Filter activities within the current challenge timeframe (e.g., last 7 days).
    2.  `$group`: Group all valid activities by `teamId` and use `$sum` to calculate the total `points` for each team.
    3.  `$sort`: Sort the resulting teams by their total score in descending order.
    4.  (Optional) `$lookup`: Join with the `teams` collection to get the `teamName`.

- **Output format**: A JSON array of ranked teams, sent back to the frontend.

**Sample scenario**:
_Walk through a concrete example of your aggregation module in action_

[Example: "10 workers rate restaurant review sentiment. Scores: 7 positive, 2 negative, 1 neutral. Aggregation weights by worker reliability scores, outputs 0.82 positive sentiment with confidence interval."]

### Integration: QC ↔ Aggregation

**How do these modules interact?**
[Describe the relationship. Does QC happen before aggregation? After? Iteratively? Do they share data structures?]

**Data flow diagram** (if different from main flow diagram):
[Location or description]

---

## User Interface & Mockups

### Interfaces Required

_You need mockups for ALL user-facing interfaces_

**For Crowd Workers:**
- [ ] Task interface / HIT design
- [ ] Instructions page
- [ ] Training/qualification interface (if applicable)

**For End Users:**
- [ ] Main interface
- [ ] Results display
- [ ] Any configuration/input screens

**For Administrators (your team):**
- [ ] Dashboard/monitoring
- [ ] Data management interface

### Mockup Details

**Mockup location**: [e.g., `docs/mockups/` folder or links to Figma/Marvel/etc.]

**For each interface, describe**:

#### Interface 1: [Name]
- **User type**: [Crowd worker / End user / Admin]
- **Purpose**: [What is this interface for?]
- **Key elements**: [What must be visible/interactive?]
- **Mockup file**: [filename or link]
- **Notes**: [Any important design decisions or requirements]

#### Interface 2: [Name]
- **User type**: [Crowd worker / End user / Admin]
- **Purpose**: [What is this interface for?]
- **Key elements**: [What must be visible/interactive?]
- **Mockup file**: [filename or link]
- **Notes**: [Any important design decisions or requirements]

_Continue for all interfaces..._

### Task Design (for crowd workers)

**If using MTurk or similar platform:**

**HIT title**: [Your HIT title]

**HIT description**: [What workers will see in the HIT listing]

**Task instructions**: 
_Write the actual instructions workers will see. Be specific and clear._

[Your instructions here]

**Example task**:
[Show workers exactly what one complete task looks like]

**Estimated time per task**: [X minutes]

**Payment per task**: $[amount]

**Number of tasks per HIT**: [number]

**Qualifications required**: [e.g., >95% approval rate, >100 HITs, US location]

---

## Technical Stack

### Technologies

**Frontend**: [e.g., React, Vue, vanilla JS, none (MTurk only)]

**Backend**: [e.g., Python/Flask, Node.js/Express, Django]

**Database**: [e.g., PostgreSQL, MongoDB, Firebase, SQLite]

**Crowdsourcing Platform**: [e.g., MTurk, custom, social media, class volunteers]

**ML/AI Tools** (if applicable): [e.g., scikit-learn, TensorFlow, OpenAI API]

**Hosting/Deployment**: [e.g., Heroku, AWS, Vercel, local]

**Other tools**: [Any other important tools or services]

### Repository Structure

**Current structure**:
```
your-repo/
├── README.md
├── docs/
│   ├── flow-diagram.pdf
│   ├── mockups/
│   └── ...
├── src/
│   ├── qc/
│   ├── aggregation/
│   └── ...
├── data/
│   ├── raw/
│   ├── sample-qc-input/
│   ├── sample-qc-output/
│   ├── sample-agg-input/
│   └── sample-agg-output/
└── ...
```

**Explain any deviations**: [If your structure differs, explain why]

---

## Data Management

### Input Data

**Source**: [Where will your input data come from?]

**Format**: [File type, structure, schema]

**Sample data location**: `data/raw/` 

**Sample data description**:
[Describe what sample data you've gathered and what it represents]

**How much data do you need?**
- For testing/development: [amount]
- For your final demo/analysis: [amount]

**Data collection plan**:
[How and when will you gather the full dataset?]

### QC Module Data

**Input location**: `data/sample-qc-input/`

**Input format**:
```
[Show example structure - can be JSON, CSV, etc.]
```

**Output location**: `data/sample-qc-output/`

**Output format**:
```
[Show example structure]
```

**Sample scenario documentation**:
[In your data/ directory, include a README explaining the sample QC data]

### Aggregation Module Data

**Input location**: `data/sample-agg-input/`

**Input format**:
```
[Show example structure]
```

**Output location**: `data/sample-agg-output/`

**Output format**:
```
[Show example structure]
```

**Sample scenario documentation**:
[In your data/ directory, include a README explaining the sample aggregation data]

### Data Dependencies

**Does your QC module output feed into your aggregation module?**
[Yes/No and explain the relationship]

**Data flow between modules**:
[Describe how data moves through your system]


## Crowd Recruitment & Management

### Recruitment Strategy

**Where will workers come from?**
[Be specific: MTurk? Class volunteers? Social media? Where exactly?]

**How will you reach them?**
[Describe your recruitment approach]

**When will you recruit?**
[Timeline for recruitment activities]

### Worker Incentives

**Compensation model**: 
- Payment per task: $[amount]
- Estimated time per task: [X minutes]
- Effective hourly rate: $[amount/hour]

**Or alternative incentive**: [e.g., course credit, gamification, intrinsic motivation]

**Justification**: [Why this incentive structure will work]

### Scale Requirements

**For MVP/Demo**:
- Minimum workers needed: [number]
- Minimum tasks completed: [number]
- Timeline: [when you need this by]

**For Full Analysis**:
- Target workers: [number]
- Target tasks: [number]
- Timeline: [when you need this by]

### Backup Plan

**If recruitment fails or is insufficient**:
- [ ] Use MTurk/paid workers (budget: $[amount])
- [ ] Simplify task to require fewer workers
- [ ] Use simulated/synthetic data
- [ ] Other: [specify]

---

## Project Milestones & Timeline

### Week-by-Week Plan

**Week 1 (Dates: 11/13 - 11/19)**
- Milestone: Core Backend Setup & Recruitment Validation
- Tasks:
  - [ ] Finalize database schema and set up MongoDB Atlas - [Alex]
  - [ ] Build User Auth endpoints (Signup/Login) - [Alex]
  - [ ] Create "Kinnect" Google Form and post to 3 channels - [Shivi]
  - [ ] Create dummy data script to test database connection - [Emily]
- Deliverable: A backend that can create users; 30+ signups on the Google Form.

**Week 2 (Dates: 11/20 - 11/26)**
- Milestone: Core Loop Functional & Challenge Launch
- Tasks:
  - [ ] Build React frontend (Auth, Activity Form, Leaderboard) - [Caroline]
  - [ ] Build Backend QC Module (Logging endpoint) - [Emily]
  - [ ] Build Backend Aggregation Module (Leaderboard endpoint) - [Shivi]
  - [ ] Connect Frontend to Backend API - [All]
  - [ ] Email live app link to recruits and officially start the challenge - [Alex]
- Deliverable: A live, deployed web app; Kinnect is officially running.

**Week 3 (Dates: 11/27 - 12/3)**
- Milestone: Run Challenge & Add Polish
- Tasks:
  - [ ] Monitor live data daily; perform manual QC spot-checks - [Emily]
  - [ ] Implement "Personal Streaks" feature (Stretch Goal) - [Caroline/Alex]
  - [ ] Send 1 mid-week engagement email (e.g., "Leaderboard Update") - [Shivi]
  - [ ] Fix any critical bugs reported by users - [All]
- Deliverable: A stable app with 1 week of real user data collected.

**Week 4 (Dates: 12/4 - 12/10)**
- Milestone: Data Analysis & Final Presentation
- Tasks:
  - [ ] Stop the challenge and export all data from MongoDB - [Shivi]
  - [ ] Analyze data (generate engagement graphs, team comparisons) - [Shivi/Emily]
  - [ ] Create final 5-slide presentation deck - [All]
  - [ ] Rehearse live demo - [All]
- Deliverable: Final presentation slides, analyzed dataset, and working demo.

### Critical Path

**Blocking dependencies** (what MUST be done before other work can proceed):
1. **Database Schema** must be done before **Auth** and **Activity Logging**.
2. **Activity Logging API** must be done before **Aggregation Module** can be tested.
3. **Recruitment (Google Form)** must be successful before **Week 2 Launch**.

**Parallel work** (what can be done simultaneously):
- **Caroline** can build the Frontend UI (using mock data) while **Emily** builds the Backend API.
- **Shivi** can recruit users while the technical team builds the app.

**Integration points** (when pieces must come together):
- **11/24**: Frontend/Backend Integration Day (Connecting React forms to Express endpoints).
- **11/26**: Deployment Day (Pushing to Vercel/Heroku for public access).
---

## Risk Management

### Technical Risks

**Risk 1**: Frontend/Backend Integration Issues (CORS, API mismatches).
- **Likelihood**: Medium
- **Impact**: High (App functionality breaks)
- **Mitigation**: Define strict API JSON contracts (inputs/outputs) in the README before coding. Pair program the initial connection.
- **Backup plan**: Fall back to a simpler "Monolith" structure (serving React static files directly from the Express server) to avoid CORS issues.

**Risk 2**: Deployment Failure (Vercel/Heroku config issues).
- **Likelihood**: Medium
- **Impact**: High (Users can't access app)
- **Mitigation**: Deploy a "Hello World" version early in Week 1 to verify the pipeline works.
- **Backup plan**: Use Firebase Hosting/Functions for everything as it is a unified platform.

### Crowd-Related Risks

**Risk 1**: Recruitment Failure (Cold Start) - getting < 30 users.
- **Likelihood**: High
- **Impact**: High (Leaderboards look empty)
- **Mitigation**: Pre-commitment strategy in Week 1 via Google Form. Personal outreach to club leaders.
- **Backup plan**: Pivot UI to a "Collaborative Goal" (e.g., "Can Penn hit 1000 miles?") to hide the lack of team competition; use team-generated simulated data to fill gaps.

**Risk 2**: Data Fraud (Users logging fake 100-mile runs).
- **Likelihood**: High
- **Impact**: Medium (Demotivates honest users)
- **Mitigation**: Automated QC hard limits (e.g., max 50 miles). Manual admin spot-checks twice a week.
- **Backup plan**: Pivot the analysis to focus on "detecting fraud in crowdsourcing" rather than the fitness results themselves.

### Resource Risks

**Risk 1**: Team member sickness/unavailability during the short timeline.
- **Likelihood**: Low
- **Impact**: Medium
- **Mitigation**: Clear ownership of modules, but ensuring code is pushed to GitHub daily so others can pick up work.
- **Backup plan**: Cut the "Personal Streaks" and "Dashboard" features to focus solely on the Log -> Leaderboard loop.

---

## Evaluation Plan

### What You'll Measure

**Primary metrics**:
1. **User Retention**: % of users who log at least one activity in Week 1 AND Week 2. (Target: >50%)
2. **Engagement Volume**: Average # of activities logged per active user per week. (Target: >3)
3. **Team Participation**: % of users who join a specific team vs. staying independent. (Target: >90%)

**Secondary metrics**:
1. **QC Rejection Rate**: % of activities blocked by the hard-limit filter.
2. **Activity Distribution**: Breakdown of activity types (Run vs. Walk vs. Lift).

### Analysis Approach

**What questions will your analysis answer?**
1. Is a simple, gamified leaderboard sufficient to drive multi-week consistency?
2. Does being on a specific team (e.g., "Run Club") lead to higher engagement than a general team?
3. How much manual cleaning was required on top of the automated QC?

**What comparisons will you make?**
- [ ] Compare crowd vs. expert performance (N/A)
- [ ] Compare crowd vs. automated baseline (N/A)
- [ ] Compare different QC methods (Automated vs. Manual)
- [ ] Compare different aggregation methods (N/A)
- [ ] Analyze cost/quality tradeoffs (N/A)
- [X] Other: Compare engagement levels over time (Novelty effect vs. Retention).

**Data you'll collect for analysis**:
- **User Logs**: timestamps, team affiliation.
- **Activity Logs**: type, duration/distance, points value.
- **QC Logs**: rejected submission attempts.

**Analysis methods**:
We will use Python (Pandas) to ingest the JSON data exports from MongoDB. We will visualize engagement trends using simple line charts and compare team performance using bar charts.

---
## Ethical Considerations

### Worker Treatment

**Fair compensation**: Our "workers" are volunteers. Compensation is intrinsic (fun, motivation, social connection). We ensure this by making the app genuinely useful as a fitness tool, not just a data-collection tool.

**Informed consent**: The signup page will include a clear statement: "You are participating in a student project for NETS 2130. Your anonymized activity data will be displayed on public leaderboards."

**Rejection policy**: Work is only rejected if it fails objective sanity checks (e.g., >24 hours duration). Users are notified immediately with a specific error message so they can correct typos.

### Data Ethics

**Privacy**: We will NOT display full names on the public leaderboard (Usernames only). We will not collect GPS location data, only summary statistics (miles/minutes).

**Consent**: Obtained via checkbox during Signup.

**Data storage**: Stored in a private MongoDB Atlas cluster. Access is restricted to team members. Data will be deleted at the end of the semester.

### Potential Harms

**Could your project be misused?**: Users could harass low-performing teams (though no chat feature exists).

**Could it cause harm?**: Gamification can encourage over-exercise.

**Mitigation**: We implement daily point caps (e.g., max 100 points/day) to discourage unhealthy behavior. We include low-intensity activities (Walking, Stretching) to be inclusive.
---

## Documentation Standards

### Code Documentation

**Each module must include**:
- Docstrings for all functions/classes
- README in module directory
- Example usage
- Input/output format specifications

**Current documentation status**:
- [ ] QC module: [Fully documented / Partially documented / Not yet documented]
- [ ] Aggregation module: [Fully documented / Partially documented / Not yet documented]
- [ ] Other modules: [List status]

### Repository README

**Your main README.md must include**:
- [ ] Project overview and goals
- [ ] Setup instructions
- [ ] How to run the system
- [ ] Where to find QC and aggregation code
- [ ] Data format specifications
- [ ] Team member contacts
- [ ] License information

### Ongoing Documentation

**How will you keep documentation current?**
[Describe your process for maintaining docs as code evolves]

---

## Questions for Teaching Staff

### Technical Questions

1. Is it acceptable to use a simplified "hard limit" QC for this class? something more statistical (like deviation from mean) is not really fair for our scope. Users have such different workout habits, there should be a pretty wide spread.

### Scope Questions

1. Is 40 users a reasonable size? 

### Resource Questions

1. None. We are using free tiers for all services.

### Other Concerns

None at this time.

---

## Commitment

**We commit to**:
- [X] Building a working prototype with functional QC and aggregation modules
- [X] Creating comprehensive documentation in our GitHub repository
- [X] Recruiting and managing a real crowd (or simulated crowd)
- [X] Collecting sufficient data for meaningful analysis
- [X] Meeting project milestones on schedule
- [X] Communicating proactively if we encounter blockers
- [X] Treating crowd workers ethically and fairly

**Team signatures**:

- Alexandra Oh, 11/13/25
- Caroline Cummings, 11/13/25
- Emily Lo, 11/13/25
- Shivi Jain, 11/13/25

---

## Submission Checklist

This submission **is a working document**. You may not have finalized all version (of the flow diagram, the sample data, etc.), which is **acceptable**.

Before submitting this proposal, verify you have:

- [X] Completed all sections of this template
- [X] Provided team availability for TA meetings
- [X] Listed team skills and learning needs
- [X] Included point values for all components (total 15-20)
- [X] Described detailed implementation timeline
- [X] Identified risks and mitigation strategies
- [X] Had all team members review and sign

Then:

- [ ] Set up GitHub repository with required directory structure
- [ ] Prepared questions for teaching staff
- [ ] Created flow diagram showing QC and aggregation modules
- [ ] Created mockups for all user-facing interfaces
- [ ] Added sample input/output data for QC module
- [ ] Added sample input/output data for aggregation module

**Submission method**:
- **You are able to make multiple successive submission to iterate, complete this proposal.**
- Pull request to `ideation-fall-2025` repository, in `round5_final` folder
- Should be in the root of your GitHub organization

**Submission deadline**: Thursday, Nov. 13 at 11:59PM ET

