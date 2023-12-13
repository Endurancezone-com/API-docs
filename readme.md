# Endurance Zone API

## Overview of the API

The Endurance Zone API is intended for developers who want to write applications that interact with Endurance Zone. Below is a description of some of the basic concepts of the API and an overview of the different endpoints that the API supports.

## Accessing the API

The Endurance Zone API is available in both our staging and production environments:

| Environment       | URL													   |
| ----------        | -------------											   |  
| Staging           | https://api.staging.myezrewards.com/{version}/{endpoint} |
| Production        | https://api.myezrewards.com/{version}/{endpoint}         |

Where ```version``` is the version of the API that you wish to access (currently v1). Access to the API must be made using TLS1.2 (less secure versions of TLS are not supported). Calls made over plain HTTP will fail.

## Authorisation

In order to use the Endurance Zone API your application will need a valid API key. For security reasons different keys are required for different environments.

The API key should be used in all calls to the API by providing it in the ```ApiKey``` header.

Endpoints will return a ```401``` status code if the API Key provided is missing, invalid or expired.  

## Endpoints

### Create new member

A new member is created by POSTing to the following endpoint: 

```[POST] /members```

| Form field        | Description															|
| ----------        | -------------															|
| FirstName         | Member first name														|
| LastName          | Member last name                                                      |
| EmailAddress      | Member's email address												|
| PartnerMemberId   | (Optional) Your unique identifier for this member	                    |
| Country           | Acceptable values are: ```Canada```, ```US```, ```UK```				|
| Type              | ```New```																|
| MemberLevel       | Acceptable values are: ```Basic```, ```Premium```, ```Premium Plus``` |

All form fields are required.

A successful response looks like:
```
{
    "MemberUrl": "https://rewards.client.com/preload/client?iframe=ffe2SLrm4qrvyulsci",
    "ResultDetails": "Member created successfully",
    "Result": "200-OK"
}
```

### Renew member

Renewing a subscription will set the renewal date to one year from the current date and will set the member status to ‘Active’. Either an ```EmailAddress``` or ```PartnerMemberId``` must be specified to identify the member that should be renewed.

```[POST] /members```

| Form field        | Description															|
| ----------        | -------------															|
| EmailAddress      | Member's email address												|
| PartnerMemberId   | Your unique identifier for this member	                            |
| Type              | ```Renewal```															|

A successful response looks like: 

```
{
    "ResultDetails": "Member subscription renewed successfully",
    "Result": "200-OK"
}
```

### Manage member

Member details can be edited by submitting a request to the manage member endpoint including all required information. Either an ```EmailAddress``` or ```PartnerMemberId``` must be specified to identify the member that is being changed.

When a member manage request is received, if a member exists with a matching login, that record will be updated to reflect the submitted information. The fields that are updated are: 
- First Name 
- Last Name 
- MemberLevel
- Email address
  
Managing members would typically be used to upgrade or downgrade a Member level i.e. Basic -> Premium, Premium -> Premium Plus, Premium -> Basic etc… 

```[POST] /members```

| Form field      | Description															|
| ----------      | -------------														|
| EmailAddress    | Member's email address												|
| PartnerMemberId | Your unique identifier for this member	                |
| Type            | ```Manage```														|
| FirstName       | Member first name							    					|
| LastName        | Member last name                                                    |
| Country         | Acceptable values are: ```Canada```, ```US```, ```UK```				|
| MemberLevel     | Acceptable values are: ```Basic```, ```Premium```, ```Premium Plus``` |
| NewEmailAddress | New email address                                                   |

If you wish to change the email address of a member, the new email address should be added as an optional parameter titled ‘NewEmailAddress’. FirstName, LastName and MemberLevel can be changed directly in the submitted parameters.

### Cancel member

To cancel a member use the following endpoint and form fields. Either an ```EmailAddress``` or ```PartnerMemberId``` must be specified to identify the member that should be cancelled.

```[POST] /members```

| Form field        | Description															|
| ----------        | -------------															|
| EmailAddress      | Member's email address												|
| PartnerMemberId   | Your unique identifier for this member	                            |
| Type              | ```Cancel```															|

A successful response looks like: 

```
{
    "ResultDetails": "Member subscription cancelled successfully",
    "Result": "200-OK"
}
```

### Lookup member

Use the following endpoint and form fields to retrieve information related to a specific member. Either an ```EmailAddress``` or ```PartnerMemberId``` must be specified to identify the member that is being looked up.

```[POST] /members```

| Form field        | Description															|
| ----------        | -------------															|
| EmailAddress      | Member's email address												|
| PartnerMemberId   | Your unique identifier for this member	                            |
| Type              | ```Lookup```															|

A typical response would be:
```
{
    "RenewalDate": "2024-12-05",
    "API": "Members",
    "JoinDate": "2023-12-06",
    "ResultDetails": "Lookup Results",
    "Result": "200-OK",
    "Identifier": 8791480,
    "MemberURL": "https://rewards.client.com/preload/client?iframe=ffe2SLrm4qrvyulsci",
    "MemberLevel": "Basic",
    "LastName": "LastName",
    "Status": "Active",
    "FirstName": "FirstName",
    "EmailAddress": "newone@test.com"
}
```
