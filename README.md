---
marp: true
theme: gaia
transition: fade
paginate: true
---

<!-- _class: lead -->

# Mobile instrument tests: Reliability, Scale and Support

###### Konstantin Aksenov

---

<!-- _class: lead -->

![1 w:256 h:256](https://icongr.am/material/comment-question-outline.svg?color=ff9900)

# Lets highlight the problem first

---

# ![](https://icongr.am/material/comment-question-outline.svg?size=48&color=ff9900) Storyline of UI tests in project

  * Your team (Probably QA) decided to move from **Manual** UI feature testing to **Instrumetal**
  * After sometime you have about 100 UI tests in project, and from time to time **Local** execution is not consistent anymore (*few random UI tests failing for some unknown reasons*) but you sill ok with it (*rerun can help*)
  * You decided to add new CI check for Pull Requiest automation and run UI tests for every change (*100 UI tests taking about 5 munites for each run, but every 10% percent of runs failing because of flakines*)

---

# ![](https://icongr.am/material/comment-question-outline.svg?size=48&color=ff9900) Storyline of UI tests in project

  * Team growing, you have about 20 developers for each platform, and about 1000 UI tests.
  * Now you not able to run UI tests for each Pull Request anymore, because almost each run fails (*Simple math: Success rate for each tests 0.998 ^ 1000 = **13% Success***).
  * Developer compalining about execution time. Each run taking **more then one hour**
  * You need to find solution...

---


# ![](https://icongr.am/material/comment-question-outline.svg?size=48&color=ff9900) Two possible ways

  * ![](https://icongr.am/fontawesome/cloud.svg?size=128&color=ff9900)  Run in Cloud
  * ![](https://icongr.am/fontawesome/cogs.svg?size=128&color=ff9900)  Run on Prem
---

# ![](https://icongr.am/fontawesome/cloud.svg?size=48&color=ff9900) Solution: Run in Cloud

|Pros|Cons|
|---|---|
|:white_check_mark: Easy for setup |:x: Flexibility|
|:white_check_mark: Can be scaled |:x: You can pay too much|
|:white_check_mark: Not required maintainence ||
|:white_check_mark: Different platforms support ||

---

# ![](https://icongr.am/fontawesome/cloud.svg?size=48&color=ff9900) Solution: Run in Cloud

Popular cloud platforms for UI tests
* Firebase Lab
* AppCenter
* Perfecto
* Marathon Cloud
  
---


# ![](https://icongr.am/fontawesome/cogs.svg?size=48&color=ff9900) Solution: Run on Prem

|Pros|Cons|
|---|---|
|:white_check_mark: Flexibility |:x: Extra work on environment setup|
|:white_check_mark: Can be much cheaper then Cloud |:x: Maintanence|
|| :x: Specific set of skills (*Mobile DevOps*)|
  
---

# ![](https://icongr.am/fontawesome/cogs.svg?size=48&color=ff9900) UI Test execution tradeoffs

"Fast, Cheap, Stable" Diagramm
  
---

# ![](https://icongr.am/fontawesome/cogs.svg?size=48&color=ff9900) Test runner: why we need it?

"Fast, Cheap, Stable" Diagramm
  
---

# ![](https://icongr.am/fontawesome/cogs.svg?size=48&color=ff9900) Test runner: core functionality

* Stablity: Strategies for flakiness controll (retries)
* Scalability: Run in parallel on multiple devices or agents

  
---

# ![](https://icongr.am/fontawesome/cogs.svg?size=48&color=ff9900) Test runner: extra functionality

* Application management: Install/Reinstall Application
* Filtering: Execute only preselected subset of tests
* Batching: Combine tests in batches for performed execution
* Resource management: Upload and download resources from device
* Reporting: Well-looking execution reports
  
---

# ![](https://icongr.am/fontawesome/cogs.svg?size=48&color=ff9900) Marathon
###### *Cross-platform test runner*

---

# ![](https://icongr.am/fontawesome/cogs.svg?size=48&color=ff9900) Marathon: How to start

Install CLI tooling

*MacOS*
```
brew tap malinskiy/tap 
brew install malinskiy/tap/marathon-cloud
```

*Linux*
Install from Marathon Release binaries