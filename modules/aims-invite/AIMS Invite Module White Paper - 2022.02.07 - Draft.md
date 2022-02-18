---
layout: default
nav_exclude: true
---

# AIMS Invite Module

## Purpose
The AIMS invite module’s purpose is to provide a way to invite existing B2C users and non-B2C users to enroll in various other modules and sub-modules in AIMS.  The following scenario are common, and a typical invite may include one or more:

- Self-provisioning of a B2C identity.
- Enrollment of a B2C identity in a B2C group for the purpose of gaining authorization to AIMS resources.
- Association of B2C identity to AIMS business records for the purpose of gaining authorization to manage said records.

The invite process may include none or all these scenario’s and should be flexible to accommodate other business scenario’s not yet currently in use.

## Requirements
The AIMS Invite Module should be able to do the following:

- Send emails to specified email addresses that provides clear and concise information on the nature of the invite and the enrollment process.
- Provide a unique code or web slug for the user to visit once the have self-provisioned a B2C identity.
- Once this unique code or web slug has been accessed the various scenarios are executed depending on the nature of the enrollment and the pre-defined parameters necessary to achieve the business objective.
- Once these scenarios are executed the user’s browser is redirected to a pre-defined URI.
- Various information is capture at the time of enrollment (enrollment being when an authenticated users successfully accesses the unique code or web slug controller/action). This information should include the following:
  - Date/time of access
  - Ip Address of Access
  - Client Agent
- The invite should be flag in such a way that the users cannot execute the scenarios again but can be redirected to the pre-defined URI.

## Specifications
The AIMS Invite Module consists of two (2) data domains/concepts: “Invite Channels” and “Invites”.

### Invite Channels
Invite Channels are the different types of possible invites and can help predefine certain attributes to the invite such as email template, reflection actions, etc. etc.   (See [InspectionType](https://dev.azure.com/ksag/AIMS%20System/_git/kda-aims-aspnet-mvc?path=/KdaAims.Core/Module/Inspections/InspectionType.cs) in the Inspection module for a similar concept.)

### Invites
Invites are the actual unique records generated for each desired invite.  It should include parameters like:

- Status
- Web Slug
- Redirect URI
- Invite Email Address
- Pre-defined Post Enrollment Actions

The invite domain should make heavy use of reflection to execute post enrollment actions.  It is recommended that this process make use of the AIMS Actions and AIMS Entity Relationship systems.

### Software Roles and Responsibilities
The AIMS Invite Module’s responsibility is to facilitate the enrollment process and manage the post enrollment actions, but it must remain agnostic to the AIMS module/domain leveraging its purpose.

#### AIMS Module (ie Commercial Hemp) Responsibilities
- Determine which invite channel is being used.
- Define post enrollment actions.
- Initiate the creation of the invite.
- **Module specific methods that are called during post enrollment actions. (Calls to these methods will always include KDA Portal UserGuid first and AimsRequest last.)**

#### AIMS Invite Module Responsibilities
- Generate the unique web slug.
- Send the invite email.
- Ensure proper access to the invite enrollment action/page.
- Capture client information upon successful enrollment.
- Execute post enrollment actions. **(Calls to these methods will always include KDA Portal UserGuid first and AimsRequest last.)**
- Redirect user to pre-defined URI.

