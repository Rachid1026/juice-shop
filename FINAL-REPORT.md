# Final Report: Secure DevSecOps Deployment of OWASP Juice Shop

**Internship Project: Cybersecurity & DevSecOps Training**  
**Student:** Rachid Dwyer  
**Date Completed:** June 2025

---

## Project Summary
This final report documents the secure deployment of the OWASP Juice Shop application using Docker, the integration of DevSecOps principles into its development and deployment lifecycle, and the validation of security controls through vulnerability scanning and penetration testing. The outcome is a hardened web application with improved visibility, reduced attack surface, and automated monitoring.

---

## Identified Security Issues & Fixes

**Issue:** Missing HTTP Security Headers  
**Tool:** OWASP ZAP  
**Fix:** Implemented HTTP header middleware using Helmet.js in `server.ts`:

```ts
import helmet from 'helmet';
app.use(
  helmet({
    contentSecurityPolicy: {
      useDefaults: true,
      directives: {
        defaultSrc: ["'self"],
        scriptSrc: ["'self"", "'unsafe-inline"],
        styleSrc: ["'self"", "'unsafe-inline"],
        objectSrc: ["'none"],
        upgradeInsecureRequests: []
      }
    }
  })
);
```

**Result:** Security headers were successfully applied to all HTTP responses, which was verified during a follow-up ZAP scan.

---

## CI/CD Security Integration & Tools Used

- **Containerization:** Docker was used to isolate the application from the host OS.
- **Vulnerability Scanning:** OWASP ZAP was used in manual and active modes to identify missing headers, insecure configurations, and other flaws.
- **Static Analysis:** Bandit was used to analyze the application code for security vulnerabilities.
- **Security Headers:** Helmet.js middleware added to protect against clickjacking, XSS, and other browser-based attacks.
- **Configuration Hardening:** CSP, upgradeInsecureRequests, and self-restricted sources were explicitly defined.

---

## Penetration Testing Results

**Initial Scan Results:**  
- Critical: Missing HTTP headers (Content-Security-Policy, X-Frame-Options)  
- Medium: Cookie flags not set, improper content types  
- Low: Insecure resources allowed over HTTP

**Remediation:**  
- Updated `server.ts` with Helmet.js as detailed above
- Confirmed headers present using browser dev tools and re-scan

**Post-Fix Scan:** ZAP reported no remaining critical or header-related vulnerabilities.

---

## Monitoring and Logging

A logging mechanism was implemented to improve visibility into user authentication attempts. This was done by importing a custom logger and placing login audit logs within the login controller.

**Logging Snippet:**
```ts
// vuln-code-snippet start loginAdminChallenge loginBenderChallenge loginJimChallenge
export function login() {
  function afterLogin (user: { data: User, bid: number }, req: Request, res: Response, next: NextFunction) {
    verifyPostLoginChallenges(user) // vuln-code-snippet hide-line
    BasketModel.findOrCreate({ where: { UserId: user.data.id } })
      .then(([basket]: [BasketModel, boolean]) => {
        const token = security.authorize(user)
        logger.info(`Successful login for user: ${user.data.email} from IP: ${req.ip}`)
        user.bid = basket.id // keep track of original basket
        security.authenticatedUsers.put(token, user)
```

**Purpose:**
- Tracks successful login activity
- Stores log entries with user and IP address metadata
- Supports auditability for security monitoring

---

## Lessons Learned
- Header-based security misconfigurations can be easy to overlook but dangerous in production.
- OWASP ZAP is a powerful tool for discovering practical, real-world web vulnerabilities.
- DevSecOps is not just about coding securely, but about building security into every stage—from build to deployment.
- Logging user activity provides valuable traceability and accountability.

---

## Outcome
The OWASP Juice Shop was securely deployed in a Dockerized environment, hardened against common web vulnerabilities, integrated with logging and monitoring, and validated through penetration testing. This project demonstrated an end-to-end DevSecOps approach to secure web application delivery.

**GitHub Repository:** https://github.com/Rachid1026/juice-shop
