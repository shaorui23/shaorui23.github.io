title: 'Ruby: 对象模型例子'
date: 2015-06-05 17:29:27
category: ruby
tags: [tech ruby]
---

重温《Ruby 元编程》，里面有个对象模型小结的例子值得分享下。

```
module Printable
  def print
    puts "Printable: print"
  end

  def prepare_cover
    puts "Printable: prepare_cover"
  end
end

module Document
  def print_to_screen
    puts "Document: print_to_screen"
    prepare_cover
    format_for_screen
    print
  end

  def format_for_screen
    puts "Document: format_for_screen"
  end

  def print
    puts "Document: print"
  end
end

class Book
  include Document
  include Printable
end

b = Book.new
b.print_to_screen
```

起初以为答案应该是：
```
Document: print_to_screen
Printable: prepare_cover
Document: format_for_screen
Document: print
```

事实上搞错了一个include 包含模块的一个道理是：这个模块会被插入到祖先链中，__位置就在类的正上方__。所以：
```
irb: Book.ancestors
[Book, Printable, Document, Object, PP::ObjectMixin, Kernel, BasicObject]
```

正确答案应该是：
```
Document: print_to_screen
Printable: prepare_cover
Document: format_for_screen
Printable: print
```

几点需要注意的对象模型小结：

> 1.  调用方法时，Ruby 首先向右找到接受者所属的类，然后在向上查找祖先链，知道找到该方法或到达顶链为止。
> 2.  定义一个模块时，该模块会扮演self的角色。
