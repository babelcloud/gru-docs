# Gru Quick Start
Gru can help you manage unit test automatically. There are two ways to use Gru:
1. Write/update unit test when you write/update source code.
2. Write unit tests for existing source code in a batch mode.

With Gru, you can easily get over 80% coverage for your code.

If you have any question, please connect to us:
connect@gru.ai,or join our [slack](https://forms.office.com/pages/responsepage.aspx?id=pv-U4cZeVEmv6PWOhkVcioQxW7gnJCNAlyoGvQ7Z9OJUMENBQzZJTU5JSlIyMUxYQk1DOVhaNDVXSi4u&route=shorturl).  

## Creat Gru Account
Log in at [Gru.ai](https://gru.ai). Test Gru currently only supports use with GitHub accounts. You need a GitHub account to log in to gru.ai.

<img width="1313" alt="image" src="https://github.com/user-attachments/assets/de12df9f-1afb-45ee-bd34-e623290d2a10" />

<img width="1312" alt="image" src="https://github.com/user-attachments/assets/b1b58ce8-566f-45da-bd63-c14110357c79" />

## Install Github Application
Follow the steps to install Test Gru.

<img width="1315" alt="image" src="https://github.com/user-attachments/assets/fc3f97f4-8f45-4cdd-9cd7-fa06e41448a0" />
<img width="1314" alt="image" src="https://github.com/user-attachments/assets/4ad8cd6b-bbc4-4387-bc15-91f3184f5028" />

Then select a repo, setup the configuration.
<img width="1313" alt="image" src="https://github.com/user-attachments/assets/c4a35e2e-b0d1-4d2e-9e24-65386641ff67" />

We recommend you try Gru on your own repo so that you can better understand how Gru can help with the unit test in production. But you can also fork the following example repos to quick start:
- [TypeScript Sample](https://github.com/gru-agent/testgru-example-ts)
- [Python Sample](https://github.com/gru-agent/testgru-example-py)
- [Java Sample](https://github.com/gru-agent/testgru-example-java)
- [Go Sample](https://github.com/gru-agent/testgru-example-go)

## Configuration - Auto Setup
It is very important to setup the configuration correctly. The purpose of the configuration is to tell Gru how to write and run tests in your repo.

Please follow the steps to setup the configuration.

IMPORTANT: Because Gru will actually clone your repo and run commands on your repo, some of the steps may take some time to complete. Please be patient.

Gru will start a sandbox, clone your repo and guide you through the setup process.
<img width="1313" alt="image" src="https://github.com/user-attachments/assets/7f1de39c-2bae-44f9-a1a5-1c74f56a7405" />


Gru will detect each of the command needed, but Gru need you verify each of them. Please click the "Run" button to verify the command can be executed successfully in the sandbox. 
<img width="1313" alt="image" src="https://github.com/user-attachments/assets/1361b26b-a78a-48f8-add3-c8e7b6921ad8" />

The terminal on the right is for you to explore the environment in the sandbox. It's mainly for you to debug why the command is not executed as expected. But you can also execute any command you want. Please note that Gru will NOT get any information from your manual command execution. You should put back the information into the form on the left side.

NOTE: If terminal is not showing correctly, please click the "Refresh" button.
<img width="1313" alt="image" src="https://github.com/user-attachments/assets/65b63ab6-a8ed-46b3-a1de-e722e76a56af" />


When configuring lint/format command, please make sure the command is run against a specific file.
<img width="1313" alt="image" src="https://github.com/user-attachments/assets/823b7586-3a8d-4631-aef4-29adcfaa21e0" />

After everything is configured, Gru will try to verify all the commands from scratch. Please be patient here. If any command is not executed as expected, you can edit the command and verify again. 
<img width="1313" alt="image" src="https://github.com/user-attachments/assets/af6fe975-6a81-4616-b656-47d3210e8932" />


If everything is OK, click the "Done" button and the configuration will be saved.
<img width="1313" alt="image" src="https://github.com/user-attachments/assets/220346e8-5665-4410-a816-3779b401b535" />

NOTE: You MUST setup the configuration before Gru can work!

## Configuration - Manual Setup
You can always manually edit the configuration. Go to "Settings" page of any repo, you can edit the configuration there.
<img width="1313" alt="image" src="https://github.com/user-attachments/assets/40b9323e-a913-40f0-9042-188776b0c54a" />

Please note that the auto setup process can only set the basic configurations. If you need advanced options, learn more: https://github.com/babelcloud/gru-docs/blob/main/README.md#3-configuration .

## Trigger Gru to work

### Auto Trigger by Pull Request
When you complete the configuration, Test Gru will monitor the code changes in your repository. Whenever you submit a PR, Test Gru will automatically detect and determine if the PR requires unit tests and add/edit tests for it.

<img width="1290" alt="image" src="https://github.com/user-attachments/assets/2cdd8180-61dd-49cc-92a8-1a75613ddf64" />

Example PR trigger: https://github.com/promptfoo/promptfoo/pull/3832


After Gru completes writing the test code, it will run the tests. Once it confirms there are no issues with the test code, it will submit a PR with the unit test code to the current PR.
<img width="1315" alt="image" src="https://github.com/user-attachments/assets/79c8cc90-85ca-4c97-8c99-a6c8959ec5df" />

Example PR: https://github.com/promptfoo/promptfoo/pull/3835

### Manual Trigger
You can also manually select multiple files and trigger Gru to work. Go to https://gru.ai/:test and select the files you want to add tests for.
<img width="1315" alt="image" src="https://github.com/user-attachments/assets/018e6d5d-7c59-42e9-a7aa-c75e7088b879" />
<img width="1315" alt="image" src="https://github.com/user-attachments/assets/515fcac8-42a2-4e0c-9ea4-b437f9f9b6b8" />

Gru will submit a PR for each of the triggered files.
<img width="1315" alt="image" src="https://github.com/user-attachments/assets/9c801ed6-f15e-4464-b704-38a571958a1a" />


