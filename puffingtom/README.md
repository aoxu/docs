Star Legend Lua API
======================

Changelog
----------------------
在v1.1版中对Lua层整体进行了重构，包括模块划分、接口设计和数据设计，主要遵循以下几个原则：
1. 每个模块的相同功能接口，保持相同的命名，并尽量在通用逻辑上复用接口。
2. 相同意义的数据项，保持相同的命名，并且修正拼写错误的命名。
3. 在接口参数中使用名称或ID。
4. 由于避免与lua标准库函数type名字冲突，类型使用genre表示。

用户模块 user
----------------------
跟用户相关的接口，例如查用户的资源数据。

### firstLogin 首次登录初始化
####传入参数
```json
{
    "user":"lua table string"
}
```
####传出参数
{}

### getResource 查询资源量
查询资源量，包括金和氢的当前总量/总容量。实际是操作resource模块的数据。
####传入参数
{}
####传出参数
```json
{
    "gold":{
        "currentCount":"当前金量",
        "capacity":"金容量"
    },
    "hydrogen":{
        "currentCount":"当前氢量",
        "capacity":"氢容量"
    }
}
```

### getUserData 查询用户数据
####传入参数
{}
####传出参数
很多字段，包括保护剩余时间。
```json
{
    "protectionRemainingTime":"保护剩余时间"
}
```

### changeGems 增加/消耗宝石数
原先的`addGem`和`deductGem`合并，改名为`changeGems`。
####传入参数
```json
{
    "gems":"number, 增加用正数，减少用负数"
}
```
####传出参数
```json
{
    "count":"增减后的拥有宝石数"
}
```

### getGems 查询宝石数
原先的`getUserGem`改名为`getGems`。
####传入参数
{}
####传出参数
```json
{
    "count":"gems count"
}
```

### finishGuide 完成新手引导
原先的`setGuideFinish`改名为`finishGuide`。
####传入参数
{}
####传出参数
{}

### getBuildSkills 查询建造技能状态
原先的`getBuilderNumberLimit`和`getIdleBuilder`合并，改名为`getBuildSkills`。
####传入参数
{}
####传出参数
```json
{
    "idleCount":"空闲的建造技能个数",
    "capacity":"建造技能容量"
}
```

### changeBuildSkills 增加/减少建造技能数
原先的`addBuilder`和`deductBuilder`合并，改名为`changeBuildSkills`。
####传入参数
```json
{
    "operation":"add 增加/ deduct减少"
}
```
####传出参数
{}

### changeScore 增加/减少天梯积分
原先的`addScore`改名为`changeScore`。
####传入参数
```json
{
    "score":"变化的积分数，增加用正数，减少用负数"
}
```
####传出参数
```json
{
    "count":"增减后的积分"
}
```

### setFacebookData 设定Facebook数据
原先的`setFbInfo`改名为`setFacebookData`。
####传入参数
```json
{
    "id":"facebook id",
    "name":"facebook name"
}
```
####传出参数
{}

### setProtection 开启/关闭保护
原先的`setProtectionRemainingTime`改名为`setProtection`。
####传入参数
```json
{
    "protectionRemainingTime":"number"
}
```
####传出参数
{}

建筑模块 architecture
----------------------
### canBuild 查询是否满足建造/升级条件
检查指定建筑是否满足建造、升级的各种条件。
####传入参数
```json
{
    "id":"建筑id",
    "index":"同类建筑编号，可选参数，当查询是否可升级时传入"
}
```
####传出参数
```json
{
    "canBuild":"是否可建造/升级",
    "isBuilding":"是否正在建造",
    "isDependArch":"是否满足建筑科技树依赖条件",
    "isResource":"是否有足够的资源（金、氢）",
    "needResource":{
        "gold":"金币不够时，还差的金币数",
        "hydrogen":"氢不够时，还差的氢量"
    },
    "needGems":"资源不够时，兑换资源所需的宝石数",
    "isBuildSkill":"是否有足够的建造技能数"
}
```

