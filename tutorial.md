# Sho's 20-min Python BeautifulSoup Tutorial

Data analysis starts with getting access to the data and sometimes this data is on the web. For instance you might be interested in extracting data from a list of recipes and indredients on a website.  

Your goal might simply be evaluating the data, like counting the occurences of an item. Or to persist the data for later analysis. This is were a webscraping tool like BeautifulSoup comes in. You could be acquiring data from a file, database or API. In this case we treat this as getting data from a file.

BeautifulSoup is a python package that allows you to extract text data from from the web (as HTML) or from XML.

This tutorial will be based off bs4 (Beautiful Soup 4) and assumes you have python and pip installed. The syntax also assumes python3 though most of this should still work as is in python2.

note:bs3 only works with the python2 series, but bs4 works on both python 2 and python 3.

Ready? To get started install bs4

```bash
$ pip install beautifulsoup4
```

---

# Getting to the data

There are several ways to extract the data from the webpage before we start analysis. One of these is using 'requests'. Another option is to use 'urllib', which is little more limited. So after importing beautifulsoup import 'requests'

```python
from bs4 import BeautifulSoup
import requests
```
It's up to you if you want to create a special variable for the url you are interested in. The next step will be to pull the text from the url.

```python
url = 'http://forecast.weather.gov/MapClick.php?lat=37.7772&lon=-122.4168#.WYjKidPyvdQ' # url of whatever page you are interested in
page = requests.get(url)
soup = BeautifulSoup(page,'html.parser')
```
There are other parsers available other than html.parser (lxml and html5lib).

Now we have our text represented as an object. To print out the raw text (html in this case) we use the prettify() directive which shows the full dowloaded document.

```python
print(soup.prettify())
```
You can see what this page looks like without being prettified

```python
print(page.content)
```

---

# Exploring the Page and Data

Now that we have the data we can start exploring it. For starters, lets see if we can find the title of the page

```python
print(soup.title)
```
If you used the url from this walkthrough you should see something like below for your result

```html
<title>National Weather Service</title>
```
What about the name of that specific element?

```python
print(soup.title.name)
```
You get this
```text
title
```
---
