# netcat

当ubuntu和MacOS置于同一网络下的时候可以使用netcat进行互传

- IP地址

  - ubuntu下获得IP地址

    使用ifconfig，最后一项wlo1中含有Linux的IP地址

  - macOS下获得IP地址

    路径：系统偏好设置 -> 网络 -> 高级 -> TCP/IP

    即可找到IPv4地址

- netcat语句

  - 接收方监听语句

    ```zsh
    nc -l -p 12345 > file_name
    ```

  - 发送方发送语句

    ```zsh
    nc receive_IP 12345 < /path/to/file
    ```

    
