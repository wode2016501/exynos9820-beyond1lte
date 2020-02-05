# Cruel Kernel Tree for G973F (Samsung S10)

![CI](https://github.com/CruelKernel/exynos9820-beyond1lte/workflows/CI/badge.svg)

Based on samsung sources and android common tree.

## How to install

This kernel works on BSKO && BSL4 firmware.
First of all, TWRP Recovery + multidisabler should be installed in all cases.
It's a preliminary step.

### TWRP

Reboot to TWRP Recovery. Flash boot.img in boot slot.

### Heimdall

Reboot to Download Mode.
```bash
$ sudo heimdall flash --BOOT boot.img
```

### Odin

Steps:
- Pack boot.img to AP.zip
- Reboot to Download Mode.
- Flash AP.zip in AP slot.

### Franco Kernel Manager

Flash boot.img with FK Manager.

## How to customize the kernel

It's possible to customize the kernel and build it from the browser.
First of all, create and account on GitHub. Next, **fork** this repository.
At this step you've got your copy of the sources and you can build it with
GitHub Actions. You need to open github actions [configuration file](.github/workflows/main.yml)
and **edit** it from the browser. For example, to alter the kernel configuration
you need to edit lines:
```YAML
    - name: Kernel Configure
      run: |
        ./build config name=cruel \
                +magisk           \
                +ttl              \
                +wireguard
```

You can change the name of the kernel by replacing ```name=cruel``` with, for example,
```name=my_own_kernel```. You can remove wireguard from the kernel if you don't need it
by changing "+" to "-" or by removing the "+wireguard" line and "\\" on the previous line.

Available configuration presets can be found at [configs](kernel/configs/) folder.
Only the *.conf files prefixed with "cruel" are valuable.
For example:
* magisk - integrates magisk into the kernel. This allows to have root without
  booting from recovery. Enabled by default.
* ttl - adds iptables filters for altering ttl values of network packets. This
  helps to bypass tethering blocking in mobile networks.
* wireguard - adds wireguard module to the kernel.
* insecure - removes kernel protection mechanisms. Potentially, can increase
  the kernel performance. Don't use it if you don't know what you are doing.
* 300hz - increases kernel clock rate from 250hz to 300hz. Potentially, can
  increate response time. Disabled by default, untested.
* 1000hz - increases kernel clock rate from 250hz to 1000hz. Potentially, can
  increate response time. Disabled by default, untested.

After editing the configuration in the browser, save it and **commit**.
Next, you need to **switch** to the "Actions" tab. At this step you will find that
GitHub starts to build the kernel. You need to **wait** about 25-30 mins while the github build
the kernel. If the build is successfully finished, you will find your boot.img in the Artifacts
section.

To keep your version of the sources in sync with main tree, please look at this tutorial:
https://stackoverflow.com/questions/20984802/how-can-i-keep-my-fork-in-sync-without-adding-a-separate-remote/21131381#21131381
