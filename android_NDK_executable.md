### ndk编译cpp文件为可执行程序

方式1.编译动态连接的可执行程序

```
cmake .. -DCMAKE_TOOLCHAIN_FILE=/Users/liushichao/Library/Android/sdk/ndk/21.0.6113669/build/cmake/android.toolchain.cmake \
	-DANDROID_ABI=armeabi-v7a \
	-DANDROID_NATIVE_API_LEVEL=21 \
	-DANDROID_STL=c++_shared \
```

方式2.编译静态连接的可执行程序

```
cmake .. -DCMAKE_TOOLCHAIN_FILE=/Users/liushichao/Library/Android/sdk/ndk/21.0.6113669/build/cmake/android.toolchain.cmake \
	-DANDROID_ABI=armeabi-v7a \
	-DANDROID_NATIVE_API_LEVEL=21 \
	-DANDROID_STL=c++_static \
```

### 运行动态连接的可执行程序

1.android不会自动搜索当前文件夹下的so,需要将动态连接库拷贝到/system/lib/xxx.so中

2./system/lib默认是只读的，需要重新mount一下，如下指令

3.mount -o rw,remount /system

4.chmod 644 /system/lib/xxx.so

### 运行静态编译的可执行程序

1.root android deveice

2.ndk compile executable

3.adb push executor data/exesh

4.adb shell 

5.su

6.cd data

7.chmod 777 exesh #must not be in sdcard directory, because android system forbid chmod +x in sdcard directory.

8. ./exesh

enjoy it ~


