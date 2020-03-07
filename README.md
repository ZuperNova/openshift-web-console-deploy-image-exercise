# openshift-web-console-deploy-image-exercise
An exercise in deploying a prebuilt Docker image to Openshift 4, using its web console.

## Goals
* Introduce Openshift's web console
* Deploy an app from a prebuilt Docker image
* Check status and logs in the web console
* Learn basic troubleshooting of a failed deployment


## Steps

### Background
Quickly review the source for the NodeJS app we are deploying at: 
  https://github.com/svejk-ciber/nodejs-helloworld (you dont need to clone the repo). Notice how the Github Actions pipeline builds and publishes a Docker image from the NodeJS source.

### Setup

1. Open Openshift's web console in your web browser (open the `Console` tab in Katacoda playground).
1. Log in with the developer user
1. CRC (this includes Katacoda): Switch the view to the `Developer`mode. Otherwise you risk running commands as the cluster-admin user.
1. Create a project with a name of for example `hello`.

### Create an app from an image and check deployment status
1.  In the new project, choose the `Topology` view from the left menu
1.  Create an app from an image:
  1. Choose `Container Image`
  1. 
