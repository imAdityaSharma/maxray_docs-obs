### **Authentication Service: Design, Responsibilities, and Implementation**

The **Authentication Service** is a critical microservice that handles user authentication, session management, and access control for the MaxRay platform. It ensures secure access to the system by validating credentials, issuing tokens, and managing user roles.

---

### **Responsibilities**

1. **User Authentication**:
    - Validates user credentials (email/password) during login.
    - Supports Single Sign-On (SSO) and OAuth2.0 for secure authentication.
2. **Token Management**:
    - Issues **JWT (JSON Web Tokens)** upon successful authentication.
    - Handles token validation and expiration checks.
    - Supports refresh tokens for extending user sessions.
3. **Role-Based Access Control (RBAC)**:
    - Associates users with specific roles (Patient, Doctor, Paramedic, Admin).
    - Validates user permissions based on their role.
4. **Password Management**:
    - Implements secure password hashing using algorithms like **bcrypt**.
    - Provides password reset functionality via email or OTP.
5. **Multi-Factor Authentication (MFA)**:
    - Optionally enforces MFA for high-privilege users (e.g., Admins, Doctors).
6. **Audit Logging**:
    - Tracks login attempts, token generation, and failed authentication events.
---
### **Secondary Responsibilities
1. **Session Management**:
    - Allow users to view and terminate active sessions.
2. **Account Lockout**:
    - Lock accounts temporarily after multiple failed login attempts to prevent brute-force attacks.
3. **Consent Management**:
    - Capture and store user consent for data access and processing.
4. **Device and Location Tracking**:
    - Log and monitor user login activities based on device and location.
5. **Audit Logging**:
    - Log all authentication events (e.g., logins, logouts, MFA) for traceability and compliance.
6. **Integration with Third-Party Identity Providers**:
    - Support integration with identity providers (e.g., Google, Azure AD).
### **Technology Stack**
- **Programming Language**: Python Flask.
- **Database**: PostgreSQL for user credentials and roles.
- **Token Management**: JWT for session handling.
- **Password Hashing**: bcrypt or Argon2 for secure storage.
- **Caching**: Redis for managing blacklisted tokens (e.g., during logout).
- **Email Service**: SMTP or a third-party service like SendGrid for password reset emails.

---
### ** Interactive Flow for Authentication Service**
**UML Use Case Diagram: Authentication Service Interactions**
```UML
@startuml
actor User
actor Admin

User --> (Register Account)
User --> (Login)
User --> (Reset Password)
User --> (Enable Multi-Factor Authentication)
User --> (Manage Active Sessions)

Admin --> (Audit Login Activities)
Admin --> (Manage Roles and Permissions)
@enduml
```

### **High-Level Architecture**
``` css
[Client Devices (Mobile/Web)]
   |
   v
[API Gateway] --> [Authentication Service]
                     |
                     |---> [PostgreSQL (User Data)]
                     |---> [Redis (Token Blacklist)]
                     |---> [SMTP (Email Notifications)]

```
**Service Structure**
``` css
authentication-service/
├── app.py
├── models/
│   └── user.py
├── routes/
│   └── auth.py
├── utils/
│   ├── hash.py
│   ├── jwt.py
│   └── email.py
├── requirements.txt
└── Dockerfile
```

### **API Endpoints**

|**Method**|**Endpoint**|**Description**|
|---|---|---|
|POST|`/auth/login`|Authenticate user and issue JWT.|
|POST|`/auth/logout`|Blacklist token and log user out.|
|POST|`/auth/refresh`|Issue a new JWT using a refresh token.|
|POST|`/auth/reset-password`|Send password reset email/OTP.|
|PUT|`/auth/update-password`|Update user password.|
|GET|`/auth/validate`|Validate JWT and return user info.|
### **Traceability for Requirements**

