# Dataview table Filter Menu for Obsidian
Dynamically created filter menu for dataview tables in obsidian

# How to use

### 1. Download the "Filter Table.md" file
  There are examples for steps 3. and 4. inside the file.
  
### 2. Change the path to your notes

  On line 3 ```let dvPages = dv.Pages('"NOTES_FOLDER_PATH"');``` change ```NOTES_FOLDER_PATH``` to the folder containing your note files.

### 3. Define the categories array

  Below the comment ```// CATEGORY ARRAY``` on line 11 define the filter dropdown menu buttons.
  - ```menuName``` is the text you want to appear on the button
  - ```el``` is the name of the filter array that you will define in the next step
  - ```fmName``` is the name of the key in your frontmatter that will be filtered by this array element

### 4. Define your Filter arrays

  Below the comment ```//FILTER ARRAYS``` on line 7 define all the values that you want to filter by. Each array should contain values that can be found in the key of your frontmatter that you're filtering.
  - ```FILTER_VALUES_PLACEHOLDER``` change this to the same name you wrote in the ```el``` attribute in the previous step
  - ```['TEMP1','TEMP2']``` change this to the values that can be found in the key of your frontmatter (that this array will be filtering)


### 5. Map and sort your dataview table
  - Standard dataviewjs table mapping. Change the headers, change what you want to sort by and map your frontmatter keys to the headers.

# Issues

 - Currently it's best to turn off live preview. I will try to fix this later on.
 - You cannot create a menu for sorting, will probably add later on.
 - There is no way to exclude values. Might change the checkboxes to custom divs with states so you can chose wheter you want to turn it off, include or exclude from the filter.
 - The filter works "top-down" or rather "left-right". It will first filter by the first category then those results will be filtered by the second category and so on. Currently no plan on "fixing" this.
 - There is no styling. The reason is that the code is still a bit untidy so i didn't botter styling it much yet since it's likely to change. 
