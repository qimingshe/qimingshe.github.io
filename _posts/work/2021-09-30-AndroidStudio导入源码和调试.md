## 单独编译idegen模块生成idegen.jar
source build/envsetup.sh
mmm development/tools/idegen

命令执行完后会输出idegen.jar的文件路径

## 生成AndroidStudio工程配置文件-android.ipr和android.iml
development/tools/idegen/idegen.sh 生成android.ipr和android.iml文件

## 导入源码
打开Android Studio,通过File->open找到源码目录下的android.ipr文件就可以直接导入工程了.

##加快导入
默认会把所有android源码导入到AndroidStudio中,如果只关注Framework层源码,可以在android.iml文件中添加下面的文件,排除掉不需要导入的目录

      <excludeFolder url="file://$MODULE_DIR$/art"/>
      <excludeFolder url="file://$MODULE_DIR$/bionic"/>
      <excludeFolder url="file://$MODULE_DIR$/bootable"/>
      <excludeFolder url="file://$MODULE_DIR$/build"/>
      <excludeFolder url="file://$MODULE_DIR$/compatibility"/>
      <excludeFolder url="file://$MODULE_DIR$/dalvik"/>
      <excludeFolder url="file://$MODULE_DIR$/developers"/>
      <excludeFolder url="file://$MODULE_DIR$/development"/>
      <excludeFolder url="file://$MODULE_DIR$/device"/>
      <excludeFolder url="file://$MODULE_DIR$/disregard"/>
      <excludeFolder url="file://$MODULE_DIR$/external"/>
      <excludeFolder url="file://$MODULE_DIR$/hardware"/>
      <excludeFolder url="file://$MODULE_DIR$/kernel"/>
      <excludeFolder url="file://$MODULE_DIR$/libcore"/>
      <excludeFolder url="file://$MODULE_DIR$/libnativehelper"/>
      <excludeFolder url="file://$MODULE_DIR$/manifest"/>
      <excludeFolder url="file://$MODULE_DIR$/out"/>
      <excludeFolder url="file://$MODULE_DIR$/pdk"/>
      <excludeFolder url="file://$MODULE_DIR$/platform_testing"/>
      <excludeFolder url="file://$MODULE_DIR$/prebuilts"/>
      <excludeFolder url="file://$MODULE_DIR$/sdk"/>
      <excludeFolder url="file://$MODULE_DIR$/shortcut-fe"/>
      <excludeFolder url="file://$MODULE_DIR$/system"/>
      <excludeFolder url="file://$MODULE_DIR$/test"/>
      <excludeFolder url="file://$MODULE_DIR$/toolchain"/>
      <excludeFolder url="file://$MODULE_DIR$/tools"/>
      <excludeFolder url="file://$MODULE_DIR$/vendor"/>
    </content>
    
    
