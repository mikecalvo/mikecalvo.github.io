---
title: Assignment #1
layout: default
---

# Assignment #1
## sellit.com
### Due Date: 2/27/2015

---

# Introduction
- Implement an auction system similar to eBay
- Project will be completed in a series of 3 assignments

---
# Deliverables
1. Access to source code repository (preferrably git)
1. Working Grails project on which I can run without error or failure:
  - ./grailsw test-app
  - ./grailsw run-app
  - ./grailsw war
1. All requirements implemented and tested with passing tests

# Account Requirements
- Create web app page allowing a user to create an account
- Account creation requires email, password, name, address
- Password must be 8-16 characters with at least 1 number and 1 letter
- Create web application screen to display/edit account details
- Edit/Detail page must show date account created and last updated date

---
# Create Listing Requirements
- Create a web app page allowing a user to create a listing
- Listing is required to have a name, description, start date, listing days and starting price
- Listing page requires a seller account to be selected
- Listing page requires a deliver option to be selected (US Only, Worldwide, Pick Up Only)

---
# Show Listings
- A listing is complete when the current date is after the start date plus listing days has passed
- Create a web app page showing non-expired listings
- Listing page supports searching for a listing by name or description
- Listing page will show 10 listings at a time and support paging
- Listing page will allow searching for completed listings (checkbox)
- Selecting a listing from the list page will take user to a listing detail page where the listing details are displayed

---

# Bidding Requirements
- The listing detail page will allow for creating a bid for the listing
- A new bid requires a bid amount and a bidder account
- A new bid must be at list 50 cents more than the current highest bid
- Listing page will show the current highest bid amount
- Provide a way to show the bid history for a listing
- When the listing completes, the highest bidder is the winner
- For completed listings, display the winner account

---
# Review Requirements
- On completed listings pages, provide the ability to set the rating for both seller and buyer
- Rating includes a short description (50 characters) and thumbs up or down
- Once the rating is specified for a seller or buyer it cannot be changed
- On the account detail page display a read-only listing of reviews for the account (both buyer and seller)
