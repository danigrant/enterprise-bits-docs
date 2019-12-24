# Getting Started With Team Management

Team Management enables teams of users to collaborate and use your software together without having to share credentials. This is a core functionality requested by all enterprises.

## Create a namespace for your application

In the dashboard, create a new namespace for your application. Namespaces allow you to keep settings for a single app in one organized place and not let settings for that app mess with settings for other apps.

## Create Your First Team

To create a team, hit the following endpoint and substitute [app] with the name of your app's namespace:

```
POST /app/:app/teams
```

Required parameters

| Name          | Type          | Example                         |
| ------------- | ------------- | ------------------------------- |
| name          | string        | "dani co"                       |
| logo-url      | string        | "https://example.com/logo.png"  |

## Create Permissions That Will Be Needed By Those In Your Team

Permissions are granular read and edit flags that are combined to create broader more useful roles. EnterpriseBits ships with some default permissions created: `team:edit` and `team:read` for seeing and making changes to the team, respectively, and `team-security:read` and `team-security:edit` for managing security configurations of teams. All `edit` permissions contain `read` by default.

Permissions are common across teams in an application namespace.

You may wish to create your own custom permissions for actions specific to your application. For example, you may wish to create a permission for managing billing in your application. To create new `billing:read` and `billing:edit` permissions:

```
POST /app/:app/teams/permissions
```

Required parameters

| Name          | Type          | Example       |
| ------------- | ------------- | ------------- |
| permission    | string        | "billing"  (this automatically creates a billing:read and billing:edit permission)   |

## Combine Permissions To Create Roles

Permissions are the granular developer level access control rules, and roles are user-friendly and seen in the dashboard. Roles are made up of one or more permissions. To create a new role called "Super Admin" that is made up of the default `team:edit` and the new `billing:edit` permission you just created:

```
POST /app/:app/teams/roles
```

Required parameters

| Name          | Type          | Example       |
| ------------- | ------------- | ------------- |
| name          | string        | "Super Admin" |
| permissions   | array         | ["team:edit", "billing:edit"] |
