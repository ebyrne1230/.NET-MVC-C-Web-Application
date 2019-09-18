# .NET-MVC-C-Web-Application
Work done with Prosper IT Consulting Dev Ops Team

This is the placeholder for live project work done for a Portland Based company called Erectors, Inc.
As a team we built a company Intranet Web Application using C# leveraging the power of .NET frameworks with MVC.
This project included end to end work.


Notes

For my C# Live Projects there were 2 different projects I was able to do work on.

The first sprint was for the file ConstructionNew.sin (I housed it C:\Users\Student\source\repos\ebyrne1230\TechAcademyCSharpLiveProject)

I was able to work on and complete 4 stories

Titles:

EB-4480-ModifyDateDisplay

EB-4481-Back2DashboardLinks

EB-4488-ModifyScheduleIndex

EB-AddForemanFromJobView


Organzing By Story

ModifyDateDisplay

The Story Description:  The schedule start and end dates are currently set to the display mode of yyyy-mm-dd.  This MUST REMAIN the same
so the editors for those dates can get the current value.  Use formatting statements to change the way these dates show up in the various schedule views (ie the dashboard and the index list)  Hint: This most likely needs to be done in the cshtml, but if you can get it to work on the controller side, that would be epic!

So initially for the schedule views, the webpage would display with the yyyy-mm-dd format for each scheduled job.  I was tasked with the challenge to go through the code in the various view pages and adjust the current formatting statements to render the wanted result.

Provide before snippets and after snippets to show the gained resolution.

The changes made were on the Schedule Index view page, the Schedule Details View page, and on the Schedule Delete View page.



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


Inside the Views/Schedules Folder

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































































