#  View Queries for KCNA Notes

## Cloud Native Concepts

### Simple test


```dataview
LIST
FROM "1 Cloud Native Concepts"
```

```dataview
TABLE
FROM "1 Cloud Native Concepts/Cloud Native Concepts/Benefits of Cloud Native"
```

```dataview
list
from "../../KCNA-notes"
where file.name = "1 Cloud Native Concepts"
```

### Query 1: List All Cloud Native Philosophies

```dataview
table section as "Philosophy", text as "Description"
from "KCNA-notes"
where contains(file.name, "Cloud Native Concepts") and contains(section, "Philosophies")
```

### Query 2: Benefits of Cloud Native

```dataview
list
from "KCNA-notes"
where contains(file.name, "Cloud Native Concepts") and contains(section, "Benefits of Cloud Native")
```

### Query 3: Misconceptions About Cloud Native

```dataview
list
from "KCNA-notes"
where contains(file.name, "Cloud Native Concepts") and contains(section, "Misconceptions About Cloud Native")
```

### Query 4: Key Practices in Cloud Native Development

```dataview
table section as "Practice", text as "Details"
from "KCNA-notes"
where contains(file.name, "Cloud Native Concepts") and contains(section, "Key Practices in Cloud Native Development")
```

### Query 5: Challenges Addressed by Cloud Native

```dataview
list
from "KCNA-notes"
where contains(file.name, "Cloud Native Concepts") and contains(section, "Challenges Addressed by Cloud Native")
```

### Query 6: Ecosystem and Governance

```dataview
table section as "Organization", text as "Details"
from "KCNA-notes"
where contains(file.name, "Cloud Native Concepts") and contains(section, "Ecosystem and Governance")
```

### Query 7: KCNA Exam Details

```dataview
table section as "Exam Section", text as "Details"
from "KCNA-notes"
where contains(file.name, "Cloud Native Concepts") and contains(section, "KCNA Exam")
```

### Query 8: Pro Tips for KCNA Exam

```dataview
list
from "KCNA-notes"
where contains(file.name, "Cloud Native Concepts") and contains(section, "Pro Tip")
```

### Query 9: Tags Used in the Note

```dataview
list
from "KCNA-notes"
where file.name = "1 Cloud Native Concepts"
```

### Query 10: Links to External Resources

```dataview
list
from "KCNA-notes"
where contains(file.name, "Cloud Native Concepts") and contains(text, "http")
```
