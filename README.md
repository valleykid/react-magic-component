# react-magic-component
React component with binding events or lifecycles by selector

## Features

## Examples

old:
```
class LoggingButton extends React.Component {
  // This syntax ensures `this` is bound within handleClick.
  // Warning: this is *experimental* syntax.
  handleClick = () => {
    console.log('this is:', this);
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        Click me
      </button>
    );
  }
}
```

new:
```
function LoggingButton (props, context) {
  return (
    <button>
      Click me
    </button>
  );
}

create(LoggingButton, {
  events: {
    'onClick button': 'handleClick'
  },
  handleClick() {
    console.log('this is:', this); // this -> LoggingButton
  }
});
```

## Quick Start

## License
[MIT](https://tldrlegal.com/license/mit-license)
