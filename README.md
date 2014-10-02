WP-OAuth
========

A WordPress plugin that allows users to login or register by authenticating with an existing Google, Facebook or LinkedIn account via OAuth 2.0. Easily drops into new or existing sites, integrates with existing users.

Functions in a similar way to the StackExchange/StackOverflow login system.

**In this readme:** [Features](#features) - [Quick Start](#quick-start) - [FAQ](#faq) - [Roadmap](#roadmap) - [History](#history)

*The Settings page is where you'll configure the plugin:*

![wpoa](http://files.glassocean.net/github/wpoa1.jpg)

*The Your Profile page is where users will manage their linked accounts:*

![wpoa](http://files.glassocean.net/github/wpoa2.jpg)

Features
--------
* Fully integrates with WordPress. Drops into existing WordPress sites and integrates with existing WordPress users.
* Supports third-party authentication with Google, Facebook and LinkedIn via OAuth 2.0. Providers can be enabled or disabled.
* Authenticated users are automatically registered and/or logged into their WordPress user accounts. We request the minimum amount of user info (currently the user's email address only) for use when linking their third party accounts to their WordPress user account. No other user info is requested or collected.
* Users can manage their third-party login providers via the standard "Your Profile" WordPress page.
* Pushes the login result message into the DOM which can be extracted via Javascript for notifying the user. Avoids polluting the response url. This feature can also be disabled.
* Supports WordPress Multisite.
* Supports cURL or stream context for the authentication flow.
* The authentication flow was adapted from samples provided by Google, Facebook and LinkedIn. It has been rigorously tested and debugged for solid error handling per several OAuth2 documentations and other resources. Provider implementations share much of the same code (very high code re-use) and the differences between the providers have been fully documented.
* Doesn't require third-party OAuth libraries; everything is built into the plugin first-class. Previously, WP-OpenLogin required LightOpenID and Facebook-PHP-SDK, but this is no longer necessary. Keeps the bloat low and the performance high.

Quick Start
-----------
1. Download and install the WP-OAuth plugin.
2. Setup your desired authentication providers' API key/secret in the WordPress backend under Settings > WP-OAuth.
3. Enable the included login buttons, or add login buttons anywhere to your site/theme with the included shortcodes.

FAQ
---
*I don't get it...what's OAuth? Why does it matter?*
With WP-OAuth, people will have the ability of logging into your WordPress site using their existing Google, Facebook or LinkedIn account. Most people probably already have one of those accounts, and they probably don't want to register/manage yet another account. That's where WP-OAuth bridges the gap by offering a fast and secure alternative login method which might even boost your membership and user retention. Think about it...people like to use the same passwords everywhere; they might even use their same online banking password for logging into your WordPress site. You never know, and you don't want to be held responsible or liable for that.

*Okay then, how does it work?*
Let's say a person chooses to log into your WordPress site with Google. First they click the Google button on your site and are immediately redirected to Google's login page. Right after logging into Google they will be prompted to grant your website permission to access some[1] of their Google user info. Right here, you are observing OAuth in its very basic form. So once they grant access, they are redirected back to your WordPress site, along with an OAuth identity provided by Google. This identity is unique for every Google user and it never changes, so we tie the Google user's identity to a WordPress user account. Now any time in the future when that Google user logs in with Google, we'll match the Google user's identity to an existing WordPress account and log them into your site automatically. If they don't have a WordPress account, WP-OAuth can register one for them automatically as well.

[1] We only request the minimum amount of user info from Google (and all other providers) that will give us the user's OAuth identity. During a request for user info, Google (and all other providers) may send additional information about the user, such as their full name or public email address. We have no way of turning that off, but it shouldn't matter because we discard this info immediately after extracting the user's OAuth identity. I'm not aware of any OAuth provider that lets us *only* ask for a user's OAuth identity, which is kind of a let down and makes me wonder why that is.

*Can I trust WP-OAuth with keeping sensitive user info private and secure? Can WP-OAuth or someone malicious steal a user's  password or hijack their account? If a site gets hacked where WP-OAuth is installed, will sensitive user info be at risk?* WP-OAuth (and the official [OAuth2 spec](https://tools.ietf.org/html/rfc6749)) will never get access to a user's password from their third-party provider; instead they will be redirected to the third-party provider and then redirected back to your site with an identity. There are no exceptions, we don't need your password and we certainly don't want the responsibility of storing it securely! That is the wonderful nature of OAuth. When a user decides to authenticate with a third-party provider through WP-OAuth, we only ask for their basic user info - the minimum info required to obtain the user's OAuth identity. This is not to say WP-OAuth or the OAuth2 spec is fool-proof, as nothing truly is. It turns out that each provider has decided to implement their OAuth2 API slightly differently, which leaves a lot of room for human error when it comes to developers who build and implement OAuth clients, such as WP-OAuth. For more information, see [here](http://lifehacker.com/5918086/understanding-oauth-what-happens-when-you-log-into-a-site-with-google-twitter-or-facebook).

*How is WP-OAuth different than other "OAuth" plugins such as OAuth2 Complete or OAuth Provider?* These turn your site into an OAuth2 *provider* which is probably not what you want. Google, Facebook and LinkedIn are providers and our goal is to be a *consumer* which lets us delegate user authentication to those providers.

*How is WP-OAuth different than other "Connect" plugins such as Google Apps Login or WordPress Social Connect?* These are similar alternatives.

*How is WP-OAuth different than OpenID, OpenID Connect and OpenID Authorization 2.0?* OpenID functions on the assumption that each person will have their own unique identity that can be validated by a third party. However, most people don't have an OpenID and probably don't care for one, which makes OAuth a better choice for non-enterprise login systems because most people *do* have a Google, Facebook or LinkedIn account and should already be familiar with the authentication process of those providers.

*How is WP-OAuth different than SAML or Single Sign-On?* The latter two technologies are for enterprise-scale apps and environments where a single identity is used to gain access to any site or app within an enterprise environment, whether locally or remotely. Basically, the user signs in once and for the lifetime of their session they will have secure, authenticated access to all resources in the enterprise environment. This is not exactly what OAuth2 was designed for. OAuth2 lets us request information about a user from a trusted third-party, which we can assume to be authentic, allowing us to identify the user in some way and associate that user identity with a user account in our site or app. The main difference is that with OAuth2, each site or app that is requesting your user information *must be explicitly granted permission by you* in order to access it. This access can also be revoked *by you*. This gives users full control over their personal info and what sites or apps may use it. OAuth2 was designed with REST in mind and functions exclusively over the HTTP protocol, whereas SAML allows for other implementations.

*Are my third-party credentials safe? Can my Google password get hacked?* Your credentials are safe, and no, your password cannot get hacked. WP-OAuth (and the official OAuth2 spec) will never get access to a user's password. There are no exceptions, we don't need your password and we certainly don't want the responsibility of storing it securely! That is the wonderful nature of OAuth. For more information, see [here](http://lifehacker.com/5918086/understanding-oauth-what-happens-when-you-log-into-a-site-with-google-twitter-or-facebook).

Roadmap
-------
* Login with Github; Login with Reddit.

History
-------
This project is a continuation of [WP-OpenLogin](http://github.com/perrybutler/wp-openlogin) which was originally developed with OpenID in mind. OAuth 2.0 is now the standard.
