- forked from: https://github.com/sagikimhi/openu-os-20594
- see also: https://github.com/raj-maurya/xv6-public_modifiedOS


# mmn 1

```bash
cd maman01

docker build -t maman01-xv6 .

# docker run --rm -it --platform linux/amd64 -v "$(pwd)/xv6-01:/maman01" maman01-xv6
docker run --rm -it --platform linux/amd64 -v "$(pwd)/xv6-01:/maman01" sagi-maman01-xv6


make clean qemu
```


# mmn 2

```bash
cd maman02
docker build -t maman02-xv6 -f part1-Dockerfile .
docker run --rm -it --platform linux/amd64 -v "$(pwd)/xv6-02:/maman02" maman02-xv6
make clean qemu
```


