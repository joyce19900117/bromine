# Available test commands #

Bromine currently supports the list of commands below:

## assertText ##

This command asserts if expected text matches the text of an UITextField or any component with text attribute found with the given XPath query.

**Required parameters:**
> viewXPath (search for views matching this XPath)

> text (the expected text)

---

## checkMatchCount ##

This command verifies that the specified number of nodes matching
the given XPath query are found.

**Required parameters:**
> viewXPath (search for views matching this XPath)

> matchCount (number of nodes found must equal this number)

---

## outputView ##

This command outputs the current view hierarchy, starting with the
keyWindow, to a file or stdout.

**Required parameters:**

**Optional parameters:**
> outputPath (file path to save xml description of hierarchy, if omitted print to stdout)

> viewXPath (only output views matching this XPath - NOT WORKING YET)

---

## pause ##

This command keeps does nothing except setting the minimum interval
to the next command according to the 'seconds' parameter

**Required parameters:**
> seconds (interval between pause and the next command)

---

## scrollToRow ##

Scrolls a UITableView selected by an XPath query to the specified
rowIndex (and optionally sectionIndex).

**Required parameters:**
> viewXPath (search for a table view matching this XPath)

> rowIndex (scroll the table view to this row)

**Optional parameters:**
> sectionIndex (scroll the table view to the rowIndex in this section)


---

## setText ##

This command sets the text of an UITextField or any component with text attribute
found with the given XPath query.

**Required parameters:**
> viewXPath (search for views matching this XPath)

> text (the text to be set)

---

## simulateTouch ##

Performs a synthesized touch down and touch up in a single view selected
by a given XPath query.

**Required parameters:**
> viewXPath (search for a view matching this XPath)


---

## touchBackButton ##

Performs a synthesized touch down and touch up in the current navigation back button


---

## waitForElement ##

This command keeps running until the elements associated with
the given XPath query are avaiable or gives a timeout.

**Required parameters:**
> viewXPath (search for views matching this XPath)

**Optional parameters:**
> count (wait for a specified number of elements matching viewXPath)