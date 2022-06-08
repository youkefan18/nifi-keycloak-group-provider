# nifi-keycloak
Keycloak NiFi AuthProvider

originally forked from https://github.com/ChrisEnglert/nifi-keycloak

## How to use:
1. `mvn clean package` and find the .nar file under nifi-keycloak-provider
2. ```
    kubectl cp ./nifi-keycloak-provider-*.nar nifi-dev-0:/opt/nifi/nifi-current/extensions -c server
    kubectl cp ./nifi-keycloak-provider-*.nar nifi-dev-1:/opt/nifi/nifi-current/extensions -c server
    kubectl cp ./nifi-keycloak-provider-*.nar nifi-dev-2:/opt/nifi/nifi-current/extensions -c server
   ```

## UserGroupProvider
org.apache.nifi.authorization.KeycloakUserGroupProvider


### NiFi properties


```properties
# OpenId Connect SSO Properties #
nifi.security.user.oidc.discovery.url=http://{HOST}:{PORT}/auth/realms/{REALM}/.well-known/openid-configuration
nifi.security.user.oidc.connect.timeout=5 secs
nifi.security.user.oidc.read.timeout=5 secs
nifi.security.user.oidc.client.id={CLIENTID}
nifi.security.user.oidc.client.secret={CLIENTSECRET}
nifi.security.user.oidc.preferred.jwsalgorithm=RS256
nifi.security.user.oidc.additional.scopes=
nifi.security.user.oidc.claim.identifying.user=preferred_username
```

### Authorizers.xml



```xml
<userGroupProvider>
    <identifier>keycloak-user-group-provider</identifier>
    <class>org.apache.nifi.authorization.KeycloakUserGroupProvider</class>

    <!-- KetCloak Server auth endpoint (https://{KEYCLOAK_HOST}:{KEYCLOAK_PORT}/auth)-->
    <property name="ServerUrl">{KEYCLOAK_URL}/auth</property>
    
    <!-- The KeyCloak realm to sync the Users -->
    <property name="Realm">{KEYCLOAK_REALM}</property>

    <!-- The KeyCloak username of the account with the permission to list users ("admin") -->
    <property name="Username">{KEYCLOAK_CLIENT_USERNAME}</property>
    
    <!-- The KeyCloak password  -->
    <property name="Password">{{KEYCLOAK_CLIENT_PASSWORD}</property>

    <!-- The KeyCloak realm to sync the users ( admin-cli ) -->
    <property name="ClientID">{KEYCLOAK_CLIENT_ID}</property>
    
    <!-- The KeyCloak realm to sync the Users -->
    <property name="ClientSecret">{KEYCLOAK_CLIENT_SECRET}</property>
</userGroupProvider>
```    


## Build
```shell script
$ mvn clean package
```
