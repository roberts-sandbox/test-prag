---
layout: default
published: true
---

# Regular Expression that finds query parameters in SQL statements explained

Recently, I encounter a similar scenario to the following one.  I have to implement a class in the Persistence Layer.  This class contains methods that are called from the Business Layer.  Normally, the SQL statements are hard-coded inside these methods.  But a there's small twist here in that the SQL are configurable.  When business logic changes, the SQL statements have to be adjusted.  We don't want to disturb, i.e. make any code change to the classes in the Persistence Layer.  The changes will only take place in where the SQL statements are configured, be it another table in a database or in a config file.  There might be more or fewer parameters to the SQL statements, which will require the callers in the Business Layer to change the arguments that gets passed in.  

Assume we are dealing with Oracle (although this concept is database agnostic), some examples of the underlying SQL statements in this Persistence Layer class are:

<script src="https://gist.github.com/pragmaticlogic/5279484.js"> </script>

All of the above are legal and valid SQL statements.  The Regular Expression must be able to correctly handle any and all of the above.

