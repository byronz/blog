---
title: "Continuous Delivery"
date: 2017-10-14T10:36:09-04:00
tags: [CI, Test, Automation]
---



## Release Pipeline

![release overview](/img/release.png)

1. Everything starts from a commit, typically for a feature development. This should automatically trigger a build job on our CI server; the unit tests is also included in the job.

    a. If any automation test needed for the feature, the automation cases can be implemented by tester in parallel.
2. If everything is OK, we can deploy everything in stage, this step is currently manual. Note: the deploying steps also include a step to report build version to winterfell.
3. The regression can be automatically trigger by this report on winterfell side upon the report

    a. Additional exploratory tests and load tests might need to manually perform at this moment as it has better chance to discover defects.
4. winterfell offers the interface to analyze the test results

    a. If regression failed, the failure analysis is performed. The additional patch is requested
5. Code merge from develop into master, which triggers again the build job and unit tests.
6. Deploys the build in production and reports winterfell

    a. smoke test is triggered to quickly determine if anything suspicious during deployment. A rollback is executed in case of trouble

### Components

- SCM (github)
- Niagara (automation test suite)
- Winterfell (infrastructure)
- CI Server
- Cloud (aws)

### Test Flow

![ss](https://continuousdelivery.com/images/pipeline-branching.png)
## アニメ

![test strategy](https://upload.wikimedia.org/wikipedia/commons/c/c9/Continous_Delivery_Test_Strategy.jpg)
![acceptance testing](https://upload.wikimedia.org/wikipedia/commons/4/47/Continous_Delivery_Automated_Acceptance_Testing.jpg)

![continuous delivery](http://continuousdelivery.com/wp-content/uploads/2014/02/01_CD_the_idea_low-res.jpg)

## Reference

- https://www.gocd.org/2017/07/05/product-manager-guide-continuous-delivery.html

- http://www.informit.com/articles/article.aspx?p=1833567