

```python
# Exploring gun violence 
import csv
f = open("guns.csv")
r = csv.reader(f)
data = list(r)
data[0:2]
```




    [['',
      'year',
      'month',
      'intent',
      'police',
      'sex',
      'age',
      'race',
      'hispanic',
      'place',
      'education'],
     ['1',
      '2012',
      '01',
      'Suicide',
      '0',
      'M',
      '34',
      'Asian/Pacific Islander',
      '100',
      'Home',
      '4']]




```python
headers = data[0]
data = data[1:]
data[0:5]
```




    [['1',
      '2012',
      '01',
      'Suicide',
      '0',
      'M',
      '34',
      'Asian/Pacific Islander',
      '100',
      'Home',
      '4'],
     ['2', '2012', '01', 'Suicide', '0', 'F', '21', 'White', '100', 'Street', '3'],
     ['3',
      '2012',
      '01',
      'Suicide',
      '0',
      'M',
      '60',
      'White',
      '100',
      'Other specified',
      '4'],
     ['4', '2012', '02', 'Suicide', '0', 'M', '64', 'White', '100', 'Home', '4'],
     ['5',
      '2012',
      '02',
      'Suicide',
      '0',
      'M',
      '31',
      'White',
      '100',
      'Other specified',
      '2']]




```python
# Counting number of gun violence acts in each year
years = [i[1] for i in data]
year_counts = {}
for year in years:
    if year not in year_counts:
        year_counts[year] = 1
    else:
        year_counts[year] += 1
        
year_counts
```




    {'2012': 33563, '2013': 33636, '2014': 33599}




```python
#Counting gun deaths by year and month
import datetime
dates = [datetime.datetime(year=int(row[1]), 
                           month=int(row[2]), day=1) for row in data]  
dates[0:5]
```




    [datetime.datetime(2012, 1, 1, 0, 0),
     datetime.datetime(2012, 1, 1, 0, 0),
     datetime.datetime(2012, 1, 1, 0, 0),
     datetime.datetime(2012, 2, 1, 0, 0),
     datetime.datetime(2012, 2, 1, 0, 0)]




```python
date_counts = {}
for date in dates:
    if date not in date_counts:
        date_counts[date] = 1
    else:
        date_counts[date] += 1
       
```


```python
# Exploring gun deaths by gender
sex_counts = {}
race_counts = {}
for row in data:
    sex = row[5]
    race = row[7]
    if sex not in sex_counts:
        sex_counts[sex] = 1
    else:
        sex_counts[sex] += 1
    if race not in race_counts:
        race_counts[race] = 1
    else:
        race_counts[race] += 1    
        
print(sex_counts)
print ("")
print(race_counts)       


##Findings 
#Gun deaths in the US seem to disproportionately affect men vs women. They also seem to disproportionately affect minorities, although having some data on the percentage of each race in the overall US population would help.
#There appears to be a minor seasonal correlation, with gun deaths peaking in the summer and declining in the winter. It might be useful to filter by intent, to see if different categories of intent have different correlations with season, race, or gender.
```

    {'F': 14449, 'M': 86349}
    
    {'White': 66237, 'Native American/Native Alaskan': 917, 'Hispanic': 9022, 'Asian/Pacific Islander': 1326, 'Black': 23296}



```python
# Exploring proportion of gun deaths per 100,000 people per race.
# Reading in a second data set
c = open("census.csv")
d = csv.reader(c)
census_data = list(d)
census_data[0:1]

```




    [['Id',
      'Year',
      'Id',
      'Sex',
      'Id',
      'Hispanic Origin',
      'Id',
      'Id2',
      'Geography',
      'Total',
      'Race Alone - White',
      'Race Alone - Hispanic',
      'Race Alone - Black or African American',
      'Race Alone - American Indian and Alaska Native',
      'Race Alone - Asian',
      'Race Alone - Native Hawaiian and Other Pacific Islander',
      'Two or More Races']]




```python
census_data = census_data[1:]
census_data[0:5]
```




    [['cen42010',
      'April 1, 2010 Census',
      'totsex',
      'Both Sexes',
      'tothisp',
      'Total',
      '0100000US',
      '',
      'United States',
      '308745538',
      '197318956',
      '44618105',
      '40250635',
      '3739506',
      '15159516',
      '674625',
      '6984195']]




