# Dataview table Filter Menu for Obsidian
Dynamically created filter menu for dataview tables in obsidian

# How to use

### 1. Download the "Filter Table.md" and "filterMenu.css" files
  There are examples for steps 3. and 4. inside the "Filter Table.md" file.
  You will also need the "filterMenu.css" file, place this into the snippets folder of your vault and enable it in the settings (Keep in mind the styling is just a placeholder for now.)
  
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
  - ```['TEMP1','TEMP2']``` change this to the values that can be found in the key of your frontmatter (that this array will be filtering). These will also be used as labels in the dropdown menu.


### 5. Map and sort your dataview table
  - In the ```buildTable()``` function use the standard dataviewjs table mapping. Change the headers, change what you want to sort by and map your frontmatter keys to the headers.

# Spells Example

```// FILTER ARRAYS
let schoolTypes = ['abjuration','conjuration','divination','enchantment','evocation','illusion','necromancy','transmutation'];
let levelTypes = ['cantrip','1st-level','2nd-level','3rd-level','4th-level','5th-level','6th-level','7th-level','8th-level','9th-level'];
let compTypes = ['V','S','M'];
let concentration = ['Yes','No'];
let castingTimes = ['Action','Bonus','minute','hour','Reaction'];
let classNames = ['Artificer','Bard','Cleric','Druid','Paladin','Ranger','Sorcerer','Warlock','Wizard'];

// CATEGORY ARRAY
let categories = [{
				menuName: "School", 
				el: schoolTypes, 
				fmName: "school" 
				}, { 
				menuName: "Level",
				el: levelTypes,
				fmName: "level"
				}, {
				menuName: "Components",
				el: compTypes,
				fmName: "comp"
				}, {
				menuName: "Concentration",
				el: concentration,
				fmName: "concentration"
				}, {
				menuName: "Cast time",
				el: castingTimes,
				fmName: "time"
				}, {
				menuName: "Class",
				el: classNames,
				fmName: "class"
				}];

// GENERATE THE TABLE
function buildTable() {
	if (tbl) {
	    tbl.remove();
	}
	dv.table(["Name","School", "Level", "Concentration", "Casting Time", "Class"], 
		tableContents.sort(t => t.file.link)
		.map(t => [t.file.link, t.school, t.level, t.concentration, t.time, t.class])
	);
	tbl = document.getElementsByClassName("dataview")[0];
}
```
And this is the frontmatter of one of the spells.
```
---
level: 1st-level
school: abjuration 
class: Artificer, Ranger, Wizard
ritual: Yes
time: 1 'minute'
range: 30 feet
comp: V, S, M (a tiny bell and a piece of fine silver wire)
duration: 8 'hours'
concentration: No
---
```

So if i wanted to add a filter for rituals:
  - Add this to filter arrays ```let ritualFilter = ['Yes','No'];```
  - Add this to the category array
```
{
menuName: "Ritual", 
el: ritualFilter, 
fmName: "ritual" 
}
```

# Issues

 - Currently it's best to turn off live preview. I will try to fix this later on.
 - You cannot create a menu for sorting, will probably add later on.
 - There is no way to exclude values. Might change the checkboxes to custom divs with states so you can chose wheter you want to turn it off, include or exclude from the filter.
 - The filter works "top-down" or rather "left-right". It will first filter by the first category then those results will be filtered by the second category and so on. Currently no plan on "fixing" this.
 - Filters reset when you switch notes. Not sure if i can fix this but I'll see.
 - There is no styling. The reason is that the code is still a bit untidy so i didn't botter styling it much yet since it's likely to change. 
