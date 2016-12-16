---
title: 'Uncaught TypeError: _react2.default.render is not a function 解决方法'
date: 2016-12-11 15:39:25
tags: [react]
categories: [react]
---

# 在浏览器上报错`Uncaught TypeError: _react2.default.render is not a function`

# 原因

> React版本过高，不支持React.render()方法。

```
'use strict';
import React from 'react';
import ReactDOM from 'react-dom';
import LocalDb from 'localDb';
import TodoHeader from './TodoHeader';
import TodoMain from './TodoMain';
import TodoFooter from './TodoFooter';


/**
 * 从上面的渲染（render）方法可以看出，组件的结构分为三部分，就是上中下。
 * 上面的TodoHeader是用来输入任务的地方，中间的TodoMain是用来展示任务列表的,
 * 下面的TodoFooter提供一些特殊的方法，比如全选、删除等。
 */

class App extends React.Component{
    constructor(){
        super();
        this.db = new LocalDb('ReactDemo');
        //定义组件状态
        this.state  ={
            todos:this.db.get('todos') || [],
            isAllChecked:false
        };
    }

// 判断是否所有任务的状态都完成，同步底部的全选框
    allChecked(){
        let isAllChecked = false;
        if (this.state.todos.every(todo  => todo.isDone)){
            isAllChecked = true;
        }
        this.setState({
            todos:this.state.todos,
            isAllChecked:isAllChecked
        });
    }

// 添加任务，是传递给Header组件的方法
    addTodo(todoItem){
        this.state.todos.push(todoItem);
        this.db.set('todos',this.state.todos);
        this.allChecked();

    }

// 删除当前的任务，传递给TodoItem的方法
    deleteTodo(index){
        this.state.todos.splice(index,1);
        this.setState({todos:this.state.todos});
        this.db.set('todos',this.state.todos);
    }

// 清除已完成的任务，传递给Footer组件的方法
    clearDone(){
        let todos = this.state.todos.filter(todo => !todo.isDone);
        this.setState({
            todos:todos,
            isAllChecked:false
        });
        this.db.set('todos',todos);
    }

// 改变任务状态，传递给TodoItem和Footer组件的方法
    changeTodoState(index,isDone,isChangeAll=false){
        if(isChangeAll){
            this.setState({
                todos:this.state.todos.map( (todo) => {
                    todo.isDone = isDone;
                    return todo;
                }),
                isAllChecked:isDone
            });
        }else{
            this.state.todos[index].isDone = isDone;
            this.allChecked();
        }
        this.db.set('todos',this.state.todos);
    }

//组件渲染方法
    render(){
        let info = {
            isAllChecked:this.state.isAllChecked,
            todoCount:this.state.todos.length || 0,
            todoDoneCount:(this.state.todos && this.state.todos.filter((todo)=>todo.isDone)).length || 0
        };
        return (
        <div className="todo-wrap">
            <TodoHeader addTodo={this.addTodo.bind(this)} />
            <TodoMain todos={this.state.todos} deleteTodo={this.deleteTodo.bind(this)} changeTodoState={this.changeTodoState.bind(this)} />
            <TodoFooter {...info} changeTodoState={this.changeTodoState.bind(this)} clearDone={this.clearDone.bind(this)} />
        </div>
    );
    }
}

React.render(<App/>,document.getElementById('app'));   //这句的问题

```

# 解决方法

1. 降低React版本（不推荐）
2. 将最后一句`React.render(<App/>,document.getElementById('app'));`改成`ReactDOM.render(<App/>,document.getElementById('app'));` （如果没有引入react-dom是会报找不到ReactDOM方法的）

# 其实有关DOM操作的时候，都需要用ReactDOM来代替React。

例如：这里也会报同样的错误

`React.findDOMNode(this).style.background = '#eee';`

改成这样就好了

`ReactDOM.findDOMNode(this).style.background = '#eee';`
