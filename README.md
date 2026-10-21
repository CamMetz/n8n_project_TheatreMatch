# n8n_project_TheatreMatch

## 1. Project Overview

TheatreMatch is an automation project designed to help users discover theatre performances in Paris that match their interests, budget, and availability.

The system collects theatre event information from the Paris Open Data API, stores events in data tables, and uses automation and AI to provide personalized search results and notifications through Telegram.

The main goal is to make theatre discovery easier, faster, and more personalized.

## 2. Project Objectives

The main objectives are to:

- Automatically collect theatre event data.
- Store and organize event information.
- Allow users to save their preferences.
- Search for performances using natural language.
- Use Gemini AI to interpret search requests and classify theatre genres.
- Send automatic notifications about relevant performances.
- Avoid sending duplicate notifications for the same event.

## 3. Technologies Used

| Technology | Purpose |
|---|---|
| n8n | Workflow automation and orchestration |
| Paris Open Data API | Theatre event data source |
| Gemini AI | Natural-language search and genre classification |
| Telegram Bot | User interaction and notifications |
| n8n Data Tables | Storage for events, users, search state, and notification history |

## 4. System Architecture

TheatreMatch is organized into four main workflows.

### 4.1. Theatre Event Import

**Workflow:** `TheatreMatch — Import des spectacles`

This workflow retrieves theatre event data from the Paris Open Data API, processes the available information, and stores events in the `TheatreMatch_Events` data table.

The workflow also includes a genre-classification step using Gemini AI. Events are processed in batches to reduce the number of model requests.

### 4.2. Telegram Search and Preferences

**Workflow:** `TheatreMatch`

This workflow handles user interactions through Telegram.

Users can save their preferences and search for theatre performances using natural-language requests. Gemini AI helps interpret search queries and identify relevant events from the stored data.

### 4.3. Automatic Notifications

**Workflow:** `TheatreMatch - Alertes automatiques`

This workflow compares stored events with user preferences and prepares a summary of matching performances.

Notifications are sent through Telegram. A notification history is stored to help prevent duplicate notifications for the same user and event.

### 4.4. Daily Scheduling

**Workflow:** `TheatreMatch - Planification quotidienne`

This workflow uses a Schedule Trigger to launch the automatic notification process every day.

The event import is scheduled for 08:00, while automatic notifications are scheduled for 09:00, using the `Europe/Paris` time zone.

## 5. Data Storage

The project uses n8n Data Tables to store information.

- **TheatreMatch_Events:** theatre titles, descriptions, genres, dates, occurrences, venues, prices, booking links, and source identifiers.
- **TheatreMatch_Users:** Telegram user identifiers and saved preferences.
- **TheatreMatch_SearchState:** temporary state used during the search process.
- **TheatreMatch - Notifications:** notification history, including the user, event, timestamp, and status.

## 6. Main Features

- Automated theatre event collection.
- Personalized user preferences.
- Natural-language search through Telegram.
- AI-assisted genre classification.
- Daily personalized notifications.
- Notification history and duplicate prevention.
- Event information including dates, venues, prices, and booking links when available.

## 7. How to Use TheatreMatch

1. Open the TheatreMatch Telegram bot.
2. Save your preferences using the available preference commands.
3. Start a search with `/recherche`.
4. Enter a request describing the type of performance you want.
5. Review the matching performances returned by the bot.
6. Receive automatic notifications when relevant performances match your saved preferences.

The exact results depend on the available event data and the user's preferences.

## 8. Installation and Configuration

The project runs on n8n and requires the following components:

- An n8n instance.
- Access to the Paris Open Data API.
- A configured Gemini AI credential.
- A Telegram bot and Telegram credentials.
- The required n8n Data Tables.
- The four configured workflows.

Credentials and API keys must be configured securely in n8n and must not be committed to a public repository.

## 9. Current Status

The core search workflow has been tested successfully through Telegram.

The import workflow includes batch-based genre classification, and the automatic notification workflows have been published. The complete scheduled execution of the import and notification process still needs to be verified through execution logs.

## 10. Future Improvements

Possible improvements include:

- Improving genre classification accuracy.
- Handling incomplete event information more reliably.
- Adding more advanced search filters.
- Improving error handling and API retry logic.
- Monitoring scheduled executions and Gemini API usage.
- Adding more event sources in the future.

## 11. Project Goal

TheatreMatch demonstrates how workflow automation, AI, APIs, and messaging platforms can be combined to build a practical personalized recommendation system.
