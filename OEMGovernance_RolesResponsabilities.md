# Deep dive into governed self service

In a OEM use case, we have to think about security and users management at 2 levels: 
- Multi tenant : OEM partner manages one tenant for each customer.
- Customer tenant : In a tenant end-users can have different profiles and permissions giving access to several applications with different level of capabilities (example : Self-Service)

The purpose of this section is to present a common and simple OEM configuration for user management and security inside a tenant.
The goal is to get a clear starting point to help you to define your own governance needs and how do the setup.

***Use case :*** 

Let's imagine a partner deploying Qlik in their product. They want to deliver dashboards to the end-users. Some of them can also add new Qlik sheets in the dashboard.
This self-service analytics must be governed:
- Core product must not be impacted by user content.
- New dashboard releases must not impact user content.
- A global supervisor is be able to manage user content.
- End-customers cannot add their own data in the tenant. They work on a Qlik application with a data model built by the partner.

# 1. Identify your user profiles, roles and responsabilities

First step is to identify the different user profiles you need on your tenant(s).

![image](https://user-images.githubusercontent.com/24877503/236837192-fa8b3df8-ecb2-4967-a7b4-3a2b6a99e56c.png)

Our primary use case here considers the "dashboards consumption" (what can you do or see inside a dashbaord?). We can add roles or profiles to also manage static reporting, Qlik application automation, AutoML (statistical use cases) based on the partner's requirements. The beauty of Qlik Cloud is that you have all functionality available in the tenant. 

# 2. Qlik Cloud : Security components

At this stage it is important to understand the different type of security and user management components in Qlik Cloud.

To manage users and content security Qlik provides differents tools : 
- [Identity provider (IdP)](https://help.qlik.com/en-US/cloud-services/Subsystems/Hub/Content/Sense_Hub/Admin/mc-create-idp-configuration.htm) : manages identity information for users and provides authentication services. Users and groups can be created in Qlik Cloud through the IdP and used for security access management. Qlik Cloud supports both OIDC and JWT as IdP.
- [Entitlements](https://help.qlik.com/en-US/cloud-services/Subsystems/Hub/Content/Sense_Hub/Admin/SaaS-user-allocations.htm) : When users join the tenant, you assign them a Professional entitlement or Analyzer entitlement. Professional entitlement is intended for users who will create content in Qlik Sense. Analyzer entitlement is intended for users who only consume sheets and apps created by others.
- [Qlik Security Roles](https://help.qlik.com/en-US/cloud-services/Subsystems/Hub/Content/Sense_Hub/Admin/SaaS-roles.htm) : Security roles provide a set of tenant-level permissions to users and administrators, beyond the general permissions granted by the user entitlements. Note : tenant settings allow you to manage global entitlement for the tenant.
- [Spaces](https://help.qlik.com/en-US/cloud-services/Subsystems/Hub/Content/Sense_Hub/Spaces/Spaces.htm) : Spaces are sections of the Qlik Cloud Analytics hub used to both:
  - Collaboratively develop apps
  - Control access to apps.

# 3. Spaces and application development lifecycle

For analytics, there are 3 kind of [spaces](https://help.qlik.com/en-US/cloud-services/Subsystems/Hub/Content/Sense_Hub/Spaces/Spaces.htm) : 
- **Personal** : Your own private work area. If you have the entitlement you can create content as application or data connections.
- [**Shared**](https://help.qlik.com/en-US/cloud-services/Subsystems/Hub/Content/Sense_Hub/Spaces/working-in-shared-spaces.htm) : Used to develop apps collaboratively and share them with other users. Shared spaces are for development, so there is no governed self-service. User content such as a sheet can be only *Private* or *Public*.
- [**Managed**](https://help.qlik.com/en-US/cloud-services/Subsystems/Hub/Content/Sense_Hub/Spaces/working-in-managed-spaces.htm) : Used for providing governed access to apps with strict access control both for the app and the app data. Managed spaces enforce strict governed self-service. User content as sheet can be *Private* or *Community*. Core product sheets (i.e. *Public*) cannot be edit by end-user. 

*Note : There is also Data Space for Data Integration project, but this one is out of scope for the current use case.*

Permissions in space can be set for each user our a group of users based on their Idp groups.
Permissions management is different between [*Shared*](https://help.qlik.com/en-US/cloud-services/Subsystems/Hub/Content/Sense_Hub/Spaces/managing-shared-spaces.htm) and [*Managed*](https://help.qlik.com/en-US/cloud-services/Subsystems/Hub/Content/Sense_Hub/Spaces/working-in-managed-spaces.htm) space.

Here is a simple example of a application lifecycle workflow where content is developed in a shared space and then published in managed space.

![image](https://user-images.githubusercontent.com/24877503/236929877-d0fc5505-72c1-44fa-8a14-5fddb1cf71a7.png)

We recommend this series for a full understanding of application lifecycle in Qlik Cloud : 
- [Part 1](https://community.qlik.com/t5/Design/Systems-Development-Lifecycle-SDLC-with-the-Qlik-Active/ba-p/1898366)
- [Part 2](https://community.qlik.com/t5/Design/Systems-Development-Lifecycle-SDLC-with-the-Qlik-Active/ba-p/1931106)

Note :
- App in managed space cannot be exported. Your master branch or main application must be kept in a shared space in parallel of publishing it in a managed space.
- That is a starting point to built your own workflow. Thanks to the Qlik APIs, Qlik Application Automation and Qlik Data connections you can implement advanced development process with versioning. check this [video on Github integration in Qlik Application Automation](https://www.youtube.com/watch?v=brLxm8Liz5Y)

### Qlik App development and Self-Service : What's the difference?

**Sheets development in a shared space**

- Collaborative work on design is possible by using native features as ”My Sheets”, “Make Public Sheets”.
- All users granted with *Can Edit* can create and publish sheets, ownership is not required.
- It allows to create *base* sheets in an application

![image](https://user-images.githubusercontent.com/24877503/236949725-75b84563-3eac-487e-a421-223316760b9f.png)

**Self-Service in a managed space**

- Public sheets are managed by your product team.
- Quality is ensured by the development process - It is your core product which you support.
- Self-Service sheets are managed by contributors (Customers in our example).
- When you release a new version of the application only Public Sheets are overwritten. User content is kept.

![image](https://user-images.githubusercontent.com/24877503/236950731-a984e47e-b1c6-4135-b74e-2ef3a96c1b91.png)

![image](https://user-images.githubusercontent.com/24877503/236950509-1fca8e28-cbb9-4a55-8eaa-6f3b282131aa.png)

### Some considerations in our OEM use case

- In a multi tenant context, development will be done in one specific tenant. Then, the app template or product will be deployed to customers tenant.
- In customer tenants shared spaces will be used anyway to import the applications before publishing.
- Edit role in a shared space allows users to create data connections, meaning that **we don't recommend to grant access to a shared space to end-customers by assinging them "can edit" privileges**.
- For OEM, **we don't recommend to grant end-customers access to create content in personnal space** (at least for data connections). 
- Self-service and authoring must occur in governed applications in a managed space. Admin can be allowed to manage self-service sheets (unpublishing for example) this enables you to add some governance around the process by cleaning up the environment by assessing whether content is of sufficient quality.

![image](https://github.com/BaptisteDurand/QlikCloud_DocumentOEMGovernance_RolesResponsabilities/assets/12411165/8360bc2a-c19b-430f-879a-b0411409cc50)


# 4. Qlik Cloud : Simple implementation

Let's go back to our 4 users profiles

### Administrator - *Paul*

In our [example](https://integrationdemo2.qlik.com/) Paul is an administrator with the rights to connect interactively to all the tenants.
So, Paul account has been granted with the [Tenant Admin role](https://help.qlik.com/en-US/cloud-services/Subsystems/Hub/Content/Sense_Hub/Admin/SaaS-user-permissions.htm).

Another approach could be to used a bot user to [configure and managed the customer tenants](https://qlik.dev/manage/platform-operations/overview) without a direct connection.

### Developer - *Martin*

Martin must at least have access to the tenant where development is done. On this tenant he needs to be able to create apps, and connect to the data.

He needs : 
- A professional entitlement 
- To have access to a shared space to develop applications with at least the *Can edit* role
- To have a access to a managed space to publish applications for internal testing with at least *Can publish*	and *Can contribute* 

According to the size of the developer team additional roles can be granted and revoked to allow a fine grained authorization model to ensure an optimized collaboration between developers (even with GIT version control). 

### Contributor - *Linda*

As an end-customer user Linda has access to one specific tenant (because we give each of your customers its own tenant), and she should be able to do limited self service: create sheets and use the insight advisor.

Linda needs : 
- A professional entitlement to be able to create sheets in a published app
- To have access to the managed space(s) where apps are published with the *Can Contribute* role
- **NOT** have access to any shared space (at least with the *Can edit* role) to not create new data connection.
- **NO** Qlik Security Role
 
### Consumer - *John*

As an end-customer user John needs to have display only access to one specific tenant.

John needs : 
- An analyzer entitlement to be able to create sheets in a published app
- To have access to the managed space(s) where apps are published with the *Can View* role
- To have **NO** Qlik Security Role

**Tenant global settings**

To prevent unexpected and unauthorized changes by end users, the following tenant level settings (in the console) need to be considered: 
- Professional entitlements can create shared spaces : Disabled
- Professional entitlements can create private content : Disabled (disables the ability to create content in her personal space)

# Going further

The goal of this OEM use case is to present a simple configuration to : 
- Focus on the common use case : Open dashboard and analytics self-service to end-customer's users
- Identify the main user profiles
- Get the main security components to setup the tenant
- Configure the main security settings
- Start to consider application development workflows  

Recomended next steps : 
- Implement authentication with groups management
- Deep dive in Qlik application develomment and deployment
- Consider advanced *Active Intelligence* capabilities as Qlik Reporting or AutoML.
