# Schedule
![schedule](schedule.png)

Once a Schedule is added, the corresponding Recipe and Configuration will become read-only (except for the Recipe name), in order to prevent any unintentional changes that may affect the scheduled future Jobs. 

Note that incremental load is not automatically turned on for scheduled transformations, which means the full data set will be transformed in each iteration.