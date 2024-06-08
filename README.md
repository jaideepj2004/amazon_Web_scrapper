# Amazon Web scrapper

Overview
This project is a web scraper designed to extract product details, customer reviews, and other relevant information from Amazon. The scraped data includes product name, product details, customer name, customer review, customer rating, price of the product, etc. The data is then stored in a MySQL database. Additionally, a web app built using HTML and CSS allows users to interact with the scraper by providing input directly through the website.

Features
Scrape product details from Amazon
Extract customer reviews and ratings
Save the scraped data to a MySQL database
Simple web interface for input
Requirements
Software
Python 3.x
MySQL Server
Flask (for the web app)
BeautifulSoup (for web scraping)
Requests (for HTTP requests)
HTML, CSS (for the web interface)
Python Packages
Install the required Python packages using pip:



pip install flask
pip install beautifulsoup4
pip install requests
pip install mysql-connector-python
Setup
Clone the Repository:



git clone (https://github.com/jaideepj2004/amazon_Web_scrapper)
cd amazon-web-scraper
Set Up MySQL Database:

Install MySQL Server if not already installed.
Create a database named amazon_scraper.
Create a table named products with the following structure:
sql

CREATE TABLE products (
    id INT AUTO_INCREMENT PRIMARY KEY,
    product_name VARCHAR(255),
    product_details TEXT,
    customer_name VARCHAR(255),
    customer_review TEXT,
    customer_rating FLOAT,
    price DECIMAL(10, 2)
);
Configure Database Connection:

Update the database connection details in your Python script (scraper.py):


import mysql.connector

db_config = {
    'user': 'yourusername',
    'password': 'yourpassword',
    'host': 'localhost',
    'database': 'amazon_scraper'
}
