# Getting Started With EnterpriseReady

Welcome! EnterpriseReady is a platform for developers who are building software used by enterprise customers. EnterpriseReady's goal is to take care of all of the enterprise features required by enterprise customers so that you can focus on what you care about - building on your core product.

EnterpriseReady is an API and React Component Library that helps you add enterprise features:

* Multi-User Team Management
* Role Based Access Control
* Audit Logging
* Single Sign-On
* Advanced Account Security
* Notifications, Alerts and Reporting
* Vendor Assessment Forms, SLAs and Contracts

To see what EnterpriseReady can do, let's dive in and try it out.

## Getting Started With Teams

If you haven't already, create an account at enterpriseready.com. After you register, you'll be asked to create your first space. Think of spaces like folders in your desktop, they just help you keep things organized. Rules and settings can be shared and re-used inside a space. Typically a space maps to a single application of yours, but it doesn't have to.

Once you create your space, you'll be directed to the team management page. Since your customers work in teams in your app, teams are at the core of the EnterpriseReady service. To try out EnterpriseReady, get started by creating your first team. You can do that by clicking the +New Team button in the dashboard, or you can do that via API:

```
POST enterpriseready.com/api/space/:space_id/teams
```

Required parameters

| Name          | Type          | Example                         |
| ------------- | ------------- | ------------------------------- |
| name          | string        | "Company Corp"                  |

Now you have a team to try out EnterpriseReady's functionality with. Woo!

Try inviting yourself to your newly created team. You can do this in the dashboard by clicking +Add User, or you can do this via API:

```
POST enterpriseready.com/api/space/:space_id/teams/:team_id
```

Required parameters

| Name          | Type          | Example       |
| ------------- | ------------- | ------------- |
| users         | array         | [{ "user_email": "user@example.com", "user_id": "123456" "permissions": ["team:edit"] |

The user id set in EnterpriseReady should be the same as the internal user id you have in your database so that way you can correlate users in EnterpriseReady with your internal record keeping.

Now go check your email, you will now have an invitation to join your team. Once you accept that emailed invitation, you will officially be part of your team on EnterpriseReady. If you want users to join teams on your platform without needing to accept an email invitation, you will just set "force_add" to true in the request. You can customize the invitation email in the dashboard based on what you would like your users to see when they invite their teammates to your application.

## Getting Started With Role Based Access Control (RBAC)

When we added you as a user to your new team, you may have noticed we gave you a permission of `team:edit`. EnterpriseReady ships with a few default permissions that work out of the box. One is `team:read` and `team:edit`. Users with the `team:read` permission can see who is in the team and the settings of the team. Users with the `team:edit` permission can make changes to the people and settings of the team.

You can also define your own permissions on EnterpriseReady. To create a new permission, go to the permission section of the dashboard where you will see existing permissions (at this point, just the default ones) and click the +New Permission button. Or, as always you can use the API:

```
POST enterpriseready.com/api/space/:space_id/permissions
```

Required parameters

| Name          | Type          | Example       |
| ------------- | ------------- | ------------- |
| permission    | string        | "billing"     |

 The API above will automatically create a `billing:read` and `billing:edit` permission so that you do not have to define both.

 These new permissions you should be related to functionality in your app your enterprise customers may want to gate access to. Then when your users are interacting with your application, when they go to perform an action, you can check if they have the required permission. To check if a user has the required permission for an action, you can check the `is_allowed` API endpoint:

 ```
 POST enterpriseready.com/api/space/:space_id/teams/:team_id/is_allowed
 ```

 Required parameters

 | Name          | Type          | Example        |
 | ------------- | ------------- | -------------- |
 | user_id       | string        | "123456"       |
 | permissions   | array         | ["billing:read"] |

 The above API request returns true or false whether a given user in a given team has a specified list of permissions.

## Getting Started With Audit Logs

One feature enterprise customers require is an audit log of every action performed by one of their team members.

You can send actions to the audit logging service via API like this:

```
POST enterpriseready.com/api/space/:space_id/teams/:team_id/audit_log
```

Required parameters

| Name          | Type          | Example        |
| ------------- | ------------- | -------------- |
| event_name    | string        | "log in"       |
| ip_address    | string        | "1.1.1.1"      |
| user_id       | string        | "123456"       |
| metadata      | object        | { "extra_info": "goes here" } |

`timestamp` is an optional parameter - if it is not set, the current datetime is used.

Go ahead and try it - send an event to the audit logs. And once you do, you can then immediately query the audit logs to get back the event you just sent:

```
GET enterpriseready.com/api/space/:space_id/teams/:team_id/audit_log?since=2020-01-01
```

Every action within EnterpriseReady is audited and recorded in audit logs so if you visit your EnterpriseReady settings you can see the audit logs in action, you will see your previous actions of creating the team, adding the user and assigning them a permission.

## And you're on your way!

Our goal is to help you quickly get all of the enterprise features out of the way. If there is anything we could change or add to make that easier, let us know: mayor@dani.town.
