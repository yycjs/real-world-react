title: React in the Real World
output: index.html
theme: theme
controls: false

-- title

# React in the Real World

-- title

# Brought to you by

-- presenter

![David Luecke](http://gravatar.com/avatar/a14850281f19396480bdba4aab2d52ef?s=200)

## David Luecke

* [<i class="fa fa-github"></i> daffl](https://github.com/daffl)
* [<i class="fa fa-twitter"></i> @daffl](http://twitter.com/daffl)

-- presenter

![Eric Kryski](http://gravatar.com/avatar/23aba778a7daae99348aeb0728cf4aec?s=200)

## Eric Kryski

* [<i class="fa fa-github"></i> ekryski](https://github.com/ekryski)
* [<i class="fa fa-twitter"></i> @ekryski](http://twitter.com/ekryski)
* [<i class="fa fa-home"></i> erickryski.com](http://erickryski.com)

-- sponsors

# Our Sponsors

![Assembly](img/sponsors/assembly_logo.png)

![Village Brewery](img/sponsors/village_brewery_logo.png)

![Startup Calgary](img/sponsors/startup_calgary_logo.png)

![PetroFeed](img/sponsors/petrofeed_logo.png)

--

# Last Month

* Christian gave an overview of ArrangoDB
* Olivier talked about the Unix philosophy

-- image

<img src="img/react-logo.png" alt="React">

-- image

## Not Another Framework...

<img src="img/eddie-murphy.gif" alt="Eddie Murphy">

-- title

# But React is Not a Framework

--

# So what is it?

- A performant view engine
- A way of defining reusable components
- Provides hooks into a well defined component life cycle
- A clever way to manage "smart" (stateful) and "dumb" (not stateful) components
- Virtual DOM diff-ing
- JSX
- Some optional "add-ons" (ie. measuring performance, animations, etc.)

--

## The Component Life Cycle

- `getInitialState()`
- `getDefaultProps()`
- `componentWillMount()`
- `componentDidMount()`
- `componentWillReceiveProps()`
- `componentWillUpdate()`
- `componentDidUpdate()`
- `render()`
- `componentWillUnmount()`

--

## Virtual DOM Diffing?

Executing JavaScript is faster than manipulating the DOM. So we determine what should change in JS and re-render the parts that changed **every time, at the right time**.

For more info see [here](http://stackoverflow.com/questions/21109361/why-is-reacts-concept-of-virtual-dom-said-to-be-more-performant-than-dirty-mode), [here](http://fluentconf.com/fluent2014/public/schedule/detail/32395), [here](https://github.com/Matt-Esch/virtual-dom) and [here](https://youtu.be/XxVg_s8xAms?t=30m27s).

-- image

<img src="img/wat1.gif" alt="WAT">

--

## JSX

Define all of your component in **one file**. Template, logic, and you can even do styles.

-- image

<img src="img/wat2.gif" alt="WAT">

-- title

# Smart vs. Dumb Components

--

## A Dumb Component

The component just displays some state, doesn't manipulate it.

```javascript
var React = require('react');
var Button = React.createClass({
	propTypes: {
		title: React.PropTypes.string,
		onClick: React.PropTypes.func
	},

	render: function(){
		return (
			<button className="button" onClick={this.props.onClick}>{this.props.title}</button>
		);
	}
});

module.exports = Button;
```

--

## A Smart Component

When the component manipulates its own internal state.

```javascript
var React = require('react');
var cx = require('classnames');
var ToggleButton = React.createClass({
	propTypes: {
		title: React.PropTypes.string,
		onClick: React.PropTypes.func
	},
	getInitialState: function(){
		return { clicked: false };
	},
	_onClick: function(){
		this.setState({ clicked: !this.state.clicked });
		this.props.onClick.apply(null, arguments);
	},
	render: function(){
		var classes = cx({
			"toggle-button": true,
			"active": this.state.clicked
		});

		return (
			<button className={classes} onClick={this._onClick}>{this.props.title}</button>
		);
	}
});

module.exports = ToggleButton;
```

-- image

<img src="img/flux-logo.png" alt="Flux Logo">

--

# Flux is Not a Framework

- It's not MVC. Draws influence from MVC, CQRS, FRP.
- It's doesn't provide models/collections
- It doesn't make assumptions about the structure of your data
- It doesn't make assumptions about where your data comes from
- It doesn't give you ajax or websockets

--

# So what is it?

- An architectural pattern
- An async event mechanism for passing data with a synchronous feel.
- Uni-directional data flow
- Encourages immutability
- Trade off in favour of verbosity for readability and easy understanding.

-- image

## Data flows in One Direction

<img src="img/one-direction.gif" alt="One Direction" width="500">

-- image

## High Level Flux

<img src="img/flux-architecture-1.png" alt="Flux Architecture Overview" style="width: 800px; max-width: 800px;">

-- image

## A Little More Detail

<img src="img/flux-architecture-2.png" alt="Flux Architecture Detail" style="width: 800px; max-width: 800px;">

--

# Some Flux Abstractions

- [Alt](http://alt.js.org/)
- [Marty](http://martyjs.org/)
- [Reflux](https://github.com/spoike/refluxjs)
- [Fluxible](http://fluxible.io/)

-- image

# Let's put it all together

<img src="img/excited.gif" alt="Excited" width="400">

--

# Pros of React/Flux

- Very performant (~16ms renders if you do things right), [which reduces Jank](http://www.html5rocks.com/en/tutorials/speed/rendering/)
- Great error messages, especially when using `propTypes`
- "Learn once, write anywhere"
- Easy to understand
- Reduces weird side effects/race conditions
- Easy to test
- Predictable
- Scalable
- Flexible
- Properly self contained components

--

# Cons of React/Flux

- Could be smaller (121k minified, not gzipped)
- Flux is a bit verbose. Worth the trade-off IMHO
- Have to compile/transpile to test (when using JSX)
- Have to re-invent the wheel on a lot of existing libraries/components
- Working with non React modules/libraries can be clunky
- Some weird quirks

--

# ProTips™

- Keep components small
- Attributes are camelcase (ie. `autoFocus`, `allowFullScreen`)
- Put computed data calculations inside `render()` or in helper functions called from render.
- If your component has a lot of helper methods, consider moving them to a separate file, especially if they can be shared.
- Don't use Backbone models, you don't need them.
- Don't use mixins, as they will be deprecated. Use inheritance instead!

--

# Moar ProTips™

- If a component needs to let other components know about it's state, the state should probably live in a common store. Callbacks get messy.
- Autocomplete and autofocus on forms is still clunky.
- Sometimes you need a `setTimeout` inside of `componenDidMount`. Like when using non-react modules, or are using [velocity](http://julian.com/research/velocity/) for animations.
- Can render and animate SVGs but some animation attributes are still unsupported.
- [Be wary](https://medium.com/@goatslacker/react-0-13-x-and-autobinding-b4906189425d) of `this` context when using React's new class style

--

# Some Good Resources

- [The Why of React](https://www.youtube.com/watch?v=KVZ-P-ZI6W4)
- [Why Flux](https://youtu.be/KtmjkCuV-EU)
- [React Website](https://facebook.github.io/react/)
- [Flux Website](https://facebook.github.io/flux/)
- [React components](http://react.rocks/)
- [Moar React components](http://react-components.com/)
- [Comparison of Flux Implementations](https://github.com/voronianski/flux-comparison)

--

# Next Month

- Brief intro to [Ethereum](https://www.ethereum.org) by Derek.
- Wanna talk? Come on up!
- Some possible topics:
	- React Native
	- Feathers release/demo
	- Building native desktop apps
	- Using SVG's to make your app sexy
