---
title: "Project 6-Multi-lingual Named Entity Recognization"
excerpt: "NER<br/><img src='/images/proj6.png'>"
collection: projects
---

As the Senior Solutions Architect, you have have been tasked with optimizing the data transfer to more cost-effective solutions. You have identified the cost savings of moving a MySQL database currently running on an EC2 instance to an RDS Aurora MySQL instance, and have decided to use the AWS Data Migration Service. Let's get started!

Ensure you are in us-east-1 and log in to the AWS console using the credentials provided.

Note: The lab uses an RDS MySQL instance target to stand in for an Aurora DB to speed the lab's setup for you. The migration process is the same.

# Project overview


## Preprocessing

- 1. Eliminate handles and URLs

- 2. Tokenize the string into words. 

- 3. Remove stop words like "and, is, a, on, etc."

- 4. Stemming- or convert every word to its stem. Like dancer, dancing, danced, becomes 'danc'. You can use porter stemmer to take care of this. 

- 5. Convert all your words to lower case. 
