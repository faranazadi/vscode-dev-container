# Personal VSCode Dev Container
This is the VSCode Dev Container that I use for personal projects which, at the time of writing this, includes the following tools:

* Terraform
* Pulumi
* AWS CDK
* Ansible
* Golang
* Python3
* TypeScript
* NodeJS/Npm   
* AWS CLI 
* Git

This is a somewhat 'beefy' container, and might possibly be an anti-pattern to what Microsoft intend/recommend, as I have only seen Dev Containers created on a per-project basis. However, it works well for my needs and the slowest part is only the initial image building process, which is to be expected.

## Prerequisites 
* Docker
* Visual Studio Code
* Dev Containers Extension (Extension ID: ms-vscode-remote.remote-containers). See Microsoft's docs [here](https://code.visualstudio.com/docs/devcontainers/containers).

## Installation and Usage
### Automated Method
Open VSCode within the context of this repo and you will be prompted with the following message/notification: 

```bash
Folder contains a Dev Container configuration file. Reopen folder to develop in a container (learn more).
```

If you click 'Reopen in container', this will build the image for you and attach VSCode to a running container of said image. Voila!

## Contributing
All pull requests are welcome. If you decide to use this and find any bugs, please feel free to open an issue.

## Acknowledgements
[Jacob Woffenden](https://github.com/jacobwoffenden) for initially introducing me to Microsoft's Dev Containers and some ideas from his personal one.

## License
[MIT](https://choosealicense.com/licenses/mit/)