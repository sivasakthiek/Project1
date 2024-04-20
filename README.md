# Project1
<!DOCTYPE html>
<html>
<head>
    <title>Yellow Pages UAE Restaurants</title>
    <style>
        table {{
            width: 100%;
            border-collapse: collapse;
        }}
        th, td {{
            border: 1px solid black;
            padding: 8px;
            text-align: left;
        }}
        th {{
            background-color: #f2f2f2;
        }}
    </style>
</head>
<body>
    <h1>Yellow Pages UAE Restaurants</h1>
    <p>Scraped on {datetime.now().strftime('%Y-%m-%d %H:%M:%S')}</p>
    <table>
        <tr>
            <th>Name</th>
            <th>Location</th>
            <th>City</th>
            <th>P.O. Box</th>
            <th>Phone</th>
            <th>Mobile</th>
            <th>Company Page Link</th>
            <th>Logo URL</th>
        </tr>
"""

for business in businesses:
    html_content += f"""
        <tr>
            <td>{business['name']}</td>
            <td>{business['location']}</td>
            <td>{business['city']}</td>
            <td>{business['po_box']}</td>
            <td>{business['phone']}</td>
            <td>{business['mobile']}</td>
            <td><a href="{business['company_page_link']}">{business['company_page_link']}</a></td>
            <td><img src="{business['logo_url']}" style="max-width: 100px; max-height: 100px;"></td>
        </tr>
    """

html_content += """
    </table>
    <pyscript>
import requests
from bs4 import BeautifulSoup

url = 'https://www.yellowpages-uae.com/uae/restaurant'
response = requests.get(url)
soup = BeautifulSoup(response.text, 'html.parser')

businesses = []
for business in soup.find_all('div', class_='single-listing'):
    name = business.find('h3').text.strip()
    location = business.find('span', class_='location').text.strip()
    city = business.find('span', class_='city').text.strip()
    po_box = business.find('span', class_='po-box').text.strip()
    phone = business.find('span', class_='phone').text.strip()
    mobile = business.find('span', class_='mobile').text.strip()
    company_page_link = business.find('a')['href']
    logo_url = business.find('img')['src']

    businesses.append({
        'name': name,
        'location': location,
        'city': city,
        'po_box': po_box,
        'phone': phone,
        'mobile': mobile,
        'company_page_link': company_page_link,
        'logo_url': logo_url
    })

for business in businesses:
    print(business)
    </pyscript>
</body>
</html>
