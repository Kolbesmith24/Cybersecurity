# What is it?
**Access control** determines whether the user is allowed to carry out the action that they are attempting to perform.
### Vertical Access Controls
Mechanisms that restrict access to sensitive functionality to specific types of users.
- different types of users have access to different application functions. For example, an administrator might be able to modify or delete any user's account, while an ordinary user has no access to these actions. 

Vertical access controls can be more fine-grained implementations of security models designed to enforce business policies such as separation of duties and least privilege.
### Horizontal Access Controls
Mechanisms that restrict access to resources to specific users.
- Different users have access to a subset of resources of the same type. For example, a banking application will allow a user to view transactions and make payments from their own accounts, but not the accounts of any other user.
### Context-dependent Access Controls
Restrict access to functionality and resources based upon the state of the application or the user's interaction with it.
- Prevent a user performing actions in the wrong order. For example, a retail website might prevent users from modifying the contents of their shopping cart after they have made payment.
## Examples of Broken Access Controls
Broken access control vulnerabilities exist when a user can access resources or perform actions that they are not supposed to be able to.
### Vertical Privilege Escalation
If a user can gain access to functionality that they are not permitted to access then this is vertical privilege escalation.
- For example, if a non-administrative user can gain access to an admin page where they can delete user accounts
#### Unprotected Functionality
Vertical privilege escalation arises where an application does not enforce any protection for sensitive functionality
- For example, administrative functions might be linked from an administrator's welcome page but not from a user's welcome page. However, a user might be able to access the administrative functions by browsing to the relevant admin URL.
	`https://insecure-website.com/admin`
- In some cases, the administrative URL might be disclosed in other locations, such as the `robots.txt` file:
	`https://insecure-website.com/robots.txt`

Even if the URL isn't disclosed anywhere, an attacker may be able to use a wordlist to brute-force the location of the sensitive functionality.

In some cases, sensitive functionality is concealed by giving it a less predictable URL. (security by obscurity)

Sometimes URLs might be disclosed in JavaScript that constructs the user interface based on the user's role:
```javascript
<script> 
	var isAdmin = false; 
	if (isAdmin) { 
		...
		var adminPanelTag = document.createElement('a');
		adminPanelTag.setAttribute('href', 'https://insecure-website.com/administrator-panel-yb556'); 
		adminPanelTag.innerText = 'Admin panel'; 
		... 
	} 
</script>
```
- This script adds a link to the user's UI if they are an admin user. However, the script containing the URL is visible to all users regardless of their role.
#### Parameter-based access control methods
Some applications determine the user's access rights or role at login, and then store this information in a user-controllable location. This could be:
- A hidden field.
- A cookie.
- A preset query string parameter.

The application makes access control decisions based on the submitted value. For example:
`https://insecure-website.com/login/home.jsp?admin=true` 
`https://insecure-website.com/login/home.jsp?role=1`

This approach is insecure because a user can modify the value and access functionality they're not authorized to, such as administrative functions.