### getConfig 查询配置
原先的`getArchConfig`改名为`getConfig`。  
可以查询建筑的相关配置，包括可建造的等级，每个等级的价格，建造时间。
例外的是，只有SpringJump是按建造的个数来定价格和建造时间的，所以SpringJump的配置是按个数排列的，也可以按个数查询，为了简化接口，个数没有新增字段，而是复用level字段来传递。
####传入参数
```json
{
    "id":"可选参数，建筑id",
    "level":"可选参数，建筑等级，如果是查SpringJump，这个参数表示第几个",
    "genre":"可选参数，defense表示查询所有防御建筑"
}
```
####传出参数
```json
{
    "data":"对应建筑配置数据"
}
```

### getUserData 查询用户数据
原先的`getUserArch`改名为`getUserData`。
####传入参数
```json
{
    "id":"architecture id",
    "index":"architecture index"
}
```
####传出参数
对应用户数据

### build 建造/升级建筑
原先的`buildArch`改名为`build`。
####传入参数
```json
{
    "id":"建筑id",
    "index":"在升级时传入"
}
```
####传出参数
```json
{
    "index":"新建时返回新建筑的index"
}
```

### finishNow 快速完成建造/升级建筑
原先的`finishByGems`改名为`finishNow`。
用宝石快速完成。
####传入参数
```json
{
    "id":"建筑id",
    "index":"建筑index"
}
```
####传出参数
```json
{
    "gems":"消费宝石数"
}
```

### getUnlockedDifference 查询CommandCenter解锁的建筑
传入Command Center等级，查询升到这一级会解锁的建筑，包括最高等级和最大数量。
####传入参数
```json
{
    "level":"Command Center level"
}
```
####传出参数
```json
{
    "data":[
        {"id":"建筑id", "name":"名称", "count":"最大数量差", "level":"最高等级"},
        ...
    ]
}
```

### processStatus
计算建筑数据的最新状态。
*传入参数*
```
{
    "data":{
        json string, "architecture":"lua table string"
    }
}
```
*传出参数*
```
{
    "data":"json string"
}
```


英雄模块 hero
----------------------
由于升级英雄祭坛建筑就可以解锁新英雄，所以建造完成的逻辑有变化。

### getUnlockedDifference 查询英雄祭坛解锁的英雄
原先的`getHeroInfo`改名为`getUnlockedDifference`。
####传入参数
```json
{
    "level":"英雄塔等级"
}
```
####传出参数
```json
{
    "data":[
        {
            "id":"hero id",
            "name":"hero name",
            "level":"解锁的英雄最高等级"
        }
    ]
}
```

### getConfig 查询配置
原先的`getHeroConfig`改名为`getConfig`。
####传入参数
```json
{
    "id":"可选参数，hero id",
    "level":"可选参数，hero level"
}
```
####传出参数
```json
{
    "data":"config"
}
```
### getUserData 查询用户数据
原先的`getUserHeroes`改名为`getUserData`。
####传入参数
```json
{
    "id":"可选参数，hero id"
}
```
####传出参数
```json
{
    "data":{
        "id":"英雄id",
        "level":"英雄等级，如果是正在召唤的新英雄，等级为0",
        "isPaused":"是否正在暂停召唤，只有为false时才能用",
        "lives":"现在的生命数",
        "summonFinishedMoment":"召唤完成时刻",
        "summonRemainingTime":"召唤剩余时间",
        "recoverFinishedMoment":"恢复到满血时刻"
    }
}
```

技能模块 skill
----------------------
原来的`skills`改名为`skill`，所有模块名使用单数，跟architecture和hero模块名一致。
由于升级技能研究中心就可以解锁新技能，所以建造完成的逻辑有变化。

### getUnlockedDifference 查询技能学院解锁的技能
原先的`getSkillInstituteInfo`改名为`getUnlockedDifference`。

### getUserData 查询用户数据
原先的`getSkillsList`改名为`getUserData`。

### getConfig 查询配置
####传入参数
```json
{
    "id":"skill id",
    "level":"skill level",
    "genre":"可选参数，传common表示查询公共配置"
}
```
####传出参数
配置数据。


