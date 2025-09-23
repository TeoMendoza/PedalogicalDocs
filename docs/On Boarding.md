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

Step 5 - Now, lets get into some of the more interesting & technical capabilities of ASP.NET and C#. Inside the OnBoarding.razor.cs, add the following code chunk.
```csharp
using Microsoft.AspNetCore.Components;
using Microsoft.EntityFrameworkCore;
using AiTutor.Data;
#pragma warning disable CA1848

namespace AiTutor.Components.OnBoarding;

public partial class OnBoardingComponent : ComponentBase
{
    [Inject] IDbContextFactory<ApplicationDbContext> DbContextFactory { get; set; } = default!;
    [Inject] NavigationManager NavigationManager { get; set; } = default!;
    [Inject] public ILogger<OnBoardingComponent> Logger { get; set; } = default!;
}
```
In ASP.NET, using other sections of code outside of your folder scope requires direct reference/requesting of the functionality. Doing this requires a using statement. Sets of code are organized into namespaces. Essentially just a way to reference where your code is without having to reference the direct file path. Namespaces can encapsulate multiple files. So, if you wanted to access code, classes, etc, from our OnBoarding tutorial, you'd have to include a using statement referencing its namespace. The other using statements that we have included are required for making sure we do not get errors with the rest of the code chunk. In the code chunk, we define a public partial class that inherits from the ComponentBase class, a class provided by ASP.NET that allows for communicaiton between the backend and frontend file. The partial keyword ensures we can extend the class in other areas of the code, if needed. Typically it isn't needed, but it is a convention we follow. The [Inject] keywords are what we call Dependency Injection. Dependency Injection is a coding concept that essentially takes work away from the code youa re currently working, by asking for what it needs, rather than creating it interally. The inject keyword is handeled by .NET, where it in the background handles the creation & assignment of the required service. The {get;set;} keywords define basic get and set methods for the object, making it into a property. This is useful in many cases, first, it allows for reference of the variable in the .razor file, which we will demonstrate later. Additionally, it allows you define more complex/protected get and set methods easily, without having to fully flesh out a method for it. The default keyword tells .NET to initialize the varible with its default value, typically null, but not always. Regardless, .NET may complain and throw a warning, saying that the variable may be null. By adding the ! signature, we tell .NET that it won't be null by the time we use it. Now, for the actual things we are injecting, DbContextFactory is a service we use to build connections to the databse, Navigation Manager is a ASP.NET provided service that lets us navigate to different pages, and the Logger allows us to Log information for debugging and other similar purposes.

Next, go to the OnBoarding.razor file and remove the following: 
```html
<div>
    <h1>Hello New Pedalogical Worker!</h1>
</div>
```
Now, insert this code underneath the @page: 
```html
@inherits AiTutor.Components.OnBoarding.OnBoardingComponent
```
This connects the code from our .razor.cs file to our .razor file, allowing us to use the variables, methods, etc, within the backend file, in our frontend. 

Step 6 - Now, lets expand on our new page. We will now explore using actual data and how logic involving it might actually look. Lets now navigate to the Models folder, its path is AiTutor/Data/Models. In the Models folder, create a new folder called OnBoarding. Next, create a file called Company.cs and another file called Worker.cs. In the Company.cs file, copy the following code:
```csharp
namespace AiTutor.Data.Models.OnBoarding;

public class Company
{
    public int Id { get; set; }
    public string CompanyName { get; set; } = "Willamette University";
    public List<Worker> Workers { get; set; } = [];
}
```
In the Worker.cs file, copy the following code: 
```csharp
namespace AiTutor.Data.Models.OnBoarding;

public class Worker
{
    public int Id { get; set; }
    public required int CompanyId { get; set; }
    public Company? Company { get; set; }
    public required string FirstName { get; set; }
    public required string LastName { get; set; }
    public required Job Job { get; set; }
}

public enum Job
{
    ResearchAssistant,
    AssistantProfessor,
    TeachingAssistant,
    AssociateProfessor
    
}
```


What we have defined here are two classes with a relationship. In english, you could say that a company has many workers, while a single worker only has one company. This relationship is intuitively understood by you, the human, but it's not as clear to the computer, it wants to know exactly what the relationship is, to be able to throw errors if something breaks that relationship. This is why databases are called "relational". In this case, this is a One To Many Relationship. To define this, go to the ApplicationDbContext.cs file, it's path should be AiTutor/Data/ApplicationDbContext.cs. Add this code to the bottom, below all the existing code.

