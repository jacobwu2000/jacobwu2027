---
layout: post
title: "Lesson Plan: Role-Based Access Control (RBAC) in Java"
description: Roles in submitting, teaching, and grading — AP CSA Units 3 and 4
permalink: /sprint2/submission-roles
toc: true
---

**Course Context:** AP Computer Science A (Unit 3: Class Creation & Unit 4: Data Collections)

**Teaching Focus:** Roles in Submitting, Teaching, and Grading (Backend Java Logic)

---

## 1. LxD Cycle Framework

### Empathize

- **Learner Misconception:** Students often treat class instance variables as raw data containers (using `public` fields for convenience) without recognizing how encapsulation enforces system security and business logic.
- **Target Need:** Understanding how private state, mutator validation, and object collections prevent unauthorized data modification in multi-user web applications.

### Define

- **POV Statement:** High school Java learners building web applications need a concrete way to enforce authorization at the class level so that user role constraints (Student vs. Teacher/Grader) cannot be bypassed.
- **Learning Objective:** Students will design an encapsulated `Submission` class (Unit 3) and implement role-filtered list processing in a `GradebookManager` class using `ArrayList` traversals (Unit 4).
- **Success Criteria:**
  1. Write a Java class with `private` instance variables, appropriate constructors, and accessors/mutators.
  2. Implement conditional role validation (`if-else` logic) inside mutator methods to restrict grade modifications.
  3. Traverse an `ArrayList<Submission>` to filter visible objects based on the requesting user's permissions.

### Ideate

- **How Might We (HMW):** How might we use a web gradebook model to show how Java OOP principles protect data from unauthorized users?
- **Selected Activity:** A progressive code walk-through (Simple to Complex) starting with a single `Submission` object and expanding to an `ArrayList` gradebook pipeline.

---

## 2. Lesson Structure & Code Progression

### Part 1: Tech Talk & Hook (First 5 Minutes)

- **Prompt/Hook:** "Imagine a system where `submission.score = 100;` could be run by anyone from the browser console. Why is setting fields to `public` a massive security flaw?"
- **Concept:** Connect physical/real-world roles (Student, Teacher, Grader) to AP CSA **Unit 3 Encapsulation** (`private` modifiers, mutator guard logic) and **Unit 4 Data Collections** (`ArrayList` filtering).

### Part 2: Code Examples (Simple to Complex)

#### Step 1: Simple — Encapsulation & Role Validation (Unit 3)

```java
public class Submission {
    // Encapsulated state: private instance variables
    private String studentId;
    private String assignmentContent;
    private double score;
    private boolean isGraded;

    public Submission(String studentId, String assignmentContent) {
        this.studentId = studentId;
        this.assignmentContent = assignmentContent;
        this.score = 0.0;
        this.isGraded = false;
    }
    
    // Accessors
    public String getStudentId() { return studentId; }
    public double getScore() { return score; }
    public boolean isGraded() { return isGraded; }
    
    // Mutator with Role-Based Guard Logic (Unit 3 + Unit 2 Logic)
    public boolean assignGrade(String userRole, double newScore) {
        if (userRole.equals("TEACHER") || userRole.equals("GRADER")) {
            if (newScore >= 0.0 && newScore <= 100.0) {
                this.score = newScore;
                this.isGraded = true;
                return true;
            }
        }
        return false; // Access denied or invalid score
    }
}
```

#### Step 2: Complex — Collection Traversal & Authorization Filtering (Unit 4)

```java
import java.util.ArrayList;

public class GradebookManager {
    private ArrayList<Submission> submissionList;

    public GradebookManager() {
        this.submissionList = new ArrayList<>();
    }

    public void addSubmission(Submission sub) {
        submissionList.add(sub);
    }

    // Role-based retrieval: Teachers see all; Students see only their own
    public ArrayList<Submission> getSubmissionsForUser(String userId, String userRole) {
        ArrayList<Submission> authorizedList = new ArrayList<>();

        if (userRole.equals("TEACHER") || userRole.equals("GRADER")) {
            return submissionList; // Full access
        }

        // Unit 4: Traversal and filtering logic
        for (Submission sub : submissionList) {
            if (sub.getStudentId().equals(userId)) {
                authorizedList.add(sub);
            }
        }
        return authorizedList;
    }
}
```

---

## 3. Hacks & Practice Tasks

### Popcorn Hack (In-Class Check for Understanding)

- **Task:** Modify `assignGrade` in `Submission.java` to prevent a grade update if the assignment has already been graded, UNLESS the `userRole` is strictly `"TEACHER"` (Graders cannot override existing grades).
- **Time:** 3-5 minutes.

### Homework Hack

- **Task:**
  1. Extend `Submission` by adding a `private String status` field (`"SUBMITTED"`, `"UNDER_REVIEW"`, `"GRADED"`).
  2. Write a method `updateStatus(String userRole, String newStatus)` enforcing valid status state transitions based on role.
  3. Write a method in `GradebookManager` that traverses `submissionList` and returns the average score of all graded submissions for a given student ID.

---

## 4. 1-Point Grading Plan

| Metric | 0.5 Points (Partial) | 1.0 Point (Full Mastery) |
|--------|----------------------|--------------------------|
| Popcorn Hack | Identifies if condition placement but contains syntax errors or logical bugs in role validation. | Correctly implements role checks with compound boolean logic (`&&`, <code>&#124;&#124;</code>). |
| Homework Hack | Encapsulates fields and iterates through ArrayList, but fails to restrict state transitions by role or compute averages correctly. | Accurately implements class encapsulation, role-based state mutators, and an ArrayList traversal that returns computed metrics. |

---

## 5. Peer Feedback & Revision Record

- **Initial Draft Feedback:** Peer noted that using an external `User` class added unnecessary setup overhead for a 15-minute concept lesson.
- **Revision Implemented:** Replaced complex `User` class parameters with lightweight `String userRole` and `String userId` method arguments to keep focus strictly on **Unit 3 encapsulation** and **Unit 4 ArrayList processing**.

---

## 6. Completed Hacks

My completed popcorn hack, MCQ, and homework hacks for the OCS version of this lesson are on the [Submission Roles Hacks]({{site.baseurl}}/sprint2/submission-roles/hacks) page.
