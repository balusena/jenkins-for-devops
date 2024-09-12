# Jenkins and Docker Integration

Jenkins and Docker integration allows you to automate the building, testing, and deployment of applications within Docker
containers. By configuring Docker inside Jenkins, you can run Jenkins jobs in isolated environments, ensuring consistency
across different environments. First, install the "Docker" plugin in Jenkins. Then, set up Docker credentials and integrate
Docker commands in your Jenkins Pipeline or Freestyle jobs. Jenkins can build Docker images, push them to a Docker registry
(e.g., DockerHub), and deploy containers. This integration streamlines CI/CD workflows, enhances scalability, and simplifies
managing dependencies in DevOps processes for containerized applications.

## 1.DockerHub credentials configured in Jenkins: First register your DockerHub credentials in jenkins permanently
```
Dashboard > Manage Jenkins > Security(Credentials) > Stores scoped to Jekins(System)(Global Credentials Unrestricted) > Add Credentials

New Credentials?
kind?
[username with password]----> select this in list

Scope?
[Global(jenkins,nodes,items,child items,etc]

Username?
[docker_username]

Password?
[Concealed[docker_password]

ID? 
[balusena_dockerhub]

Description?
[My DockerHub Credentials]

Create/save  --------> Click on it

Global Credentials(Unrestricted)
Credentials that should be available irrespective of domain specification to requirements matching.
| ID                    | Name                                               | Kind                     | Description               |
|-----------------------|----------------------------------------------------|--------------------------|---------------------------|
| `balusena_dockerhub`  | `docker_username/****** (My DockerHub Credentials)`| `Username with password` | `My DockerHub Credentials`|
```

  