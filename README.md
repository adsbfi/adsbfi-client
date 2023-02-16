# adsb.fi feed client

- These scripts aid in setting up your receiver to feed [adsb.fi](https://adsb.fi/).
- They will not disrupt any existing feed clients already present.

## 1: Find coordinates and elevation

<https://www.freemaptools.com/elevation-finder.htm>

## 2: Install the feed client
```
curl -L -o /tmp/feed.sh https://adsb.fi/feed.sh
sudo bash /tmp/feed.sh
```

## 3: Check your feed is working

If the feeder is on your home network, the website will show its status.
- <https://adsb.fi/>


Alternatively use netstat to check that your feeder is connected.

The feed IP for adsb.fi is 65.109.2.208

```
netstat -t -n | grep -E '30004|31090'
```
Expected Output:
```
tcp        0    182 localhost:43530     65.109.2.208:31090      ESTABLISHED
tcp        0    410 localhost:47332     65.109.2.208:30004      ESTABLISHED
```

## 4: Optional: Install [local interface](https://github.com/adsbfi/tar1090) for your data

The interface will be available at http://192.168.X.XX/adsbfi  
Replace the IP address with the address of your Raspberry Pi.

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
curl -L -o /tmp/update.sh https://adsb.fi/update.sh
sudo bash /tmp/update.sh
```

### If you encounter issues, please do a reboot and then supply these logs on Discord (last 20 lines for each is sufficient)

```
sudo journalctl -u adsbfi-feed --no-pager
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
curl -L -o /tmp/feed.sh https://adsb.fi/feed.sh
sudo bash /tmp/feed.sh
```

### Disable / Enable adsb.fi MLAT-results in your main decoder interface (readsb / dump1090-fa)

This is enabled by default. You probably don't need to change that.

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

### Restart the feed client

```
sudo systemctl restart adsbfi-feed
sudo systemctl restart adsbfi-mlat
```

### Show status

```
sudo systemctl status adsbfi-feed
sudo systemctl status adsbfi-mlat
```

### Removal / disabling the services

```
sudo bash /usr/local/share/adsbfi/uninstall.sh
```

Alternatively you can disable the services and the feed client won't run anymore.

```
sudo systemctl disable --now adsbfi-feed
sudo systemctl disable --now adsbfi-mlat
```
