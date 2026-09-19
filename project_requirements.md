# NEXORA — COLLEGE MANAGEMENT PORTAL

> **One Portal. Three Roles. Connected Academic Operations.**

**Version:** 1.0
**Stage:** Requirements + Implementation
**Roles:** Admin · Faculty · Student

---

# 1. PROJECT OVERVIEW

NEXORA is a centralized, role-aware college management platform designed to manage academic, administrative, financial, examination, communication, documentation, analytics, and student-support operations through a single system.

NEXORA uses **Role-Based Access Control (RBAC)** to ensure that every user can access only the resources and operations permitted to their role.

```text
                    NEXORA
                       │
          ┌────────────┼────────────┐
          │            │            │
        ADMIN        FACULTY      STUDENT
          │            │            │
          └────────────┼────────────┘
                       │
                Authentication
                       │
                      RBAC
                       │
                  API Layer
                       │
               Business Logic
                       │
                  Database
                       │
          ┌────────────┼────────────┐
          │            │            │
       Analytics     Audit       Reports
```

---

# 2. PRIORITY LEVELS

| Priority  | Meaning                                          |
| --------- | ------------------------------------------------ |
| 🔴 High   | Core functionality required for portal operation |
| 🟡 Medium | Important supporting functionality               |
| 🟢 Low    | Optional / future functionality                  |

---

# 3. THREE SEPARATE LOGIN PORTALS

NEXORA must provide **three separate login pages**.

```text
NEXORA
│
├── Admin Portal
│   └── /admin/login
│
├── Faculty Portal
│   └── /faculty/login
│
└── Student Portal
    └── /student/login
```

A public portal-selection page may also be provided:

```text
/login
```

The page allows the user to choose:

```text
┌──────────────┐  ┌──────────────┐  ┌──────────────┐
│    ADMIN     │  │   FACULTY    │  │   STUDENT    │
│      🔐      │  │      👨‍🏫     │  │      🎓      │
│ [ Login ]    │  │ [ Login ]    │  │ [ Login ]    │
└──────────────┘  └──────────────┘  └──────────────┘
```

---

# 4. ADMIN LOGIN

## Route

```text
/admin/login
```

## Login Fields

```text
Admin ID / Email
Password
Remember Me
Forgot Password
Login
```

## Authentication Flow

```text
Admin
   ↓
/admin/login
   ↓
Enter Credentials
   ↓
Credential Validation
   ↓
Verify ADMIN Role
   ↓
Permission Validation
   ↓
Admin Dashboard
```

## Admin Dashboard

```text
/admin/dashboard
```

## Admin Modules

```text
User Management
Student Management
Faculty Management
Department Management
Course Management
Academic Calendar
Timetable
Attendance
Grades
Examinations
Fees
Documents
Grievances
Academic Flags
Notifications
Analytics
Compliance
Workflow Management
Audit Logs
Settings
```

---

# 5. FACULTY LOGIN

## Route

```text
/faculty/login
```

## Login Fields

```text
Faculty ID / Email
Password
Remember Me
Forgot Password
Login
```

## Authentication Flow

```text
Faculty
   ↓
/faculty/login
   ↓
Enter Credentials
   ↓
Credential Validation
   ↓
Verify FACULTY Role
   ↓
Permission Validation
   ↓
Faculty Dashboard
```

## Faculty Dashboard

```text
/faculty/dashboard
```

## Faculty Modules

```text
My Courses
Timetable
Attendance
Assignments
Quizzes
Grades
Academic Flags
Leave Requests
Course Proposals
Discussions
Messages
Notifications
Profile
```

---

# 6. STUDENT LOGIN

## Route

```text
/student/login
```

## Login Fields

```text
Student ID / Register Number
Password
Remember Me
Forgot Password
Login
```

## Authentication Flow

```text
Student
   ↓
/student/login
   ↓
Enter Credentials
   ↓
Credential Validation
   ↓
Verify STUDENT Role
   ↓
Permission Validation
   ↓
Student Dashboard
```

## Student Dashboard

```text
/student/dashboard
```

## Student Modules

```text
My Profile
My Courses
Timetable
Attendance
Assignments
Quizzes
Marks
Examinations
Fees
Documents
Grievances
Academic Flags
Messages
Notifications
Performance Analytics
```

---

# 7. LOGIN SECURITY

Although the login pages are separate, authentication should use a shared secure backend.

```text
Admin Login ───┐
Faculty Login ─┼──> Authentication Service
Student Login ─┘              │
                              ↓
                       Credential Check
                              │
                         Role Validation
                              │
                         Session Creation
```

Backend authentication endpoints:

```text
POST /api/auth/admin/login
POST /api/auth/faculty/login
POST /api/auth/student/login

POST /api/auth/admin/logout
POST /api/auth/faculty/logout
POST /api/auth/student/logout

POST /api/auth/admin/forgot-password
POST /api/auth/faculty/forgot-password
POST /api/auth/student/forgot-password
```

