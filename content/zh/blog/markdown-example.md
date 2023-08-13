---
author: wallezen
title: Markdown 示例
date: 2023-08-08
description: 常用 Markdown 语法的示例效果。
tags: ["Markdown", "示例"]
thumbnail: https://futurelog-1251943639.cos.accelerate.myqcloud.com/img/up25Ya-20230808.png
photoCredits: <a href="https://twitter.com/wallezen007">wallezen</a>
photoSource: <a href="/">Futurelog</a>
draft: false
modules:
    - leaflet
    - katex
---

## Text 示例

- basic text

~~This line of text is meant to be treated as deleted text.~~

_This line of text renders as underlined._

**This line of text renders as bold text.**

*This line of text renders as italicized text.*

- lead text

<p class="lead">
  This is a lead paragraph. It stands out from regular paragraphs.
</p>

- emoji[^1]

That is so funny! :smiley:
    
[^1]: [Emoji cheat sheet](https://www.webfx.com/tools/emoji-cheat-sheet/)

- abbreviation

<p><abbr title="HyperText Markup Language">HTML</abbr></p>


## List 示例

- Fruit
  - Apple
  - Orange
  - Banana
- Dairy
  - Milk
  - Cheese


- [x] Write the press release
- [ ] Update the website
- [ ] Contact the media


## Quote 示例

> Don't communicate by sharing **memory**, share memory by **communicating**.<br>
> — <cite>Rob Pike[^2]</cite>
{.blockquote}

[^2]: The above quote is excerpted from Rob Pike's [talk](https://www.youtube.com/watch?v=PAAkCSZUG1c) during Gopherfest, November 18, 2015.

## Cite 示例

This is a reference to a book[^3].

[^3]: The Go Programming Language. Alan A. A. Donovan and Brian W. Kernighan. 2015. Addison-Wesley Professional.


## Math 示例

This is an inline $-b \pm \sqrt{b^2 - 4ac} \over 2a$ formula

This is not an inline formula:
$$x = a_0 + \frac{1}{a_1 + \frac{1}{a_2 + \frac{1}{a_3 + a_4}}}$$  
$$\forall x \in X, \quad \exists y \leq \epsilon$$


## Table 示例

- basic table

| Italics   | Bold     | Code   |
| --------- | -------- | ------ |
| _italics_ | **bold** | `code` |
{.table}

- table with alignment

{{< table "table-striped" >}}
| #  | Item        | Left aligned | Center aligned |   Right aligned|
| -- | ----------- |:-------------|:--------------:| --------------:|
| 1. | First item  | some text    | more text      | even more text |
| 2. | Second item | some text    | more text      | even more text |
| 3. | Third item  | some text    | more text      | even more text |
{{< /table >}}

- table with border

| #  | Item        |
| -- | ----------- |
| 1. | First item  |
| 2. | Second item |
| 3. | Third item  |
{.table .table-bordered .border-primary}

## Code 示例
### go
```go {hl_lines=[3,4,5,6,7]}
package main

import (
    "fmt"
    "io/ioutil"
    "os/exec"
)

func main() {

    dateCmd := exec.Command("date")

    dateOut, err := dateCmd.Output()
    if err != nil {
        panic(err)
    }
    fmt.Println("> date")
    fmt.Println(string(dateOut))

    grepCmd := exec.Command("grep", "hello")

    grepIn, _ := grepCmd.StdinPipe()
    grepOut, _ := grepCmd.StdoutPipe()
    grepCmd.Start()
    grepIn.Write([]byte("hello grep\ngoodbye grep"))
    grepIn.Close()
    grepBytes, _ := ioutil.ReadAll(grepOut)
    grepCmd.Wait()

    fmt.Println("> grep hello")
    fmt.Println(string(grepBytes))

    lsCmd := exec.Command("bash", "-c", "ls -a -l -h")
    lsOut, err := lsCmd.Output()
    if err != nil {
        panic(err)
    }
    fmt.Println("> ls -a -l -h")
    fmt.Println(string(lsOut))
}
```

### python
```python {hl_lines=[3]}
import matplotlib.pyplot as plt

price = [2.50, 1.23, 4.02, 3.25, 5.00, 4.40]
sales_per_day = [34, 62, 49, 22, 13, 19]

plt.scatter(price, sales_per_day)
plt.show()
```

## Video 示例
### youtube
<iframe width="100%" height="315" src="https://www.youtube.com/embed/7EmboKQH8lM" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### bilibili
<iframe src="https://player.bilibili.com/player.html?aid=202816429&bvid=BV1Ea411w7zE&cid=258477979&page=1&high_quality=1" width="100%" height="420" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>

## Presentation 示例

- slidev
<iframe src="https://futurelog.xyz/slides/slidev-demo/1" allow="fullscreen" allowfullscreen="" width="100%" height="420" style="border:0"></iframe>

- webslides
<iframe src="https://futurelog.xyz/slides/demos/why-webslides.html#slide=1" allow="fullscreen" allowfullscreen="" width="100%" height="520" style="border:0"></iframe>

## Figma 示例
<iframe style="border: 1px solid rgba(0, 0, 0, 0.1);" width="800" height="450" src="https://www.figma.com/embed?embed_host=share&url=https%3A%2F%2Fwww.figma.com%2Ffile%2Fkwig31ctYeZ7GOAiRidZNw%2FUntitled%3Fnode-id%3D0%253A1" allowfullscreen></iframe>

## Image 示例
![forest-x1920](https://futurelog.xyz/galleries/travel/forest-x1920.jpg)


## ProcessOn 示例
<iframe id="embed_dom" name="embed_dom" frameborder="0" width="100%" height="520" src="https://www.processon.com/embed/5b188be6e4b00490ac8bdc2f"></iframe>

