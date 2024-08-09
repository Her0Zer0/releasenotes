# releasenotesv2

Release Notes is a globally scoped app that allows users to create notes using a form link option that provides a modal form for data entry. Included with the update set is an example portal widget that pulls in information using either a filter or configuration rule. The Release Note table allows references to any table available in the instance and links them with a Document ID. 
<br/><br/>
**Process Overview:**
When a user completes their story and wishes to create a release note to provide information about the latest development, they can use the form link Add/Edit Release Note to display a modal form for entry. Once the release note is submitted and is in a published state, it can be seen in the portal widget if it matches the configuration rule criteria.

<br/><br/><br/>
## Getting Started

### Activating Add/Edit Release Note UI Action
After committing the import set to your instance. Navigate to **System Definition > UI Actions** and filter the list for **Add/Edit Release Note**. 

This UI Action is inactive by default because it is set on the Global table. If activated in the current state, this Form Link will show on every form. If you want it on a specific form, change it to the table you wish to have it available on or make your changes and use **Insert and Stay** in the context menu to create a copy. 

>**NOTE:** _The form link is only visible to users with the rn_editor role and after the record has been set to inactive by default._ 

<br/><br/>
### Creating a Release Note Config Rule

**Navigate to Release Notes > Release Note Config Rules** and click on **New**.

Fill in the following fields:

**Name**: Used for identifying rules. 

**Table**: The table to query against for release notes.

**Filter By**: Used to narrow the search of release notes. 

**Description**: Some details on what is expected from this rule. 

Then click **Submit**.

>**NOTE**: An example is included for Change Request [change_request] with a simple filter. This filter allows the Release Notes widget to pull Published Release Notes for Change records that match that filter. 

<br/><br/>
### Updating the Portal Page

Navigate to **Release Notes > Demo**. 

On click of the demo Portal Page Release Notes [release_notes] is visible. If notes are not available or the widget doesn't have a filter you should see an informative message. 

To see Release Notes by _Release Note Config Rules_, one should be added on the portal page. 

Navigate to **Service Portal > Pages** and search for **release_notes** in the **ID** field. Open the record and scroll to the bottom of the page. Once there, click the Form link **Open in Designer**.

In the **Page Designer** click the pencil icon to edit the widget options. Add your **Release Note Config Rule** and click save. 

After creating release notes and publishing those notes on records that match the Release Note Config Rule filter, navigate back to **Release Notes > Demo** and view the changes.

<br/><br/>
### Creating a Release Note. 

After activating the UI Action **Add/Edit Release Note** and a **Release Note Config Rule**, find/create a record that meets your criteria. 

Scroll to the bottom of the form and click on the **Add/Edit Release Note**. 

A form modal will appear and you will need to fill in the following fields. 

**Subject**: Title line of your release note. 

**Extra Content**: Link that wraps the subject and opens a new window if you want to provide more content to your readers that can't be said in a release note. 

**Type**: The type of change/enhancement/feature that the release note is trying to describe. Options available are New Feature, New Product, Product Change, API, UI/UX, and Bug Fix.

**Content**: This is the note itself and is rendered as HTML as provided in the note. 

**State**: The lifecycle of the release note. Only Published notes can be seen in the widget. 

**Document**: The record the release note is for. Autopopulated if using the UI Action. 

**Referenced Table**: The table that contains the document record. Autopopulated if using the UI Action. 
<br/><br/>
### Using the Release Notes With Filter Widget

This widget was provided as an example if you have Agile Development installed on your instance. To use it navigate to **Service Portal > Pages** and find the **Release Notes** page. Open the page in **Page Designer** and add the widget from the widget list on the left panel. 

Then create release notes that have a Release [rm_release_scrum] in a **State of Complete**. 

The story notes should be available if they are in a published state on the **Release Notes > Demo** page. 

<br/><br/><br/>
## Components Installed

### Roles
Name: rn_admin<br/>
Description: Provides read/create/write/delete access to the release notes application. rn_admin can also read/create/write/delete config rules. 

Name: rn_editor<br/>
Description: Provides read/create/write access to release notes.

Name: rn_viwer
Description: Provides read access to release notes. 

<br/><br/>
### Group
Name: Release Note Viewer<br/>
role: rn_viewer<br/>

Name: Release Note Editor<br/>
Role: rn_editor<br/>

Name: Release Note Admin<br/>
Role: rn_admin<br/>

<br/><br/>
### Table
Name: Release Notes [u_release_notes] - Contains release notes and a reference to the table and record that created them.<br/>
Can See: rn_editor, rn_viewer, rn_admin<br/>
Can Create/Write: rn_editor, rn_admin<br/>

Name: Release Note Config Rules [u_release_note_config_rule] - Contains configuration style rules that act as a wrapper for easier grouping. <br/>
Can See: rn_editor, rn_viewer, rn_admin<br/>
Can Create/Write: rn_admin<br/>

<br/><br/>
### Business Rule: 
Name: One Note Per Document<br/>
Action: Aborts and shows an error message on the Release Note record if more than one note is attempted for insertion for the same document.<br/>

<br/><br/>
### Script Includes:
Name: ReleaseNote - Base script for methods concerning release notes. <br/>
Name: ReleaseNoteUtil - Extended from ReleaseNote to provide easier override methods without having to modify multiple scripts in the platform. <br/>

<br/><br/>
### Modules: 
Release Notes - Main application module. <br/>
All - Provides a list of release notes that have been created. <br/>
Can See: rn_editor, rn_viewer, rn_admin<br/>

Demo - Shows a portal page to see the release notes widget after configuration and adding notes for display. <br/>
Can See: rn_editor, rn_admin<br/>

Release Note Config Rules - Provides a list of rules to build conditional statements while pulling release notes from the source table. Think of this as a wrapper. <br/>
Can See: rn_admin

<br/><br/>
### UI Actions: 
Name: Add/Edit Release Notes 
Table: [global] Inactive by default. 
Action: Opens the form for Release note submission. 
Can Execute: rn_editor

<br/><br/>
### Client Scripts: 
Name: Table is read-only if populated
Table: [u_release_notes]
Action: Onload, if the table is populated the Table field is set to read-only. 

Name: Mandatory if Publishing
Table: [u_release_notes]
Action: OnChange, if the State is set to Published the fields Content, Subject, Type, and Table are set as mandatory. 

<br/><br/>
### Portal Page
Name: Release Note Demo<br/>
ID: release_notes<br/>
Example: /sp?id=release_notes

<br/><br/>
### Widgets
Name: Release Note<br/>
ID: release_note<br/>
Action: Provide the base layout of each note recorded. 

Name: Release Note List<br/>
ID: release_note_list<br/>
Action: Provides the querying and filtering features to display release notes. 

Name: Release Notes With Filter
ID: release_note_w_filter<br/>
Action: Provides dropdown selectable options for Story [rm_story] release notes if the Release [rm_release_scrum] has been completed.<br/>
Only useful if you have Agile Development installed in your instance.  

