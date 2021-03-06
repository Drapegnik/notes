# useful scripts

- checkout `env` vars: [#script]

```bash
printenv | grep PATH
```

## commands & tricks

> check out [`@b0rk`](https://twitter.com/b0rk) on twitter!

### bash

[#bash #tricks #cheat-sheet]

<p align="center">
  <img alt="bash" src="http://res.cloudinary.com/dzsjwgjii/image/upload/v1527380485/bash.jpg"/>
</p>

## grep

[#grep]

<p align="center">
  <img alt="grep" src="http://res.cloudinary.com/dzsjwgjii/image/upload/v1527380485/grep.jpg"/>
</p>

## xargs

[#xargs #pipe]

<p align="center">
  <img alt="xargs" src="http://res.cloudinary.com/dzsjwgjii/image/upload/v1527380485/xargs.jpg"/>
</p>

## ssh

copying public key on server using ssh

[#ssh #remote #server]

```bash
cat ~/.ssh/id_rsa.pub | ssh username@remote_host "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"
```

## wait

[#wait #bash #timeout #port]

[wait-for-it](https://github.com/vishnubob/wait-for-it)

```bash
./wait-for-it.sh -t 0 www.google.com:80 -- echo "google is up"
```
