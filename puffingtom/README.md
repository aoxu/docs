Star Legend Lua API
======================

用户模块 user
----------------------

### login
用户登录 同步数据，和数据初始化  

####传入参数  

```
待定
```

######返回信息：  
```
{"ret":0,"errMsg":""}
```
##### 错误情况

* -101,参数错误


- - -

### getUserInfo
获取客户端用户信息  


####传入参数

```
{

}
 
```

######返回信息：
```
{
    "errMsg": "",
    "data": {
        "gameCenter": "kinder12345",
        "lastPlunder": 20,
        "facebookName": "",
        "id": 1,
        "registerTime": "1369195200",
        "nickName": "kinder",
        "goldCoinCount": 0,
        "idleBuilderNum": 1,
        "facebookId": "",
        "lastLoginTime": "1369281600",
        "status": 0,
        "selectedHeroId": 1,
        "gemCount": 10,
        "region": "en"
    },
    "ret": 0
}
ps:数据结构待定

```
####错误情况

* -102,信息不存在 



### getUserGold
获取用户金矿存储金币总额

####传入参数

```
{

}
 
```
######返回信息：

```
{
    "errMsg": "",
    "ret": 0,
    "goldCount": 0
}

```

#####错误情况
* -102,信息不存在


### addGold
增加金币到金矿存储  
####传入参数

```
{
"gold" : -- 金币数量
}

```
ps:金币库存已满 金币就会被系统吞掉

######返回信息：

```
{
    "errMsg": "",
    "ret": 0,
    "goldCount": 1000
}

```
#####错误情况
* -101,参数错误
* -102,信息不存在


### deductGold
扣除金矿中的金币库存  
####传入参数

```
{

"gold" : -- 金币数量

}

```
######返回信息：

```

{
    "errMsg": "",
    "ret": 0,
    "goldCount": 1000
}
 
```
#####错误信息
* -101,参数错误
* -102,信息不存在
* -103,金币不足 


### getUserGem
查询宝石总数
####传入参数
```
{
 
}

```

######返回信息：
```
{
    "errMsg": "",
    "gemCount": 10,
    "ret": 0
}

```
#####错误情况
* -101,信息不存在
 


### addGem
增加宝石  
####传入参数

```
{
"gem": -- 宝石数量
}

```


######返回信息：
```
{
    "errMsg": "",
    "gemCount": 310,
    "ret": 0
}

```
#####错误情况
* -101，信息不存在
* -102，参数错误

 


### deductGem
扣除宝石  
####传入参数

```
"gem": -- 宝石数量

```

######返回信息：
```

{
    "errMsg": "",
    "gemCount": 10,
    "ret": 0
}
 
```
#####错误情况
* -101，信息不存在
* -104，宝石不足
 


### getIdleBuilder
获取空闲builder 个数
####传入参数

```
{

}
```
######返回信息：

```

{
    "errMsg": "",
    "builderNum": 1,
    "ret": 0
}
 
```
#####错误情况
* -101，信息不存在


#### addBuilder
增加空闲builder 个数 一次增加1
####传入参数

```
{

}
```
####返回信息：

```

{
    "errMsg": "",
    "builderNum": 1,
    "ret": 0
}
 
```
####错误情况
* -101，信息不存在


### deductBuilder
减少空闲builder 个数 一次减1
####传入参数

```
{

}
```
######返回信息：

```

{
    "errMsg": "",
    "builderNum": 1,
    "ret": 0
}
 
```
#####错误情况
* -101,信息不存在
* -107，空闲builder 不足




建筑模块 architecture
----------------------

### listArchConfig
查询所有建筑id列表。
####传入参数
```
{
}
```
####返回参数
```
{
    idArray -- 配置里的所有建筑id列表
}

```
####示例返回数据
```
{
    "1":{ "id":1, "name":"TownHall" },
    "2":{ "id":2, "name":"GoldMine" },
    "3":{ "id":3, "name":"HeroAltar" },
    "4":{ "id":4, "name":"SkillResearch" },
    "5":{ "id":5, "name":"DefenseDepartment" },
    "6":{ "id":6, "name":"Groceries" },
    "7":{ "id":7, "name":"ExploreTower" }
}
```

### getArchConfig
查询建筑配置。  
可以按id查询所有级别的建筑配置，或者指定id和level查询建筑配置。
####传入参数
```
{
    "id":"建筑id",
    "level":"建筑级别，可选参数"
}
```
####传出参数（根据传入参数，有2种格式）
#####if level == nil then. 根据level不同，可能返回多个建筑
```
{
    "name":"建筑名称",
    "1":{
        "level":"建筑级别",
        "dependArchId":"依赖建筑id",
        "dependArchLevel":"依赖建筑级别",
        "capacity":"个数上限", 
        "price":"价格", 
        "totalTime":"建造/升级总时间",
        "maxLevel":{
            "建筑id":"建筑最高能够建造的等级",
            ...
        },
        "maxCouunt":{
            "建筑id":"建筑最多能够建造的个数",
            ...
        }
    }
}
```
#####if level ~= nil then
```
{
    "name":"建筑名称", 
    "level":"建筑级别", 
    "dependArchId":"依赖建筑id", 
    "dependArchLevel":"依赖建筑级别",
    "capacity":"个数上限",
    "price":"价格",
    "totalTime":"建造/升级总时间",
    "maxLevel":{
        "建筑id":"建筑最高能够建造的等级",
        ...
    },
    "maxCouunt":{
        "建筑id":"建筑最多能够建造的个数",
        ...
    }
}
```
####示例传入数据
```
{
    "id":1,
    "level":1
}
```
####示例返回数据
#####if level == nil then
```
{
    "id":1,
    "name":"TownHall",
    "1":{
        "level":1, "dependArchId":0, "dependArchLevel":0, "capacity":1, "price":10000, "totalTime":60,
        "maxLevel":{ "1":1, "2":1, "3":1, "4":1, "5":1, "6":1, "7":1 },
        "maxCount":{ "2":1 }
    },
    "2":{
        "level":2, "dependArchId":1, "dependArchLevel":1, "capacity":1, "price":20000, "totalTime":60,
        "maxLevel":{ "1":2, "2":2, "3":2, "4":2, "5":2, "6":2, "7":3 },
        "maxCount":{ "2":1 }
    },
    "3":{
        "level":3, "dependArchId":1, "dependArchLevel":2, "capacity":1, "price":30000, "totalTime":60,
        "maxLevel":{ "1":2, "2":2, "3":2, "4":2, "5":2, "6":2, "7":2 },
        "maxCount":{ "2":2 }
    }
}
```
#####if level ~= nil then
```
{
  "capacity": 1,
  "dependArchId": 1,
  "dependArchLevel": 2,
  "id": 1,
  "level": 3,
  "name": "TownHall",
  "price": 30000,
  "totalTime": 60,
  "maxLevel":{ "1":2, "2":2, "3":2, "4":2, "5":2, "6":2, "7":2 },
  "maxCount":{ "2":2 }
}
```
####异常情况
* -301 没有指定的建筑配置（传入的id或level找不到）

