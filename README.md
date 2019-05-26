# Todos
#scraping data from wiki cities
import csv
import requests
from bs4 import BeautifulSoup

url = 'https://en.wikipedia.org/wiki/List_of_United_States_cities_by_population'
response = requests.get(url)
html = response.content
print(html)                                                   #fetching url and printing its HTML code
soup = BeautifulSoup(html)
print(soup.prettify())                                        #printing the HTML code in a pretty format
table=soup.find('table',attrs={'class':'wikitable sortable'}) # table is identified by <table> tag
list_of_rows = []
for row in table.findAll('tr'):                               # rows are created using <tr> tag
    list_of_cells = []
    for cell in row.findAll('td'):                            # cells are created using <td> tag
        text = cell.text.replace('&nbsp;', '')
        list_of_cells.append(text)
    list_of_rows.append(list_of_cells)                  # making a list of list of all cell values, because that is how csv is structured

outfile = open("./cities.csv", "w")                     # writing the list into a .csv file
writer = csv.writer(outfile)
writer.writerows(list_of_rows)
 

