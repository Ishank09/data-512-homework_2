# Wikipedia ORES API Data Retrieval

## Introduction
This project involves retrieving data from the Wikipedia API and utilizing the ORES (Objective Revision Evaluation Service) machine learning tool to obtain quality predictions for articles. The project is divided into several steps, each described in detail below.

## Prerequisites
Before running the code, ensure that you have the necessary packages installed. You can install the required packages using the following command:

pip install -r requirements.txt


## Folder Structure

This project is organized with the following folder structure:

- **data:** This directory contains the datasets and any other relevant data files used in the project.

- **src:** This directory is dedicated to storing the source code of the Python project. It includes the main scripts, modules, and packages essential for the project's functionality.

- **analysis:** This directory houses any scripts or notebooks related to data analysis, visualization, or any other data processing tasks relevant to the project.

- **readme.md:** This file serves as the primary source of documentation for the project. It contains information about the project, its functionalities, and instructions on how to set it up and use it.

- **license.md:** This file includes the license information for the project, providing details about the terms of use, distribution, and any other relevant legal information.

- **requirement.txt:** This file lists all the Python packages and their corresponding versions required to run the project. It enables the easy setup of the development environment with the necessary dependencies.




## Steps and files (1-1 mapping)

### Step 1: Data Extraction from Wikipedia API
Source: data_extraction.ipynb

1. The initial step involves extracting data from the Wikipedia API, specifically fetching article titles and last revision IDs. The process is implemented in the data_extraction.ipynb notebook.
2. Attribution is given to the original source of the code.
3. Constants and configurations are defined, including the API endpoint, latency assumptions, and request headers.
4. A function request_pageinfo_per_article is defined to request page info for a single article from the Wikipedia API.
5. Wikipedia data is loaded from a CSV file (../data/us_cities_by_state_SEPT.2023.csv).
6. Page info is requested and stored for Wikipedia articles in a CSV file (../data/wiki_page_info.csv).
Note: The full code details and implementation can be found in the data_extraction.ipynb notebook.


### Step 2: ORES API Prediction
Source: ores_prediction.ipynb

1. The next step involves utilizing the ORES API to obtain quality predictions for the articles.
2. Constants for the ORES API are defined along with the function to make the ORES API request.
3. The code is attributed to Dr. David W. McDonald, used under the Creative Commons CC-BY license.
4. The script checks for existing entries in the CSV file (../data/ores_predictions.csv) to prevent duplicates.
5. The ORES score is obtained and stored for each article in the CSV file (ores_predictions.csv)
Note: The full code details and implementation can be found in the ores_prediction.ipynb notebook.


### Step 3: Data preprocessing
Source: data_preprocessing.ipynb

1. The script reads a CSV file containing Wikipedia articles (../data/ores_predictions.csv)
2. It extracts the titles of the articles and checks if they contain a US state name as the last word after a comma.
3. Articles not conforming to the naming convention are filtered out.
4. The script saves the cleaned data to a new CSV file ('../data/cleaned_data.csv')
Note: The full code details and implementation can be found in the data_preprocessing.ipynb notebook.


### Step 4: Data Merging
Source: data_merging.ipynb

1. Read the initial datasets ('ores_predictions.csv', 'us_cities_by_state_SEPT.2023.csv', and 'US States by Region - US Census Bureau.xlsx') containing information about ORES predictions, US cities, and US state divisions.
2. Data Integration: Combine the relevant data from the 'us_cities_by_state_SEPT.2023.csv' and 'ores_predictions.csv' datasets, utilizing the common key, and create a unified dataframe.
3. Merge the unified dataframe with the 'US States by Region - US Census Bureau.xlsx' dataset based on the shared key, resulting in a comprehensive dataset encompassing US state regional divisions, populations, and Wikipedia article details.
4. Select only the necessary columns, including 'State', 'DIVISION', 'article_title', 'Last_Revision_ID', and 'Prediction', for the final dataset to ensure relevance and coherence.
5. Rename the selected columns to enhance the readability and interpretability of the final dataset.
6. Save the processed and refined dataset as a CSV file named 'resulting_data.csv', containing a consolidated view of US state divisions, regional information, and associated Wikipedia article details.
Note: The full code details and implementation can be found in the data_merging.ipynb notebook.







## Data:

### us_cities_by_state_SEPT.2023.csv'
US Cities Dataset 

This dataset contains information about various cities in the United States. The file 'us_cities_by_state_SEPT.2023.csv' includes the following columns:

- State: The state to which the city belongs.
- Page_title: The title of the Wikipedia page for the respective city.
- URL: The URL link to the Wikipedia page of the city.

Dataset Description

The dataset provides information on cities in the United States as of September 2023. It covers different states and includes data points such as the state name, the name of the city, and a link to the corresponding Wikipedia page for more detailed information.

Example

A snippet of the data from the file 'us_cities_by_state_SEPT.2023.csv' is provided below:

| State    | Page_title              | URL                                             |
| -------- | ----------------------- | ----------------------------------------------- |
| Alabama  | Abbeville, Alabama      | [Link](https://en.wikipedia.org/wiki/Abbeville,_Alabama)       |
| Alabama  | Adamsville, Alabama     | [Link](https://en.wikipedia.org/wiki/Adamsville,_Alabama)       |
| ...      | ...                     | ...                                             |

### wiki_page_info.csv
Wikipedia Page Information Dataset 

This dataset provides information about various Wikipedia pages. The file 'wiki_page_info.csv' includes the following columns:

- Title: The title of the Wikipedia page.
- Last_Revision_ID: The ID of the last revision made to the Wikipedia page.

Dataset Description
The dataset offers insights into the Wikipedia pages related to different topics as of the latest revision. It contains data points such as the title of the page and the respective ID of the last revision, which can be useful for tracking changes and version control of the Wikipedia pages.

Usage
You can use this dataset to track the latest revisions of various Wikipedia pages. The Last_Revision_ID column can be particularly helpful for understanding the version history and tracking any changes made to the Wikipedia pages listed in the dataset.

Example
A snippet of the data from the file 'wiki_page_info.csv' is provided below:

| Title               | Last_Revision_ID |
| ------------------- | ---------------- |
| Abbeville, Alabama  | 1171163550       |
| Adamsville, Alabama | 1177621427       |
| ...                 | ...              |

### ores_predictions.csv
ORES Predictions Dataset 

This dataset contains predictions obtained using the ORES (Objective Revision Evaluation Service) API for various Wikipedia articles. The file 'ores_predictions.csv' includes the following columns:

- Title: The title of the Wikipedia article.
- Last_Revision_ID: The ID of the last revision made to the Wikipedia article.
- Prediction: The predicted ORES score for the respective Wikipedia article.

Dataset Description
The dataset presents the ORES scores predicted for different Wikipedia articles. It encompasses data points such as the article title, the ID of the last revision, and the predicted ORES score. These scores are obtained through the ORES API, which provides an assessment of the quality and characteristics of Wikipedia articles.

Usage
You can utilize this dataset to analyze the predicted ORES scores for various Wikipedia articles. The Last_Revision_ID column can be used for tracking the version history, and the Prediction column can offer insights into the quality assessment of the Wikipedia articles listed in the dataset.

Example
A snippet of the data from the file 'ores_predictions.csv' is provided below:

| Title               | Last_Revision_ID | Prediction |
| ------------------- | ---------------- | ---------- |
| Abbeville, Alabama  | 1171163550       | FA         |
| Adamsville, Alabama | 1177621427       | GA         |
| ...                 | ...              | ...        |



### US States by Region - US Census Bureau.xlsx
US States by Region Dataset - US Census Bureau 

This dataset provides information about the United States divided into different regions and divisions. The file 'US States by Region - US Census Bureau.xlsx' includes the following columns:

- REGION: The broad regions of the United States, including Northeast, Midwest, South, and West.
- DIVISION: The divisions within each region, providing a more detailed geographical breakdown.
- STATE: The individual states belonging to each respective division and region.

Dataset Description
The dataset categorizes the United States into different regions and divisions as defined by the US Census Bureau. It provides information on states falling within each region and division, allowing for a comprehensive understanding of the geographical distribution of states within the United States.

Usage
You can use this dataset to analyze and understand the regional and divisional structure of the United States. The REGION and DIVISION columns can be particularly useful for segmenting the data based on different geographical regions and divisions within the country.

Example
A snippet of the data from the file 'US States by Region - US Census Bureau.xlsx' is provided below:

| REGION           | DIVISION           | STATE          |
| ---------------- | ------------------ | -------------- |
| Northeast        | New England         | Connecticut    |
| Northeast        | New England         | Maine          |
| ...              | ...                 | ...            |



# ores_prediction.ipynb Problems in fetching.

Certainly! Initially, I encountered numerous failures while making API requests to ORES for obtaining article quality scores. These failures were primarily due to HTTP errors such as 502, 504, and 429 (Too Many Requests). To counter these issues and ensure the reliability of the data retrieval process, I implemented a retry mechanism within the request_ores_score_per_article function.

The retry mechanism was designed to allow the function to make several attempts in case of a failed request. It was implemented using a while loop that tracked the number of retries attempted. Each time a failure occurred, the function waited for an increasing duration before making the next attempt. The waiting time followed an exponential backoff strategy, starting with a base wait time of 1 second and doubling with each subsequent retry.

During the execution, the code provided detailed error messages, including the specific HTTP error that occurred along with the corresponding status code. It also printed informative messages about the failed attempts, specifying which articles encountered issues while retrieving scores from the API.

Despite the retry mechanism, some failures persisted due to intermittent issues with the API. However, the implemented retries significantly reduced the impact of these failures, ensuring a more robust and reliable data retrieval process.

The logs indicated that the function attempted several retries for each encountered failure, ultimately enhancing the overall resilience of the data retrieval system.

Among the encountered failures, three specific titles consistently resulted in API failures, namely "Jennings, Missouri," "Jefferson Township, Greene County, Pennsylvania," and "Alma, Wisconsin." 

Due to the persistent API failures for the specific titles "Jennings, Missouri," "Jefferson Township, Greene County, Pennsylvania," and "Alma, Wisconsin," it was necessary to manually add these titles to the list for subsequent data retrieval attempts. By adding these titles manually, it was possible to ensure that the article quality scores were obtained, despite the challenges presented by these particular entries. This manual intervention enabled the comprehensive collection of data for the overall analysis, ensuring that the information for these specific titles was included in the final dataset for further examination and processing.