### getUserArch
列出用户所有建筑。  
还可以传入id或index，查询用户指定的建筑。  
####传入参数
```
{
    "id":"建筑id，可选参数",
    "index":"同类建筑编号，同一个id下保证不重复，可选参数"
}
```
####返回参数（根据传入参数的不同，可能有多种返回结果）
```
{
    "1":{
		"id":"建筑id",
		"1":{
			"index":"同类建筑编号",
			"level":"建筑级别",
			"pos":{ "x":"x座标", "y":"y座标" },
			"finishedTime":"完成时刻",
			"isBuilding":"是否正在建造/升级",
			"remainingTime":"建造/升级剩余时间"
		} -- 根据index的不同，可能有多个
	} -- 根据id的不同，可能有多个
}
```
####示例传入数据
```
{
    "id":1,
    "index":1
}
```
####示例返回数据
#####传入参数为空的情况
```
{
	"1":{
		"id":1,
		"1":{
			"index":1, "level":1, "pos":{ "x":100, "y":100 }, "finishedTime":0, "isBuilding":false, "remainingTime":0
		}
	},
	"2":{
		"id":2,
		"1":{
			"index":1, "level":1, "pos":{ "x":150, "y":150 }, "finishedTime":18948788493, "isBuilding":true, "remainingTime":1234
		},
		"2":{
			"index":2, "level":1, "pos":{ "x":200, "y":200 }, "finishedTime":18948788493, "isBuilding":true, "remainingTime":1234
		},
		"3":{
			"index":3, "level":1, "pos":{ "x":300, "y":300 }, "finishedTime":18948788493, "isBuilding":true, "remainingTime":1234
		}
	}
}
```
#####传入id的情况
```
{
	"id":1,
	"1":{
		"index":1, "level":1, "pos":{ "x":100, "y":100 }, "finishedTime":0, "isBuilding":false, "remainingTime":0
	}
}
```
#####传入id和index的情况
```
{
	"index":1, "level":1, "pos":{ "x":100, "y":100 }, "finishedTime":0, "isBuilding":false, "remainingTime":0
}
```
####异常情况
* -302 没有指定的建筑（传入的index找不到）

### canBuild
查询是否满足建造/升级的条件。
返回结果中包含哪些条件满足，哪些条件不满足。
####传入参数
```
{
    "id":"建筑id"
    "index":"同类建筑编号，可选参数，当查询是否可升级时传入"
}
```
####返回参数
```
{
    "canBuild":"是否可建造/升级",
    "isBuilding":"是否正在建造"
    "isDependArch":"是否满足建筑科技树依赖条件"
    "isGold":"是否有足够的金币",
    "needGold":"金币不够时，还差的金币数",
    "needGem":"金币不够时，兑换金币所需的宝石数",
    "isBuildSkill":"是否有足够的建造技能数"
}
```
####示例传入数据
```
{
    "id":1,
    "index":1
}
```
####示例返回数据
```
{
    "canBuild":0,
    "isBuidling":true,
    "isDependArch":true,
    "isGold":false,
    "needGold":1000,
    "needGem":10,
    "isBuildSkill":false
}
```
####异常情况
* -302 没有指定的建筑（传入的index找不到）

### buildArch
建造/升级建筑。  
根据传入index来确定是建造新建筑还是升级已有建筑。建造/升级建筑会减去金币、Mr.O的建造技能数，并且需要等待时间完成。
####传入参数
```
{
    "id":"建筑id",
    "index":"同类建筑编号。可选参数，如果升级，则需要传入。"
}
```
####返回参数
{}
####示例传入数据
```
{
    "id":1,
    "index":1
}
```
####异常情况
* -302 没有指定的建筑（传入的index找不到）
* -303 无法建造，不满足建筑科技树依赖条件
* -304 无法建造，金币不足
* -305 无法建造，Mr.O没有足够的空闲技能数
* -306 无法建造，因为正在建造中

### buildFinishDelegate
建造/升级建筑完成的处理。注册到定时器中，当时间到时触发执行。
####传入参数
```
{
    "id":"建筑id",
    "index":"同类建筑编号",
    "level":"当前等级，如果是建造新建筑，则为0",
    "pos":"x,y座标"
}
```
####返回参数
{}
####示例传入数据
```
{
    "id":1,
    "index":1,
    "level":1,
    "pos":{"x":100, "y":200}
}
```


#### finishByGems
宝石快速完成建造/升级
#####传入参数
```
{
	"id":"建筑id",
	"index":"同类建筑编号" 
}
```
#####返回参数：
```
{
	"gems":"消费宝石数"
}
```
#####传入数据示例
```
{
    "id":1,
    "index":2
}
```
#####返回数据示例
```
{
    "gems":123
}
```



#### getStatus
查询建筑状态
#####传入参数
```
{
	"id":"建筑 id",
	"index":"同类建筑编号" 
}
```
#####返回参数
```
{
	"remainingTime":"剩余时间",
	"totalTime":"建造/升级总时间",
}
```
#####示例传入数据
```
{
    "id":1,
    "index":2
}
```
#####示例返回数据
```
{
    "remainingTime":123,
    "totalTime":12345
}
```

英雄模块 hero
----------------------

### getHeroConfig
#####接口说明：得到英雄的配置信息
#####传入参数说明：
```
{
	heroId --英雄id
	level  --英雄等级，可选
}
```
#####返回参数：
```
* heroList:英雄等级配置列表
{
	"ret":0, 				--返回值，判断是否拉取到
	"heroName":"MR.Q",		--英雄名称
	"maxLevel": 5			--最大等级
	"levels":				--英雄等级配置数据
	{
	"heroAltarLevel":1,		--依赖英雄塔
	"jumpHeight":2, 		--弹跳高度
	"lifeSpeed":60,			--生命恢复速度
	"price":10,				--当前等级购买价格
	"jumpWidth":5,			--弹跳宽度
	"lives":5,				--最高生命值
	"waitingTime":60,		--升级等待时间
	"speed":6,				--英雄运动速度
	"level":1,				--等级
	"id":1				--专属技能
	}
}
```
####示例返回数据
```
{
	"ret":0,
	"errMsg":"",
	"heroName":"MR.Q",
	"maxLevel":5,
	"levels":
	{
	"heroAltarLevel":1,
	"jumpHeight":2,
	"lifeSpeed":60,
	"price":10,
	"jumpWidth":5,
	"lives":5,
	"waitingTime":60,
	"speed":6,
	"level":1,
	"id":1
	}
}

```

#####错误情况：
```
* 无效的英雄id
* 无效的等级
```


### add
#####接口说明：召唤新的英雄
#####传入参数说明：
```
{
	heroId --英雄id
}
```

