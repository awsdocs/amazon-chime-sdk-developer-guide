# Granting users permissions to a resource<a name="adding-permissions"></a>

The following example shows how to use the [AddResourcePermissions](https://docs.aws.amazon.com/workdocs/latest/APIReference/API_AddResourcePermissions.html) API to grant `CONTRIBUTOR` permissions to a `USER` on a resource\. You can also use the API to give permissions to a user or group on a folder or document\.

```
AddResourcePermissionsRequest request = new AddResourcePermissionsRequest();
    request.setResourceId("resource-id");
    Collection<SharePrincipal> principals = new ArrayList<>();;
    SharePrincipal principal = new SharePrincipal();
    principal.setId("user-id");
    principal.setType(PrincipalType.USER);
    principal.setRole(RoleType.CONTRIBUTOR);
    principals.add(principal);
    request.setPrincipals(principals);
    AddResourcePermissionsResult result = workDocsClient.addResourcePermissions(request);
```