The backend must verify both:

```text
Valid Credentials
+
Expected Role
```

A Student account must not be able to authenticate through:

```text
/admin/login
/faculty/login
```

and similarly for the other roles.

---

# 8. PORTAL ROLES

## 8.1 ADMIN

Admin owns institution-level configuration, user lifecycle, academic structure, financial configuration, approvals, analytics, and compliance.

### Admin Responsibilities

```text
Manage users
Manage roles
Manage departments
Manage academic calendar
Manage courses
Configure fees
Assign rooms
Manage timetable
Publish announcements
Approve academic requests
Manage examinations
Monitor analytics
Manage compliance
Manage documents
Review grievances
Manage audit logs
```

### Admin Permissions

```text
CREATE
READ
UPDATE
DELETE / DEACTIVATE
APPROVE
CONFIGURE
PUBLISH
GENERATE REPORTS
```

---

# 9. FACULTY

Faculty members manage academic activities for courses assigned to them.

### Faculty Responsibilities

```text
Upload course materials
Record attendance
Create assignments
Create quizzes
Grade submissions
Submit marks
Raise academic flags
Post course announcements
Participate in discussions
Apply for leave
Request timetable changes
Propose courses
```

### Faculty Permissions

```text
READ ASSIGNED DATA
CREATE
UPDATE
UPLOAD
RECORD
GRADE
SUBMIT
RAISE
REQUEST APPROVAL
```

---

# 10. STUDENT

Students manage academic activities related to their own records and enrolled courses.

### Student Responsibilities

```text
Register for courses
View timetable
Track attendance
Submit assignments
View marks
Pay fees
Register for examinations
Download hall tickets
View performance analytics
Raise grievances
Request documents
Communicate with faculty
```

### Student Permissions

```text
VIEW PERSONAL DATA
REGISTER
SUBMIT
PAY
DOWNLOAD
TRACK
RAISE REQUESTS
PARTICIPATE
```

---

# 11. FUNCTIONAL REQUIREMENTS

Every requirement follows:

```text
Agent → Action → Entity
```

---

# 12. ADMIN REQUIREMENTS

| ID     | Requirement                                          | Relation                              | Priority |
| ------ | ---------------------------------------------------- | ------------------------------------- | -------- |
| ADM-01 | Create, update, deactivate and archive user accounts | Admin → Manages → User Account        | 🔴       |
| ADM-02 | Create and publish academic calendar                 | Admin → Publishes → Academic Calendar | 🔴       |
| ADM-03 | Create departments and assign department heads       | Admin → Configures → Department       | 🔴       |
| ADM-04 | Generate institution-wide analytics                  | Admin → Generates → Analytics Report  | 🟡       |
| ADM-05 | Configure fees, scholarships and payment status      | Admin → Configures → Fee Structure    | 🔴       |
| ADM-06 | Assign rooms and timetable slots                     | Admin → Assigns → Timetable / Room    | 🟡       |
| ADM-07 | Broadcast notices and targeted alerts                | Admin → Broadcasts → Notification     | 🟡       |
| ADM-08 | Approve or reject course proposals                   | Admin → Approves → Course Proposal    | 🟢       |
| ADM-09 | Manage compliance records                            | Admin → Tracks → Compliance Document  | 🟢       |
| ADM-10 | Manage examination schedules                         | Admin → Manages → Examination         | 🔴       |
| ADM-11 | Manage document requests                             | Admin → Approves → Document Request   | 🟡       |
| ADM-12 | Manage grievances                                    | Admin → Resolves → Grievance Ticket   | 🟡       |

---

# 13. FACULTY REQUIREMENTS

| ID     | Requirement                                | Relation                                    | Priority |
| ------ | ------------------------------------------ | ------------------------------------------- | -------- |
| FAC-01 | Upload lecture notes, slides and materials | Faculty → Uploads → Course Material         | 🔴       |
| FAC-02 | Record daily attendance                    | Faculty → Records → Attendance              | 🔴       |
| FAC-03 | Create assignments and quizzes             | Faculty → Creates → Assignment / Quiz       | 🔴       |
| FAC-04 | Enter and submit internal/final marks      | Faculty → Submits → Grade Record            | 🔴       |
| FAC-05 | Raise academic concern flags               | Faculty → Raises → Academic Flag            | 🟡       |
| FAC-06 | Post announcements and discussions         | Faculty → Posts → Announcement / Discussion | 🟡       |
| FAC-07 | Request timetable changes                  | Faculty → Requests → Schedule Change        | 🟢       |
| FAC-08 | Apply for leave                            | Faculty → Applies → Leave Request           | 🟡       |
| FAC-09 | Propose courses/syllabus modifications     | Faculty → Proposes → Course Proposal        | 🟢       |
| FAC-10 | Grade assignment submissions               | Faculty → Grades → Assignment Submission    | 🔴       |

