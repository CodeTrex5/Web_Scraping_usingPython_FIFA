
# Name:- ABHISHEK KUMAR SINHA
# Email:- 20bca014.abhishekkumarsinha@giet.edu

import requests
import re
from bs4 import BeautifulSoup
import pandas as pd

# Parameters

NAME = []
AGE = []
OVA = []
POT = []
TEAM_NAME = []
CONTRACT = []
VALUE= []
WAGE = []
TOTAL_STATS = []


#WESITE LINK LOOP

a = [0, 60, 120, 180, 240, 300, 360, 420, 480, 540]

for i in a:
 url = f'https://sofifa.com/players?offset={i}'
 resp = requests.get(url)
 soup = BeautifulSoup(resp.content, 'lxml')
 
 
#WEB SCRAPING(NAME)

 for i in soup.findAll('a'):
  for j in i.findAll('div',class_="bp3-text-overflow-ellipsis"):
   for name in j:
    re.sub('^<div.*">|</div>|<span>.*</span>|<span.*">|</span>','',str(name))
   NAME.append(name)
   
   
#WEB SCRAPING(AGE)

 for i in soup.findAll('td',class_="col col-ae"):
  for age in i:
   re.sub('','',str(age))
  AGE.append(age)


#WEB SCRAPING(OVA)

 for i in soup.findAll('td',class_="col col-oa"):
   for ova in i:
    ova_string=re.sub('^<span|</span>|class=.*">','',str(ova))
   OVA.append(ova_string)
   
   
#WEB SCRAPING(POT)

 for i in soup.findAll('td',class_="col col-pt"):
   for pot in i:
    pot_string=re.sub('^<span.*">|</span>','',str(pot))
   POT.append(pot_string)  
    
   
#WEB SCRAPING(TEAM NAME)

 for i in soup.findAll('tbody',class_="list"):
  for j in i.findAll('div',"bp3-text-overflow-ellipsis"):
   for k in j.findAll('a'):   
    for team in k:
     team_name=re.sub('^<img.*"/>','',str(team))
     TEAM_NAME.append(team_name)
       
   
#WEB SCRAPING(CONTRACT)


 for i in soup.findAll('div',class_="bp3-text-overflow-ellipsis"):
  for j in i('div',class_="sub"):
   for contract in j:
    string_value=re.sub('^<span.*">.*</span>','',str(contract))
    string_contract=string_value.strip()   
   CONTRACT.append(string_contract)    
    
   
    
#WEB SCRAPING(VALUE)

 for i in soup.findAll('td',class_="col col-vl"):
  for value in i:
   string_value=re.sub('^€','',str(value))
   VALUE.append(string_value)   
   
   
 #WEB SCRAPING(WAGE)

 for i in soup.findAll('td',class_="col col-wg"):
  for wage in i:
   string_value=re.sub('^€','',str(wage))
   WAGE.append(string_value)    
 
    
 #WEB SCRAPING(TOTAL_STATS)

 for i in soup.findAll('td',class_="col col-tt"):
  for total_stats in i:
   string_total_stats=re.sub('^€','',str(total_stats))
   TOTAL_STATS.append(string_value)   
  


  
#FINAL OUTPUT

df = pd.DataFrame({"NAME":NAME,
                   "AGE":AGE,
                   "OVA":OVA,
                   "POT":POT,
                   "TEAM_NAME":TEAM_NAME,
                   "CONTRACT":CONTRACT,
                   "VALUE":VALUE,
                   "WAGE":WAGE,
                   "TOTAL_STATS":TOTAL_STATS
                   })
df.head() 
df.to_csv('FIFA.csv',index=False) #Export the data

