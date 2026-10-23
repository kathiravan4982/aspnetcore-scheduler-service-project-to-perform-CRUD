# Service Project to Perform CRUD Operations on Scheduler Event Data

## Overview

This sample shows how to build a REST service that performs create, read, update, and delete operations on scheduler event data with ASP.NET Web API OData on .NET Framework 4.5, storing its data in SQL Server LocalDB. The `EventDatasController` class exposes create, read, update, and delete operations over an event store, and the `ScheduleDataEntities1` Entity Framework database context persists each change. The service exposes its endpoints over an OData route so that a client application, such as a calendar or scheduler page, can load, add, edit, and remove appointments over HTTP.

## Key Features

- Lists every event in an OData feed at the `odata/EventDatas` route, with each record pulled from SQL Server through an Entity Framework `DbSet` (the table-tracked collection of events).
- Provides full create, read, update, and delete operations for event records through the `GetEventDatas`, `Post`, `Patch`, and `Delete` actions of the `EventDatasController` class.
- Creates a new event through a request that posts to the `Post` action, adds it with the Entity Framework `Add` method, and assigns the next available identifier before saving.
- Edits an existing event that is looked up by its identifier with the `Find` method, applies the submitted changes for the subject, times, location, description, and recurrence fields, and saves them to the database.
- Deletes an event selected by its key after an OData delete request, calls the Entity Framework `Remove` method on the events `DbSet`, and returns to the service.
- Filters appointments by range with a second `GetEventDatas` overload that accepts `StartDate` and `EndDate`, returning only the records that fall inside the requested window.
- Converts incoming start and end times to local time before persisting them, and handles cross-origin requests so a separate front-end page can call the service.

## Prerequisites

- Visual Studio 2022 or Visual Studio Code
- .NET SDK compatible with the project's target framework
- SQL Server 2017 or later / SQL Server Express / SQL Server LocalDB

## How to Run the Project

### Visual Studio 2022

1. Clone or download this repository.
2. Open the project file:

   `Restful_Crud.csproj`

3. Update the SQL Server connection string under the `connectionStrings` section in `Web.config` with the server name, database name, and authentication details for a database that you are authorized to access.
4. Restore the NuGet packages by rebuilding the project.
5. Build the project.
6. Run the application using `Ctrl+F5`.
7. Open the application URL displayed by Visual Studio after launch.
8. Verify that the service starts under IIS Express and that requesting the `odata/EventDatas` route returns the stored event data as JSON.

### Visual Studio Code

1. Clone or download this repository.
2. Open the repository folder in Visual Studio Code.
3. Open the integrated terminal.
4. Restore the NuGet packages listed in `packages.config`:

```bash
nuget restore
```

5. Build the project.
6. Run the project on IIS Express, since this project requires a web host and is not launched with the `dotnet run` command.
7. Open the local URL displayed after the application starts.
8. Verify that requesting the `odata/EventDatas` route returns the stored event data as JSON.

## Project Structure

- `Controllers/EventDatasController.cs` — OData controller that defines the create, read, update, and delete actions for scheduler event data.
- `App_Start/ODataConfig.cs` — registers the OData model and maps the OData service route for the event data entity set.
- `Models/EventData.cs` — entity class that models an event with subject, start and end time, time zone, location, description, and recurrence fields.
- `Models/EventModel.edmx` — Entity Framework model that describes the event data schema and its database mapping.
- `Models/CrudData.cs` — model that extends the event data with the properties accepted by insert and update requests.
- `App_Data/ScheduleData.mdf` — LocalDB database file that stores the sample event data.

## Support and Feedback

- To report an issue specific to this sample, open a GitHub issue in this repository.
- Official documentation: [ASP.NET Web API documentation](https://learn.microsoft.com/en-us/aspnet/web-api/overview)
