# CI&CD

## jenkins

## pipeline

1. 新建 pipeline


use local docker

- /var/run/docker.sock:/var/run/docker.sock 
- /usr/local/bin/docker:/usr/bin/docker

```yaml
version: '3.7'

services:
  jenkins:
    image: jenkins/jenkins
    restart: always
    environment:
      - JENKINS_PASSWORD=123456
    volumes:
      - /Users/bryce/docker/datas/jenkins:/var/jenkins_home
      - /var/run/docker.sock:/var/run/docker.sock 
      - /usr/local/bin/docker:/usr/bin/docker
    ports:
      - 6001:8080
      - 50000:50000


```

jenkins node ssh 需要手动登陆一下才能成功

Ansible 解决自动部署问题
## Reference

[jenkins](https://www.w3cschool.cn/jenkins/jenkins-173a28n4.html)