# Web前端精髓

## 父子数据传递

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <div id="app">
        <parent></parent>
    </div>
    <template id="ban">
        <!--模板可以直接在上面定义然后直接用id引入  -->
        <div>
            <div>parentwu</div>
            <!-- 传递数据 -->
            <child :str="msg"></child>
        </div>
    </template>
    <script type="text/javascript" src="./vue2.js"></script>
    <script type="text/javascript">
        var vm = new Vue({
            el: "#app",
            data: {
            },
            components: {
                parent: {
                    template: "#ban",
                    data() {
                        return {
                            msg: "xianqiang"
                        }
                    },
                    components: {
                        child: {
                            template: "<div>child{{str}}</div>",
                            // 接收数据
                            props:['str']
                        }             
                    }
                }
            }
        })
    </script>
</body>
</html>
```
## 子父传递数据

```js
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <div id="app">
        <parent></parent>
    </div>
    <template id="ban">
        <!--模板可以直接在上面定义然后直接用id引入  -->
        <div>
            <div>parent年龄:{{age}}</div>
            <child @s="p"></child>
            <!--接收到s声名一个方法p  方法p是在我们的父组件中 -->
        </div>
    </template>
    <script type="text/javascript" src="./vue2.js"></script>
    <script type="text/javascript">
        var vm = new Vue({
            el: "#app",
            data: {

            },
            components: {
                parent: {
                    methods: {
                        p(data) {
                            this.age = data;
                        }
                    },
                    template: "#ban",
                    data() {
                        return {
                            age: 0
                        }
                    },
                    components: {
                        child: {
                            template: "<div @click='sayage'>child</div>",
                            methods: {
                                sayage() {
                                    // 向上发射数据emit,s是名字age是发射的数据，发射到儿子组件上
                                    this.$emit("s",this.age);
                                }
                            },
                            data() {
                                return {
                                    age: 18
                                }
                            }
                        }
                    }
                }
            }
        })
    </script>
</body>
</html>
```

## 子组件数据更改父组件数据也更改

```js
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>

<body>
    <div id="app">
        <parent></parent>
    </div>
    <template id="ban">
        <!--模板可以直接在上面定义然后直接用id引入  -->
        <div>
            <div>parentwu</div>{{msg.msg}}
            <child :str="msg"></child>
        </div>
    </template>
    <script type="text/javascript" src="./vue2.js"></script>
    <script type="text/javascript">
        var vm = new Vue({
            el: "#app",
            data: {

            },
            components: {
                parent: {
                    template: "#ban",
                    data() {
                        return {
                            msg: {msg:"xianqiang"}
                            // 使用对象，对象是引用数据类型的，取值时候msg.msg
                        }
                    },
                    components: {
                        child: {
                            template: "<div @click='updata'>child{{str.msg}}</div>",
                            methods: {
                                updata() {
                                    // 在vue2.0中子组件是不允许更改父组件
                                    this.str.msg = "none"
                                }
                            },
                            props:['str']
                        }                        
                    }
                }
            }
        })
    </script>
</body>
</html>
```
## 子组件数据更改父组件数据不更改

```js
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>

<body>
    <div id="app">
        <parent></parent>
    </div>
    <template id="ban">
        <!--模板可以直接在上面定义然后直接用id引入  -->
        <div>
            <div>parentwu</div>{{msg}}
            <child :str="msg"></child>
        </div>
    </template>
    <script type="text/javascript" src="./vue2.js"></script>
    <script type="text/javascript">
        var vm = new Vue({
            el: "#app",
            data: {

            },
            components: {
                parent: {
                    template: "#ban",
                    data() {
                        return {
                            msg: "xianqiang"
                            // 使用对象，对象是引用数据类型的，取值时候msg.msg
                        }
                    },
                    components: {
                        child: {
                            data() {
                                // 第三方变量
                                return {a: ""}
                            },
                            mounted() {
                                this.a = this.str;
                                // 不让我们更改父亲的数据，我们就自己定义数据，防止了控制台报错的问题
                            },
                            template: "<div @click='updata'>child{{a}}</div>",
                            methods: {
                                updata() {
                                    // 在vue2.0中子组件是不允许更改父组件
                                    this.a = "none"
                                }
                            },
                            props:['str']
                        }                        
                    }
                }
            }
        })
    </script>
</body>

</html>
```
## 非父子组件的数据交互

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>

<body>
    <div id="app">
        <parent></parent>
        <child></child>
    </div>
    <template id="ban">
        <div>
            <div>hello <button type="button" @click="send">发送数据</button></div>
        </div>
    </template>
    <!--事件vuex  -->
    <template id="mo">
        <div>
            <div>您好 {{msg}}</div>
        </div>
    </template>
    <script type="text/javascript" src="./vue2.js"></script>
    <script type="text/javascript">
        var Event = new Vue;
        var vm = new Vue({
            el: "#app",
            data: {
            },
            components: {
                parent: {
                    template: "#ban",
                    data() {
                        return {
                            msg: "信息"
                        }
                    },
                    methods: {
                        send() {
                        // 发射数据
                            Event.$emit("wo", this.msg)
                        }
                    }
                },
                child: {
                    mounted() {
                    // 监听数据
                        Event.$on("wo", (data) => {
                            this.msg = data
                        })
                    },
                    data() {
                        return {
                            msg: ""
                        }
                    },
                    template: "#mo"
                }
            }
        })
    </script>
</body>

</html>
```

> 支持作者请点击右上角的Star按钮
