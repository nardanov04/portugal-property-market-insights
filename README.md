# Portugal Property Market Insights

What do Portuguese property listings reveal about price? This project cleans 184k raw listings with Python (pandas) and maps the patterns by district, property type, size, facilities and energy certificate in Google Sheets, with extra pandas checks to separate real patterns from mix effects.

**Business context:** a real estate company wants to grow sales by 10% within 6 months and needs to know where to focus its pricing, sales and marketing.

## Summary

- Location, property type and size explain most price differences, and listing prices are very uneven: the average is about €397k, but the median is €260k.
- Six districts combine high listing volume with below-average prices: Porto, Braga, Coimbra, Aveiro, Santarém and Leiria, holding about 46% of all listings. Five of them are clear value markets, while Porto is priced much closer to the premium districts.
- Parking, elevators, bathrooms and good energy ratings go with higher prices, but they mostly mark larger, higher-end properties.
- Recommendation: price by district, type and size together, segment districts into value, mid-priced and premium, and add sales data before committing to the 10% goal.

## Project files

- [Analysis in Google Sheets](https://docs.google.com/spreadsheets/d/1mHKexepp26TlmmocqAJqM_4p-Vnli5B6poI3LY1lq_U/edit) (formulas, pivot tables and chart for each question, view only)
- [Presentation in Google Slides](https://docs.google.com/presentation/d/1kxftvdiAImpzCWTTTURn9_QK6MW7UhmNnOA9Kkd29qQ/edit) (findings and recommendations, view only)
- [Data cleaning notebook](portugal_data_cleaning.ipynb)
- [Raw data](portugal_listings_raw.csv) and [clean data](portugal_listings_clean.csv) (too large to preview on GitHub, download to view)

To rerun the cleaning, keep the notebook and the raw CSV in the same folder, install pandas and run all cells.

## Data and cleaning

The raw dataset has 184,332 listings and 23 columns. The notebook cleans it in six steps:

| Step | What was done | Why |
|---|---|---|
| 1. Remove duplicates | Dropped 10,996 exact duplicate rows, leaving 173,336 | So the same listing is not counted twice. Duplicates were checked across all 23 columns, not just the 8 used later, so that different listings that look alike on 8 columns are not deleted by mistake |
| 2. Select columns | Kept Price, District, Type, TotalArea, Parking, Elevator, NumberOfBathrooms and EnergyCertificate | These are the fields the 7 questions need. The other columns had too many missing values, and District is a more reliable grouping than City or Town, which split the data into very small groups |
| 3. Remove missing values | Dropped rows with a missing value in any of the 8 columns, leaving 149,721 | Avoids guessing values, and still keeps about 86% of the de-duplicated rows |
| 4. Remove unrealistic values | Kept prices from €1,000 to €60M, areas above 0 and up to 8M m², and 0 to 15 bathrooms, leaving 148,619 | The raw data had prices from €1 to €2 billion, negative areas and a bathroom count of -13, which would distort every average. The cut-offs are judgement calls |
| 5. Remove non-Portugal rows | Dropped 26 rows with the district "Z - Fora de Portugal" | It is not a district in Portugal |
| 6. Merge labels | Combined "NC", "No Certificate" and "Not available" into "Not Certificated" | Three labels meant the same thing, and merging gives one clean group |

**Result:** 148,593 listings and 8 columns with no missing values, which is about 81% of the raw rows.

## How to read the findings

Questions 1 to 7 were answered in Google Sheets using averages. Two things limit how far averages can be trusted:

- **A few expensive listings pull averages up.** The overall average listing price is about €397k, but the median is €260k. The top 10% of listings hold about 40% of the total listed value.
- **The property mix differs by district.** Land, houses and apartments have very different median prices (€65k, €277k and €299k), so a district with lots of land looks cheap whatever its homes cost.

Where it matters, the findings add checks run in pandas on the clean data (medians, apartments only, houses only). These are marked "extra check" and are not in the Sheets.

## Findings

### Q1. How does average price vary between districts?

![Average price by district](images/Average%20price%20by%20district.webp)

- **What the data shows:** prices differ about five times between the most and least expensive districts. Lisboa is highest at about €612k, followed by Faro (about €549k), Setúbal (about €449k) and Porto (about €372k). Castelo Branco is lowest at about €116k, followed by Viseu and Guarda at about €138k each.
- **Extra check, part of the gap is property mix.** Lisboa's listings are 60% apartments and 6.5% land, while Santarém's are 17% apartments and 26% land. Comparing apartments only, Lisboa's median is about €380k against about €189k in Santarém, a gap of about 2 times instead of the 2.8 times shown by the averages. The gap shrinks but does not disappear, so location carries real weight.
- **Insight:** Faro is a market of two halves. Its houses have a median of about €643k, as high as Lisboa's (about €625k), while its apartments (about €320k) are much cheaper, which gives it the widest absolute price spread among the large districts. Several island districts have only 1 to 3 listings, so their averages are not reliable.

### Q2. Do property types affect price?

- **What the data shows:** type matters a lot. The highest averages are Mansion (about €3.1M), Estate (about €2.6M), Hotel (about €2.0M) and Manor (about €1.5M), but these types have very few listings. The lowest are Storage (about €35k), Garage (about €44k), Transfer of lease (about €125k) and Land (about €198k).
- **Insight:** apartments (42% of listings, average about €396k) and houses (32%, about €446k) are almost three quarters of the market and sit close to the overall average, so they set the market baseline. The premium types are niche and say little about the typical buyer.
- **Extra check:** land has an average of about €198k but a median of only about €65k, so a few high-priced plots lift its average to three times its typical price.

### Q3. Does size affect price, and what is a competitive price per m²?

| Size band | Listings | Average price | Average price per m² |
|---|---|---|---|
| Under 100 m² | 47,900 | €230k | €3,851 |
| 100 to 499 m² | 72,059 | €475k | €2,630 |
| 500 to 999 m² | 8,527 | €582k | €885 |
| 1,000 to 4,999 m² | 11,781 | €379k | €219 |
| 5,000 m² and above | 8,326 | €521k | €36 |

- **What the data shows:** price per m² falls sharply as size grows, from about €3,851 under 100 m² to about €36 above 5,000 m², while total price does not rise in a straight line. Listings of 500 to 999 m² average the most (about €582k), and the 1,000 to 4,999 m² band is cheaper than the band before it.
- **Insight:** the overall average of about €2,587 per m² describes only the two smallest bands, which hold 80% of listings. For larger properties the price per m² is a fraction of that, so one "competitive price per m²" would misprice small homes and large plots alike. A competitive price has to be judged within a size band.

### Q4. Do parking, elevators and bathrooms affect price?

- **What the data shows:** all three go with higher prices. Average price rises with parking spaces (€319k with none, €479k with 1, €515k with 2, €716k with 3), is about 31% higher with an elevator (about €477k against €365k), and climbs with bathrooms from about €343k at 2 to about €1.23M at 5. Listings with 0 bathrooms average more (€262k) than those with 1 (€220k), because about 57% of them are land.
- **Extra check, these features mostly mark bigger properties.** Among apartments, those with an elevator have a median of €345k against €244k without (+41%), but they are also about 22% larger (107 m² against 88 m²). Apartment medians rise from €260k with no parking (88 m²) to €595k with 3 spaces (185 m²). In houses the parking premium levels off, from €335k with 1 space to €390k with 3, even though the area more than triples.
- **Insight:** bathrooms are the steadiest signal. Apartment medians rise from €236k (1 bathroom, 72 m²) to €1.25M (5 bathrooms, 227 m²), and houses from €100k to €870k. Houses with no bathrooms have a median of only €69k, the lowest group. Elevators are almost an apartment-only feature (94% of listings with one are apartments), so the overall elevator gap partly compares apartments with houses and land.

### Q5. Does an energy certificate affect price, and does it vary by type?

![Energy certificate pivot table](images/Energy%20certificate%20pivot%20table.png)

- **What the data shows:** better ratings go with higher prices overall: A to B- listings average about €580k to €660k, against about €280k to €340k for E, F and G. The order is not perfect, as A+ averages less than A and B, and G has very few listings.
- **Extra check, the pattern holds within property types and is much steeper for houses.** House medians fall from €660k for A to €145k for F (about 4.6 times lower), while apartment medians fall from €405k to €214k (about 1.9 times lower). Better-rated apartments are also larger (116 m² for A against 91 m² for F), so rating partly tracks size and age.
- **Insight, "no certificate" means different things by type.** 35% of listings have no certificate, but 99% of land has none, and land makes up 37% of the uncertified group, which is why the uncertified average (about €290k) looks so low. For houses, no certificate behaves like the lowest rating (median €118k, close to G at €113k). For apartments it does not (median €330k, between B- and C). Certification also varies by district: about 80% of listings in Lisboa have a certificate, against 44% in Braga.

### Q6. How many listings does each district have, and do busier districts have more varied prices?

- **What the data shows:** there are 27 districts and an average of about 5,503 listings per district, but the spread is very uneven. Lisboa (38,127) and Porto (25,745) together hold 43% of all listings, and only 9 districts are above the average.
- **Insight, volume does not make prices steadier.** In each of the 12 largest districts the standard deviation is larger than the average price, from about 1.1 times the average in Porto to 2.0 times in Santarém. In absolute terms Faro (about €944k) and Lisboa (about €786k) are the most spread out. A district average alone is a weak guide to what a given listing should cost and needs to be split by property type.

### Q7. Which districts have high volume and competitive pricing?

![Flagged districts](images/Flagged%20districts.webp)

- **What the data shows:** a district is flagged when it has more listings than the 5,503 average **and** an average price below the overall €397k. Six pass: Porto, Braga, Coimbra, Aveiro, Santarém and Leiria, which together hold about 46% of all listings. Lisboa, Setúbal and Faro have high volume but average prices above the overall average.
- **Extra check, the six are not alike.** Comparing apartments only, the median price per m² is about €1,683 in Santarém, €2,196 in Braga, €2,324 in Leiria, €2,500 in Coimbra and €2,579 in Aveiro, but €3,333 in Porto, close to Faro (€3,286) and above Setúbal (€2,933). Porto passes the test on its overall average, but its apartments are priced much closer to the premium districts.
- **Insight:** the "competitive" label is relative to an overall average that Lisboa's expensive, apartment-heavy market pulls up. It is a good screen for finding active, lower-priced districts, but it needs the type and size checks above before it guides pricing decisions.

## Conclusion

1. **Location, type and size explain most of the price differences.** Part of the district gap is property mix, since a district with more land looks cheaper, but about two thirds of the gap between Lisboa and Santarém remains when comparing apartments only, so location matters in its own right.
2. **The market is concentrated and skewed.** Two districts hold 43% of listings, and the top 10% of listings hold about 40% of the value, so averages overstate what a typical property costs.
3. **Facilities and energy ratings mark the type of property.** Parking, elevators, bathrooms and good energy ratings all go with higher prices, and the link survives inside property types, but they also go with larger and newer properties, so they signal a segment more than they add a fixed premium. Houses show the strongest link with the energy rating.
4. **The opportunity is a group of active, lower-priced districts, but it is not one market.** Five districts (Santarém, Braga, Leiria, Coimbra and Aveiro) are clear value markets, Porto is a large mid-priced market, and Lisboa, Setúbal and Faro are premium markets.

## Recommendations

1. **Split the districts into three segments and treat them differently.** The apartment and house figures are medians from the extra check.

   | Segment | District | Apartment median per m² | Apartment median price | House median price |
   |---|---|---|---|---|
   | Value | Santarém | €1,683 | €189k | €165k |
   | Value | Braga | €2,196 | €240k | €277k |
   | Value | Leiria | €2,324 | €245k | €230k |
   | Value | Coimbra | €2,500 | €248k | €119k |
   | Value | Aveiro | €2,579 | €260k | €249k |
   | Mid-priced | Porto | €3,333 | €306k | €335k |
   | Premium | Lisboa | €4,150 | €380k | €625k |
   | Premium | Faro | €3,286 | €320k | €643k |
   | Premium | Setúbal | €2,933 | €275k | €490k |

   Lead volume-focused sales and marketing in the five value districts, scale Porto as the large mid-priced market, and in the premium districts focus on positioning and higher-value property types rather than competing on price.

2. **Price with three keys together: district, property type and size band.** The district alone is not enough, since one district can price houses and apartments very differently (Coimbra houses have a median of €119k, against €248k for its apartments). Use medians alongside averages as benchmarks, because a few expensive listings pull averages up.
3. **Judge price per m² within a size band.** The small bands run at about €2,630 to €3,851 per m² and the large bands at €219 or less, so they should never be compared with each other.
4. **Use facilities and energy ratings as selling points, but price them against size.** Bathrooms are the clearest signal of a higher-value listing, followed by parking and elevators. When one listing has more features than another, check whether it is simply larger before treating the gap as a premium.
5. **Test an energy certificate programme, starting with houses.** The link between rating and price is strongest for houses, and between 39% and 56% of listings in the six flagged districts have no certificate (against 20% in Lisboa), partly because land, which is almost never certified, makes up about 12% to 26% of their listings (6.5% in Lisboa). Helping sellers of uncertified houses get a certificate is worth testing, with results measured, because the data shows only an association.
6. **Add sales and date data before committing to the 10% goal.** The dataset holds asking prices only. Adding completed sales, sale prices and listing dates would show which districts and property types actually convert, how far asking prices fall at sale, and how long listings take to sell.

## Limitations

- Prices are listing prices from the dataset, referred to as selling prices in the analysis.
- The findings show associations, not causes. Features like parking and energy ratings go together with size, type and age.
- The cut-off values used to remove unrealistic data were set by judgement, and area is assumed to be in m².
- The dataset has no sales or date information, so the 10% sales goal cannot be tested directly.
- Small groups (island districts and rare property types) have very few listings, so their averages should be read with care.
- The extra checks (medians, apartments only, houses only) were run in pandas on the clean data and are not part of the Google Sheets.

## Tools

Python (pandas), Jupyter Notebook, Google Sheets (pivot tables, lookup and conditional formulas, charts), Google Slides
