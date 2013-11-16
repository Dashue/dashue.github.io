---
layout: post
title: Review TFS 11: The things I look forward to
categories: Software Development
published: true
---
With TFS 11 around the corner (with in-place upgrade from 2010), having been rewritten with the promise of a faster and smarter experience I felt like compiling a list of the features I look forward to the most. Ordered by pain factor solving desc.

###Automerge content changes (finally)
Finally TFS has come to it&#8217;s senses and allows for automerge of non-conflicting content changes. 

###My work pane and Search work items
No need for custom work item queries, work related to you will be easily available in the work item pane. Searching for items is also available here.

###Easier to transition between work items states
No more open work item and changing state to in progress, just right-click item in my work pane and click start.

###Suspending
Easier task switching by using suspend/resume. Saves visual studio state, open files and position in currently open file. Suspended tasks are listed in you work pane.

###New Pending changes page
Shows tree of pending changes. Allows or leaving files in excluded state without having to exclude changes every check-in (good for personalized web.config etc.).

###New built-in diff tool
TFS team has finally gotten around to fix the built in diff tool, which now supports more view modes and reflects live changes. Which means diff will update if you make changes to your version when showing base/yours. The merge tool has also gotten some love.

###Local workspaces
Removes offline mode, and the time out method require to go offline. Doesn&#8217;t handle modified files by reading the read-only bit on the file (Took them until 2012 to fix it). File system watcher watch for change made outside of VS, and lists them nicely in the pending changes pane.

###Shelving
Shelveset merge (with current and baseless) and searching of shelvesets.

###Code Review workflow
Request code-review workflow on suspended changeset or checked in changeset as well as code reviewing functionality with the ability to comment on specific changes and even alter the changeset.

###Rollback changesets
Awesome

###Async operations
Modality and blocking operations has now been removed, no more waiting on visual studio to complete retrieving things (search items etc.).

###New Builds page
Shows builds associated to me (started by me etc.). Extensible

###Summary:

I really like the new way that TFS 11 shows data; putting the concept of "My work" in focus, showing my work items and my builds etc. This combined with the new functionalities provided, working in TFS 11 will be much more smother and faster than it's predecessors and I am really looking forward til it's released and part of my development environment.

Reference: [Developer collaboration with Team Foundation Server 11](http://channel9.msdn.com/Events/BUILD/BUILD2011/TOOL-811T)