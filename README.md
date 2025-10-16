### EX8 Web Scraping On E-commerce platform using BeautifulSoup
### DATE: 16/10/2025
### AIM: To perform Web Scraping on Amazon using (beautifulsoup) Python.
### Description: 
<div align = "justify">
Web scraping is the process of extracting data from various websites and parsing it. In other words, it’s a technique 
to extract unstructured data and store that data either in a local file or in a database. 
There are many ways to collect data that involve a huge amount of hard work and consume a lot of time. Web scraping can save programmers many hours. Beautiful Soup is a Python web scraping library that allows us to parse and scrape HTML and XML pages. 
One can search, navigate, and modify data using a parser. It’s versatile and saves a lot of time.
<p>The basic steps involved in web scraping are:
<p>1) Loading the document (HTML content)
<p>2) Parsing the document
<p>3) Extraction
<p>4) Transformation

### Procedure:

1) Import necessary libraries (requests, BeautifulSoup, re, matplotlib.pyplot).
2) Define convert_price_to_float(price) Function: to Remove non-numeric characters from a price string and convert it to a float.
3) Define get_amazon_products(search_query) Function: to Scrape Amazon for product information based on the search query.
4) Fetch and parse the HTML content then Extract product names and prices from the search results and Sort product information based on converted prices in ascending order.
5) Return sorted product data as a list of dictionaries.
6) Call get_amazon_products(search_query) to get product data based on the user's search query.
7) Check if products are found; if not, display "No products found."
8) Visualize Product Data using a Bar Chart

### Program:
```python
import requests
from bs4 import BeautifulSoup
import matplotlib.pyplot as plt
import re

def convert_price_to_float(price_str):
    clean_price = re.sub(r'[^\d.]', '', price_str)
    return float(clean_price) if clean_price else 0.0

def get_snapdeal_products(search_query):
    url = f'https://www.snapdeal.com/search?keyword={search_query.replace(" ", "%20")}'
    headers = {
        'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/98.0.4758.102 Safari/537.36'
    }

    response = requests.get(url, headers=headers)
    products_data = []

    if response.status_code == 200:
        soup = BeautifulSoup(response.content, 'html.parser')
        products = soup.find_all('div', {'class': 'product-tuple-listing'})

        for product in products:
            title = product.find('p', {'class': 'product-title'})
            price = product.find('span', {'class': 'product-price'})
            rating = product.find('div', {'class': 'filled-stars'})

            if title and price:
                product_name = title.text.strip()
                product_price = convert_price_to_float(price.get('data-price', price.text.strip()))
                product_rating = rating['style'].split(':')[-1].replace('%', '').strip() if rating else "No rating"

                products_data.append({
                    'Product': product_name,
                    'Price': product_price,
                    'Rating': product_rating
                })

                print(f'Product: {product_name}')
                print(f'Price: {product_price}')
                print(f'Rating: {product_rating}')
                print('---')
    else:
        print('Failed to retrieve content')

    return products_data

def visualize_product_data(products):
    if products:
        product_names = [p['Product'][:30] + '...' if len(p['Product']) > 30 else p['Product'] for p in products]
        product_prices = [p['Price'] for p in products]

        plt.figure(figsize=(12, 8))
        plt.barh(product_names, product_prices, color='skyblue')
        plt.xlabel('Price (INR)')
        plt.ylabel('Product')
        plt.title('Prices of Products on Snapdeal')
        plt.tight_layout()
        plt.show()
    else:
        print('No products to display.')

if __name__ == "__main__":
    search_query = input('Enter product to search on Snapdeal: ')
    products = get_snapdeal_products(search_query)
    visualize_product_data(products)
```

### Output:
<img width="1268" height="785" alt="image" src="https://github.com/user-attachments/assets/e834d39b-bcb0-406a-9010-fa2d6c9c16a1" />
<img width="1218" height="624" alt="image" src="https://github.com/user-attachments/assets/662b7d6e-215c-4e08-ab36-29fd73776785" />
<img width="1232" height="770" alt="image" src="https://github.com/user-attachments/assets/45564ec6-53c1-4ec4-825c-f9cf5d813e3e" />




### Result:
Thus, Web Scraping on Amazon using (beautifulsoup) Python is executed successfully.
