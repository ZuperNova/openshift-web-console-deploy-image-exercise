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

### Create an app from an image
1.  In the new project, choose the `Topology` view from the left menu
1.  Create an app from an image:
  1. Choose `Container Image`
  1. Enter `svejkciber/nodejs-helloworld:00ce99e2badb634a700862bc97963a8d28032f46` as `Image Name`
  1. Enter a name and application name, e,g. `hello`.
  1. Ignore the warning about `Image runs as root`. More on how to fix this issue in a later lab...
  1. Hit `Create`.
1. Inspect the resource created in Openshift: Left-click on the node in the topology. From there, you should see the following
   resources:
   * A DeploymentConfig
   * A Service
   * A Route
   * A ReplicationController: it seems that you have to use `Advanced | Search` to find it.
   * A Pod
   * No Builds or BuildConfigs, since this is a pure dpeloyment.
   
 ### Inspect logs and correct the deployment error
 The deployed pod fails to start up. 
 
 1. Select the failing pod from the deployment config view and inspect its log. Choose the tab `Logs`
    It should show an error like:
    ```
    /usr/src/app/app.js:6
  throw "WORLD_STATE is not set in the process environment.";  
  ^
WORLD_STATE is not set in the process environment.
(Use `node --trace-uncaught ...` to show where the exception was thrown)
    ``` 
 
 1. Fix the error and redeploy
   1. Go back to the deployment configuration and set the missing environment variable
     1. Choose the tab `Environment`
     1. Add the environment variable as a `Single Value`. The name of the variable must match the above error message, the value can be any string.
     1. Save the configuration change with the button on the bottom of the page.
   1. Go back to the deployment configuration and observe that an automatic redeploy started (and probably finished).
     1. Verify by checking that 
       * a new replication controller was created recently.
       * a new pod was started successfully
   1. Check that there are no errors in the application log. The application log should now show:
   ```
   Server running at http://0.0.0.0:3000/
   ```
   1. Why did an automatic redeploy start? _Hint_: Look at the `Triggers` section in the `Deployment Config Overview`.
   
1. Test the application
   Open the application's route in the web browser (or curl) by clicking on the URL added on the topology diagram, or from the
   `Routes` section of the `Resources` tab of the deployment configuration. 

### Bonus exercises
If time permits, the following variations can be useful:
1. Create a S2I build from the git repo of the NodeJS app (build it yourself with Openshift)
2. Fork the git repo, replace the pipeline credentials for deploying to DockerHub with your own, 
   repeat the exercise your forked repo. Trigger another build (just commit anything) and 
   publishment of your Docker image. Verify that the trigger `ImageChange` works as expected 
   in the deployment configuration of the Openshift project.  
