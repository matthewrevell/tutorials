---
title: "Organizations and Team Work"
slug: "organizations"
meta_desc: "Exoscale allows you to create Organizations, team workspaces where different users can operate on a same account and that will have a separate billing"
tags: "quick-start,organizations"
---

Exoscale allows you to create Organizations, team workspaces where different
users can operate on a common account with its own billing.

This is ideal for a company for example, where an Owner wants to give rights
to their technical staff to operate on machines while letting the accounting
department handle billing concerns.

## Personal Accounts vs Organizations

When you register with Exoscale, you are creating a Personal Account.
A Personal Account represents a single User: yourself.

Logging in, account details are relative to you as a User: account funds,
billing address and every detail in the Account menu belongs to you.
All operations performed are restricted to your account and accessing
resources requires your own credentials only.

Organizations, on the other hand, represent a workspace for several Users,
like a company. Organizations have a separate billing address and credit, and
can be accessed by multiple users with different roles.

If you operate as a company, we strongly advise you to create an Organization
as first step. **Funds cannot be transferred from your personal account.**
New Organizations will be created with a credit of 0 and a SUSPENDED status.
You will need to make a payment for that Organization before being able to
create resources.

Organizations can have unlimited users and have no added cost.

[@todo image on orgs and accounts]

## Creating an Organization

When you log in you will always find yourself on your personal account.

Selecting the `ACCOUNT` menu on the right and the `ORGANIZATIONS` sub-menu will
bring you to the Organizations view. Here you'll find a list of Organizations
you belong to and your role inside each Organization.

To create an Organization, simply fill in the details at the bottom of the
page. Organization details include **name and billing address**, which you can
always edit at a later time. Once all details are filled, submit your request
and your Organization will be ready in a matter of seconds.

From the Organizations list you are able to **switch context** or **leave** an
Organization you are a member of.

## Switching Context

Now that you have your Organization ready, you can jump from your personal
account to your Organization account. Remember: **when you log-in you will
always find yourself on your personal account and you will need to switch
context before operating on your Organization.**

The easiest way to switch context is to click on the account menu on the top
right corner. This menu's title tells you which account context you are
currently in. Open it to list your Organizations and switch to the relevant
one in a convenient way.

## Add Users to your Organization and manage them

Once you have switched to your Organization context, account details are
relative to your Organization. Some menus will also be different: you will no
longer see an `ACCOUNT / ORGANIZATIONS` menu but an `ACCOUNT / USERS` menu
instead.

From the Users view you can invite your collaborators to operate in
your Organization. The invitee will receive an email asking to create an account
on Exoscale. Users you invite will have their personal account and will need
to switch to your Organization once logged in.

When inviting users, you need to select a role for each of them from the
following options:

* **Owner**: An Owner has the same rights as the creator of the organization,
  including inviting and revoking people.
* **Tech** roles can manage resources being used (compute, object storage…),
  but they cannot make payments or edit administrative information, nor
  invite users.
* **Admin** roles can edit your account details and make payments, but don't
  have access to resources and cannot invite other users.

From the same Users list view you will be able to modify each user's role or
remove them from your Organization.

## Accounts and API keys

Each Personal Account has its own API Keys: you will find them under
your account details. You also get a pair of keys for each Organization you
belong to: your personal API credentials differ from your Organization
credentials and each user in an Organization gets a new, unique pair of keys
to operate on that Organization.
