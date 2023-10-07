**What is insurance claim and why does it matter?**
Insurance claims are made by the policyholder to an insurance company for financial compensation or coverage for a loss under the terms of an insurance policy. If an event occurs that causes loss to things such as property, illness, accident, or damage, the policyholder can file a claim to receive financial compensation from the insurance company. Insurance is crucial because it provides financial protection against unexpected losses. Without insurance, individuals and businesses might face financial hardships due to unexpected losses and risks. 


**Our data:**
Our project is on the data set gathered from insurance claims in Texas. This data set is from texas.gov and gives out information such as the complaint type, complaint confirmation, and how it was resolved. The Texas Insurance Claims Dataset is a comprehensive repository of insurance claims filed within the state of Texas, providing valuable insights into the insurance landscape in this region. This data set encompasses a range of insurance types, including auto, home, health, and property insurance claims collected over a multi-year period. It contains detailed information about each claim, such as the claimant's demographic data, policy details, claim amount, claim date, and the cause of the claim, whether it be related to accidents, natural disasters, or other incidents. Additionally, the dataset includes geographical information, allowing for spatial analysis and regional comparisons, as Texas is known for its diverse climate and varying risk factors across different regions.

Our data set can be accessed [here][https://data.texas.gov/dataset/Insurance-complaints-All-data/ubdr-4uff].


**Our goal:**
Our goal is to take note of and provide for relations between the different information gathered within our data set. Our hypothesis would include the relationships between the parties involved in the case, the reasoning behind the complaint, and the time to receive and close the cases. Our group would like to gain more information on the factors at play within insurance using the different data types as a guide. For example, one hypothesis we came up with was that the most frequent insurance claim was filed as a life, accident, or health claim. Using correlations, our group plans to find important conclusions within the insurance industry in Texas.


**Types of Insurance Analyzed**
``` {r}


# Count the frequency of each coverage type in the "Coverage type" column
coverage_count <- table(Insurance_complaints_All_data$`Coverage.type`)

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
The pie graph above displays the types of insurance coverage that are analyzed in this data set. As you can see, Automobile and Accident and Health insurance coverage consist of most portion of the pie. These two types of coverage are among the most popular types of insurance coverage for several reasons:

-   Everyday Necessities
-   Legal Requirement
-   Financial Protection
-   Employer-Sponsored

The other insurance coverage is less popular than the two that are mentioned. However, they are very crucial during the process of managing risk. 



**Hypothesis 1:**
The type of insurance complaints related to "Life, Accident, and Health" issues will be the most frequent among the various insurance complaints. This type of insurance is the highest due to numerous factors:

-   complaints about medical procedure coverage
-   complaints about the policy
-   complaints about the billing processes

This will increase the likelihood of disputes and disagreements between the insurance company and the policyholder, leading to many complaints about denied insurance claims. 

```{r}


# Count the frequency of each unique complaint type in the dataset
complaint_type <- table(Insurance_complaints_All_data$`Complaint.type`)

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
**Visualization 1:** 
This bar plot only displays three out of sixteen insurance types. These three insurance types are the top three that received the most complaints from customers. Based on the graph, our hypothesis is not correlated to the visualization. As a result, the Property and Casualty category received the most complaints out of all complaints. This may be due to a couple of reasons:

-   claim complexity: claims related to damage can involve complex assessments and negotiation
-   property and claims are higher than expected: high frequency of complaints due to claim denials or settlement

Although Property and Casualty complaints are on top of the chart, Life, Accident, and Health complaints are just slightly behind. 
 


**Hypothesis 2:** 
Insurance companies that process insurance claims receive fewer complaints. Life insurance takes the longest because claims like this can involve suspicious circumstances that may prolong the claim processing time. Additionally, it takes time for the insurance companies to ensure the claim is legitimate. 

```{r}

# Convert 'Received date' and 'Closed date' columns to Date objects for average time calculation
Insurance_complaints_All_data$`Received date` <- as.Date(Insurance_complaints_All_data$`Received.date`, format = "%m/%d/%Y")
Insurance_complaints_All_data$`Closed date` <- as.Date(Insurance_complaints_All_data$`Closed.date`, format = "%m/%d/%Y")

# Calculate resolved time for each complaint
Insurance_complaints_All_data$Resolved_time <- as.numeric(difftime(Insurance_complaints_All_data$`Closed date`, Insurance_complaints_All_data$`Received date`, units = "days"))


# Calculate average resolved time for each complaint type
average_resolved_time <- Insurance_complaints_All_data %>%
  group_by(`Coverage.type`) %>%
  summarise(Average_resolved_time = mean(Resolved_time, na.rm = TRUE))

# Remove the variable named "NA" 
filtered_resolved_time <- average_resolved_time[!is.na(average_resolved_time$`Coverage.type`), ]

# Plotting the bar graph
plot <- ggplot(data = filtered_resolved_time, aes(x = reorder(`Coverage.type`, Average_resolved_time), y = Average_resolved_time)) +
  geom_bar(stat = "identity", fill = "skyblue") +
  labs(title = "Average Resolved Time for Each Insurance Coverage type",
       x = "Coverage type",
       y = "Average Resolved Time (Days)") +
    theme(axis.text.x = element_text(angle = 45, hjust = 1),
        plot.title = element_text(hjust = 0.5)) 
plot

```
**Visualization 2:**
In the above bar graph, we wanted to see how long a claim stays open or how long it takes insurance companies to resolve it. Using the "Received date" and "Closed date" columns from the data set, we found when the claim stayed open. From there, we compared the average time for each insurance coverage type with the other types. It turns out that Life and annuity coverage and the Accident and Health coverage's claim life span are pretty close. One reason for this might be due to the high frequency of claims from the policyholders and the high frequency of disputes from insurance companies. 

**Reference:**
"Insurance Claims and Policy Processing Clerks." Career Information Center, edited by Kristin B. Mallegg and Joseph Palmisano, 10th ed., vol. 6: Finance, Macmillan Reference USA, 2014, pp. 52-55. Gale In Context: College, link.gale.com/apps/doc/CX3723700249/CSIC?u=txshracd2598&sid=bookmark-CSIC&xid=60a07f3d. Accessed 6 Oct. 2023.

Insurance, Texas Department of. “Insurance Complaints: All Data: Open Data Portal.” Https://Data.Texas.Gov/, 6 Oct. 2023, data.texas.gov/dataset/Insurance-complaints-All-data/ubdr-4uff. 
