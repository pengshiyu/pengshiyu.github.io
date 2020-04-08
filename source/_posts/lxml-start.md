---
title: lxml模块常用体系结构
date: 2020-04-08 10:25:24
tags:
    - lxml
categories:
    - 爬虫
---

lxml模块可以对html文档进行增、删、改、查，可以很方便的处理网页

<!-- more -->

lxml模块常用体系结构：
```python
lxml.etree
    class
        XMLParser
        XMLPullParser   
        HTMLParser
        HTMLPullParser
        XPath

    function
        Comment(text=None)
        Element(_tag, attrib=None, nsmap=None, **_extra)
        ElementTree(element=None, file=None, parser=None)
        
        # 解析字符串
        HTML(text, parser=None, base_url=None)
        XML(text, parser=None, base_url=None)
        fromstring(text, parser=None, base_url=None)
        parse(source, parser=None, base_url=None)
        
        # 序列化
        tostring(element_or_tree, encoding=None, method="xml", xml_declaration=None, pretty_print=False, with_tail=True, standalone=None, doctype=None, exclusive=False, inclusive_ns_prefixes=None, with_comments=True, strip_text=False, )
        tounicode(element_or_tree, method="xml", pretty_print=False, with_tail=True, doctype=None)
        
        # 默认解析器lxml.etree.XMLParser
        get_default_parser() 
        set_default_parser(parser=None)
        
        # 修剪
        strip_attributes(tree_or_element, *attribute_names)
        strip_elements(tree_or_element, with_tail=True, *tag_names)
        strip_tags(tree_or_element, *tag_names)


lxml.etree._Element
    https://lxml.de/api/lxml.etree._Element-class.html
    
    addnext(self, element)
    getnext(self)
    addprevious(self, element)  
    getprevious(self)

    append(self, element)
    insert(self, index, element)
    clear(self, keep_tail=False)
    remove(self, element)
    replace(self, old_element, new_element)

    extend(self, elements)  
    find(self, path, namespaces=None)
    findall(self, path, namespaces=None)    
    findtext(self, path, default=None, namespaces=None)
    
    getchildren(self)
    getparent(self)
    getroottree(self)   
    index(self, child, start=None, stop=None)
    
    makeelement(self, _tag, attrib=None, nsmap=None, **_extra)
    
    # 属性操作
    items(self)
    keys(self)
    get(self, key, default=None)
    set(self, key, value)
    values(self)
    
    # 选择器
    cssselect(...)
    xpath(self, _path, namespaces=None, extensions=None, smart_strings=True, **_variables)
    
    # 属性
    attrib get(), set(), keys(), values() and items()
    base
    tag
    tail
    text


lxml.etree.ElementTree
    __init__(self, element=None, file=None)
    find(self, path, namespaces=None)
    findall(self, path, namespaces=None)
    findtext(self, path, default=None, namespaces=None)     
    getiterator(self, tag=None)
    getroot(self)
    iter(self, tag=None)
    iterfind(self, path, namespaces=None)   source code
    parse(self, source, parser=None)    source code
    write(self, file_or_filename, encoding=None, xml_declaration=None, default_namespace=None, method=None) source code
    write_c14n(self, file)



lxml.http
    https://lxml.de/api/lxml.html-module.html
    class
        HTMLParser
        HtmlComment
        HtmlElement
    
    function
        document_fromstring(html, parser=None, ensure_head_body=False, **kw)    source code
        fragment_fromstring(html, create_parent=False, base_url=None, parser=None, **kw)
        Element(*args, **kw)
        parse(filename_or_url, parser=None, base_url=None, **kw)
        open_in_browser(doc, encoding=None)
        fromstring(html, base_url=None, parser=None, **kw)
        tostring(doc, pretty_print=False, include_meta_content_type=False, encoding=None, method='html', with_tail=True, doctype=None)


lxml.html.html5parser:
    class   
        HTMLParser
    function
        fromstring(html, guess_charset=None, parser=None)

class HtmlElement(_Element)
    cssselect(self, expr, translator='html')
    set(self, key, value=None)
```

平时用的多的是html文档处理，所以下面具体看下html相关内容
1、element <-> string 之间转换
```python
from lxml import html

text = '<div><span><p>内容</p></span></div>'

# 文档方式
root1 = html.document_fromstring(text)
print(html.tostring(root1, encoding="unicode"))
# <html><body><div><span><p>内容</p></span></div></body></html>

# 片段方式
root2 = html.fragment_fromstring(text)
print(html.tostring(root2, encoding="unicode"))
# <div><span><p>内容</p></span></div>

```

