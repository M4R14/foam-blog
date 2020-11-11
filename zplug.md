# zplug

[website](https://github.com/zplug/zplug)

## ติดตั้ง

1. ติดตั้ง [zplug](https://github.com/zplug/zplug) ด้วยคำสั่ง `$ curl -sL --proto-redir -all,https https://raw.githubusercontent.com/zplug/installer/master/installer.zsh | zsh`
2. `$ code  ~/.zshrc`
3. เพิ่ม
    ```.zshrc
        source ~/.zplug/init.zsh

        # Load theme file
        zplug 'dracula/zsh', as:theme

        # Install plugins if there are plugins that have not been installed
        
        if ! zplug check --verbose; then
            printf "Install? [y/N]: "
            if read -q; then
                echo; zplug install
            fi
        fi

        # Then, source plugins and add commands to $PATH
        zplug load --verbose
    ```
4. `$ zsh`
