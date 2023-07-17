## updateActiveIssues
The `updateActiveIssues` function is used to add a new issue to the list of active issues stored in the browser's localStorage.

- It takes an `issue` object as an argument. The `issue` object is expected to contain details about the issue, but it may not yet have an `id`.

- `const issues = JSON.parse(localStorage.getItem('issueLogger.issues') || "[]")` retrieves the current list of issues from localStorage. If there are no issues stored yet, it defaults to an empty array (`[]`).

- The `while (!issue.id)` loop is used to generate a unique id for the issue. It continues generating random numbers until it finds one that doesn't match an id of an existing issue.

   - `const issueID = Math.floor(Math.random() * 99999)` generates a random number between 0 and 99999.

   - `if (issues.find((item) => item.id === issueID))` checks if there's already an issue with the generated id. If there is, it continues to the next iteration of the loop, generating a new random id.

   - `issue.id = issueID` assigns the generated id to the `issue` if it's unique.

- `const issueWithId = issue` assigns the `issue` object (now with a unique id) to `issueWithId`.

- `issues.push(issueWithId)` adds the new issue to the array of existing issues.

- `localStorage.setItem('issueLogger.issues', JSON.stringify(issues))` updates the issues stored in localStorage with the new array of issues, including the new issue. 

- Finally, `return issueWithId` returns the new issue object, now including its unique id.

Overall, this function is used to generate a unique id for a new issue, add the new issue to the existing list of issues, and update the list of issues in localStorage.


## updateActiveIssuesDOM
The `updateActiveIssuesDOM` function is used to update the DOM to reflect the current list of active issues. 

- It takes an optional `newIssueId` argument. This is the ID of a new issue that has just been added.

- `const issues = getActiveIssues()` retrieves the current list of active issues.

- The function then checks two conditions in the `if` statement: whether there are any active issues and whether a `newIssueId` has been provided.

- If both conditions are true, this indicates that a new issue has been added. The function then does the following:

    - It hides the `noIssuesMsg` element and aligns the remaining content at the start of its parent container.

    - It checks if the ID of the last issue in the list matches the `newIssueId`. If it does, this issue is assigned to `newIssue`. Otherwise, it searches for an issue with the `newIssueId` in the list of issues and assigns this issue to `newIssue`.

    - It creates a new `div` element (`placeholderElement`), generates the HTML for the new issue card using the `issueCardTemplate` function, and appends the `placeholderElement` to the `issuesListElement`.

    - It replaces the `placeholderElement` with the new issue card HTML.

- If there are active issues but no `newIssueId` has been provided, this indicates that the page has just been loaded or reloaded. The function then does the following:

    - It hides the `noIssuesMsg` element and aligns the remaining content at the start of its parent container.

    - It iterates over each issue in the list of issues, generates the HTML for the issue card using the `issueCardTemplate` function, and appends a new `div` element (the `placeholderElement`) to the `issuesListElement` for each issue.

    - It replaces each `placeholderElement` with the corresponding issue card HTML.

In summary, this function updates the DOM to reflect the current list of active issues, either by adding a single new issue card when a new issue is added, or by adding all active issues when the page is loaded or reloaded.

## deleteActiveIssuesDOM 
This function is used to remove all issue cards from the DOM when the issues data gets cleared.

- `const nodesForDeletion = [ ...issuesListElement.children ].slice(1)`: This line is creating an array from the children of `issuesListElement`, which represents the list of issues in the DOM. It then uses `.slice(1)` to get a new array that excludes the first child. This is because the first child node is likely a static part of the layout (such as a title or header), and we don't want to delete that. 

- `nodesForDeletion.forEach((node) => {...})`: This line iterates over the `nodesForDeletion` array. For each node, it removes the node from the `issuesListElement` using the `.removeChild(node)` method. 

- `noIssuesMsg.style.display = "block"`: This line changes the CSS `display` property of `noIssuesMsg` (which is likely a message saying "No issues" or similar) to `block`, making it visible.

- `noIssuesMsg.parentElement.style.justifyContent = "center"`: This line centers the `noIssuesMsg` element within its parent element. This is likely done for aesthetic reasons, to make the interface look better when there are no issues displayed.

In summary, this function is used to clear all issue cards from the DOM and display a "No issues" message when the issue data gets cleared.
