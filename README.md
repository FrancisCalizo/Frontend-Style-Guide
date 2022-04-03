# Frontend-Style-Guide

A mostly reasonable approach to React and JSX.

# Table of Contents (Under Construction)
1. [Introduction](#introduction)
2. [Code Formatting ](#overall)

--- 

## Introduction
This is an opinionated style guide for developing applications in ES6+ with React.

This style guide is mostly based on the standards that are currently prevalent in JavaScript, although some conventions may still be included or prohibited on a case-by-case basis.

### Overall Code Formatting & Style
---
Code formatting and linting should automatically be enforced by your IDE. By doing this, we are ensuring:

(Prettier) Code is formatted automatically, allowing us to skip this discussion on style during code reviews

(ESLint) We have the first layer of static testing that catches typos and type errors as you write code

(Prettier & ESLint) Your time and energy isn’t wasted

More Information on configuring Prettier & ESLint within your IDE/Code Editor can be found here:

https://allamericanhealthcare.atlassian.net/wiki/spaces/ENGR/pages/1420001281

#### Prettier
Prettier improves code readability and makes the coding style consistent for teams. Developers are more likely to adopt a standard rather than writing their own code style from scratch, so tools like Prettier will make your code look great without you ever having to dabble in the formatting.

The following is our current prettier configuration used for JS files (can be found in the .prettierrc.json file in the root folder):

```
{
    "printWidth": 80,
    "tabWidth": 2,
    "semi": true,
    "singleQuote": true,
    "trailingComma": "es5",
    "bracketSpacing": true
}
```

#### ESLint
ESLint is a static code analysis tool for identifying problematic patterns found in JavaScript code. Code linting is a type of static analysis that is frequently used to find problematic patterns or code that doesn't adhere to certain style guidelines. Linting tools like ESLint allow developers to discover problems with their JavaScript code without executing it. ESLint also allows developers to create their own linting rules.

Within our frontend directory, search for the `.eslintrc.json` file in the root folder. This file contains the current “rules” in which the linter will choose to enforce. These rules can be configured by the developer.

## Naming Conventions
---
File Names
The name of the filename should be explanatory. Use PascalCase for filenames.

E.g., `ShiftCard.js`.

#### Component Names
Use the filename as the component name. For example, ShiftCard.js should have a reference name of ShiftCard. However, for root components of a directory, use index.jsx as the filename and use the directory name as the component name:

```
// Bad
import Footer from './Footer/Footer';

// Bad
import Footer from './Footer/index';

// Good
import Footer from './Footer';
Function Names
For the most part, function names should start with verbs to increase readability.
```

Preferred/common verbs to begin function names with:

- fetch
- get
- handle
- set
- on

```
// Bad
function clientContacts() => 
  axios.get('/clientContacts')
  
// Good
function fetchClientContacts =>
  axios.get('/clientContacts')

// Bad
function rowClick(rowNumber) => {
  selectedRow(rowNumber)
  
  ...
}
  
// Good
function handleRowClick(rowNumber) => {
  setSelectedRow(rowNumber)
  
  ...
}
```

#### Handlers
Aim to name handle methods with handle[EventName]

Example:

```
// Bad
<Component onClick={click}

// Good
<Component onClick={handleClick} 
Variables & Booleans
Use explanatory names when defining variables. The name should clearly, without ambiguity indicate what the variable is.

// Bad
const data = ['foo', 'bar'];

// Good
const clientNames = ['foo', 'bar'];
Booleans: Do name Boolean properties with an affirmative phrase (isValidated instead of isNotValidated). Additionally, aim to prefix Boolean properties with is, can, or has, but only where it adds value:

// Bad
const darkMode = false;

// Good
const isDarkMode = false;

// Bad
let validCredentials = true;

// Good
let hasValidCredentals = true;
```

## React Code Standards & Style Guide
--- 
#### Folder & File Structure [Coming Soon…]
Coming soon…

#### Functional Components vs Class Components
Aim to use functional components over class components:

New code should use functional components with Hooks

Old code can keep using class components, unless ready to rewrite

The official React team stance (according to the docs), is:

When you’re ready, we’d encourage you to start trying Hooks in new components you write. [...] We don’t recommend rewriting your existing classes to Hooks unless you planned to rewrite them anyway (e.g. to fix bugs).

#### Ordering within Component file
Having a set and standardized ordering when creating a component file will increase readability.

Preferred ordering for React Functional Component:

1. NPM Library imports (or any other imports that are not #2 or #3)

2. Component Library imports

3. Relative imports

4. Defined constant variables

5. Functions outside component scope

6. PropTypes

7. React Component Definition

7a. Instantiated hooks

7b .Destructured props

7c. State definitions & Refs

7d. React Hook Calls

7e. Component methods

7f. Return function (JSX)

8. Styles

Example:

```
// #1 NPM Library imports (or any other imports that are not #2 or #3)
import React, { useState, useEffect } from 'react';
import PropTypes from 'prop-types';
import styled from 'styled-components'
import _ from 'lodash';

// #2 Component Library imports
import { makeStyles } from '@material-ui/core/styles';
import AppBar from '@material-ui/core/AppBar';
import Tabs from '@material-ui/core/Tabs';
import Tab from '@material-ui/core/Tab';

// #3 Relative imports
import TabPanel from './TabPanel';
import foo from '../../bar';

// #4 Defined Constants
const TODAY = new Date();
const ALLOWED_SHIFTS = 5;

// #5 Functions outside component scope
function a11yProps(index) {
  return {
    id: `scrollable-force-tab-${index}`,
    'aria-controls': `scrollable-force-tabpanel-${index}`,
  };
}

// #6 PropTypes
TabPanel.propTypes = {
  children: PropTypes.node,
  index: PropTypes.any.isRequired,
  value: PropTypes.any.isRequired,
};

// #6 PropTypes, again
ScrollTabs.propTypes = {
  /**
   * Name of each tab.
   */
  tabNames: PropTypes.arrayOf(PropTypes.string).isRequired,
  /**
   * Icon for each tab. Matches with the tabName with corresponding array index
   */
  tabIcons: PropTypes.arrayOf(PropTypes.node),
  /**
   * Panels for each tab. Matches with the tabName with corresponding array index
   */
  children: PropTypes.node.isRequired,
};

// #7 React Component Definition
export default function ScrollTabs(props) {
  // #7a Instantiated hooks
  const classes = useStyles();

  // #7b Destructured props
  const {
    tabNames,
    tabIcons,
    moreDropdownTabNames = null,
    ...rest
  } = props;

  // #7c State definitions & Refs
  const [tabValue, setTabValue] = useState(0);
  const [tabPanelValue, setTabPanelValue] = useState(0);
  const [anchorEl, setAnchorEl] = useState(null);
  const inputRef = useRef(null);

  // #7d React Hook Calls
  useEffect(() => {
    ...
    ...
  }, [])  
  
  // #7e Component methods
  const handleSubmit = (e) => {
     ...
     ...
     return e;
  }

  // #7f Return function (JSX)
  return (
    <div className={classes.root}>
      <AppBar position="static" color="default">
        <Tabs
          variant="scrollable"
          scrollButtons="on"
          indicatorColor="primary"
          textColor="primary"
          {...rest}
          value={tabValue}
          onChange={(_, newValue) => {
            // The tab panel value should not change if we
            // are clicking on the "More" dropdown Tab
            if (newValue < tabNames.length) {
              setTabPanelValue(newValue);
              setTabValue(newValue);
            }
          }}
        >
          {tabNames.map((tabName, index) => (
            <Tab
              key={tabName}
              label={tabName}
              icon={
                tabIcons ? (
                  <SvgIcon component={tabIcons[index]} viewBox="0 0 24 24" />
                ) : null
              }
              {...a11yProps(index)}
            />
          ))}
          
          {!_.isEmpty(moreDropdownTabNames) && (
            <Tab label="More" onClick={handleClickPopover} />
          )}
        </Tabs>            
      </AppBar>

      {children.map((child, index) => (
        <TabPanel key={index} value={tabPanelValue} index={index}>
          {child}
        </TabPanel>
      ))}
    </div>
  );
}

// #8 Styles
const useStyles = makeStyles((theme) => ({
  root: {
    flexGrow: 1,
    width: '100%',
    marginTop: 35,
    backgroundColor: theme.palette.background.paper,
  },
}));

// #8 Styles, again
const StyledContainer = styled.div`
  width: 100%;
  background: #fff;
`
```

#### Destructure your props
Aim to always destructure your props, especially when more than 2 props from an object are being used in the same place.

Since, all the props are being destructured right into the function definition, it’s easy to comprehend what all props are being used in the component by taking a high-level look over it

The code becomes much more readable since you don’t have to prepend props. every time you want to use a prop.

The third and most important benefit of this approach is, it is relatively easy to specify a default value for the prop.

```
// Bad
export default function ShiftCard(props) {
  ...
  ...
  return (
    <div>
      <h1>{props.title}<h1/>
      
      {props.children}
      
      <button onClick={props.handleSubmit}>
        Submit
      </button>
    </div>
  )
}
  
// Good
export default function ShiftCard(props) {
  const {
    title = 'Default Title', 
    children, 
    handleSubmit
  } = props;
  ...
  ...
  return (
    <div>
      <h1>{title}<h1/>
      
      {children}
      
      <button onClick={handleSubmit}>
        Submit
      </button>
    </div>
  )
}
```

#### PropTypes
Aim to include PropTypes when defining components

PropTypes are a bonus for ensuring that:

Components use the correct data type and pass the correct type of props

Receiving components receive the right type of props.

We have an extra layer of automated quality control.

More Information on PropTypes:

Github: https://github.com/facebook/prop-types

React Docs: https://reactjs.org/docs/typechecking-with-proptypes.html

Example

```
ScrollTabs.propTypes = {
  /**
   * Name of each tab.
   */
  tabNames: PropTypes.arrayOf(PropTypes.string).isRequired,
  /**
   * Icon for each tab. Matches with the tabName with corresponding array index
   */
  tabIcons: PropTypes.arrayOf(PropTypes.node),
  /**
   * Panels for each tab. Matches with the tabName with corresponding array index
   */
  children: PropTypes.node.isRequired,
};
```

#### Absolute vs. Relative Importing [Coming soon….]

