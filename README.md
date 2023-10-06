---
title: "SDS Project 1"
output: html_document
date: "2023-10-04"
---
Perscription: 
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

Hypothesis 2: 

```{r}

```

