---
title: "Django 中图片的上传及显示"
description: "处理静态资源对于一个站点来说是必不可少的，对于 Django 来说，它有一套自己规定的静态资源管理方法，如果需要在 Django 中激活图片上传功能，不但需要自己写一个 Controller 来完成文件的接受，还需要使用额外的数据库资源来存储文件，本文将阐述如何使用 Django 接受、保存并返回图片。"
date: "2018-05-04"
slug: "6"
categories:
    - 技术
tags:
    - Django
    - Python
    - Backend
keywords:
    - django
    - python
---

在 `Django` 中，上传文件不同于普通服务器的上传方法，在普通服务器中只需要使用一个 `Controller` 来控制文件的上传即可完成，但是在 `Django` 中，则需要额外使用数据库资源来存储文件。本文将说明如何使用 `Django` 接收、保存并且返回图片。

# ☕ 准备
首先，你需要为你的 `Python` 安装 `pillow`，`pillow` 是一个 `Python` 图像库，`Django` 的图片方面的功能使用到了它，所以我们需要事先安装：

```
pip install pillow
```

安装完成之后我们需要在 `Django` 的 `settings.py` 中更改一些设置：

```python
# settings.py
# 在末尾添加

MEDIA_ROOT = os.path.join(BASE_DIR, 'media').replace('\\', '/')

MEDIA_URL = '/media/'
```

# 🎫 Model
之前说到了 `Django` 的图片需要使用额外的数据库资源来存储文件，这样的设定并不是把图片数据本身存在数据库，而是 `Django` 将会自动将文件上传到你设置的位置，并且把上传之后的图片 `path` 存入数据库，这样你只需要访问数据库中的 `path` 即可访问到图片。

在你的应用目录下的 `models.py` 里新建一个图片 `Model`

```python
from django.db import models

class Image(models.Model):
    # 图片
    img = models.ImageField(upload_to='img')
    # 创建时间
    time = models.DateTimeField(auto_now_add=True)
```

这样做之后，一旦数据库对象被创建，`img` 表列接受的图片对象将会自动被上传到 `/media/img` 文件夹中，在上传完成之后，`img` 将会保存图片的 `path`。

# 🧀 View
主流服务器接受文件都需要自己写一个响应，`Django` 也不例外。

在你的应用下的 `views.py` 中新建一个响应:

```python
from .models import Image
from django.shortcuts import HttpResponse
import json

MEDIA_SERVER = 'http://127.0.0.1:8000/media/'

class ImageTool:
    @staticmethod
    def get_new_random_file_name(file_name):
        find_type = False
        for c in file_name:
            if c == '.':
                find_type = True
        if find_type:
            type = file_name.split('.')[-1]
            return str(uuid.uuid1()) + '.' + type
        else:
            return str(uuid.uuid1())


def image_upload(request):
    source = request.FILES.get('image')
    if source:
      source.name = ImageTool.get_new_random_file_name(source.name)
      image = Image(
          img=source
      )
      image.save()
      return HttpResponse(json.dumps({
          'success': True,
          'path': MEDIA_SERVER + image.img.url
      }))
  else:
      return HttpResponse(json.dumps({
          'success': False,
          'error_code': 100
      }))
```

响应的主体是 `image_upload` 方法，而 `ImageTool` 中 `get_new_random_file_name` 方法是为了获取一个新的 `uuid` 随机新名字，这样做的原因是因为图片可能有重名的状况，虽然如果遇到这样的事情 `Django` 会自动为我们处理，但是为了保持名字的可管理性和统一性，自己写一个重命名的方法会更好。

# 🍗 Url
最后只需要在 `url` 中添加文件上传 `view` 的 `url` 即可:

```python
# urls.py

from django.urls import path
from . import views

urlpatterns = [
    ...

    path('file/image_upload', views.file__image_upload)
]
```

# 🎉 上传图片和访问图片
完成这些后，你只需要在前端需要上传图片的地方将 `url` 指向这个地址，就能将图片成功上传，上传完成之后你可以使用 `/media/` 加上数据库中图片的 `path` 就能访问到图片。

