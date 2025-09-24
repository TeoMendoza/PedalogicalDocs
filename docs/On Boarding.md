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

Before we talk about what's inside the function, it's important we talk about the function itself, since it's special. OnInitializedAsync is a special function signature recognized by .NET that runs first, when the page loads. It is typically used in cases where you want to load some data or do some modifications to data before showing it to users. The async part of the function is another important part of it. The OnInitialized function can be without the Async part, but they have different purposes. All it means is that there are asynchronous operations inside the OnInitialized function. But what does async even mean? An asynchronous operation is an operation that doesn't block the main thread. The main thread is essentially what allows for UI rendering and processing. If you have an operation that takes time, like a database request, you typically want it to be asynchronous, that way the application isn't frozen while it's going to the database to gather information. Additionally, asyncrhonous requests can also be awaited. What this means is that the code waits for the operation to complete before continuing. This is extremeley powerful and useful in many contexts involving database requests.

Inside our OnInitialized function, we first Log our query message, this isn't relavent other than to show you that what you passed in through the url when navigating gets parsed and stored into the variable. Next, we begin working with the database. First, we establish a connection to the database using the DbContextFactory. This gets a current snapshot of the database and the data it has the current moment. Next, we ask the database to give us some information using our await keyword. All database operations should be asynchronous, that is a pattern that is standard and scalable. What we are requesting is to look at the Companies table and grab the first company it can find. If it can find the company, it returns null (this is the default part of the method signature). Additionally, from the first one, we use a .Include to also include the companies workers. This is not magic however, it doesn't just know which workers are in the company. It does this using the defined relationship from the previous step. The company has an Id value defined, a unique identifier and primary key. By giving each worker in the company this Id under the CompanyId field, the database can look through the workers table and grab all workers with the associated Company Id, this is why it is so powerful, we skip so much unecessary logic with a little bit of pre-measures. This can scaffold down into multiple sub includes, making it even more powerful in many cases! Next, because we haven't actually put any company into the database yet, the whole database call will return null. In this case, we want to insert a Company into the database. So we first check if Company is null and then instantiate a new one, add it to the Companies table, and then save our changes asynchronously once again. Then we call a function to add some current staff (which we will define soon, it will say it doesn't know what that func is which is fine for now). Once we have presumably added our staff, we reload our Company which should now exist in the database! We also make sure to catch it with an exception, to ensure that if anything went wrong we know, since after adding it to DB, there should be no reason why it doesn't load. Next we set our Workers variable to the Companies Workers, just so referencing them is easier. We also create a New Worker that has some filler data, so that when we hook it up to the front end, users can create a new worker that links to this variable. 

Now, lets add the last bit of our backend code

```csharp
protected async Task AddWorkerToCompany(Worker Worker)
    {
        if (string.IsNullOrWhiteSpace(Worker.FirstName) || string.IsNullOrWhiteSpace(Worker.LastName))
        {
            Logger.LogWarning("Rejected add: worker must have first and last name.");
            return;
        }

        using var DbContext = DbContextFactory.CreateDbContext();

        DbContext.Workers.Add(Worker);
        await DbContext.SaveChangesAsync();

        Workers.Add(Worker);
        StateHasChanged();

        NewWorker = CreateBlankWorker(Company!.Id);
        ShowNewWorkerForm = false;
    }

    private static async Task AddCurrentStaff(ApplicationDbContext DbContext, Company Company)
    {
        Worker Lucas = new()
        {
            CompanyId = Company.Id,
            FirstName = "Lucas",
            LastName = "Cordova",
            Job = Job.AssistantProfessor
        };

        Worker Teo = new()
        {
            CompanyId = Company.Id,
            FirstName = "Teo",
            LastName = "Mendoza",
            Job = Job.ResearchAssistant
        };

        Worker Ben = new()
        {
            CompanyId = Company.Id,
            FirstName = "Ben",
            LastName = "Webster",
            Job = Job.ResearchAssistant
        };

        DbContext.Workers.AddRange(Lucas, Teo, Ben);
        await DbContext.SaveChangesAsync();
    }

    protected void ToggleShowResearchAssistants()
    {
        ShowResearchAssistants = !ShowResearchAssistants;
    }

    private static Worker CreateBlankWorker(int CompanyId) => new()
    {
        CompanyId = CompanyId,
        FirstName = "Temporary First Name",
        LastName = "Temporary Last Name",
        Job = Job.AssistantProfessor
    };

    protected void NavigateHome() => NavigationManager.NavigateTo("/");
```

