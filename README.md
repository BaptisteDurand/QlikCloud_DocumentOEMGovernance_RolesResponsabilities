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


# 1. Identify your user profiles, roles and responsabilities

First step is to identify the different user profiles you need on your tenant(s).


![image](https://user-images.githubusercontent.com/24877503/236837192-fa8b3df8-ecb2-4967-a7b4-3a2b6a99e56c.png)


We are keeping a simple use case only based on the dashboards consumption. We can add roles or profiles to also manage static reporting regarding the partner's requirements.

# Application Lifecycle example

# Qlik Cloud : Security role and spaces

# How to Setup



