<!-- TOC start (generated with https://github.com/derlin/bitdowntoc) -->

- [Wikipedia ORES API Data Retrieval](#wikipedia-ores-api-data-retrieval)
   * [Introduction](#introduction)
   * [Prerequisites](#prerequisites)
   * [Folder Structure](#folder-structure)
   * [Steps and files (1-1 mapping)](#steps-and-files-1-1-mapping)
      + [Step 1: Data Extraction from Wikipedia API](#step-1-data-extraction-from-wikipedia-api)
      + [Step 2: ORES API Prediction](#step-2-ores-api-prediction)
      + [Step 3: Data preprocessing](#step-3-data-preprocessing)
      + [Step 4: Data Merging](#step-4-data-merging)
      + [Step 5: Analysis](#step-5-analysis)
   * [Results](#results)
      + [Section 4a: Total Articles per Population by State](#section-4a-total-articles-per-population-by-state)
      + [Section 4b: High-Quality Articles per Population](#section-4b-high-quality-articles-per-population)
      + [Section 5: Results](#section-5-results)
      + [Top 10 US states by coverage:](#top-10-us-states-by-coverage)
      + [Bottom 10 US states by coverage:](#bottom-10-us-states-by-coverage)
      + [Top 10 US states by high quality:](#top-10-us-states-by-high-quality)
      + [Bottom 10 US states by high quality:](#bottom-10-us-states-by-high-quality)
      + [Census divisions by total coverage:](#census-divisions-by-total-coverage)
      + [Census divisions by high quality coverage:](#census-divisions-by-high-quality-coverage)
   * [Data:](#data)
      + [us_cities_by_state_SEPT.2023.csv'](#us_cities_by_state_sept2023csv)
      + [wiki_page_info.csv](#wiki_page_infocsv)
      + [ores_predictions.csv](#ores_predictionscsv)
      + [US States by Region - US Census Bureau.xlsx](#us-states-by-region-us-census-bureauxlsx)
      + [wp_scored_city_articles_by_state.csv](#wp_scored_city_articles_by_statecsv)
- [Research Implications](#research-implications)
   * [ores_prediction.ipynb Problems in fetching.](#ores_predictionipynb-problems-in-fetching)

<!-- TOC end -->

<!-- TOC --><a name="wikipedia-ores-api-data-retrieval"></a>
# Wikipedia ORES API Data Retrieval



<!-- TOC --><a name="introduction"></a>
## Introduction
This project involves retrieving data from the Wikipedia API and utilizing the ORES (Objective Revision Evaluation Service) machine learning tool to obtain quality predictions for articles. The project is divided into several steps, each described in detail below.

<!-- TOC --><a name="prerequisites"></a>
## Prerequisites
Before running the code, ensure that you have the necessary packages installed. You can install the required packages using the following command:

pip install -r requirements.txt


<!-- TOC --><a name="folder-structure"></a>
## Folder Structure

This project is organized with the following folder structure:

- **data:** This directory contains the datasets and any other relevant data files used in the project.

- **src:** This directory is dedicated to storing the source code of the Python project. It includes the main scripts, modules, and packages essential for the project's functionality.

- **analysis:** This directory houses any scripts or notebooks related to data analysis, visualization, or any other data processing tasks relevant to the project.

- **readme.md:** This file serves as the primary source of documentation for the project. It contains information about the project, its functionalities, and instructions on how to set it up and use it.

- **license.md:** This file includes the license information for the project, providing details about the terms of use, distribution, and any other relevant legal information.

- **requirement.txt:** This file lists all the Python packages and their corresponding versions required to run the project. It enables the easy setup of the development environment with the necessary dependencies.




<!-- TOC --><a name="steps-and-files-1-1-mapping"></a>
## Steps and files (1-1 mapping)

<!-- TOC --><a name="step-1-data-extraction-from-wikipedia-api"></a>
### Step 1: Data Extraction from Wikipedia API
Source: data_extraction.ipynb

1. The initial step involves extracting data from the Wikipedia API, specifically fetching article titles and last revision IDs. The process is implemented in the data_extraction.ipynb notebook.
2. Attribution is given to the original source of the code.
3. Constants and configurations are defined, including the API endpoint, latency assumptions, and request headers.
4. A function request_pageinfo_per_article is defined to request page info for a single article from the Wikipedia API.
5. Wikipedia data is loaded from a CSV file (../data/us_cities_by_state_SEPT.2023.csv).
6. Page info is requested and stored for Wikipedia articles in a CSV file (../data/wiki_page_info.csv).
Note: The full code details and implementation can be found in the data_extraction.ipynb notebook.


<!-- TOC --><a name="step-2-ores-api-prediction"></a>
### Step 2: ORES API Prediction
Source: ores_prediction.ipynb

1. The next step involves utilizing the ORES API to obtain quality predictions for the articles.
2. Constants for the ORES API are defined along with the function to make the ORES API request.
3. The code is attributed to Dr. David W. McDonald, used under the Creative Commons CC-BY license.
4. The script checks for existing entries in the CSV file (../data/ores_predictions.csv) to prevent duplicates.
5. The ORES score is obtained and stored for each article in the CSV file (ores_predictions.csv)
Note: The full code details and implementation can be found in the ores_prediction.ipynb notebook.


<!-- TOC --><a name="step-3-data-preprocessing"></a>
### Step 3: Data preprocessing
Source: data_preprocessing.ipynb

1. The script reads a CSV file containing Wikipedia articles (../data/ores_predictions.csv)
2. It extracts the titles of the articles and checks if they contain a US state name as the last word after a comma.
3. Articles not conforming to the naming convention are filtered out.
4. The script saves the cleaned data to a new CSV file ('../data/cleaned_data.csv')
Note: The full code details and implementation can be found in the data_preprocessing.ipynb notebook.


<!-- TOC --><a name="step-4-data-merging"></a>
### Step 4: Data Merging
Source: data_merging.ipynb
1. Data Reading: Importing three datasets, namely 'cleaned_data.csv', 'us_cities_by_state_SEPT.2023.csv', and 'US States by Region - US Census Bureau.xlsx', containing information about ORES predictions, US cities, and US state divisions, respectively.
2. Removal of Duplicates: Removing duplicate rows from the 'cleaned_data.csv' and 'us_cities_by_state_SEPT.2023.csv' datasets, based on the 'title' and 'page_title' columns.
3. Data Preprocessing: Standardizing and cleaning data by extracting necessary columns, converting strings to lowercase, and removing leading special characters for better data uniformity.
4. Data Merging: Merging the 'ores_df' and 'cities_df' based on 'title' and 'page_title' columns to create a comprehensive dataframe with combined information.
5. Integration with Regional Data: Combining the merged dataframe with the 'regions_df' based on the 'state' column, enabling a holistic view of US states, their regions, and corresponding Wikipedia article details.
6. Population Data Merge: Merging the 'final_df' with the 'population_df' based on the 'state' and 'Geographic Area' columns, incorporating population information into the comprehensive dataset.
7. Saving the Merged Dataset: Exporting the merged dataframe to 'wp_scored_city_articles_by_state.csv' to create a consolidated dataset containing US state divisions, regional details, population, Wikipedia article information, and ORES predictions.
Note: The full code details and implementation can be found in the data_merging.ipynb notebook.

<!-- TOC --><a name="step-5-analysis"></a>
### Step 5: Analysis
Source: data_analysis.ipynb
1. Import the necessary libraries such as pandas and numpy.
2. Load the dataset from 'wp_scored_city_articles_by_state.csv'.
3. Calculate the total articles per population (articles per capita) by state. This includes reading the data, cleaning it, and performing the necessary calculations.
4. Calculate the total articles per capita by regional division.
5. Calculate high-quality articles per population (articles tagged with 'FA' or 'GA') both by state and by regional division.
6. For each analysis subsection, display the top 10 and bottom 10 US states based on coverage and high-quality articles per capita.
7. Display the census divisions ranked by total coverage and high-quality coverage.




<!-- TOC --><a name="results"></a>
## Results
<!-- TOC --><a name="section-4a-total-articles-per-population-by-state"></a>
### Section 4a: Total Articles per Population by State
The top 5 US states by articles per capita are:
	1. Vermont: 0.000091
	2. Maine: 0.000202
	3. Iowa: 0.000012
	4. Alaska: 0.000164
	5. Pennsylvania: 0.000012
<!-- TOC --><a name="section-4b-high-quality-articles-per-population"></a>
### Section 4b: High-Quality Articles per Population
By State (Top 5):

	1. Vermont: 0.000010
	2. Wyoming: 0.000042
	3. Montana: 0.000003
	4. Pennsylvania: 0.000024
	5. Missouri: 0.000004

By Regional Division (Top 5):

	1. Middle Atlantic: 0.000015
	2. West North Central: 0.000016
	3. New England: 0.000044
	4. East South Central: 0.000017
	5. East North Central: 0.000013

<!-- TOC --><a name="section-5-results"></a>
### Section 5: Results
<!-- TOC --><a name="top-10-us-states-by-coverage"></a>
### Top 10 US states by coverage:
	1. Vermont
	2. Maine
	3. Iowa
	4. Alaska
	5. Pennsylvania
	6. Michigan
	7. Wyoming
	8. Arkansas
	9. Missouri
	10. Minnesota
<!-- TOC --><a name="bottom-10-us-states-by-coverage"></a>
### Bottom 10 US states by coverage:
	1. Nevada
	2. California
	3. Arizona
	4. Virginia
	5. Florida
	6. Oklahoma
	7. Kansas
	8. Maryland
	9. Wisconsin
	10. Washington

<!-- TOC --><a name="top-10-us-states-by-high-quality"></a>
### Top 10 US states by high quality:
	1. Vermont
	2. Wyoming
	3. Montana
	4. Pennsylvania
	5. Missouri
	6. Alaska
	7. Oregon
	8. Iowa
	9. Maine
	10. Minnesota

<!-- TOC --><a name="bottom-10-us-states-by-high-quality"></a>
### Bottom 10 US states by high quality:
	1. Virginia
	2. Nevada
	3. Arizona
	4. California
	5. Florida
	6. Maryland
	7. Kansas
	8. Oklahoma
	9. Massachusetts
	10. Louisiana

<!-- TOC --><a name="census-divisions-by-total-coverage"></a>
### Census divisions by total coverage:
	1. Middle Atlantic
	2. West North Central
	3. New England
	4. East North Central
	5. East South Central
	6. West South Central
	7. Mountain
	8. Pacific
	9. South Atlantic
<!-- TOC --><a name="census-divisions-by-high-quality-coverage"></a>
### Census divisions by high quality coverage:
	1. Middle Atlantic
	2. West North Central
	3. New England
	4. East South Central
	5. East North Central
	6. West South Central
	7. Mountain
	8. Pacific
	9. South Atlantic
These results provide insights into the distribution of Wikipedia articles and their quality across different US states and census divisions


<!-- TOC --><a name="data"></a>
## Data:

<!-- TOC --><a name="us_cities_by_state_sept2023csv"></a>
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

<!-- TOC --><a name="wiki_page_infocsv"></a>
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

<!-- TOC --><a name="ores_predictionscsv"></a>
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



<!-- TOC --><a name="us-states-by-region-us-census-bureauxlsx"></a>
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


<!-- TOC --><a name="wp_scored_city_articles_by_statecsv"></a>
### wp_scored_city_articles_by_state.csv
Wikipedia Scored City Articles by State Dataset 

This dataset presents information about Wikipedia articles related to cities, along with their respective scores and additional data points. The file 'wp_scored_city_articles_by_state.csv' includes the following columns:

- State: The state to which the city belongs.
- Regional_Division: The regional division where the city is located.
- Article_Title: The title of the Wikipedia article for the city.
- Revision_ID: The ID of the last revision made to the Wikipedia article.
- Article_Quality: The quality score assigned to the Wikipedia article.
- Population: The population of the corresponding geographic area.

Dataset Description
The dataset provides insights into Wikipedia articles related to cities, including their quality scores, geographic area, and population. It aims to offer a comprehensive overview of various cities in different states and their respective Wikipedia article quality.

Usage
You can utilize this dataset to analyze Wikipedia articles related to cities and their quality scores. The Article_Quality column can be particularly useful for understanding the quality assessment of the Wikipedia articles, while the Population column can provide context about the population of the corresponding geographic areas.

Example

A snippet of the data from the file 'wp_scored_city_articles_by_state.csv' is provided below:

| State   | Regional_Division    | Article_Title          | Revision_ID | Article_Quality | Geographic_Area | Population |
| ------- | -------------------- | ---------------------- | ----------- | --------------- | --------------- | ---------- |
| Alabama | East South Central   | Abbeville, Alabama     | 1171163550  | C               | alabama         | 5074296    |
| Alabama | East South Central   | Adamsville, Alabama    | 1177621427  | C               | alabama         | 5074296    |
| ...     | ...                  | ...                    | ...         | ...             | ...             | ...        |

Feel free to explore the dataset and use it for various analytical purposes related to Wikipedia articles on cities and their respective states.



<!-- TOC --><a name="research-implications"></a>
# Research Implications

<!-- TOC --><a name="ores_predictionipynb-problems-in-fetching"></a>
## ores_prediction.ipynb Problems in fetching.

Certainly! Initially, I encountered numerous failures while making API requests to ORES for obtaining article quality scores. These failures were primarily due to HTTP errors such as 502, 504, and 429 (Too Many Requests). To counter these issues and ensure the reliability of the data retrieval process, I implemented a retry mechanism within the request_ores_score_per_article function.

The retry mechanism was designed to allow the function to make several attempts in case of a failed request. It was implemented using a while loop that tracked the number of retries attempted. Each time a failure occurred, the function waited for an increasing duration before making the next attempt. The waiting time followed an exponential backoff strategy, starting with a base wait time of 1 second and doubling with each subsequent retry.

During the execution, the code provided detailed error messages, including the specific HTTP error that occurred along with the corresponding status code. It also printed informative messages about the failed attempts, specifying which articles encountered issues while retrieving scores from the API.

Despite the retry mechanism, some failures persisted due to intermittent issues with the API. However, the implemented retries significantly reduced the impact of these failures, ensuring a more robust and reliable data retrieval process.

The logs indicated that the function attempted several retries for each encountered failure, ultimately enhancing the overall resilience of the data retrieval system.

Among the encountered failures, three specific titles consistently resulted in API failures, namely "Jennings, Missouri," "Jefferson Township, Greene County, Pennsylvania," and "Alma, Wisconsin." 

Due to the persistent API failures for the specific titles "Jennings, Missouri," "Jefferson Township, Greene County, Pennsylvania," and "Alma, Wisconsin," it was necessary to manually add these titles to the list for subsequent data retrieval attempts. By adding these titles manually, it was possible to ensure that the article quality scores were obtained, despite the challenges presented by these particular entries. This manual intervention enabled the comprehensive collection of data for the overall analysis, ensuring that the information for these specific titles was included in the final dataset for further examination and processing.




