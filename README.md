设计模式存在根本原因是为了代码复用，增加可维护性。有如下原则
- 【开放原则】 对扩展开放，对修改关闭，ps高考的试卷
- 【里氏转换原则】子类继承父类，单独掉完全可以运行，ps盗版光盘
- 【依赖倒转原则】引用一个对象，如果这个对象有底层类型，直接引用底层，ps三个和尚打水，直接可从井里打，但是中间把水打出来放到一个桶里。
- 【接口隔离原则】每一个接口应该是一种角色，ps汽车USB插口。
- 【合成/聚合复用原则】新的对象应使用一些已有的对象，使之成为新对象的一部分，ps手里有一些相机的零件，而又去买了一个新的相机。
- 【迪米特原则】一个对象应对其他对象有尽可能少的了解，ps现实中的对象

综述：站在巨人的肩膀上整体HOLD系统架构。

### 单例模式

#### 概念解读

> 单例就是保证一个类只有一个实例，实现的方法一般就是先判断实例存在与否，
如果存在直接返回，如果不存在就创建了再返回，这就确保了一个类只有一个实例对象。
在javascript里，单例作为一个命名空间提供者，从全局命名空间里提供一个唯一的访问点来访问该对象。

#### 作用和注意事项

- 作用：
    - 模块间通信。
    - 系统中某个类的对象只能存在一个。
    - 保护自己的属性和方法。
- 注意事项
    - 注意this的使用。
    - 闭包容易造成内存泄露，不需要的赶快干掉。
    - 注意new的成本。（继承）

#### 代码实战

```js

    //1.独立的对象 建2个 一个xiaowang 一个xiaoli
    //2.让xiaoli跟xiaowang通过门铃进行通信
    //3.先看一下xiaownag家有没有门 如果有门直接通过门铃通讯did 如果没有先建门
    //4.两个单例之间开始通信

    var xiaowang = (function (argument) {
        var xiaowangjia = function (message) {
            this.menling = message;
        };
        var men;
        var info = {
            sendMessage: function (message) {
                if(!men) {
                    men = new xiaowangjia(message);
                }
                return men;
            }
        };
        return info;
    })();
    var xiaoli = {
        callXiaowang: function (msg) {
            var _xw = xiaowang.sendMessage(msg);
            alert(_xw.menling);
            _xw = null; // 等待垃圾回收机制
        }
    };
    xiaoli.callXiaowang('didi');


    // 页面上有6个按钮 a b c => top, d e f => banner
    var top = {
        init: function () {
            this.render();
            this.bind();
        },
        a: 4,
        render: function () {
            var me = this;
            me.btna = $('#a');
        },
        bind: function () {
            var me = this;
            me.btna.click(function () {
                me.test();
            })
        },
        test: function () {
            a = 5;
        }
    };

    var banner = {
        init: function () {
            this.render();
            this.bind();
        },
        a: 4,
        render: function () {
            var me = this;
            me.btna = $('#a');
        },
        bind: function () {
            var me = this;
            me.btna.click(function () {
                me.test();
            })
        },
        test: function () {
            a = 5;
        }
    };

    top.init();
    banner.init();

```

### 构造函数模式

#### 概念解读

> 构造函数用于创建特定类型的对象--不仅声明了使用的对象，构造函数还可以
接受参数以便第一次创建对象的时候设置对象的成员值。你可以自定义自己的构造函数，
然后在里面声明自定义类型对象的属性或方法。
在javascript里，构造函数通常是认为用来实现实例的，javas没有类的概念，
但是有特殊的构造函数。通过new关键字来调用自定义的构造函数，
在构造函数内部，this关键字引用的是新创建的对象。

#### 作用和注意事项

- 作用
    - 用于创建特定类型的对象。
    - 第一次声明的时候给对象赋值。
    - 自己声明构造函数，赋予属性和方法。
- 注意事项
    - 声明函数的时候处理业务逻辑。
    - 区分和单例的区别，配合单例实现初始化。
    - 构造函数大写字母开头。（建议）
    - 注意new的成本。（继承）

#### 代码实战

```js

    //AA公司, 这里是为了配合单例模式
    var AA = {
        zaomen: function(huawen) {
            var _huawen = '普通';
            if (huawen) {
                _huawen = huawen;
            }
            this.suo = '普通';
            this.huawen = _huawen;
            this.create = function () {
                return '【锁头】' + this.suo + '【花纹】' + this.huawen;
            }
        }
    };

    //BB公司
    var BB = {
        zaomen: function(huawen) {
            var _huawen = '普通';
            if (huawen) {
                _huawen = huawen;
            }
            this.suo = '普通';
            this.huawen = _huawen;
            this.create = function () {
                return '【锁头】' + this.suo + '【花纹】' + this.huawen;
            }
        }
    };

    var xiaozhang = new AA.zaomen();
    alert('xiaozhang' + xiaozhang.create());

    var xiaoli = new BB.zaomen('绚丽');
    alert('xiaoli' + xiaoli.create());

```