## ConnectEm -  Your one stop to simple Daily Meeting scheduler

Hey folks,

Over the weekend, I came across some amazing submissions from the community. This really pushed me to complete my open-source project before the deadline. I have collaborated with my college buddies - [Jasbindar Singh](https://www.linkedin.com/in/singhjs/) and [Ibtesam Ansari](https://www.linkedin.com/in/ibtesamansari/). We haven't worked together in a long time. So, this is sort of a work reunion.

![giphy-minions.gif](https://cdn.hashnode.com/res/hashnode/image/upload/v1625126814042/HeZqvy0Xa.gif)

### 💡 The Idea 

Let's start with the problem we aim to solve. While going through the Linkedin/Twitter feed, you must have come across posts on taking mentoring sessions. People want to share their wisdom and knowledge but they always struggle to respond to flooded DMs and countless comments. The traditional way of planning such sessions is tedious and needed a change. 

![giphy-tedious.gif](https://cdn.hashnode.com/res/hashnode/image/upload/v1625005508779/qW0vDuB0Y.gif)

What if there was a one-stop solution for scheduling meetings. People are trying to give back to the community and the least we can do is make the process seamless.

### 🚀 Introducing ConnectEm

[ConnectEm](https://connectem.netlify.app/) is a one stop to simple Daily Meeting scheduler. Here's a quick demo of the website as of today,

%[https://www.youtube.com/watch?v=xT6fBeKzMmU]


- [Live](https://connectem.netlify.app/)
- [Frontend Repository](https://github.com/piyushsinha24/ConnectEm-Frontend) / [Backend Repository](https://github.com/piyushsinha24/ConnectEm-Backend)
- [API Documentation](https://documenter.getpostman.com/view/8870498/TzeZFnL2#6bf13e9f-e92d-4a52-9836-00c232c1920a)

If a video demo isn't your thing, let me walk you through the flow:

- This is our landing page with options to `login`, `signup` and `learn more`:

![connectem-landing.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1625094784076/HWd0qSStC.png)

- Click on `learn more` to see how the site works:

![connectem-learnmore.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1625128507986/B17aScIf_.png)

- Click on `signup` to create your account:

![connectem-signup.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1625090382457/nXiMHGkO4.png)

- On successful `signup`, you'll be taken to your dashboard. This is where you'll find the list of `events` created. I added some events in advance to show how it looks:

![connectem-dashboard.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1625094452868/Fp-bUmVci.png)

- On clicking the `create` button on the dashboard, it will take you to the `create event` page. Enter the needed details as shown on the page. We support `multiple dates` & a `time slot` for the event. Also, remember to enter the `slots` available for each session:

![connectem-createEvent.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1625095177251/U3osU4BG9.png)

- The `host` will receive the `mail` after the event is created successfully:

![connectem-eventCreateMail.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1625127384547/OaNmploXn.png)

- The newly created event will be added to the dashboard. Let's take a look at the event card created. It shows some details of the event and two buttons. The left one is for toggling event state as `public/closed` and the right one copies the event `booking` link to the clipboard.

![connectem-eventCard.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1625100003794/SPPcP2q0m.png)

- Tap on the card to see the complete details:

![connectem-eventInfo.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1625100653214/rnR2Pe7zL.png)

- Let's visit the event `booking` link we talked about not long ago. This is the link you'll share on social media so that the interested ones can visit the link and `book` available `slots` on a `first come first serve basis`.

![connectem-eventBookingLink.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1625101161718/k8KxQ8PWj.png)

- Interested ones can book slots based on availability:

![connectem-bookSlots.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1625116568416/TeZTjg0oS.png)

- Both the `host` and the `attendees` will receive the confirmation mail on successful slot booking. The mail will consist of the `meet invite` for the slot booked and the option to add the event to your `calendar`:

![connectem-eventBookedHost.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1625127657830/ggDErCP7C.png)


![connectem-eventBookedAttendee.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1625127793363/S3FXeBPks.png)

### 🍔 Technology Stack

#### Frontend

- [React](https://reactjs.org/) - a JavaScript library for building user interfaces.
- [TailwindCSS](https://tailwindcss.com/) - a utility-first CSS framework for rapid UI development.
- [Netlify](https://www.netlify.com/) - an intuitive Git-based workflow and powerful serverless platform to build and deploy web apps.

#### Backend

- [Nest](https://nestjs.com/) - a progressive Node.js framework for building efficient, reliable and scalable server-side applications.
- [HarperDB](https://harperdb.io/) - a distributed database with NoSQL and SQL capabilities.
- [Heroku](https://www.heroku.com/) - a platform as a service (PaaS) to build, monitor and deploy web apps and APIs.

### 🛣️ The Journey

#### Planning

It was quite a project to build and needed an organized workflow. We made use of [Trello](https://trello.com/) to assign tasks and keep track of progress.

![Trello-board.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1625001693278/WjC5X6vUIK.png)

#### Designing

We needed a tool to brainstorm the design ideas and [Figma](https://www.figma.com/) served the purpose. It is collaborative by nature. 

![figma.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1625006197159/ejo4Ds01W.png)

#### Building

Let's start with the frontend:

- To simplify the process of getting started with `React`, we used `create-react-app` template. 
- Added `ESLint` and `Prettier` to follow the best coding practices and ensure readability.
- Setting up `TailwindCSS` and added `design` tokens.
- Created `hooks` and `services` for `authentication`, `events` and `bookings`.
- Built `components` and integrated them into `pages`.
- Added declarative routing with `react-router`.

Things we did in the backend:

- Setting up `HarperDB` cloud instance.
- Built `authentication` from scratch.
- APIs for `creating an event`, `fetching events` and `bookings`.
- Sending `mails` to host on event creation and bookings.
- Attendees also receive a confirmation `mail` on booking success.

#### Deploying

- Frontend is deployed on `Netlify`.
- Backend is deployed on `Heroku`.

### ✨ Upcoming Features

- Feed to search public meetings.
- Auto-generate Google meet links while creating events.

### 🤝 Contributing

Whenever you find a bug, create an `issue` and tag relevant people. 

When developing a `feature`:

- Always branch out from the `develop` branch.
- Branch name convention- `feature/whatever-you-building-name`.
- Test your feature on your `feature branch` and then create PR to `develop` with screenshot, if applicable.
- After PRs are accumulated on develop branch, we will use release versions and release to `master`.

That's all for this submission blog. All the best to everyone!

<a target="_blank" href="https://hashnode.com/n/harperdbhackathon">#HarperDBHackathon</a>


