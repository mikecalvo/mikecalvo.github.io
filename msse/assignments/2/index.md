---
title: Assignment #2
layout: default
---

# Assignment #2
## sellit.com
### Due Date: 4/3/2015

---

# Introduction
- Add Security to web application
- Add RESTful web services to application
- Functional Testing

---

# Security Requirements
- Protect access to adding a new listing for only logged in users
- Create listing page will automatically set the logged in user as the seller
- Active listing page will automatically set the logged in user as the bidder
- Reviews are protected so that only sellers and winners can provide feedback
- View/Edit profile page is protected by account

---

# Category Requirements
- When creating a listing provide the option of putting the listing in a category
- Categories are hierarchical (Electronics->Computers->Mac)
- New categories can be created when adding the listing
- User can specify category through a dropdown (select) or typeahead control.  The hierarchy of the category selected must be evident to the user
- Create a web app page that shows the complete category hierarchy

---

# Listing Image Requirements
- When creating a listing, up to 3 images can be specified (upload file from browser)
- The images are displayed on the listing detail page
- The first image is displayed on the listings page

---

# REST API Requirements
- Create a RESTful API using JSON for the following functions
- API endpoints creating or updating data must be secured
- Get listings - default active only, support parameter for completed
- Get bids for a listing
- Get/Create/Edit/Delete listing
- Get/Edit account
- Create bid
- Create feedback (both seller and buyer)

---

# Notification Requirements
- Notfiy the seller and buyer via email when the listing expires
