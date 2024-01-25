# DevOpsCD

The main purpose of the project is to experiment and visualize how to implement a Continuous Integration and Continuous Deployment system in a Devops environment.

## Getting Started

Building (if necessary) and running the container:
```bash
docker-compose up -d --build
```

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
