my# Selenium Project

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

Detect rule types from the catalog:
* Uniqueness — logic mentions "unique", "combination", "key", "duplicate"
* Fixed value — logic mentions "must have value", "must equal", "must be"
* Allowed list — multiple allowed values listed in the rule
* Reference lookup — logic mentions "refer", "master", "lookup", "valid codes"
* Cross-field mapping — rule spans multiple columns showing value-to-value pairs

OUTPUT FORMAT
Single Excel sheet. Sort by: Section (GOLDEN = 1, RULE_BY_RULE = 2, APPLIES_TO_TESTS = 3, COMBINATION = 4), then rule_number ascending, then Test_Type: VALID → INVALID → INVALID_OVERLAP → NEGATIVE → EDGE → APPLIES_TO_TEST.
Place these metadata columns at the START of each row, before the base data columns:
* Sequence_No — Running number (1, 2, 3, ...)
* Rule — Which rule this row tests. "ALL" for golden records. "Rule 0 + Rule 5" for combinations.
* Test_Type — GOLDEN / VALID / INVALID / INVALID_OVERLAP / NEGATIVE / EDGE / APPLIES_TO_TEST / COMBINATION
* Expected_Result — PASS (no violation) or FAIL (violation detected)
* Description — One short line: what was tested and why it should pass or fail.
* Adjustment_Reason — For FAIL rows: field changed and from/to what. For VALID and APPLIES_TO_TEST rows: field set and to what. For GOLDEN rows: leave blank.
Then all base data columns follow.

BEFORE DELIVERING — VERIFY
1. Parsed Inputs Summary produced and confirmed before generation
2. Every value from provided files or pattern-based fabrication as last resort — zero placeholders
3. Each invalid row in Section 2 breaks exactly one rule, all other applicable rules satisfied — overlaps marked as INVALID_OVERLAP
4. All applicable rules enforced — no row ignores rules whose applies_to condition it satisfies
5. Allowed values covered up to cap of 10; mapping rules cover each side at least once
6. Uniqueness keys identified from rule text, not guessed — no accidental duplicates
7. Applies-to condition test exists for every filtered rule — applies_to invalidated, valid rule values placed, row passes
8. At least 5 Golden Records with varied combinations
9. Combination scenarios cover different rule groupings
10. Each test row from a different base row where possible — reuse noted in Description
11. Template rows created for any applies_to condition missing from base data — noted in Description
12. Output sorted: Section → rule_number → Test_Type, Sequence_No preserved
13. Rule and Description filled for every row. Adjustment_Reason filled for FAIL, VALID, and APPLIES_TO_TEST rows, blank for GOLDEN.
