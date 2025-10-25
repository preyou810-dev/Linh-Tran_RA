Web Scraper for Shanghai Yangpu District Announcements
Introduction
This is a Python script designed to automatically scrape data from the planning proposal announcement website of Yangpu District, Shanghai. The script extracts information about elevator installation projects, including the district, address, and announcement date, then saves the results in an organized file.

Main Features
Data Crawling: Automatically accesses the specified URL and downloads the website's HTML content.

Raw Data Storage: Saves all original crawled titles to crawled_titles.txt for reference and verification.

Information Extraction: Parses each title to extract 3 key pieces of information:

区 (District)

地址 (Address)

公告日期 (Announcement Date)

Safe Excel Writing:

Appends extracted data to a sheet named shanghai in the 模版.xlsx file.

Specifically: Data writing begins from column B, leaving column A empty.

Safe: The writing process is designed to not affect, change, or delete any other existing sheets in the Excel file.

Installation and Usage Guide
Requirements
Python 3.8+

uv (An extremely fast Python package manager and virtual environment)

Installation Steps
Install uv: If you don't have uv, install it.

Bash

# macOS / Linux
curl -LsSf https://astral.sh/uv/install.sh | sh

# Windows (Powershell)
powershell -c "irm https://astral.sh/uv/install.ps1 | iex"
Create a virtual environment: Open a terminal in the project directory and run the following command to create a virtual environment named .venv.

Bash

uv venv
Activate the virtual environment:

Bash

# macOS / Linux
source .venv/bin/activate

# Windows (Command Prompt)
.venv\Scripts\activate
After activation, you will see (.venv) at the beginning of your command line.

Install dependencies: Use uv to install the necessary libraries quickly.

Bash

uv pip install requests beautifulsoup4 pandas openpyxl
Prepare the template Excel file:

This is a mandatory step. Create an Excel file in the same directory and name it exactly 模版.xlsx.

Open the file and create the sheets you need. For example: create a sheet named shenzhen with your desired format and another sheet named shanghai (it can be empty or have existing headers). The script will find the shanghai sheet to work on.

Run the script: After completing the steps above, run the script with the following command:

Bash

python main.py
The script will start connecting, scraping data, and updating your 模版.xlsx file.

How it Works
The script operates in a 2-part process:

Part 1: Data Collection and Processing

The script uses the requests library to send an HTTP GET request to the URL, spoofing a browser User-Agent to avoid being blocked.

BeautifulSoup4 is used to parse the returned HTML.

It finds all <li> tags based on a predefined CSS selector.

For each item found, it extracts the full title and announcement date.

Simple string parsing logic is applied to split the District and Address from the title, based on finding the 区 character and removing a common suffix string.

Part 2: Safe File Writing with openpyxl

This is the most critical part to ensure the integrity of the Excel file.

The script uses openpyxl to load the entire 模版.xlsx file into memory in its original state.

It only operates on the shanghai sheet. If the sheet doesn't exist, it will be created.

The script identifies the last empty row in the sheet and starts writing new data from there, writing to specific cells to ensure data starts correctly from column B.

Finally, it saves the entire file. Because the script never reads or edits other sheets, they are preserved 100%.

Current Limitations
Hard-coded configuration: Important information such as the URL, filenames, sheet name, and especially the suffix string (common_suffix) to be removed from the title are all defined directly in the code. If they change, you must edit the code.

Only scrapes the first page: The script does not currently handle pagination. It only retrieves data from the first page of the list.

Simple title parsing logic: The logic for parsing the title to get the District and Address is quite simple (based on the 区 character and a fixed string). It may fail if the title format changes or if there are exceptions.

Dependent on HTML structure: The CSS selector (ul.uli16...) is very specific to the current structure of the website. If the website changes its layout (even a small class change), the script will fail to find data and stop working.
