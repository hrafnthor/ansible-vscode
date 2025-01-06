# Ansible VSCode

An Ansible role for fetching and installing Visual Studio Code.

Will only run if it can not find the program `code` on the path.

## Requirements

This role requires the python package `jsonschema` to be installed on the system.

```shell
pip install jsonschema
```

## Setup

Before the role can be used it needs to be added to the machine running the playbook, and as of writing this, this role is not hosted on Ansible-Galaxy only on Github.

1. Create a requirements.yml file in the root directory of the playbook being worked on.

2. Add the following definition inside the requirements.yml file:

```yaml
- name: hth-microsoft-vscode
  src: https://github.com/hrafnthor/ansible-vscode.git
  scm: git
  version: "0.0.3"
````

3. Install the requirements by executing

```yaml
ansible-galaxy install -r .requirements.yml
```

This will allow any playbook run from this machine to use the role hth-microsoft-vscode


## Role Variables

#### Default variables

`vscode_client_download_latest_url`

Is the url that directs to the latest VS Code client for debian. If no version name is given then the role will download from this url.
Find out more [here](https://code.visualstudio.com/docs/setup/linux)

---

`vscode_client_download_url_host`

The host url used when dowloading version specific clients.
Defaults to `https://update.code.visualstudio.com`.
Find out more [here](https://code.visualstudio.com/docs/supporting/faq#_previous-release-versions).

---

`vscode_client_architecture`

The architecture to download for. Is only used when downloading a specific version. Defaults to `linux-deb-x64`.
Find out more [here](https://code.visualstudio.com/docs/supporting/faq#_previous-release-versions).

---

`vscode_client_release`

The release type. Is only used when downloading a specific version. Defaults to `stable`.
Find out more [here](https://code.visualstudio.com/docs/supporting/faq#_previous-release-versions).

---

`vscode_client_download_path`

The local path to where the client installer will be downloaded. Defaults to `/tmp`

#### Input variables

All variables are optional unless otherwise stated.

```yml
vscode:
  gather_facts: [boolean]  Indicates if task should gather facts or leave that to parent playbook. Defaults to yes.
  remove: [boolean] Indicates if VSCode should be removed from the system. Defaults to false.
  version: [string] The client version to download (for example '1.92.2')
  checksum: [non empty string] The checksum for the version artifact. See below on how to find that information.
```

##### Example

```yaml
vscode:
  gather_facts: false
  version: "1.96.2"
  checksum: "ff58dfdb0e5674d8e42e1f3907be75a36587b1ca45e586ac06353e97869474e7"
```

#### Checksum

To get the checksum for the client being downloaded, simply query `https://update.code.visualstudio.com/api/versions/<version>/<architecture>/<release>` endpoint the correct `version name`, `architecture` and `release type` and receive:

```json
{
    "url": "https://az764295.vo.msecnd.net/stable/660393deaaa6d1996740ff4880f1bad43768c814/code_1.80.0-1688479026_amd64.deb",
    "name": "1.80.0",
    "version": "660393deaaa6d1996740ff4880f1bad43768c814",
    "productVersion": "1.80.0",
    "hash": "ed368fa20f7186bf6d1d5578fe6b787b41500ce5",
    "timestamp": 1688476473091,
    "sha256hash": "3fbc0fd2bf960fc495950e406ba24ee800c047156d1e88885239655e922e15a7"
}
```
<sup>
	Result from querying https://update.code.visualstudio.com/api/versions/1.80.0/linux-deb-x64/stable
</sup>


License
-------

MIT license. See attached license file.

Author Information
------------------

Hrafn Thorvaldsson.
http://www.hth.is
