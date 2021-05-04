---
title: "Sturgeon Manager"
excerpt: "I developed Sturgeon Manager as the software platform that manages production, inventory, pricing, and customers for the Mote Marine Lab's Sturgeon Demonstration Program as a software contractor from 2013 to 2014."
header:
  image: /assets/images/sturgeon/Mote_Caviar.jpg
  teaser: assets/images/sturgeon/Mote_Caviar.jpg
tags:
  - Aquaculture
  - VB.NET
  - Microsoft Access
  - Relational Database
  - Software Contractor
  - Crystal Reports
---
I developed [Sturgeon Manager](https://github.com/mochiliu/Sturgeon-Manager-v1.0) as the software platform that manages production, inventory, pricing, and customers for the [Mote Marine Lab's](https://mote.org/) [Sturgeon Demonstration Program](https://www.heraldtribune.com/article/LK/20120416/News/605201142/SH) as a software contractor from 2013 to 2014. The goal of the non-profit fish farm was to demonstrate that expensive caviar can be produced in a farm setting rather than from poaching endangered animals in the wild. The farm was later spun off as a [private for-profit company](https://www.forbes.com/sites/robindschatz/2015/11/30/caviar-dreams-a-florida-startup-bets-on-sustainable-sturgeon-farming-amid-palms-and-spanish-moss/?sh=5bf1494d7180).

<figure>
  <center><img src="/assets/images/sturgeon/deckhand.jpg" style="width:50%"></center>
  <figcaption>Me working on the farm moving fish!</figcaption>
</figure>

When I worked as an intern at sturgeon farm, I noticed that the business keeps track of its orders and inventory on disorganized Excel files and lose pieces of paper. To help the farm gain control of its own data, I pitched a plan to build a database that allows the farm to digitalize and analyze information from production to sales. The scope of my project quickly expanded to 15 interconnected tables with tens of thousands of entries, including individual fish, packaged caviar containers, customers, pricing, and invoices. 

<figure>
  <center><img src="/assets/images/sturgeon/relationships.png" style="width:100%"></center>
  <figcaption>Relational database that tracks data on the farm.</figcaption>
</figure>

Working with my clients, I also developed the front-end application [Sturgeon Manager](https://github.com/mochiliu/Sturgeon-Manager-v1.0) for intuitive operation, built-in product barcode scanning and printing, and automated report generation. Since the system became operational, it accurately managed an inventory and invoices valuing more than a million dollars, while saving employees thousands of man-hours by streamlining workflow.

<figure>
  <center><img src="/assets/images/sturgeon/invoice_page.png" style="width:100%"></center>
  <figcaption>Screenshot of the GUI for managing invoices. Every tin of caviar is tracked from the harvested fish through shipment.</figcaption>
</figure>

The front end application was written in VB.NET. The relational database was constructed and queried using Microsoft Access. The automated reporting was made using Crystal Reports. None of this would have been possible without Pro Sherman, who I worked with closely to implement this overhaul. He also meticulously wrote the documentation shown below.

<object data="https://mochiliu.github.io/assets/images/sturgeon/SturgeonManagerManual.pdf" type="application/pdf" width="100%" height="100px"> 
  <p>It appears you don't have a PDF plugin for this browser.
   you can <a href="https://mochiliu.github.io/assets/images/sturgeon/SturgeonManagerManual.pdf">click here to
  download the PDF file.</a></p>  
</object>

Mote Caviar photo courtesy of [Mote Marine Lab](https://mote.org/news/article/mote-marine-laboratory-and-southeast-venture-holdings-announce-spin-off).