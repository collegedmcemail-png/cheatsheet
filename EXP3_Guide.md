# Experiment 3: Scrum-Based Agile Project Using Jira

## 3.1 Objective

Create and manage a Scrum project for an **Online Library Management System** in Jira. Configure the workflow, create different work-item types, organize the backlog, create a Sprint, move work through the workflow, and invite a team member.

## 3.2 Prerequisites

- A Jira account
- Permission to create projects and invite users
- Access to:

  ```text
  https://your-site.atlassian.net
  ```

  For example:

  ```text
  https://example.atlassian.net
  ```

If your institution provides a local Jira server, use its URL instead.

---

## 3.3 Sign In to Jira

1. Open the Jira URL.
2. Sign in with your account.
3. If prompted, complete the initial setup and create the first team site.
4. Select the relevant organization or site.

---

## 3.4 Create a New Scrum Project

### Manual Procedure

1. Select **Projects** in the top navigation.
2. Click **Create project**.
3. Select **Scrum software development**.
4. Choose a template, such as:
   - Scrum software development
   - Kanban software development, if required by your institution
5. Click **Use template**.
6. Enter the project details:

   - **Name**:

     ```text
     Online Library Management System
     ```

   - **Key**:

     ```text
     OLMS
     ```

   The key must be unique and generally contains only uppercase letters.

7. Select a name for the project lead.
8. Select the appropriate privacy or template settings.
9. Click **Create**.

### Result

The Scrum board should display columns such as:

- To Do
- In Progress
- In Review
- Done

---

## 3.5 Configure Workflow Columns

The exact menu varies between Jira Cloud and Jira Server/Data Center.

### Jira Cloud Procedure

1. Open the project.
2. Select **Project settings**.
3. Select **Project details**.
4. Under **Scrum** or **Workflow**, select **Edit**.
5. Verify or rename the columns:
   - To Do
   - In Progress
   - In Review
   - Done
6. Select **Save**.
7. Open the board and verify the columns appear.

### Alternative Board Configuration

1. Open the project board.
2. Select **Project settings** or **Board settings**.
3. Select **Columns**.
4. Edit the columns as required.
5. Select **Save**.

### Jira Server Procedure

1. Open **Administration**.
2. Open the project's **Issue types** or **Workflow**.
3. Open **Workflows**.
4. Open the associated workflow.
5. Review statuses and map them to the desired board columns.
6. Select **Save** and then update the board configuration.

If renaming board columns is restricted, create the project using the Scrum template and verify that the default workflow already matches the required states.

---

## 3.6 Create a Story

1. Select **Create** on the Jira board.
2. Select **Story** as the issue type.
3. Enter the summary:

   ```text
   As a library member, I want to search for available books
   ```

4. Enter the description:

   ```text
   As a registered library member, I want to search the catalog by title,
   author, subject, or ISBN so that I can quickly find books that are
   available for borrowing.
   ```

5. Set the priority:

   ```text
   High
   ```

6. In the **Sprint** field, leave the story in the backlog initially.
7. If an **Epic** field is available, create or select:

   ```text
   Catalog Search
   ```

8. Select **Create**.

### Recommended Acceptance Criteria

```text
Given that the catalog contains books,
When a member enters a search term,
Then matching books and their availability are displayed.
```

---

## 3.7 Create Two Tasks

### Task 1: Design the Database

1. Select **Create**.
2. Select **Task**.
3. Enter the summary:

   ```text
   Design the library database schema
   ```

4. Enter the description:

   ```text
   Create database tables for books, members, loans, returns, fines,
   and administrators. Document primary keys, relationships, and data types.
   ```

5. Set the priority:

   ```text
   High
   ```

6. Leave the Sprint field set to the backlog.
7. Select **Create**.

### Task 2: Develop the Member Login Page

1. Select **Create**.
2. Select **Task**.
3. Enter the summary:

   ```text
   Develop the member login page
   ```

4. Enter the description:

   ```text
   Create a responsive login page that validates a member's credentials
   and displays appropriate validation and authentication messages.
   ```

5. Set the priority:

   ```text
   Medium
   ```

6. Leave the issue in the backlog.
7. Select **Create**.

---

## 3.8 Create a Bug

1. Select **Create**.
2. Select **Bug**.
3. Enter the summary:

   ```text
   Incorrect availability status shown for reserved books
   ```

