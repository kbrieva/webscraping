# Simple Web Scraping: Downloading PNG file in **pngmart.com**

I used this video to learn, check the [link](https://www.youtube.com/watch?v=Al20Pyuc5Ck) for the video tuts



# Python Webscraping

## Install Python
I installed the latest Python version, which is Python 3.11.4.

## Install Miniconda
To manage packages and create virtual environments, I downloaded Miniconda from [here](https://docs.conda.io/projects/miniconda/en/latest/miniconda-install.html).
```markdown
## Create Environment in Miniconda
Using the Anaconda prompt, I created a new Conda environment named "myenv" using the following command:
```bash
conda create --name myenv
```

## Activate the Environment
After creating the environment, I activated it with the following command:
```bash
conda activate myenv 
```

## Install Libraries
To perform web scraping, I installed the necessary libraries: requests, BeautifulSoup4, and lxml.
I install it in my environment

You can install them using the following commands:
```bash
pip install requests
pip install BeautifulSoup4
pip install lxml
```

## Install Jupyter Notebook
To run the web scraping program interactively, I installed Jupyter Notebook with the following command:
```bash
pip install notebook
```

## Run Jupyter Notebook
With the "myenv" environment activated, you can run Jupyter Notebook by simply typing the following command in the Anaconda prompt:
```bash
jupyter notebook # I use Jupyter notebook to check every single code
```

```bash
import requests as req
from bs4 import BeautifulSoup as bea
import os
import time
import shutil
import random

main_url = "https://www.pngmart.com/sitemap.xml"
random_delay = random.uniform(1, 20) #I use a delay to prevent overloading the server when making requests to download PNG files.
file_path = 'PNG' #this is the location of the downloaded file.

def url_get(url): #in this function I use the requests and BeautifulSoup
    try:
        response = req.get(url) 
        if response.status_code == 200:
            soup = bea(response.text, 'html.parser')
            return soup
            
        else:
            return "Error occur"
    except Exception as e:
        print(f"Error occur {e}")

site_map = [] #I create a list of the link and store it in this variable
list = url_get(main_url).findAll('loc')

for link in list:
    if 'posts' in link.text:
        site_map.append(link.text) #this is to store the list of link in site_map

s_1 = url_get(site_map[13]).findAll('loc') #In this code, I use it to find or search for the designated element or attribute that I want to see the output.

list_s1 = []
png_download = 1000
for link in s_1:
    list = (link.text)
    list_s1.append(list)
    test = url_get(list).find('a', 'download')['href']
    title = list.split('/')[-1]+"-"+test.split('/')[-1]
    main_title = os.path.join(file_path, title)
    if not os.path.exists(main_title):
        response = req.get(test)
        with open(main_title, 'wb') as file:
            file.write(response.content)
            time.sleep(random_delay)
            png_download -= 1
            print(png_download, title)
    else:
        print('already download, skipping ', main_title)

# Thank you, if you have any suggestion please recreate the code
In my code, I added a file location, counter and implemented a check to see if the file has already been downloaded.
