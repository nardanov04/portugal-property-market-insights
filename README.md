# Portugal Property Market Insights

What do Portuguese property listings reveal about price? This project cleans 184k raw listings with Python (pandas) and maps the patterns by district, property type, size, facilities and energy certificate in Google Sheets.

**Business context:** a real estate company wants to grow sales by 10% within 6 months and needs to know where to focus its pricing, sales and marketing.

## Project files

- [Analysis in Google Sheets](https://docs.google.com/spreadsheets/d/1mHKexepp26TlmmocqAJqM_4p-Vnli5B6poI3LY1lq_U/edit) (formulas, pivot tables and chart for each question, view only)
- [Presentation in Google Slides](https://docs.google.com/presentation/d/1kxftvdiAImpzCWTTTURn9_QK6MW7UhmNnOA9Kkd29qQ/edit) (findings and recommendations, view only)
- [Data cleaning notebook](portugal_data_cleaning.ipynb)
- [Raw data](portugal_listings_raw.csv) and [clean data](portugal_listings_clean.csv) (too large to preview on GitHub, download to view)

## Key questions

1. How does the average price differ by district?
2. How does the average price differ by property type?
3. How does price per m² change with property size?
4. How are parking, elevators and bathrooms linked to price?
5. How do energy certificate ratings relate to price for each property type?
6. How much do prices vary within each district?
7. Which districts combine a high number of listings with competitive pricing?

## Data and cleaning

The raw dataset has 184,332 rows and 23 columns. Cleaning steps in the notebook:

1. Removed 10,996 duplicate rows (checked across all 23 columns), leaving 173,336.
2. Kept the 8 columns needed to answer the questions: Price, District, Type, TotalArea, Parking, Elevator, NumberOfBathrooms and EnergyCertificate.
3. Removed rows with missing values.
4. Removed unrealistic values using judgement cut-offs (for example price below €1,000, area of zero or less, and negative bathroom counts).
5. Removed the district "Z - Fora de Portugal", which is not in Portugal.
6. Merged "NC", "No Certificate" and "Not available" into one category, "Not Certificated".

The final clean dataset has 148,593 rows and 8 columns.

## Key findings

![Average price by district](images/q1_price_by_district.png)

- Average prices differ widely by district: Lisboa is the highest at about €612k and Castelo Branco the lowest at about €116k.

![Energy certificate pivot table](images/q5_energy_certificate_pivot.png)

- Better energy ratings generally go with higher prices: A and B listings average roughly €580k to €660k, while F, G and uncertified listings average about €280k to €290k.
- Smaller properties have a higher price per m², and listings with more facilities tend to be priced higher.

![Flagged districts](images/q7_flagged_districts.png)

- Six districts combine high listing volume with competitive pricing: Porto, Braga, Coimbra, Aveiro, Santarém and Leiria.

## Recommendations

![Conclusion and recommendations slide](images/recommendations_slide.png)

## Limitations

- Prices are listing prices from the dataset, referred to as selling prices in the analysis.
- The findings show associations, not causes.
- Cut-off values for unrealistic data were set by judgement.
- The data has no sales or time information, so the 10% sales goal cannot be tested directly.

## Tools

Python (pandas), Jupyter Notebook, Google Sheets (pivot tables, lookup and conditional formulas, charts), Google Slides
