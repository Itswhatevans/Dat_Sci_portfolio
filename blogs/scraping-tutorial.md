---
title: "Web Scraping Blog"
author: "Jordan Evans"
date: "2026-03-11"
categories: ["Data Science", "Quarto"]
---

# Scraping and Analyzing Rocket Launch Data

## Introduction / Motivation

Rocket launches have fascinated engineers and data scientists for decades. With the rise of commercial spaceflight, the number and frequency of launches have changed dramatically. This project demonstrates how to gather, clean, and explore rocket launch data using publicly available APIs, while teaching others how to replicate the process.

## Motivating Question

**How has global rocket launch activity changed over time?**  

We explore trends from the early space race (1957–1969) to the modern commercial era (2015–2019).

## Ethics and API Usage

Data were collected from the [Launch Library API](https://thespacedevs.com/llapi), which is publicly accessible for educational use. We took care to:

- Respect API rate limits and throttling
- Include a `User-Agent` in requests
- Avoid storing private keys

This ensures ethical and responsible data scraping.

## Steps to Gather the Data

1. Pull rocket launch data in two periods:
   - Early space race (1957–1969)
   - Modern commercial era (2015–2019)
2. Use Python `requests` to access the API and handle pagination.
3. Pause between requests to avoid throttling.
4. Save each dataset as CSV.
5. Clean data in Python:
    - See breakdown below under "Data Cleaning & Tranformations"
6. Combine the two datasets for analysis.

## Final Dataset Overview

- **Total observations:** 1000 launches  
- **Features:** 10 columns  
- **Source:** [Launch Library API](https://thespacedevs.com/llapi)  
- **Code repository:** [My Webscraping Repo](https://github.com/Itswhatevans/web_scraping_blog)

### Data Dictionary

| Variable | Description |
|----------|-------------|
| rocket_family | Launch vehicle designation (e.g., Falcon 9) |
| mission | Payload or mission name (e.g., Starlink 6-15) |
| rocket | Rocket configuration (full rocket model name) |
| provider | Launch service provider or organization |
| launch_date | Launch date and time (UTC) |
| year | Year of launch (numeric) |
| launch_site | Launch pad or complex |
| location | Geographic location of launch site |
| status | Launch outcome (e.g., Launch Successful, Launch Failure) |

### Data Cleaning & Transformations

- Split `name` into `rocket_family` and `mission` using Python string methods
- Standardized whitespace and capitalization
- Converted `launch_date` to `datetime` and extracted `year`
- Converted improper `launch_site` entries, which were not interpretable, to NaN. 
    - Totaling 151 observations.
    - Since `launch_site` is an unessential variable, we won't worry about the NaN values
- Dropped redundant columns for clarity

### Data Quality Considerations

- Some missions may have missing or ambiguous names
- Early data may have inconsistent formatting for provider or rocket
- Status labels are standardized, but historical reports may be incomplete
- The dataset does not include every launch worldwide — only those recorded in the API

## Summary Statistics (Example)

- Launches per year show a clear increase in the modern era compared to the early space race.
- Top providers include SpaceX, NASA, Soviet Space Program, and US Navy.
- Launch success rate trends can be visualized using the `status` column.

## Further Resources

- [Launch Library API Documentation](https://thespacedevs.com/llapi)
- [SpaceX Launch Manifest](https://www.spacex.com/launches/)
- [NASA Launch Schedule](https://www.nasa.gov/launchschedule)

---