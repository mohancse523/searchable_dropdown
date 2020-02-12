# searchable_dropdown

Searchable dropdown item inside a dialog with single or multiple selection

<h5>Normal Usage</h5>
<br/><img src="https://raw.githubusercontent.com/icemanbsi/searchable_dropdown/master/doc/sample1.gif"/>


## Props
| props                   | types           | defaultValues                                                                                                     |
| :---------------------- | :-------------: | :---------------------------------------------------------------------------------------------------------------: |
| items                   | List<DropdownMenuItem<T>>        |                                                                                                  |
| onChanged (only not mutiple)              | ValueChanged<T> |                                                                                                                   |
| value (only not mutiple)                   | T               |                                                                                                                   |
| style                   | TextStyle       |                                                                                                                   |
| searchHint              | Widget          |                                                                                                                   |
| hint                    | Widget          |                                                                                                                   |
| disabledHint            | Widget          |                                                                                                                   |
| icon                    | Widget          |                                                                                                                   |
| underline               | Widget          |                                                                                                                   |
| iconEnabledColor        | Color           |                                                                                                                   |
| iconDisabledColor       | Color           |                                                                                                                   |
| iconSize                | Double          | 24                                                                                                                |
| isCaseSensitiveSearch (only when not using searchFn)  | bool            | false                                                                                                             |
| isExpanded              | bool            | false                                                                                                             |
| closeButtonText         | String          | "Close"                                                                                                           |
| displayClearButton      | bool            | false                                                                                                             |
| clearIcon               | Widget          | Icon(Icons.clear)                                                                                                 |
| onClear                 | Function        | null                                                                                                             |
| selectedValueWidgetFn   | Function        | null                                                                                                             |
| assertUniqueValue (only not mutiple)       | bool            | true                                                                                                             |
| keyboardType       | TextInputType            | TextInputType.text                                                                                                             |
| validator       | Function            | null                                                                                                             |
| label       | Function            | null                                                                                                             |
| searchFn       | Function            | null                                                                                                             |
| multipleSelection       | bool            | false                                                                                                             |
| selectedItems (only mutiple)       | List<int>            | null                                                                                                             |
| displayItemWhenMultiple (only mutiple)       | Function            | null                                                                                                             |
| doneText (only mutiple)       | String            | "Done"                                                                                                             |
| doneButtonFn (only mutiple)       | Function            | null                                                                                                             |
| onChangedMultiple (only mutiple)       | Function            | null                                                                                                             |


## Usage
```dart
import 'package:searchable_dropdown/searchable_dropdown.dart';

List<DropdownMenuItem> items = [];
String selectedValue;

Widget widget() {
  for(int i=0; i < 20; i++){
    items.add(new DropdownMenuItem(
      child: new Text(
        'test ' + i.toString(),
      ),
      value: 'test ' + i.toString(),
    ));
  }

  return new SearchableDropdown(
     items: items,
     value: selectedValue,
     hint: new Text(
       'Select One'
     ),
     searchHint: new Text(
       'Select One',
       style: new TextStyle(
           fontSize: 20
       ),
     ),
     onChanged: (value) {
       setState(() {
         selectedValue = value;
       });
     },
   );
}
```

### Values
Values of the `DropdownMenuItem` objects must be strings or implement the `toString()` method. Those strings are used for the search.

### Clear button
A clear button can be displayed to unselect the selected value.

### Display selected value differently
It can be that one wants to display the selected value differently than in the list. In this case, the parameter selectedValueWidgetFn can be used to customize the rendering widget. Here is an example with an outcome similar to the one in the main.dart example:
```
selectedValueWidgetFn: (value) => Padding(
  padding: EdgeInsets.fromLTRB(20, 20, 20, 20),
  child:Text(
    value??"Select please",
    style: value==null?TextStyle(color:Theme.of(context).hintColor):null,
  )
),
```

### Assert unique value
It can be that the selected value doesn't correspond to a single or any items. In this case, an assert will generate an error. If, you would like to avoid this assert, cou can call the `SearchableDropDown` constructor with the following parameter:
```
assertUniqueValue: false,
```

