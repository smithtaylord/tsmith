# Deploying to Render with Dotnet checklist
## Building your client
Before we can host our project we need to build our client for production. We will need to set up our server and client folders appropriately. You will do this all `OUTSIDE` of the workspace; make sure that you do not open the workspace after opening VSCode. Here it is important to note the difference between the .client folder and the client folder inside of the server. The *.client* folder that you see outside of the workspace is what holds the `pre-compiled` code for the front-end project. The *client* inside your server folder is our `compiled` code for the front end. Our goal is to essentially create a working copy of the .client folder (front-end) and build it into the client folder inside of the server. By building the client in the server, it eliminates the need to build both separate folders independently when deploying the project.
* Remove all console.logs from your project as these will throw errors in the build. Any logger.logs from the logger utility are fine to leave.
* Outside of the workspace, access the package.json file and update the node version to `^18` if not already.
 - Locate the vite.config.js file inside of the .client folder.  You will need to change the out-directory to indicate where vite should 'build' or construct the front end client. You will change this to point to the client that is inside of the server folder:
> This file pathing should look something like `../{{project_name}}/wwwroot`
* Next, you are going to run the build command that will create the working copy of the front end inside of the server. Open up a terminal instance inside of VSCode and cd into the .client folder and run 'npm i' to install any necessary node modules then run 'npm run build' command. This command will tell vite to construct your client in the designated out directory; remember this was changed to the client folder inside of the server in the previous step.
* cd `.\{{project_name}}.client\`
* npm run build
![clientTerminal](https://cwcurriculum.blob.core.windows.net/fullstack/assets/img/cdClientBuild.png)
After the build command finishes running, you should be able to now open up the server and locate a folder called `wwwroot` . You will see a copy of your front-end inside of this folder. If you run into any errors during the build command, make sure to read them in the console, they will tell you where to look in order to resolve any issues.
> *Errors will typically arise from trying to load local images; make sure your file-pathing for local images are pointed to the right spot and contain no space characters in their path*.
## Setting up Render Web Service
* Go to render and select create new web service. If you have not already, you will need to link your GitHub account to render.
* To set up this app you'll need to connect it to the GitHub repo for the project. Here you should see a list of all of your GitHub repositories. Click the `connect` button for the corresponding repo you want to deploy.
* In Render you will want to select a container environment.
* In the settings of render you will want to adjust your settings to set the root directory of your project repository to the dotnet server of your repository that you have linked
![rootDir](https://cwcurriculum.blob.core.windows.net/fullstack/assets/img/render-root-directory.jpg)
* Next, select the 'advanced' tab to set up your environment variables. You will need to add a variable key and value for each variable in your .env file
in this form you will have to add a variable for every key and value in your .env
for example:
  + Key: AUTH_AUDIENCE => Value: YourAuthAudience
  + key: AUTH_DOMAIN => Value: YourAuthDomain
  + key: CONNECTION_STRING => Value: MYSQLConnectionString
![envVariables](https://cwcurriculum.blob.core.windows.net/fullstack/assets/img/envVariables.png)
* In the advanced settings you will need to specify the location of the the Dockerfile
![rootDir](https://cwcurriculum.blob.core.windows.net/fullstack/assets/img/render-dockerfile.jpg)
* Click on the 'create web service' ... render will now begin building your deployment!
* This takes some time so be patient but once that is done there should be a link at the top of the page to your hosted site!
* Test things out and make sure things work.
> A common problem is that your Auth0 account is not set up to accept your new render site url as an allowed domain, so you will want to go add that to the auth settings of the Auth0 application you used.