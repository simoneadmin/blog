调用sonar API 接口，地址如下：
http://sonar.*****.com/api/measures/search_history?component=tcmp-devops-service&metrics=sqale_index%2Cduplicated_lines_density%2Cncloc%2Ccoverage%2Cbugs%2Ccode_smells%2Cvulnerabilities&ps=1000

url上出现乱码符号，不清楚乱码代表什么意思，整理如下对应关系：

字符 - URL编码值
空格 - %20

" - %22

# - %23

% - %25

& - %26

( - %28

) - %29

+ - %2B

, - %2C

/ - %2F

: - %3A

; - %3B

< - %3C

= - %3D

> - %3E

? - %3F

@ - %40

- %5C

| - %7C

URL特殊字符转义，URL中一些字符的特殊含义，基本编码规则如下：
1、空格换成加号(+)

2、正斜杠(/)分隔目录和子目录

3、问号(?)分隔URL和查询

4、百分号(%)制定特殊字符

5、#号指定书签

6、&号分隔参数

对于包含中文的Url的处理问题，不同浏览器有不同的表现。例如对于IE，如果你勾选了高级设置“总是以UTF-8发送Url”，那么Url中的路 径部分的中文会使用UTF-8进行Url编码之后发送给服务端，而查询参数中的中文部分使用系统默认字符集进行Url编码。为了保证最大互操作性，建议所有放到Url中的组件全部显式指定某个字符集进行Url编码，而不依赖于浏览器的默认实现。
