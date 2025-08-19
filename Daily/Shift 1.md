Uncompleted Task
```dataview
TASK FROM "metadata/Template/Daily"
	where meta(section).subpath = "Shift 1" and !completed
```
Completed Task
```dataview
task from "metadata/Template/Daily"
where completed and meta(section).subpath = "Shift 1"
```
