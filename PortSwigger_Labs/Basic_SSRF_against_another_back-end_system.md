# Basic SSRF Against Another Back-End System

## Lab Overview

In this lab, we learned how to identify internal servers behind a web application, enumerate internal IP addresses, and exploit an SSRF vulnerability to access a back-end administrative interface.

We used the stock-check functionality to manipulate URLs and interact with internal resources that were not directly accessible from the outside.

![Overview](Screenshots/01_ssrf_problem.png)

## 1. Identifying the SSRF Entry Point

After exploring the application, we identified the stock-check functionality as a potential entry point for SSRF testing.

![Stock Functionality](Screenshots/02-stock-functionality.png)

## 2. Capturing and Analysing the HTTP Request

The next step was to capture the HTTP request using Burp Suite. This allowed us to inspect the parameters and understand how the application handled the stock-check request.

![Request Capture](Screenshots/03-http-request-capture.png)

After examining the request, we identified the parameter responsible for fetching the stock information.

![SSRF Vulnerability Identification](Screenshots/04-ssrf-vulnerability-identification.png)

## 3. Testing the SSRF Vulnerability

We modified the request and observed how the application handled the supplied URL. This helped us confirm that the server was making requests based on our input.

![Initial Testing](Screenshots/05-initial-testing.png)

## 4. Internal Host Enumeration

After confirming the SSRF vulnerability, we used Burp Suite Intruder with a Sniper attack to enumerate internal hosts.

We configured the payload to test the last octet of the internal IP address, using values ranging from `2` to `255`.

![Internal Host Enumeration](Screenshots/06-internal-host-enumeration.png)

We placed the payload in the last octet of the target IP address and started the attack.

![SSRF Payload](Screenshots/07-ssrf-payload.png)

By analysing the responses, we identified an internal host that was accessible through the SSRF vulnerability.

![Successful SSRF Exploitation](Screenshots/08-successful-ssrf-exploitation.png)

## 5. Discovering the Internal Admin Interface

After identifying the internal host, we modified the request to access its administrative endpoint.

![Admin Request Discovery](Screenshots/09-admin-request-discovery.png)

We then analysed the HTTP response and examined the request pattern to understand how the administrative functionality worked.

![Admin Request Analysis](Screenshots/10-admin-request-analysis.png)

## 6. Modifying the Request to Delete the User

Using the information obtained from the response, we modified the HTTP request to perform the intended administrative action.

![Modified HTTP Request](Screenshots/11-modified-http-request.png)

Finally, we used the modified request to delete the user `carlos`, successfully completing the lab.

![Lab Completion](Screenshots/12-lab-completion.png)

## Key Takeaways

- Identifying potential SSRF entry points in web applications.
- Capturing and manipulating HTTP requests using Burp Suite.
- Understanding how SSRF can expose internal network resources.
- Enumerating internal hosts using Burp Suite Intruder.
- Analysing HTTP responses to identify accessible internal services.
- Exploiting SSRF to interact with internal administrative functionality.

### Main Takeaway

SSRF is not limited to accessing a single internal URL. When combined with internal host enumeration and response analysis, it can expose back-end services and potentially provide access to sensitive administrative functionality.s