# RT_Smart GNU 移植 minizip记录（基于xmake实现）

由于minizip除了依赖于文件一些操作函数外并不依赖于其他库，所以个人直接编译运行；另外本次移植的是使用的xmake完成移植。

## 移植记录

1、选择合适的移植库，xmake提供了一些可以跨平台移植的库，见[https://xrepo.xmake.io/#/packages/cross](https://xrepo.xmake.io/#/packages/cross)，这里我选择了`minizip`来进行移植。

2、编写`xmake.lua`配置`ToolChains`并且引入`minizip`依赖

```lua
add_rules("mode.debug", "mode.release")

toolchain("aarch64-linux-musleabi")
    set_kind("standalone")
    set_sdkdir("$(projectdir)/../../tools/gnu_gcc/aarch64-linux-musleabi_for_x86_64-pc-linux-gnu")
    on_load(function(toolchain)

	os.setenv("PROJ_DIR", os.projectdir())  --For lua embed build script
        toolchain:load_cross_toolchain()
        toolchain:set("toolset", "cxx", "aarch64-linux-musleabi-g++")
        toolchain:set("toolset", "cc", "aarch64-linux-musleabi-gcc")
	    -- add flags for aarch64
        toolchain:add("cxflags",     "-march=armv8-a -D__RTTHREAD__  -Wall -n --static -DHAVE_CCONFIG_H", {force = true})                                             
        toolchain:add("ldflags",     "-march=armv8-a -D__RTTHREAD__  -Wall -n --static", {force = true})  
        toolchain:add("ldflags",     "-T $(projectdir)/../../linker_scripts/aarch64/link.lds", {force = true})
        if not is_config("pkg_searchdirs", "dropbear") then
            toolchain:add("ldflags",     "-L$(projectdir)/../../sdk/rt-thread/lib/aarch64/cortex-a -Wl,--whole-archive -lrtthread -Wl,--no-whole-archive", {force = true})
        end
        toolchain:add("includedirs", "$(projectdir)/../../sdk/rt-thread/include", {force = true})
        toolchain:add("includedirs", "$(projectdir)/../../", {force = true})
        toolchain:add("includedirs", "$(projectdir)", {force = true})
        toolchain:add("includedirs", "$(projectdir)/../../sdk/rt-thread/components/dfs", {force = true})
        toolchain:add("includedirs", "$(projectdir)/../../sdk/rt-thread/components/drivers", {force = true})
        toolchain:add("includedirs", "$(projectdir)/../../sdk/rt-thread/components/finsh", {force = true})
        toolchain:add("includedirs", "$(projectdir)/../../sdk/rt-thread/components/net", {force = true})
        toolchain:add("linkdirs",    "$(projectdir)/../../sdk/rt-thread/lib/aarch64", {force = true})    
        
        if is_config("kind", "debug") then
            toolchain:add("cxflags", "-g -gdwarf-2", {force = true})
        else
            toolchain:add("cxflags", "-O2", {force = true})
        end

    end)

toolchain_end()

add_requires("minizip")

target("minizip")
  set_toolchains("aarch64-linux-musleabi")
  set_kind("binary")
  add_files("src/minizip.c")
  add_packages("minizip")

target("miniunz")
  set_toolchains("aarch64-linux-musleabi")
  set_kind("binary")
  add_files("src/miniunz.c")
  add_packages("minizip")

target("minizip_test")
  set_toolchains("aarch64-linux-musleabi")
  set_kind("binary")
  add_files("src/main.c")
  add_packages("minizip")
```

配置`cconfig.h`，这个文件如果用scons会自动生成，但是在xmake工程当中不会自动生成，所以需要自己实现

```c
#ifndef CCONFIG_H__
#define CCONFIG_H__
/* Automatically generated file; DO NOT EDIT. */
/* compiler configure file for RT-Thread in GCC/MUSL */

#define HAVE_SYS_SIGNAL_H 1
#define HAVE_SYS_SELECT_H 1
#define HAVE_PTHREAD_H 1

#define HAVE_FDSET 1

#define HAVE_SIGACTION 1
#define HAVE_SIGEVENT 1
#define HAVE_SIGINFO 1
#define HAVE_SIGVAL 1

#endif
```

3、编写`src/main.c`程序，这里我用`minizip`实现了压缩`example.txt`到`example.zip`

```c
/*
 * Copyright (c) 2006-2018, RT-Thread Development Team
 *
 * SPDX-License-Identifier: GPL-2.0
 *
 * Change Logs:
 * Date           Author        Notes
 * 2023-05-17     wcx1024979076 The first version
 */

#include "stdio.h"
#include "zip.h"
 
int main()
{
    // 文件名
    const char *zipfile = "example.zip";
    // 需要压缩的文件
    const char *file = "example.txt";
 
    zipFile zf = zipOpen(zipfile, APPEND_STATUS_CREATE);
    if(zf == NULL)
    {
        printf("Error creating  %s \n", zipfile);
        return 1;
    }
 
    // 压缩文件
    int err = zipOpenNewFileInZip(zf, file, NULL, NULL, 0, NULL, 0, NULL, Z_DEFLATED, Z_BEST_COMPRESSION);
    if(err != ZIP_OK)
    {
        printf("Error adding %s to %s \n", file, zipfile);
        return 1;
    }
 
    // 读取文件并压缩
    FILE *f = fopen(file, "rb");
    char buf[1024];
    int len;
    while((len = fread(buf, 1, sizeof(buf), f)) > 0)
    {
        zipWriteInFileInZip(zf, buf, len);
    }
    fclose(f);
 
    zipCloseFileInZip(zf);
    zipClose(zf, NULL);

    printf("Successfully created %s \n", zipfile);
    return 0;
}
```

4、`xmake`编译链接生成`mininet`可执行文件，打包进入 `sd.bin`

这里我使用的`mcopy`来实现的（用的是Codespace来写的代码，无root权限，不能使用mount挂载），具体命令为

```sh
mcopy -i sd.bin /path/of/the/minizip ::
```

5、用`qemu`虚拟机运行即可

运行结果：

![qemu运行结果](https://pic.tim-wcx.ltd/img/qemu_run_minizip.png)

6、代码见：

[https://github.com/WCX1024979076/userapps](https://github.com/WCX1024979076/userapps)

## 参考链接

感谢 xmake-io/smart-build 仓库所提供的 RT_Smart xmake 编译脚本

[https://github.com/xmake-io/smart-build/blob/main/toolchains/aarch64.lua](https://github.com/xmake-io/smart-build/blob/main/toolchains/aarch64.lua)

zlib仓库

[https://github.com/madler/zlib](https://github.com/madler/zlib)

xrepo提供的minizip/xmake.lua

[https://github.com/xmake-io/xmake-repo/blob/master/packages/m/minizip/xmake.lua](https://github.com/xmake-io/xmake-repo/blob/master/packages/m/minizip/xmake.lua)

xrepo提供的zlib/xmake.lua

[https://github.com/xmake-io/xmake-repo/blob/master/packages/z/zlib/xmake.lua](https://github.com/xmake-io/xmake-repo/blob/master/packages/z/zlib/xmake.lua)