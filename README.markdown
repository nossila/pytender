pytender is a python API wrapper for the wonderful tender support service (http://tenderapp.com)

This wrapper is currently a work in progress and doesn't support the entire tender API yet. However, I think we currently have a good base. If you like the API wrapper and would like to help out, please fork it and do so!

Please direct all bugs and feature requests to the lighthouse page for this project:

http://chrisdrackett.lighthouseapp.com/projects/37333-python-tender/overview


Requirements
============

* python 2.5+
* [tpg](http://christophe.delord.free.fr/tpg/index.html)
* [M2Crypto](http://chandlerproject.org/bin/view/Projects/MeTooCrypto)

Installation
============

just add tender.py to your python path.

Multipass Login
===============

This app supports tender's multipass login. This currently works like so:

### Connect to tender and create a tender client:

	>> tender = TenderClient('appname', 'secret', 'user email')

### you can then use the client to get a multipass login url:

	>> tender.multipass_url('http://your_app.tenderapp.com', tender.multipass())
	http://your_app.tenderapp.com?sso=somemultipasshere

### this url can be used to the user with the email above to your tender site

Examples
========

	>> tender = TenderClient('appname', 'secret', 'user email')

### we can get info on the current user:

	>> tender.profile().name
	u'Chris Drackett'

### dates are converted into datetime objects.

	>> tender.profile().created_at
	datetime.datetime(2009, 8, 26, 21, 28, 5)

### TenderUser can be used to get all the discussions for that user...

	>> discussions = tender.profile().discussions()

	>> discussions.count()
	53

	>> discussions[1].title
	u'discussion title'

	>> discussions[1].href
	u'http://your.tenderapp.com/discussions/questions/1'

	>> discussions[1].is_public
	True

### Actions can be taken on the discussion

	>> discussions[1].toggle()
	u'Public' # returns the new state

	>> discussions[1].change_category('123')
	u'Public' # returns the new state

### And then comments for that discussion

	>> comments = discussions[1].comments()

	>> comments[0].formatted_body
	u'<div><p>this is the comment body</p></div>'

	>> comments[0].user().name
	u'Chris Drackett

	>> comments[0].created_at
	datetime.datetime(2009, 9, 12, 3, 21, 14)

### From the tender API object we can also get categories

	>> all_categories = tender.categories()

	>> all_categories[0].name
	u'Questions'

	>> all_categories[0].href
	u'http://your.tenderapp.com/discussions/questions'

	>> all_categories[0].summary
	u'Ask us anything!'

	>> question_discussions = all_categories[0].discussions()

	>> question_discussions[0].title
	u'This is a question'
	
### We can create discussions from category object using the signed in user

	>> all_categories[0].create_discussion('Title', 'Body')
	<tender.TenderDiscussion object at 0x1011bafd0>

### Or using another user by e-mail address

	>> all_categories[0].create_discussion('Title', 'Body', author_email='email')
	<tender.TenderDiscussion object at 0x1011bafd0>