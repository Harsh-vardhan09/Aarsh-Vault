Canteen Service  
Application

## Requirements

### Functional requirements:-

- Initiator can create an event with date and details.
- Initiator can add the details like.  
    VIP  
    food type(High tea and lunch)
- selecting food types shows the relevant menu.
- Initiator submits the request for further approvals.
- It gets sent to the Lead and HR for approval or rejection with the feedback for rejection.
- If approved gets sent to the relevant caterer selected by creator for catering.
- After event it shows server and then the initiator can give feedback.
- caterer can propose new menu and admin confirms it for all other user.
- admin can generate MIS report for all the event that took place.
- admin assign roles and approves roles
    

### NON Functional Requirements

- Security for data leaks and other issues
- It should be able to perform in all the time
- High Availibility
- Clean Architecture
- Easy scaling

## TOOLS and Tech

| Module                   | Purpose                                                     | Technology                                                                                                              |
| ------------------------ | ----------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| Web frontend             | Dashboards for Initiator/Approver/HR/Admin                  | ASP.NET Core MVC or Blazor (keeps it pure .NET); React/Angular is a fine alternative if your team prefers a JS frontend |
| *Mobile app              | Submit/approve/track on the go, push alerts                 | Flutter (Dart), single codebase for Android & iOS, consuming the same REST API                                          |
| Backend API              | Business logic, RBAC, request routing                       | ASP.NET Core Web API (C#)                                                                                               |
| Data access / ORM        | Maps C# entities to MySQL                                   | Entity Framework Core + Pomelo.EntityFrameworkCore.MySql                                                                |
| Database                 | Persistent storage                                          | MySQL 8                                                                                                                 |
| Auth                     | Login, tokens, RBAC enforcement                             | ASP.NET Core Identity + JWT Bearer auth                                                                                 |
| Approval workflow engine | Manages the Lead → HR → Caterer chain and state transitions | Custom state machine in C# (or the Elsa/Workflow Core library if you want a visual workflow designer later)             |
| Caterer & menu module    | Onboarding, menu submission, admin approval queue           | ASP.NET Core Web API + EF Core                                                                                          |
| Notification service     | Email/push/in-app alerts on every status change             | MailKit (SMTP) for email, Firebase Cloud Messaging for mobile push                                                      |
| MIS/reporting module     | Filtered dashboards, exports                                | EF Core/raw SQL aggregation + ClosedXML (Excel export) / QuestPDF (PDF export)                                          |
| File storage             | Caterer menu images/attachments                             | Azure Blob Storage or local file storage, depending on hosting                                                          |
| *Hosting & CI/CD         | Deployment pipeline                                         | IIS or Azure App Service; GitHub Actions or Azure DevOps                                                                |
| *Caching (optional)      | Faster reads for menus/master data                          | Redis or ASP.NET IMemoryCache                                                                                           |

`*-> phase 2`

---

![[_Excalidraw/Projects.md#^frame=eIiIA58vJ9BuZXpRlFYYf|100%]]

