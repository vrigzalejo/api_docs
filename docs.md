---
title: API Reference

language_tabs:
  - shell
  - ruby
  - python

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='http://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

#Overview
##Getting Started
##Authentication
###Overview
The SurveyMonkey API currently supports OAuth 2.0.  You can read more about [OAuth 2.0](http://oauth.net/2/) and see the [official RFC spec](http://tools.ietf.org/html/rfc6749).  We also have an [OAuth Guide](http://developer.localmonkey.com/docs/guides/oauth-guide/) that will walk you through the process of using the SurveyMonkey APIs.

![](https://developer.surveymonkey.com/files/oauth_2.png)

###OAuth Client Libraries
* [PHP](http://www.phpclasses.org/package/7700-PHP-Authorize-and-access-APIs-using-OAuth.html)
* [PHP OAuth 2.0 Client](https://github.com/fkooman/php-oauth-client)
* [Cocoa](http://github.com/leebyron/cocoa-oauth2)
* [iPhone and iPad](http://github.com/lukeredpath/LROAuth2Client)
* [iOS and Mac OS X (draft 10)](http://github.com/nxtbgthng/OAuth2Client)
* Java
  * [Apache Amber (draft 22)](http://incubator.apache.org/amber/download.html)
  * [Spring Social](http://www.springsource.org/spring-social)
  * [Spring Security for OAuth](http://static.springsource.org/spring-security/oauth/)
  * [Restlet Framework (draft 30)](http://www.restlet.org/)
* Python
  * [sanction](http://github.com/demianbrecht/sanction)
  * [rauth](http://github.com/litl/rauth)
* [Ruby Gem](http://github.com/intridea/oauth2)
* [Ruby](http://github.com/aflatter/oauth2-ruby)
* [Javascript](http://github.com/andreassolberg/jso)
* .NET
  * [DotNetOpenAuth](http://www.dotnetopenauth.net/)
  * [Spring Social for .NET](http://www.springframework.net/social/)

###Unauthorizing an App
To unauthorize an app simply log into the user's SurveyMonkey and follow [these instructions](http://help.surveymonkey.com/articles/en_US/kb/How-do-I-link-my-SurveyMonkey-account-to-my-Facebook-account)

##Requests/Responses
###Requests
>Example Request

```shell
curl -H "Authorization:bearer XXXYYYZZ" -H "Content-Type: application/json" http://api.surveymonkey.net/v2/surveys/get_response_counts?api_key=XYZ --data-binary '{"collector_id": "23907195"}'
```

All of our requests are done with the HTTP POST method, are accompanied with a JSON payload and a few required HTTP request headers.

####Required Request Headers
Key | Description
--------- | -----------
Authorization | User Authorization token (always starts with 'bearer')
Content-Type | Should always be 'application/json' right

####Required Request Parameters
Key | Description
--------- | -----------
api_key | Your api key should always be a param on the url



###Responses
>Example Response

```json
{
    "data": {
        "completed": 10,
        "started": 1
    },
    "status": 0
}
```

All our API requests return HTTP 200 (OK) with a JSON payload that includes a `status` key containing one of the following codes.

####Status Codes
Key | Description
------- | -------
0 | Success
1 | Not Authenticated
2 | Invalid User Credentials
3 | Invalid Request
4 | Unknown User
5 | System Error

##API Limits and Quotas
New API keys start out with a default rate-limit of 1,000 requests per day, and 2 requests per second, so please remember to swap in your own Application key when copying example request URLs from our interactive docs.

You may also associate your developer account with a your SurveyMonkey account. Doing so will enable us to adjust your rate limits to match your SurveyMonkey plan.To associate your accounts, sign into your developer account by clicking the "Sign In" button in the upper right. When you do that the first time, you'll see a yellow banner with a button that allows you to connect your developer account to your SurveyMonkey account. Click that and connect your accounts. The rate limits changes will take effect within about one hour.

If you need a higher rate limit, you can [upgrade your SurveyMonkey account](https://www.surveymonkey.com/pricing/upgrade/) to the appropriate plan level.
###Request Limits
Plan | Max Requests Per Second | Max Requests Per Day
------- | ------- | -------
Default | 2 | 1,000
SELECT | 4 | 5,000
GOLD | 6 | 7,000
PLATINUM | 10 | 15,000

###Response Limits
We also have some global response limits for database utilization fair use.

Property | Limit
------- | -------
Max Page Size | 1000 unless otherwise specified
Max Survey Size | 1000 questions, surveys over limit will not be returned

###Increasing Limits
If you think you may be in danger of exceeding your request-limit threshold, please [fill out this form](https://surveymonkey.wufoo.com/forms/apis-advanced-access-request/) with an estimate of your anticipated API activity. We should be able to increase your limit to meet your application's needs.

##Data Types
###Primitives
####Integer
An integer number with a maximum value of 2147483647. Negatives are disallowed unless otherwise specified.
####String
A string of text up to 255 characters unless otherwise specified
####Boolean
A boolean value: true or false. In JSON it will be represented using the native boolean type.
####DateString
Dates are always in the format YYYY-MM-DD HH:MM:SS. All DateStrings are implicitly in UTC. They MUST be entered like this (we don't support ISO 8601 or RFC 2822 yet)
####Array
A simple list of values. In JSON this will be an array.
####Null
A null value. In JSON this is represented as the native null type.

###URLs
A valid URL where the protocol can be either http or https depending on the resource and the user's account settings.

###Question Types
SurveyMonkey has many different question types.  These combinations of family and sybtype can be found in the `get_survey_details` response.

Question Family | Question Subtype | Description
------- | -------
`single_choice` | `menu` | Dropdown
Multiple Choice Question Type | three
allow multiple option UNCHECKED | four

#Guides
##OAuth Guide
##Request Guide
##Polling Guide
#API Methods
##POST: create_flow
###Details
Create a survey, email collector, and email message based on a template or existing survey.
###Notes
* You cannot specify both template_id and from survey_id
* Maximum number of recipients supported is 10000

###Endpoint
`https://api.surveymonkey.net/v2/batch/create_flow?api_key=your_api_key`

###Request Data
>Example Request

```shell
curl -H 'Authorization:bearer XXXYYYZZZ' -H 'Content-Type: application/json' https://api.surveymonkey.net/v2/batch/create_flow?api_key=your_api_key --data-binary '{"survey": {"template_id": "568", "survey_title": "Ice and Fire Event"}, "collector": {"type": "email", "name" : "email invite", "recipients": [{"email": "xyzstannis@surveymonkey.com", "first_name": "Stannis", "last_name": "Baratheon", "custom_id": "94301"}, {"email": "xyzjoffrey@surveymonkey.com", "first_name": "Joffrey", "last_name": "Baratheon", "custom_id": "94401"}, {"email": "renly@surveymonkey"}]}, "email_message" : { "subject" : "Ice and Fire event" , "reply_email" : "xyzcersei@surveymonkey.com", "body_text" : "We are conducting a survey, and your response would be appreciated. Here is a link to the survey: [SurveyLink] This link is uniquely tied to this survey and your email address. Please do not forward this message. Thanks for your participation! Please note: If you do not wish to receive further emails from us, please click the link below, and you will be automatically removed from our mailing list. [RemoveLink]"}}'
```

Name | Description | Return Type
------ | ------- | -------
survey.template_id (Optional) | The template to use for creating the survey | String
survey.from_survey_id (Optional) | The existing survey to use for creating the survey | String
survey.survey_title (Required) | Title of the survey | String
survey.survey_nickname (Required) | Nickname (internal label) of survey | String
survey.language_id (Optional) | The language of the template | String
collector.type Conditionally Required) | Type of collector. Only 'email' is supported for now | String-ENUM
collector.name (Optional) | Name of collector. Defaults to 'New Email Invitation' | String
collector.thank_you_message (Optional) | Message for thank you page, which is enabled if set. | String
collector.redirect_url (Optional) | Redirect to this url upon survey completion. | String
collector.recipients[\_].email (Conditionally Required) | Email of the recipient | String
collector.recipients[\_].first_name (Optional) | First name of the recipient | String
collector.recipients[\_].last_name (Optional) | Last name the recipient | String
collector.recipients[\_].custom_id (Optional) | Custom id of the recipient | String
collector.send (Optional) | Sends email message to recipients if True. Defaults to False | Boolean
email_message.reply_email (Conditionally Required) | Reply email of the email message to be sent to recipients | String
email_message.subject (Conditionally Required) | Subject of the email message to be sent to recipients | String
email_message.body_text (Optional) | The plain text body of the email message to be sent to recipients. Default template is used if this is not specified | String
email_message.disable_footer (Optional) | Email messages have a Survey Monkey footer. Qualified accounts may pass True here to remove it.  Defaults to True for qualified accounts - otherwise, False. | Boolean

###Response Fields
>Example Response

```json
{
    "data": {
        "collector": {
            "collector_id": "23913464", 
            "date_created": "2013-09-11 22:09:00", 
            "date_modified": "2013-09-11 22:09:00", 
            "name": "email invite", 
            "open": true, 
            "type": "email"
        }, 
        "email_message": {
            "email_message_id": "7755535"
        }, 
        "recipients_report": {
            "bounced_emails": [], 
            "duplicate_emails": [], 
            "invalid_emails": [
                "renly@surveymonkey"
            ], 
            "optedout_emails": [], 
            "valid_emails_count": 2
        }, 
        "redirect_url": "https://www.surveymonkey.com/apis/surveys/create/interstitial?sm=mDtJLDZl8h9RaOyDrs12nfedhLCnjw4I2QUmxhcdZ6VgxTBAGAhEJ_2B2TLEhiY8zdgD6bMuwAI6Me_0Aj0nLQM5bwcWFc1P7hokqpWpEc5Ky1a5PqgE9NvPqmd_2BkV_2Baj6BcP3pdoHuaH5eecoIsvC_2BJ6JeQm_0AheAlhZsgrX3kOjG8DJ8_3D", 
        "survey": {
            "analysis_url": "https://www.surveymonkey.com/MySurvey_Responses.aspx?sm=gFGDHnnCzqw8rX48IXyrXvwDiy0nTmDC1rIdf6AS3Hg_3D", 
            "date_created": "2013-09-12 05:09:00", 
            "date_modified": "2013-09-12 05:09:00", 
            "language_id": 1, 
            "num_responses": 0, 
            "question_count": 14, 
            "survey_id": "100487745", 
            "title": "Ice and Fire Event"
        }
    }, 
    "status": 0
}
```

Name | Description | Return Type
------- | ------- | -------
status (Required) | Status code returned with every response | Integer
data.survey.survey_id (Required) | Survey Id | String
data.survey.date_created (Optional) | Date survey was created | Date String
data.survey.date_modified (Optional) | Date survey was last modified | Date String
data.survey.title (Optional) | Title of survey | String
data.survey.nickname (Optional) | Nickname (internal label) of survey | String
data.survey.language_id (Optional) | Language of survey | Integer-ENUM
data.survey.question_count (Optional) | Number of questions in the survey | Integer
data.survey.num_responses (Optional) | Number of responses for the survey | Integer
data.survey.analysis_url (Optional) | Url to analysis page | String
data.collector.collector_id (Required) | Collector Id | String
data.collector.date_created (Required) | Date collector was created | Date String
data.collector.date_modified (Required) | Date collector was last modified | Date String
data.collector.name (Optional) | Name of the collector | String
data.collector.open (Optional) | Whether collector is open to collect responses | Boolean
data.collector.type (Optional) | Collector type | String-ENUM
data.email_message.email_message_id (Optional) | Email message id | String
data.recipients_report.valid_emails_count (Optional) | The number of emails that were valid and added to the collector | Integer
data.recipients_report.invalid_emails\[\_] (Optional) | List of emails that are invalid | Array
data.recipients_report.duplicate_emails\[\_] (Optional) | List of emails that are duplicate | Array
data.recipients_report.bounced_emails\[\_] (Optional) | List of emails that are bounced | Array
data.recipients_report.optedout_emails\[\_] (Optional) | List of emails that are opted out | Array
data.recipients_report.recipients[\_].recipient_id (Optional) | Recipient ids for emails that were valid and added to the collector | Array
data.recipients_report.recipients[\_].email (Optional) | Emails that were valid and added to the collector | Array
data.redirect_url (Optional) | The url to redirect to for the next step in the create flow | String

##POST: get_survey_details
###Details
Retrieve a given survey's metadata.

###Notes
* Surveys with over 200 survey pages will not be returned
* Surveys with over 1000 questions will not be returned

###Endpoint
`https://api.surveymonkey.net/v2/surveys/get_survey_details?api_key=your_api_key`

###Request Data
>Example Request

```shell
curl -H 'Authorization:bearer XXXYYYZZZ' -H 'Content-Type: application/json' https://api.surveymonkey.net/v2/surveys/get_survey_details/?api_key=your_api_key --data-binary '{"survey_id":"100399456"}'
```

| Name | Description | Return Type
------- | ------- | -------
survey_id (Required) | Survey ID | String

###Response Fields
>Example Response

```json
{
  "data": {
    "date_created": "2013-02-01 23:55:00",
    "date_modified": "2013-05-14 17:19:00",
    "language_id": 1,
    "num_responses": 13,
    "pages": [
      {
        "heading": "",
        "page_id": "101119927",
        "questions": [
          {
            "answers": [],
            "heading": "Open answer question",
            "position": 1,
            "question_id": "316083320"
          },
          {
            "answers": [
              {
                "answer_id": "3024959615",
                "position": 1,
                "text": "Answer 1",
                "type": "row",
                "visible": true
              },
              {
                "answer_id": "3024959616",
                "position": 2,
                "text": "Answer 2",
                "type": "row",
                "visible": true
              },
              {
                "answer_id": "3024959613",
                "text": "Other (please specify)",
                "type": "other",
                "visible": true
              }
            ],
            "heading": "Multichoice",
            "position": 2,
            "question_id": "316083321"
          },
          {
            "answers": [
              {
                "answer_id": "3024962636",
                "position": 1,
                "text": "Row 1",
                "type": "row",
                "visible": true
              },
              {
                "answer_id": "3024962637",
                "position": 2,
                "text": "Row 2",
                "type": "row",
                "visible": true
              },
              {
                "answer_id": "3024962638",
                "position": 3,
                "text": "Row 3",
                "type": "row",
                "visible": true
              },
              {
                "answer_id": "3024962634",
                "text": "Other (please specify)",
                "type": "other",
                "visible": true
              },
              {
                "answer_id": "3024962639",
                "position": 1,
                "text": "Col 1",
                "type": "col",
                "visible": true
              },
              {
                "answer_id": "3024962640",
                "position": 2,
                "text": "Col 2",
                "type": "col",
                "visible": true
              },
              {
                "answer_id": "3024962641",
                "position": 3,
                "text": "Col 3",
                "type": "col",
                "visible": true
              }
            ],
            "heading": "Want to test some columns?",
            "position": 3,
            "question_id": "316084090"
          }
        ],
        "sub_heading": null
      },
      {
        "heading": "",
        "page_id": "101121158",
        "questions": [
          {
            "answers": [
              {
                "answer_id": "3024965122",
                "position": 1,
                "text": "How much did you like us?",
                "type": "row",
                "visible": true
              },
              {
                "answer_id": "3024965123",
                "position": 2,
                "text": "How much did you like our music?",
                "type": "row",
                "visible": true
              },
              {
                "answer_id": "3024965124",
                "position": 1,
                "text": "A little",
                "type": "col",
                "visible": true
              },
              {
                "answer_id": "3024965125",
                "position": 2,
                "text": "A lot",
                "type": "col",
                "visible": true
              },
              {
                "answer_id": "3024965126",
                "position": 3,
                "text": "SO MUCH",
                "type": "col",
                "visible": true
              }
            ],
            "heading": "Still not weighted.",
            "position": 1,
            "question_id": "316084761"
          }
        ],
        "sub_heading": ""
      },
      {
        "heading": "",
        "page_id": "101121155",
        "questions": [
          {
            "answers": [
              {
                "answer_id": "3024964761",
                "position": 1,
                "text": "No",
                "type": "row",
                "visible": true
              },
              {
                "answer_id": "3024964762",
                "position": 2,
                "text": "Not Really",
                "type": "row",
                "visible": true
              }
            ],
            "heading": "Is this a weighted question?",
            "position": 1,
            "question_id": "316084724"
          }
        ],
        "sub_heading": ""
      },
      {
        "heading": "",
        "page_id": "101121160",
        "questions": [
          {
            "answers": [
              {
                "answer_id": "3024965139",
                "position": 1,
                "text": "How much do you like us?",
                "type": "row",
                "visible": true
              },
              {
                "answer_id": "3024965140",
                "position": 2,
                "text": "How much do you use SurveyMonkey?",
                "type": "row",
                "visible": true
              },
              {
                "answer_id": "3024965141",
                "position": 3,
                "text": "How likely are you to recommend us again?",
                "type": "row",
                "visible": true
              },
              {
                "answer_id": "3024965137",
                "text": "Other (please specify)",
                "type": "other",
                "visible": true
              },
              {
                "answer_id": "3024965133",
                "position": 1,
                "text": "A little",
                "type": "col",
                "visible": true,
                "weight": 1
              },
              {
                "answer_id": "3024965134",
                "position": 2,
                "text": "A little more",
                "type": "col",
                "visible": true,
                "weight": 2
              },
              {
                "answer_id": "3024965135",
                "position": 3,
                "text": "A lot",
                "type": "col",
                "visible": true,
                "weight": 3
              },
              {
                "answer_id": "3024965136",
                "position": 4,
                "text": "A hell of a lot",
                "type": "col",
                "visible": true,
                "weight": 4
              }
            ],
            "heading": "OK, come on rated...",
            "position": 1,
            "question_id": "316084770"
          },
          {
            "answers": [
              {
                "answer_id": "3024965849",
                "position": 1,
                "text": "1",
                "type": "row",
                "visible": true
              },
              {
                "answer_id": "3024965850",
                "position": 2,
                "text": "2",
                "type": "row",
                "visible": true
              },
              {
                "answer_id": "3024965851",
                "position": 3,
                "text": "3",
                "type": "row",
                "visible": true
              },
              {
                "answer_id": "3024965852",
                "position": 4,
                "text": "4",
                "type": "row",
                "visible": true
              },
              {
                "answer_id": "3024965853",
                "position": 5,
                "text": "5",
                "type": "row",
                "visible": true
              },
              {
                "answer_id": "3024965854",
                "position": 1,
                "text": "1",
                "type": "col",
                "visible": true,
                "weight": 1
              },
              {
                "answer_id": "3024965855",
                "position": 2,
                "text": "2",
                "type": "col",
                "visible": true,
                "weight": 2
              },
              {
                "answer_id": "3024965856",
                "position": 3,
                "text": "3",
                "type": "col",
                "visible": true,
                "weight": 3
              },
              {
                "answer_id": "3024965857",
                "position": 4,
                "text": "4",
                "type": "col",
                "visible": true,
                "weight": 4
              },
              {
                "answer_id": "3024965858",
                "position": 5,
                "text": "5",
                "type": "col",
                "visible": true,
                "weight": 5
              }
            ],
            "heading": "Ok, really really really rated this time",
            "position": 2,
            "question_id": "316084970"
          }
        ],
        "sub_heading": ""
      },
      {
        "heading": "",
        "page_id": "101125402",
        "questions": [
          {
            "answers": [
              {
                "answer_id": "3024981690",
                "position": 1,
                "text": "Whack",
                "type": "row",
                "visible": true
              },
              {
                "answer_id": "3024981691",
                "position": 2,
                "text": "Really whack",
                "type": "row",
                "visible": true
              },
              {
                "answer_id": "3024981692",
                "position": 3,
                "text": "Super whack",
                "type": "row",
                "visible": true
              },
              {
                "answer_id": "3024981693",
                "position": 1,
                "text": "Menu 1",
                "type": "col",
                "visible": true
              },
              {
                "answer_id": "3024981696",
                "position": 2,
                "text": "Menu 2",
                "type": "col",
                "visible": true
              },
              {
                "answer_id": "3024981699",
                "position": 3,
                "text": "Menu 3",
                "type": "col",
                "visible": true
              }
            ],
            "heading": "Matrix of drop down menus",
            "position": 1,
            "question_id": "316089557"
          },
          {
            "answers": [
              {
                "answer_id": "3025263536",
                "position": 1,
                "text": "Statement One",
                "type": "row",
                "visible": true
              },
              {
                "answer_id": "3025263537",
                "position": 2,
                "text": "Statement Two",
                "type": "row",
                "visible": true
              },
              {
                "answer_id": "3025263538",
                "position": 3,
                "text": "Statement Three",
                "type": "row",
                "visible": true
              },
              {
                "answer_id": "3025263539",
                "position": 1,
                "text": "Strongly Disagree",
                "type": "col",
                "visible": true,
                "weight": 1
              },
              {
                "answer_id": "3025263540",
                "position": 2,
                "text": "Disagree",
                "type": "col",
                "visible": true,
                "weight": 2
              },
              {
                "answer_id": "3025263541",
                "position": 3,
                "text": "Agree",
                "type": "col",
                "visible": true,
                "weight": 3
              },
              {
                "answer_id": "3025263542",
                "position": 4,
                "text": "Strongly Agree",
                "type": "col",
                "visible": true,
                "weight": 4
              }
            ],
            "heading": "Evaluate the following statements.",
            "position": 2,
            "question_id": "316141731"
          },
          {
            "answers": [
              {
                "answer_id": "3025241151",
                "position": 1,
                "text": "Option Apple",
                "type": "row",
                "visible": true
              },
              {
                "answer_id": "3025241152",
                "position": 2,
                "text": "Option Banana",
                "type": "row",
                "visible": true
              },
              {
                "answer_id": "3025241153",
                "position": 3,
                "text": "Option Cherry",
                "type": "row",
                "visible": true
              }
            ],
            "heading": "Which do you like best?",
            "position": 3,
            "question_id": "316138019"
          },
          {
            "answers": [
              {
                "answer_id": "3025263533",
                "position": 1,
                "text": "Option Apple",
                "type": "row",
                "visible": true
              },
              {
                "answer_id": "3025263534",
                "position": 2,
                "text": "Option Banana",
                "type": "row",
                "visible": true
              },
              {
                "answer_id": "3025263535",
                "position": 3,
                "text": "Option Cherry",
                "type": "row",
                "visible": true
              }
            ],
            "heading": "Which do you like best?",
            "position": 4,
            "question_id": "316141730"
          },
          {
            "answers": [],
            "heading": "Untitled",
            "position": 5,
            "question_id": "316138020"
          },
          {
            "answers": [
              {
                "answer_id": "3025241157",
                "position": 1,
                "text": "Statement One",
                "type": "row",
                "visible": true
              },
              {
                "answer_id": "3025241158",
                "position": 2,
                "text": "Statement Two",
                "type": "row",
                "visible": true
              },
              {
                "answer_id": "3025241159",
                "position": 3,
                "text": "Statement Three",
                "type": "row",
                "visible": true
              },
              {
                "answer_id": "3025241154",
                "position": 1,
                "text": "1",
                "type": "col",
                "visible": true,
                "weight": 1
              },
              {
                "answer_id": "3025241155",
                "position": 2,
                "text": "2",
                "type": "col",
                "visible": true,
                "weight": 2
              },
              {
                "answer_id": "3025241156",
                "position": 3,
                "text": "3",
                "type": "col",
                "visible": true,
                "weight": 3
              }
            ],
            "heading": "Evaluate the following statements.",
            "position": 6,
            "question_id": "316138021"
          },
          {
            "answers": [
              {
                "answer_id": "3025263526",
                "position": 1,
                "text": "Statement One",
                "type": "row",
                "visible": true
              },
              {
                "answer_id": "3025263527",
                "position": 2,
                "text": "Statement Two",
                "type": "row",
                "visible": true
              },
              {
                "answer_id": "3025263528",
                "position": 3,
                "text": "Statement Three",
                "type": "row",
                "visible": true
              },
              {
                "answer_id": "3025263529",
                "position": 1,
                "text": "Strongly Disagree",
                "type": "col",
                "visible": true,
                "weight": 1
              },
              {
                "answer_id": "3025263530",
                "position": 2,
                "text": "Disagree",
                "type": "col",
                "visible": true,
                "weight": 2
              },
              {
                "answer_id": "3025263531",
                "position": 3,
                "text": "Agree",
                "type": "col",
                "visible": true,
                "weight": 3
              },
              {
                "answer_id": "3025263532",
                "position": 4,
                "text": "Strongly Agree",
                "type": "col",
                "visible": true,
                "weight": 4
              }
            ],
            "heading": "Evaluate the following statements.",
            "position": 7,
            "question_id": "316141729"
          }
        ],
        "sub_heading": ""
      }
    ],
    "question_count": 14,
    "survey_id": "100399456",
    "title": {
      "enabled": true,
      "text": "Example survey"
    }
  },
  "status": 0
}
```
 Name | Description | Return Type
------- | ------- | -------
status (Required) | Status code returned with every response | Integer
data.survey_id (Required) | Survey id | String
data.date_created (Required) | Date the survey was created | Date String
data.date_modified (Required) | Date the survey was last modified | Date String
data.custom_variable_count (Required) | Number of custom variables | Integer
data.custom_variables[\_].variable_label (Required) | The label describing the custom variable | String
data.custom_variables[\_].question_id (Required) | The question id which identifies the custom variable | String
data.language_id (Required) | Language the survey is in | ENUM - Integer
data.num_responses (Required) | Number of responses to survey | Integer
data.question_count (Required) | Number of questions in the survey | Integer
data.nickname (Required) | The nickname for the survey | String
data.title.enabled (Required) | If the title is enabled | Boolean
data.title.text (Required) | The contents of the survey title | String
data.pages[\_].page_id (Required) | The page id | String
data.pages[\_].heading (Required) | The page heading | String
data.pages[\_].sub_heading (Required) | The page sub heading | String
data.pages[\_].questions[\_].question_id (Required) | The question's id | String
data.pages[\_].questions[\_].heading (Required) | The question's heading | String
data.pages[\_].questions[\_].position (Required) | The question's position on the page | Integer
data.pages[\_].questions[\_].type.family (Required) | The question's family type | String
data.pages[\_].questions[\_].type.subtype (Required) | The question's subtype  | String
data.pages[\_].questions[\_].type.name (Required) | Used to designate a special case of that family/subtype | String
data.pages[\_].questions[\_].answers[\_].answer_id (Required) | The answer's id | String
data.pages[\_].questions[\_].answers[\_].position (Required) | The answer's position in the question | Integer
data.pages[\_].questions[\_].answers[\_].text (Required) | The answer text | String
data.pages[\_].questions[\_].answers[\_].type (Required) | The answer's type | String
data.pages[\_].questions[\_].answers[\_].visible (Required) | If the answer is visible | Boolean
data.pages[\_].questions[\_].answers[\_].weight (Required) | The answer's weight | Integer
data.pages[\_].questions[\_].answers[\_].apply_all_rows (Required) | Whether the 'other' option is seen under each row on a rating scale question | Boolean
data.pages[\_].questions[\_].answers[\_].is_answer (Required) | Signifies if the 'other' option is counted as an answer | Boolean
data.pages[\_].questions[\_].answers[\_].items[\_].answer_id (Optional) | The menu matrix's answer id | String
data.pages[\_].questions[\_].answers[\_].items[\_].position (Optional) | The menu matrix's position | Integer
data.pages[\_].questions[\_].answers[\_].items[\_].type (Optional) | The menu matrix's type | String
data.pages[\_].questions[\_].answers[\_].items[\_].text (Optional) | The menu matrix's text | String

##POST: get_user_details
###Details
Returns basic information about the logged-in user

###Endpoint
`https://api.surveymonkey.net/v2/user/get_user_details?api_key=your_api_key`

>Example Request

```shell
curl -H 'Authorization:bearer XXXYYYZZZ' -H 'Content-Type: application/json' https://api.surveymonkey.net/v2/user/get_user_details/?api_key=your_api_key
```
###Request Parameters
None

###Response Fields
>Example Response

```json
{
    "data": {
        "user_details": {
            "is_paid_account": true,
            "username": "bob",
            "is_enterprise_user": false
        }
    }, 
    "status": 0
}
```

Name | Description | Return Type
------- | ------- | -------
status (Required) | Status code returned with every response | Integer
data.is_paid_account (Required) | Returns if the user is a paid member and eligible for result exports | Boolean
data.is_enterprise_user (Required) | Returns if the account is a SurveyMonkey Enterprise account | Boolean
data.username (Required) | Returns the username of the current user | String
data.enterprise_details.group_id (Optional) | The group identifier if the user is a member of a group | string
data.enterprise_details.group_name (Optional) | The group name if the user is a member of a group | string
data.enterprise_details.total_seats_invoiced (Optional) | The number of seats purchased for this enterprise account | Integer
data.enterprise_details.total_seats_active (Optional) | The number of seats currently being used by this enterprise account | Integer
data.enterprise_details.salesforce_enabled (Optional) | Returns if the user has salesforce feature enabled | Boolean
data.enterprise_details.salesforce_pro_enabled (Optional) | Returns if the user has salesforce pro feature enabled | Boolean

##POST: get_survey_list
###Details
Retrieves a paged list of surveys in a user's account.

###Notes
* DateStrings must be in the format YYYY-MM-DD HH:MM:SS. All DateStrings are implicitly in UTC.
* All start dates are greater than or equal to the date passed in
* All end dates are strictly less than the date passed in

###Endpoints
`https://api.surveymonkey.net/v2/surveys/get_survey_list?api_key=your_api_key`

###Request Data
>Example Request

```shell
curl -H 'Authorization:bearer XXXYYYZZZ' -H 'Content-Type: application/json' https://api.surveymonkey.net/v2/surveys/get_survey_list/?api_key=your_api_key --data-binary '{"fields":["title","analysis_url","date_created","date_modified"], "start_date":"2013-02-02 00:00:00", "end_date":"2013-04-12 22:43:01", "order_asc":false, "title":"test3"}'
```

Name | Description | Return Type
------- | ------ | -------
page (Optional) | Page number. Defaults to 1 | Integer
page_size (Optional) | Number of surveys to return per page. Default is 1000. | Integer
start_date (Optional) | Surveys must be created after this date. | DateString
end_date (Optional) | Surveys must be created before this date. | DateString
title (Optional) | Nickname of survey to search against | String
recipient_email (Optional) | Surveys must be sent to this email | String
order_asc (Optional) | If 'true' sorts by creation time ASC, if 'false' sorts by creation time DESC. Defaults to 'false'. | Boolean
fields (Optional) | Additional fields to return. One or more of :[title, analysis_url, preview_url, date_created, date_modified, language_id, question_count, num_responses] note: 'title' returns the nickname of the survey | Array

###Response Fields
>Example Response

```json
{
    "data": {
        "page": 1,
        "page_size": 1000,
        "surveys": [
            {
                "analysis_url": "https://www.surveymonkey.com/MySurvey_Responses.aspx?sm=glvSt%2bEoPz3oA1ZfYo%2fTLbEyBTLoQry%2fCEohSIvk8y8%3d&",
                "date_created": "2013-04-12 22:42:00",
                "date_modified": "2013-04-12 22:42:00",
                "survey_id": "100422402",
                "title": "test3"
            }
        ]
    },
    "status": 0
}
```

Name | Description | Return Type
------- | ------ | -------
status (Required) | Status code returned with every response | Integer
data.page (Required) | Page id | Integer
data.page_size (Required) | Page size | Integer
data.surveys[_].survey_id (Required) | Survey Id | String
data.surveys[_].date_created (Optional) | Date survey was created | Date String
data.surveys[_].date_modified (Optional) | Date survey was last modified | Date String
data.surveys[_].title (Optional) | Nickname of survey | String
data.surveys[_].language_id (Optional) | Language of survey | ENUM - Integer
data.surveys[_].question_count (Optional) | Number of questions in the survey | Integer
data.surveys[_].num_responses (Optional) | Number of responses for the survey | Integer
data.surveys[_].analysis_url (Optional) | Url to analysis page | String
data.surveys[_].preview_url (Optional) | Url to preview page | String

##POST: send_flow
###Details
Create an email collector and email message attaching them to an existing survey.

###Notes
* Maximum number of recipients supported is 1000
* Basic users will have requests fail if they have more than 3 collectors, response will have the `upgrade_info` dictionary

###Endpoint
`https://api.surveymonkey.net/v2/batch/send_flow?api_key=your_api_key`

###Request Data
>Example Request

```shell
curl -H 'Authorization:bearer XXXYYYZZZ' -H 'Content-Type: application/json' https://api.surveymonkey.net/v2/batch/send_flow?api_key=your_api_key --data-binary '{"survey_id": "100487745", "collector": {"type": "email", "name" : "email invite", "recipients": [{"email": "xyzstannis@surveymonkey.com", "first_name": "Stannis", "last_name": "Baratheon", "custom_id": "94301"}, {"email": "xyzjoffrey@surveymonkey.com", "first_name": "Joffrey", "last_name": "Baratheon", "custom_id": "94401"}, {"email": "renly@surveymonkey"}]}, "email_message" : { "subject" : "Ice and Fire event" , "reply_email" : "xyzcersei@surveymonkey.com", "body_text" : "We are conducting a survey, and your response would be appreciated. Here is a link to the survey: [SurveyLink] This link is uniquely tied to this survey and your email address. Please do not forward this message. Thanks for your participation! Please note: If you do not wish to receive further emails from us, please click the link below, and you will be automatically removed from our mailing list. [RemoveLink]"}}'
```

Name | Description | Return Type
------ | ------- | -------
survey_id (Required) | The existing survey to use for creating the collector | String
collector.type (Conditionally Required) | Type of collector. Only 'email' is supported for now | String-ENUM
collector.name (Optional) | Name of collector. Defaults to 'New Email Invitation' | String
collector.thank_you_message (Optional) | Message for thank you page, which is enabled if set. | String
collector.redirect_url (Optional) | Redirect to this url upon survey completion. | String
collector.recipients[_].email (Conditionally Required) | Email of the recipient | String
collector.recipients[_].first_name (Optional) | First name of the recipient | String
collector.recipients[_].last_name (Optional) | Last name the recipient | String
collector.recipients[_].custom_id (Optional) | Custom id of the recipient | String
collector.send (Optional) | Sends email message to recipients if True. Defaults to False | Boolean
email_message.reply_email (Conditionally Required) | Reply email of the email message to be sent to recipients | String
email_message.subject (Conditionally Required) | Subject of the email message to be sent to recipients | String
email_message.body_text (Optional) | The plain text body of the email message to be sent to recipients. Default template is used if this is not specified | String
email_message.disable_footer (Optional) | Email messages have a Survey Monkey footer. Qualified accounts may pass True here to remove it.  Defaults to True for qualified accounts - otherwise, False. | Boolean


###Response Fields
>Example Response

```json
{
    "data": {
        "collector": {
            "collector_id": "23913464", 
            "date_created": "2013-09-11 22:09:00", 
            "date_modified": "2013-09-11 22:09:00", 
            "name": "email invite", 
            "open": true, 
            "type": "email"
        }, 
        "email_message": {
            "email_message_id": "7755535"
        }, 
        "recipients_report": {
            "bounced_emails": [], 
            "duplicate_emails": [], 
            "invalid_emails": [
                "renly@surveymonkey"
            ], 
            "optedout_emails": [], 
            "valid_emails_count": 2
        }, 
        "redirect_url": "https://www.surveymonkey.com/apis/surveys/create/interstitial?sm=mDtJLDZl8h9RaOyDrs12nfedhLCnjw4I2QUmxhcdZ6VgxTBAGAhEJ_2B2TLEhiY8zdgD6bMuwAI6Me_0Aj0nLQM5bwcWFc1P7hokqpWpEc5Ky1a5PqgE9NvPqmd_2BkV_2Baj6BcP3pdoHuaH5eecoIsvC_2BJ6JeQm_0AheAlhZsgrX3kOjG8DJ8_3D", 
        "survey": {
            "analysis_url": "https://www.surveymonkey.com/MySurvey_Responses.aspx?sm=gFGDHnnCzqw8rX48IXyrXvwDiy0nTmDC1rIdf6AS3Hg_3D", 
            "date_created": "2013-09-12 05:09:00", 
            "date_modified": "2013-09-12 05:09:00", 
            "language_id": 1, 
            "num_responses": 0, 
            "question_count": 14, 
            "survey_id": "100487745", 
            "title": "Ice and Fire Event"
        }
    }, 
    "status": 0
}
```

Name | Description | Return Type
------ | ------- | -------
status (Required) | Status code returned with every response | Integer
data.survey.survey_id (Required) | Survey Id | String
data.survey.date_created (Optional) | Date survey was created | Date String
data.survey.date_modified (Optional) | Date survey was last modified | Date String
data.survey.title (Optional) | Title of survey | String
data.survey.language_id (Optional) | Language of survey | Integer-ENUM
data.survey.question_count (Optional) | Number of questions in the survey | Integer
data.survey.num_responses (Optional) | Number of responses for the survey | Integer
data.survey.analysis_url (Optional) | Url to analysis page | String
data.collector.collector_id (Required) | Collector Id | String
data.collector.date_created (Required) | Date collector was created | Date String
data.collector.date_modified (Required) | Date collector was last modified | Date String
data.collector.name (Optional) | Name of the collector | String
data.collector.open (Optional) | Whether collector is open to collect responses | Boolean
data.collector.type (Optional) | Collector type | String-ENUM
data.email_message.email_message_id (Optional) | Email message id | String
data.recipients_report.valid_emails_count (Optional) | The number of emails that were valid and added to the collector | Integer
data.recipients_report.invalid_emails\[_] (Optional) | List of emails that are invalid | Array
data.recipients_report.duplicate_emails\[_] (Optional) | List of emails that are duplicate | Array
data.recipients_report.bounced_emails\[_] (Optional) | List of emails that are bounced | Array
data.recipients_report.optedout_emails\[_] (Optional) | List of emails that are opted out | Array
data.recipients_report.recipients[_].recipient_id (Optional) | Recipient ids for emails that were valid and added to the collector | Array
data.recipients_report.recipients[_].email (Optional) | Emails that were valid and added to the collector | Array
data.redirect_url (Optional) | The url to redirect to for the next step in the send flow | String
upgrade_info.restrictions[_].message (Optional) | Message present for any restrictions with the request | String
upgrade_info.restrictions[_].code (Optional) | Code for the restriction | String
upgrade_info.upgrade_url (Optional) | Upgrade url that can help a user remove the restriction | String

##POST: get_collector_list

###Details
Retrieves a paged list of collectors for a survey in a user's account

###Notes
* DateStrings must be in the format YYYY-MM-DD HH:MM:SS. All DateStrings are implicitly in UTC.
* All start dates are greater than or equal to the date passed in
* All end dates are strictly less than the date passed in

###Endpoint
`https://api.surveymonkey.net/v2/surveys/get_collector_list?api_key=your_api_key`

###Request Data
>Example Request

```shell
curl -H 'Authorization:bearer XXXYYYZZZ' -H 'Content-Type: application/json' https://api.surveymonkey.net/v2/surveys/get_collector_list/?api_key=your_api_key --data-binary '{"survey_id": "100399456", "fields":["collector_id", "url", "open", "type", "name", "date_created", "date_modified"], "start_date":"2013-02-05 00:00:00", "end_date":"2013-04-16 22:47:00", "order_asc":true, "page_size":1, "page":2}'
```

Name | Description | Return Type
------ | ------- | -------
survey_id (Required) | Survey Id | String
page (Optional) | Page number. Defaults to 1 | Integer
page_size (Optional) | Number of surveys to return per page. Defaults to 1000. | Integer
start_date (Optional) | Collectors must be created after this date | DateString
end_date (Optional) | Collectors must be created before this date | DateString
name (Optional) | Name of collector to search against | String
order_asc (Optional) | If 'true' sorts by creation time ASC, if 'false' sorts by creation time DESC. Defaults to 'false'. | Boolean
fields (Optional) | Additional fields to return.  One or more of :[url, open, type, name, date_created, date_modified] | Array
    

###Response Fields
>Example Response

```json
{
    "data": {
        "collectors": [
            {
                "collector_id": "23907889",
                "date_created": "2013-04-16 22:35:00",
                "date_modified": "2013-04-18 22:15:00",
                "name": "%%%% % % test",
                "open": true,
                "type": "url",
                "url": "https://www.surveymonkey.com/s/KLXZ3ZV"
            }
        ],
        "page": 2,
        "page_size": 1
    },
    "status": 0
}
```

Name | Description | Return Type
------ | ------- | -------
status (Required) | Status code returned with every response | Integer
data.page (Required) | Page id | Integer
data.page_size (Required) | Page size | Integer
data.collectors[_].collector_id (Required) | Collector Id | String
data.collectors[_].date_created (Optional) | Date collector was created | Date String
data.collectors[_].date_modified (Optional) | Date collector was last modified | Date String
data.collectors[_].name (Optional) | Name of the collector | String
data.collectors[_].open (Optional) | Whether collector is open to collect responses | Boolean
data.collectors[_].type (Optional) | Collector type | String-ENUM
data.collectors[_].url (Optional) | If collector is a Web Collector, then the url for the collector | String
data.collectors[_].thank_you_message (Optional) | Thank you message for the collector | String
data.collectors[_].redirect_url (Optional) | Redirect url for the collector | String

##POST: create_recipients

###Details
Adds a set of new recipients to an existing collector.

###Endpoint
`https://api.surveymonkey.net/v2/collectors/create_recipients?api_key=your_api_key`

###Request Data
>Example Request

```shell
curl -H 'Authorization:bearer XXXYYYZZZ' -H 'Content-Type: application/json' https://api.surveymonkey.net/v2/collectors/create_recipients?api_key=your_api_key --data-binary '{"collector_id": "246", "recipients": [{"email": "xyzstannis@surveymonkey.com", "first_name": "Stannis", "last_name": "Baratheon", "custom_id": "94301"}, {"email": "xyzjoffrey@surveymonkey.com", "first_name": "Joffrey", "last_name": "Baratheon", "custom_id": "94401"}, {"email": "renly@surveymonkey"}]}'
```

Name | Description | Return Type
------ | ------- | -------
collector_id (Required) | The ID of the collector to which you want to add recipients | String
send (Optional) | Sends email message to recipients if True. Defaults to False | Boolean
email_message_id (Conditionally Required) | The ID of the email message which is to be sent (required where send is set to True) | String
recipients[_].email (Conditionally Required) | Email of the recipient | String
recipients[_].first_name (Optional) | First name of the recipient | String
recipients[_].last_name (Optional) | Last name the recipient | String
recipients[_].custom_id (Optional) | Custom id of the recipient | String

###Response Fields
>Example Response

```json
```

Name | Description | Return Type
------- | ------- | -------
status (Required) | Status code returned with every response | Integer
data.collector.collector_id (Required) | Collector Id | String
data.collector.date_modified (Required) | Date collector was last modified | Date String
data.collector.open (Optional) | Whether collector is open to collect responses | Boolean
data.recipients_report.recipients[_].recipient_id (Required) | ID of recipient added to collector | String
data.recipients_report.recipients[_].email (Required) | Email of recipient added to collector | String
data.recipients_report.valid_emails_count (Required) | The number of emails that were valid and added to the collector | Integer
data.recipients_report.invalid_emails\[_] (Optional) | List of emails that are invalid | Array
data.recipients_report.duplicate_emails\[_] (Optional) | List of emails that are duplicate | Array
data.recipients_report.bounced_emails\[_] (Optional) | List of emails that are bounced | Array
data.recipients_report.optedout_emails\[_] (Optional) | List of emails that are opted out | Array
upgrade_info.restrictions[_].message (Optional) | Message present for any restrictions with the request | String
upgrade_info.restrictions[_].code (Optional) | Code for the restriction | Integer
upgrade_info.upgrade_url (Optional) | Upgrade url that can help a user remove the restriction | String




##POST: create_survey

###Details
Create a survey based on template or existing survey.

###Notes
* You cannot specify both template_id and from_survey_id

###Endpoint
`https://api.surveymonkey.net/v2/surveys/create_survey?api_key=your_api_key`

###Request Data
>Example Request

```shell
curl -H 'Authorization:bearer XXXYYYZZZ' -H 'Content-Type: application/json' https://api.surveymonkey.net/v2/surveys/create_survey?api_key=your_api_key --data-binary '{"template_id": "568", "survey_title": "Ice and Fire Event"}'
```

Name | Description | Return Type
------ | ------- | -------
template_id (Optional) | The template to use for creating the survey | String
from_survey_id (Optional) | The existing survey to use for creating the survey | String
survey_title (Required) | Title of the survey | String




###Response Fields
>Example Response

```json
{
    "data": {
        "analysis_url": "http://www.surveymonkey.com/MySurvey_Responses.aspx?sm=N2ZB5dQlKUGggfbp79n4_2Fi3J1R6PIla2Ug1_2FnNBKpco_3D", 
        "date_created": "2013-08-17 05:33:00", 
        "date_modified": "2013-08-17 05:33:00", 
        "language_id": 1, 
        "num_responses": 0, 
        "question_count": 14, 
        "redirect_url": "https://www.surveymonkey.com/apis/surveys/create/interstitial?sm=b1q_2BmKekcuOQyec4_2BtdwSjJ21Ot8yf_2BV6MRISFiV_2F1ydeZAmNJsHbzfFrzSNLF1ZhG87ZfYIXuAi_0AGJnO_2BmTkD7zBblD5_2FbD0q83iEWyRIIIU1SohINF_2F6c8d5aCxe7_2F6_2FpNt4ZQjZqLYftQtnPw061Rp_0A5TYdh_2B7tMLco26IRQXs_3D", 
        "survey_id": "43625459", 
        "title": "Ice and Fire Event"
    }, 
    "status": 0
}
```

Name | Description | Return Type
------ | ------- | -------
status (Required) | Status code returned with every response | Integer
data.survey_id (Required) | Survey Id | String
data.date_created (Optional) | Date survey was created | Date String
data.date_modified (Optional) | Date survey was last modified | Date String
data.title (Optional) | Title of survey | String
data.language_id (Optional) | Language of survey | Integer
data.question_count (Optional) | Number of questions in the survey | Integer
data.num_responses (Optional) | Number of responses for the survey | Integer
data.analysis_url (Optional) | Url to analysis page | String




##POST: create_email_message

###Details
Create an email message.

###Notes
* body_html overrides body_text in the email message sent to recipients

###Endpoint
`https://api.surveymonkey.net/v2/emails/create_email_message?api_key=your_api_key`


###Request Data
>Example Request

```shell
curl -H 'Authorization:bearer XXXYYYZZZ' -H 'Content-Type: application/json' https://api.surveymonkey.net/v2/surveys/create_email_message?api_key=hur88stdgyp9gvmmmbreybd7 --data-binary '{"collector_id":"123", "email_message":{ "subject" : "Ice and Fire event" , "reply_email" : "xyzcersei@surveymonkey.com", "body_text" : "We are conducting a survey, and your response would be appreciated. Here is a link to the survey: [SurveyLink] This link is uniquely tied to this survey and your email address. Please do not forward this message. Thanks for your participation! Please note: If you do not wish to receive further emails from us, please click the link below, and you will be automatically removed from our mailing list. [RemoveLink]"}}'
```

Name | Description | Return Type
------ | ------- | -------
collector_id (Required) | The collector id for which we are creating the email message | String
email_message.reply_email (Required) | Reply email of the email message to be sent to recipients | String
email_message.subject (Required) | Subject of the email message to be sent to recipients | String
email_message.body_text (Optional) | The plain text body of the email message to be sent to recipients. Default template is used if this is not specified | String

###Response Fields
>Example Response

```json
{
    "data": {
        "analysis_url": "http://www.surveymonkey.com/MySurvey_Responses.aspx?sm=N2ZB5dQlKUGggfbp79n4_2Fi3J1R6PIla2Ug1_2FnNBKpco_3D", 
        "date_created": "2013-08-17 05:33:00", 
        "date_modified": "2013-08-17 05:33:00", 
        "language_id": 1, 
        "num_responses": 0, 
        "question_count": 14, 
        "redirect_url": "https://www.surveymonkey.com/apis/surveys/create/interstitial?sm=b1q_2BmKekcuOQyec4_2BtdwSjJ21Ot8yf_2BV6MRISFiV_2F1ydeZAmNJsHbzfFrzSNLF1ZhG87ZfYIXuAi_0AGJnO_2BmTkD7zBblD5_2FbD0q83iEWyRIIIU1SohINF_2F6c8d5aCxe7_2F6_2FpNt4ZQjZqLYftQtnPw061Rp_0A5TYdh_2B7tMLco26IRQXs_3D", 
        "survey_id": "43625459", 
        "title": "Ice and Fire Event"
    }, 
    "status": 0
}
```

Name | Description | Return Type
------ | ------- | -------
status (Required) | Status code returned with every response | Integer
data.email_message_id (Optional) | Email message id | String




##POST: get_template_list

###Details
Retrieves a paged list of templates provided by Survey Monkey.

###Notes
* If templates are returned that the user cannot access, the `upgrade_info` dictionary will be returned

###Endpoint
`https://api.surveymonkey.net/v2/templates/get_template_list?api_key=your_api_key`

###Request Data
>Example Request

```shell
curl -H 'Authorization:bearer XXXYYYZZZ' -H 'Content-Type: application/json' https://api.surveymonkey.net/v2/templates/get_template_list?api_key=your_api_key --data-binary '{"page": 1, "page_size": 10, "language_id": 1, "category_id": "131", "fields" : ["title"]}'
```

Name | Description | Return Type
------ | ------- | -------
page (Optional) | Page number. Defaults to 1 | Integer
page_size (Optional) | Number of templates to return per page | Integer
language_id (Optional) | Language Id to filter templates with | Integer-ENUM
category_id (Optional) | Category Id to filter templates with | String
show_only_available_to_current_user (Optional) | Filter templates to only show templates available to current user if set to True | Boolean
fields (Optional) | Additional fields to return. One or more of :[language_id, title, short_description, long_description, is_available_to_current_user, is_featured, is_certified, page_count, question_count,  preview_url, category_id, category_name, category_description, date_modified, date_created] | Array

###Response Fields
>Example Response

```json
{
    "data": {
        "page": 1, 
        "page_size": 2, 
        "templates": [
            {
                "is_available_to_current_user": true, 
                "category_description": "Industry Specific", 
                "category_id": "131", 
                "category_name": "Industry Specific", 
                "is_certified": false, 
                "is_featured": false, 
                "language_id": 1, 
                "long_description": "Ask airline passengers about their most and least favorite airlines and find out where they purchase their tickets.", 
                "page_count": 1, 
                "preview_url": "http://www.surveymonkey.com/s.aspx?PREVIEW_MODE=DO_NOT_USE_THIS_LINK_FOR_COLLECTION&sm=xGFqaRtYyLLvPusLzk7RlzHaDeBzi8ZPBJreME16Cmo_3D", 
                "question_count": 10, 
                "short_description": "Ask airline passengers about their most and least favorite airlines and find out where they purchase their tickets.", 
                "template_id": "350", 
                "title": "Airline Passenger Feedback Template"
            }, 
            {
                "is_available_to_current_user": true, 
                "category_description": "Industry Specific", 
                "category_id": "131", 
                "category_name": "Industry Specific", 
                "is_certified": false, 
                "is_featured": false, 
                "language_id": 1, 
                "long_description": "Ask people about their experiences with digital apps.", 
                "page_count": 1, 
                "preview_url": "http://www.surveymonkey.com/s.aspx?PREVIEW_MODE=DO_NOT_USE_THIS_LINK_FOR_COLLECTION&sm=ngTg6Ba5qNR_2BAEDxN4abqBK32Eu5wPf2L9esqoXDGEo_3D", 
                "question_count": 6, 
                "short_description": "Ask people about their experiences with digital apps.", 
                "template_id": "426", 
                "title": "Apps Template"
            }
        ]
    }, 
    "status": 0
}
```

Name | Description | Return Type
------ | ------- | -------
status (Required) | Status code returned with every response | Integer
data.page (Required) | Page id | Integer
data.page_size (Required) | Page size | Integer
data.templates[_].template_id (Required) | Template Id | String
data.templates[_].language_id (Optional) | Language id of template | Integer-ENUM
data.templates[_].title (Optional) | Title of template | String
data.templates[_].short_description (Optional) | Short description of template | String
data.templates[_].long_description (Optional) | Long description of template | String
data.templates[\_].is_available_to_current_user (Optional) | Indicates whether current user is allowed to use this template | Boolean
data.templates[_].is_featured (Optional) | Indicates whether this is a featured template | Boolean
data.templates[_].is_certified (Optional) | Indicates whether this template is certified by experts at Survey Monkey | Boolean
data.templates[_].page_count (Optional) | Number of pages in the template | Integer
data.templates[_].question_count (Optional) | Number of questions in the template | Integer
data.templates[_].preview_url (Optional) | Url to preview this template | String
data.templates[_].category_id (Optional) | Id of template category | String
data.templates[_].category_name (Optional) | Name of category | String
data.templates[_].category_description (Optional) | Description of category | String
data.templates[_].date_created (Optional) | Date template was created | Date String
data.templates[_].date_modified (Optional) | Date template was modified | Date String
upgrade_info.restrictions[_].message (Optional) | Message present for any restrictions with the request | String
upgrade_info.restrictions[_].code (Optional) | Code for the restriction | Integer
upgrade_info.upgrade_url (Optional) | Upgrade url that can help a user remove the restriction | String



##POST: create_collector

###Details
Create a weblink collector.

###Notes
* Basic users can create a maximum of 3 collectors per survey

###Endpoint
`https://api.surveymonkey.net/v2/collectors/create_collector?api_key=your_api_key`

###Request Data
>Example Request

```shell
curl -H 'Authorization:bearer XXXYYYZZZ' -H 'Content-Type: application/json' https://api.surveymonkey.net/v2/collectors/create_collector?api_key=your_api_key --data-binary '{"survey_id": "100399456", "collector":{"type": "weblink", "name": "My Collector"}}'
```

Name | Description | Return Type
------ | ------- | -------
survey_id (Required) | The survey id for which we are creating the collector | String
collector.type (Required) | Type of collector. Only 'weblink' is supported for now | String-ENUM
collector.name (Optional) | Name of collector. Defaults to 'New Link' | String
collector.thank_you_message (Optional) | Message for thank you page, which is enabled if set. | String
collector.redirect_url (Optional) | Redirect to this url upon survey completion. | String




###Response Fields
>Example Response

```json
{
    "data": {
        "collector": {
            "collector_id": "1004023433", 
            "date_created": "2014-03-27 16:46:00", 
            "date_modified": "2014-03-27 16:46:00", 
            "name": "My Collector", 
            "open": true, 
            "type": "weblink", 
            "url": "https://www.surveymonkey.com/s/SHLXPHZ"
        }
    }, 
    "status": 0
}
```

Name | Description | Return Type
------ | ------- | -------
status (Required) | Status code returned with every response | Integer
data.collector.collector_id (Required) | Collector Id | String
data.collector.date_created (Required) | Date collector was created | Date String
data.collector.date_modified (Required) | Date collector was last modified | Date String
data.collector.name (Required) | Name of the collector | String
data.collector.open (Required) | Whether collector is open to collect responses | Boolean
data.collector.type (Required) | Collector type | String-ENUM
data.collector.url (Optional) | If collector is a Web Collector (type 'weblink') then the url for the collector | String




##POST: get_respondent_list

###Details
Retrieves a paged list of respondents for a given survey and optionally collector

###Notes
* Surveys with over 500,000 respondents will not be returned
* DateStrings must be in the format YYYY-MM-DD HH:MM:SS. All DateStrings are implicitly in UTC.
* All start dates are greater than or equal to the date passed in
* All end dates are strictly less than date passed in
* Basic users will only have the first 100 responses returned and will have the upgrade_info dictionary

###Endpoint
`https://api.surveymonkey.net/v2/surveys/get_respondent_list?api_key=your_api_key`

###Request Data
>Example Request

```shell
curl -H 'Authorization:bearer XXXYYYZZZ' -H 'Content-Type: application/json' https://api.surveymonkey.net/v2/surveys/get_respondent_list/?api_key=your_api_key --data-binary '{"survey_id": "100399456", "fields":["collector_id", "url", "open", "type", "name", "date_created", "date_modified"], "start_date":"2013-02-05 00:00:00", "end_date":"2013-04-16 22:47:00", "order_asc":true, "page_size":1, "page":2}'
```

Name | Description | Return Type
------ | ------- | -------
survey_id (Required) | Survey Id | String
collector_id (Optional) | Collector_id | String
page (Optional) | Page number. Defaults to 1 | Integer
page_size (Optional) | Number of surveys to return per page. Defaults to 1000, but basic users can only ever retrieve the first 100 respondents. | Integer
start_date (Optional) | Respondents must be created after this date | DateString
end_date (Optional) | Respondents must be created before this date | DateString
start_modified_date (Optional) | Respondents must be modified after this date | DateString
end_modified_date (Optional) | Respondents must be modified before this date | DateString
order_asc (Optional) | If 'true' sorts ASC, if 'false' sorts DESC. Defaults to 'false' (DESC). | Boolean
order_by (Optional) | Column to sort results by. Defaults to `respondent_id` | one of the following: [respondent_id, date_modified, date_start]
fields (Optional) | Additional fields to return.  One or more of :[date_start, date_modified, collector_id, collection_mode, custom_id, email, first_name, last_name, ip_address, status, analysis_url, recipient_id] | Array


###Response Fields
>Example Response

```json
{
    "data": {
        "page": 1,
        "page_size": 5,
        "respondents": [
            {
                "analysis_url": "https://www.surveymonkey.com/MySurvey_ResponsesDetail.aspx?sm=2k1FgYX4h3_2FspnRy_2BGzlCGRHeOdtLQ7yVncz8OZOXwJDxZCidfQEM7vhAWcYFyi725M6BQtRxzLm_0ANkz8bBPIOw_3D_3D",
                "custom_id": "",
                "date_modified": "2013-04-17 21:44:46",
                "date_start": "2013-04-17 21:44:37",
                "first_name": "",
                "respondent_id": "2500098680"
            }
        ]
    },
    "status": 0
}
```

Name | Description | Return Type
------ | ------- | -------
status (Required) | Status code returned with every response | Integer
data.page (Required) | Page id | Integer
data.page_size (Required) | Page size | Integer
data.respondents[_].respondent_id (Required) | Respondent Id | String
data.respondents[_].date_start (Optional) | Date the respondent started answering survey | Date String
data.respondents[_].date_modified (Optional) | Date respondent last modified their responses | Date String
data.respondents[_].collector_id (Optional) | Collector Id | String
data.respondents[_].collection_mode (Optional) | Way the respondent answered | String-ENUM
data.respondents[_].custom_id (Optional) | Custom id set by client | String
data.respondents[_].email (Optional) | Email address for respondent | String
data.respondents[_].first_name (Optional) | First name of respondent | String
data.respondents[_].last_name (Optional) | Last name of the respondent | String
data.respondents[_].ip_address (Optional) | IP address of respondent | String
data.respondents[_].status (Optional) | Status of respondent in survey | String-ENUM
data.respondents[_].analysis_url (Optional) | Url to analysis page | String
data.respondents[_].recipient_id (Optional) | ID generated for recipients of survey invitations via email collectors | String
upgrade_info.restrictions[_].message (Optional) | Message present for any restrictions with the request | String
upgrade_info.restrictions[_].code (Optional) | Code for the restriction | Integer
upgrade_info.upgrade_url (Optional) | Upgrade url that can help a user remove the restriction | String




##POST: get_responses

###Details
Takes a list of respondent ids and returns the responses that correlate to them.  To be used with 'get_survey_details'

###Notes
* Surveys with over 500,000 reponses are not available via the API currently
* Text responses returned are truncated after 32,768 characters
* Max number of respondents per call is 100

###Endpoint
`https://api.surveymonkey.net/v2/surveys/get_responses?api_key=your_api_key`


###Request Data
>Example Request

```shell
curl -H 'Authorization:bearer XXXYYYZZZ' -H 'Content-Type: application/json' https://api.surveymonkey.net/v2/surveys/get_responses/?api_key=your_api_key --data-binary '{"survey_id":"103994756", "respondent_ids": ["2503019027", "2500039028", "2500039029", "2503019064"]}'
```

Name | Description | Return Type
------ | ------- | -------
respondent_ids (Required) | Respondents to retrieve | Array
survey_id (Required) | Survey Id | String

###Response Fields
>Example Response

```json
{
    "data": [
        {
            "questions": [
                {
                    "answers": [
                        {
                            "col": "3024965133",
                            "row": "3024965139"
                        },
                        {
                            "col": "3024965134",
                            "row": "3024965140"
                        },
                        {
                            "col": "3024965135",
                            "row": "3024965141"
                        },
                        {
                            "row": "0",
                            "text": "Other!"
                        }
                    ],
                    "question_id": "316084770"
                },
                {
                    "answers": [
                        {
                            "col": "3024965125",
                            "row": "3024965122"
                        },
                        {
                            "col": "3024965124",
                            "row": "3024965123"
                        }
                    ],
                    "question_id": "316084761"
                },
                {
                    "answers": [
                        {
                            "row": "3024959616"
                        }
                    ],
                    "question_id": "316083321"
                },
                {
                    "answers": [
                        {
                            "row": "0",
                            "text": "This is an open answer"
                        }
                    ],
                    "question_id": "316083320"
                },
                {
                    "answers": [
                        {
                            "col": "3024962639",
                            "row": "3024962638"
                        },
                        {
                            "col": "3024962640",
                            "row": "3024962637"
                        },
                        {
                            "col": "3024962639",
                            "row": "3024962636"
                        }
                    ],
                    "question_id": "316084090"
                },
                {
                    "answers": [
                        {
                            "row": "3024964761",
                            "text": "9"
                        },
                        {
                            "row": "3024964762",
                            "text": "1"
                        }
                    ],
                    "question_id": "316084724"
                }
            ],
            "respondent_id": "2500019027"
        }
    ],
    "status": 0
}
```

Name | Description | Return Type
------ | ------- | -------
status (Required) | Status code returned with every response | Integer
data[_].respondent_id (Required) | Respondent id | String
data[\_].questions[\_].question_id (Required) | Id of the question | String
data[\_].questions[\_].answers[\_].row (Optional) | Id of a row type answer | String
data[\_].questions[\_].answers[\_].col (Optional) | Id of a col type answer | String
data[\_].questions[\_].answers[\_].col_choice (Optional) | Id of a col_choice answer | String
data[\_].questions[\_].answers[\_].text (Optional) | Id of a text type answer | String



##POST: get_response_counts

###Details
Returns how many respondents have started and/or completed the survey for the given collector.

###Endpoint
`https://api.surveymonkey.net/v2/surveys/get_response_counts?api_key=your_api_key`

###Request Data
>Example Request

```shell
curl -H 'Authorization:bearer XXXYYYZZZ' -H 'Content-Type: application/json' https://api.surveymonkey.net/v2/surveys/get_response_counts/?api_key=your_api_key --data-binary '{"collector_id": "23907195"}'
```

Name | Description | Return Type
------ | ------- | -------
collector_id (Required) | Id of the collector gathering responses | String

###Response Fields
>Example Response

```json
{
    "data": {
        "completed": 10,
        "started": 1
    },
    "status": 0
}
```

Name | Description | Return Type
------ | ------- | -------
status (Required) | Status code returned with every response | Integer
data.completed (Required) | Number of respondents that have completed the survey linked to by the collector | Integer
data.started (Required) | Number of respondents have started but not completed the survey linked to the collector | Integer