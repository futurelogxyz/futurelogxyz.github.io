---
author: wallezen
title: 组件示例
date: 2023-08-08
description: 使用短代码轻松添加常见的 Bootstrap 组件，展示组件的使用方法。
tags: ["组件", "示例"]
thumbnail: https://futurelog-1251943639.cos.accelerate.myqcloud.com/img/FK6E7d-20230808.png
photoCredits: <a href="https://twitter.com/wallezen007">wallezen</a>
photoSource: <a href="/">Futurelog</a>
draft: false
modules:
    - leaflet
---

## 1. Accordion

{{< accordion id="accordion-default" >}}
  {{< accordion-item header="Accordion Item #1" show="true" >}}
    This is the first item's accordion body. It supports HTML content. The item is shown by adding the value
    <code>show</code> to the <code>class</code> argument.
  {{< /accordion-item >}}
  {{< accordion-item header="Accordion Item #2" >}}
    This is the second item's accordion body. It too supports HTML content.
  {{< /accordion-item >}}
  {{< accordion-item header="Accordion Item #3" >}}
    This is the third item's accordion body.
  {{< /accordion-item >}}
{{< /accordion >}}

## 2. Alert

{{< alert type="info" >}}
    A simple info type alert—check it out!
{{< /alert >}}

{{< alert type="danger" >}}
    A simple danger type alert—check it out!
{{< /alert >}}

{{< alert color="warning" >}}
    A simple warning color alert—check it out!
{{< /alert >}}

{{< alert color="success" >}}
    A simple success color alert—check it out!
{{< /alert >}}

## 3. Badge

<h4>Example heading with Badge <span class="badge bg-secondary">New</span></h4>
<h4>Example heading with Badge <span class="badge text-bg-danger">Danger</span></h4>
<h4>Example heading with Badge <span class="badge text-bg-warning">Warning</span></h4>
<h4>Example heading with Badge <span class="badge text-bg-success">Success</span></h4>

## 4. Breadcrumb

{{< breadcrumb >}}

## 5. Button

{{< button color="dark" tooltip="Click on the inbox to view your unread messages" href="#!" badge="99+" >}}
    Inbox
{{< /button >}}

## 6. Card

{{< card path="bootstrap-elements" header="publication" footer="tags" orientation="horizontal" color="" class="col-sm-12 col-lg-8 mx-auto w-90" >}}

## 7. Carousel

{{< carousel ratio="16x9" class="col-sm-12 col-lg-8 mx-auto w-100" >}}
  {{< img src="https://futurelog-1251943639.cos.accelerate.myqcloud.com/img/forest-20230808.jpg" caption="forest" >}}
  {{< img src="img/hero-left.jpg" caption="hero left" >}}
  {{< img src="img/hero-right.jpg" caption="hero right" >}}
{{< /carousel >}}

## 8. Collapse

{{< button collapse="collapse-1" >}}
    toggle collapse
{{< /button >}}
{{< collapse id="collapse-1" class="p-3 border rounded" >}}
    Some placeholder content for the collapse component. This panel is <i>hidden by default</i> but
    revealed when the user activates the relevant trigger.
{{< /collapse >}}

## 9. Command
- bash
{{< command user="wallezen" host="localhost" shell="bash" >}}
export MY_VAR=123
echo "hello"
(out)hello
echo one \
two \
three
(out)one two three
echo "goodbye"
(out)goodbye
{{< /command >}}

- sql
{{< command prompt="mysql>" shell="sql" >}}
set @my_var = 'foo';
set @my_other_var = 'bar';
CREATE TABLE people ((con)
first_name VARCHAR(30) NOT NULL,(con)
last_name VARCHAR(30) NOT NULL(con)
);
(out)Query OK, 0 rows affected (0.09 sec)
insert into people(con)
values ('John', 'Doe');
(out)Query OK, 1 row affected (0.02 sec)
select *(con)
from people(con)
order by last_name;
(out)+------------+-----------+
(out)| first_name | last_name |
(out)+------------+-----------+
(out)| John       | Doe       |
(out)+------------+-----------+
(out)1 row in set (0.00 sec)
{{< /command >}}

## 10. Docs

{{< docs name="main" file="config/_default/hugo.toml" id="docs-collapse-1" show="false" >}}

## 11. File

{{< file path="config/_default/languages.toml" id="file-collapse-1" show="true">}}

## 12. Example

{{< example lang="hugo" >}}
    {{</* command */>}}
    export MY_VAR=123
    {{</* /command */>}}
{{< /example >}}

## 13. Icon

{{< example lang="hugo" >}}
    {{</* icon fa square-check */>}}
    {{</* fa square-check */>}}
    {{</* fab weixin */>}}
    {{</* fas circle-check */>}}
{{< /example >}}

## 14. Image

- image shortcode
{{< image src="https://futurelog-1251943639.cos.accelerate.myqcloud.com/img/forest-20230808.jpg" caption="forest" ratio="16x9" class="rounded" >}}

- markdown image: only can display original size

![hero-left-20230808](https://futurelog-1251943639.cos.accelerate.myqcloud.com/img/hero-left-20230808.jpg)

## 15. Link

- {{< link hinode_docs >}}Named link with default settings{{< /link >}}
- {{< link name=hinode_docs cue=false tab=false >}}Named link opening in current tab w/o icon{{< /link >}}
- {{< link name=hinode_docs cue=true tab=true >}}Named link opening in new tab with icon{{< /link >}}
- {{< link hinode_docs />}}
- {{< link "https://developer.mozilla.org" >}}External link{{< /link >}}
- {{< link "/bootstrap-elements" >}}Internal link with title{{< /link >}}

## 16. Map

- leaflet Map
{{< map id="leaflet-map-shenzhen" lat=22.54 long=114.06 popup="Shenzhen" popup-lat=22.54845664  popup-long=114.06455184 >}}

- Baidu Map

-  Google Map
<iframe class="w-100" src="https://www.google.com/maps/embed?pb=!1m14!1m12!1m3!1d236113.05374274624!2d114.168639!3d22.38131!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!5e0!3m2!1sen!2shk!4v1691492724590!5m2!1sen!2shk" width="600" height="450" style="border:0;" allowfullscreen="" loading="lazy" referrerpolicy="no-referrer-when-downgrade"></iframe>


## 17. Tab

{{< nav type="tabs" id="tabs-1" >}}
  {{< nav-item header="Nav Item #1" show="true" >}}
    This is the first item's nav body. It supports HTML content. The item is shown by adding the value
    <code>show</code> to the <code>class</code> argument.
  {{< /nav-item >}}
  {{< nav-item header="Nav Item #2" >}}
    This is the second item's nav body. It too supports HTML content.
  {{< /nav-item >}}
  {{< nav-item header="Nav Item #3" disabled="true" />}}
{{< /nav >}}

## 18. Release

{{< release version="v0.14.0" short="false" state="deprecated" >}}

{{< release version="v0.14.1" short="false" >}}

## 19. Tooltip

{{< tooltip color="primary" title="Tooltip" href="#!" placement="top" >}}Top{{< /tooltip >}}&bull;
{{< tooltip color="secondary" title="Tooltip" href="#!" placement="bottom" >}}Bottom{{< /tooltip >}}&bull;
{{< tooltip color="info" title="Tooltip" href="#!" placement="left" >}}Left{{< /tooltip >}}&bull;
{{< tooltip color="warning" title="Tooltip" href="#!" placement="right" >}}Right{{< /tooltip >}}


## 20. Timeline

{{< timeline data="timeline-zh" >}}


