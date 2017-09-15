# react-magic-component
React component with binding events or lifecycles by selector

## Features
- `initMagic`初始化注入环境，建议在`main文件`中执行;
- 施展魔法的方式:
  - 使用`create`创建组件;
  - 定义组件时包含`getMagicConfig`方法;
  - 使用组件时包含`[data-]magicCong`属性;
  - props中包含`[data-]magic-*`属性;
- 能施展的魔法包括:
  - 特定的props，默认有locale、language、style;
  - `react-redux`的connect;
  - 组件的生命周期;
  - 组件的事件;
- 绑定生命周期和事件时:

  > 有三个对象需要注意: 目标组件/标签(elInst)、方法源组件(mtInst)、数据源组件(dsInst)

  - <b>数据源组件</b>指回调函数依赖的逻辑数据所在的组件。一般指你在哪个组件上定义魔法，该组件就是数据源组件。
  - <b>方法源组件</b>指回调函数的来源。如果是匿名函数或不包含`.`，则与数据源组件相同，反则用`Xxx.method`设定，方法源则为`Xxx`。
  - this指向: 生命周期指向目标组件，事件指向方法源组件；
  - 回调函数的后两个参数: 生命周期分别为`[mtInst, dsInst]`，事件分别为`[elInst, dsInst]`;
  - 如果事件名(或方法名)后跟上`:0`，则不覆盖<b>old</b>，非零或默认覆盖;
  - 如果事件名(或方法名)后跟上`:^0`，则<b>new->old</b>顺序执行，默认按<b>old->new</b>;
  - 原生命周期函数的返回值会是新函数的第一个值，覆盖或无原生命函数则是undefined;
- 魔法组件不影响普通嵌套，不会像connect私自加一层组件(HOC);
- 请尽量不要在最外层组件如App上设置魔法，原因是外层组件会在mount改写前执行而影响注入;
- `[data-]magic-*`属性只能设置为string类型，如果不设置来源(`Xxx.method`)则寻找有此方法的最靠近的祖父魔法组件;【非冒泡式】

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
  props: {
    'onClick button': 'handleClick'
  },
  handleClick() {
    console.log('this is:', this); // this -> LoggingButton
  }
});
```

And it's cooler if you use decorator syntax. (ES7)
```
@create({
  props: {
    componentDidMount() {
      console.log('Component did mount');
    }
  }
})
class Bar extends React.Component {
  render() {}
}
```

## Quick Start

- [https://github.com/valleykid/dva-example-user-dashboard](https://github.com/valleykid/dva-example-user-dashboard)
- [https://github.com/valleykid/react-redux-todos](https://github.com/valleykid/react-redux-todos)

## License
[MIT](https://tldrlegal.com/license/mit-license)

