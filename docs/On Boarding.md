Welcome to the Pedalogical On Boarding Document! By following the documentation, you will familiarze yourself with the Pedalogical codebase & conventions, C# coding, the ASP.NET Framework, PostgreSQL, and Git branching & merging! 

Intro/Overview Section - Pedalogical uses the ASP.NET Web Development Framework. What this means is that a pre-built base set of functionalities built for web development are provided to us when we created the project. These functionalities make communicating between Front-end & Back-end functionality much simpler and fast. If you are unfamiliar with the terms, front-end refers to the things that users see and interact with, typically HTML and CSS. Back-end refers to the logic that goes on behind the scenes to make sure that what users are seeing is up to date.

Git - Pedalogical uses Git Version Control to manage the project. Git provides many helpful tools for making sure the project stays organized and that development is seperate from a deployed version of the project. Typically, projects will have branches. Branches are different versions of the codebase. The "main" branch is the version of the code that is deployed, meaning what users interact with. Additionally, there are usually Development branches, which are branches where new features are added, tested, etc. In Pedalogical, we use a main branch for our deployed version, and a single development branch for new changes. However, since there are multiple people typically working on the project, it is not possible for everyone to use the Development branch, due to the issues with differences in code as development happens. Because of this, the development branch is protected, meaning you cannot push your changes directly to Development. Instead, you must make a branch off of Development, and do your work there. But this raises the question, how do we combine all our changes into Dev, and then eventually into main? To accomplish this, we use merging. Merging is a functionality that allows developers to combine their code. Merging however is not as simple as clicking a button, more often than not, there are merge conflicts. Merge conflicts arise when there are more than one set of changes to the same part of code, in this scenario, git doesn't know which to take (or to take both), so it's up to the developer to resolve these issues. THis can be confusing, so let's move to an example to illustrate when a merge conflict may happen. 

Merge Conflict - Say we have 2 Pedalogical Developers, Teo and Ben. Teo is tasked on fixing a bug with the Home page, while Ben is tasked on cleaning up the code for the Home page. Both of these tasks require Teo and Ben working in the same area in the codebase. To begin, both Teo and Ben create new branches from the Development branch. Eventually, Teo finishes his task first, so he merges his code back into Development. Now, since Ben hasn't merged his code yet, Development hasn't been updated to be different from where Teo branched off of. As a result, there are no merge conflicts, Git essentially just takes Teo's changes and updates them in Dev. However, now when Ben tries to merge, Git raises a merge conflict, because it can't just take Ben's code and put it onto Development, since Teo has put some new code that Ben's branch doesn't have. So, before merging, Ben must go through the conflicts, and manually tell Git what to do with the different versions of code. Typically, per conflict, you have a couple options: accepting 1 of the 2 versions of code, accepting both versions of the code, or doing one of the previous options and manually updating the code in the case where there needs some code written to make sure things work. After all merge conflicts have been resolved, the code can be merged into Development. Now, this does not necessarily mean the code works, but there are no conflicts from Git's perspective. Luckily, our Pedalogical Development branch is protected, so before you can push the merged changes, it must build (compile).

Making A New Branch - Now that you have a basic understanding of everything, let's try getting some hands on expirience. Because merging can be extremeley complicated and error prone, this on boarding exercise will not require you to solve any merge conflicts. It will require you merge, but it will be very smooth and easy. However, do be aware that merging is not always going to be easy. To avoid having to solve merge conflicts, communicate with the Team and try to avoid working in the same areas of the codebase, although it is inevitable, merge conflicts are preventable is many cases, especially with a smaller team. 

To begin, first identify whether you are using Git through the command line or through Github Desktop. Either works, although it is reccomended that you use Github Desktop. Github Desktop is extremeley helpful for simple Git tasks and general organization & representation of changes to the repository. The command line becomes helpful for more complex tasks involving Git, which we will not cover today, and you likely will not run into often. 

Step 1 (Github Desktop) - First, create a new branch of from Development. On Github Desktop, click the Current Branch Tab, which should open a dropdown. Select the Development branch to switch the the development branch. Now go back to the Current Branch Tab and open the dropdown once again. In the top right, it should say new branch, click that. Then name your branch OnBoarding-*YourName*. Before clicking create, make sure that you select to branch of from Development, NOT main. Once created, click the Publish Branch Button. This publishes the branch to the remote repository, which means that others can access the branch aswell, and you can merge your code.

Step 1 (Command Line) - First, run this command to switch to the Development Branch: <code>git checkout Development</code> in the command line. Make sure you are already inside the AiTutor Repository (use cd and the file path to get there if you aren't) or else the command won't work. Next, run this command to make your new branch: <code>git checkout -b OnBoarding-*YourName*</code>.  Lastly, run this command To publish the branch to the remote repository: <code>git push -u origin OnBoarding-*YourName*</code> 

Step 2 (Github Desktop & Command Line) - At this point, if everything was done correctly, you should be currently on your new branch. Next, repeat the process, creating a new branch called OnBoarding-*YourName*-Second. Make sure to create this branch from the branch you previously created, NOT Development.

Step 3 - From this point, the instructions are majority agnostic to whether you are using Github Desktop or the Command Line. We will now begin working in the codebase. First, identify the *Components* folder within the project, it's path should be AiTutor/Components. Within this folder, create a new folder called OnBoarding. Next, create three files within the OnBoarding folder: OnBoarding.razor, OnBoarding.razor.cs, OnBoarding.razor.css. Once you have made these files, confirm everything with your project is working by running the command dotnet build. If the project builds, which it should, we are good to continue. 

Step 4 - Lets begin understanding how the ASP.NET framework works, namely how to go from code to viewable web pages. Inside your OnBoarding.razor file, add the following code.
```html
@page "/OnBoarding"

<div>
    <h1>Hello New Pedalogical Worker!</h1>
</div>
```
Now, navigate to the Home.razor file. It's path should be AiTutor/Components/Pages/HomeFolder/Home.razor. Scroll down to the bottom of the page, and add the following code.
```html
<a class="btn btn-primary" href="/OnBoarding">On Boarding Page</a>
```
This will allow us to navigate, view, and access the page we created in the previous step, and view our message. Try it out!

Step 5 - 






