---
title: "Confidential - PDF x QR Code Scan (TryHackMe)"
datePublished: Tue Jan 27 2026 17:19:00 GMT+0000 (Coordinated Universal Time)
cuid: cmkwv238i000a02l7f5un1hrw
slug: confidential-pdf-x-qr-code-scan-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1769533860682/771de684-87fb-4cfc-9ba5-f652be9a051a.png
tags: pdf, qrcode, tryhackme, digital-forensics, ctf-writeup

---

## Introduction

This challenge focuses on PDF forensics and steganography techniques. The scenario involves a confidential document from alleged "black hat hackers" containing a hidden QR code that's been covered by another image layer. Our objective is to extract and decode this QR code to retrieve a secret invite code.

* **Challenge Category:** Digital Forensics
    
* **Skills Required:** PDF analysis, image extraction, basic Linux command-line tools
    
* **Key Learning Points:** Understanding PDF structure, extracting embedded objects, and recognizing layered content obfuscation
    

## Confidential

We got our hands on a confidential case file from some self-declared "black hat hackers"... it looks like they have a secret invite code available within a QR code, but it's covered by some image in this PDF! If we want to thwart whatever it is they are planning, we need your help to uncover what that QR code says!

Access this challenge by deploying the machine attached to this task by pressing the green "Start Machine" button. This machine shows in Split View in your browser, if it doesn't automatically display you may need to click "Show Split View" in the top right.  
  
The file you need is located in **/home/ubuntu/confidential** on the VM.

Check out similar content on TryHackMe:

* [Intro to digital forensics](https://tryhackme.com/room/introdigitalforensics)
    

### Answer the questions below

1. Uncover and scan the QR code to retrieve the flag! `flag{e08e6ce2f077a1b420cfd4a5d1a57a8d}`
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769533825999/d25a7897-13d4-45c9-a4fc-6211733a37f0.png align="center")

```bash
pdfimages -all Repdf.pdf output_prefix
ubuntu@thm-confidential:~/confidential$ ls  
Repdf.pdf  output_prefix-000.png  output_prefix-001.png  output_prefix-002.png
ubuntu@thm-confidential:~/confidential$ 
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769533852872/4528f3a5-6401-4c1e-b1b2-2f4d54ac4f77.png align="center")

  
opened `out_prefix-001.png` and scanned it.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769533983782/069b3e5e-4f56-41b3-90e4-4187c67f3571.jpeg align="center")

## Conclusion

This challenge demonstrated a common technique used in PDF-based steganography: hiding information by layering images on top of each other. By using `pdfimages`, we were able to extract all embedded images from the PDF file separately, revealing the hidden QR code that was obscured in the original document.

**Key Takeaways:**

* PDFs can contain multiple image layers that may not all be visible in standard viewers
    
* The `pdfimages` utility is essential for extracting embedded images for forensic analysis
    
* Attackers may hide sensitive information within document objects, making metadata and object extraction critical skills in digital forensics
    
* Always examine all extracted artifacts individually, as hidden content may only be visible when isolated from other layers
    

This technique is commonly used in real-world scenarios for both malicious purposes (data exfiltration, hidden communications) and legitimate ones (watermarking, copyright protection). Understanding how to extract and analyze PDF components is a fundamental skill in cybersecurity investigations.