```python
# Mapping values from census 
mapping= {"Hispanic": 44618105, 
          "White": 197318956,
          "Black": 40250635,
        "Asian/Pacific Islander": 15159516 + 674625,
        "Native American/Native Alaskan": 3739506,
          }

    
```


```python
#Linking values from census dictionary to data dictionary race_counts
race_per_hundredk = {}
for k, v in race_counts.items():
    race_per_hundredk[k] = (v/mapping[k]) * 100000
print ("Gun deaths per 100,000 people by race")    
race_per_hundredk    
```

    Gun deaths per 100,000 people by race





    {'Asian/Pacific Islander': 8.374309664161762,
     'Black': 57.8773477735196,
     'Hispanic': 20.220491210910907,
     'Native American/Native Alaskan': 24.521955573811088,
     'White': 33.56849303419181}




```python
##Exploring gun deaths by intent to understand gun related murder rate per thousand people
intents = [row[3] for row in data]
races = [row[7] for row in data]
homicide_race_counts = {}
for i, race in enumerate(races):
    if intents[i] == "Homicide":
        if race not in homicide_race_counts:
            homicide_race_counts[race] = 1
        else:
            homicide_race_counts[race] += 1
print("Homicide counts for all races")            
homicide_race_counts            
            
```

    Homicide counts for all races





    {'Asian/Pacific Islander': 559,
     'Black': 19510,
     'Hispanic': 5634,
     'Native American/Native Alaskan': 326,
     'White': 9147}




```python
homicide_per_hundredk = {}
for k, v in homicide_race_counts.items():
    homicide_per_hundredk[k] = (v/mapping[k]) * 100000
    
print("Homicide rate per 100,000 people for all races") 
print(homicide_per_hundredk)    
```

    Homicide rate per 100,000 people for all races
    {'White': 4.6356417981453335, 'Asian/Pacific Islander': 3.530346230970155, 'Hispanic': 12.627161104219914, 'Native American/Native Alaskan': 8.717729026240365, 'Black': 48.471284987180944}



```python
## Findings
# It would appear that the Homicide rate per race per 100k people 
# is significantly higher among the Black community, ~49% than any
# other race. Hispanic community with a 12% rate stands out as well.

```


```python
## Exploring link between month and homicide rate 
months = [row[2] for row in data]
intents = [row[3] for row in data]
month_homicide = {}
for i, month in enumerate(months):
    if intents[i] == "Homicide":
        if month not in month_homicide:
            month_homicide[month] = 1
        else:
            month_homicide[month] += 1
            
print("Homicide counts for all months for all races")            
month_homicide           
```

    Homicide counts for all months for all races





    {'01': 2829,
     '02': 2178,
     '03': 2780,
     '04': 2845,
     '05': 2976,
     '06': 3130,
     '07': 3269,
     '08': 3125,
     '09': 2966,
     '10': 2968,
     '11': 2919,
     '12': 3191}




```python
#We observe that July had the highest number of Homicides. Except for February, all the other months
# had homicide counts fairly close to each other.
```


```python
## Exploring homicide rate by gender
homicide_gender = {}
genders = [row[5] for row in data]
for i, gender in enumerate(genders):
    if intents[i] == "Homicide":
        if gender not in homicide_gender:
            homicide_gender[gender] = 1
        else:
            homicide_gender[gender] += 1
print("Homicide counts by gender")            
print(homicide_gender)   
print("Overall deaths by gender")
print(sex_counts)            
        
```

    Homicide counts by gender
    {'F': 5373, 'M': 29803}
    Overall deaths by gender
    {'F': 14449, 'M': 86349}



```python
# As we can observe, There are 5 times as many male victims as females 
# But that does not mean that homicide rates are higher for men as well. 
# We need to do further analysis.

#From our sex counts dictionary, we know that male gun deaths in that 
# time period were : 
homicide_rate_gender = {}

for k, v in homicide_gender.items():
    homicide_rate_gender[k] = (v/sex_counts[k])*100
print("Homicide rate by gender") 
print(homicide_rate_gender)    

```

    Homicide rate by gender
    {'F': 37.18596442660392, 'M': 34.51458615618016}