科技模块 tech
---------------------
科技中心负责英雄和技能的升级。实际上科技中心读写的是英雄和技能模块的数据。

### getItems 查询升级项目
查询所有在科技中心里的升级项目，如果级别为0表示玩家还没有这个技能。
####传入参数
{}
####传出参数
数组下标就是id。
```json
{
    "hero":{
        "unlocked":{
            {
                "id":"hero id",
                "level":"current level",
                "price":"upgrade price",
            },
            ...
        },
        "locked":{
            {
                "id":"hero id",
                "level":"current level",
                "price":"upgrade price",
                "dependArchLevel":"depending tech center level",
                "isMaxLevel":"true / false, 是否已满级"
            },
            ...
        }
    },
    "skill":{
        "unlocked":{
            {
                "id":"skill id",
                "level":"current level",
                "price":"upgrade price"
            },
            ...
        },
        "locked":{
            {
                "id":"skill id",
                "level":"current level",
                "price":"upgrade price",
                "dependArchLevel":"depending tech center level",
                "isMaxLevel":"true / false，是否已满级"
            },
            ...
        }
    }
}
```

### canUpgrade 查询是否满足升级的条件
####传入参数
```json
{
    "genre":"hero / skill",
    "id":"hero id / skill id"
}
```
####传出参数
```json
{
    "canUpgrade":"true / false, true表示满足升级的所有条件",
    "isUpgrading":"true / false, false表示当前没有正在升级的项目",
    "isDependArch":"true / false, true表示满足依赖建筑的等级条件",
    "isResource":"true / false, true表示资源足够",
    "isGems":"true / false, true表示宝石足够",
    "needGems":"number, 不够时所需的宝石数"
}
```

### upgrade
####传入参数
```json
{
    "genre":"hero / skill",
    "id":"hero id / skill id"
}
```
####传出参数
{}

### finishNow 立即完成
####传入参数
```json
{
    "genre":"hero / skill",
    "id":"hero id / skill id"
}
```
####传出参数
```json
{
    "gems":"消费的宝石数"
}
```
### getStatus 查询升级状态
查询正在升级的项目，包括英雄和技能。
####传入参数
{}
####传出参数
```
{
    "hero":{
        "id":"hero id",
        "level":"当前等级",
        "finishedMoment":"升级完成时刻",
        "remainingTime":"升级剩余时间"
    },
    "skill":{
        "id":"skill id",
        "level":"当前等级",
        "finishedMoment":"升级完成时刻",
        "remainingTime":"升级剩余时间"
    }
}
```

防御建筑模块 defense
----------------------
模块名从`defenseBuild`改为`defense`。  
原先的防御建筑接口和数据，与建造建筑相关的部分都合并到了architecture模块。只保留了getConfig接口用于查询战斗相关的配置。

### getConfig 查询防御建筑配置
####传入参数
```json
{
    "id":"可选参数，建筑id",
    "level":"可选参数，建筑等级"
}
```
####传出参数
```json
{
    "data":"对应建筑配置"
}
```


资源模块 resource
-----------------------
由于新增了一种资源类型：氢 hydrogen，并且资源建筑拆分为采集和存储两个部分，所以原先的金矿（Goldmine）模块修改为资源模块（Resource）。资源模块对两种资源做相同的逻辑处理，包括采集，存储，消费，宝石兑换，资源建筑的建造升级等。

### getConfig 查询配置
查询 所有/指定等级 的 金/氢 建筑配置信息。
####传入参数
```json
{
    "genre":"gold / hydrogen",
    "arch":"collector / storage",
    "level":"可选参数，建筑等级"
}
```
####传出参数
对应配置信息。

### getUserData 查询用户数据
查询 所有/指定index 的 金/氢 建筑数据。
####传入参数
```json
{
    "genre":"资源类型，gold / hydrogen",
    "arch":"可选参数，建筑名称，collector / storage",
    "index":"可选参数，建筑的index"
}
```
####传出参数
对应用户数据，包括现有资源量，资源上限，以及每个资源建筑的用户数据。

