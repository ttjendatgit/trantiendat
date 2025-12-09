---
title: "Blog 1"
date: "2025-09-09"
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---



# Enhancing Telecommunications Security with AWS

**Authors:** Kal Krishnan, Danny Cortegaca and Ruben Merz on 07 FEB 2025 in Best Practices, Intermediate (200), Security, Identity, & Compliance, Thought Leadership Permalink

# Implementing CISA's Guidance for Enhanced Visibility and Hardening of Telecommunications Infrastructure

In response to recent cybersecurity incidents by actors from the People's Republic of China, multiple cybersecurity agencies led by the U.S. Cybersecurity and Infrastructure Security Agency (CISA) have jointly issued comprehensive guidance for securing telecommunications infrastructure. As communications service providers (CSPs) move their workloads to the cloud, they must take steps to effectively implement these security measures in the cloud environment.

This blog post describes how CSPs can use Amazon Web Services (AWS) features to implement this guidance while benefiting from the cloud's advantages.

The guidance focuses on two main areas:

- **Enhanced Visibility:** Enabling security teams to monitor, detect, and respond to potential threats through comprehensive insight into digital assets.
  
- **Enhanced Hardening of Systems and Devices:** Implementing robust security controls and configurations to mitigate vulnerabilities and help prevent unauthorized access.

# Overview of Cloud Fundamentals

Before delving into the specific guidance in this post, it's important to understand how security recommendations apply differently to public cloud environments versus private infrastructure. A common trend in the telecommunications industry is to view the public cloud as merely an extension of private clouds. This can lead to misunderstandings about security capabilities and inefficient use of native public cloud security features.

The fundamental difference lies in how public cloud architecture—especially designed for multi-tenancy—is built on a foundation of strong tenant isolation as a core design principle.

In AWS, virtual resources are isolated by default and require explicit configuration to connect. For example, when you create a virtual private cloud (VPC) using Amazon VPC, this logically isolated network does not permit traffic in or out until specific routes and gateways are intentionally configured. Similarly, Amazon Simple Storage Service (Amazon S3) buckets are private by default, requiring explicit configuration to grant access. This isolation extends to our core virtualization infrastructure through the AWS Nitro System, which provides unprecedented workload isolation—even AWS's most privileged operators have no technical access to customer workloads. Furthermore, data moving between virtual machines running on the Nitro System or across our global backbone is automatically encrypted, providing additional layers of protection beyond customer-deployed encryption.

This security-by-design and security-by-default philosophy permeates AWS service design and operations. It's not merely a design choice—it's a business imperative driven by the operational resilience imperative and customer trust in the public cloud model. Our commitment to these principles is demonstrated by our signing of CISA's Secure by Design pledge.

When AWS customers operate on the public cloud platform, understanding the shared responsibility model is crucial. This model clearly delineates security responsibilities: AWS is responsible for **security *of* the cloud**, while the customer is responsible for **security *in* the cloud**. This division of responsibility significantly reduces your operational burden because AWS assumes responsibility for securing everything pertaining to and within the cloud services they provide, from physical protection of data centers onward. This allows you to focus your security resources on what matters most—protecting your applications and workloads—while AWS handles the undifferentiated heavy lifting of infrastructure security.

This shared responsibility model becomes even more beneficial when considering the inherent economies of scale in public cloud operations. AWS's massive scale enables us to invest more in securing the platform than any single enterprise could achieve independently, creating a security multiplier effect that benefits all customers. A compelling example of this scale advantage is our comprehensive threat intelligence program, which deploys honeypot sensors across our entire global network. These sensors observe over 100 million potential threat interactions and probes daily. Using artificial intelligence and machine learning (AI/ML), we analyze this information and take rapid, often automated actions to mitigate threats. In the first half of 2023 alone, this program allowed us to analyze the origin of roughly 230,000 Layer 7 distributed denial-of-service (DDoS) events. We also provide this intelligence to customers through services like Amazon GuardDuty, extending the benefits of our scale to customers.

AWS's operational scale not only enables exemplary threat intelligence gathering but also necessitates comprehensive automation of security operations. Routine tasks, such as feature and patch deployment and configuration updates, are completely automated through deployment processes. Automation provides the added benefit of removing humans from the loop, thereby minimizing the risk of error.

Our scale also facilitates comprehensive compliance with security standards across multiple industries and jurisdictions. Our global presence and diverse customer base require adherence to the strictest security requirements worldwide. Through the AWS Compliance Program, we have achieved 143 security standards and compliance certifications, from ISO standards for security and privacy on a cloud platform to industry-specific regulations in finance and healthcare, and government security programs. This includes independent verification of our claims about the isolation properties of the Nitro System virtualization infrastructure. As a result, you benefit from compliance at scale, having access to secure, certified infrastructure deployed with advanced security systems.

