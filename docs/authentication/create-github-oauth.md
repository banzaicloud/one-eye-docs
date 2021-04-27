---
title: Authenticate One Eye to GitHub
shorttitle: GitHub
weight: 200
---

To authenticate One Eye access using GitHub accounts, you must create an **OAuth application** on GitHub (Dex documentation refers this as a **github client**).

These applications require a callback url where the authentication process continues after GitHub has finished its own part. As a result, you need a separate application for every root domain.

### Create your own GitHub client

To create a custom GitHub client ID, you have to register an OAuth application on GitHub. Complete the following steps. For details on registering OAuth applications, see the [official GitHub documentation](https://docs.github.com/en/developers/apps/creating-an-oauth-app).

1. Open the [OAuth apps page on GitHub](https://github.com/settings/developers).
1. Select **Register a new application** or **New OAuth App** (whichever is visible).
1. Complete the form. Set the **Authorization callback URL** field to `dex.{root domain}/dex/callback` (for example, to https://dex.localhost.banzaicloud.io/dex/callback if you are using the default root domain).
1. Select **Generate new client secret**.
1. Save the **clientID** and **clientSecret** pair. You will need them in your Dex configuration, for example, in the Observer custom resource:

    ```yaml
    ...
    spec:
      dex:
        dexchart: {}
        enabled: true
        gitHubClientID: 00729926b04259000f58
        gitHubClientSecret: 734b140eca8005aa32f562851e34bdb2bffbc7fe
        gitHubEnabled: true
    ```

> Note: If you want to use this GitHub OAuth application in production, we recommend you transfer its ownership to a GitHub organization. For details, see [Transferring ownership of an OAuth App](https://docs.github.com/en/developers/apps/transferring-ownership-of-an-oauth-app).
