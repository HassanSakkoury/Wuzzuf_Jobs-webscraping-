# Wuzzuf_Jobs-webscraping-
import requests
from bs4 import BeautifulSoup
import csv
from itertools import zip_longest
import pandas as pd
result = requests.get('https://wuzzuf.net/search/jobs/?q=python&a=hpb')
src = result.content
soup = BeautifulSoup(src, 'lxml')
job_title = []
company_name = []
company_address = []
job_titles = soup.find_all('h2', {'class': 'css-m604qf'})
com_name = soup.find_all('a', {'class': 'css-17s97q8'})
comp_location = soup.find_all('span', {'class': 'css-5wys0k'})
for i in range(len(job_titles)):
    job_title.append(job_titles[i].text)
print(job_title)    
for i in range(len(com_name)):
    company_name.append(com_name[i].text)
print(company_name)    
for i in range(len(comp_location)):
    company_address.append(comp_location[i].text)
print(company_address)    
final = [job_title, company_name, company_address]
final_res = zip_longest(*final)
with open ('tagme3.csv', 'w') as myfile:
    wr = csv.writer(myfile)
    wr.writerow(['job_title', 'company_name', 'company_address'])
    wr.writerows(final_res)
df = pd.read_csv('tagme3.csv')
df.head()
df['job_title'].value_counts(ascending=False)
df.describe()
