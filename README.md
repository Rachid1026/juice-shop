# Juice Shop Secure Deployment – DevSecOps Project

**Internship Project: Cybersecurity & DevSecOps Training**  
**Author:** Rachid Dwyer  
**Date Completed:** June 2025

---

## Project Summary

This project focused on securing and testing a vulnerable web application, OWASP Juice Shop, using DevSecOps principles. The project involved containerizing the application with Docker, scanning for vulnerabilities using OWASP ZAP, remediating a critical security issue, and validating the fix with a follow-up scan.

---

## Tools & Technologies Used

- **OWASP Juice Shop** – Vulnerable web application for security training
- **Docker** – Containerized the application for isolated deployment
- **Visual Studio Code** – Code inspection and middleware integration
- **OWASP ZAP** – Web application vulnerability scanner
- **Node.js & Express** – Web server technologies used in the application
- **Helmet.js** – Middleware added to mitigate missing HTTP security headers

---

## Key Accomplishments

- Deployed the OWASP Juice Shop application in a secure Docker container using industry best practices
- Implemented logging functionality to monitor application behavior and aid in future threat detection
- Integrated OWASP ZAP into the workflow to perform both manual exploration and automated active scanning
- Identified multiple security weaknesses, including missing HTTP security headers and misconfigurations
- Applied secure coding techniques by configuring Helmet middleware in the application’s source code
- Rebuilt and re-deployed the containerized app to confirm mitigation was successful through follow-up scanning
- Strengthened hands-on skills in secure DevOps, application hardening, vulnerability remediation, and continuous improvement practices

---

## Repository Structure

- `Dockerfile` – Contains secure build instructions for Juice Shop
- `server.ts` – Modified to include Helmet security middleware
- `Pentest_Report.md` – Full penetration testing report and findings
- `README.md` – Project summary and documentation

---

## Next Steps

- Explore additional security headers and middleware for deeper protection
- Set up CI/CD pipeline with automated vulnerability scanning
- Expand remediation efforts to address additional issues flagged by ZAP

---

**GitHub Repository:** https://github.com/Rachid1026


