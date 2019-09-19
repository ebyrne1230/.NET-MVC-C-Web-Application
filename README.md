# .NET-MVC-C-Web-Application
Work done with Prosper IT Consulting Dev Ops Team

This is the placeholder for live project work done for a Portland Based company called Erectors, Inc.
As a team we built a company Intranet Web Application using C# leveraging the power of .NET frameworks with MVC.
This project included end to end work.

Testing the relative link concept

[Before and After images of this story](BeforeAndAfter4GitHub/IMG_4245.JPG)
[Test Image 1 (full size)](BeforeAndAfter4GitHub/2010 062(resized).JPG)
[Test Image 2 (full size)](BeforeAndAfter4GitHub/2010 062(small).JPG)
[Test Image 3 (full size)](BeforeAndAfter4GitHub/2010 062.JPG)


# Live Project

## Introduction

I took part in a non-paid internship with Prosper IT Consulting working as a software developer as part of a Dev Ops team that worked on building a web application that would perform as an intranet for a Portland based construction company called Erectors, Inc.

This project leveraged the power of .NET frameworks and MVC.  The main programming language used was C# in addition to the front-end languages.

Below are descriptions of stories I helped work on in an AGILE environment along with code snippets and navigation links.  

*Jump to: [Front End Stories](#front-end-stories), [Back End Stories](#back-end-stories), [Other Skills](#other-skills), [Page Top](#live-project)*

## Front End Stories
* [ModifyDateDisplay](#modifydatedisplay)
* [Back2DashboardLinks](#back2dashboardlinks)


### ModifyDateDisplay

The Story Description:  The schedule start and end dates are currently set to the display mode of yyyy-mm-dd.  This MUST REMAIN the same
so the editors for those dates can get the current value.  Use formatting statements to change the way these dates show up in the various schedule views (ie the dashboard and the index list)  Hint: This most likely needs to be done in the cshtml, but if you can get it to work on the controller side, that would be epic!

So initially for the schedule views, the webpage would display with the yyyy-mm-dd format for each scheduled job.  I was tasked with the challenge to go through the code in the various view pages and adjust the current formatting statements to render the wanted result.

My resolution for this was mostly front-end focused, residing within the schedule views.  I chose not to alter the controller this time.

Inside the Views/Schedules Folder I added formatting statements to each of these documents:

Index.cshtml (returns the view for the schedule to the user)
Note the original code that was commented out and the formatting changes inputted directly below.

```cshtml

@model IEnumerable<ConstructionNew.Models.Schedule>

@{
    ViewBag.Title = "Schedules";
}

<h2>Employee Schedules</h2>

<p>
    @Html.ActionLink("Create New Schedule", "Create")
</p>
<table class="table">
    <tr>
        <th>
            @Html.DisplayNameFor(model => model.Person.UserName)
        </th>
        <th>
            @Html.DisplayNameFor(model => model.Job.JobTitle)
        </th>
        <th>
            @Html.DisplayNameFor(model => model.StartDate)
        </th>
        <th>
            @Html.DisplayNameFor(model => model.EndDate)            
        </th>
        <th></th>
    </tr>

@foreach (var item in Model) {
    <tr>
        <td>
            @Html.DisplayFor(modelItem => item.Person.UserName)
        </td>
        <td>
            @Html.DisplayFor(modelItem => item.Job.JobTitle)
        </td>
        <td>
            @*@Html.DisplayFor(modelItem => item.StartDate)*@ @*Made Modification to start and enddates in order to create uniformity for datetime across all views and dashboard*@
            @{
                string parameterValue = item.StartDate.ToString("MM-dd-yyyy");
            }
            @Html.DisplayFor(modelItem => parameterValue)
        </td>
        <td>
            @*@Html.DisplayFor(modelItem => item.EndDate)*@ 
            @if (item.EndDate.HasValue)
            {
                @Convert.ToDateTime(item.EndDate).ToString("MM-dd-yyyy")
            }
        </td>
        @if (User.IsInRole("Admin"))
        {
            <td>
                @Html.ActionLink("Edit", "Edit", new { id = item.ScheduleId }) |
                @Html.ActionLink("Details", "Details", new { id = item.ScheduleId }) |
                @Html.ActionLink("Delete", "Delete", new { id = item.ScheduleId })
            </td>
        }
        else
        {
            <td>
                @Html.ActionLink("Details", "Details", new { id = item.ScheduleId })

            </td>
        }
    </tr>

}

</table>

```

Made additional modifications to:

Details.cshtml (returns the details view to the user)
Note the original code that was commented out and the formatting changes inputted directly below.

```cshtml

@model ConstructionNew.Models.Schedule

@{
    ViewBag.Title = "Details";
}

<h2>Schedules</h2>

<div>
    <h4>Schedule Details:</h4>
    <hr />
    <dl class="dl-horizontal">
        <dt>
            @Html.DisplayNameFor(model => model.Person.UserName)
        </dt>

        <dd>
            @Html.DisplayFor(model => model.Person.UserName)
        </dd>

        <dt>
            @Html.DisplayNameFor(model => model.Job.JobTitle)
        </dt>

        <dd>
            @Html.DisplayFor(model => model.Job.JobTitle)
        </dd>

        <dt>
            @Html.DisplayNameFor(model => model.StartDate)
        </dt>

        <dd>
            @*@Html.DisplayFor(model => model.StartDate)*@ @*This was the original code.  Made modification to StartDate display for uniformity.*@
            @{
                string parameterValueA = Model.StartDate.ToString("MM-dd-yyyy");
            }
            @Html.DisplayFor(Model => parameterValueA)
        </dd>

        <dt>
            @Html.DisplayNameFor(model => model.EndDate)
        </dt>

        <dd>
            @*@Html.DisplayFor(model => model.EndDate)*@ @*This was the original code.  Made modification to StartDate display for uniformity.*@

            @if (Model.EndDate.HasValue)
            {
                @Convert.ToDateTime(Model.EndDate).ToString("MM-dd-yyyy")
            }      
        </dd>      
        
    </dl>
</div>
@if (User.IsInRole("Admin"))
{
    <p>
        @Html.ActionLink("Edit", "Edit", new { id = Model.ScheduleId }) |
        @Html.ActionLink("Back to List", "Index")
    </p>
}
else
{
    <p>
        @Html.ActionLink("Back to List", "Index")
    </p>

}

```

Modifications were also made to:

Delete.cshtml (returns the delete view for the schedules to the user)
Note the original code that was commented out and the formatting changes inputted directly below.


```cshtml

@model ConstructionNew.Models.Schedule

@{
    ViewBag.Title = "Delete";
}

<h2>Schedules</h2>

<h3>Are you sure you want to delete this?</h3>
<div>
    <h4></h4>
    <hr />
    <dl class="dl-horizontal">
       
        <dt>
            @Html.DisplayNameFor(model => model.Person.UserName)
        </dt>

        <dd>
            @Html.DisplayFor(model => model.Person.UserName)
        </dd>

        <dt>
            @Html.DisplayNameFor(model => model.Job.JobTitle)
        </dt>

        <dd>
            @Html.DisplayFor(model => model.Job.JobTitle)
        </dd>

        <dt>
            @Html.DisplayNameFor(model => model.StartDate)
        </dt>

        <dd>           
            @*@Html.DisplayFor(model => model.StartDate)*@ @*This was the original code.  Made modification to StartDate display for uniformity.*@
            @{
                string parameterValueA = Model.StartDate.ToString("MM-dd-yyyy");
            }
            @Html.DisplayFor(Model => parameterValueA)
        </dd>
        <dt>
            @Html.DisplayNameFor(model => model.EndDate)
        </dt>

        <dd>
            @*@Html.DisplayFor(model => model.EndDate)*@ @*This was the original code.  Made modification to StartDate display for uniformity.*@

            @if (Model.EndDate.HasValue)
            {
                @Convert.ToDateTime(Model.EndDate).ToString("MM-dd-yyyy")
            }

        </dd>
    </dl>

    @using (Html.BeginForm()) {
        @Html.AntiForgeryToken()

        <div class="form-actions no-color">
            <input type="submit" value="Delete" class="btn btn-default" /> |
            @Html.ActionLink("Back to List", "Index")
        </div>
    }
</div>

```

*Jump to: [Front End Stories](#front-end-stories), [Back End Stories](#back-end-stories), [Other Skills](#other-skills), [Page Top](#live-project)*

### Back2DashBoardLinks

The Story Description:  (Not the original) The idea was to review all pages accessible by all users.  To go through thoroughly and to 
provide a way for all users in any view on the site to have the option to navigate directly back to the Dashboard view that opens once a user is logged into the program.


Because of how tedious this assignment was I am chosing not to copy every page I made modifications to.  I am providing a couple of 
snippets to give you a visual example of the code additions.

This was focused on the front-end aspect to the application.  All modifications added were of action links using Razor and Html syntax.

For example on the Schedule Index in the Views folder I added the following code:


```cshtml

@using System.Web.UI.WebControls
@using ConstructionNew.Models
@using ConstructionNew.Enums
@using ConstructionNew.Extensions
@using System.Collections.Generic



@model IEnumerable<ConstructionNew.Models.Job>



@{
    ViewBag.Title = "Schedules";
}

<h5 style="text-align:right">
    @Html.ActionLink("Back to Dashboard", "Index", "Dashboard")
</h5>

```

This tag would appear on the page on the upper right side and if the user clicked the link they would be rerouted to the Dashboard page.



Another example is at the bottom portion of the code for the Index.cshtml in the Jobs/View folder I added this tag:

```cshtml

        else
        {
            <td>
               
                @Html.ActionLink("Details", "Details", new { id = item.JobId })
               
            </td>    
        }

        
    </tr>
        }
    }


</table>
<p>
    @Html.ActionLink("Back to Dashboard", "Index", "Dashboard")
</p>
```

Note the actionlink.  It again will appear to the user as an underlined link in blue that if they clicked on it they would be routed to the Dashboard View page.

All together these actionlinks would provide the user easy access to the Dashboard View page from anywhere within the site.

*Jump to: [Front End Stories](#front-end-stories), [Back End Stories](#back-end-stories), [Other Skills](#other-skills), [Page Top](#live-project)*


## Back End Stories
* [ModifyScheduleIndex](#modifyscheduleindex)
* [AddForemanFromJobView](#addforemanfromjobview)


### ModifyScheduleIndex

Description: Have the schedule index display all schedules sorted by job, similar to the partial view.  Make sure there are working links to edit and delete each schedule item.  Each job does not require full details, just the job title and number need be visible.

Prior to my story the current form of the Employee Schedules was a plain table listing: Job Title, Job#, UserName, Start Date, and End Date.  The table would include a new instance for the Job Title and Job # with the other information each time a new user was assigned.

The goal of this story was to reformat the display so it would look similar to that portion of the dashboard view with respect to organizing all employees assigned to a particular job under that same job rather than be a new iteration of the job and job number.  A cleaner and more organized look.

This task would involve both FrontEnd and BackEnd strategy in order to complete the requirements.

For the Front End portion I would have to make modification in the Views/Schedules Folder to the Index.cshtml file.

Note the code before:

```cshtml

@model IEnumerable<ConstructionNew.Models.Schedule>

@{
    ViewBag.Title = "Schedules";
}

<h2>Employee Schedules</h2>

<p>
    @Html.ActionLink("Create New Schedule", "Create")
</p>
<table class="table">
    <tr>
        <th>
            @Html.DisplayNameFor(model => model.Person.UserName)
        </th>
        <th>
            @Html.DisplayNameFor(model => model.Job.JobTitle)
        </th>
        <th>
            @Html.DisplayNameFor(model => model.StartDate)
        </th>
        <th>
            @Html.DisplayNameFor(model => model.EndDate)            
        </th>
        <th></th>
    </tr>

@foreach (var item in Model) {
    <tr>
        <td>
            @Html.DisplayFor(modelItem => item.Person.UserName)
        </td>
        <td>
            @Html.DisplayFor(modelItem => item.Job.JobTitle)
        </td>
        <td>
            @*@Html.DisplayFor(modelItem => item.StartDate)*@ @*Made Modification to start and enddates in order to create uniformity for datetime across all views and dashboard*@
            @{
                string parameterValue = item.StartDate.ToString("MM-dd-yyyy");
            }
            @Html.DisplayFor(modelItem => parameterValue)
        </td>
        <td>
            @*@Html.DisplayFor(modelItem => item.EndDate)*@ 
            @if (item.EndDate.HasValue)
            {
                @Convert.ToDateTime(item.EndDate).ToString("MM-dd-yyyy")
            }
        </td>
        @if (User.IsInRole("Admin"))
        {
            <td>
                @Html.ActionLink("Edit", "Edit", new { id = item.ScheduleId }) |
                @Html.ActionLink("Details", "Details", new { id = item.ScheduleId }) |
                @Html.ActionLink("Delete", "Delete", new { id = item.ScheduleId })
            </td>
        }
        else
        {
            <td>
                @Html.ActionLink("Details", "Details", new { id = item.ScheduleId })

            </td>
        }
    </tr>

}

</table>

```

After modifications to the code... 
The new index.cshtml page

```cshtml

@using System.Web.UI.WebControls
@using ConstructionNew.Models
@using ConstructionNew.Enums
@using ConstructionNew.Extensions
@using System.Collections.Generic



@model IEnumerable<ConstructionNew.Models.Job>



@{
    ViewBag.Title = "Schedules";
}



@*//Main Schedule Index Page Template for View  Option 1*@
<div class="container-fluid"
<div class="row ">
    <div class="panel-group top-buffer col-lg-9" id="accordion" role="tablist" aria-multiselectable="true" style="width:100%">
        <div class="panel panel-default ">
            <div class="panel-heading" role="tab" id="headingOne">
                <h2 class="panel-title">
                    Employee Schedules
                    @if (User.IsInRole("Admin"))
                    {

                        @Html.ActionLink("Create New", "Create", "Schedules", null, new { @class = "createNew" })

                    }
                </h2>

            </div>
            <br />
            
            <div class="container">
                @foreach (Job job in Model)
                {
                    
                    <div class="panel panel-info">
                        <div class="panel-heading">
                            <table style="width:100%">
                                <tr>
                                    <td style="width:40px">
                                        <span class="label label-success">@job.JobNumber</span>
                                    </td>
                                    <td>
                                        <span style="font-weight:bold">@job.JobTitle</span>
                                        <br />@job.StreetAddress @job.City, @job.State
                                    </td>
                                    @*<td>
                                    <span>Start Time: @schedule.Job.ShiftTimes</span>
                                </td>*@
                                    @*<td>*@
                                    @*If there is a note, Display it*@
                                    @*@if (!String.IsNullOrEmpty(schedule.Job.Note))*@
                                    @*{
                                        <span>Note: @schedule.Job.Note</span>
                                    }
                                </td>*@
                                    <td style="text-align:right">
                                        <span>
                                            <i class="glyphicon glyphicon-info-sign"></i>
                                            @*Action Link to Job Details*@
                                            @Html.ActionLink("Details", "Details", "Jobs", new { id = job.JobId }, null)
                                        </span>
                                    </td>
                                </tr>
                            </table>
                        </div> 

                        <div class="panel-body">
                            <table style="width:100%">
                                <tr>
                                    <th>Name</th>
                                    @*<th></th>  
                                    <th></th>*@  @*These stayed in for allignment purposes*@
                                    <th><i class="glyphicon glyphicon-calendar"> </i> Start Date</th>
                                    <th><i class="glyphicon glyphicon-calendar"> </i> End Date</th>
                                </tr>
                                @*<br />*@ @*//Optional break to add more space if you think is necessary*@
                                <br />

                                @foreach (Job jobs in Model)
                                {

                                    foreach (var j in jobs.Schedules)

                                    {

                                        if (j.Job.JobNumber == job.JobNumber)
                                        {
                                            <tr style="border-bottom: 1px solid #ddd">
                                                <td style="width:20%">
                                                    @j.Person.FName @j.Person.LName
                                                </td>

                                                <td style="width:20%">
                                                    @j.StartDate.ToString("MM-dd-yyyy")
                                                </td>
                                                <td style="width:20%">
                                                    @if (j.EndDate.HasValue)
                                                    {
                                                        @Convert.ToDateTime(j.EndDate).ToString("MM-dd-yyyy")
                                                    }
                                                </td>

                                                <td>
                                                    <span>
                                                        <i class="glyphicon glyphicon-info-sign"></i>
                                                        @*Action Link to Job Details*@
                                                        @Html.ActionLink("Details", "Details", "Schedules", new { id = j.ScheduleId }, null)
                                                    </span>
                                                </td>

                                                @if (User.IsInRole("Admin") || User.IsInRole("Manager"))
                                                {
                                                    <td style="text-align:right">
                                                        <a href="@Url.Action("Edit", "Schedules", new { id = j.ScheduleId }, null)">
                                                            <span class="glyphicon glyphicon-pencil" aria-hidden="true"></span>
                                                        </a>
                                                    </td>
                                                }

                                                @if (User.IsInRole("Employee"))
                                                {
                                                    <td style="text-align:right">
                                                        <a href="@Url.Action("Details", "Schedules", new { id = j.ScheduleId }, null)">
                                                            <span class="glyphicon glyphicon-info-sign" aria-hidden="true"></span>
                                                        </a>
                                                    </td>
                                                }
                                            </tr>
                                        }
                                    }
                                 }
                            </table>
                        </div>                        
                    </div>              
                 }                
            </div>
            <br />
        </div>
    </div>
</div>
</div>
```

On the back-end I had to make some changes to the SchedulesController in order for the information that we wanted to be sorted and passed to the view for the user to see.

This is a snippet of the current SchedulesController before my code changes:

```csharp

using System;
using System.Collections.Generic;
using System.Data;
using System.Data.Entity;
using System.Linq;
using System.Net;
using System.Web;
using System.Web.Mvc;
using ConstructionNew.Models;

namespace ConstructionNew.Controllers
{
    public class SchedulesController : Controller
    {
        private ApplicationDbContext db = new ApplicationDbContext();

        // GET: Schedules
        [Authorize] //used to restrict view of unregistered users
        public ActionResult Index()
        {
            // Added ordering by Startdate to make index view a little easier to read for testing
            // Doesn't need to stay this way. JS 5/5/19
            return View(db.Schedules.OrderBy(s => s.StartDate).ToList());
        }

        // GET: Schedules/Details/5
        [Authorize] //used to restrict view of unregistered users
        public ActionResult Details(Guid? id)
        {
            if (id == null)
            {
                return new HttpStatusCodeResult(HttpStatusCode.BadRequest);
            }
            Schedule schedule = db.Schedules.Find(id);
            if (schedule == null)
            {
                return HttpNotFound();
            }
            return View(schedule);
        }

        // GET: Schedules/Create
        [Authorize(Roles = "Admin")]
        public ActionResult Create()
        {
            // Obtain data for drowpdown lists
            ViewBag.jobsite = GetJobSites();
            ViewBag.person = GetUsers();
            return View();
        }


        // POST: Schedules/Create
        // To protect from overposting attacks, please enable the specific properties you want to bind to, for 
        // more details see https://go.microsoft.com/fwlink/?LinkId=317598.
        [HttpPost]
        [ValidateAntiForgeryToken]
        [Authorize(Roles = "Admin")]
        public ActionResult Create(string Person, Guid JobSite, DateTime StartDate, DateTime? EndDate)
        {
            // EndDate should be after StartDate
            if (EndDate.HasValue && EndDate < StartDate)
            {
                ModelState.AddModelError("EndDate", "You must choose an End Date that is after the Start Date.");
            }
            else if (ModelState.IsValid)
            {
                // Post Method receives simple parameters from the view
                // Person and JobSite are used to fetch a Job and User from the db
                // which model binding doesn't automatically do.
                Job job = db.Jobs.Where(j => j.JobId == JobSite).First();
                ApplicationUser applicationUser = db.Users.Where(u => u.Id == Person).First();
                // Those can then be passed to the constructor along with the dates to build a schedule.
                Schedule schedule = new Schedule(applicationUser, job, StartDate, EndDate);
                db.Schedules.Add(schedule);
                db.SaveChanges();
                return RedirectToAction("Index");
            }

            // If Schedule creation failed, we need to get users and jobs to pass to the view again...
            ViewBag.jobsite = GetJobSites();
            ViewBag.person = GetUsers();
            // ... and display an error message.
            ModelState.AddModelError(string.Empty, "There was an error and your schedule was not created.");
            return View();
        }

        // GET: Schedules/Edit/5
        [Authorize(Roles = "Admin")]
        public ActionResult Edit(Guid? id)
        {
            if (id == null)
            {
                return new HttpStatusCodeResult(HttpStatusCode.BadRequest);
            }
            Schedule schedule = db.Schedules.Find(id);
            if (schedule == null)
            {
                return HttpNotFound();
            }
            ViewBag.jobsite = GetJobSites(schedule.Job.JobId.ToString()); // Pass in Job Id as string to GetJobsites method to retrieve selected object to be used as default selection. 
            ViewBag.person = GetUsers(schedule.Person.Id); // Pass in Person Id to GetUsers method to retrieve selected object to be used as default selection. 
            return View(schedule);
        }
        // POST: Schedules/Edit/5
        // To protect from overposting attacks, please enable the specific properties you want to bind to, for 
        // more details see https://go.microsoft.com/fwlink/?LinkId=317598.
        [HttpPost]
        [ValidateAntiForgeryToken]
        [Authorize(Roles = "Admin")]
        public ActionResult Edit([Bind(Include = "ScheduleId,StartDate,EndDate")] Schedule schedule, string Person, Guid JobSite)
        {


            ApplicationUser _person = db.Users.Where(u => u.Id == Person).First();
            Job _job = db.Jobs.Where(j => j.JobId == JobSite).First();

            Schedule Schedule = db.Schedules.Where(s => s.ScheduleId == schedule.ScheduleId).First();
            Schedule.Job = _job;
            Schedule.Person = _person;
            Schedule.EndDate = schedule.EndDate;
            Schedule.StartDate = schedule.StartDate;

            if (ModelState.IsValid)
            {
                db.Entry(Schedule).State = EntityState.Modified;
                db.SaveChanges();
                return RedirectToAction("Index");
            }
            return View(schedule);
        }

        // GET: Schedules/Delete/5
        [Authorize(Roles = "Admin")]
        public ActionResult Delete(Guid? id)
        {
            if (id == null)
            {
                return new HttpStatusCodeResult(HttpStatusCode.BadRequest);
            }
            Schedule schedule = db.Schedules.Find(id);
            if (schedule == null)
            {
                return HttpNotFound();
            }
            return View(schedule);
        }

        // POST: Schedules/Delete/5
        [Authorize(Roles = "Admin")]
        [HttpPost, ActionName("Delete")]
        [ValidateAntiForgeryToken]


        public ActionResult DeleteConfirmed(Guid id)
        {
            Schedule schedule = db.Schedules.Find(id);
            db.Schedules.Remove(schedule);
            db.SaveChanges();
            return RedirectToAction("Index");
        }

        protected override void Dispose(bool disposing)
        {
            if (disposing)
            {
                db.Dispose();
            }
            base.Dispose(disposing);
        }

        private IEnumerable<SelectListItem> GetJobSites(string selectedJobId = null)  // Added selectedJobId that is set to null to be passed in when a default field value is needed
        {
            // Create a list of JobTitle/JobId pairs to pass to the view with viewbag.jobsite
            // The DropDownList will display the JobTitle list and
            // The POST data receives the corresponding JobId

            return db.Jobs.OrderBy(o => o.JobNumber).Select(m => new SelectListItem()
            {
                Value = m.JobId.ToString(),
                Text = m.JobTitle,
                Selected = selectedJobId == m.JobId.ToString() // Add Select variable that holds the Id of the current selected item that gets passed when the corresponding edit button is clicked
            });
        }

        private IEnumerable<SelectListItem> GetUsers(string selectedUserId = null) // Added selectedUserId that is set to null to be passed in when a default field value is needed
        {
            // Create a list of UserName/Id pairs to pass to the view with viewbag.person
            // The DropDownList will display the UserName list and
            // The POST data receives the corresponding Id

            // Eventually we might need to change this to pass FName/Id pairs as
            // usernames are probably not very relevant to the person creating the schedules.
            // They are probably going to want to see the workers actual full name.
            // Our current seed data does not contain First/Last names to do this yet.
            // Feel free to delete this comment block if we end up implementing this change,
            // or if we decide to stick with UserName for the create view.
            // -- Jeremy Stewart -- 5/5/2019
            //
            return db.Users.Where(x => x.UserName != "SiteAdmin").OrderBy(o => o.UserName).Select(m => new SelectListItem()
            {
                Value = m.Id,
                Text = (m.FName + " " + m.LName),
                Selected = selectedUserId == m.Id // Add Select variable that holds the Id of the current selected item that gets passed when the corresponding edit button is clicked
            });
        }

        private List<Schedule> GetSchedules()
        {
            var schedules = db.Schedules.OrderBy(o => o.Job.JobNumber).ToList();
            return schedules;
        }
    }
}
```

This is what my code looked like after I made some changes to the SchedulesController to allow for required sorting of information by use of our model.

```csharp
using System;
using System.Collections.Generic;
using System.Data;
using System.Data.Entity;
using System.Linq;
using System.Net;
using System.Web;
using System.Web.Mvc;
using ConstructionNew.Models;

namespace ConstructionNew.Controllers
{
    public class SchedulesController : Controller
    {
        private ApplicationDbContext db = new ApplicationDbContext();

        // GET: Schedules
        [Authorize] //used to restrict view of unregistered users
        public ActionResult Index()
        {
            // Added ordering by Startdate to make index view a little easier to read for testing
            // Doesn't need to stay this way. JS 5/5/19
            //return View(db.Schedules.OrderBy(s => s.StartDate).ToList());
            //return View(db.Schedules.OrderBy(s => s.Job.JobNumber).ToList());
            //var jobsAssigned = db.Jobs.OrderBy(s => s.JobNumber).ToList();

            //In order to allow for the handshake between Job library and Schedule library jobs had to be created within the controller
            //Looking for an option to allow for both this and a way to sort by Job number  -- EB 5/24/19

            var jobs = db.Jobs.Where(j => j.Schedules.Count > 0).ToList();           

            return View(jobs);

        }

        // GET: Schedules/Details/5
        [Authorize] //used to restrict view of unregistered users
        public ActionResult Details(Guid? id)
        {
            if (id == null)
            {
                return new HttpStatusCodeResult(HttpStatusCode.BadRequest);
            }
            Schedule schedule = db.Schedules.Find(id);
            if (schedule == null)
            {
                return HttpNotFound();
            }
            return View(schedule);
        }

        // GET: Schedules/Create
        [Authorize(Roles = "Admin")]
        public ActionResult Create()
        {
            // Obtain data for drowpdown lists
            ViewBag.jobsite = GetJobSites();
            ViewBag.person = GetUsers();
            return View();
        }


        // POST: Schedules/Create
        // To protect from overposting attacks, please enable the specific properties you want to bind to, for 
        // more details see https://go.microsoft.com/fwlink/?LinkId=317598.
        [HttpPost]
        [ValidateAntiForgeryToken]
        [Authorize(Roles = "Admin")]
        public ActionResult Create(string Person, Guid JobSite, DateTime StartDate, DateTime? EndDate)
        {
            // EndDate should be after StartDate
            if (EndDate.HasValue && EndDate < StartDate)
            {
                ModelState.AddModelError("EndDate", "You must choose an End Date that is after the Start Date.");
            }
            else if (ModelState.IsValid)
            {
                // Post Method receives simple parameters from the view
                // Person and JobSite are used to fetch a Job and User from the db
                // which model binding doesn't automatically do.
                Job job = db.Jobs.Where(j => j.JobId == JobSite).First();
                ApplicationUser applicationUser = db.Users.Where(u => u.Id == Person).First();
                // Those can then be passed to the constructor along with the dates to build a schedule.
                Schedule schedule = new Schedule(applicationUser, job, StartDate, EndDate);
                db.Schedules.Add(schedule);
                db.SaveChanges();
                return RedirectToAction("Index");
            }

            // If Schedule creation failed, we need to get users and jobs to pass to the view again...
            ViewBag.jobsite = GetJobSites();
            ViewBag.person = GetUsers();
            // ... and display an error message.
            ModelState.AddModelError(string.Empty, "There was an error and your schedule was not created.");
            return View();
        }

        // GET: Schedules/Edit/5
        [Authorize(Roles = "Admin")]
        public ActionResult Edit(Guid? id)
        {
            if (id == null)
            {
                return new HttpStatusCodeResult(HttpStatusCode.BadRequest);
            }
            Schedule schedule = db.Schedules.Find(id);
            if (schedule == null)
            {
                return HttpNotFound();
            }
            ViewBag.jobsite = GetJobSites(schedule.Job.JobId.ToString()); // Pass in Job Id as string to GetJobsites method to retrieve selected object to be used as default selection. 
            ViewBag.person = GetUsers(schedule.Person.Id); // Pass in Person Id to GetUsers method to retrieve selected object to be used as default selection. 
            return View(schedule);
        }
        // POST: Schedules/Edit/5
        // To protect from overposting attacks, please enable the specific properties you want to bind to, for 
        // more details see https://go.microsoft.com/fwlink/?LinkId=317598.
        [HttpPost]
        [ValidateAntiForgeryToken]
        [Authorize(Roles = "Admin")]
        public ActionResult Edit([Bind(Include = "ScheduleId,StartDate,EndDate")] Schedule schedule, string Person, Guid JobSite)
        {


            ApplicationUser _person = db.Users.Where(u => u.Id == Person).First();
            Job _job = db.Jobs.Where(j => j.JobId == JobSite).First();

            Schedule Schedule = db.Schedules.Where(s => s.ScheduleId == schedule.ScheduleId).First();
            Schedule.Job = _job;
            Schedule.Person = _person;
            Schedule.EndDate = schedule.EndDate;
            Schedule.StartDate = schedule.StartDate;

            if (ModelState.IsValid)
            {
                db.Entry(Schedule).State = EntityState.Modified;
                db.SaveChanges();
                return RedirectToAction("Index");
            }
            return View(schedule);
        }

        // GET: Schedules/Delete/5
        [Authorize(Roles = "Admin")]
        public ActionResult Delete(Guid? id)
        {
            if (id == null)
            {
                return new HttpStatusCodeResult(HttpStatusCode.BadRequest);
            }
            Schedule schedule = db.Schedules.Find(id);
            if (schedule == null)
            {
                return HttpNotFound();
            }
            return View(schedule);
        }

        // POST: Schedules/Delete/5
        [Authorize(Roles = "Admin")]
        [HttpPost, ActionName("Delete")]
        [ValidateAntiForgeryToken]


        public ActionResult DeleteConfirmed(Guid id)
        {
            Schedule schedule = db.Schedules.Find(id);
            db.Schedules.Remove(schedule);
            db.SaveChanges();
            return RedirectToAction("Index");
        }

        protected override void Dispose(bool disposing)
        {
            if (disposing)
            {
                db.Dispose();
            }
            base.Dispose(disposing);
        }

        private IEnumerable<SelectListItem> GetJobSites(string selectedJobId = null)  // Added selectedJobId that is set to null to be passed in when a default field value is needed
        {
            // Create a list of JobTitle/JobId pairs to pass to the view with viewbag.jobsite
            // The DropDownList will display the JobTitle list and
            // The POST data receives the corresponding JobId

            return db.Jobs.OrderBy(o => o.JobNumber).Select(m => new SelectListItem()
            {
                Value = m.JobId.ToString(),
                Text = m.JobTitle,
                Selected = selectedJobId == m.JobId.ToString() // Add Select variable that holds the Id of the current selected item that gets passed when the corresponding edit button is clicked
            });
        }

        private IEnumerable<SelectListItem> GetUsers(string selectedUserId = null) // Added selectedUserId that is set to null to be passed in when a default field value is needed
        {
            // Create a list of UserName/Id pairs to pass to the view with viewbag.person
            // The DropDownList will display the UserName list and
            // The POST data receives the corresponding Id

            // Eventually we might need to change this to pass FName/Id pairs as
            // usernames are probably not very relevant to the person creating the schedules.
            // They are probably going to want to see the workers actual full name.
            // Our current seed data does not contain First/Last names to do this yet.
            // Feel free to delete this comment block if we end up implementing this change,
            // or if we decide to stick with UserName for the create view.
            // -- Jeremy Stewart -- 5/5/2019
            //
            return db.Users.Where(x => x.UserName != "SiteAdmin").OrderBy(o => o.UserName).Select(m => new SelectListItem()
            {
                Value = m.Id,
                Text = (m.FName + " " + m.LName),
                Selected = selectedUserId == m.Id // Add Select variable that holds the Id of the current selected item that gets passed when the corresponding edit button is clicked
            });
        }

        private List<Schedule> GetSchedules()
        {
            var schedules = db.Schedules.OrderBy(o => o.Job.JobNumber).ToList();
            return schedules;
        }

        public List<string> FetchWeeks(int year)
        {
            List<string> weeks = new List<string>();
            DateTime startDate = new DateTime(year, 1, 1);
            startDate = startDate.AddDays(1 - (int)startDate.DayOfWeek);
            DateTime endDate = startDate.AddDays(6);
            while(startDate.Year < 1 + year)
            {
                weeks.Add(string.Format("{0:MM/dd/yyyy} to {1:MM/dd/yyyy}", startDate, endDate));
                startDate = startDate.AddDays(7);
                endDate = endDate.AddDays(7);
            }
            return weeks;
        }

        public ActionResult MasterScheduleEdit()
        {

            var weeks = FetchWeeks(2019);
            ViewBag.Weeks = new SelectList(weeks);
            return View();
        }
    }
}
```
This would ultimately lead to rendering a page for the user that looked very similar to that of the Dashboard View and specific to each job organized with the usernames that were associated with it rather than being repeated and listed separatly.


*Jump to: [Front End Stories](#front-end-stories), [Back End Stories](#back-end-stories), [Other Skills](#other-skills), [Page Top](#live-project)*





### AddForemanFromJobView

Description:
With the expectation that the available foremen will be consistent throughout the development of the project, add an optional form 
to the create job view that allows the user to select a foreman and add dates.  A drop-down selector should include the list of foremen.  Hint: You may wish to use a partial view for this form to make the models easier.

So the current create job view had a form that asked the user to select and/or enter: Job Title, Job#, UserName, Start and End Dates.

I was to add a secondary form on the same page that would allow for the selection of a foreman to add to the job.

The solution to this story was split between work done on the backend with modifications made to the JobsController and work on the 
frontend which included the creation of a partial view page that would work off of another view page but in doing so allow for the rendering of the information we wanted to be seen for the forms to be functional.


Before any code modifications have been made the JobsConstroller.cs file looked like this:

```csharp

using System;
using System.Collections.Generic;
using System.Data;
using System.Data.Entity;
using System.Linq;
using System.Net;
using System.Web;
using System.Web.Mvc;
using ConstructionNew.Models;

namespace ConstructionNew.Controllers
{
    public class JobsController : Controller
    {
        private ApplicationDbContext db = new ApplicationDbContext();

        // GET: Jobs
        [Authorize] //used to restrict view of unregistered users
        public ActionResult Index()
        {
            return View(db.Jobs.ToList());
        }

        // GET: Jobs/Details/5
        [Authorize] //used to restrict view of unregistered users
        public ActionResult Details(Guid? id)
        {
            if (id == null)
            {
                return new HttpStatusCodeResult(HttpStatusCode.BadRequest);
            }
            Job job = db.Jobs.Find(id);
            if (job == null)
            {
                return HttpNotFound();
            }
            return View(job);
        }

        // GET: Jobs/Create
        [Authorize(Roles = "Admin")]
        public ActionResult Create()
        {
            return View();
        }

        // POST: Jobs/Create
        // To protect from overposting attacks, please enable the specific properties you want to bind to, for 
        // more details see https://go.microsoft.com/fwlink/?LinkId=317598.
        [HttpPost]
        [ValidateAntiForgeryToken]
        [Authorize(Roles = "Admin")]
        public ActionResult Create([Bind(Include = "JobId,JobTitle,JobNumber,ShiftTimes,StreetAddress,City,State,Zipcode,Note")] Job job)
        {
            if (ModelState.IsValid)
            {
                if (db.Jobs.Any(j => j.JobTitle.Equals(job.JobTitle)))
                {
                    ModelState.AddModelError("JobTitle", "This job title is currently being used, please change the title," +
                                             " or delete/modify the job titled " + "\""+ job.JobTitle + "\".");
                    return View(job);
                }
                if (db.Jobs.Any(j => j.JobNumber.Equals(job.JobNumber)))
                {
                    ModelState.AddModelError("JobNumber", "This job number is currently being used, please change the number," +
                                             " or delete/modify the job numbered " + "\"" + job.JobNumber + "\".");
                    return View(job);
                }
                else
                {
                    job.JobId = Guid.NewGuid();
                    db.Jobs.Add(job);
                    db.SaveChanges();
                    return RedirectToAction("Index");
                }
            }
            return View(job);
        }

        // GET: Jobs/Edit/5
        [Authorize(Roles = "Admin")]
        public ActionResult Edit(Guid? id)
        {
            if (id == null)
            {
                return new HttpStatusCodeResult(HttpStatusCode.BadRequest);
            }
            Job job = db.Jobs.Find(id);
            if (job == null)
            {
                return HttpNotFound();
            }
            return View(job);
        }

        // POST: Jobs/Edit/5
        // To protect from overposting attacks, please enable the specific properties you want to bind to, for 
        // more details see https://go.microsoft.com/fwlink/?LinkId=317598.
        [HttpPost]
        [ValidateAntiForgeryToken]
        [Authorize(Roles = "Admin")]
        public ActionResult Edit([Bind(Include = "JobId,JobTitle,JobNumber,ShiftTimes,StreetAddress,City,State,Zipcode,Note")] Job job)
        {
            if (ModelState.IsValid)
            {
                db.Entry(job).State = EntityState.Modified;
                db.SaveChanges();
                return RedirectToAction("Index");
            }
            return View(job);
        }

        // GET: Jobs/Delete/5
        [Authorize(Roles = "Admin")]
        public ActionResult Delete(Guid? id)
        {
            if (id == null)
            {
                return new HttpStatusCodeResult(HttpStatusCode.BadRequest);
            }
            Job job = db.Jobs.Find(id);
            if (job == null)
            {
                return HttpNotFound();
            }
            return View(job);
        }

        // POST: Jobs/Delete/5
        [HttpPost, ActionName("Delete")]
        [ValidateAntiForgeryToken]
        [Authorize(Roles = "Admin")]
        public ActionResult DeleteConfirmed(Guid id)
        {
            Job job = db.Jobs.Find(id);
            db.Jobs.Remove(job);
            db.SaveChanges();
            return RedirectToAction("Index");
        }

        protected override void Dispose(bool disposing)
        {
            if (disposing)
            {
                db.Dispose();
            }
            base.Dispose(disposing);
        }
    }
}


```

I've included a snippet of the changes made to the JobsController to allow for new data to be bound and posted to the database
including foreman assignment, also for the creation of a partialview which will eventually be visible as a secondary form to the user.

After:

```csharp

using System;
using System.Collections.Generic;
using System.Data;
using System.Data.Entity;
using System.Linq;
using System.Net;
using System.Web;
using System.Web.Mvc;
using ConstructionNew.Models;

namespace ConstructionNew.Controllers
{
    public class JobsController : Controller
    {
        private ApplicationDbContext db = new ApplicationDbContext();

        // GET: Jobs
        [Authorize] //used to restrict view of unregistered users
        public ActionResult Index()
        {

            return View(db.Jobs.OrderBy(i => i.JobNumber).ToList());
        }

        // GET: Jobs/Details/5
        [Authorize] //used to restrict view of unregistered users
        public ActionResult Details(Guid? id)
        {
            if (id == null)
            {
                return new HttpStatusCodeResult(HttpStatusCode.BadRequest);
            }
            Job job = db.Jobs.Find(id);
            if (job == null)
            {
                return HttpNotFound();
            }

            var jobDetailsVM = new JobDetailsViewModel
            {
                JobId = job.JobId,
                JobTitle = job.JobTitle,
                JobNumber = job.JobNumber,
                StreetAddress = job.StreetAddress,
                City = job.City,
                State = job.State,
                Zipcode = job.Zipcode,
                Note = job.Note,
                ShiftTimes = job.ShiftTimes,
                ForemanName = "None Assigned",
                Phone = "",
                
            };
            //Get foreman data
            var query = (from a in db.Users
                         join b in db.Schedules on a.Id equals b.Person.Id
                         where a.WorkType == Enums.WorkType.Foreman && b.Job.JobNumber == job.JobNumber
                         select new
                         {
                             a.FName,
                             a.LName,
                             a.PhoneNumber
                         }).FirstOrDefault();
            //Add foreman data to VM if a result was found
            if (query != null)
            {
                jobDetailsVM.ForemanName = query.FName + " " + query.LName;
                jobDetailsVM.Phone = query.PhoneNumber;
            }

            return View(jobDetailsVM);
        }



        // GET: Jobs/Create
        [Authorize(Roles = "Admin")]
        public ActionResult Create()
        {
            // Obtain data for drowpdown lists            
            ViewBag.foremen = GetForemen();
            return View();
        }

        // POST: Jobs/Create
        // To protect from overposting attacks, please enable the specific properties you want to bind to, for 
        // more details see https://go.microsoft.com/fwlink/?LinkId=317598.
        [HttpPost]
        [ValidateAntiForgeryToken]
        [Authorize(Roles = "Admin")]
        public ActionResult Create([Bind(Include = "JobId,JobTitle,JobNumber,ShiftTimes,StreetAddress,City,State,Zipcode,Note")] Job job, [Bind(Include ="Person, StartDate,EndDate")] Schedule schedule, String Foremen)
        {
            if (ModelState.IsValid)
            {
                if (db.Jobs.Any(j => j.JobTitle.Equals(job.JobTitle)))
                {
                    ModelState.AddModelError("JobTitle", "This job title is currently being used, please change the title," +
                                             " or delete/modify the job titled " + "\"" + job.JobTitle + "\".");
                    return View(job);
                }
                if (db.Jobs.Any(j => j.JobNumber.Equals(job.JobNumber)))
                {
                    ModelState.AddModelError("JobNumber", "This job number is currently being used, please change the number," +
                                             " or delete/modify the job numbered " + "\"" + job.JobNumber + "\".");
                    return View(job);
                }
                else
                {
                    job.JobId = Guid.NewGuid();
                    db.Jobs.Add(job);
                    db.SaveChanges();
                    if (schedule != null)
                    {
                        ApplicationUser foreman = db.Users.Where(f => f.Id == Foremen).FirstOrDefault();
                        schedule.Job = db.Jobs.Where(j => j.JobId == job.JobId).FirstOrDefault();
                        schedule.Person = foreman;
                        schedule.ScheduleId = Guid.NewGuid();
                        db.Schedules.Add(schedule);
                        db.SaveChanges();
                    }
                    return RedirectToAction("Index");
                }
            }            
            return View(job);
        }

        // GET: Jobs/Edit/5
        [Authorize(Roles = "Admin")]
        public ActionResult Edit(Guid? id)
        {
            if (id == null)
            {
                return new HttpStatusCodeResult(HttpStatusCode.BadRequest);
            }
            Job job = db.Jobs.Find(id);
            if (job == null)
            {
                return HttpNotFound();
            }
           
            return View(job);
        }

        // POST: Jobs/Edit/5
        // To protect from overposting attacks, please enable the specific properties you want to bind to, for 
        // more details see https://go.microsoft.com/fwlink/?LinkId=317598.
        [HttpPost]
        [ValidateAntiForgeryToken]
        [Authorize(Roles = "Admin")]
        public ActionResult Edit([Bind(Include = "JobId,JobTitle,JobNumber,ShiftTimes,StreetAddress,City,State,Zipcode,Note")] Job job)
        {
            if (ModelState.IsValid)
            {
                db.Entry(job).State = EntityState.Modified;
                db.SaveChanges();
                return RedirectToAction("Index");
            }
            return View(job);
        }

        // GET: Jobs/Delete/5
        [Authorize(Roles = "Admin")]
        public ActionResult Delete(Guid? id)
        {
            if (id == null)
            {
                return new HttpStatusCodeResult(HttpStatusCode.BadRequest);
            }
            Job job = db.Jobs.Find(id);
            if (job == null)
            {
                return HttpNotFound();
            }
            return View(job);
        }

        // POST: Jobs/Delete/5
        [HttpPost, ActionName("Delete")]
        [ValidateAntiForgeryToken]
        [Authorize(Roles = "Admin")]
        public ActionResult DeleteConfirmed(Guid id)
        {
            Job job = db.Jobs.Find(id);
            db.Jobs.Remove(job);
            db.SaveChanges();
            return RedirectToAction("Index");
        }

        private IEnumerable<SelectListItem> GetForemen(string selectedForemanId = null)  // Added selectedForemenId that is set to null to be passed in when a default field value is needed
        {
            // Create a list of Foreman to pass to the viewbag foreman
            // The DropDownList will display the Foremen list and
            // The POST data receives the corresponding ForemenId

            return db.Users.Where(f => f.WorkType == Enums.WorkType.Foreman).OrderBy(g => g.UserName).Select(h => new SelectListItem()
            {
                Value = h.Id,
                Text = h.FName + " " + h.LName,
                Selected = selectedForemanId == h.Id // Add Select variable that holds the Id of the current selected item that gets passed when the corresponding edit button is clicked
            });
        }


        public ActionResult CreateJobsPartialView()
        {
            ViewBag.foremen = GetForemen();

            return PartialView("CreateJobsPartialView");
        }

        protected override void Dispose(bool disposing)
        {
            if (disposing)
            {
                db.Dispose();
            }
            base.Dispose(disposing);
        }  
       
    

    }
}



```

At this point I would need to create a createjobspartialview for the Views/Jobs folder. This would eventually allow for the user to see the form to select a foreman.

```cshtml
@using System.Web.UI.WebControls
@using ConstructionNew.Models
@using ConstructionNew.Enums
@using ConstructionNew.Extensions
@using System.Collections.Generic






@model ConstructionNew.Models.Schedule



@{
    

    ViewBag.Title = "Foremen";
}

@using (Html.BeginForm())
{
    @*@Html.AntiForgeryToken()*@

    <div class="form-horizontal">
        <h4>Job Foreman Assignment</h4>
        <hr />
        @*@Html.ValidationSummary(true, "", new { @class = "text-danger" })*@


        <div class="form-group">
            @Html.LabelFor(model => model.Person.Id, htmlAttributes: new { @class = "control-label col-md-2" })
            <div class="col-md-10">
                @Html.DropDownList("Foremen", ViewBag.foremen as SelectList, "--Select Foreman--", new { style = "height:40px; width:450px;" })

                @*@Html.ValidationMessageFor(model => model.Person.Id, "", new { @class = "text-danger" })*@
            </div>
        </div>
        <div class="form-group">
                @Html.LabelFor(model => model.StartDate, htmlAttributes: new { @class = "control-label col-md-2" })
                <div class="col-md-10">
                    @Html.EditorFor(model => model.StartDate, new { htmlAttributes = new { @class = "form-control", style = "height:40px; width:450px;" } })
                    @*@Html.ValidationMessageFor(model => model.StartDate, "", new { @class = "text-danger" })*@
                </div>
        </div>
        
        <div class="form-group">
                @Html.LabelFor(model => model.EndDate, htmlAttributes: new { @class = "control-label col-md-2" })
                <div class="col-md-10">
                    @Html.EditorFor(model => model.EndDate, new { htmlAttributes = new { @class = "form-control", style = "height:40px; width:450px;" } })
                    @*@Html.ValidationMessageFor(model => model.EndDate, "", new { @class = "text-danger" })*@
                </div>
        </div>



        <div class="form-group">
            <div class="col-md-offset-2 col-md-10">
                <input type="submit" value="Create" class="btn btn-default" />
            </div>
        </div>
    </div>
}

```

I didn't include the snippet but this would be passed through the Jobs Create View page by action link that allowed for the user
to see an additional form that appears to be part of the same page which would allow for foreman selection and be bound to each 
new job assignment.

*Jump to: [Front End Stories](#front-end-stories), [Back End Stories](#back-end-stories), [Other Skills](#other-skills), [Page Top](#live-project)*


### Other Skills

* Teaming with a group of developers working to identify both front-end and back-end issues/bugs and finding solutions to improve usability and efficieny in an application.
* Attending stand-up meetings physically and remotely, discussing any questions or concerns with team members to improve overall organization and communication.
* Practice with Team/Pair Programming when a developer has issues solving a particular problem.

*Jump to: [Front End Stories](#front-end-stories), [Back End Stories](#back-end-stories), [Other Skills](#other-skills), [Page Top](#live-project)*


Thanks for viewing :)











































