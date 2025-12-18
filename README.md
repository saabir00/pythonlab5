# pythonlab5
## main.py
```
import requests
from bs4 import BeautifulSoup
from urllib.parse import urljoin

base_url = "https://books.toscrape.com/"

# 1. Get the main page
response = requests.get(base_url)
soup = BeautifulSoup(response.text, "html.parser")
print("Main page status code:", response.status_code)
# 2. Find all books
books = soup.find_all("article", class_="product_pod")
print("List of books on the page:")
for book in books:
    title = book.find("h3").find("a")["title"]
    print(title)
# 3. Extract book links (absolute URLs)
book_urls = [urljoin(base_url, book.find("h3").find("a")["href"]) for book in books]
print("List of book URLs:")
for book in books:
    link = book.find("h3").find("a")["href"]
    full_link = urljoin(base_url, link)
    print(full_link)
# 4. Parse the first book page
book_page = requests.get(book_urls[0])
book_soup = BeautifulSoup(book_page.text, "html.parser")
title = book_soup.find("h1").text
price = book_soup.find("p", class_="price_color").text
availability = book_soup.find("p", class_="instock availability").text.strip()
print("Title:", title)
print("Price:", price)
print("Availability:", availability)
```
