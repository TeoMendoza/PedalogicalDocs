# Welcome to the Pedalogical Onboarding Document

By following this guide, you will familiarize yourself with the Pedalogical codebase and its conventions, C# coding, the ASP.NET framework, PostgreSQL, and Git branching and merging.

---

## Intro / Overview

Pedalogical is built on the ASP.NET web development framework. This framework provides a robust set of pre-built functionalities designed for building modern web applications. These built-in tools simplify communication between the front end and the back end of the project, allowing us to develop features more quickly and reliably.

If you are new to the terminology:
- **Front end** refers to everything users see and interact with directly (typically written in HTML, CSS, and JavaScript).  
- **Back end** refers to the application logic running behind the scenes to process data and ensure the information displayed on the front end is up to date.

---

## Git

Pedalogical uses Git version control to manage its codebase. Git helps us keep the project organized and ensures that development can proceed without interfering with the deployed version.

Git projects are typically organized into **branches**. A branch is a version of the codebase. The `main` branch represents the deployed version of Pedalogical, the one users interact with. In addition, there is usually a **development branch**, where new features are added and tested.

At Pedalogical, we use:
- A `main` branch for production (deployed code).  
- A `development` branch for ongoing work.  

Because multiple developers work on the project simultaneously, we cannot all work directly on the development branch. If everyone pushed changes there, the codebase would quickly become unstable. To prevent this, the development branch is **protected**, meaning you cannot push changes to it directly. Instead, you create a **feature branch** from development, do your work there, and later merge it back into development. This raises the question: how do we combine all of our changes into development, and eventually into main? The answer is **merging**.

Merging is the process of combining code from one branch into another. While this can sometimes be straightforward, it often leads to **merge conflicts**. A merge conflict happens when two branches contain changes to the same part of the code, and Git cannot automatically decide which version to keep. In these cases, it is up to the developer to resolve the conflict.

---

## Merge Conflicts

Here is a practical example:  

Suppose we have two developers, **Teo** and **Ben**.  
- Teo is fixing a bug on the Home page.  
- Ben is cleaning up the code for the Home page.  

Both developers create branches from the development branch. Teo finishes first and merges his changes into development. Because Ben has not merged yet, development has no conflicts, Git simply applies Teo’s changes.

Later, when Ben attempts to merge his branch, Git detects conflicts. This is because Teo’s changes have already modified parts of the code that Ben also worked on. Git does not know how to combine them automatically. Before the merge can succeed, Ben must resolve these conflicts by choosing one version, keeping both, or making manual edits to ensure everything works together. Once the conflicts are resolved, the branch can merge into development.  

Note: a successful merge only means there are no Git-level conflicts. It does not guarantee that the application works as expected. That is why our development branch is protected, before merged changes can be accepted, the project must compile successfully.

---

## Making a New Branch

Now that you understand the basics, let us get some hands-on experience.  

This onboarding exercise will have you create and merge a branch, but without dealing with merge conflicts. In practice, conflicts can be complex and error-prone, so this simplified workflow will let you get comfortable with the process before encountering them in real work.  

To reduce the chances of merge conflicts in the future, always communicate with your team about what areas of the code you are working on. While conflicts are inevitable in larger projects, they can often be avoided in smaller teams through good coordination.

---

## Git Tools

You can use either **GitHub Desktop** or the **command line** for Git operations. Both work, though we recommend GitHub Desktop for onboarding. GitHub Desktop provides a clean interface for common tasks like committing and merging, making it easier to visualize your changes. The command line is more powerful and flexible, but typically only necessary for advanced Git operations, which you are unlikely to need early on.


## Step 1 (GitHub Desktop)

First, create a new branch from Development. In GitHub Desktop, click the Current Branch tab, which will open a dropdown. Select the Development branch to switch to it.  

Next, go back to the Current Branch tab and open the dropdown again. In the top right, you will see an option for New Branch. Click that. Name your branch `OnBoarding-YourName`.  

Before clicking Create Branch, make sure that you select Development as the branch to base your new branch on, not main.  

Once the branch is created, click the Publish Branch button. This publishes the branch to the remote repository, which allows others to access it and enables you to merge your code later.  

## Step 1 (Command Line)

First, switch to the Development branch by running this command in the terminal:

```bash
git checkout Development
```
Make sure you are already inside the AiTutor repository (use cd and the file path to get there if you are not), otherwise the command will not work.
Next, create a new branch with this command:

