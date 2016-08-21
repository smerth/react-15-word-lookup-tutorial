# React Word Lookup Tutorial

## Intro

This project is based on Hendrik Swanepoel's  [Thinking in React](http://tagtree.tv/thinking-in-react) video.  I decided to use Facebook's [Comment React Tutorial](https://github.com/reactjs/react-tutorial) as a starting point...

## Installation

Start with a project folder

```bash
cd desktop && mkdir project-name
```

Get Facebook's comment tutorial code from github

```bash
wget https://github.com/reactjs/react-tutorial/archive/master.zip
```

unzip it

```bash
unzip master.zip
```

rename folder

```bash
mv react-tutorial-master thinking_in_react
```

cd into project

```bash
cd thinking_in_react
```

install modules

```bash
npm install
```

open the project on the editor

```bash
sublime .
```

serve on localhost port 3000

```bash
node server.js
```



## Add Foundation 6 to project

The React Tutorial Master from Facebook contains a very simple Express server which serves the public folder.  In addition it serves an app.json file which you can use to model a live api for your app to consume.

There are no tasks to build a distribution or copy assests into the public folder so the quickest way to get going is to copy ```foundation.flex.min.css``` and ```foundation.min.js``` into the public css and js folder respectively.

@ ```index.html``` add the necessary assets.

```html
<head>
    <meta charset="utf-8">
    <title>React Tutorial</title>
    <!-- Not present in the tutorial. Just for basic styling. -->
    <link rel="stylesheet" href="css/base.css" />
    <link rel="stylesheet" href="css/foundation-flex.min.css" />
    <script src="https://cdnjs.cloudflare.com/ajax/libs/react/15.0.1/react.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/react/15.0.1/react-dom.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/babel-core/5.6.16/browser.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/2.2.2/jquery.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/remarkable/1.6.2/remarkable.min.js"></script>
    <script src="scripts/foundation.min.js"></script>
</head>
```



## Step 1: Draw a picture

Draw a diagram of what you want to build

## Step 2: Break the UI into a component hierarchy

Our app will contain the following component heirarchy

```
FilterableWordTable (which will contain...)

  SearchBar

  WordTable (which will contain...)

	WordRow (one row per word.)
```


So , we have four components.

## Step 3: Build a static version of the interface



@ ```index.html```

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>React Tutorial</title>
    <!-- Not present in the tutorial. Just for basic styling. -->
    <link rel="stylesheet" href="css/base.css" />
    <link rel="stylesheet" href="css/foundation-flex.min.css" />
    <script src="https://cdnjs.cloudflare.com/ajax/libs/react/15.0.1/react.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/react/15.0.1/react-dom.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/babel-core/5.6.16/browser.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/2.2.2/jquery.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/remarkable/1.6.2/remarkable.min.js"></script>
    <script src="scripts/foundation.min.js"></script>
  </head>
  <body>
    <div class="row">
        <div class="columns medium-6 medium-offset-3">
            <div id="container">
                <label>
                    <h4>Chinese Word Lookup</h4>
                    <input type="search" placeholder="Search for word">
                </label>
                <table>
                    <thead>
                        <tr>
                            <th width="">Word</th>
                            <th width="">Pinyin</th>
                            <th width="">Meaning</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr>
                            <td><a href="#">Word</a></td>
                            <td>Pinyin</td>
                            <td>Meaning</td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </div>
    </div>
  </body>
</html>
```



## Step 4: Build Components

To start lets replace the content of our page ```div id="container"``` with some test content which we will develop into our first component called WordRow.

### Add a Script tag for babel to grab and process our React code

```html
<script type="text/babel">

</script>
```



### Define a React component inside this Script tag

```javascript
var WordRow = React.createClass({
  render: function() {
    return (
      <tr className="word">
        Placeholder text for a word row.
      </tr>
    );
  }
});
ReactDOM.render(
  <WordRow />,
  document.getElementById('word-row')
);
```

Test in the browser, the test content should replace the #container content.

### Replace the placeholder content with actual markup for a row

```javascript
var WordRow = React.createClass({
  render: function() {
    return (
      <tr>
          <th width="">Word</th>
          <th width="">Pinyin</th>
          <th width="">Meaning</th>
      </tr>
    );
  }
});
ReactDOM.render(
  <WordRow />,
  document.getElementById('container')
);
```



You'll have an error for illegal dom nesting because we are rendering a table row but its not nested in a table as it should be.  That's ok because we are building components from the bottom up.

We expect to pull in variables from props.  So we can add appropriate props in curly braces.

```html
<tr>
    <td>
      <a href="#">{this.props.word.name}</a>
    </td>
    <td>{this.props.word.pinyin}</td>
    <td>{this.props.word.definition}</td>
</tr>
```



### Next code a component for the table



```javascript
var WordTable = React.createClass({
  render: function() {
    return (
      <table>
          <thead>
              <tr>
                  <th width="">Word</th>
                  <th width="">Pinyin</th>
                  <th width="">Meaning</th>
              </tr>
          </thead>
          <tbody>
            {rows}
          </tbody>
      </table>
    );
  }
});
```



We have placed a variable {rows} where the body of the table should be.  We already have the component for the table row which we are calling WordRow.

We want to set the value of the {rows} variable to render our WordRow component but we want that rendering to take in some data.

### Add a data object just above ReactDOM.render




    var words = [{
          name : "你好",
          pinyin: "Nǐ hǎo",
          definition: "hello"
      },{
          name : "学生",
          pinyin: "xuéshēng",
          definition: "student"
      },{
          name : "怎么",
          pinyin: "zěnme",
          definition: "how"
      },{
          name : "好久",
          pinyin: "hǎojiǔ",
          definition: "for a long time"
      },{
          name : "没有",
          pinyin: "méiyǒu",
          definition: "no"
      }
    ];



### Map the data to the component

We use a map to marry this data to the component and set that component, rendered with data, as our {rows} variable.

```javascript
var WordTable = React.createClass({
  render: function() {

    var rows = this.props.words.map(function(word){
      return <WordRow key={word.name} word={word} />;
    });

    ...
```

```this.props.words``` is the data object we defined above.  Since the data object ```words``` is an array each ```word``` (singular) is passed into the function.

The key uniquely identifies each child, which is important if components are being swapped around as in a search app or a filtering view.

```word={word}``` is where we set the name of the property to the data key. We are passing each word, from our array, into our render function. That is the ```={word}``` part of this expression. All we are doing is giving it a name on our props.

So, ```word={word}```

Yields these variables:

```html
<td>
  <a href="#">{this.props.word.name}</a>
</td>
<td>{this.props.word.pinyin}</td>
<td>{this.props.word.definition}</td>
```



But if it is convenient we can set the name on props to something else:

So, ```cheeseburger={word}```

Sets the name of the incoming "words" on props to be "cheeseburger" which are then available as the variables:

```html
<td>
  <a href="#">{this.props.cheeseburger.name}</a>
</td>
<td>{this.props.cheeseburger.pinyin}</td>
<td>{this.props.cheeseburger.definition}</td>
```

That's convenient.

### Render the table with data

Lastly the row is now set to render using data passed down from the table via a map.  So when the table renders we must tell it where the data is coming from.

```javascript
ReactDOM.render(
  <WordTable words={words} />,
  document.getElementById('container')
);
```



To be clear lets run though it, the other way around, from parent components down to children.

You have some data.

```javascript
var library = [ { // data array... }]
```

You have a parent component called "BookCase". When it renders you pass it the library array of data but you name the property more convenietly to "books", because… you feel like it...

```javascript
ReactDOM.render(
  <BookCase books={library} />,
  document.getElementById('container')
);
```

When you define a child component of BookCase called Shelves the data is available as a property on props called "books".  So you can pass each book to a child component called BookDetails.

At this point you can set the name of the property for each individual book to something else… like "text" (or, any other arbitrary name.)

```javascript
var Shelves = React.createClass({
  render: function() {

    var details = this.props.books.map(function(book){
      return <BookDetails key={book.name} text={book} />;
    });
```

So now when you render BookDetails you can call on the data using the dot syntax:

```html
<td>
  <a href="#">{this.props.text.name}</a>
</td>
<td>{this.props.text.isbn}</td>
<td>{this.props.text.author}</td>
```



### Create a new component for the search bar

```javascript
var SearchBar = React.createClass({
  render: function() {
        return (
        <label>
            <h4>Chinese Word Lookup</h4>
            <input type="search" placeholder="Search for word" />
        </label>
    );
  }
});
```



###  Create our top level component which holds the table and searchbar

```javascript
var FilterableWordTable = React.createClass({
  render: function() {
    return (
        <div className="spacer">
            <SearchBar   />
            <WordTable words={this.props.words} />
        </div>
    );
  }
});
```



Note we have moved the ```SearchBar``` and the ```WordTable``` into ```FilterableWordTable```.  They are now children components of ```FilterableWordTable```.

We need to pass the data object stored in properties as "words" into the ```WordTable```.

Next we need to change the ```React.renderDOM``` to render the ```FilterableWordTable``` instead of ```WordTable```.  The {words} data is passed in just as it was before.

```javascript
ReactDOM.render(
  <FilterableWordTable words={words} />,
  document.getElementById('container')
);
```

So, data comes into the app at the level of ```FilterableWordTable``` as ```words={words}```.

Then it is passed to the ```WordTable``` as ```words={this.props.words}```

It is passed to the var ```rows``` where it is mapped to JSX variables and placed on the props of ```WordTable```

```javascript
var rows = this.props.words.map(function(word){
  return <WordRow key={word.name} word={word} />;
});
```

Lastly the variables are accessed by dot notation in the WordRow

```javascript
var WordRow = React.createClass({
  render: function() {
        return (
        <tr>
            <td>
              <a href="#">{this.props.word.name}</a>
            </td>
            <td>{this.props.word.pinyin}</td>
            <td>{this.props.word.definition}</td>
        </tr>
    );
  }
});
```



Note that the rendering looks OK because the only foundation classes we are using are outisde of the JSX template

```html
<div class="row">
    <div class="columns medium-6 medium-offset-3">
        <div id="container">
        </div>
    </div>
</div>
```

If we attempt to use foundation classes inside the JSX template we need to change "class" to use "className" because "class" is a reserved word in javascript.





## Step 4: Deal with state

we need to identify which component mutates, or owns, this state.

Remember: React is all about one-way data flow down the component hierarchy. It may not be immediately clear which component should own what state. This is often the most challenging part for newcomers to understand, so follow these steps to figure it out:

For each piece of state in your application:

- Identify every component that renders something based on that state.

- Find a common owner component (a single component above all the components that need the state in the hierarchy).

- Either the common owner or another component higher up in the hierarchy should own the state.

If you can't find a component where it makes sense to own the state, create a new component simply for holding the state and add it somewhere in the hierarchy above the common owner component.

Let's run through this strategy for our application:

- ProductTable needs to filter the product list based on state and SearchBar needs to display the search text and checked state.

- The common owner component is FilterableProductTable.

- It conceptually makes sense for the filter text and checked value to live in FilterableProductTable

Cool, so we've decided that our state lives in FilterableProductTable.



In this example the state should live in FilterableWordTable since the SearchBar and WordTable require access to the state values to render their views.

## Step 5: Add inverse data flow

Set default values for intial state. In this case an empty string.

```javascript
var FilterableWordTable = React.createClass({
  getInitialState: function() {
    return {
      filterText : ""
    };
  },
```

connect the input field value to the state

```javascript
var SearchBar = React.createClass({
  render: function() {
        return (
        <label>
            <h4>Chinese Word Lookup</h4>
            <input value={this.props.filterText} type="search" placeholder="Search for word" />
        </label>
    );
  }
});
```

Pass this state through to the searchbar component when it renders via its props

```javascript
<div className="spacer">
    <SearchBar filterText={this.props.filterText} />
    <WordTable words={this.props.words} />
</div>
```

So, when the app starts we add filterText to the State object. We have set the value of the input field equal to filterText. And when we render the searchBar we are passing the value of filter into the searchBar component.



##  Step 5: Add the filter

How do we change the value of filterText?

We add an onChange event to the input field and assign it a callback function.

```javascript
<input value={this.props.filterText} onChange={this.handleChange} type="search" placeholder="Search for word" />
```

When something changes in the input field the callback function fires



```javascript
handleChange: function() {
  this.props.onFilterInput(
    this.refs.filterTextInput.getDOMNode().value;
    );
},
```





```html
<input ref="filterTextInput" value={this.props.filterText} onChange={this.handleChange} type="search" placeholder="Search for word" />
```



Our SearchBar getDOMNode() expects a callback function to be specified on its props.



```javascript
handleUserInput: function(filterText) {
    this.setState({
        filterText: filterText
    });
},
```



chain filter function onto the rows variable



```javascript
var rows = this.props.words.map(function(word){
  return <WordRow key={word.name} word={word} />;
});
```

with

```javascript
var props = this.props;
var rows = props.words
    .filter(function(word){
      return word.pinyin.toLowerCase().indexOf(props.filterText.toLowerCase()) > -1;
    })
    .map(function(word){
  return <WordRow key={word.name} word={word} />;
});
```



## Deployment

[![Deploy](https://www.herokucdn.com/deploy/button.png)](https://heroku.com/deploy)



