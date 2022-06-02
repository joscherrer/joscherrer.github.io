
## Dotfiles

Install yadm
```bash
# For ubuntu
sudo apt install yadm

# For arch
yay -Syu yadm-git

# For RHEL
curl -fLo /usr/local/bin/yadm https://github.com/TheLocehiliosan/yadm/raw/master/yadm && chmod a+x /usr/local/bin/yadm
```

Then install dotfiles

```bash
# Bootstrap only works on Ubuntu for now
yadm clone git@github.com:joscherrer/dotfiles.git --bootstrap
```
