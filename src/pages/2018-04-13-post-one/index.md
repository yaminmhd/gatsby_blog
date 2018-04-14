---
path: "/post-one"
date: "2018-04-13"
title: "Using react contextual"
author: "Yamin"
---

This is my very first blog post in Gatsby. Still trying to understand react contextual

### Using react contextual
Use Provider to distribute state and actions. We can connect components either by using Higher Order Components or render-props
***

1) Declare the store
```
import {Provider, subscribe} from 'react-contextual';
const store = {
  initialState: {counter: 0},
  actions: {
    increment: counter => ({count: count+1})
  }
}
```

2) Pass the store as props to the Provider and wrap around the application
```
class App extends React.Component {
  render() {
    return (
      <Provider {...store}>
        <Home/>
        <NavBar/>
        ....
      </Provider>
    );
  }
}
```

3) In the component you plan to use the state and actions which we refer to as context. So we have to use the method defined by us called `mapContextToProps`.

* We will have to define the subscribe function by passing in the ProviderContext which is provided by react-contextual. In my understanding the providerContext contains all the information from the store inclusive of the states and actions
`import {ProviderContext, subscribe} from react-contextual;`

* Then we can pass the ProviderContext to the function mapContextToProps. This function mapContextToProps will take in the ProviderContext as argument and the Home component is subscribe to this so they can use the context state as props
`export default subscribe(ProviderContext, mapContextToProps)(Home);`

* In `Home.js`
```
import {ProviderContext, subscribe} from react-contextual;

const mapContextToProps = context => {
  return mapNumberContextToProps(context);
}

export default subscribe(ProviderContext, mapContextToProps)(Home);
```


* In `numberContext.js`
```
export function mapNumberContextToProps(context){
  return{
    numberContext: {
      incrementNumber: context.actions.increment
    }
  }
}
```

* So now in the Home component, imagine there is a button with an onClick handler that triggers increases the state. We can implement using `{this.props.numberContext.incrementNumber}`