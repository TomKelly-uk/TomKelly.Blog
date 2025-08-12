## Microsoft Graph API Made Simple: Accessing Entra in Power Automate

Power Automate has over 1,000 connectors, so you'd think you can access any data you need. Right?

Well, not always. While Power Automate is incredibly powerful, it's easy to forget that we're working with cutting edge technology, and that comes with drawbacks. Sometimes the built in connectors don’t expose the specific data you need, or the connector simply doesn't exist yet. That’s where APIs come in.

In this guide, I'll show you how to bypass those limitations using Microsoft Graph API to access user data from Entra ID. You don’t need any prior experience with APIs we'll walk through each step together.

1. [What is an API?] (#WhatIsAPI) 
2. [Register your app in Entra] (#RegisterApp)
3. [Create our flow] (#CreateFlow)
4. [Configure our HTTP request] (#ConfigureRequest)
5. [Parse our output] (#ParseOutput)
6. [Conclusion] (#Conclusion)

### What is an API? {#WhatIsAPI}

An API is a Application Programming Interface and it is a set of definitions and protocols that enable two different applications to talk to eachother, it enables data to be taken from one system and securely consumed in another. For example, our Weather apps don't store the actual weather informaton, it uses an API to request the weather information from a centralised weather provider.

In our example, Graph API is Microsofts API that lets us access data across  their apps, such as Entra, Teams, Sharepoint etc. We will be using this API to get user data from Entra, such as account information. 

### Register your app in Entra {#RegisterApp}