```bash
git checkout -b OnBoarding-YourName
```

Finally, publish your new branch to the remote repository with this command:

```bash
git push -u origin OnBoarding-YourName
```

--- 

## Step 2 (GitHub Desktop and Command Line)

At this point, if everything was done correctly, you should already be on your new branch. Now repeat the process, creating another branch called `OnBoarding-YourName-Second`. Make sure to create this branch from the branch you just created (`OnBoarding-YourName`), not from Development.  

---

## Step 3

From this point forward, the instructions apply whether you are using GitHub Desktop or the command line.  

We will now begin working in the codebase. First, locate the `Components` folder within the project. Its path should be `AiTutor/Components`.  

Inside the `Components` folder, create a new folder named `OnBoarding`.  

Within the `OnBoarding` folder, create the following three files: `OnBoarding.razor`, `OnBoarding.razor.cs`, and `OnBoarding.razor.css`.  

Once these files are created, confirm that your project is working correctly by running the build command:

```bash
dotnet build
```
If the project builds successfully (which it should), you are ready to continue.

---

## Step 4

Now let’s begin understanding how the ASP.NET framework works, specifically how to go from code to a viewable web page.  

Inside your `OnBoarding.razor` file, add the following code:

```html
@page "/OnBoarding"

<div>
    <h1>Hello New Pedalogical Worker!</h1>
</div>
```

Next, navigate to the `Home.razor` file. Its path should be `AiTutor/Components/Pages/HomeFolder/Home.razor`. Scroll to the bottom of the file and add this code:
```html
<a class="btn btn-primary" href="/OnBoarding">On Boarding Page</a>
```

This creates a navigation link to the page you just built, allowing you to view and access your onboarding page. Save your changes and try it out to confirm that everything is working.

---

## Step 5

Now let us get into some of the more technical capabilities of ASP.NET and C#. Inside the `OnBoarding.razor.cs` file, add the following code:

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

In ASP.NET, using code that lives outside your current folder scope requires explicitly referencing it. You do this with a `using` statement. Code is organized into `namespace`s, which are labels that group related files. Namespaces let you reference code, classes, and methods without specifying a direct file path.

The `using` statements at the top of the file import the namespaces required for this class to compile.

In the code above, we define a `public partial class` that inherits from the `ComponentBase` class. `ComponentBase` is provided by ASP.NET and enables communication between backend logic and the frontend Razor file. The `partial` keyword allows the class definition to be split across multiple files if needed. While this is not always necessary, it is a convention we follow in Pedalogical.

The `[Inject]` attributes demonstrate **dependency injection**. Dependency injection is a pattern where external services are provided by the framework, rather than being created manually inside your class. The .NET runtime creates and assigns these services for you.

The `{ get; set; }` syntax defines properties, which allow values to be accessed and updated. Properties can be referenced directly in the Razor file, and you can add more complex logic later if needed.

The `default` keyword initializes the variable with its default value, often null. Since .NET may warn about possible null values, the `!` operator tells the compiler that the value will not be null at runtime.

Here is what the injected services do in this example:
- `DbContextFactory` creates database contexts so you can talk to the database.
- `NavigationManager` is an ASP.NET service that lets you navigate between pages.
- `Logger` is used for logging, which helps with debugging and monitoring.

---

Next, go to the `OnBoarding.razor` file and remove the following code:

```html
<div>
    <h1>Hello New Pedalogical Worker!</h1>
</div>
```

Replace it with this line, inserted directly beneath the `@page` directive:

```html
@inherits AiTutor.Components.OnBoarding.OnBoardingComponent
```

This connects your `.razor.cs` file to your `.razor` file, which allows you to use the variables, methods, and services defined in the backend file inside the frontend file.

---

## Step 6

Now let us expand on our new page. We will start using actual data and see what logic involving it might look like.  

Navigate to the `AiTutor/Data/Models` folder. Inside the `Models` folder, create a new folder called `OnBoarding`. Next, create two files: `Company.cs` and `Worker.cs`.  

In the `Company.cs` file, add the following code:

```csharp
namespace AiTutor.Data.Models.OnBoarding;

public class Company
{
    public int Id { get; set; }
    public string CompanyName { get; set; } = "Willamette University";
    public List<Worker> Workers { get; set; } = [];
}
```

In the `Worker.cs` file, add the following code:

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

