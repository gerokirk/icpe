libname long '/Users/jenniferkirk/OneDrive - University of Maryland School of Medicine/SP2021/BAM_PROJECT/SAS';


/* create formats for key new variables that will be created*/
proc format;
value use 0 = 'Non-BAM' /* use is a variable establishing whether the reason for prescribing a medication is primarily/secondarily for BMD loss or not */
		1 = 'Secondary Use BAM'
		2 = 'Primary BAM';
value studytime 0 = 'Baseline Year 2013' /* Years over which this study spans 2013-2017 2013 is the baseline year because we use it to identify our study sample from our total population*/
		1 = '2014'
		2 = '2015'
		3 = '2016'
		4 = '2017';
value drugtype 
		0='Not BAM'
		1 = 'Bisphosphonate'
		2 = 'BAM hormone like'
		3 = 'SERM'
		4 = 'monoclonal antibodies'
		5 = 'Rx Vitamin';	
value grouping 
0=	'Generalists' /*(internal medicine or family)*/
1 = 'Geriatric Specialists'
2 = 'Nurse practitioners'
3 = ' Physicians Assistants'
4 = 'Specialists'/*  Surgeons, OB/GYN & Urologists, rheumatologic & endocrinologic & OMT*/;		
run;

/* This next line adds extra information to the log on the time spent running a datastep or proc*/

options fullstimer;

/* Import Raw Claims datasets*/

/*Import 2013 to permanet dataset*/

DATA long.PartD_Prescriber_PUF_NPI_Drug_13 ;
	LENGTH
		npi								$ 10
		nppes_provider_last_org_name	$ 70
		nppes_provider_first_name		$ 20
		nppes_provider_state			$  2
		nppes_provider_city				$ 40
		specialty_description			$ 75
		description_flag				$  1
		drug_name		 		 		$ 30
		generic_name					$ 30
		bene_count						   8
		total_claim_count				   8
		total_30_day_fill_count			   8
		total_day_supply				   8
		total_drug_cost					   8
		bene_count_ge65					   8
		bene_count_ge65_suppress_flag	$  1
		total_claim_count_ge65			   8
		ge65_suppress_flag				$  1
		total_30_day_fill_count_ge65	   8
		total_day_supply_ge65			   8
		total_drug_cost_ge65			   8
	;
	
	INFILE 'C:\Users\jenni\OneDrive\Documents\FALL2020\Longitudinal_analysis\SAS\PartD_Prescriber_PUF_NPI_Drug_13.txt'
		dlm='09'x
		pad missover
		firstobs = 2
		dsd;

	INPUT
		npi							
		nppes_provider_last_org_name
		nppes_provider_first_name
		nppes_provider_city
		nppes_provider_state
		specialty_description
		description_flag
		drug_name
		generic_name
		bene_count
		total_claim_count
		total_30_day_fill_count
		total_day_supply
		total_drug_cost
		bene_count_ge65
		bene_count_ge65_suppress_flag
		total_claim_count_ge65
		ge65_suppress_flag
		total_30_day_fill_count_ge65
		total_day_supply_ge65
		total_drug_cost_ge65
	;	

	LABEL
		npi								= "National Provider Identifier"
		nppes_provider_last_org_name	= "Last Name/Organization Name of the Provider"
		nppes_provider_first_name		= "First Name of the Provider"
		nppes_provider_city				= "City of the Provider"
		nppes_provider_state			= "State Code of the Provider"
		specialty_description			= "Provider Specialty Type"
		description_flag				= "Source of Provider Specialty"
		drug_name		 		 		= "Brand Name"
		generic_name					= "USAN Generic Name - Short Version"
		bene_count						= "Number of Medicare Beneficiaries"
		total_claim_count 				= "Number of Medicare Part D Claims, Including Refills"
		total_30_day_fill_count			= "Number of Standardized 30-Day Fills, Including Refills"
		total_day_supply 				= "Number of Day's Supply for All Claims"
		total_drug_cost 				= "Aggregate Cost Paid for All Claims"
		bene_count_ge65					= "Number of Medicare Beneficiaries Age 65+"
		bene_count_ge65_suppress_flag	= "Reason for Suppression of Bene_Count_Ge65"
		total_claim_count_ge65			= "Number of Claims, Including Refills, for Beneficiaries Age 65+"
		ge65_suppress_flag				= "Reason for Suppression of Total_Claim_Count_Ge65, Total_30_Day_Fill_Count_Ge65, Total_Day_Supply_Ge65 and Total_Drug_Cost_Ge65"
		total_30_day_fill_count_ge65	= "Number of Standardized 30-Day Fills, Including Refills, for Beneficiaries Age 65+"
		total_day_supply_ge65			= "Number of Day's Supply for All Claims for Beneficiaries Age 65+"
		total_drug_cost_ge65			= "Aggregate Cost Paid for All Claims for Beneficiaries Age 65+"
;

RUN;

/*Convert npi (the id variable to numeric value) vastly speeds up processing time & add time variable*/

data long.bam2013;
set long.PartD_Prescriber_PUF_NPI_Drug_13;
     npi_num = input(npi, 10.);
	 time=0;
	 format time studytime.;
run;