---

# 14. STUDENT REQUIREMENTS

| ID     | Requirement                                 | Relation                                  | Priority |
| ------ | ------------------------------------------- | ----------------------------------------- | -------- |
| STU-01 | Register for available courses              | Student → Registers → Course              | 🔴       |
| STU-02 | View timetable and attendance               | Student → Views → Timetable / Attendance  | 🔴       |
| STU-03 | Submit assignments and view feedback        | Student → Submits → Assignment            | 🔴       |
| STU-04 | View marks and semester results             | Student → Views → Grade Card              | 🔴       |
| STU-05 | Pay fees and download receipts              | Student → Pays → Fee / Receipt            | 🔴       |
| STU-06 | Register for exams and download hall ticket | Student → Registers → Exam / Hall Ticket  | 🔴       |
| STU-07 | Track academic performance                  | Student → Tracks → Performance Analytics  | 🟡       |
| STU-08 | Raise and track grievances                  | Student → Raises → Grievance Ticket       | 🟡       |
| STU-09 | Request official documents                  | Student → Applies → Document Request      | 🟡       |
| STU-10 | Participate in course discussions           | Student → Participates → Discussion Board | 🟢       |

---

# 15. CORE ENTITIES

The main database entities are:

```text
User
StudentProfile
FacultyProfile
Department

AcademicYear
Semester
AcademicCalendar

Course
CourseEnrollment
CourseMaterial

Attendance
AttendanceSession

Assignment
AssignmentSubmission
Quiz
QuizQuestion
QuizAttempt

GradeRecord

Timetable
Room

Examination
ExamSchedule
ExamRegistration
HallTicket
ExamResult

FeeStructure
StudentFee
Payment
Receipt
Scholarship

AcademicFlag

GrievanceTicket

DocumentRequest
GeneratedDocument

Notification
Announcement
Message
DiscussionThread

CourseProposal
LeaveRequest
ScheduleChangeRequest

ComplianceRequirement
ComplianceDocument

Workflow
WorkflowInstance
WorkflowHistory

AuditLog
AnalyticsReport
```

---

# 16. DATA RELATIONSHIPS

```text
Department
 ├── Faculty
 └── Course

Faculty
 ├── Courses
 ├── Attendance
 ├── Assignments
 ├── Grades
 ├── Academic Flags
 ├── Leave Requests
 └── Course Proposals

Student
 ├── Course Enrollment
 ├── Attendance
 ├── Assignments
 ├── Grades
 ├── Fees
 ├── Exams
 ├── Documents
 ├── Grievances
 └── Academic Flags

Course
 ├── Faculty
 ├── Students
 ├── Assignments
 ├── Attendance
 ├── Grades
 └── Timetable
```

---

# 17. ENTITY DETAILS

## User

```text
userID
role
email
passwordHash
status
createdAt
lastLogin
```

Roles:

```text
ADMIN
FACULTY
STUDENT
```

## Department

```text
deptID
name
headFacultyID
budget
established
```

## Course

```text
courseCode
title
credits
syllabusURL
deptID
facultyID
```

## Attendance

```text
attendanceID
courseID
studentID
date
status
markedByFacultyID
```

Statuses:

```text
PRESENT
ABSENT
LATE
EXCUSED
```

## Assignment

```text
assignID
courseID
title
dueDate
rubricURL
maxMarks
```

## Grade Record

```text
gradeID
studentID
courseID
internalMarks
finalMarks
grade
semester
CGPA
```

## Fee Record

```text
feeID
studentID
amount
dueDate
paidDate
status
receiptURL
```

Statuses:

```text
PENDING
PAID
OVERDUE
PARTIAL
CANCELLED
```

---

# 18. APPROVAL WORKFLOW ENGINE

NEXORA should use a reusable workflow engine.

## Grade Submission

```text
Faculty
   ↓
Enter Marks
   ↓
Submit
   ↓
Admin Review
   ↓
Approved / Rejected
   ↓
Result Published
```

## Course Proposal

```text
Faculty
   ↓
Create Proposal
   ↓
Department Review
   ↓
Admin Review
   ↓
Approved / Rejected
   ↓
Course Created
```

## Leave

```text
Faculty
   ↓
Leave Request
   ↓
Admin Review
   ↓
Approved / Rejected
```

## Grievance

```text
Student
   ↓
Create Ticket
   ↓
Assignment
   ↓
Investigation
   ↓
Resolution
   ↓
Notification
   ↓
Closed
```

## Document

```text
Student
   ↓
Request
   ↓
Validation
   ↓
Approval
   ↓
Document Generation
   ↓
Verification
   ↓
Download
```

