Gaon Kamai
Village Income Hub

Helping rural youth find viable income opportunities without leaving their villages.

Show Image Show Image Show Image Show Image


Table of Contents
Overview
Problem Statement
Solution
Key Features
System Architecture
Tech Stack
Getting Started
Usage
Matching Methodology
Data Model
Customising the Data
Sample Output
Limitations
Roadmap
Impact Measurement
Author
Overview

Gaon Kamai is a lightweight, explainable matching tool that connects rural youth with income opportunities they can start in or near their own village. A user answers seven simple questions, and the system returns a ranked shortlist with expected income, startup cost, training options, funding schemes, and the reason each opportunity was recommended.

This project was developed in response to the topic:

How can rural youth find viable income opportunities without leaving their villages?

Problem Statement

Rural youth often migrate to cities not because work does not exist locally, but because:

They cannot see the opportunities available around them.
They cannot connect with buyers, trainers, or funding sources.
They have no guidance on how to take the first step.

Gaon Kamai addresses this information and guidance gap.

Solution

The system converts a youth's profile (education, skills, budget, available time, smartphone access) into a ranked list of practical options across three income streams:

Stream	Examples
Digital work	Online tutoring, data entry, CSC services, freelance translation
Local micro-enterprise	Dairy, poultry, mushroom farming, tailoring, food processing, mobile repair
Local services	Solar pump repair, electrician work, drone spraying, produce transport

For every recommendation, the user also sees the training source and funding scheme that can help them start.

Key Features
Simple intake: seven questions, designed for low-literacy and low-bandwidth settings.
Ranked recommendations: top three matches with a score out of 100.
Explainable results: each match includes a plain-language reason.
Practical pathway: income range, startup cost, training, and funding in one view.
Persistent records: every profile is stored in a local SQL database.
Outcome tracking ready: an outcomes table supports follow-up impact measurement.
Zero dependencies: runs on standard Python with no installation steps.
System Architecture
   +-----------------+      +---------------------+      +------------------+
   |  Youth profile  | ---> |   Eligibility       | ---> |  Weighted        |
   |  (7 questions)  |      |   filters           |      |  scoring engine  |
   +-----------------+      |  - education        |      +--------+---------+
                            |  - smartphone       |               |
                            +---------------------+               v
   +-----------------+                                  +------------------+
   |  SQLite database| <--------------------------------|  Top 3 ranked    |
   |  (4 tables)     |   profiles saved                 |  recommendations |
   +-----------------+                                  +------------------+
Tech Stack
Layer	Technology
Language	Python 3.8+
Database	SQLite (built-in sqlite3 module)
Interface	Command-line (console)
External libraries	None
Getting Started
Prerequisites
Python 3.8 or newer
Installation
bash
git clone <your-repository-url>
cd gaon_kamai

No further installation is required. The database file gaon_kamai.db is created automatically on first run and loaded with the sample opportunities.

Usage

Interactive mode

bash
python gaon_kamai.py

Demo mode (runs with a built-in sample profile)

bash
python gaon_kamai.py --demo
Input fields
Field	Description
Name	Name of the youth
Village / Block	Location (stored with the profile)
Education	8th, 10th, 12th, or graduate
Skills	Comma-separated, chosen from the displayed list
Budget	Amount available to invest, in rupees
Daily hours	Hours per day available for work
Smartphone	y or n
Matching Methodology

Matching is deliberately rule-based and transparent, so every result can be explained to the user and audited by reviewers.

Step 1: Eligibility filters

An opportunity is removed before scoring if:

the youth's education is below the minimum required, or
the opportunity requires a smartphone and the youth does not have one.
Step 2: Weighted score (out of 100)
Factor	Weight	Calculation
Skill match	40	Share of required skills the youth already has
Budget fit	25	Full score if within budget; reduced proportionally if above
Local demand	20	Demand rating (1-5) in the block, scaled to 20
Time fit	10	Full score if daily hours are sufficient; otherwise proportional
Low-cost bonus	5	Awarded when startup cost is Rs 50,000 or less

Results are sorted by score and the top three are shown.

Data Model
Table	Purpose	Key fields
opportunities	Catalogue of income opportunities	name, category, min_edu, skills, startup_cost, income range, hours_needed, training, funding
local_demand	Demand rating per opportunity per block	opportunity_id, block, demand (1-5)
youth_profiles	Profiles of users	name, block, edu, skills, budget, hours, smartphone
outcomes	Follow-up results for impact tracking	youth_id, opportunity_id, started, monthly_income, checked_on
Customising the Data
Open gaon_kamai.py.
Edit the SEED list to add or modify opportunities.
Edit the DEMAND dictionary using ratings from your own village or block survey.
Delete gaon_kamai.db and run the program again to load the new data.
Sample Output
Demo profile: 12th pass, skills=['computer', 'typing', 'english'], budget=Rs 20,000, 4 hrs/day

===== Aapke liye Top opportunities =====

1. Data entry / typing services  [Digital]  - Score: 92.0/100
   Kamai (mahine): Rs 4,000 - Rs 10,000
   Shuruaati kharch: Rs 3,000
   Training: PMKVY Computer Basics
   Funding: None needed
   Kyun: aapki skills match (computer, typing); budget ke andar

2. CSC / online form-filling centre  [Digital]  - Score: 87.7/100
   Kamai (mahine): Rs 6,000 - Rs 18,000
   Shuruaati kharch: Rs 25,000
   Training: CSC Academy
   Funding: Mudra Shishu loan
   Kyun: aapki skills match (computer, typing); loan/subsidy ki zaroorat padegi;
         aapke block mein demand high hai

3. Online tutoring (school students)  [Digital]  - Score: 69.3/100
   Kamai (mahine): Rs 4,000 - Rs 12,000
   Shuruaati kharch: Rs 2,000
   Training: Skill India Digital / DIKSHA
   Funding: None needed
   Kyun: aapki skills match (english); budget ke andar; aapke block mein demand high hai
Limitations

This is a prototype. Please note:

Sample data: opportunity details, income ranges, and demand ratings are illustrative estimates. They must be replaced with verified, survey-based data from the target block before real-world use.
Single-block demand: demand is stored for one sample block. The village or block entered by the user is saved but not yet used to filter recommendations.
Exact-word skills: skills must be entered using the words shown in the on-screen list.
Console interface: suitable for volunteer-assisted use; not yet suitable for direct use by most villagers.
No automated tests: validation so far is limited to manual demo runs.
Roadmap
 Block-wise demand filtering using real survey data
 Hindi (Devanagari) interface
 Telegram / WhatsApp bot for low-bandwidth phones
 Follow-up module to record results at 30 and 60 days
 Local service request board (villagers post needs; trained youth respond)
 Unit tests for the scoring engine
 Predictive model for opportunity success, once sufficient outcome data exists
Impact Measurement

The following metrics are planned for a pilot in one block:

Metric	Definition
Reach	Number of youth profiled and matched
Activation	Share who start an income activity within 60 days
Earnings	Average monthly income earned
Local value	Income retained within the village economy
Author

Kapil Baghel B.Tech, Computer Science & Engineering NRI Institute of Information Science and Technology, Bhopa
