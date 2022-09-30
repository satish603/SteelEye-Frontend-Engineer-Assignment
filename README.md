##### Explain what the simple List component does.
- The List component allows you to display     a list of pages or links. You can build     the list of links by defining a parent     page (and displaying its child pages),     manually choosing pages, or defining search criteria.
- `List` get an array of objects wich are data to display and renders childrens `SingleListItem` components and pass them the data they need to display and also the click handler to update the state to know which list item is selected
##### What problems / warnings are there with code?
``` src\App.js
  Line 39:6:  React Hook useEffect has a missing dependency: 'setSelectedIndex'. Either include it or remove the dependency array  react-hooks/exhaustive-deps

Search for the keywords to learn more about each warning.
To ignore, add // eslint-disable-next-line to the line before.

WARNING in [eslint]
src\App.js
  Line 39:6:  React Hook useEffect has a missing dependency: 'setSelectedIndex'. Either include it or remove the dependency array  react-hooks/exhaustive-deps

webpack compiled with 1 warning 
```
And many more Warning while debugging the code It has been discussed in below qustion with solution to tagle them.

##### Please fix, optimize, and/or modify the component as much as you think is necessary.
- `Parameters` Passed in const [setSelectedIndex,selectedIndex ] = useState(null); is missed placed. Just swapping the Parameters solves an error
- Removing  index from the following
    - `WrappedSingleListItem.`
    - `WrappedSingleListItem.propTypes`
- There was error in return function where index was pass instead of key to correct That I have replaced `index` with `key`. 
- Also in return function the onClick should not pass now index as argument because it has already been removed it should be set to `onClick={onClickHandler}`
- In `WrappedListComponent.propTypes` shapeof should to corrected to `shape` also array of object should be defined as `arrayOf(object)`.

### Final CODE After Debugging And Output
```JAVASCRIPT
import React, { useState, useEffect, memo } from 'react';
import PropTypes from 'prop-types';

// Single List Item
const WrappedSingleListItem = ({
  //index,//remove index
  isSelected,
  onClickHandler,
  text,
}) => {
  return (
    <li
      style={{ backgroundColor: isSelected ? 'green' : 'red'}}
      onClick={onClickHandler}//onClickHandler(index) -> onClickHandler
    >
      {text}
    </li>
  );
};

WrappedSingleListItem.propTypes = {
  //index: PropTypes.number,//remove index
  isSelected: PropTypes.bool,
  onClickHandler: PropTypes.func.isRequired,
  text: PropTypes.string.isRequired,
};

const SingleListItem = memo(WrappedSingleListItem);

// List Component
const WrappedListComponent = ({
  items,
}) => {
  const [selectedIndex, setSelectedIndex] = useState(null);

  useEffect(() => {
    setSelectedIndex(null);
  }, [items]);

  const handleClick = index => {
    setSelectedIndex(index);
  };

  return (
    <ul style={{ textAlign: 'left' }}>
      {items.map((item, index) => (
        <SingleListItem
          onClickHandler={() => handleClick(index)}
          text={item.text}
          key={index}//rename index to key
          isSelected={selectedIndex === index}
        />
      ))}
    </ul>
  )
};

WrappedListComponent.propTypes = {
  items: PropTypes.array(PropTypes.shape({
    text: PropTypes.string.isRequired,
  })),
};
//shapeof -> shape
WrappedListComponent.defaultProps = {
  items: null,
};

const List = memo(WrappedListComponent);

export default List;
```
![image](https://user-images.githubusercontent.com/60779510/193199407-97d7c491-e58f-42bf-8406-f056b1ec912f.png)
![image](https://user-images.githubusercontent.com/60779510/193199444-21046ce7-a229-4092-af59-940436cdd247.png)
![image](https://user-images.githubusercontent.com/60779510/193199475-512b2935-29ba-411b-b3b1-fd1984ee32ab.png)
![image](https://user-images.githubusercontent.com/60779510/193199513-0ea321be-42cf-4b05-b809-2684085e0aa0.png)

# Getting Started with Create React App

This project was bootstrapped with [Create React App](https://github.com/facebook/create-react-app).

## Available Scripts

In the project directory, you can run:

### `npm start`

Runs the app in the development mode.\
Open [http://localhost:3000](http://localhost:3000) to view it in your browser.

The page will reload when you make changes.\
You may also see any lint errors in the console.

### `npm test`

Launches the test runner in the interactive watch mode.\
See the section about [running tests](https://facebook.github.io/create-react-app/docs/running-tests) for more information.

### `npm run build`

Builds the app for production to the `build` folder.\
It correctly bundles React in production mode and optimizes the build for the best performance.

The build is minified and the filenames include the hashes.\
Your app is ready to be deployed!

See the section about [deployment](https://facebook.github.io/create-react-app/docs/deployment) for more information.

### `npm run eject`

**Note: this is a one-way operation. Once you `eject`, you can't go back!**

If you aren't satisfied with the build tool and configuration choices, you can `eject` at any time. This command will remove the single build dependency from your project.

Instead, it will copy all the configuration files and the transitive dependencies (webpack, Babel, ESLint, etc) right into your project so you have full control over them. All of the commands except `eject` will still work, but they will point to the copied scripts so you can tweak them. At this point you're on your own.

You don't have to ever use `eject`. The curated feature set is suitable for small and middle deployments, and you shouldn't feel obligated to use this feature. However we understand that this tool wouldn't be useful if you couldn't customize it when you are ready for it.

## Learn More

You can learn more in the [Create React App documentation](https://facebook.github.io/create-react-app/docs/getting-started).

To learn React, check out the [React documentation](https://reactjs.org/).

### Code Splitting

This section has moved here: [https://facebook.github.io/create-react-app/docs/code-splitting](https://facebook.github.io/create-react-app/docs/code-splitting)

### Analyzing the Bundle Size

This section has moved here: [https://facebook.github.io/create-react-app/docs/analyzing-the-bundle-size](https://facebook.github.io/create-react-app/docs/analyzing-the-bundle-size)

### Making a Progressive Web App

This section has moved here: [https://facebook.github.io/create-react-app/docs/making-a-progressive-web-app](https://facebook.github.io/create-react-app/docs/making-a-progressive-web-app)

### Advanced Configuration

This section has moved here: [https://facebook.github.io/create-react-app/docs/advanced-configuration](https://facebook.github.io/create-react-app/docs/advanced-configuration)

### Deployment

This section has moved here: [https://facebook.github.io/create-react-app/docs/deployment](https://facebook.github.io/create-react-app/docs/deployment)

### `npm run build` fails to minify

This section has moved here: [https://facebook.github.io/create-react-app/docs/troubleshooting#npm-run-build-fails-to-minify](https://facebook.github.io/create-react-app/docs/troubleshooting#npm-run-build-fails-to-minify)
