---
layout: post
title:  "Eric S Raymond’s Unix Philosophy applied to design Cloud Architectures"
description: "Eric Steven Raymond in his book, The Art of Unix Programming, wrote 17 rules of Unix philosophy. I find most of these rules very relevant for designing cloud architectures and has its application in the design thought process." 
categories: [architecture]
tags: [cloud]
date:   2017-03-14 07:55:09 +0530
---
Eric Steven Raymond in his book [The Art of Unix Programming](https://books.google.co.in/books?id=H4q1t-jAcBIC&printsec=frontcover&source=gbs_ge_summary_r&cad=0#v=onepage&q&f=false) wrote 17 rules of [Unix philosophy](https://en.wikipedia.org/wiki/Unix_philosophy). I find most of these rules very relevant for designing cloud architectures and has its application in the design thought process.

1. **Rule of Modularity: Write simple parts connected by clean interfaces.**
Design for modularity and decouple the various components and services.
2. **Rule of Clarity: Clarity is better than cleverness.**
Write code that can be easily understood by somebody reading it. Well commented with a simple logical flow and humanly readable.
3. **Rule of Composition: Design programs to be connected with other programs.**
Design modules which are independent from each other. An output of one module may be able to invoke another one on demand. Any change or replacement of a module should have little impact on another module with something to glue them together for example, AWS SQS.
4. **Rule of Separation: Separate policy from mechanism; separate interfaces from engines.**
By separating the back end from the front end and bridging them together. An example, is a case where the data generated is much larger than the speed with which a database can take those as an input. So a queuing mechanism to accept the input at a higher speed and transferring the data back into the database at a speed which is reasonable. Any impact or change to any module should have no impact on the other module.
5. **Rule of Simplicity: Design for simplicity; add complexity only where you must.**
Plan with just the components required. Minimalistic and simple design. Clear architecture diagram helps.
6. **Rule of Parsimony: Write a big program only when it is clear by demonstration that nothing else will do.**
7. **Rule of Transparency: Design for visibility to make inspection and debugging easier.**
8. **Rule of Robustness: Robustness is the child of transparency and simplicity.** Things will break. Design for failure. Design for elasticity and scalability, considering special situations. Code for auto recovery than just monitoring and recovery through human intervention.
9. **Rule of Representation: Fold knowledge into data so program logic can be stupid and robust.**
10. **Rule of Least Surprise: In interface design, always do the least surprising thing.**
11. **Rule of Silence: When a program has nothing surprising to say, it should say nothing.**
12. **Rule of Repair: Repair what you can — but when you must fail, fail noisily and as soon as possible.**
Design for auto recovery. Use alarms and notifications. Log events, but design in a way that logs are not expected to be read by humans on a regular basis. Trigger notifications and alerts from the logs for exceptions to be looked into. Consider the triggers to take automatic actions.
13. **Rule of Economy: Programmer time is expensive; conserve it in preference to machine time.**
Write codes in high level languages like Python etc. No point reinventing the wheel. Reuse code. Build on what has already been built.
14. **Rule of Generation: Avoid hand-hacking; write programs to write programs when you can.**
15. **Rule of Optimization: Prototype before polishing. Get it working before you optimize it.**
Monitor the performance and fine tune. A huge benefit of the cloud is to take advantage of the scalability and not being too worried about capacity planning at the start. Monitor performance and fine tune progressively. Use DevOps to roll out incremental updates and fine tune performance. Think agile development.
16. **Rule of Diversity: Distrust all claims for “one true way”.**
Design with open system in mind. Leverage APIs.
17. **Rule of Extensibility: Design for the future, because it will be here sooner than you think.**
Expandable and scalable designs keeping future growth in mind.