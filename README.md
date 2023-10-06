Introduction:
Our project is on the data set gathered from insurance claims in Texas. This data set is from texas.gov and gives out information such as the complaint type, complaint confirmation, and how it was resolved. The Texas Insurance Claims Dataset is a comprehensive repository of insurance claims filed within the state of Texas, providing valuable insights into the insurance landscape in this region. This dataset encompasses a range of insurance types, including auto, home, health, and property insurance claims collected over a multi-year period. It contains detailed information about each claim, such as the claimant's demographic data, policy details, claim amount, claim date, and the cause of the claim, whether it be related to accidents, natural disasters, or other incidents. Additionally, the dataset includes geographical information, allowing for spatial analysis and regional comparisons, as Texas is known for its diverse climate and varying risk factors across different regions.

Our goal is to take note of and provide for relations between the different information gathered within our dataset. Our hypothesis would include the relationships between the parties involved in the case, the reasoning behind the complaint itself, and the time taken to receive and close the cases. Our group would like to gain more information on the factors at play within insurance using the different data types as a guide. For example, one hypothesis we came up with was that the most frequent insurance claim was filed as a life, accident, or health claim. Using correlations, our group plans to find important conclusions within the insurance industry in Texas.


Overview:
``` {r}
# Count the frequency of each coverage type in the "Coverage type" column
coverage_count <- table(Insurance_complaints_All_data$`Coverage type`)

# Create a data frame with coverage types and their frequencies
coverage_freq <- data.frame(Coverage_Type = as.factor(names(coverage_count)),
                             Frequency = as.numeric(coverage_count))

custom_colors <- rainbow(length(unique(coverage_freq$Coverage_Type)))

# Create a pie chart 
p <- ggplot(coverage_freq, aes(x = "", y = Frequency, fill = Coverage_Type)) +
  geom_bar(width = 1, stat = "identity") +
  coord_polar("y") +
  scale_fill_manual(values = custom_colors) +
  theme_void() +
  labs(title = "Pie Chart of Insurance Coverage Types",
       fill = "Insurance Type") +
  guides(fill = guide_legend(title = "Insurance Type"))

p + theme(legend.text = element_text(size = 8),
          legend.position = "right",
          legend.title = element_text(size = 10),
          plot.caption = element_text(size = 10, color = "gray"),
          plot.title = element_text(hjust = 0.5)) 

```



Hypothesis 1: 
Out of all the insurance types, the Property and Casualty insurance have the most complaints from the customers. 


Visualization 1: This bar plot only display three out of sixteen insurance types. These three inusrance types are the top three that received the most complaints from customers.  
```{r}


# Count the frequency of each unique complaint type in the dataset
complaint_type <- table(Insurance_complaints_All_data$`Complaint type`)

# Sort the complaint types based on their frequencies in descending order
sorted_counts <- sort(complaint_type, decreasing = TRUE)

# Select the top 3 most frequent complaint types
top_complaints <- names(sorted_counts[1:3])

# Create a data frame with the selected complaint types and their corresponding frequencies
frequency_insurance <- data.frame(Complaint_type = factor(top_complaints),
                                  Frequency = as.numeric(sorted_counts[1:3]))

library(ggplot2)

# Create a bar plot
ggplot(frequency_insurance, aes(x = Complaint_type, y = Frequency, fill = Complaint_type)) +
  theme(legend.position = "none", plot.title = element_text(hjust = 0.5)) + 
  labs(
    x = "Insurance Type",
    y = "Frequency",
    title = "Number of Complaints Types from Customers"
  ) + 
  geom_bar(stat = "identity") +
  scale_fill_manual(values = c("#1f77b4", "#ff7f0e", "#2ca02c"))



```

Hypothesis 2: Insurance companies that process insurance claims receives fewer complaints. "Life, Accident, and Health" insurance tends to take the longest because claims like this can involve suspicious circumstances that may prolong the claim processing time. 

```{r}

# Convert 'Received date' and 'Closed date' columns to Date objects for average time calculation
Insurance_complaints_All_data$`Received date` <- as.Date(Insurance_complaints_All_data$`Received date`, format = "%m/%d/%Y")
Insurance_complaints_All_data$`Closed date` <- as.Date(Insurance_complaints_All_data$`Closed date`, format = "%m/%d/%Y")

# Calculate resolved time for each complaint
Insurance_complaints_All_data$Resolved_time <- as.numeric(difftime(Insurance_complaints_All_data$`Closed date`, Insurance_complaints_All_data$`Received date`, units = "days"))


# Calculate average resolved time for each complaint type
average_resolved_time <- Insurance_complaints_All_data %>%
  group_by(`Complaint type`) %>%
  summarise(Average_resolved_time = mean(Resolved_time, na.rm = TRUE))

# Remove the variable named "NA" 
filtered_resolved_time <- average_resolved_time[!is.na(average_resolved_time$`Complaint type`), ]

# Plotting the bar graph
plot <- ggplot(data = filtered_resolved_time, aes(x = reorder(`Complaint type`, Average_resolved_time), y = Average_resolved_time)) +
  geom_bar(stat = "identity", fill = "skyblue") +
  labs(title = "Average Resolved Time for Each Insurance Complaint Type",
       x = "Complaint Type",
       y = "Average Resolved Time (Days)") +
    theme(axis.text.x = element_text(angle = 45, hjust = 1),
        plot.title = element_text(hjust = 0.5)) 


plot
```

