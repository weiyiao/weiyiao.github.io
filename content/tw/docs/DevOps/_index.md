---
title: "DevOps"
linkTitle: "DevOps"
date: 2019-08-26
weight: 1
description: >
  Coretronic Cloud Platform DevOps
---

## Project Build Status

| IdentityServer | AdminWeb | WebAPI & IoT WebAPI |
| -------- | -------- | -------- |
| [![Build Status](https://dev.azure.com/coretronic/CoretronicCloudPlatform/_apis/build/status/IdentityServer-CI?branchName=master)](https://dev.azure.com/coretronic/CoretronicCloudPlatform/_build/latest?definitionId=7&branchName=master)     | [![Build Status](https://dev.azure.com/coretronic/CoretronicCloudPlatform/_apis/build/status/AdminWeb-CI?branchName=master)](https://dev.azure.com/coretronic/CoretronicCloudPlatform/_build/latest?definitionId=9&branchName=master)     | [![Build Status](https://dev.azure.com/coretronic/CoretronicCloudPlatform/_apis/build/status/CoretronicCloudPlatform-WebAPI-CI?branchName=master)](https://dev.azure.com/coretronic/CoretronicCloudPlatform/_build/latest?definitionId=11&branchName=master)     |

## Project Timeline

```mermaid
gantt
    title Coretronic Cloud Platform Gantt Diagram
    
    dateFormat  YYYY-MM-DD
    axisFormat  %m/%d
    
    section Identity
    Single sign-on    :done,    sso,    2019-06-01, 30d
    section Device
    Device    :done, d, 2019-06-01, 10d
    License    :done, l, 2019-06-10, 10d
    Opening    :done, after l, 10d
    section IoTHub
    Server      :done, crit,    2019-07-01  , 60d
    Agent      :crit,    2019-07-25,    60d
    section OTA
    Firmware    :crit,    2019-08-01,    60d
    Software    :crit,    2019-09-01,    60d
    section Remote
    Call:done, 2019-08-01,    30d
    Customer Remote    : 2019-09-01,    60d
    section Analysis
    Collect    : 2019-09-01,    30d
    Analysis    : 2019-10-01,    30d
```