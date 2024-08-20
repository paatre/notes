# OAuth 2.0

## Introduction

To understand the need for OAuth, we'll first need to understand the background and history of the problem it solves.

### Background

When you want to grant a third-party application access to your account and the data it contains, the third-party app needs a way to authenticate itself to the server. Before OAuth, the only way to do this was to share your username and password with the third-party app. For example, if you wanted to give a third-party app access to your Twitter account back then, you would have to share your Twitter username and password with the app, so it could log in a retrieve the data it needed from your account for some purpose, i.e. posting tweets on your behalf etc. This was a huge security risk, as you were essentially giving the third-party app full access to your account, and if the app was malicious, it could do a lot of damage. Also, passwords were often stored in plain text, which made them easy to steal and harvest, and even change the password to lock you out of your own account. The way to revoke access to the third-party app was to change your password, which isn't a very user-friendly way to manage access to your account.

### History

Due to the problems mentioned above, many services started to implement their own authorization mechanisms (FlickrAuth, AuthSub, BBAuth, etc.), which were all different and incompatible with each other. To tackle this problem, a group of people got together, formed a discussion group, and created **a standard for API access control**, which they called OAuth 1. OAuth Core 1.0 was released in December 2007. A few years later, the OAuth specification work was moved to the IETF.

OAuth 1 was a good start, but it had some problems. It was complex, hard to implement, and had some security issues. So, the group went back to the drawing board and created OAuth 2.0, which was released in October 2012. OAuth 2.0 is a much simpler and more secure protocol than OAuth 1, and it has become the de facto standard for API access control.

## How OAuth Works

TBD
