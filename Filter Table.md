```dataviewjs


let tbl;
let dvPages = dv.pages('"_DM Screen/Spells/Blocks"');
let tableContents = dvPages;
let fltArr = [];

// FILTER ARRAYS
let schoolTypes = ['abjuration','conjuration','divination','enchantment','evocation','illusion','necromancy','transmutation'];
let levelTypes = ['cantrip','1st-level','2nd-level','3rd-level','4th-level','5th-level','6th-level','7th-level','8th-level','9th-level'];
let compTypes = ['V','S','M'];
let concentration = ['Yes','No'];
let castingTimes = ['Action','Bonus','minute','hour','Reaction'];
let classNames = ['Artificer','Bard','Cleric','Druid','Paladin','Ranger','Sorcerer','Warlock','Wizard'];

// CATEGORY ARRAY
let categories = [{
				tabName: "School", // NAME OF THE FILTER MENU
				el: schoolTypes, // FILTER ARRAY FOR THIS MENU
				fmName: "school", // NAME OF YOUR FRONTMATTER ATTRIBUTE
				}, { 
				tabName: "Level",
				el: levelTypes,
				fmName: "level",
				}, {
				tabName: "Components",
				el: compTypes,
				fmName: "comp",
				}, {
				tabName: "Concentration",
				el: concentration,
				fmName: "concentration",
				}, {
				tabName: "Cast time",
				el: castingTimes,
				fmName: "time",
				}, {
				tabName: "Class",
				el: classNames,
				fmName: "class",
				}];

// GENERATE THE TABLE
function buildTable() {
	if (tbl) {
	    tbl.remove();
	}
	dv.table(["Name","School", "Level", "Concentration", "Casting Time", "Class"], // ENTER TABLE HEADERS HERE
		tableContents.sort(t => t.file.link)
		.map(t => [t.file.link, t.school, t.level, t.concentration, t.time, t.class]) // ENTER YOUR FRONTMATTER ATTRIBUTE NAMES HERE
	);
	tbl = document.getElementsByClassName("dataview")[0];
}

// FILTER BY CHECKED
function isChecked(){
	tableContents = dvPages;

	for (let i = 0; i < categories.length; i++) {
		fltArr = [];
		document.querySelectorAll("#drop-"+categories[i].fmName+" div input:checked").forEach(element => fltArr.push(element.parentElement.getAttribute("data-value")));

		tableContents = tableContents.filter(q => {
		let match = true;
		if (fltArr.length > 0) {
			match = fltArr.some(r => q[categories[i].fmName].includes(r));
		}
		return match;
		});
	}

	buildTable();
}

// OPEN FILTER MENU ON CLICK
function showMenu() {
	var dropdowns = document.getElementsByClassName("dropdown-content");
    var i;
    for (i = 0; i < dropdowns.length; i++) {
      var openDropdown = dropdowns[i];
      if (openDropdown.classList.contains('show')) {
        openDropdown.classList.remove('show'); 
   }
 } 
	this.firstChild.classList.toggle("show");
}

// CLOSE MENUS ON WINDOW CLICK
window.addEventListener("click", function(event) {
  if (event.target.matches('.dropbtn')) {
	  return;
	  }
	  
    var dropdowns = document.getElementsByClassName("dropdown-content");
    var i;
    for (i = 0; i < dropdowns.length; i++) {
      var openDropdown = dropdowns[i];
      if (openDropdown.classList.contains('show')) {
        openDropdown.classList.remove('show'); 
   }
 } 
})


//FILTER MENU CREATION

let root = await this.container.createEl("div", {cls: "dropMenu"});

for (let i = 0; i < categories.length; i++) {
	const testButton = root.createEl("button", {cls:"dropbtn", attr:{id:"drop-"+categories[i].fmName}})
	testButton.addEventListener("click",showMenu);
	const container = testButton.createEl("div", {cls:"dropdown-content"});
	testButton.createEl("h5", {text:categories[i].tabName, cls:"inaktiv"});
	let element = categories[i].el;
	for (let j = 0; j < element.length; j++) {
		container.createEl("label",{text:element[j].charAt(0).toUpperCase()+element[j].slice(1), 
			attr:{"data-value":element[j]}}).createEl("input", {type:"checkbox"}).addEventListener("click",isChecked);
	}
}

//INITIAL TABLE
buildTable();
```