2、助手函数
```python
# 为了方便查看结果，统一使用输出函数
def log_html(element):
    print(html.tostring(element, encoding='unicode', pretty_print=True))


# 在浏览器中打开（调试用）
html.open_in_browser(root)
```

3、创建元素
```python

# 创建元素 添加属性
root = html.Element('div', {'width': '23px'})
root.text = "根元素"
root.tail = "元素后面的文本"
root.base = "www.baidu.com"

log_html(root)
# <div width="23px" xml:base="www.baidu.com">根元素</div>元素后面的文本

# 获取属性
print(root.attrib)  
# {'width': '23px', '{http://www.w3.org/XML/1998/namespace}base': 'www.baidu.com'}
print(root.text)    # 根元素
print(root.tag)     # div
print(root.tail)    # 元素后面的文本
print(root.base)   # None


# 元素属性
print(root.keys())
# ['width']

print(root.values())
# ['23px']

print(root.items())
# [('width', '23px')]

root.set('height', '300px')
print(root.get('height'))
# 300px

root.attrib.pop('height')

```

4、修改元素增加、删除、替换
```python
# 创建元素
root = html.Element('div')

# 添加子节点
p = html.Element('p')

# 追加一个元素
root.append(p)

# 追加多个元素
root.extend([html.Element('span1'), html.Element('span2')])

# 插入元素
root.insert(0, html.Element('span3'))
root.insert(2, html.Element('span4'))
# log_html(root)
"""
<div>
    <span3></span3>
    <p></p>
    <span4></span4>
    <span1></span1>
    <span2></span2>
</div>
"""

# 选择并移除节点
root.remove(root.xpath("//span1")[0])
root.remove(root.cssselect("span2")[0])
root.remove(root.xpath("//span3")[0])

# 替换节点
root.replace(root.xpath("//span4")[0], html.Element('span5'))

# 添加兄弟节点
p.addprevious(html.Element('span', text='前面'))
p.addnext(html.Element('span', text='后面'))
log_html(root)
"""
<div>
<span text="前面"></span>
<p></p>
<span text="后面"></span>
<span5></span5>
</div>
"""

# 清除所有内容
root.clear()
log_html(root)
# <div></div>

# 添加注释
root.append(html.HtmlComment("注释"))
log_html(root)
# <div><!--注释--></div>
```

5、元素查找
```python
# 创建元素
text = '<div><p>p中的内容<span>span1的内容</span><span width="200px">span2的内容</span></p></div>'
root = html.fragment_fromstring(text)

# css 或 xpath查询元素
print(root.cssselect('span'))
# [<Element span>, <Element span>]

print(root.xpath('//span'))
# [<Element span >, <Element span >]

# 查找子元素
print(root.find('.//span'))
# <Element span at>

print(root.findall('.//span'))
# [<Element span at>, <Element span at>]

print(root.findtext('.//span'))
# span1的内容

# 子元素
print(root.getchildren())
# [<Element p at >]

# 父元素
span = root.xpath('//span[@width="200px"]')[0]
print(span.getparent())
# <Element p at>

# 兄弟元素
print(span.getnext())
# None

print(span.getprevious().text)
# span1的内容

# 获取根元素
print(log_html(span.getroottree()))
"""
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN" "http://www.w3.org/TR/REC-html40/loose.dtd">
<html><body><div><p>p中的内容<span>span1的内容</span>
<span width="200px">span2的内容</span></p></div></body></html>
"""

# 查找子元素位置
print(span.getparent().index(span))
# 1

```

6、etree提供的功能函数
```python

from lxml import html, etree

# 创建元素
text = '<div><p width="200px">p中的内容<span width="200px">span的内容</span></p>p后的内容</div>'
root = html.fragment_fromstring(text)

# 移除属性
etree.strip_attributes(root, ['width'])
log_html(root)
# <div><p>p中的内容<span>span的内容</span>span后的内容</p></div>

# 元素和子元素
# etree.strip_elements(root, ['p'], with_tail=True)
# log_html(root)
# <div></div>

# 移除元素标签，保留子元素
etree.strip_tags(root, ['p'])
log_html(root)
# <div>p中的内容<span>span的内容</span>p后的内容</div>

```

>参考
[https://lxml.de/api/](https://lxml.de/api/)
