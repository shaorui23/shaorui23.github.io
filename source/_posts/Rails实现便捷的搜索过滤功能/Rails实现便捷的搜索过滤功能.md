---
layout: post
title: Rails实现便捷的搜索过滤功能
date: 2016-07-14 08:29:44
tags:
---


## Rails实现便捷的搜索过滤功能


      
搜索是Rails 开发中必不可少的拓展功能。常见的<a href="https://www.ruby-toolbox.com/categories/rails_search" target="_blank" rel="external">带有搜索功能的gem</a>有很多，例如Redis-serach, Sunspot 和 Elasticsearch。但是在普通的Rails 应用中引入他们难免过于庞大，如果只是为了实现简单的搜索功能，他们就有些大材小用了。

Rails 自带 <a href="http://guides.rubyonrails.org/active_record_querying.html#scopes" target="_blank" rel="external">scopes</a> 功能，能够提供可拓展性很强的功能便于你使用搜索过滤等需求。</p>

### 使用Scopes-构建搜索功能

假设应用有一个Article 的 Model，前台可以支持搜索。可以通过输入名字，选择版本，按是否已经出版来找到想要的文章。

```
class Article < ActiveRecord::Base
  scope :name, -> (name) { where("name like ?", "%#{name}%")}
  scope :version, ->(version) { where version: version }
  scope :published, -> { where(published: true) }
end
```

用scope chain 组合成你要的结果：

```
class Article < ActiveRecord::Base
  scope :name, -> (name) { where("name like ?", "%#{name}%")}
  scope :version, ->(version) { where version: version }
  scope :published, -> { where(published: true) }
end
```

然后在controller 里面就可以构建action：

```
def  index
  @articles = Article.scoped
  @articles = @articles.published if params[:published].present?
  @articles = @articles.name(params[:name]) if params[:name].present?
  @articles = @articles.version(params[:version]) if params[:version].present?
  @articles
end
```

到这一步我们就构建好了基本搜索功能了。
<hr>

Article:Index 方法里面为了过滤出想要的article数据，明显用了好多可以重复的代码。这里我们可以抽出来复用：

```
module Searchable
  extend ActiveSupport::Concern
  module ClassMethods
    def search(filtering_params)
      results = self.scoped
      filtering_params.each do |key, value|
        results = results.send(key, value) if value
      end
      results
    end
  end
end
```

然后在Model 里面include：

```
class Article
  include Searchable
end
```

在Controller里就可以简单的调用了：

```
class Article
  include Searchable
end
```

<hr>
关于为什么在module里面引用：<code>extend ActiveSupport::Concern</code>. 可以看<a href="https://github.com/rails/rails/blob/master/activesupport/lib/active_support/concern.rb" target="_blank" rel="external">源代码</a>，另外还有一篇文章详细讲述了原理：<a href="https://ihower.tw/blog/archives/3949" target="_blank" rel="external">深入Rails3: ActiveSupport::Concern</a></p>

>  ActiveSupport::Concern 就是來幫助解決這個難題，我們希望宿主可以不需要知道 modules 之間的 dependencies 關係。dependencies 關係寫在 module 裡面就好了。