---
layout: post
title: "On avoiding complexity"
date:   2020-09-20 14:21:24
description: ""
category:
tags: ["Software Development"]
---

In software engineering we like to keep things simple. The [KISS principle](https://en.wikipedia.org/wiki/KISS_principle) to _"Keep it simple stupid"_
and [YAGNI](https://en.wikipedia.org/wiki/You_aren%27t_gonna_need_it) because _"You aren't gonna need it"_ come to mind.

There is a well known spreadsheet often referenced in articles. It displays the [Top 25 Automakers by Market Cap](https://docs.google.com/spreadsheets/d/1HflVng6sYIb6Gs4pOKiDGtqU5YJ2-hgdM4pRNaT62gs/edit#gid=1052727546). The Market Cap translates to what the stock market thinks the company is worth.

For a while now the chart is being led by Tesla. Why? Because the market thinks Tesla is worth that much. Is it rational or reasonable? Going into arguments one might hear Tesla has a lot of advantages. Access to cheap capital. Battery supply and technology. Software. Vertical integration. Huge amounts of collected data. And of course a CEO which most people know by name. Elon Musk.

Let's think about overtaking Tesla from the perspective of what is often referred to as "legacy automakers". Where would they start?

The legacy automaker does not need to start with building it's own [self driving chip](https://www.theverge.com/2019/4/22/18511594/tesla-new-self-driving-chip-is-here-and-this-is-your-best-look-yet) due to some differences with Nvidia. It might be possible to take smaller steps.

During a SpaceX event [Elon Musk stated](https://twitter.com/Erdayastronaut/status/1203840982497792005):

> The best part is no part. The best process is no process. It weighs nothing. Costs nothing. Can’t go wrong

and

> The thing I'm most impressed with in when I have the design meetings with SpaceX is "What did you undesign?" Undesigning is the best thing. Just delete it. That's the best thing.

Can we apply that philosophy to other fields?

Why not? Remembering the small steps: Let's take a trivial example such as newsletter registrations. Not rocket science.

The following forms will be in german. Sorry for that. There is no need to translate the content. It’s enough to give us a rough overview of the amount of text and input fields. In short. Complexity.

Starting with [Mercedes-Benz](https://newsletter.mercedes-benz.de/anmeldung/).

<img src="https://www.dropbox.com/s/80u98b5k48aewfn/benz_newsletter.png?raw=1" class="image__left"/>

We can select topics we are interested in, such as sports, design, lifestyle and more. Required.

The we get asked for a salutation, an optional honorific, given name, surname and of course the e-mail address.

The text on the bottom informs us about some legal implications.

Mercedes-Benz uses six inputs of which five are required.

<br>
<br>
<br>
How does [BMW](https://www.bmw.de/de/footer/footer-section/newsletteranmeldung.html) do it?

<img src="https://www.dropbox.com/s/3kquv0jfb6ouz3z/bmw_newsletter.jpeg?raw=1" class="image__left"/>

Similar to Mercedes-Benz. Although BMW makes do with one input field less. They do not ask us for our given name.

Sadly the confirmation button is not fully visible on Safari Mobile and we get a wall of text explaining things to us.

As a result four inputs of which three are required.
<br>
<br>
<br>
<br>
<br>
<br>
<br>

Next [Audi](https://www.audi.de/de/brand/de/neuwagen/layer/newsletter-bestellen.html)

<img src="https://www.dropbox.com/s/9x4s3g26tls9wxp/audi_newsletter.PNG?raw=1" class="image__left"/>
Salutation, optional honorific (Dr., Prof., Prof. Dr.), given name, surname and e-mail.
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<img src="https://www.dropbox.com/s/1dejxgrln29hbha/audi_newsletter.jpeg?raw=1" class="image__left"/>

Even solving a Captcha is required. Of course we don’t want all these bots receiving newsletters.

Audi uses six inputs. Five of them required
<br>
<br>
<br>
<br>
<br>
<br>

Turning to [Volkswagen](https://www.volkswagen.de/app/formulare/vw-de/newsletter/de).

<img src="https://www.dropbox.com/s/5g696p0l6k4h1fk/vw_newsletter.PNG?raw=1" class="image__left"/>

We are offered the optional honorific as a group of radio buttons instead of a dropdown. An improvement for selections where there aren’t lots of choices.

The checkbox states whether we want to register for the newsletter. We still have to press a button further down though.

Why the button alone would not be enough seems odd.

We have six inputs. Five are required.

<br>
<br>
On with [Porsche](https://contact.porsche.com/germany/dialog/newsletter/subscribe/)

<img src="https://www.dropbox.com/s/gys0tp6pbdcr3fc/porsche_newsletter.PNG?raw=1" class="image__left"/>

Salutation, optional honorific, given name, surname and e-mail.

Although we are asked for our country. The choices in the german version are Germany and Austria. Radio buttons would have improved that.

Seven inputs. Six required.
<br>
<br>
<br>
<br>
<br>
<br>
<br>

How about [Opel](https://www.opel.de/tools/newsletter.html)?

<img src="https://www.dropbox.com/s/kwko08j3ehvz4y6/opel_newsletter.jpeg?raw=1" class="image__left"/>


Wait? Seems they are not asking for the honorific. Thus no optional fields. Thumbs up.

One could guess there are some doctors and professors driving an Opel but the experience of Opel might differ. As a result we have to enter the e-mail twice. After all if Opel expects users to not have a PhD they might misspell their e-mail. Joking aside. Leaving the honorific reduces complexity. Although they relinquish their lead a few input fields later by asking for the e-mail twice.

Summarised: Five inputs. All of them required.

<hr>

Let’s recap. All six (Mercedes-Benz, BMW, Audi, Volkswagen, Porsche and Opel) ask for personal data one might deem unnecessary for the act of sending a newsletter. They differ in nuances. BMW makes do without a given name, Audi has a captcha, Porsche asks for the country and Opel does not offer the honorific.

7 inputs for Porsche. 6 for Mercedes-Benz, Audi and Volkswagen. 5 for Opel and 4 for BMW.

We get that all the info is used somewhere. If only for greeting the user properly in the first line of the newsletter.

Would it be possible without it?

How does [Tesla](https://www.tesla.com/de_DE/updates), do it?

<img src="https://www.dropbox.com/s/zcljgy9z01cce0m/tesla_newsletter.png?raw=1" class="image__left"/>

One simple field. No legal texts. No checkboxes. Only a field for the mail and one button to confirm your choice.

Leaving enough space for a picture of a car. The product you are interested in. Probably the reason for you registration in the first place.

Clean design. It’s also a signup that fits into a single screen on a smartphone. No scrolling required.

So how come for something that only requires a valid e-mail address we are asked for much more? All we can do is guess. Maybe because there is some company document around which defines how one should address a (potential) customer.

Does the customer want to be addressed that way? Maybe. Maybe all he is interested in is the information of the newsletter. Not a greeting containing his given name and surname with proper honorific and a correct salutation. If we really care then we could give an optional text input field where the customer can enter how he wants to be addressed. Or simply have all the non e-mail fields optional. Then we could track how many registrations fill out this optional field and thus whether we need it at all.

Aside from not bothering the user any more than necessary there are additional benefits.

We avoid complexity in our software. No need to have tests around which verify logic such as `Honorific is not required for a newsletter registration` or `Both surname and e-mail are required for a newsletter registration`. Remember the agile principle of simplicity and the art of maximizing the amount of work not done?

Moving to other markets also becomes easier. If we are launching in Korea we won’t have to translate all the fields and then, when creating the e-mail, make sure the family name comes first when in Germany the family name comes last. Less of the things that can go wrong.

As an addition we avoid edge cases dealing with users who are addressed by a single name ([mononym](https://en.wikipedia.org/wiki/Mononymous_person)).

Now can we just remove the fields, or make them optional, and be done with it? Probably not because some systems might require the data and do not expect a firstname to be without a value. Now we're in the position to decide what are the priorities. There are always opportunity costs. Are newsletter registrations something the company should spend time and money on? Only the company can decide.

Let's hope the article helps to make the decision easier and to avoid the pitfalls again when redesigning or overhauling these systems in the future.