4. Enter the description:

   ```text
   The catalog currently displays reserved books as available.
   A book with an active reservation should be shown as Reserved or
   Unavailable until the reservation expires or the loan is completed.
   ```

5. Set the priority:

   ```text
   Critical
   ```

6. If an environment field is available, select:

   ```text
   Development
   ```

7. Leave the bug in the backlog.
8. Select **Create**.

---

## 3.9 Work-Item Summary

| Type | Summary | Priority | Initial Location |
|---|---|---|---|
| Story | As a library member, I want to search for available books | High | To Do |
| Task | Design the library database schema | High | To Do |
| Task | Develop the member login page | Medium | To Do |
| Bug | Incorrect availability status shown for reserved books | Critical | To Do |

---

## 3.10 Organize the Work in the Backlog

1. Open the **Backlog** view.
2. Confirm that all four work items appear in the backlog.
3. Drag and reorder the items if necessary.

A reasonable order is:

1. Incorrect availability status shown for reserved books
2. Design the library database schema
3. As a library member, I want to search for available books
4. Develop the member login page

Optionally, create an Epic named:

```text
Library Catalog MVP
```

Then associate the Story and relevant Tasks with that Epic.

---

## 3.11 Create a Sprint

### Procedure

1. Open the **Backlog**.
2. Find the sprint controls and select **Create sprint**.
3. Enter:

   ```text
   Sprint 1
   ```

4. Set the start date and duration, for example:
   - Start: current date
   - Duration: 1 or 2 weeks
5. Select **Create**.
6. Drag all four work items into **Sprint 1**.
7. Verify that the sprint contains:
   - Two Tasks
   - One Story
   - One Bug

### Manual Alternative

If drag-and-drop is unavailable:

1. Open each issue.
2. Find the **Sprint** field.
3. Select **Sprint 1**.
4. Save each issue.

---

## 3.12 Move a Work Item Through Every Stage

Choose one item, such as the Story.

### To Do → In Progress

1. Open the Story.
2. Select the status or drag the card to **In Progress**.
3. Wait for Jira to save the change.
4. Verify the issue status is **In Progress**.

### In Progress → In Review

1. Drag the card to **In Review**, or edit the issue status.
2. Verify that the card appears under **In Review**.

### In Review → Done

1. Drag the card to **Done**, or edit the status.
2. Resolve any required transition fields.
3. Verify that the Story is in the **Done** column.

### Add a Comment

During the workflow, add comments such as:

```text
Requirements reviewed and implementation started.
```

```text
Development completed and ready for testing.
```

```text
Testing passed and the story is ready to close.
```

---

## 3.13 Invite a Team Member

### Jira Cloud Procedure

1. Open **Project settings**.
2. Select **People** or **Members**.
3. Click **Add people**.
4. Enter the member's email address.
5. Select a project role, such as:
   - Developer
   - Administrator
   - Project viewer
6. Send the invitation.

### Jira Server/Data Center Procedure

1. Open **Administration**.
2. Open **Users and Groups**.
3. Create the user if necessary.
4. Open the project.
5. Open **Project settings → People**.
6. Add the user and assign a project role.
7. Send an email invitation if required.

### Demonstration Alternative

If another email address is unavailable:

1. Open the invite dialog.
2. Enter an institutional or test email address.
3. Show the role-selection screen.
4. Close the dialog without completing the invitation, if necessary.

Do not invite unauthorized individuals merely to obtain a screenshot.

---

## 3.14 Manual Verification Checklist

- [ ] Scrum project created with a unique key.
- [ ] Workflow columns are To Do, In Progress, In Review, and Done.
- [ ] Two Task issues exist.
- [ ] One Story issue exists.
- [ ] One Bug exists.
- [ ] Every issue has a description.
- [ ] Every issue has a priority.
- [ ] Backlog contains all four items.
- [ ] Sprint 1 contains all four items.
- [ ] One item moved from To Do to Done.
- [ ] A team-member invitation was demonstrated.

---

## 3.15 Conclusion

Jira provided a centralized Scrum project for planning and tracking the Online Library Management System. Different work-item types allowed the team to manage stories, tasks, and defects consistently. The backlog, Sprint, workflow columns, priorities, and assignments made project status visible, supported cross-functional collaboration, and helped the team deliver work incrementally.

---