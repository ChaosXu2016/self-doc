# Service Work

Service Work是运行在浏览器和web程序之间的代理服务，可以在网络可用时作为浏览器和网络之间的代理。它旨在提升web程序的历险体验。

## 特点

1. Service Worker是一个注册在制定源和路径下的事件驱动worker（后台单独线程跑的一个脚本）
2. 由于运行在worker上下文，所以Service Worker不能访问DOM元素
3. 不会阻塞主线程的Javascript运行
4. 设计上完全异步，无法使用同步的Api（XHR和localStorage）
5. 出于安全考虑，Service Worker只能由HTTPS承载。且在Firefox的用户隐私模式时不可用。

## 生命周期

1. install
2. active
3. Fetch

