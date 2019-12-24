# Getting Started With Audit Logging

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

## Search & Filtering

### Filtering Request Structure

```
GET enterpriseready.com/api/space/:space_id/teams/:team_id/audit_log?user=={user_id}&event=={event_name}&since=2020-01-01&until=2020-01-02&limit={limit}
```
