+++
title = "TypeScript 初体验"
date = 2020-07-30T01:04:00+08:00
draft = false
creator = "Emacs 26.3 (Org mode 9.3.7 + ox-hugo)"
+++

## PropTypes 转换 {#proptypes-转换}

PropTypes 是 React 对 `prop` 的一种类型检查机制，但从 v15.5 开始，官方已经用 `prop-types` 库替代了 `React.PropTypes` ，在 TypeScript 项目中使用
JavaScript 库时需要下载声明文件 `*.d.ts` ，即下载对应的 `@types/prop-types` ，用法如下

| React              | + TypeScript      |
|--------------------|-------------------|
| `PropTypes.bool`   | `boolean`         |
| `PropTypes.func`   | `() => void`      |
| `PropTypes.number` | `number`          |
| `PropTypes.object` | `{}`              |
| `PropTypes.string` | `string`          |
| `PropTypes.node`   | `React.ReactNode` |
| `PropTypes.any`    | `any`             |

另外，强制类型检查方式也不一样，举个例子，前者用
`isTrue: PropTypes.bool.isRequired` 和 `name: PropTypes.string` ，后者用 `isTrue: boolean` ， `name?:stirng`

前面都是比较简单的，如果遇到复杂一点的呢，比如原项目中有如下 `shape` 定义：

```js
export default PropTypes.arrayOf(
    PropTypes.oneOfType([
      fooShape,
      PropTypes.shape({
	  a: PropTypes.any.isRequired,
	  b: PropTypes.arrayOf(fooShape).isRequired,
      }),
    ]),
);
```

如何把它转化为 TypeScript 定义呢，我的解决方式结合 `interface` 和 `type` ：

```js
interface BarShape {
    a: any,
    b: FooShape[]
}

export default type X = Array<FooShape | BarShape>;
```

以上内容有参考 stackoverflow，后来发现 [TS spec](https://github.com/microsoft/TypeScript/blob/master/doc/spec.md) 都已经写得很清楚了。当然，也并不是所有的 JavaScript 库都需要单独下载 `.d.ts` 文件，比如该项目中用到的[`nanoid`](https://github.com/ai/nanoid/issues/204#issuecomment-606189297)，作者已经提前写好了


## enum {#enum}

有如下代码片段：

{{< highlight js "hl_lines=10 15" >}}
const ALIGNMENTS = {
    MIDDLE: 'middle',
    TOP: 'top',
};

...

class DualListBox extends React.Component {
    static propTypes = {
      alignActions: PropTypes.oneOf([ALIGNMENTS.MIDDLE, ALIGNMENT.TOP]),
      ...
    };

    static defaultProps = {
      alignActions: ALIGNMENTS.MIDDLE,
      ...
    };

    render() {
      return ();
    };
};
{{< /highlight >}}

这里 ALIGNMENTS 既可以用做类型检查又可以赋值给变量，但是在 TS 中 `const` 的方法，只能起一种作用，所以我采用了 `enum` 解决上述问题：

```js
enum ALIGNMENTS {
    MIDDLE = 'middle',
    TOP = 'top',
}
```


## FontAwesome {#fontawesome}

原生，用 `@types` 过于繁琐了


## CDN {#cdn}

React 和 React DOM