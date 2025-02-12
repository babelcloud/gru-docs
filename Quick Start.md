Test gru is still in testing phase.
If you have any question, please connect to us:
connect@gru.ai,or join our [slack](https://forms.office.com/pages/responsepage.aspx?id=pv-U4cZeVEmv6PWOhkVcioQxW7gnJCNAlyoGvQ7Z9OJUMENBQzZJTU5JSlIyMUxYQk1DOVhaNDVXSi4u&route=shorturl).  

Currently, Test Gru only supports Node.js/TypeStript. We are gradually adding support for other languages.
Here are the examples to help you [quickly start](#Quick Start with Example Repository):
[TypeScript Example](https://github.com/gru-agent/testgru-example-ts)
[Python Example](https://github.com/gru-agent/testgru-example-py)
[Java Example](https://github.com/gru-agent/testgru-example-java)
[Go](https://github.com/gru-agent/testgru-example-go)


# Creat Test Gru Account
Log in at [Gru.ai](https://gru.ai). Test Gru currently only supports use with GitHub accounts. You need a GitHub account to log in to gru.ai.

![image](https://github.com/user-attachments/assets/79d337d2-21e4-4067-b1fd-a0eb48709976)

![image](https://github.com/user-attachments/assets/47d87154-5fa6-4de0-b56a-f7ee1f56e651)
# Enter Test Gru
Click the top left corner to select [Test Gru](https://gru.ai/:test).

![image](https://github.com/user-attachments/assets/b86cc641-de39-4993-a302-60daad6ccca3)

# Install Github Application
Follow the steps to install Test Gru.

![image](https://github.com/user-attachments/assets/f1137208-b617-400d-92cb-1c5406d561f1)
![image](https://github.com/user-attachments/assets/4842701d-a3b6-4193-89cf-201d0ed6b1d4)
Then select a repo, perform the configuration.
![image](https://github.com/user-attachments/assets/9849d812-57d3-48f3-86de-ee2427d88e49)
![image](https://github.com/user-attachments/assets/3a9688d8-f513-4612-8546-28101669387d)

# Use Example For Quick Start
1. Fork this repo:
[TypeScript Example](https://github.com/gru-agent/testgru-example-ts)
[Python Example](https://github.com/gru-agent/testgru-example-py)
[Java Example](https://github.com/gru-agent/testgru-example-java)
[Go](https://github.com/gru-agent/testgru-example-go)
2. choose language and framework to gennerate 
![image](https://github.com/user-attachments/assets/c5a2d478-8bc7-4fe2-bc3c-a9e56f95190a)
3. dispatch src/user.ts
![image](https://github.com/user-attachments/assets/8daf5a66-b4db-4d3c-b613-2f1f823e0397)
4. Test Gru submits a PR
![image](https://github.com/user-attachments/assets/bed72345-3730-4b44-985c-466111edde3b)

# grutest.yaml 
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

# Trigger Test Gru to work

## Auto Rrigger by Pull Request
When you complete the configuration, Test Gru will automatically take over your repository. Whenever you submit a PR, Test Gru will automatically detect software that requires unit tests and add tests for it.

![image](https://github.com/user-attachments/assets/ed3bbd18-455f-40bc-99fd-ca3485665651)

After Gru completes writing the test code, it will run the tests. Once it confirms there are no issues with the test code, it will submit a PR with the unit test code to the current PR.

![image](https://github.com/user-attachments/assets/e7af14a3-c9b2-4fba-9bc6-2feb5eacecc6)

## Manual Trigger
You can manually trigger Test Gru on the [Gru.ai](gru.ai/:test).
![image](https://github.com/user-attachments/assets/87fa45b1-2d4b-4871-9dd7-0c257f794341)

It can be triggered by PR or existing code files.
![image](https://github.com/user-attachments/assets/10e5d5d9-b73b-4fba-a2e4-6e11e045a4ff)

![image](https://github.com/user-attachments/assets/d327e627-d341-4ccf-af59-e47bf5195a98)
