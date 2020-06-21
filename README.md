## Introduction

This project provides an all-in-one script for Microsoft AppCenter: https://appcenter.ms/

This script automates the whole flow to create project, connect repo, configure build settings, and generate mobile apps.

## Installation

With Node environment, add this script to your dev dependencies.

`yarn add appcenter-auto-builder --dev` 

Or
   
`npm install appcenter-auto-builder --save-dev`

## Usage

```
import {startAppCenterBuilder} from "appcenter-auto-builder";
import {AppCenterBuilderConfiguration} from "appcenter-auto-builder/type";

const config: AppCenterBuilderConfiguration = {
    apiToken: "<Your API Token>",
    project: {
        name: "test-rn-ios",
        os: "iOS",
        platform: "React-Native",
        description: "My test app",
    },
    repo: {
        url: "https://github.com/dionshihk/some-ios-project",
    },
    owner: {
        type: "individual",
        name: "<Your Account Name>",
    },
    buildSetting: {
        trigger: "manual",
        artifactVersioning: {buildNumberFormat: "buildId"},
        environmentVariables: [
            {name: "TEST_ENV_1", value: "1"},
            {name: "TEST_ENV_2", value: Date.now().toString()},
        ],
        toolsets: {
            javascript: {
                packageJsonPath: "package.json",
                nodeVersion: "12.x",
            },
            xcode: {
                 ....
            },
        },
    },
    extraBuildEnvironmentVariables: [{name: "APP_SECRET", value: {type: "app-secret"}}],
    disconnectRepoOnFinish: true,
    buildEstDuration: 200,
};

startAppCenterBuilder(config);
```

A complete example can be found here:

https://github.com/dionshihk/appcenter-auto-builder/blob/master/test/index.demo.ts

To learn more about `AppCenterBuilderConfiguration` interface, please read `type.d.ts`, with proper comments.

## Good To Know

You are strongly suggested trying to create a project from scratch on AppCenter manually. Because of:

- Connect to your repo (`BitBucket/Github`) at least once manually, to authorize your AppCenter account with OAuth.
This step cannot be done with pure script.

- Learn the complicated build configuration data structure of iOS/Android project.
The official API specification is not well-documented.
GET `/v0.1/apps/{ownerName}/{appName}/repo_config` to find out the meaning of each field, in corresponding with `type.ts/BuildConfiguration` interface. 

- Build the app once, to figure out a great number for `AppCenterBuilderConfiguration.buildEstDuration`.