DATA long.PartD_Prescriber_PUF_NPI_Drug_14;
	LENGTH
		npi								$ 10
		nppes_provider_last_org_name	$ 70
		nppes_provider_first_name		$ 20
		nppes_provider_state			$  2
		nppes_provider_city				$ 40
		specialty_description			$ 75
		description_flag				$  1
		drug_name		 		 		$ 30
		generic_name					$ 30
		bene_count						   8
		total_claim_count				   8
		total_30_day_fill_count			   8
		total_day_supply				   8
		total_drug_cost					   8
		bene_count_ge65					   8
		bene_count_ge65_suppress_flag	$  1
		total_claim_count_ge65			   8
		ge65_suppress_flag				$  1
		total_30_day_fill_count_ge65	   8
		total_day_supply_ge65			   8
		total_drug_cost_ge65			   8
	;
	
	INFILE 'C:\Users\jenni\OneDrive\Documents\FALL2020\Longitudinal_analysis\SAS\PartD_Prescriber_PUF_NPI_Drug_14.txt'
		dlm='09'x
		pad missover
		firstobs = 2
		dsd;

	INPUT
		npi							
		nppes_provider_last_org_name
		nppes_provider_first_name
		nppes_provider_city
		nppes_provider_state
		specialty_description
		description_flag
		drug_name
		generic_name
		bene_count
		total_claim_count
		total_30_day_fill_count
		total_day_supply
		total_drug_cost
		bene_count_ge65
		bene_count_ge65_suppress_flag
		total_claim_count_ge65
		ge65_suppress_flag
		total_30_day_fill_count_ge65
		total_day_supply_ge65
		total_drug_cost_ge65
	;	

	LABEL
		npi								= "National Provider Identifier"
		nppes_provider_last_org_name	= "Last Name/Organization Name of the Provider"
		nppes_provider_first_name		= "First Name of the Provider"
		nppes_provider_city				= "City of the Provider"
		nppes_provider_state			= "State Code of the Provider"
		specialty_description			= "Provider Specialty Type"
		description_flag				= "Source of Provider Specialty"
		drug_name		 		 		= "Brand Name"
		generic_name					= "USAN Generic Name - Short Version"
		bene_count						= "Number of Medicare Beneficiaries"
		total_claim_count 				= "Number of Medicare Part D Claims, Including Refills"
		total_30_day_fill_count			= "Number of Standardized 30-Day Fills, Including Refills"
		total_day_supply 				= "Number of Day's Supply for All Claims"
		total_drug_cost 				= "Aggregate Cost Paid for All Claims"
		bene_count_ge65					= "Number of Medicare Beneficiaries Age 65+"
		bene_count_ge65_suppress_flag	= "Reason for Suppression of Bene_Count_Ge65"
		total_claim_count_ge65			= "Number of Claims, Including Refills, for Beneficiaries Age 65+"
		ge65_suppress_flag				= "Reason for Suppression of Total_Claim_Count_Ge65, Total_30_Day_Fill_Count_Ge65, Total_Day_Supply_Ge65 and Total_Drug_Cost_Ge65"
		total_30_day_fill_count_ge65	= "Number of Standardized 30-Day Fills, Including Refills, for Beneficiaries Age 65+"
		total_day_supply_ge65			= "Number of Day's Supply for All Claims for Beneficiaries Age 65+"
		total_drug_cost_ge65			= "Aggregate Cost Paid for All Claims for Beneficiaries Age 65+"
;

RUN;
data long.bam2014;
set long.PartD_Prescriber_PUF_NPI_Drug_14;
     npi_num = input(npi, 10.);
	 time=1;
	 format time studytime.;
run;

DATA long.PartD_Prescriber_PUF_NPI_Drug_15;
	LENGTH
		npi								$ 10
		nppes_provider_last_org_name	$ 70
		nppes_provider_first_name		$ 20
		nppes_provider_state			$  2
		nppes_provider_city				$ 40
		specialty_description			$ 75
		description_flag				$  1
		drug_name		 		 		$ 30
		generic_name					$ 30
		bene_count						   8
		total_claim_count				   8
		total_30_day_fill_count			   8
		total_day_supply				   8
		total_drug_cost					   8
		bene_count_ge65					   8
		bene_count_ge65_suppress_flag	$  1
		total_claim_count_ge65			   8
		ge65_suppress_flag				$  1
		total_30_day_fill_count_ge65	   8
		total_day_supply_ge65			   8
		total_drug_cost_ge65			   8
	;
	
	INFILE 'C:\Users\jenni\OneDrive\Documents\FALL2020\Longitudinal_analysis\SAS\PartD_Prescriber_PUF_NPI_Drug_15.txt'
		dlm='09'x
		pad missover
		firstobs = 2
		dsd;

	INPUT
		npi							
		nppes_provider_last_org_name
		nppes_provider_first_name
		nppes_provider_city
		nppes_provider_state
		specialty_description
		description_flag
		drug_name
		generic_name
		bene_count
		total_claim_count
		total_30_day_fill_count
		total_day_supply
		total_drug_cost
		bene_count_ge65
		bene_count_ge65_suppress_flag
		total_claim_count_ge65
		ge65_suppress_flag
		total_30_day_fill_count_ge65
		total_day_supply_ge65
		total_drug_cost_ge65
	;	

	LABEL
		npi								= "National Provider Identifier"
		nppes_provider_last_org_name	= "Last Name/Organization Name of the Provider"
		nppes_provider_first_name		= "First Name of the Provider"
		nppes_provider_city				= "City of the Provider"
		nppes_provider_state			= "State Code of the Provider"
		specialty_description			= "Provider Specialty Type"
		description_flag				= "Source of Provider Specialty"
		drug_name		 		 		= "Brand Name"
		generic_name					= "USAN Generic Name - Short Version"
		bene_count						= "Number of Medicare Beneficiaries"
		total_claim_count 				= "Number of Medicare Part D Claims, Including Refills"
		total_30_day_fill_count			= "Number of Standardized 30-Day Fills, Including Refills"
		total_day_supply 				= "Number of Day's Supply for All Claims"
		total_drug_cost 				= "Aggregate Cost Paid for All Claims"
		bene_count_ge65					= "Number of Medicare Beneficiaries Age 65+"
		bene_count_ge65_suppress_flag	= "Reason for Suppression of Bene_Count_Ge65"
		total_claim_count_ge65			= "Number of Claims, Including Refills, for Beneficiaries Age 65+"
		ge65_suppress_flag				= "Reason for Suppression of Total_Claim_Count_Ge65, Total_30_Day_Fill_Count_Ge65, Total_Day_Supply_Ge65 and Total_Drug_Cost_Ge65"
		total_30_day_fill_count_ge65	= "Number of Standardized 30-Day Fills, Including Refills, for Beneficiaries Age 65+"
		total_day_supply_ge65			= "Number of Day's Supply for All Claims for Beneficiaries Age 65+"
		total_drug_cost_ge65			= "Aggregate Cost Paid for All Claims for Beneficiaries Age 65+"
;

RUN;
data long.bam2015;
set long.PartD_Prescriber_PUF_NPI_Drug_15;
     npi_num = input(npi, 10.);
	 time=2;
	 format time studytime.;
run;

