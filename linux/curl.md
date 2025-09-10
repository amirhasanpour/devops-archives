# **Curl Command in Linux - Complete Guide**

---

The `curl` command in Linux is a powerful tool used for transferring data to or from a server using various protocols, including **HTTP, HTTPS, FTP, SCP, SFTP, LDAP, and more**. It is widely used for making API requests, downloading files, and testing network connections.

---

# **Basic Syntax**
```sh
curl [options] [URL]
```
For example:
```sh
curl https://example.com
```
This fetches the HTML content of `https://example.com` and prints it to the terminal.

---

# **Commonly Used Options**

## **1. Download a File**
```sh
curl -O https://example.com/file.zip
```
- `-O`: Saves the file with the same name as on the server.

Alternatively, specify a custom name:
```sh
curl -o myfile.zip https://example.com/file.zip
```
- `-o`: Saves the file with a custom name.

---

## **2. Send HTTP GET Request**
```sh
curl https://api.example.com/data
```
This retrieves data from the API.

---

## **3. Send HTTP POST Request**
```sh
curl -X POST -d "username=user&password=pass" https://example.com/login
```
- `-X POST`: Specifies the HTTP method as `POST`.
- `-d`: Sends data in the request body.

Sending JSON data:
```sh
curl -X POST -H "Content-Type: application/json" -d '{"name":"John"}' https://example.com/api
```
- `-H`: Sets the HTTP header.

---

### **4. Send HTTP Request with Authentication**
```sh
curl -u user:password https://example.com/protected
```
- `-u`: Sends basic authentication credentials.

---

### **5. Follow Redirects**
```sh
curl -L https://short.url
```
- `-L`: Follows redirected URLs.

---

### **6. Save Output to a File**
```sh
curl -o output.txt https://example.com
```
This saves the response to `output.txt` instead of displaying it.

---

### **7. Show Response Headers**
```sh
curl -I https://example.com
```
- `-I`: Fetches only the headers, not the body.

---

### **8. Test APIs (Verbose Mode)**
```sh
curl -v https://api.example.com
```
- `-v`: Shows detailed request and response information.

---

### **9. Download a File in Background**
```sh
curl -O https://example.com/largefile.zip &
```
- `&`: Runs the command in the background.

---

