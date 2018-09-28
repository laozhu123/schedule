### sql

#### 查询数量
```
var totals[]int
	_, err = model.NewOrm().Raw("SELECT count(*) from device join shop_device on device.id = shop_device.device_id" +
		" where shop_device.shop_id = ? and shop_device.status = 0 and device.shop_type = 0", shopId).QueryRows(&totals)
```

```
sql := fmt.Sprintf("select count(*) from (select t1.*,car_in_shop.status as status1 from " +
			"(select * from staff_car_no where shop_id = ? and status = 0) as t1 left join car_in_shop " +
			"on t1.car_no = car_in_shop.car_no) as t2 where t2.status1 != 0 and t2.car_type_id in (%s)",
			strings.Trim(strings.Join(strings.Fields(fmt.Sprint(cls)), ", "), "[]"))
		err = model.NewOrm().Raw(sql, shopId).QueryRow(&num)
```