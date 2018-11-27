mac redis redis-server，目录：/usr/local/bin
homebrew安装redis，redis-cli shutdown没问题


导入、使用其他包全局变量时，变量需要大写，可以在被导入包写一个方法返回该变量，小写即可


defer rs.Close() 使用位置，defer

p, err := profile.ByUserId(a.UserId)的用法，貌似:=左边只要有未声明的变量即可，一个方法体内部使用:=声明了一次，后续本方法体内都不需要声明，:=左边必须要有未声明的变量


log.Fatalln(err)会导致代码阻塞，无法继续执行接下去的代码


jwt简介：https://www.jianshu.com/p/576dbf44b2ae


import (
	"beehashs/middleware"
	"beehashs/models/account"
	"beehashs/models/profile"
	"github.com/astaxie/beego"
	"log"
)

导入的包，不和package对应，和项目文件夹对应.  import对应包后，引用对象和package有关


eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJuYW1lIjoiNTAwOTVlZmJkNDlkNDVmMWFiOTFkM2RlMDUxMzVhYzIiLCJhZG1pbiI6dHJ1ZSwiZXhwIjoxNTIyNDY2OTg2fQ.OFhLw618Q1P8ZEmjF8FFTXpuJ1XOJ_mUKEtvARTMhuA



// 传参？
type GetMealTicketRequest struct {
	ShopId int
	EffectiveTime  string
}

type GetMealTicketReply struct {
	client.ResponseHeader
	Data *MealTicket `json:"data"`
}


serId      int `json:"-"` 

func (this *Service) CreateMealTicket(mt *model.MealTicket) (err error) {
	err = model.CreateUserGroup(sid, shusrId, name)
	if err != nil {
		beego.Debug("err--", err)
		this.Log.Error(err)
	}
	return
}
返回参数err error是否可以为变量 err = model.CreateUserGroup(sid, shusrId, name) return 语句没有返回两个定义的参数？



先编辑install.sh，添加入自己的项目


然后编译项目./install.sh meal_ticket，如果错误，按照提示进行修改


编辑meal_ticket.conf，修改里面的配置


在s4s_v3_build文件目录下
运行项目./bin/meal_ticket -c conf/meal_ticket.conf
./install.sh fast_repair
./bin/fast_repair -c conf/fast_repair.conf

./install.sh api_shop
./bin/api_shop -c conf/api_shop.conf


查看日志 tail -f log/fast_repair.log 
编译go文件 go build main.go




func GetUserTicketsByUid(uid, tid int) (userTickets []*UserTicket, err error) {
	r := rand.Int()
	db, err := DBM.GetSlaveDB(r)
	if err != nil {
		return
	}
	defer DBM.PutDB(r, db)

	sql := "select * from s_user_ticket where user_id =? and ticket_id=?"
	err = db.MFindAll(&userTickets, sql, uid, tid)

	return
}

return


int64直接转int int(i)

git remote add origin http://git.mys4s.cn/backend/meal_ticket.git
git remote add origin http://git.mys4s.cn/backend/fast_repair.git


git push -u origin master

通常我们提交git的时候都是

git add .
git commit -m "some str"
git push
这三大步，而实际上，你只需要两条命令就够了，除非有新的文件要被添加进去。

git commit -am "some str"
git push


`orm:"-" json:",omitempty"`  为空则不输出


// 根据goodsid删除商品信息，update商品信息，置status=1
func (this *Service) DeleteGoodsById(goodsid int) (err error) {
	goods, err := model.GetGoodsById(goodsid) // 如果查找不到记录err!=nil，但是goods不为nil，因为GetGoodsById中已经创建对象了，此处写法有隐患，导致无法找到错误，err向上报出，最后到controller中处理
	goods.Status = 1
	err = model.UpdateGoods(goods)
	if err != nil {
		this.Log.Error(err)
	}
	return
}

func GetGoodsById(id int) (goods *Goods, err error) {
	r := rand.Int()
	db, err := DBM.GetSlaveDB(r)
	if err != nil {
		return
	}
	defer DBM.PutDB(r, db)

	goods = &Goods{}
	sql := fmt.Sprintf("select * from %s where id = ?", goods.TableName())
	beego.Debug(sql)
	err = db.MFindOne(goods, sql, id)
	return
}


api-shop.conf中fast_repair:
[fast_repair]
version=v3
addrs=192.168.18.168:18016
secret=9e1a86c2cf0a282cb24dc0cab1c2fb090c074668df269a2ead646ccb39b1eefde6711d03e1f217ee07e2cf9b2266168d

addrs=127.0.0.1:18016



ssh work@192.168.18.168
2wsx#EDC
cd s4s_test/
cd build/
./fetch_master.sh api_shop
./fetch_master.sh fast_repair
./install.sh fast_repair
./restart_fast_repair.sh 
./install.sh api_shop
./restart_api_shop.sh

./restart_user.sh

./import_mservice.sh fast_repair

https://blog.csdn.net/misakaqunianxiatian/article/details/51103734




This package has installed:
•    Node.js v8.11.1 to /usr/local/bin/node
•    npm v5.6.0 to /usr/local/bin/npm
Make sure that /usr/local/bin is in your $PATH.


coffee-script@1.12.7: CoffeeScript on NPM has moved to "coffeescript" (no hyphen)   
/usr/local/bin/vue -> /usr/local/lib/node_modules/vue-cli/bin/vue
/usr/local/bin/vue-init -> /usr/local/lib/node_modules/vue-cli/bin/vue-init
/usr/local/bin/vue-list -> /usr/local/lib/node_modules/vue-cli/bin/vue-list


problem
// 小b端获取店铺会员时，gateway需要调用user批量查询用户信息微服务接口
type GetUserInfoByIdsRequest struct {
UserIds []int
Sense   bool
}

type GetUserInfoByIdsReply struct {
client.ResponseHeader
Data []*user.User // 调用user中model?
}


不能跨微服务调用别的model里面的struct



interface赋值int类型，再取出来进行int判断转换时
// 遍历
size := len(userInfos)
for i := 0; i < size; i++ {
for j := 0; j < size; j++ {
uid := int(userInfos[i]["UserId"].(float64))
if cds[j].UserId == uid {
cds[j].User = userInfos[i]
break
}
}
}
