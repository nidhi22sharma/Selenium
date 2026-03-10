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

SECTION 1: GOLDEN RECORDS
  → Rows where ALL rules are valid simultaneously
  → This is the fully compliant baseline

SECTION 2: RULE-BY-RULE SCENARIOS (in order, one rule per section)
  For each rule (in the order they appear in the catalog):
    → Valid permutations: rows covering ALL allowed values/combos for this rule
    → Invalid scenarios: rows violating ONLY this rule, all other rules remain valid
    → Negative scenarios: blank/null in the tested field, all other rules valid
    → Edge cases: case/whitespace/truncation variations, all other rules valid

SECTION 3: APPLICABLE-TO BOUNDARY TESTS
  For each rule that has a WHERE/filter condition:
    → Row where the filter IS satisfied and the rule field is invalid → FAIL
    → Row where the filter is NOT satisfied and the rule field has same "invalid" value → PASS (rule doesn't apply)

SECTION 4: COMBINATION INVALID SCENARIOS
  → Rows where MULTIPLE rules are violated simultaneously
  → Generate at least 5-10 combinations covering different rule groups
  → Include one row where as many rules as possible are violated at once

SECTION 5: ALL-VALID COMPLETE RECORD
  → Final rows confirming all rules pass together (same as golden records, can use different valid value combinations)

Principle 3: Full permutations, not single examples.
	∙	If a rule has an allowed value list with N values, generate a VALID row for EACH of the N values.
	∙	If a rule has a mapping (value A → value B), generate a VALID row for EACH mapping pair.
	∙	For INVALID scenarios, generate at least 3-4 different violation types per rule.
Principle 4: Multi-row test sets for dataset-level rules.
If a rule checks something across multiple rows (like uniqueness/duplicate detection):
	∙	VALID: 2+ rows each with a DIFFERENT combination of key fields
	∙	INVALID: 2 rows with IDENTICAL key field values (proving duplicate detection)
	∙	EDGE: 2 rows where most key fields match but one differs (should pass)
	∙	EDGE: Same key values with case differences (to test case sensitivity)
Principle 5: Invalid rows violate EXACTLY ONE rule.
Each invalid test row should break ONLY the rule being tested. All other fields must satisfy their respective rules. This ensures each failure is traceable to a specific rule.
Exception: Combination scenarios in Section 4 deliberately violate multiple rules.
Principle 6: Respect Applicable-To conditions.
If a rule only applies under a WHERE clause:
	∙	Test rows for that rule must set the filter field so the condition IS met (rule fires)
	∙	Boundary test rows must also include the filter NOT being met (rule should not fire)
Principle 7: Base every row on real data.
	∙	Start each test row as a COPY of a real row from the base data extract
	∙	Modify ONLY the field(s) relevant to the test scenario
	∙	Keep all other columns exactly as they are from the real data


OUTPUT FORMAT
Excel file with these sheets:
Sheet 1: “Test_Data”
All columns from the base data extract, PLUS:

EXECUTION ORDER
	1.	Read the rule catalog completely. Identify all rules, their types, and dependencies.
	2.	Read the base data extract. Note column headers and select 3-5 template rows.
	3.	Read all reference/master files. Extract valid value lists.
	4.	Identify the rule type for each rule:
	∙	Uniqueness: Keywords like “unique”, “combination”, “key”, “duplicate”
	∙	Fixed value: Keywords like “must have value”, “must equal”, “must be”
	∙	Allowed list: Multiple allowed values listed
	∙	Reference lookup: Keywords like “refer”, “master”, “lookup”, “valid codes”
	∙	Cross-field mapping: Two or more columns under one rule showing value pairs
	5.	Generate data section by section in the order defined above.
	6.	Create summary sheets.

SELF-CHECK BEFORE DELIVERING
Verify all of the following:
	1.	Does every field value come from the rule catalog, base data, or reference files? (Zero invented placeholder values)
	2.	For each invalid row: is exactly one rule violated? (Except combination scenarios)
	3.	For each invalid row: are all OTHER rules still satisfied?
	4.	Are all values in every allowed list covered with at least one valid row?
	5.	Are all mapping pairs covered with at least one valid row each?
	6.	Do uniqueness/dataset-level rules have multi-row test sets?
	7.	Do filtered rules (with WHERE conditions) have boundary tests?
	8.	Is there at least one golden record where all rules pass?
	9.	Are there combination scenarios where multiple rules fail together?
	10.	Does the cascading order hold — each rule section keeps all other rules valid?
	11.	Are the Violated_Field, Violated_Value, and Correct_Value columns filled for every FAIL row?




