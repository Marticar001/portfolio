1. Import any additional libraries you may need here.
```Python
#!pip install pandas
!pip install bs4
#!pip install requests
```

2. Gather the contents of the webpage in text format using the `requests` library and assign it to the variable`html_data`
```Python
#Write your code here
url = 'https://en.wikipedia.org/wiki/List_of_largest_banks'
html_data = requests.get(url).text
```

3. Question 1:  Print out the output of the following line, and remember it as it will be a quiz question:
```Python
data[101:124]
```
Answer: 'List of largest banks -'

4. Using BeautifulSoup parse the contents of the webpage.
```Python
soup = BeautifulSoup(html_data,"html.parser")
```

5. Load the data from the `By market capitalization` table into a pandas data frame.  The dataframe should have the country `Name` and `Market Cap (US$ Billion)` as column names. Using the empty dataframe `data` and the given loop extract the necessary data from each row and append it to the empty dataframe.
```Python
data = pd.DataFrame(columns=["Name", "Market Cap (US$ Billion)])

for row in soup.find_all('tbody')[3].find_all('tr'):
    col = row.find_all('td')
    
    if len(col) > 0:
        name = col[1].text.strip()
        market_cap = float(col[2].string.strip())
        data = data.append({"Name":name, "Market Cap (US$ Billion)": market_cap}, ignore_index = True)
```

6. Question 3: Display the first five rows using the `head` function
```Python
data.head()
```
