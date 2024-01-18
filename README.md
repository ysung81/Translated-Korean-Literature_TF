import requests
from bs4 import BeautifulSoup
import re
import json

url = "https://search.worldcat.org/title/49795553"
res = requests.get(url)
res.raise_for_status()

soup = BeautifulSoup(res.text, "html.parser")

# Find the script tag containing "subjectsText" using regex
pattern = re.compile(r'"subjectsText":\s*\[(.*?)\]', re.DOTALL)
match = pattern.search(str(soup))

# Extract subjectsText if found
if match:
    subjects_text = json.loads(f'[{match.group(1)}]')
    print(subjects_text)
else:
    print("Unable to find subjectsText in the page source.")
