# Feature: User Registration (version 3)

**Status:** Approved

## Overview

Allows a new user to create a TaskFlow account using an email address and password, so they can start creating and managing their own tasks.

## User Story

As a new visitor,
I want to register for an account,
So that I can start using TaskFlow to manage my tasks.

## Acceptance Criteria

- Given a visitor on the registration page, when they submit a valid email and password, then an account is created and they are logged in automatically.
- Given a visitor submits an email that's already registered, when they submit the form, then they see an error message and no duplicate account is created.
- Given a visitor submits a password shorter than 8 characters, when they submit the form, then they see a validation error and the account is not created.
- Given a new account is created, when registration completes, then a welcome email is sent to the registered address.

## Out of Scope

- Social login (Google/Microsoft SSO) — tracked separately.
- Email verification before first login.

## Open Questions

- Should password complexity rules go beyond minimum length (e.g. require a number or symbol)?
- Do we need CAPTCHA or other bot protection at launch?
