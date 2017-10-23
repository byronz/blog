---
title: "Continuous Delivery"
date: 2017-10-14T10:36:09-04:00
tags: [CI, Test, Automation]
---

## Release Pipeline

- SCM (github)
- Niagara (automation test suite)
- JIRA
- Winterfell (infrastructure)
- CI Server
- Cloud (aws)

Everything starts from a commit, typically for a feature development. This should automatically trigger a build job on your CI server. the unitary tests is also included in the job.

If any automation test needed for the feature, the test case can be implemented in paraelle.

If everything is OK, we can deploy everything in stage, this step is currently *manual*.
Note: the deploy ansible steps also include a step to report build version to winterfell

The regression can be automatically trigger by this report on Winterfell side upon the report

Additonal exploratory tests and load tests might need to manually perform at this moment as it has better chance to discover defects.

We can merge the develop branch into master if everything is OK. This triggers another build job in jenkins, and again unit tests are executed.




## the continuous delivery flow




![ss](https://continuousdelivery.com/images/pipeline-branching.png)
## アニメ

![test strategy](https://upload.wikimedia.org/wikipedia/commons/c/c9/Continous_Delivery_Test_Strategy.jpg)
![acceptance testing](https://upload.wikimedia.org/wikipedia/commons/4/47/Continous_Delivery_Automated_Acceptance_Testing.jpg)

![continuous delivery](http://continuousdelivery.com/wp-content/uploads/2014/02/01_CD_the_idea_low-res.jpg)

## Reference

- https://www.gocd.org/2017/07/05/product-manager-guide-continuous-delivery.html

- http://www.informit.com/articles/article.aspx?p=1833567