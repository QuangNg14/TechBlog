---
title: Post#7 Messenger Clone - Realtime Messaging Platform with Firebase 
catalog: true
date: 2021-02-01 08:53:06
subtitle: 30-min read
header-img: "605f46e0efe323669bc2a5a5b7b3835b.png"
tags: [javascript, reactjs, nodejs, api]
readtime: 30-min read
---

## Welcome back to my tech blog. This time I will show you my work during the winter break: Messenger Clone 

During the winter break, as everybody knows, there were many crashes to big platforms such as Facebook and Messenger. Noticing this, I decided to create a **real-time messaging** platform for me and my friends (potentially become bigger in the future). I hope you will find this post interesting because it has a lot of cool techniques that I have just learned.

### List of important parts in the project:
1. Firebase Authentication system 
2. Friends relationship management (add, remove friends like on Facebook)
3. Firebase storage to store and upload images
4. Real-time messaging of individuals using Firestore and Redux 
5. Real-time messaging of groups using Firestore and Redux 

### Important Note:
One important thing to note here is that I am **always** using the information and the ids from Firestore not from the auto generated user of Firebase Authentication. This means that anytime the user logs in and inputs any information, it will be uploaded encriptedly to the Firestore and we will draw the information from the database down to our client.

### Setting Up Firebase in a React Project 
I have written a blog about how to set up firebase from start to finish [Here](https://decodecraft.com/TodoApp/). It is vital that you set up everything correctly in order for Firebase to work. In this project, I will use 3 services Firebase Firestore, Firebase Authentication, and Firebase Storage 

### 1. Authentication

#### 1.1 Login Form
Definitely one of the core parts of any project. In this project, I created a Private Router for authentication which will only allow authenticated users to enter the platform. 

We start by creating 2 pages with 2 forms: Login and Signup. We will use useRef to handle the value that the users input in both forms. 

```javascript

          <Form onSubmit={handleSubmit}>
            <Form.Group id="email">
              <Form.Label>Email</Form.Label>
              <Form.Control type="email" ref={emailRef} required></Form.Control>
            </Form.Group>

            <Form.Group id="password">
              <Form.Label>Password</Form.Label>
              <Form.Control type="password" ref={passwordRef} required></Form.Control>
            </Form.Group>

            <Button disabled={loading} className="w-100" type="submit">
              Log in
          </Button>
          </Form>

```

The form is structure quite standardly. I am using all the components in **react-bootstrap** (Form, Button, Modal). We have 2 form groups correspond to the email and the password. We then save the reference using **useRef** to store the input from user. Finally, there is a button to handle the submit event of the form.

In the **submit** event, we will execute the following actions. First we will run the login function with email and password. This action is loaded from a **useContext** which I will mention later. Then we will store the result of the successful **Authentication** call and save it to the Database and **LocalStorage** (this is used to remember the user if you want to have that feature). I am also using **useHistory** to navigate between the webpages

```javascript
try {
      setError("")
      setLoading(true)
      await login(emailRef.current.value, passwordRef.current.value)
      .then((data) => {
        const name = data.user.displayName.split(" ");
        const firstName = name[0];
        const lastName = name[1];

        const loggedInUser = {
            firstName,
            lastName,
            uid: data.user.uid,
            email: data.user.email
        }

        localStorage.setItem('user', JSON.stringify(loggedInUser));
      })

      history.push("/")
    }
    catch {
      setError("Failed to login. Please check your password or username and try again")
    }
```

This is an application of try catch function. Note that here the **login** will be imported from our useContext (an Authentication context) and will be described later in section 1.3 

#### 1.2 Signup Form

This is very similar to the Login Form. It has 5 form groups firstname, lastname, **email, password and confirmpassword** (you can add more but the last 3 is a must).

Here we will also use **useRef** to handle the input value from the users. The Signup Form is below: 

```javascript
<Form onSubmit={handleSubmit}>
          <Form.Group id="email">
            <Form.Label>Email</Form.Label>
            <Form.Control type="email" ref={emailRef} required></Form.Control>
          </Form.Group>

          <Form.Group id="password">
            <Form.Label>Password</Form.Label>
            <Form.Control type="password" ref={passwordRef} required></Form.Control>
          </Form.Group>

          <Form.Group id="password-confirm">
            <Form.Label>Password Confirmation</Form.Label>
            <Form.Control type="password" ref={passwordConfirmRef} required></Form.Control>
          </Form.Group>

          <Form.Group id="first-name">
            <Form.Label>First Name</Form.Label>
            <Form.Control type="first-name" ref={firstNameRef} required></Form.Control>
          </Form.Group>

          <Form.Group id="last-name">
            <Form.Label>Last Name</Form.Label>
            <Form.Control type="last-name" ref={lastNameRef} required></Form.Control>
          </Form.Group>
          <Button disabled={loading} className="w-100" type="submit">
            Sign Up
          </Button>
        </Form>
```

Again, very similar to the Login Form, we will have a submit button to handle the **submit event** of the form. Here is when it gets a bit **different**. 

We will first check if the **password** is equal to the **confirmpassword**. I think Bootstrap Forms requires 6 characters at the minimum but you can freely add more conditions. If they are equal, we would perform the try catch function just as in the Login Form:

```javascript
    try{
      setError("")
      setLoading(true)
      let result = await signup(emailRef.current.value, passwordRef.current.value)
      await result.user.updateProfile({
        displayName: firstNameRef.current.value + " " + lastNameRef.current.value
      })
      await db.collection("users").add({
        email: emailRef.current.value,
        firstName: firstNameRef.current.value,
        lastName: lastNameRef.current.value,
        uid: result.user.uid,
        createdAt: new Date(),
        isOnline: true,
        profileImage: "https://i.pinimg.com/originals/e0/7a/22/e07a22eafdb803f1f26bf60de2143f7b.png",
        friendList: [],
        pendingFriends: [],
      })
      const loggedInUser = {
        firstName: firstNameRef.current.value,
        lastName: lastNameRef.current.value,
        uid: result.user.uid,
        email: result.user.email
      }
      await localStorage.setItem("user", JSON.stringify(loggedInUser))
      history.push("/")
    }
    catch{
      setError("Failed to create an account. Password must be at least 6 characters or username already existed")
    }
```

We set the **result** variable to be the result of the successful call to the signup function of Firebase Authentication. Again, the **signup** function will be **imported** from the **useContext** that we will mention in section 1.3. Then I will update the **displayName** of the currentUser that is successfully signed up to Firebase (note that the displayName is a provided feature). However, as I have mentioned in my **Important Notes** I will uploaded the users' information to Firestore even if we already have the currentUser from Authentication. This will allows us to freely manage the encripted information and apply it to later stage of the project.

```javascript
      await db.collection("users").add({
        email: emailRef.current.value,
        firstName: firstNameRef.current.value,
        lastName: lastNameRef.current.value,
        uid: result.user.uid,
        createdAt: new Date(),
        isOnline: true,
        profileImage: "https://i.pinimg.com/originals/e0/7a/22/e07a22eafdb803f1f26bf60de2143f7b.png",
        friendList: [],
        pendingFriends: [],
      })
```

Here is the function I called to add to my Firestore database. It includes some fields such as **uid** (id given to us from Firebase Authentication), **createdAt** (exact time that the user signed up), isOnline (manage if the user is currently online or not), **profileImage** (to store profile image), **friendList** (to store our current friendlist), and **pendingFriends** (to store the users sent us a request but we have not respond). After pushing the data to Firestore, we are basically done for signup. Now we should switch to Login page and start building our **Dashboard**. Note that in my website I also built a **Update Profile** and a **Forgot Password** page but that is for later and not the main function of the project  


### 2. Friends relationship management (add, remove friends like on Facebook)

This will be our Dashboard page. It will help you show the personal information, manage your friends, and the list of all users currently using the app so that people can **add friend** and connect to each other. So the core function of this page is to handle **adding and removing** friends. Like I mentioned in the Signup section (1.2), we will use 2 arrays to handle friends **pendingFriends** (users sent us a request but we have not respond) and **friendList** (the people are in our friends' list) 

This is my dashboard page:

{% asset_img dashboard.png %}

It has a really simple design with a navigating bar (drawer) to the left side. Now, I will show you how to create the **relationship** between the users. 

First, I will use a useEffect to call to the Firestore database and list out all the users that are using the app. In order to do this, we create a **User** component that has 3 props:
**id** (the document id of the friend that I sent a friend request to in Firestore), **docId** (the document id of the current user in Firestore), and **user** (the data of the current usere in Firestore).

Everytime we want to pull real-time information from the database to reflect the changes, we will write a useEffect hook. Here I have **pendingFriends** (users sent us a request but we have not respond) and **sentFriendRequests** (users that we sent a friend request to). Remember this data is taken from the **user** collection in the database

```javascript
  useEffect(() => {
    db.collection("users").doc(docId).onSnapshot((doc) => {
      if (doc.data().pendingFriends) {
        setPendingFriends(doc.data().pendingFriends)
      }
    })
  }, []);

  useEffect(() => {
    db.collection("users").doc(docId).onSnapshot((doc) => {
      if (doc.data().sentFriendRequests) {
        setSentFriendRequests(doc.data().sentFriendRequests)
      }
    })
  }, []);
```

Second, we will have 4 operations that we want to execute: **sent** a friend request to someone, **accept** a friend request, **decline** a friend request, and **remove** someone from our friend list. The way you handle these actions is up to you but here is how I handle it 

#### 1. Sent a friend request to someone 
When we sent a friend request to someone, we want to add the document id of that user to our sentFriendRequest list and add the current user's document id to the pendingList of that user.
For example, A sent to B a request -> add B to **sentFriendRequest** list of A and add A to **pendingList** of B. Here is how to execute it. In Firebase, whenever we want to change a array type data, we can use the **FieldValue.arrayUnion** provided by Firebase

```javascript
    const handleAddFriend = async (id) => {
    setPending(true)
    await db.collection("users").doc(id).update({
      pendingFriends: firebase.firestore.FieldValue.arrayUnion(docId)
    })
    await db.collection("users").doc(docId).update({
      sentFriendRequests: firebase.firestore.FieldValue.arrayUnion(id)
    })
  }
```


#### 2. Accept a friend request from someone

When A accepts B friend request, we want to add A to B's **friendList**, add B to A's friendList, remove B from **pendingList** of A and remove A from **sendFriendRequest** list of B. Here is the code to perform this action

```javascript
    const handleAcceptFriend = async (id) => {
    await db.collection("users").doc(docId).update({
      friendList: firebase.firestore.FieldValue.arrayUnion(id)
    })

    await db.collection("users").doc(id).update({
      friendList: firebase.firestore.FieldValue.arrayUnion(docId)
    })

    await db.collection("users").doc(docId).update({
      pendingFriends: pendingFriends.filter((friendId) => friendId != id && friendId != docId)
    })

    await db.collection("users").doc(id).update({
      sentFriendRequests: sentFriendRequests.filter((friendId) => friendId != docId)
    })
```

#### 3. Decline a friend request of someone

When A declines the friend request from B, we basically just remove B from the pendingList of A and remove A from the sentFriendRequest of B

```javascript
  const handleDeclineFriend = async (id) => {
    await db.collection("users").doc(id).update({
      pendingFriends: pendingFriends.filter((friendId) => friendId != id && friendId != docId)
    })

    await db.collection("users").doc(docId).update({
      pendingFriends: pendingFriends.filter((friendId) => friendId != id && friendId != docId)
    })

    await db.collection("users").doc(id).update({
      sentFriendRequests: sentFriendRequests.filter((friendId) => friendId != id && friendId != docId)
    })

```

#### 4. Remove someone from our friendlist

When A removes B from A's friendlist, we want to remove A from B's friendList and remove B from A's friendList. 


```javascript
  const handleRemoveFriend = (e) => {
    e.preventDefault()
    // console.log(friendList)
    db.collection("users").doc(docId).update({
      friendList: friendList.filter((friend) => friend != friendId && friend != docId)
    })

    db.collection("users").doc(friendId).update({
      friendList: friendList.filter((friend) => friend != docId && friend != friendId)
    })
```

Right now, we are done with the friends' relationship operations. Now we will try to display the operations to the screen in the way we want. For example, our current user is **A**. Note that the *logic* will flow like this. First, next to every user (that is not our friend yet) in the List of Users will have a "**add friend**" button. After sending **B** a friend request, we will change that button to **Pending** and disable it. Then in B's screen, the buttons next to A will be **yes/no** (represent if B want to add friend A or not). If B choose yes, they will both be added to each other's **friendList** and remove from the each other list of users. If B choose no, then A will be removed from B's users list. Here is the ternary expression I used to display this logic in the Dashboard

```javascript
{
            sentFriendRequests && sentFriendRequests.includes(id) ?
              (
                <Button onClick={() => handleAddFriend(id)} disabled={true}>Pending</Button>
              )
              :
              (
                <div>
                  {pendingFriends && pendingFriends.includes(id) ?
                    (
                      <div style={{display: "flex", flexDirection:"row"}}>
                        <Button style={{marginRight: 20}} onClick={() => handleAcceptFriend(id)}>
                          <FontAwesomeIcon style={{fontSize: 16}} icon={faCheck}/>
                        </Button>
                        <Button onClick={() => handleDeclineFriend(id)}>
                          <FontAwesomeIcon style={{fontSize: 20}} icon={faTimes}/>
                        </Button>
                      </div>
                    )
                    :
                    (
                      <>
                        <Button onClick={() => handleAddFriend(id)} disabled={pending}>{pending ? "Pending" : (
                          <FontAwesomeIcon icon={faUserPlus}/>
                        )}</Button>
                      </>
                    )
                  }
                </div>
              )
          }
```

The rest of the dashboard is pretty simple. Here is where I list of the current users that are not my friend.

```javascript
    userList && userList.map((user) => {
            if (!friendList.includes(user.id)) {
              return (
                <User id={user.id} docId={docId} user={user.user} />
              )
            }
          })
```

{% asset_img allusers.png %}

And here is where I list my friends in the left side of the dashboard. 

```javascript
    {friendList && friendList.map((friendId) => {
      return (
        <Friend sentFriendRequests={sentFriendRequests} friendList={friendList} docId={docId} friendId={friendId} />
      )
    })}
```

### 3. Firebase storage to store and upload images

One of the most important features in the app is to handle **profile images**: upload or change them. Here we will use Firebase **Storage** to upload the image and then extract the **link** from Storage to store it to corresponding users' database

```javascript
  <input type="file" onChange={handleImageAsFile} />
  <button onClick={handleFirebaseUpload}>Upload</button>
```

We will display these 2 basic HTML on the screen. 1 is an input where we set the type to files. We will only accept image format here. I will capture the event **handleImageAsFile**

```javascript
  const handleImageAsFile = (e) => {
    const image = e.target.files[0]
    setImageAsFile(imageAsFile => image)
  }
```

Here is an example of the files in storage. Remember the path that leads to the images is:
"/images/{name of image}"

{% asset_img storage.png %}

We save our image to a state **imageAsFile**. This will be a **Image File** so we cannot access this yet. From here we have to extract the link to this file from Storage. We will first **upload** the image we selected to Firebase Storage and **access** the **path** that leads to the image using reference and the method **getDownloadURL** of Firebase.

```javascript
  const handleFirebaseUpload = (e) => {
    e.preventDefault()
    if (imageAsFile === '') {
      console.log(`not an image`)
    }
    const uploadTask = storage.ref(`/images/${imageAsFile.name}`).put(imageAsFile)
    uploadTask.on("state_changed", (snapShot) => {
    }, (err) => {
      console.log(err)
    }, () => {
      storage.ref('images').child(imageAsFile.name).getDownloadURL()
        .then(fireBaseUrl => {
          setImageAsUrl(prevObject => ({ ...prevObject, imgUrl: fireBaseUrl }))
          db.collection("users").doc(docId).update({
            profileImage: fireBaseUrl
          })
        })
    })
    setOpen(false)
  }
```

Here we will get our downloadURL of the image to be **firebaseURL**. After receiving this, we will save this to the **corresponding** user database of that user. Now all we have to do is take the URL and display it on the screen 

### 4. Real-time messaging of individuals using Firestore and Redux 

It has been quite a long set up right ... We are finally here, the most important and also the core feature of my project the real-time chat between individuals and groups. In this section 4, I will show you how to use Redux to do real-time messaging.

First we need to set up **Redux and Redux Thunk** (this is critical in order to perform asynchronous actions in Redux)

First in the main index.js outside, we must set up as the following:

```javascript
ReactDOM.render(
  <Provider store={store}>
    <BrowserRouter>
        <App />
    </BrowserRouter>
  </Provider>,
  document.getElementById('root')
);
```

Basically, the **store** variable will be from Redux and it will contain all of our states. We just need to **wrap** our App with the **Provider** and we are good to go.

Now we will set up the reducer where will we perform the messaging logic. Below is my reducer folder structure. We will not need the authReducer but just an index.js file and the userReducer

{% asset_img reduxstructure %}

The user.reducer.js will contain all the definitions for the actions (name, type and corresponding payload) that we will perform

```javascript
import { userConstants } from "../actions/constants"

const initState = {
  users: []
}

export default (state = initState, action) => {
  switch (action.type) {
    case `${userConstants.GET_REALTIME_USERS}_REQUEST`:
      break;
    case `${userConstants.GET_REALTIME_USERS}_SUCCESS`:
      state = {
        ...state,
        users: action.payload.users
      }
      break;

    case userConstants.GET_REALTIME_MESSAGES:
      state = {
        ...state,
        conversations: action.payload.conversations
      }
      break;
    case `${userConstants.GET_REALTIME_MESSAGES}_FAILURE`:
      state = {
        ...state,
        conversations: []
      }
      break;
    case userConstants.GET_REALTIME_MESSAGES_GROUP:
      // console.log("have sent message group")
      state = {
        ...state,
        conversationsGroup: action.payload.conversationsGroup
      }
      break;
    case `${userConstants.GET_REALTIME_MESSAGES_GROUP}_FAILURE`:
      state = {
        ...state,
        conversationsGroup: []
      }
      break;
  }
  return state;
} 
```

The most important actions here are GET_REALTIME_MESSAGES and GET_REALTIME_MESSAGES_GROUP. In these actions, we will update the **state** conversations (for individuals chat) and conversationsGroup (for group chat) with the **corresponding** newState we got after the user texts something. 

Next we will set up the most **important** file of the webchat, **user.action.js**. This is where all the magic happens ! 

The first **asynchronous** action we will perform is **createMessage**. This is fired when the user sends something to another person.

```javascript
export const createMessage = (messageObject) =>{
  return async (dispatch) => {
    db.collection("conversations").add({
      ...messageObject,
      isView: false,
      createdAt: new Date()
    })
    .then((data) => {
      // console.log(data)
      //success
    })
    .catch((err) => {
      console.log(err)
    })
  }
}
```

Remember using **Redux Thunk** will help us perform async functions like above. We have a parameter **messageObject**. This parameter will be taken from the client side after the user sent something. REMEMBER THIS PARAMETER.

After firing createMessage, we will instantly fire the next function below:

```javascript
export const getRealTimeConversations = (user) =>{
  return async (dispatch) => {
    db.collection("conversations")
    .where('user_uid_2', 'in', [user.uid_1, user.uid_2])
    // .where('user_uid_2', 'in', [user.uid_2, user.uid_1])
    .orderBy("createdAt", "asc")
    .onSnapshot((snapShot) => {
      const conversations = []
      //doc.data() -> vao 1 document
      snapShot.forEach((doc) => {
        //nếu như conversation của 2 người match, 1 người là ng gửi và ng kia nhận được 
        // thì mới push vào conversation
        if((doc.data().user_uid_1 === user.uid_1 && doc.data().user_uid_2 === user.uid_2)
        ||
        (doc.data().user_uid_1 === user.uid_2 && doc.data().user_uid_2 === user.uid_1)){
          conversations.push({id: doc.id, conver: doc.data()})
        }
        // console.log(conversations)
      })
      dispatch({
        type: userConstants.GET_REALTIME_MESSAGES,
        payload: { conversations }
      })
    })
  }
}
```

All the encrypted messages will be stored in the conversations collection in Firestore. Here everytime a person sends a message to another person, **we will retake all the data from that conversation in Firestore and add the new message**. This might seem to be time consuming but the next filtering step will improve our **performance** by a lot.

To query and filter in the Firebase, I use the **where** method. The user.uid_1 and user.uid_2 are the ids (generated by the Firebase Authentication) of the current user and the person you are messaging to. Whenever the **current user** sends a message, it will **reload** the *conversation* in the database and it will look for all conversation with the user_uid_2 that is contained in the array [user.uid_1 and user.uid_2]. This will ensure that if A sends a message to B, the first **filter** will get all the **conversations** that B is either the sender or the receiver. 

Then we sort the order by ascending because we want the lastest messages to be seen last. Finally, this condition will check if the conversation we are taking from the database is exactly the conversation between A and B 

```javascript
  if((doc.data().user_uid_1 === user.uid_1 && doc.data().user_uid_2 === user.uid_2)
    ||
    (doc.data().user_uid_1 === user.uid_2 && doc.data().user_uid_2 === user.uid_1)){
      conversations.push({id: doc.id, conver: doc.data()})
    }
```

After that we will **dispatch** the action with the payload of conversations so that our reducer file will receive the action. So we are finished with the user.action.js. We have finished setting up for the real-time chat. Now let's move to the HomePage Screen (which is the Chat screen)

  const initChat = (user) => {
    setCurrentChatId(user.id)
    setChatStarted(true)
    setChatGroup(false)
    setChatUser(`${user.data.firstName} ${user.data.lastName}`)
    dispatch(getRealTimeConversations({ uid_1: docId, uid_2: user.id }))
    // setUserUid(user.data.uid)
    // console.log(user)
  }

First, we have to set up the conversations column, which lists all of our friends and groups (just like in Messenger). Here is the set up for the column 

```javascript
    <div>
      {
        newCurrentFriendList && newCurrentFriendList.map((user) => {
          if(user.id != docId){
            return (
              <User chatGroup={chatGroup} currentChatId={currentChatId} id={user.id} key={user.data.uid} user={user} onClick={initChat} />
            )
          }
        })
      }
    </div>
```

We will map out all of the current user's friends, each in a component called **User**. 

{% asset_img sideconver.png %}

When we **click** into each user, we will have to initiate the conversations between 2 people (which means we need to use the **actions** we just wrote in Redux). 

```javascript
  const initChat = (user) => {
    setCurrentChatId(user.id)
    setChatStarted(true)
    setChatGroup(false)
    setChatUser(`${user.data.firstName} ${user.data.lastName}`)
    dispatch(getRealTimeConversations({ uid_1: docId, uid_2: user.id }))
  }
```

We get the **id** of the user that we just clicked into as **id**. Then we set the ChatUser as the name of the user we are chatting with. Finally, we dispatch the action **getRealTimeConversations** to load all the conversations with that person. 

Next, when we send a message to a user, we need to initiate the the **sendMessage** function to execute the action we did in user.action.js 

```javascript
  const sendMessage = async (e) => {
    e.preventDefault()
    const messageObject = {
      user_uid_1: docId,
      user_uid_2: currentChatId,
      message: message,
      haveReply: replyMessage ? true : false,
      replyMessage: replyMessage
    }
    console.log(messageObject)
    if (message) {
      dispatch(createMessage(messageObject))
      .then(() => {
        setMessage('')
      })
    }
    setReplyMessage("")
  }
```

Remember the **messageObject** that I told you to remember. Inside the messageObject will be all the information about the 2 users and that message: **message** (the text that the user sent), **haveReply** (Is the message replied or not), **replyMessage** (the replyMessage if this message has 1). Finally, we will dispatch the **createMessage** action from the user.action.js

Here is the most sophisticated part of the project, where I set up and display all the messages: 

```javascript
(chatStarted && !chatGroup) ?
user.conversations && user.conversations.map((conver) => {
  return (
    <div style={{ textAlign: conver.conver.user_uid_1 == docId ? 'right' : "left" }}>
      {
        conver.conver.user_uid_1 == docId ?
          (<div className="maindiv" style={{ display: "flex", flexDirection: "column", alignItems: "flex-end", zIndex: 9999 }}>
            {(open6 && (currentMessageEmoji && currentMessageEmoji == conver.id)) ? (
              <div className="emojiContainer">
                <div onClick={(e) => handleReaction(e, conver.id)} className="emoji-li">{emoji.getUnicode("heart")}</div>
                <div onClick={(e) => handleReaction(e, conver.id)} className="emoji-li">{emoji.getUnicode(emoji.names[11])}</div>
                <div onClick={(e) => handleReaction(e, conver.id)} className="emoji-li">{emoji.getUnicode(emoji.names[70])}</div>
                <div onClick={(e) => handleReaction(e, conver.id)} className="emoji-li">{emoji.getUnicode(emoji.names[73])}</div>
                <div onClick={(e) => handleReaction(e, conver.id)} className="emoji-li">{emoji.getUnicode(emoji.names[55])}</div>
                <div onClick={(e) => handleReaction(e, conver.id)} className="emoji-li">{emoji.getUnicode(emoji.names[116])}</div>
                <div onClick={(e) => handleReaction(e, conver.id)} className="emoji-li">{emoji.getUnicode(emoji.names[117])}</div>
              </div>
            ) : null}
            <div>
              <div className="maindiv" key={conver.id} className="messageContainerCurrent">
                <div className="hide" id={conver.id} onClick={(e) => handleShowEmojis(e, conver.id)}>{emoji.getUnicode("grinning")}</div>
                <FontAwesomeIcon className="hide" onClick={(e) => handleReplyMess(e, conver.id)} icon={faReply} />
                <div className={conver.conver.user_uid_1 == docId ? "messageStyle" : "messageStyleWhite"} >
                  {conver.conver.haveReply ? (
                    <div className="chatReplyMessage">
                      {conver.conver.replyMessage}
                    </div>
                  ) : null}
                  {conver.conver.message}
                </div>
              </div>
              <div className="emojiMessage" style={{ position: "absolute", right: "1%", marginTop: -16 }}>{conver.conver.emojiSingle}</div>
            </div>
          </div>
          )
          :

```

The above code is the display of the message from the sender. The message from the receiver will be the same but just  A **message** can be splited into **4 parts**:
1. Checking what user this is the sender or the receiver

```javascript
conver.conver.user_uid_1 == docId ?
```
This line of code helps us know that this is the sender of the message. 

2. Emoji container

Do you know the emojis we typically use in Messenger. Here we use a emoji unicode library to display the emojis

3. Showing the Reacted Emoji

Here is where we handle the event of a user reacts to a message. We need a container to display the emoji. For individuals chatting, we only need to add an **emojiSelected** field in each message document.

```javascript
  const handleReaction = async (e, id) => {
    e.preventDefault()
    setSelectedEmoji(e.currentTarget.textContent)
    const emojiSelected = e.currentTarget.textContent
    await db.collection("conversations").doc(id).update({
      emojiSingle: emojiSelected == selectedEmojiOnDatabase ? "" : emojiSelected
    })
    await db.collection("conversations").doc(id).onSnapshot((doc) => {
      if (doc) {
        setSelectedEmojiOnDatabase(doc.data().emojiSingle)
      }
    })
    setOpen6(false)
  }
```

4. Displaying the Reply portion

Of course, we need a reply function for each message. Here we basically add another field **replyMessage** to each message document in the database. 

```javascript
  const handleReplyMess = (e, id) => {
    // console.log("reply")
    e.preventDefault()
    db.collection("conversations").doc(id).get().then((doc) => {
      setReplyMessage(doc.data().message)
    })
  }
```

And thats it, we can now chat individually to each other. For the groups's function is really similar to the individual chat so I will let the [Github Link to Project](https://github.com/MRSNOO/Messenger-Clone-New) so that you can understand it better. Here is the link to the Website [MessengerClone](https://todoapp-aaff8.web.app/). I hope you can experience the website and give me some feedback.

### I hope you like this post. It is really long but it contains a lot of useful information about web technologies. I will see you guys in my next post.



