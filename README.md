


# Spark Project Odyssey

## Pre-requisits
- Git Bash
- Text file
- Spark
## Steps
1. Get a text file. I used the book "The Odyssey" as the text file from http://classics.mit.edu/Homer/odyssey.mb.txt . Use ```curl "URL for Website" -O "file name to be saved"``` I used ``` curl "http://classics.mit.edu/Homer/odyssey.mb.txt" -O "odyssey.txt"```
2. Clean the data. I used sed commands through git bash to clean the data. ``` sed -E's/b(*option1|option2|...*)\b/*substitute*/g' *yourtxtfile*.txt > *cleanedtxtfile*.txt```
I used
	``` sed -E 's/\b(the|The|be|Be|to|To|of|Of|And|and|a|A|in|In|that|That|have|Have|i|I|it|It|for|For|not|Not|on|On|with|With|he|He|as|As|you|You|do|Do|at|At|this|This|his|His|by|By|from|From|they|They|we|We|say|Say|her|Her|or|Or|an|An|will|Will|my|My|one|One|all|All|would|Would|there|There|their|Their|what|What|so|So|up|Up|out|Out|if|If|about|About|who|Who|get|Get|which|Which|go|Go|me|Me|.)\b/ /g' odyssey.mb.txt > cleanodyssey.txt```
This command helped to eliminate top 50 most common words in english which starts with both lowercase and uppercase letters. I used https://en.wikipedia.org/wiki/Most_common_words_in_English  to get the top 50 most commonly used english words. 
