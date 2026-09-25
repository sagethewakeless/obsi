%% Maybe write description one day %%

Check the current value:

```bash
sysctl vm.swappiness
```

Set temporary (values between 0-200):

```bash
sudo sysctl -w vm.swappiness=<value>
```

To set permanently, create `/etc/sysctl.d/99-swappiness.conf` with:

```
vm.swappiness = <value>
```