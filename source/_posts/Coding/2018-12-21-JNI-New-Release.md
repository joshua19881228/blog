---
date: 2018-12-21 00:00:00
title: "JNI和cv::Mat配合中的一个坑"
category: "Coding"
tag: ["JNI", "OpenCV"]
---

最近开发一个需要从Java端将图像传至native层的功能，发现了一个奇怪现象，native层每次获取到的图像都会有一些像素值发生随机变化。原始代码类似如下

```C
cv::Mat imgbuf2mat(JNIEnv *env, jbyteArray buf, int width, int height){
    jbyte *ptr = env->GetByteArrayElements(buf, 0);
    cv::Mat img(height, width, img_type, (unsigned char *)ptr);
    env->ReleaseByteArrayElements(buf, ptr, 0);
    return img;
}

static void nativeProcessImageBuff
        (JNIEnv *env, jobject thiz,
        jbyteArray img_buff,
        jint width,
        jint height)
{
    cv::Mat img = imgbuf2mat(env, img_buff, width, height, img_type, img_rotate);
    //do somethint to the image
    process(img);
}
```

简单来说就是先把图像内容转成cv::Mat，然后对图像做一些处理，但是即使传入相同的一张图像，处理结果每次都不一样。后来发现其实应该先处理图像，再释放引用，修改后的代码类似如下

```C
cv::Mat imgbuf2mat(JNIEnv *env, jbyte *ptr, int width, int height){
    cv::Mat img(height, width, img_type, (unsigned char *)ptr);
    return img;
}

static void nativeProcessImageBuff
        (JNIEnv *env, jobject thiz,
        jbyteArray img_buff,
        jint width,
        jint height)
{
    jbyte *ptr = env->GetByteArrayElements(img_buff, 0);
    cv::Mat img = imgbuf2mat(env, ptr, width, height);
    //do somethint to the image
    process(img);
    env->ReleaseByteArrayElements(img_buff, ptr, 0);
}
```