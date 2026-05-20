# Crypto & Meme Coins Volume Momentum Analyzer
**A Data Analytics Project using MS Excel (Power Query, Power Pivot, and DAX)**

## 1. Project Description & Goal
In the cryptocurrency market, prices change very fast. Many regular investors lose money because they buy a coin too late, when the price is already too high (FOMO). 

The goal of this project is to find when "whales" (big investors) start buying a coin *before* the price goes up. To do this, I analyzed the daily trading volume. When the volume becomes much higher than normal (a "Volume Spike"), it means something big is happening.

### What this project found:
* **Market-Wide Volume Spike:** On **May 4, 2026**, the data showed a major volume increase across the whole market. Volumes went **from 220% up to 348%** above normal levels.
* **Top Performing Coin:** **PEPE** had the biggest volume explosion, reaching **348%** above its 3-day average.
* **Risk Categories:** I divided the coins into "Layer 1" (safer, like BTC, SOL) and "Meme Coins" (high risk, like PEPE, DOGE) so users can filter by risk.

## 2. Tools & Technologies Used
* **Power Query:** For cleaning and transforming the raw data.
* **Power Pivot & Data Model:** For connecting tables and building a Star Schema.
* **DAX Formulas:** For creating dynamic calculations (Measures).
* **Excel Dashboard:** Using Pivot Tables, interactive Slicers, and Charts.

---

## 3. Data Cleaning & Preparation (Power Query)
I imported the raw crypto data into Power Query and cleaned it using these steps:
1. **Data Types:** I changed the columns to the correct formats. `Date` became Date, and financial columns (`Volume`, `Market_Cap`) became Currency.
2. **New Metric (Turnover Rate):** I created a custom column called `Turnover_Rate` by dividing `[Volume] / [Market_Cap]`. This shows how fast money moves in each coin.
3. **Filtering Errors:** I added a rule where `Volume` must be greater than 0, to remove any broken data or errors.

---

## 4. Data Modeling (Star Schema)
To make the file light and professional, I did not keep everything in one flat sheet. I used Power Pivot to build a simple Star Schema model:
* **Fact Table:** `raw_crypto_data` (contains the daily numbers like volume and price).
* **Dimension Table:** I generated a clean `Calendar` table with no missing dates.
* **Relationship:** I connected `Calendar[Date]` to `raw_crypto_data[Date]` with a **One-to-Many (1:*) relationship**.

---

## 5. Calculations using DAX Measures
Instead of regular Excel formulas, I wrote 3 dynamic DAX measures to find the volume spikes:

### 1. Total Volume
Calculates the total trading volume:
```dax
Total_Volume := SUM(raw_crypto_data[Volume]).

---
                                            
 2. 3-Day Moving Average Volume
Calculates the average volume of the last 3 days. I used DATESBETWEEN to make sure today's spike does not change the historical average:  
    Vol_3Day_MA := 
CALCULATE(
    AVERAGEX(raw_crypto_data, raw_crypto_data[Volume]),
    DATESBETWEEN(
        'Calendar'[Date],
        MAX('Calendar'[Date]) - 3,
        MAX('Calendar'[Date]) - 1
    )
).

3. Volume Spike Alert (The Main Indicator)
Divides today's volume by the 3-day average. Anything above 100% means volume is growing. Over 200% means a major spike:
  
    Volume_Spike_Alert := DIVIDE([Total_Volume], [Vol_3Day_MA], 0)
```
                                                               
                                                                   
 6. Dashboard & Results
I built a clean, interactive dashboard to show the insights:

The Matrix Table: Shows the dates on the side and the coins on top. It only displays the Volume_Spike_Alert percentage, making it very easy to read.

Conditional Formatting: Cells turn red/green automatically when a coin goes over 200% momentum.

Interactive Slicers: Users can click buttons to filter by specific Ticker or Category (Layer 1 vs Meme Coins).

Trend Chart: A line chart that visualizes the volume jumps clearly.

Developed by George Sarakas | Junior Data Analyst

