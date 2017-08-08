# Sho's 20-min Python BeautifulSoup Tutorial

Data analysis starts with getting access to the data and sometimes this data is on the web. For instance you might be interested in extracting data from a list of recipes and indredients on a website. You could be acquiring data from a file, database or API.   

Your goal might simply be evaluating the data, like counting the occurences of an item. Or to persist the data for later analysis. This is where a webscraping tool like BeautifulSoup comes in. 

BeautifulSoup is a python package that allows you to extract text data from from the web (as HTML) or from XML.

This tutorial will be based off bs4 (Beautiful Soup 4) and assumes you have python and pip installed. The syntax also assumes python3 though most of this should still work as is in python2.

note:bs3 only works with the python2 series, but bs4 works on both python 2 and python 3.
credit: Thanks to [Vik Paruchuri](https://www.dataquest.io/blog/web-scraping-tutorial-python/) for reference material for this walkthrough

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
# A Little More Exploring 

Let's see if we can find the first h1 value and what attributes it has

```python
print(soup.h1)
print(soup.h1.attrs)
```
I see when I run it

```html
<h1 style="font-size: 11pt;">Severe Storms, Heavy Rain and Local Flash Flooding Possible</h1>
```
```css
{'style': 'font-size: 11pt;'}
```
Now if I wanted to print the value of the style attribute I run this.

```python
print(soup.h1.attrs['style'])
```
For which I get this.

```css
font-size: 11pt;
```
Time for some real work
---
# Extracting for We Really Care About

To find an element or piece of data in the DOM you can simply use 'find'. In the case of this walkthrough we want weather data and we're interested in that section.

By inspecting using web developer on this page we find the section we are interested in and how we can look it up using

```css
id="seven-day-forecast"
```
You can pass it that criteria into the find command

``python
seven_day = soup.find(id="seven-day-forecast")
print(seven_day)
```
From the results you will notice there are multiple day forecasts and we probably want to grab them all. To grab several elements which match a certain condition we use 'find_all'.

In this next step we'll grab all the forecasts. If you look at previous results you'll notice that all forcasts have this in common: 'class="tombstone-container"'

We'll use this then to grab all the forcasts (it's quite possible that there may be more than one atttibute that is unique).

```python
forecast_items = seven_day.find_all(class_="tombstone-container")
```
To take a closer look at this we can grab the first forecast_items element and print out what it looks like

```python
tonight = forecast_items[0]
print(tonight.prettify())
```
