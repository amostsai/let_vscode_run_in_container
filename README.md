# let vscode run in container

1. 在host執行xhost
2. cd {your project folder}
3. nano docker-compose.yml
4. 貼上以下內容
```
version: "3"
services:
    ubuntu:
        image: amostsai/ubuntu_base:latest
        container_name: ubuntu_dev
        environment:
            - DISPLAY=$DISPLAY
            - LC_MEASUREMENT=zh_TW.UTF-8
            - LC_PAPER=zh_TW.UTF-8
            - LC_MONETARY=zh_TW.UTF-8
            - LANG=zh_TW.UTF-8
            - LC_NAME=zh_TW.UTF-8
            - LC_ADDRESS=zh_TW.UTF-8
            - LC_NUMERIC=zh_TW.UTF-8
            - LANGUAGE=zh_TW:en_US:en
            - LC_TELEPHONE=zh_TW.UTF-8
            - LC_IDENTIFICATION=zh_TW.UTF-8
            - LC_TIME=zh_TW.UTF-8
        volumes:
            - /tmp/.X11-unix:/tmp/.X11-unix
            - $HOME/.Xauthority:/root/.Xauthority
            - /etc/localtime:/etc/localtime:ro
            - /usr/lib:/usr/lib:ro
            - /lib:/lib:ro
            - $HOME/.config/Code:/root/.config/Code
            - $HOME/project:/root/project
            - $HOME/sample_code:/root/sample_code
            - $HOME/.vscode:/root/.vscode
            - /usr/share:/usr/share:ro
        devices:
            - /dev/snd:/dev/snd
        network_mode: "host"
        working_dir: /usr/bin
        command: >
          sh -c "ln -s /usr/share/code/bin/code /usr/bin/code2 &&
                 tail -F anything"

5. docker-compose up
6. docker exec -it ubuntu_dev bash
7. code2 --user-data-dir=.  /tmp
```

## 補充:如果要讓vscode能追蹤第三方函數庫
1. 打開vscode
2. 點左側「除錯」鈕
3. 點左側上方的「齒輪」開啟launch.json
4. 加上「"debugStdLib": true」
