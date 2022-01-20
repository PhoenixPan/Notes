# RESTful

(Resource) Representational State Transfer is a designed pattern built on HTTP, as long as the procedure satisfy certain principles, we could say that this procedure is RESTful.

In traditional website we use `/getBook.action?id=1` (verb) to get resources. It's a procedural-oriented style. However, in RESTful, we use noun to specify where the resources are `/books/1` and apply proper request to access the same endpoint, such as GET, POST, PUT, and DELETE. It's a resource-oriented style.

Some servers use GET request to obtain resources and POST request to create, update, and delete resources. These are not real RESTful and sometimes call hybrid RESTful.

**We know what you want to do through HTTP request**  
**We know what you want through URI**  
**We know how things going through status code**

1. Representations: Different resources have differect representations, such as txt, html, xml for text,jpg or png for image. json is currently the most popular representation but other formats are also fine if appropriate.
2. URIs: Each URI exposes a specific resource, like `/books/1`.
3. Client-Server: Separation of user interface concerns and the data storage concerns.
4. Stateless: The server does not keep any session information. If serves keep session info, then a request has to be handled by a particular server. To achieve, a RESTful request should contain all info that servers might need. Without this burden on the server side, any server in the cluster can handle this request. Since we no longer need to worry about the context of the request, it dramatically reduces the complexity and improves the efficiency.
5. Cache: Data within a response to a request must be labeled as cacheable or non-cacheable. If a response is cacheable, then a client cache is given the right to reuse that response data for later or equivalent requests. Increase efficiency, scalability, and performance by partially or completely eliminating some interactions.
6. Uniform interface: Allowing atomic CRUD(create, read, update, and delete) operations. Different technical structure on each side will not affect communication, as long as APIs are provided.
7. Layered system: Add intermediary layers between the end server and the clients. This may improve system scalability by enabling load balancing and by providing shared caches.

\*Client-Server model: partitions tasks or workloads between clients and servers.

## Design Server API

1. URL root:
   https://example.org/api/
2. Versoning:

   1. Bad practices: add into the URI  
      https://example.org/api/v1/

   2. Good practice: add into HTTP header - accept
      Accept: vnd.example-com.foo+json; version=1.0

3. Use noun rather than verb, pluralrather than singular

   1. Bad practices:  
      /getProducts  
      /listOrders  
      /retrieveClientByOrder?orderId=1

   2. Good practices:  
      GET /products : will return the list of all products  
      POST /products : will add a product to the collection  
      GET /products/4 : will retrieve product #4PATCH/  
      PUT /products/4 : will update product #4  
      **Let the keywords do what they suppose to do! not just GET and POST anymore**

4. Encrypt the payload before send out if necessary

## References

http://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm
http://www.ruanyifeng.com/blog/2011/09/restful.html
