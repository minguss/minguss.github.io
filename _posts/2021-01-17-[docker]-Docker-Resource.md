---
layout: post
title:  "[docker]-맥북에서 Docker Resource 줄이기"
author: minguss
categories: devlog
tags: MAC docker
---

Docker build를 거의 30분에 한번꼴로 돌리는데, 이러다가 맥이 망가지는거 아닌가 하는 걱정이 들었다.  
주로 잡아먹는 영역이 Docker와 VSCode인데, VSCode는 꼭 필수적인 컴포넌트와 Workspace를 정말 작업공간만 열어놓고 쓰니까 한결 괜찮아졌다.  
Docker build는 여전히 리소스를 많이 먹는데, 조금이나마 fan돌아가는데는 도움이 되었던 것 같다.  

![1](https://user-images.githubusercontent.com/22410442/105340043-baf10a80-5c20-11eb-8631-e3e0f53a3d86.png)


기본적으로 위와같이 CPU를 6개를 잡아먹는데, 이걸 1-2개로 줄여봤다.  
어차피 Docker build는 한 프로세스에서 잡아먹으며, docker image개발용이라면 1-2개로도 충분하더라

![2](https://user-images.githubusercontent.com/22410442/105340244-fc81b580-5c20-11eb-83d8-7bf9bd2c840e.png)
