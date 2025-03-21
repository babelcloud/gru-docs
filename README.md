# Documentation of Gru Agents

## 1. Introduction
Gru Agents is a set of agents that can be used to automate tasks in software development. Currently, the only agent that is publicly available is Test Gru which can be used to generate unit tests automatically in your repository.
The following diagram shows the workflow of Test Gru:
![image](https://github.com/user-attachments/assets/6d279aa2-3f0c-458d-a35b-e6a2c3f24978)

- User can select files to manually trigger Test Gru to add or update unit tests for the selected files.
- Test Gru will be triggered automatically when a new Pull Request or Push is created.
- Test Gru will analyze the trigger event and determine the files that are affected by the change.
- Once unit tests are ready(passed all executions, lint, etc...), Test Gru will create a Pull Request as deliverable.
- User can review the unit tests and merge the Pull Request or comment on the Pull Request to request changes.

## 2. Installation
1. Login to [Gru.ai](https://gru.ai)
2. Follow the instructions to install the agent in your repository.
[Quick Start](https://github.com/babelcloud/gru-docs/blob/main/Quick%20Start.md)

## 3. Configuration
For each of the repository, you MUST configure before you can use the agent. The configuration is for the following purposes:
1. Tell the agent how to setup the environment for the repository.
2. Tell the agent how to run the tests, lint, and format the code.
3. Tell the agent the language and framework of the repository.
4. Tell the agent the rules of the repository such as where to put the unit tests, what is the naming convention of the unit tests, etc...

Use the [Test Gru Console](https://gru.ai/:test) to edit the configuration.

### Schema
The configuration yaml should follow the schema [Test Gru Configuration Schema](https://gru.ai/api/agent/public/test-gru/schema/public-schema).

Here is an basic example of the configuration:

```yaml
version: "1.0"
name: "My Test Configuration"
global:
  setup:
    - npm install
pipeline:
  runTest:
    exec:
      - npx vitest run {{testFilePath}}
settings:
  exportFunctionOrClass: allow
  include:
    - src
  exclude:
    - src/agents/utils/build-context-msg/index.ts
    - src/scripts/generate-interface.ts
    - src/agents/model/*
  testPlacementStrategies:
    - type: co-located
      testFilePattern: "{{sourceFileName}}.test.ts"
  language: typescript
  framework: vitest
```

#### Supported Languages and Frameworks
Test Gru currently supports the following languages and frameworks:

| Language | Supported Frameworks |
|----------|---------------------|
| TypeScript | jasmine, jest, mocha, vitest |
| JavaScript | jasmine, jest, mocha, vitest |
| Python | pytest |
| Java | junit4, junit5, testng |
| Go | gotest |
| Rust | rusttest |

#### Prebuilt Parameters
The following parameters are prebuilt by Gru and can be used in the configuration.
- `{{sourceFilePath}}`: The path of the source code file, e.g. `src/utils/format.ts`.
- `{{sourceFileName}}`: The name of the source code file, e.g. `format`.
- `{{testFilePath}}`: The path of the test code file, e.g. `test/utils/format.test.ts`.

#### Basic Configuration Fields
The following fields are the MUST have in any configuration.
- `version`: The version of the configuration schema.
- `name`: (Optional) A descriptive name for your configuration.
- `global.setup`: The commands to setup the environment for the repository. You can put multiple commands in one item or separate items.
For example:
    ```yaml
    global:
      setup:
        - npm install
        - npm run build
    ```
    ```yaml
    global:
      setup:
        - |
          npm install
          npm run build
    ```
    Please note that the above two configurations are not equivalent. The first one will run `npm install` and `npm run build` in two separate sessions. The second one will run `npm install && npm run build` in one session.
- `pipeline.runTest.exec`: The commands to run the tests. Similar to `global.setup`, you can put multiple commands in one item or separate items. In most cases, you should put the commands in one item.
- `settings.exportFunctionOrClass`: Whether to export functions or classes. The available options are:
  - `allow`: Allow the agent to export the functions without asking the user.
  - `inquire`: Ask the user before exporting the functions.
  - `not-allow`: Do not export the functions.
- `settings.include`: The folders of source code to trigger the agent. You can specify multiple folders or use wildcard to match multiple folders. If not specified, the agent will NOT be triggered in any case.
- `settings.exclude`: The change of folders/files of source code that should NOT trigger the agent. You can specify multiple folders or use wildcard to match multiple folders. This is useful when you want to exclude some files from being tested. If not specified, the agent will be triggered by any source code changes in the folders specified in `settings.include`. 
- `settings.testPlacementStrategies`: The strategies to place the unit tests. There are four strategies you can choose from:
    - `co-located`: The unit tests are placed in the same folder as the source code. For example:
        ```yaml
        settings:
          testPlacementStrategies:
            - type: co-located
              testFilePattern: "{{sourceFileName}}.test.ts"
        ```
    - `centralized`: The unit tests are placed in a centralized folder. For example:
        ```yaml
        settings:
          testPlacementStrategies:
            - type: centralized
              testFilePattern: test_{{sourceFileName}}.py
              testDir: tests
        ```
    - `regex`: Define your own rules to place the unit tests. For example:
        ```yaml
        testPlacementStrategies:
          - type: regex
            testFilePattern: "{{sourceFileName}}.test.ts"
            testDirPattern: "/src(.*)/g"
            testDirReplacement: "test/$1"
        ```
        The above configuration will place the unit tests in the `test` folder but keep the folder structure of the `src` folder.
    - `inline`: The unit tests are placed inline within the source file. This is particularly useful for languages like Rust.
        ```yaml
        testPlacementStrategies:
          - type: inline
        ```
- `settings.language`: The language of the repository (typescript, javascript, python, java, go, rust).
- `settings.framework`: The testing framework of the repository (varies by language, see supported frameworks table).

#### Advanced Configuration Fields
*The following fields are optional, use them only if you understand what they are for.*
- `env.image`: You can specify a custom image for the agent to use, for example `ghcr.io/babelcloud/sandbox-unit-tester:main`. Gru will use this image to start the sandbox. If not provided, Gru will use the default image.
- `env.credential`: The credential for accessing the image, it can be a Github PAT or other credentials depending on the image hosting service.
- `env.arch`: The architecture of the environment, either `amd64` or `arm64`.
- `global.cleanup`: The commands to clean up the environment after the agent is done.
- `pipeline.runTest.pre`: The commands to run before the tests are executed, usually execute tools to fix code format, such as `npx prettier {{testFilePath}} --write`.
- `pipeline.runTest.post`: The commands to run after the tests are executed, usually execute tools to check code format or lint, such as `eslint_d {{testFilePath}} --fix --no-warn-ignored --max-warnings=0`.
- `pipeline.updateSource.post`: The commands to run after the source code is updated(if you enable `settings.exportFunctionOrClass`, Gru may update your source code), usually execute tools to format the code, such as `npx prettier {{sourceFilePath}} --write`.
- `on.push.include`: Which branches to trigger the agent. You can specify multiple branches or use wildcard to match multiple branches. If not specified, the agent will NOT be triggered by push events.
    ```yaml
    on:
      push:
        include:
          - main
          - feature/*
    ```
- `on.push.exclude`: Which branches to exclude from trigging the agent. You can specify multiple branches or use wildcard to match multiple branches. If not specified, all branches listed in `on.push.include` will trigger the agent.
    ```yaml
    on:
      push:
        exclude:
          - testgru-*
    ```
- `on.pullRequest.event`: Specific pull request events to trigger the agent. Available options are `open` and `merged`.
    ```yaml
    on:
      pullRequest:
        event:
          - open
    ```
- `on.pullRequest.include`: Pull Request events with the specified conditions will trigger the agent. Conditions include the source branch name, target branch, and title of the pull request. For example:
    ```yaml
    on:
      pullRequest:
        include:
          branch:
            - "**"
          title:
            - "feature/*"
    ```
    
    The configuration now also supports more detailed source and target branch specifications:
    ```yaml
    on:
      pullRequest:
        include:
          sourceBranch:
            - from: original
              name: "feature/*"
          targetBranch:
            - name: main
          title:
            - "feature/*"
    ```
    The `from` field in `sourceBranch` can be:
    - `all`: Match branches from both forked and original repositories
    - `forked`: Match only branches from forked repositories
    - `original`: Match only branches from the original repository
    
    Note: 
    1. If no `on.push` or `on.pullRequest` is specified, the default behavior is to trigger the agent by all pull request events.
    2. If any `on.push` or `on.pullRequest` is specified, the agent will be triggered by according to the specified conditions.
- `on.pullRequest.exclude`: Similar to `include`, but specifies conditions under which the agent should NOT be triggered.
- `settings.codeLengthLimit`: The maximum length of the source code or test file that Test Gru can handle. Default is 1000.
- `settings.workingDir`: The working directory of the agent. This is useful when you want to run the tests in a specific directory such as monorepo. If not specified, the agent will use the root directory of the repository.
- `settings.repoRoot`: Specifies a custom repository root directory.
- `settings.gitignore`: List of patterns that Test Gru should ignore (similar to .gitignore).
- `settings.mockIgnore`: The packages that should NOT be mocked. For example:
    ```yaml
    settings:
      mockIgnore:
        - ajv
        - lodash
        - lodash-es
        - moment
        - uuid
        - bytes
    ```
    Test Gru will try best to NOT mock above packages when generating unit tests.
- `settings.pullRequest.titleFormat`: The format of the title of the pull request. 
    ```yaml
    settings:
      pullRequest:
        titleFormat: "test: add unit test for {{sourceFilePath}}"
    ```
    The above configuration will generate a title like `test: add unit test for src/utils/test.ts`.
- `settings.pullRequest.branchFormat`: The format of the branch name of the pull request. 
    ```yaml
    settings:
      pullRequest:
        branchFormat: "gru/{{sourceFilePath}}-{{timestamp}}"
    ```
    The above configuration will generate a branch name like `gru/src-utils-test-ts-1739530013160`.
- `settings.pullRequest.targetBranch.updated`: What to do when the branch that triggered the agent is updated.
    ```yaml
    settings:
      pullRequest:
        targetBranch:
          updated: rebase 
    ```
    The available options are `rebase`, `redo`, `skip`. This option is only effective when the agent is triggered by a pull request.
    - `rebase`: Rebase the branch on to the target branch.
    - `redo`: Regenerate the pull request based on the updated branch.
    - `skip`: Do nothing. This is the default behavior.
- `settings.exampleRef`: It is recommended to provide an example reference for the agent to generate unit tests. For example:
    ```yaml
    settings:
      exampleRef:
        - match: "src/store/**/*.ts"
          examples:
            - src/store/chat/slices/aiChat/actions/__tests__/generateAIChat.test.ts
            - src/store/global/action.test.ts
        - match: "src/services/message/**/*.ts" 
          examples:
            - src/services/message/client.test.ts
    ```
    You can specify multiple `match` and `examples` to provide references for different parts of the codebase.
- `settings.experimental`: Enables experimental features:
    ```yaml
    settings:
      experimental:
        skipAnalyzeExistingTest: true
        withMockedSymbolCode: true
    ```
    - `skipAnalyzeExistingTest`: Skip analyzing existing tests when generating new tests.
    - `withMockedSymbolCode`: Generate mocked code for external symbols.
    
### Dry Run
It is highly recommended to do a dry run before you save the configuration file. You can do this by clicking the `Dry Run` button in the configuration editor. Make sure the dry run is successful before you save the configuration. However, if you do believe the configuration is correct, you can save it without a successful dry run.

### Multiple Configurations
You can have multiple configurations in one repository. For example, you can have a configuration for submodule A and another configuration for submodule B. The configurations will take effect at the same time. For example:

```yaml
version: "1.0"
name: "Go Configuration"
global:
  setup: []
pipeline:
  runTest:
    exec:
      - go test {{testFilePath}} -v
settings:
  include:
    - pkg/**
  exportFunctionOrClass: not-allow
  testPlacementStrategies:
    - type: co-located
      testFilePattern: "{{sourceFileName}}_test.go"
  language: go
  framework: gotest
---
version: "1.0"
name: "Service Go Configuration"
global:
  setup: []
pipeline:
  runTest:
    exec:
      - go test {{testFilePath}} -v
settings:
  include:
    - service/pkg/**
  exportFunctionOrClass: not-allow
  testPlacementStrategies:
    - type: co-located
      testFilePattern: "{{sourceFileName}}_test.go"
  workingDir: service
  language: go
  framework: gotest
```
Use `---` to separate different configurations.

## 4. Usage
The usage of the agent is self-explanatory. Treat gru as a remote work colleague and collaborate with you only through Github.

## 5. Troubleshooting
If Gru is not working as expected, please check [Test Gru Console](https://gru.ai/:test), find the repository and assignment, you should see the job execution details.

## 6. FAQ
For any questions, please contact us at [connect@gru.ai](mailto:connect@gru.ai).
