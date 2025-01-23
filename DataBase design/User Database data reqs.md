
## **Base user**
* **Demographic information**
	+ Name (first name, last name)
	+ Date of birth
	+ Address
		+ house no.
		+ apartment
		+ colony
		+ city
		+ pin/zip code
		+ state
	+ Contact number
		+ primary
		+ recovery
	+ Email
		+ primary
		+ recovery
	+ photo id
		+ AADHAR /social security no.
		+ latest photograph
## **Patient** (inherits base user)

* **Medical history**
	+ List of allergies (medications, food, environmental)
	+ List of chronic conditions (e.g., diabetes, hypertension)
	+ List of current medications
	+ Previous surgeries or medical procedures
	+ previous medical history docs ( optional)
* **Health metrics**
	+ Weight
	+ height
	+ Blood pressure
	+ Blood glucose levels
	+ Other relevant health metrics specific to their condition(s)

## **Doctor** (inherits base user)
* **Professional information**
	+ Name (first name, last name)
	+ Medical license number
	+ Specialty (e.g., primary care, cardiology, oncology)
	+ Years of experience
	+ hospital/organisation/clinic
* **Clinical data**
	+ List of patients assigned to them
	+ Treatment plans and progress notes for each patient
	+ Lab results and test reports
* **Education and training**
	+ Medical school attended
	+ Residency programs completed
	+ Continuing education credits
+ Availability schedule
	+ in/out time
	+ week days
	+ physical meet availability
	+ teleconference availability

## **Paramedic** (inherits base user)
* **Professional information**
	+ Emergency medical technician (EMT) certification number
	+ Years of experience
* **Incident data**
	+ List of emergencies responded
	+ Patient vitals and treatment administered during incidents
	+ Incident reports and summaries
* **Training and certifications**
	+ Emergency medical technician (EMT) certification level
	+ Advanced life support (ALS) or basic life support (BLS) training
	+ Other relevant certifications or training


## **Common fields**

All three user types will require:

* Authentication information (username, password)
* Contact information (email, phone number)

You may also want to consider additional fields specific to your organization's needs, such as insurance information for patients or hospital affiliation for doctors.

**Schema considerations**

When designing the schema, keep in mind the following:

1. **Normalization**: Ensure that each table has a clear purpose and is not duplicated across tables.
2. **Data integrity**: Implement constraints (e.g., foreign keys) to maintain data consistency between related tables.
3. **Security**: Consider using access controls and encryption to protect sensitive patient data.
4. **Scalability**: Design the schema to accommodate growth in user numbers and data volume.

Remember to review and refine your schema based on feedback from users, stakeholders, and technical teams to ensure it meets the needs of all parties involved.