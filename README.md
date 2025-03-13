# Search-Query-Mapping

1. My approach to labeling the dataset (both manually and with automation):
Python Code for Labeling: I wrote Python code to label the dataset, and the code is included in the attached Excel file.
Lists for automation: I created the not_item, brand, determiner, and ingredient lists using ChatGPT.
Additionally, regularly creating and updating these lists would be helpful for future labeling tasks. The reason for creating the “not” item list was due to the large number of items; ChatGPT was more effective in generating this list than the item list. I also included the brand and determiner lists in the not item list, as entries containing only a brand name or a determiner indicate that the entry is not an item.

Normalization: I normalized these lists and the original dataset entries by converting all characters to lowercase and removing spaces to avoid mismatches during comparisons.
Exact match or check if the entry contains:
- Item: For the not_item list, I used exact matching, as item entries could contain adjectives or brand names.
- Brand: For the brand and determiner lists, I checked if the original entries contained these elements.
- Determiner: Since determiners could appear multiple times, I joined them with commas. This approach could later be used for coding by parsing the labels with a comma separator.
- Ingredient: I used partial matching for ingredient labeling due to the high number of entries containing ingredients. While this approach resulted in some errors (for example, “orange cake” being incorrectly classified), the number of such cases was small. Partial matching improved overall accuracy, and any incorrect entries were manually fixed.
After creating the labels in Python, I exported my output to a separate sheet and used a VLOOKUP to bring the results back into the original dataset.

 Multi Keyword: I used an Excel formula to quickly detect whether entries contained more than one word:
=IF(LEN(A3)-LEN(SUBSTITUTE(A3," ",""))>0,TRUE,FALSE)
Prepositions: Since the number of prepositions was small, I detected and added prepositions using an Excel formula. I also added the “de” preposition, which is Spanish or French.
- To detect if the entry contains a preposition:
=IF(OR(REGEXMATCH(A3," to "),REGEXMATCH(A3," of "),REGEXMATCH(A3," for "),REGEXMATCH(A3," de ")),TRUE,FALSE)
- To bring the preposition:
=IF(REGEXMATCH(A3," to "),"to",IF(REGEXMATCH(A3," of "),"of",IF(REGEXMATCH(A3," for "),"for",IF(REGEXMATCH(A3," de "),"de",""))))
Detecting language: I used the Excel formula to detect the language: =DETECTLANGUAGE()
Comments:
- Not logical entry: Automatically write a comment if the entry is not an item, brand, or ingredient:
=IF(AND(B3=FALSE, C3=FALSE,D3=FALSE), "not an item, brand or ingredient", "")
- Typos: For entries with typos like “handfree”, “jucie”, “carefree”, “suger free”, etc., I handled
them manually.
- Brand names: Terms like 'double dare', and 'double up' are brand names, not determiners. I also
added this information to the comment.
Manual correction: As part of the case study, automation achieved an accuracy of 95.49% compared to manual work. The manual corrections further improved the overall accuracy. During manual corrections, I also updated the relevant lists to ensure consistency and aid future work. I also ensured that terms like “double” and “royal” were not mistakenly marked as determiners but as brand names.

 2. If the dataset were scaled to 10,000 queries:
- For the Python code, using functions like isin(), contains(), and avoiding for loops would be an
efficient approach.
- For larger datasets, I could refer to Delivery Hero’s food list and filter out non-ingredients from
food-labeled entries, as non-ingredients are easier to identify when viewing all foods in a list.
- Instead of processing the entire dataset at once, I would divide it into smaller batches.
- I would look for patterns to label entries directly. For example, if the entry is an ingredient, it is
directly an item. Another example is that entries containing only a brand name or a determiner indicate that the entry is not an item.

