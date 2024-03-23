---
title: Markdown Syntax Guide
date: 2020-01-01
authors:
  - name: imfing
    link: https://github.com/imfing
    image: https://github.com/imfing.png
  - name: Octocat
    link: https://github.com/octocat
    image: https://github.com/octocat.png
tags:
  - Markdown
  - Example
  - Guide
excludeSearch: true
---

This article offers a sample of basic Markdown syntax that can be used in Hugo content files.
<!--more-->

## Basic Syntax

### Headings

```
# Heading 1
## Heading 2
### Heading 3
#### Heading 4
##### Heading 5
###### Heading 6
```

## Heading 2
### Heading 3
#### Heading 4
##### Heading 5
###### Heading 6

### Emphasis

```text
*This text will be italic*
_This will also be italic_

**This text will be bold**
__This will also be bold__

_You **can** combine them_
```

*This text will be italic*

_This will also be italic_

**This text will be bold**

__This will also be bold__

_You **can** combine them_

### Lists

#### Unordered

```
* Item 1
* Item 2
  * Item 2a
  * Item 2b
```

* Item 1
* Item 2
  * Item 2a
  * Item 2b

#### Ordered

```
1. Item 1
2. Item 2
3. Item 3
   1. Item 3a
   2. Item 3b
```

### Images

```markdown
![GitHub Logo](https://github.githubassets.com/images/modules/logos_page/GitHub-Mark.png)
```

![GitHub Logo](https://github.githubassets.com/images/modules/logos_page/GitHub-Mark.png)

### Links

```markdown
[Hugo](https://gohugo.io)
```

[Hugo](https://gohugo.io)

### Blockquotes

```markdown
As Newton said:

> If I have seen further it is by standing on the shoulders of Giants.
```

> If I have seen further it is by standing on the shoulders of Giants.

### Inline Code

```markdown
Inline `code` has `back-ticks around` it.
```

Inline `code` has `back-ticks around` it.

### Code Blocks

#### Syntax Highlighting

````markdown
```go
func main() {
    fmt.Println("Hello World")
}
```
````

```go
func main() {
    fmt.Println("Hello World")
}
```

### Tables

```markdown
| Syntax    | Description |
| --------- | ----------- |
| Header    | Title       |
| Paragraph | Text        |
```

| Syntax    | Description |
| --------- | ----------- |
| Header    | Title       |
| Paragraph | Text        |


## Shortcodes
- Cards

{{< cards >}}
  {{< card link="/" title="Image Card" image="https://source.unsplash.com/featured/800x600?landscape" subtitle="Unsplash Landscape" >}}
  {{< card link="/" title="Local Image" image="/images/card-image-unprocessed.jpg" subtitle="Raw image under static directory." >}}
  {{< card link="/" title="Local Image" image="images/space.jpg" subtitle="Image under assets directory, processed by Hugo." method="Resize" options="600x q80 webp" >}}
{{< /cards >}}

- Callouts
{{< callout type="info" >}}
  Please visit GitHub to see the latest releases.
{{< /callout >}}

{{< callout emoji="🌐" >}}
  Hugo can be used to create a wide variety of websites, including blogs, portfolios, documentation sites, and more.
{{< /callout >}}

{{< callout type="warning" >}}
  A **callout** is a short piece of text intended to attract attention.
{{< /callout >}}


{{< callout type="error" >}}
  Something went wrong and it's going to explode.
{{< /callout >}}

- Details
{{% details title="Details" %}}

This is the content of the details.

Markdown is **supported**.

{{% /details %}}

{{% details title="Click me to reveal" closed="true" %}}

This will be hidden by default.

{{% /details %}}


- Filetree

{{< filetree/container >}}
  {{< filetree/folder name="content" >}}
    {{< filetree/file name="_index.md" >}}
    {{< filetree/folder name="docs" state="closed" >}}
      {{< filetree/file name="_index.md" >}}
      {{< filetree/file name="introduction.md" >}}
      {{< filetree/file name="introduction.fr.md" >}}
    {{< /filetree/folder >}}
  {{< /filetree/folder >}}
  {{< filetree/file name="hugo.toml" >}}
{{< /filetree/container >}}

- Icon

{{< icon "github" >}}


- Steps

{{% steps %}}

### Step 1

This is the first step.

### Step 2

This is the second step.

{{% /steps %}}


- Tabs

{{< tabs items="JSON,YAML,TOML" >}}

  {{< tab >}}**JSON**: JavaScript Object Notation (JSON) is a standard text-based format for representing structured data based on JavaScript object syntax.{{< /tab >}}
  {{< tab >}}**YAML**: YAML is a human-readable data serialization language.{{< /tab >}}
  {{< tab >}}**TOML**: TOML aims to be a minimal configuration file format that's easy to read due to obvious semantics.{{< /tab >}}

{{< /tabs >}}

## iframe

- qq music
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=95% height=65 src="https://i.y.qq.com/n2/m/outchain/player/index.html?songid=290873678&songtype=0"></iframe>

- gamma 

<iframe src="https://gamma.app/embed/5ffiu0t41xifbv6" style="width: 700px; max-width: 100%; height: 450px" allow="fullscreen" title="提示工程 - Prompt 编写的策略与模板"></iframe>

- xmind

<iframe src="https://xmind.ai/embed/GATuQsR4?sheet-id=b6a2b210-e0a2-4ce9-a062-bc4387716f33" width="100%" height="540px" frameborder="0" scrolling="no" allow="fullscreen"></iframe>

- youtube
<iframe width="560" height="315" src="https://www.youtube.com/embed/jvqFAi7vkBc?si=-OyoTTt_ubNdk8Be" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

- feishu
<iframe src="https://q8ep7dobil.feishu.cn/mindnotes/Gad3bi5q3mtzS9n0rSecx4DVnSc?from=from_copylink" width="100%" height="600px" frameborder="0" scrolling="no" allow="fullscreen"></iframe>

<iframe src="https://q8ep7dobil.feishu.cn/docx/S1EEdJ38RoQfDYxsR5VcRGh8nod?openbrd=1&doc_app_id=501&blockId=doxcneALDDTzBfzjkFTkhbnBPqc&blockType=whiteboard&blockToken=EBC0wSRPvhCbqDbkijKc6FtTn0c#doxcneALDDTzBfzjkFTkhbnBPqc" width="100%" height="600px" frameborder="0" scrolling="no" allow="fullscreen"></iframe>

## References

- [Markdown Syntax](https://www.markdownguide.org/basic-syntax/)
- [Hugo Markdown](https://gohugo.io/content-management/formats/#markdown)
