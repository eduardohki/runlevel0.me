+++
title = "Essential Tools and Practices for Effective IT Platforms"
date = "2025-02-20T12:00:00+01:00"

description = "A non-exhaustive list of tools every platform should provide to its internal users, and of practices that every technical organization should consider adopting."
categories = ['Software Engineering', 'Platform Engineering']
series = ['Egineering Practices']
tags = ['Practices', 'Tools']
+++

## Introduction

This is an ongoing and non-exhaustive list of tools every platform should provide to its internal users,
and of practices that every technical organization should consider adopting.

These considerations are opinionated, but are the result of my experience in leading and enabling technical
teams while conveying the insights and expected outcomes to executive and non-technical teams.

## Essential Tools

### Password Manager

Password managers are the absolutely minimal baseline for security, as they are like having a secure
digital vault that does all the heavy lifting in terms of setting complex and secure passwords and avoiding
password reuse accounts. Despite being called "password" managers, they are really "secret" managers,
and can (and should) be used to store critical secrets like SSH keys, TLS Certificate Keys, API Tokens, etc.

Not all password managers are the same, thus you may need to consider the following requirements:

* **_Absurdly_ easy to use**: using password managers need to make things easier, not harder.
  The key here is: the secure way is the easiest way.
  Bonus points if the provider has good educational material you can share with your users!

* **Centrally-managed**: it should allow you to share password between teams, centrally recover accounts,
  set different/multiple levels of access and leverage your existing SSO from Google/Microsoft/etc to log-in.
  Solutions like KeePass and Chrome's built-in password manager won't do!

* **Managed for you**: you don't want to self-host your own Password Manager, as it is non-trivial and requires
  operational excellence you may frankly not have. Just buy one that has a good compliance and trust record
  and move on with your team's life.

* **Provide a CLI**: this is often overlooked, but it makes a huge difference for operations and software teams!
  Having a CLI one can use to securely fetch secrets during development/build/tests/deployment means folks will
  have productive means to avoid _storing secrets in git_ (even if "encrypted") or having to manage secrets
  manually instead of having all these things securely automated. This is another use of the _secure digital vault_
  I mentioned above.

* **Support for MFA/2FA login**: this is critical for security, as it helps reduce the risk of leaking secrets
  if a user's master password gets leaked.

* **Hosted in your region**: basically you want your provider to host your secrets in the country/region you are based in.
  This means being hosted in the EU if you operate in the EU, in the US if you are on the US, etc.

Solutions I have used that meet these requirements are:

* [1Password](https://1password.com/business-security): for me it has the best usability of all Password Managers,
  and has excellent educational resources for non-technical folks.
* [Bitwarden](https://bitwarden.com/products/business/): this one has the advantage of being open-source and has
  the widest SSO integration options available. In the past the usability wasn't so good as 1Passwords', but
  they have recently done a great job by improvit it significantly, so it isn't a concern for me anymore.

## Essential Practices

### Product Teams should have end-to-end ownership of their application(s) lifecycle

This includes owning their code, databases, monitoring, patching and infrastructure.

The role of the Platform Team is to educate, enable and provide _paved ways_ for teams to be more more productive
while keeping their capability to independently decide how they will solve these problems.

Should Product Teams need or choose to deviate from these _paved ways_, they must take the responsibility for and
demonstrate maturity in relation to concerns that the Platform already addresses, e.g. infrastructure costs,
security, observability and so forth. By doing this the Platform Teams therefore don't fall into the common trap
of being overwhelmed by the amount of things they need to maintain, while not being a blocker for innovation coming
from Product Teams. It is also common that other Product Teams may benefit from such "deviations", meaning that
these are naturally added to the _paved ways_ supported by the Platform Team.

### Optimize resources around Teams

Teams should be considered as the minimal unit of value delivery, therefore all concerns regarding code
repositories, infrastructure, continuous integration and deployment should take this into account.
The Platform Team _must adjust its own internal services and approach_ to meet these needs, rather than
assuming it knows what's right for the Product Teams.

In practice, this can be defined as having:

* A single code repository per Product Team instead of per technology or application: so we avoid the coordination
  and testing overhead of making changes across multiple repositories and applications.

* A dedicated cloud environment and infrastructure resources per product instead of per environment: in this way,
  we enable greater team independence while having clear isolation of resources for each Product Team.

This enforces that teams collaborate by setting clear boundaries in the form of APIs (be it via software or
via formalities), which is more productive for everyone.
