- forked from: https://github.com/sagikimhi/openu-os-20594
- see also: https://github.com/raj-maurya/xv6-public_modifiedOS

```bash
cd maman01

docker build -t maman01-xv6 .

docker run --rm -it --platform linux/amd64 -v "$(pwd)/xv6-01:/maman01" maman01-xv6

make clean qemu
```
