---
layout: post
title: "Octopress插件使用"
date: 2018-10-26 12:17:16 +0800
comments: true
categories: 插件
---

# octopress插件使用

## codeblock插件
{% codeblock %}
Awesome code snippet
{% endcodeblock %}

<!-- more -->

> Last night I lay in bed looking up at the stars in the sky and I thought to myself, where the heck is the ceiling.

{% blockquote %}
Last night I lay in bed looking up at the stars in the sky and I thought to myself, where the heck is the ceiling.
{% endblockquote %}


### 发邮件反馈插件
* https://getsimpleform.com/

### 集成图表
* http://mostlyblather.com/blog/2015/05/23/mermaid-jekyll-octopress/

### octopress转到hexo
* https://changchen.me/blog/20180807/octopress-to-hexo/

### octopress集成OTC
* https://github.com/dafi/jekyll-toc-generator

#### 安装nokogiri
1. To use tocGenerator.rb you need [nokogiri](http://www.nokogiri.org/)
2. copy the file tocGenerator.rb into the _plugins folder
3. copy the file css/toc.css to you css site and include into _layouts (this is recommended but not necessary)
4. finished

#### 如何使用

```html source/_layout/default.html
<!DOCTYPE html>
<html>
  <head>
    <meta charset='utf-8' />
    <title>{{ page.title }}</title>
    <!-- This css contains the default style for TOC -->
    <link rel="stylesheet" type="text/css" media="screen" href="css/toc.css">
  </head>
  <body>
    <div>
        {{ content | toc_generate }}
    </div>
  </body>
</html>
```

### 安装category插件
- 创建文件 *category_list.rb*，放在pluginss目录下

```ruby plugins/category_list.rb
module Jekyll

  class CategoryCloud < Liquid::Tag

    def initialize(tag_name, markup, tokens)
      @opts = {}
      if markup.strip =~ /\s*counter:(\w+)/iu
        @opts['counter'] = ($1 == 'true')
        markup = markup.strip.sub(/counter:\w+/iu,'')
      end
      super
    end

    def render(context)
      lists = {}
      max, min = 1, 1
      config = context.registers[:site].config
      category_dir = config['root'] + config['category_dir'] + '/'
      categories = context.registers[:site].categories
      categories.keys.sort_by{ |str| str.downcase }.each do |category|
        count = categories[category].count
        lists[category] = count
        max = count if count > max
      end

      html = ''
      lists.each do | category, counter |
        url = category_dir + category.to_url.gsub(/_|\P{Word}/u, '-').gsub(/-{2,}/u, '-').downcase
        style = "font-size: #{100 + (60 * Float(counter)/max)}%"
        html << "<a href='#{url}' style='#{style}'>#{category}"
        if @opts['counter']
          html << "(#{categories[category].count})"
        end
        html << "</a> "
      end
      html
    end
  end

  class CategoryList < Liquid::Tag

    def initialize(tag_name, markup, tokens)
      @opts = {}
      if markup.strip =~ /\s*counter:(\w+)/iu
        @opts['counter'] = ($1 == 'true')
        markup = markup.strip.sub(/counter:\w+/iu,'')
      end
      super
    end

    def render(context)
      html = ""
      config = context.registers[:site].config
      category_dir = config['root'] + config['category_dir'] + '/'
      categories = context.registers[:site].categories
      categories.keys.sort_by{ |str| str.downcase }.each do |category|
        url = category_dir + category.to_url.gsub(/_|\P{Word}/u, '-').gsub(/-{2,}/u, '-').downcase
        html << "<li><a href='#{url}'>#{category}"
        if @opts['counter']
          html << " (#{categories[category].count})"
        end
        html << "</a></li>"
      end
      html
    end
  end

end

Liquid::Template.register_tag('category_cloud', Jekyll::CategoryCloud)
Liquid::Template.register_tag('category_list', Jekyll::CategoryList)
```

- 在目录*source/_includes/custom/asides* 创建*category_list.html*

{% codeblock lang:html category_list.html %}
<section>
  <h1>Categories</h1>
  <ul id="category-list">{ % category_list counter:true % }</ul>
</section>
{% endcodeblock %}

- 修改根目录下的*_config.yml*文件

```yml _config.yml
default_asides: [..., custom/asides/category_list.html]

```

- 在目录*blog/categories/* 创建 *all.html*

{% codeblock lang:html all.html %}
---
layout: page
title: "categories"
footer: false
---

{ % category_list [counter:true] % }
{% endcodeblock %}

- 修改目录*source/_includes/custom/navigation.html*文件，增加一个分类入口 

```html source/_includes/custom/navigation.html
<ul class="main-navigation">
  <li><a href="{{ root_url }}/">Blog</a></li>
  <li><a href="{{ root_url }}/blog/archives">Archives</a></li>
  <!-- 增加分类入口 -->
  <li><a href="{{ root_url }}/blog/categories/all.html">Categories</a></li>
</ul>
```