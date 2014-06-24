---
layout: post
title: "c语言版本  图片缩略图（GraphicsMagick）"
description: ""
category: 
tags: []
---
{% include JB/setup %}
>写一个图片缩略图的代码，GraphicsMagick在性能上是比较好的，并且提供多种语言接口供调用。不过非常诟病的是资料太少了（不光是中文，英文好的资料也不多），官网也没有imageMagick那么细致的提供一些示例代码，只能自己慢慢摸索，下面说几个关键的地方，希望对很多朋友有用吧！     
1 include <magick/api.h> 这个文件到底在哪里？编译的时候会出错！   
		*这个文件可以在源代码的目录中找到*     
2 gcc -o demo demo.c -O `GraphicsMagickWand-config --cppflags --ldflags --libs`  会提示GraphicsMagickWand-config找不到！     
		*在下载解压后的GraphicsMagick目录中写你自己的代码，然后就在这个目录里执行gcc命令，就不会报错了*      
3 请提前安装好GraphicsMagick，可以使用yum或者apt-get    
4 很多的struct真的找不到，我也很无奈，例如DrawingWand,具体应该怎么使用，我也暂时没有找到      

>以下是书写的代码：    
```c
#include <stdlib.h>
#include <stdio.h>
#include <string.h>
#include <wand/magick_wand.h>

int main(int argc,char **argv)
{
    MagickWand *magick_wand;
    MagickPassFail status = MagickPass;
    const char *infile, *outfile;

    if (argc != 3)
    {
        fprintf(stderr,"Usage: %s: infile outfile\n",argv[0]);
        return 1;
    }

    infile=argv[1];
    outfile=argv[2];

    // Initialize GraphicsMagick API
    InitializeMagick(*argv);

    // Allocate Wand handle
    magick_wand=NewMagickWand();

    // Read input image file
    if (status == MagickPass)
    {
        status = MagickReadImage(magick_wand,infile);
    }

    // 这里是核心的地方，做缩略图的函数
    if (status == MagickPass)
    {
       status = MagickResizeImage(magick_wand,300,260,LanczosFilter,0.5);    
    }

    // Write output file
    if (status == MagickPass)
    {
        status = MagickWriteImage(magick_wand,outfile);
    }

    // Diagnose any error
    if (status != MagickPass)
    {
        char *description;
        ExceptionType severity;

        description=MagickGetException(magick_wand,&severity);
        (void) fprintf(stderr,"%.1024s (severity %d)\n",
                description,severity);
    }

    // Release Wand handle
    DestroyMagickWand(magick_wand);

    // Destroy GraphicsMagick API
    DestroyMagick();

    return (status == MagickPass ? 0 : 1);
}
```   