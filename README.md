<div align="center">

# The Museum of Edible Earth

</div>

---

### **Important Notes:** 

- The following data is real data from a real collection exhibit, however most of this information is publicly available on the exhibit website and catalog. Furthermore, I have received permission from masharu, the exhibit curator, to use the visuals and dashboards in my portfolio, and the master dataset will not be shown due to privacy reasons.
- Files are labelled from 01-04. They can be read in numeric order. The dashbord is screenshotted and linked below, and it is the last step in the process.

### **Scenario and Objective:**

The Museum of Edible Earth is a collection exhibit created and curated by Masharu Studios. It is a collection of different mineral samples associated with geophagy, the practice of consuming rocks and earth for health or cultural purposes. I have been asked to answer the following questions:

- What is the distribution of collection samples over the years?

- Where were the samples collected?

- What are the distributions of taste and texture?

- What are the distributions of shape, color and composition?

### **Data Report:**

<img width="1249" height="999" alt="The Museum of Edible Earth (1)" src="https://github.com/user-attachments/assets/a40f1816-4af8-4a99-aea0-d6d748e29bbc" />

To interact with the dashboard, see the Tableau section of the project, linked here:

[Dashboard](https://public.tableau.com/views/EdibleEarthPortfolio/Dashboard1?:language=en-US&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link)

### **Data Preprocessing:**

Aside from generic cleaning (outliers, duplicates and missing values), this data presented unique preprocessing challenges. The mineral taste and texture descriptions were actually obtained publicly from comment sections on the website and placed into a word cloud on the mineral sample's catalog page like so: 

<img width="1292" height="627" alt="image" src="https://github.com/user-attachments/assets/dce3520e-876b-4fcf-95be-50ee50f06339" />
<br>

This presented the following problems:

1. Because the descriptions from the comment section are so varied, there are over 600 unique descriptions for a relatively small collection of minerals, which isn't very good for visualization. To solve this, I created a list of 20 tastes and 20 textures, and I applied them to each unique value in Excel. Below are a few examples.


<div align="center">
<img width="660" height="695" alt="image" src="https://github.com/user-attachments/assets/9ecee814-54de-4d4a-b75f-784ef0c4e5fc" />
</div>
<br>
2. Next, I have to map the new shortlist of flavors and textures onto the original data. I used the following code to ensure that each original flavor/texture was replaced with a combination of the tastes and textures from my shortlists.

```Python
taste_map = df2.set_index('Description')['Taste'].to_dict()
texture_map = df2.set_index('Description')['Texture'].to_dict()

def process_column(df, target_map):
    return (
        df['Tags'].str.split(',').explode().str.strip()
        .map(target_map).dropna().groupby(level=0)
        .unique().str.join(', ')
    )

df3['Taste'] = process_column(df3, taste_map)
df3['Texture'] = process_column(df3, texture_map)
```

For analysis, visualization and ensuring each value only appears once per cell, the data will be split using a method called 'explode'. This will split the values of my taste or texture column and duplicate everything around it in a new table. After dropping duplicates and ensuring the data was stripped and clean, I got the following output:

|SKU|Name|Taste|Texture|
|---|---|---|---|
|CG002C|Mabele Smoked Clay|N/A|N/A|
|CG001A|Clay Dappermarkt|Bitter, Sour|Dusty, Dry|
|CH001A|Gletschermilch|N/A|N/A|
|CI001A|Salty Calaba|Experience, Earthy, Milky, Herbal, Bitter, Sour, Salty, Sweet|Chalky, Experience, Dense, Creamy, Crunchy, Dry, Soft, Smooth, Airy, Damp, Dusty, Powdery, Damp, Grainy, Sticky|
|CI002A|Salty Calaba|Milky, Earthy, Herbal, Bitter, Sour, Salty, Tasty|Creamy, Crunchy, Melty, Soft, Damp, Dry, Dusty, Sticky|

To see the full scraping and cleaning process, please see the scraping section and the preprocessing section of the project, linked below.

[Dictionary](01_Dictionary_and_Context.ipynb)  
[Scraping](02_Scraping_Dirt_S.ipynb)  
[Preprocessing](03_CleaningDirt.ipynb)  

### **Results and Observations:**

Let's go through and answer each of the data questions:

- Where and how were the minerals acquired? What countries where they commonly acquired?

<img width="1249" height="286" alt="The Museum of Edible Earth (1)" src="https://github.com/user-attachments/assets/a69d3042-4081-470b-ae73-59039bccc277" />
<br>

Most of the samples were collected in 2018 and 2019-2021. Most acquisitions were done over the internet, or received as gifts. A vast majority of samples came from Russia, Suriname and Ukraine.

- What is the distribution of taste? What are the most dominant tastes?

<img width="1227" height="492" alt="image" src="https://github.com/user-attachments/assets/21ff1ae6-aa4b-4f86-908e-5f430752172c" />
<br>

The top 5 tastes are **Earthy, Milky, Herbal, Bitter and Sweet.** I won't count 'Tasty' or 'Experience', as they're a different kind of descriptive.

- What is the distribution of texture? What are the most dominant textures?
  
<img width="1227" height="492" alt="image" src="https://github.com/user-attachments/assets/4d4e4f91-a031-4bb0-82a3-76ed96a1bde4" />
<br>

The top 5 textures are **Soft, Hard, Damp, Creamy and Crunchy.** I won't count 'Tasty' or 'Experience', as they're a different kind of descriptive.

- What is the distribution of Shape, Color and Composition?
  
<img width="1219" height="274" alt="image" src="https://github.com/user-attachments/assets/a7fb243e-a598-47cc-8816-5b2fdf33fec2" />
<br>

The most common sample attributes are a clay-like composition, a white color and a raw shape.

To see all comments, conclusions and Python visuals, please see the analysis section of this project, linked below:

[Analysis](04_EdibleAnalysis.ipynb)
