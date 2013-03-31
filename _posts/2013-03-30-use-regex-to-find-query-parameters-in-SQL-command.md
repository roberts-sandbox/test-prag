---
layout: default
published: false
---

I have to implement a class in the Persistence Layer.  This class contains methods that are called from the Business Layer.  Normally, the SQL statements are hard-coded inside these methods.  But a there's small twist here in that the SQL are configurable.  The parameters to the SQL might also be more or fewer.  The only thing that remains unchanged is the return data type of these methods.  

Assume we are dealing with Oracle (although this concept is database agnostic), some examples of the underlying SQL statements in this Persistence Layer class are:

