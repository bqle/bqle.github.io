---
title: "Photon Commerce"
date: 2022-10-06T08:42:53-04:00
draft: false
---

*My internship with Photon Commerce (Fall and Summer '22)*

<!--more-->
Photon Commerce is a San Francisco-based fintech startup aiming to automate the process of paying for invoices through automating the extraction of key features and automating the money transfer necessary for these payments. My primary goal coming into the internship was clear. I was to develop algorithms that better extract key features from invoices. However, I quickly realized there was significant improvement in the company's data processing pipeline.

### Template-Extraction Pipeline
At my work with Photon Commerce over the summer, one of the biggest challenges we face in automating the extraction of key is identifying templates. Out of a dataset of thousands of files of invoices that come in, a template is considered a file that is representative of a significant portion of that dataset - either content-wise or structurally. Usually, there is a one-to-one correspondence between the vendor (e.g. Workday, FedEx, Saleforce, etc.) and the files that we consider to be templates. This makes sense because the same vendor would most likely use the same template (with the same color, design, and basic information). However, one problem Photon faced was that they had to manually compile this list of templates with every additional set of invoices they collect every month. This manual approach to template identification is prone to human error, lacks organization, and consumes a lot of engineers' time - time that could be spent on algorithm development. 

I came up with a plan to address this issue and automate the extraction of their template identification pipeline, which I proposed to the CEO. Impressed by my plan, he retasked me to implement the automated extraction pipeline. I worked very hard to deliver, and ultimately did. He commended me at the end of the project, "Your work was bold in scope, and you ultimately completed it, saving our company a significant amount of resources and ultimately improved a fundamental part of our product."


Ultimately, I created an automated extraction algorithm using k-means clustering that is automatically run whenever our S3 receives a new batch. I created a novel algorithm for measuring document similarity based on the location of key fields, their values, and visual aspects of the document. Due to the size of the datasets involved (on the order of 10,000s), all this algorithm is run on an automatically-managed remote AWS instance - which saves engineers' compute time and remove the necessity of having someone run the algorithm on their box. After the extraction algorithm completes, the new templates are automatically saved to the S3 bucket - along with information about the batch they came from and the batch accuracy results. I also created an intuitive website that helps labelers label new templates with their key fields efficiently, with their results automatically saved on the cloud. 


I had hoped my pipeline would be able to decrease the dependence on manual labelling. Ultimately, it improved the company's extraction accuracy and increased reproducibility, speed, cost, error-prevention, and version control of their template extraction process.

### FedNow Wrapper

During the latter part of my internship, I got a chance to work towards the other goal of Photon: to automate the payments of bills & invoices. In a conversation with the CEO, I learned that the Federal Reserve had just announced a real-time payment (RTP) system that similar to The Clearing House (TCH)'s RTP system, allowing financial institutions to settle and clear transfers in near real-time. The purpose of this FedNow system is mainly to provide alternatives to the market. The Fed fears having a sole dependence on a single RTP tool may create economic security issues if TCH's service ever becomes unavailable.

Photon happened to be one of the first institutions to adopt the FedNow system. As my final project at Photon, I was tasked to create a simple REST API wrapper around this service in conjunction with one of Photon's partner banks. In one month, I worked hard to propose, desgin, and build a REST API wrapper alongside one of the senior engineers in the company. We planned for the endpoint groups we would have, the database architecture, and integrations for this new service with our existing services. 

Ultimately, although this wrapper is still a draft, it contains over 60 endpoints addressing a variety of needs of the customers (like setting up as an institution/individual, requesting/sending money, sending money as a group, webhooks, etc.)
The codebase was also rigorously tested and I received commendation from the CEO on my development speed, thoroughness, and documentation. The API will be incorporated into one of the company's [products](https://www.photoncommerce.com/smart-pay) next year. 

### Reflection
My experience at Photon taught me lots about the fintech world - both the good and the bad. Besides the technology suite that I was able to work with frequently (AWS, Django, Heroku, cron, etc.), I also learned to work in a team, propose and present my ideas to the CEO, and learn from the feedback I receive. My internship at Photon was an enriching experience that I will remember for time to come.


