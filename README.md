# react_notes


Notes : 
javascript Map() function is using to display array of numbers.

const arr = [1,2,3,4];
const dbl = arr.map((arr)=>
  <li>{arr * 2}</li>
)

and render into ul.
ReactDom.render(
  <ul>{dbl}</ul>, document.getElementById("tag");
);

KYES: 
are using to identify the array items in which to changing , adding or deleting the items, it is assigned to element to identify in the array.

function NumberList(props){
  const values = props.numbers;
  const listItems = values.map((numbe,i) => 
      <li key={i}>
         { numbe }
      </li>
   );
return (
<ul>{listItems}</ul>                         
);                          
}
                       

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(<NumberList  numbers={numbers} />, 
                document.getElementById('root')
);

React _ Redux https://www.valentinog.com/blog/react-redux-tutorial-beginners/

Javascript, ES6, and React.
A stateful React component is a Javascript ES6 class.
Every stateful React component carries its own state.
In a React component the state holds up data and the component might render such data to the user.
The state could also change in response to actions and events: in React you can update the local component’s state with setState. Even the simplest JavaScript application has a state.

Redux:
Redux is just a library among the others, yet it is one of the most important. It helps giving each React component the exact piece of state it needs.
Redux contains patterns. We should learn the patterns for managing UI state because they will be invaluable in our career.
Redux holds up the state within a single location (Store).
Also with Redux the logic for fetching and managing the state lives outside React.

Consider using Redux when:
•	multiple React components needs to access the same state but do not have any parent/child relationship
•	you start to feel awkward passing down the state to multiple components with props

The state of the whole application lives inside the store.
We should create a store for wrapping up the state.

Redux store
The store in Redux is like the human brain: it’s kind of magic.
The Redux store is fundamental: the state of the whole application lives inside the store.
With Redux we should create a store for wrapping up the state.
1.	npm i redux --save-dev
2.	mkdir -p src/js/store

Create a new file named index.jsin src/js/storeand finally initialize the store:
1.	// src/js/store/index.js
2.	
3.	import { createStore } from "redux";
4.	import rootReducer from "../reducers/index";
5.	
6.	const store = createStore(rootReducer);
7.	
8.	export default store;

createStore is the function for creating the Redux store.
createStore takes a reducer as the first argument, rootReducer in our case.

What does a reducer do
In Redux reducers produce the state. 
In Redux the state must return entirely from reducers.
A reducer is just a Javascript function. A reducer takes two parameters: the current stateand an action (more about actions soon).
The third principle of Redux says that the state is immutable and cannot change in place.
In plain React the local state changes in place with setState. In Redux you cannot do that.

In our example we’ll be creating a simple reducer taking the initial state as the first parameter. As a second parameter we’ll provide action. As of now the reducer will do nothing than returning the initial state.

1.	mkdir -p src/js/reducers
2.	// src/js/reducers/index.js
3.	
4.	const initialState = {
5.	articles: []
6.	};
7.	
8.	const rootReducer = (state = initialState, action) => state;
9.	
10.	export default rootReducer;

It returns the initial state without doing anything else.

Redux actions
How does a reducer know when to produce the next state?
The second principle of Redux says the only way to change the state is by sending a signal to the store. This signal is an action. “Dispatching an action” is the process of sending out a signal.



Redux actions are nothing more than Javascript objects. This is what an action looks like:

1.	{
2.	type: 'ADD_ARTICLE',
3.	payload: { name: 'React Redux Tutorial', id: 1 }
4.	}

Every action needs a type property for describing how the state should change.
In the above example the payload is a new article. A reducer will add the article to the current state later.

It is a best pratice to wrap every action within a function. Such function is an action creator.
Redux action.
1.	// src/js/actions/index.js
2.	
3.	import { ADD_ARTICLE } from "../constants/action-types";
4.	
5.	export const addArticle = article => ({ type: ADD_ARTICLE, payload: article });

•	The Redux store is like a brain
•	The state of the application lives as a single, immutable object within the store
•	As soon as the store receives an action it triggers a reducer
•	The reducer returns the next state.
•	The reducer calculates the next state depending on the action type. Moreover, it should return at least the initial state when no action type matches. 
•	When the action type matches a case clause the reducer calculates the next state and returns a new object.

•	import { ADD_ARTICLE } from "../constants/action-types";
•	
•	const initialState = {
•	articles: []
•	};
•	
•	const rootReducer = (state = initialState, action) => {
•	switch (action.type) {
•	case ADD_ARTICLE:
•	state.articles.push(action.payload);
•	return state;
•	default:
•	return state;
•	}
•	};
•	
•	export default rootReducer;
Use spread to avoid to change the initial state.
1.	import { ADD_ARTICLE } from "../constants/action-types";
2.	
3.	const initialState = {
4.	articles: []
5.	};
6.	
7.	const rootReducer = (state = initialState, action) => {
8.	switch (action.type) {
9.	case ADD_ARTICLE:
10.	return { ...state, articles: [...state.articles, action.payload] };
11.	default:
12.	return state;
13.	}
14.	};
15.	
16.	export default rootReducer;

The object spread operator is still in stage 3. Install Object rest spread transform to avoid a SyntaxError Unexpected token when using the object spread operator in Babel:
1.	npm i --save-dev babel-plugin-transform-object-rest-spread

.babelrcand update the configuration:
1.	{
2.	"presets": ["env", "react"],
3.	"plugins": ["transform-object-rest-spread"]
4.	}