#####返回参数：
```
{
	ret --是否召唤成功
}
```
#####错误情况：
```
* 无效的英雄id
* 已经召唤过了
* 英雄塔级别不够
* 金币不够
```
####示例返回数据
```
{
	"ret":0, 
	"errMsg":"",
	"hero":
	{
		"startedMoment": 1369812987, --新增开始时间，用于显示时间进度
		"unlockedMoment":1369812987,
		"status":1,
		"heroId":2,
		"lives":0,
		"level":0
	}
}


```


### addAtOnce
#####接口说明：使用宝石使新的英雄立即可以使用
#####传入参数说明：
```
{
	heroId --英雄id
}
```

#####返回参数：
```
{
	ret --是否召唤成功
}
```
#####错误情况：
```
* 无效的英雄id
* 已经召唤过了
* 英雄塔级别不够
* 金币不够
```
####示例返回数据
```
{
	"ret":0,
	"errMsg":"",
	"gem":20,
	"hero":
	{
		"unlockedMoment":0,
		"status":3,
		"heroId":2,
		"lives":5,
		"level":1
	}
}

```


### upgrade
#####接口说明：升级英雄
#####传入参数说明：

```
{
	heroId --英雄id
}
```

#####返回参数：
```
{
	ret --是否升级成功
}
```
#####错误情况：
```
{
	* 无效的英雄id
	* 已经最高等级
	* 英雄塔级别不够
	* 金币不够
}
```
####示例返回数据
```
{
	"ret":0,
	"errMsg":"",
	"hero":
	{
		"startedMoment": 1369812987, --新增开始时间，用于显示时间进度
		"unlockedMoment":1369813683,
		"status":2,
		"heroId":2,
		"lives":5,
		"level":1
	}
}

```

### upgradeAtOnce
#####接口说明：使用宝石直接升级英雄
#####传入参数说明：

```
{
	heroId --英雄id
}
```

#####返回参数：
```
{
	ret --是否升级成功
}
```
#####错误情况：
```
{
	* 无效的英雄id
	* 已经最高等级
	* 英雄塔级别不够
	* 金币不够
}
```
####示例返回数据
```
{
	"ret":0,
	"errMsg":"",
	"gem":20,
	"hero":
	{
		"heroId":2,
		"level":2,
		"lives":5,
		"unlockedMoment":0,
		"status":3
	}
}

```

### getUserHeros
#####接口说明：得到用户的英雄列表
#####传入参数说明：
```
{
	heroId --可选参数，不传则返回所有
}
```

#####返回参数：
```
* 用户英雄信息
```

#####错误情况：
```
* 无
```
####示例返回数据
```
{
	"ret":0,
	"errMsg":"",
	"heroList":
	[{
		"startedMoment": 1369812987, --新增开始时间，用于显示时间进度
		"unlockedMoment":1,
		"totalTime":600,--总的建造时间
		"remainingTime":600,--剩余时间
		"status":1,
		"heroId":2,
		"lives":5,
		"level":2,
		"heroName":"Mr.Q"
	}]
}
状态说明：
heroStatusUnbuild=0 --未召唤
heroStatusBuilding=1--召唤中
heroStatusUpgrading=2--升级中
heroStatusUsing=3--使用中
```

### attachPublicSkill
#####接口说明：英雄绑定公共技能
#####传入参数说明：
```
* heroId：英雄id
* id：公共技能
```
#####返回参数：
```
* 绑定是否成功
```

#####错误情况：
```
* 无效的英雄id
* 无效的技能id
* 用户未拥有该英雄
* 用户未拥有该技能
```




### loseLife
#####接口说明：英雄战斗过程中掉血
#####传入参数说明：
```
* heroId：英雄id
```
#####返回参数：
```
* 当前血量
```
#####错误情况：
```
* 无效的英雄id
* 用户未拥有该英雄
* 已经无血可减
```

####示例返回数据
```
{
	"ret":0,
	"errMsg":"",
	"hero":
	{
		"lifeAddedMoment":1369817925,
		"heroId":2,
		"unlockedMoment":0,
		"status":3,
		"level":2,
		"lives":4
	}
}


```


### setFullLives
#####接口说明：直接用宝石充能时使用，设置英雄满血
#####传入参数说明：
```
* heroId：英雄id
```
#####返回参数：
```
* 当前血量
```

#####错误情况：
```
* 无效的英雄id
* 用户未拥有该英雄
* 已经满血
```

####示例返回数据
```
{
	"ret":0,
	"gem":20,
	"errMsg":"",
	"hero":
	{
		"lifeAddedMoment":0,
		"level":2,
		"unlockedMoment":0,
		"status":3,
		"heroId":2,
		"lives":5
	}
}


```

### canBuild
#####接口说明：返回单个英雄的状态，用于界面展示时使用
#####传入参数说明：
```
* heroId：英雄id
```
#####返回参数：
```
* result.canBuild = false
        result.isBuilding = false
        result.isDependArch = false
        result.isGold = false
        result.isConflict = false
{
	ret, --是否升级成功
	errMsg,
	canBuild, --是否可以建造或者升级
	isBuilding,--是否正在建造或者升级
	isDependArch, --是否受英雄祭坛限制
	isGold,		--金币是否足够
	isConflict---是否冲突（英雄祭坛正在建造或者升级英雄）
}
```

#####错误情况：
```
* 无效的英雄id
```

####示例返回数据
```
{
	"ret":0,
	"errMsg":"",
	canBuild:0, --是否可以建造或者升级
	isBuilding:0,--是否正在建造或者升级
	isDependArch:0, --是否受英雄祭坛限制
	isGold:0,		--金币是否足够
	isConflict:0---是否冲突（英雄祭坛正在建造或者升级英雄）
}
```

技能模块 skills
----------------------

### initDb
内部使用，初始化技能
####传入参数
```
{
}
```
######返回信息：

```
{

}
```


### getSkillInstituteInfo
获取技能研究所信息
####传入参数
```
{ 
"level": -- 建筑等级 不传入返回所有等级 是数组
}

```

######返回信息：

```
{
    "ret": 0,
    "errMsg": "",
   	"data":{
   	"id":1
   	"maxLevel":
   	"maxNumber": --可拥有数
   	"nameEn":
   	}
    不传入level
     "ret": 0,
    "errMsg": "",
   	"data":{
   	 [1]:{
   	 	"id":1
   		"maxLevel":
   		"maxNumber": --可拥有数
   		"nameEn":
   	 }
   	 [2]:{
   		"id":1
   		"maxLevel":
   		"maxNumber": --可拥有数
   		"nameEn":
   	 }
   	}
}

```
#####错误情况
* -101，参数错误
* -102，信息不存在



### getConfig
获取技能信息 
####传入参数
```
{
"id": -- 技能id
"level": -- 技能等级
}

```
######返回信息：

