# Quick Wins
1. Check `robots.txt`
2. Check page source for JavaScript that leaks hidden pages
3. Check user-controllable information for access rights / roles, such as
	- A hidden field (page source)
	- A static cookie
	- A preset query string parameter in url (`www.website.com/home.jsp?role=1`)
		- if you find a url parameter, login, forward login request to burp and see if there is a cookie that you can change (such as `Admin=false`)
	- An action to the user account (such as changing an account email/password)
		- Capture in Burp, see if there is any `id` you can change. 
		- Send request to repeater and observe response to see if any `id` is defined
			- if there is, add the `id` name to your request with another id value to see if you can perform actions with another users permissions