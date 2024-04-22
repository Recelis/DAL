+++
title = 'Entra'
date = 2024-03-21T08:45:39+10:00
draft = true
+++

### Enterprise Application

### Application Registration

Azure AD would have a list of application registrations. These are applications that users will be able access. The people who can manage the application registration are the 'owners'. Applications do not have a set of members who can access this application, because Azure/Entra only provides permissions based on 'roles'. This is why Azure/Entra is a `role-based access control system`.

### Roles

The different permission levels are defined by an application registration's roles. This might be [Reader, Writer, Admin].

### Groups

Groups are a set of users (or nested groups). Members in a group are called 'group members', while the people managing a group are called the 'owners'.

#### AppRole Assignment

The culmination of roles and groups is the App Role assignment where a role is assigned to a group or to individual users. This gives users/groups the permissions provided by the role. The appRole assignments can be viewed on the Enterprise Application page.

### Permissions

In Azure, applications can be given permissions to access other APIs. Permissions can grouped into two types `Delegated` vs `Application`. Delegated Permissions are when the application acts on the logged in user's behalf while Application Permissions act on the behalf of the application. The privileges that an application can have to access other APIs is defined ... _how?_
