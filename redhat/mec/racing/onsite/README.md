
```bash
# 192.168.40.120
# registry.access.redhat.com/openshift3/node:v3.11
# change image to registry.sigma.cmri
oc edit ds ovs-vsctl-amd64

docker pull registry.redhat.io/openshift3/node:v3.11.98
docker tag registry.redhat.io/openshift3/node:v3.11.98 registry.sigma.cmri/openshift3/node:v3.11.98
docker push registry.sigma.cmri/openshift3/node:v3.11.98
docker tag registry.sigma.cmri/openshift3/node:v3.11.98  registry.sigma.cmri/openshift3/node:v3.11
docker push registry.sigma.cmri/openshift3/node:v3.11


docker load -i nttmec_cpu.tar.gz
docker tag da1f6a4a3d15ebc67fe098a9f15cd207de306703584a08da36b6aa527e87cce4 registry.sigma.cmri/test/nttmec_cpu
docker push registry.sigma.cmri/test/nttmec_cpu

docker load -i nttmec_gpu.tar.gz
docker tag 72719d7ba3f5ceaac97e84c146e96b690cfc4d2f24bce577265fc60085aa9d8f registry.sigma.cmri/test/nttmec_gpu
docker push registry.sigma.cmri/test/nttmec_gpu

docker tag 8d799e8b85d80577bd96abae0e9a75b281d43097c5afe8db3eded6aaebbdfc86 registry.sigma.cmri/test/nttmec_gpu:02
docker push registry.sigma.cmri/test/nttmec_gpu:02

docker run --rm -it registry.sigma.cmri/test/nttmec_gpu:02 bash

oc apply -f demo.yaml

oc create serviceaccount mysvcacct -n nvidia
oc adm policy add-scc-to-user privileged system:serviceaccount:myproject:mysvcacct -n nvidia
oc adm policy remove-scc-from-user privileged system:serviceaccount:myproject:mysvcacct -n nvidia
oc adm policy add-scc-to-user privileged -z mysvcacct -n nvidia
oc adm policy remove-scc-from-user privileged -z mysvcacct -n nvidia
oc adm policy add-scc-to-user anyuid -z mysvcacct -n nvidia
oc adm policy remove-scc-from-user anyuid -z mysvcacct -n nvidia

docker run --rm -it registry.sigma.cmri/test/nttmec_cpu bash
docker run --rm -it registry.sigma.cmri/test/nttmec_gpu bash

oc run busybox --image=registry.sigma.cmri/centos/tools --command -- sleep 36000

docker build -t registry.sigma.cmri/test/nttmec_cpu:wzh -f cpu.Dockerfile ./
docker push registry.sigma.cmri/test/nttmec_cpu:wzh
docker build -t registry.sigma.cmri/test/nttmec_gpu:wzh -f gpu.Dockerfile ./
docker push registry.sigma.cmri/test/nttmec_gpu:wzh

yum install atomic-openshift-clients

# to check which user has role
oc edit scc privileged

docker run --rm -it --privileged --security-opt label=type:nvidia_container_t  -p 28080:8080 registry.sigma.cmri/test/nttmec_gpu bash

docker run  --user 1000:1000 --security-opt=no-new-privileges --cap-drop=ALL --security-opt label=type:nvidia_container_t     registry.sigma.cmri/mirrorgooglecontainers/cuda-vector-add:v0.1


/usr/local/cuda-10.1/nvml/example
make
./supportedVgpus

/usr/local/cuda/nvvm/libnvvm-samples
build.sh
cd /usr/local/cuda/nvvm/libnvvm-samples/install/bin
./simple

htpasswd -b /etc/origin/master/htpasswd zw  ****pwd****
oc adm policy add-role-to-user view zw -n zhuowang
oc adm policy add-role-to-user edit zw -n zhuowang

oc adm policy add-role-to-user admin zw -n zhuowang

docker load -i centos-7-nginx-rtmp.tar
docker tag haipenge/centos-7-nginx-rtmp:latest registry.sigma.cmri/test/centos-7-nginx-rtmp:latest
docker push registry.sigma.cmri/test/centos-7-nginx-rtmp:latest

docker run --rm -it --name=rtmp-server registry.sigma.cmri/test/centos-7-nginx-rtmp:latest

docker load -i facego.tar
docker tag facego:v1.1 registry.sigma.cmri/test/facego:v1.1
docker push registry.sigma.cmri/test/facego:v1.1

docker load -i kafka.tar
docker tag wurstmeister/kafka:latest registry.sigma.cmri/test/kafka:latest
docker push registry.sigma.cmri/test/kafka:latest

docker load -i mobilesoldier_v0.2.tar
docker tag mobilesoldier:v0.2 registry.sigma.cmri/test/mobilesoldier:v0.2
docker push registry.sigma.cmri/test/mobilesoldier:v0.2

docker load -i mysql.tar
docker tag mysql-oc:last registry.sigma.cmri/test/mysql-oc:last
docker push registry.sigma.cmri/test/mysql-oc:last

docker load -i ocean-api.tar
docker tag ocean-api:last registry.sigma.cmri/test/ocean-api:last
docker push registry.sigma.cmri/test/ocean-api:last

docker load -i ocean-deletefile.tar
docker tag ocean-deletefile:v1 registry.sigma.cmri/test/ocean-deletefile:v1
docker push registry.sigma.cmri/test/ocean-deletefile:v1

docker load -i ocean-manager.tar
docker tag ocean-manager:v1 registry.sigma.cmri/test/ocean-manager:v1
docker push registry.sigma.cmri/test/ocean-manager:v1

docker load -i ocean-nginx.tar
docker tag nginx-ocean:v1 registry.sigma.cmri/test/nginx-ocean:v1
docker push registry.sigma.cmri/test/nginx-ocean:v1

docker load -i ocean-socket.tar
docker tag ocean-socket:v1 registry.sigma.cmri/test/ocean-socket:v1
docker push registry.sigma.cmri/test/ocean-socket:v1

docker load -i redis.tar
docker tag redis:latest registry.sigma.cmri/test/redis:latest
docker push registry.sigma.cmri/test/redis:latest

docker load -i zookeeper.tar
docker tag zookeeper:latest registry.sigma.cmri/test/zookeeper:latest
docker push registry.sigma.cmri/test/zookeeper:latest

oc create serviceaccount mysvcacct -n zhuowang
oc adm policy add-scc-to-user privileged -z mysvcacct -n zhuowang
oc adm policy add-scc-to-user anyuid -z mysvcacct -n zhuowang
# hostaccess
# hostmount-anyuid
# hostnetwork

```

应用是tensorflow的，发现如果设置的环境变量CUDA_VISIBLE_DEVICES，会指定GPU运行，如果指定到不存在的GPU，就会报错。解决办法，就是把这个环境变量给去掉。