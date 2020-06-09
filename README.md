nmcli
=====

nmcli is a python wrapper library for the network-manager cli client.

## Quick Sample

Here is a simple usecase.

```python
import nmcli

try:
    print(nmcli.connection())
    print(nmcli.device())
    print(nmcli.device.wifi())
    print(nmcli.general())

    nmcli.device.wifi_connect('AP1', 'passphrase')
    nmcli.connection.modify('AP1', {
            'ipv4.addresses': '192.168.1.1/24',
            'ipv4.gateway': '192.168.1.255',
            'ipv4.method': 'manual'
        })
    nmcli.connection.down('AP1')
    nmcli.connection.up('AP1')
    nmcli.connection.delete('AP1')
except Exception as e:
    print(e)
```

## Dependency

* NetworkManager
  * `sudo apt install network-manager` (Debian)
* User who can execute nmcli with sudo with NOPASSWD

## API

### connection

#### nmcli.connection

Get a list of connections.

```
nmcli.connection() -> List[Connection]
```

#### nmcli.connection.add

Create a new connection using specified properties.

```
nmcli.connection.add(
    conn_type: str,
    options: Optional[ConnectionOptions] = None,
    ifname: str = "*",
    name: str = None,
    autoconnect: bool = False) -> None
```

#### nmcli.connection.modify

Add, modify or remove properties in the connection profile.

```
nmcli.connection.modify(name: str, options: ConnectionOptions) -> None
```

#### nmcli.connection.delete

Delete a configured connection.

```
nmcli.connection.delete(name: str) -> None
```

#### nmcli.connection.up

Activate a connection.

```
nmcli.connection.up(name: str) -> None
```

#### nmcli.connection.down

Deactivate a connection from a device without preventing the device from further auto-activation.

```
nmcli.connection.down(name: str) -> None
```

#### nmcli.connection.show

Show details for specified connections.

```
nmcli.connection.show(name: str) -> ConnectionDetails
```

### device

#### nmcli.device

Print status of devices.

```
nmcli.device() -> List[Device]
```

#### nmcli.device.wifi

List available Wi-Fi access points.

```
nmcli.device.wifi() -> List[DeviceWifi]
```

#### nmcli.device.wifi_connect

Connect to a Wi-Fi network specified by SSID or BSSID.

```
nmcli.device.wifi_connect(ssid: str, password: str) -> None
```

### general

#### nmcli.general

Show overall status of NetworkManager.

```
nmcli.general() -> General
```

### dummy

#### DummyConnectionControl

ConnectionControl dummy class.

```
DummyConnectionControl(
    result_call: List[Connection] = None,
    result_show: ConnectionDetails = None,
    raise_error: Exception = None
)
```

* Parameters
    * Optional
        * result_call
            * Execution result of `__call__`.
        * result_show
            * Execution result of `show`.
        * raise_error
            * Error that occurs when executing the method.

#### DummyDeviceControl

DeviceControl dummy class.

```
DummyDeviceControl(
    result_call: List[Device] = None,
    result_wifi: List[DeviceWifi] = None,
    raise_error: Exception = None
)
```

* Parameters
    * Optional
        * result_call
            * Execution result of `__call__`.
        * result_wifi
            * Execution result of `wifi`.
        * raise_error
            * Error that occurs when executing the method.


#### DummyGeneralControl

GeneralControl dummy class.

```
DummyGeneralControl(result_call: General = None, raise_error: Exception = None)
```

* Parameters
    * Optional
        * result_call
            * Execution result of `__call__`.
        * raise_error
            * Error that occurs when executing the method.

## Change Log

### 0.2.2

Fix the parsing bug of connection names that contain spaces (connection.show).

Added getting items that begin with a capital letter.

### 0.2.1

Fix the parsing bug of connection names that contain spaces.

### 0.2.0

Added dummy classes.

### 0.1.0

Initial release.

## License

MIT
