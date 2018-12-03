uaac target --skip-ssl-validation uaa.dev.cfdev.sh
uaac token client get admin -s admin-client-secret
uaac client add cf-mgmt \
  --name cf-mgmt \
  --secret cf-mgmt-secret \
  --authorized_grant_types client_credentials,refresh_token \
  --authorities cloud_controller.admin,scim.read,scim.write



mkdir cf-mgmt && cd cf-mgmt

cf-mgmt export-config --config-dir=config-me \
  --system-domain=dev.cfdev.sh \
  --user-id=cf-mgmt \
  --client-secret=cf-mgmt-secret

cf-mgmt-config generate-concourse-pipeline
echo vars.yml >> .gitignore