The most important methods in Redux:
•	getState for accessing the current state of the application
•	dispatch for dispatching an action
•	subscribe for listening on state changes

•	import store from "../js/store/index";
•	import { addArticle } from "../js/actions/index";
•	
•	window.store = store;
•	window.addArticle = addArticle;

And then in console f12
1.	store.getState()
2.	store.subscribe(() => console.log('Look ma, Redux!!'))
3.	store.dispatch( addArticle({ name: 'React Redux Tutorial for Beginners', id: 1 }) )

React and Redux together.
For React there is react-redux.
1.	npm i react-redux --save-dev

The application is made of the following components:
•	an App component
•	a List component for displaying articles
•	a Form component for adding new articles
react-redux is a Redux binding for React. It’s a small library for connecting Redux and React in an efficient way.
The most important method you’ll work with is connect
U_se connect with two or three arguments depending on the use case. The fundamental things to know are:
•	the mapStateToProps function
•	the mapDispatchToProps function
mapStateToProps does exactly what its name suggests: it connects a part of the Redux state to the props of a React component.
mapDispatchToProps does something similar, but for actions. mapDispatchToProps connects Redux actions to React props. 

Connecting Redux with React we’re going to use Provider.
Provider is an high order component coming from react-redux. Provider wraps up your React application and makes it aware of the entire Redux’s store.
React must talk to the store for accessing the state and dispatching actions.
Open up src/js/index.js, wipe out everything and update the file with the following code:
1.	import React from "react";
2.	import { render } from "react-dom";
3.	import { Provider } from "react-redux";
4.	import store from "./store/index";
5.	import App from "./components/App";
6.	
7.	render(
8.	<Provider store={store}>
9.	<App />
10.	</Provider>,
11.	document.getElementById("app")
12.	);


1.	// src/js/components/App.js
2.	import React from "react";
3.	import List from "./List";
4.	
5.	const App = () => (
6.	<div className="row mt-5">
7.	<div className="col-md-4 offset-md-1">
8.	<h2>Articles</h2>
9.	<List />
10.	</div>
11.	</div>
12.	);
13.	
14.	export default App;

1.	// src/js/components/List.js
2.	
3.	import React from "react";
4.	import { connect } from "react-redux";
5.	
6.	const mapStateToProps = state => {
7.	return { articles: state.articles };
8.	};
9.	
10.	const ConnectedList = ({ articles }) => (
11.	<ul className="list-group list-group-flush">
12.	{articles.map(el => (
13.	<li className="list-group-item" key={el.id}>
14.	{el.title}
15.	</li>
16.	))}
17.	</ul>
18.	);
19.	
20.	const List = connect(mapStateToProps)(ConnectedList);
21.	
22.	export default List;
 
Finally the component gets exported as List. List is the result of connecting the stateless component ConnectedList with the Redux store.
A stateless component does not have its own local state. Data gets passed to it as props

Form component and Redux actions
It’s a form for adding new items to our application.
Plus it is a stateful component.
A stateful component in React is a component carrying its own local state
Even when using Redux it is totally fine to have stateful components.
Not every piece of the application’s state should go inside Redux.

The component contains some logic for updating the local state upon a form submission.
Plus it receives a Redux action as prop. This way it can update the global state by dispatching the addArticle action.

1.	// src/js/components/Form.js
2.	import React, { Component } from "react";
3.	import { connect } from "react-redux";
4.	import uuidv1 from "uuid";
5.	import { addArticle } from "../actions/index";
6.	
7.	const mapDispatchToProps = dispatch => {
8.	return {
9.	addArticle: article => dispatch(addArticle(article))
10.	};
11.	};
12.	
13.	class ConnectedForm extends Component {
14.	constructor() {
15.	super();
16.	
17.	this.state = {
18.	title: ""
19.	};
20.	
21.	this.handleChange = this.handleChange.bind(this);
22.	this.handleSubmit = this.handleSubmit.bind(this);
23.	}
24.	
25.	handleChange(event) {
26.	this.setState({ [event.target.id]: event.target.value });
27.	}
28.	
29.	handleSubmit(event) {
30.	event.preventDefault();
31.	const { title } = this.state;
32.	const id = uuidv1();
33.	this.props.addArticle({ title, id });
34.	this.setState({ title: "" });
35.	}
36.	
37.	render() {
38.	const { title } = this.state;
39.	return (
40.	<form onSubmit={this.handleSubmit}>
41.	<div className="form-group">
42.	<label htmlFor="title">Title</label>
43.	<input
44.	type="text"
45.	className="form-control"
46.	id="title"
47.	value={title}
48.	onChange={this.handleChange}
49.	/>
50.	</div>
51.	<button type="submit" className="btn btn-success btn-lg">
52.	SAVE
53.	</button>
54.	</form>
55.	);
56.	}
57.	}
58.	
59.	const Form = connect(null, mapDispatchToProps)(ConnectedForm);
60.	
61.	export default Form;

Update App to include the Form component:
1.	import React from "react";
2.	import List from "./List";
3.	import Form from "./Form";
4.	
5.	const App = () => (
6.	<div className="row mt-5">
7.	<div className="col-md-4 offset-md-1">
8.	<h2>Articles</h2>
9.	<List />
10.	</div>
11.	<div className="col-md-4 offset-md-1">
12.	<h2>Add a new article</h2>
13.	<Form />
14.	</div>
15.	</div>
16.	);
17.	
18.	export default App;
Install uuid with:
1.	npm i uuid --save-dev
npm start

git link https://github.com/valentinogagliardi/minimal-react-redux-webpack/tree/master/src/js









