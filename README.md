![image](https://github.com/pranav98711/Covid-Analysis-Using-R/assets/58882791/e9fae5ad-a656-4773-91b3-5b264107372d)# Covid Analysis Using R


We have COVID 19 dataset from Kaggle -> https://www.kaggle.com/datasets/sudalairajkumar/novel-corona-virus-2019-dataset/versions/25?resource=download. Let's read this dataset in R

`
data <- read.csv("D:\\Files\\Projects\\Covid19_R\\Data\\COVID19_line_list_data.csv")
`

Taking a quick look at the data:

`
describe(data)
`

This provides us a holistic summary of complete data. We observe that we have 1085 rows and 27 columns.
For this project, we'll analyze two parameters majorly -> 
1. Death
2. Gender

![image](https://github.com/pranav98711/Covid-Analysis-Using-R/assets/58882791/ff4f05e5-0b5c-47dc-a023-cab860cc7672)

On scrolling down, we see 183 are missing genders.

![image](https://github.com/pranav98711/Covid-Analysis-Using-R/assets/58882791/c554a20c-16c2-42b1-b7b9-f7b910a7fd30)

If we scroll to death, we see it has multiple values apart from 0 and 1.

![image](https://github.com/pranav98711/Covid-Analysis-Using-R/assets/58882791/d46d7cd0-8421-437c-887d-c146ad79f0a0)