What we have defined here are two classes with a relationship. In plain terms, a company has many workers, while a single worker only has one company. Humans understand this intuitively, but the computer requires explicit instructions about these relationships so it can enforce them and throw errors when the rules are broken. This is why databases are called relational.  

In this case, the relationship is **One to Many**. To define this relationship, go to the `ApplicationDbContext.cs` file located at `AiTutor/Data/ApplicationDbContext.cs`. Add the following code to the bottom of the file, beneath the existing code:

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

This code does three important things:  

1. The `DbSet` lines allow us to work with `Company` and `Worker` data inside our code using LINQ, which we will cover in later steps.  
2. The `ToTable` lines configure the table names in the database so they are more intuitive.  
3. The `HasOne`...`WithMany`...`HasForeignKey` block defines the One to Many relationship between `Company` and `Worker`.  

When defining relationships, you always define them from the perspective of the dependent. In this case, the dependent is the `Worker`, because a company can exist with any number of workers, but a worker cannot exist without a company. We strictly define that a worker can only have one `Company`, while a `Company` can have many `Workers`. The `HasForeignKey` defines the link a worker must have to its company, which allows us to easily retrieve all workers for a given company.  

Finally, the `OnDelete(DeleteBehavior.Cascade)` ensures that when a company is deleted, all workers under that company are also deleted. This behavior is one-directional: if a worker is deleted, the company remains.  

Now that the classes and relationships are defined, the last step is to run a migration so the database reflects these changes. In your terminal, run the following commands:

```bash
dotnet ef migrations add OnBoardingMigration
```

Then update the database:

```bash
dotnet ef database update
```

---

## Step 7

Now let us do something useful with the new `Worker` and `Company` models. Open the code behind file for your onboarding component, which is `OnBoarding.razor.cs`. Replace its current contents with the following:

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

What changed here:

- Added `using AiTutor.Data.Models.OnBoarding;` so this file can reference the `Company` and `Worker` models created in Step 6.  
- Added a query parameter `QueryMessage` with `[SupplyParameterFromQuery]`. This lets you pass values through the page URL and have them bound to the property. This pattern is common for passing identifiers across pages.  
- Declared a nullable `Company` and a list of `Workers`.  
- Declared a computed property `ResearchAssistants` that filters `Workers` using LINQ. `Where` applies a filter, and `ToList()` materializes the result into a list.  
- Added a `NewWorker` placeholder that will be used for adding a worker from the UI.  
- Added two booleans that the UI will use for conditional rendering and filtering.  

Now add the following method to the bottom of the same class:

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

        Company = await DbContext.Companies
            .Include(c => c.Workers)
            .FirstAsync() 
            ?? throw new InvalidOperationException("Company must exist");
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

About this method:

- `OnInitializedAsync` is a lifecycle method that Blazor calls when the component is initialized. Use it to load data that the page needs before rendering.  
- It is `async` because it performs asynchronous work, such as database calls. Asynchronous operations prevent blocking the main thread, which keeps the UI responsive.  
- It logs the incoming `QueryMessage` to show how URL bound parameters are received.  
- It creates a scoped `DbContext` via `DbContextFactory`. Each context instance represents a snapshot of the database state for the duration of that scope.  
- It tries to load the first `Company` and includes its `Workers`. The `Include` uses the relationship defined in Step 6, so `Company.Workers` is populated.  
- If no company exists yet, it creates one, saves it, seeds initial staff with `AddCurrentStaff`, then reloads the company with workers and throws an exception if the company still cannot be found.  
- It sets the `Workers` list for easier access and prepares a `NewWorker` placeholder for the UI binding in a later step.  

Now add the remaining backend helpers beneath the previous method, still inside the same class:

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

What these helpers do:

- `AddWorkerToCompany` validates input, adds the new `Worker` to the database, saves changes, updates the local `Workers` list, triggers a UI refresh with `StateHasChanged`, resets `NewWorker` using `CreateBlankWorker`, and hides the new worker form.  
- `AddCurrentStaff` seeds a few initial `Worker` rows for the first `Company`. It uses `AddRange` to batch the insert in one operation.  
- `ToggleShowResearchAssistants` flips the filter flag. The `ResearchAssistants` computed property uses a ternary expression to return either the filtered list or the full list.  
- `CreateBlankWorker` returns a boilerplate `Worker` tied to a given `CompanyId`. This helps keep initialization consistent.  
- `NavigateHome` uses `NavigationManager` to return to the home page. The `=>` syntax is an expression body, which is a concise form for single line methods.  

