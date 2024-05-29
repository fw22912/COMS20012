# Lab 1 Spring 2024
## Dr. Alma Oracevic

### Problem 1

#### a. Examples of Confidentiality, Integrity, and Availability Requirements for Automated Cash Deposit Machine

- **Confidentiality**:
  - Requirement: Protect user account information and transaction details from unauthorized access.
  - Importance: High. Compromise could lead to identity theft or financial fraud.
  
- **Integrity**:
  - Requirement: Ensure the deposited amount is accurately credited to the correct account.
  - Importance: High. Errors could lead to financial losses or disputes.
  
- **Availability**:
  - Requirement: Ensure the machine is operational and available to users whenever needed.
  - Importance: Medium. Downtime can cause inconvenience but typically isn't as critical as confidentiality or integrity.

#### b. Requirements for a Payment Gateway System

- The requirements for a payment gateway system would be similar but with higher emphasis on real-time integrity and availability to ensure smooth transactions and immediate confirmation.

#### c. Security Implications of Third-Party Integrations

- Third-party integrations increase the attack surface, requiring robust security measures such as encryption, secure APIs, and regular security audits to ensure no vulnerabilities are introduced through these integrations.

#### d. Extending CIA Requirements to External Entities

- **Confidentiality**: Ensure external entities comply with stringent data protection regulations to safeguard user information.
- **Integrity**: Verify that third-party systems accurately process and report data.
- **Availability**: Ensure third-party services have SLAs guaranteeing uptime and reliability.

#### e. Impact of Different User Authentication Methods

- Stronger authentication methods (e.g., multi-factor authentication) enhance confidentiality and integrity but might slightly reduce availability due to more complex login processes.

### Problem 2

#### a. Confidentiality Most Important

- Example: Publishing system for financial statements of publicly traded companies.
- Reason: Unauthorized disclosure could lead to insider trading and legal consequences.

#### b. Data Integrity Most Important

- Example: Regulatory compliance reports for a financial institution.
- Reason: Accurate data is essential to comply with legal standards and avoid penalties.

#### c. System Availability Most Important

- Example: Real-time stock market data publishing system.
- Reason: Traders rely on up-to-date information for decision-making; downtime can lead to significant financial loss.

#### d. Importance of User Authentication

- Ensures only authorized users can access or modify sensitive financial reports, maintaining both confidentiality and integrity.

#### e. Impact of DDoS Attacks and Mitigation Strategies

- DDoS attacks can cripple system availability. Mitigation strategies include:
  - Using Content Delivery Networks (CDNs) to distribute traffic.
  - Implementing rate limiting and web application firewalls.
  - Employing redundant systems and failover mechanisms.

#### f. Handling Confidentiality Concerns with Third-Party Analytics

- Use encrypted data transmissions and anonymize data where possible.
- Establish strict data handling agreements and regularly audit third-party security practices.

### Problem 3

#### Impact Levels for Different Assets

- **a. Student Blog**
  - Confidentiality: Low
  - Integrity: Moderate
  - Availability: Low

- **b. Examination Section**
  - Confidentiality: High
  - Integrity: High
  - Availability: Moderate

- **c. Pathological Laboratory System**
  - Confidentiality: High
  - Integrity: High
  - Availability: High

- **d. Student Information System**
  - Personal and Academic Data:
    - Confidentiality: High
    - Integrity: High
    - Availability: Moderate
  - Administrative Data:
    - Confidentiality: Moderate
    - Integrity: Moderate
    - Availability: Moderate
  - System as a Whole:
    - Confidentiality: High
    - Integrity: High
    - Availability: Moderate

- **e. Library Management System**
  - Student Data:
    - Confidentiality: Moderate
    - Integrity: Moderate
    - Availability: Moderate
  - Book Data:
    - Confidentiality: Low
    - Integrity: Moderate
    - Availability: High
  - System as a Whole:
    - Confidentiality: Moderate
    - Integrity: Moderate
    - Availability: High

#### f. Prioritizing Security Controls with Budget Constraints

1. Implement strong authentication and access controls to protect confidentiality.
2. Ensure data integrity through regular backups and checksums.
3. Invest in availability solutions like redundant systems and failover mechanisms.

### Problem 4

#### a. Confidentiality, Integrity, and Availability Requirements for Smart Home Automation System

- **Confidentiality**: Protect personal data and video feeds from unauthorized access.
  - Importance: High
- **Integrity**: Ensure commands between devices are not tampered with.
  - Importance: High
- **Availability**: Ensure the system remains operational to maintain home security.
  - Importance: High

#### b. Maintaining Critical Functionalities During Failures

- Use redundant network connections and failover mechanisms to maintain critical functions like security camera monitoring and door locks during device failure or network disruption.

#### c. Addressing Privacy Concerns with Security Cameras

- Implement end-to-end encryption for video feeds and restrict access to authorized users only.
- Regularly update firmware to prevent exploits.

#### d. Mitigating Denial-of-Service Attacks

- Deploy DDoS protection services, such as rate limiting, network firewalls, and redundant servers to ensure the availability of critical home automation services.

#### e. Guaranteeing Command Integrity

- Use cryptographic techniques like digital signatures and secure communication protocols (e.g., TLS) to prevent tampering or manipulation of commands between devices.
