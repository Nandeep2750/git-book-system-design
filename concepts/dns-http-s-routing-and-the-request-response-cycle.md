# DNS, HTTP(S), Routing, and the Request-Response Cycle

## HTTP **(Hypertext Transfer Protocol)**

**HTTP (Hypertext Transfer Protocol)** is the most widely used protocol on the web to transfer data between your browser and a website’s server. It works on a request-response model: your browser sends a request, and the server sends back a response with the requested information.

### **What is HTTP?**

* HTTP is an application layer protocol, part of the TCP/IP suite.
* It enables web-based applications to communicate and exchange data, including images, videos, documents, and other multimedia content.
* Think of HTTP as the messenger that delivers requests and responses between your browser and the website’s server.

### **Key Features of HTTP:**

* **Connectionless:** After sending a request, the client disconnects from the server. When the response is ready, a new connection is established to deliver it.
* **Stateless:** Each request is independent. The server does not remember previous requests from the same client. Any state or session information must be managed separately.
* **Data Flexibility:** HTTP can deliver any type of data, as long as both client and server can interpret it.

### **How HTTP works**:

* You type a URL or click a link.
* Your browser sends an HTTP request to the server.
* The server processes the request and sends back an HTTP response (with a status code, headers, and data).
* Your browser displays the content.

## **HTTPS (Hypertext Transfer Protocol Secure)**:&#x20;

This is the secure version of HTTP. HTTPS encrypts the data exchanged between your browser and the server, protecting sensitive information like passwords and payment details.

### **How HTTPS works**:

* The browser connects to the server using HTTPS.
* The server sends its SSL/TLS certificate to the browser.
* The browser verifies the certificate.
* An encrypted connection is established.
* All data is encrypted before being sent and decrypted upon arrival.

\




{% embed url="https://youtu.be/eesqK59rhGA?si=8Do2dnhcSw8Ov-ii" %}
