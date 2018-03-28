Notes for Pluralsight "Web Accessibility: Getting Started"


Not just code, but design, responsive design to handle devices dimensions and pixel densities, colour palette needs to be designed with contrast and readability and color blindness in mind. Typography, forms, easy to touch.

Response design is accessibility concern

Bright light needs higher contrast

Busy environment need simple ui

Colors, color blindness, most common is red/green deficiency. Don't rely soley on color to distinguish between things. Low end devices show colors differently.l, contrast helps.

Typography, typefaces, hierarchy understandable and recognisable, setting body copy to ready to read and consume. Font size, not large. 16-20px. Not good to set in px values, should use relative units em or rem,. Contrast and size should be distinguishable between header and body content. Line length, not to long not to sort, should be natural to read from one line to the next, 66 chars best. Like height, 125% of text size. Avoid justified text, makes it hard to read for dyslexic people, left aligned easier. Don't use so caps, as they can be hard to read and screen readers might interpret them wrong.

Form design
	Make easy to use and understand. Clearly identify required fields, don't just really on color here. Identify special format requirements, "must be 8 characters". Clear descriptive form labels. "Enter your email address" "what's your occupation".
	Don't use placeholder text as replacement for real label
		Dissapears when focused
	Feedback for errors and warning should be clear
		Dont rely solely on color but instead ok/error icon with message stating ok/error

Touch Targets
	Smaller touch targets harder to touch
	Minimum 44-48px, the bigger the better
	Enough space between touch targets

Focus an active indicators
	Browsers already provide these, we as develpers need to make sure we don't remove/override accidentally.
	Need to be unobstructed
	Needs contrast
	Don't rely solely on color

Motion and vestibular disorders
	Dont make people dizzy / sick
	Most often caused by transforms, animations, parallax
	Keep it simple, any animation or transition should have a purpose.
	Possibly a way to disable animations

Coding for Accessibility
	Proper document structure
	Use the correct html tags
	Don't use text in images
	Add alt description to images
	Accessible from the keyboard
	Focus on performance

	Proper document structure
		Never skip heading levels, i.e h5 should have parent h4, not h2

		Use lists
			ul, ol, dl

		Facilitate keyboard navigation
			Flow through in the correct order (tab)
				Menu / menu items
				Main content 
				Side content

	Images
		Only refer to images as content, not images for layout
		No description? Give empty alt attribute to indicate to screen readers to ignore it.
		Needs description, alt attribute for simple images (photos).
		longdesc="GraphDesc.txt" attribute for graphs, charts, more intricate image content. 
			Can link to completely different page or id on the same page.
			Browser support almost nonexistent, recommened to provide a regular link to graph description which everyone can see.
			<img src="graph.jpg" aria-describedby="desc" alt="Graph" />
			<a href="GraphDesc.html" id="desc">
				Get Graph Details
			</a>

	Content Accessibility
		Users came for the content, no matter disability/no disability, content should be accessible.
		Skip to main content link at the top of the page

		Audio content
			Transcripts

		Video content
			Transcripts as well as captions

		Truncated content and Read more links
			Screen readers will hit those links hearing "Read more, read more", should be more descriptive "Read more about our products"
			Can use css to hide the verbose text and show "Read more" as .about-link:after {content: "Read More";}

	Html attributes
		Always provide language attribute: <html lang="en">
		Always provide lang for sections in other language on page.
		<abbr> tag should have title
		Tab index, Should only be needed if we build our own custom components, like a span to represent a button 

	Forms 
		Always provide labels with for attributes, associates labels with controls.
		Always provide required attribute for required controls
		Create logical groupings with fieldset and legends
		Put associated labels, using the for attribute
		Don't use placeholder text, isn't supported throughout technology and not reliable.
		Use placeholder text for hints an tips
		Use required attribute on inputs as well as text indicating required field.
		Use type attribute of inputs (number, date, etc.)
		Clear instructions for complex fields (passwords e.g)
		Tab order of forms (should require little to no effort given we have the correct html structure)

	Keyboard control
		Use Focus an active indicators, never remove with display:none instead restyle
		Tab control, left to right, top to bottom. Controlled by how we structure our markup.
		Use standard elements like links and buttons, and not spans with onclick. Tabindex of 0 remedies this situation, it inserts the element at the default control position.
		Skip stepping through every navigation link in menus. "Skip to main content" before menu. Link as first content of page with href="#primaryContent"
			If not pretty, make it visible hidden and only show on focus.

	CSS & Accessibility
		For content that should only be available to assistive technologies ("Main Menu Content" e.g)
		Use sparringly, as mostly content should be visible for both sighted and non-sighted users.
		width:0, height:0, display: none and visibility:hidden prevent assistive technologies from reading their content.
		Way to do it : {
			position: absolute;
			left: -9999px;
			top: auto;
			width:1px;
			height:1px;
			overflow: hidden;
		} or: {
			position: absolute;
			clip: 1px 1px 1px 1px; // For ie6 and ie6
			clip: 1px, 1px, 1px, 1px;
		}

		:focus and :active to help aid visually where user is at.

	Performance
		Img sprites
		Critical path css (Inline above the fold css)

	Progressive enhancement
		Support lowest device and enhance for better devices
		"Mobile first"
		Makes sure the most people have access to your site.

ARIA Roles & Attributes (Accesible Rich Interent Applications)
	Roles (what a widget is or does) 
		Landmark roles
			role=""
			application
			banner
			navigation
			main
			search
			complimentary
			form
			contentinfo
		Widget roles
			alert
			alertdialog
			button
			checkbox
			dialog
			gridcell
			link
			log
			marquee
			menuitem
			menuitemcheckbox
			menuitemradio
			option
			Progress meter
			radio
			scrollbar
			slider
			spinbutton
			status
			tab
			tabpanel
			textbox
			timer
			tooltip
			treeitem
		Container roles
			combobox
			grid
			listbox
			menu
			menubar
			radiogroup
			tablist
			tree
			treegrid
		Document structure roles
			article
			columnheader
			definition
			directory
			document
			group
			heading
			img
			list
			listitem
			math
			note
			presentation
			region
			row
			rowgroup
			rowheader
			separator
			toolbar

		<input aria-describedby="password-help" type="password" id="password" required />
		<div role="tooltip" id="password-help">Must be at least 8 characters long</div>

	States / Aria Attributes
		Represents states
		Statesa
			busy
			disabled
			grabbed
			hidden
			invalid
			checked
			disabled
			selected
			hidden
	Aria Properties
		Describe relationship between elements
			drag & drop
			describedby
			flowto
			haspopup
			label
			labelledby
			live regions
			modal

	Aria and Keyboard Control
		Uses tab and shift tab to nagivate the site
		Tab navigates between elements
		Arrows navigates between items in elements
		Spacebar activates controls

WCAG Web Content Accessiblity Guidelines
	Perceivable: Cannot be invisible to all their senses
	Operable: Must be able to operate all UI controls
	Understandable: Must be able to understand UI and information
	Robust: Should not be fragile, and work on assistive technologies. Should be future proof.

Testing for Accessibility
	https://www.w3.org/TR/WCAG10/full-checklist.html
	NVDA screen reader
	wave.webaim.org <- best of it's kind
	Chrome extensions: Accessibility Developer Tools
	tenon.io, integrates into developer process
	bitsofco.de/2015/the-accessibility-cheatsheet
	tota11y <- quick and easy to test
	periodic table of aria roles
	periodic table of aria attributes