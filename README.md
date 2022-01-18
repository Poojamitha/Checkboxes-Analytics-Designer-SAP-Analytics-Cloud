# Checkboxes-Analytics-Designer-SAP-Analytics-Cloud
Analytics Designer designer script
//Script object utils script fuctions//
//Set dimension checkboxes//
Checkbox_Columns.removeAllItems();
Checkbox_Rows.removeAllItems();
Checkbox_Free.removeAllItems();

var CurrentDimensionColumn = ArrayUtils.create(Type.string);
var CurrentDimensionRows= ArrayUtils.create(Type.string);
console.log(["Current Dimension columns should be empty",CurrentDimensionColumn.slice()]);
console.log(["Current Dimension rows should be empty",CurrentDimensionRows.slice()]);

//Dimension in columns:
var dimCol = Table_1.getDimensionsOnColumns();
if(dimCol.length>0){
	for(var i=0;i<dimCol.length;i++){
		CurrentDimensionColumn.push(dimCol[i]);
		console.log(["CurrentDimensionColumn",dimCol[i]]);
	}
}

//Dimension in Rows:
var dimRow = Table_1.getDimensionsOnRows();
if(dimRow.length>0){
	for(i=0;i<dimRow.length;i++){
		CurrentDimensionRows.push(dimRow[i]);
		console.log(["CurrentDimensionRow",dimRow[i]]);
	}
}

//Get all dimensions:
if (All_Dimensions.length>0){
	for(i=0;i<All_Dimensions.length;i++){
		if(All_Dimensions[i]!==""){
			Checkbox_All_Dimensions.setSelectedKeys([All_Dimensions[i]]);
			var dimdesc=Checkbox_All_Dimensions.getSelectedKeys();
			Checkbox_Free.addItem(All_Dimensions[i],dimdesc[0]);
			console.log(["All_Dimensions",All_Dimensions[i],dimdesc[0]]);
		}
	}
}
console.log(["Current Dimension Column",CurrentDimensionColumn]);
console.log(["Current Dimension Rows",CurrentDimensionRows]);

// Remove the dimensions from the free list,which are in rows/columns:
//Rows:
if(CurrentDimensionRows.length>0){
	for(i=0;i<CurrentDimensionRows.length;i++){
		if(CurrentDimensionRows[i]!==""){
			Checkbox_Free.setSelectedKeys([CurrentDimensionRows[i]]);
			dimdesc=Checkbox_Free.getSelectedTexts();
			Checkbox_Rows.addItem(CurrentDimensionRows[i],dimdesc[0]);
			Checkbox_Free.removeItem(CurrentDimensionRows[i]);
		}
	}
}

//Columns

if(CurrentDimensionColumn.length>0){
	for(i=0;i<CurrentDimensionColumn.length;i++){
		if(CurrentDimensionColumn[i]!==""){
			Checkbox_Free.setSelectedKeys([CurrentDimensionColumn[i]]);
			dimdesc=Checkbox_Free.getSelectedTexts();
			Checkbox_Columns.addItem(CurrentDimensionColumn[i],dimdesc[0]);
			Checkbox_Free.removeItem(CurrentDimensionColumn[i]);
		}
	}
}

//Set Measures filter//

//remove Measures:
Table_1.getDataSource().removeDimensionFilter("Account");

//Add Measures:
Table_1.getDataSource().setDimensionFilter("Account",SelectedIds);

//Save current selection into global variable:
Selected_Measures=SelectedIds;

//Button-1 onClick//

var selkeys = Checkbox_Rows.getSelectedKeys();
for (var i=0;i<selkeys.length;i++){
//		Remove Dimension
	Table_1.removeDimension(selkeys[i]);
}
Utils.Set_Dimension_Checkboxes();

//Button remove_3//

var selkeys = Checkbox_Columns.getSelectedKeys();
for (var i=0;i<selkeys.length;i++){
//		Remove Dimension
	Table_1.removeDimension(selkeys[i]);
}
Utils.Set_Dimension_Checkboxes();

//Button add_rows//

var selkeys = Checkbox_Free.getSelectedKeys();
for (var i=0;i<selkeys.length;i++){
//		Add dimension to Rows in table
	Table_1.addDimensionToRows(selkeys[i]);
}
Utils.Set_Dimension_Checkboxes();

//Buttons_add_columns//

var selkeys = Checkbox_Free.getSelectedKeys();
for (var i=0;i<selkeys.length;i++){
//		Add dimension to Rows in table
	Table_1.addDimensionToRows(selkeys[i]);
}
Utils.Set_Dimension_Checkboxes();

//Button Set_all//

Checkbox_Measures.setSelectedKeys(All_Measures);
Utils.Set_Measure_Filters(All_Measures);

//Button_Remove_1//

Checkbox_Measures.setSelectedKeys([""]);
Utils.Set_Measure_Filters([""]);

//Button set_select//

Utils.Set_Measure_Filters(Checkbox_Measures.getSelectedKeys());

//Canvas onInitialization//

//Get all the measures from the table data Source:
var measures =Table_1.getDataSource().getMeasures();

//Define Array:
var selectedkeys = ArrayUtils.create(Type.string);
 if(measures.length>0){
	 for (var i=0;i<measures.length;i++){
//Add measure to the checkbox:
		 Checkbox_Measures.addItem(measures[i].id,measures[i].description);
//Add measures to the selected keys:
		 selectedkeys.push(measures[i].id);
	 }
 }
Checkbox_Measures.setSelectedKeys(selectedkeys);
console.log(["selectedkeys",selectedkeys]);
All_Measures=selectedkeys;
//Define array:
var SelectedDims = ArrayUtils.create(Type.string);
var dims = Table_1.getDataSource().getDimensions();
if (dims.length>0){
	for(i=0;i<dims.length;i++){
		Checkbox_All_Dimensions.addItem(dims[i].description);
		SelectedDims.push(dims[i].id);
	}
}
console.log(["SelectedDims",SelectedDims]);
All_Dimensions=SelectedDims;
Utils.Set_Measure_Filters(selectedkeys);
Utils.Set_Dimension_Checkboxes();

