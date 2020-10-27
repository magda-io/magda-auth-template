# magda-auth-template

![Version: 1.0.0](https://img.shields.io/badge/Version-1.0.0-informational?style=flat-square)

A Magda Authentication Plugin Template. You can use this as a base to build your own Magda Authentication Plugin.

**Homepage:** <https://github.com/magda-io/magda-auth-template>

## About this document

This document is auto-generated from helm chart [values file](deploy/magda-auth-template/values.yaml) comments and template [README.md.gotmpl](./README.md.gotmpl) using [helm-docs](https://github.com/norwoodj/helm-docs).

Once you installed `helm-docs`, you can re-generate the docs by running:

```
yarn helm-docs
```

## How to use this template repo

To generate a new repo from this template repo, please see [here](https://docs.github.com/en/free-pro-team@latest/github/creating-cloning-and-archiving-repositories/creating-a-repository-from-a-template).

If you are new to this, please have a read [Authentication Plugin Specification](https://github.com/magda-io/magda/blob/master/docs/docs/authentication-plugin-spec.md).

Once you create a new repo, you can:
- Replace `magda-auth-template` keywords with your auth plugin name
- Add your authentication logic to [createAuthPluginRouter.ts](./src/createAuthPluginRouter.ts)
  - You can find passport.js `strategies` that support different IDPs (identity providers) or authentication servers from [here](http://www.passportjs.org/packages/).
- Update authentication config in [index.ts](./src/index.ts)
- Update environment variable defined in [github action workflow files](./.github/workflows) to your config to get [Github Action](https://docs.github.com/en/free-pro-team@latest/actions) CI running.

> Magda provides NPM packages [@magda/authentication-plugin-sdk](https://www.npmjs.com/package/@magda/authentication-plugin-sdk) and [@magda/auth-api-client](https://www.npmjs.com/package/@magda/auth-api-client) that you may need to implement your authentication logic.

To release a docker hub image & helm chart, just [create a release](https://docs.github.com/en/free-pro-team@latest/github/administering-a-repository/managing-releases-in-a-repository#creating-a-release) in your Github repo. This will trigger the CI job to:
- Run test cases (if any)
- Build the docker image
- Publish docker image to Docker Hub
- Publish the helm chart to your S3 bucket

## Config Magda to Use the Auth Plugin

[This commit](https://github.com/magda-io/magda/pull/3018/commits/ddba7183d6195d4cd99c8c0b0cf0b08a78552b1e) shows how to config Magda to use your auth plugin via Helm values file config.

You can also check [Magda Gateway helm chart document](https://github.com/magda-io/magda/blob/e8e60fc2f8e655d82486eec48d0225a9b1b9d895/deploy/helm/internal-charts/gateway/README.md) for more details.

## How to Customise Authentication Process

Here is [an example](https://github.com/magda-io/magda-auth-google/commit/f8d6ce53c64b8f1de9a64daf1a6ee2358177d39e) (based on [magda-auth-google](https://github.com/magda-io/magda-auth-google)) to show:
How you can :
- Decide & Set user's organisation unit by matching user's profile
- Decide & set user's role by matching user's profile
- Customised session data
- More non-authentication related HTTP endpoints

## Source Code

* <https://github.com/magda-io/magda-auth-template>

## Requirements

Kubernetes: `>= 1.14.0-0`

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| authPluginRedirectUrl | string | `nil` | the redirection url after the whole authentication process is completed. Authentication Plugins will use this value as default. The following query paramaters can be used to supply the authentication result: <ul> <li>result: (string) Compulsory. Possible value: "success" or "failure". </li> <li>errorMessage: (string) Optional. Text message to provide more information on the error to the user. </li> </ul> This field is for overriding the value set by `global.authPluginRedirectUrl`. Unless you want to have a different value only for this auth plugin, you shouldn't set this value. |
| autoscaler.enabled | bool | `false` | turn on the autoscaler or not |
| autoscaler.maxReplicas | int | `3` |  |
| autoscaler.minReplicas | int | `1` |  |
| autoscaler.targetCPUUtilizationPercentage | int | `80` |  |
| defaultAdminUserId | string | `"00000000-0000-4000-8000-000000000000"` | which system account we used to talk to auth api The value of this field will only be used when `global.defaultAdminUserId` has no value |
| defaultImage.imagePullSecret | bool | `false` |  |
| defaultImage.pullPolicy | string | `"IfNotPresent"` |  |
| defaultImage.repository | string | `"docker.io/data61"` |  |
| global | object | `{"authPluginRedirectUrl":"/sign-in-redirect","externalUrl":"","image":{},"rollingUpdate":{}}` | only for providing appropriate default value for helm lint |
| image | object | `{}` |  |
| replicas | int | `1` | no. of initial replicas |
| resources.limits.cpu | string | `"50m"` |  |
| resources.requests.cpu | string | `"10m"` |  |
| resources.requests.memory | string | `"30Mi"` |  |
