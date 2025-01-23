### **Database Design for Role-Based Access Control (RBAC)**

RBAC ensures users have access only to the functionalities permitted by their roles. Below is the database schema:

#### **Tables and Relationships**
1. **Users Table**:
    - Stores basic user details.

| **Field**     | **Type**     | **Description**                 |
| ------------- | ------------ | ------------------------------- |
| user_id       | UUID         | Unique user identifier.         |
| email         | VARCHAR(255) | User email (unique).            |
| password_hash | VARCHAR(255) | Hashed password.                |
| phone_number  | VARCHAR(15)  | User's phone number (optional). |
| mfa_enabled   | BOOLEAN      | Indicates if MFA is enabled.    |
| created_at    | TIMESTAMP    | Account creation timestamp.     |

---

2. **Roles Table**:
    - Defines user roles (e.g., Admin, Doctor, Patient).

| **Field** | **Type**    | **Description**          |
| --------- | ----------- | ------------------------ |
| role_id   | UUID        | Unique role identifier.  |
| role_name | VARCHAR(50) | Role name (e.g., Admin). |

---

3. **Permissions Table**:
    - Lists system-level permissions.

| **Field**       | **Type**     | **Description**               |
| --------------- | ------------ | ----------------------------- |
| permission_id   | UUID         | Unique permission identifier. |
| permission_name | VARCHAR(100) | Name of the permission.       |

---

4. **User_Roles Table**:
    - Links users with roles.

| **Field** | **Type** | **Description**             |
| --------- | -------- | --------------------------- |
| user_id   | UUID     | References `Users.user_id`. |
| role_id   | UUID     | References `Roles.role_id`. |

---

5. **Role_Permissions Table**:
    - Links roles with permissions.

| **Field**     | **Type** | **Description**                         |
| ------------- | -------- | --------------------------------------- |
| role_id       | UUID     | References `Roles.role_id`.             |
| permission_id | UUID     | References `Permissions.permission_id`. |
