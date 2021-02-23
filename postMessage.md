官方文档：https://developer.mozilla.org/zh-CN/docs/Web/API/Window/postMessage
1、window.postMessage() 方法可以安全地实现跨源通信
    对于两个不同页面的脚本，只有当执行它们的页面位于具有相同的协议（通常为https），端口号（443为https的默认值），以及主机  (两个页面的模数 Document.domain设置为相同的值) 时，这两个脚本才能相互通信。
2、A页面中通过iframe集成B页面
    2.1、A页面向B页面发送消息，使用postMessage
        语法格式：otherWindow.postMessage(message, targetOrigin, [transfer]);
        2.1.1、otherWindow：B页面的一个引用，比如使用iframe集成的B页面的contentWindow属性等
        2.1.2、message：将要发送到B页面的数据
        2.1.3、targetOrigin：指定B页面中的哪些窗口可以接收到消息，值可为'*'或者具体的URL
                在发送消息的时候，如果目标窗口的协议、主机地址或端口这三者的任意一项不匹配targetOrigin提供的值，
                那么消息就不会被发送；只有三者完全匹配，消息才会被发送。
                这个机制用来控制消息可以发送到哪些窗口
               tips：必须保证它的值和信息的预期接受者的origin属性完全一致，防止被第三方截获
        2.1.4、transfer：可选参数，是一串和message同时传递的对象
                这些对象的所有权将被转移给消息的接收方，而发送一方将不再  保有所有权。
3、B页面监听A页面发送的消息
    3.1、B页面监听消息
        语法格式：window.addEventListener("message", receiveMessage, false);
        3.1.1、第一个参数字符串"message"属固定写法，不可更改
        3.1.2、receiveMessage：代表具体的监听函数，这个函数会接收到一个参数event
                event是一个对象，包含data、origin、source等属性
                data：从A页面发送的消息
                origin：消息发送方的origin，这里指A页面的origin
                source：对发送消息的窗口对象的引用
        3.1.3、指定事件在冒泡阶段执行
4、安全问题
    4.1、如果仅仅是集成B页面，不监听任何消息，不用再B页面添加监听器
    4.2、使用postMessage将数据发送到其他窗口时，始终指定精确的目标origin，而不是'*'
    4.3、监听时，始终使用使用origin属性验证发件人的身份，即是否为所期望的页面发送的消息，保证安全性
5、浏览器兼容性
    可参考：https://developer.mozilla.org/zh-CN/docs/Web/API/Window/postMessage