# User Management Screen — UI Specification

**Prepared by:** Helia Ghazizadeh
**Date:** May 2025

---

## What is this document?

This is a UI spec for the User Management screen, written for the developers who are going to build it. I tried to cover what each part does, how it behaves, and what the user sees at different moments — without making it too long.

Also worth mentioning — this screen is for admins, not regular users. So the whole focus should be on speed and efficiency, not simplicity. Admins know what they're doing, the interface should just get out of their way.

---

## What the screen does

The User Management screen is for admins to manage system users. You can view all users in a table, create new ones, edit existing ones, assign roles, and enable or disable accounts.

---

## Layout

The screen splits into two panels side by side — a table on the left and a form on the right. The action bar sits at the bottom.

I moved the action bar to the bottom intentionally. Admins usually scan the table first before taking any action — so it makes more sense for the buttons to sit after the content, not above it disconnected from everything. Having them at the top also created an awkward gap between the toolbar and the table that hurt readability.

Here's a rough sketch of how I'd lay it out:

```
+------------------------------------+-----------------------------+
|  ID  | User Name  | Email | Enabled|   New User                  |
|------|------------|-------|--------|                             |
|  1   | AdminUser  | admin | true   |  Username:  [_________]    |
|  2   | Test User  | test  | true   |  Display:   [_________]    |
|      |            |       |        |  Phone:     [_________]    |
|      |            |       |        |  Email:     [_________]    |
|      |            |       |        |  Roles:     [Select ▾ ]    |
|      |            |       |        |  Enabled:   [ ]            |
+------------------------------------+-----------------------------+
|  [+ New User]  [✓ Hide Disabled]                  [Save User]   |
+------------------------------------------------------------------+
```

---

## When the page first loads

- The table shows all existing users
- The right panel shows an empty form ready to fill in, with the header "New User"
- The "Hide Disabled User" checkbox is unchecked — all users visible
- The "Save User" button is greyed out until required fields are filled
- No row is selected

---

## The user table

| Column | Sortable | Notes |
|---|---|---|
| ID | Yes | Auto-generated, read-only |
| User Name | Yes | Unique |
| Email | Yes | |
| Enabled | Yes | Shows true or false |

- Clicking a column header sorts it — first click ascending, second descending
- Clicking a row highlights it and loads that user's data into the form on the right. The form header changes to "Edit User"
- Disabled users (Enabled = false) appear slightly greyed out in the table
- If no users exist: *"No users found. Click '+ New User' to add one."*

---

## The form panel

| Field | Required | Notes |
|---|---|---|
| Username | Yes | No spaces, min 3 characters, must be unique |
| Display Name | No | Free text |
| Phone | No | Numbers only |
| Email | Yes | Valid email format |
| User Roles | Yes | Multi-select — Guest, Admin, SuperAdmin |
| Enabled | No | Checkbox, unchecked by default |

The form header says **"New User"** when creating and **"Edit User"** when editing an existing user.

---

## Main interactions

**Creating a new user**
1. Click `+ New User` — form clears, header shows "New User"
2. Fill in Username, Email, and at least one Role
3. `Save User` becomes active — click it
4. New user appears in the table, form resets

**Editing a user**
1. Click any row — it highlights, form fills with that user's data
2. Header changes to "Edit User"
3. Make changes → click Save
4. Table updates

**Hiding disabled users**
1. Check "Hide Disabled User" — table filters to only show enabled users
2. Uncheck — everyone comes back

---

## Validation

Errors appear directly below the field with a red border and a short message. The Save button stays disabled until everything required is valid.

| Field | Error condition | Message |
|---|---|---|
| Username | Empty | "Username is required" |
| Username | Already taken | "This username is already taken" |
| Username | Has spaces | "Username cannot contain spaces" |
| Email | Empty | "Email is required" |
| Email | Wrong format | "Please enter a valid email address" |
| User Roles | Nothing selected | "Please select at least one role" |

---

## Things worth discussing before development

A few questions came up while writing this:

- Right now you can only disable a user — should there be a hard delete option too, or is disabling enough?
- Should there be a confirmation step when assigning Admin or SuperAdmin roles? It feels like a risky action without one.
- In a telecom environment there could easily be hundreds of users across different regions or teams — would a search bar or role-based filtering be needed from day one, or is that a later addition?
- Pagination needs to be decided early — it affects how the API is built too, so leaving it for later creates more work.

---

*P.I. Works Technical Assignment · May 2025*
