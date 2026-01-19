# Lab Implementation Plan: Putting it All Together: IAM Lab

## Overview

This is a Flask-based authentication and authorization system with React frontend. The lab requires implementing:

1. User and Recipe models with proper validations
2. Authentication routes (signup, login, logout, check_session)
3. Recipe routes (list and create with authorization)

## Information Gathered

### From README.md:

- User model needs: id, username, \_password_hash, image_url, bio
- Recipe model needs: id, title, instructions (50+ chars), minutes_to_complete, user_id
- User model must use bcrypt, have unique username, present username
- Recipe model must have present title, present instructions (50+ chars)
- Routes: POST /signup, GET /check_session, POST /login, DELETE /logout, GET/POST /recipes

### From Tests:

- User needs `authenticate` method that checks password
- Accessing `password_hash` should raise AttributeError
- Response JSON for users should include: id, username, image_url, bio (no password)
- Session should store user_id
- Recipe responses should include nested user object

## Plan

### Step 1: Implement User Model in models.py ✅

- Add columns: id, username, \_password_hash, image_url, bio
- Add validators for username (present, unique)
- Add hybrid_property for password_hash that encrypts and raises AttributeError on read
- Add authenticate method
- Add relationship to Recipe (has many)

### Step 2: Implement Recipe Model in models.py ✅ Done

- Add columns: id, title, instructions, minutes_to_complete, user_id
- Add validators for title (present) and instructions (present, 50+ chars)
- Add relationship to User (belongs to)

### Step 3: Implement Routes in app.py ✅ Done

- Signup: POST /signup - Create user, save session, return 201 or 422
- CheckSession: GET /check_session - Return user data if logged in, else 401
- Login: POST /login - Authenticate, save session, return user data or 401
- Logout: DELETE /logout - Clear session, return 204 or 401
- RecipeIndex:
  - GET: Return all recipes with user data, require login
  - POST: Create new recipe, require login, return 201 or 422

### Step 4: Run migrations and seed ✅ Done

- Initialize Flask db
- Run migrations
- Seed database
- Run tests

## Final Status: All Tests Passing! ✅

- 19 tests passed (7 model tests + 12 app tests)

## Dependent Files to be Edited

1. `/home/culprit58/Desktop/development/code/phase4/labs/python-p4-iam-putting-it-all-together-lab/server/models.py`
2. `/home/culprit58/Desktop/development/code/phase4/labs/python-p4-iam-putting-it-all-together-lab/server/app.py`

## Followup Steps

1. Run model tests: `pytest testing/models_testing/`
2. Run all tests: `pytest`
3. Test the application in browser
