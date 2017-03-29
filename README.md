> 极客学院--javascript设计模式系列课程--学习笔记

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

### 建造者模式

#### 概念解读

> 建造者模式可以将一个复杂对象的构建与其表示相分离，使得同样的构建过程可以创建不同的表示。
也就是说如果我们用了建造者模式，那么用户就需要指定需要建造的类型就可以得到它们，而具体建造的过程和细节就不需要知道了。
建造者模式实际，就是一个指挥者，一个建造者，一个使用者调用具体建造者工作得出结果的客户。
建造者模式主要用于“分步骤构建一个复杂的对象”，在这其中“分步骤”是一个稳定的算法，而复杂对象的各个部分则经常变化。

#### 作用和注意事项

- 作用
    - 分步创建一个复杂的对象
    - 解耦封装过程和具体创建的组件
    - 无需关心组件如何组装
- 注意事项
    - 一定要一个稳定的算法进行支持
    - 加工工艺是暴露的
    
#### 代码实战

```js

    //1.产出的东西是房子
    //2.baogongtou 调用工人进行开工 而且他要很清楚工人们具体的特长
    //3.工人是盖房子的 工人可以建卧室 建客厅 建厨房
    //4.包工头只是一个接口而已 不干活
    function Fangzi() {
        this.woshi = '';
        this.keting = '';
        this.chufang = '';
    }

    function Baogongtou() {
        this.gaifangzi = function (gongren) {
            gongren.jian_woshi();
            gongren.jian_keting();
            gongren.jian_chufang();
        }
    }

    function Gongren() {
        this.jian_woshi = function () {
            console.log('卧室我盖好了');
        };
        this.jian_keting = function () {
            console.log('客厅建好了');
        };
        this.jian_chufang = function () {
            console.log('厨房建好了');
        };
        this.jiaogong = function () {
            var _fangzi = new Fangzi();
            _fangzi.woshi = "ok";
            _fangzi.keting = "ok";
            _fangzi.chufang = "ok";
            return _fangzi;
        }
    }

    var gongren = new Gongren();
    var baogongtou = new Baogongtou();
    baogongtou.gaifangzi(gongren);
    //主人要房子
    var myfangzi = gongren.jiaogong();
    console.log(myfangzi);

```

### 工厂模式

#### 概念解读

> 工厂模式定义一个用于创建对象的接口，这个接口由子类决定实例化哪一个类。
该模式使一个类的实例化延迟到了子类。而子类可以重写接口方法以便创建的时候指定自己的对象类型（抽象工厂）。
这个模式十分有用，尤其是创建对象的流程赋值的时候，比如依赖于很多设置文件等。
并且，你会经常在程序里看到工厂方法，用于让子类定义需要创建的对象类型。

#### 作用和注意事项

- 作用
    - 对象的构建十分复杂
    - 需要依赖具体的环境创建不同实例
    - 处理大量具有相同属性的小对象
- 注意事项
    - 不能滥用工厂，有时候仅仅是给代码增加复杂度

#### 代码实战

```js

    //1.工厂应该有厂长 来决定运行的是哪条产品线
    /*
    var gongchang = {};
    gongchang.chanyifu = function (argument) {
        this.gongren = 50;
//        alert('我们有' + this.gongren);
    };
    gongchang.chanxie = function () {
        alert('产鞋子~');
    };
    gongchang.yunshu = function () {
        alert('运输');
    };
    gongchang.changzhang = function (para) {
        // new js我们说了 构造函数模式 单例模式
        return new gongchang[para]();
    };
    var me = gongchang.changzhang('chanyifu');
    alert(me.gongren);
    */
//----------------------------------------
    //这是一个简单的工厂模式
    var XMLHttpFactory = function () {

    };
    XMLHttpFactory.createXMLHttp = function () {
        var XMLHttp = null;
        //XMLHttpFactory.createXMLHttp()这个方法是根据当前环境的具体情况返回一个XHP对象
        if(window.XMLHttpRequest) {
            XMLHttp = new XMLHttpRequest();
        } else if(window.ActiveXObject) {
            XMLHttp = new ActiveXObject('Microsoft.XMLHttp')
        }
        return XMLHttp;
    };
    var AjaxHander = function () {
        var XMLHttp = XMLHttpFactory.createXMLHttp();
    };

//    -------------------------------------------------
    //这是一个抽象工厂
    var XMLHttpFactory = function () {

    };
    XMLHttpFactory.prototype = {
        //如果真的要调用这个方法会抛出一个错误，它不能被实例化，只能用来派生子类
        createFactory: function () {
            throw new Error('This is an abstract class');
        }
    };
    //派生子类
    var XHRHandler = function () {
        XMLHttpFactory.call(this);
    };
    XHRHandler.prototype = new XMLHttpFactory();
    XHRHandler.prototype.constructor = XHRHandler;

    //重新定义createFactory 方法
    XHRHandler.prototype.createFactory = function () {
        var XMLHttp = null;
        if(window.XMLHttpRequest) {
            XMLHttp = new XMLHttpRequest();
        } else if(window.ActiveXObject) {
            XMLHttp = new ActiveXObject('Microsoft.XMLHttp')
        }
        return XMLHttp;
    }
    var AjaxHander = function () {
        var XMLHttp = XHRHandler.createFactory();
    };

```

### 代理模式

#### 概念解读

> 代理，顾名思义就是帮别人做事。
代理模式，为其他对象提供一种代理以控制对这个对象的访问。
代理模式使得代理对象控制具体对象的引用。代理几乎可以是任何对象：
文件，资源，内存中的对象，或者是一些难以复制的东西。

#### 作用和注意事项

- 作用
    - 远程代理（一个对象将不同空间的对象进行局部代理）
    - 虚拟代理（根据需要创建开销很大的对象如渲染网页暂时用占位符代替真图）
    - 安全代理（控制真实对象的访问权限）
    - 智能指引（调用对象代理处理另外一些事情如垃圾回收机制）
- 注意事项
    - 不能滥用代理，有时候仅仅是给代码增加复杂度

#### 代码实战

```js

    //代理模式需要3方
    //1.买家
    function maijia(argument) {
        this.name = '小明';
    }
    //2.中介
    function zhongjie() {

    }
    zhongjie.prototype.maifang = function () {
        new fangdong(new maijia()).maifang('20')
    };
    //3.房东
    function fangdong(maijia) {
        this.maijia._name = maijia.name;
        this.maifang = function (money) {
            alert('收到了来自【' + this.maijia._name + '】' + money + '人名币');
        }
    }
    (new zhongjie).maifang();

```

### 命令模式

#### 概念解读

> 用来对方法调用进行参数化处理和传送，经过这样处理过得方法调用可以在任何需要的时候执行。
也就是说该模式旨在将函数的调用、请求和操作封装成一个单一的对象，然后对这个对象进行一系列的处理。
它也可以用来消除调用操作的对象和实现操作的对象之间的耦合。这为各种具体的类的更换带来了极大的灵活性。

#### 作用和注意事项

- 作用
    - 将函数的封装、请求、调用结合为一体
    - 调用具体的函数解耦命令对象与接收对象
    - 提高程序模块化的灵活性
- 注意事项
    - 不需要接口一致的，直接调用函数即可，以免造成浪费

#### 代码实战

```js

    var lian = {};
    lian.paobing = function (pao_num) {
        alert(pao_num + '炮' + '开始战斗');
    };
    lian.bubing = function (bubing_num) {
        alert(bubing_num + '人' + '开始战斗');
    };
    lian.lianzhang = function (mingling) {
        lian[mingling.type](mingling.num);
    };
    //司令开始发布命令
    lian.lianzhang({
        type: 'paobing',
        num: 100
    });
    lian.lianzhang({
        type: 'bubing',
        num: 500
    });

```

### 观察者模式（发布订阅模式）

#### 概念解读

> 定义了一种一对多的关系，让多个观察者对象同时监听某一个主题对象，
这个主题对象的状态发生变化时就会通知所有观察者对象，使得它们能够自动更新自己。

#### 作用和注意事项

- 作用
    - 支持简单的广播通信，自动通知所有已经订阅过得对象
    - 页面载入后目标对象很容易与观察者存在一种动态关联，增加了灵活性
    - 目标对象与观察者之间的抽象耦合关系能够单独扩展以及重用
- 注意事项
    - 监听要在触发之前

#### 代码实战

```js
//需要引入jquery
//~自执行
    ~(function () {
        var o = $({});
        $.jianting = function () {
            o.on.apply(o, arguments);
        };
        $.fabu = function () {
            o.trigger.apply(o, arguments);
        };
        $.qingchu = function () {
            o.off.apply(o, arguments);
        }
    })();
    $.jianting('/test/ls', function (e, a, b, c) {
        alert(a + '||' + b + '||' + c);
    });
    $.jianting('/test/ls', function (e, a, b, c) {
        alert('ok');
    });
    $.fabu('/test/ls', [1,2,3])

```

### 适配器模式

#### 概念解读

> 适配器模式是将一个类（对象）的接口（方法或属性）转换成客户希望的另外一个接口（方法或属性），
适配器模式使得原本由于接口不兼容而不能一起工作的那些类（对象）可以一起工作。

#### 作用和注意事项

- 作用
    - 使用一个已经存在的对象，但其方法或接口不符合你的要求
    - 创建一个可复用的对象，该对象可以与其他不相关或不可见的对象协同工作
    - 使用已经存在的一个或多个对象，但是不能进行继承已匹配它的接口
- 注意事项
    - 与代理模式的区别，代理模式是不改变原接口适配，是原接口不符合规范

#### 代码实战

```js

function pp() {
        this.test = function () {
            console.log('我是新test')
        }
    }
    pp.prototype.gogo = function (first_argument) {
        console.log('我是新gogo')
    };
    function shipeiqi() {
        var s = new pp;
        aa = {
            test: function () {
                s.test();
            },
            go: function () {
                s.gogo()
            }
        };
        return aa;
    }
    var aa = shipeiqi();
    aa.test();
    aa.go();

```

### 职责链模式

#### 概念解读

> 职责链模式是使多个对象都有机会处理请求，从而避免请求的发送者和接受者之间的耦合关系。
将这个对象连成一条链，并沿着这条链传递该请求，直到有一个对象处理他为止。
链中收到请求的对象要么亲自处理它，要么转发给下一个候选者。提交方并不明确有多少个对象会处理它，
任一候选者都可以响应相应的请求，可以在运行时刻决定哪些候选者参与到链中。

#### 作用和注意事项

- 作用
    - dom的冒泡有些类似职责链
    - nodejs 当controller中有很多负责操作逻辑的时候拆分中间件
    - 解耦发送者和接受者
- 注意事项
    - javascript中的每一次【.】是有代价的，要在必要的时候应用

#### 代码实战

```js

function laoban (xiangmujingli) {
        if(xiangmujingli) {
            this.xiangmujingli = xiangmujingli;
        }
    }
    laoban.prototype.write = function (php) {
        this.xiangmujingli.write(php);
    };
    function xiangmujingli(coder) {
        if(coder) {
            this.coder = coder;
        }
    }
    xiangmujingli.prototype.write = function (php) {
        this.coder.write(php);
    };
    function coder(php) {
        this.write(php);
    }
    coder.prototype.write = function (php) {
        console.log('coding...' + php);
    }
    var begin = new laoban(new xiangmujingli(new coder()));
    begin.write('php');

```