# Using sntop(1) to monitor tinc-ninuxto status

Install sntop

```shell
sudo apt install sntop
cp .../tinc-ninuxto/docs/sntoprc-ninuxto ~/.sntoprc
```

Run sntop(1) from the shell

```shell
sntop
```

Display sntop inside a terminal on the UDOO NEO:

```shell
export DISPLAY=:0
lxterminal --geometry=100x25 --title="snop TINC ninuxto" -e sntop -f sntoprc-ninuxto
```

<!-- EOF -->