| **Req ID** | **Description**                                   | **Traceability**       | **Test ID** | **Verification & Validation**                        |
| ---------- | ------------------------------------------------- | ---------------------- | ----------- | ---------------------------------------------------- |
| RQ-AU001   | Users must securely register.                     | Linked to SYS.2, SWE.1 | TST-AU001   | Verify registration process and validation.          |
| RQ-AU002   | Users must log in using secure credentials.       | Linked to SYS.3, SWE.4 | TST-AU002   | Test login with valid and invalid credentials.       |
| RQ-AU003   | Role-based access must control user permissions.  | Linked to SYS.5, SWE.2 | TST-AU003   | Ensure RBAC enforcement for different roles.         |
| RQ-AU004   | Authentication tokens must be secure and valid.   | Linked to SYS.3, SWE.4 | TST-AU004   | Validate token creation, expiration, and revocation. |
| RQ-AU005   | Multi-factor authentication must be supported.    | Linked to SYS.3, SWE.2 | TST-AU005   | Test MFA setup and validation processes.             |
| RQ-AU006   | Account lockout must prevent brute-force attacks. | Linked to SYS.3, SWE.4 | TST-AU006   | Verify lockout after consecutive failed logins.      |
| RQ-AU007   | Log all authentication-related activities.        | Linked to SYS.3, SWE.4 | TST-AU007   | Ensure complete and immutable logging.               |

---

### **5. Feature Testing and Test Cases**

#### **Feature 1: User Registration**

**Test Cases**:

|**Test ID**|**Test Case Description**|**Expected Result**|
|---|---|---|
|TST-AU001-1|Register with valid credentials.|Registration is successful.|
|TST-AU001-2|Register with already existing email.|Registration fails with error message.|
|TST-AU001-3|Register with weak password.|Registration fails with password policy error.|

---

#### **Feature 2: User Login**

**Test Cases**:

|**Test ID**|**Test Case Description**|**Expected Result**|
|---|---|---|
|TST-AU002-1|Login with valid credentials.|Login is successful.|
|TST-AU002-2|Login with incorrect password.|Login fails with error message.|
|TST-AU002-3|Login after account lockout.|Login is denied with lockout message.|

---

#### **Feature 3: Token-Based Authentication**

**Test Cases**:

|**Test ID**|**Test Case Description**|**Expected Result**|
|---|---|---|
|TST-AU004-1|Generate token after successful login.|Token is created and returned.|
|TST-AU004-2|Access resource with expired token.|Access is denied with token expiry error.|
|TST-AU004-3|Revoke token and attempt to reuse.|Access is denied for revoked token.|

---

#### **Feature 4: Multi-Factor Authentication**

**Test Cases**:

|**Test ID**|**Test Case Description**|**Expected Result**|
|---|---|---|
|TST-AU005-1|Set up MFA using an authenticator app.|MFA setup is successful.|
|TST-AU005-2|Log in with correct MFA code.|Login is successful.|
|TST-AU005-3|Log in with incorrect MFA code.|Login fails with MFA error.|

---

#### **Feature 5: Account Lockout**

**Test Cases**:

|**Test ID**|**Test Case Description**|**Expected Result**|
|---|---|---|
|TST-AU006-1|Fail login 5 consecutive times.|Account is locked.|
|TST-AU006-2|Attempt login after lockout duration expires.|Login is successful.|

---

### **6. Functional Safety Considerations**

1. **Token Security**:
    - Use short-lived tokens and refresh tokens for session management.
    - Testing: Ensure tokens are encrypted and expire as expected.
2. **Session Termination**:
    - Allow users to terminate active sessions from any device.
    - Testing: Validate session termination and token invalidation workflows.
3. **Audit Logging**:
    - Log all authentication events, including successful and failed logins, password resets, and MFA attempts.
    - Testing: Ensure logs are complete, immutable, and accessible for audits.
4. **Brute-Force Protection**:
    - Implement IP blocking and account lockout mechanisms for repeated failed login attempts.
    - Testing: Simulate brute-force attacks and validate countermeasures.
---
