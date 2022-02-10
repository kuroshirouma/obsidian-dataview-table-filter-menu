```dataviewjs
let tbl;
let dvPages = dv.pages('"NOTES_FOLDER_PATH"');
let tableContents = dvPages;
let fltArr = [];

// FILTER ARRAYS
let FILTER_VALUES_PLACEHOLDER1 = ['TEMP1','TEMP2']; //EXAMPLE: let schoolTypes = ['abjuration','divination']
let FILTER_VALUES_PLACEHOLDER2 = ['TEMP3','TEMP4']; 

// CATEGORY ARRAY
let categories = [{
				menuName: "CHANGE_ME", 			// NAME OF THE FILTER MENU - EXAMPLE: tabName: "Schools",
				el: FILTER_VALUES_PLACEHOLDER1, // FILTER ARRAY FOR THIS MENU - EXAMPLE: el: schoolTypes,
				fmName: "FM_KEY_NAME" 			// NAME OF YOUR FRONTMATTER KEY - EXAMPLE: fmName: "school"
				}, { 
				menuName: "CHANGE_ME",
				el: FILTER_VALUES_PLACEHOLDER2,
				fmName: "FM_KEY_NAME2"
				}];

// GENERATE THE TABLE
function buildTable() {
	if (tbl) {
	    tbl.remove();
	}
	dv.table(["File Name","Header2","Header3"], 				// ENTER TABLE HEADERS HERE
		tableContents.sort(t => t.file.link)  					// CHANGE WHAT TO SORT BY
		.map(t => [t.file.link, t.FM_KEY_NAME, t.FM_KEY_NAME2]) // ENTER YOUR FRONTMATTER KEYS HERE
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
	testButton.createEl("h5", {text:categories[i].menuName, cls:"inaktiv"});
	let element = categories[i].el;
	for (let j = 0; j < element.length; j++) {
		container.createEl("label",{text:element[j].charAt(0).toUpperCase()+element[j].slice(1), 
			attr:{"data-value":element[j]}}).createEl("input", {type:"checkbox"}).addEventListener("click",isChecked);
	}
}

//INITIAL TABLE
buildTable();
```