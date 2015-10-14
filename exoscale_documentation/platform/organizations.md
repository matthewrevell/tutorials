---
title: "Organizations and Team Work"
slug: "organizations"
meta_desc: "Exoscale allows you to create Organizations, team workspaces where different users can operate on a same account and that will have a separate billing"
tags: "quick-start,organizations"
---

Exoscale allows you to create Organizations, team workspaces where different
users can operate on a same account and that will have a separate billing.

This is ideal for a company for example, where an Owner wants to give rights
to his Techs to operate on the machines keeping the billing stuff for the
Admin and having a specific billing address for the company.

## Personal Accounts vs Organizations

When you register with Exoscale you are creating a Personal Account.
A Personal Account represents a single User, yourself.

Logging in, the account details are relative to you as a User: the funds you
have, the billing address and every detail in the Account menu belongs to you.
The Instances you start, any operation you do, belong to your account and
nobody without your login credentials will be able to manage them.

Organizations, on the other hand, represent a workspace for several Users,
like a company. They have their own billing address, their own funds,
and can be accessed by several different users with different roles.

If you operate as a company we strongly advise you to create an Organization
as first step. **Funds cannot be transferred from your personal account.**
New Organizations will be created with a credit of 0 and a SUSPENDED status.
You will need to make a payment for that Organization before being able to
create resources.

Organizations can have unlimited users and have no added cost.

[@todo image on orgs and accounts]

## Creating an Organization

When you log in you will always find yourself on your personal account.

Selecting the `ACCOUNT` menu on the right and the `ORGANIZATIONS` sub-menu will
bring you to the Organizations view. Here you'll find listed the Organizations
you belong and your role inside each Organization.

Fill in the details, your Organization will be ready in a matter of seconds
and you will be able to switch context. Please note **the details your provide
during the creation of an Organization will be used for billing** too.
You will be able to change them later anyway.

From the Organizations list you are able to switch context or leave an
Organization you are part of.

## Switching Context

Now that you have your Organization settled-up, you can jump from your personal
account to your Organization account. Remember: **when you log-in you will
always find yourself on your personal account and you will need to switch
context before operating on your Organization.**

The easiest way to switch context is to click on the account menu on the top
right. This menu changes his label showing you the context you are in,
Personal Account or Organization. Opened, it lists the Organizations you belong
to, and allows you to switch to those Organizations in a convenient way.

## Add Users to your Organization and manage them

Once in the Organization context the account details will be relative to your
Organization. Some sub-menu will also be different: you will no longer have an
`ORGANIZATIONS` menu but a `USERS` menu instead.

From the Users view you will be able to invite your collaborators to operate in
your Organization. The invitee will receive an email asking to create an account
on Exoscale: once again, users you invite will have their personal account
and will need to switch to your Organization once logged in.

Inviting users you may also define a role for each of them choosing between
three possible options:

* **Owner** An Owner has your same rights: he can do everything you do, and of
  course he can invite other users.
* **Tech** roles can manage resources being used (compute, object storage…),
  but they cannot make payments or edit administrative information, nor
  invite users.
* **Admin** roles can edit your account details and make payments, but don't
  have access to resources and cannot invite other users.

From the same Users list view you will be able to modify each user's role and
to remove the user from your Organization.

## Accounts and API keys

You get a pair of API Keys for each Personal Account: you will find them under
your account details. But you will also receive a pair of keys for each
Organization you are belonging to: that is, your personal API credentials
differ from your Organization credentials, and each user in an Organizations
gets a new, unique pair of keys to operate on that Organization.
