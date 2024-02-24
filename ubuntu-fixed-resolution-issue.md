Problem:

Upon plugging in additional screen to a laptop with Ubuntu 22.04 installed, the second display shows as 'Unknown Display' and only four resolutions to choose from are available:
* 1024 x 1080
* 848 x 480
* 800 x 600
* 640 x 480

where the ideal resolution is 1920 x 1080.

Solution:

Several solutions are on offer on the Internet. What worked for me is a combination of a couple solutions. Namely
1. Press Ctrl + Alt + T to enter terminal
2. Navigate tot the .config folder by typing
`cd /home/<user_name>/.config/`
3. Open the monitors.xml file by typing, for example
`nano monitors.xml`
4. Notice that one of the monitors in the code will have status 'unknown', for example you will see code like below
`   <logicalmonitor>
      <x>1600</x>
      <y>0</y>
      <scale>1</scale>
      <primary>yes</primary>
      <monitor>
        <monitorspec>
          <connector>DP-2</connector>
          <vendor>unknown</vendor>
          <product>unknown</product>
          <serial>unknown</serial>
        </monitorspec>
        <mode>
          <width>1920</width>
          <height>1080</height>
          <rate>59.993721008300781</rate>
        </mode>
      </monitor>
    </logicalmonitor>
  </configuration>`
5. The important line is the <connector> line. Note the connector code from that line. In this case it is 'DP-2'.
6. Press Ctrl + X to exit text editor.
7. Still in terminal, open the grub file by typing `sudo nano /etc/default/grub`.
8. You might be prompted do enter your password.
9. In that file, two lines will read:
`GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"
GRUB_CMDLINE_LINUX=""`
10. Amend them to input what you got from the <connector> line from point 4 to read (in this case, since we get 'DP-2')
`GRUB_CMDLINE_LINUX_DEFAULT="quiet splash video=DP-2:1920x1080@60"
GRUB_CMDLINE_LINUX="video=DP-2:d"`
11. or more generally
`GRUB_CMDLINE_LINUX_DEFAULT="quiet splash video=<connector>:1920x1080@60"
GRUB_CMDLINE_LINUX="video=<connector>:d"`
12. Press Ctrl + X and then when prompted Y to save changes
13. in terminal, type `sudo update-grub`
14. reboot with `sudo reboot`
15. Enjoy seeing your screen with the 1920x1080 resolution!

It is simple once you know it. It took me several hours of other hacks, workarounds and tutorials before I managed to put together this solution. I know for a fact that it is to do with the extra screen not being recognised by Ubuntu - when I tried it with another additional screen, all worked fine.
fdsadfas
