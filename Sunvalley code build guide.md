# Sunvalley code build guide

Version | Date| Description | Author | 
---|---|---|---
 0.0.0.1|2018-05-07|first version finished|White He


[toc]
## 1	Start
### 1.1	Intall git
To install git ,please run these commads in order
> sudo add-apt-repository ppa:git-core/ppa   
> sudo apt-get update   
> sudo apt-get install git   

if install successed ,run these commads will get the git version
> git --version

![image](https://note.youdao.com/yws/public/resource/b04ee4e84f570e382b1bf756a6504b5a/xmlnote/C781AE90850F45178CC8D6704324636E/637)   
### 1.2 generate ssh key
If you aready has a key and you don't want to use it ,please back up it firstly.
![image](https://note.youdao.com/yws/public/resource/b04ee4e84f570e382b1bf756a6504b5a/xmlnote/1A4611ACEB254FF8B990857A6917AB17/649)
Then run this commad to genrate new ssh key
> ssh-keygen   

First it confirms where you want to save the key (.ssh/id_rsa), and then it asks twice for a passphrase, which you can leave empty if you don’t want to type a password when you use the key.

Now, each user that does this has to send their public key to you or whoever is administrating the Git server (assuming you’re using an SSH server setup that requires public keys). All they have to do is copy the contents of the .pub file and email it.
For a more in-depth tutorial on creating an SSH key on multiple operating systems, see the GitHub guide on SSH keys at https://help.github.com/articles/generating-ssh-keys.

#### 1.3 Setup AOSP Environment
The Requirements can be found with Google official website https://source.android.com/setup/build/requirements.**Note** For the Java Development Kit (JDK), our code  comes with a  **OpenJDK**.

The Establishing a Build Environment can be found with Google official website https://source.android.com/setup/build/initializing

## 2	How to build android firmware
#### 2.1 get the source code
Use this commads to get the code
>  git clone  ssh://git@csl.pd2.anjoudirect.com:5002/B0_dev/02_RK3288_6_0 -b CBL001_dev    

Then a 02_RK3288_6_0 directory can be found.This directory is the source root(alias $AOSP),CD to it.  
![image](https://note.youdao.com/yws/public/resource/b04ee4e84f570e382b1bf756a6504b5a/xmlnote/E65240675A1B4E4696DB4E8552B011A1/688) 
Section 7 will introduce the code tree.  
You can use git commmads to browse commit history
> git log

### 2.2 start to build
Run this commad to build at source root direcory
>./mybuild-nail.sh 

![image](https://note.youdao.com/yws/public/resource/b04ee4e84f570e382b1bf756a6504b5a/xmlnote/C79D11CA0649411BADC20A70D0531F53/741)  
Then build is going on.If it's the first time to build,it will cost a long time(3 hours mybe,it depends on pc hardware) .Somtimes,if build errors occur,fix it and run this commad again.   
When build finished,you can you see that:  
![image](https://note.youdao.com/yws/public/resource/b04ee4e84f570e382b1bf756a6504b5a/xmlnote/60BA62452E9147C39AAF509692654B5F/743)  
Img output file can be found at 
> $AOSP/IMG/

![image](https://note.youdao.com/yws/public/resource/b04ee4e84f570e382b1bf756a6504b5a/xmlnote/BDE965818D754F52ADFE7056DF796017/747)
For example,I runned a build procces on May 4,the output is:
> $AOSP/IMG/CBL001.V0.8T20180504-2-20180504_194038/CBL001.V0.8T20180504-2-20180504_194038-update.zip

The .zip file contains firmware (upate.img) file to load.

### 2.3 mybuild-nail.sh file
mybuild-nail.sh is a shell script file.Edit with vi
> vi ./mybuild-nail.sh

There are some variables you may care in this file  
![image](https://note.youdao.com/yws/public/resource/b04ee4e84f570e382b1bf756a6504b5a/xmlnote/5CDBE9E55E0F4E9280DC498262F3F033/781)

## 3	How to upgrade firmware to device
Copy .zip file  (mentioned by Section 2.2) to Windows through Samba Service.Then go ahead with file **upgrade_firmware_and_run_testapp.docx**
## 4	How to build kernel
You can build kernel by oneself.Firstly,look ./mybuild-nail.sh file and find Kernel name(metioned by Section 2.3 mybuild-nail.sh file).Then cd to kernel directory: 
> cd $AOSP/kernel/

run these commad to build kernel:
> make make zt_nail_rk3288-tb.img
  
![image](https://note.youdao.com/yws/public/resource/b04ee4e84f570e382b1bf756a6504b5a/xmlnote/2CD6E82FD52240E4B57D25B660ED1EFB/788)  
Also adding -j4 tag means NPROC (number of process used to build).If succeed,you can see that:  
![image](https://note.youdao.com/yws/public/resource/b04ee4e84f570e382b1bf756a6504b5a/xmlnote/2572E48B769E402FBF1C9B302DBFF179/792)  
The resource.img and kernel.img can be found at directory:
>$AOSP/kernel/

These tow files will be used to upagrade device kernel. 
## 5	How to load kernel into device
Copy resource.img file and kernel.img file (mentioned by Section 2.2) to Windows through Samba Service.  
![image](https://note.youdao.com/yws/public/resource/b04ee4e84f570e382b1bf756a6504b5a/xmlnote/58D4CF139B054E43A3C10741BB38F55D/866)  

Besides,copy parameter.txt file to Windows.This file can be found at directiory:
> $AOSP/vendor/zt/rk_upgrade_tools/rockdev/parameter.txt


Then,run AndroidTool.exe on Windows:  
![image](https://note.youdao.com/yws/public/resource/b04ee4e84f570e382b1bf756a6504b5a/xmlnote/D56DEB522D704DA59AD227DE2CBE96E9/839)  


![image](https://note.youdao.com/yws/public/resource/b04ee4e84f570e382b1bf756a6504b5a/xmlnote/83555CC804054179BD8D5502D650D490/841)  


![image](https://note.youdao.com/yws/public/resource/b04ee4e84f570e382b1bf756a6504b5a/xmlnote/9B595E434AE74290BA2D76343B08460C/843)  


![image](https://note.youdao.com/yws/public/resource/b04ee4e84f570e382b1bf756a6504b5a/xmlnote/BD6AB41498FD412B92823B5CFC8F462C/847)  


![image](https://note.youdao.com/yws/public/resource/b04ee4e84f570e382b1bf756a6504b5a/xmlnote/30AAF16B94AC47E3A892C91732611E84/850)  


![image](https://note.youdao.com/yws/public/resource/b04ee4e84f570e382b1bf756a6504b5a/xmlnote/6E6B37C093A54350A9B44372BBB27B75/854)

## 6	How to build modules
Module can be builded by oneself.
- First, setup build env with these commads:
    > . build/envsetup.sh  
    > lunch 13

    or run myenv.sh shell script:
    > . myenv.sh

- make module
    > mmm [module path]

    or
    > cd module path  
    > mm
    
    rebuild with clear
    > mm -B 
    
    build module and build depends modules
    > mma
    
    
Module means the code block with an Android.mk file.Module can be classifed to 5 types(Except host type,like HOST_STATIC_LIBRARY):


Category | Description | Output dir | Example |
---|------|---|---
EXECUTABLE | excutable binary | $AOSP/out/target/product/<<font  color=#F08080>product_name</font>>/obj/EXECUTABLES/<<font  color=#F08080>module_name</font>>_intermediates/;<br>Install to $AOSP/out/target/product/<<font color=#F08080>product_name</font>>/system/bin/ |$AOSP/external/jpeg <br> module for libjpeg ,it also provides a executable file  /system/bin/cjpeg
STATIC_LIBRARY | c/c++ static library | $AOSP/out/target/product/<<font  color=#F08080>product_name</font>>/obj/STATIC_LIBRARIES/<<font  color=#F08080>module_name</font>>_static_intermediates/ | $AOSP/external/jpeg <br> module for libjpeg ,it provides static lib file libjpeg_static.a
SHARED_LIBRARY | c/c++ shared library | $AOSP/out/target/product/<<font  color=#F08080>product_name</font>>/obj/SHARED_LIBRARIES/<<font  color=#F08080>module_name</font>>_intermediates/;<br>Install to $AOSP/out/target/product/<<font color=#F08080>product_name</font>>/system/lib/ | $AOSP/external/jpeg <br> module for libjpeg ,it provides shared lib file libjpeg.so
STATIC_JAVA_LIBRARY | java static library |$AOSP/out/target/common/obj/JAVA_LIBRARIES/<<font color=#F08080>product_name</font>>_intermediates/ | $AOSP/vendor/zt/NailPrint/java module for NaiPrint java suport,it provides a static jar lib
JAVA_LIBRARY | java shared library |$AOSP/out/target/common/obj/JAVA_LIBRARIES/<<font color=#F08080>product_name</font>>_intermediates/;<br>Install to $AOSP/out/target/product/<<font color=#F08080>product_name</font>>/system/<<font color=#F08080>module_name</font>>/<font color=#F08080>module_name</font>.jar|$AOSP/framework/base,module for framework.jar
PACKAGE | android apk|$AOSP/out/target/product/<product_name>/system/<app or priv-app>|$AOSP/vender/zt/devicetest6.0,module for DeviceTest.apk

## 7	Code tree

```
graph TB
AOSP-->external
AOSP-->IMG(IMG:Img output directory)
AOSP-->framework
AOSP-->kernel
AOSP-->vendor
AOSP-->out
AOSP-->...
external-->jpeg
external-->libyuv
external-->OtherNavity(other native lib)
```
to be continue!