This is why, in the blog post titled *Why Cloud-First Is Not a Security Concern*, the UK's National Cyber Security Centre concluded that **"using the cloud securely should be your main concern—not the underlying security of the public cloud."**

On the other hand, private clouds are typically under the control of a single organization and are single-tenant, providing relatively weak workload isolation. Virtual resources in private clouds are often connected by default upon creation, requiring specific steps to enhance isolation. Manual operations, with their potential for error and possible threat actor involvement, often still dominate private cloud workflows. They rarely undergo the level of security scrutiny that public clouds regularly face. These, along with other differences, mean that the security risk profile of each service is inherently different, necessitating separate security controls and solution architectures to mitigate these risks.

# Implementing the Security Hardening Guidance with AWS

Your cloud resources and data reside within an AWS account. The account serves as an isolation boundary for identity and access management. When you need to share resources and data between two accounts, you must explicitly grant this access. This helps mitigate the risk of lateral movement between accounts.

Properly designing your AWS environment establishes a strong foundation for meeting CISA's cybersecurity guidance. AWS Control Tower, working with AWS Organizations, allows you to set up a well-architected multi-account environment based on security best practices.

For detailed guidance on creating a secure landing zone for telecommunications workloads, refer to our comprehensive blog post on the topic.

We have analyzed the recommendations in CISA's guidance and grouped them into six categories under the two main focus areas. Please refer to the linked GitHub page at the end of this article for more detailed guidance on relevant AWS services that can help you implement each individual security measure in the guidance.

# Logging and Monitoring

Guidance in this category emphasizes the importance of enhanced visibility to understand network activity, which is essential for anomaly detection and incident response. Key security controls include: having robust asset management capability, enabling logging at multiple levels, centralizing logging, protecting logging and monitoring infrastructure, and using security tools to detect anomalies and incidents.

Enhanced visibility is an inherent advantage of the public cloud model, particularly in AWS. This transparency is not merely an added feature but a fundamental need driven by the API-centric, pay-for-use business model. To bill customers accurately, AWS has integrated comprehensive tracking and logging functions into its core architecture. Consequently, AWS provides robust tools that enable you to collect, centralize, and monitor logs across every layer of your network workload. This level of visibility far exceeds what is typically achieved in traditional infrastructure, giving you unprecedented insight into IT assets and user activity.

Another important guidance is to centralize security-related logging and separate logging infrastructure from other production environments. You can implement this guidance in AWS using Amazon Security Lake alongside OpenSearch deployed in separate accounts, with access restricted to the security organization only. Alternatively, this solution provides a best-practice deployment method for creating collection and ingestion pipelines to enable centralization and inspection of log sources across your entire AWS workload without using Security Lake.

# Configuration and Change Management

Guidance in this category emphasizes centralization, securing, and protecting configuration. The guidance highlights the importance of detecting and alerting on unauthorized modifications, auditing configuration for compliance, and change management processes that automate frequent changes to minimize unintended drift.

In AWS, you can deploy infrastructure and configuration as code, enabling centralized configuration storage in repositories, tracking changes through version control, and deploying changes through approved change management processes. You can use code repositories and continuous integration and continuous delivery (CI/CD) processes to automate the deployment of these configurations, helping you accelerate deployment, maintain consistency, simplify management, and implement stringent, auditable change control processes.

Regardless of how infrastructure is deployed and managed, you can use AWS Config to automatically track the current state and history of a vast set of configuration information across over 100 AWS services and hundreds of their resource types. You can also write custom AWS Config rules to take automated actions whenever sensitive resources are modified, or leverage over 400 AWS-managed rules in AWS Config to send alerts or auto-remediate when critical resources change state.

# Identity and Access Management

Guidance in this category emphasizes the importance of managing operational accounts and permissions, using phish-resistant authentication methods, implementing least privilege through role-based access control, managing emergency access, and session limiting.

Authentication and authorization, which are critical components of access control, are managed through AWS Identity and Access Management (IAM), AWS IAM Identity Center, and AWS Organizations. AWS provides you with the ability to manage permissions at scale across identities, resources, and services, including the ability to require multi-factor authentication (MFA) for sign-ins. Furthermore, these capabilities support customer compliance with the principle of least privilege by encouraging the management of time-limited, session-based credentials using AWS Security Token Service (AWS STS).