---

# 19. ACADEMIC FLAG SYSTEM

Faculty can raise controlled academic/support flags.

```text
Faculty
   ↓
Raise Flag
   ↓
Admin Review
   ↓
Assign Intervention
   ↓
Action Taken
   ↓
Resolution
```

Flag types:

```text
ATTENDANCE
ACADEMIC
PLAGIARISM
MISCONDUCT
WELFARE
PERFORMANCE
```

The system must record creation, review, updates and resolution.

---

# 20. ANALYTICS HUB

## Admin Analytics

```text
Total Students
Enrollment Trends
Attendance Statistics
Department Performance
Pass/Fail Statistics
Fee Collection
Open Grievances
Academic Flags
Course Performance
```

## Faculty Analytics

```text
Course Attendance
Assignment Completion
Average Marks
Grade Distribution
Student Performance
```

## Student Analytics

```text
CGPA
Semester Trend
Attendance
Assignment Completion
Internal Marks
Course Performance
```

Any predictive/risk analytics must be treated as decision-support information rather than a definitive classification.

---

# 21. DOCUMENT AUTOMATION

Supported documents:

```text
Hall Ticket
Bonafide Certificate
Grade Card
Transcript
Transfer Certificate
Course Completion Certificate
Payment Receipt
```

Workflow:

```text
Request
 ↓
Validation
 ↓
Approval
 ↓
Generation
 ↓
Digital Verification
 ↓
Download
 ↓
Archive
```

---

# 22. NOTIFICATION SYSTEM

Notification events:

```text
Assignment Published
Assignment Deadline Approaching
Attendance Warning
Exam Registration Open
Hall Ticket Available
Fee Due
Payment Successful
Grade Published
Course Registration Open
Grievance Updated
Document Approved
Leave Updated
Academic Flag Created
Institutional Announcement
```

Channels:

```text
IN_APP
EMAIL
SMS
PUSH
```

The first implementation should prioritize:

```text
IN_APP
EMAIL
```

---

# 23. MESSAGING

Communication channels:

```text
Admin → Faculty
Admin → Students
Faculty → Students
Faculty ↔ Students
Course → Discussion Board
```

Features:

```text
Direct Messaging
Threaded Discussions
Announcements
Attachments
Read/Unread
Search
```

---

# 24. RBAC PERMISSION MATRIX

| Module          | Admin            | Faculty              | Student              |
| --------------- | ---------------- | -------------------- | -------------------- |
| User Management | Manage           | ❌                    | ❌                    |
| Department      | Manage           | View                 | ❌                    |
| Courses         | Manage           | Assigned             | View/Register        |
| Attendance      | Manage           | Manage               | Own View             |
| Assignments     | View             | Manage               | Submit/View          |
| Grades          | Manage/Approve   | Create/Submit        | Own View             |
| Timetable       | Manage           | Own View             | Own View             |
| Fees            | Manage           | ❌                    | Own Pay/View         |
| Examinations    | Manage           | View                 | Register/View        |
| Academic Flags  | Manage           | Create/View          | Allowed View         |
| Grievances      | Manage           | Limited              | Create/View Own      |
| Documents       | Manage           | Limited              | Request/Download     |
| Notifications   | Broadcast        | Course               | Receive              |
| Analytics       | Institution      | Assigned             | Personal             |
| Compliance      | Manage           | Assigned View        | ❌                    |
| Messaging       | Broadcast/Direct | Course/Direct        | Course/Direct        |
| Audit           | Full             | Own relevant actions | Own relevant actions |

---

# 25. CORE BUSINESS RULES

## BR-01 Role Isolation

Users can access only resources permitted by their role.

## BR-02 Student Privacy

Students can access only their own academic, attendance, financial and document data.

## BR-03 Faculty Course Scope

Faculty can modify academic records only for assigned courses.

## BR-04 Grade Approval

Final grades must pass the configured approval process before publication.

## BR-05 Attendance Ownership

Only authorized faculty can create or modify attendance.

## BR-06 Payment Verification

A payment becomes `PAID` only after server-side verification.

## BR-07 Examination Eligibility

Exam registration must validate configured eligibility rules.

## BR-08 Document Approval

Restricted documents cannot be downloaded before approval.

## BR-09 Auditability

Sensitive operations must create audit records.

## BR-10 Workflow Integrity

Workflow entities must follow valid state transitions.

---

# 26. NON-FUNCTIONAL REQUIREMENTS

## Performance

```text
Efficient dashboards
Indexed queries
Pagination
Caching where required
Asynchronous report generation
```

## Security

```text
Secure password hashing
Protected sessions
RBAC
Input validation
API authorization
Secure file uploads
Audit logging
Secret management
```

## Availability

```text
Reliable access
Database backups
Recovery procedures
```

