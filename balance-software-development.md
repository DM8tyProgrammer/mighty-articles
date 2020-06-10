---
title: Finding Balance in Software Development
subtitle: Continuous change is the key to balance software development.
description: 'In the philosophical view of Software development, there exist, two realms that affect the reality of Software: product end and Technology end. These two realms are so closely linked that a change in one leads to change in another field.'
image: 'https://miro.medium.com/max/2000/1*VTiL7IE0ATS4FhOPbiuP3Q.png'
datePublished: '2020-05-15'
lastModified: '2020-06-11'
---

Software development is not only about code. It is also about the process and the people around it. At the core, Product (Business) Managers and Developers turn the idea of Software into reality.

In the philosophical view of Software development, there exist two realms that affect the reality of Software: Product end and Technology end.

These are different in the perception of an application:

> An Application is a set of features that support a business function. - Product View

> An Application is a set of code, framework, libraries, services. - Developer View

In contrast, these are also associated in a way that a change in one realm introduces change(s) in another realm.

## Tap in Product Realm

Customer Grievances, Competition Edge, UX improvements, Feedback, and Law Changes: all these introduce changes. After a rigorous thought process (cost-benefit analysis), these changes are passed to developers for transforming into code.
At developers' end, they might have to write new code or adjust with the existing codebase. The complexity of adjustment depends upon changes and existing code.

```
complexity = f(changes, existing_code)
```

_Patching and re-patching of code_ disturb the structural balance of Software. When we patch, we are so focused on the piece of code to work that we ignore the overall stability of Software.

Many times, fixing or enhancing one feature leads to disturbing other features. At one point in time, Software becomes a minefield.

### Do it right from the beginning

A Software can only be well designed if we know what kind of changes to expect in the future.

> We are human; we can p̵r̵e̵d̵i̵c̵t̵ estimate a limited version of the future.

_This option might not always be viable._

The external APIs are needed to be done right from the beginning; with time, it is harder to change interaction points because of uncontrolled dependants. It is nice to have developers with a mixed level of expertise or experience to corrected have better API design from the beginning.

### Continuous Refactoring or Refactor as you go

_Continuous Refactoring_ is a practical solution to keep the structure of Software in check. Sometimes, it may require re-structuring functionalities having no role with current business changes to maintain Software structure stability or consistency.

> There is risk involved with Refactoring, which may cause breaking of existing features and restless nights.

### Test-Driven Development

Test-Driven Development (TDD) helps to mitigate breaking existing features risk. It helps to keep the existing-functionalities in check before and after refactoring.

> You don’t need to follow the rule book of TDD. You just need to have some way of proving the existing features are not disturbed.

## Tap in Technology Realm

New tools, code practices, and technologies attract developers (as everyone wants to do better). When we start the development of any project, we freeze code practices: tools, frameworks, libraries, and coding style. These practices stay constant for years.

More the difference of the current Software development practice to new market trends, more it is harder to keep developers motivated on the current project running on old tracks. With time, the project's business value becomes so much that it is harder to mess up with existing practices.

At one point in the time, when developers quit, finding new developers to work on existing projects is tough.

_Money becomes the only bait_; after all, money solves all the problems. What for how long it would keep developers motivated?

![balance](https://miro.medium.com/max/2000/1*VTiL7IE0ATS4FhOPbiuP3Q.png)

## Continuous Change

I love to borrow some concepts from Design Thinking; whether it is the design of the code, practices, or process, it should be ongoing.

Change is an only forward option, and it does mean you readily accept the changes. To respond to a change, you need to have an open attitude and carefully crafted plan keeping the balance between stability and evolution.

> In the software world, the definition of "right" is always temporary, contextual and opinionated. You have to (re)-discover and (re)-align your version of "right" before it is too late.