### getStatus
查询所有/指定资源建筑的状态，用于资源建筑状态条的显示。
可以是采集器，也可以是仓库，但是仓库没有时间的字段。
####传入参数
```json
{
    "genre":"资源名称，gold / hydrogen",
    "arch":"建筑名称，collector / storage",
    "index":"可选参数，建筑的index"
}
```
####传出参数
```json
{
    "currentCount":"当前数量",
    "capacity":"最大容量",
    "remainingTime":"产满剩余时间",
    "totalTime":"产满总时间",
    "finishedMoment":"产满时刻"
}
```

### gather 收取采集器
收取采集器已经产出的资源，放到仓库当中。
采集后的资源均匀存放在仓库里。
####传入参数
```json
{
    "genre":"gold / hydrogen",
    "index":"建筑index"
}
```
####传出参数
```json
{
    "gatheredCount":"收取出来的数量",
    "currentCount":"采集器里剩余的数量"
}
```

### changeResource 增减资源
原来的`addGold`和`deductGold`合并。
增加或减少资源，不影响采集器，只把增减体均匀分散到在每个仓库里。
####传入参数
两个可选参数，可以只传入其中一个。
```json
{
    "gold":"增减数，增加用正数，减少用负数",
    "hydrogen":"增减数，增加用正数，减少用负数"
}
```
####传出参数
{}

杂货铺模块 grocery
----------------------
原先的`groceryShop`模块改名为`grocery`。

### getUnlockedDifference 查询杂货铺解锁的新杂货
原先的`getGroceryShopInfo`改名为`getUnlockedDifference`。
####传入参数
```json
{
    "level":"杂货铺等级"
}
```
####传出参数
```json
{
    {
        "id":"grocery id",
        "name":"grocery name",
        "level":"new level"
    },
    ...
}
```

### getConfig 查询配置
####传入参数
```json
{
    "id":"可选参数，杂货id",
    "level":"可选参数，杂货等级"
}
```
####传出参数
配置数据。

### getItems 查询可购买的杂货商品
原先的`getCanBuy`改名为`getItems`。
####传入参数
```json
{
    "id":"grocery id"
}
```
####传出参数

商店模块 shop
----------------------
原先的`gameStore`模块改为`shop`，因为game多余，然后store与datastore里的存储意义有可能造成混淆，shop则与COC里一致。

### getNeedGemsForResource 计算兑换资源所需的宝石
原先的`goldExchangeGem`改名，去除兑换的歧义，这个接口只计算兑换资源需要多少宝石，仅仅是查询，不做兑换操作。由于所需宝石数往往是多个，所以传出参数的`gem`改为`gems`。
####传入参数
两个可选参数，可以只传入其中一个。
```json
{
    "gold":"可选参数，可以为0",
    "hydrogen":"可选参数，可以为0"
}
```
####传出参数
```json
{
    "gems":"number"
}
```

### getResourceProducts 查询资源商品
原先的`exchange`改名为`getResourceProducts`。
####传入参数
{}
####传出参数
```json
{   
    "data":{
        "fullGold":{
            "resource":"need gold count to full",
            "gems":"need gems to buy gold"
        },
        "halfGold":{
            "resource":"need gold count to half full",
            "gems":"need gems to buy gold"
        },
        "fullHydrogen":{
            "resource":"need hydrogen count to full",
            "gems":"need gems to buy hydrogen"
        },
        "halfHydrogen":{
            "resource":"need hydrogen count to half full",
            "gems":"need gems to buy hydrogen"
        }
    }
}
```

### getGemsProducts 查询宝石商品
原先的`getStoreItems`改名为`getGemsProducts`。
####传入参数
{}
####传出参数
```json
{
    "data":[
        {"id":"com.weedo.PuffingTom.Gems.50", "count":50, "price":2.99},
        {"id":"com.weedo.PuffingTom.Gems.150", "count":150, "price":4.99},
        {"id":"com.weedo.PuffingTom.Gems.350", "count":350, "price":9.99},
        {"id":"com.weedo.PuffingTom.Gems.1200", "count":1200, "price":29.99},
        {"id":"com.weedo.PuffingTom.Gems.4000", "count":4000, "price":99.99},
    ]
}
```

