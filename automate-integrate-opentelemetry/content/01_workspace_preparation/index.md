## Prepare your workspace

Welcome to the **Automate and integrate OpenTelemetry with Dynatrace lab**.

Before we begin, your workspace must get prepared. In this section we will

- Turn on ECC controller in your Dynatrace environment
- Access our Development Host
- Pre-build the application for todays session.

### Turn on ECC

Go to Dynatrace Menu -> Settings -> OneAgent features -> ECC

![Development Machine](../../../assets/images/ubuntu.png)

### Access development host

In addition to your Dynatrace Environment we have provisioned a development host for you.

Here we will build and run the applications for todays session.

#### Log into your development environment

The IP Address and credentials for this Ubuntu host can be found within the `Environments` tab.

![Development Machine](../../../assets/images/ubuntu.png)

There is no need to open a Terminal Window to that host - this machine can get accessed with your browser.

Open a new browser window and paste in the IP Address into the address line. You should see the login screen now. Enter the password that has been provided for you alongside with the IP Address.

![Code Server Login](../../../assets/images/code-server-login.png)

We are using <a href="https://github.com/coder/code-server" target="_blank">Visual Studio Code Server</a> today in order to build and launch applications.

![Code Server Login](../../../assets/images/vs-code-first.png)

> <sub>A small popup may become visible at the bottom right of your screen, notifying you that the environment is getting accessed via an insecure domain. You can safely click on the ``I understand`` button here. None of the examples of today contain any confidential information.</sub>

#### Open a terminal tile in Visual Studio Code
Your development environment also provides access to the command line of your host.

Click on the Menu Button ![New Terminal](../../../assets/images/menu-button.png) in the top left corner and select `Terminal` and in here `New Terminal`.

![New Terminal](../../../assets/images/new-terminal.gif)

As a result a terminal tile will appear at the bottom of the screen.

![New Terminal 2](../../../assets/images/new-terminal-2.png)

You current directory within this bash terminal should be `dtu_training` at this point.

### Prebuild the application

![New Terminal 2](../../../assets/images/cd-shopizer.png)

Next we're pre-building the application.

```bash
cd shopizer
mvn clean install
```

This initial build will take a minute or two to complete.

![New Terminal 2](../../../assets/images/mvn-clean-install.png)

> ***Hint***
>
> In the event that you close the browser window/codeserver tab accidentally, you can always follow the steps below.
> 
> - Relaunch the browser with the IP address.
> - Login to codeserver.
> - In the **Getting Started** screen, click on **Open a folder** to start,
> - or use the hamburger menu ![New Terminal](../../../assets/images/menu-button.png) -- File -- Open Folder
> - Select **shopizer** folder 

### Run the application

To run the application, type this command `mvn spring-boot:run` in the `sm-shop ` directory.

```bash
cd sm-shop
mvn spring-boot:run
```



### You have arrived!

At this stage your development machine is ready to run the applications we are going to use today.