# hooks-demo

提供一个简单的使用hooks解决react组件间通信的demo

## 准备

### 环境准备

```
node v12.13.1
npm 6.12.1
create-react-app 3.3.0

// 可根据自己情况灵活选择
```

### 搭建结构

```bash
create-react-app hooks-demo
cd hooks-demo
npm run start
```
### 删除无用文件
最终目录结构如下：

![1.png](./doc/1.png)

## 搭建静态结构

### 目标功能
![2.png](./doc/2.png)

如上图所示：

- 点击`add`向列表中添加元素
- 点击`-`删除对应元素
- 点击`clear`清空列表

### 组件结构
![3.png](./doc/3.png)

可以发现，`Detail`组件展示列表的数据，将要被`App`、 `Detail`、 `Header`三个组件分别操作

主要代码如下：

```javascript
const App = () => {
  return (
    <section className="App">
      <div style={{ marginBottom: "15px" }}>
        <b>hooks-demo</b>
        <button>clear</button>
      </div>

      <Header />
      <Detail />
    </section>
  )
}

const Detail = () => {
  return (
    <section className="detail">
      <ul className="detail__list">
        <li className="detail__list-item">
          <span>张三</span>
          <button className="detail__list-item-del">-</button>
        </li>

        <li className="detail__list-item">
          <span>李四</span>
          <button className="detail__list-item-del">-</button>
        </li>

        <li className="detail__list-item">
          <span>王五</span>
          <button className="detail__list-item-del">-</button>
        </li>
      </ul>
    </section>
  )
}

const Header = () => {
	return (
		<section className="header">
			<input type="text" className="header__input" />
			<button className="header__confirm">add</button>
		</section>
	)
}
```

## 搭建动态结构

新建`hooks.jsx`，主要代码如下：

```javascript
const Types = {
  ADD_ITEM: "ADD_ITEM",
  DEL_ITEM: "DEL_ITEM",
  CLEAR_ALL: "CLEAR_ALL"
}

export const ActionCreators = {
  add: (item) => ({ type: Types.ADD_ITEM, data: item }),
  del: (index) => ({ type: Types.DEL_ITEM, data: index }),
  clear: () => ({ type: Types.CLEAR_ALL }),
}

export const DefaultData = { list: [] }

export const Reducer = (state, action) => {
  switch (action.type) {
    case Types.ADD_ITEM:
      return { ...state, list: [...state.list, action.data] }
    case Types.DEL_ITEM:
      return { ...state, list: state.list.filter((_, index) => index !== action.data) }
    case Types.CLEAR_ALL:
      return { ...state, list: [] }
    default:
      return state
  }
}

export const Context = React.createContext({})
```

对`App.jsx`、 `Detail.jsx`、 `Header.jsx`分别做如下修改：

![4.png](./doc/4.png)

![5.png](./doc/5.png)

![6.png](./doc/6.png)

最终效果如下：
![7.gif](./doc/7.gif)

## 参考资料

- [react.js小书](http://huziketang.mangojuice.top/books/react/lesson1) 小书详细描述了 react 及 redux 的原理
- [react-learn-ts](https://github.com/baibai-lee/react-learn-ts) 我在学习react时积累的一个demo，其中更完整地使用的react的各项api
- [todolist](http://www.todolist.cn/)  一个简易的 todolist，以上的两个demo灵感均来源于此