### buyGemsProduct 购买宝石商品
原先的`buyItem`改名为`buyGemsProducts`。
####传入参数
```json
{
    "index":"选择购买的宝石商品序号"
}
```
####传出参数
```json
{
    "count":"购买之后的宝石数"
}
```

### getNeedGemsForTime 计算兑换时间所需的宝石
原先的`exchangeTimeToGems`改名为`getNeedGemsForTime`。
这个接口只计算兑换资源需要多少宝石，仅仅是查询，不做兑换操作。
计算的逻辑分为两种，分别是养成和恢复。养成指的是建筑的建造/升级，英雄的解锁/升级，技能的解锁/升级。恢复指的是战斗前的准备，包括英雄回血和技能充能。
####传入参数
```json
{
    "genre":"develop 养成 / recover 恢复",
    "timestamp":"完成时刻"
}
```
####传出参数
```json
{
    "gems":"所需宝石数"
}
```

### buyResource 宝石兑换指定数量的资源
原先的`gemExchangeGold`改名为`buyResource`.
####传入参数
```json
{
    "genre":"gold / hydrogen",
    "gems":"number"
}
```
####传出参数
```json
{
    "gold":"gold added",
    "hydrogen":"hydrogen added"
}
```


进攻模块(探索塔) attack
----------------------
探索塔模块在逻辑层处理的是进攻相关的逻辑，exploreTower改名为attack。这样有3个好处：
1. lua层只关注逻辑，应该以逻辑来划分模块和命名。
2. lua模块与建筑名称无关，这样完全不受建筑改名的影响。
3. 只有一个词，书写更简单。

### getConfig 查询配置
查询探索塔配置，例如最大携带技能容量，一次进攻所需的资源。  
####传入参数
{}
####传出参数
配置信息。

### addToBattle 添加到战备状态
选择英雄、技能添加到战斗准备状态中，技能会开始充能，并扣除购买费用。
####传入参数
```json
{
    "genre":"hero / skill",
    "id":"hero id / skill id"
}
```
####传出参数
{
}

### removeFromBattle 从战斗状态中去掉
从战斗准备状态中去掉英雄、技能。
####传入参数
```json
{
    "genre":"hero / skill",
    "id":"hero id / skill id"
}
```
####传出参数
{}

### getStatus 查询战斗状态
用于界面上的状态显示，包括当前选择的英雄和技能，恢复状态，等级。
场景分为prepare（准备）和attack（攻击），在准备场景中，已经选择的英雄和技能会自动恢复；在战斗场景中，携带到战斗中的英雄和技能不会自动恢复，并且与准备场景中的英雄和技能数据是隔离的。
如果当前满血的英雄个数为0，则无法开始攻击。
如果总的remainingTime为0，说明已经满血或者已经充满技能。
####传入参数
```json
{
    "genre":"hero / skill",
    "scene":"selected / unselected for hero"
}
```
####传出参数
```json
{
    "data":{
        "remainingTime":"所有英雄恢复满血 / 技能充满的总剩余时间",
        "finishedMoment":"所有英雄恢复满血 / 技能充满的完成时刻",
        "readyCount":"当前有血的英雄个数 / 技能无此字段",
        "space":"英雄无此字段 / 选择的技能占用空间",
        "selectedCount":"当前英雄个数 / 技能无此字段",
        "capacity":"英雄容量 / 技能容量"
        "list":[
            {
                "id":"hero id / skill id，注意：list数组下标是排列顺序，不是id",
                "level":"等级",
                "remainingTime":"英雄恢复到满血的剩余时间 / 技能无此字段",
                "readyCount":"当前英雄血量 / 当前充好的技能个数",
                "selectedCount":"英雄满血时的血量 / 已购买的技能个数",
                "nextRemainingTime":"英雄无此字段 / 技能充完当前这一个的剩余时间"
            },
            ...
        ]
    }
}
```

