# Project Charter- Customer 360

## Business background
* **Who are the stakeholders?**
	* Business Analysts
	* Business Strategists
	* PACCAR Dealers
	* IT Architects
	* Data Governance Stewards
	* Regulatory Managers
	* Marketing Managers
	* Finance Managers
	* Leasing Managers
* **What are the basic business problems we are trying to address?**
	* Identify opportunities to create customer loyalty/affinity programs
	* Create location-based driver (concierge) services
	* Enable targeted marketing campaigns by purchase probability
	* Increase effectiveness and decrease cost of sales and marketing efforts
	* Support advanced credit risk analysis
	* Ensure regulatory compliance through appropriate customer and dealer incentives
	* Identify additional sales opportunities in the Aftermarket
	* Provide better and more consistent customer service
	* Develop a better understanding of the total lifetime value of each customer 
	* Improve and optimize customer-related business processes
	* Possible monetization of data relating to PACCAR's customer relationships
* **What is this problem worth to the stakeholders?**
	* TBD

## Scope
* **Questions/issues to be addressed by this POC:**
	* What business entities/parties/roles should be included in Customer MDM? How are they defined?
	* What data do we need to extract and/or purchase (and from which sources) in order to achieve a complete view of a customer?
	* What cleansing or transformation of the data from each source is needed?
	* How can/should the data from the different sources be integrated together?
	* What is the best taxonomy (organization) for Customer data? What level of abstraction (e.g., "Party") is appropriate?
	* What customer data needs to be managed centrally? Which data should be managed in the source systems?
	* What history of changes to data values do we want to keep?
	* What level of security needs to be established for which customer data attributes?
	* What metadata (data about the data) needs to be created and maintained to make the customer data useful?
	* What type and degree of stewardship by the business is needed to maintain the quality of customer data?
	* What technologies and tools are needed to most effectively create and manage Customer master data?
	* How can this master data be effectively leveraged in support of the above-mentioned business objectives?
* **Statement of work:**
	* Identification and modeling of customer-related data entities
	* Identification of data sources
	* Creation of MDM solution architecture and MDM repository
	* Extraction and profiling of data from each source
	* Cleansing, transformation, integration and loading of data into MDM repository
	* Creation of associated end-user metadata
	* Evaluation of MDM tools and technologies

## Personnel
* Project Team:
	* Data Science Team:
		* Project Lead- Danny Godbout
		* PM- Danny Godbout
		* Data Scientist(s)- Danny Godbout
	* Customers:
		* Business Analyst- Scott Flint
		* Business contact- Erich Coit, Doug Gunter
		* End users of data product- TBD
	* External Partners:
		* Consulting- NA

## Metrics
* **What are the qualitative objectives? (e.g. reduce user churn)**
	* Correct Classification- Decisions made by the algorithm match the decisions made by subject matter experts
	* Yield- Percent of claims processed by the algorithm (low-confidence claims are passed on for manual review)
	* Speed- Tool processes claims in a timely fashion.
* **What are the quantifiable metrics, their baseline (current) values and target values  (e.g. reduce the fraction of users with 4-week inactivity from 60% to 40%)**
	* Yield- Simple % of total claims flagged for payment or supplier recovery: (pay + supplier) / (pay + supplier + manual review)
		* Baseline: TBD
		* Target: TBD (Dependent on review of baseline data)
	* Kappa Statistic- Score agreement between algorithm and subject matter experts. Kappa chosen over simple percent-agreement to compensate for biased classes (i.e. if 99.999% of claims were approved, an algorithm could simply approve all claims and agree 99.999% of the time. Kappa adjusts weighting for the smaller .001% remaining class to avoid "lazy" learners.)
		* Baseline: 0.0 (Does any capability documentation, i.e. MSA, exist for the current process?)
		* Target: > 0.6 is good, >0.8 stretch goal
	* Time per prediction- Measure time to process a claim.
		* Baseline: 1200 claims per week
		* Target: > 1200 claims per week
* **How will we measure the metric? (e.g. A/B test on a specified subset for a specified period; or comparison of performance after implementation to baseline)**
	* Yield- Snapshot 2-4 weeks of claims, measure yield of algorithm output compared to manual process.
	* Kappa- Measure metric on the same 2-4 week snapshot as yield.
	* Time- Use code profiling tools to measure time to review a batch of claims.
	* After implementation- Ongoing reports of yield, and auditing process to verify a sample of predictions.

## Plan

| Milestone | Est. Completion | Description |
| ---:| ---: | ---: | ---: |
| Build data pipeline | Nov. 4 | 1. Gain access to warranty prod database <br/> 2. Wiretap original claims from mainframe into a separate database |
| Accumulate data | Nov. 18 - Dec. 2 (est.) | Accumulate approximately 5000 copies of both original **and** processed claims |
| Stakeholder charter review | Nov. 25 | Complete project charter document with stakeholders to confirm scope and targets |
| Review baseline data | Dec. 2 | Basic data mining, calculation of summary statistics, transformation and tidying of data |
| Approach 1: ML | Jan. 6 | Build machine learners for approach 1 |
| Approach 2: Anomaly Detection | Jan. 6 | Build anomaly detection for approach 2 |
| Review performance | Jan. 20 | Review performance against metrics with project stakeholders |
| Go/ No-Go for Pilot | Jan. 20 | Identify need for additional iterations, and/or make decision on whether to proceed with a pilot |


## Architecture
### Data
* **What data do we expect? Raw data in the customer data sources (e.g. on-prem files, SQL, on-prem Hadoop etc.)**
	* Processed claims obtained from the Production Warranty database (SQL)
	* Original claims will be extracted from DWC on mainframe daily, generating a flat file to be transmitted via FTP to a network folder. Flat files will be ingested into an on-prem Hadoop platform and stored in Hive for SQL access.
* **What quantity of data do we expect? (e.g. Megabytes, Terabytes?)**
	* 100s of MB to 1s of GBs.

### Data processing in analytical environment
* **Is the data tractable on a single machine, or will it require clustered resources?**
   * Expectation is that data are manageable on a single PC. Snapshots of data sources will be stored as RData files for analysis.

### What tools and data storage/analytics resources will be used in the solution
* SQL Server Management Studio- Data capture from production warranty database.
* Apache Kafka (or similar)- Ingestion of mainframe flat files into a project data warehouse.
* Apache Hadoop, Hive- Project data warehouse, storing mainframe exports of original warranty claims.
* R- Data processing and modeling.

### How will the operationalized web service(s) be consumed in the business workflow of the customer?
* **How will the customer use the model results to make decisions**
	* The model will automate the decision process for a portion of claims, reducing labor hours for claim adjudication.
	* The model will flag unusual values requiring additional information from a dealership.
	* With the transition to the Pega warranty system, the model can produce rules to populate Pega's rule engine.
* **Data movement pipeline in production**
	* TBD
* **Document any substantive changes to customer workflow**
	* TBD

## Communication
* Communication schedule
	* Update provided to stakeholders upon milestone completion, or every 2 weeks.
* Who are the contact persons on both sides?
	* ITD: Danny Godbout
	* Vehicle Divisions: Erich Coit, Doug Gunter, Jake Montero
