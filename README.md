# Salesforce Timesheet Application

Welcome to the repository for my innovative Salesforce project that powers the Timesheet application, developed during my Salesforce Internship at "Intermino - OdooTM and SalesforceTM partner." Explore the intricacies of my Timesheet Application.

<img src="https://github.com/rijadhmz/Salesforce-Timesheet-App/blob/main/images/salesforce.png?raw=true" width="100" height="100">

## Data Model

This project showcases a data model with four custom objects: Employee, Project, Timesheet, and Timesheet Week.

![Data-Model](https://github.com/rijadhmz/Salesforce-Timesheet-App/blob/main/images/data-model.png?raw=true)

### Employee Object

Effectively manage employee details, including EmployeeID, Name, Department, Email, Position, Supervisor (linked to another employee), and Token. This object ensures efficient employee tracking within the system.

### Project Object

Organize and oversee projects with fields like ProjectID, Name, Description (long text area), and Project Manager (linked to an employee). This object enhances project management and organization.

### Timesheet Object

Empower employees to log timesheets with fields like TimesheetID, TimesheetWeekID (linked to Timesheet Week), EmployeeID (linked to Employee), ProjectID (linked to Project), Task, Task Description, Day (picklist for day selection), and Hours. This object enables precise time tracking for tasks and projects.

### Timesheet Week Object

Structure timesheets by week, linking them to employees and projects with fields like TimesheetID, EmployeeID (linked to Employee), ProjectID (linked to Project), TaskID (linked to Task), and Week (date for the week). This object streamlines timesheet organization based on weeks.

The data model is the cornerstone of the Timesheet application, offering enhanced employee information management, project organization, and accurate time tracking.

## Lightning App for Admins on Salesforce

Introducing 'Timesheet Admin,' my Lightning App tailored for administrators on Salesforce. Seamlessly manage Timesheet records, timesheet weeks, projects, and employees in one comprehensive solution.

![Admin-App](https://github.com/rijadhmz/Salesforce-Timesheet-App/blob/main/images/admin-app.png?raw=true)

## HTML Email Template

A pivotal aspect of this project is the ability to send personalized timesheet reminders to employees. Witness the impact of the HTML email template designed for delivering consistent and professional reminders.

- **Company Logo:** Project a professional image with the inclusion of the company logo, reinforcing brand identity.
- **Personalized Greeting:** Establish a personal touch with dynamically populated employee names in greetings.
- **Timesheet Application Link:** Provide easy access to the Timesheet App through direct links.
- **Unique Employee Links:** Explore the special feature that sends personalized emails to each employee with a unique link. This link, crafted using their unique token, ensures secure and personalized access to the Timesheet App.

![Email-Template-HTML](https://github.com/rijadhmz/Salesforce-Timesheet-App/blob/main/images/email-template-html.png?raw=true)

![Email-Template-Preview](https://github.com/rijadhmz/Salesforce-Timesheet-App/blob/main/images/email-template-preview.png?raw=true)

## Flow Overview

Dive into the details of the automation flow, which is at the heart of the reminder system.

- **Fetching Employee Records:** Begin by fetching all employee records using the 'Get Records' element, allowing individualized interactions.
- **Personalized Email Reminders:** Utilize a loop to send tailored email reminders to each employee, featuring their name and Timesheet App link.
- **Efficient Timesheet Completion:** This flow guarantees timely and personalized reminders, boosting timesheet completion rates.

![Flow](https://github.com/rijadhmz/Salesforce-Timesheet-App/blob/main/images/flow.png?raw=true)

## Timesheet App

Experience the user-friendly Timesheet App, accessed through the unique email link.

- **Branded Interface:** The app boasts company branding, including logos and colors, providing a familiar environment.
- **Personalized Experience:** The app dynamically displays employee names, fostering a personal connection.
- **Easy Timesheet Entry:** Seamlessly record timesheets by selecting timesheet weeks, tasks, projects, and hours.
- **Real-time Validation:** Receive immediate feedback on data input, correcting errors and missing information swiftly.
- **Submission Assurance:** Upon successful submission, employees receive confirmation messages, instilling confidence in their saved timesheets.

![Timesheet-App](https://github.com/rijadhmz/Salesforce-Timesheet-App/blob/main/images/timesheet-app.png?raw=true)

# Visualforce Page and Apex Controller: Timesheet Submission

In order to make my app work properly, I needed a Visualforce page and its accompanying Apex controller that facilitate seamless timesheet submission. This section explains the structure and functionality of the code.

## Code Explanation

### External Dependencies

The Visualforce page incorporates a Bootstrap CSS file (bootstrap.min.css) to enhance the styling of the interface.

### Navigation Bar

The navigation bar features the company logo `<img>` and a personalized greeting `<h1>`. The greeting message dynamically displays the employee's name using merge syntax `{!employeeName}`.

### Form Section

The <apex:form> tag encapsulates the form elements designed to capture timesheet data. This form includes input fields for week selection, task description, project assignment, and hours logged. Each input field is associated with a corresponding property in the Apex controller through value="{!timesheetRecord.fieldName}".

### Error and Info Messages

Two <div> sections handle error and info messages based on specific conditions. The error message section uses the class alert alert-danger and appears when hasErrors is true. Similarly, the info message section employs the class alert alert-info and is visible when hasInfo is true. Within each section, messages are displayed using the <apex:repeat> tag, which iterates over the errorMessages and infoMessages lists.

### Submit Button

The <apex:commandButton> labeled "Submit" triggers the saveTimesheetRecord action in the Apex controller when clicked.

### Apex Controller Code Explanation

#### Controller Properties

The Apex controller defines several properties that store data and control message display:

- `timesheetRecord`: Holds the timesheet record being created or modified.
- `hasFilledTimesheet`: Boolean flag indicating if the user has already filled in the timesheet for the week.
- `hasErrors`: Boolean flag indicating the presence of form submission errors.
- `hasInfo`: Boolean flag indicating the existence of info messages.
- `errorMessages`: List of error messages for display in the Visualforce page.
- `infoMessages`: List of info messages for display in the Visualforce page.
- `token`: Stores the token value extracted from the URL.
- `employeeName`: Contains the employee's name based on the token.

#### Constructor

The constructor is responsible for initializing controller properties. It creates a new Timesheet__c instance for the timesheetRecord property, initializes errorMessages and infoMessages lists, retrieves the token from the URL, and fetches the employee's name based on the token using the getEmployeeNameFromToken helper method.

#### hasFilledTimesheet Method

This method checks whether the user has already completed the timesheet for the week. It verifies the existence of a non-null TimesheetWeekID__c and EmployeeID__c in the timesheetRecord. Additionally, it queries the database to identify any existing timesheet records with matching week and employee values.

#### saveTimesheetRecord Method

Triggered upon clicking the "Submit" button, this method clears previous error and info messages and resets flags. It attempts to save the timesheet record through these steps:

- Sets EmployeeID__c based on the token from the URL.
- Checks whether the timesheet is already filled using the hasFilledTimesheet method.
- If already filled, adds an error message and sets hasErrors to true.
- If not filled, inserts the timesheet record, adds an info message, and sets hasInfo to true.
- Handles exceptions by adding an error message and setting hasErrors to true.

#### Helper Methods

- `getEmployeeIdFromToken`: Retrieves the employee's ID based on the token.
- `getEmployeeNameFromToken`: Retrieves the employee's name based on the token. These methods query the Employee__c object using the token to retrieve relevant data.