### finishNow 快速完成战斗准备
快速完成所有的英雄回血、技能充能。
####传入参数
```json
{
    "genre":"hero / skill"
}
```
####传出参数
```json
{}
```

### attack 开始攻击
本接口要在匹配到对手时，进入观察模式前调用。
这一刻会以所有已选英雄的当前血量和技能充好的个数，携带进战斗。之后再准备好的技能会移动到战备集合，直到战斗结束，才可以使用。
####传入参数
{}
####传出参数
{}

### deduct 英雄减血 / 使用技能
####传入参数
```json
{
    "genre":"hero / skill",
    "id":"hero id / skill id",
    "num":"可选参数，减少个数，如果不传，为1"
}
```
####传出参数
{}

### cure 治疗英雄
在战斗结束时调用。
####传入参数
```json
{
    "id":"hero id",
    "num":"可选参数，加血数量，默认为1"
}
```
####传出参数
{}

### canAdd 是否可以添加英雄、技能
####传入参数
```json
{
    "genre":"hero / skill",
    "id":"hero id / skill id"
}
```
####传出参数
```json
{
    "canAdd":"true / false",
    "isResource":"true / false, true代表资源足够",
    "isSpace":"true / false, true代表空间足够",
    "needResource":{
        "gold":"资源不够时，所需的金",
        "hydrogen":"资源不够时，所需的氢"
    },
    "needGems":"资源不够时，兑换资源所需的宝石数"
}
```

### getRechargingSkill 查询正在充能的技能
####传入参数
{}
####传入参数
```json
{
    "data":"字段同getStatus，但是只有一个技能的数据。"
}
```

### getAttackPrice 查询进攻费用
####传入参数
{}
####传出参数
```json
{
    "gold":"number",
    "hydrogen":"number"
}
```

### getGameOverGems 查询战斗结束的宝石奖励
####传入参数
```json
{
    "id":"hero id"
}
```
####传出参数
```
{
    "gems":"奖励宝石数"
}
```

定时器模块 timerList
----------------------

### register
#####接口说明：注册充能事件，相同的id，会覆盖数据
#####传入参数说明：
```
{
    rechargeId          --充能id
    callback            --回调函数
    callbackParams      --回调函数参数
    timestamp           --回调触发时间点
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
    rechargeId          --充能id
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
{   "ret":0,
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
计算战斗资源收益。传入的是对方的资源数据。资源数据经过了加工处理，服务器计算了当前时刻的产量，客户端计算了掠夺走的数量。返回出来的是服务器可以直接拿去覆盖掉的数据，不需要任何处理。
####传入参数
```json
{
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
####传入参数
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
####传出参数
无
####示例传入数据
```
{
    "weapons":[2,1,3]
}
```

### finishNow
立即完成飞船的cooldown，并扣除宝石。
####传入参数
无
####传出参数
```
{
    gems = "消费的宝石数"
}
```
####示例传出数据
```
{
    gems = 12
}
```

联盟 alliance
----------------------

### getConfig 查询配置
#### 传入参数
无
#### 传出参数
```json
{
    "data":{
        "capacity":50
    }
}
```

### collectReward 分配悬赏收益
####传入参数
```json
{
    "score":"增减积分，正负整数",
    "gold":"掠夺的金数量",
    "hydrogen":"掠夺的氢数量"
}
```
####传出参数
```json
{
    "offerer":{
        "score":"增减积分",
        "gold":"获得的金数量",
        "hydrogen":"获得的氢数量"
    },
    "attacker":{
        "score":"增减积分",
        "gold":"获得的金数量",
        "hydrogen":"获得的氢数量"
    }
}
```

### canOffer 是否可以发起悬赏
####传入参数
{}
####传出参数
```json
{
    "canOffer":"true/false，是否可以发起悬赏",
    "remainingTime":"悬赏冷却剩余时间",
    "finishedMoment":"冷却结束时刻",
    "needGems":"立即发起悬赏所需的宝石数"
}
```

### finishNow 立即发起悬赏
####传入参数
{}
####传出参数
```json
{
    "gems":"消费的宝石数"
}
```