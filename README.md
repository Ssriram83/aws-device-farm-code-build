### Example to show case android CI pipeline using AWS codepipeline

Step 1: Create a new Android Project in Android Studio

Step 2: Add a buildspec.yaml. 
The spec itself is super simple - it instructs how to build and where the artifacts would be. 
Now.. this is a strip down version of the buildspec.yaml. Its very extensible and you can have prebuild and post build steps. 

```
version: 0.2
phases:
  build:
    commands:
      - chmod +x ./gradlew
      - ./gradlew assembleDebug
artifacts:
  files:
    - 'app/build/outputs/apk/debug/app-debug.apk'
  discard-paths: yes
``` 

Step 3: Create Code Pipeline
Think of codepipeline as the glue that can brings different stages of teh CI together. Its designed to be very flexible and integrates with different tools. 
The typical steps would look like: 
1. Configure the source repo. (eg:  github or codecommit)
2. Configure the build step - which basically involves describing the build env (eg: linux with android studio) and linking to the buildspec. 
  - Quick aside on the build step: Usually we have pre-build - to determine yardstick needed to compile. This might involve running unit tests and codescan (eg:sonarqube or codacy). 
  - Post build would typically involve versioning the artifact and publishing it to a repo.  
3. Configure the test step - which involves running functional integration or performance tests. In this poc - I have integrated the mobile app with device farm. Which would execute the mobile app in different devices. 

Detailed instructions for setting it up: (link) [https://aws.amazon.com/blogs/mobile/automatically-build-your-android-app-with-aws-codebuild/]