# Human Computer Interaction #

*A more detailed summarization of the lecture **Human Computer Interaction 2**. Held by Prof. Andreas Butz at the LMU Munich in 2012/13. Material not relevant for the test already excluded.*

## Usability ##

> ... the effectiveness, effiency and statisfaction with which a specified set of users can achieve a specified set of tasks.

**Importance of Usability**

* Increase Productivity
* Reduce Costs (support, effiency)
* Increase Sales / Revenue
* Enhance customer loyality
* Win new Customers

### Usability / HCI Basics ##

**Goals**

* Facade & machinery and their integration:
		What the user sees <-> What happens.
* Create adequate conceputal models

**Process**

* Investigate requirements (Studies, Focus Groups)
* Take usabiltiy as a central element of all development activities (Quality Assurance)
* Iterative development (Paper Prototypes -> High Fidelity Prototypes)
* Guidelines and principles (e.g. [Nielsen: 10 Usability Heuristics](http://www.nngroup.com/articles/ten-usability-heuristics/))
* Evaluation 

### Web Usability ###

**Importance of usability on the web**

* Anybody can use the web, huge span of user variety

**Important for Web Usability (Steve Krug)**

* "We don't read pages, we scan them" (don't reading everything)
* "We Satisfice" (rather experimentation than guessing)
* "We Muddle Thorugh" (don't care about how and why things work)

#### Factors in Web Usability (Web II) ####

**Visual Hierarchy**
 
* bigger = more important
* nesting for grouping of information

**Conventions**

* examples: Shopping Cart, Search function
* might be against the designers 

**Screen Estate**

* Core Content vs. Navigation vs. Advertising

**Simplicity Principle**

* "Simplicity always wind over complexity" (J. Nielsen)
* Can help to achieve better performance
* applies to everything: design, copy, ...


#### Challenges in Web Usability ####

**Cross Platform Design**

* Screen size and device may vary
* Static vs. fluid design
	* *Sadly no mention of responsive webdesign*
* Browser vary

**Response Times**

* Users dislike long loading times
* Classification: (Miller 1968)
	* 0.1 second (good)
	* 1 second (ok)
	* 10 seconds (bad, user goes to different task)
* Generally improving (faster network connections and computers)
* Multimedia content faces new challenges (pre-loading, buffering, ...)
* RIA (Rich Internet Applications) & AJAX for lower response times

**Linking** 

* Visual formatting of links
* Link targets (anchors, external site, open a new window?)

**URL Design**

* Easy domain name (easy to remember, short, simple spelling)
* Slugs (archival URLs)

**Navigation**

* Browse or search?
* No sense of scale, direction and location

**Search**

* Use conventions / best practices
* allow limiting the search scope

**Content Design**

* Design for the end user
* Ask questions
* Hide internal organization and terminology unknown to most users

#### Working on Web Usability ####

**Nielsen Usability Engineering Life Cycle**

* Pre-Design Phase:
	* Study users in their regular environment
	* Comparative user tests on competing web sites
* Design Phase:
	* Simple prototypes of different design approaches (A/B Testing)
	* Iterate the design as often as possible
* Post Design Phase:
	* Get statistics and feedback
	* Start planning the next design iteration

**Structural Design**

* Design information and navigational structure
* Include non-technical staff
* Concept map (/= internal organization of the company)
* Design on a blackboard

**Common Information Structures**

* Linear 
	* Purely linear
	* Linear w/ options
	* Linear w/ alternatives
	* Linear w/ side-branches
* Circular
	* Closed guided path
	* Variants / side-paths
	* Single entry-point
* Information Grid
	* Ordered on orthogonal criteria (catalogue)
	* (optional) mutli-dimensional
* Hierarchical Information Structure
	* Deep Hierarchy
	* Flat Hierarchy
* Linked Information Structures (*pure web*)
	* difficult for orientation
	* extremely expressive

**Frames**

*they are just bad. don't use them. no idea why we need to go into this in 2013. you can't print them, bookmark them, search them and they are not even in the HTML standard anymore.*

#### Potential Problems on the Web ####

**Downright Errors**

* broken links / missing images
* Firewall errors, no connection to server
* Scripting errors that render the page unusable
* HTML problems that break the DOM-flow

**Annoying or inaccessible page design**

*