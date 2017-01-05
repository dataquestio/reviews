
# Exploring US Births

## The Dataset

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; The dataset comprises datapoints describing the number of births in the US on specific dates between years 1994 and 2014. The dataset comes from two sources:

* **Centers for Disease Control and Prevention's National Center for Health Statistics (NCHS)**, which provided data for 1994-2003; and
* **Social Security Administration (SSA)**, which provided data for the interval 2000-2014.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; The two datasets were having overlapping datapoints for years 2000, 2001, 2002 and 2003. I have compared the overlapping datapoints and decided to use the ones from the SSA only for several reasons. The values from the SSA were *invariably* bigger. I couldn't think of any reason why the ones from SSA could have had a systematic error in their method of registering births. Furthermore, the differences between the values from the two sources were relatively small, within a range of 60-438 per day and averaging to 223 births per day (rounded to 3 s.f.). Even so, choosing SSA data added a total of 326,511 births to the final dataset, which represents 0.004% (1 s.f.) of the total number of births. In case this increment was wrong, I considered that it could account anyway for all the possible unregistered births over the 21 years covered by the dataset. The main reason why I didn't make an average between the datapoints of the two sources was the lack of variation in the differences between the two, with datapoints from SSA being *always* bigger.


## A Short Analysis Of The Dataset

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; The total number of babies born in the US between 1994 and 2014 is 85,712,738. This means that, on average, 4,081,559 babies were born each year. About 11,175 babies were born each day, which amounts to 466 babies each hour, 8 every minute and one baby born every 7.5s. (all the average integers are rounded to the nearest unit)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; The most prolific year was 2007, during which 4,380,784 were born, whilist only 3,880,894 were born in 1997, the year with the least births. August was the month most babies were born (7,624,626 - in all August months during 1994-2014), whilst February recorded the least, 6,511,697, but this is to understand, considering that it has fewer days. From the 30/31 days months, April recorded least, 6,850,733. If we go 9 months backwards, it means that most succesful conceptions happened in November, whilist the least happened in July. I conjecture that this may be due to the high temperatures of July, which either (i) inhibits people's sexual desire, either (ii) affect negatively the quality of sperm and the process of fertilisation, either (iii) both (i) and (ii). The abstract of these two studies([1][1] and [2][2]) seem to confirm (ii). Also, [some][3] reason that (i) couldn't be the case, because in summer months people tend to wear less clothes on, which naturally makes for a greater sexual desire.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Tuesday is the day of the week most babies were born (14,074,616 in total), whilst Sunday had the least activity, with only 8,368,365 births.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; The greatest number of registered births in a day belongs to a Wednesday, on Sep 9, 2009, when a baby was born in the US every 5.4s, amounting to a 16081 births that day. The least number of births (5,728) in a day was registered in 2011, on the Christmas Day, which was also a Sunday that year. I examined the variations year by year for these two dates and I concluded that it is not the norm for Sep 9 to have such a high number every year, whilst for the Christmas Day low values were a constant across the years, with variations within a low range. 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Another feature of the dataset (which didn't result from my analysis) is represented by the lower than average number of births on each 13th of the month, especially when this is on Friday. This aspect is explored by Carl Bialik from [FiveThirtyEight][4]. He had another approach towards the overlapping values, choosing to average them.

## Some coding details

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; I wrote 5 functions, trying to make them as general as I could, having in mind the purpose of using them later on other datasets:

1. **`csv_read`** - takes as input a .csv file and returns a list of lists of integers; splitting characters can be specified;
2. **`find_totals`** - takes as input a list and returns a dictionary with the dictionary's values representing totals; can be specificied which columns to be represented by the key/value pairs;
3. **`minimax`** - takes as input a dictionary and returns the extremes values of the dictionary along with their keys specified;
4. **`yr_variation`** - takes as input a dictionary with years as keys and returns the difference between a year and a previous one; the difference for the first year is set by default to 0; the starting year can be specified;
5. **`variation_by_yr`** - takes as input a list and returns a dictionary showing the difference between a year and a previous one for certain dates; it can show variation for single-datapoint dates (e.g. Friday, September, the 1st of any month etc.), but also for double-datapoint dates (e.g. Friday 13th, Sep 9, Dec 25 etc.). 


[1]: https://www.ncbi.nlm.nih.gov/pubmed/8875063
[2]: https://www.ncbi.nlm.nih.gov/pubmed/19267606
[3]: http://www.slate.com/articles/life/explainer/2012/07/sex_in_the_summertime_does_hot_weather_make_people_more_likely_to_make_love_.html
[4]: http://fivethirtyeight.com/features/some-people-are-too-superstitious-to-have-a-baby-on-friday-the-13th/



```python
#Defining the functions to be used

def read_csv(file_name, spl_char1 = '\n', spl_char2 = ','):
    split_it = open(file_name).read().split(spl_char1)
    final_list = []
    for row in split_it[1:]:
        split_again = row.split(spl_char2)
        temporary_list = [int(value) for value in split_again]
        final_list.append(temporary_list)
    return final_list        
```


```python
def find_totals(a_list, key = 0, value = 4):
    totals = {}
    for list_member in a_list:
        if list_member[key] in totals:
            totals[list_member[key]] += list_member[value]
        else:
            totals[list_member[key]] = list_member[value]
    return totals
```


```python
def minimax(dictionary):
    counter_max = None
    counter_min = None
    
    for key, value in dictionary.items():
        if counter_max is None or value > counter_max:
            counter_max = value
    for key, value in dictionary.items():
        if counter_min is None or value < counter_min:
            counter_min = value
    
    max_min = {}
    for key, value in dictionary.items():
        if value == counter_max:
            max_min[key] = value
        if value == counter_min:
            max_min[key] = value
    
    return max_min
        
```


```python
def yr_variation(d, start = 1994):
    previous = d[start]  
    dif_by_year = {}
    
    for key, value in d.items():
        difference = value - previous
        dif_by_year[key] = difference
        previous = value
        
    dif_by_year[start] = 0
    
    return dif_by_year 
```


```python
def variation_by_yr(a_list, clmn1 = 1, specific_val1 = 1, clmn2 = 1, specific_val2 = 1, two_values = False ):
    value_across_yrs = []
    for row in a_list:
        if two_values:
            if row[clmn1] == specific_val1 and row[clmn2] == specific_val2:
                value_across_yrs.append(row)
        else:
            if row[clmn1] == specific_val1:
                value_across_yrs.append(row)
     
    value_by_year = {}
    for row in value_across_yrs:
        if row[0] in value_by_year:
            value_by_year[row[0]] += row[4]
        else:
            value_by_year[row[0]] = row[4]
    
    variations = yr_variation(value_by_year)
    
    return variations
```


```python
#Read the two datsets and compared their overlapping datapoints

nchs_data = read_csv('US_births_1994-2003_CDC_NCHS.csv')
ssa_data = read_csv('US_births_2000-2014_SSA.csv')

nchs_data_00_03 = [row for row in nchs_data if row[0] >= 2000]

differences = []
for nchs, ssa in zip(nchs_data_00_03, ssa_data):
    if nchs[0] == ssa[0]:
        diff = ssa[4] - nchs[4]
        differences.append(diff)

max(differences) #438
min(differences) #60
sum(differences) / len(differences)  #223.48459958932239 
sum(differences)
```




    326511




```python
#Merged the two datasets

nchs_94_99 = [row for row in nchs_data if row[0] < 2000]
data_94_14 = nchs_94_99 + ssa_data
```


```python
#Totals

totals_year = find_totals(data_94_14, 0)
totals_month = find_totals(data_94_14, 1)
totals_dmonth = find_totals(data_94_14, 2)
totals_wday = find_totals(data_94_14, 3)
total_overall = sum(totals_wday.values())
```


```python
#Averages

sum(totals_year.values()) # 85,712,738 babies born (bb) during 1994 - 2014
sum(totals_year.values()) / len(data_94_14) # 11,175 bb each day (rounded to the nearest unit)
```




    11175.063624511082




```python
#Minimax

minmax_yr = minimax(totals_year) 
minmax_month = minimax(totals_month) 
minmax_dmonth = minimax(totals_dmonth) 
minmax_wday = minimax(totals_wday)

births = [row[4] for row in data_94_14]
max(births) #16081
min(births) #5728
max_row = [row for row in data_94_14 if row[4] == 16081] # [2009, 9, 9, 3, 16081]
min_row = [row for row in data_94_14 if row[4] == 5728] # [2011, 12, 25, 7, 5728]
```


```python
#Variations by year

variation_yr = yr_variation(totals_year)
variation_9sep = variation_by_yr(data_94_14, 1, 9, 2, 9, True)
variation_xmas = variation_by_yr(data_94_14, 1, 12, 2, 25, True)
```