## Scalability

The system should support:

```text
Multiple Departments
Multiple Programmes
Multiple Semesters
Multiple Academic Years
Thousands of Students
Large Document Storage
Multiple Faculty Members
```

## Maintainability

```text
Modular architecture
Reusable components
Clear API contracts
Centralized validation
Centralized errors
Database migrations
Automated testing
```

---

# 27. RECOMMENDED TECHNOLOGY STACK

## Frontend

```text
Next.js
TypeScript
Tailwind CSS
shadcn/ui
React Hook Form
Zod
Recharts
```

## Backend

```text
Next.js API Routes / Server Actions
TypeScript
Prisma ORM
Zod
```

For larger deployments:

```text
Node.js / NestJS
REST API
```

## Database

```text
PostgreSQL
Prisma ORM
```

## File Storage

```text
Object/File Storage
```

---

# 28. PROJECT STRUCTURE

```text
nexora/
│
├── app/
│   ├── login/
│   │   └── page.tsx
│   │
│   ├── admin/
│   │   ├── login/
│   │   ├── dashboard/
│   │   ├── users/
│   │   ├── students/
│   │   ├── faculty/
│   │   ├── departments/
│   │   ├── courses/
│   │   ├── attendance/
│   │   ├── grades/
│   │   ├── exams/
│   │   ├── fees/
│   │   ├── documents/
│   │   ├── grievances/
│   │   ├── analytics/
│   │   ├── compliance/
│   │   └── audit/
│   │
│   ├── faculty/
│   │   ├── login/
│   │   ├── dashboard/
│   │   ├── courses/
│   │   ├── timetable/
│   │   ├── attendance/
│   │   ├── assignments/
│   │   ├── quizzes/
│   │   ├── grades/
│   │   ├── flags/
│   │   ├── leave/
│   │   └── discussions/
│   │
│   ├── student/
│   │   ├── login/
│   │   ├── dashboard/
│   │   ├── courses/
│   │   ├── timetable/
│   │   ├── attendance/
│   │   ├── assignments/
│   │   ├── quizzes/
│   │   ├── marks/
│   │   ├── exams/
│   │   ├── fees/
│   │   ├── documents/
│   │   ├── grievances/
│   │   └── messages/
│   │
│   └── api/
│
├── components/
│   ├── ui/
│   ├── forms/
│   ├── tables/
│   ├── charts/
│   ├── navigation/
│   └── dashboard/
│
├── modules/
│   ├── auth/
│   ├── users/
│   ├── courses/
│   ├── attendance/
│   ├── assignments/
│   ├── grades/
│   ├── exams/
│   ├── fees/
│   ├── documents/
│   ├── grievances/
│   ├── notifications/
│   ├── messaging/
│   ├── analytics/
│   ├── compliance/
│   ├── workflows/
│   └── audit/
│
├── lib/
│   ├── auth/
│   ├── database/
│   ├── permissions/
│   ├── validation/
│   └── notifications/
│
├── prisma/
│   ├── schema.prisma
│   ├── migrations/
│   └── seed.ts
│
├── tests/
├── middleware.ts
├── .env
├── .env.example
└── README.md
```

---

# 29. IMPLEMENTATION PHASES

## PHASE 0 — PROJECT SETUP

```text
Initialize Git repository
Create Next.js project
Configure TypeScript
Configure Tailwind
Configure shadcn/ui
Configure PostgreSQL
Configure Prisma
Configure ESLint
Configure Prettier
Configure environment variables
```

---

# 30. PHASE 1 — AUTHENTICATION + THREE LOGIN PORTALS

Build first:

```text
/login

/admin/login
/faculty/login
/student/login
```

Implement:

```text
Credential validation
Password hashing
Session management
Role verification
Route protection
Logout
Forgot password
Account activation/deactivation
Failed login monitoring
```

### Result

```text
Admin Login → Admin Dashboard
Faculty Login → Faculty Dashboard
Student Login → Student Dashboard
```

---

# 31. PHASE 2 — USER + ACADEMIC FOUNDATION

Implement:

```text
User Management
Student Management
Faculty Management
Department Management
Course Management
Course Enrollment
Academic Year
Semester
Academic Calendar
```

---

# 32. PHASE 3 — TIMETABLE + ATTENDANCE

Implement:

```text
Room Management
Timetable
Faculty Schedule
Student Schedule
Attendance Sessions
Attendance Records
Attendance Analytics
```

---

# 33. PHASE 4 — ASSIGNMENTS + QUIZZES + GRADES

Implement:

```text
Course Materials
Assignments
File Uploads
Assignment Submission
Quiz
Quiz Questions
Quiz Attempts
Grading
Grade Records
Grade Approval
```

---

# 34. PHASE 5 — EXAMINATION MANAGEMENT

Implement:

