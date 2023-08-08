---
author: wallezen
title: Markdown 示例
date: 2023-08-08
description: 常用 Markdown 语法的示例效果。
tags: ["Markdown", "示例"]
thumbnail: https://futurelog-1251943639.cos.accelerate.myqcloud.com/img/up25Ya-20230808.png
photoCredits: <a href="https://twitter.com/wallezen007">wallezen</a>
photoSource: <a href="/">Futurelog</a>
draft: true
modules:
    - leaflet
    - katex
---


## Quote 示例

> Don't communicate by sharing **memory**, share memory by **communicating**.<br>
> — <cite>Rob Pike[^1]</cite>
{.blockquote}

[^1]: The above quote is excerpted from Rob Pike's [talk](https://www.youtube.com/watch?v=PAAkCSZUG1c) during Gopherfest, November 18, 2015.

## Cite 示例

This is a reference to a book[^2].

[^2]: The Go Programming Language. Alan A. A. Donovan and Brian W. Kernighan. 2015. Addison-Wesley Professional.


## Math 示例

This is an inline $-b \pm \sqrt{b^2 - 4ac} \over 2a$ formula

This is not an inline formula:
$$x = a_0 + \frac{1}{a_1 + \frac{1}{a_2 + \frac{1}{a_3 + a_4}}}$$  
$$\forall x \in X, \quad \exists y \leq \epsilon$$


## Code Demo
### go
```go {hl_lines=[8]}
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

## Video Demo
### youtube
<iframe width="100%" height="315" src="https://www.youtube.com/embed/7EmboKQH8lM" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### bilibili
<iframe src="https://player.bilibili.com/player.html?aid=202816429&bvid=BV1Ea411w7zE&cid=258477979&page=1&high_quality=1" width="100%" height="420" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>

## Presentation Demo

- slidev
<iframe src="https://bytememo.com/presentations/slidev-demo" allow="fullscreen" allowfullscreen="" width="100%" height="420" style="border:0"></iframe>

- webslides
<iframe src="https://futurelog.xyz/webslides" allow="fullscreen" allowfullscreen="" width="100%" height="520" style="border:0"></iframe>

## Figma Demo
<iframe style="border: 1px solid rgba(0, 0, 0, 0.1);" width="800" height="450" src="https://www.figma.com/embed?embed_host=share&url=https%3A%2F%2Fwww.figma.com%2Ffile%2Fkwig31ctYeZ7GOAiRidZNw%2FUntitled%3Fnode-id%3D0%253A1" allowfullscreen></iframe>

## Image Demo
![hero-left-20230808](https://futurelog-1251943639.cos.accelerate.myqcloud.com/img/hero-left-20230808.jpg)


## ProcessOn Demo
<iframe id="embed_dom" name="embed_dom" frameborder="0" width="100%" height="520" src="https://www.processon.com/embed/5b188be6e4b00490ac8bdc2f"></iframe>

