# book_data_scraper
# it is simply taking details about books from https://books.toscrape.com/ and listing the books which's price are lower than 70 euro and higher than 30 euro. this filter is just for example. It can be anything.

import requests
from bs4 import BeautifulSoup

def fetch_webpage(url):
    response = requests.get(url)
    response.encoding = 'utf-8'  # Specify the encoding
    return response.text

def parse_price(price_str):
    price_str = price_str.replace('£', '')  # Remove the pound symbol
    return float(price_str)  # Convert to float

url = 'http://books.toscrape.com/'
html_content = fetch_webpage(url)
soup = BeautifulSoup(html_content, 'html.parser')

for book in soup.find_all('article', class_='product_pod'):
    title = book.h3.a['title']
    price = parse_price(book.find('p', class_='price_color').text)
    if 30 <= price <= 70:
        print(f"{title} - £{price}")

