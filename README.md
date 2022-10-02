# Frontend Engineer Assignment

# React Component Code Review

## QUESTIONS:
1. Explain what the simple List component does.
2. What problems / warnings are there with code?
3. Please fix, optimize, and/or modify the component as much as you think is necessary.

## SOLUTIONS:
### 1. Explain what the simple List component does.
```
```
### 2. What problems / warnings are there with code?
#### A) In the useState() hook, the first parameter should be the variable representing current state and second parameter should be the set function that is used to update the state of the variable:
> Incorrect:
```
const [setSelectedIndex, selectedIndex] = useState();
```
> Correct:
```
const [selectedIndex, setSelectedIndex] = useState();
```

#### B) Error in defining WrappedListComponent.propTypes - it would be arrayOf instead of shapeOf.
- In propsTypes of WrappedListComponent, we are using depricated way to call propTypes validator's directly which is incorrect after react version 15.
- So, instead of array  and shapeOf we have to use arrayOf and shape respectively:
> Incorrect:
```
WrappedListComponent.propTypes = {
  items: PropTypes.array(PropTypes.shapeOf({
    text: PropTypes.string.isRequired,
  })),
};
```
> Correct:
```
WrappedListComponent.propTypes = {
  items: PropTypes.arrayOf(PropTypes.shape({
      text: PropTypes.string.isRequired,
    })
  ),
};
```
#### C) In WrappedListComponent.defaultProps, null items cannot be mapped.
- To correct, this error  we initialise the array items with some default values.
> Incorrect:
```
WrappedListComponent.defaultProps = {
  items: null,
};
```
> Correct:
```
WrappedListComponent.defaultProps = {
  items: [
    { text: "STEEL EYE", index: 1 },
    { text: "FRONTEND ENGINEER", index: 2 },
    { text: "ASSIGNMENT", index: 3 },
  ], 
};
```
#### D) Unique key prop is missing for List items
- The unique key prop is not provided to map. So it will show a Warning: Each child in a list should have a unique "key" prop.
> Incoorect:
```
 <ul style={{ textAlign: 'left' }}>
      {items.map((item, index) => (
        <SingleListItem
          onClickHandler={() => handleClick(index)}
          text={item.text}
          index={index}
          isSelected={selectedIndex}
        />
      ))}
  </ul>
```
> Correct:
```
    <ul style={{ textAlign: "left" }}>
      {items.map((item, index) => (
        <SingleListItem
          onClickHandler={() => handleClick(index)}
          text={item.text}
          index={index}
          key={index} 
          isSelected={selectedIndex}
        />
      ))}
    </ul>
```
#### E) onClick events should have a function reference instead of function call
- When a parameter is to be passed in an event handler, it needs to be wrapped with an arrow function.
> Incorrect:
```
   <li style={{ backgroundColor: isSelected ? "green" : "red" }}
        onClick={onClickHandler(index)}>
       {text}
   </li>
```
> Correct: 
```
    <li style={{ backgroundColor: isSelected ? "green" : "red" }}
        onClick={() => onClickHandler(index)}>
      {text}
    </li>
```

### F) Passing incorrect propType value in isSelected.
- Passing a number selectedIndex to isSelected which should be a bool
- PropType of isSelected is boolean but here passed proptype is number. so we have to provide bool values
- Also There should be a function reference instead of call
>Incorrect:
```
    <li
      style={{ backgroundColor: isSelected ? 'green' : 'red'}}
      onClick={onClickHandler(index)}
    >
```
> Correct:
```
    <li
      style={{ backgroundColor: isSelected ? "green" : "red" }}
      onClick={() => onClickHandler(Boolean(index))} 
    >
```

### 3. Please fix, optimize, and/or modify the component as much as you think is necessary.
```
import React, { useState, useEffect, memo } from "react";
import PropTypes from "prop-types";

// Single List Item
const WrappedSingleListItem = ({ index, isSelected, onClickHandler, text }) => {
  return (
    <li
      style={{ backgroundColor: isSelected ? "green" : "red" }}
      onClick={() => onClickHandler(index)}
      //onClick={onClickHandler(index)} //There should be a function reference instead of call
      //onClick={() => onClickHandler(Boolean(index))} //correction
    >
      {text}
    </li>
  );
};

WrappedSingleListItem.propTypes = {
  index: PropTypes.number,
  isSelected: PropTypes.bool,
  onClickHandler: PropTypes.func.isRequired,
  text: PropTypes.string.isRequired,
};

const SingleListItem = memo(WrappedSingleListItem);

// List Component
const WrappedListComponent = ({ items }) => {
  //const [selectedIndex, setSelectedIndex] = useState(''); //correction
  const [selectedIndex, setSelectedIndex] = useState();  //correction
  //const [selectedIndex, setSelectedIndex] = useState(false);  //correction

  useEffect(() => {
    setSelectedIndex(null); //correction
    //setSelectedIndex(null);
  }, [items]);

  const handleClick = (index) => {
    setSelectedIndex(index);
    //setSelectedIndex(true);
    //setSelectedIndex(Boolean(index)); //correction
    //console.log("This is index: " + index); //correction
  };

  return (
    <ul style={{ textAlign: "left" }}>
      {items.map((item, index) => (
        <SingleListItem
          onClickHandler={() => handleClick(index)}
          text={item.text}
          index={index}
          key={index} //correction
          //isSelected={selectedIndex}
          isSelected={selectedIndex === index}
          //isSelected={Boolean(selectedIndex)}
          //isSelected={selectedIndex ? true : false}
        />
      ))}
    </ul>
  );
};

WrappedListComponent.propTypes = {
  items: PropTypes.arrayOf(
    //correction: array to arrayOf
    PropTypes.shape({
      //correction: shape to shapeOf
      text: PropTypes.string.isRequired,
    })
  ),
};

WrappedListComponent.defaultProps = {
  items: [
    { text: "STEEL EYE", index: 1 },
    { text: "FRONTEND ENGINEER", index: 2 },
    { text: "ASSIGNMENT", index: 3 },
  ], //correction
};


/*
WrappedListComponent.defaultProps = {
  items: [{ text: "First Item" }, { text: "Second Item" }],
};
*/

/*
WrappedListComponent.defaultProps = {
items: [
{ index: 1, text: "Ujjawal" },
{ index: 2, text: "Shivam" }
]
};
*/

const List = memo(WrappedListComponent);

export default List;



```
