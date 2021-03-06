# XSS


## Content Security Policy

Github 使用 Electron 构建编辑器 Atom，其使用了 CSP 来限制潜在的 XSS 代码执行：

```html
// index.html
<!DOCTYPE html>
<html>
   <head>
      <meta http-equiv="Content-Security-Policy" content="default-src * atom://*; img-src blob: data: * atom://*; script-src 'self' 'unsafe-eval'; style-src 'self' 'unsafe-inline'; media-src blob: data: mediastream: * atom://*;">
      <script src="index.js"></script>
   </head>
   <body tabindex="-1"></body>
</html>
```

The script-src 'self' 'unsafe-eval', means that JavaScript from the same origin as well as code created using an eval like construct will by be executed. However, any inline JavaScript is forbidden.

In a nutshell, the JavaScript from “index.js” would be executed in the following sample, the alert(1) however not, since it is inline JavaScript:

```js
<!DOCTYPE html>
<html>
   <head>
      <meta http-equiv="Content-Security-Policy" content="default-src * atom://*; img-src blob: data: * atom://*; script-src 'self' 'unsafe-eval'; style-src 'self' 'unsafe-inline'; media-src blob: data: mediastream: * atom://*;">
   </head>
   <!-- Following line will be executed since it is JS embedded from the same origin -->
   <script src="index.js"></script>
   <!-- Following line will not be executed since it is inline JavaScript -->
   <script>alert(1)</script>
</html>
```
