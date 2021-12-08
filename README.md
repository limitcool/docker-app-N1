# docker-app
## N1自用docker-compose项目合集

模板文件是使用 `Compose` 的核心，涉及到的指令关键字也比较多。但大家不用担心，这里面大部分指令跟 `docker run` 相关参数的含义都是类似的。

默认的模板文件名称为 `docker-compose.yml`，格式为 YAML 格式。

```yaml
version: "3"

services:
  webapp:
    image: examples/web #镜像名称
    ports:
      - "80:80" # 宿主机端口:容器内端口
    volumes:
      - "/data:/data" # 宿主机目录:容器内目录
```

### 以N1实例

  n1-media:
  # 镜像源地址
```yaml
image: taoskycn/n1-media-server:latest
# 容器名
container_name: n1
# 环境内部设置
environment:
# 主机名
  - hostname=n1
  - name=n1
# 卷
volumes:
# 系统内文件:Docker内文件
  - ./hass/config:/config
  - ./hass/localtime:/etc/localtime:ro
# 系统内端口:docker内端口
ports:
 - "8124:8123"
```

## 不支持arm架构的docker-app

- nginx-webui
- euterpe



n1-media-server

```bash
docker run -it -d --name N1 -p 1001-1006:1001-1006 -p 51413:51413 -p 548:548 --privileged=true --mount type=bind,source=/mnt/disk/docker-volume/n1-media/exmedia,target=/media/exmedia taoskycn/n1-media-server:0.2 /bin/ash
```

1. 进入容器 `docker exec -it N1 /bin/ash`
2. 启动各个服务`/root/start.sh` 回车
3. 如需停止 `/root/stop.sh`
4. 退出容器 `exit`

## 访问和设置

- Transmission Web：`http://<N1的IP>:1001`，账号密码`transmission`:`transmission`，已更换WebUI，配置文件位于`/root/.config/transmission-daemon/settings.json`。
- Caddy 文件浏览：`http://<N1的IP>:1002`，账号密码`caddy`:`caddy`，显示外接存储下的`download`目录，配置文件位于`/etc/Caddyfile`。
- Caddy WebDAV：`http://<N1的IP>:1002/webdav`，账号密码`caddy`:`caddy`，显示外接存储下的`download`目录，可配合`Potplayer`, `Nplayer`, `Kodi`等软件用于播放视频。
- FileBrowser文件管理：`http://<N1的IP>:1003`，账号密码`caddy`:`caddy`，可在Web界面修改各项设置。
- **（新增）** AFP服务：在Mac OS X的Finder下`Command + K` 输入`afp://192.168.2.107` 点击连接，默认账号密码`afp`:`afp`，可通过`passwd afp`进行密码修改，该服务端口为`548`，外网访问需映射该端口。
- 如需外网访问，使用路由器设置端口映射即可，考虑到安全因素，建议修改密码后操作**

## 注意事项

- `docker run`会映射端口，运行前确认端口未被使用。
- “51413”为transmission交换数据的端口，需要映射到公网（没有好像会影响上传）
- 如需手动修改Transmission配置文件，先退出进程。
- 如外网访问各服务问题，可能是运营商屏蔽了特殊端口，可尝试更换端口。

