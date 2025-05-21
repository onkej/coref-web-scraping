Website Types and Scraping Approaches
======

**Table of Contents**
- [Website Types and Scraping Approaches](#website-types-and-scraping-approaches)
  - [JSON-based Parsing](#json-based-parsing)
    - [Ctrip 携程](#ctrip-携程)
    - [CEnews 中国环境](#cenews-中国环境)
    - [People 人民网](#people-人民网)
  - [URL-based Parsing](#url-based-parsing)
    - [Query String with `?`](#query-string-with-)
      - [Weibo 微博](#weibo-微博)
      - [MEE 生态环境部](#mee-生态环境部)
    - [Query String without `?`](#query-string-without-)
      - [Anjuke 安居客](#anjuke-安居客)

Websites use different content loading mecanisms. Accordingly, there're two types of scraping methods: JSON-based vs. URL-based.

## JSON-based Parsing
This approach is recommended on modern websites built with AJAX/API infrastructure (React/Vue/Angular), where **pagination and filter changes are not reflected in the URLs**.

Since JSON is already structured data that can be directly processed without HTML parsing, it is strongly recommended when possible. This way we don't have to navigate complex HTML tree any more. 

APIs often provide more complete and consistent data than what's visible on the webpage


### Ctrip 携程
Ctrip provides both mobile and PC pages, which apply different loading mecanism: ***infinite scrolling*** on mobile version, and ***pagination without URL changes*** on PC version. Both can be scraped using API requests, but I guess the mobile one allows to load more comments than the PC one (`10 comments * 300 pages = 3000`, at most).

The APIs can be found in Dev Tool's **Network** details.

### CEnews 中国环境

### People 人民网



## URL-based Parsing
This approach is used on traditional websites, where pagination and filter applications will be marked somewhere in the URL in some way.

### Query String with `?`
A typical and comprehensible way is using
**query string** with `?`, followed by optional `key=value` pairs (filters).  

For some, like [**Weibo**](#weibo-微博), the queried URL could be the same as the one shown in browser's search bar. 

But for others, e.g. [**MEE**](#mee-生态环境部), the queries URL could be hidden in the **Network** details in Dev Tool.

#### Weibo 微博 

`https://s.weibo.com/weibo?q=生态文明&scope=ori&haspic=1&timescope=custom:2025-05-01-0:2025-05-21-0&Refer=g`

`https://s.weibo.com/weibo?q=生态文明&typeall=1&haspic=1&timescope=custom:2025-05-01-0:2025-05-21-0&Refer=g`

`https://s.weibo.com/weibo?q=#生态文明#&haspic=1&timescope=custom:2025-05-01-0:2025-05-21-0&Refer=g`

```
q:              生态文明
typeall:        1 (type=all, default)
scope:          ori (type=original 原创)
haspic:         1 (has picture=True)
timescope:      custom:2025-05-01-0:2025-05-21-0
Refer:          g (not sure what it means)
```


#### MEE 生态环境部

`https://www.mee.gov.cn/was5/web/search?channelid=270514&searchword=&page=1&orderby=-docreltime&searchscope=&timestart=2008.01.01&timeend=2025.05.21&period=&chnls=11&andsen=&total=生态文明&orsen=&exclude=`

```
channelid:      270514
searchword:	
page:           1
orderby:        -docreltime
searchscope:
timestart:      2008.01.01
timeend:        2025.05.21
period:     	
chnls:          11
andsen:         
total:          生态文明
orsen:      
exclude:    
```



### Query String without `?`
Another way is more obscure and, at least in Chinese context, more arbitrary.

#### Anjuke 安居客
`https://zh.zu.anjuke.com/fangyuan/gaoxinququ-q-tangjiawan/zj204-fx1-x1-tw2-tj1-dtf1-lx1/`

```
zh:         珠海
zu:         租 (apartments for rental)
gaoxinququ: 区 = 高新区
tangjiawan: 镇 = 唐家湾
zj204:      租金在 1000-1500 之间 (4th place)
fx1:        房型 = 一室 (1st place)
x1:         出租类型 = 整租 (1st place)
tw2:        朝向 = 朝南 (towards) (2nd place)
tj1:        展示被推荐的房源 = True (show recommended ones)
dtf1:       电梯房 = True
lx1:        房屋类型 = 普通住宅 (1st place)
```


