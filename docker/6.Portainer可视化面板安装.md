##### 可视化

-----

- portainer

```shell
docker run -d -p 8088:9000 \
--restart=always -v /var/run/docker.sock:/var/run/docker.sock --privileged=true portainer/portainer		# 访问ip:8088
```

- Rancher(CI/CD)

  

------

**Portainer：**Docker图形化界面管理工具！提供一个后台面板供我们操作！