```csharp
public DbSet<Company> Companies => Set<Company>();
public DbSet<Worker> Workers => Set<Worker>();

modelBuilder.Entity<Company>().ToTable("Companies");
modelBuilder.Entity<Worker>().ToTable("Workers");

modelBuilder.Entity<Worker>()
    .HasOne(w => w.Company)
    .WithMany(c => c.Workers)
    .HasForeignKey(w => w.CompanyId)
    .OnDelete(DeleteBehavior.Cascade); 
```
What this code is doing is a couple important things. The DbSet allows us to work with the Worker and Company data inside our code in a special fashion using somethign called LINQ, which we will cover in later steps. The ToTable allows us to configure the table name of a class inside the database to something more intuitive. Lastly, the last section of code is how we define the One To Many relationship between Company and Worker. When defining a relationship like this, you always define it from the perspective of the dependent. In this case, the worker, because a company can have any amount of workers, it's agnostic to how many and who those workers are. However, a worker cannot exist without a company, so it is the dependent class. We strictly define that it can only have one Company, then we clarify that the same company can have other workers, defining the One to Many relationship. We then define the forgein key, which is the identifer that the Worker will have that links it to a specific company, this will allows us to grab all workers from a company easily. Lastly, we have an on delete behavior that tells the database to delete all workers under a specifc company when its deleted. This does not mean that if a worker is deleted, the comapny is deleted, it's only one direction.

Now we are done defining the classes and relationships to be used in our code! The last thing we have to do is run a migration, we ensures our changes are reflected in the database. In your terminal, run the command <code>dotnet ef migrations add OnBoardingMigration</code> then run <code>dotnet ef database update</code>

Step 7 - Now let's actually do some cool stuff with our new Worker and Company models. Let's go back to our OnBoarding.cs file we made a couple steps ago. Replace your current code with the following.
```csharp
using Microsoft.AspNetCore.Components;
using Microsoft.EntityFrameworkCore;
using AiTutor.Data;
using AiTutor.Data.Models.OnBoarding;
#pragma warning disable CA1848

namespace AiTutor.Components.OnBoarding;

public partial class OnBoardingComponent : ComponentBase
{
    [Inject] IDbContextFactory<ApplicationDbContext> DbContextFactory { get; set; } = default!;
    [Inject] NavigationManager NavigationManager { get; set; } = default!;
    [Inject] public ILogger<OnBoardingComponent> Logger { get; set; } = default!;
    [SupplyParameterFromQuery] public string QueryMessage { get; set; } = default!;
    protected Company? Company { get; set; }
    protected List<Worker> Workers { get; set; } = [];
    protected IReadOnlyList<Worker> ResearchAssistants => ShowResearchAssistants ? Workers.Where(w => w.Job == Job.ResearchAssistant).ToList() : Workers;
    protected Worker NewWorker { get; set; } = default!;
    protected bool ShowResearchAssistants { get; set; }
    protected bool ShowNewWorkerForm { get; set; }
}
```
In this, we have added a couple new things. Firstly, we have added a using statement at the top that allows us to access the Worker and Company models we made in the previous step. Additionally, we have added a query parameter. This is a value that we pass in through the url when navigating to our page, that we parse and store as a variable. It will not be particularly useful in this exercise, but it's good to know how to use it, as it is widely used for passing through Id's across pages throughout the project. Next, we define a Company variable, making it nullable. Next, we define a List of Workers that will be the Workers from our company. We also define another list of Workers that uses LINQ, which stands for language integrated query, a nicely defined set of commands that help us query our data, both in memory and from the database. In this LINQ, we use a Where command, which is a filter, with the parameter being our filter condition, in this case, it takes in the paramter w, which is a Worker, then we check whether the workers Job is equal to a Research Assistant. Then we turn that all into a List. We also define another Worker called NewWorker which will let us add new workers to our company. We also define some booleans that we will use for within the html. 

Now, add this code to the bottom of the file (within the Class)

```csharp
protected override async Task OnInitializedAsync()
    {
        Logger.LogInformation("Hello new Pedalogical worker! QueryMessage = {QueryMessage}", QueryMessage);

        using var DbContext = DbContextFactory.CreateDbContext();

        Company = await DbContext.Companies.Include(c => c.Workers).FirstOrDefaultAsync();

        if (Company is null)
        {
            Company = new();
            DbContext.Companies.Add(Company);
            await DbContext.SaveChangesAsync();

            await AddCurrentStaff(DbContext, Company);

            Company = await DbContext.Companies.Include(c => c.Workers).FirstAsync() ?? throw new InvalidOperationException("Company Must Exist");
        }

        Workers = Company.Workers;

        NewWorker = new()
        {
            CompanyId = Company.Id,
            FirstName = "Temporary First Name",
            LastName = "Temporary Last Name",
            Job = Job.AssistantProfessor
        };
    }
```