---

## Step 8

Before we move on to the front end, let us set up some styling for the onboarding page. CSS is used to style HTML elements and make them visually appealing for users.  

In this project, we follow a convention of having shared CSS at higher levels of the project to avoid repeating styles across components. However, during development this can be difficult to manage, so it is not enforced strictly until later in the process.  

Add the following to your `OnBoarding.razor.css` file:

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

This stylesheet provides utility classes for layout, typography, buttons, cards, tables, and forms. You will use these styles in the next steps when building the front end of the onboarding page.


---

## Step 9

Almost done. Since this is a backend focused exercise, we will only cover the basics of front end development here. You can learn more by exploring existing pages in the project for reference.

**Important correction:** replace the contents of your `OnBoarding.razor` file, not the `.razor.cs` file, with the markup below.

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
                        <InputText id="first" class="input" @bind-Value="NewWorker.FirstName" />
                    </div>

                    <div class="form-field">
                        <label for="last">Last Name</label>
                        <InputText id="last" class="input" @bind-Value="NewWorker.LastName" />
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

Key ideas in this markup:

- You can write C# directly in a Razor page by prefixing expressions with `@`.  
- Event handlers such as `@onclick` link UI elements to methods in your component class. You can reference an existing method like `ToggleShowResearchAssistants`, or use a lambda like `@(() => ShowNewWorkerForm = !ShowNewWorkerForm)`.  
- `EditForm` is provided by ASP.NET, and it handles validation and form submission. Use `OnValidSubmit` to specify what should happen when the form is valid and submitted.  
- `@bind-Value` creates two way binding between inputs and your component properties, for example binding `NewWorker.FirstName` so user input updates the model automatically.

When you are ready to test, run the app and navigate to your onboarding page:

```bash
dotnet run
```

Open the browser to the app’s base URL, then go to `/OnBoarding`. Interact with the page, add a worker, and toggle the research assistant filter. If the result is not what you expected, review the steps and code to understand why, then iterate.


---

## Step 10

Quick step. If anything is unclear, review the previous steps and your code. Try to connect each concept you have learned, and make sure you understand how they work together, from models and relationships, to data access with `ApplicationDbContext`, to dependency injection with `[Inject]`, to component lifecycle with `OnInitializedAsync`, to two way binding with `@bind-Value`, to event handling with `@onclick`, to navigation with `NavigationManager`. Confirm that you can explain, in your own words, how data flows from the database to the component, how the UI displays that data, and how user actions update the database and refresh the UI.

When you feel confident, move on to the final step.

---

## Step 11

Last step. In this step, we will merge the branches we created at the beginning of the exercise.  

First, make sure you have committed and pushed all of your changes, either using GitHub Desktop or the command line. Then follow the instructions for your preferred workflow.

### GitHub Desktop

In GitHub Desktop, click the big blue button labeled **Create Pull Request**. This will open a tab in your default browser, directing you to GitHub. Log in if prompted.  

In the top left, you will see the base branch and your current branch, connected by an arrow. Click the base branch and switch it to `OnBoarding-YourName`. This ensures that you are merging code from `OnBoarding-YourName-Second` into `OnBoarding-YourName`.  

Click **Create Pull Request**. Once the pull request is open, reload the page, scroll down, and click **Merge pull request**. Confirm if prompted. Your branches are now merged.  

### Command Line

In your terminal, run the following command to open a pull request:

```bash
gh pr create --base OnBoarding-YourName --head OnBoarding-YourName-Second
```

This will open a tab in your browser. Log in if needed. In the top left, you will see the base branch and your current branch. Switch the base branch to `OnBoarding-YourName`, so the merge will go from `OnBoarding-YourName-Second` into `OnBoarding-YourName`.  

Click **Create Pull Request**, then reload the page. Scroll down and click **Merge pull request**, confirming if necessary. Your branches are now merged.  

---

Easy, right? At least this time it should be. If there are merge conflicts, GitHub will not let you merge until they are resolved. The first time you encounter this, ask someone experienced to walk you through the process.  

And that is it. You have successfully built a page, created models, added backend logic, and merged your changes into a branch. This represents the entire development process in a nutshell.  

The last step is to switch back to the `Development` branch, or whichever branch you were assigned at the start. Since this was only an exercise, delete the two onboarding branches you created so the training code does not remain in the repository.  

Good luck with the rest of your development work.
