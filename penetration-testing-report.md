# Penetration Testing Report: OWASP Juice Shop Secure Deployment

**Project Title:** Cybersecurity & DevSecOps Training  
**Prepared By:** Rachid Dwyer  
**Date Completed:** June 2025  

---

## Executive Summary

This penetration test targeted the OWASP Juice Shop application deployed in a secure Docker container as part of a DevSecOps internship project. The goal was to simulate a real-world security assessment of a vulnerable web application, apply a fix to one major vulnerability, and validate its resolution through rescanning. This work reflects core principles of secure software development, vulnerability management, and iterative improvement within the DevSecOps lifecycle.

Initial scans using OWASP ZAP uncovered critical and high-risk vulnerabilities such as missing HTTP security headers. A security hardening fix was implemented using the Helmet.js middleware in the application’s Express server configuration. The Docker image was rebuilt and redeployed, and a final scan confirmed the successful mitigation of the targeted issue.

---

## Testing Methodology

- **Application:** OWASP Juice Shop (v17.3.0)
- **Deployment:** Docker container using image `juice-shop-secure`
- **Scanning Tool:** OWASP ZAP by Checkmarx
- **Scan Mode:** Manual Explore followed by Active Scan
- **Scan Target:** `http://localhost:3000`
- **Test Machine:** Windows 10 host running Docker Desktop and ZAP GUI
- **Browser for Manual Explore:** Chrome (system default)

The test simulated an unauthenticated attacker interacting with the application and scanning for misconfigurations, missing protections, and insecure default behaviors.

---

## Initial Findings

The first ZAP scan revealed multiple issues, including:

- **Missing HTTP Security Headers:**
  - `X-Frame-Options`
  - `Content-Security-Policy`
  - `X-XSS-Protection`
  - `Strict-Transport-Security`
- **Medium & Low Severity Issues:**
  - Insecure cookies
  - Server error information disclosure
  - Use of internal IPs in error messages
  - Missing cache control headers

**Notable Finding (Targeted for Fix):**
> Absence of HTTP security headers (particularly `X-Frame-Options` and `Content-Security-Policy`) left the application vulnerable to clickjacking and framing attacks.

---

## Fix Implementation

The most critical issue addressed in this report was the absence of essential HTTP security headers.

**Remediation Steps:**

1. Opened the application source code in Visual Studio Code.  
2. Located `server.ts` in the root directory of the Juice Shop project.  
3. Imported Helmet.js middleware at the top of the file:

   ```ts
   import helmet from 'helmet';
   ```

4. Applied the middleware immediately after initializing the Express `app`:

   ```ts
   app.use(helmet());
   ```

5. Saved the file, then rebuilt the Docker container from the project root using:

   ```bash
   docker build --no-cache -t juice-shop-secure .
   ```

6. Deployed the secured version using:

   ```bash
   docker run --rm -p 3000:3000 juice-shop-secure
   ```

This fix successfully added several essential security headers, including `Content-Security-Policy`, `X-Frame-Options`, and `X-Content-Type-Options`, helping to mitigate clickjacking and content injection risks.

---

## Validation

To validate the effectiveness of the applied security fix, a second OWASP ZAP scan was performed using the same methodology:

- **Scan Type:** Manual Explore + Active Scan  
- **Target:** http://localhost:3000  
- **Result:**  
  - The previous critical finding regarding missing HTTP headers was no longer detected.  
  - Only low-level and informational findings remained.  
  - No active medium- or high-severity issues were identified in the rescanned version of the application.

---

## Conclusion

This penetration test exercise demonstrated the process of identifying, remediating, and validating a web application vulnerability using open-source tools. By leveraging OWASP ZAP for scanning and applying middleware like Helmet.js, the project highlighted practical DevSecOps skills including:

- Secure Docker container usage  
- Static and dynamic analysis of a web application  
- Secure coding best practices using Express and middleware  
- Manual and automated vulnerability validation  
- Secure development lifecycle awareness


