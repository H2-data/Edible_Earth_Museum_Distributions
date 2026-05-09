<div align="center">

# The Museum of Edible Earth

</div>

---

### **IMPORTANT NOTE:** 

The following data is real data from a real collection exhibit, however most of this information is publicly available on the exhibit website and catalog. Furthermore, I have received permission from masharu, the exhibit curator, to use the visuals and dashboards in my portfolio, and the master dataset will not be shown due to privacy reasons.

### **Scenario and Objective:**

The Museum of Edible Earth is a collection exhibit created and curated by Masharu Studios. It is a collection of different mineral samples associated with geophagy, the practice of consuming rocks and earth for health or cultural purposes. I have been asked to answer the following questions:

- What is the distribution of collection samples over the years?

- Where were the samples collected?

- What are the distributions of taste and texture?

- What are the distributions of shape, color and composition?

### **Data Report:**

<img width="1249" height="999" alt="The Museum of Edible Earth (1)" src="https://github.com/user-attachments/assets/a40f1816-4af8-4a99-aea0-d6d748e29bbc" />

To interact with the dashboard, see the Tableau section of the project, linked here:

### **Data Preprocessing:**

Aside from generic cleaning (outliers, duplicates and missing values), this data presented unique preprocessing challenges. The mineral taste and texture descriptions were actually obtained publicly from comment sections on the website and placed into a word cloud on the mineral sample's catalog page like so: 

<div align="center">
  <img width="800" height="452" alt="image" src="https://github.com/user-attachments/assets/4d96ef2f-bde6-4319-9299-d0de595ff69d" />
</div>

This presented the following problems:

1. Because the descriptions from the comment section are so varied, there are over 600 unique descriptions for a relatively small collection of minerals, which isn't very good for visualization. To solve this, I created a list of 20 tastes and 20 textures, and I applied them to each unique value in Excel. Below are a few examples.

| Description | Taste | Texture |
| :--- | :--- | :--- |
| baking soda | Chemical, Bitter | Powdery |
| calcium | Metallic | Chalky |
| candy | Sweet | Hard |
| cinnamon | Sweet, Spicy | Powdery |
| dry | N/A | Dry |

<img width="167" height="377" alt="image" src="https://github.com/user-attachments/assets/9bf13743-2fc9-4585-ba94-08b7f864bfad" />

2. Next, I have to map the new shortlist of flavors and textures onto the original data. I used the following code to ensure that each original flavor/texture was replaced with a combination the tastes and textures from my shortlist.

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

Scraping:
Preprocessing:

### **Results and Observations:**

Let's go through and answer each of the data questions:

- Where and how were the minerals acquired? What countries where they commonly acquired?

<img width="1249" height="286" alt="The Museum of Edible Earth (1)" src="https://github.com/user-attachments/assets/a69d3042-4081-470b-ae73-59039bccc277" />

- What is the distribution of taste? What are the most dominant tastes?

<img width="1228" height="675" alt="image" src="https://github.com/user-attachments/assets/2c5279af-a176-4d66-bac5-43e97867ef8b" />

- What is the distribution of texture? What are the most dominant textures?
  
<img width="1232" height="662" alt="image" src="https://github.com/user-attachments/assets/08a918f4-68e2-4e9b-935b-41dc67c2f3d3" />

- What is the distribution of Shape, Color and Composition?
  
<img width="1219" height="274" alt="image" src="https://github.com/user-attachments/assets/a7fb243e-a598-47cc-8816-5b2fdf33fec2" />

To see all comments and Python visuals, please see the analysis section of this project, linked below:

To see the final Tableau dashboard, please see the dashboard section of this project, linked below.

Analysis:
Tableau:

### **Conclusions:**
