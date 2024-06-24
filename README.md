# 🚴 Cyclistic Bike Usage Analysis 🚴

## 📋 Introduction

Cyclistic, a bike-share program in Chicago, features a fleet of over 5,800 bicycles and 600 docking stations. The company offers various bike types, including reclining bikes, hand tricycles, and cargo bikes, making bike-sharing accessible to a broader audience. The majority of Cyclistic users ride for leisure, but approximately 30% use the bikes for commuting. Cyclistic offers flexible pricing plans, including single-ride passes, full-day passes, and annual memberships. The company’s finance analysts have determined that annual members are more profitable than casual riders. Therefore, the marketing team aims to convert casual riders into annual members.

This report aims to analyze the differences in usage patterns between casual riders and annual members. The findings will provide insights that will inform the development of targeted marketing strategies to increase the number of annual memberships.

## 📊 Data Preparation

### 📂 Data Sources

The data used for this analysis comprises Cyclistic's historical trip data over the past 12 months. The data includes information on ride start and end times, ride duration, user type (casual or annual member), and more.

### 🛠️ Data Cleaning and Processing

1. **Conversion of Data Types:**
   - The `started_at` and `ended_at` columns were converted to datetime format.
   - A new column `ride_length` was created to calculate the duration of each ride.
   - A `day_of_week` column was created to identify the day each ride started.

2. **Handling Missing Values:**
   - Missing values in the `ride_length` and `day_of_week` columns were removed.
   - Negative ride lengths were filtered out.

### 🗂️ Data Aggregation

The cleaned data was aggregated to calculate:
- Average ride length for casual riders and annual members.
- Total number of rides for each user type.
- Average ride length by day of the week for both user types.

## 🔍 Analysis and Key Findings

### ⏱️ Average Ride Length by User Type

![Average Ride Length by User Type](insert-plot-here)

The analysis shows that casual riders tend to have longer average ride lengths compared to annual members. This suggests that casual riders may be using the bikes for leisure activities, which typically involve longer trips, while annual members might be using the bikes for shorter, routine commutes.

### 🕒 Distribution of Ride Start Times by User Type

![Distribution of Ride Start Times by User Type](insert-plot-here)

The distribution of ride start times indicates that casual riders often start their rides during weekends and peak times (e.g., late mornings and afternoons). In contrast, annual members exhibit more consistent usage patterns throughout the day, likely related to commuting to and from work.

### 📅 Average Ride Length by Day of the Week

![Average Ride Length by Day of the Week](insert-plot-here)

The average ride length by day of the week reveals that casual riders have significantly longer rides during weekends. This further supports the idea that casual riders use Cyclistic bikes primarily for leisure. On the other hand, annual members maintain relatively stable ride lengths throughout the week, consistent with daily commuting.

## 💡 Recommendations

Based on the analysis, the following recommendations are proposed to convert casual riders into annual members:

1. **🎉 Targeted Promotions:**
   - Offer special promotions or discounts during weekends to encourage casual riders to experience the benefits of membership. Weekend campaigns could highlight the cost savings and convenience of annual memberships.

2. **💼 Enhanced Membership Benefits:**
   - Emphasize benefits that appeal to commuters, such as discounted rates for daily rides, priority access to bikes, and special commuter perks. Highlighting these benefits could attract casual riders who commute regularly.

3. **📍 Location-Based Campaigns:**
   - Implement marketing campaigns at popular casual rider locations, such as tourist spots and recreational areas. On-site sign-up offers and promotions could convert casual riders who frequently use bikes in these areas into annual members.

## 📝 Conclusion

The analysis provides valuable insights into the different usage patterns of casual riders and annual members. Casual riders primarily use Cyclistic bikes for longer, leisure trips, particularly on weekends, while annual members use the bikes for shorter, more consistent commutes throughout the week. By targeting promotions and enhancing membership benefits, Cyclistic can effectively convert casual riders into annual members, driving future growth and profitability.

## 📚 Appendix

### 🧹 Data Cleaning and Processing Code

```python
import pandas as pd
import os

# Define the path to your combined data
combined_data_path = r'C:\Users\ALEX-DHEBS\Desktop\Portfolio\Bike_share_analysis\Cyclistic_Data\combined_cyclistic_data.csv'

# Load the combined data
all_data = pd.read_csv(combined_data_path)

# Convert 'started_at' and 'ended_at' to datetime
all_data['started_at'] = pd.to_datetime(all_data['started_at'])
all_data['ended_at'] = pd.to_datetime(all_data['ended_at'])

# Create 'ride_length' column
all_data['ride_length'] = all_data['ended_at'] - all_data['started_at']

# Create 'day_of_week' column
all_data['day_of_week'] = all_data['started_at'].dt.dayofweek + 1  # Monday=1, Sunday=7

# Handle missing values
all_data = all_data.dropna(subset=['ride_length', 'day_of_week'])

# Filter out negative ride lengths
all_data = all_data[all_data['ride_length'] > pd.Timedelta(0)]

# Convert 'ride_length' to seconds for easier aggregation
all_data['ride_length'] = all_data['ride_length'].dt.total_seconds()

# Save the cleaned data to a new CSV file
cleaned_file_path = os.path.join(os.path.dirname(combined_data_path), 'cleaned_cyclistic_data.csv')
all_data.to_csv(cleaned_file_path, index=False)
