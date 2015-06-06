## Making Infrastructure S'mores With Chef

---

# whoami

![inline 65%](images/chef.png)![200%](images/ado.png)
![60%](images/dodchi.png)![50%](images/licenseplate.jpg)

---

![right](images/chef-code.png)
# What is Chef?

- Define reusable resources and infrastructure state as code
- Manages deployment and
on-going automation
- Community content available
for all common automation tasks

---

![](images/panic.jpg)
## Anyone can do anything?

---

![](images/oldtech.jpg)

# Old Way

## Communicate via tickets

^ This sucks because we are explaining things in English, and there can be confusion.

---

![right](images/newhotness.gif)
# New Way
## Communicate via code

^ What is a version control system but basically a communication tool?

---

# Domain experts

- Systems are complicated today
- Nobody can know everything about the stack
- Let your domain experts contribute their portion directly

^ If you have a group that just writes the configuration code as requested by someone else, all you've done is move silos around.

---

# Configuration Drift

^ Configuration drift occurs when the desired state of the system no longer matches the current state of the system.

^ This can happen because you change the desired state (we want SSH listening on another port) or something happens on the box (someone logs onto it interactively and makes a change)

---
![left filtered](images/burgess.jpg)


# Don't do things by hand
> Every time someone logs onto a system by hand, they jeopardize everyone's understanding of the system
-- Mark Burgess

---
# People make mistakes

## This doesn't scale

^ You aren't going to fix the humans, so fix the system

---
# Infrastructure as code

> Enable the reconstruction of the business from nothing but a source code repository, an application data backup, and bare metal resources
-- Jesse Robins

---
## Versioned

## Modularized

## Tested

---

# Executable Documentation

---
![left](images/hugemistake.png)

## How do I make sure nobody messes stuff up?
