replace(replace(replace(replace(replace(replace(replace(
'<PASTE-ONE-CARD-HTML-HERE>',
'{{CARD_TAG}}', toUpper(items('Apply_to_each')?['Category'])),
'{{CARD_CONTEXT}}', items('Apply_to_each')?['Context']),
'{{CARD_TITLE}}', items('Apply_to_each')?['Title']),
'{{CARD_BODY}}', items('Apply_to_each')?['Description']),
'{{CARD_RESULT}}', items('Apply_to_each')?['Result']),
'{{CARD_OWNER}}', items('Apply_to_each')?['Owner']),
'{{CARD_IMPACT}}', items('Apply_to_each')?['Impact'])
