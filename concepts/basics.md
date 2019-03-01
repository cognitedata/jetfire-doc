# Basics
In Jetfire, a transformation consists of two parts: Recipe and Configuration. A transformation is executed in either of the following two cases: 1. when user explicitly performs a transformation by clicking on the "Transform" button, 2.  according to the user-defined schedule. Each execution of a transformation is called a Job.

## Recipe
Contains query and destination data type.

## Configuration
Contains destination project (API key), and database name + table name if the destination data type is "Raw". Currently, there is a one-to-one relationship<sup>[\*]</sup> between Recipe and Configuration.

## Schedule
Contains an interval that specifies when and how often the transformation should be executed. There is a one-to-one relationship<sup>[\*]</sup> between Configuration and Schedule. Once a Schedule is added, the corresponding Recipe and Configuration will become read-only (except for the Recipe name), in order to prevent any unintentional changes that may affect the scheduled future Jobs.

## Job
Contains information about an execution of the transformation, including start time, end time, executed query, destination, execution result, and metrics. Jobs are grouped by Recipes, (or equivalently, grouped by Configurations).

#

[\*]: Recipe, Configuration, Schedule are all editable, but they are all linked to unique transformation IDs. Thus, for example, an edited Recipe is still considered the same Recipe as the old one.