DATA long.PartD_Prescriber_PUF_NPI_Drug_16;
	LENGTH
		npi								$ 10
		nppes_provider_last_org_name	$ 70
		nppes_provider_first_name		$ 20
		nppes_provider_state			$  2
		nppes_provider_city				$ 40
		specialty_description			$ 75
		description_flag				$  1
		drug_name		 		 		$ 30
		generic_name					$ 30
		bene_count						   8
		total_claim_count				   8
		total_30_day_fill_count			   8
		total_day_supply				   8
		total_drug_cost					   8
		bene_count_ge65					   8
		bene_count_ge65_suppress_flag	$  1
		total_claim_count_ge65			   8
		ge65_suppress_flag				$  1
		total_30_day_fill_count_ge65	   8
		total_day_supply_ge65			   8
		total_drug_cost_ge65			   8
	;
	
	INFILE 'C:\Users\jenni\OneDrive\Documents\FALL2020\Longitudinal_analysis\SAS\PartD_Prescriber_PUF_NPI_Drug_16.txt'
		dlm='09'x
		pad missover
		firstobs = 2
		dsd;

	INPUT
		npi							
		nppes_provider_last_org_name
		nppes_provider_first_name
		nppes_provider_city
		nppes_provider_state
		specialty_description
		description_flag
		drug_name
		generic_name
		bene_count
		total_claim_count
		total_30_day_fill_count
		total_day_supply
		total_drug_cost
		bene_count_ge65
		bene_count_ge65_suppress_flag
		total_claim_count_ge65
		ge65_suppress_flag
		total_30_day_fill_count_ge65
		total_day_supply_ge65
		total_drug_cost_ge65
	;	

	LABEL
		npi								= "National Provider Identifier"
		nppes_provider_last_org_name	= "Last Name/Organization Name of the Provider"
		nppes_provider_first_name		= "First Name of the Provider"
		nppes_provider_city				= "City of the Provider"
		nppes_provider_state			= "State Code of the Provider"
		specialty_description			= "Provider Specialty Type"
		description_flag				= "Source of Provider Specialty"
		drug_name		 		 		= "Brand Name"
		generic_name					= "USAN Generic Name - Short Version"
		bene_count						= "Number of Medicare Beneficiaries"
		total_claim_count 				= "Number of Medicare Part D Claims, Including Refills"
		total_30_day_fill_count			= "Number of Standardized 30-Day Fills, Including Refills"
		total_day_supply 				= "Number of Day's Supply for All Claims"
		total_drug_cost 				= "Aggregate Cost Paid for All Claims"
		bene_count_ge65					= "Number of Medicare Beneficiaries Age 65+"
		bene_count_ge65_suppress_flag	= "Reason for Suppression of Bene_Count_Ge65"
		total_claim_count_ge65			= "Number of Claims, Including Refills, for Beneficiaries Age 65+"
		ge65_suppress_flag				= "Reason for Suppression of Total_Claim_Count_Ge65, Total_30_Day_Fill_Count_Ge65, Total_Day_Supply_Ge65 and Total_Drug_Cost_Ge65"
		total_30_day_fill_count_ge65	= "Number of Standardized 30-Day Fills, Including Refills, for Beneficiaries Age 65+"
		total_day_supply_ge65			= "Number of Day's Supply for All Claims for Beneficiaries Age 65+"
		total_drug_cost_ge65			= "Aggregate Cost Paid for All Claims for Beneficiaries Age 65+"
;

RUN;
data long.bam2016;
set long.PartD_Prescriber_PUF_NPI_Drug_16;
     npi_num = input(npi, 10.);
	 time=3;
	 format time studytime.;
run;


DATA long.PartD_Prescriber_PUF_NPI_Drug_17;
	LENGTH
		npi								$ 10
		nppes_provider_last_org_name	$ 70
		nppes_provider_first_name		$ 20
		nppes_provider_state			$  2
		nppes_provider_city				$ 40
		specialty_description			$ 75
		description_flag				$  1
		drug_name		 		 		$ 30
		generic_name					$ 30
		bene_count						   8
		total_claim_count				   8
		total_30_day_fill_count			   8
		total_day_supply				   8
		total_drug_cost					   8
		bene_count_ge65					   8
		bene_count_ge65_suppress_flag	$  1
		total_claim_count_ge65			   8
		ge65_suppress_flag				$  1
		total_30_day_fill_count_ge65	   8
		total_day_supply_ge65			   8
		total_drug_cost_ge65			   8
	;
	
	INFILE 'C:\Users\jenni\OneDrive\Documents\FALL2020\Longitudinal_analysis\SAS\PartD_Prescriber_PUF_NPI_Drug_17.txt'
		dlm='09'x
		pad missover
		firstobs = 2
		dsd;

	INPUT
		npi							
		nppes_provider_last_org_name
		nppes_provider_first_name
		nppes_provider_city
		nppes_provider_state
		specialty_description
		description_flag
		drug_name
		generic_name
		bene_count
		total_claim_count
		total_30_day_fill_count
		total_day_supply
		total_drug_cost
		bene_count_ge65
		bene_count_ge65_suppress_flag
		total_claim_count_ge65
		ge65_suppress_flag
		total_30_day_fill_count_ge65
		total_day_supply_ge65
		total_drug_cost_ge65
	;	

	LABEL
		npi								= "National Provider Identifier"
		nppes_provider_last_org_name	= "Last Name/Organization Name of the Provider"
		nppes_provider_first_name		= "First Name of the Provider"
		nppes_provider_city				= "City of the Provider"
		nppes_provider_state			= "State Code of the Provider"
		specialty_description			= "Provider Specialty Type"
		description_flag				= "Source of Provider Specialty"
		drug_name		 		 		= "Brand Name"
		generic_name					= "USAN Generic Name - Short Version"
		bene_count						= "Number of Medicare Beneficiaries"
		total_claim_count 				= "Number of Medicare Part D Claims, Including Refills"
		total_30_day_fill_count			= "Number of Standardized 30-Day Fills, Including Refills"
		total_day_supply 				= "Number of Day's Supply for All Claims"
		total_drug_cost 				= "Aggregate Cost Paid for All Claims"
		bene_count_ge65					= "Number of Medicare Beneficiaries Age 65+"
		bene_count_ge65_suppress_flag	= "Reason for Suppression of Bene_Count_Ge65"
		total_claim_count_ge65			= "Number of Claims, Including Refills, for Beneficiaries Age 65+"
		ge65_suppress_flag				= "Reason for Suppression of Total_Claim_Count_Ge65, Total_30_Day_Fill_Count_Ge65, Total_Day_Supply_Ge65 and Total_Drug_Cost_Ge65"
		total_30_day_fill_count_ge65	= "Number of Standardized 30-Day Fills, Including Refills, for Beneficiaries Age 65+"
		total_day_supply_ge65			= "Number of Day's Supply for All Claims for Beneficiaries Age 65+"
		total_drug_cost_ge65			= "Aggregate Cost Paid for All Claims for Beneficiaries Age 65+"
;

RUN;

/* add time variable to each dataset as well as formats*/

