# Jenkins Shared Library Setup and Usage

This document provides a step-by-step guide to setting up and using a shared library in Jenkins.

## 1. Configure Global Pipeline Library

Ensure that the shared library is correctly configured in Jenkins:

- Go to `Manage Jenkins`.
- Select `Configure System`.
- Scroll down to `Global Pipeline Libraries`.
- Add a new library configuration:
  - **Name**: `pipeline-library-demo` (this should match the name used in the `@Library` annotation).
  - **Default version**: Specify a branch, tag, or commit hash (e.g., `main` or `master`).
  - **Retrieval method**: Select `Modern SCM`.
  - **Source Code Management**: Choose `Git`.
  - **Project Repository**: Enter the repository URL (e.g., `https://github.com/saieswarnookala/jenkins.git`).

## 2. Verify Repository URL

Ensure that the URL `https://github.com/saieswarnookala/jenkins.git` is correct and accessible. You can do this by trying to clone the repository manually using Git on your local machine:

```bash
git clone https://github.com/saieswarnookala/jenkins.git
```

If the repository URL is incorrect or inaccessible, correct it and update the Jenkins configuration.

## 3. Structure Your Repository
Ensure that your repository is structured correctly with the required directories. Here’s an example structure:

```
jenkins
│
├── vars
│   └── sayHello.groovy
│
├── src
│   └── (optional package structure)
│
└── (other files)
```

* vars: This directory contains Groovy scripts that define global functions, such as sayHello.groovy.
* src: This directory contains Groovy or Java classes that can be used in your pipeline scripts.


## 4. Create the Required Directories
Create the vars and/or src directories in your repository if they don't already exist.

## 5. Define a Function in vars/sayHello.groovy
Here’s an example of what the vars/sayHello.groovy file might look like:
```
def call(String name) {
    echo "Hello, ${name}"
}
```
## 6. Commit and Push Changes
After structuring your repository and adding the necessary files, commit and push the changes to your GitHub repository:
```
git add vars/sayHello.groovy
git commit -m "Add sayHello.groovy"
git push origin main
```

## 7. Update Your Pipeline Script
Here’s how your Jenkinsfile should look:
```
@Library('pipeline-library-demo') _
pipeline {
    agent any
    stages {
        stage('Demo') {
            steps {
                echo 'Hello world'
                script {
                    sayHello('Alex')
                }
            }
        }
    }
}

```

## 8. Verify the Configuration
Make sure that your Jenkins configuration for the global library is still correct, pointing to the appropriate repository and branch.

After following these steps, your Jenkins pipeline should be able to load the shared library correctly and execute the sayHello function.

Troubleshooting Tips
* **Check Permissions:** Ensure that Jenkins has the necessary permissions to access the Git repository.
* **Network Access:** Verify that the Jenkins server has network access to GitHub.
* **Library Configuration:** Double-check the library configuration for any typos or misconfigurations.

