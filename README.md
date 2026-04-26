<img width="1904" height="872" alt="image" src="https://github.com/user-attachments/assets/25ceef80-41e6-4021-aae8-a77c4a1d4c2e" />
Static Application Security Testing (SAST) with SonarCloud
Project uses SonarCloud (SonarQube Cloud) to perform deep code analysis. The main goal is to identify security vulnerabilities, bugs, and code smells before the deployment stage.

📊 Scan Results Analysis
As shown in our latest analysis:

Security (Rating E): Identified 50 Open Issues. This confirms the application contains significant security vulnerabilities (like SQL Injection, XSS, or hardcoded secrets) which we use for DevSecOps demonstration.

Reliability (Rating E): Found 45 Reliability issues, indicating potential runtime crashes.

Security Hotspots: 38 hotspots were flagged for manual review, highlighting sensitive code parts that could be exploited.

🛠️ Why SonarCloud in this Pipeline?
Quality Gate: I've integrated SonarCloud into the Jenkins pipeline to act as a Quality Gate. If the code doesn't meet the security standards, the pipeline automatically fails, preventing insecure code from reaching production.

Shift-Left Security: By catching these 50 security issues during the build phase, we implement the "Shift-Left" approach, reducing the cost and risk of fixing vulnerabilities later.

<img width="1932" height="899" alt="image" src="https://github.com/user-attachments/assets/4b2bf023-98e7-4e9c-a77a-26a89b2b7a03" />
Container Security with Snyk
I used Snyk Container Scanning to analyze the Dockerfile. The scan revealed significant vulnerabilities in our base image.

Current Image (openjdk:8-slim): Found 166 vulnerabilities (8 Critical, 30 High).

Snyk Recommendation: Upgrading to openjdk:26-slim reduces the risk to 67 vulnerabilities (only 1 Critical).

Impact: By using Snyk, we can identify insecure base images early in the CI/CD pipeline and shift towards more secure, up-to-date versions automatically.

<img width="1918" height="868" alt="image" src="https://github.com/user-attachments/assets/c75642e3-49b0-4d6b-9378-8e460f60601c" />
fter successful deployment to AWS EKS, an automated DAST scan was performed using OWASP ZAP. The scan results provide a comprehensive security overview of the live application.

📈 Key Findings:
Alert Summary: Found 3 Medium and 9 Low risk vulnerabilities. This confirms that the application is functional but has security gaps that need to be addressed (perfect for a DevSecOps demo).

Network Health: 37% of responses returned 200 OK, while 44% resulted in 5xx Internal Server Errors, indicating that our DAST tool successfully stressed the application's error-handling mechanisms.

Scan Depth: Total of 50 endpoints were discovered and tested, covering 88% of HTML content and 98% of GET methods.




