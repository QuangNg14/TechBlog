---
title: Post#4 Creating Doctorally - Online Supporting Platform
catalog: true
date: 2020-10-18 08:53:06
subtitle: 20-min read
header-img: "Hospital.png"
tags: [javascript, reactjs, nodejs, api]
readtime: 20-min read
---

## It has been a long time since I last wrote on my blog. So I want to add some usefel posts in my break time

During the Covid-19 pandemic, me and my friends have created a online platform to connect **healthcare workers** to **volunteers** with their basic needs called Doctorally. Here I am going to explain quickly how we did it.

Again this is a **MERN** stack. I have explained it here in [this post](https://decodecraft.com/CreateBlog/). I am going to divide the post into 2 parts the **frontend** and the **backend**

### FRONTEND

For the frontend the main tools we used was: 

1. Material UI Framework. You can find the details [here](https://material-ui.com/)
2. React-router-dom and react-router. 
3. React hooks: useContext and useEffect
4. Authentication
5. Custom authentication hook.
6. API calling from Mapbox, Openstreetmap and custom API. 

### 1. Material UI Framework

This framework gives us a very beautiful layout. We can handle or store the data we get from the server by using **Containers** or **Tables** of Material UI. 

We can get all the layouts we want Button, Box, Container, Grid. For example this is a **Table** the that handles that data from an API.

{% asset_img material.PNG %}

This is a basic set up of the Tables that we are using to list out all the data: 
```javascript
<TableContainer className={classes.container}>
  <Table stickyHeader aria-label="sticky table">
    <TableHead>
      <TableRow>
        <StyledTableCell align='center'>
          <div
            style={{
            fontWeight: 'bold',
            fontFamily: 'Faustina'
            }}>{getLongLineText(locale.lang, "covid19_data", "heads", "country")}
          </div>
```

Remember to **import** all the necessaries depencies the most commonly used in Material UI is '@material-ui/core'.


### 2. React-router-dom and react-router

Since our website is a single page website, using react router can help us quickly change between the pages and pass data to different places through **props** or **query** or **params**

```javascript
  <Route exact path="/" component={HomePage} />
  <Route exact path="/about" component={About} />
  <Route exact path="/volunteer" component={Volunteer} />
  <Route exact path="/volunteer/signup" component={VolunteerSignUp} />
  <Route exact path="/volunteer/signup/success" component={SuccessVolunteer} />
```

The **exact path** will be the link to our pages. For example, if our domain is (this)[https://doctorally-test.herokuapp.com/]. Then if we access https://doctorally-test.herokuapp.com/about, we will get the About page. 

The **Component** we passed in will be rendered if we access that path. So the About Component will be rendered if the About page is accessed. 

When we want to pass extra information or access a page that is **unique** to each person (that could be a page that we can only access after logged in), we will pass the **params** "/:id" into the path.

```javascript
  <Route exact path="/requests/:id" component={RequestResponse} />
```

This means that after the doctors create a request in the **RequestResponse** component, there will be a unique id generated. We can only access this request by having that id.

Remember to wrap the App in the main index.js file in BrowserRouter for the Router to work 
```javascript
  <BrowserRouter>
    <App />
  </BrowserRouter>
```

### 3. React hooks: useContext, useEffect, useHistory, useParams
Beside useState which will obviously be used a lot, we used some additional hooks as listed in the title. 

#### 3.1 useContext
This is a very efficient way to maintain a website in 2 languages and quickly switch between each other. Our website has 2 languages Vietnamese and English. We will store all the texts in the website by variables 

```javascript
const ENG = {
  header: {
    about: "About",
    volunteer: "Volunteer",
    request_help: "Request Help",
  },

  homepage: {
    volunteer: {
      title: "Volunteer",
      description_strong: "Small actions spread love. ",
    }
}
```

And we store all the Vietnamese versions of the words also in variables

```javascript
const VIE = {
  header: {
    about: "Về chúng mình",
    volunteer: "Tình nguyện",
    request_help: "Yêu cầu giúp đỡ",
  },

  homepage: {
    volunteer: {
      title: "Tình nguyện",
      description_strong: "Hành động nhỏ loan tỏa yêu thương. ",
    }
  }
```

In order to switch between the 2, we create a context and called it localeContext.
So this variable will have 2 values English or Vietnamese, if we want to change all the words to another language we can just change this variable. 

This will be the function that sets up the localeContext. The getText function we browse in all the texts we store upper in the objects **ENG** and **VIE** and get the appropriate words.

```javascript

const localeContext = React.createContext({ lang: "ENG", setLang: () => { } });

const getText = (loc, key, lang) => {
  if (lang === "VIE") {
    return VIE[loc][key];
  }
  return ENG[loc][key];
};

```

This is one application in the website where we get the text and display it inside a button

```javascript
const locale = useContext(localeContext);
 <Button>
  {getText("header", "supply_stores", locale.lang)}
</Button>
```
#### 3.2 useEffect

We use this every time we want to load the data automatically from the server whenever we get into the page. 

Assume that we want to perform GET method from the API "/helpRequest". This will get all the helprequest data from the Mongodb.

```javascript
  const [requestData, setRequestData] = useState([]);
  const [invalidate, setInvalidate] = useState(true);
  useEffect(() => {
    if (invalidate) {
      axios.get("/helpRequest")
        .then((res) => {
          // setRequestData(res);
          console.log(res)
          setInvalidate(false);
        })
        .catch((err) => {
          console.log(err);
        });
    }
  }, [invalidate]);
```

This function will be run everytime the state invalidate is changed. Invalidate is a state to cleanup the function, meaning that this will not be run continuously while we are still in the page but only run when we reload or switch to the page. 
After the we perform the **GET** request, we will store the **res** data into the requestData state and set the invalidate to false so that the function will not be called again.

#### 3.3 useHistory, useParams

useHistory is a React hook that helps us change the page really efficiently. Remember before we have our react-router set up. Here we can imagine the history is our website link and if we are at the main page and want to navigate to about page we can simply do 

```javascript
  const history = useHistory();
  history.push("/about")
```

So similar question to what we have before in react-router, if we want to switch to a page that is unique to each person, what will we do? 

Here we assume that we have the id of the page that we want to switch to is **id**. We can push it into the history through params 

```javascript
  const history = useHistory();
  const handleBoxClick = () => {
    history.push(`requests/${id}`);
  };
```

Now if we switch to the ${domain}/requests/${id} page, we can get the id that we just passed through the params by using the hook useParams
```javascript
  const { id } = useParams();
```

### 4. Authentication
This is quite a complicate process that involves that backend as well. First in the frontend we will set up our submit form using Formik (a more efficient form). 

This is the basic form of the username input 
```javascript
      <Form onSubmit={formik.handleSubmit}>
        <Form.Group>
          <Form.Label className="code">Username</Form.Label>
          <Form.Control
            type="text"
            name="username"
            value={formik.values.username}
            onChange={formik.handleChange}
            isInvalid={formik.errors.username}
          />
          <Form.Control.Feedback type="invalid">
            {formik.errors.username}
          </Form.Control.Feedback>
        </Form.Group>
      </Form>
```

This big Form will have an **handleSubmit** function that is embedded in the Formik to send all the data 

After submitting the form, we will call to the login api and after that we will receive a new user with a newly generated JWT token. We will the add this token to the localStorage in order to use the Remember Me function. When we log out, we will delete this jwt token from the localStorage.

```javascript
const formik = useFormik({
  ...
onSubmit: (values) => {
      fetchLogin(values.username, values.password, values.role)
        .then((authUser) => {
          if (values.rememberMe) {
            localStorage.setItem("jwt", authUser.token);
          }
          setAuthUser(authUser);
          history.push("/")
        })
        .catch(() => {
          setFailureModalVisible(true);
        });
    },
})

```

We will do a similar thing to the Register Method

### 5. Custom authentication hook.
This is a very useful hook to use when we want to only allow access to a page if the person has authenticated (logged in).

```javascript
const withAuth = (WrappedComponent) => (props) => {
  const { authUser, setAuthUser } = useContext(authCtx);
  useEffect(() => {
    if (!authUser) {
      const jwt = localStorage.getItem('jwt');
      if (jwt) {
        fetchProfile(jwt).then((user) => setAuthUser(user));
      }
    }
  }, [authUser, fetchProfile, setAuthUser]);

  if (profileApi.loading) {
    return <Loading />;
  }
  if (authUser !== null) {
    return <WrappedComponent {...props} />;
  }
  return <Auth {...props} />;
};
```

We will store the data of the authenticated user after logging in to a authUser context so that we can access this data everywhere in our app. We will get the jwt in the localStorage and find out the appropriate current user. Then we will pass all the props of the (WrappedComponent) into the withAuth component. So for example if we want that user can only access OfferHelp if they have logged in then we will wrap it up as the following: 
```javascript
export default withAuth(OfferHelp);
```

### 6. API calling from Mapbox, Openstreetmap and custom API. 

In our app, we use Openstreemap API to get the nearby stores from the healthcare worker's locations. So here is what we will do. We will get the input location of the healthcare workers. Then we will use Openstreetmap to convert it into longitude and latitude. After that, we will implement that longitude and latitude into the Mapbox API to get all the shops that are nearby the healthcare's workers locations (I can use Google Map API for this but it is quite costly) 

This is we get the longitude and latitude of the location 
```javascript
const fetchNearbyStoreDataLocation = async (location) => {
  const response = await fetch(`https://nominatim.openstreetmap.org/search?q=${location}&format=json&polygon_geojson=1&addressdetails=1`)
  const resJson = await response.json()
  return resJson
}
```

Then we will store the data we get from the **fetchNearbyStoreDataLocation** into a state called **locationCordinate**

```javascript
  const [locationCordinate, setLocationCordinate] = useState({
    lat: 0,
    lon: 0
  })
   fetchNearbyStoreDataLocation(newQuery)
      .then((res) => {
        setLocationCordinate({
          lat: res[0].lat,
          lon: res[0].lon
        })
```

After that, we will get the nearby stores from the location coordinates data using mapbox 
```javascript
const fetchNearbyStoreData = async (locationCordinate) => {
  var tileset = 'mapbox.mapbox-streets-v8'
  var radius = 2000; //meters
  var limit = 30; // The maximum amount of results to return
  var layers = 'poi_label'
  var query = 'https://api.mapbox.com/v4/' + tileset + '/tilequery/' + locationCordinate.lon + ',' + locationCordinate.lat + '.json?radius=' + radius + '&limit=' + limit + '&layers=' + layers + '&access_token=' + mapboxgl.accessToken

  const response = await fetch(query)
  const resJson = await response.json()
  return resJson
}
```

### BACKEND
The backend is mostly used to handle the API calls that we make in the frontend so we will have 3 main **API calls**

1. Authentication 
2. HelpRequests
3. Volunteer

We use these libraries and dependencies: express, mongoose, cors, body-parser, path, jsonwebtoken(jwt). For the database, we will use the online cloud Mongodb in order to deploy it later.

#### 1. Authentication
First we must create the API for both the login and the register request
```javascript
router.post('/register', (req, res) => {
  const { username, password, role } = req.body;
  register(username, password, role)
    .then((result) => {
      res.json({ success: true });
    })
    .catch((err) => {
      switch (err.message) {
        case ERROR.USERNAME_EXISTED:
          res.status(409).json({ success: false, err: ERROR.USERNAME_EXISTED });
          break;
        default:
          res.status(500).json({ success: false, err: ERROR.INTERNAL_ERROR });
          break;
      }
    });
});

router.post('/login', (req, res) => {
  const { username, password, role } = req.body;
  login(username, password, role)
    .then((user) => {
      const token = generateJWT(user);
      res.json({
        user: user,
        token: token,
      });
    })
    .catch((err) => {
      res.status(401).json({ success: false, err: err.message });
    });
});
```

We will receive the req.body directly from the form that we create in the Frontend by calling the API "/login" and "/register". In here we can see that there are 2 functions login and register. 

For example for the register function, we will 
  1. check if the user has already existed 
  2. create a newUser with 2 fields username and role(doctor or volunteer)
  3. generate an encripted password from the password user entered
  4. save the user to the database
```javascript
const register = async (username, password, role) => {
  const user = await User.findOne({ username });
  if (user) throw new Error(ERROR.USERNAME_EXISTED);
  const newUser = new User({
    username, role
  });
  newUser.generatePassword(password);
  return newUser.save();
};
```

#### 2. HelpRequests
So the helpRequest can only be accessed by users will healthcare workers or doctor's role. The doctors will make helpRequest and send it to the server. Once we have the data of the request, we will save all of them in their respective fields in the Mongoose Scheme and save to the Database

``` javascript
router.get("/", (req, res) => {
  HelpRequest.find((err, help) => {
    if (err) {
      console.log(err);
    } else {
      res.json(help);
    }
  });
});

router.post("/", (req, res) => {
  const data = req.body;
  const newData = Object.keys(data[0]).filter((key) => data[0][key] == true);
  const helpRequest = new HelpRequest({
    medicalSupplies: data[0].medicalSupplies,
    masks: data[0].masks,
    ...
  });
  helpRequest.save();
});
```

#### 3. Volunteer
The volunteer is a little bit more complex. There are 2 types of volunteers: volunteers without helping a specific doctor and with helping a specific doctor. For type 1, we can just filled out a form and send it to the server and the information will be saved to the database. However, for type 2, we will need the id of the doctor whose the volunteer is helping, and then push that to the server.
```javascript
router.get("/", (req, res) => {
  console.log(req.body);
  VolunteerRequest.find((err, help) => {
    if (err) {
      console.log(err);
    } else {
      res.json(help);
    }
  });
});

router.post("/", (req, res) => {
  const data = req.body;
  const newData = Object.keys(data[0]).filter((key)=> data[0][key] == true)
  const volunteerRequest = new VolunteerRequest({
    meals: data[0].meals,
    drinks: data[0].drinks,
    idDoctor: data[1].idDoctor,
    ...
  });
  volunteerRequest.save();
```

This is the end of this post, I hope you enjoy it and learn something new from the post. I would like to thank all my team members Hoang Lam, Tuan Hoang, Hoang Minh and Hong Minh for creating this website together. If you want to check out the website, here is the [link](https://doctorally-test.herokuapp.com/). In the next post, we will see how to deploy a project with a server using heroku.