Add this code within our class, underneath all our existing code. Now there's quite alot here, but you'll see alot of similarities and repeated chunks to some degree. Our first function, AddWorkerToCompany, takes in a parameter Worker, confirms that it has the data we are requiring, which is a First and Last Name, and if it does, we add that worker to the database, save our changes, and reinitialize our Worker to a blank worker. We also reset the ShowNewWorkerForm boolean, this will make sense when we go to the front end. We then have our AddCurrentStaff function which we called in OnInitialized, it initiliazes three workers that we have predefined, and then adds them all at once to the database and saves changes. Note: The add range function is a special form of an add that can add multiple things in one operation. It is preferred when you have to add multiple things to the same table, because instead of forming a new request and inserting for each worker, it forms one request and adds them all at once. We then have a ToggleShowResearchAssistants function, we just toggles our boolean variable, this will be used on the front end to allow the user to filter all the workers to only the research assistants if they would like. If you go back to our ResearchAssistants variable, you will see the ShowResearchAssistants boolean with a question mark and a colon. This is called a ternary operation. Think of it like so: If ShowResearchAssistants (this is asking whether its true, !ShowResearchAssistants would ask if false), then ResearchAssistants equal our filtered group with the where command. Otherwise, ResearchAsisstants equal our normal workers list. This is a simplified but powerful if else statement. You may ask why this isn't used more, a ternary operation MUST return a value, it cannot be used to call a function or anything of the sort, it has to be giving back something to work with. Our next function is just our simple create blank worker function, that takes in a company Id and returns a boiler plate worker. You may notice, in our OnInitialized function, we do this manually, but at this point, we can replace the manual code with our function if we would like! No need to, just so you see that we can make our code more organized if we would like, something we typically would reccomend as you develop more. Lastly, we have our Navigate Home Function. You may be asking what the => signature means, this is a signature that allows for a function to be simplified only if it will have one line. In this case, all we want is to navigate back to the home page, so we can use our => signature. Now, our backend code is done!

Step 8 - Quick step, before we do our front end, lets just fill out our OnBoarding.css file. Css is just styling classes that we can use to make the thigns we show our users pretty. As a rule of the project, we have different levels of css attempting to have shared css at more general levels of the project so that we don't have to repeat css styles. However, it can be difficult to follow while in development, so it is something we are not 100% strict with, and try to do once the actual development is done. Add this to your .css file