Software running in the cloud needing to call cloud APIs automatically receives temporary, frequently rotated credentials through IAM roles for Amazon Elastic Compute Cloud (Amazon EC2), Amazon Elastic Container Service (Amazon ECS), Amazon Elastic Kubernetes Service (Amazon EKS), and AWS Lambda, eliminating the need for long-term credentials that could be leaked or compromised. Access to cloud APIs from on-premises software can be securely bootstrapped from enterprise identity management technologies using AWS IAM Roles Anywhere. You can even protect authentication for non-cloud technologies by combining roles and using AWS Secrets Manager to protect and automatically rotate secrets like database passwords.

# Network and Traffic Management

Guidance in this category emphasizes segmenting workloads and networks to limit lateral movement and internet exposure, monitoring and regulating traffic flow using policies, and securing unused ports.

You can achieve micro-segmentation of the network, an important aspect of modern security architecture, through VPCs and subnets. For example, you can separate internet-facing components of an application from components that don't require this access by placing them in separate VPCs and only permitting internet access on the VPC that requires it. You can control traffic within and between segments using multiple networking services—for example, route tables, internet gateways, transit gateways, and firewall services. This segmentation minimizes risk from unauthorized activities originating from the internet and limits lateral movement in case of a compromise.

To implement guidance for out-of-band management, you can design your network connections to separate management traffic from network signaling traffic using subnets—for example, a single EC2 instance can have multiple elastic network interfaces (ENIs) attached to different subnets or even different VPCs: one interface permitting only management traffic and another permitting only signaling traffic.

# Strong Cryptography for Data-at-Rest and Data-in-Transit

Guidance in this category emphasizes the use of strong cipher suites, secure versions of cryptographic protocols, and PKI-based certificates to protect data at rest and in transit.

Encryption, the cornerstone of data protection, is comprehensively addressed in AWS services. AWS service API endpoints support TLS 1.3 (and a minimum of TLS 1.2) with secure, standards-based cipher suites, cryptographic keys, and advanced security features like HKDF (HMAC-based Extract-and-Expand Key Derivation Function) for enhanced security. AWS services managing customer secrets transmitted over the wire also support post-quantum cryptography. For example, AWS Key Management Service (AWS KMS), AWS Certificate Manager, and AWS Secrets Manager support optional post-quantum key exchange hybrids for the TLS network encryption protocol. When using the Border Gateway Protocol (BGP), AWS uses Resource Public Key Infrastructure (RPKI) and Route Origin Authorizations (ROAs) to protect Amazon's IP address space and routes from misconfiguration and hijacking.

You can also use AWS encryption services like AWS KMS, AWS CloudHSM, and AWS Certificate Manager to secure your data in transit and at rest. Keys you create in AWS KMS are protected by Hardware Security Modules (HSMs) validated to FIPS 140-2 Level 3, and there is no mechanism that allows anyone, including AWS service operators, to view, access, or export the plaintext key material.

AWS Secrets Manager helps you securely manage, retrieve, and rotate database credentials, application credentials, OAuth tokens, API keys, and other secrets throughout their lifecycle. For more details on AWS encryption solutions and best practices, please refer to AWS Encryption Best Practices.

# Vulnerability Management

This guidance emphasizes mitigating exploitation risk through proper lifecycle management, regular patching, and removal of insecure protocols. AWS helps address these requirements through both shared responsibility and innovative architectural approaches.

Under the shared responsibility model, AWS manages the security of the underlying infrastructure. This includes maintaining updated systems and services, disabling insecure protocols and unused ports, and providing a Security Bulletin for timely vulnerability notifications. AWS services are supported under contractually defined terms, so you don't need to worry about expiring infrastructure components.

For your applications, AWS enables a transformative approach to vulnerability management through ephemeral resources and immutable infrastructure. Instead of maintaining long-lived instances requiring continuous patching, you can maintain a single, hardened, and regularly updated Amazon Machine Image (AMI) as your golden image. When updates are needed, instead of patching running instances, you simply deploy new instances with your application code installed from the updated AMI. Similar approaches apply to container-based workloads. AWS Lambda-based workloads further minimize your patching responsibility, as you only need to update the code containing your business logic (and any support layers you've selected)—AWS automatically patches the underlying hypervisor, operating system, and container. This approach allows you to keep your system in a known, safe state while reducing both the threat surface and operational cost. You can further enhance security using AWS networking features like security groups to disable insecure protocols, such as enforcing HTTPS instead of HTTP.

# Conclusion

The comprehensive guidance from cybersecurity agencies provides an essential framework for securing telecommunications infrastructure. As demonstrated in this article, AWS offers a robust set of services and capabilities that align with the recommendations from CISA and allied governments. From enhanced visibility through logging and monitoring, to strong identity management, network segmentation, encryption, and vulnerability management, AWS provides the tools you need to implement these security controls effectively while maintaining operational performance. The shared responsibility model, combined with AWS's continuous security innovation, enables telecommunications companies to build and maintain secure, resilient cloud environments.