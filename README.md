# content-movement-testing
Allows external organisation to test for content movement within International SNOMED CT in the current authoring cycle, via a set of ECL statements.

## Alert levels
The email that is produced will identify an alert level which is keyed to the amount of content movement detected.   This may be configured per ECL statement.

## Folder Structure
    <folder> - Entity name. For example "uk-nhs" the name of this folder will be passed to the report along with an email distribution list and (optionally) the name of a high usage list.
      <folder> - Category.  For example "procedures" - each folder at this level will appear in a separate tab in the final report.
        <filename.json> - each json file will contain an ECL statement that is designed to detect if the model has been changed in a particular section of content

## Configuration File


## ECL File
    {
        "name": "Xrays using contrast",
        "ecl" : "<< 168537006 |Plain X-ray (procedure)| : { 260686004 |Method (attribute)| = 312254007 |Plain X-ray imaging - action (qualifier value)|, 424361007 |Using substance (attribute)| = 419098001 |Radiographic imaging contrast media (substance)| }",
        "alertNew": { count : 10,
                            setLevel : 3 },
        "alertInactive": { count : 5,
                            setLevel : 4 },
        "highUsageAlertInactive": { count : 1,
                            setLevel : 7 },
        "alertMovedOut": { count : 1,
                            setLevel : 7 },
	"alertMovedIn": { count : 3,
                            setLevel : 6 },
        "highUsageAlertMovedOut": { count : 1,
                            setLevel : 9 }
	"highUsageAlertMovedIn": { count : 1,
                            setLevel : 9 }
    }

In this configuration the count is the number of concepts meeting that condition that will result in the alert level being set to the value shown by 'setLevel'. The types of alert are as follows:

| Item | Meaning |
| --- | --- |
| alertNew | Detects newly created concepts appearing in this ECL selection |
| alertInactive | Detects concepts previously appearing in this ECL selection, that have been inactivated in this authoring cycle |
| highUsageAlertInactive | As per alertInactive, but triggered when the concepts appear in the specified list of 'high usage' content |
| alertMovedOut | Detects when concepts remain active, but due to changes in modelling, are no longer captured by this ECL selection |
| alertMovedIn | Detects concepts that have previously existed, but due to changes in modelling, are now captured by this ECL selection when they were not previous included |
| highUsageAlertMovedOut | As per alertMovedOut, but triggered when the concepts appear in the specified list of 'high usage' content |
| highUsageAlertMovedIn | As per alertMovedIn,  but triggered when the concepts appear in the specified list of 'high usage' content |

## Report Output

This report will send an email to the distribution list supplied.   The subject of the email will indicate the alert level and the email will contain a link to a GoogleSheet.  The GoogleSheet can be viewed by anyone who has the link, and a Google account is not required.

In addition to a tab for each , a tab will be produced that lists concepts added and removed, with an indicator if a removed concept has been identified as 'high usage'.
