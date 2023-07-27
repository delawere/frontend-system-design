Let's assume that we have to implement design for Facebook. It's a frontend system design, therefore typically we don't expect server-side design on the interview. However, it still possible. The best way is to ask inteviewer, what type of design is expected. Usually, there are 4 possible answers:
- Product design
- Frontend Design
- Fronted + API Design
- Frontend + API + Backend Design

Product design - is mostly focused on product's features, rather than technical realization. We act a role of a Product Manager. For example, in our case, one of the Facebook features is a Feed. In product design, we shouldn't spend too much time for explanation _how exactly Feed works_. In this type of interview, we possibly should dedicate more attention to the users experience and core audience of the application.

Frontend Design - is the most classic type of the design. In that case, we have to design web-application. Server (and API) is a blackbox. We don't care how it works, we just receive data from _somewhere_. In that type of design, we should emphasize the most important features of the application and focused on their technical implementation. Also, we should describe how our application provides high performance, accessability and security. 

Frontend + API Design - case is very similar to the previous one, but we also have to describe how exactly we receive data from the server, e.g. describe _endpoints_. Usually, this type of interview includes protocol choosing. For most cases, REST would be a universal choice. We have to pay attention to the endpoins naming. Names should follow REST rules. For example: instead of `/users/get-user/:id`, we should use `/users/:id`. 

Frontend + API + Backend Design - the most difficult type of the interview in my opinion. In order to cover all topics within interview time, we have to define each part in broad strokes. I offer to focus on main parts of the application: high-level architecture, rendering, API, Application Server and Database Storages, without going deeper. In my opinion, it would be better to avoid computations on the server side (how much users we should handle per day, how much replicas do we need etc) and shift attention to the frontend side, if we have time. 

# Requirements 
_Functional requirements_ is a fetures of the application. For example, for the Facebook, Feed is a functional requirements, whereas performance is a non-functional requirement. The same for the backend-side: if we have to implement search engine, searching is functional requirement and availability of server is a non-functional requirements. 

Application may have one or more functional requirements. 
Application may have zero or more non-functional requirements.

_Out of scope_ is a special zone, where we can syncronize with the interviewer. If we propose requirements, we should mention that some parts will be out of scope, because their implementation takes too much time or/and trivial. The good example here is an authorization and authentication. Almost each application need these features. However, their implementation is highly depends on the bissiness needs and can take all interview's time. 
When we define all requirements, we have to discuss it with the interviewer. 

Let's define requirements for our Facebook's clone
## Functional requirements
- Feed
- Browsing posts
- Adding new posts
- Like/Unlike posts

## Non-functional requirements
- Performance
- Accessability
- Localization
- Security

## Out of scope
- Authentication

# High-Level Architecture

## Frontend Design
In this part we have to define main components of the system. In each type of design, we divide Web application into 3 main parts: Model, View and Controller. For the particular application, MVP or MVVM approach may be more suitable, but our goal is to define system in common terms. 
View is a UI. It consists of smaller UI components. In our case, View consists of Feed, PostDetails and AddNewPost components. 

Controller is a core of application. It's responsible for implmenting bussiness logic. Usually, it is responsible for two types of works: 
- Handle user events
- Prepare server response to the View

API module is responsible for sending requests. Technically, it's a part of controller, but for simplifying we display it as separated module. 

Model is a client storage. Typically, Redux or other storage may play this role. There are 4 types of storages:
- Client Storage. Storage keeps client-side data. It's a tricky point, because on most web applications Client Storage is used for storing Server Responses. There is possible, but not usual, to use Client Storage for keeping _all_ clients data. For example, instead of saving form values to the Components State, we use Client Storage. In that case, Client Storage is a Single Source of Truth. During the interview we avoid mention particullar framework and libraries, so we can say that Client Storage is used for client-side data only, without details.
- Responses Storage. Responses storage is a caching mechanism. It's responsible for storing server responses. If we need to request the same data in another place, we just get it from the cache. Classic approach is to use Stores (Redux, MobX) for this goal. However, it has disadvantage - we created implicit connections between components. For example, Component A and Component B need data from the server. Component A fetches it, and Component B consumes. This is a implicit connection between Component A and Component B. Modern approach (propogated by RTK Query, React Query, Apollo and other libraries) is to divide clients data and responses into two different stores.
- Cache API. Responsible for storing _assets_. Asset is a static file, like HTML, CSS, JS, Image etc. Cache API is a special type of storage that used by Service Workers. We discusied it later, in the PWA section.
- Persistent Storage. Technically, any type of storage may be persistent. Persistent storage is a storage that persist data through the long period of time. On the web, two possible options are _Local Storage_ or _IndexedDB_. I personally do not recommend to use Local Storage for storing data. It has a limited amount of space (5-10 Mb depends on the browser) and syncronious API (slow write/read). Moreover, LS is able to store only text data. IndexedDB is more difficult in development, but has no these disadvantages. We add persistent to the Client Storage by duplicating data to the IndexedBD. 

View and Controller are required parts. Whereas, Model is optional. For example, Widgets (small embeded UI compoments) may not have storage. 

For Facebook, high-level design of component may look like:
<img src="https://github.com/delawere/frontend-system-design/assets/35735722/f56a6ab2-8348-4136-998b-9421a88798bf" width="600">

With arrows we show the realtions between components. 
**View**:
  - **Feed**. Show recent (or most popular) posts for user. 
  - **Create new post**. Screen for creating new posts.
**Controller**. Responsible for preparing data to the view and handling user events (create new post, like/unlike posts etc).
**API**. API Module is responsible for sending requests to the server and handling responses.
**Client's Storage**. In client's storage we keep client's data. For example, when we receive posts, we firstly save it to the Client's storage.
**Persistent Storage**. Browser storage, such as Local Storage. We can duplicate some data to this storage, in order to have access to them, when user close tab or even browser. For example, we should save information about new post. User can write part of the post and come back to it later.
**Server**. Server handler client's requests.
 
## Backend Design


## High-Level Components
## UI
## Rendering approach. CSR vs SSR vs Hydration vs Static
## SPA vs MPA
## (Optional) Dataflow
## (Optional) BFF

# Data Model and API
## Data Model
## API

# Details
## Localization
## Performance
## Accessability
## Support wide range of devices
## Specefied Details
## (Optional) Offline mode and PWA
## (Optional) Testing
## (Optional) Security
