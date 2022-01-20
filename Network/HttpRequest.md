## Access-Control-Allow-Origin
Error message:   
*XMLHttpRequest cannot load http://localhost:xxxx/feedback/xxx.jsp. Response to preflight request doesn't pass access control check: No 'Access-Control-Allow-Origin' header is present on the requested resource. Origin 'null' is therefore not allowed access.*

This is because the request is sent to a different origin that refuses it. For two documents to be considered to have the same origin, the **protocol (http/https)**, the **domain** and the **port** (the default 80 or :xx) have to be indentical. So localhost with different ports cannot communicate directly.  

Set a response header could resolve this issue:
```java
// Java
HttpServletResponse response = new HttpServletResponse();
response.setHeader("Access-Control-Allow-Origin","*"); // Allow request from all domains
```

Referece:  
http://stackoverflow.com/questions/1077218/are-different-ports-on-the-same-server-considered-cross-domain-ajax-wise

## Cross origin requests are only supported for protocol schemes: http, data, chrome, chrome-extension, https.
Don't launch from file system (file://) but from a http location.
