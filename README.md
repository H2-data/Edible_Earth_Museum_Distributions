<div align="center">

# The Museum of Edible Earth

</div>

---

### **IMPORTANT NOTE:** 

The following data is real data from a real collection exhibit, however most of this information is publicly available on the exhibit website and catalog. Furthermore, I have received permission from masharu to use the visuals and dashboards in my portfolio, and the master dataset will not be shown due to privacy reasons.

### **Scenario and Objective:**

The Museum of Edible Earth is a collection exhibit created and curated by Masharu Studios. It is a collection of different mineral samples associated with geophagy, the practice of consuming rocks and Earth for health or cultural purposes. I have been asked to answer the following questions:

- What is the distribution of collection samples over the years?

- Where were the samples collected?

- What are the distributions of taste and texture?

- What are the distributions of shape, color and composition?

### **Data Preprocessing:**

Aside from generic cleaning (Outliers, Duplicates and Missing Values), this data presented unique preprocessing challenges. The mineral taste and texture descriptions were actually obtained publicly from comment sections on the website and placed into a word cloud on the mineral sample's catalog page like so: 

This presented the following problems:

1. Because the descriptions from the comment section are so varied, there are over 600 unique descriptions for a relatively small collection of minerals. To solve this, I created a list of 20 tastes and 20 textures, and I applied them to each unique value in Excel. Below are a few examples.

[INSERT EXAMPLES HERE]

2. Next, I have to map the flavors and textures onto the old ones. I used the following code to ensure that each flavor was replaced with a combination the chosen tastes and textures from my list, and that each one only appears once per row.

Here is the sample output:

[INSERT EXAMPLES HERE]

For analysis and visualization, it will be split using a method called 'explode'. This will split the values of my taste or texture column and duplicate everything around it in a new table.

[INSERT EXAMPLES HERE]

To see the full scraping and cleaning process, please see the scraping section and the cleaning section of the project, linked below.

### **Results and Observations:**

Let's go through and answer each of the data questions:

- What is the distribution of collection samples over the years?

counts = df['Acq Year'].value_counts().reset_index()
countsy = counts.sort_values(by = 'Acq Year')
countsy

[INSERT TABLEAU IMAGE DASHBOARD HERE]

- Where and how were the minerals acquired? What are the top 10 countries for mineral acquisition?

[INSERT TOP 10 TABLE, MAP AND MAYBE LIKE A BAR/BUBBLE CHART HERE]

- What is the distribution of taste? What are the top 5?

```Python
taste_counts = df_exploded1['Taste'].value_counts().reset_index()
taste_counts.nlargest(5, 'count').set_index('Taste')
```
[INSERT TABLE AND VISUAL HERE]

|Taste|Sample Count|
|---|---|
|Earthy|119|
|Milky|96|
|Experience|86|
|Herbal|74|
|Bitter|71|

- What is the distribution of texture? What are the top 5?

```Python
texture_counts = df_exploded2['Texture'].value_counts().reset_index()
texture_counts.nlargest(5, 'count').set_index('Texture')
```

[INSERT TABLE AND VISUAL HERE]

|Texture|Sample Count|
|---|---|
|Soft|144|
|Damp|134|
|Hard|130|
|Creamy|96|
|Crunchy|90|

What is the distribution of Shape, Color and Composition?

[INSERT TABLE AND VISUAL HERE]

To see all comments and Python visuals, please see the analysis section of this project, linked below:

To see the final Tableau dashboard, please see the dashboard section of this project, linked below.

### **Conclusions:**

- Overall 
