


# Spark Project Odyssey

## Pre-requisits
- Git Bash
- Text file
- Spark
## Steps
1. Get a text file.
 I used the book "The Odyssey" as the text file from http://classics.mit.edu/Homer/odyssey.mb.txt . Use ```curl "URL for Website" -O "file name to be saved"``` I used ``` curl "http://classics.mit.edu/Homer/odyssey.mb.txt" -O "odyssey.txt"```
2. Clean the data.
 I used sed commands through git bash to clean the data. ``` sed -E's/b(*option1|option2|...*)\b/*substitute*/g' *yourtxtfile*.txt > *cleanedtxtfile*.txt```
I used
	``` sed -E 's/\b(the|The|be|Be|to|To|of|Of|And|and|a|A|in|In|that|That|have|Have|i|I|it|It|for|For|not|Not|on|On|with|With|he|He|as|As|you|You|do|Do|at|At|this|This|his|His|by|By|from|From|they|They|we|We|say|Say|her|Her|or|Or|an|An|will|Will|my|My|one|One|all|All|would|Would|there|There|their|Their|what|What|so|So|up|Up|out|Out|if|If|about|About|who|Who|get|Get|which|Which|go|Go|me|Me|.)\b/ /g' odyssey.mb.txt > cleanodyssey.txt```
This command helped to eliminate top 50 most common words in english which starts with both lowercase and uppercase letters. I used https://en.wikipedia.org/wiki/Most_common_words_in_English  to get the top 50 most commonly used english words. 
Here I have replaced these words with space (" ") so, in the end I will not be considering space (" ") as an output of word count. We will also not take punctuation as words.
3. Process the data with spark.
-  You should already have spark in your machine. If not go to [https://github.com/denisecase/setup-spark](https://github.com/denisecase/setup-spark) by Denise Case to complete the spark set up.
- Open PowerShell in the folder where you have the cleaned text. Then write ``` spark-shell```. This will open the spark with scala.
- Then use the code below to create a RDD  with the cleaned file that we created.
```val ordd = sc.textFile("cleanodyssey.txt")``` 
- Then split the data and save it using the code below.
```val splitordd=ordd.flatMap(line=> line.split(" "));```
- After spliting the data we need to count the words, So now we are mapping by giving each ward a key value of 1 so that we can count them. To do this use the code below.
```val mapordd=splitordd.map(word=>(word,1));```
- As we have mapped by giving 1 to each word, now we will count the repetation of each word with the code below.
```val reduceordd = mapordd.reduceByKey(_+_);```
This reduces the mapped data to single word and counts on the number that we gavw to each word while we were mapping.
- Now, let's short these data in asscending order to get the number of words repeated in ascending order with the code below.
```val sortordd=reduceordd.sortBy(_._2, false)```
- After shorting, let's take a look at what we got by using the code below.
```sortordd.take(10)```
As a result we get
```res1: Array[(String, Int)] = Array(("",92554), (,,920), (was,917), (had,830), (is,742), (him,729), (but,702), (your,617), (are,504), (she,481))```
Here we can see space(" ") has been repeatade in the highest number which is followed by comma(,). As they are not words and as we stated in the begining that we will not be considering space and punctuations as words. So, we will take 12 words to ignore space and comma by using the code below.
```sortordd.take(12)```
Now i get the following as output where we can see top 10 words been used in The Odyssey after ignoring top 50 mostly used words in english, comma and space.
```res2: Array[(String, Int)] = Array(("",92554), (,,920), (was,917), (had,830), (is,742), (him,729), (but,702), (your,617), (are,504), (she,481), (them,474), (were,459))```
4. Export and process data in excel.
- Now we are done with processing the data in spark. So, let's export the data by using the code below.
``` sortordd.saveAsTextFile("wordcountoutput")```
- After that we can take the data in excel and create the table with the data. In excel enter data word and count in separate columns to create the table.
- select the top ten data in both columns and create a bargraph by going to insert and click on bargraph as you like.
![Bargraph](https://github.com/susanmaharjan/spark-project-odyssey/blob/main/Screenshot%20(45).png)
