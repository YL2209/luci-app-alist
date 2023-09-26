# luci-app-alist-个人定制版

一种支持多个存储的文件列表程序。

--------------


# 1、创建模板


<a href="https://github.com/codespaces/templates" target="_blank">
<img class="Avatar AuthorInfo-avatar css-1oqflzh" src="https://github.githubassets.com/images/modules/open_graph/github-logo.png" width = "100" height = "100">点击下面的标题 Build software better, together 创建一个空白模板即可</a>

![](https://cdn.nlark.com/yuque/0/2022/png/34107111/1669531953205-b438ffbc-7cd1-4d6f-ab0b-cddba2d958f4.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_24%2Ctext_5a6J56izIEFsaXN0IFYz%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_24%2Ctext_5a6J56izIEFsaXN0IFYz%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

--------------

# 2、首先先把前代码克隆下来
<!-- ![]() -->
  - 前端:
    ```shell
    git clone --recurse-submodules https://github.com/alist-org/alist-web.git
    ```
  ###### 
  -    [将diy.css文件](https://github.com/YL2209/luci-app-alist-customized/blob/master/diy.css)——>[放入/alist-web/public/static](https://github.com/alist-org/alist-web/tree/main/public/static)

    git clone https://github.com/YL2209/luci-app-alist-customized.git
    mv luci-app-alist-customized/diy.css alist-web/public/static


  - 引入外部文件:[alist-web/index.html](https://github.com/alist-org/alist-web/blob/main/index.html)
    - 头部 
        ```html
        <!--引入字体，全局字体使用-->
        <link rel="stylesheet" href="https://npm.elemecdn.com/lxgw-wenkai-webfont@1.1.0/lxgwwenkai-regular.css" />
        <!--引入动画-->
        <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/animate.css/4.1.1/animate.min.css" media="defer" onload="this.media='all'">
        <!--引入自定义css-->
        <link rel="stylesheet" href="/static/diy.css" />
        ```
        
    - 尾部
        ```html
        <!--引入头部信息-->
        <script async src="https://cdn.jsdelivr.net/gh/XXKDB/img_cdn/time/diytitle.js"></script>
        <!--引入动画-->
        <script defer src="https://cdn.jsdelivr.net/gh/graingert/wow@1.3.0/dist/wow.min.js"></script>
        <script defer data-pjax src="https://npm.elemecdn.com/hexo-butterfly-wowjs/lib/wow_init.js"></script>\
        <!--引入看板娘-->
        <script defer data-pjax src="https://cdn.jsdelivr.net/gh/YL2209/live2d-widget-mini@latest/autoload.js"></script>
        ```

  - 添加动画：[参考原创文章](https://akilar.top/posts/abab51cf/)
    - 图标文件夹动画：[alist-web/src/pages/home/folder/GridItem.tsx ：36](https://github.com/alist-org/alist-web/blob/main/src/pages/home/folder/GridItem.tsx)
        ```tsx
        +++ class="grid-item wow animate__flipInY"
        ```

    - logo动画：[alist-web\src\pages\home\header\Header.tsx ：37](https://github.com/alist-org/alist-web/blob/main/src/pages/home/header/Header.tsx)
        ```tsx
        +++ class="wow animate__bounceIn"
        src={logo()!}
        ```

    - 主页图标：[alist-web\src\pages\home\Nav.tsx：19](https://github.com/alist-org/alist-web/blob/main/src/pages/home/Nav.tsx)
        ```tsx
        +++ <Breadcrumb class="nav wow animate__fadeInTopLeft" id="rootdiy3" w="$full">
        ```

    - 文件目录：[alist-web\src\pages\home\Obj.tsx：34](https://github.com/alist-org/alist-web/blob/main/src/pages/home/Obj.tsx)
        ```tsx
        <VStack
        +++ class="obj-box wow animate__zoomIn"
        >
        ```

    - 描述目录：[alist-web\src\components\Markdown.tsx：51](https://github.com/alist-org/alist-web/blob/main/src/components/Markdown.tsx)
        ```tsx
        +++ <Box class="markdown wow animate__zoomIn" pos="relative" w="$full">
        ```
--------------

## 然后安装 pnpm

  - 使用npm安装pnpm
      ```shell
      npm install -g pnpm
      ```

## 其次下载语言文件并且初始化
  - 使用npm安装pnpm
      ```shell
      cd alist-web 
      wget https://crowdin.com/backend/download/project/alist/zh-CN.zip 
      unzip zh-CN.zip 
      node ./scripts/i18n.mjs
      rm -rf zh-CN.zip
      ```

## 安装项目所有依赖，同时编译前端文件
  - 输入命令
      ```shell
      pnpm install && pnpm run build
      ```

## 打包编译好的前端/diat
  - 输入命令
      ```shell
      tar -zcvf alist-web-3.28.0.tar.gz dist
      ```
--------------

# 下载openwrt SDK并解压
  - 输入命令
      ```shell
      cd ..
      wget https://downloads.openwrt.org/releases/22.03.5/targets/x86/64/openwrt-sdk-22.03.5-x86-64_gcc-11.2.0_musl.Linux-x86_64.tar.xz

      tar xvJf openwrt-sdk-22.03.5-x86-64_gcc-11.2.0_musl.Linux-x86_64.tar.xz
      
      rm -rf openwrt-sdk-22.03.5-x86-64_gcc-11.2.0_musl.Linux-x86_64.tar.xz
      ```
## 更新 feeds 
  - 输入命令
      ```shell
      cd openwrt-sdk-22.03.5-x86-64_gcc-11.2.0_musl.Linux-x86_64

      ./scripts/feeds update -a

      ./scripts/feeds install -a
      ```

## 下载并编译Luci-app-alist插件

- 安装开发包.

  - ubuntu/debian:
    ```shell
    sudo apt update
    sudo apt install libfuse-dev
    ```

- 输入您的开放式目录

- Openwrt 官方快照

  *1. 需要 golang 1.19.x 或最新版本（修复了 OpenWrt 旧分支的版本。）*
  ```shell
  rm -rf feeds/packages/lang/golang
  git clone https://github.com/sbwml/packages_lang_golang -b 20.x feeds/packages/lang/golang
  ```

  *2. 获取 luci-app-alist 源代码和构建*
  ```shell
  git clone https://github.com/sbwml/luci-app-alist package/alist
  make menuconfig # choose LUCI -> Applications -> luci-app-alist
  make package/alist/luci-app-alist/compile V=s # build luci-app-alist
  ```

## 自定义魔改前端（可选）

  *1. [更改文件](https://github.com/sbwml/luci-app-alist/blob/master/alist/Makefile)*
  ```shell
  #
  # Copyright (C) 2015-2016 OpenWrt.org
  #
  # This is free software, licensed under the GNU General Public License v3.
  #

  include $(TOPDIR)/rules.mk

  PKG_NAME:=alist
  PKG_VERSION:=3.28.0
  PKG_WEB_VERSION:=3.28.0
  PKG_RELEASE:=2

  PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
  PKG_SOURCE_URL:=https://codeload.github.com/alist-org/alist/tar.gz/v$(PKG_VERSION)?
  PKG_HASH:=7944c1ac07c5cad71e84f6087fba1bc28dd552b220f479bc38369455e38d93d4

  PKG_LICENSE:=GPL-3.0
  PKG_LICENSE_FILE:=LICENSE
  PKG_MAINTAINER:=sbwml <admin@cooluc.com>

  # define Download/$(PKG_NAME)-web
  #   FILE:=$(PKG_NAME)-web-$(PKG_WEB_VERSION).tar.gz
  #   URL_FILE:=dist.tar.gz
  #   URL:=https://github.com/alist-org/alist-web/releases/download/$(PKG_WEB_VERSION)/
  #   HASH:=62f7163f4651762d92da84d4eec51f4246cdb4841c1e7082ad0da1cacb0c2a00
  # endef

  PKG_BUILD_DEPENDS:=golang/host
  PKG_BUILD_PARALLEL:=1
  PKG_USE_MIPS16:=0
  PKG_BUILD_FLAGS:=no-mips16

  GO_PKG:=github.com/alist-org/alist
  GO_PKG_LDFLAGS:=-w -s
  GO_PKG_LDFLAGS_X:= \
    $(GO_PKG)/v3/internal/conf.Version=v$(PKG_VERSION)-$(ARCH) \
    $(GO_PKG)/v3/internal/conf.WebVersion=$(PKG_WEB_VERSION)

  include $(INCLUDE_DIR)/package.mk
  include $(TOPDIR)/feeds/packages/lang/golang/golang-package.mk

  define Package/$(PKG_NAME)
    SECTION:=net
    CATEGORY:=Network
    SUBMENU:=Web Servers/Proxies
    TITLE:=A file list program that supports multiple storage
    URL:=https://alist-doc.nn.ci/
    DEPENDS:=$(GO_ARCH_DEPENDS) +ca-bundle
  endef

  define Package/$(PKG_NAME)/description
    A file list program that supports multiple storage, powered by Gin and Solidjs.
  endef

  ifneq ($(CONFIG_USE_MUSL),)
    TARGET_CFLAGS += -D_LARGEFILE64_SOURCE
  endif

  define Build/Prepare
    $(call Build/Prepare/Default)
    mv /alist-web/$(PKG_NAME)-web-$(PKG_WEB_VERSION).tar.gz $(DL_DIR)/
    $(TAR) --strip-components=1 -C $(PKG_BUILD_DIR)/public/dist -xzf $(DL_DIR)/$(PKG_NAME)-web-$(PKG_WEB_VERSION).tar.gz
    $(CP) ./files/assets/. $(PKG_BUILD_DIR)/public/dist/assets/
  endef

  define Package/$(PKG_NAME)/install
    $(call GoPackage/Package/Install/Bin,$(PKG_INSTALL_DIR))
    $(INSTALL_DIR) $(1)/usr/bin
    $(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/alist $(1)/usr/bin
  endef

  $(eval $(call BuildPackage,$(PKG_NAME)))
  ```
--------------

![](https://user-images.githubusercontent.com/16485166/190462187-5d54725e-1d9b-45f3-854f-403b882fb223.png)