```
{
    "errMsg": "",
    "data":[],
    "ret": 0
    参数不传入返回所有
}
```
#####错误情况
* -101,信息不存在
* -102,参数错误

 


### getSkillsList
获取技能等级信息 可以都不传入参数 
####传入参数
```
{
"id" : -- id
"level": -- 技能等级
"type": 1 or nil  传入 则默认去掉 builder 技能 预留null空位
}

```
######返回信息：

```
{
    "errMsg": "",
    "data": 
    	tmp["id"]	= 0 -- id
		tmp["nameEn"]	= 0 -- 名称
		tmp["maxLevel"] = 0 -- 最大等级
		tmp["attribute"] = 0 -- 技能详细信息
		tmp["rechargeTime"] = 0 --充能时间
		tmp["maxCapacity"] = 0 --最大拥有个数
		tmp["hasSeveral"] = 0 --已有个数
		tmp["chargedTime"] = 0 --充能时间
		tmp["buyNeedGold"] = 0 --单个购买充能价格
		tmp["finishedTime"] = 0 --完成时间
		tmp["needGold"] = 0 --金币不足时返回的差额
		tmp["needGem"] = 0 --差额兑换的宝石数
		tmp["level"]=0, --等级
		tmp["isGold"] true or false
		tmp["isResearching"] true or false
    "ret": 0
}
ps:当id 为 nil 时 返回所有技能信息

```
#####错误情况
* -101，参数错误
* -102，信息不存在



### getStatus
获取技能状态
####传入参数
```
{
"id": -- 技能id
}
```
######返回信息：

```
{
    "errMsg": "",
    "data": {
    ["id"]
    ["level"]
    ["finishedTime"]
    ["remainingTime"]
    ["totalTime"]
    ["needGem"] --快速完成宝石
    ["isResearching"] -- 是否在学习中 true or false
    },
    "ret": 0
}

```
#####错误情况
* -101，参数错误
* -102，信息不存在



### research
技能学习或升级
####传入参数
```

{
“id”: --
}
```

######返回信息：

```
{
    "errMsg": "",
    "remainingTime"
    "finishedTime"
    "totalTime"
     "ret": 0
}

```

####错误情况
* -101，参数错误
* -102，信息不存在




### skillIsFinished
完成回调
####传入参数
```
{
“id”:-- id
}

```


######返回信息：
```
{
    "errMsg": "",
    "ret": 0
}

```
####错误信息
* -101，参数错误




### finishByGems
升级技能  
####参数传入
```
{
"id": -- 技能id
}

```
######返回信息：

```

{
    "errMsg": "",
    "ret": 0
}

```
####错误信息
* -101，参数错误
* -102，信息不存在
* -103，金币不足
* -106，等级不足
* -104，宝石不足
* -112，建筑正忙



### addFinishedTime
技能研究所升级时 调用 增加研究或升级的完成时间 
####参数传入
```
{
"time":-- 时间 秒

}
```

######返回信息：

```

{
    "errMsg": "",
    "ret": 0
}

```

#####错误情况
* -101,参数错误



### hasAction
判断技能研究所是否在研究或升级

####传入参数

```
{

}

```
######返回信息：

```
{
	“ret”:0,
	"action":0, -- or 1
	"errmMsg": "",
}

```
 

### addSkillNum
增加技能容量个数

####传入参数

```
{
“skillName”:--技能名称,
“num”:-- 个数
}

```
######返回信息：

```
{
    "errMsg": "",
    "skillNum": 5,
    "ret": 0
}

```
#####错误情况
* -101,参数不正确
* -102,没有技能技能信息



### deductSkillNum
减少技能容量个数

####传入参数

```
{
"skillName":--技能名称,
"num":-- 个数
}

```
######返回信息：

```
{
    "errMsg": "",
    "skillNum": 4,
    "ret": 0
}

```
#####错误情况
* -101,参数不正确
* -102,没有技能技能信息
* -502，技能数不足


### finishByGems
宝石升级消除时间
####传入参数

```
{
"id":--id,
}

```
######返回信息：

```
{
    "errMsg": "",
    "ret": 0
}

```
#####错误情况
* -101，参数不正确
* -102，没有技能技能信息
* -104，宝石不足

防御建筑模块 defense
----------------------

### getDefenseInfo
获取防御建筑的Id的列表 data 的索引即是Id
####传入参数
```
{
	level - 防御部等级 
}
```
######返回信息：

```
传入了防御部等级 返回当前传入等级的信息
{
    "errMsg": "",
    "data":{
    ["id":1,"maxLevel":2,"maxNumber":2,"nameEn":"xxxxx"],
    ["id":1,"maxLevel":2,"maxNumber":2,"nameEn":"xxxxx"],
    ["id":1,"maxLevel":2,"maxNumber":2,"nameEn":"xxxxx"],
    ["id":1,"maxLevel":2,"maxNumber":2,"nameEn":"xxxxx"],
    ["id":1,"maxLevel":2,"maxNumber":2,"nameEn":"xxxxx"],
    ["id":1,"maxLevel":2,"maxNumber":2,"nameEn":"xxxxx"],
    ["id":1,"maxLevel":2,"maxNumber":2,"nameEn":"xxxxx"],
    },
    "ret": 0
}
不传入等级 将返回防御部等级 所有信息


```

### getDefenseBuildInfo
获取所有建筑或指定建筑的信息
####传入参数
```
{
"id":--防御建筑id
"level":--level 不传 返回1级信息
}
```
######返回信息：

```
{
    "ret": 0,
    "errMsg": "",
    "data":{
    "id":1, -- 总时间
    "nameEn":"xxxx", -- 剩余时间
    "maxLevel":5, -- 所需要宝石
    "attribute":{对应建筑的等级详细信息},
    "islevel":true, -- 需要防御部等级是否够
    "buildCount":5,--最大建造数量 或购买数量
    "isFull":true, -- 是否满
    "alreadyBuildCount":111,--已经建造或购买数量
    "isGold":true, -- 建造或升级的金币是否够
    "needGold":1000,--还差多少金币 
    "isBuyPrice":true, -- 购买价格是否够 
    "needBuyPrice":100, -- 还差多少购买价格
    "isBuilder":true, -- builder数量是否够
    }
}

```


### getStatus
查询当前防御建筑状态 返回当前剩余时间 总时间 完成时间 和结束完成的所需要宝石
####传入参数
```
{
"id":--防御建筑id
"index":--index
}
```

######返回信息：

```
{
    "ret": 0,
    "errMsg": "",
    "data":{
    "totalTime":222, -- 总时间
    "remainingTime":2222, -- 剩余时间
    "needGem":22, -- 所需要宝石
    "finishedTime":2222 -- 完成时间
    }
}

```

### canBuild
查询当前建筑是否能升级或建造 或购买  还差多少钱等等
####传入参数