data long.bam2017;
set long.PartD_Prescriber_PUF_NPI_Drug_17;
     npi_num = input(npi, 10.);
	 time=4;
	 format time studytime.;
run;

data bam2014a;
	set bam2014;
if 		
		specialty_description='Orthopaedic Surgery'|
		specialty_description='Orthopedic Surgery'|
		specialty_description='Gynecological/Oncology' |
		specialty_description='Obstetrics/Gynecology' |
		specialty_description='Urology' |
		specialty_description='Rheumatology' |
		specialty_description='Endocrinology'|
		specialty_description='Osteopathic Manipulative Medicine' |	
		specialty_description='Physical Medicine and Rehabilitation'|
		specialty_description='Hematology/Oncology' |
		specialty_description='Medical Oncology' |
		specialty_description='Radiation Oncology' |
		specialty_description='Pulmonary Disease' |
		specialty_description='Nephrology'|		
		specialty_description='Specialist'|
		specialty_description='Gastroenterology'	
then 	group=4;
else if specialty_description='Physician Assistant' 
then 	group=3;
else if specialty_description='Licensed Practical Nurse'|
		specialty_description='Certified Clinical Nurse Specialist' |
		specialty_description='Certified Nurse Midwife'| 
		specialty_description='Registered Nurse'|
		specialty_description='Nurse Practitioner'
then	group=2;
else if specialty_description='Geriatric Medicine' 
then	group=1;
else if specialty_description='Family Medicine' |
		specialty_description='Family Practice' |
		specialty_description='General Practice'|
		specialty_description='Internal Medicine'|
		specialty_description='Preventive Medicine'|
		specialty_description='Emergency Medicine'|
		specialty_description='General Surgery' 	
then 	group=0;
else group=. ;
if group=. then delete;
if 
	drug_name='ACTIVELLA'|
	generic_name='ESTRADIOL/NORETHINDRONE ACET'|
	drug_name='ALORA'|generic_name='ESTRADIOL'| 
	drug_name='ALOSETRON HCL'|
	generic_name='ALOSETRON HCL'|
	drug_name='AMABELZ'|
	generic_name= 'ESTRADIOL/NORETHINDRONE ACET' |
	drug_name='CLIMARA'|	
	drug_name='CLIMARA PRO'|
	generic_name='ESTRADIOL/LEVONORGESTREL'|
	drug_name='DUAVEE'|	
	generic_name='ESTROGENS,CONJ/BAZEDOXIFENE'|
	drug_name='ESTRADIOL*'|
	generic_name='ESTRADIOL/LEVONORGESTREL'	|
	drug_name='CYSTADANE'	|
	generic_name='BETAINE'|	
	drug_name='DUAVEE'	|
	generic_name='ESTROGENS,CONJ/BAZEDOXIFENE'|
	drug_name='ESTRADIOL-NORETHINDRONE ACETAT'|
	generic_name='ESTRADIOL/NORETHINDRONE ACET'|
	drug_name='ESTROPIPATE'|generic_name='ESTROPIPATE'|
	drug_name='FEMHRT'	|generic_name='NORETHINDRONE AC-ETH ESTRADIOL'|
	drug_name='FYAVOLV'	|drug_name='LOPREEZA'|	generic_name='ESTRADIOL/NORETHINDRONE ACET'|
	drug_name='MENEST'|	generic_name='ESTROGENS,ESTERIFIED'|
	drug_name='MENOSTAR	'|drug_name='MIMVEY'	|drug_name='MIMVEY LO'|	drug_name='MINIVELLE'|	
	drug_name='NORETHINDRON-ETHINYL ESTRADIOL'|
	drug_name='PREFEST'|generic_name='ESTRADIOL/NORGESTIMATE'	|
	drug_name='PREMARIN*'	|generic_name='ESTROGENS, CONJUGATED'	|
	drug_name='PREMPHASE'|	generic_name='ESTROGEN,CON/M-PROGEST ACET'	|
	drug_name='PREMPRO'|drug_name='VIVELLE-DOT'		
then use=1;
else if 
drug_name='ACTONEL'	|generic_name='RISEDRONATE SODIUM'|
drug_name='ALENDRONATE SODIUM'|	generic_name='ALENDRONATE SODIUM'|	
drug_name='ATELVIA'	|drug_name='BINOSTO'|	drug_name='BONIVA*'|generic_name='IBANDRONATE SODIUM'|	
drug_name='CALCITONIN-SALMON'|	generic_name='CALCITONIN,SALMON,SYNTHETIC'|	
drug_name='EVISTA'|	generic_name='RALOXIFENE HCL'|
drug_name='FORTEO'	|generic_name='TERIPARATIDE'|	
drug_name='FORTICAL'|	drug_name='FOSAMAX PLUS D'	|generic_name='ALENDRONATE SODIUM/VITAMIN D3'|	
drug_name='IBANDRONATE SODIUM*'|	drug_name='MIACALCIN*'|drug_name='PROLIA' |generic_name='DENOSUMAB'|	
drug_name='RALOXIFENE HCL'|	generic_name='RALOXIFENE HCL'|	
drug_name='RECLAST'|	generic_name='ZOLEDRONIC ACID/MANNITOL-WATER'	|
drug_name='RISEDRONATE SODIUM'	|	drug_name='RISEDRONATE SODIUM DR'|	drug_name='TYMLOS'| generic_name='ABALOPARATIDE'|	
drug_name='VITAMIN D2'|	generic_name='ERGOCALCIFEROL (VITAMIN D2)'|	
drug_name='ZOLEDRONIC ACID'|	generic_name='ZOLEDRONIC ACID/MANNITOL-WATER' then use=2;
else use=0;
if  
	generic_name='RISEDRONATE SODIUM'|
	generic_name='ALENDRONATE SODIUM'|
	generic_name='IBANDRONATE SODIUM'| 
	generic_name='TERIPARATIDE'|
	generic_name='ALENDRONATE SODIUM/VITAMIN D3'|
	generic_name='ZOLEDRONIC ACID/MANNITOL-WATER'
		then type=1;
else if	generic_name='CALCITONIN,SALMON,SYNTHETIC'|	generic_name='ABALOPARATIDE' then	type=2;
else if	generic_name='RALOXIFENE HCL' then type=3;
else if  generic_name='DENOSUMAB' then type=4;
else if generic_name='ERGOCALCIFEROL (VITAMIN D2)' then type=5;
else type=0;
if use=1|use=0 then nonbam_total_claims=total_claim_count;
else nonbam_total_claims=.;
if use=2 then bam_total_claims=total_claim_count;
else bam_total_claims=.;
if use=. then delete;
run;

