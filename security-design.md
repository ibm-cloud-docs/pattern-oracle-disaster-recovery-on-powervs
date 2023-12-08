---
copyright:
  years: 2023
lastupdated: "2023-11-28"

subcollection: <repo-name>

keywords:
---
\\\# Security design decisions

{: \\\#Security-design}

Security Considerations

It is a best practice to isolate, secure, manage, and monitor all ingress and egress traffic to the environment and to centralize these functions in a hub and spoke model through an Edge VPC. As a result, it’s recommended that security and isolation is established using a combination of Security Groups (SG), Network Access Control Lists (NACL) and routing/FW rules defined in the Edge VPC. This is in addition to the cloud account level protection provided in IBM Cloud Identity and Access Management Services.

Implementation of firewall(s) in the Transit/Edge can secure and route traffic to the IBM Cloud environment as well as from the Customer network.

Enable logging to facilitate the firewall activity analysis if needed to meet client security monitoring and IPS/IDS requirements.

All traffic, both public and private, should route through the Edge/Transit VPC for routing, isolation, and logging.

To restrict, log and monitor administrative access to the environment, a bastion (jump) host is recommended in the Edge VPC for all administrative access. In addition, all Bastion access and activity should be logged and monitored through Privileged Access Management (PAM) services.

Security is a shared responsibility between cloud provider and customer at both the infrastructure and application layers.

Please consider the use of Cloud internet services if there are requirements for IDS,IPS and DDOS services in your environment.

### Encryption

IBM provides two cloud based key management services that integrate with PowerVS, Key Protect Service which is “bring your own key” (BYOK) and Hyper Protect Crypto Service which is “keep your own key” (KYOK).

1. IBM Key Protect is a full-service multi-tenant encryption solution that allows data to be secured and stored in IBM Cloud using the latest envelope encryption techniques. You can integrate Key Protect with Power Systems Virtual Server to securely store and protect encryption key information for AIX
2. IBM Cloud Hyper Protect Crypto Services (HPCS) is a dedicated key management service and hardware security module (HSM) in IBM Cloud. You can integrate HPCS with Power Systems Virtual Server to securely store and protect encryption key information for AIX

Additional details can be found [here](https://cloud.ibm.com/docs/power-iaas?topic=power-iaas-integrate-hpcs).

### Database Encryption

Transparent Data Encryption (TDE) is a well-established technology to encrypt sensitive data in databases and is used by the Oracle database. With TDE, a database system encrypts data on database storage media, such as table spaces and files, and on backup media. The database system automatically and transparently encrypts and decrypts data when it is used by authorized users and applications. Database users do not need to be aware of TDE and database applications do not need to be adapted specifically for TDE. Typically, TDE uses a two-tiered key hierarchy, which is composed of a TDE master encryption key and a TDE data encryption key. The TDE data encryption key is used to encrypt and decrypt user data, while the TDE master encryption key is used to encrypt and decrypt the TDE data encryption key. You can keep complete and exclusive control of your TDE master encryption keys by storing them in IBM Cloud Hyper Protect Crypto Services. For this purpose, you need to use the PKCS \#11 integration feature of Hyper Protect Crypto Services. More details can be found [here](https://cloud.ibm.com/docs/hs-crypto?topic=hs-crypto-tutorial-tde-pkcs11).