```
{
	id = id 防御建筑id
	index = index 传入index 就是查询当前已有的 没有的可以不穿 默认返回1级的配置信息
}
```
######返回信息：

```
{
    "ret": 0,
    "errMsg": "",
    "data":{
    "id":1, -- 总时间
    "nameEn":"xxxx", -- 剩余时间
    "maxLevel":5, -- 所需要宝石
    "attribute":{对应建筑的等级详细信息},
    "islevel":true, -- 需要防御部等级是否够
    "buildCount":5,--最大建造数量 或购买数量
    "isFull":true, -- 是否满
    "alreadyBuildCount":111,--已经建造或购买数量
    "isGold":true, -- 建造或升级的金币是否够
    "needGold":1000,--还差多少金币 
    "isBuyPrice":true, -- 购买价格是否够 
    "needBuyPrice":100, -- 还差多少购买价格
    "isBuilder":true, -- builder数量是否够
    "needGem":需要宝石 就是升级或建造时 金币差额 兑换宝石的个数
    "needBuyGem": 阶梯特有 购买阶梯的时候 钱不够 会出现
    }
}

```

### build
防御建筑的建造 或 升级
####传入参数

```
{
	id = id 防御建筑id
	index = index 传入index就是升级
}
```
######返回信息：

```
{
	"ret":0,
	"errMsg":"xx",
	"usedGold":222, --花费金币
	"totalTime":2222, -- 总时间
	"index":1, --index
	"finishedTime":1111 -- 完成时间 
	
这里金币不足时 我也做了输出 金币还差多少 需要多少宝石去换
}


```

####错误情况
* -101 参数错误
* -102 信息不存在

### isFinished
建造完成回调

####传入参数
```
{
id --  建筑Id
index -- 
}

```
######返回信息：

```
{
    "ret": 0
    "errMsg":"xxx"
}

```




### finishByGems
用宝石加速
####传入参数
```

{
“id”: -- 建筑Id
“index” -- index 
}


```

######返回信息：


```
{
    "ret": 0
    "errMsg":"xxx"
}

```

资源模块 resource
------------------

### setUserGoldmine
金矿建造或升级完成时 调用 保存玩家金矿功能信息

####传入参数
```
“level”: -- level
"index": -- 建筑Id

```

######返回信息：
```
{
    "ret": 0,
    "errMsg": ""
}

```

### getGoldmineInfo
获取对应等级或全部等级的金矿功能信息

####传入参数
```
"level": -- level

```

######返回信息：
```

{
    "errMsg": "",
    "config": [
        {
            "productionLimit": 2000,
            "perMinute": 20,
            "saveGoldLimit": 20000
        },
        {
            "productionLimit": 3000,
            "perMinute": 30,
            "saveGoldLimit": 30000
        },
        {
            "productionLimit": 4000,
            "perMinute": 40,
            "saveGoldLimit": 40000
        }
    ],
    "ret": 0
}

```
####错误情况
* 对应等级信息不存在

### getUserGoldmineInfo
得到玩家金矿信息

####传入参数
```
"index": -- index

```

######返回信息：
```

{
    "errMsg": "",
    "config": {
        "saveGold": 10000,
        "productionLimit": 2000,
        "productionFinishedTime": 1369924136,
        "id": 1,
        "level": 1,
        "perMinute": 20,
        "saveGoldLimit": 20000
    },
    "ret": 0
}

```

### produce

####传入参数
```
{
"index":-- 建筑Id
}


```

######返回信息：
```

{
    "errMsg": "",
    "ret": 0,
    "gold": 2000
}

```

### gather
金矿金币收取

####传入参数
```

{
"index":-- 建筑Id
}

```

######返回信息：
```

{
    "errMsg": "",
    "ret": 0,
    "gold": 12000 -- 库存
}

```
####错误情况
* 没有金币产生


### getSaveLimit

####传入参数
```

{

}

```

返回金币存储上限
######返回信息：
```
{
    "errMsg": "",
    "saveGoldCount": 20000,
    "ret": 0
}

```



### exchange

####传入参数
```

{

}

```

返回金币存储上限
######返回信息：
```
{"errMsg":"","data":{"half":{"gem":2732,"gold":273200},"all":{"gem":5464,"gold":546400}},"ret":0} 

```

杂货铺模块 grocery
----------------------
 
### getCanBuy
#####接口说明：得到所有杂货列表
#####传入参数说明：
* id：杂货id or 不传

```
{
"id":1 
}
or
{
}

```

#####返回参数：
* 杂货的配置列表

```

{
"ret":0,
"errMsg":"",
"data":{
	[{
	"id":1,
	"name":"xxxx",
	"canBuyNumber":4,
	"buyNumber":2,
	"needGold":500,
	"isGold":true,
	"isNumber":true,
	"isLevel":true
	}],
}

}

```


#####错误情况：
* 无效的杂货id
* 获取金币失败
* 获取宝石失败预留



### buyGrocery
#####接口说明：购买杂货
#####传入参数说明：
* id：杂货id

#####返回参数：
* 购买是否成功

#####错误情况：
* 无效的杂货id
* 个数已满
* 杂货部等级部足
* 金币不够

商店模块 shop
----------------------

### getStoreItems
#####接口说明：得到商店商品信息
#####传入参数说明：
* 无

#####返回参数：
* 商品列表

#####错误情况：
* 无



### buyItem
#####接口说明：购买商品
#####传入参数说明：
* type：商品类型，1-金币，2-宝石
* itemId

#####返回参数：
* 购买是否成功，返回用户拥有商品情况

#####错误情况：
* 无效的商品名

### exchangeTimeToGems
#####接口说明：转换时间成对应的宝石数
#####传入参数说明：
```
{
	timestamp：时间戳
}
```
#####返回参数：
```
* 对应的宝石数
```
#####错误情况：
```
* 无
```
#####示例返回例子
```
{
	ret=0,
	errMsg="",
	gems=20,
}
```

### goldExchangeGem
用宝石兑换金币时，查询所需的宝石数。由于每个宝石都可以兑换很多金币，所以兑换后得到的金币数可能超出所需的金币数一点。
####传入参数
```
{
	gold = "所需金币数"
}
```
####返回参数
```
{
	gem = "兑换消费的宝石数"
}
```
####示例传入数据
```
{
	gold = 123
}
```
####示例返回数据
```
{
	gem = 3
}
```

### gemExchangeGold
用宝石兑换金币，
####输入参数
```
{
	gem = "消费的宝石数"
}
```
####返回参数
```
{
	gold = "兑换出来的金币数"
}
```
####示例输入数据
```
{
	gem = 3
}
```
####示例返回数据
```
{
	gold = 123
}
```

天梯模块 ladder
----------------------

