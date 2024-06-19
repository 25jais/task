import requests
from bs4 import BeautifulSoup

# URL of the web page you want to scrape
url = 'https://example.com/news'

# Send a GET request to the URL
response = requests.get(url)

# Parse the HTML content
soup = BeautifulSoup(response.text, 'html.parser')

# Find all article titles (assuming they are in <h2> tags)
article_titles = soup.find_all('h2', class_='article-title')

# Iterate over the titles and print or store them
for title in article_titles:
    print(title.text.strip())  # .strip() removes any surrounding whitespace

# Example of storing titles in a list (you can modify this to save in a file, database, etc.)
titles_list = [title.text.strip() for title in article_titles]

# Print the list of titles
print(titles_list)
import requests
from bs4 import BeautifulSoup

# Function to extract information from a web page
def extract_information(url):
    try:
        # Send a GET request to the URL
        response = requests.get(url)
        
        # Raise an exception if the request was unsuccessful
        response.raise_for_status()
        
        # Parse the HTML content
        soup = BeautifulSoup(response.text, 'html.parser')
        
        # Extract text content
        text_content = soup.get_text()
        
        # Extract all links (href attributes)
        links = [link.get('href') for link in soup.find_all('a') if link.get('href')]
        
        # Extract image URLs (src attributes)
        images = [img.get('src') for img in soup.find_all('img') if img.get('src')]
        
        # Return the extracted information
        return {
            'text_content': text_content,
            'links': links,
            'images': images
        }
    
    except requests.exceptions.RequestException as e:
        print(f"Error fetching URL: {e}")
        return None

# Example usage:
if __name__ == "__main__":
    url = 'https://example.com'
    result = extract_information(url)
    
    if result:
        print(f"Text content:\n{result['text_content']}\n")
        print(f"Links:")
        for link in result['links']:
            print(link)
        print(f"\nImages:")
        for img in result['images']:
            print(img)
    else:
        print("Extraction failed. Check the URL and try again.")
