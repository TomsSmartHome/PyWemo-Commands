# PyWemo-Commands

Here are the steps to install [PyWemo](https://github.com/pywemo/pywemo) and its dependencies to have your Wemo devices connect to Wifi without needing the Belkin app or cloud.

## Prerequisites

I used the Windows store, but you can also install Python 3 from [Python.org](https://www.python.org/downloads/).

Once it's installed, open PowerShell / Terminal and run this command to verify it can be seen:

```bash
python3 -V
```

Then install PyWemo:

```bash
pip3 install pywemo
```

OpenSSL is required for connecting to wifi.

**These steps are Windows-specific.** I don't have Mac OS or Linux, but for those it might already be installed.

Open a Powershell window as Admin, then search for OpenSSL:

```bash
winget search openssl
```

Copy the "FireDaemon.OpenSSL" value from that command for the next step.

Install OpenSSL:

```bash
winget install --id=FireDaemon.OpenSSL -e
```

Add OpenSSL to your Env PATH:

```bash
$Env:PATH += ";C:Program FilesFireDaemon OpenSSL 3bin"
```

Verify OpenSSL installation worked with:

```bash
openssl --version
```

You might need to close and re-open your shell (or just reboot) for some of the above commands to take effect.

## Connecting device to wifi

After connecting to the device's wifi network on your computer, start Python:

```bash
python3
```

And then run the following:

```python
import pywemo
devices = pywemo.discover_devices()
devices[0].toggle() # Only if you want to test it works before continuing.
devices[0].setup(ssid='Your wifi name', password='your wifi password/network key')
```

If PyWemo doesn't discover anything, you might need to try connecting manually (or your device might not be supported). [This Pywemo page](https://github.com/pywemo/pywemo?tab=readme-ov-file#if-discovery-doesnt-work-on-your-network) has more details.

After a few moments, the Wemo device reboots and connects to your wifi. Home Assistant should then auto-discover it!

If you need to factory reset your Wemo device, connect to it like above but run this instead of connecting to wifi:

```bash
devices[0].reset(data=True, wifi=True)
```