### findPlayer
服务器接口，按天梯积分匹配对手。
####传入参数
```
{
    "uid":"用户id"
}
```
####返回参数
```
{
    "name":"用户名称",
    "uid":"对手用户id",
    "score":"对手当前天梯积分",
    "winScore":"进攻胜利后奖励积分",
    "loseScore":"进攻失败后减去积分",
    "winGold":"进攻胜利后金币收益",
}
```
####示例传入数据
```
{
    "uid":123
}
```
####示例返回数据
```
{
    "name":"用户名",
    "uid":456,
    "score":1250,
    "winScore":20,
    "loseScore":-30,
    "winGold":1000,
}
```
####异常情况
* 没有可匹配的玩家，请稍后再试。（只有机器人故障时出现）
* 正在游戏的玩家太多，请稍后再试。

### battleOver
服务器接口，战斗结束，上传战斗结果。  
在一局游戏结束时，客户端发送游戏结果给服务器。如果离线玩家被击败，则离线玩家从正在被攻击的池子移动到保护池；如果离线玩家获胜，则离线玩家从正在被攻击的池子移动回离线玩家池，允许再次被分配。
####传入参数
```
{
    "attackUid":"进攻用户id",
    "defenseUid":"防守用户id",
    "result":"战斗结果，赢是true，输是false"
}
```
####传出参数
{}
####示例传入数据
```
{
    "attackUid":123,
    "defenseUid":456,
    "result":true
}
```

### map_upload
服务器接口，按uid上传tmx地图文件。module是 map

####传入参数
```
{
    "attackUid":"进攻用户id",
    "defenseUid":"防守用户id",
    "result":"战斗结果，赢是true，输是false"
}
```


### map_download
服务器接口，按uid下载tmx地图文件。module是 map




### checkExpireProtection
服务器定时任务，把超过保护期的玩家放进离线玩家池（可被攻击）。

### checkExpireBattle
服务器定时任务，把超过一局战斗游戏时间的玩家放进离线玩家池（可被攻击）。

### addOfflinePlayer
服务器定时任务，把所有从在线变成离线的玩家移进离线玩家池（可被攻击）。

### delOfflinePlayer
服务器接口，随着登录从离线玩家池中移除（不可被攻击）。

进攻模块(探索塔) attack
----------------------

### getExploreTowerInfo
返回探索塔信息 按等级返回 

####传入参数
```
{
"level": --level or nil

}

```

####返回信息

```

{
    "errMsg": "",
    "config": [
        {
            "skillNum": 2
        },
        {
            "skillNum": 3
        },
        {
            "skillNum": 4
        }
    ],
    "ret": 0
}

```

### attack
出击，保存用户选择英雄和技能
####传入参数
```
{
"heroId": -- 英雄Id
"revenge":0,--复仇 0 否 1是
"skillsList":[1,2,3,4,5] -- skillId
}

```
####返回信息

```
{
    "errMsg": "",
    "data": {
        "revenge": 0,
        "score": 0,
        "heroLifeNum": 5,
        "skillsList": [
            null,--循环时 需要判断是否是空
            {
                "level": 1,
                "skillId": 1
            }
        ],
        "heroId": 1,
        "preyPro": 25,
        "preyGold": 0
    },
    "ret": 0
}

```

### makeSkill
购买技能

####传入参数
```

{
"skillId": -- 技能 index name
}


```

####返回信息

```
{
    "errMsg": "",
    "data": {
        "2": [
            {
                "finishedTime": 1371262577,
                "rechargeTime": 500,
                "skillId": 2
            }
        ],
        "allTime": 1371262577
    },
    "ret": 0
}

```




### getMakeSkillInfo
获取技能充能列表
####传入参数

```
{

}

```

####返回信息

```
{
    "errMsg": "",
    "data": {
        "2": {
            "finishedTime": 0,
            "nameEn": "attack",
            "waitChargedNum": 1,--待充能个数
            "id": 2,
            "buyNeedGold": 200,
            "remainingTime": 0,
            "isUprgade": true,
            "totalTime": 10,
            "hasSeveral": 4, -- 剩余个数
            "maxLevel": 3,
            "maxCapacity": 5,--最多拥有个数
            "status": 0, --状态 -1 未解锁 0 已解锁 1 学习中 2 升级中
            "level": 1,
            "chargedTime": 1371234060,
            "rechargeTime": 500 --单个充能时间
        },
        "3": {
            "finishedTime": 0,
            "nameEn": "rampage",
            "waitChargedNum": 0,
            "id": 3,
            "hasSeveral": 0,
            "remainingTime": 0,
            "isUprgade": true,
            "totalTime": 190,
            "buyNeedGold": 200,
            "maxLevel": 3,
            "maxCapacity": 5,
            "status": -1,
            "level": 1,
            "chargedTime": 0,
            "rechargeTime": 500
        },
        "4": {
            "finishedTime": 0,
            "nameEn": "protect",
            "waitChargedNum": 0,
            "id": 4,
            "hasSeveral": 0,
            "remainingTime": 0,
            "isUprgade": true,
            "totalTime": 20000,
            "buyNeedGold": 200,
            "maxLevel": 3,
            "maxCapacity": 5,
            "status": -1,
            "level": 1,
            "chargedTime": 0,
            "rechargeTime": 500
        },
        "5": {
            "finishedTime": 0,
            "nameEn": "fly",
            "waitChargedNum": 0,
            "id": 5,
            "hasSeveral": 0,
            "remainingTime": 0,
            "isUprgade": true,
            "totalTime": 190,
            "buyNeedGold": 200,
            "maxLevel": 3,
            "maxCapacity": 5,
            "status": -1,
            "level": 1,
            "chargedTime": 0,
            "rechargeTime": 5
        },
        "6": {
            "finishedTime": 0,
            "nameEn": "magnet",
            "waitChargedNum": 0,
            "id": 6,
            "hasSeveral": 0,
            "remainingTime": 0,
            "isUprgade": true,
            "totalTime": 190,
            "buyNeedGold": 200,
            "maxLevel": 3,
            "maxCapacity": 5,
            "status": -1,
            "level": 1,
            "chargedTime": 0,
            "rechargeTime": 5
        },
        "7": {
            "finishedTime": 0,
            "nameEn": "blink",
            "waitChargedNum": 0,
            "id": 7,
            "hasSeveral": 0,
            "remainingTime": 0,
            "isUprgade": true,
            "totalTime": 190,
            "buyNeedGold": 200,
            "maxLevel": 3,
            "maxCapacity": 5,
            "status": -1,
            "level": 1,
            "chargedTime": 0,
            "rechargeTime": 5
        },
        "leftTime": 725,---总的剩余时间 技能充能
        "allTime": 1371263077 --- 总的完成时间
    },
    "ret": 0
}

```

### chargedFinish
技能充能 完成时的回调

####传入参数
```
{
"id":-- 充能Id
"skillId": -- skillId
}

```
####返回信息

```
{
    "errMsg": "",
    "ret": 0
}

```

### usedSkill
战斗中使用技能，出战信息的技能个数 和 原本技能个数 都会扣

