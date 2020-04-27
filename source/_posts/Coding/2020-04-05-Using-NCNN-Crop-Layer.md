---
date: 2020-04-05 00:00:00
title: "Using NCNN Crop Layer"
category: ["Coding"]
tag: ["NCNN"]
---

> ncnn is a high-performance neural network inference framework optimized for the mobile platform.

I've been using NCNN for quite a while. And recently after compiling the latest version, I was surprised the network could not give the correct output. Besides, the program crashed randomly.

Cropping seemed to be the reason when I digging into the source code. The cropping operation crops not only the 2D feature map but also the channel dim when the input blob is a 3-dim tensor. So I modified `_outc = ref_dims == 3 ? ref_channels : channels;` to `_outc = channels`. I'm not sure whether there is another way to avoid this operation. The modification temporately cope the problem.

```cpp
void Crop::resolve_crop_roi(const Mat &bottom_blob, const Mat &reference_blob, int &_woffset, int &_hoffset, int &_coffset, int &_outw, int &_outh, int &_outc) const
{
    int w = bottom_blob.w;
    int h = bottom_blob.h;
    int channels = bottom_blob.c;
    int dims = bottom_blob.dims;

    int ref_w = reference_blob.w;
    int ref_h = reference_blob.h;
    int ref_channels = reference_blob.c;
    int ref_dims = reference_blob.dims;

    if (dims == 1)
    {
        _woffset = woffset;
        _outw = ref_w;
    }
    if (dims == 2)
    {
        _woffset = woffset;
        _hoffset = hoffset;
        _outw = ref_w;
        _outh = ref_h;
    }
    if (dims == 3)
    {
        _woffset = woffset;
        _hoffset = hoffset;
        _coffset = coffset;
        _outw = ref_w;
        _outh = ref_h;
        // _outc = ref_dims == 3 ? ref_channels : channels;
        _outc = channels;
    }
}

```

The following image shows the result of a foreground segmentation nework before and after the modification.

![Output Comparison](/img/Coding/using_ncnn_cropping.png "Output Comparison")