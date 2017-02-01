# Intro

Based on 
* https://github.com/ReactTraining/react-router/blob/master/docs/guides/ComponentLifecycle.md
* https://developmentarc.gitbooks.io/react-indepth/content/life_cycle/update/component_will_receive_props.html


## Hit /

we have these set up.
App
* Home
* Invoice
* Account

hit /
* App, component did mount (2)
* Home, component did mount (1, from child to parent)
* Invoice, nothing
* Account, nothing


## Nav from / to /invoices/123

* App	(1) componentWillReceiveProps, (4) componentDidUpdate
* Home	(2) componentWillUnmount
* Invoice	(3) componentDidMount
* Account	N/A


### What is componentWillReceiveProps for dummy?

See this: https://developmentarc.gitbooks.io/react-indepth/content/life_cycle/update/component_will_receive_props.html

Think about an input box (this.setState(this.state.name)) and a component with (props: this.state.name). Everytime, we type in box, the component will receive new props.

Why do we need componentWillReceiveProps?

componentWillReceiveProps is called before render. It allows to do some smart things like comparing old and new props.


So why App has (1) componentWillReceiveProps?

Because router pass props to App. Those props are child components, params from url, location


### What is componentWillUnmount for dummy?

There is no componentDidUnmount, because react wants you to remove all handlers, before they unmount the from dom. componentWillUnmount becomes the last thing to do.

### Why Home has componentWillUnmount?
Because it move from / to /invoices/123. Home component will not be there any more.

### Other

Invoice	(3) componentDidMount, because we land on /invoices/123

After everything is done. App (4), finally can say component did update



### Move from /invoices/123 to /invoices/789

* App	(1) componentWillReceiveProps, (4) componentDidUpdate
* Home	N/A
* Invoice	(2) componentWillReceiveProps, (3) componentDidUpdate
* Account	N/A

OK, if it is single page. The flow is top -> down, then down -> top
Route changes, then App (1) parent will receive new props
After that, it is Invoice's props (2) will change.
Once Invoice (3) is done, go up, App (4)


### /invoices/789 to /accounts/123

* App	(1) componentWillReceiveProps, (4) componentDidUpdate
* Home	N/A
* Invoice	(2) componentWillUnmount
* Account	(3) componentWillMount, (3.1) componentDidMount


### what is componentWillUpdate?

Every time re-render, we call component will update.
