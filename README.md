# Continuous Integration: Jenkins + Docker image

This repository contains a Jenkinsfile that can be used to build your Docker images from places like Bitbucket locally on the Jenkins server then push it to your DockerHub repo.  You can configure your Bitbucket and Jenkins instance with webhooks so that when you push to a specific branch in Bitbucket, your Jenkins server would automatically run this Jenkinsfile pipeline.

NOTE: This file is just setting a static version number.  You will need to update this Jenkinsfile's NEW_VERSION_NUMBER variable with a version of some sort.