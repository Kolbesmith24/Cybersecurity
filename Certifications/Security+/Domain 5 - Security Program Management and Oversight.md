# Security Policy Framework

## Overview
The Security Policy Framework provides an overview of an organization’s security policies, standards, guidelines, and procedures. It ensures that security practices are aligned with business objectives and legal requirements, serving as the foundation for all security-related actions.

## Security Policies
- Establish the foundation for an organization's security program.
- Define security expectations and objectives.
- Compliance is mandatory, and failure to comply can result in legal and organizational consequences.

## Security Guidelines
- Offer non-binding advice to help organizations improve their security posture.
- Compliance is optional, but following guidelines can help reduce risk and enhance security measures.

## Data Security Policy Criteria
- Outlines authority and expectations regarding data security.
- Defines how data is accessed, shared, and protected.
- Provides procedures for requesting data access in line with security requirements.

## Data Storage Policies
- Establish rules for data storage locations (e.g., on-premises or cloud).
- Define access controls and encryption requirements to protect sensitive data.

## Data Transmission Policies
- Apply to data in transit (data in motion).
- Include guidelines for secure transmission (e.g., using SSL/TLS) and encryption standards to ensure integrity and confidentiality.

## Data Life Cycle Policies
- Address all stages of data, from creation to deletion.
- Include data retention rules and disposal procedures for safely deleting or destroying data no longer needed.

## Acceptable Use Policies (AUPs)
- Define acceptable and prohibited uses of organizational resources (e.g., networks, computers, and data).
- Ensure resources are used for legitimate and authorized purposes only.

## Software Development Life Cycle (SDLC)
- A structured process for software development, including planning, designing, coding, testing, and maintaining software.
- Formalizes security requirements at each phase to minimize vulnerabilities and enhance security.

## Change Management and Change Control Policies
- Outline procedures for requesting, reviewing, and approving changes to IT systems and infrastructure.
- Ensure changes are well-managed to avoid unnecessary risks and vulnerabilities.

## Security Standards
- Document technical and operational requirements for securing assets, systems, and data (e.g., password policies, encryption, and physical security).
- Compliance with these standards is mandatory for consistent security.

## Security Procedures
- Provide step-by-step instructions for executing security tasks (e.g., patching, user access management).
- Compliance is often mandatory, but some procedures may be situational.

## Policy Reviews and Updates
- Regular reviews of policies and procedures are necessary to ensure alignment with laws, regulations, and business needs.
- Identifying gaps and inconsistencies helps organizations adapt policies to remain compliant.

## Static or Dynamic Policies
- Security policies are not static; they need regular evaluation to adapt to emerging threats, regulatory requirements, and technological changes.

## Policy Considerations
- Take into account legal, regulatory, and compliance requirements specific to the organization, as well as industry best practices.

## Governance
- Refers to the framework of procedures, roles, and controls that guide cybersecurity efforts to align with business goals.
- Ensures accountability and adherence to security objectives.

## Delegation of Security Responsibility
- Typically delegated by the CEO to the Chief Information Security Officer (CISO), who oversees cybersecurity strategy and risk management.

## Governance Approaches
- **Centralized Governance**: A top-down approach where security policies are created and enforced by a central authority, ensuring consistency across the organization.
- **Decentralized Governance**: A bottom-up approach where individual business units or departments have the responsibility to achieve their own cybersecurity objectives, offering flexibility at the local level.

## Data Protection Roles
- **Data Controllers**: Individuals or entities responsible for determining the purposes and means of processing personal data.
- **Data Processors**: Third-party service providers who process personal data on behalf of data controllers, bound by contracts to ensure security and compliance.
- **Data Protection Officer (DPO)**: Oversees data protection strategy, ensuring compliance with privacy regulations (e.g., GDPR).
- **Data Subjects**: Individuals whose personal data is being processed, holding rights such as access, correction, and deletion under laws like GDPR.

## Risk Management

### Risk Assessment
- Identifies, evaluates, and prioritizes risks to the organization’s assets, systems, and data.
- Helps in understanding vulnerabilities and threats, enabling the implementation of risk management strategies.

### Threat
- A potential cause of harm to an organization's assets, systems, or data.
- Can be internal (e.g., employee negligence) or external (e.g., cyber-attacks).

### Threat Vector
- The method or pathway an attacker uses to exploit vulnerabilities in a system or network (e.g., phishing, malware, social engineering).

### Risk
- The likelihood of a threat exploiting a vulnerability and causing harm to the organization.
- Risk is a combination of the probability of an event occurring and the potential impact.

### Prioritizing Risks
- Risks are prioritized based on the likelihood of occurrence and the potential impact.
- Higher likelihood or greater impact risks should be addressed first to mitigate damage.

# Risk Assessment and Management Notes

## Types of Risk Assessments

