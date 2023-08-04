# OpenVPN Scramble options for OpenWRT

The Scramble options can be used to obfuscate an OpenVPN connection, this can be useful to escape censoring.  
It is supported by a number of OpenVPN providers e.g. TorGuard, [StrongVPN](https://blog.strongvpn.com/strongvpn-scramble/), IPvanish etc.  
But also available for other third partiy fimrware (DDWRT) and for Android, Windows and MacOS.  
For some background please read: https://tunnelblick.net/cOpenvpn_xorpatch.html  

Scramble options must be added to OpenVPN by adding a series of patches and compile your own build with it (or the OpenVPN-OpenSSL package).  

Patches are available for OpenVPN 2.5.x and for 2.6.x and are based on [Tunelblick's patches](https://github.com/Tunnelblick/Tunnelblick/tree/master/third_party/sources/openvpn) but adapted to work seamlessly with with OpenWRT.  

## To compile:
Copy all patch files to `feeds/packages/net/openvpn/patches`  
For OpenVPN 2.5.x use the patches from the 2.5 directory.  
For OpenVPN 2.6.x use the patches from the 2.6 directory, but take note this is not compatible with DCO so for OpenVPN 2.6.x add to the makefile (`feeds/packages/net/openvpn/makefile`): --disable-dco  

I have tested for OpenVPN 2.5.8 with OpenWRT 23.05 and that was working and obfuscating with a DDWRT router as client.  

On compiling, the patches are executed automatically.  

For a quick check to see if the scramble options are avaialble in your build, from commandline:  
`strings /usr/sbin/openvpn | grep scramble`  

## Usage
Note: scramble options must be the same on client and server side!

In the OpenVPN config add one of the four options, note: scramble options must be the same on client and server side!:  

`scramble "password"`
scramble is the leftmost option name. This can be followed by a string which will be used to perform a simple xor operation the packet payload.  
Note for tunnelblick this option is:
scramble xormask "password"

`scramble reverse`  
This simply reverses all the data in the packet. This should be enough to get past the regular expression detection in both China and Iran.  

`scramble xorptrpos`  
This performs a xor operation, utilising the current position in the packet payload.

`scramble obfuscate "password"`  
This method is more secure. It utilises the 3 types of scrambling mentioned above. "password" is the string which you want to use.

Both DDWRT OpenVPN Client and server supports scramble there are also clients available for Android and Windows:
https://github.com/lawtancool

Android:  
https://github.com/lawtancool/ics-openvpn-xor/releases

Windows:  
https://github.com/lawtancool/openvpn-windows-xor/releases

See also:  
https://forum.openwrt.org/t/scramble-obfuscate-in-openvpn/151570  
https://scramblevpn.wordpress.com/2017/04/16/compile-patched-openvpn-ipk-package-for-openwrtlede-router/   
https://shenzhensuzy.wordpress.com/2019/01/26/openvpn-with-xor-patch/  




