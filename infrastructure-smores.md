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

^ I thought devops was about trust? Can't we just have people do whatever they want?

---

#[fit]Testing is essential

^ It's not about trusting that someone won't do something malicious. Again, people make mistakes. Sidney Dekker’s Field Guide to Understanding Human Error is probably the most important book for people like us, meaning people that are in the IT world – it’s very accessible and gives lots of examples from fields outside of IT, but they’ve very relevant to what we do.

---
![right](images/cute.gif)
#Communicate through code (redux)

^ If you have people who care about things, they should be part of the code review process.

---
##What happens when you have one group writing all the automation?

---

![](images/crackers.jpg)

^ If the infrastructure team does it all, the s'mores might just be a bunch of graham crackers, because that is all they know.

---
![](images/disconnected.jpg)

^ If the teams don't collaborate, then nothing merges together

---
![](images/bananas.jpg)

^ And if you have a group that doesn't understand your systems at all, you get a banana instead of a s'more.

---
#[fit]How do we solve this?

---
#[fit]Use a pipeline

^ Infrastructure doesn't live in a vacuum. Things depend on each other.

---

#[fit]Chef audit mode as the final test

---

### Example of an audit cookbook

```ruby
control '6.9 Ensure FTP Server is not enabled' do
  it 'is not running the vsftpd service' do
    expect(service('vsftpd')).to_not be_running
    expect(service('vsftpd')).to_not be_enabled
  end

  it 'is not listening on port 21' do
    expect(port(21)).to_not be_listening
  end
end

```
^ This cookbook is executed after all recipes are applied in the pipeline

---
## Encourage local testing with Foodcritic

^ We want to test this stuff locally so people know if they do non-compliant chef code

---
# Example Foodcritic custom rule
```ruby

rule 'COMP001', 'Do not allow recipes to mount disk volumes' do
  tags %w{recipe compliance}
  recipe do |ast|
    mountres = find_resources(ast, :type => 'mount').find_all do |cmd|
      cmd
    end
    execres  = find_resources(ast, :type => 'execute').find_all do |cmd|
      cmd_str = (resource_attribute(cmd, 'command') || resource_name(cmd)).to_s
      cmd_str.include?('mount')
    end
    mountres.concat(execres).map{|cmd| match(cmd)}
  end
end
```
---
# Error output from foodcritic
```bash
$ foodcritic –I /afs/getchef.com/foodcritic-rules/rules.rb .
COMP001: Do not allow recipes to mount disk volumes: ./recipes/default.rb:20
COMP001: Do not allow recipes to mount disk volumes: ./recipes/default.rb:26
```
---
![right](images/micdrop.gif)
# Questions?
---
##resources
- Sidney Dekker - *Field Guide to Human Error*
- foodcritic.io
- https://github.com/chef-cookbooks/audit-cis
- http://jtimberman.housepub.org/blog/2015/04/03/chef-audit-mode-introduction/
- twitter.com/mattstratton
