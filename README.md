# DevOpsCD

The main purpose of the project is to experiment and visualize how to implement a Continuous Integration and Continuous Deployment system in a Devops environment.

## Getting Started

Building (if necessary) and running the container:
```bash
docker compose --project-name DevOpsCD up -d
```
To clean up:
```bash
docker compose --project-name DevOpsCD down --rmi all --volumes --remove-orphans
```

*\*Note: These commands are only help to see the project in a docker container but aren't useful for the project himself.*

## Steps for this project
- Fork the [repository](https://github.com/mohametdia/ST2DCE-PRJ.git) and clone it locally
- Adding this Readme.md file
- Change `MyController.java` to add our names

### Jenkins
- Start a Jenkins container in Docker desktop and open it in the browser
  ![img](screenshots/Jenkins_container.png)
- Create a new pipeline job
  ![img](screenshots/Jenkins_new_item.png)
  ![img](screenshots/Jenkins_new_item_pipeline.png)
  - Create `Jenkinsfile` in the root of the project
  - Configure the pipeline job, adding the `Jenkinsfile` to script section
    ![img](screenshots/Jenkins_pipeline_config.png)
    ![img](screenshots/Jenkins_pipeline_config_script.png)
  - Adding credentials
    ![img](screenshots/Jenkins_admin.png)
    ![img](screenshots/Jenkins_credentials.png)
    ![img](screenshots/Jenkins_credentials_add.png)
    ![img](screenshots/Jenkins_credentials_add_docker.png)
  - Adding tools
    ![img](screenshots/Jenkins_tools.png)
    ![img](screenshots/Jenkins_tools_maven.png)
  - Adding Plugin `Docker`
    ![img](screenshots/Jenkins_plugin.png)
    ![img](screenshots/Jenkins_plugin_docker.png)
  - Create a cloud docker
    ![img](screenshots/Jenkins_cloud.png)
  - Add `0.0.1-SNAPSHOT` in the Dockerfile
  - Finally we can test and we obtain the following result
    ![img](screenshots/Jenkins_deploy_succeed.png)
  - Adding Plugin `Kubernetes`
    ![img](screenshots/Jenkins_plugin_kubernetes.png)
  - Create a cloud kubernetes
