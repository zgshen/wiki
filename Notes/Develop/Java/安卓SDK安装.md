
# SDK 安装

到 https://developer.android.google.cn/studio#command-tools 下载 Command tools。

创建一个 androidsdk 目录把下载的 Command tools 解压放进去，再在 cmdline-tools 文件夹里创建一个 latest 文件夹，把除 latest 文件夹之外的文件和文件夹移到 latest 里面，命令行 cd 进 bin 文件夹，`./sdkmanager --list` 可以看到 SDK 的各种工具包，包括已安装和未安装的。

```bash
# nathan @ nathan-tp in ~/app/androidsdk/cmdline-tools/latest/bin [21:29:59] 
$ ./sdkmanager --list                          
Warning: IO exception while downloading manifest                                
Installed packages:=====================] 100% Computing updates...             
  Path                 | Version | Description                       | Location            
  -------              | ------- | -------                           | -------             
  platform-tools       | 33.0.1  | Android SDK Platform-Tools 33.0.1 | platform-tools      
  platforms;android-29 | 5       | Android SDK Platform 29           | platforms/android-29

Available Packages:
  Path                                                                                     | Version      | Description                                                         
  -------                                                                                  | -------      | -------                                                             
  add-ons;addon-google_apis-google-15                                                      | 3            | Google APIs                                                         
  add-ons;addon-google_apis-google-16                                                      | 4            | Google APIs                                                         
  add-ons;addon-google_apis-google-17                                                      | 4            | Google APIs                                                         
  add-ons;addon-google_apis-google-18                                                      | 4            | Google APIs                                                         
  add-ons;addon-google_apis-google-19                                                      | 20           | Google APIs                                                         
  add-ons;addon-google_apis-google-21                                                      | 1            | Google APIs                                                         
  add-ons;addon-google_apis-google-22                                                      | 1            | Google APIs                                                         
  add-ons;addon-google_apis-google-23                                                      | 1            | Google APIs                                                         
  add-ons;addon-google_apis-google-24                                                      | 1            | Google APIs                                                         
  build-tools;19.1.0                                                                       | 19.1.0       | Android SDK Build-Tools 19.1                                        
  build-tools;20.0.0                                                                       | 20.0.0       | Android SDK Build-Tools 20                                          
  build-tools;21.1.2                                                                       | 21.1.2       | Android SDK Build-Tools 21.1.2                                      
  build-tools;22.0.1                                                                       | 22.0.1       | Android SDK Build-Tools 22.0.1                                      
  build-tools;23.0.1                                                                       | 23.0.1       | Android SDK Build-Tools 23.0.1                                      
  build-tools;23.0.2                                                                       | 23.0.2       | Android SDK Build-Tools 23.0.2                                      
  build-tools;23.0.3                                                                       | 23.0.3       | Android SDK Build-Tools 23.0.3 
  ......
```

"platforms;android-29" 工具包安装命令 `./sdkmanager --install "platforms;android-29"`


"platforms;android-29" 工具包卸载命令 `./sdkmanager --uninstall "platforms;android-29"`

其他工具包同理，参考 https://developer.android.google.cn/studio/command-line/sdkmanager。

