# @ajaybhatia/react-native-meteor

Meteor-like methods for React Native.

## What is it for ?

The purpose of this library is :

- To set up and maintain a ddp connection with a ddp server, freeing the developer from having to do it on their own.
- Be fully compatible with react-native and help react-native developers.
- **To match with [Meteor documentation](http://docs.meteor.com/) used with React.**

## Install

```
yarn add @ajaybhatia/react-native-meteor
```

or

```
npm i --save @ajaybhatia/react-native-meteor
```

[!! See detailed installation guide](https://github.com/ajaybhatia/react-native-meteor/blob/master/docs/Install.md)

## Compatibility notes

Since RN 0.26.0 you have to use ws or wss protocol to connect to your meteor server. http is not working on Android.

It is recommended to always use the latest version of react-native-meteor compatible with your RN version:

- For RN > 0.49, use `react-native-meteor@latest`
- For RN > 0.45, use `react-native-meteor@1.1.x`
- For RN = 0.45, use `react-native-meteor@1.0.6`
- For RN < 0.45, you can use version `react-native-meteor@1.0.3` in case or problems.

`Meteor.Collection().find()` (for backwards compatibility) returns documents. If you are used to the usual Meteor find usage (`find().fetch()`), and want `Meteor.Collection().find()` to return a cursor instead, please see the [cursoredFind option](https://github.com/ajaybhatia/react-native-meteor/blob/master/docs/api.md#meteorcollectioncollectionname-options)

### Warning < RN 0.57.8 Android bug

There was a [bug in the react native websocket android implementation](https://github.com/react-native-community/react-native-releases/blob/master/CHANGELOG.md#android-specific) that meant the close event wasn't being received from the server. Therefore RN versions prior to React-native 0.57.8 will not detect users being logged out from the server side. There could also be other bugs resulting from this.

## Example usage

```javascript
import React, { Component } from 'react';
import { View, Text } from 'react-native';
import Meteor, { withTracker, MeteorListView } from '@ajaybhatia/react-native-meteor';

Meteor.connect('ws://192.168.X.X:3000/websocket'); //do this only once

class App extends Component {
  renderRow(todo) {
    return <Text>{todo.title}</Text>;
  }
  render() {
    const { settings, todosReady } = this.props;

    return (
      <View>
        <Text>{settings.title}</Text>
        {!todosReady && <Text>Not ready</Text>}

        <MeteorListView
          collection="todos"
          selector={{ done: true }}
          options={{ sort: { createdAt: -1 } }}
          renderRow={this.renderRow}
        />
      </View>
    );
  }
}

export default withTracker(params => {
  const handle = Meteor.subscribe('todos');
  Meteor.subscribe('settings');

  return {
    todosReady: handle.ready(),
    settings: Meteor.collection('settings').findOne(),
  };
})(App);
```

## Documentation

- Learn how to getting started from [connecting your components](docs/connect-your-components.md).
- The [API reference](docs/api.md) lists all public APIs.
- Visit the [How To ?](docs/how-to.md) section for further information.

## Author

- Ajay Bhatia ([@ajaybhatia](https://github.com/ajaybhatia))
- Théo Mathieu ([@Mokto](https://github.com/Mokto)) from [inProgress](https://in-progress.io)
- Nicolas Charpentier ([@charpeni](https://github.com/charpeni))

## Want to help ?

Pull Requests and issues reported are welcome! :)

## License

react-native-meteor is [MIT Licensed](LICENSE).
