## jQuery 拓展

#### jQuery插件

 
  1, 类级别的插件开发
         
  
     1-1,添加一个全局函数 
        jQuery.foo = function () {
            console.log('Hello World');
        }
            $.foo();    Hello World
         jQuery.foo();  Hello World

    1-2,使用jQuery.extend(object)添加全局函数
        jQuery.extend({
             name:"kevin",
            bar: function () {
                console.log('bar function running...')
            }
        });

        $.bar();bar function running...

    1-3,使用命名空间
        jQuery.MyFun = {
            joo: function () {
                console.log('joo fun running...')
            }
        }

        $.MyFun.joo();joo fun running...

      
2,对象级别的插件开发
       

        (function ($) {
            $.fn.MyPlugin = function () {
                console.log('object')
            }
        })(jQuery)

        $("#box").MyPlugin();object

      
例如： 
      

        (function ($) {
            $.fn.MyHighLight = function (options) {
                var _default = {
                    foreground: "red",
                    background: "yellow"
                }
                var ops = $.extend(_default, options);
                var _this = this;
                 样式内容都是内联的  style="background-color: red; font-size: 25px;"
                var obj = {
                    "background-color": ops.foreground,
                    "font-size": "25px"
                }
                $(_this).css(obj);
            }
        })(jQuery)

        $("#box").MyHighLight({ "foreground": "blue" });

       
3, jquery 为插件开发提供了两个方法:
######
jQuery.extend(object);---  为jQuery类添加类方法，可以理解为添加静态方法,jQuery.fn.extend(object);---对jQuery.prototype进得扩展，就是为jQuery类添加“成员函数”。 其中 jQuery.fn = jQuery.prototype
         

        jQuery.extend({
            max: function (a, b) { return a > b ? a : b },
            min: function (a, b) { return a > b ? b : a }
        });

        console.log($.max(2, 4));--4

        jQuery.fn.extend({
            log: function () { console.log('object  is  running ....') }
        });

        $("#box").log(); object  is  running ....


        
4, $.extend()方法的几种处理方式
         

    $.extend(item,item2,item3)

        var item1 = { name: "kevin", age: 23, sex: "男" };
        var item2 = { name: "tom", age: 11, address: "深圳市罗湖区" };
        var item = $.extend(item1,item2);
        console.log(item); {name: "tom", age: 11, sex: "男", address: "深圳市罗湖区"}
        console.log(item1);{name: "tom", age: 11, sex: "男", address: "深圳市罗湖区"}

        var item = $.extend({}, item1, item2);
        console.log(item);{name: "tom", age: 11, sex: "男", address: "深圳市罗湖区"}
        console.log(item1){ name: "kevin", age: 23, sex: "男" };

         var item= $.extend(true,{},item1,item2);
        console.log(item);{name: "tom", age: 11, sex: "男", address: "深圳市罗湖区"}


         var item = $.extend(true, item1, item2);item1 item2 
         //不做关联，----item1与item浅拷贝
         console.log(item);{name: "tom", age: 11, sex: "男", address: "深圳市罗湖区"}
         console.log(item1);{name: "tom", age: 11, sex: "男", address: "深圳市罗湖区"}
         console.log(item2); { name: "tom", age: 11, address: "深圳市罗湖区" }

        var item =   $.extend(false, item1, item2); 
         //item item1 item2  都不做关联--此为深拷贝
         console.log(item);{name: "tom", age: 11, sex: "男", address: "深圳市罗湖区"}
         console.log(item1);{name: "kevin", age: 23, sex: "男"}
         console.log(item2);{name: "tom", age: 11, address: "深圳市罗湖区"}