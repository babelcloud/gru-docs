# Gru Quick Start
Test gru is still in testing phase.
If you have any question, please connect to us:
connect@gru.ai,or join our [slack](https://forms.office.com/pages/responsepage.aspx?id=pv-U4cZeVEmv6PWOhkVcioQxW7gnJCNAlyoGvQ7Z9OJUMENBQzZJTU5JSlIyMUxYQk1DOVhaNDVXSi4u&route=shorturl).  

Currently, Test Gru only supports Node.js/TypeStript. We are gradually adding support for other languages.
Here are the examples to help you quickly start:

[TypeScript Sample](https://github.com/gru-agent/testgru-example-ts)

[Python Sample](https://github.com/gru-agent/testgru-example-py)

[Java Sample](https://github.com/gru-agent/testgru-example-java)

[Go Sample](https://github.com/gru-agent/testgru-example-go)


## Creat Test Gru Account
Log in at [Gru.ai](https://gru.ai). Test Gru currently only supports use with GitHub accounts. You need a GitHub account to log in to gru.ai.

<img width="1313" alt="image" src="https://github.com/user-attachments/assets/de12df9f-1afb-45ee-bd34-e623290d2a10" />

<img width="1312" alt="image" src="https://github.com/user-attachments/assets/b1b58ce8-566f-45da-bd63-c14110357c79" />

## Enter Test Gru
Click the top left corner to select [Test Gru](https://gru.ai/:test).

<img width="1311" alt="image" src="https://github.com/user-attachments/assets/554153d2-1516-41da-b089-31d7d4778604" />


## Install Github Application
Follow the steps to install Test Gru.

<img width="1315" alt="image" src="https://github.com/user-attachments/assets/fc3f97f4-8f45-4cdd-9cd7-fa06e41448a0" />
<img width="1314" alt="image" src="https://github.com/user-attachments/assets/4ad8cd6b-bbc4-4387-bc15-91f3184f5028" />


Then select a repo, perform the configuration.
<img width="1313" alt="image" src="https://github.com/user-attachments/assets/c4a35e2e-b0d1-4d2e-9e24-65386641ff67" />
<img width="1314" alt="image" src="https://github.com/user-attachments/assets/68bfc952-8f55-48f1-a9bd-f83b108f7fad" />
You can try using the default configuration. To ensure accuracy, you can click "Verify configuration" to test it.


## Use Example For Quick Start
1. Fork this repo:

[TypeScript Example](https://github.com/gru-agent/testgru-example-ts)

[Python Example](https://github.com/gru-agent/testgru-example-py)

[Java Example](https://github.com/gru-agent/testgru-example-java)

[Golang Example](https://github.com/gru-agent/testgru-example-go)

2. Save the default configuration.

<img width="1309" alt="image" src="https://github.com/user-attachments/assets/d7dabe25-a12c-42fa-8c3e-4cc96362bbbd" />

3. dispatch src/user.ts

<img width="1315" alt="image" src="https://github.com/user-attachments/assets/4941be30-7a39-43f8-9d5f-d53763373623" />

4. Test Gru submits a PR
<img width="1315" alt="image" src="https://github.com/user-attachments/assets/3779383a-70ca-4419-bac0-73b2b832d2de" />
<img width="1314" alt="image" src="https://github.com/user-attachments/assets/6f06387c-6e34-4b46-af0b-d67adc7fe26e" />



## grutest.yaml 
Configuration file example

```
version: "0.1"
global:
  setup:
    - npm install
pipeline:
  runTest:
    // If your project uses ESLint and Prettier, you can configure the pre-stage here.
    // pre:
    //    - npx eslint --fix {{sourceFilePath}} 
    //    - npx prettier {{sourceFilePath}} --write
    exec:
       - npx vitest run {{testFilePath}}
    // If your project has certain requirements for the final submitted code, you can configure the post-stage here.
    // post:
    //   - npm run lint
    //   - npx tsc --noEmit
settings:
  // IF you allow TestGru to add export to your source code classes or functions when it needs to test your source code
  exportFunctionOrClass: allow
  // Location of the source code project
  include:
      - src
  // Location of the test files
  testPlacementStrategies:
    - type: co-located
      testFilePattern: "{{sourceFileName}}.spec.ts"
```
Learn more about configuratin: https://github.com/babelcloud/gru-docs/blob/main/README.md#3-configuration

## Trigger Test Gru to work

### Auto Rrigger by Pull Request
When you complete the configuration, Test Gru will automatically take over your repository. Whenever you submit a PR, Test Gru will automatically detect software that requires unit tests and add tests for it.

<img width="1290" alt="image" src="https://github.com/user-attachments/assets/2cdd8180-61dd-49cc-92a8-1a75613ddf64" />


After Gru completes writing the test code, it will run the tests. Once it confirms there are no issues with the test code, it will submit a PR with the unit test code to the current PR.



### Manual Trigger
You can manually trigger Test Gru on the [Gru.ai](gru.ai/:test).
<img width="1315" alt="image" src="https://github.com/user-attachments/assets/4941be30-7a39-43f8-9d5f-d53763373623" />

It can be triggered by PR or existing code files.

