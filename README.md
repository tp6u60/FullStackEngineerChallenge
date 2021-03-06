# Full Stack Developer Challenge

This is an interview challengs. Please feel free to fork. Pull Requests will be ignored.

## Update By CHEN, YI AN

### Quick Start
Please download nodejs first: https://nodejs.org/en/download/

```
# clone this respository

$ git clone https://github.com/tp6u60/FullStackEngineerChallenge.git

$ cd FullStackEngineerChallenge/reviews-system
$ bash quickstart.sh
```
or you can: 
```
#!/bin/bash

# make sure you have nodejs & npm
$ node -v
$ npm -v

# install yarn
$ npm install --global yarn

$ yarn install
$ yarn start
```

client is on port 8080 (http://localhost:8080)

server is on port 5000


### All sssumptions & mock data
- mock users (`mock{number}`): mock1 ~ mock4
- accont will be `mock{number}@paypay.com`
- password will be `mock{number}`
- mock1 ~ mock3 are employees
- mock4 is administrator

### Already impliment

- It can run
  - Change Language
  - Login / Logout
  - Modify Reviews in `employee page`
  - Save changes in `employee page`
- UI
  - login page
  - admin page
  - employee page
  - add employee dialog
  - modify employee dialog
  - modify reviews dialog
- API
  - GET all staff
  - GET staff by name
  - GET reviews by name
  - UPDATE reviews by name


### Developed With
 - VueJS
 - Typescript
 - vuex
 - vuetify
 - express
 - font-awesome
 - i18n
 - sql.js


## Requirements

Design a web application that allows employees to submit feedback toward each other's performance review.

_Partial solutions are acceptable._ It is not necessary to submit a complete solution that implements every requirement.

### Admin view

- Add/remove/update/view employees
- Add/update/view performance reviews
- Assign employees to participate in another employee's performance review

### Employee view

- List of performance reviews requiring feedback
- Submit feedback

## Challenge Scope

- High level description of design and technologies used
- Server side API (using a programming language and/or framework of your choice)
  - Implementation of at least 3 API calls
  - Most full stack web developers at PayPay currently use Java, Ruby on Rails, or Node.js on the server(with MySQL for the database), but feel free to use other tech if you prefer
- Web app
  - Implementation of 2-5 web pages using a modern web framework (e.g. React or Angular) that talks to server side
    - This should integrate with your API, but it's fine to use static responses for some of it
- Document all assumptions made
- Complete solutions aren't required, but what you do submit needs to run.

## How to complete this challenge

- Fork this repo in github
- Complete the design and code as defined to the best of your abilities
- Place notes in your code to help with clarity where appropriate. Make it readable enough to present to the PayPay interview team
- Complete your work in your own github repo and send the results to us and/or present them during your interview

## What are we looking for? What does this prove?

- Assumptions you make given limited requirements
- Technology and design choices
- Identify areas of your strengths