### Label and validator
Label and error can be returned by a function as a Widget:
```
      validator: (value){return(value==null?Row(
        children: <Widget>[
          Icon(Icons.error,color: Colors.red,size: 14,),
          SizedBox(width: 5,),
          Text("Mandatory",style: TextStyle(color: Colors.red,fontSize: 13),),
        ],
      ):null);},
      label: (value){return(Row(
        children: <Widget>[
          Icon(Icons.info,color: Colors.blueAccent,size: 14,),
          SizedBox(width: 5,),
          Text("Oil producer",style: TextStyle(color: Colors.blueAccent,fontSize: 13),),
        ],
      ));},
```
![image](https://user-images.githubusercontent.com/32125299/74223889-f6b4ed80-4cb7-11ea-972b-bd62fcef29d8.png)
Or as a String:
```
      validator: (value){return(value==null?"Mandatory":null);},
      label: (value){return("Oil producer");},
```
![image](https://user-images.githubusercontent.com/32125299/74223999-311e8a80-4cb8-11ea-91a1-73f853277703.png)

### Search keyboard
The keyboard type for searches can be defined as follows:
```
keyboardType: TextInputType.number,
```
![image](https://user-images.githubusercontent.com/32125299/74224388-0a148880-4cb9-11ea-9fc3-82491474e44d.png)

### Search function
One may wish to search through the list items differently than just by finding the keyword string matching in the toString() of the value. For example, one may wish to search with multiple keywords if they are separated by a space. The example below shows how to make use of the `searchFn` for this purpose:
```dart
      searchFn: (String keyword, List<DropdownMenuItem<MyData>> items) {
        List<int> ret = List<int>();
        if (keyword != null && items != null) {
          keyword.split(" ").forEach((k) {
            int i = 0;
            items.forEach((item) {
              if (keyword.isEmpty || (k.isNotEmpty &&
                  (item.value.toString().toLowerCase().contains(k.toLowerCase())))) {
                ret.add(i);
              }
              i++;
            });
          });
        }
        if(keyword.isEmpty){
          ret = Iterable<int>.generate(items.length).toList();
        }
        return (ret);
      },
```
Here is an example of result:

![image](https://user-images.githubusercontent.com/32125299/74232473-29b3ad00-4cc9-11ea-94bd-fd4b64739e20.png)

### Multiple selection
Multiple selection is available through the `multipleSelection` parameter:
```dart
multipleSelection: true,
```
![image](https://user-images.githubusercontent.com/32125299/74260975-b75bc080-4cfa-11ea-92c9-89e0090493c7.png)
This disables `onChanged` callback function. See below.

There is a way to customize the display of the selected and unselected items through the `displayItemWhenMultiple` parameter function:
```dart
      displayItemWhenMultiple: (item,selected) {
        return (Row(children: [
          selected ? Icon(Icons.check, color: Colors.green,) : Icon(
            Icons.check_box_outline_blank, color: Colors.grey,),
          SizedBox(width: 7),
          item
        ]));
      },
```
![image](https://user-images.githubusercontent.com/32125299/74260857-8a0f1280-4cfa-11ea-936c-167701c87021.png)

The done button is displayed only when in multiple selection mode. It can be customized either by setting the String parameter `doneText` or by setting the function parameter `doneButtonFn`:
```dart
doneText: "OK",
```
![image](https://user-images.githubusercontent.com/32125299/74261413-816b0c00-4cfb-11ea-99c7-5df6ac463aa0.png)
Or:
```dart
      doneButtonFn: (newContext){return FlatButton(
          onPressed: () {
            Navigator.pop(newContext);
          }, child: Text("I'm through"));},
```
![image](https://user-images.githubusercontent.com/32125299/74261685-e32b7600-4cfb-11ea-8290-6fb9a15176c2.png)

The multiple items set by default can be set through the parameter `selectedItems` as follows:
```dart
  List<int> selectedItems = [1,3];
...
      selectedItems: selectedItems,
```
Where the int values are the indexes of the selected items in the list of items.
![image](https://user-images.githubusercontent.com/32125299/74298875-47c1f180-4d4b-11ea-8451-8a584d9a7c29.png)

Once the selection is done, the `selectedItems` list above is updated or the values can be retrieved through the `onChangedMultiple` function parameter:
```dart
      onChangedMultiple: (List<int> selectedIndexes){
        selectedIndexes.forEach((item){
          print("selected index: $item");
        });
      },
```
The indexes of the selected items are sent to this callback function. Note that in the multiple selection case, the `onChanged` callback function is not called as the parameter type is not a list.

The `validator` function can also be used in the multiple selection case:
```dart
validator: (List<int>selectedIndexes){return(selectedIndexes==null||selectedIndexes.length==0?"Mandatory":null);},
```

The `selectedValueWidgetFn` function can be used in the mutiple selection case the same way it is used in the single selection case:
```dart
      selectedValueWidgetFn: (value) => Padding(
          padding: EdgeInsets.fromLTRB(20, 20, 20, 20),
          child:Text(
            value.toString(),
            style: TextStyle(color:Colors.green),
          )
      ),
```

The clear button works the same way in multiple and single cases.
