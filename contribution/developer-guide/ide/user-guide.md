---
description: Brief description of the different views and the functions that they provide.
---

# User guide

## Layout Overview

![Overview of application areas](../../../.gitbook/assets/image%20%286%29.png)

The layout consists of two different areas. There is a navigation bar on the left and the application window that holds the current view. 

### Ruleset Overview

The first thing you see is the ruleset overview. 

![Ruleset overview](../../../.gitbook/assets/image%20%2817%29.png)

Clicking the plus card on the top left adds another ruleset to the collection. Clicking on any of the other cards is going to open the selected ruleset. The rulesets are always going to be ordered by the date they were last edited. This means that the ruleset on top is always going to be the one that was edited most recently.

### Ruleset Editor

When a ruleset was selected the editor loads.

![Ruleset editor](../../../.gitbook/assets/image%20%2836%29.png)

This view shows the title and description of the ruleset on the top. The left column contains the editable ruleset. The right column is split into two parts again. The top part is the schema editor that displays the current schema and allows for the schema to be edited. The bottom part shows all of the variables that are defined in the ruleset. 

#### Editing title and description

Both the title and the description are inline editable. You can just select them and change them without the need to manually save.

![Edit title and description](../../../.gitbook/assets/image%20%2833%29.png)

#### Editing the ruleset

The area to edit the ruleset is basically an enhanced textfield. It highlights certain keywords in different colors to help you distinguish different parts of a rule. 

![Suggestions and autocomplete](../../../.gitbook/assets/image%20%2843%29.png)

Pressing `Ctrl`+`Space` shows suggestions for what you might want to add and adds templates to create a rule, variable or constrained rule. 

#### Editing the schema

The schema that is displayed on the right side can be edited as well. 

![Schema editor](../../../.gitbook/assets/image%20%2882%29.png)

The different colors correspond to the available types. Blue meaning it is a **Text**, green meaning it is a **Number** and red meaning it is a **Yes/No** value.

Hovering over a chip using the mouse makes a delete button appear. Clicking the chip shows a dialog to edit this attributes name, type and value.

![Edit an attribute](../../../.gitbook/assets/image%20%2814%29.png)

The same dialog is also shown when adding a new attribute.

### Test suite \(design preview\)

The test suite page is a work in progress that does not have any actual functionality yet.

![Test suite](../../../.gitbook/assets/image%20%2819%29.png)

The layout consists of a read only version of the editor to preview the current ruleset, two gauges that show the percentage of rules that are covered by the test cases and the percentage of test cases that succesfully passed.

The data displayed in the table is randomly generated and has no connection to the displayed percentages. For future versions it is planned that the table items are editable and that other rows can be added in order to test the created ruleset.



