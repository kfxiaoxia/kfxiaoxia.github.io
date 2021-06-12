---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults
layout: home
---

<link rel="stylesheet" href="/assets/css/style.css">

<h1 style="font-size: 60; font-weight: bold; color: red;">#Welcome here ...</h1>
<h1 style="font-size: 60; font-weight: bold; color: red;">#We are 🚧 </h1>
<h1 style="font-size: 60; font-weight: bold; color: red;">#See you next time ...</h1>


```
bdos    equ    0005H    ; BDOS entry point
start:  mvi    c,9      ; BDOS function: output string
        lxi    d,msg$   ; address of msg
        call   bdos
        ret             ; return to CCP
  
msg$:   db    'Hello, world!$'
end     start
```

