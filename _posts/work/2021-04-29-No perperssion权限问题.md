


no permissions (user in plugdev group; are your udev rules wrong?); see [http://developer.android.com/tools/device.html]

在/etc/udev/rules.d/70-android.rules写入以下内容

SUBSYSTEM=="usb", ATTR{idVendor}=="*", MODE="0666",GROUP="plugdev"
SUBSYSTEM=="usb", ATTRS{idVendor}=="*", ATTRS{idProduct}=="*",MODE="0666"

sudo chmod a+r 70-android.rules  #将此目录下面的所有rules文件加上权限
sudo service udev restart

* cd to your adb path *
sudo ./adb kill-server
sudo ./adb devices

adb devices
