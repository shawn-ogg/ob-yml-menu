# ob-yml-menu
Openbox yaml menu generator


For everybody who loves Openbox but hates the xml configuration files.

I sometimes change my openbox menu (new programs, pipemenus, etc.) and
was looking for a slim solution.

The yaml version of it is quite elegant:
```
---
openbox_menu:
    - Terminal: $terminal  # defaults to xterm. Use eg. --terminal rxvt for a different one
    - File Manager: $terminal -e mc
    - separator:
    - Development:
        - Geany:  # defaults to 'geany'
        - Meld:
        - PyCharm:
        - VS Code: code
        - Vim: $terminal -e vim
    - Browser:
        - Chromium:
    - Mail/Chat:
        - Thunderbird:
        - Telegram: telegram-desktop
    - Multimedia:
        - Audacious:
        - Electronplayer:
        - PulseAudio Volume Control: pavucontrol
    - Pipemenus:
        - Remote Hosts: |
            ~/bin/bl-sshconfig-pipemenu
    - System:
        - htop: $terminal -e htop
        - Openbox:
            - Reconfigure: openbox --reconfigure
    - separator:
    - Logout: oblogout
```

A couple of things to notice:
- The root node has to be named `openbox_menu`
- The `$terminal` variable gets substituted to whatever you want (xterm by default)
- If you don't specify a value for a menu item it will take the lowercase key
- Pipemenu entries start with a pipe `|` :-)


### Usage
```
usage: ob-yml-menu [-h] [--terminal TERMINAL] yml_file

Convert yml file to openbox xml format

positional arguments:
  yml_file             openbox menu in yml format

optional arguments:
  -h, --help           show this help message and exit
  --terminal TERMINAL  default terminal to use (defaults to xterm if not set)
```

So a possible invocation would be like this (_beware it will overwrite your menu.xml!_):
```
./ob-yml-menu --terminal terminator example.yml > ~/.config/openbox/menu.xml
```

### Installation Notes

On **Debian/Ubuntu** flavored systems you may need to install PyYAML:
```
sudo apt-get install python3-yaml
```

You can use an **AUR** package on **Arch Linux**:\
https://aur.archlinux.org/packages/ob-yml-menu/


