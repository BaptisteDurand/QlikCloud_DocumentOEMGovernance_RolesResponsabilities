# Context

In a OEM use case, we have to think the security and users management at 2 levels : 
- Multi tenant : OEM partner manages one tenant for each customer.
- Customer tenant : In a tenant end-users can have different profiles and permissions giving access to several applications with different level of capabilities (example : Self-Service)

The purpose of this section is to present a common and simple OEM configuration for user management and security inside a tenant.
The goal is to get a clear starting point to help you to define your own governance needs and how to setup them.

***Use case :*** 

Let's imagine a partner deploying Qlik in their product. They want to deliver dashboards to the end-users. Some of them can also add new Qlik sheets in the dashboard.
This analytics self-service must be governed :
- Core product must not be impacted by user content.
- New dashboard release must not impact user content.
- A global supervisor can be able to manage user content.
- End-customers cannot add their own data in the tenant. They work on a Qlik application with a data model built by the partner.


# 1. Identify your user profiles, roles and responsabilities

First step is to identify the different user profiles you need on your tenant(s).


![image](https://user-images.githubusercontent.com/24877503/236837192-fa8b3df8-ecb2-4967-a7b4-3a2b6a99e56c.png)


We are keeping a simple use case only based on the dashboards consumption. We can add roles or profiles to also manage static reporting regarding the partner's requirements.

# 2. Qlik Cloud : Security components

At this stage it is important to understand the different type of security and user management components in Qlik Cloud.

To manage users and content security Qlik provides differents tools : 
- [Identity provider](https://help.qlik.com/en-US/cloud-services/Subsystems/Hub/Content/Sense_Hub/Admin/mc-create-idp-configuration.htm) : manages identity information for users and provides authentication services. Users groups can be created in Qlik Cloud through the IdP and used for security access management. Qlik Cloud support OIDC for Idp but also JWT.
- [Entitlements](https://help.qlik.com/en-US/cloud-services/Subsystems/Hub/Content/Sense_Hub/Admin/SaaS-user-allocations.htm) : When users join the tenant, you assign them Professional entitlement or Analyzer entitlement. Professional entitlement is intended for users who will create content in Qlik Sense. Analyzer entitlement is intended for users who only consume sheets and apps created by others.
- [Qlik Security Roles](https://help.qlik.com/en-US/cloud-services/Subsystems/Hub/Content/Sense_Hub/Admin/SaaS-roles.htm) : Security roles provide a set of tenant-level permissions to users and administrators, beyond the general permissions granted by the user entitlements. Note : tenant settings allow you to manage global entitlement for the tenants.
- [Spaces](https://help.qlik.com/en-US/cloud-services/Subsystems/Hub/Content/Sense_Hub/Spaces/Spaces.htm) : Spaces are sections of the Qlik Cloud Analytics hub used to both:
  - Collaboratively develop apps
  - Control access to apps.
There are 2 types of space : Shared or Managed with different security level.

# 3. Qlik Cloud : Simple implementation

To implement

Paul : 



# 4. Application Lifecycle example

# Going further



