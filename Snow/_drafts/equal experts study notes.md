
AGILE
=====
Refers to software development methodologies based on 
	
* __iterative development__
* __self-organizing cross-functional teams__ that define and evolve requirements and solutions
* __frequent inspection and adaptation__ of the process itself
* __teamwork__
* __accountability__
* __rapid delivery of high-quality software__ 
* __aligns development with customer needs__
	
SCRUM
===
* a __lightweight process framework__ for agile development,
	* overhead of the process is kept as small as possible to maximize the amount 
	of productive time available for getting useful work done.
* __most widely-used one__

Main benefits
---
* __Increase the quality of the deliverables__
* __more responsive to changing requirements__
* __Provide better estimates while spending less time creating them__
* __Constantly work on high-value features__
* __Allows for constant repriorization__
* __Team members enjoy seeing their work used and valued__
* __Provides visibility and awareness which allows to address issues early__

Scrum Tasks
===

User Story
---
A User Story describes a desired feature (functional requirement) in narrative form. 
User Stories are usually written by the Product Owner, and are the Product Owner’s responsibility. 
The format is not standardized, but typically has a name, some descriptive text, 
references to external documents (such as screen shots), and information about how the 
implementation will be tested. 

The elements in a User Story are usually:

__Name:__
	The Name is a descriptive phrase or sentence. 
	The example uses a basic “Role-Action-Reason” organization. 
	Another common style, popularized by Mike Cohn, follows the template 
	“As a <type of user>, I want <some goal> so that <some reason>.” 
	The choice of template is less important than having a workable standard of some kind.

__Description:__ 
	This is a high-level (low-detail) description of the need to be met. 
	For functional (user-facing) requirements, the description is put in narrative form. 
	For non-functional requirements, the description can be worded in any form that is 
	easy to understand. In both cases, the key is that the level of detail is modest, 
	because the fine details are worked out during the implementation phase, in discussions 
	between team members, product owners, and anyone else who is involved. 
	(This is one of the core concepts of Scrum: Requirements are specified at a level that 
	allows rough estimation of the work required to implement them, not in detail.)

__Screens and External Documents:__ 
	If the Story requires user-interface changes (especially non-trivial ones), 
	the Story should contain or link to a prototype of the changes. 
	Any external documents required to implement the Story should also be listed.

__How to test:__ 
	The implementation of a Story is defined to be complete if, and only if, 
	it passes all acceptance tests developed for it. This section provides a brief 
	description of how the story will be tested. As for the feature itself, 
	the description of testing methods is short, with the details to be worked out 
	during implementation, but we need at least a summary to guide the estimation process.
	There are two reasons for including the information about how to test the Story. 
	The obvious reason is to guide development of test cases (acceptance tests) for the 
	Story. The less-obvious, but important, reason, is that the Team will need this
	information in order to estimate how much work is required to implement the story 
	(since test design and execution is part of the total work).

For example, a Story might resemble the following:
*	Name: Planner enters new contact into address book, 
		so that he can contact the person later by postal or electronic mail
*	Description: Planner enters standard contact information (first and last name, 
		two street address lines, city, state, zip / postal code, country, etc.) 
		into contact-entry screen. He clicks “Save” to keep the data, and “Cancel” to 
		discard data and return to previous screen.
*	Screens and External Documents:
		http://myserver/screens/contact-entry.html
*	How to test: 
		Tester enters and saves the data, finds the name in the address book, and clicks on it. 
		He sees a read-only view of the contact-entry screen, with all data previously entered.   

Story / Technical Story
---
Not all requirements for new development represent user-facing features, but do represent 
significant work that must be done. These requirements often, but not always, represent work 
that must be done to support user-facing features. We call these non-functional requirements 
“Technical Stories.” Technical Stories have the same elements as User Stories, but need not 
be cast into narrative form if there is no benefit in doing so. Technical Stories are usually 
written by Team members, and are added to the Product Backlog. The Product Owner must be familiar
with these Stories, and understand the dependencies between these and User Stories in order to 
rank (sequence) all Stories for implementation.
	
Defect / Bug
---
A Defect, or bug report, is a description of a failure of the product to behave in the
expected fashion. Defects are stored in a bug-tracking system, which may or may not
be physically the same system used to store the Product Backlog. If not, then someone (usually
the Product Owner) must enter each Defect into the Product Backlog, for sequencing and scheduling.

