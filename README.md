---
marp: true
theme: gaia
transition: fade
paginate: true
---

<style>
img[alt~="center"] {
  display: block;
  margin: 0 auto;
}
</style>

<!-- _class: lead -->

### Mobile instrument tests: Reliability, Scale and Support

###### Konstantin Aksenov

---

<!-- _class: lead -->

![1 w:256 h:256](https://icongr.am/material/comment-question-outline.svg?color=ff9900)

### Lets highlight the problem first

---

### ![](https://icongr.am/material/comment-question-outline.svg?size=48&color=ff9900) Storyline of UI tests in project

  * Your team (Probably QA) decided to move from **Manual** UI feature testing to **Instrumetal**
  * After sometime you have about 100 UI tests in project, and from time to time **Local** execution is not consistent anymore (*few random UI tests failing for some unknown reasons*) but you sill ok with it (*rerun can help*)
  * You decided to add new CI check for Pull Requiest automation and run UI tests for every change (*100 UI tests taking about 5 munites for each run, but every 10% percent of runs failing because of flakines*)

---

### ![](https://icongr.am/material/comment-question-outline.svg?size=48&color=ff9900) Storyline of UI tests in project

  * Team growing, you have about 20 developers for each platform, and about 1000 UI tests.
  * Now you not able to run UI tests for each Pull Request anymore, because almost each run fails (*Simple math: Success rate for each tests 0.998 ^ 1000 = **13% Success***).
  * Developer compalining about execution time. Each run taking **more then one hour**
  * You need to find solution...

---


### ![](https://icongr.am/material/comment-question-outline.svg?size=48&color=ff9900) Possible ways

  * ![](https://icongr.am/fontawesome/cloud.svg?size=64&color=ff9900)  Run in Cloud
  * ![](https://icongr.am/fontawesome/cogs.svg?size=64&color=ff9900)  Run on Prem
  * ![](https://icongr.am/fontawesome/cut.svg?size=64&color=ff9900)  Create subsets of tests *(PR / Nightly / Release)*
---

### ![](https://icongr.am/fontawesome/cloud.svg?size=48&color=ff9900) Run in Cloud

|Pros|Cons|
|---|---|
|:white_check_mark: Easy for setup |:x: Flexibility|
|:white_check_mark: Can be scaled |:x: You can pay too much|
|:white_check_mark: Not required maintainence ||
|:white_check_mark: Different platforms support ||

---

### ![](https://icongr.am/fontawesome/cloud.svg?size=48&color=ff9900) Run in Cloud

Popular cloud platforms for UI tests
* Firebase Lab
* AppCenter
* Perfecto
* Marathon Cloud
  
---


### ![](https://icongr.am/fontawesome/cogs.svg?size=48&color=ff9900) Run on Prem

|Pros|Cons|
|---|---|
|:white_check_mark: Flexibility |:x: Extra work on environment setup|
|:white_check_mark: Can be much cheaper then Cloud |:x: Maintanence|
|| :x: Specific set of skills (*Mobile DevOps*)|
  
---

### ![](https://icongr.am/fontawesome/cogs.svg?size=48&color=ff9900) UI Test execution tradeoffs

![w:600 center](images/cost_triangle.png)
  
---

### ![](https://icongr.am/fontawesome/cogs.svg?size=48&color=ff9900) Test runner: why we need it?

##### Change **weights** between Speed, Quality and Cost parameters
  
---

### ![](https://icongr.am/fontawesome/cogs.svg?size=48&color=ff9900) Test runner: core functionality

* Stablity: Strategies for flakiness controll (retries)
* Scalability: Run in parallel on multiple devices or agents

  
---

### ![](https://icongr.am/fontawesome/cogs.svg?size=48&color=ff9900) Test runner: extra functionality

* Application management: Install/Reinstall Application
* Filtering: Execute only preselected subset of tests
* Batching: Combine tests in batches for performed execution
* Resource management: Upload and download resources from device
* Reporting: Well-looking execution reports
  
---

### ![](https://icongr.am/fontawesome/cogs.svg?size=48&color=ff9900) Marathon
###### *Cross-platform test runner*
Supported plarforms:
* Android
* iOS
* Flutter

---


### ![](https://icongr.am/fontawesome/cogs.svg?size=48&color=ff9900) Marathon (Android)

![w:600 center](images/marathon_struct.svg)

---
### ![](https://icongr.am/fontawesome/cogs.svg?size=48&color=ff9900) Marathon: How to start

Install CLI tooling

*MacOS*
```
brew tap malinskiy/tap 
brew install malinskiy/tap/marathon-cloud
```

*Linux*
Install from Marathon Release binaries

---
### ![](https://icongr.am/fontawesome/cogs.svg?size=48&color=ff9900) Marathon: Quick Start (Android)

```yaml
name: "My awesome tests"
outputDir: "marathon"
debug: false
vendorConfiguration:
  type: "Android"
  applicationApk: "dist/app-debug.apk"
  testApplicationApk: "dist/app-debug-androidTest.apk"
```

---
### ![](https://icongr.am/fontawesome/cogs.svg?size=48&color=ff9900) Marathon: Quick Start (IOS)

```yaml
name: "My application"
outputDir: "derived-data/Marathon"
vendorConfiguration:
  type: "iOS"
  bundle:
    application: "sample.app"
    testApplication: "sampleUITests.xctest"
    testType: xcuitest
```

---
### ![](https://icongr.am/fontawesome/cogs.svg?size=48&color=ff9900) Marathon: Execute!
```bash
foo@bar:~$ marathon
00% | [omni]-[127.0.0.1:5037:emulator-5554] com.example.AdbActivityTest#testUnsafeAccess started
03% | [omni]-[127.0.0.1:5037:emulator-5554] com.example.AdbActivityTest#testUnsafeAccess failed
03% | [omni]-[127.0.0.1:5037:emulator-5554] com.example.ScreenshotTest#testScreencapture started
05% | [omni]-[127.0.0.1:5037:emulator-5554] com.example.ScreenshotTest#testScreencapture failed
05% | [omni]-[127.0.0.1:5037:emulator-5554] com.example.ParameterizedTest#test[0] started
08% | [omni]-[127.0.0.1:5037:emulator-5554] com.example.ParameterizedTest#test[0] ended
...

```

---
### ![](https://icongr.am/fontawesome/cogs.svg?size=48&color=ff9900) Marathon: Analyse the results
```bash
foo@bar:~$ marathon
...
...
...
Marathon run finished:
Device pool omni:
        22 passed, 15 failed, 3 ignored tests
        Failed tests:
                com.example.MainActivityFlakyTest#testTextFlaky
                ...
        Flakiness overhead: 1849ms
        Raw: 22 passed, 15 failed, 3 ignored, 6 incomplete tests
                com.example.MainActivityFlakyTest#testTextFlaky failed 1 time(s)
        Incomplete tests:
                com.example.BeforeTestFailureTest#testThatWillNotSeeTheLightOfDay incomplete 3 time(s)
Total time: 0H 1m 45s
Marathon execution failed
```

---

### ![](https://icongr.am/fontawesome/cogs.svg?size=48&color=ff9900) Marathon reports
![w:1200 center](images/marathon_report.png)

---

### ![](https://icongr.am/fontawesome/cogs.svg?size=48&color=ff9900) Marathon Allure reports
![w:1200 center](images/marathon_allure_report.png)

---

### ![](https://icongr.am/fontawesome/cogs.svg?size=48&color=ff9900) Customize your strategy: Filtering


* "fully-qualified-test-name"
* "fully-qualified-class-name"
* "simple-test-name"
* "simple-class-name"
* "package"
* "method"
* "annotation"

---

### ![](https://icongr.am/fontawesome/cogs.svg?size=48&color=ff9900) Customize your strategy: Batching


* Isolate batching
* Fixed size batching
```yaml
batchingStrategy:
  type: "fixed-size"
  size: 5
  durationMillis: 100000
  percentile: 80.0
  timeLimit: "-PT1H"
  lastMileLength: 10
```
* Test class batching

---

### ![](https://icongr.am/fontawesome/cogs.svg?size=48&color=ff9900) Customize your strategy: Retries


* No Retries
* Fixed quota retry strategy
```yaml
retryStrategy:
  type: "fixed-quota"
  totalAllowedRetryQuota: 100
  retryPerTestQuota: 3
```

---

### ![](https://icongr.am/fontawesome/cogs.svg?size=48&color=ff9900) Customize your strategy: Dynamic configuration

Marathonfile support environment variable interpolation in the Marathonfile

```yaml
filteringConfiguration:
  allowlist:
    - type: "fragmentation"
      index: ${MARATHON_FRAGMENT_INDEX}
      count: 10
```

and then execute the testing as following:

```bash
foo@bar:~$ MARATHON_FRAGMENT_INDEX=0 marathon
```