# Todo List

## Automate Deployment


### Scripts

Could avoid DockerHub's automated deployment and use a script to push all tags.


### DockerHub

Alternatively, create automated builds on DockerHub for each branch and tag.
I've taken this route so far.

This entails:

  * Tagging every date-stamped image
  * Creating a branch for each updating image

To help out with this process, I would like to create a scripts to:

  * Manage tagged builds:
      * input folder to build (this specifies version number)
      * get date from system
      * tag current commit using version and date
      * push tag to GitHub
      * DockerHub will then build this image automatically
  * Manage branch builds:
      * check out branch to build
      * merge with desired branch
      * push to GitHub
      * Beware: if this script is tracked and we check out different branches, its contents could change!