data bam2013;
	set bam2013_2;
if 		
		specialty_description='Orthopaedic Surgery'|
		specialty_description='Orthopedic Surgery'|
		specialty_description='Gynecological/Oncology' |
		specialty_description='Obstetrics/Gynecology' |
		specialty_description='Urology' |
		specialty_description='Rheumatology' |
		specialty_description='Endocrinology'|
		specialty_description='Osteopathic Manipulative Medicine' |	
		specialty_description='Physical Medicine and Rehabilitation'|
		specialty_description='Hematology/Oncology' |
		specialty_description='Medical Oncology' |
		specialty_description='Radiation Oncology' |
		specialty_description='Pulmonary Disease' |
		specialty_description='Nephrology'|		
		specialty_description='Specialist'|
		specialty_description='Gastroenterology'	
then 	group=4;
else if specialty_description='Physician Assistant' 
then 	group=3;
else if specialty_description='Licensed Practical Nurse'|
		specialty_description='Certified Clinical Nurse Specialist' |
		specialty_description='Certified Nurse Midwife'| 
		specialty_description='Registered Nurse'|
		specialty_description='Nurse Practitioner'
then	group=2;
else if specialty_description='Geriatric Medicine' 
then	group=1;
else if specialty_description='Family Medicine' |
		specialty_description='Family Practice' |
		specialty_description='General Practice'|
		specialty_description='Internal Medicine'|
		specialty_description='Preventive Medicine'|
		specialty_description='Emergency Medicine'|
		specialty_description='General Surgery' 	
then 	group=0;
else group=. ;
if group=. then delete;
if 
	drug_name='ACTIVELLA'|
	generic_name='ESTRADIOL/NORETHINDRONE ACET'|
	drug_name='ALORA'|generic_name='ESTRADIOL'| 
	drug_name='ALOSETRON HCL'|
	generic_name='ALOSETRON HCL'|
	drug_name='AMABELZ'|
	generic_name= 'ESTRADIOL/NORETHINDRONE ACET' |
	drug_name='CLIMARA'|	
	drug_name='CLIMARA PRO'|
	generic_name='ESTRADIOL/LEVONORGESTREL'|
	drug_name='DUAVEE'|	
	generic_name='ESTROGENS,CONJ/BAZEDOXIFENE'|
	drug_name='ESTRADIOL*'|
	generic_name='ESTRADIOL/LEVONORGESTREL'	|
	drug_name='CYSTADANE'	|
	generic_name='BETAINE'|	
	drug_name='DUAVEE'	|
	generic_name='ESTROGENS,CONJ/BAZEDOXIFENE'|
	drug_name='ESTRADIOL-NORETHINDRONE ACETAT'|
	generic_name='ESTRADIOL/NORETHINDRONE ACET'|
	drug_name='ESTROPIPATE'|generic_name='ESTROPIPATE'|
	drug_name='FEMHRT'	|generic_name='NORETHINDRONE AC-ETH ESTRADIOL'|
	drug_name='FYAVOLV'	|drug_name='LOPREEZA'|	generic_name='ESTRADIOL/NORETHINDRONE ACET'|
	drug_name='MENEST'|	generic_name='ESTROGENS,ESTERIFIED'|
	drug_name='MENOSTAR	'|drug_name='MIMVEY'	|drug_name='MIMVEY LO'|	drug_name='MINIVELLE'|	
	drug_name='NORETHINDRON-ETHINYL ESTRADIOL'|
	drug_name='PREFEST'|generic_name='ESTRADIOL/NORGESTIMATE'	|
	drug_name='PREMARIN*'	|generic_name='ESTROGENS, CONJUGATED'	|
	drug_name='PREMPHASE'|	generic_name='ESTROGEN,CON/M-PROGEST ACET'	|
	drug_name='PREMPRO'|drug_name='VIVELLE-DOT'		
then use=1;
else if 
drug_name='ACTONEL'	|generic_name='RISEDRONATE SODIUM'|
drug_name='ALENDRONATE SODIUM'|	generic_name='ALENDRONATE SODIUM'|	
drug_name='ATELVIA'	|drug_name='BINOSTO'|	drug_name='BONIVA*'|generic_name='IBANDRONATE SODIUM'|	
drug_name='CALCITONIN-SALMON'|	generic_name='CALCITONIN,SALMON,SYNTHETIC'|	
drug_name='EVISTA'|	generic_name='RALOXIFENE HCL'|
drug_name='FORTEO'	|generic_name='TERIPARATIDE'|	
drug_name='FORTICAL'|	drug_name='FOSAMAX PLUS D'	|generic_name='ALENDRONATE SODIUM/VITAMIN D3'|	
drug_name='IBANDRONATE SODIUM*'|	drug_name='MIACALCIN*'|drug_name='PROLIA' |generic_name='DENOSUMAB'|	
drug_name='RALOXIFENE HCL'|	generic_name='RALOXIFENE HCL'|	
drug_name='RECLAST'|	generic_name='ZOLEDRONIC ACID/MANNITOL-WATER'	|
drug_name='RISEDRONATE SODIUM'	|	drug_name='RISEDRONATE SODIUM DR'|	drug_name='TYMLOS'| generic_name='ABALOPARATIDE'|	
drug_name='VITAMIN D2'|	generic_name='ERGOCALCIFEROL (VITAMIN D2)'|	
drug_name='ZOLEDRONIC ACID'|	generic_name='ZOLEDRONIC ACID/MANNITOL-WATER' then use=2;
else use=0;
if  
	generic_name='RISEDRONATE SODIUM'|
	generic_name='ALENDRONATE SODIUM'|
	generic_name='IBANDRONATE SODIUM'| 
	generic_name='TERIPARATIDE'|
	generic_name='ALENDRONATE SODIUM/VITAMIN D3'|
	generic_name='ZOLEDRONIC ACID/MANNITOL-WATER'
		then type=1;
else if	generic_name='CALCITONIN,SALMON,SYNTHETIC'|	generic_name='ABALOPARATIDE' then	type=2;
else if	generic_name='RALOXIFENE HCL' then type=3;
else if  generic_name='DENOSUMAB' then type=4;
else if generic_name='ERGOCALCIFEROL (VITAMIN D2)' then type=5;
else type=0;
if use=1|use=0 then nonbam_total_claims=total_claim_count;
else nonbam_total_claims=.;
if use=2 then bam_total_claims=total_claim_count;
else bam_total_claims=.;
if use=. then delete;
run;


