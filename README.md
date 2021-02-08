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
/*code to remove datasets from working memory to speed up processes*/

# icpe
ICPE Abstract and Manuscript
