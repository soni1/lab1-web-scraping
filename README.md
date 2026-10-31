# Web Scraping and Data Collection

A university coursework project completed for the **Data Collection and Quality** course at **Dalarna University**, using Python and Jupyter notebooks.

## Project Aim

The aim was to collect information from different websites, organise it into useful formats, and explore how website structure and access restrictions affect scraping.

The project covers three tasks:

1. Extracting text about machine learning.
2. Collecting product names and prices.
3. Retrieving weather information using a fallback source when the first request fails.

## About the Data

### Machine Learning Text

Headlines and selected paragraphs were collected from:

- Stanford HAI’s explanation of machine learning.
- Carnegie Mellon University’s Machine Learning Department website.

The output contains a headline and one selected paragraph from each source, rather than complete articles.

### Product Listings

Product information was collected from one Multitronic Android-category listing page.

The saved run returned **24 listings**, with these fields:

| Column | Description |
|---|---|
| `Product` | Product name |
| `Price` | Listed price |
| `Currency` | Currency associated with the price |

The returned listings included phones and an accessory. They were not filtered to phones only.

### Weather Information

The weather task focused on **Helsinki, Finland**. The extracted information included:

- Data source and location.
- Temperature.
- Feels-like temperature.
- Weather condition.

The saved files represent the information available during the recorded runs. Website content, prices, weather, and listing counts can change.

## Data Sources

- [Stanford HAI](https://hai.stanford.edu/ai-definitions/what-is-machine-learning)
- [Carnegie Mellon Machine Learning Department](https://www.ml.cmu.edu/)
- [Multitronic product listing](https://www.multitronic.fi/puhelimet-ja-tarvikkeet/matkapuhelimet/Android)
- [Timeanddate Helsinki weather](https://www.timeanddate.com/weather/finland/helsinki)
- [Weather Underground Helsinki weather](https://www.wunderground.com/weather/fi/helsinki)

## What I Did

### Task 1 — Text Scraping

- Requested two university webpages using `requests`.
- Parsed the HTML using Beautiful Soup.
- Extracted headings and selected paragraphs using length and duplicate checks.
- Saved the text with source labels in a local text file.

### Task 2 — Product Scraping

- Inspected the returned HTML to locate product information.
- Extracted names, prices, and currencies from product elements.
- Organised the results into a pandas table.
- Saved a standard CSV and an additional semicolon-separated CSV for convenient viewing in Excel with suitable regional settings.

### Task 3 — Weather Retrieval with Fallback

- Requested the Timeanddate weather page with a timeout.
- Used HTTP error handling to detect a failed request.
- Tried Weather Underground when the primary request failed.
- Used a regular expression to extract weather values from the returned text.
- Converted temperatures when Fahrenheit was explicitly identified.
- Saved the result in a local text file.

## Main Findings

The selected university pages and product listing provided information that could be extracted from their returned HTML.

During the saved weather run, Timeanddate returned **HTTP 403 Forbidden**. The fallback source provided weather information, allowing the task to produce a saved result.

The project demonstrated that extraction rules depend on the website. A successful request alone does not confirm that the required information has been collected correctly; the extracted results also need inspection.

## Conclusions and Limitations

Simple HTML parsing can collect useful text and structured listings, but changes to a webpage can break the extraction rules.

The text selection uses the first paragraph meeting a length threshold, which may select unsuitable text if a page changes. Product scraping covers one listing page, not the complete catalogue, and does not compare prices between retailers.

The weather notebook also has limitations:

- If Timeanddate responds successfully, the current code saves a retrieval confirmation rather than extracting weather values.
- Fallback is triggered by a request error, not every possible extraction failure.
- A failed fallback request is not separately handled.
- If no temperature unit is found, the extracted value is labelled Celsius without verification and needs checking.

The notebooks do not run JavaScript or provide scheduled monitoring.

## Project Structure

```text
lab1-web-scraping/
├── notebooks/
│   ├── 01_task1_text_scraping.ipynb
│   ├── 02_task2_product_scraping.ipynb
│   └── 03_task3_weather_fallback.ipynb
├── output/
│   ├── task1_text.txt
│   ├── task2_products.csv
│   ├── task2_products_semicolon.csv
│   └── task3_weather.txt
├── reference/
├── .gitignore
├── requirements.txt
└── README.md
```

## How to Run

You need Python, an internet connection, VS Code, and its Python and Jupyter extensions.

1. Download or clone this repository and open its main folder in VS Code.
2. Open a terminal in the main folder and create an environment:

   ```powershell
   python -m venv .venv
   ```

3. Install the dependencies:

   ```powershell
   .\.venv\Scripts\python.exe -m pip install -r requirements.txt
   ```

4. Ensure the `output` folder exists. The notebooks save files there but do not create the folder automatically.
5. Open a notebook and select `.venv` as its kernel.
6. Run its cells from top to bottom.

Each notebook can run independently. The notebooks use relative paths such as `../output/task1_text.txt`, so their working directory must be the `notebooks` folder.

Running the export cells replaces the corresponding output files. Requests may fail if websites change or restrict access. Check each website’s access rules and avoid unnecessary repeated requests.

## Outputs

| File | Contents |
|---|---|
| `task1_text.txt` | Selected machine learning text from two university websites |
| `task2_products.csv` | Product names, prices, and currencies |
| `task2_products_semicolon.csv` | Alternative CSV using semicolon separators and comma decimals |
| `task3_weather.txt` | Extracted weather information or a status message |

## Main Packages

- `requests` — requesting webpages.
- `beautifulsoup4` — parsing HTML.
- `pandas` — organising product data and exporting CSV files.
- `ipykernel` — running notebooks in VS Code.