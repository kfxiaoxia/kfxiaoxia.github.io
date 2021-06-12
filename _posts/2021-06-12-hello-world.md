---
layout: post
title: "Hello World"
date:  2021-06-12 16:30:11 +0800
categories: log
---

# Hello World

```
bdos    equ    0005H    ; BDOS entry point
start:  mvi    c,9      ; BDOS function: output string
        lxi    d,msg$   ; address of msg
        call   bdos
        ret             ; return to CCP
  
msg$:   db    'Hello, world!$'
end     start
```