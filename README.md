# Implement-Client-Script-UI-Policy-Incident-
Implement and configure Client Scripts and UI Policies for the Incident form to automate field behavior, enforce client-side validations, control field visibility/mandatory/read-only states, and improve the overall user experience and data consistency.
# ServiceNow Incident Form Automation Using Client Scripts & UI Policies

## Project Overview

This project focuses on implementing and configuring Client Scripts and UI Policies in the ServiceNow Incident Management module to automate field behavior, enforce client-side validations, control field visibility, mandatory and read-only states, and improve overall user experience and data consistency.

The project demonstrates how ServiceNow client-side scripting and UI policies can be used to streamline Incident management and minimize incomplete or invalid records.

## Objectives

* Configure UI Policies to control Incident form fields dynamically.
* Make Assignment Group mandatory for High Impact incidents.
* Control Urgency field behavior based on Impact.
* Implement onChange Client Scripts for dynamic form interactions.
* Implement onSubmit Client Scripts for form validation.
* Implement onCellEdit Client Scripts for list-level validation.
* Improve data consistency and user experience.

## Technologies Used

* ServiceNow Platform
* Incident Management (ITSM)
* UI Policies
* UI Policy Actions
* Client Scripts
* JavaScript
* ServiceNow GlideForm API

## Project Modules

### 1. High Impact Control – UI Policy

**Objective:** Make Assignment Group mandatory when Incident Impact is High.

Configuration:

* Table: Incident
* Condition: Impact is High (1)
* UI Policy Action: Assignment Group → Mandatory
* Reverse if false: True

**Expected Result:** Assignment Group becomes mandatory when Impact is High and reverts when the condition is no longer met.

### 2. Urgency Control – UI Policy

**Objective:** Configure dynamic Urgency field behavior based on Incident Impact.

Configuration:

* Table: Incident
* Condition: Impact is High
* UI Policy Action: Urgency → Mandatory
* Reverse if false: True

**Expected Result:** Urgency becomes mandatory for High Impact incidents.

### 3. onChange Client Script

**Objective:** Display a notification when Incident Impact changes to High.

```javascript
function onChange(control, oldValue, newValue, isLoading) {

    if (isLoading || newValue == '') {
        return;
    }

    if (newValue == '1') {
        g_form.addInfoMessage(
            'High Impact Incident: Please assign the appropriate group and urgency.'
        );
    }
}
```

### 4. onSubmit Client Script

**Objective:** Prevent submission of High Impact incidents without an Assignment Group.

```javascript
function onSubmit() {

    var impact = g_form.getValue('impact');
    var assignmentGroup = g_form.getValue('assignment_group');

    if (impact == '1' && assignmentGroup == '') {

        g_form.addErrorMessage(
            'Assignment Group is mandatory for High Impact incidents.'
        );

        g_form.setFocus('assignment_group');

        return false;
    }

    return true;
}
```

### 5. onCellEdit Client Script

**Objective:** Display a warning when Impact is changed to High directly from the Incident list.

```javascript
function onCellEdit(sysIDs, table, oldValues, newValue, callback) {

    if (newValue == '1') {

        alert(
            'You are setting this Incident to High Impact. Please verify the assignment group.'
        );

    }

    callback(true);
}
```

## Testing Scenarios

| Test Case                       | Expected Result                    |
| ------------------------------- | ---------------------------------- |
| Set Impact to High              | Assignment Group becomes mandatory |
| Change Impact to Medium         | Mandatory state is reverted        |
| Set Impact to High              | Urgency becomes mandatory          |
| Change Impact to High           | Information message appears        |
| Submit without Assignment Group | Submission is blocked              |
| Submit with Assignment Group    | Submission is allowed              |
| Edit Impact from list view      | Warning message appears            |

## Key Features

* Dynamic field behavior
* Mandatory field enforcement
* Client-side form validation
* Real-time user feedback
* List-level field change handling
* Improved Incident data consistency
* Enhanced user experience

## Learning Outcomes

Through this project, I gained practical experience in:

* Configuring ServiceNow UI Policies and UI Policy Actions.
* Developing Client Scripts using JavaScript.
* Understanding onLoad, onChange, onSubmit, and onCellEdit execution types.
* Using the GlideForm API.
* Implementing client-side validations.
* Automating Incident form behavior.

## Conclusion

This project demonstrates the effective use of ServiceNow Client Scripts and UI Policies to automate Incident form behavior and enforce validation rules. By combining dynamic field controls, mandatory field enforcement, and client-side scripting, the solution improves data quality, reduces incomplete records, and enhances the overall Incident management experience.

---

**Project Domain:** IT Service Management (ITSM)
**Platform:** ServiceNow
**Module:** Incident Management
**Project Type:** Group Project
