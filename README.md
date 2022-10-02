# Frontend Engineer Assignment

# React Component Code Review

## QUESTIONS:
1. Explain what the simple List component does.
2. What problems / warnings are there with code?
3. Please fix, optimize, and/or modify the component as much as you think is necessary.

## SOLUTIONS:
>1 Explain what the simple List component does.
```
```
>2 What problems / warnings are there with code?
```
```
>3 Please fix, optimize, and/or modify the component as much as you think is necessary.
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
