---
layout: two-cols-header
---

<h1 mt-10>Thanks</h1>

My gratitude goes to

[@Alex Nicosia](https://www.linkedin.com/in/alexnicosia/) & [PyCon LT](https://pycon.lt/)'s organizers! ❤️ 

::left::

# Let's connect!

<logos-github-icon /><logos-gitlab-icon /> [@ninoCan](https://github.com/ninoCan)  <logos-linkedin-icon ml-3 /> [antonino.cangialosi](https://linkedin.com/in/antonino.cangialosi)

*Slides & resources: [talk-operation-data-engineer](https://github.com/ninoCan/talk-operation-data-engineer)*

<img 
style="scale: 40%; margin-top: -135px; margin-left: -40px; border-radius: 25px;"
src="../assets/qr-code.png">

::right::

<h1 style="margin-top: -50px;">We are hiring!</h1>

<img ml-15 mt-10 class="rounded" style="background: #fff; transform: scale(1.3); padding: 30px" src="https://www.agilelab.it/hubfs/logo-agilelab.png">

<h3 ml-15 mt-10 ><span>Check our <a href="https://careers.agilelab.it/">open positions!</a></span></h3>

<!--
TIMING: 30 seconds

"Thank you. I want to thank Alex Nicosia and the PyCon LT organizers — this talk wouldn't exist without them."

"I'm Nino. You can find me on GitHub and LinkedIn at the handles on screen. The slides and all resources are at the repo linked here — feel free to scan the QR code."

"And if you're on the job market: we're hiring at Agile Lab. Come talk to me after."

"Happy to take questions!"

LIKELY QUESTIONS:
- Q: Is the Unified Star Schema book worth reading?
  A: Yes, especially if you work heavily in OLAP. It goes much deeper into union bridge patterns and covers edge cases this talk doesn't have time for.
- Q: When should I NOT use an OBT?
  A: When the consumers are other engineers or pipelines that need normalized data. OBTs are a terminal node in the data journey — for dashboards and analysts, not for upstream processing.
- Q: How do you handle updates in an OBT?
  A: Typically you don't — OLAP tables are append-only or full refreshes. If you need update semantics, you're probably looking at the wrong tool for the job.
-->
