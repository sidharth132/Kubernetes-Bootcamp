1. docker run -d --name my-nginx-container --memory 512m --cpus 1 nginx
2. ps aux | grep '[n]ginx' | sort -n -k 2 | head -n 1 | awk '{print $2}'
3. lsns -p pid
4. systemd-cgls --no-pager
5. cat /sys/fs/cgroup/memory/system.slice/docker-5ba642ac2146b6d7f2c538d673a480f2ab6a4cec8142eae034286fdefcb5d024.scope/memory.stat # need to change container ID

6. cat memory.stat

Kubernetes 
=========
1. crictl ps # use for check container status
2. kubectl run nginx --image=nginx 