data bam2015a;
	set bam2015;
if 		
		specialty_description='Orthopaedic Surgery'|
		specialty_description='Orthopedic Surgery'|
		specialty_description='Gynecological/Oncology' |
		specialty_description='Obstetrics/Gynecology' |
		specialty_description='Urology' |
		specialty_description='Rheumatology' |
		specialty_description='Endocrinology'|
		specialty_description='Osteopathic Manipulative Medicine' |	
		specialty_description='Physical Medicine and Rehabilitation'|
		specialty_description='Hematology/Oncology' |
		specialty_description='Medical Oncology' |
		specialty_description='Radiation Oncology' |
		specialty_description='Pulmonary Disease' |
		specialty_description='Nephrology'|		
		specialty_description='Specialist'|
		specialty_description='Gastroenterology'	
then 	group=4;
else if specialty_description='Physician Assistant' 
then 	group=3;
else if specialty_description='Licensed Practical Nurse'|
		specialty_description='Certified Clinical Nurse Specialist' |
		specialty_description='Certified Nurse Midwife'| 
		specialty_description='Registered Nurse'|
		specialty_description='Nurse Practitioner'
then	group=2;
else if specialty_description='Geriatric Medicine' 
then	group=1;
else if specialty_description='Family Medicine' |
		specialty_description='Family Practice' |
		specialty_description='General Practice'|
		specialty_description='Internal Medicine'|
		specialty_description='Preventive Medicine'|
		specialty_description='Emergency Medicine'|
		specialty_description='General Surgery' 	
then 	group=0;
else group=. ;
if group=. then delete;
if 
	drug_name='ACTIVELLA'|
	generic_name='ESTRADIOL/NORETHINDRONE ACET'|
	drug_name='ALORA'|generic_name='ESTRADIOL'| 
	drug_name='ALOSETRON HCL'|
	generic_name='ALOSETRON HCL'|
	drug_name='AMABELZ'|
	generic_name= 'ESTRADIOL/NORETHINDRONE ACET' |
	drug_name='CLIMARA'|	
	drug_name='CLIMARA PRO'|
	generic_name='ESTRADIOL/LEVONORGESTREL'|
	drug_name='DUAVEE'|	
	generic_name='ESTROGENS,CONJ/BAZEDOXIFENE'|
	drug_name='ESTRADIOL*'|
	generic_name='ESTRADIOL/LEVONORGESTREL'	|
	drug_name='CYSTADANE'	|
	generic_name='BETAINE'|	
	drug_name='DUAVEE'	|
	generic_name='ESTROGENS,CONJ/BAZEDOXIFENE'|
	drug_name='ESTRADIOL-NORETHINDRONE ACETAT'|
	generic_name='ESTRADIOL/NORETHINDRONE ACET'|
	drug_name='ESTROPIPATE'|generic_name='ESTROPIPATE'|
	drug_name='FEMHRT'	|generic_name='NORETHINDRONE AC-ETH ESTRADIOL'|
	drug_name='FYAVOLV'	|drug_name='LOPREEZA'|	generic_name='ESTRADIOL/NORETHINDRONE ACET'|
	drug_name='MENEST'|	generic_name='ESTROGENS,ESTERIFIED'|
	drug_name='MENOSTAR	'|drug_name='MIMVEY'	|drug_name='MIMVEY LO'|	drug_name='MINIVELLE'|	
	drug_name='NORETHINDRON-ETHINYL ESTRADIOL'|
	drug_name='PREFEST'|generic_name='ESTRADIOL/NORGESTIMATE'	|
	drug_name='PREMARIN*'	|generic_name='ESTROGENS, CONJUGATED'	|
	drug_name='PREMPHASE'|	generic_name='ESTROGEN,CON/M-PROGEST ACET'	|
	drug_name='PREMPRO'|drug_name='VIVELLE-DOT'		
then use=1;
else if 
drug_name='ACTONEL'	|generic_name='RISEDRONATE SODIUM'|
drug_name='ALENDRONATE SODIUM'|	generic_name='ALENDRONATE SODIUM'|	
drug_name='ATELVIA'	|drug_name='BINOSTO'|	drug_name='BONIVA*'|generic_name='IBANDRONATE SODIUM'|	
drug_name='CALCITONIN-SALMON'|	generic_name='CALCITONIN,SALMON,SYNTHETIC'|	
drug_name='EVISTA'|	generic_name='RALOXIFENE HCL'|
drug_name='FORTEO'	|generic_name='TERIPARATIDE'|	
drug_name='FORTICAL'|	drug_name='FOSAMAX PLUS D'	|generic_name='ALENDRONATE SODIUM/VITAMIN D3'|	
drug_name='IBANDRONATE SODIUM*'|	drug_name='MIACALCIN*'|drug_name='PROLIA' |generic_name='DENOSUMAB'|	
drug_name='RALOXIFENE HCL'|	generic_name='RALOXIFENE HCL'|	
drug_name='RECLAST'|	generic_name='ZOLEDRONIC ACID/MANNITOL-WATER'	|
drug_name='RISEDRONATE SODIUM'	|	drug_name='RISEDRONATE SODIUM DR'|	drug_name='TYMLOS'| generic_name='ABALOPARATIDE'|	
drug_name='VITAMIN D2'|	generic_name='ERGOCALCIFEROL (VITAMIN D2)'|	
drug_name='ZOLEDRONIC ACID'|	generic_name='ZOLEDRONIC ACID/MANNITOL-WATER' then use=2;
else use=0;
if  
	generic_name='RISEDRONATE SODIUM'|
	generic_name='ALENDRONATE SODIUM'|
	generic_name='IBANDRONATE SODIUM'| 
	generic_name='TERIPARATIDE'|
	generic_name='ALENDRONATE SODIUM/VITAMIN D3'|
	generic_name='ZOLEDRONIC ACID/MANNITOL-WATER'
		then type=1;
else if	generic_name='CALCITONIN,SALMON,SYNTHETIC'|	generic_name='ABALOPARATIDE' then	type=2;
else if	generic_name='RALOXIFENE HCL' then type=3;
else if  generic_name='DENOSUMAB' then type=4;
else if generic_name='ERGOCALCIFEROL (VITAMIN D2)' then type=5;
else type=0;
if use=1|use=0 then nonbam_total_claims=total_claim_count;
else nonbam_total_claims=.;
if use=2 then bam_total_claims=total_claim_count;
else bam_total_claims=.;
if use=. then delete;
run;

