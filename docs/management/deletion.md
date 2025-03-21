---
layout: page
title: Content Deletion
parent: Management
nav_order: 3
---

Objects may be deleted from the repository for the following reasons:

- Errors during ingest
- File replacement
- Deaccession decisions made by other programs

## Starting a Deletion Process

1. Discuss the need for a deletion with the rest of the Digital Repository team.
2. Confirm that the objects are not referenced by external systems.
   - If objects have records in other systems, discuss the update process with affected systems (see below).
3. Create a new entry in the Deletion Log spreadsheet. Describe the reason for the deletion and the objects to be deleted.
4. In the top-level `ToDelete` SO, create a new SO that will contain the objects to be deleted. Name the SO with the ID number from the Deletion Log spreadsheet.
5. Move the objects to the new SO.
   - For small numbers of objects (less than 10), you can perform the move via drag-and-drop or through context menus.
   - For large numbers of objects, use the `Entity` API to move the objects.
6. Start a `Deletion` workflows for the SO that contains the objects to be deleted. In the description field, copy the description from the Deletion Log.
7. When the workflow is ready for approval, send the link to a team member.
8. If necessary, produce reports for external systems to be updated.

## Completing a Deletion Process

1. Review the description of the deletion in the workflow and the log.
2. Confirm that objects to be deleted match the description.
3. Approve the deletion.
4. Log the date in the spreadsheet.

## Updating External Systems

Once an object is referenced in a descriptive record such as a finding aid or a catalog record, a much higher justification is needed to delete it.

### SPEC

1. Send a report of all objects to be deleted with SPEC staff
2. SPEC staff will update/deactivate object records for deleted materials.
