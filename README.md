# WSL (Windows Subsystem for Linux) | Linux integration on Windows

## References

* [WSL Documentation](https://docs.microsoft.com/fr-fr/windows/wsl/)
* [Activate WSL on Windows](https://lecrabeinfo.net/installer-wsl-windows-subsystem-for-linux-sur-windows-10.html)
* [Docker Desktop WSL2 integration on Windows](https://docs.docker.com/desktop/windows/wsl/)
* [Share USB devices with Hyper-V and WSL guests](https://github.com/dorssel/usbipd-win)
* [Kali Linux desktop integration](https://www.kali.org/docs/wsl/win-kex/)

## Share USB devices with WSL machines

### Windows host :

* Install `usbipd-win` via Powershell :
```
winget install --interactive --exact dorssel.usbipd-win
```

* List all USB devices :
```
usbipd list
```

* Share `2-10` device :
```
usbipd bind --busid=2-10
```

### WSL host :

* Install `usbip` package :
```
sudo apt update
sudo apt install linux-tools-5.4.0-77-generic hwdata
sudo update-alternatives --install /usr/local/bin/usbip usbip /usr/lib/linux-tools/5.4.0-77-generic/usbip 20
```

* List shared devices from `192.168.128.1` host (Windows host) :
```
usbip list -r192.168.128.1
```

* Attach `2-10` device from `192.168.128.1` host :
```
usbip attach -r192.168.128.1 -b2-10
```

## Add Windows integration for Kali Linux WSL2

* Enable WSL on Windows
* Install Kali on the Microsoft Store
* Install entire Kali system :
```
sudo apt install -y kali-linux-large
```

* Install `Win-KeX` package :
```
sudo apt update
sudo apt install -y kali-win-kex
```

* Start `Win-KeX` service :
> Window mode :
```
kex --win -s
```
> Enhanced session mode :
```
kex --esm --ip -s
```
> Seamless mode :
```
kex --sl -s
```

* Add following configuration to Windows terminal configuration JSON file :
```
{
        "guid": "{55ca431a-3a87-5fb3-83cd-11ececc031d2}",
        "hidden": false,
        "name": "Win-KeX",
        "commandline": "wsl -d kali-linux kex --sl --wtstart -s",
        "startingDirectory" : "//wsl$/kali-linux/home/kevin"
},
```
