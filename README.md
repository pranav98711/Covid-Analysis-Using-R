# Covid Analysis Using R

In this project, we aim to achieve two key objectives:
1. Investigate the claim that older individuals are more susceptible to death from COVID-19
2. Examine the impact of gender on the likelihood of death from COVID-19.

___

We have COVID 19 dataset from Kaggle -> https://www.kaggle.com/datasets/sudalairajkumar/novel-corona-virus-2019-dataset/versions/25?resource=download. Let's read this dataset in R

`
data <- read.csv("D:\\Files\\Projects\\Covid19_R\\Data\\COVID19_line_list_data.csv")
`

Taking a quick look at the data:

`
describe(data)
`

This provides us a holistic summary of complete data. We observe that we have 1085 rows and 27 columns.

![image](https://github.com/pranav98711/Covid-Analysis-Using-R/assets/58882791/c2d42fca-5808-4ed5-9a10-fb8e7b62fcab)


For this project, we'll analyze two parameters majorly -> 
1. Death
2. Gender

![image](https://github.com/pranav98711/Covid-Analysis-Using-R/assets/58882791/ff4f05e5-0b5c-47dc-a023-cab860cc7672)

On scrolling down, we see 183 are missing genders.

![image](https://github.com/pranav98711/Covid-Analysis-Using-R/assets/58882791/c554a20c-16c2-42b1-b7b9-f7b910a7fd30)

If we scroll to death, we see it has multiple values apart from 0 and 1.

![image](https://github.com/pranav98711/Covid-Analysis-Using-R/assets/58882791/d46d7cd0-8421-437c-887d-c146ad79f0a0)


Viewing the data as present in tabular format ->

`
View(data)
`

![image](https://github.com/pranav98711/Covid-Analysis-Using-R/assets/58882791/572fe9e5-1efa-4866-85a9-bb9e22c977dd)


Now, if we see unique values in the death column 

`
unique(data$death)
`

![image](https://github.com/pranav98711/Covid-Analysis-Using-R/assets/58882791/fb791876-0bd0-442e-9e2c-74dc76b8e69c)

We can clean up by transforming the "death" column into a binary indicator.:

`
data$death_dummy <- as.integer(data$death != 0)
`

For validation, we see if there are just 2 unique values present in data or not: 

`
unique(data$death)
`

![image](https://github.com/pranav98711/Covid-Analysis-Using-R/assets/58882791/a9c85f7a-2ed4-4f56-9db9-4427093a7a48)

Now, we can find death rate.


`
sum(data$death_dummy) / nrow(data)
`

![image](https://github.com/pranav98711/Covid-Analysis-Using-R/assets/58882791/ebfbadad-f707-42d4-bd48-aa4786c220d7)

The death rate is computed as the proportion of cases resulting in death, revealing that approximately 5.5% of cases resulted in death.


## AGE
According to media, the average person that dies from corona virus is older than the person who survives. Can we prove if this is correct using out data? 

## claim: people who die are older

`
dead = subset(data, death_dummy == 1)
alive = subset(data, death_dummy == 0)
mean(dead$age)
mean(alive$age)
`

![image](https://github.com/pranav98711/Covid-Analysis-Using-R/assets/58882791/7e272330-23a3-44fb-86ee-b07987108a1c)

Oh, we got NA. Why is that?

If we see the data, some of entries have NA. So, we can recalculate the averages while ignoring these missing values ->


`
mean(dead$age, na.rm = TRUE)
mean(alive$age, na.rm = TRUE)
`

![image](https://github.com/pranav98711/Covid-Analysis-Using-R/assets/58882791/778bf505-a6b6-417c-b3ba-82baa214b1a7)

Difference between alive and dead age is 28. Is this statistically significant?

For this, we will use the t_test statistics
Let's try for confidence interval of 95%

`
t.test(alive$age, dead$age, alternative="two.sided", conf.level = 0.95)
`

![image](https://github.com/pranav98711/Covid-Analysis-Using-R/assets/58882791/e888d942-2771-4796-b19e-f61d49e060db)


We see there's a 95$% chance that the difference between person who is alive and dead is between -24.2 and -16.7. So, we can say on average the person who is alive is much much younger.

Let's try for 99% and we get similar values

`
t.test(alive$age, dead$age, alternative="two.sided", conf.level = 0.99)
`

![image](https://github.com/pranav98711/Covid-Analysis-Using-R/assets/58882791/507ea826-52be-47ef-a08d-bd1a96c3e871)


We can also see that the p-value is 2.2e-16, which is basically 0.
So we can say there's 0% chance the ages in our population are actually equal.


Normally, if p-value < 0.05, we reject null hypothesis. Here, p-value ~ 0, so we reject the null hypothesis and say that people indeed dying from COVID are much older than the ones not dying. We conclude that this is statistically significant



## GENDER
Let's try something similar for the Gender.

Are women more likely to die from COVID or it's vice versa? Let's find out.

## claim: gender has no effect

`
men = subset(data, gender == "male")
women = subset(data, gender == "female")
mean(men$death_dummy, na.rm = TRUE) 
mean(women$death_dummy, na.rm = TRUE)
`

![image](https://github.com/pranav98711/Covid-Analysis-Using-R/assets/58882791/a78c7ed9-3abd-4329-a6c5-8f2bd19f5c63)

We can see that men have a death rate of 8.5% as opposed to 3.6% of women. That is pretty large discrepancy. Is this statistically significant? 
Let's use t-test

`
t.test(men$death_dummy, women$death_dummy, alternative="two.sided", conf.level = 0.99)
`


![image](https://github.com/pranav98711/Covid-Analysis-Using-R/assets/58882791/67f97cad-4fe7-4baf-a247-c06e035957ae)


99% confidence: Men have from 0.8% to 8.8% higher chance of dying.
P-value = 0.002, which is less than 0.05, so we reject the null hypothesis and conclude this is statistically significant
Therefore, the men do have a higher death rate as compared to women



As a conclusion to our objectives:
1. Our analysis indicates a strong correlation between older age and an elevated risk of COVID-19-related mortality.
2. Our findings suggest that gender has a significant impact on the likelihood of death from COVID-19, with men exhibiting a notably higher risk compared to women.
