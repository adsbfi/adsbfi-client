# adsb.fi feed client

- These scripts aid in setting up your current ADS-B receiver to feed adsb.fi.
- They will not disrupt any existing feed clients already present

## 1: Find coordinates / elevation:

<https://www.freemaptools.com/elevation-finder.htm>

## 2: Install the adsbfi feed client
```
curl -L -o /tmp/adsbfi.sh https://adsb.fi/feed.sh
sudo bash /tmp/adsbfi.sh
```

## 3: Check these thid URL to check if your feed is working

- <https://adsb.fi/>

### Optional: local interface for your data http://192.168.X.XX/adsbfi

Install / Update:
```
sudo bash /usr/local/share/adsbfi/git/install-or-update-interface.sh
```
Remove:
```
sudo bash /usr/local/share/tar1090/uninstall.sh adsbfi
```

### Update the feed client without reconfiguring

```
curl -L -o /tmp/fiupdate.sh https://adsb.fi/feed-update.sh
sudo bash /tmp/fiupdate.sh
```


### If you encounter issues, please do a reboot and then supply these logs on the forum (last 20 lines for each is sufficient):

```
sudo journalctl -u  adsbfi-feed --no-pager
sudo journalctl -u adsbfi-mlat --no-pager
```


### Display the configuration

```
cat /etc/default/adsbfi
```

### Changing the configuration

This is the same as the initial installation.
If the client is up to date it should not take as long as the original installation,
otherwise this will also update the client which will take a moment.

```
curl -L -o /tmp/adsbfi.sh https://adsb.fi/feed.sh
sudo bash /tmp/adsbfi.sh
```

### Disable / Enable adsbfi MLAT-results in your main decoder interface (readsb / dump1090-fa)

- Disable:

```
sudo sed --follow-symlinks -i -e 's/RESULTS=.*/RESULTS=""/' /etc/default/adsbfi
sudo systemctl restart adsbfi-mlat
```
- Enable:

```
sudo sed --follow-symlinks -i -e 's/RESULTS=.*/RESULTS="--results beast,connect,127.0.0.1:30104"/' /etc/default/adsbfi
sudo systemctl restart adsbfi-mlat
```

### Restart

```
sudo systemctl restart adsbfi-feed
sudo systemctl restart adsbfi-mlat
```


### Systemd Status

```
sudo systemctl status adsbfi-mlat
sudo systemctl status adsbfi-feed
```


### Removal / disabling the services:

```
sudo bash /usr/local/share/adsbfi/uninstall.sh
```

If the above doesn't work, you may be using an old version that didn't have the uninstall script, just disable the services and the scripts won't run anymore:

```
sudo systemctl disable --now adsbfi-feed
sudo systemctl disable --now adsbfi-mlat
```
