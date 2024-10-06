# Project Report: End-to-End Earthquake Data Engineering and Analysis in Microsoft Fabric

## 1. Introduction

This project demonstrates an end-to-end data engineering and analysis pipeline using Microsoft Fabric, encompassing the use of **Data Factory**, **Data Engineering**, and **Power BI**. The objective is to ingest, process, and visualize earthquake data from the **United States Geological Survey (USGS) API** using **Medallion Architecture**. The data pipeline follows a bronze, silver, and gold layer structure, facilitating incremental improvements in data quality and preparation for business reporting.

### Goals
- Fetch real-time earthquake data.
- Incrementally process data using Medallion Architecture.
- Build automated pipelines using Data Factory for daily ingestion and transformation.
- Visualize earthquake events through a Power BI report.

## 2. Technologies and Tools Used

- **Microsoft Fabric**:
  - **Data Factory**: for orchestration of the data pipeline.
  - **Lakehouse**: for data storage.
  - **Power BI**: for data visualization.
  - **Spark, Python**: for processing data.
  - **Delta Tables**: to store data incrementally.
  
- **External Libraries**:
  - **Requests**: for making API calls.
  - **Reverse Geocoder**: for mapping latitude/longitude coordinates to country codes.

## 3. Data Source

The project utilizes the **USGS Earthquake Catalog API**, which provides real-time data on global earthquake events. Key parameters such as time, magnitude, depth, and location (latitude, longitude) are retrieved.

## 4. Medallion Architecture Overview

The **Medallion Architecture** organizes data in three progressive layers:
- **Bronze Layer**: Raw data ingestion in its original format.
- **Silver Layer**: Cleaned and transformed data, making it more structured and ready for analysis.
- **Gold Layer**: Refined, aggregated, and business-ready data optimized for reporting.

  ![image](https://github.com/user-attachments/assets/096a2802-cc43-422f-a58f-2554e78b4205)
  Image Credits: Pathfinder Analytics

## 5. Project Setup

### 5.1 Creating a Fabric Workspace and Lakehouse
- A new workspace was created for this project in **Microsoft Fabric**.
- A **Lakehouse** was set up to store and manage the processed earthquake data.

### 5.2 Overview of the Earthquake API
- The API provided earthquake data in **GeoJSON** format, which was fetched using Python’s **requests** library.

## 6. Data Processing

### 6.1 Bronze Layer: Raw Data Ingestion
- The **bronze notebook** fetched earthquake data via API requests and stored it in the Lakehouse as JSON files.
- A daily data retrieval mechanism was set up using start and end dates.

**Code Overview**:
- **Requests** and **json** modules were used to make API requests and store the JSON response.
- DataFrame creation to load raw JSON data into the bronze layer.

### 6.2 Silver Layer: Data Transformation
- The **silver notebook** transformed the JSON structure into a tabular format for ease of analysis.
- Key attributes such as **magnitude**, **location**, and **time** were extracted and cleaned.
- **Unix timestamps** were converted to standard timestamp formats for further analysis.

**Code Overview**:
- Extracted features from JSON elements (e.g., magnitude, significance).
- Converted latitude/longitude to individual columns.

### 6.3 Gold Layer: Business-Ready Data
- The **gold notebook** added additional business logic by:
  - Mapping latitude/longitude values to country codes using the **Reverse Geocoder** library.
  - Categorizing earthquakes based on significance into **low**, **moderate**, or **high**.
  
- The transformed data was stored in the **gold layer** as Delta tables.

**Code Overview**:
- Used a custom **UDF** function to map coordinates to country codes.
- Created significance classification logic.

## 7. Data Attribute Definitions

- **id**: A unique string identifier for each earthquake event record.
- **latitude**: The latitude of the earthquake event, stored as a double.
- **longitude**: The longitude of the earthquake event, also stored as a double.
- **elevation**: The elevation at which the earthquake event occurred, expressed in meters, stored as a double.
- **title**: A string representing the title or name of the earthquake event.
- **place_description**: A string providing a detailed description of the location where the earthquake occurred.
- **sig**: A bigint (large integer) representing the significance score of the earthquake event, indicating its impact level.
- **mag**: A double value representing the magnitude of the earthquake on the Richter scale or other magnitude scales.
- **magType**: A string indicating the type of magnitude scale used for measuring the earthquake (e.g., ML, Mw).
- **time**: A timestamp recording the exact time the earthquake event occurred.
- **updated**: A timestamp indicating the last update time for the event data, showing when it was last modified or refreshed.

## 8. Building the Power BI Report

The **Power BI report** was built to visualize earthquake events around the world using a **Map visual**. The report allows filtering based on:
- **Date range**.
- **Significance level** (low, moderate, high).

**Key visuals included**:
- A **map** showing the distribution of earthquakes by country and significance.
- **Slicers** to filter data based on time and significance.

## 9. Automation Using Data Factory

A **Data Factory pipeline** was created to automate the data processing workflow:
- **Bronze Notebook Task**: Fetches raw data from the USGS API.
- **Silver Notebook Task**: Transforms and cleans the raw data.
- **Gold Notebook Task**: Refines data for business reporting.

The pipeline was set up to run daily, with dynamic date parameters passed into each notebook for daily ingestion and transformation.

## 10. Challenges and Solutions

- **Handling large datasets**: Optimized using Delta format tables for efficient storage and retrieval.
- **Scheduling pipelines**: Used Data Factory to automate the daily ingestion of earthquake events.

## 11. Results

The project successfully demonstrated the ability to:
- Ingest earthquake data from the USGS API.
- Transform data incrementally using Medallion Architecture.
- Automate the pipeline to run daily.
- Visualize earthquake data in a Power BI report.

**Final Power BI Report**:
- Showcased worldwide earthquake events.
- Provided insights into the magnitude and frequency of earthquakes by region.

  ![image](https://github.com/user-attachments/assets/12216fba-4021-42b2-b665-5785126859ec)

## 12. Future Enhancements

- Further analyze additional attributes like **depth** and **magnitude type**.
- Implement more advanced visualizations in Power BI, such as **trend analysis**.
- Extend the automation to include **real-time alerts** for significant earthquake events.

## 13. Conclusion

This project provided hands-on experience with **Microsoft Fabric’s Data Engineering, Data Factory, and Power BI** services. The integration of the Medallion architecture with automated pipelines enabled efficient, scalable data processing, and meaningful insights via Power BI visualizations.

## 14. References
- [Pathfinder Analytics : https://www.youtube.com/watch?v=Av44Nrhl05s]
- [USGS Earthquake API Documentation](https://earthquake.usgs.gov/fdsnws/event/1/)
- [Microsoft Fabric documentation](https://learn.microsoft.com/en-us/fabric/)
