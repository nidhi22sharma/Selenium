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

Does every field value come from the rule catalog, base data, or reference files? (Zero invented placeholder values)
For each invalid row: is exactly one rule violated? (Except combination scenarios)
For each invalid row: are all OTHER rules still satisfied?
Are all values in every allowed list covered with at least one valid row?
Are all mapping pairs covered with at least one valid row each?
Do uniqueness/dataset-level rules have multi-row test sets?
Do filtered rules (with WHERE conditions) have boundary tests?
Is there at least one golden record where all rules pass?
Are there combination scenarios where multiple rules fail together?
Does the cascading order hold — each rule section keeps all other rules valid?
Are the Violated_Field, Violated_Value, and Correct_Value columns filled for every FAIL row?



