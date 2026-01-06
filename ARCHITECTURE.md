**Architecture**

```
ARCHITECTURE.md
```

This is **NOT README content** — it is a **standalone architecture document**.


#  ALIGN – Detailed System Architecture

## 1. Introduction

ALIGN is an **AI-based Alumni–Student Mentorship Platform** designed to bridge the gap between students seeking guidance and alumni willing to mentor. The system leverages **Google AppSheet** as a low-code application platform and **Google Sheets** as the backend database to deliver a scalable, secure, and role-driven solution within a short development cycle.

The architecture emphasizes:

* Role-based access control
* Intelligent mentor matching
* Secure cloud-native design
* Rapid development suitable for hackathons and academic projects


## 2. Architectural Objectives

The primary objectives of the ALIGN architecture are:

* To provide **secure authentication** using Google Sign-In
* To support **distinct user roles** (Students and Alumni)
* To implement **intelligent mentorship matching**
* To ensure **data isolation and privacy**
* To maintain **scalability and extensibility**
* To minimize infrastructure and operational complexity


## 3. Overall Architecture Overview

ALIGN follows a **layered cloud architecture**, where responsibilities are clearly separated across multiple layers.

### High-Level Flow

```
End Users (Students / Alumni)
        ↓
Google Authentication (USEREMAIL)
        ↓
AppSheet Application Layer
        ↓
Business Logic & Matching Engine
        ↓
Google Sheets Database
```

Each layer is independently manageable and collectively ensures smooth system operation.


## 4. Architecture Layers

### 4.1 User Layer

The User Layer consists of two primary user groups:

#### a) Students

* Register and manage their profiles
* Browse available alumni mentors
* Send mentorship requests
* Track request status (Pending / Accepted / Rejected)
* Communicate with mentors via the platform

#### b) Alumni

* Register and maintain professional profiles
* View incoming mentorship requests
* Accept or reject student requests
* Provide guidance and mentorship

User identity is authenticated using **Google Sign-In**, ensuring:

* No manual credential management
* Secure access
* Reduced onboarding friction


### 4.2 Authentication & Identity Management

Authentication is handled implicitly through Google AppSheet using:

* `USEREMAIL()` function
* Google account-based login

Role identification is performed by checking:

* Whether the logged-in email exists in the **Students** table
* Or exists in the **Alumni** table

This approach eliminates the need for a separate authentication server while maintaining strong identity assurance.


### 4.3 Application Layer (Google AppSheet)

The Application Layer is the core of ALIGN and is implemented entirely using **Google AppSheet**.

Responsibilities include:

* User interface rendering
* Navigation and routing
* Form handling and validation
* Workflow execution
* Action triggers (Send Request, Accept, Reject)

#### Key Application Modules

* **My Info**
  Displays user-specific profile details dynamically.

* **Find Mentors**
  Allows students to browse and filter alumni mentors.

* **Mentorship Requests**
  Displays all mentorship requests relevant to the logged-in user.

* **Incoming Requests (Alumni)**
  Enables alumni to review and respond to student requests.

* **Status Tracking**
  Allows students to track the progress of their requests.

The UI is responsive and supports both **web and mobile access**.


### 4.4 Business Logic & Matching Layer

The matching logic in ALIGN is implemented using **rule-based intelligence** within AppSheet expressions.

#### Matching Parameters

**Student Attributes**

* Interests
* Skills
* Help Needed

**Alumni Attributes**

* Skills
* Industry
* Job Role
* Experience

#### Matching Strategy

* Alumni are filtered based on relevance to student needs
* AppSheet expressions dynamically control visibility
* Matching logic is implemented using:

  * Ref relationships
  * Conditional expressions
  * Filtered views and slices

Although rule-based, this design allows future integration of:

* Machine Learning recommendation models
* Weighted scoring systems
* AI-driven mentor ranking


### 4.5 Data Layer (Google Sheets)

Google Sheets acts as a **relational backend database**.

#### Core Tables

1. **Students**

   * Student_ID
   * Name
   * Email
   * Year
   * Interests
   * Skills
   * Help Needed

2. **Alumni**

   * Alumni_ID (Primary Key)
   * Name
   * Email
   * Company
   * Role
   * Skills
   * Industry

3. **Match_Requests**

   * Match_ID (Primary Key)
   * Student_ID (Email)
   * Alumni_ID (Ref → Alumni)
   * Status

#### Data Relationships

* `Match_Requests[Alumni_ID]` → Ref → `Alumni`
* Student identity linked via email

This relational approach ensures:

* Data integrity
* Automatic data fetching
* Reduced duplication
* Clean normalization


### 4.6 Security & Access Control Layer

Security is enforced using **AppSheet-native mechanisms**.

#### Access Control Techniques

* USEREMAIL-based filtering
* Role-based slices
* Conditional view visibility
* Ref-based data access

#### Security Rules

* Students can view only:

  * Their own profile
  * Their own mentorship requests

* Alumni can view only:

  * Requests addressed to them
  * Their own profile

This ensures **strict data isolation** and prevents unauthorized access.


## 5. Workflow Execution

### Student Workflow

1. Login using Google account
2. View personal profile
3. Browse mentors
4. Send mentorship request
5. Track request status

### Alumni Workflow

1. Login using Google account
2. View incoming requests
3. Accept or reject requests
4. Status updates reflected instantly

All workflows are handled via **AppSheet Actions**.


## 6. Scalability & Extensibility

The architecture supports future enhancements such as:

* ML-based recommendation engines
* Chat analytics
* Performance dashboards
* Resume and portfolio sharing
* Migration to Firebase or custom backend
* Mobile-first deployment


## 7. Design Benefits

* Cloud-native and scalable
* Secure and role-driven
* Rapid development cycle
* Minimal infrastructure overhead
* Hackathon and academic ready
* Future-proof design


## 8. Conclusion

The ALIGN system architecture successfully combines **low-code development**, **intelligent matching**, and **secure access control** to deliver a robust mentorship platform. The modular and scalable design ensures that the system can evolve into a full-fledged AI-driven solution with minimal re-engineering.

