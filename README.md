# SoftwareAssurance
This is the repository for Team 5 in the Software Assurance class at UNO.

# Contributing
Follow our team's standard development life cycle (SDLC) when making changes.

## Roles
* Contributor - Someone who makes changes and submits a Pull Request for them.
* Reviewer - Someone who reviews and approves changes

## Actions:
 * Contributing Work
 	* Make sure there is a GitHub issue for your work
 		* Create an issue if needed:
			1. Navigate to the repository on GitHub
			2. Select the "Issues" menu item
			3. Click the "New Issue button"
				- Add a descriptive title
				- Set yourself as the assignee (unless you are just creating issues to catalog future work)
 	* Move your issue in the current project board to "In Progress"
 		* Add your issue to the board if needed
 			1. Select the "Projects" menu item
 			2. Click the current project board
 			3. Click the "Add cards" button on the right
 			4. Search for your issue
 			5. Drag your issue into the board to automatically create an associated card
 	* Checkout the `master` branch
 		* In a terminal, navigate to the project and use  `git checkout master`
 	* Pull the latest changes
 		* In a terminal, use `git pull`
 	* Create and checkout a separate branch for your changes
 		* In a terminal, use `git checkout -b <your branch name>` to create and checkout your new branch
 	* Make your changes
 		* Stage them
 			* In a terminal, use `git add <name of file or directory with changes to stage>`
 		* Check that you have everything you want to commit staged
 			* In a terminal, use `git status` to see what is currently staged and what is not.
 		* Commit them and include the issue number (ex: '#21' in your commit message)
 			* In a terminal, use `git commit -m "Added SDLC doc #21"` to commit changes and include your issue number in your message (substituting your issue number and message)
 	* Push your changes to the remote repository
 		* In a terminal, use `git push --set-upstream origin <your branch name>` to push and create a remote version of your branch
 			* If you've already run the above command once on your branch, you can simply run `git push` for subsequent changes
	* Create a Pull Request on GitHub to merge your changes into `master`
		* Note in the description if you'd like more than one Reviewer for your changes
		* Include the [keyword](https://docs.github.com/en/github/managing-your-work-on-github/linking-a-pull-request-to-an-issue) "closes" and the issue number in the description to automatically link the two (ex: "closes #21")
	* Message the group on Discord letting them know there is a PR out there
	* If the Reviewer(s) request changes, repeat from the "Make your changes" step (except for creating a new PR
 * Reviewing Work
 	* Look at the changes made in the PR
 		* If everything looks good:
 			* Approve the PR
 			* If you or the Contributor feel the PR needs one more review:
 				* Message the Group in Discord
 			* If you are the final Reviewer:
 				* Merge the feature branch into `master`
 				* Delete the feature branch
 				* Move the associated issue's card to "Done" on the current project board
		* If changes should be made
		 	* Tell the Contributer
