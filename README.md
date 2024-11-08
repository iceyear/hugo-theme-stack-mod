Codeberg 仓库地址：[Ice Year](https://codeberg.org/iceyear)/[hugo-theme-stack-mod](https://codeberg.org/iceyear/hugo-theme-stack-mod)

[GitHub 仓库](https://github.com/iceyear/hugo-theme-stack-mod) 仅为 Codeberg 仓库镜像！

本主题由 [CaiJimmy](https://github.com/CaiJimmy) 制作并[发布](https://github.com/CaiJimmy/hugo-theme-stack)，这个仓库是由 [Ice Year](https://codeberg.org/iceyear) 基于 [Mantyke](https://github.com/Mantyke/) 的魔改版本进行二次修改，结合了其他的一些参考资料并跟进官方 commits ，添加了一些特性，以及修复了一些 bug.

## 使用方式

**从零建立博客**：Fork仓库到自己账号下，用Github 注册 Vercel，依次点击Overview → New Project → import刚刚Fork的仓库，设置`FRAMEWORK PRESET`为Hugo → 点击`Environment Variables`，设置`NAME`为`HUGO_VERSION`，`Value`为`Hugo版本号（如0.89.0）` → 点击Add → 点击Deploy，稍等十来秒即可部署完成。下载仓库到本地后使用Github Desktop更新文章。（注，本地预览需安装Hugo）

**旧博客更换主题**：不同主题方式不同，推荐只保留原博客的content文件夹，迁移到本仓库content文件夹后再按情况调整。

<br>

## 魔改内容

- 调整文章页面为三栏显示（代码来自[ShadowySpirits](https://github.com/ShadowySpirits/hugo-theme-stack)）
- 文章按年份分类
- 增加文章字数统计与站点总字数、总篇数显示
- 修改全站字体为思源宋体
- 增加一个引用短代码（短代码来自[荷戟独彷徨](https://guanqr.com/)）
- 添加一个友情链接页面并设置为双栏（友情链接代码来自[Bore](https://bore.vip/archives/3bf3725e/#%E6%B7%BB%E5%8A%A0%E5%8F%8B%E6%83%85%E9%93%BE%E6%8E%A5-shortcodes)，双栏代码来自[BB_Roin](https://tech.randomwaves.space/posts/21-12-08-make-hugo-stack-theme-links-display-in-two-columns/)）
- 一系列基于个人美观喜好的CSS修改
- 右侧栏增加Categories小部件
- 结合了其他的一些参考资料并跟进官方 commits ，添加了一些特性，以及修复了一些 bug

<br>

## 部分使用说明

### 引用样式短代码

```fallback
{{< quote >}}
三月，因久旱不雨，苏轼赴郿，祈雨于太白山之上清宫。数日后，虽有微雨，父老以为不足，于是，再陪宋太守亲往祭祷，回程路上，便见道中有云气自山中来，如群马奔突而至车座左右，苏轼一时好奇心起，开笼收云归家，作《攓云篇》。
{{< /quote >}}
```

<br>

### 友情链接使用方式

友链头像放在`/assets/link-img`，友链数据放在`/data/links.json`

```
[
    {
        "title": "Ice Year の位面",
        "website": "https://blog.iceyear.eu,org",
        "image": "avatar.png",
     "description": "Il n'y a pas de hasard, il n'y a que des rendez-vous."
    },
    {
        "title": "友情链接2",
        "website": "",
        "image": "",
     "description": ""
    }
]
```

<br>

### 修改页尾信息

站点名称及建站时间请修改以下代码

站点名称及链接：

```
#位置：layout/partials/footer/footer.html

    <section class="copyleft">
        🄯 
        {{ if and (.Site.Params.footer.since) (ne .Site.Params.footer.since (int (now.Format "2006"))) }}
            {{ .Site.Params.footer.since }} - 
        {{ end }}
        {{ now.Format "2006" }} <a href="https://stack-theme-mod.vercel.app/">Example Site</a>·<i class="fas fa-bell"></i> <a id="days">0</a>Days<br>
      {{$var :=  $scratch.Get "total"}}{{$var = div $var 100.0}}{{$var = math.Ceil $var}}{{$var = div $var 10.0}}共书写了{{$var}}k字·共 {{ len (where .Site.RegularPages "Section" "post") }}篇文章</br><span><p>
    </section>
```

```
#位置：layout/partials/footer/footer.html

var s1 = '2022-02-13';//设置为建站时间
s1 = new Date(s1.replace(/-/g, "/"));
s2 = new Date();
var days = s2.getTime() - s1.getTime();
var number_of_days = parseInt(days / (1000 * 60 * 60 * 24));
document.getElementById('days').innerHTML = number_of_days;
```

<br>

## 其他常见修改

### 设置博客头像

位置：`/assets/img`，请将头像命名为`avatar.png`，替换原头像文件

### 删除博客头像

位置：`/layouts/partials/sidebar/left.html`，注释或删去以下部分：

```
        {{ with .Site.Params.sidebar.avatar }}
            {{ if (default true .enabled) }}
            <figure class="site-avatar">
                <a href="{{ .Site.BaseURL | relLangURL }}">
                {{ if not .local }}
                    <img src="{{ .src }}" width="300" height="300" class="site-logo" loading="lazy" alt="Avatar">
                {{ else }}
                    {{ $avatar := resources.Get (.src) }}

                    {{ if $avatar }}
                        {{ $avatarResized := $avatar.Resize "300x" }}
                        <img src="{{ $avatarResized.RelPermalink }}" width="{{ $avatarResized.Width }}"
                            height="{{ $avatarResized.Height }}" class="site-logo" loading="lazy" alt="Avatar">
                    {{ else }}
                        {{ errorf "Failed loading avatar from %q" . }}
                    {{ end }}
                {{ end }}
                </a>
                {{ with $.Site.Params.sidebar.emoji }}
                    <span class="emoji">{{ . }}</span>
                {{ end }}
            </figure>
            {{ end }}
        {{ end }}
```

### 修改博客背景颜色

位置：`assets\scss\variables.scss`

```
--body-background: #f6f6f6;
```

### 指定首页分类标签颜色

位置：`content\categories`，创建分类同名文件夹后，在文件夹内创建`_index.md`文件，写入frontmatter

```
title: "做猫贵在折腾" //分类名称
description: "简介 Blablabla" //不需要可以删了
image: "ffxiv_20210830_230509_817.png" //分类题图，不需要也可以删了
style:
    background: "#80aba9" //分类标签底色
    color: "#fff"
---
```

### 配置各类图标

图标放在：`/assets/icons`，svg格式。

<br>
