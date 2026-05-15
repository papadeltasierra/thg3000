# Strategies

## Possible Strategies
All the following strategies begin with getting `root` access to the router, either via the serial port of maybe using [AutoFlashGUI].
1. Standard [OpenWRT] configuration e.g. [Bridged AP over Ethernet]
1. Hidden [OpenWRT] commands
1. Ben Waterson's [Technicolor-Exploit]
1. Complete flashing with [OpenWRT]


## Standard [OpenWRT] Configuration
- Save existing configuration twice, 1 hour apart and compare
   - They should be the same proving there is no time-sensitive information in the configuration stored
- Get `root` access
- Use standard [OpenWRT] programming to enable [Bridged AP over Ethernet]
- Save configuration via standard GUI
- Confirm that configuration storage file has changed
- Test router to confirm it all works as expected
- Reboot the router and retest to prove that configuration was restored correctly over reboot.

### Sidequest
- Figure out how the configuration is encrypted
- Unsign it and look at the configuration
- Test whether this would work for a vanilla router
    - Drop configuration
    - Decrypt configuration
    - Modify configuration to enable [Bridge AP over Ethernet]
    - Encrypt configuration
    - Load configuration back onto the router

## Hidden [OpenWRT] Configuration
- Determine whether there are unused [OpenWRT] GUI interfaces that could be leveraged

## Ben Waterson's [Technicolor-Exploit]
- So can we flash with custom firmware?  Maybe?

## Complete Reflashing with [OpenWRT]
> This will not be possible if the Broadcom processor is hardened to not accept unencyrpted and unsigned images and if this is the case, we will be unable to determine out what the correct encryption key or signing key are.
- Build [OpenWRT] images
- Figure out how to reflash router
- Flash [OpenWRT].

[hacking Technicolor Gateways]: https://hack-technicolor.readthedocs.io/en/stable/
[technicolor-tg799vac-hacks]: https://github.com/davidjb/technicolor-tg799vac-hacks
[AutoflashGUI]: https://github.com/mswhirl/autoflashgui/blob/master/README.md
[Bridged AP over Ethernet]: https://openwrt.org/docs/guide-user/network/wifi/wifiextenders/bridgedap
[OpenWRT]: https://openwrt.org/docs/start
[technicolor-exploit]: https://github.com/benwaterson/technicolor-exploit