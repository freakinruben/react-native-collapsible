# react-native-collapsible
*Animated collapsible component for React Native using the Animated API*

Pure JavaScript, supports dynamic content heights and components that is aware of its `collapsed` state (good for toggling arrows etc).

## Installation

```
npm install --save react-native-collapsible
```

## Collapsible Usage

```js
var Collapsible = require('react-native-collapsible');
<Collapsible collapsed={isCollapsed}>
  <SomeCollapsedView />
</Collapsible>
```

## Properties

| Prop | Description | Default |
|---|---|---|
|**`align`**|Alignment of the content when transitioning, can be `top`, `center` or `bottom`|`top`|
|**`collapsed`**|Wether to show the child components or not|`true`|
|**`duration`**|Duration of transition in milliseconds|`300`|
|**`easing`**|Function or function name from [`Easing`](https://github.com/facebook/react-native/blob/master/Libraries/Animated/src/Easing.js) (or [`tween-functions`](https://github.com/chenglou/tween-functions) if < RN 0.8). Collapsible will try to combine `Easing` functions for you if you name them like `tween-functions`. |`easeOutCubic`|

## Accordion Usage

This is a convenience component for a common use case, see demo below. 

```js
var Accordion = require('react-native-collapsible/Accordion');
<Accordion 
  sections={['Section 1', 'Section 2', 'Section 3']}
  renderHeader={this._renderHeader}
  renderContent={this._renderContent}
/>
```

## Properties

| Prop | Description |
|---|---|---|
|**`sections`**|An array of sections passed to the render methods|
|**`renderHeader(content, index, isActive)`**|A function that should return a renderable representing the header|
|**`renderContent(content, index, isActive)`**|A function that should return a renderable representing the content|
|**`onChange(index)`**|An optional function that is called when currently active section is changed, `index === false` when collapsed|
|**`initiallyActiveSection`**|Which index in the `sections` array to be initially open. Defaults to none. |
|**`underlayColor`**|The color of the underlay that will show through when tapping on headers. Defaults to black. |
|**`align`**|See `Collapsible`|
|**`duration`**|See `Collapsible`|
|**`easing`**|See `Collapsible`|

## Demo

![demo](https://cloud.githubusercontent.com/assets/378279/8047315/0237ca2c-0e44-11e5-9a16-1da052406eb0.gif)

## Example 

Check full example in the `Example` folder. 

```js
var React = require('react-native');
var Accordion = require('react-native-collapsible/Accordion');

var SECTIONS = [
  {
    title: 'First',
    content: 'Lorem ipsum...',
  },
  {
    title: 'Second',
    content: 'Lorem ipsum...',
  }
];

var AccordionView = React.createClass({
  _renderHeader(section) {
    return (
      <View style={styles.header}>
        <Text style={styles.headerText}>{section.title}</Text>
      </View>
    );
  },

  _renderContent(section) {
    return (
      <View style={styles.content}>
        <Text>{section.content}</Text>
      </View>
    );
  },

  render: function() {
    return (
      <Accordion
        sections={SECTIONS}
        renderHeader={this._renderHeader}
        renderContent={this._renderContent}
      />
    );
  }
});
```

### Transition backgrounds

If you combine with the [`react-native-animatable`](https://github.com/oblador/react-native-animatable) library you can easily transition the background color between the active and inactive state or add animations. 

Lets augment the example above with:
```js
var Animatable = require('react-native-animatable');

(...)

  _renderHeader(section, index, isActive) {
    return (
      <Animatable.View
        style={styles.header}
        duration={300}
        transition="backgroundColor"
        style={{ backgroundColor: (isActive ? 'rgba(255,255,255,1)' : 'rgba(245,252,255,1)') }}>
        <Text style={styles.headerText}>{section.title}</Text>
      </Animatable.View>
    );
  },
  _renderContent(section, i, isActive) {
    return (
      <Animatable.View
        style={styles.content}
        duration={300}
        transition="backgroundColor"
        style={{ backgroundColor: (isActive ? 'rgba(255,255,255,1)' : 'rgba(245,252,255,1)') }}>
        <Animatable.Text
          duration={300}
          easing="ease-out"
          animation={isActive ? 'zoomIn' : false}>
          {section.content}
        </Animatable.Text>
      </Animatable.View>
    );
  },

(...)
```

To produce this (slowed down for visibility): 

![accordion-demo](https://cloud.githubusercontent.com/assets/378279/10767769/2ddfe234-7cb4-11e5-8ef1-c0f8c67ead58.gif)


## License

[MIT License](http://opensource.org/licenses/mit-license.html). © Joel Arvidsson 2015

