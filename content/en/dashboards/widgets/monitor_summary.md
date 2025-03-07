---
title: Monitor Summary Widget
kind: documentation
description: "Display a summary view of all your Datadog monitors, or a subset based on a query."
widget_type: "manage_status"
aliases:
- /graphing/widgets/monitor_summary/
further_reading:
- link: "/dashboards/graphing_json/"
  tag: "Documentation"
  text: "Building Dashboards using JSON"
---

The monitor summary widget displays a summary view of all your Datadog monitors, or a subset based on a query.

{{< img src="dashboards/widgets/monitor_summary/monitor-summary-overview.png" alt="monitor summary" >}}

## Setup

{{< img src="dashboards/widgets/monitor_summary/monitor-summary-setup.png" alt="monitor summary setup" style="width:80%;">}}

### Configuration

1. Select one of the three summary types: `Monitor`, `Group` or `Combined`
    - The `Monitor` summary type lists statuses and names of monitors matching the [monitor query][1]. Multi alert monitors have only one row in the results list and their status is the multi alert monitor's overall status. The Status Counts are the number of matching monitors with each status type.

    {{< img src="dashboards/widgets/monitor_summary/monitor_summary_type.png" alt="monitor summary type" style="width:80%;">}}

    - The `Group` summary type lists statuses, names, and groups of monitors matching the monitor query. Multi alert monitors are broken into several rows in the results list and correspond to each group and that group's specific status in the multi alert monitor. The `Group` summary type also supports `group` and `group_status` facets in its monitor query similar to the [Triggered Monitors][2] page. The Status Counts are the number of matching monitor groups with each status type.

    {{< img src="dashboards/widgets/monitor_summary/group_summary_type.png" alt="group summary type" style="width:80%;">}}

    - The `Combined` summary type lists the number of group statuses and names of the monitors matching the monitor query. Multi alert monitors have only one row in the results list like in the `Monitor` summary type but the groups column displays the number of groups in each status type instead of the monitor's overall status. Similar to the `Group` summary type, the `Combined` summary type also supports the `group` and `group_status` facets in its monitor query. The Status Counts still show the count of overall monitor statuses like in the `Monitor` summary type.

    {{< img src="dashboards/widgets/monitor_summary/combined_summary_type.png" alt="combined summary type" style="width:80%;">}}

2. Enter a monitor query to display the monitor summary widget over a subset of your monitors.

    **Note** In addition to the facets listed in the link above, the `Group` and `Combined` summary types also support the `group` and `group_status` facets for group-level searching, similar to the [Triggered Monitors][2] page.

#### Template variables

To use template variables created in your dashboard in the monitor summary search query, follow the same query format as the Manage Monitor page.

**Example**

1. Filtering on Monitor `scope` with a `$service` template variable.

   To leverage `scope` in the manage or triggered monitor page, you have to do `scope:service:web-store`.
   Therefore in the widget you have to do `scope:$service` to then apply the template variable value to the widget.

   {{< img src="dashboards/widgets/monitor_summary/templatevariable-example-scope.png" alt="Scope Template variable" style="width:80%;">}}


2. Filtering on Monitor `group` with a `$env` template variable.

   To leverage `group` in the manage or triggered monitor page, you have to do `group:env:prod`.
   Therefore in the widget you have to do `group:$env` to then apply the template variable value to the widget.

   {{< img src="dashboards/widgets/monitor_summary/templatevariable-example-group.png" alt="Group Template variable" style="width:80%;">}}

## Options

#### Display preferences

Choose to show only the `Count` of monitors per monitor status type, a `List` of monitors, or `Both`. The `Text` and `Background` options specify whether the status colors should be applied to the text or background of the Status Counts. The `Hide empty Status Counts` option, when enabled, only shows the Status Counts for statuses that have more than zero monitors in the result list.

{{< img src="dashboards/widgets/monitor_summary/display-preferences.png" alt="display preferences" style="width:80%;">}}

Selecting the `Show triggered column` option filters the results to monitors or monitor groups that are in a triggered state (`Alert`, `Warn`, or `No Data`) and sorts them from most recently triggered to least recently triggered. An additional column is added indicating the amount of time that has elapsed since the monitor/group last triggered.

{{< img src="dashboards/widgets/monitor_summary/monitor-summary.png" alt="display preferences" style="width:80%;">}}

#### Title

Display a custom title for your widget by checking the `Show a title` check box:

{{< img src="dashboards/widgets/monitor_summary/widget_title.png" alt="widget title" style="width:80%;">}}

You can optionally define the title's size and alignment.

## API

This widget can be used with the **[Dashboards API][3]**. See the following table for the [widget JSON schema definition][4]:

{{< dashboards-widgets-api >}}

## Further Reading

{{< partial name="whats-next/whats-next.html" >}}

[1]: /monitors/manage/
[2]: /monitors/manage/#grouped-results
[3]: /api/latest/dashboards/
[4]: /dashboards/graphing_json/widget_json/
