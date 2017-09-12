# react-magic-component
React component with binding events or lifecycles by selector

## Features
- `initMagic`初始化注入环境，建议在`main文件`中执行;
- 施展魔法的方式:
  - 使用`create`创建组件;
  - 定义组件时包含`getMagicConfig`方法;
  - 使用组件时包含`[data-]magicCong`属性;
- 能施展的魔法包括:
  - 特定的props，默认有locale、language、style;
  - `react-redux`的connect;
  - 组件的生命周期;
  - 组件的事件;
- 绑定生命周期和事件时:

  > 有三个对象需要注意: 目标组件/元素<elInst>、方法源组件<mtInst>、数据源组件<dsInst>

  - <b>数据源组件</b>指回调函数依赖的逻辑数据所在的组件。一般指你在哪个组件上定义魔法，该组件就是数据源组件。
  - <b>方法源组件</b>指回调函数的来源。如果是匿名函数或不包含`.`，则与数据源组件相同，反则用`Xxx.method`设定，方法源则为`Xxx`。
  - this指向: 生命周期指向目标组件，事件指向方法源组件；
  - 回调函数的后两个参数: 生命周期分别为`[mtInst, dsInst]`，事件分别为`[elInst, dsInst]`;
  - 支持设定是否覆盖<b>old-event/liferycle</b>，事件名后跟上`:0`不覆盖，反则或默认覆盖;
  - 支持设定<b>old-event/liferycle</b>是否在<b>new-event/liferycle</b>前执行，事件名后跟上`:^0`，默认之后执行;
  - 原生命周期函数的返回值会是新函数的第一个值，覆盖或无原生命函数则是undefined;

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

## Quick Start

## License
[MIT](https://tldrlegal.com/license/mit-license)
