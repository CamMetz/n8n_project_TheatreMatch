# TheatreMatch — Main Workflow: Node-by-Node Explanation

## 1. Telegram Trigger

The workflow starts with the **Telegram Trigger** node. It listens for incoming messages from users on Telegram. Whenever a user sends a message to the bot, this node starts the workflow and passes the message data to the next node.

## 2. Switch Node

The **Switch** node analyses the incoming message and directs it to the appropriate branch depending on the user's command or message type. This allows the bot to handle different actions separately.

### Branch 1: `/start`

**Send a Message**

When a user sends `/start`, the bot sends a welcome message explaining how to use TheatreMatch and which commands are available.

### Branch 2: `/preferences`

**Edit Fields**

This node prepares and structures the user's data, such as their Telegram ID and preference status.

**Upsert Rows**

This node saves the user's information in the database. It inserts a new record or updates an existing one, depending on whether the user is already registered.

**Send a Message**

The bot confirms the action and guides the user through the next steps for setting their preferences.

### Branch 3: `/recherche`

**Upsert Rows**

This node updates the user's search state in the database. It records that the user wants to search for theatre shows.

**Send a Message**

The bot asks the user to describe the type of show they are looking for, for example by specifying a genre, a date, a location or a budget.

### Branch 4: Responses to `/preferences`

This branch handles the information sent by the user after they have been asked to provide their preferences, such as their favourite genre, maximum price, preferred arrondissement and preferred period.

**Get Rows**

This node retrieves the relevant user information from the database.

**IF Node**

This node checks a condition to determine which path the workflow should follow.

**Merge Node**

The Merge node combines the incoming data from the True output of the IF node and the Switch node. This allows the workflow to bring together the information needed for the next steps.

**Edit Fields**

This node prepares the preference data in the correct format before it is processed further.

**Second IF Node**

This node checks another condition to determine how the preference information should be handled.

**Upsert Rows**

This node saves or updates the user's preferences in the database, so TheatreMatch can use them for future recommendations.

**Send a Text Message**

The bot sends a confirmation message to the user, indicating that their preferences have been processed.

### Branch 5: Fallback

The fallback branch handles messages that do not directly match one of the commands recognised by the Switch node. It also helps distinguish between an unrelated message and a response to an ongoing search.

**Get Rows**

This node retrieves the user's current state from the database. This helps the workflow understand what the user is expected to do next.

**IF Node**

This node checks whether the incoming message is related to an ongoing search or is an unrelated message.

**False Branch — Unrelated Message**

If the condition is false, the bot sends a simple message explaining how the user can interact with TheatreMatch or which commands they can use.

**True Branch — Search Request**

If the condition is true, the workflow continues processing the user's search request.

**Get Rows**

This node retrieves the relevant theatre shows from the database.

**Code — Filter Shows**

This Code node filters the available shows according to the user's search request. It prepares a list of relevant events for the AI model to analyse.

**Basic LLM Chain**

This node uses a language model to analyse the filtered shows and select the most relevant recommendations based on the user's request. It formats the results into a clear and readable response while using the available data.

**Send a Message**

The bot sends the recommendations to the user through Telegram.

**Delete Rows**

This node deletes the temporary database record used to track the ongoing search. This helps reset the search state after the request has been processed.

## Conclusion

The main TheatreMatch workflow connects Telegram, database operations, conditional logic and artificial intelligence.

The Switch node directs incoming messages to the correct branch. The database nodes store and retrieve user information and preferences. The IF and Merge nodes control the workflow's logic, while the Basic LLM Chain generates personalised theatre recommendations.

Together, these nodes allow users to register their preferences, search for theatre shows and receive recommendations directly through Telegram.
