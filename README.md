# How to use
1. Git clone this
2. ```bash
        sudo mv mrdoc /usr/local/bin/mrdoc
    ```
3. ```bash
        cp env-sample .env
    ```
    And modify it for your needs
4. Place your apps in code/%appname% 
5. Create nginx config in configuration/nginx/sites (examples in congiguration/nginx/conf_example)
6. add in /etc/hosts - 127.0.0.1 %appnam%
7. mrdoc build
8. mrdoc run
9. PROFIT
