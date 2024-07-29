# AYApplicationLoad

### #2: Student Status Calculation
###### <hr />

##### Inputs:
AD: Admission Decision in AY File <br />
SD: Submitted Date in AY File

	for all AD in AY file:
		if ( AD = null )
			if(SD == NULL)
				SS = "Inquiry"
			else
				SS = "Applicant"
		else
			SS = "Applicant"
			if(AD.CONTAINS ("Admit") OR AD.CONTAINS ("Admitted"))
				SS = "Admit"
			
	end for
	

	Student Status (AYApplication__c > Student_Status__c) Mapped to (RFIForm__c > Student_Status__c):
	-------------------------------------------------------------------------------------------------
	IF(
	(ISBLANK(Admission_Decision__c) || Admission_Decision__c == ""),
	(IF((ISBLANK(Submitted_Date__c) || Submitted_Date__c == ""), "Inquiry", "Applicant")),
	(IF((CONTAINS(LOWER(Admission_Decision__c), "admit") || (CONTAINS(LOWER(Admission_Decision__c), "admitted"))), "Admit", "Applicant"))
	)
	

#### Outputs:
SS: Student Status <br />


### #1: Term Calculation
##### <hr />

##### Inputs:
T<sub>i</sub>: Term codes from AY Files <br />
T<sub>c</sub>: Term code of current recruitment cycle.
    The checked-box in the Term record is the current recruitment cycle
  
#### Procedure

	for all T_i in AY file:

		if ( T_i == null OR T_i <= T_c )
			T_o = T_c

		else if(T_i < T_c)
			T_o = T_i

		T_o_nxt = Next term from T_o
		Sem = Semester Term of T_o_nxt
		Year = Year from T_o_nxt

		Final outputs to write:
		======================
		RFI TermLink = T_o
		RFI Term = sem
		RFI Year = Year
  
	End for  

#### Outputs:
T<sub>o</sub>: Term code to link RFI Term <br />
Sem : RFI Term in text <br />
Year: RFI Year
<hr />
