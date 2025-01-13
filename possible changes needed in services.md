1. API Gateway
    - **Encryption**: Ensure all API traffic is encrypted using TLS 1.3.
    - **Access Control**: Implement role-based access controls (RBAC) to allow only authorized requests to pass through.
    - **Audit Trails**: Maintain logs of all API requests, including timestamps and user details, for auditing purposes.
    - **Rate Limiting:** Protect against DDoS attacks by limiting request rates per user or device.

2. Authentication Service
    - Consent Management: Enable storage of user consent records for data access and processing.
    - Compliance with User Rights:
        - Provide mechanisms to delete user accounts and associated data upon request.
        - Allow users to review and manage their consent settings.

3. Patient Service
    - Data Minimization: Collect only the data necessary for delivering healthcare services.
    - Data Anonymization: Use anonymized or de-identified data for analytics and research.
    - Access Control:
        - Allow patients to manage who can access their data.
        - Provide emergency access to critical data such as allergies and medication history.

4. Doctor Service
     - Secure Data Sharing:
        - Enable doctors to access patient records only with patient consent.
        - Ensure secure transfer of X-rays, prescriptions, and other medical data.
    - Notification of Breaches: Notify doctors immediately in case of a data breach affecting their records or patient information.
    - Auditability: Track all interactions with patient data by doctors. 
5. Paramedic Service
    - Proxy Consent: Allow collection of patient data through proxy consent when patients are incapacitated.
    - Secure Uploads: Ensure secure and encrypted uploading of diagnostic data such as X-rays or test results.
    - Access Restriction: Restrict paramedic access to only the data required for their specific tasks.

6. Teleconference Service
    - End-to-End Encryption: Use E2EE for all video and audio communications.
    - Data Storage:
        - Do not store call recordings unless explicitly permitted by both doctor and patient.
        - Store minimal metadata (e.g., session timestamps) for compliance and billing purposes.
    - Access Logs: Maintain logs of participants and session details for auditing.

7. X-ray Service
    - Encryption and Storage:
        - Store X-ray images securely using encrypted file systems.
        - Use access controls to ensure only authorized users can view or download X-rays.
    - Metadata Management: Ensure X-ray metadata (e.g., patient details, timestamps) is linked securely and complies with data minimization principles.
    - Anonymization for Research: Anonymize X-ray data when used for research or analytics.

8. Notification Service
    - Secure Notifications: Use secure channels (e.g., FCM with encryption) for sending notifications about consultations or emergencies.
    - Opt-In Mechanisms: Allow users to opt in or out of different notification categories (e.g., reminders, emergency alerts).
    - Data Protection: Do not include sensitive health data in notification messages.

9. Admin Service
    - User Activity Monitoring:
        - Log all administrative actions, including user management and data access.
        - Implement alerts for unauthorized activities.
    - Policy Enforcement:
        - Enforce policies for data storage, access, and transmission across all services.
        - Regularly review and update policies to align with DISHA.
    - Data Access Restrictions: Provide admins access only to anonymized or de-identified data unless explicitly permitted.

10. Emergency Request Service
    - Location Data Protection: Encrypt and minimize patient location data used for emergency requests.
    - Immediate Access:
        - Ensure emergency responders have immediate access to critical patient data (e.g., allergies, medications) without compromising privacy.
    - Logs: Maintain logs of emergency requests and actions taken.