data bam2016a;
	set bam2016;
if 		
		specialty_description='Orthopaedic Surgery'|
		specialty_description='Orthopedic Surgery'|
		specialty_description='Gynecological/Oncology' |
		specialty_description='Obstetrics/Gynecology' |
		specialty_description='Urology' |
		specialty_description='Rheumatology' |
		specialty_description='Endocrinology'|
		specialty_description='Osteopathic Manipulative Medicine' |	
		specialty_description='Physical Medicine and Rehabilitation'|
		specialty_description='Hematology/Oncology' |
		specialty_description='Medical Oncology' |
		specialty_description='Radiation Oncology' |
		specialty_description='Pulmonary Disease' |
		specialty_description='Nephrology'|		
		specialty_description='Specialist'|
		specialty_description='Gastroenterology'	
then 	group=4;
else if specialty_description='Physician Assistant' 
then 	group=3;
else if specialty_description='Licensed Practical Nurse'|
		specialty_description='Certified Clinical Nurse Specialist' |
		specialty_description='Certified Nurse Midwife'| 
		specialty_description='Registered Nurse'|
		specialty_description='Nurse Practitioner'
then	group=2;
else if specialty_description='Geriatric Medicine' 
then	group=1;
else if specialty_description='Family Medicine' |
		specialty_description='Family Practice' |
		specialty_description='General Practice'|
		specialty_description='Internal Medicine'|
		specialty_description='Preventive Medicine'|
		specialty_description='Emergency Medicine'|
		specialty_description='General Surgery' 	
then 	group=0;
else group=. ;
if group=. then delete;
if 
	drug_name='ACTIVELLA'|
	generic_name='ESTRADIOL/NORETHINDRONE ACET'|
	drug_name='ALORA'|generic_name='ESTRADIOL'| 
	drug_name='ALOSETRON HCL'|
	generic_name='ALOSETRON HCL'|
	drug_name='AMABELZ'|
	generic_name= 'ESTRADIOL/NORETHINDRONE ACET' |
	drug_name='CLIMARA'|	
	drug_name='CLIMARA PRO'|
	generic_name='ESTRADIOL/LEVONORGESTREL'|
	drug_name='DUAVEE'|	
	generic_name='ESTROGENS,CONJ/BAZEDOXIFENE'|
	drug_name='ESTRADIOL*'|
	generic_name='ESTRADIOL/LEVONORGESTREL'	|
	drug_name='CYSTADANE'	|
	generic_name='BETAINE'|	
	drug_name='DUAVEE'	|
	generic_name='ESTROGENS,CONJ/BAZEDOXIFENE'|
	drug_name='ESTRADIOL-NORETHINDRONE ACETAT'|
	generic_name='ESTRADIOL/NORETHINDRONE ACET'|
	drug_name='ESTROPIPATE'|generic_name='ESTROPIPATE'|
	drug_name='FEMHRT'	|generic_name='NORETHINDRONE AC-ETH ESTRADIOL'|
	drug_name='FYAVOLV'	|drug_name='LOPREEZA'|	generic_name='ESTRADIOL/NORETHINDRONE ACET'|
	drug_name='MENEST'|	generic_name='ESTROGENS,ESTERIFIED'|
	drug_name='MENOSTAR	'|drug_name='MIMVEY'	|drug_name='MIMVEY LO'|	drug_name='MINIVELLE'|	
	drug_name='NORETHINDRON-ETHINYL ESTRADIOL'|
	drug_name='PREFEST'|generic_name='ESTRADIOL/NORGESTIMATE'	|
	drug_name='PREMARIN*'	|generic_name='ESTROGENS, CONJUGATED'	|
	drug_name='PREMPHASE'|	generic_name='ESTROGEN,CON/M-PROGEST ACET'	|
	drug_name='PREMPRO'|drug_name='VIVELLE-DOT'		
then use=1;
else if 
drug_name='ACTONEL'	|generic_name='RISEDRONATE SODIUM'|
drug_name='ALENDRONATE SODIUM'|	generic_name='ALENDRONATE SODIUM'|	
drug_name='ATELVIA'	|drug_name='BINOSTO'|	drug_name='BONIVA*'|generic_name='IBANDRONATE SODIUM'|	
drug_name='CALCITONIN-SALMON'|	generic_name='CALCITONIN,SALMON,SYNTHETIC'|	
drug_name='EVISTA'|	generic_name='RALOXIFENE HCL'|
drug_name='FORTEO'	|generic_name='TERIPARATIDE'|	
drug_name='FORTICAL'|	drug_name='FOSAMAX PLUS D'	|generic_name='ALENDRONATE SODIUM/VITAMIN D3'|	
drug_name='IBANDRONATE SODIUM*'|	drug_name='MIACALCIN*'|drug_name='PROLIA' |generic_name='DENOSUMAB'|	
drug_name='RALOXIFENE HCL'|	generic_name='RALOXIFENE HCL'|	
drug_name='RECLAST'|	generic_name='ZOLEDRONIC ACID/MANNITOL-WATER'	|
drug_name='RISEDRONATE SODIUM'	|	drug_name='RISEDRONATE SODIUM DR'|	drug_name='TYMLOS'| generic_name='ABALOPARATIDE'|	
drug_name='VITAMIN D2'|	generic_name='ERGOCALCIFEROL (VITAMIN D2)'|	
drug_name='ZOLEDRONIC ACID'|	generic_name='ZOLEDRONIC ACID/MANNITOL-WATER' then use=2;
else use=0;
if  
	generic_name='RISEDRONATE SODIUM'|
	generic_name='ALENDRONATE SODIUM'|
	generic_name='IBANDRONATE SODIUM'| 
	generic_name='TERIPARATIDE'|
	generic_name='ALENDRONATE SODIUM/VITAMIN D3'|
	generic_name='ZOLEDRONIC ACID/MANNITOL-WATER'
		then type=1;
else if	generic_name='CALCITONIN,SALMON,SYNTHETIC'|	generic_name='ABALOPARATIDE' then	type=2;
else if	generic_name='RALOXIFENE HCL' then type=3;
else if  generic_name='DENOSUMAB' then type=4;
else if generic_name='ERGOCALCIFEROL (VITAMIN D2)' then type=5;
else type=0;
if use=1|use=0 then nonbam_total_claims=total_claim_count;
else nonbam_total_claims=.;
if use=2 then bam_total_claims=total_claim_count;
else bam_total_claims=.;
if use=. then delete;
run;


