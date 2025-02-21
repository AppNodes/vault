
*Savio*  - I'm just prefixing this note with ZZZ so it shows up last in the list.
# Value Proposition

### Goal 1
I want to be able to enable developers to begin work on a project within 1 day of coming on board.

**This means**
As a developer I need to be able to understand what the app does currently, and what it's supposed to do very fast.

**Influences and Notes**
- Snowflake Method - https://www.advancedfictionwriting.com/articles/snowflake-method/
- A picture is worth 1000 words

### Goal 2
I want to reduce the feedback loop between development teams and product owners.

**This means**
We need to enable non-development staff to clearly communicate needs to developers so that there doesn't need to be a back-and-forth between teams in order to clarify things.

**Influences and Notes**
- Deep Work by Cal Newport, as well as the Basecamp Pitch Ideology.
	- We went from email to slack/teams messages, which basically resulted in us treating messages as ephemeral - which resulted in people starting to send incomplete thoughts instead of fully compiling their ideas before communicating them.
	- Ideally we want a product owner to be able to create a task, and the developers have enough information so they don't need to ask for clarification.
	- We also want the product owners to be able to create a task really quickly, so they don't feel the need to add a bunch of unnecessary detail.  
	- More words = Less Clarity, Fewer words = More clarity.
	- We want this task creation to be as visual as possible.
- Clarify location of things.
	- Sometimes people will get a request to fix a report called... "student list" - and there are 3 unique reports called student list depending on the user's role, or context of the service they are using.  Having the users/product owners identify the specific node allows the developer to be confident that they are working on the right one.

### Goal 3
Enable product owners to visually understand how their requests fit into the larger system.

**This means**
- Request/Ticket Planning should happen within the AppNodes platform.
- During the planning process, we should try to show if there is similar functionality elsewhere (maybe based on keywords, or page titles)
- We need layering of the service - eg, some views may be more valuable for POs, and some may be more valuable for devs.

**Influences and Notes**
- Project owners often times group tasks weird, or duplicate tasks because they don't understand the dev context.
	- eg.  There may be 1 ticket for changing the color of an element, and another one to change a placeholder on the same page, and logically they should be in the same ticket, but it increases friction for production flow.
	- Sometimes Project owners will create multiple tickets to fix a problem that exists in a process that is called from multiple places - because they don't actually understand how the system works.  
- This will increase awareness of how long a task takes. 
	- if a PO can see that they're asking for 20 different nodes to be modified, they will automatically understand that it isn't trivial.


---


# Technical Ideas

Key classes should be 

- `Project` ( This will be a collection of all the nodes for a given app that someone is planning out)
- `Node` - Each element we render should be derived from a node, Nodes should have a type, and a parent.
- `Collection` - a collection of nodes.  This can be for showcasing a specific piece of functionality, or planning a task, or identifying a specific area within the plan.
- `Comment` - this should be a child of a `Node`, and optionally a child of a `Collection` as well - so that we can contain node comments to a specific task.
- `Connector` - This is a joiner, but I think we should be able to add labels and tags to it.
- `Tag` - This should be able to be applied to `Connector`s and `Node`s.  - just has a piece of data that allows you to filter, or search by.


### Node Types
- `Redirect Middleware` - eg, a user role, permission, or user group, if you attempt to access a resource under here, you will be redirected to somewhere.
- `Error Middleware` if you attempt to access a resource under here, you will be shown an error
- `Visibility Middleware` - elements under this middleware will not be displayed to users who do not have the permission/role/pass the middleware
- `Domain` - eg - `/admin` or some domain space.
- `Process` - some logic that occurs during an event
- `Nav Menu` - eg, Side Nav, or Top Nav
- `Page` - a page that a user lands on
- `Section` - a visual section - could be a group of a nav bar, or section of a page, etc.
- `Button` - something that calls a process or moves to another page
- `Workflow` - a collection of pages that are displayed in a unique order, where you can't navigate directly to the flow.  (PROBABLY A FUTURE FEATURE)
- `Notification` - something that notifies the user



### Node Usage Examples
**Legend**
P - Project
N.{Type} - is Node of Type
C - Collection
COM - Comment
CON - Connector
TAG - Tag

```markdown-tree
P - EconoRoute
	CON
		N.Redirect Middleware - Guest
			CON
				N.Page - Register `/register`
					CON
						N.Section - Already have an account?
							CON
								N.Button - Send user to `/login`
			CON
				N.Page - Login `/login`
					CON
						N.Section - Need to sign up?
							CON
								N.Button - Send user to `/register`
			CON
				N.Page - Forgot Password `/forgot-password`
	CON
		N.Redirect Middleware - Auth
			CON
				N.Nav Menu - Top Nav Bar
					CON
						N.Nav Menu - Team Selection
							CON
								N.Visibility Middleware - Has Team
									CON
										N.Button - Select Team
											CON
												N.Process - changes users team to the selected team
							CON
								N.Visibility Middleware - Is Team Admin
									CON
										N.Section - Team Settings
											CON
												N.Button - Send user to `/team-settings` page																
					CON
						N.Nav Menu - Profile Dropdown
							CON
								N.Button - Profile - Send user to `/profile`
							CON
								N.Button - Logout
									CON
										N.Process - Ends Session & Logs user out
					
```

