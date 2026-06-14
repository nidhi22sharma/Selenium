replace(replace(replace(replace(replace(
'<PASTE-ONE-CARD-HTML-HERE>',
'{{CARD_TAG}}', toUpper(items('Apply_to_each')?['Category'])),
'{{CARD_TITLE}}', items('Apply_to_each')?['Submission']),
'{{CARD_BODY}}', items('Apply_to_each')?['Description']),
'{{CARD_RESULT}}', items('Apply_to_each')?['KeyMetrics/Impact']),
'{{CARD_OWNER}}', items('Apply_to_each')?['Submitter'])
