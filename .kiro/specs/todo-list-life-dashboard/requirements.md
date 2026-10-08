# Requirements Document

## Introduction

The Todo List Life Dashboard is a single-page personal productivity website that helps a user start the day organized. The dashboard displays the current time, date, and a time-based greeting, provides a 25-minute focus timer, manages a persistent to-do list, and offers configurable quick links to favorite websites. All data is stored client-side using the browser Local Storage API, so the dashboard requires no backend server and can run as a standalone web app or a browser extension.

## Glossary

- **Dashboard**: The single-page web application that hosts the greeting, focus timer, to-do list, and quick links features.
- **Greeting_Module**: The Dashboard component that displays the current time, current date, and a time-of-day greeting message.
- **Focus_Timer**: The Dashboard component that counts down from 25 minutes and responds to Start, Stop, and Reset actions.
- **Todo_Module**: The Dashboard component that manages creation, editing, completion, and deletion of tasks.
- **Task**: A single to-do item consisting of a text description and a completion state (done or not done).
- **Quick_Links_Module**: The Dashboard component that manages and opens user-defined links to external websites.
- **Quick_Link**: A single saved link consisting of a display label and a destination URL.
- **Local_Storage**: The browser Local Storage API used to persist Task and Quick_Link data on the client device.
- **Time_Of_Day**: A period derived from the current hour, classified as morning, afternoon, or evening.

## Requirements

### Requirement 1: Greeting and Clock Display

**User Story:** As a user, I want to see the current time, date, and a greeting when I open the dashboard, so that I feel oriented and welcomed at the start of my session.

#### Acceptance Criteria

1. WHEN the Dashboard loads, THE Greeting_Module SHALL display the current local time in hours and minutes.
2. WHEN the Dashboard loads, THE Greeting_Module SHALL display the current local date including day, month, and year.
3. WHILE the Dashboard is open, THE Greeting_Module SHALL update the displayed time at least once every 60 seconds.
4. WHERE the current Time_Of_Day is morning, THE Greeting_Module SHALL display a morning greeting message.
5. WHERE the current Time_Of_Day is afternoon, THE Greeting_Module SHALL display an afternoon greeting message.
6. WHERE the current Time_Of_Day is evening, THE Greeting_Module SHALL display an evening greeting message.

### Requirement 2: Focus Timer

**User Story:** As a user, I want a 25-minute focus timer with Start, Stop, and Reset controls, so that I can time focused work sessions.

#### Acceptance Criteria

1. WHEN the Dashboard loads, THE Focus_Timer SHALL display a countdown value of 25 minutes and 0 seconds.
2. WHEN the user activates the Start control, THE Focus_Timer SHALL begin counting down from the current displayed value toward 0.
3. WHILE the Focus_Timer is counting down, THE Focus_Timer SHALL update the displayed countdown value once every second.
4. WHEN the user activates the Stop control, THE Focus_Timer SHALL pause the countdown and retain the current displayed value.
5. WHEN the user activates the Reset control, THE Focus_Timer SHALL stop the countdown and set the displayed value to 25 minutes and 0 seconds.
6. WHEN the Focus_Timer countdown reaches 0 minutes and 0 seconds, THE Focus_Timer SHALL stop the countdown and present a session-complete indication to the user.
7. IF the user activates the Start control while the Focus_Timer is already counting down, THEN THE Focus_Timer SHALL continue the current countdown without resetting the displayed value.

### Requirement 3: Add Tasks

**User Story:** As a user, I want to add tasks to a to-do list, so that I can keep track of what I need to do.

#### Acceptance Criteria

1. WHEN the user submits a task with non-empty description text, THE Todo_Module SHALL create a new Task with that description and a completion state of not done.
2. WHEN the Todo_Module creates a new Task, THE Todo_Module SHALL display the new Task in the task list.
3. WHEN the Todo_Module creates a new Task, THE Todo_Module SHALL clear the task input field.
4. IF the user submits a task with empty description text, THEN THE Todo_Module SHALL reject the submission and SHALL leave the task list unchanged.

### Requirement 4: Edit Tasks

**User Story:** As a user, I want to edit an existing task, so that I can correct or update its description.

#### Acceptance Criteria

1. WHEN the user activates the edit control for a Task, THE Todo_Module SHALL present the current description of that Task in an editable field.
2. WHEN the user confirms an edit with non-empty description text, THE Todo_Module SHALL update that Task with the new description.
3. IF the user confirms an edit with empty description text, THEN THE Todo_Module SHALL reject the edit and SHALL retain the previous description of that Task.

### Requirement 5: Mark Tasks as Done

**User Story:** As a user, I want to mark tasks as done, so that I can see which items I have completed.

