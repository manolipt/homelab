# Nextcloud Custom Image

## Default User Groups

By default, this image creates two user groups: "Nextcloud Users" and "Nextcloud Admins". This is just for convenient organization: you can turn it off by defining the `NO_DEFAULT_GROUPS` environment variable in the Nextcloud Application container spec.

## Trusted Proxy Auto-Configuration

If the trusted proxies for the Nextcloud instance are not provided by the user (typically through the `TRUSTED_PROXIES` environment variable on the Nextcloud Application container spec), this image will auto-configure nextcloud to trust all local IPs per the [RFC 1918 standard](https://en.wikipedia.org/wiki/Private_network).

If you wish to override this behavior such that no proxies are trusted, simply define the `NO_TRUSTED_PROXIES` environment variable with any value in the Nextcloud app spec.

## Environment Variable Configuration

The following environment variable configurations are supported, in addition to the standard variables used with the base Nextcloud image.

### Startup Apps

The `NEXTCLOUD_STARTUP_APPS` environment variable can be specified as a space-delimited list of apps to install on the Nextcloud instance at startup.

If using the [OIDC environment variables](#open-id-connect-authentication) for SSO setup, the `user_oidc` plugin does not need to be specified (it will be installed automatically).

### Default Phone Region

If the `DEFAULT_PHONE_REGION` environment variable is defined, the default phone region will be set to its value. See the [nextcloud documentation](https://docs.nextcloud.com/server/latest/admin_manual/configuration_server/config_sample_php_parameters.html#default-phone-region) for information on how this value is set.

### Open ID Connect Authentication

The following environment variables need to be defined for the OIDC provider to be configured at startup:

- `OIDC_PROVIDER_NAME`: Identifier for the provider.
- `OIDC_CLIENT_ID`: Client ID from the provider.
- `OIDC_CLIENT_SECRET`: Client secret from the provider. **Limit this to 64 characters when configuring the provider.**
- `OIDC_CONFIGURATION_URL`: Configuration path for the provider. For Authentik, this is the URL ending in in `.well-known/openid-configuration` from the OIDC provider.

If all of these variables are defined, the image will automatically install the [user_oidc](https://github.com/nextcloud/user_oidc) app to the Nextcloud instance and attempt to configure the provider on startup and set up automatic login passthrough for the provider. The standard user login screen can still be accessed from `https://<NEXTCLOUD-HOSTNAME>/login?direct=1` to debug issues with the pre-configured admin user.
