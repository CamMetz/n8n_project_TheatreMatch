# TheatreMatch : Automatic Alerts Workflow Documentation

## 1. Workflow Overview

The **TheatreMatch - Automatic Alerts** workflow identifies theatre performances that match users' preferences, checks whether they have already been notified, and sends a personalized summary via Telegram.

It is triggered by another workflow, allowing it to run automatically as part of the daily scheduling process.

## 2. Node-by-Node Explanation

### 1. When Executed by Another Workflow
**Purpose:** Trigger the workflow when another workflow calls it.

This node makes the alerts workflow reusable. The daily scheduling workflow can execute it without duplicating its processing logic.

### 2. Get Rows - Users
**Purpose:** Retrieve registered users from the `TheatreMatch_Users` Data Table.

The retrieved data includes Telegram IDs and user preferences, such as preferred theatre genre, maximum budget, preferred period, and geographical area.

### 3. Get Rows - Events
**Purpose:** Retrieve theatre performances stored in the `TheatreMatch_Events` Data Table.

These events are used to identify performances that match each user's preferences.

### 4. Merge
**Purpose:** Combine the user data and event data.

This node brings together the two data sources so that the following Code nodes can process them together.

### 5. Code : Prepare the Data
**Purpose:** Identify the different types of records.

The code distinguishes users from events based on characteristic fields and adds a `recordType` property with either the value `user` or `event`.

This classification makes it easier to separate the two categories during the next processing step.

### 6. Code : Filter Matching Events
**Purpose:** Identify performances that match users' preferences.

The code applies several matching criteria:
- Future event availability.
- Preferred theatre genre.
- Maximum budget.
- Preferred geographical area.
- Preferred days of the week.

It generates the user-event matches that will be considered for notifications.

### 7. Get Rows : Notifications
**Purpose:** Retrieve the notification history.

The `TheatreMatch_Notifications` Data Table stores information about previously sent notifications. This history helps prevent users from receiving repeated recommendations for the same performances.

### 8. Merge : Matches and Notification History
**Purpose:** Combine the matching events with the existing notification history.

This allows the next processing step to compare selected performances against notifications that have already been recorded.

### 9. Code : Remove Duplicates
**Purpose:** Exclude notifications that have already been sent.

The code compares the selected matches with the notification history, using identifiers such as the Telegram user ID and the event ID (`sourceEventId`).

Only new user-event matches should continue to the notification stage.

### 10. Code : Group Performances
**Purpose:** Prepare a summary for each user.

Instead of sending a separate Telegram message for every performance, this node groups the selected events into a personalized summary for each user.

### 11. Send a Text Message
**Purpose:** Send the summary through Telegram.

The message contains performances matching the user's preferences, including available details such as the title, date, venue, price, and booking link.

### 12. Code : Prepare Notification Records
**Purpose:** Prepare the data to be stored in the notification history.

After the message is sent, this node prepares the records associated with the notified performances. These may include the user ID, event ID, sending date, and notification status.

### 13. Insert Rows : Notifications
**Purpose:** Save the notification records in the `TheatreMatch_Notifications` Data Table.

This final step maintains a history of sent notifications. During subsequent executions, this history helps prevent the same performances from being recommended repeatedly to the same users.

## 3. Workflow Summary

1. Retrieve users and theatre performances.
2. Combine and classify the data.
3. Match performances against user preferences.
4. Compare the results with the notification history.
5. Remove duplicates and group performances by user.
6. Send personalized summaries through Telegram.
7. Record the notifications in the Data Table.

## 4. Benefits of the Workflow

This workflow automates personalized theatre recommendations and reduces the need for users to search for suitable performances manually.

It also maintains a notification history to limit repeated messages and uses a modular architecture that makes the system easier to maintain. Scheduling, event importing, and notification delivery are handled by separate workflows.

*Note: This documentation describes the intended role of each node based on the workflow structure provided. Exact filtering and storage behavior depends on the JavaScript code and node settings configured in n8n.*
