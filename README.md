# ScrapySimpleTutorial
A simple tutorial for [scrapy](https://scrapy.org) the crawler.

Assume you have a Mac OS and have python installed.

## Make new working directory
Open terminal. Use the following command to make a directory `scrapy_test` and `change_directory` to the `scrapy_test`.
```
$ mkdir scrapy_test
$ cd scrapy_test
```

## Install Scrapy
```
$ pip install Scrapy
```

## Create Scrapy project
```
$ scrapy startproject quote_project
```
Here the `quote_project` in the name of the project.

Now you have folder called `quote_project`. Now we enter the `quote_project` folder and create a spider for a specific website.
```
$ cd quote_project
$ scrapy genspider quotes quotes.toscrape.com
```
Now we have a file called `quotes.py` in the `quote_project/spider` folder. It should looks the same as
```python
# -*- coding: utf-8 -*-
import scrapy


class QuotesSpider(scrapy.Spider):
    name = 'quotes'
    allowed_domains = ['quotes.toscrape.com']
    start_urls = ['http://quotes.toscrape.com/']

    def parse(self, response):
        pass
```
The `name = 'quotes'` is the name of the spider. It will be used when we want run the spider.

The `allowed_domains` is a list of domains that we are interested. Any websites don't have one of these domain names will be ignored.

## Settings
Open file `quote_project/settings.py`.

If you cannot find the `robots.txt` file in the website you want to collect, modify
```python
ROBOTSTXT_OBEY = True
```
to
```python
ROBOTSTXT_OBEY = False
```
For example, [`www.google.com/robots.txt`](https://www.google.com/robots.txt) will show you the allowed and disallowed rules.
Whereas []() doesn't have any files there. You may want to search for the rules of the website you are working on to best obey the rules.

Modify
```python
#DOWNLOAD_DELAY = 3
```
to
```python
DOWNLOAD_DELAY = 3
```
This means wait 3 seconds between two requests so that the server of the website won't be too busy.

## Spider
Now we can go to spider `quotes.py` file.

We need to modify the `parse` function to do the work.
```python
def parse(self, response):
    pass
```
When you run this spider, the pares function will be called, and the parameter `response` is a response of the request of `http://quotes.toscrape.com/` (this is the start_urls).
```python
class QuotesSpider(scrapy.Spider):
    name = 'quotes'
    allowed_domains = ['quotes.toscrape.com']
    start_urls = ['http://quotes.toscrape.com/']
```
If there are many websites in `start_urls`, this function will be called many times.

To understand what is the response, we need to use the terminal.

Open terminal and change directory to qoute_project (the outter one).

To make sure you are at the correct directory, if you enter command `ls`, it will shows this:
```
$ ls
quote_project	scrapy.cfg
```
Now you can run command (don't forget the quotations marks, both single and double quotations are good)
```
scrapy shell 'http://quotes.toscrape.com/'
```
It will shows
```
2019-01-32 24:60:60 [scrapy.core.engine] INFO: Spider opened
2019-01-32 24:60:61 [scrapy.core.engine] DEBUG: Crawled (200) <GET http://quotes.toscrape.com/> (referer: None)
[s] Available Scrapy objects:
[s]   scrapy     scrapy module (contains scrapy.Request, scrapy.Selector, etc)
[s]   crawler    <scrapy.crawler.Crawler object at 0x10583bf60>
[s]   item       {}
[s]   request    <GET http://quotes.toscrape.com/>
[s]   response   <200 http://quotes.toscrape.com/>
[s]   settings   <scrapy.settings.Settings object at 0x107a76630>
[s]   spider     <QuotesSpider 'quotes' at 0x107d4e908>
[s] Useful shortcuts:
[s]   fetch(url[, redirect=True]) Fetch URL and update local objects (by default, redirects are followed)
[s]   fetch(req)                  Fetch a scrapy.Request and update local objects
[s]   shelp()           Shell help (print this help)
[s]   view(response)    View response in a browser
>>>
```
Now you can check `response`, it is the same one in the parse parameter
```
>>> response
<200 http://quotes.toscrape.com/>
```

Next, we want to check the structure of the webpage. Use command
```
>>> view(response)
True
```
This will open a webpage in your browser. For some urls, this webpage may be a little different from the webpage if you enter the url of the website directly in your browser. It is because some website used dynamic loading.
