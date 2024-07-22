# Event-Driven Sales Data Processing.

## Requirement
Sales data often arrives in JSON format, containing both order information and contact information. However, inconsistencies and incomplete records can lead to data integrity issues, complicating downstream processing and analysis.

The current challenge is to design a robust, scalable, and automated system to handle incoming sales data files, validate each record, and appropriately store the data based on its validity. Specifically, the system should:

Automatically detect and process new sales data files uploaded to an S3 bucket.
Validate each record to ensure the presence of both order information and contact information.
Insert complete records into a DynamoDB table for reliable storage and further processing.
Redirect incomplete records to a Dead Letter Queue (DLQ) in SQS for further inspection and correction.
The solution must be highly scalable to handle varying volumes of data, ensure data integrity, and streamline the sales data management process.

## Example records.

Here the first record has both order information and contact information but the second record has only order information.
>{
>    "orders": [
>        {
>            "contact-info": {
>                "name": "Foo Bar",
>                "email": "foobar@test.com"
>            },
>            "order-info": {
>                "OrderId": "1",
                "item-id": "101",
                "item-desc": "Item One",
                "qty": "1"
            }
        },
        {
            "order-info": {
                "orderId": "2",
                "item-id": "102",
                "item-desc": "Item Two",
                "qty": "2"
            }
        }
    ]
}

## Architecture
![architecture pic](project_architecture.png)

When a new sales data file is uploaded to the S3 bucket, an EventBridge rule triggers an AWS Step Functions workflow to process the file. Each record in the file is assessed using an AWS Lambda function to determine if it contains both order information and contact information.

If both order and contact information are present, the lambda function returns the record and is inserted into a DynamoDB table
by the workflow which can be used for further processing and storage.
If the contact information is missing, the lambda function raises an exception and the workflow places the record in a Dead Letter Queue (DLQ) in SQS for further inspection and correction.

The 

> Live demo [_here_](https://www.example.com). <!-- If you have the project hosted somewhere, include the link here. -->

## Table of Contents
* [General Info](#general-information)
* [Technologies Used](#technologies-used)
* [Features](#features)
* [Screenshots](#screenshots)
* [Setup](#setup)
* [Usage](#usage)
* [Project Status](#project-status)
* [Room for Improvement](#room-for-improvement)
* [Acknowledgements](#acknowledgements)
* [Contact](#contact)
<!-- * [License](#license) -->


## General Information
- Provide general information about your project here.
- What problem does it (intend to) solve?
- What is the purpose of your project?
- Why did you undertake it?
<!-- You don't have to answer all the questions - just the ones relevant to your project. -->


## Technologies Used
- Tech 1 - version 1.0
- Tech 2 - version 2.0
- Tech 3 - version 3.0


## Features
List the ready features here:
- Awesome feature 1
- Awesome feature 2
- Awesome feature 3


## Screenshots
![Example screenshot](./img/screenshot.png)
<!-- If you have screenshots you'd like to share, include them here. -->


## Setup
What are the project requirements/dependencies? Where are they listed? A requirements.txt or a Pipfile.lock file perhaps? Where is it located?

Proceed to describe how to install / setup one's local environment / get started with the project.


## Usage
How does one go about using it?
Provide various use cases and code examples here.

`write-your-code-here`


## Project Status
Project is: _in progress_ / _complete_ / _no longer being worked on_. If you are no longer working on it, provide reasons why.


## Room for Improvement
Include areas you believe need improvement / could be improved. Also add TODOs for future development.

Room for improvement:
- Improvement to be done 1
- Improvement to be done 2

To do:
- Feature to be added 1
- Feature to be added 2


## Acknowledgements
Give credit here.
- This project was inspired by...
- This project was based on [this tutorial](https://www.example.com).
- Many thanks to...


## Contact
Created by [@flynerdpl](https://www.flynerd.pl/) - feel free to contact me!


<!-- Optional -->
<!-- ## License -->
<!-- This project is open source and available under the [... License](). -->

<!-- You don't have to include all sections - just the one's relevant to your project -->
