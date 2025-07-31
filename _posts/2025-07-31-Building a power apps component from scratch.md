Sometimes the out of the box Power Apps components just don’t cut it. Maybe you want a more personalised look that matches your brand, need extra logic that goes beyond the capability of the standard component, or you need to integrate with third-party services. That’s where building your own custom PCF component comes in – it’s easier than you might think!

I’m going to run from start to finish on the whole process together and explain each step in detail, so that you understand the why as well as the how. I also have a quick reference section at the bottom so that once you’ve read through, if you need the key information you can use this when referring back. Our component example can be found in my github page here.  

We will build out a simple component that has been styled to look exactly like an out-of-the-box microsoft component, but with the additional functionality of adding in custom placeholder text before you type. It will look as it does below.
![Finished Product](https://tomkelly.uk/assets/img/Building%20a%20power%20apps%20component/Finished%20product.png)

We will break this down into the following stages to follow along:

0. [[#0. System setup|System Setup]]
1. [[#1. Creating your project|Creating your project]]
2. Building your component
3. Deploying your component
4. Using your component


## 0.   System setup

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

## 1. Creating your project

Now that we have installed all the tools above, creating the project itself is straightforward. We will be building it from a template with a few commands.

- Open Visual Studio, in the terminal **navigate to a directory** where we will be building out the component. In this case we will be building it in a folder called “PreambleTextField”.
	- If you cannot see the terminal, we can open one from the terminal menu bar.
	![1 - Creating your project - New terminal.png](https://tomkelly.uk/assets/img/Building%20a%20power%20apps%20component/1%20-%20Creating%20your%20project%20-%20New%20terminal.png)