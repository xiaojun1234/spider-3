# spider-3
# 正则表达式实践
## re.search 和 re.match 一样，但比re.match好用，它不需要从头匹配
# 最常规的匹配
<pre>
import re
content = 'Hello 123 4567 World_This is a Regex Demo'
print(len(content))
result = re.match('^Hello\s\d\d\d\s\d{4}\s\w{10}.*Demo$',content)
print(result)
print(result.group())
print(result.span())
</pre>
# 匹配目标
<pre>
import re
content = 'Hello 123 4567 World_This is a Regex Demo'
result = re.match('^Hello\s(\d+)\sWorld.*Demo$',content)
print(result)
print(result.group())
print(result.group(1))   /*第一个括号的值')*/
</pre>
# 贪婪匹配(下例，第一个括号只匹配到‘7’)
<pre>
import re
content = 'Hello 123 4567 World_This is a Regex Demo'
result = re.match('^He.*(\d+).*Demo$')
print(result)
print(result.group(1))
</pre>
# 非贪婪匹配(则加'?')
<pre>
import re
content = 'Hello 123 4567 World_This is a Regex Demo'
result = re.match('^He.*?(\d+).*Demo$')
print(result.group(1))
</pre>
# ‘.’代替不了换行问题(使用re.S)
<pre>
import re
content = ''Hello 123 4567 World_This 
is a Regex Demo''
result = re.match('^He.*(\d+).*Demo$',content,re.S)
print(result.group(1))
</pre>
# 转义
<pre>
import re
content = 'price is $5.00'
result = re.match('price is \$5\.00',content)
print(result)

re.search 只能匹配一个，re.findall 可以匹配多个
result = re.findall('(<a.*?>)?')     /* ‘?’这样可以理解为有或没有，输出只输出括号的内容*/
</pre>
# re.sub  匹配替换字符串
<pre>
import re
content = 'Hello 1234567 World_This is a Regex Demo'
content = re.sub('\d+','',content)
print(content)
</pre>
#利用re.sub插入参数
<pre>
import re
content = 'Hello 1234567 World_This is a Regex Demo'
content = re.sub('\d+',r'\1 8910',content)    /*（\1）是转义，r是原字符不变 */
print(content)
</pre>
# 利用re.sub删除一些参数(实例)
<pre>
import re
html = '''<dir id="songs-llist">
    <h2 class="title">经典老歌</h2>
    <p class="introduction">
        经典老歌列表
    </p>
    <ul id="list" class="list-group">
        <li date-view="2">一路上有你</li>
        <li date-view="7"><a href="/3.mp3" singer="任贤齐">沧海一声笑</a></li>
        <li date-view="7" class="active">
            <a href="/4.mp3" singer="齐秦">往事随风</a>
        </li>
        <li date-view="7"><a href="/5.mp3" singer="家驹">光辉岁月</a></li>
        <li date-view="7"><a href="/6.mp3" singer="陈慧琳">记事本</a></li>
        <li date-view="7">
            <a href="/7.mp3" singer="邓丽君">但愿人长久</a>
        </li>
    </ul>
</dir>'''
html = re.sub('`<a.*?>`|`</a>`','',html)
print(html)
results = re.findall('<li.*?>(.*?)</li>',html,re.S)
print(results)
for result in results:
    print(result.strip())      /*去掉换行符*/
</pre>
# re.compile将正则表达式转换为对象
<pre>
import re
content = "Hello 123445 is Demo"
pattern = re.compile('hello.*Demo',re.S)
result = rematch(pattern,content)
print(result)
</pre>
# 实战练习
<pre>
import requests
import re
response = request.get('https://book.douban.com/').text
pattern = re.compile('<li.*?cover.*?href="(.*?)".*?title="(.*?)".*?more-meta.*?author">(.*?)</span>.*?year">(.*?)</span>.*?</li>',re.S)
results = re.findall(pattern,response)
for result in results:
    url,name,author,date = result
    author = re.sub('\s','',author)
    date = re.sub('\s','',date)
    print(url,name,author,date)
</pre>
