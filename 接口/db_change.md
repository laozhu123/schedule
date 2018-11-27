# db_change
## 挪车码
- user_move_car_sms_record(m)

ALTER TABLE `user_move_car_sms_record` ADD `longtitude` char(255) COLLATE utf8_unicode_ci NOT NULL DEFAULT '';
ALTER TABLE `user_move_car_sms_record` ADD `latitude` char(255) COLLATE utf8_unicode_ci NOT NULL DEFAULT '';
ALTER TABLE `user_move_car_sms_record` ADD `image1` char(255) COLLATE utf8_unicode_ci NOT NULL DEFAULT '';
ALTER TABLE `user_move_car_sms_record` ADD `image2` char(255) COLLATE utf8_unicode_ci NOT NULL DEFAULT '';

ALTER TABLE `user_move_car_sms_record` ADD `read_status`  int(11) NOT NULL DEFAULT;



- MvcarExtendOrderDataArchive(new)归档使用  （归档都去除）


## 管家
- our_user(添加permissions_new)


## 爱卡
- car_price_article（添加评论字段）


## 驾考
- t_new_err_num（新增错题数）
- t_user_err_book(用户错题集)
- t_user_right_wrong(用户错题和对题)
- t_answer_record(答题打点表)
- t_user_login_times
- 
- t_exam_record(添加了vip)
- t_vip_user_info
- t_user_vip_order
- t_user_touch_open_record
- t_user_skill_record
- t_skill_num
- t_exam_user_simplify
- t_exam_simplify
- t_exam_user_simplify_test
- t_skill_question
- t_simplify_question