```text
Examination
Exam Schedule
Exam Registration
Eligibility
Hall Ticket
Exam Results
```

---

# 35. PHASE 6 — FEES

Implement:

```text
Fee Structure
Student Fees
Scholarships
Payments
Payment Verification
Receipts
Payment History
```

---

# 36. PHASE 7 — DOCUMENTS + GRIEVANCES

Implement:

```text
Document Requests
Document Generation
Digital Verification
Download
Grievance Tickets
Ticket Assignment
Resolution Workflow
```

---

# 37. PHASE 8 — NOTIFICATIONS + MESSAGING

Implement:

```text
Announcements
In-App Notifications
Email Notifications
Direct Messaging
Discussion Boards
Unread Counters
Message Search
```

---

# 38. PHASE 9 — ACADEMIC FLAGS + WORKFLOW ENGINE

Implement:

```text
Academic Flags
Workflow Definitions
Workflow Instances
Workflow History
Approval Transitions
Intervention Tracking
```

---

# 39. PHASE 10 — ANALYTICS + COMPLIANCE

Implement:

```text
Admin Analytics
Faculty Analytics
Student Analytics
Attendance Analytics
Result Analytics
Fee Analytics
Compliance Tracker
Document Vault
Deadline Tracking
```

---

# 40. PHASE 11 — AUDIT + REPORTING

Implement:

```text
Audit Logs
Activity History
CSV Reports
PDF Reports
Institutional Reports
Attendance Reports
Grade Reports
Fee Reports
Exam Reports
Compliance Reports
```

---

# 41. PHASE 12 — TESTING + SECURITY

## Unit Tests

Test:

```text
Validation
Permissions
Attendance calculations
Grade calculations
CGPA calculations
Fee calculations
Workflow transitions
```

## Integration Tests

Test:

```text
Authentication
API authorization
Database operations
Attendance
Assignments
Grades
Exams
Fees
Documents
Notifications
```

## End-to-End Tests

### Admin Flow

```text
Admin Login
 ↓
Create Faculty
 ↓
Create Course
 ↓
Assign Faculty
 ↓
Publish Course
```

### Faculty Flow

```text
Faculty Login
 ↓
Open Course
 ↓
Mark Attendance
 ↓
Create Assignment
 ↓
Grade Submission
 ↓
Submit Grades
```

### Student Flow

```text
Student Login
 ↓
Register Course
 ↓
View Timetable
 ↓
Submit Assignment
 ↓
View Attendance
 ↓
View Marks
 ↓
Pay Fee
 ↓
Register Exam
```

---

# 42. DASHBOARDS

## ADMIN DASHBOARD

```text
Total Students
Total Faculty
Departments
Active Courses
Attendance Overview
Result Analytics
Fee Collection
Pending Approvals
Open Grievances
Academic Flags
Compliance Alerts
Announcements
```

## FACULTY DASHBOARD

```text
Assigned Courses
Today's Timetable
Attendance
Assignments
Pending Evaluations
Grade Submission
Academic Flags
Leave Requests
Course Discussions
Notifications
```

## STUDENT DASHBOARD

```text
Profile
Current Semester
Timetable
Attendance
Assignments
Internal Marks
CGPA
Examination Registration
Fees
Academic Flags
Grievances
Documents
Notifications
```

---

# 43. API ARCHITECTURE

## Authentication

```text
POST /api/auth/admin/login
POST /api/auth/faculty/login
POST /api/auth/student/login
POST /api/auth/logout
GET  /api/auth/me
```

## Users

```text
GET    /api/users
POST   /api/users
GET    /api/users/:id
PATCH  /api/users/:id
DELETE /api/users/:id
```

## Courses

```text
GET    /api/courses
POST   /api/courses
GET    /api/courses/:id
PATCH  /api/courses/:id
DELETE /api/courses/:id
```

## Attendance

```text
GET   /api/attendance
POST  /api/attendance
PATCH /api/attendance/:id
```

## Assignments

```text
GET  /api/assignments
POST /api/assignments
POST /api/assignments/:id/submit
POST /api/assignments/:id/grade
```

## Grades

```text
GET  /api/grades
POST /api/grades
POST /api/grades/:id/submit
POST /api/grades/:id/approve
POST /api/grades/:id/reject
```

Additional APIs should follow the same structure for:

```text
Exams
Fees
Documents
Grievances
Notifications
Messages
Workflows
Analytics
Compliance
Audit
```

---

# 44. AUTHORIZATION FLOW

Every protected operation must follow:

```text
Request
   ↓
Authenticated?
   ↓
Expected Role?
   ↓
Resource Ownership?
   ↓
Permission Check?
   ↓
Business Rule Validation?
   ↓
Database Operation
   ↓
Audit Log
```

Frontend route protection alone is not sufficient.

---

