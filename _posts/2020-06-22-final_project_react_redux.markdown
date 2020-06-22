---
layout: post
title:      "Final Project: React/Redux"
date:       2020-06-22 04:53:03 +0000
permalink:  final_project_react_redux
---


##### For my final project I decided to build a stock tracker, where a user can add a list of stocks they purchased along with the amount of each stock, purchase price and potential sellling price. I got into stocks when the pandemic hit and stocks started plummeting and quickly realized that an app like this would be very useful, at least, to me.

##### First I set up my Rails API backend to be able to persist my data, which I had up and runing in a few minutes once I figured out what my data would look like (stock has a name, amount, purchase price and selling price)

##### Next came React which is a JavaScript library which I used to build a smoother faster (Reactive) user interface. as opposed to vanilla JS which dealt a lot with manipulating DOM elements based on user activity, React gives us the ability to create, use and re-use components which are rendered depending on the user activity. In React we can inject HTML into JS files instead of making separate files for each. To get started on I used the "create-react-app" command which is a tool that gives a head start to developers by setting up a barebone application, you can go from having nothing to opening a webpage that has a nice React logo with just that one command. 

##### Now for the cherry on top, enter Redux. 
##### Redux store is essentially a container for your app's data and you can connect that data to your components using the react-redux connect feature and that data will be passed in as props. In order to modify the data we can dispatch functions which will manipulate and return a copy of our data as we want our original data to be immutable. See below

```

import { connect } from 'react-redux

const mapDispatchToProps = (dispatch) => {
  return {
      fetchStocks: () => dispatch(fetchStocks())
  }
}

const mapStateToProps = state => {
  return {
    stocks: state.stocks,
    message: state.message
  }
}

export default connect(mapStateToProps, mapDispatchToProps)(AllStocks);
```

##### Here I have two functions, mapStateToProps and mapDispatchToProps but really I could've called them bob and carl, the name does nothing for functionality but it is by convention they are called that, as you can see the name tells you what those function are doing which should be only of the goals of every line of code.
