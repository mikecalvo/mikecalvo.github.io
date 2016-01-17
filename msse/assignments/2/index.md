---
title: Assignment #2
layout: default
---

# Assignment #2
## twtr
### Due Date: 3/11/2016

# Assignment Overview
- Use Grails Controllers to add HTTP request handling to twtr application
- Implement querying of data using GORM
- Implement functional tests that issue HTTP requests to your running server to verify requirements

# Account Requirements
- A1: Create a REST endpoint that receives JSON data to create an Account
- A2: Return an error response from the create Account endpoint if the account values are invalid
- A3: Create a REST endpoint that returns JSON data with Account values for a user based on an account id or handle address. (data-driven test)

# Message Requirements
- M1: Create a REST endpoint will create a Message given a specified Account id or handle and message text
- M2: Return an error response rom the create Message endpoint if user is not found or message text is not valid (data-driven test)
- M3: Create a REST endpoint that will return the most recent messages for an Account.  The endpoint must honor a limit parameter that caps the number of responses.  The default limit is 10.  (data-driven test)
- M4: Support an offset parameter into the recent Messages endpoint to provide paged responses.
- M5: Create a REST endpoint that will search for messages containing a specified search term.  Each response value will be a JSON object containing the Message details (text, date) as well as the Account (handle)

# Follow Requirements
- F1: Create a REST endpoint that will allow one account to follow another.
- F2: For the endpoint created for requirement A3, add properties for total counts of follers and following for the account.
- F3: Add an endpoint to get the followers for an account.  This will return the details about the followers (handle, name, email, id).  Add the limit and offset logic implemented for messages to this endpoint.
- F4: Create a 'feed' endpoint which will return the most recent messages by Accounts being followed by an Account.  Include a response limit parameter.  Include a parameter to only look for messages after a specified date.
