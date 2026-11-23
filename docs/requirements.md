# Requirements: PeerLearn

Project board: Project board: https://github.com/users/barakaomar443-ctrl/projects/4

## 1. Roles
- Student: a university student who searches, reads, rates and asks questions about explanations
- Explainer: a student who writes explanations of concepts they understand and answers questions
- Admin: manages subjects and removes inappropriate explanations

## 2. User stories
- US-01 As a student, I want to search explanations by topic or subject
  so that I can find help for a concept quickly. [Must]
- US-02 As an explainer, I want to post an explanation of a concept I understand
  so that my classmates can learn it from me. [Must]
- US-03 As a student, I want to open an explanation and read it
  so that I can understand the concept. [Must]
- US-04 As a student, I want to rate an explanation from 1 to 5
  so that the best explanations are easy to find. [Must]
- US-05 As a student, I want to ask a question under an explanation
  so that I can clear up what I did not understand. [Should]
- US-06 As an explainer, I want to answer questions on my explanation
  so that my classmates understand the concept fully. [Should]
- US-07 As a student, I want an AI-generated summary of an explanation
  so that I can review it in under a minute. [Should]
- US-08 As a student, I want a short quiz generated from an explanation
  so that I can test my understanding. [Should]
- US-09 As a student, I want to see the most viewed and top rated explanations
  so that I study the topics my classmates find most useful. [Should]
- US-10 As an admin, I want to add and rename subjects
  so that explanations are organized under clear categories. [Must]
- US-11 As an admin, I want to remove inappropriate explanations
  so that the platform stays useful and respectful. [Should]
- US-12 As an admin, I want a dashboard of usage statistics
  so that I can see which subjects and explanations are popular. [Could]
- US-13 As a student, I want to register and log in with my university e-mail
  so that my ratings and questions are linked to my account. [Must]
- US-14 As an explainer, I want to edit my own explanation
  so that I can fix mistakes and improve it. [Should]
- US-15 As a student, I want to bookmark explanations
  so that I can come back to them before the exam. [Could]

## 3. Acceptance criteria
Top 5 stories. The full scenarios are in the GitHub Issues.

### US-13 Register and log in
```gherkin
Feature: Register and log in
  Scenario: Student registers with a valid account
    Given the student is on the "Register" page
    When the student enters the e-mail "sara@univ.edu" and a password of 8 characters
    Then the account is created
    And the student can log in

  Scenario: E-mail already used
    Given an account with "sara@univ.edu" exists
    When someone registers with the same e-mail
    Then the request is rejected with "Account already exists"

  Scenario: Password is too short
    Given the student is on the "Register" page
    When the student enters a password of 5 characters
    Then the request is rejected with "Password must be at least 8 characters"
```

### US-02 Post an explanation
```gherkin
Feature: Post an explanation
  Scenario: Explainer posts a valid explanation
    Given the explainer is on the "New explanation" page
    When the explainer enters title "Binary Search", subject "Algorithms" and a text of 200 characters
    Then the explanation is saved
    And a student can find it by searching "Binary"

  Scenario: Missing title
    Given the explainer is on the "New explanation" page
    When the explainer submits the form without a title
    Then the request is rejected with "Title is required"

  Scenario: Text is too short
    Given the explainer is on the "New explanation" page
    When the explainer enters a text of 20 characters
    Then the request is rejected with "Explanation must be at least 100 characters"
```

### US-01 Search explanations by topic or subject
```gherkin
Feature: Search explanations
  Scenario: Search by topic returns matching explanations
    Given the app contains an explanation titled "Binary Search" in the subject "Algorithms"
    When a student searches for "Binary"
    Then the results include "Binary Search"

  Scenario: Search finds nothing
    Given the app contains no explanation matching "Zebra"
    When a student searches for "Zebra"
    Then the student sees the message "No explanations found"

  Scenario: Search with an empty text
    Given a student is on the search page
    When the student submits an empty search
    Then the system asks the student to enter a topic or subject
```

### US-03 View an explanation
```gherkin
Feature: View an explanation
  Scenario: Student opens an existing explanation
    Given the explanation "Binary Search" exists
    When the student opens it
    Then the title, subject, author name and full text are shown

  Scenario: Explanation does not exist
    Given no explanation with id 999 exists
    When the student opens the explanation with id 999
    Then the student sees "Explanation not found"

  Scenario: View counter increases
    Given the explanation "Binary Search" has 4 views
    When a student opens it
    Then it has 5 views
```

### US-04 Rate an explanation
```gherkin
Feature: Rate an explanation
  Scenario: Student gives a valid rating
    Given a student has not rated "Binary Search"
    When the student rates it 4
    Then the rating is saved
    And the average rating of "Binary Search" is updated

  Scenario: Rating out of range
    Given a student is viewing "Binary Search"
    When the student tries to rate it 6
    Then the request is rejected with "Rating must be between 1 and 5"

  Scenario: Explainer rates their own explanation
    Given an explainer wrote "Binary Search"
    When the same explainer tries to rate it
    Then the request is rejected with "You cannot rate your own explanation"
```

## 4. Non-functional requirements
- NFR-01 (Performance) Search returns results in < 2 s for 1,000 explanations. [Should]
- NFR-02 (Performance) An explanation page loads in < 2 s. [Should]
- NFR-03 (Performance) An AI summary or quiz is shown in < 10 s. [Could]
- NFR-04 (Security) Passwords are stored hashed (bcrypt, cost >= 12). [Must]

## 5. MVP scope
Must stories in the MVP: US-13, US-10, US-02, US-01, US-03, US-04
Must NFR in the MVP: NFR-04
Reason: together they let a student register, an admin create subjects,
an explainer post an explanation, and classmates search, read and rate it,
with safely stored passwords. The Should and Could stories (US-05 to US-09,
US-11, US-12, US-14, US-15) stay in the backlog. The AI summary and quiz
(US-07, US-08) come right after the MVP.