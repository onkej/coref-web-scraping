Website Types and Scraping Approaches
======

**Table of Contents**
- [Website Types and Scraping Approaches](#website-types-and-scraping-approaches)
  - [How to Know Which Approach to Use? (Answered by Claude 3.7 Sonnet)](#how-to-know-which-approach-to-use-answered-by-claude-37-sonnet)
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

## How to Know Which Approach to Use? (Answered by Claude 3.7 Sonnet)
1. Inspect Network Requests
     - Open browser Dev Tools using ``Fn + F12``, go to the `Network` tab
     - On the website, perform a request by applying a filter or navigate to next page
     - On Dev Tools, click `XHR/Fetch` tab to view the corresponding request that returns data
     - Click that request to view details
  
2. Check Response Types
     - Click `Response` to view the response type:
     - If you see JSON responses with the actual data, use the JSON-based parsing
       - If JSON-based, should also click `Request` to inspect the corresponding parameters controlling pagination and filters 
       - See [examples below](#json-based-parsing)
      
     - If you only see HTML documents, use the URL-based parsing
       - See [examples below](#url-based-parsing)
     

3. Test a Simple Request
     - Try accessing a suspected API endpoint directly
     - If it returns valid JSON with expected data, the JSON-based API approach will work


## JSON-based Parsing
This approach is recommended on modern websites built with AJAX/API infrastructure (React/Vue/Angular), where **pagination and filter changes are not reflected in the URLs**.

Since JSON is already structured data that can be directly processed without HTML parsing, it is strongly recommended when possible. This way we don't have to navigate complex HTML tree any more. 

APIs often provide more complete and consistent data than what's visible on the webpage


### Ctrip 携程
Ctrip provides both mobile and PC pages, which apply different loading mecanism: ***infinite scrolling*** on mobile version, and ***pagination without URL changes*** on PC version. Both can be scraped using API requests, but I guess the mobile one allows to load more comments than the PC one (up to `10 comments/page * 300 pages = 3000 comments`).

The API entry can be found in Dev Tool's **Network**: `https://m.ctrip.com/restapi/soa2/13444/json/getCommentCollapseList?_fxpcqlniredt=09031109310692966540&x-traceID=09031109310692966540-1747919168810-7858839`. 

Use `https://m.ctrip.com/restapi/soa2/13444/json/getCommentCollapseList` for request.

**Request** (Mobile)  
Request in JSON:
```
{
	"arg": {
		"channelType": 7,
		"collapseType": 1,
		"commentTagId": 0,
		"pageIndex": 2,
		"pageSize": 10,
		"pageType": 1,
		"poiId": null,
		"resourceId": 126481,
		"resourceType": 11,
		"sortType": 6,
		"sourceType": 1,
		"starType": 0,
		"videoImageSize": "700_392"
	},
	"contentType": "json",
	"head": {
		"auth": "",
		"cid": "09031109310692966540",
		"ctok": "",
		"cver": "1.0",
		"extension": [
			{
				"name": "source",
				"value": "web"
			},
			{
				"name": "technology",
				"value": "H5"
			},
			{
				"name": "os",
				"value": "PC"
			},
			{
				"name": "application",
				"value": ""
			}
		],
		"lang": "01",
		"sid": "8888",
		"syscode": "09",
		"xsid": ""
	}
}
```

**Response** (Mobile)  
Response in JSON (first comment):
```
{
	"0": {
		"commentId": 207779349,
		"poiInfo": null,
		"userInfo": {
			"userId": 23228959,
			"uid": null,
			"clientAuth": "B8660ADFEB463134FC1D3F250DEFBD30",
			"userNick": "匿名用户",
			"userImage": "https://dimg02.c-ctrip.com/images/1me5m12000babvc40EDFD.png",
			"userMedals": [],
			"userMember": "铂金贵宾",
			"identitiesName": "",
			"userMemberCode": "20",
			"url": "",
			"vIcon": "",
			"identityDesc": "",
			"tag": null,
			"showTagList": null,
			"levelValueIcon": "https://pages.c-ctrip.com/livestream/userpage/ic_level_5.png",
			"urls": {
				"appUrl": null,
				"h5Url": null,
				"wechatUrl": null,
				"mainWechatUrl": null
			}
		},
		"extInfo": {
			"playTime": null,
			"duration": 0,
			"durationType": 0,
			"avgCost": null,
			"airplane": null,
			"seatLevel": null,
			"departure": null,
			"arrival": null
		},
		"replyInfo": [],
		"replyTypeList": null,
		"commentKeywordList": [],
		"commentTagInfo": [],
		"resourceId": 126481,
		"resourceType": 11,
		"businessId": 1834371,
		"businessType": 70,
		"districtId": 22,
		"sourceType": 5,
		"externalResourceId": 1128135034270899,
		"hasVoted": false,
		"isUnUseful": false,
		"showUsefulModule": null,
		"isPicked": false,
		"isGood": false,
		"isOwner": false,
		"fromType": 1,
		"fromTypeText": "来自订单",
		"publishTime": "/Date(1743773025000+0800)/",
		"publishStatus": 6,
		"usefulCount": 0,
		"replyCount": 0,
		"score": {
			"source": "5.0",
			"parsedValue": 5
		},
		"touristType": 2,
		"images": [
			{
				"imageId": 1471448615,
				"height": 2532,
				"width": 1899,
				"imageSrcUrl": "https://dimg04.c-ctrip.com/images/0EQ1e12000jkcs0auEB3E_W_640_10000.jpg?proc=autoorient",
				"imageThumbUrl": "https://dimg04.c-ctrip.com/images/0EQ1e12000jkcs0auEB3E_D_180_180.jpg?proc=autoorient",
				"tagText": null,
				"tagId": null
			},
			{
				"imageId": 1471448624,
				"height": 1899,
				"width": 2532,
				"imageSrcUrl": "https://dimg04.c-ctrip.com/images/0EQ4q12000jkd56tp8EC7_W_640_10000.jpg?proc=autoorient",
				"imageThumbUrl": "https://dimg04.c-ctrip.com/images/0EQ4q12000jkd56tp8EC7_D_180_180.jpg?proc=autoorient",
				"tagText": null,
				"tagId": null
			},
			{
				"imageId": 1471448614,
				"height": 1899,
				"width": 2532,
				"imageSrcUrl": "https://dimg04.c-ctrip.com/images/0EQ6412000jkb9503AEA8_W_640_10000.jpg?proc=autoorient",
				"imageThumbUrl": "https://dimg04.c-ctrip.com/images/0EQ6412000jkb9503AEA8_D_180_180.jpg?proc=autoorient",
				"tagText": null,
				"tagId": null
			},
			{
				"imageId": 1471448623,
				"height": 1899,
				"width": 2532,
				"imageSrcUrl": "https://dimg04.c-ctrip.com/images/0EQ6m12000jkbb8hp116F_W_640_10000.jpg?proc=autoorient",
				"imageThumbUrl": "https://dimg04.c-ctrip.com/images/0EQ6m12000jkbb8hp116F_D_180_180.jpg?proc=autoorient",
				"tagText": null,
				"tagId": null
			},
			{
				"imageId": 1471448613,
				"height": 1899,
				"width": 2532,
				"imageSrcUrl": "https://dimg04.c-ctrip.com/images/0EQ6b12000jkcsrq78EF7_W_640_10000.jpg?proc=autoorient",
				"imageThumbUrl": "https://dimg04.c-ctrip.com/images/0EQ6b12000jkcsrq78EF7_D_180_180.jpg?proc=autoorient",
				"tagText": null,
				"tagId": null
			},
			{
				"imageId": 1471448622,
				"height": 1899,
				"width": 2532,
				"imageSrcUrl": "https://dimg04.c-ctrip.com/images/0EQ4x12000jkd29enA5A0_W_640_10000.jpg?proc=autoorient",
				"imageThumbUrl": "https://dimg04.c-ctrip.com/images/0EQ4x12000jkd29enA5A0_D_180_180.jpg?proc=autoorient",
				"tagText": null,
				"tagId": null
			}
		],
		"videos": [],
		"scores": [
			{
				"logicValue": 1,
				"logicName": null,
				"score": {
					"source": "5.0",
					"parsedValue": 5
				},
				"name": "景色"
			},
			{
				"logicValue": 2,
				"logicName": null,
				"score": {
					"source": "5.0",
					"parsedValue": 5
				},
				"name": "趣味"
			},
			{
				"logicValue": 3,
				"logicName": null,
				"score": {
					"source": "5.0",
					"parsedValue": 5
				},
				"name": "性价比"
			}
		],
		"voteUsers": [],
		"content": "武夷山景色太美了，山清水秀的好风光，建议乘坐竹筏子，一定要提前预约哦。有时间过来建议最好玩两天的，现在不收门票，非常优惠哦。非常推荐。",
		"languageType": "zh-cn",
		"translateContent": null,
		"translateLanguageType": null,
		"canEdit": false,
		"jumpUrl": "/trip_flutter?flutterName=flutter_trip_shoot_review_detail&businessId=207779349",
		"jumpH5Url": "https://m.ctrip.com/webapp/you/comment/detail/126481/11/207779349.html",
		"replyJumpUrl": "/rn_destination_video/main.js?CRNModuleName=destinationlive&CRNType=1&initialPage=CommentFloat&id=207779349&isPresent=0&topPercent=0.3&isTransparentBg=YES&scene=review&topBackgroundColor=rgba(0,0,0,0.6)",
		"publishTypeTag": "2025-04-04 发布点评",
		"isTripShoot": false,
		"aiTagIdSens": null,
		"replyTag": null,
		"replyContent": null,
		"replyTime": null,
		"setTitle": null,
		"outerTitle": null,
		"impressionTags": null,
		"recommendItems": null,
		"childrenTag": null,
		"ipLocatedName": "黑龙江",
		"replyIpLocatedName": null,
		"isFollow": false,
		"isDeleted": false,
		"clientInfo": "{\"clientId\":\"\",\"currency\":\"\",\"platform\":\"\",\"cctPlatform\":\"\"}",
		"ip": null,
		"jumpMiniAppUrl": "/pages/gs/comment/detail?BusinessId=0&BusinessType=0&CommentId=207779349&POIId=0&SightId=0",
		"isAnonym": true,
		"theForkLogoUrl": null,
		"timeDuration": null,
		"touristTypeDisplay": "家庭亲子",
		"originContent": null,
		"collectCnt": 0,
		"hasCollected": false,
		"isUnderReview": null
	}
}
```

### CEnews 中国环境
![CEnews: Dev Tools - Network - XHR](./img/cenews-1.png)
![CEnews: Dev Tools - Network - XHR - Request](./img/cenews-2.png)
![CEnews: Dev Tools - Network - XHR - Response](./img/cenews-3.png)

**Request**
```
keyWord=%E7%94%9F%E6%80%81%E6%96%87%E6%98%8E&searchLocation=0&searchType=0&beginTime=2007-01-01%2000%3A00%3A00&endTime=2025-05-22%2023%3A59%3A59&orderType=1&pageNumber=1&pageSize=20&columnID=-1&subSiteID=1&siteID=-1&includeSubNode=1&total=39824
```
Request in JSON:
```
{
	"keyWord": "生态文明",
	"searchLocation": "0",
	"searchType": "0",
	"beginTime": "2007-01-01 00:00:00",
	"endTime": "2025-05-22 23:59:59",
	"orderType": "1",
	"pageNumber": "1",
	"pageSize": "20",
	"columnID": "-1",
	"subSiteID": "1",
	"siteID": "-1",
	"includeSubNode": "1",
	"total": "39824"
}
```

**Response**  
Response in JSON (first article):
```
{
	"0": {
		"fileID": 1354001,
		"title": "读图丨“首都老兵”牵手青少年探秘水质监测站",
		"highlightTitle": "读图丨“首都老兵”牵手青少年探秘水质监测站",
		"highlightContent": "也在青少年心中播种下<span style='background-color: yellow'>生</span><span style='background-color: yellow'>态</span><span style='background-color: yellow'>文</span><span style='background-color: yellow'>明</span>的种子，为推动生物多样性保护凝聚青春力量",
		"source": "中国环境APP",
		"editor": "程梓桐",
		"author": "周莹",
		"authorID": 0,
		"abstract": "北京西城开展生物多样性日科普志愿活动",
		"publishTime": "2025-05-22 07:30:00",
		"articleType": 1,
		"articleStatus": 1,
		"linkID": 0,
		"keywords": "北京 生物多样性",
		"countClick": 16,
		"countDiscuss": 0,
		"countPraise": 0,
		"pic1": "https://res.cenews.com.cn/data3/1/images/2025/0521/17478390677141604_450x338.jpg",
		"pic2": "",
		"pic3": "",
		"bigPic": 0,
		"articleUrl": "",
		"videoUrl": "",
		"columnID": 167,
		"columnName": "发布厅",
		"srcColumnID": 167,
		"srcColumnName": "发布厅",
		"images": 1,
		"videos": 0,
		"atts": 0,
		"duration": 0
	}
}
```

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

![MEE: Dev Tools - Network - XHR - Response](./img/mee-resp.png)

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