####传入参数
```
{
"num":--使用个数
"skillId": -- 技能 skillId
}

```
####返回信息

```
{
    "errMsg": "",
    "leftNumber": 3,
    "ret": 0
}

```




### chargedSkillFull
宝石完成所有充能

####传入参数
```
{
}

```
####返回参数

```

{
    "errMsg": "",
    "needGem": 1,
    "ret": 0
}

```

### deductHeroBlood
英雄战斗中扣血 战斗信息 和 原本英雄的血量 都会扣
####传入参数
```
{
"heroId":--英雄Id
"bloodNum": -- 扣除的血量
}

```
####返回信息

```
{
    "errMsg": "",
    "ret": 0
}

```

### clearUpMakeSkill
清除技能充能列表以及timelist
技能升级成功时调用，用宝石充能时调用
####传入参数
```
{
"skillId": -- 技能名称
}

```

####返回信息

```
{
    "errMsg": "",
    "ret": 0
}

```


### getAttackInfo
得到进攻信息

####传入参数
```
{

}

```

####返回信息

```
{
    "errMsg": "",
    "data": {
        "revenge": 0, --是否复仇
        "score": 0, -- 积分
        "heroLifeNum": 5,--英雄生命
        "skillsList": [
            null,--技能是按Id 顺序排序的· 返回json时会预留一个空位 需要做判断
            {
                "level": 1,--技能等级
                "num": 3,--技能个数
                "skillId": 2--技能id
            }
        ],
        "heroId": 1,--英雄id
        "preyPro": 25,--掠夺比例
        "preyGold": 0--抢得金币
    },
    "ret": 0
}

```

- 
### getAttackGold
获取探索的价钱

#####传入参数说明：
```
{

}
```
####返回参数

```
{"ret":0,
"data":100,
"errMsg":""
}

```

### getAttackDefault
返回上一次使用的技能和英雄

result["heroId"] = heroId
result["skillList"] = skillList
return result
####10.16 canCharge()<a id="fight_canCharge"></a>
#####传入参数说明：
```
{
	skillId -- 技能id
}
```
####返回参数

```
{"ret":0,
"needGold":100, --当充能价钱不够时 返回差额
"needGem":100, -- 差额换成宝石
"isCharge" : true or false
"errMsg":""
}

```

定时器模块 timerList
----------------------

### register
#####接口说明：注册充能事件，相同的id，会覆盖数据
#####传入参数说明：
```
{
	rechargeId 			--充能id
	callback   			--回调函数
	callbackParams 		--回调函数参数
	timestamp			--回调触发时间点
}
```

#####返回参数：
```
{
}
```

#####错误情况：
```
无
```



### cancel
#####接口说明：取消充能事件
#####传入参数说明：
```
{
	rechargeId 			--充能id
}
```

#####返回参数：
```
{
}
```

#####错误情况：
```
无
```


### check
#####接口说明：检查充能事件，达到时间的，会触发finish操作
#####传入参数说明：
```
{
}
```

#####返回参数：
```
{	"ret":0,
	"info":[{
		"callback":"hero.heroAddDelegate",
		"params":{
			"lives":5,
			"heroId":1
		}
	}],
	"errMsg":""
}
```
#####错误情况：
```
无
```


### finish
#####接口说明：完成充能事件，触发回调函数，内部模块调用，对外不公开
#####传入参数说明：
```
{
	rechargeInfo --充能信息
}
```

#####返回参数：
```
{
}
```

#####错误情况：
```
无
```

数据存储模块 datastore
----------------------

### import
导入用户数据。支持只导入部分字段，例如参数里只有用户的英雄数据，这个特性可以在增量同步的时候使用。
####传入参数
```
{
    "data":{
        "用户数据表名":{value值，一个lua table转换成的json对象},
        ...
}
```
####返回参数
{}
####示例传入数据
```
{
    "data":{
        "userArch":{            
            [1] = {
                id = 1,
                [1] = { index = 1, level = 1, pos = { x = 100, y = 100 }, finishedTime = 0
                }
            },
            [2] = {
                id = 2,
                [1] = { index = 1, level = 3, pos = { x = 150, y = 150 }, finishedTime = 18948788493
                },
                [2] = { index = 2, level = 3, pos = { x = 200, y = 200 }, finishedTime = 0
                },
                [3] = { index = 3, level = 4, pos = { x = 150, y = 150 }, finishedTime = 18948788493
                },
                [4] = { index = 4, level = 4, pos = { x = 200, y = 200 }, finishedTime = 18948788493
                }
                [5] = { index = 5, level = 3, pos = { x = 300, y = 300 }, finishedTime = 18948788493
                }
            }
        }
    }
}
```
### export
导出所有用户数据。
####传入参数
{}
####返回参数
```
{
    "data":{
        "用户数据表名":{value值，一个lua table转换成的json对象},
        ...
}
```
####示例返回数据
```
{
    "data":{
        "userArch":{
            [1] = {
                id = 1,
                [1] = { index = 1, level = 1, pos = { x = 100, y = 100 }, finishedTime = 0
                }
            },
            [2] = {
                id = 2,
                [1] = { index = 1, level = 3, pos = { x = 150, y = 150 }, finishedTime = 18948788493
                },
                [2] = { index = 2, level = 3, pos = { x = 200, y = 200 }, finishedTime = 0
                },
                [3] = { index = 3, level = 4, pos = { x = 150, y = 150 }, finishedTime = 18948788493
                },
                [4] = { index = 4, level = 4, pos = { x = 200, y = 200 }, finishedTime = 18948788493
                }
                [5] = { index = 5, level = 3, pos = { x = 300, y = 300 }, finishedTime = 18948788493
                }
            }
        }
    }
}
```

杂物模块 otherItems
----------------------

### canOutPutOtherItems
#####接口说明：可以生产杂物的信息
#####传入参数说明：
```
{
	id -- 杂物Id  不传入可以获得所有
}
```

#####返回参数：
```
{
"ret":0,
"errMsg":"xxxx",
"data":[
	{
	"id":1 --杂物Id
	"name":名字
	"maxNumber":可生产最大数
	"list":[{
		"id":1,--杂物Id
		"type":1,--类型
		}],
		"existNumber":10,-存在个数
		…..
	}
],

}
```

#####错误情况：
```
无
```



### build
#####接口说明：生产杂物
#####传入参数说明：
```
{
	id -- 杂物Id  
	type -- 类型
}
```

#####返回参数：
```
{
"ret":0,
"errMsg":"xxxx",
"index":"1" --这里是字符串 跟以往不一样

}
```

#####错误情况：
```
*已满
```



### canPluck
#####接口说明：能否采集杂物
#####传入参数说明：
```
{
	id -- 杂物Id  
	index - 可传可不传
}
```

