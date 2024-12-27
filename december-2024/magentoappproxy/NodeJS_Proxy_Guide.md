
# Quick Guide to Using Your Node.js Proxy Server

**Purpose**:  
This Node.js proxy server forwards GraphQL requests from your frontend to a Magento backend, handling SSL encryption and bypassing certificate validation for local development with self-signed certificates. We created this proxy server to easily access the Magento GraphQL API from an Android Emulator, ensuring secure communication during development.

## 1. Prerequisites
- Install **Node.js** and **npm**.
- Install required packages:  
  ```bash
  npm install express http-proxy-middleware
  ```

## 2. Generate SSL Certificates
- Use OpenSSL to generate a self-signed certificate for local development:
  ```bash
  openssl genpkey -algorithm RSA -out ssl/key.pem
  openssl req -new -key ssl/key.pem -out ssl/cert.csr
  openssl x509 -req -in ssl/cert.csr -signkey ssl/key.pem -out ssl/cert.pem
  ```
- This creates `key.pem` and `cert.pem` files in the `ssl` folder.

## 3. Edit `/etc/hosts`
- Add the following line to your **`/etc/hosts`** file to resolve `magentoappproxy.test` locally:
  ```text
  127.0.0.1 magentoappproxy.test
  ```

## 4. Update `magentoGraphQLURL`
- Change the `magentoGraphQLURL` in the code to point to your local Magento GraphQL API URL:
  ```js
  const magentoGraphQLURL = 'https://app.mage246.test/graphql';  // Update if needed
  ```

## 5. Run the Proxy
- Start the server with:
  ```bash
  node proxy-server.js
  ```
- The proxy will be available at `https://magentoappproxy.test:4000/graphql`.

## 6. How It Works
- Requests to `/graphql` are forwarded to Magento’s GraphQL endpoint.
- SSL certificates are used for secure communication, with a custom agent to bypass self-signed certificate errors.

## 7. Testing
- Use a GraphQL client (e.g., Postman) to test the proxy by sending requests to `https://magentoappproxy.test:4000/graphql`.

## 8. Testing the Proxy on Android Emulator

Open the Chrome Browser. Access the url: `https://10.0.2.2:4000`

## 9. Important Notes
- Ensure the self-signed certificate is trusted by your client (e.g., Android Emulator).
- If you encounter issues, double-check the `/etc/hosts` file and certificate trust settings.