- **One-Time Risk Assessment**: A risk assessment conducted for a specific project, event, or change. This is typically a one-off process, not part of a regular schedule.
- **Ad-Hoc Risk Assessment**: A risk assessment performed as needed, often in response to unexpected changes or emerging risks, without a predefined schedule.
- **Recurring Risk Assessment**: A regular risk assessment conducted on a fixed schedule, such as monthly, quarterly, or annually, to monitor and evaluate risks over time.
- **Continuous Risk Assessment**: A dynamic approach where risks are continuously monitored using automated tools, alerts, and real-time analytics. This allows rapid identification and response to emerging threats.

## Risk Assessment Methods

- **Qualitative Risk Assessment**: Assesses risks using subjective criteria (e.g., high, medium, low) based on expert judgment, without numeric values.
- **Quantitative Risk Assessment**: An objective approach that uses numeric values for likelihood, impact, and other variables, calculating metrics like Single-Loss Expectancy (SLE) and Annualized Loss Expectancy (ALE).

## Risk Calculation Metrics

- **Asset Value (AV)**: The total estimated value of an asset, considering original cost, depreciation, and replacement cost. AV helps determine the potential financial impact of losing or damaging the asset.
- **Exposure Factor (EF)**: The percentage of damage or loss an asset would incur if a specific risk occurs.
- **Single-Loss Expectancy (SLE)**: The expected financial loss to an asset if a specific risk occurs once. It is calculated as:
  - **SLE = Asset Value (AV) * Exposure Factor (EF)**

- **Annualized Rate of Occurrence (ARO)**: The estimated number of times a specific risk is likely to occur annually.
- **Annualized Loss Expectancy (ALE)**: The total expected loss from a risk over a year, calculated as:
  - **ALE = SLE * ARO**

- **Mean Time to Failure (MTTF)**: The average time a non-repairable component is expected to function before it fails.
- **Mean Time Between Failures (MTBF)**: The average time between failures of a repairable component.
- **Mean Time to Repair (MTTR)**: The average time required to repair a faulty component and restore it to service.

### Downtime Calculation
- **Downtime** = **MTBF** + **MTTR**

## Risk Management/Treatment Strategies

- **Risk Avoidance**: Altering business processes or practices to eliminate a risk entirely.
- **Risk Transference**: Shifting the financial or operational impact of a risk to another party (e.g., through insurance or outsourcing).
- **Risk Mitigation**: Taking actions to reduce the likelihood of a risk occurring or minimizing the impact if it does (e.g., security patches, access controls).
- **Risk Acceptance**: Acknowledging a risk and taking no further action if the potential impact is deemed low or the cost of mitigation outweighs the potential damage.

## Risk Appetite and Tolerance

- **Risk Appetite**: The amount of risk an organization is willing to accept in pursuit of its objectives.
  - **Expansionary Risk Appetite**: A higher tolerance for risk, typically seen in growth-focused organizations.
  - **Neutral Risk Appetite**: A balanced approach, where risks are only taken when the benefits outweigh the costs.
  - **Conservative Risk Appetite**: A low tolerance for risk, focusing on protecting assets and stability.
  
- **Risk Threshold**: The point at which the impact of a risk is considered unacceptable, necessitating mitigation.
- **Risk Tolerance**: The level of risk an organization can endure before corrective actions are required.

## Risk Visibility and Reporting

- **Risk Visibility and Reporting Techniques**: Includes the use of risk registers and automated reporting tools to track and manage risks over time.
- **Risk Register**: A tool used to manage risk information, including risk descriptions, categories, ownership, probabilities, and mitigation actions.
  - **Contents of a Risk Register**: Risk descriptions, categories, risk owners, probabilities, impact assessments, ratings, and mitigation strategies.
  
- **Risk Matrix/Heat Map**: A visual tool used to assess and prioritize risks based on likelihood and impact, with color-coding to represent different levels of risk.
- **Risk Control Assessments**: Evaluate the effectiveness of security controls in mitigating risks.

## Security Metrics and Indicators

- **Security Metrics**: Quantitative measures used to assess the effectiveness of security controls, track performance, and detect weaknesses.
- **Key Performance Indicators (KPIs)**: Metrics that demonstrate the success or failure of a security program, such as response times, incident resolution times, or number of vulnerabilities mitigated.
- **Key Risk Indicators (KRIs)**: Metrics that measure the amount of risk an organization is exposed to, helping assess whether the risks are within acceptable limits.

## Vendor Management and Security

- **Vendor Security Policies**: Organizations must ensure that vendor security policies meet or exceed their own policies to reduce third-party risk and maintain a consistent security posture.
- **Vendor Management Life Cycle**: The process for managing vendor relationships, consisting of:
  - **Vendor Selection** → **Onboarding** → **Monitoring** → **Offboarding** → **Back to Vendor Selection**

# Vendor Management

## Vendor Selection
- The process of choosing a vendor based on the quality, reputation, and effectiveness of their risk management program.
- Ensures the vendor can meet security and compliance requirements.
- A thorough vendor selection process ensures the organization partners with a reliable and secure vendor.

