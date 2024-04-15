##### AJAX Asynchronous JavaScript and XML
- a technique in which a web app fetches content from server by making async HTTP requests, and uses new content to update parts of page
- Used for SPAs. 
- Initially,  implemented using the [`XMLHttpRequest`](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) interface, now [`fetch()`](https://developer.mozilla.org/en-US/docs/Web/API/fetch) 

##### Single Page APP
- A single webpage sent by server that makes _incremental_ updates to itself over user session
- It performs _client-side routing_ to eliminate the need to fetch new pages from server and avoid costly repaints
- CSR enables developers to manipulate *history* stack without making a document request to the server.

[10 rendering patterns](https://www.youtube.com/watch?v=Dkx5ydvtpCA)