```css
.onboarding {
  max-width: 960px;
  margin: 2rem auto;
  padding: 0 1rem;
}

.header {
  display: flex;
  align-items: flex-end;
  justify-content: space-between;
  gap: 1rem;
  margin-bottom: 1.25rem;
  flex-wrap: wrap;
}

.title {
  font-size: 1.75rem;
  line-height: 1.2;
  margin: 0;
}

.subtitle {
  margin: .25rem 0 0 0;
  color: #667085;
  font-size: .95rem;
}

.toolbar {
  display: flex;
  gap: .5rem;
  align-items: center;
}

.btn {
  appearance: none;
  border: 1px solid #d0d5dd;
  background: #fff;
  color: #344054;
  padding: .5rem .85rem;
  border-radius: .6rem;
  font-size: .95rem;
  cursor: pointer;
  transition: background .15s ease, border-color .15s ease, transform .03s ease;
}
.btn:hover { background: #f9fafb; border-color: #c7ced6; }
.btn:active { transform: translateY(1px); }

.btn-primary {
  background: #1f6feb;
  color: #fff;
  border-color: #1f6feb;
}
.btn-primary:hover { background: #175bd0; border-color: #175bd0; }

.btn-ghost {
  background: transparent;
  border-color: transparent;
  color: #475467;
}
.btn-ghost:hover { background: #f3f4f6; border-color: #e5e7eb; }

.card {
  background: #fff;
  border: 1px solid #e5e7eb;
  border-radius: .8rem;
  box-shadow: 0 1px 2px rgba(16, 24, 40, .04);
  margin-bottom: 1rem;
}

.card-header {
  padding: .9rem 1rem .5rem 1rem;
  border-bottom: 1px solid #eef2f6;
}

.section-title {
  margin: 0;
  font-size: 1.1rem;
  color: #111827;
  display: flex;
  align-items: center;
  gap: .5rem;
}

.count {
  font-size: .9rem;
  color: #667085;
}

.table-wrap {
  width: 100%;
  overflow-x: auto;
}

.table {
  width: 100%;
  border-collapse: collapse;
  font-size: .95rem;
}

.table th,
.table td {
  text-align: left;
  padding: .75rem 1rem;
  border-bottom: 1px solid #eef2f6;
  white-space: nowrap;
}

.table thead th {
  color: #475467;
  font-weight: 600;
  background: #f8fafc;
}

.empty {
  padding: 1rem;
  color: #667085;
}

.form-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
  gap: .9rem;
  padding: 1rem;
}

.form-field {
  display: flex;
  flex-direction: column;
  gap: .35rem;
}

.form-field label {
  font-size: .9rem;
  color: #475467;
}

.input {
  border: 1px solid #d0d5dd;
  border-radius: .6rem;
  padding: .55rem .7rem;
  font-size: .95rem;
  background: #fff;
  color: #111827;
}
.input:focus {
  outline: none;
  border-color: #1f6feb;
  box-shadow: 0 0 0 3px rgba(31, 111, 235, .1);
}

.form-actions {
  display: flex;
  gap: .5rem;
  padding: 0 1rem 1rem 1rem;
}
```

Step 9 - Almost done! Since this is a backend focused exercise, we will not go through anything past the basics of front end development. These are things we are confident you can learn on your own (much of the project will have things you can look at for reference!). Replace your entire OnBoarding.razor.cs file with the following code.

