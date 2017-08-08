# Sho's 20-min Python BeautifulSoup Tutorial

Data analysis starts with getting access to the data and sometimes this data is on the web. For instance you might be interested in extracting data from a list of recipes and indredients on a website. You could be acquiring data from a file, database or API.   
--
Your goal might simply be evaluating the data, like counting the occurences of an item. Or to persist the data for later analysis. This is where a webscraping tool like BeautifulSoup comes in. 

BeautifulSoup is a python package that allows you to extract text data from from the web (as HTML) or from XML.
--
This tutorial will be based off bs4 (Beautiful Soup 4) and assumes you have python and pip installed.

note:bs3 only works with the python2 series, but bs4 works on both python 2 and python 3.
reference and credit: Thanks to [Vik Paruchuri](https://www.dataquest.io/blog/web-scraping-tutorial-python/) 

Ready? To get started install bs4
--
```bash
$ pip install beautifulsoup4
```
---
# Getting to the data

There are several ways to extract the data from the webpage before we start analysis. One of these is using 'requests'. 

So, after importing beautifulsoup import 'requests'

```python
from bs4 import BeautifulSoup
import requests
```
--
It's up to you if you want to create a special variable for the url you are interested in. The next step will be to pull the text from the url.

```python
url = 'http://forecast.weather.gov/MapClick.php?lat=37.7772&lon=-122.4168#.WYjKidPyvdQ' # url of whatever page you are interested in
page = requests.get(url)
soup = BeautifulSoup(page,'html.parser')
```
--
There are other parsers available other than html.parser (lxml and html5lib).

---

Now we have our text represented as an object. To print out the raw text (html in this case) we use the prettify() directive which shows the full downloaded document.

```python
print(soup.prettify())
```
--

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
--

If you used the url from this walkthrough you should see something like below for your result

```html
<title>National Weather Service</title>
```
--
What about the name of that specific element?
--
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
--
You should see something like this when you run it

```html
<h1 style="font-size: 11pt;">Severe Storms, Heavy Rain and Local Flash Flooding Possible</h1>
```
```css
{'style': 'font-size: 11pt;'}
```
--
Now if I wanted to print the value of the style attribute I run this.

```python
print(soup.h1.attrs['style'])
```
--
For which I get this.

```css
font-size: 11pt;
```
---
# Extracting data we really care about

To find an element or piece of data in the DOM you can simply use 'find'. In the case of this walkthrough we want weather data and we're interested in that section.

By inspecting using web developer on this page we find the section we are interested in and how we can look it up using

```css
id="seven-day-forecast"
```
--
You can pass it that criteria into the find command

```python
seven_day = soup.find(id="seven-day-forecast")
print(seven_day)
```
---

From the results you will notice there are multiple day forecasts and we probably want to grab them all. To grab several elements which match a certain condition we use 'find_all'.

In this next step we'll grab all the forecasts. If you look at previous results you'll notice that all forcasts have this in common: 'class="tombstone-container"'

We'll use this then to grab all the forcasts (it's quite possible that there may be more than one atttibute that is unique).

```python
forecast_items = seven_day.find_all(class_="tombstone-container")
```
--
To take a closer look at this we can grab the first forecast_items element and print out what it looks like

```python
tonight = forecast_items[0]
print(tonight)
```
--
We get this

```html
<div class="tombstone-container">
<p class="period-name">Tonight<br/><br/></p>
<p><img alt="Tonight: Increasing clouds, with a low around 58. West wind 16 to 21 mph decreasing to 10 to 15 mph after midnight. Winds could gust as high as 26 mph. " class="forecast-icon" src="newimages/medium/nbkn.png" title="Tonight: Increasing clouds, with a low around 58. West wind 16 to 21 mph decreasing to 10 to 15 mph after midnight. Winds could gust as high as 26 mph. "/></p>
  <p class="short-desc">Increasing<br/>Clouds</p>
  <p class="temp temp-low">Low: 58 °F</p>
</div>
```
---
# Optimizing Our Data Extraction
Firstly, BeautifulSoup provides a way to use CSS selectors to narrow down our search. You can specify an id and class or multiple classes or combine multiple css attributes.

```python
period_tags = seven_day.select(".tombstone-container .period-name")
```
This says, fetch me all elements that within a tag with class = tombstone-container and and also members of class = period-name
--
Now, rather extract elements one at a time I can apply list comprehension to do this in few steps

```python
periods = [pt.get_text() for pt in period_tags]
print(periods)
```
--
Which gives us 

```json
['Tonight', 'Tuesday', 'TuesdayNight', 'Wednesday', 'WednesdayNight', 'Thursday', 'ThursdayNight', 'Friday', 'FridayNight']
```
---

Now we can repeat this with other elements we are interested in

```python
short_descs = [sd.get_text() for sd in seven_day.select(".tombstone-container .short-desc")]
temps = [t.get_text() for t in seven_day.select(".tombstone-container .temp")]
descs = [d["title"] for d in seven_day.select(".tombstone-container img")]
print(short_descs)
print(temps)
print(descs)
```
--
This gives us the rest of the data we are interested in

```json
['IncreasingClouds', 'Patchy Fogthen Sunny', 'IncreasingClouds', 'Patchy Fogthen Sunny', 'Patchy Fog', 'Patchy Fogthen Sunny', 'Patchy Fog', 'Patchy Fogthen Sunny', 'Patchy Fog']
['Low: 58 °F', 'High: 70 °F', 'Low: 58 °F', 'High: 70 °F', 'Low: 57 °F', 'High: 68 °F', 'Low: 57 °F', 'High: 68 °F', 'Low: 57 °F']
['Tonight: Increasing clouds, with a low around 58. West wind 16 to 21 mph decreasing to 10 to 15 mph after midnight. Winds could gust as high as 26 mph. ', ... , 'Friday Night: Patchy fog after 11pm.  Otherwise, mostly cloudy, with a low around 57.']
```
---
# We can now save the data for later use

---

# Exercise

1. Go to the python wikipedia page: url = https://en.wikipedia.org/wiki/Python_(programming_language)
2. Extract all the url links on that page
3. Save those links to a file

Bonus:
Extract all the data in the 'Summary of Python 3's built-in types' table and save it to a file
