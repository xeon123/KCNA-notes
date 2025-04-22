# Dataview Queries for KCNA Notes

```dataview
TABLE title, category, created
FROM ""
WHERE contains(topics, "Kubernetes")
SORT created DESC

```

## Query 1: List All Cloud Native Philosophies

```dataview
TABLE title, created
FROM ""
WHERE contains(tags, "cloud-native") OR contains(topics, "Cloud Native")
SORT created ASC
```

## Query 2: Benefits of Cloud Native

```dataview
TABLE
FROM "1 Cloud Native Concepts"
WHERE contains(text, "### Benefits of Cloud Native")
```

## Query 3: Misconceptions About Cloud Native

```dataview
list
from "KCNA-notes"
where contains(file.name, "1 Cloud Native Concepts") and contains(section, "Misconceptions About Cloud Native")
```

## Query 4: Key Practices in Cloud Native Development

```dataview
table section as "Practice", text as "Details"
from "KCNA-notes"
where contains(file.name, "1 Cloud Native Concepts") and contains(section, "Key Practices in Cloud Native Development")
```

## Query 5: Challenges Addressed by Cloud Native

```dataview
list
from "KCNA-notes"
where contains(file.name, "1 Cloud Native Concepts") and contains(section, "Challenges Addressed by Cloud Native")
```

## Query 6: Ecosystem and Governance

```dataview
table section as "Organization", text as "Details"
from "KCNA-notes"
where contains(file.name, "1 Cloud Native Concepts") and contains(section, "Ecosystem and Governance")
```

## Query 7: KCNA Exam Details

```dataview
table section as "Exam Section", text as "Details"
from "KCNA-notes"
where contains(file.name, "1 Cloud Native Concepts") and contains(section, "KCNA Exam")
```

## Query 8: Pro Tips for KCNA Exam

```dataview
list
from "KCNA-notes"
where contains(file.name, "1 Cloud Native Concepts") and contains(section, "Pro Tip")
```

## Query 9: Tags Used in the Note

```dataview
list file.tags as "Tags"
from "KCNA-notes"
where file.name = "1 Cloud Native Concepts"
```

## Query 10: Links to External Resources

```dataview
list
from "KCNA-notes"
where contains(file.name, "1 Cloud Native Concepts") and contains(text, "http")
```

## Query 11: Notes by Status

```dataview
table title, status
from "KCNA-notes"
where status = "draft"
```

## Query 12: Recently Updated Notes

```dataview
table title, updated
from "KCNA-notes"
sort updated desc
```

## Query 13: Notes by Topic

```dataview
table title, topics
from "KCNA-notes"
where contains(topics, "Kubernetes")
```

## Query 14: Notes by Category

```dataview
table title, category
from "KCNA-notes"
where category = "KCNA Notes"
```

## Query 15: Notes with External Links

```dataview
list
from "KCNA-notes"
where contains(text, "http")
```

## Query 16: Autoscaling Types

```dataview
list types
from "KCNA-notes"
where file.name = "2 Cloud-Native Architecture Fundamentals.md"
```
