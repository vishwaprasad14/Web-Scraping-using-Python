# Web-Scraping-using-Python
This project contains a Python script to scrape product data from Amazon, save it to a CSV file, and send an email notification if the price drops below a certain threshold.

### Prerequisites
Before running the script, ensure you have the following libraries installed:

- BeautifulSoup
- requests
- smtplib
- pandas
- csv

### Project Overview
This project performs the following tasks:
- Connects to an Amazon product page and scrapes the product title and price.
- Cleans up the scraped data.
- Saves the data into a CSV file.
- Appends new data to the CSV file daily.
- (Optional) Sends an email notification when the product price drops below a specified amount.

### Code Breakdown
Import Libraries
- from bs4 import BeautifulSoup
- import requests
- import time
- import datetime
- import smtplib
- import pandas as pd
- import csv

### Connect to Website and Pull Data
URL = 'https://www.amazon.com/Funny-Data-Systems-Business-Analyst/dp/B07FNW9FGJ/ref=sr_1_3?dchild=1&keywords=data%2Banalyst%2Btshirt&qid=1626655184&sr=8-3&customId=B0752XJYNL&th=1'

headers = {
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/78.0.3904.108 Safari/537.36",
    "Accept-Encoding":"gzip, deflate",
    "Accept":"text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8",
    "DNT":"1",
    "Connection":"close",
    "Upgrade-Insecure-Requests":"1"
}

page = requests.get(URL, headers=headers)
soup1 = BeautifulSoup(page.content, "html.parser")
soup2 = BeautifulSoup(soup1.prettify(), "html.parser")

title = soup2.find(id='productTitle').get_text()
price = soup2.find(id='priceblock_ourprice').get_text()

print(title)
print(price)


### Clean Up Data
price = price.strip()[1:]
title = title.strip()

print(title)
print(price)


### Create a Timestamp
today = datetime.date.today()
print(today)


### Create and Write to CSV
header = ['Title', 'Price', 'Date']
data = [title, price, today]

with open('AmazonWebScraperDataset.csv', 'w', newline='', encoding='UTF8') as f:
    writer = csv.writer(f)
    writer.writerow(header)
    writer.writerow(data)


### Append Data to CSV
with open('AmazonWebScraperDataset.csv', 'a+', newline='', encoding='UTF8') as f:
    writer = csv.writer(f)
    writer.writerow(data)


### Read and Print CSV
df = pd.read_csv('AmazonWebScraperDataset.csv')
print(df)


### Automate the Script to Run Daily
def check_price():
    # (Add the full scraping and CSV writing code here)
    pass

while True:
    check_price()
    time.sleep(86400)


### Send Email Notification
def send_mail():
    server = smtplib.SMTP_SSL('smtp.gmail.com', 465)
    server.ehlo()
    server.login('your_email@gmail.com', 'your_password')
    
    subject = "The Shirt you want is below $15! Now is your chance to buy!"
    body = "Link here: https://www.amazon.com/Funny-Data-Systems-Business-Analyst/dp/B07FNW9FGJ/ref=sr_1_3?dchild=1&keywords=data+analyst+tshirt&qid=1626655184&sr=8-3"
    msg = f"Subject: {subject}\n\n{body}"
    
    server.sendmail('your_email@gmail.com', 'recipient_email@gmail.com', msg)
    server.quit()