#####返回参数：
```
{
	"ret":0,
	"errMsg":"xxxx",
	"data":[
		{
			"id":1,--杂物Id
			"name":xxx,--名称
			"type":1,--种类
			"totalTime":400,--采集所需要时间
			"gold":200,--采集所需要金币
			"gem":1,--得到宝石数量
			"gemPro":55,--得到宝石概率
			"finishTime":0--采集完成时间
			"index":"1",---第几个杂物
			"isGold":true, 是否够黄金采集 true 够 false 不够	
			"isBuilder":true, --是否有足够builder
			"remainTime":0, --剩余时间
			"isGather":false, --- 是否在采集中
	},
	...]
}
```

#####错误情况：
```
*已满
```




### pluck
#####接口说明：采集杂物
#####传入参数说明：
```
{
	id -- 杂物Id  
	index -- 杂物index 
}
```

#####返回参数：
```
{
	"ret":0,
	"errMsg":"xxxx",
	"gold":100,
	"totalTime":1000,
}
```

#####错误情况：
```
*已满
```



### isFinished
#####接口说明：采集杂物完成
#####传入参数说明：
```
{
	id -- 杂物Id  
	index -- 杂物index 
}
```

#####返回参数：
```
{
	"ret":0,
	"errMsg":"xxxx",
}
```

#####错误情况：
```
*已满
```



### finishByGems
#####接口说明：用宝石采集杂物完成
#####传入参数说明：
```
{
	id -- 杂物Id  
	index -- 杂物index 
}
```

#####返回参数：
```
{
    "ret":0,
    "errMsg":"xxxx",
    "useGem":1,
    "addGem":0
}
```

#####错误情况：
```
*已满
```

战斗模块 battle
----------------------

### getResult
计算战斗结果。
####传入参数
```
{
	"destroyedBuildings":"被摧毁的建筑数",
	"totalBuildings":"总建筑数",
	"isTownHallDestroyed":"大厅是否被摧毁",
	"winScore":"胜利奖励积分",
	"loseScore":"失败减去积分"
}
```
####返回参数
```
{
	"isVictory":"是否胜利",
	"destroyRate":"整数，以百分比显示的摧毁比例",
	"score":"积分增减值",
	"stars":"胜利星级"
}
```
####示例传入数据
```
{
	"destroyedBuildings":3,
	"totalBuildings":6,
	"isTownHallDestroyed":false,
	"winScore":20,
	"loseScore":-10
}
```
####示例返回数据
```
{
	"isVictory":true,
	"destroyRate":50,
	"score":7,
	"stars":1
}
```

### getLootResource
计算战斗资源收益。传入的是对方的建筑和资源数据，其中建筑数据是datastore中的原样数据，而资源数据经过了加工处理，服务器计算了当前时刻的产量，客户端计算了掠夺走的数量。返回出来的是服务器可以直接拿去覆盖掉的数据，不需要任何处理。
####传入参数
```json
{
    "architecture":"建筑模块的用户数据",
    "resource":{
        "gold":{
            "collector":[
                {
                    "index":1,
                    "level":2,
                    "currentCount":"服务器返回的当前产量",
                    "lootCount":"被抢走的量"
                },
                ...
            ],
            "storage":[
                {
                    "index":1,
                    "level":3,
                    "currentCount":"当前存储量",
                    "lootCount":"被抢走的量"
                },
                ...
            ]
        },
        "hydrogen":{
            "collector":[
                {
                    "index":1,
                    "level":3,
                    "currentCount":"服务器返回的当前产量",
                    "lootCount":"被抢走的量"
                },
                ...
            ],
            "storage":[
                {
                    "index":1,
                    "level":4,
                    "currentCount":"当前存储量",
                    "lootCount":"被抢走的量"
                },
                ...
            ]
        },
    },
}
```
####传出参数
```json
{
    "resource":{
        "gold":{
            "collector":[
                {
                    "index":1,
                    "level":2
                    "finishedMoment":"产满的完成时刻",
                },
                ...
            ],
            "storage":[
                {
                    "index":1,
                    "level":3,
                    "currentCount":"当前存储量"
                },
                ...
            ]
        },
        "hydrogen":{
            "collector":[
                {
                    "index":1,
                    "level":3,
                    "finishedMoment":"产满的完成时刻",
                },
                ...
            ],
            "storage":[
                {
                    "index":1,
                    "level":4,
                    "currentCount":"当前存储量",
                },
                ...
            ]
        },
    },
}
```

飞船模块 battleship
----------------------

### getStatus
查询飞船状态，包括拥有的武器。
####传入参数
无
####返回参数
```
{
    "canUse":"是否可以使用",
    "remainingTime":"到下次可用的剩余时间",
    "finishedTime":"下次可用的时刻",
    "weapons":[
    	{
    		"id":"武器ID",
    		"count":"武器数量"
    	},
    	...
    ]
}
```
####示例返回数据
```
{
    "canUse":true,
    "remainingTime":0,
    "finishedTime":1360000000,
    "weapons":[
    	{
    		"id":1,
    		"count":1
    	},
    	...
    ]
}
```
### getConfig
查询武器配置。如果不传入武器id参数，则查询所有武器。
####传入数据
```
{
	"id":"可选参数，武器id"
}
```
####返回数据
```
{
	"weapons":[
		{
			"id":"武器ID",
			"武器属性名":"属性值"
		},
		...
	]
}
```
####示例传入数据
```
{}
```
####示例传出数据
```
{
	"weapons":[
		{
			"id":1,
		    "launchTime":2,
		    "damage":1,
		    "goldPrice":250
		},
		{
		    "id":2,
		    "affectTime":10,
		    "goldPrice":100
		},
		{
			"id":3,
			"affectTime":8,
			"goldPrice":200
		},
		...
	]
}
```
### buyWeapon
购买武器。购买的顺序决定了释放的顺序。
如果购买成功，将扣除金币，否则不扣除。
####传入数据
```
{
	"id":"武器ID"
}
```
####返回数据
```
{
	"canBuy":"是否可以购买",
	"lackGold":"缺少的金币数",
	"needGems":"兑换金币所需的宝石数"
}
```
####示例传入数据
```
{
	"id":1
}
```
####示例返回数据
```
{
	"canBuy":false,
	"lackGold":500,
	"needGems":30
}
```
### useWeapon
使用武器。触发飞船冷却。
####传入参数
```
{
	id = "使用的武器id"
}
```
####传出参数
无
####示例传入数据
```
{
	id = 1
}
```

### adjustWeaponsOrder
调整武器顺序。
###传入参数
```
{
	"weapons":[
		排第1位的武器ID,
		排第2位的武器ID,
		排第3位的武器ID,
		...
	]
}
```
###传出参数
无
###示例传入数据
```
{
	"weapons":[2,1,3]
}
```

### finishNow
立即完成飞船的cooldown，并扣除宝石。
###传入参数
无
###传出参数
```
{
    gems = "消费的宝石数"
}
```
###示例传出数据
```
{
    gems = 12
}
```

