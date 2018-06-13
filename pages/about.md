---
layout: page
title: About
description: 有恒何必三更眠五更起
keywords: 怖放弃, bu fangqi
comments: true
menu: 关于
permalink: /about/
---

我是怖放弃，有恒何必三更眠五更起。

不放弃不抛弃。

## 联系

{% for website in site.data.social %}
* {{ website.sitename }}：[@{{ website.name }}]({{ website.url }})
{% endfor %}

## Skill Keywords

{% for category in site.data.skills %}
### {{ category.name }}
<div class="btn-inline">
{% for keyword in category.keywords %}
<button class="btn btn-outline" type="button">{{ keyword }}</button>
{% endfor %}
</div>
{% endfor %}
