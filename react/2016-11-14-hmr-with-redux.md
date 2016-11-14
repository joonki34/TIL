# HMR with redux

## Problem
HMR 세팅을 했는데 redux의 store 변화를 제대로 감지하지 못하고 redux hmr 관련 warning을 뱉었다. 코드는 다음과 같았다.

### app/index.js

```javascript
import React from 'react'
import ReactDOM from 'react-dom'
import { Provider } from 'react-redux'
import { AppContainer } from 'react-hot-loader'
import configureStore from './store/configureStore'
import App from './containers/App'

const store = configureStore()

const render = () => {
  ReactDOM.render(
    <AppContainer>
      <Provider store={store}>
        <App />
      </Provider>
    </AppContainer>,
    document.getElementById('root')
  )
}


render(App)


// Hot Module Replacement API
if (module.hot) {
  module.hot.accept(render)
}
```

### app/reducers/index.js

```javascript
import { createStore, applyMiddleware } from 'redux'
import createLogger from 'redux-logger'
import reducer from '../reducers'

const loggerMiddleware = createLogger()

export default function configureStore(initialState) {
  const store = createStore(
    reducer,
    initialState,
    applyMiddleware(
      loggerMiddleware
    )
  )


  if(module.hot) {
    // Enable Webpack hot module replacement for reducers
    module.hot.accept('../reducers', () => {
      const nextRootReducer = require('../reducers').default
      store.replaceReducer(nextRootReducer)
    })
  }
```

## Solution
안되는 이유는 `app/index.js`에서 `module.hot`일 경우에 컴포넌트를 새로 가져와서 render하지 않았기 때문이다. 해결 방법은 다음과 같다.

### app/index.js
```javascript
import React from 'react'
import ReactDOM from 'react-dom'
import { Provider } from 'react-redux'
import { AppContainer } from 'react-hot-loader'
import configureStore from './store/configureStore'
import App from './containers/App'

const store = configureStore()

const render = (Component) => {
  ReactDOM.render(
    <AppContainer>
      <Provider store={store}>
        <Component />
      </Provider>
    </AppContainer>,
    document.getElementById('root')
  )
}

render(App)

// Hot Module Replacement API
if (module.hot) {
  module.hot.accept('./containers/App', () => render(require('./containers/App').default))
}
```