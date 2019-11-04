## install bs4 (Beautiful Soup 4) package if needed
from bs4 import BeautifulSoup as soup
from urllib.request import urlopen

## establish connection and download html code
client = urlopen("https://www.census.gov/programs-surveys/popest.html")
page_html = client.read()
client.close()
page_soup = soup(page_html, "html.parser")

## find all links in code
links = []
for link in page_soup.find_all('a', href=True):
    links.append(link['href'])

## add prefix to intended links
links1 = []
for link in links:
    if link[0] == "/" and link[-4:-1] == "htm":
        links1.append(str("https://www.census.gov") + link)
    else:
        links1.append(link) 

## erase trailing "/"'s
links2 = []
for link in links1:
  if 'https://' in link:
    links2.append(link)
links3 = []
links4 = []
for link in links2:
    if link[-1] == '/':
        llink = list(link)
        del (llink)[-1]
        links3 += "".join(llink) + ","
    else:
        links3 += link + ","
links4 = "".join(links3)
links5 = links4.split(",")
links6 = [ ]
for link in links5:
  if link[0:5] == "https":
      links6.append(link)

##find unique links
links7 = list(set(links6))


## Install Numpy and Pandas if needed
import numpy as np
import pandas as pd

## get today's date
import numpy as np
import pandas as pd

from datetime import date
today = str(date.today())
date = [today] * len(links7)

## create a dataframe and write .csv file
df = pd.DataFrame([date, links7], index = ["date","link"] )
df2 = df.transpose()
df2.to_csv("US_Census_Links.csv") ##rename as desired


    