```python
#We observe that 37% of all gun deaths in female population were homicides.
#The number is slightly lower at 35% for males. 
```


```python
#Now lets explore the "Accidental" rate using the same logic above
acc_gender = {}
genders = [row[5] for row in data]
for i, gender in enumerate(genders):
    if intents[i] == "Accidental":
        if gender not in acc_gender:
            acc_gender[gender] = 1
        else:
            acc_gender[gender] += 1
print("Accidental death counts by gender")            
print(acc_gender)   
print("Over deaths by gender")
print(sex_counts) 
```

    Accidental death counts by gender
    {'F': 218, 'M': 1421}
    Over deaths by gender
    {'F': 14449, 'M': 86349}



```python
acc_rate_gender = {}

for k, v in acc_gender.items():
    acc_rate_gender[k] = (v/sex_counts[k])*100
print("Accidental death rate by gender") 
print(acc_rate_gender)    

```

    Accidental death rate by gender
    {'F': 1.508754931137103, 'M': 1.64564731496601}



```python
# The accidental death rate is very low at ~1.5% for both sexes, much much lower
# than homicide rate. 

```


```python
# Now lets us explore Accidental deaths by race
intents = [row[3] for row in data]
races = [row[7] for row in data]
acc_race_counts = {}
for i, race in enumerate(races):
    if intents[i] == "Accidental":
        if race not in acc_race_counts:
            acc_race_counts[race] = 1
        else:
            acc_race_counts[race] += 1
print("Accidental death counts for all races")            
print(acc_race_counts)            
            
```

    Accidental death counts for all races
    {'White': 1132, 'Native American/Native Alaskan': 22, 'Hispanic': 145, 'Asian/Pacific Islander': 12, 'Black': 328}



```python
acc_per_hundredk = {}
for k, v in acc_race_counts.items():
    acc_per_hundredk[k] = (v/race_counts[k]) * 100000
    
print("Accidental death rates per 100000 people for all races") 
print(acc_per_hundredk)
```

    Accidental death rates per 100000 people for all races
    {'White': 1709.0145990911424, 'Asian/Pacific Islander': 904.9773755656109, 'Hispanic': 1607.1824429173132, 'Native American/Native Alaskan': 2399.1275899672846, 'Black': 1407.967032967033}



```python
## Exploring gun death rates by location
places = [row[9] for row in data]
places[0:5]
```




    ['Home', 'Street', 'Other specified', 'Home', 'Other specified']




```python
place_death = {}
for place in places:
    if place not in place_death:
        place_death[place] = 1
    else:
        place_death[place] += 1
print("Death counts by location")
print(place_death)
```

    Death counts by location
    {'School/instiution': 671, 'Industrial/construction': 248, 'Residential institution': 203, 'Home': 60486, 'NA': 1384, 'Sports': 128, 'Farm': 470, 'Other unspecified': 8867, 'Other specified': 13751, 'Street': 11151, 'Trade/service area': 3439}



```python
# we observe highest death counts in Home
# Now lets look at death counts as they relate to education
edu = [row[10] for row in data]
```


```python
edu_death = {}
for level in edu:
    if level not in edu_death:
        edu_death[level] = 1
    else:
        edu_death[level] += 1
        
edu_death        
```




    {'1': 21823, '2': 42927, '3': 21680, '4': 12946, '5': 1369, 'NA': 53}




```python
# As we can see, very high rates of gun deaths among lower education group. Highest among
# people with only High school graduate or equivalent credentials.
# Lets explore causes of death among this group '2'

death_cause_level_2 = {}
for i, intent in enumerate(intents):
    if edu[i] == '2':
        if intent in death_cause_level_2:
            death_cause_level_2[intent] += 1
        else:
            death_cause_level_2[intent] = 1
death_cause_level_2            




```


```python
# Sdeath_cause_level_2uicide takes the top spot for those who only have High school 
#graduate or equivalent education. Homicide number is pretty high too.
```
