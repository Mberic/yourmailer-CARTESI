# YourMailer
A Blockchain Based Email Service using Cartesi Rollups. This project aims to build a decentralized client and server infrastructure for sending and receiving emails. The DApp created uses the currently existing email protocols (IMAP, POP3 and SMTP). 

YourMailer aims to ensure that uses can trust the people or organizations (in this case Cartesi nodes) that are managing their email server. In early 2023, [Bard claimed](https://africa.businessinsider.com/news/googles-new-bard-chatbot-told-an-ai-expert-it-was-trained-using-gmail-data-the/fjm56cn) that Google used user's Gmail data for its training model. Google came out to deny this. However, there have a lot of incidences (like the 2016 Cambridge Analytica scandal) that have made the public lose trust in these centralized entities.

YourMailer puts users in control of their how & where their information is stored. On top of that, users will be able to choose whether their data is stored using decentralized storage or somewhere like Google Drive. 


## Installation

You need the following in order to test out this project from your local environment:

1. Download the [Thunderbird email client](https://www.thunderbird.net/en-US/). You'll need this to view emails that you've received.
2. JAMES Apache server. An open-source Java server. It supports a number of email protocols such as SMTP, IMAP and POP3. You'll need to download the docker image and set it up using the [instructions here](https://github.com/apache/james-project/blob/master/docs/modules/servers/pages/15-minute-demo.adoc)
3. Cartesi Local Development environment. Use the manual setup from [here](https://docs.cartesi.io/build-dapps/run-dapp/)
