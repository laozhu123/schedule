ssh node3
su work

cd /home/work/s4s_build_client/build
./build.sh api_admin_v5
./build.sh api_user
kubectl set image deployment/api-user -n grey api-user=registry-vpc.cn-shanghai.aliyuncs.com/s4s/api-user:v20190318142358
kubectl set image deployment/api-admin-v5 -n grey api-admin-v5=registry-vpc.cn-shanghai.aliyuncs.com/s4s/api-admin-v5:v20190318112507
kubectl set image deployment/api-vio -n grey api-vio=registry-vpc.cn-shanghai.aliyuncs.com/s4s/api-vio:v20190119160407
kubectl set image deployment/violation -n grey violation=registry-vpc.cn-shanghai.aliyuncs.com/s4s/violation:v20190314155645
kubectl set image deployment/inspection -n grey inspection=registry-vpc.cn-shanghai.aliyuncs.com/s4s/inspection:v20190225175815
kubectl set image deployment/client-mix -n grey client-mix=registry-vpc.cn-shanghai.aliyuncs.com/s4s/client-mix:v20190318112033

