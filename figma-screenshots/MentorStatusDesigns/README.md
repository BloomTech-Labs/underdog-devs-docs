# Mentor Status - Setting Availability
Detailed and displayed here is the design for [BL 709 - As a Mentor I want to set my availability](https://bloomtechlabs.atlassian.net/jira/software/c/projects/BL/boards/10?modal=detail&selectedIssue=BL-709&quickFilter=27&quickFilter=19)

The BE route that hits the DS API to toggle the Mentor's `accepting_new_mentees` boolean is in the process of being merged. After discussions with appropriate parties we have decided to add this feature to the nav bar for simplicity and findability.

### Notes
❗ `Availability` heading is subject to change as well as additional info being added to hover modal.

## User Flow
- Entry into Mentor dashboard
- Navigation is always visible with 3 items: `Availability`, Memos, and Welcome
- On hover of Availability, a context modal is displayed to describe the purpose of the switch and it's relevance to the Mentor
- User can click to toggle or de-hover to leave their Availability unchanged, remaining on the dashboard at all times

## Not Available
<img alt="navigation bar highlighting a mentor's view of the toggle switch on hover. Toggle is set to off and a modal toggle explanation window is present." src="./Mentor-status-NA.png">

## Available
<img alt="navigation bar highlighting a mentor's view after they have toggled the Availability to on. Toggle is flicked to the right in green color." src="./Mentor-status-AV.png">
