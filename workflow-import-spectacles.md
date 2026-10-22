# TheatreMatch - Show Import Workflow Documentation

## 1. Overview

The **TheatreMatch - Import des spectacles** workflow automatically retrieves theatre events from the Paris Open Data API, processes the information, classifies shows by genre using Google Gemini, and stores the results in the `TheatreMatch_Events` data table.

The workflow runs every day at **08:00 AM (Europe/Paris)** and helps keep the theatre event database up to date while reducing duplicate records.

## 2. Node-by-Node Explanation

### 1. Schedule Trigger
Starts the workflow automatically every day at 08:00 AM (Paris time). No manual intervention is required for the scheduled import.

### 2. HTTP Request
Sends a request to the Paris Open Data API to retrieve theatre events and their associated information, such as titles, descriptions, dates, venues, prices, and booking links.

### 3. Code
Processes and transforms the data received from the API into a format suitable for the next steps of the workflow.

### 4. IF
Checks a configured condition and directs the data to one of two branches: **true** or **false**. Each branch follows a different processing path.

### 5. Upsert Rows Events — False Branch
This node is directly connected to the false output of the IF node. It inserts or updates event records in the `TheatreMatch_Events` table.

### 6. Edit Fields — True Branch
Selects, renames, or prepares the fields needed for the remaining import process, ensuring that the event data follows the expected structure.

### 7. Data Table — Duplicate Filter
Checks the `sourceEventId` field against existing records in `TheatreMatch_Events`. This helps identify events that have already been imported and reduces unnecessary processing.

### 8. Regrouper par 10 (Code)
Groups the events into batches of ten. This reduces the number of individual requests sent to the AI classification step and helps manage processing and API usage.

### 9. Information Extractor
Uses Google Gemini to analyze event titles, descriptions, and available tags to determine the most appropriate theatre genre.

The classification can include categories such as comedy, tragedy, classical theatre, contemporary theatre, absurd theatre, musical theatre, drama, and undetermined.

### 10. Réassocier les genres (Code)
Matches the genres returned by Gemini to their corresponding original events. It preserves the event information and adds the classification and its justification.

### 11. Upsert Rows
Inserts new event records or updates existing records in the `TheatreMatch_Events` table. The `sourceEventId` field is used to identify events and prevent unnecessary duplicate records.

## 3. Workflow Summary

The workflow follows this general process:

1. Start the scheduled import.
2. Retrieve theatre events from the Paris Open Data API.
3. Prepare and route the incoming data.
4. Process the appropriate IF branch.
5. Filter events that have already been imported.
6. Group eligible events into batches of ten.
7. Classify theatre genres using Google Gemini.
8. Match the classifications to the original events.
9. Insert or update event records in the database.

The false branch of the IF node follows its own path directly to `Upsert Rows Events`.

## 4. Benefits

- **Automation:** Events are imported on a daily schedule.
- **Data consistency:** Event information is prepared before storage.
- **Duplicate reduction:** The `sourceEventId` field helps identify existing events.
- **AI-powered classification:** Gemini helps categorize shows by theatre genre.
- **Efficient processing:** Batch processing reduces the number of AI requests.
- **Centralized database:** The `TheatreMatch_Events` table provides the event data used by the rest of the application.

## 5. Role in the TheatreMatch Project

This workflow maintains the event database that powers TheatreMatch. The Telegram recommendation and automatic alert workflows can then use this information to suggest theatre performances that match users' preferences, including genre, budget, preferred area, and available dates.
