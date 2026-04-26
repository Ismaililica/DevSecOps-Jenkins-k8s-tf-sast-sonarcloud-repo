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