#### Acceptance Criteria

1. WHEN the user activates the completion control for a Task with a not-done state, THE Todo_Module SHALL set the completion state of that Task to done.
2. WHEN the user activates the completion control for a Task with a done state, THE Todo_Module SHALL set the completion state of that Task to not done.
3. WHERE a Task has a completion state of done, THE Todo_Module SHALL display that Task with a visual indication distinct from not-done tasks.

### Requirement 6: Delete Tasks

**User Story:** As a user, I want to delete tasks, so that I can remove items I no longer need.

#### Acceptance Criteria

1. WHEN the user activates the delete control for a Task, THE Todo_Module SHALL remove that Task from the task list.
2. WHEN the Todo_Module removes a Task, THE Todo_Module SHALL remove that Task from the displayed task list.

### Requirement 7: Persist Tasks in Local Storage

**User Story:** As a user, I want my tasks to be saved automatically, so that my list is still present when I reopen the dashboard.

#### Acceptance Criteria

1. WHEN the Todo_Module creates, edits, completes, or deletes a Task, THE Todo_Module SHALL write the current set of tasks to Local_Storage.
2. WHEN the Dashboard loads, THE Todo_Module SHALL read the stored set of tasks from Local_Storage and SHALL display each stored Task with its stored description and completion state.
3. IF no stored task data exists in Local_Storage when the Dashboard loads, THEN THE Todo_Module SHALL display an empty task list.
4. FOR ALL tasks, writing the current set of tasks to Local_Storage and then reading the stored set from Local_Storage SHALL produce a set of tasks equivalent in description and completion state (round-trip property).

### Requirement 8: Quick Links Management

**User Story:** As a user, I want buttons that open my favorite websites, so that I can reach frequently used sites in one click.

#### Acceptance Criteria

1. WHEN the user submits a Quick_Link with a non-empty label and a valid URL, THE Quick_Links_Module SHALL create a new Quick_Link with that label and URL.
2. WHEN the Quick_Links_Module creates a new Quick_Link, THE Quick_Links_Module SHALL display a button labeled with the Quick_Link label.
3. WHEN the user activates a Quick_Link button, THE Quick_Links_Module SHALL open the destination URL of that Quick_Link in a new browser tab.
4. WHEN the user activates the delete control for a Quick_Link, THE Quick_Links_Module SHALL remove that Quick_Link from the displayed list.
5. IF the user submits a Quick_Link with an empty label or an invalid URL, THEN THE Quick_Links_Module SHALL reject the submission and SHALL leave the Quick_Link list unchanged.

### Requirement 9: Persist Quick Links in Local Storage

**User Story:** As a user, I want my quick links to be saved, so that they remain available when I reopen the dashboard.

#### Acceptance Criteria

1. WHEN the Quick_Links_Module creates or deletes a Quick_Link, THE Quick_Links_Module SHALL write the current set of quick links to Local_Storage.
2. WHEN the Dashboard loads, THE Quick_Links_Module SHALL read the stored set of quick links from Local_Storage and SHALL display a button for each stored Quick_Link.
3. IF no stored quick link data exists in Local_Storage when the Dashboard loads, THEN THE Quick_Links_Module SHALL display an empty quick links list.
4. FOR ALL quick links, writing the current set of quick links to Local_Storage and then reading the stored set from Local_Storage SHALL produce a set of quick links equivalent in label and URL (round-trip property).

## Technical Constraints

- **TC-1 Technology Stack**: THE Dashboard SHALL be implemented using HTML for structure, CSS for styling, and Vanilla JavaScript without front-end frameworks, and SHALL operate without a backend server.
- **TC-2 Data Storage**: THE Dashboard SHALL store all Task and Quick_Link data client-side using the Local_Storage API.
- **TC-3 Browser Compatibility**: THE Dashboard SHALL operate in current versions of Chrome, Firefox, Edge, and Safari, and SHALL be usable both as a standalone web app and as a browser extension.

## Non-Functional Requirements

- **NFR-1 Simplicity**: THE Dashboard SHALL present a minimal interface that requires no setup steps before use.
- **NFR-2 Performance**: WHEN the user updates a Task or Quick_Link, THE Dashboard SHALL reflect the change in the displayed interface within 100 milliseconds.
- **NFR-3 Visual Design**: THE Dashboard SHALL present a clear visual hierarchy and SHALL use typography that meets WCAG 2.1 AA contrast ratios for body text.

## Project Structure Constraints

- **PS-1**: THE Dashboard SHALL include exactly one CSS file located in a `css/` directory.
- **PS-2**: THE Dashboard SHALL include exactly one JavaScript file located in a `js/` directory.
