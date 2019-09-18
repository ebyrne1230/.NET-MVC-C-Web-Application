# .NET-MVC-C-Web-Application
Work done with Prosper IT Consulting Dev Ops Team

This is the placeholder for live project work done for a Portland Based company called Erectors, Inc.
As a team we built a company Intranet Web Application using C# leveraging the power of .NET frameworks with MVC.
This project included end to end work.


# Live Project

## Introduction

I took part in a non-paid internship with Prosper IT Consulting working as a software developer as part of a Dev Ops team that worked on building a web application that would perform as an intranet for a Portland based construction company called Erectors, Inc.

This project leveraged the power of .NET frameworks and MVC.  The main programming language used was C# in addition to the front-end languages.

Below are descriptions of stories I helped work on in an AGILE environment along with code snippets and navigation links.  

*Jump to: [Front End Stories](#front-end-stories), [Back End Stories](#back-end-stories), [Other Skills](#other-skills)*

## Front End Stories
* [ModifyDateDisplay](#modify-date-display)
* [Back2DashboardLinks](#back-2-dashboard-links)


## Back End Stories
* [ModifyScheduleIndex](#modify-schedule-index)
* [AddForemanFromJobView](#add-foreman-from-job-view)



### ModifyDateDisplay

The Story Description:  The schedule start and end dates are currently set to the display mode of yyyy-mm-dd.  This MUST REMAIN the same
so the editors for those dates can get the current value.  Use formatting statements to change the way these dates show up in the various schedule views (ie the dashboard and the index list)  Hint: This most likely needs to be done in the cshtml, but if you can get it to work on the controller side, that would be epic!

So initially for the schedule views, the webpage would display with the yyyy-mm-dd format for each scheduled job.  I was tasked with the challenge to go through the code in the various view pages and adjust the current formatting statements to render the wanted result.

Provide before snippets and after snippets to show the gained resolution.

The changes made were on the Schedule Index view page, the Schedule Details View page, and on the Schedule Delete View page.


Inside the Views/Schedules Folder I added formatting statements to each of these documents:

Index.cshtml

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

Details.cshtml

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

Delete.cshtml (Schedule Delete View Page)


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






























































