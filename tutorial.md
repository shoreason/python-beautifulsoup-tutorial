# Sho's 20-min Python BeautifulSoup Tutorial

Data analysis starts with getting access to the data and sometimes this data is on the web. For instance you might be interested in extracting data from a list of recipes and indredients on a website. 

Your goal might simply be evaluating the data, like counting the occurences of an item. Or to persist the data for later analysis. This is were a webscraping tool like BeautifulSoup comes in.

BeautifulSoup is a python package that allows you to extract text data from from the web (as HTML) or from XML.

This tutorial will be based off bs4 (Beautiful Soup 4) and assumes you have python and pip installed. The syntax also assumes python3 though most of this should still work as is in python2.

note:bs3 only works with the python2 series, but bs4 works on both python 2 and python 3.

Ready? To get started install bs4

```bash
$ pip install beautifulsoup4
```

---

# Getting to the data

There are several ways to extract the data from the webpage before we start analysis. One of these is using 'requests'. However, in this case we'll use 'urllib'. So after importing beautifulsoup import 'urllib'

```python
from bs4 import BeautifulSoup
import urllib
```
It's up to you if you want to create a special variable for the url you are interested in. The next step will be to pull the text from the url.

```python
url = 'http://www.aflcio.org/Legislation-and-Politics/Legislative-Alerts' # url of whatever page you are interested in
r = urllib.urlopen(url).read()
soup = BeautifulSoup(r)
```

Now we have our text represented as an object. To print out the raw text (html in this case) we use the prettify() directive which shows the full dowloaded document.

```python
print(soup.prettify())
```

---
