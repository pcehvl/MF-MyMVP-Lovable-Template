---
id: US-000
title: Post a used textbook for sale
status: ready
priority: high
revision: 1
---

# US-000 — Post a used textbook for sale

> Fictional teaching example.

## User Story

As a student who has finished a course, I want to post my used textbook with
its course code and price, so that students taking the course next semester
can find it.

## Product Context

See `product/product.md`, MVP Scope (sellers post books; buyers search by
course code).

## User Need

Sellers need a quick way to make a book findable by the students who need
it. *(E-001: buyers search by course code)*

## Acceptance Criteria

- **AC1 — Post form available.** A signed-in student can open a "Post a
  book" form with fields for title, course code, condition and price.
- **AC2 — Required fields enforced.** If title, course code or price is
  empty, the form is not submitted and each empty field shows an error
  message next to it.
- **AC3 — Posted book listed.** After a successful post, the book appears in
  "My books" with the entered title, course code, condition and price.
- **AC4 — Visible to others.** A different signed-in student can see the
  posted book when browsing books for that course code.
- **AC5 — Only owner can edit.** Only the student who posted a book sees
  "Edit" and "Delete" for it.

## UX / UI

- One primary action on the form: "Post book".
- Price in NOK, whole numbers.

## Out of Scope

- Payment, messaging between students, photos of the book.

## Open Questions

- None blocking.

## Traceability

| Evidence | Insight | Criteria |
|---|---|---|
| E-001 | Students search by course code | AC1, AC4 |
