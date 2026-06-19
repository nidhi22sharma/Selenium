Example — Complex Validation Pattern

Sample Schema:

SOURCE_TXN(CONTRACT_ID, VALUE_DATE, AMOUNT, TXN_TYPE, REPORTING_DATE, REPORT_CODE)

TARGET_REPORT(CONTRACT_ID, VALUE_DATE, PAY_AMOUNT, RECEIVE_AMOUNT, TARGET_TYPE, SEQ_NO, REPORTING_DATE, REPORT_CODE)

Natural Language Test Case:

For a given reporting date and report code, validate that source transaction amounts are correctly populated in the target reporting table. Source records should be grouped by CONTRACT_ID and VALUE_DATE. Expected amount is SUM(AMOUNT). If expected amount is negative, PAY_AMOUNT should equal ABS(expected amount) and RECEIVE_AMOUNT should be 0. If expected amount is zero or positive, RECEIVE_AMOUNT should equal expected amount and PAY_AMOUNT should be 0. Only the first target record ordered by SEQ_NO should carry the amount; duplicate target records should have both amounts as 0. If any source record in the group has TXN_TYPE = ‘PRIMARY’, TARGET_TYPE should be ‘PRIMARY’. Return only failed records.

Expected SQL Pattern:

Use CTEs to:

1. Filter source and target by reporting date and report code.
2. Aggregate source by CONTRACT_ID and VALUE_DATE.
3. Derive expected PAY_AMOUNT, RECEIVE_AMOUNT, and TARGET_TYPE using CASE logic.
4. Rank target rows using ROW_NUMBER() over CONTRACT_ID, VALUE_DATE ordered by SEQ_NO.
5. Return only rows where actual target values do not match expected values.
6. Include FAILURE_REASON.

Expected JSON Shape:

{
“query”: “WITH … SELECT … FROM … WHERE …”,
“query_type”: “WITH”,
“tables_used”: [“SOURCE_TXN”, “TARGET_REPORT”],
“columns_used”: [“CONTRACT_ID”, “VALUE_DATE”, “AMOUNT”, “TXN_TYPE”, “REPORTING_DATE”, “REPORT_CODE”, “PAY_AMOUNT”, “RECEIVE_AMOUNT”, “TARGET_TYPE”, “SEQ_NO”],
“validation_strategy”: “Returns target records where aggregated source amount, pay/receive allocation, duplicate handling, or target type population is incorrect.”,
“error”: null,
“confidence_score”: 9,
“reasoning”: “Uses aggregation, CASE logic, and ROW_NUMBER to compare expected source-derived values with target values.”
}