Scrum roles
===

ScrumMaster
---
The ScrumMaster is the keeper of the process. He is responsible for making the process run 
smoothly, for removing obstacles that impact productivity, and for organizing and facilitating 
the critical meetings. 

Responsibilities include
*	Removing the barriers between the development Team and the Product Owner so that the Product 
*	Owner directly drives development.
*	Teach the Product Owner how to maximize return on investment (ROI), and meet his/her 
	objectives through Scrum.
*	Improve the lives of the development Team by facilitating creativity and empowerment.
*	Improve the productivity of the development Team in any way possible.
*	Improve the engineering practices and tools so that each increment of functionality 
	is potentially shippable.
*	Keep information about the Team’s progress up to date and visible to all parties.
*	In practical terms, the ScrumMaster needs to understand Scrum well enough to train 
	and mentor the other roles, and educate and assist other stakeholders who are involved in 
	the process. He should maintain a constant awareness of the status of the project 
	(its progress to date) relative to the expected progress, investigate and facilitate 
	resolution of any roadblocks that hold back progress, and generally be flexible enough 
	to identify and deal with any issues that arise, in any way that is required. 
*	He must protect the Team from disturbance from other people by acting as the interface between the two.
*	The ScrumMaster does not assign tasks to Team members, as task assignment is a 
	Team responsibility. His general approach towards the Team is to encourage and facilitate 
	their decision-making and problem-solving capabilities, so that they can work with 
	increasing efficiency and decreasing need for supervision. His goal is to have a team 
	that is not only empowered to make important decisions, but does so well and routinely.

Product Owner
---
The Product Owner is the keeper of the requirements. 
He provides the “single source of truth” for the Team regarding requirements and their 
planned order of implementation. In practice, the Product Owner is the interface between 
the business, the customers, and their product related needs on one side, and the Team on 
the other. He buffers the Team from feature and bug-fix requests that come from many sources, 
and is the single point of contact for all questions about product requirements. He works 
closely with the team to define the user-facing and technical requirements, to document the 
requirements as needed, and to determine the order of their implementation. He maintains the 
Product Backlog (which is the repository for all of this information), keeping it up to date 
and at the level of detail and quality the Team requires. The Product Owner also sets the 
schedule for releasing completed work to customers, and makes the final call as to whether 
implementations have the features and quality required for release.

Team
---
The Team is a self-organizing and cross-functional group of people who do the hands-on work of 
developing and testing the product. Since the Team is responsible for producing the product, 
it must also have the authority to make decisions about how to perform the work. The Team is 
therefore self-organizing: Team members decide how to break work into tasks, and how to allocate 
tasks to individuals, throughout the Sprint. The Team size should be kept in the range from 
five to nine people, if possible. (A larger number make communication difficult, while a 
smaller number leads to low productivity and fragility.) Note: A very similar term, “Scrum Team,” 
refers to the Team plus the ScrumMaster and Product Owner.

Scrum process
===
Product Backlog
*	created by product owner
*	Prioritized features desired by client
*	"Grooming" is to prioritize
*	Item is called "Product Backlog Item" "PBI"

Sprint Planning
*	Expand and define the most highest priority PBI's from the Product Backlog into a sprint backlog
*	Done before / upon start of sprint

Sprint Backlog
*	Current highest prio PBI's
*	Approximately a sprints worth of work

Iteration / Sprints
*	Team members grab items from the sprint backlog to work on
*	How long a sprint backlog is supposed to last before new items (highest prio) 
	from Product backlog is being pulled
*	At the end there should potentially be shippable software

Sprint review on the deliverable and Retrospective on the process
*	After each sprint
*	What did we do well
*	What can we improve
*	What shall we avoid

Daily Scrum
*	Every day there's a scrum meeting where people inform their team mates
*	What they have been doing
*	What they are to do next
*	If there are any impediments

Promotes
*	Working shippable software containing the highest prio features
*	Fail early / Early feedback
*	Minimize uncertainty and decrease risk
*	Increase productivity and quality

Things to look out for
*	Easy for dysfunctional teams to only interract 1 time a day
*	Requires skill in breaking down features and functionality
	* 	Requires definition of well defined task
*	Micromanaging product owners makes it harder for developers to self-organize