# 45. SEARCH AND FILTERING

Searchable entities:

```text
Users
Students
Faculty
Courses
Assignments
Attendance
Grades
Fees
Grievances
Documents
Notifications
```

Filters:

```text
Department
Programme
Semester
Academic Year
Role
Status
Date Range
Course
Faculty
Student
```

Use:

```text
Pagination
Sorting
Filtering
Indexed Queries
```

---

# 46. AUDIT TRAIL

Record sensitive actions:

```text
CREATE
UPDATE
DELETE
LOGIN
LOGOUT
APPROVE
REJECT
PUBLISH
DOWNLOAD
PAYMENT
GRADE_CHANGE
ATTENDANCE_CHANGE
ROLE_CHANGE
```

Audit record:

```text
auditID
userID
action
entityType
entityID
oldValue
newValue
timestamp
ipAddress
userAgent
```

---

# 47. FILE STORAGE

Files should be stored in object/file storage, not directly inside database records.

```text
Database
   ↓
File Metadata
   ↓
Object Storage
   ↓
Secure File URL
```

Validate:

```text
File Type
File Size
Upload Permission
File Name
```

---

# 48. DEPLOYMENT ARCHITECTURE

## Development

```text
VS Code
   ↓
Local Next.js
   ↓
Local/PostgreSQL
   ↓
Git
   ↓
GitHub
```

## Production

```text
User
 ↓
Web Hosting
 ↓
Next.js
 ↓
PostgreSQL
 ↓
Object Storage
 ↓
Notification Services
```

CI/CD:

```text
Git Push
 ↓
Lint
 ↓
Type Check
 ↓
Tests
 ↓
Build
 ↓
Deploy
```

---

# 49. ENVIRONMENT VARIABLES

Use:

```text
.env
.env.example
```

Example:

```text
DATABASE_URL=
AUTH_SECRET=
NEXT_PUBLIC_APP_URL=
FILE_STORAGE_URL=
EMAIL_SERVER=
PAYMENT_GATEWAY_KEY=
```

Never commit production secrets to GitHub.

---

# 50. RECOMMENDED DEVELOPMENT ORDER

```text
1. Project Setup
2. Database Setup
3. Three Login Pages
4. Authentication
5. RBAC
6. Admin Dashboard
7. Faculty Dashboard
8. Student Dashboard
9. User Management
10. Student Management
11. Faculty Management
12. Department Management
13. Course Management
14. Enrollment
15. Academic Calendar
16. Timetable
17. Attendance
18. Course Materials
19. Assignments
20. Assignment Submission
21. Quizzes
22. Grades
23. Grade Approval
24. Examinations
25. Fees
26. Payments
27. Documents
28. Grievances
29. Academic Flags
30. Notifications
31. Messaging
32. Discussion Boards
33. Workflow Engine
34. Analytics
35. Compliance
36. Audit Trail
37. Reports
38. Security Testing
39. Performance Testing
40. Deployment
```

---

# 51. MVP VERSION

The first working release should contain:

```text
AUTHENTICATION
├── Admin Login
├── Faculty Login
└── Student Login

ADMIN
├── Users
├── Departments
├── Courses
├── Timetable
├── Attendance
├── Grades
└── Dashboard

FACULTY
├── Courses
├── Timetable
├── Attendance
├── Assignments
├── Grades
└── Dashboard

STUDENT
├── Courses
├── Timetable
├── Attendance
├── Assignments
├── Marks
└── Dashboard
```

Once this works reliably, build the advanced modules.

---

# 52. DEVELOPMENT MILESTONES

## Milestone 1 — Foundation

```text
✓ Project setup
✓ Database
✓ Three login pages
✓ Authentication
✓ RBAC
✓ Dashboards
```

## Milestone 2 — Academic Core

```text
✓ Departments
✓ Courses
✓ Enrollment
✓ Timetable
✓ Attendance
```

## Milestone 3 — Assessment

```text
✓ Assignments
✓ Submissions
✓ Quizzes
✓ Grades
✓ Approval Workflow
```

## Milestone 4 — Operations

```text
✓ Examinations
✓ Fees
✓ Payments
✓ Documents
✓ Grievances
```

## Milestone 5 — Communication

```text
✓ Notifications
✓ Announcements
✓ Messaging
✓ Discussions
```

## Milestone 6 — Intelligence

```text
✓ Academic Flags
✓ Analytics
✓ Compliance
✓ Reports
✓ Audit Trail
```

## Milestone 7 — Production

```text
✓ Security testing
✓ Performance testing
✓ Bug fixing
✓ Deployment
✓ Backup
✓ Monitoring
```

---

# 53. DEFINITION OF DONE

A feature is complete only when:

