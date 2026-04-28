# Personal Task Manager API

## Overview
A backend service that lets a single user keep track of personal tasks. The user can record what needs to be done, mark items as completed, and revise or remove them as priorities change. The API is the foundation for any future client (web, mobile, or CLI) that wants to display and manipulate the task list.

## User Stories
- As a user, I want to create a task with a title, an optional description, and a status, so I can capture work I need to do.
- As a user, I want to view all my tasks at once, so I can see what's pending and what's done.
- As a user, I want to view a single task in detail, so I can read its full information.
- As a user, I want to update a task, so I can revise its details or change its status as work progresses.
- As a user, I want to delete a task, so I can remove items that are no longer relevant.
- As a user, I want to filter the task list by status (pending or completed), so I can focus on what's still active.
- As a user, I want to search tasks by title, so I can find a specific task quickly.

## Requirements

### Functional
- [ ] Create a task with a title, an optional description, and a status.
- [ ] Title is required and must be between 3 and 100 characters.
- [ ] Description is optional and must not exceed 300 characters.
- [ ] Status is restricted to a known set of values: `pending` or `completed`.
- [ ] Retrieve the full list of tasks.
- [ ] Retrieve a single task by its identifier.
- [ ] Update one or more fields of an existing task.
- [ ] Delete a task by its identifier.
- [ ] Each task records when it was created and when it was last updated.
- [ ] Invalid input is rejected with a clear, human-readable error message.
- [ ] Filter the task list by status.
- [ ] Search tasks by (partial) title.
- [ ] Paginate the task list using a limit and offset.

### Non-Functional
- [ ] **Performance** — the list endpoint stays responsive as the task count grows; pagination is supported.
- [ ] **Security** — every input is validated at the API boundary; malformed or unexpected payloads are rejected.
- [ ] **Reliability** — distinct HTTP status codes signal success, validation failures, missing resources, and server errors.
- [ ] **Consistency** — every response, success or failure, follows a single, predictable envelope shape.
- [ ] **Maintainability** — concerns are separated cleanly so the request handling, business rules, and data access can each evolve independently.
- [ ] **Configurability** — environment-specific settings (database connection, port) come from configuration rather than being hard-coded.

## Acceptance Criteria
- [ ] Creating a task with valid input returns `201 Created` with the saved task in the response data.
- [ ] Creating a task with a title shorter than 3 characters or longer than 100 characters returns `400 Bad Request` with a validation message.
- [ ] Creating a task with a description longer than 300 characters returns `400 Bad Request`.
- [ ] Creating a task with a status other than `pending` or `completed` returns `400 Bad Request`.
- [ ] Listing tasks returns `200 OK` with every stored task.
- [ ] Listing tasks with a status filter returns only the tasks that match.
- [ ] Listing tasks with a title search returns only the tasks whose title matches.
- [ ] Listing tasks with pagination respects the limit and offset values.
- [ ] Requesting a task by an identifier that does not exist returns `404 Not Found`.
- [ ] Updating an existing task returns `200 OK` with the updated record; the `UpdatedAt` timestamp changes.
- [ ] Updating a non-existent task returns `404 Not Found`.
- [ ] Deleting an existing task succeeds, and a subsequent fetch for that identifier returns `404 Not Found`.
- [ ] All successful responses use the shape `{ success: true, message, data }`.
- [ ] All error responses use the shape `{ success: false, message, error }`.

## Testing Requirements
- [ ] **Unit** — validation rules (title length, description length, allowed status values).
- [ ] **Unit** — business logic in the service layer for create, update, list filters, and delete.
- [ ] **Integration** — every endpoint exercised end-to-end against a real database.
- [ ] **Edge cases** — empty request body, missing required fields, oversized fields, unknown status, identifiers that do not exist, malformed identifiers, very large list result sets.

## Out of Scope
- User accounts, authentication, or multi-user separation of tasks.
- Due dates, priorities, tags, categories, or any task metadata beyond title/description/status.
- Sub-tasks or any form of task hierarchy.
- Real-time updates (websockets, server-sent events).
- A front-end user interface.
- Email, SMS, or push notifications.
- File attachments on tasks.
- Soft delete / archive / undo.

## Design Considerations
- The API will be consumed by future clients (web, mobile, or CLI), so the response envelope is designed to be stable and forward-compatible.
- Status is modelled as a small string enum (`pending`, `completed`) so additional states could be introduced later without breaking the schema.
- Identifiers are generated by the server to avoid collisions and to keep clients from inventing ids.
- A consistent error shape simplifies client code — clients only need one error parser, regardless of which endpoint failed.
- Configuration values that differ between environments (database URL, port) are read from environment variables rather than committed to source.

## References Used
- `.claude/blueprint.md` — project blueprint (entity definition, validation rules, endpoint list, response envelope, target folder layout).
