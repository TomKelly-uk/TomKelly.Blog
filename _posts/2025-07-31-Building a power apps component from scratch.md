Sometimes the out of the box Power Apps components just don’t cut it. Maybe you want a more personalised look that matches your brand, need extra logic that goes beyond the capability of the standard component, or you need to integrate with third-party services. That’s where building your own custom PCF component comes in – it’s easier than you might think!

I’m going to run from start to finish on the whole process together and explain each step in detail, so that you understand the why as well as the how. I also have a quick reference section at the bottom so that once you’ve read through, if you need the key information you can use this when referring back. Our component example can be found in my github page here.  

We will build out a simple component that has been styled to look exactly like an out-of-the-box microsoft component, but with the additional functionality of adding in custom placeholder text before you type. It will look as it does below.
![Finished Product](https://tomkelly.uk/assets/img/Building%20a%20power%20apps%20component/Finished%20product.png)

We will break this down into the following stages to follow along:

0. [System Setup](#SystemSetup)
1. [Creating your project](#CreateProject)
2. [Building your component](#Building)
3. Deploying your component
4. Using your component


## 0.   System setup {#SystemSetup}

Before you start any development, we will first need the right tools. If you have done this before, you can skip this step, though I recommend running through just to make sure.

- Download and install visual studio code
	- [Download Visual Studio Code - Mac, Linux, Windows](https://code.visualstudio.com/download)
	- This will be the main IDE we will be using for creating our component
- Install the power platform tools extension in Visual Studio Code. This includes all the packages and libraries needed to build our component.
	- This can be found within the Visual Studio Code marketplace
 ![0 - System Startup - Power Platform Tools.png](https://tomkelly.uk/assets/img/Building%20a%20power%20apps%20component/0%20-%20System%20Startup%20-%20Power%20Platform%20Tools.png)

- Download and install Node.js
	- This is will be used to create the component dependencies as well as do the actual building. It will be used from the Visual studio terminal, just like an extension.
	- [Node.js — Download Node.js®](https://nodejs.org/en/download)
		- During installation, a popup may appear prompting the installation of additional components. Allowing this can help avoid potential issues later.
			- This process will open PowerShell and install extra components, which typically takes a few minutes.

	- You will need to reboot your machine after this

- Download and install .NET
	- [Browse all .NET versions to download | .NET](https://dotnet.microsoft.com/en-us/download/dotnet?cid=getdotnetcorecli)
	- This will help us later deploy our solution.

## 1. Creating your project {#CreateProject}

Now that we have installed all the tools above, creating the project itself is straightforward. We will be building it from a template with a few commands.

- Open Visual Studio, in the terminal **navigate to a directory** where we will be building out the component. In this case we will be building it in a folder called “PreambleTextField”.
	- If you cannot see the terminal, we can open one from the terminal menu bar.
	![1 - Creating your project - New terminal.png](https://tomkelly.uk/assets/img/Building%20a%20power%20apps%20component/1%20-%20Creating%20your%20project%20-%20New%20terminal.png)
	- You can change directory with the following command: `cd ‘FILE PATH'`

- Run the following command to **create the initial component files from a template**. These will later be edited in the “Building your component” section.
  - `pac pcf init --namespace YOURCOMPONENTNAME --name YOURCOMPONENTNAME --template field`
![1 - Creating your project - pac pcf init.png](https://tomkelly.uk/assets/img/Building%20a%20power%20apps%20component/1%20-%20Creating%20your%20project%20-%20pac%20pcf%20init.png)

  - You should now see some configuration files in your chosen location
    ![1 - Creating your project - template files.png](https://tomkelly.uk/assets/img/Building%20a%20power%20apps%20component/1%20-%20Creating%20your%20project%20-%20template%20files.png)

- As eluded to from the previous step, we should now **create our code dependencies** with the following Node.js command. This is essential to build and test our component later.
  - `npm install`
![1 - Creating your project - npm install.png](https://tomkelly.uk/assets/img/Building%20a%20power%20apps%20component/1%20-%20Creating%20your%20project%20-%20npm%20install.png)
  - If you get an “Unauthorized” error, you need to change allow for scripts to be run by your user. This can be done in PS with the following command
	`Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned`

- We should now be in a good position to **test the template component**
  - Run the following command to test the component locally.
	   `Npm start`

  - We can now see the bare bones beginning of what will become our component. It doesn’t look like much, but we will build this out in the next section.
![1 - Creating your project - npm start test.png](https://tomkelly.uk/assets/img/Building%20a%20power%20apps%20component/1%20-%20Creating%20your%20project%20-%20npm%20start%20test.png)

We should use this command all throughout this guide to both confirm our changes as well as confirm we haven’t broken made a mistake and broken something.

## 2. Building your component{#Building}
Now that we have our template created, we can move onto building the component logic and styling. This aspect of the process will differ a lot more depending on the type of component we are creating, but we will run through this below.

If we haven’t already **opened the component** environment itself in VS Code, we should do this now. You will know this if you can see something similar to the following in a left hand pane of the application:
![2 - Building your component - Component files.png](https://tomkelly.uk/assets/img/Building%20a%20power%20apps%20component/2%20-%20Building%20your%20component%20-%20Component%20files.png)

To open it up, in CMD make sure you are in your component folder and type `Code .` (don’t forget the dot!). 
This should open up your Visual Studio Code application with the context of your component pre-loaded and should look similar to the above screenshot.

This looks like a lot. But don’t worry, you only really need to be thinking about the following files:

- ControlManifest.xml
- Index.ts

We will be running through exactly what these files do and breaking down each aspect of the code so that we know how to edit it to any scenario. 

### ControlManifest.xml
This file is basically like a recipe for your custom component, defining exactly what the component is made up of. 
These are the **essential tags** that a component will not work without:
- `<manifest>`
- `<control>`
- `<resources>`

These other tags are **optional**, but add extra features, or clarity:
- `<property>`
- `<events>`
- `<external-service-usage>`
- `<feature-usage>`
- `<data-set>`
- `<usage>`
- `<accessibility>`
- `<experimental-features>`

We will run through these tags that make up the XML file, with examples so that when it comes to building out our own component, it can be altered depending on the situation. Knowledge of how it works is much more useful than blindly following an example. 

#### `<Manifest>`
This is the root tag that contains everything, defining it as a manifest file.

#### `<Control>`
![2 - Building your component - Control.png](https://tomkelly.uk/assets/img/Building%20a%20power%20apps%20component/2%20-%20Building%20your%20component%20-%20Control.png)
This control tag essential defines what the component is called, it contains couple of different important properties as follows:

| Property    |  Description   |
| --- | --- |
|   **Namespace**  |  This is a folder/group that your component belongs to, to help organise components. It may be useful when building multiple components or working in a team.    |
|  **Constructor**   |  This is the actual name of the component class in your typescript code. Note, that this should match the name in your .ts file (we will go into that file later).

Those two properties together is what uniquely identifies your component.

|  Property   |Description     |
| --- | --- |
|  Version   |   Does what it says on the tin  |
|  **Display-name-key**   |  This is a reference to the component name displayed in Power Apps, it cannot contain any spaces. The actual text can be edited in a separate .resx file, likely in a /strings/ folder.   |
|  **Description-key**   |   This is a reference to the component description that is displayed in Power Apps, it cannot contain any spaces. The actual text can be edited in a separate .resx file, likely in a /strings/ folder.  |
|  **Control-type**   |  this defines what type of component you will be making. Here it is standard, which is most common and is a visual control. Other types are virtual and dataset, which are non visual; used for logic or data manipulation and dataset; for lists/tables of data, respectively.   |

