### ** for Doctor, Patient, and Paramedic**

### **1. Common Registration Workflow**
**Steps**:
1. **Initiate Registration**:
    - User provides basic information (e.g., email, phone, role, password).
2. **Validate Input**:
    - Ensure all fields are valid (e.g., valid email format, password complexity).
3. **Role-Specific Validation**:
    - Perform additional validations based on the user role (e.g., medical license for doctors).
4. **Save User Information**:
    - Hash the password and store user details in the database.
5. **Send Confirmation Email/OTP**:
    - Generate a confirmation token or OTP and send it to the user.
6. **Activate Account**:
    - Upon successful email/OTP verification, activate the user account.

---

### **2. Role-Specific Variations**

#### **Doctor Registration Workflow**

1. **Additional Inputs**:
    - Collect details such as specialization, medical license number, and practicing address.
2. **License Verification**:
    - Validate the provided medical license number using an external API or manual review.
3. **Role Assignment**:
    - Assign the `Doctor` role in the RBAC system.
4. **Approval Process**:
    - Optionally, mark the account as `Pending Approval` until verified by an admin.

---

#### **Patient Registration Workflow**

1. **Basic Information**:
    - Collect personal details such as name, date of birth, email, and phone number.
2. **Role Assignment**:
    - Automatically assign the `Patient` role.
3. **Health Profile** (Optional):
    - Prompt for optional health details (e.g., allergies, chronic conditions) after account activation.

---

#### **Paramedic Registration Workflow**

1. **Additional Inputs**:
    - Collect professional details such as paramedic ID, certifications, and organization name.
2. **ID Verification**:
    - Validate the paramedic ID through an external API or manual review.
3. **Role Assignment**:
    - Assign the `Paramedic` role in the RBAC system.
4. **Approval Process**:
    - Mark the account as `Pending Approval` until verified by an admin.

---

### **3. Registration Workflow Control Flow**

**UML Activity Diagram: Registration Workflow**
```uml
@startuml
start
:User provides registration details;
if (Role == Doctor?) then (yes)
  :Collect specialization, license number;
  :Verify medical license;
  if (License valid?) then (yes)
    :Assign Doctor role;
  else (no)
    :Reject registration;
    stop
  endif
else if (Role == Paramedic?) then (yes)
  :Collect paramedic ID and certifications;
  :Verify paramedic ID;
  if (ID valid?) then (yes)
    :Assign Paramedic role;
  else (no)
    :Reject registration;
    stop
  endif
else if (Role == Patient?) then (yes)
  :Collect personal details;
  :Assign Patient role;
endif
:Hash and store password;
:Send confirmation email/OTP;
if (User verifies email/OTP?) then (yes)
  :Activate account;
else (no)
  :Keep account inactive;
endif
stop
@enduml
```