data bam2017a;
	set bam2017;
if 		
		specialty_description='Orthopaedic Surgery'|
		specialty_description='Orthopedic Surgery'|
		specialty_description='Gynecological/Oncology' |
		specialty_description='Obstetrics/Gynecology' |
		specialty_description='Urology' |
		specialty_description='Rheumatology' |
		specialty_description='Endocrinology'|
		specialty_description='Osteopathic Manipulative Medicine' |	
		specialty_description='Physical Medicine and Rehabilitation'|
		specialty_description='Hematology/Oncology' |
		specialty_description='Medical Oncology' |
		specialty_description='Radiation Oncology' |
		specialty_description='Pulmonary Disease' |
		specialty_description='Nephrology'|		
		specialty_description='Specialist'|
		specialty_description='Gastroenterology'	
then 	group=4;
else if specialty_description='Physician Assistant' 
then 	group=3;
else if specialty_description='Licensed Practical Nurse'|
		specialty_description='Certified Clinical Nurse Specialist' |
		specialty_description='Certified Nurse Midwife'| 
		specialty_description='Registered Nurse'|
		specialty_description='Nurse Practitioner'
then	group=2;
else if specialty_description='Geriatric Medicine' 
then	group=1;
else if specialty_description='Family Medicine' |
		specialty_description='Family Practice' |
		specialty_description='General Practice'|
		specialty_description='Internal Medicine'|
		specialty_description='Preventive Medicine'|
		specialty_description='Emergency Medicine'|
		specialty_description='General Surgery' 	
then 	group=0;
else group=. ;
if group=. then delete;
if 
	drug_name='ACTIVELLA'|
	generic_name='ESTRADIOL/NORETHINDRONE ACET'|
	drug_name='ALORA'|generic_name='ESTRADIOL'| 
	drug_name='ALOSETRON HCL'|
	generic_name='ALOSETRON HCL'|
	drug_name='AMABELZ'|
	generic_name= 'ESTRADIOL/NORETHINDRONE ACET' |
	drug_name='CLIMARA'|	
	drug_name='CLIMARA PRO'|
	generic_name='ESTRADIOL/LEVONORGESTREL'|
	drug_name='DUAVEE'|	
	generic_name='ESTROGENS,CONJ/BAZEDOXIFENE'|
	drug_name='ESTRADIOL*'|
	generic_name='ESTRADIOL/LEVONORGESTREL'	|
	drug_name='CYSTADANE'	|
	generic_name='BETAINE'|	
	drug_name='DUAVEE'	|
	generic_name='ESTROGENS,CONJ/BAZEDOXIFENE'|
	drug_name='ESTRADIOL-NORETHINDRONE ACETAT'|
	generic_name='ESTRADIOL/NORETHINDRONE ACET'|
	drug_name='ESTROPIPATE'|generic_name='ESTROPIPATE'|
	drug_name='FEMHRT'	|generic_name='NORETHINDRONE AC-ETH ESTRADIOL'|
	drug_name='FYAVOLV'	|drug_name='LOPREEZA'|	generic_name='ESTRADIOL/NORETHINDRONE ACET'|
	drug_name='MENEST'|	generic_name='ESTROGENS,ESTERIFIED'|
	drug_name='MENOSTAR	'|drug_name='MIMVEY'	|drug_name='MIMVEY LO'|	drug_name='MINIVELLE'|	
	drug_name='NORETHINDRON-ETHINYL ESTRADIOL'|
	drug_name='PREFEST'|generic_name='ESTRADIOL/NORGESTIMATE'	|
	drug_name='PREMARIN*'	|generic_name='ESTROGENS, CONJUGATED'	|
	drug_name='PREMPHASE'|	generic_name='ESTROGEN,CON/M-PROGEST ACET'	|
	drug_name='PREMPRO'|drug_name='VIVELLE-DOT'		
then use=1;
else if 
drug_name='ACTONEL'	|generic_name='RISEDRONATE SODIUM'|
drug_name='ALENDRONATE SODIUM'|	generic_name='ALENDRONATE SODIUM'|	
drug_name='ATELVIA'	|drug_name='BINOSTO'|	drug_name='BONIVA*'|generic_name='IBANDRONATE SODIUM'|	
drug_name='CALCITONIN-SALMON'|	generic_name='CALCITONIN,SALMON,SYNTHETIC'|	
drug_name='EVISTA'|	generic_name='RALOXIFENE HCL'|
drug_name='FORTEO'	|generic_name='TERIPARATIDE'|	
drug_name='FORTICAL'|	drug_name='FOSAMAX PLUS D'	|generic_name='ALENDRONATE SODIUM/VITAMIN D3'|	
drug_name='IBANDRONATE SODIUM*'|	drug_name='MIACALCIN*'|drug_name='PROLIA' |generic_name='DENOSUMAB'|	
drug_name='RALOXIFENE HCL'|	generic_name='RALOXIFENE HCL'|	
drug_name='RECLAST'|	generic_name='ZOLEDRONIC ACID/MANNITOL-WATER'	|
drug_name='RISEDRONATE SODIUM'	|	drug_name='RISEDRONATE SODIUM DR'|	drug_name='TYMLOS'| generic_name='ABALOPARATIDE'|	
drug_name='VITAMIN D2'|	generic_name='ERGOCALCIFEROL (VITAMIN D2)'|	
drug_name='ZOLEDRONIC ACID'|	generic_name='ZOLEDRONIC ACID/MANNITOL-WATER' then use=2;
else use=0;
if  
	generic_name='RISEDRONATE SODIUM'|
	generic_name='ALENDRONATE SODIUM'|
	generic_name='IBANDRONATE SODIUM'| 
	generic_name='TERIPARATIDE'|
	generic_name='ALENDRONATE SODIUM/VITAMIN D3'|
	generic_name='ZOLEDRONIC ACID/MANNITOL-WATER'
		then type=1;
else if	generic_name='CALCITONIN,SALMON,SYNTHETIC'|	generic_name='ABALOPARATIDE' then	type=2;
else if	generic_name='RALOXIFENE HCL' then type=3;
else if  generic_name='DENOSUMAB' then type=4;
else if generic_name='ERGOCALCIFEROL (VITAMIN D2)' then type=5;
else type=0;
if use=1|use=0 then nonbam_total_claims=total_claim_count;
else nonbam_total_claims=.;
if use=2 then bam_total_claims=total_claim_count;
else bam_total_claims=.;
if use=. then delete;
run;