## Vendor Onboarding
- Verifying contract details and establishing technical operations for data transfer, handling, and integration.
- Ensures the vendor is prepared to follow the organization's security policies, procedures, and compliance requirements.

## Vendor Monitoring
- Continuous assessment of the vendor’s performance, security practices, and compliance.
- Activities include site visits, reviewing audit results, and evaluating reports from independent assessments.
- Monitoring helps identify and mitigate potential risks introduced by the vendor.

## Vendor Offboarding
- Ensuring the secure termination of the relationship, including destroying confidential information, revoking access, and confirming contract adherence.
- Minimizes risks associated with data retention and system access after the relationship ends.

# Agreements and Legal Documents

## Nondisclosure Agreements (NDAs)
- Legal agreements to ensure confidential information is kept secure and not shared with unauthorized third parties.
- Protects intellectual property and proprietary information.

## Service-Level Requirements (SLRs)
- Specific expectations regarding the performance of vendor services.
- Defines acceptable thresholds for performance, often forming part of a Service-Level Agreement (SLA).

## Service-Level Agreement (SLA)
- A written contract outlining the scope, quality, and performance standards of services provided by the vendor.
- Includes penalties or corrective actions if the vendor fails to meet expectations.

## Memorandum of Understanding (MOU)
- A non-binding agreement outlining general terms, goals, and responsibilities between two or more parties.
- Formalizes the intention to cooperate without creating enforceable legal obligations.

## Memorandum of Agreement (MOA)
- A legally binding document defining specific responsibilities, obligations, and expectations between parties.
- More detailed than an MOU and includes terms enforceable in court.

## Business Partners Agreement (BPA)
- A legally binding contract between business partners that outlines roles, responsibilities, financial arrangements, and other terms.
- Ensures both parties understand their commitments and obligations in the partnership.

# Data Protection and Privacy

## Data Ownership Provision
- The vendor’s right to use customer data is limited to activities performed on behalf of the customer, such as data processing or storage.
- Customer consent is required for the vendor to use data for their own purposes.

## Vendor Use of Customer Information
- Vendors can use customer information only for activities performed with the customer's knowledge and consent, typically for purposes outlined in the contract.

## Vendor Responsibilities with Customer Data
- Vendors must delete or securely destroy all customer data once it is no longer needed for the agreed-upon purpose.

## Sharing Customer Data with Third Parties
- Vendors cannot share customer data with third parties without explicit customer consent, except when required by law or regulatory obligations.

## Contracts to Protect Customer Data
- Contracts with vendors should include data protection provisions outlining security measures such as encryption, access controls, and incident response procedures.

# Compliance and Auditing

## Compliance Monitoring and Reporting
- Ongoing efforts to track an organization’s adherence to regulatory and internal security standards.
- Includes assessments, audits, and continuous review processes.
- Reporting ensures stakeholders are kept informed of compliance status.

## Methods of Reporting for Compliance
- **Internal Reporting**: Regular updates provided to management or the board detailing compliance status, gap analysis, and risks.
- **External Reporting**: Reports required by regulatory authorities or compliance bodies to demonstrate adherence to laws, standards, and regulations.

## Due Diligence
- The process of understanding legal, regulatory, and operational requirements before entering into business relationships.
- Includes assessing risks, compliance, and security posture.

## Components of Due Care
- **External Monitoring**: Third-party audits and assessments to verify compliance with standards and security measures.
- **Internal Monitoring**: Ongoing internal audits and reviews to assess compliance with security policies, standards, and regulations.

## Audits and Assessments in Security
- **Goal**: To verify that security controls are functioning properly and meet compliance requirements.
- Audits and assessments identify weaknesses or gaps in security posture and recommend improvements.

## Requesting Audits and Assessments
- **Assessments**: Typically requested by internal IT staff to evaluate security measures and identify vulnerabilities.
- **Audits**: Typically initiated by external parties (e.g., regulators, auditors, or board members) to assess adherence to security standards and regulations.

## Audits
- Detailed tests and evaluations to assess whether security controls and processes comply with specific standards or regulations.
- Audits are usually performed by external auditors or independent firms.

## Types of Auditors
- **Internal Auditors**: Employees who conduct audits to evaluate internal controls and compliance, typically reporting to leadership but operating independently.
- **External Auditors**: Independent auditors hired to assess an organization's compliance with industry standards and regulations.

## Gap Analysis
- A review process identifying discrepancies between the organization's current security posture and the required compliance level.
- Helps create a roadmap for addressing weaknesses.

## User Access Reviews
- A process to validate user permissions and access rights, ensuring they align with job roles and the principle of least privilege.
- Helps prevent unauthorized access and ensures compliance with security policies.

## Auditor Evaluation of Controls
- Auditors evaluate the effectiveness of security controls and processes and provide an attestation confirming that the controls meet the required standards and function as intended.

