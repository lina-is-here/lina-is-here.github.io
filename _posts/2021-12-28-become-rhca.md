---
layout: post
title:  "How to become a Red Hat Certified Architect (RHCA)"
date:   2021-12-28
tags: rhca, certification, linux
---

This year I accomplished one of my goals – [I became Red Hat Certified Architect 
(in Infrastructure)][my-rh-certification-id]. To do this, I had to pass 7 Red Hat exams: 
RHCSA (Red Hat Certified System Administrator),
RHCE (Red Hat Certified Engineer) and
5 exams of my choice.

Why? Because I can!

Here's the strategy I used, and my top tips to pass the exams and earn RHCA.

<!--more-->

## Contents
  - [General Information](#general-information)
  - [Strategy before the exam](#strategy-before-the-exam)
  - [Strategy during the exam](#strategy-during-the-exam)
  - [Strategy after the exam](#strategy-after-the-exam)
  - [Conclusion](#conclusion)


### General Information
**1. [RHCA is 7 exams, 2 compulsory and 5 of your choice.][rhca-info]**
   
   In my case, I did the following exams:
   * EX200 Red Hat Certified System Administrator
   * EX300 Red Hat Certified Engineer
   * EX288 Red Hat Certified Specialist in OpenShift Application Development
   * EX318 Red Hat Certified Specialist in Virtualization
   * EX180 Red Hat Certified Specialist in Containers and Kubernetes
   * EX280 Red Hat Certified Specialist in OpenShift Administration
   * EX447 Red Hat Certified Specialist in Ansible Best Practices

     

**2. Exams can be done at home with remote proctoring.**
   
   There's no need to go anywhere, which is very convenient. But you do need a dedicated desk in a
   room where you can sit without interruption – where other people can't enter and disturb you.
   See more on the [remote exams][redhat-remote-exams].
   

**3. Exams are hands-on exams.**
   
   There are many certifications out there but what's cool (and hard!) about Red Hat exams is that 
   they are more like a real environment, close to the work you actually need to perform. No multiple 
   choice questions or anything like that!


### Strategy before the exam
**1. Choose the exams you are more likely to pass.**
    
   That doesn't mean investigating which exam is the easiest. Although exams are very different in
   their difficulty, it doesn't mean the easy exam will be easy for you.
   Chose the exam for the technology you are familiar with. Or at least have an interest to learn.
 
**2. Set realistic goals.**
   
   From my experience, 1 exam per month is still ambitious but realistic. It is very likely to take 
   longer – you might not be able to study at the desired pace or schedule the exam whenever you want.

**3. Book the exam in advance!** 
   
   This can be very easily overlooked. Sometimes you can finish studying and try to schedule 
   the exam only to find out that the closest available date is one month away. 
   To avoid this, I usually book the exam first (a few weeks in advance) and then start preparation. 
   It also motivates me to study, as I have the date in the calendar already.
   
**4. Start with the easiest (for you) exams first.**

   Exams are continuously updated and replaced, so there might be a chance that the exam 
   you are saving for the dessert will not be available at all.
   
**5. Study the objectives of each exam.** 
   
   You will be evaluated only based on the objectives, so if the preparation material that you use 
   contains something outside the objectives, it's probably interesting and useful information, 
   but not needed for the exam!
   
**6. Know where the documentation is.**

   In general, for all exams – study the documentation, where it is, the structure, examples. 

   For Linux exams `man` and `--help` are your friends.

   For OpenShift exams – get familiar with [OpenShift docs][openshift-docs]. IMPORTANT: choose the 
   version that corresponds to your exam version.

   For Ansible exams – get familiar with [Ansible docs][ansible-docs] or `ansible-doc` command.

**7. Practice a lot.** 
   
   Whether in the practice lab or in your environment, that will save you a lot of time
   at the actual exam.

**8. Prepare the snacks!** 

   It is allowed to have a drink and some snacks at the exam (on the desk).
   As some exams are quite long (up to 4 hours), think what would you like to snack on.
   I like to have nuts, cookies and some sweets at my desk. 
   Be careful with the drinks: you are allowed to take a break after each hour of the exam, 
   but the clock doesn't stop!
   
**9. Remember your password to the Red Hat account.** 

   You will have to log in to your account before the exam starts.
   So if you don't type it often (e.g., have it in the
   password manager), check it and memorize before the exam.


### Strategy during the exam
**1. Learn Vim shortcuts to save your time.**

   You can use any editor to edit files but Vim is usually installed,
   so it's easy to just use it.
   
**2. Read all tasks first.** 

   Some tasks may depend on another, so it makes sense to do them together.

   Some tasks may be destructive - that is, if you make a mistake, all progress will be lost. Do those
   "destructive" tasks first, so to not risk all your work.
   
   After the destructive tasks, do the once that are easy for you and you understand them well.
   The goal is to get as many points as possible. Often, if the task is "half-done", you won't get
   any points at all, so it's a wasted time and effort.

**3. Follow the instruction from the exam page and not from the files.**

   Some exams may contain pre-made files, which you have to modify in a certain way.

   Those files may contain some comments – ignore them, it may only confuse you.
   

### Strategy after the exam

The results usually arrive within a few hours and contain your score,
score in each section, and the conclusion: whether you passed or failed.

**If you passed**, congratulations! Your hard work paid off, and you can start the same process
with the next exam in your list.

**If you failed**, check the score for each section and try to correspond the tasks you did to those sections.

There's no point in arguing with the grading. If you believe what you did was correct and worked, but
the score is low (or zero) in that section, try to think if there's another way of doing it, for example,
doing something through CLI instead of UI.

### Conclusion

As any exam, it requires preparation. My strategy and tips helped me to achieve it rather quickly.
That's it and good luck with your certification! 


[my-rh-certification-id]: https://rhtapps.redhat.com/verify?certId=180-267-163
[rhca-info]: https://www.redhat.com/en/services/certification/rhca
[redhat-remote-exams]: https://www.redhat.com/en/services/certification/remote-exams
[openshift-docs]: https://docs.openshift.com/container-platform/4.9/welcome/index.html
[ansible-docs]: https://docs.ansible.com/ansible/latest/index.html
