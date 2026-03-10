# Selenium Project

This is a repository for Selenium testing.

## Features
- Automated testing for web applications.
- Easy setup and integration.

## Installation
1. Clone the repository.
2. Install dependencies using `pip install -r requirements.txt`.

## Usage
Run your Selenium tests with:
```bash
python test_script.py
```

## Contributing
Contributions are welcome! Fork the repo, make changes, and submit a pull request.

## License
This project is licensed under the MIT License.

SECTION 1: GOLDEN RECORD
  → One row (or a few rows) where ALL rules are valid simultaneously
  → This is the baseline — proves that a fully compliant record exists

SECTION 2: RULE 0 SCENARIOS (while all other rules remain VALID)
  → Valid scenarios for Rule 0 (different valid permutations)
  → Invalid scenarios for Rule 0 ONLY (Rules 1, 2, 3... all still valid)
  → Negative/edge cases for Rule 0 ONLY

SECTION 3: RULE 1 SCENARIOS (while Rule 0 and all other rules remain VALID)
  → Valid scenarios for Rule 1 (different valid permutations)
  → Invalid scenarios for Rule 1 ONLY (Rules 0, 2, 3... all still valid)
  → Negative/edge cases for Rule 1 ONLY

SECTION 4: RULE 2 SCENARIOS (while Rules 0, 1, and all others remain VALID)
  → Valid scenarios for Rule 2
  → Invalid scenarios for Rule 2 ONLY
  → Negative/edge cases for Rule 2 ONLY

... continue for ALL rules ...

SECTION N+1: COMBINATION INVALID SCENARIOS
  → Rows where MULTIPLE rules are invalid simultaneously
  → Examples: Rule 0 + Rule 1 invalid, Rule 5 + Rule 7 invalid, Rule 0 + 1 + 5 + 8 invalid
  → Generate at least 5-10 combination scenarios covering different rule groups

SECTION N+2: APPLICABLE-TO BOUNDARY TESTS
  → For rules with "WHERE" conditions:
    → Row where the WHERE condition IS satisfied (rule should fire)
    → Row where the WHERE condition is NOT satisfied (rule should NOT fire, so even "invalid" values in the tested field should PASS because the rule doesn't apply)


Principle 3: For allowed value lists, generate FULL permutations — not just one example.
If Rule 7 has a mapping with 15 values (CORPORATES→STD, INSTITUTIONS→IRBA, etc.), generate:
	∙	One VALID row for EACH mapping pair (15 valid rows)
	∙	At least 3-4 INVALID rows testing different wrong pairs
	∙	At least 2 NEGATIVE rows (blank/null)
	∙	At least 2 EDGE rows (case/whitespace)
Do NOT just pick one value and call it done.
Principle 4: For uniqueness rules, generate multi-row test sets.
If Rule 0 is a uniqueness constraint on ExpID + TrdTyp + BalShtTyp:
	∙	VALID: 2+ rows where each has a DIFFERENT combination of the 3 key fields
	∙	INVALID: 2 rows with the EXACT SAME combination in all 3 key fields (but different values in non-key fields to show they’re separate records that violate uniqueness)
	∙	EDGE: 2 rows where 2 of 3 key fields match but the third differs (should still PASS)
	∙	EDGE: Same key values but with case differences (e.g., “ABC” vs “abc” — depends on case sensitivity)
Principle 5: Invalid rows should violate EXACTLY ONE rule.
When generating an invalid row for Rule X:
	∙	ONLY the field(s) checked by Rule X should have the violating value
	∙	ALL other fields must satisfy their respective rules
	∙	This ensures that when you run validation, each invalid row fails for a known, specific reason
Exception: The combination invalid scenarios in the final section deliberately violate multiple rules.
Principle 6: Respect the “Applicable To” condition.
If a rule applies only under a WHERE clause (e.g., “WHERE PnlDesc <> OTHER - FAIL”):
	∙	All VALID/INVALID/EDGE test rows for that rule must have PnlDesc set to a value that IS NOT “OTHER - FAIL” (so the rule is applicable)
	∙	ADDITIONALLY generate 1-2 rows where PnlDesc = “OTHER - FAIL” AND the tested field has an “invalid” value → Expected Result should be PASS because the rule doesn’t apply to this row
Principle 7: Use the base data (Axis extract) as the foundation for every row.
	∙	Pick a real row from the Axis exposure extract
	∙	Copy ALL its columns as-is
	∙	Then modify ONLY the specific field(s) needed for the test scenario
	∙	This ensures every test row has realistic values in all columns, not just the tested ones


STEP-BY-STEP EXECUTION ORDER
	1.	First, read the rule catalog completely. List all rules with their types.
	2.	Second, read the Axis exposure extract. Note all column headers and pick 3-5 template rows.
	3.	Third, read all reference/master files. Extract valid value lists from each.
	4.	Fourth, generate the GOLDEN RECORD(s) — rows where every single rule is satisfied.
	5.	Fifth, for each rule in order (Rule 0, then Rule 1, then Rule 2, etc.):
a. Generate all valid permutations (using all allowed values from the rule)
b. Generate invalid scenarios (using wrong-domain, wrong-pair, or values from other rules — NEVER invented values)
c. Generate negative scenarios (blank/null fields)
d. Generate edge cases (case, whitespace, truncation)
e. Generate boundary tests (filter met vs filter not met)
f. In ALL of the above, ensure every OTHER rule remains valid
	6.	Sixth, generate combination invalid scenarios (multiple rules violated in one row).
	7.	Seventh, create the summary sheets.

SELF-CHECK BEFORE DELIVERING
Before giving me the output, verify:
	1.	Does EVERY field value in the test data come from the rule catalog, base data, or reference files? (No invented values like “INVALID_VALUE”, “WRONG”, “TEST123”)
	2.	For each invalid row: is EXACTLY one rule violated (except combination scenarios)?
	3.	For each invalid row: are ALL other rules still satisfied?
	4.	Are all valid permutations covered (all values in allowed lists, all mapping pairs)?
	5.	Do uniqueness rules have multi-row test sets (pairs of duplicates)?
	6.	Do filtered rules have boundary tests (filter met vs filter not met)?
	7.	Are there golden records where ALL rules pass?
	8.	Are there combination scenarios where multiple rules fail?
	9.	Is the cascading order correct (Rule 0 first, then Rule 1 with Rule 0 valid, etc.)?

OUTPUT FORMAT
Excel file with these sheets:
Sheet 1: “Test_Data”
All columns from the Axis exposure extract, PLUS these metadata columns:


