4. 格式
------------

.. _line-length:

4.1. 行长度
~~~~~~~~~~~~~~~~~~~~~~~~~~

.. tip::
    每一行代码字符数不超过 80.
    
我们也认识到这条规则是有争议的, 但很多已有代码都已经遵照这一规则, 我们感觉一致性更重要.

优点:
    提倡该原则的人主张强迫他们调整编辑器窗口大小很野蛮. 很多人同时并排开几个代码窗口, 根本没有多余空间拉伸窗口. 大家都把窗口最大尺寸加以限定, 并且 80 列宽是传统标准. 为什么要改变呢?
    
缺点:
    反对该原则的人则认为更宽的代码行更易阅读. 80 列的限制是上个世纪 60 年代的大型机的古板缺陷; 现代设备具有更宽的显示屏, 很轻松的可以显示更多代码.
    
结论:
    80 个字符是最大值.
    
    特例:
    
    - 如果一行注释包含了超过 80 字符的命令或 URL, 出于复制粘贴的方便允许该行超过 80 字符.
    - 包含长路径的 ``#include`` 语句可以超出80列. 但应该尽量避免.
    - :ref:`头文件保护 <define_guard>` 可以无视该原则.
    

4.2. 空格还是制表位
~~~~~~~~~~~~~~~~~~~~~~~~~~

.. tip::
    只使用空格, 每次缩进 2 个空格.
    
我们使用空格缩进. 不要在代码中使用制符表. 你应该设置编辑器将制符表转为空格.

4.3. 函数声明与定义
~~~~~~~~~~~~~~~~~~~~~~~~~~

.. tip::
    返回类型和函数名在同一行, 参数也尽量放在同一行.
    
函数看上去像这样:
    .. code-block:: c++
        
        ReturnType ClassName::FunctionName(Type par_name1, Type par_name2) {
            DoSomething();
            ...
        }
    
如果同一行文本太多, 放不下所有参数:
    .. code-block:: c++
        
        ReturnType ClassName::ReallyLongFunctionName(Type par_name1,
                                                     Type par_name2,
                                                     Type par_name3) {
            DoSomething();
            ...
        }
    
甚至连第一个参数都放不下:
    .. code-block:: c++
        
        ReturnType LongClassName::ReallyReallyReallyLongFunctionName(
                Type par_name1,  // 4 space indent
                Type par_name2,
                Type par_name3) {
            DoSomething();  // 2 space indent
            ...
        }
    
注意以下几点:

    - 返回值总是和函数名在同一行;

    - 左圆括号总是和函数名在同一行;

    - 函数名和左圆括号间没有空格;

    - 圆括号与参数间没有空格;

    - 左大括号总在最后一个参数同一行的末尾处;

    - 右大括号总是单独位于函数最后一行;

    - 右圆括号和左大括号间总是有一个空格;

    - 函数声明和实现处的所有形参名称必须保持一致;

    - 所有形参应尽可能对齐;

    - 缺省缩进为 2 个空格;

    - 换行后的参数保持 4 个空格的缩进;

如果函数声明成 ``const``, 关键字 ``const`` 应与最后一个参数位于同一行:=
    .. code-block:: c++
    
        // Everything in this function signature fits on a single line
        ReturnType FunctionName(Type par) const {
          ...
        }
        
        // This function signature requires multiple lines, but
        // the const keyword is on the line with the last parameter.
        ReturnType ReallyLongFunctionName(Type par1,
                                          Type par2) const {
          ...
        }
        
如果有些参数没有用到, 在函数定义处将参数名注释起来:
    .. code-block:: c++
        
        // Always have named parameters in interfaces.
        class Shape {
         public:
          virtual void Rotate(double radians) = 0;
        }
        
        // Always have named parameters in the declaration.
        class Circle : public Shape {
         public:
          virtual void Rotate(double radians);
        }
        
        // Comment out unused named parameters in definitions.
        void Circle::Rotate(double /*radians*/) {}
    
    .. warning::
        .. code-block:: c++
            
            // Bad - if someone wants to implement later, it's not clear what the
            // variable means.
            void Circle::Rotate(double) {}


4.4. 函数调用
~~~~~~~~~~~~~~~~~~~~~~~~~~

.. tip::
    尽量放在同一行, 否则, 将实参封装在圆括号中.
    
函数调用遵循如下形式:
    .. code-block:: c++
        
        bool retval = DoSomething(argument1, argument2, argument3);
        
如果同一行放不下, 可断为多行, 后面每一行都和第一个实参对齐, 左圆括号后和右圆括号前不要留空格:
    .. code-block:: c++
        
        bool retval = DoSomething(averyveryveryverylongargument1,
                                  argument2, argument3);
                                  
如果函数参数很多, 出于可读性的考虑可以在每行只放一个参数:
    .. code-block:: c++
        
        bool retval = DoSomething(argument1,
                                  argument2,
                                  argument3,
                                  argument4);
                                  
如果函数名非常长, 以至于超过 :ref:`行最大长度 <line-length>`, 可以将所有参数独立成行:
    .. code-block:: c++
        
        if (...) {
          ...
          ...
          if (...) {
            DoSomethingThatRequiresALongFunctionName(
                very_long_argument1,  // 4 space indent
                argument2,
                argument3,
                argument4);
          }


4.5. 类格式
~~~~~~~~~~~~~~~~~~~~~~~~~~

.. tip::
    访问控制块的声明依次序是 ``public:``, ``protected:``, ``private:``, 每次缩进 1 个空格.
    
类声明 的基本格式如下:
    .. code-block:: c++
        
        class MyClass : public OtherClass {
         public:      // Note the 1 space indent!
          MyClass();  // Regular 2 space indent.
          explicit MyClass(int var);
          ~MyClass() {}
            
          void SomeFunction();
          void SomeFunctionThatDoesNothing() {
          }
            
          void set_some_var(int var) { some_var_ = var; }
          int some_var() const { return some_var_; }
            
         private:
          bool SomeInternalFunction();
            
          int some_var_;
          int some_other_var_;
          DISALLOW_COPY_AND_ASSIGN(MyClass);
        };
        
注意事项:
    - 所有基类名应在 80 列限制下尽量与子类名放在同一行.
    
    - 关键词 ``public:``, ``protected:``, ``private:`` 要缩进 1 个空格.
    
    - 除第一个关键词 (一般是 ``public``) 外, 其他关键词前要空一行. 如果类比较小的话也可以不空.
    
    - 这些关键词后不要保留空行.
    
    - ``public`` 放在最前面, 然后是 ``protected``, 最后是 ``private``.
    
    - 关于声明顺序的规则请参考 :ref:`声明顺序 <declaration-order>` 一节.
    