```html

@page "/OnBoarding"
@inherits AiTutor.Components.OnBoarding.OnBoardingComponent

@using Job = AiTutor.Data.Models.OnBoarding.Job
@using Worker = AiTutor.Data.Models.OnBoarding.Worker

<div class="onboarding">
    <header class="header">
        <div>
            <h1 class="title">@(Company?.CompanyName ?? "Company")</h1>
            @if (!string.IsNullOrWhiteSpace(QueryMessage))
            {
                <p class="subtitle">Message: @QueryMessage</p>
            }
        </div>

        <div class="toolbar">
            <button class="btn" @onclick="ToggleShowResearchAssistants">
                @(ShowResearchAssistants ? "Show All Workers" : "Show Only Research Assistants")
            </button>

            <button class="btn btn-primary" @onclick="@(() => ShowNewWorkerForm = !ShowNewWorkerForm)">
                @(ShowNewWorkerForm ? "Close New Worker Form" : "New Worker")
            </button>

            <button class="btn btn-ghost" @onclick="NavigateHome">Home</button>
        </div>
    </header>

    <section class="card">
        <div class="card-header">
            <h2 class="section-title">
                @(ShowResearchAssistants ? "Research Assistants" : "All Workers")
                <span class="count">(@ResearchAssistants.Count)</span>
            </h2>
        </div>

        @if (ResearchAssistants.Count == 0)
        {
            <div class="empty">
                <p>No workers to display.</p>
            </div>
        }
        else
        {
            <div class="table-wrap">
                <table class="table">
                    <thead>
                        <tr>
                            <th>First</th>
                            <th>Last</th>
                            <th>Job</th>
                        </tr>
                    </thead>

                    <tbody>
                        @foreach (Worker Worker in ResearchAssistants)
                        {
                            <tr>
                                <td>@Worker.FirstName</td>
                                <td>@Worker.LastName</td>
                                <td>@Worker.Job</td>
                            </tr>
                        }
                    </tbody>
                </table>
            </div>
        }
        </section>

    @if (ShowNewWorkerForm)
    {
        <section class="card">
            <div class="card-header">
                <h2 class="section-title">Create New Worker</h2>
            </div>

            <EditForm Model="NewWorker" OnValidSubmit="@(() => AddWorkerToCompany(NewWorker))">
                <div class="form-grid">
                    <div class="form-field">
                        <label for="first">First Name</label>
                        <InputText id="first" class="input" @bind-Value="NewWorker.FirstName"/>
                    </div>

                    <div class="form-field">
                        <label for="last">Last Name</label>
                        <InputText id="last" class="input" @bind-Value="NewWorker.LastName"/>
                    </div>

                    <div class="form-field">
                        <label for="job">Job</label>
                        <InputSelect id="job" class="input" @bind-Value="NewWorker.Job">
                            @foreach (Job job in Enum.GetValues<Job>())
                            {
                                <option value="@job">@job</option>
                            }
                        </InputSelect>
                    </div>
                </div>

                <div class="form-actions">
                    <button type="submit" class="btn btn-primary">Add Worker</button>
                    <button type="button" class="btn" @onclick="@(() => ShowNewWorkerForm = false)">Cancel</button>
                </div>
            </EditForm>
        </section>
    }
</div>
```

There is alot going on here, so let's highlight the important things. Firstly, you are able to write C# code in your front end file. You do this by signaling it with an @ symbol. Next, we have lots of buttons in this page, and you may be wondering how we link them to our functions we have written. We do this using the @onclick handler. You can do two things with this, you can link it either to a function in your backend, or a lambda function you define on the fly. Next, we have an EditForm tag, provided by ASP.NET, which lets us handle submitting and filling out forms or other similar things. We can define what to do On Submit, and then populate inside the form what we want to show. We can then link a button to be the button to trigger the submit, with the type = "submit" inside the button html. Additionally, We also use the @bind-Value tag. What this does is allow for variables to link from user input to our backend variables. As an example, we bind something like the First Name in the EditForm, allowing for when the user types in a first name, it goes and links to our actual FirstName variable inside our Worker. This is super helpful so that you don't need multiple variables, one for front end one in backend, and then having to link them. Those are the primary things that we wanted to cover in the front end. Most of the rest is just html and C# code that you should be relatively familiar with already. If you are unfamiliar with C#, the code in this page and other pages give many examples for how to use C#. Additionally, there is additional C# documentation that is relavent to the project that you may want to review/check out when done with this tutorial. Once you have done that, in the terminal, run <code>dotnet run</code> in the terminal, and try navigating to your OnBoarding page. See if what you thought was going to happen is what happened when interacting with the page! If not, try to look back and understand why!

Step 10 - Quick Step! Review previous steps if you are still confused have questions, try to link everything you've learned and fully understand how everything works together one last time before our last step!

Step 11 - Last step! In this step, we will learn how to merge your code between the branches we made at the beginning. First of all, make sure you push and commit all of the things we've done, either through github desktop or the command line. From here, go to your specific section to see how to merge your code.

Github Desktop - Okay, in github desktop, there should be a big blue button that says create pull request. Click it, it should open a tab in google or whatever browser you work in, that directs to Github, log in if needed.

Command Line