```text
UI Completed
     ↓
Database Completed
     ↓
API Completed
     ↓
Validation Completed
     ↓
Authorization Completed
     ↓
Business Rules Completed
     ↓
Error Handling Completed
     ↓
Audit Logging Completed
     ↓
Unit Tests Completed
     ↓
Integration Tests Completed
     ↓
Responsive UI Verified
     ↓
Documentation Updated
```

---

# 54. COMPLETE FINAL ARCHITECTURE

```text
                         ┌──────────────────────┐
                         │       NEXORA         │
                         │ College Management    │
                         │       Portal         │
                         └──────────┬───────────┘
                                    │
                             Portal Selection
                                    │
                ┌───────────────────┼───────────────────┐
                │                   │                   │
                ▼                   ▼                   ▼
        ┌──────────────┐    ┌──────────────┐    ┌──────────────┐
        │ ADMIN LOGIN  │    │FACULTY LOGIN │    │STUDENT LOGIN │
        │/admin/login  │    │/faculty/login│    │/student/login│
        └──────┬───────┘    └──────┬───────┘    └──────┬───────┘
               │                   │                   │
               ▼                   ▼                   ▼
        Admin Dashboard     Faculty Dashboard    Student Dashboard
               │                   │                   │
               └───────────────────┼───────────────────┘
                                   ▼
                         ┌────────────────────┐
                         │ Authentication +   │
                         │ RBAC Middleware    │
                         └─────────┬──────────┘
                                   ▼
                         ┌────────────────────┐
                         │ API / Business     │
                         │ Logic Layer        │
                         └─────────┬──────────┘
                                   │
       ┌───────────────────────────┼───────────────────────────┐
       │                           │                           │
       ▼                           ▼                           ▼
┌───────────────┐          ┌────────────────┐          ┌────────────────┐
│   Academic    │          │ Administration │          │ Communication  │
│   Modules     │          │    Modules     │          │    Modules     │
├───────────────┤          ├────────────────┤          ├────────────────┤
│ Courses       │          │ Users          │          │ Notifications  │
│ Attendance    │          │ Fees           │          │ Messaging      │
│ Assignments   │          │ Documents      │          │ Discussions    │
│ Grades        │          │ Grievances     │          │ Announcements  │
│ Exams         │          │ Compliance     │          └────────────────┘
│ Timetable     │          │ Workflows      │
└───────┬───────┘          └───────┬────────┘
        │                          │
        └──────────────────────────┼──────────────────────────┐
                                   ▼                          │
                          ┌────────────────────┐              │
                          │    PostgreSQL      │              │
                          │      Database      │              │
                          └─────────┬──────────┘              │
                                    │                         │
                    ┌───────────────┼───────────────┐         │
                    ▼               ▼               ▼         │
             ┌───────────┐   ┌────────────┐   ┌───────────┐ │
             │   Files   │   │   Audit    │   │ Analytics │ │
             │  Storage  │   │   Logs     │   │  Engine   │ │
             └───────────┘   └────────────┘   └─────┬─────┘ │
                                                     │       │
                                                     ▼       │
                                                ┌─────────┐  │
                                                │ Reports │  │
                                                └─────────┘  │
                                                            │
                         └──────────────────────────────────┘
```

---

# 55. FINAL PROJECT SUCCESS CRITERIA

NEXORA is considered functionally complete when:

```text
✓ Three separate login pages are available
✓ Admin authentication works
✓ Faculty authentication works
✓ Student authentication works
✓ RBAC is enforced
✓ Users can access only their authorized portals
✓ Admin can manage institutional configuration
✓ Faculty can manage assigned academic activities
✓ Students can complete core academic workflows
✓ Course and enrollment workflows work
✓ Attendance works
✓ Assignment workflows work
✓ Grade workflows work
✓ Examination workflows work
✓ Fee workflows work
✓ Document workflows work
✓ Grievances work
✓ Notifications work
✓ Approval workflows work
✓ Analytics dashboards work
✓ Audit logging works
✓ Security controls are implemented
✓ Automated tests cover critical workflows
✓ Application is deployable
```

---

# 56. FINAL NEXORA IMPLEMENTATION PRINCIPLE

```text
SEPARATE LOGIN PORTALS
          ↓
SECURE AUTHENTICATION
          ↓
ROLE-BASED ACCESS CONTROL
          ↓
ACADEMIC FOUNDATION
          ↓
ATTENDANCE + ASSIGNMENTS + GRADES
          ↓
EXAM + FEES + DOCUMENTS
          ↓
GRIEVANCES + COMMUNICATION
          ↓
WORKFLOW AUTOMATION
          ↓
ANALYTICS + COMPLIANCE
          ↓
AUDIT + REPORTING
          ↓
TESTING + SECURITY
          ↓
DEPLOYMENT
```

# NEXORA

### One Portal. Three Roles. Connected Academic Operations.

**End of Requirements and Implementation Document**
