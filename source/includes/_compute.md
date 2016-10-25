# Compute Service

Although CloudStack provides its own  native API, you may find it more convenient to use cloud.ca's wrapper for it, as it arguably simpler to use and provide a consistent way of calling all its supported services. Furthermore, it enforces the same environment role-based access control that is defined in the cloud.ca portal.

All compute service API calls must include path parameters `service_code` and `env_name`, which are used to specify which environment is targeted by your request. This information can be retrieved from the **Environment** picker in the **Services** tab, as seen below.

![Retrieve service_code and env_name](/images/service_environment.png)

