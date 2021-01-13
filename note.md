props是单向绑定的，即只能父组件向子组件传递，不能反向

1、父组件可以使用 props 把数据传给子组件。
2、子组件可以使用 $emit 触发父组件的自定义事件。

vm.$emit( event, arg ) //触发当前实例上的事件
this.$emit("function",param);   //其中function为父组件定义函数，param为需要传递参数

vm.$on( event, fn );//监听event事件后运行 fn； 

