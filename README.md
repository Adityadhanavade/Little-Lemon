# Little-Lemon
Skip to content
Search or jump to…
Pull requests
Issues
Codespaces
Marketplace
Explore
 
@Adityadhanavade 
soheilnikroo
/
little-lemon
Public
Fork your own copy of soheilnikroo/little-lemon
Code
Issues
Pull requests
Actions
Projects
Security
Insights
compelete the littel lemon
 main
@soheilnikroo
soheilnikroo committed 1 hour ago 
1 parent 7195e89
commit 76346bd
Show file tree Hide file tree
Showing 50 changed files with 19,074 additions and 758 deletions.
Filter changed files
 18,544  
package-lock.json
Large diffs are not rendered by default.

  4  
package.json
@@ -1,5 +1,5 @@
{
  "name": "little-lemon",
  "name": "lemonapp",
  "version": "0.1.0",
  "private": true,
  "dependencies": {
@@ -8,6 +8,8 @@
    "@testing-library/user-event": "^13.5.0",
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "react-helmet": "^6.1.0",
    "react-router-dom": "^6.6.2",
    "react-scripts": "5.0.1",
    "web-vitals": "^2.1.4"
  },
  50  
public/index.html
@@ -1,21 +1,19 @@
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <link rel="icon" href="%PUBLIC_URL%/favicon.ico" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <meta name="theme-color" content="#000000" />
    <meta
      name="description"
      content="Web site created using create-react-app"
    />
    <link rel="apple-touch-icon" href="%PUBLIC_URL%/logo192.png" />
    <!--

<head>
  <meta charset="utf-8" />
  <!-- <link rel="icon" href="%PUBLIC_URL%/favicon.ico" /> -->
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <meta name="theme-color" content="#000000" />
  <meta name="description" content="Web site created using create-react-app" />
  <link rel="apple-touch-icon" href="%PUBLIC_URL%/logo192.png" />
  <!--
      manifest.json provides metadata used when your web app is installed on a
      user's mobile device or desktop. See https://developers.google.com/web/fundamentals/web-app-manifest/
    -->
    <link rel="manifest" href="%PUBLIC_URL%/manifest.json" />
    <!--
  <link rel="manifest" href="%PUBLIC_URL%/manifest.json" />
  <!--
      Notice the use of %PUBLIC_URL% in the tags above.
      It will be replaced with the URL of the `public` folder during the build.
      Only files inside the `public` folder can be referenced from the HTML.
@@ -24,12 +22,21 @@
      work correctly both with client-side routing and a non-root public URL.
      Learn how to configure a non-root public URL by running `npm run build`.
    -->
    <title>React App</title>
  </head>
  <body>
    <noscript>You need to enable JavaScript to run this app.</noscript>
    <div id="root"></div>
    <!--
  <title>Little Lemon Restaurant</title>
  <meta property="og:title" content="Little Lemon Restaurant" />
  <meta property="og:description"
    content="Little Lemon Restaurant Web App is the main web site where you can find best location in town." />

  <meta property="og:url" content="https://littlelemonrestaurant.netlify.app/" />
  <meta property="og:image" content="lemon.png" />
  <link rel="icon" href="lemon.png" />
  <link rel="apple-touch-icon" href="lemon.png" />
</head>

<body>
  <noscript>You need to enable JavaScript to run this app.</noscript>
  <div id="root"></div>
  <!--
      This HTML file is a template.
      If you open it directly in the browser, you will see an empty page.
@@ -39,5 +46,6 @@
      To begin the development, run `npm start` or `yarn start`.
      To create a production bundle, use `npm run build` or `yarn build`.
    -->
  </body>
</html>
</body>

</html>
 BIN +2.12 KB 
public/lemon.png
Unable to render rich display

 BIN -5.22 KB 
public/logo192.png
Binary file not shown.
 BIN -9.44 KB 
public/logo512.png
Binary file not shown.
  10  
public/manifest.json
@@ -6,16 +6,6 @@
      "src": "favicon.ico",
      "sizes": "64x64 32x32 24x24 16x16",
      "type": "image/x-icon"
    },
    {
      "src": "logo192.png",
      "type": "image/png",
      "sizes": "192x192"
    },
    {
      "src": "logo512.png",
      "type": "image/png",
      "sizes": "512x512"
    }
  ],
  "start_url": ".",
 588  
src/App.css
Large diffs are not rendered by default.

 34  
src/App.js
@@ -1,5 +1,37 @@
import './App.css';
import React from 'react';
import { BrowserRouter as Router, Route, Routes } from 'react-router-dom';
import Home from './components/Home';
import About from './components/About';
import Contact from './components/Contact';
import NotFound from './components/Notfound';
import Header from './components/Header';
import Footer from './components/Footer';
import Booking from './components/Booking';
import ConfirmedBooking from './components/ConfirmedBooking';

function App() {
  return <div>salam</div>;
  return (
    <>
      <div className="App">
        <Router>
          <Header />
          <div>
            <Routes>
              <Route exact path="/" element={<Home />} />
              <Route path="/About" element={<About />} />
              <Route path="/Contact" element={<Contact />} />
              <Route path="/Booking" element={<Booking />} />
              <Route path="/Reservations" element={<Booking />} />
              <Route path="/Confirmedbooking" element={<ConfirmedBooking />} />
              <Route path="*" element={<NotFound />} />
            </Routes>
          </div>
          <Footer />
        </Router>
      </div>
    </>
  );
}

export default App;
 38  
src/api/bookingAPI.js
@@ -0,0 +1,38 @@
// This file contains the functions to fetch and submit the booking data
// The functions are exported to be used in the Booking component

/* Function to generate a random number based on the date */
const seededRandom = function (seed) {
    var m = 2**35 - 31;
    var a = 185852;
    var s = seed % m;
    return function () {
        return (s = s * a % m) / m;
    };
}


/* Function to fetch available time slots for a given date */
const fetchAPI = function(date) {
    let result = [];
    let random = seededRandom(date.getDate());

    for(let i = 17; i <= 23; i++) {
        if(random() < 0.5) {
            result.push(i + ':00');
        }
        if(random() < 0.5) {
            result.push(i + ':30');
        }
    }
    return result;
};


// Function to submit the booking data
// This function is not implemented, it just returns true
const submitAPI = function(formData) {
    return true;
};

export { fetchAPI, submitAPI };
 3  
src/assets/Basket.svg
Unable to render rich display

 3  
src/assets/Dish icon.svg
Unable to render rich display

 9  
src/assets/Logo .svg
Unable to render rich display

 9  
src/assets/Logo.svg
Unable to render rich display

 BIN +19.2 MB 
src/assets/Mario and Adrian A.jpg
Unable to render rich display

 BIN +19.1 MB 
src/assets/Mario and Adrian b.jpg
Unable to render rich display

 3  
src/assets/Recent.svg
Unable to render rich display

 31  
src/assets/Ui kit.svg
Unable to render rich display

 3  
src/assets/basket .svg
Unable to render rich display

 9  
src/assets/bruchetta.svg
Unable to render rich display

 9  
src/assets/creditcard.svg
Unable to render rich display

 BIN +9.27 MB 
src/assets/greek salad.jpg
Unable to render rich display

 3  
src/assets/home icon.svg
Unable to render rich display

 BIN +48 KB 
src/assets/john.png
Unable to render rich display

 BIN +8.97 KB 
src/assets/lemon dessert.jpg
Unable to render rich display

 BIN +59.9 KB 
src/assets/mary.png
Unable to render rich display

 BIN +8.65 MB 
src/assets/restauranfood.jpg
Unable to render rich display

 BIN +10.4 MB 
src/assets/restaurant chef B.jpg
Unable to render rich display

 BIN +320 KB 
src/assets/restaurant.jpg
Unable to render rich display

 BIN +298 KB 
src/assets/tom.png
Unable to render rich display

 3  
src/assets/🦆 icon _eye_.svg
Unable to render rich display

 3  
src/assets/🦆 icon _hamburger menu.svg
Unable to render rich display

 3  
src/assets/🦆 icon _hamburger menu_.svg
Unable to render rich display

 33  
src/components/About.jsx
@@ -0,0 +1,33 @@
/* About component */

import React from "react";

import Restaurant from "../assets/restaurant.jpg";
import Chefs from "../assets/restaurant chef B.jpg";

const About = () => {
  return (
    <section id="about">
      <div className="container">
        <div className="content">
          <h2>About</h2>
          <p>
            Welcome to Little Lemon! We are a family-owned and operated
            restaurant that has been serving delicious food for over 10 years.
            Our menu features a variety of Mediterranean dishes made with the
            freshest ingredients and cooked to perfection. We also offer a
            selection of beverages.Our passion for good food and friendly
            service is what sets us apart. We look forward to serving you!
            Thank you for choosing Little Lemon Restaurant and we hope to see you soon!
          </p>
        </div>
        <div className="image-container">
          <img src={Chefs} alt="Little Lemon Restaurant" />
          <img src={Restaurant} alt="Little Lemon Restaurant Chefs" />
        </div>
      </div>
    </section>
  );
};

export default About;
 14  
src/components/BookTable.jsx
@@ -0,0 +1,14 @@
import React from 'react'
import BookingForm from './BookingForm'

const BookTable = ({ availableTimes, setAvailableTimes, submitForm }) => {
  return (
    <section id='booktable'>
      <div className="container">
        <BookingForm availableTimes={availableTimes} setAvailableTimes={setAvailableTimes} submitForm={submitForm} />
      </div>
    </section>
  )
}

export default BookTable
 42  
src/components/Booking.jsx
@@ -0,0 +1,42 @@
import React from "react";
import BookTable from "./BookTable";
import { useReducer } from "react";
import { fetchAPI, submitAPI } from "../api/bookingAPI";

const Booking = () => {

  function initializeTimes() {
    const times = {
      times: [...fetchAPI(new Date())],
    };
    return times;
  }

  function reducer(state, action) {
    const newBookingDate = action.setBookingDate;
    const newTimes = fetchAPI(newBookingDate);
    return { times: [...newTimes] };
  }

  function submitForm(formData) {
    const success = submitAPI(formData);
    if (success) {
      window.location.href = '/confirmedbooking';
    }
  }

  const initialState = initializeTimes();
  const [availableTimes, setAvailableTimes] = useReducer(reducer, initialState);

  return (
    <div>
      <BookTable
        availableTimes={availableTimes}
        setAvailableTimes={setAvailableTimes}
        submitForm={submitForm}
      />
    </div>
  );
};

export default Booking;
 108  
src/components/BookingForm.jsx
@@ -0,0 +1,108 @@
import React, { useState } from 'react'

const BookingForm = ({ availableTimes, setAvailableTimes, submitForm }) => {

    // Set the initial date to tomorrow
    const initDate = new Date();
    initDate.setDate(initDate.getDate() + 1);

    // Format the date to YYYY-MM-DD
    const YYYY = initDate.getFullYear();
    const MM = initDate.getMonth() < 10 ? "0" + (initDate.getMonth() + 1) : initDate.getMonth() + 1;
    const DD = initDate.getDate() < 10 ? "0" + initDate.getDate() : initDate.getDate();

    // Set the initial state
    const [date, setDate] = useState(YYYY + "-" + MM + "-" + DD);

    // Set the initial time to 17:00
    const [time, setTime] = useState("17:00");

    // Set the initial number of guests to 2
    const [guests, setGuests] = useState(2);

    // Set the initial occassion to none
    const [occassion, setOccassion] = useState("none");

    // The function isValidDate() checks if the date is valid
    function isValidDate(dateString) {
        const yyyymmdd = dateString.split("-");
        const dateObj = new Date(parseInt(yyyymmdd[0]), parseInt(yyyymmdd[1]) - 1, parseInt(yyyymmdd[2]));
        if (dateObj <= new Date())
            return false;
        return true;
    }

    // The function getDateObject() returns a date object
    function getDateObject (dateString) {
        const yyyymmdd = dateString.split("-");
        const dateObj = new Date(parseInt(yyyymmdd[0]), parseInt(yyyymmdd[1]) - 1, parseInt(yyyymmdd[2]));
        return dateObj;
    }


    // The function handleDateChange() checks if the date is valid and updates the available times
    function handleDateChange(e) {
        if (!isValidDate(e.target.value)) {
            alert(`Sorry! Reservations not available for this date!`);
            return;
        }
        const dateObject = getDateObject(e.target.value);
        setDate(e.target.value);
        setAvailableTimes({ setBookingDate: dateObject });
    }


    // The function handleSubmit() submits the form
    // the values of the form are stored in the state
    // the state is passed to the submitForm() function
    // the submitForm() function is passed as a prop from the parent component (Booking.jsx)
    function handleSubmit(e) {
        e.preventDefault();
        const reservation = {
            date: date,
            time: time,
            guests: guests,
            occassion: occassion
        }
        submitForm(reservation);
    }

  return (
    <form onSubmit={handleSubmit}>
        <h1>Reserve a table</h1>
        <div className="form-group">
            <label htmlFor="Date">Choose Date</label>
            <input type="date" name='date' value={date} onChange={handleDateChange} required/>
        </div>
        <div className="form-group">
            <label htmlFor="time">Choose Time</label>
            <select name='time' value={time} onChange={(e) => setTime(e.target.value)} required>
                {
                    availableTimes.times.map((time, index) => {
                        return <option value={time} key={index}>{ time }</option>
                    })
                }
            </select>
        </div>
        <div className="form-group">
            <label htmlFor="guests">Number of guests</label>
            <input type="number" min="1" max="10" value={guests} onChange={(e) => setGuests(e.target.value)} required/>
        </div>
        <div className="form-group">
            <label htmlFor="occassion">Occassion</label>
            <select name='occassion' value={occassion} onChange={(e) => setOccassion(e.target.value)} >
                <option value="Birthday">Birthday</option>
                <option value="Business">Business</option>
                <option value="Date">Date</option>
                <option value="Family">Family</option>
                <option value="Friends">Friends</option>
                <option value="Engagement">Engagement</option>
                <option value="Anniversary">Anniversary</option>
            </select>
        </div>
        <input type="submit" value="Confirm Reservation" />
    </form>
  )
}

export default BookingForm
 17  
src/components/ConfirmedBooking.jsx
@@ -0,0 +1,17 @@
import React from "react";


const ConfirmedBooking = () => {
  return (
    <>
      <div className="confirmedbooking">
        <div className="container">
          <h1>Your have reserved table successfully!</h1>
          {/* <img src={ApprovalAnimated} alt="Animated Approval" /> */}
        </div>
      </div>
    </>
  );
};

export default ConfirmedBooking;
 28  
src/components/Contact.jsx
@@ -0,0 +1,28 @@
/* Contact component */

import React from 'react';
import { Link } from 'react-router-dom';

const Contact = () => {
    return (
        <div className="contact">
            <div className="container">
                <div className="row">
                    <div className="col-md-12">
                        <div className="contact__content">
                            <div className="contact__content-text">
                                <h1 className="contact__content-title">Contact Page</h1>
                                <p className="contact__content-desc">Lorem ipsum dolor sit amet, consectetur adipisicing elit. Quisquam, quod.</p>
                            </div>
                            <div className="contact__content-btn">
                                <Link className="contact__content-link" to="/">Home</Link>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    )
}

export default Contact;
 26  
src/components/FoodCard.jsx
@@ -0,0 +1,26 @@
import React from 'react'

const FoodCard = (props) => {
  return (
    <div className='foodcard'>
        <img src={props.image} alt={props.itemname} />
        <div>
          <h4>{props.itemname}</h4>
          <h4 className='price'>{props.price}</h4>
        </div>
        <p>{props.description}</p>
        <button>
          <p>Order for delivery</p>
          <div>
            <svg width="17" height="11" viewBox="0 0 17 11" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path d="M14.4708 1.72713C14.4708 0.8993 13.7206 0.221985 12.8036 0.221985H10.3028V1.72713H12.8036V3.72145L9.9027 6.99513H6.96843V3.23227H3.63404C1.79179 3.23227 0.299652 4.57938 0.299652 6.24256V8.50028H1.96685C1.96685 9.74955 3.08387 10.758 4.46764 10.758C5.85141 10.758 6.96843 9.74955 6.96843 8.50028H10.7029L14.4708 4.24825V1.72713ZM4.46764 9.25285C4.00916 9.25285 3.63404 8.91419 3.63404 8.50028H5.30124C5.30124 8.91419 4.92612 9.25285 4.46764 9.25285Z" fill="black"/>
            <path d="M6.96844 0.974548H2.80045V2.47968H6.96844V0.974548Z" fill="black"/>
            <path d="M14.4708 6.24255C13.087 6.24255 11.97 7.251 11.97 8.50026C11.97 9.74952 13.087 10.758 14.4708 10.758C15.8546 10.758 16.9716 9.74952 16.9716 8.50026C16.9716 7.251 15.8546 6.24255 14.4708 6.24255ZM14.4708 9.25283C14.0123 9.25283 13.6372 8.91417 13.6372 8.50026C13.6372 8.08635 14.0123 7.74769 14.4708 7.74769C14.9293 7.74769 15.3044 8.08635 15.3044 8.50026C15.3044 8.91417 14.9293 9.25283 14.4708 9.25283Z" fill="black"/>
            </svg>
          </div>
        </button>
    </div>
  )
}

export default FoodCard
 17  
src/components/FoodCards.jsx
@@ -0,0 +1,17 @@
import React from 'react'
import GreekSalad from '../assets/greek salad.jpg'
import Bruchetta from '../assets/bruchetta.svg'
import LemonDessert from '../assets/lemon dessert.jpg'
import FoodCard from './FoodCard'

const FoodCards = () => {
  return (
    <div className='foodcards'>
        <FoodCard image={GreekSalad} itemname="Greek Salad" price="$12.99" description="Lorem ipsum dolor sit amet consectetur adipisicing elit. Autem rerum aspernatur corporis, accusantium velit iusto et quam maiores facere cumque?" />
        <FoodCard image={Bruchetta} itemname="Bruchetta" price="$5.99" description="Lorem ipsum dolor sit amet consectetur adipisicing elit. Autem rerum aspernatur corporis, accusantium velit iusto et quam maiores facere cumque?" />
        <FoodCard image={LemonDessert} itemname="Lemon Dessert" price="$2.99" description="Lorem ipsum dolor sit amet consectetur adipisicing elit. Autem rerum aspernatur corporis, accusantium velit iusto et quam maiores facere cumque?" />
    </div>
  )
}

export default FoodCards
 36  
src/components/Footer.jsx
@@ -0,0 +1,36 @@
/* footer component */
import React from 'react';
import Logo from '../assets/Logo.svg'

const Footer = () => {
  return (
    <section id="footer">
      <div className="container">

        <ul>
          <li>Navigation</li>
          <li>Home</li>
          <li>About</li>
          <li>Menu</li>
          <li>Reservations</li>
          <li>Order Online</li>
          <li>Login</li>
        </ul>
        <ul>
          <li>Contact</li>
          <li>Address</li>
          <li>Phone Number</li>
          <li>Email</li>
        </ul>
        <ul>
          <li>Social Media Links</li>
          <li>Facebook</li>
          <li>Instagram</li>
          <li>Foodgram</li>
        </ul>
        <img src={Logo} alt="Little Lemon Restaurant Logo" />
      </div>
    </section>
  )
}
export default Footer;
 24  
src/components/Header.jsx
@@ -0,0 +1,24 @@
/* Header Component */
import React from 'react';
import { Link } from 'react-router-dom';
import Logo from '../assets/Logo.svg'

const Header = () => {
    return (
        <header>
        <div className="container">
          <img src={Logo} alt="Little Lemon logo" />
          <ul>
            <li><Link to="/">Home</Link></li>
            <li><Link to="about">About</Link></li>
            <li><Link to="menu">Menu</Link></li>
            <li><Link to="reservations">Reservations</Link></li>
            <li><Link to="order">Order-Online</Link></li>
            <li><Link to="login">Login</Link></li>
          </ul>
        </div>
      </header>
    )
}

export default Header;
 29  
src/components/Hero.jsx
@@ -0,0 +1,29 @@
import React from "react";
import RestaurantFood from "../assets/restauranfood.jpg";
import { Link } from "react-router-dom";

const Hero = () => {
  return (
    <section id="hero">
      <div className="container">
        <div className="content">
          <h1>Little Lemon</h1>
          <h2>Chicago</h2>
          <p>
            We are a family-owned Mediterranean restaurant, focused on
            traditional recipes served with a modern twist. We offer a variety
            of dishes made with the freshest ingredients and cooked to
            perfection. Our menu features a selection of appetizers, salads,
            entrees, and desserts.
          </p>
          <button>
            <Link to="booking">Reserve a table</Link>
          </button>
        </div>
        <img src={RestaurantFood} alt="Little Lemon Restaurant Food" />
      </div>
    </section>
  );
};

export default Hero;
 18  
src/components/Highlights.jsx
@@ -0,0 +1,18 @@
import React from 'react'
import FoodCards from './FoodCards'

const Highlights = () => {
  return (
    <section id="highlights">
      <div className="container">
        <div className="content">
          <h2>Specials</h2>
          <button>Online Menu</button>
        </div>
        <FoodCards />
      </div>
    </section>
  )
}

export default Highlights
 20  
src/components/Home.jsx
@@ -0,0 +1,20 @@
/* Home component */

import React from "react";
import About from "./About";
import Highlights from "./Highlights";
import Hero from "./Hero";
import Testimonials from "./Testimonials";

const Home = () => {
  return (
    <div className="App">
      <Hero />
      <Highlights />
      <Testimonials />
      <About />
    </div>
  );
};

export default Home;
 22  
src/components/Notfound.jsx
@@ -0,0 +1,22 @@
/* NotFound component */

import React from "react";
import { Link } from "react-router-dom";

const NotFound = () => {
  return (
    <section className="notfound">
      <div className="container">
        <div className="content">
          <h1>Oops! Something is wrong ...</h1>
          <h2>404: Page not found</h2>
          <button>
            <Link to="/">Go back in initial page, is better.</Link>
          </button>
         </div>
      </div>
    </section>
  );
};

export default NotFound;
 16  
src/components/Testimonial.jsx
@@ -0,0 +1,16 @@
import React from 'react'

const Testimonial = (props) => {
  return (
    <div className="testimonial">
        <img src={props.image} alt={props.name} />
        <div className="content">
          <h4>{props.name}</h4>
          <h4>{props.rating}</h4>
          <p>"{props.testimonial}"</p>
        </div>
    </div>
  )
}

export default Testimonial
 22  
src/components/Testimonials.jsx
@@ -0,0 +1,22 @@
import React from 'react'
import Testimonial from './Testimonial'
import John from '../assets/john.png'
import Mary from '../assets/mary.png'
import Tom from '../assets/tom.png'

const Testimonials = () => {
  return (
    <section id="testimonials">
      <div className="container">
        <h2>Testimonials</h2>
        <div>
          <Testimonial image={John} name="John Doe" rating="5.0" testimonial="I have been a regular at Little Lemon for years and I can confidently say that it is one of the best restaurants in town. The food is always delicious, made with the freshest ingredients and cooked to perfection. The service is also top-notch, the staff is always friendly and attentive. I highly recommend this place to anyone looking for a great dining experience." />
          <Testimonial image={Mary} name="Mary Jane" rating="4.7" testimonial="I had the pleasure of trying Little Lemon for the first time last week and I was blown away. The ambiance was cozy and inviting, and the food was some of the best I've ever had. I especially loved the bruchetta and lemon dessert, both were expertly prepared and absolutely delicious. I will definitely be returning!" />
          <Testimonial image={Tom} name="Tom McGill" rating="4.9" testimonial="I recently attended a friend's birthday party at Little Lemon and was impressed by the quality of the food and service. The staff was very kind and accommodating to make sure that everything was perfect for the special occasion. The food was amazing and everyone was raving about it. I would highly recommend Little Lemon for any event, big or small." />
        </div>
      </div>
    </section>
  )
}

export default Testimonials
 1  
src/logo.svg
Unable to render rich display

0 comments on commit 76346bd
@Adityadhanavade
 
Add heading textAdd bold text, <Ctrl+b>Add italic text, <Ctrl+i>
Add a quote, <Ctrl+Shift+.>Add code, <Ctrl+e>Add a link, <Ctrl+k>
Add a bulleted list, <Ctrl+Shift+8>Add a numbered list, <Ctrl+Shift+7>Add a task list, <Ctrl+Shift+l>
Directly mention a user or team
Reference an issue, pull request, or discussion
Add saved reply
Leave a comment
No file chosen
Attach files by dragging & dropping, selecting or pasting them.
Styling with Markdown is supported
 You’re not receiving notifications from this thread.
Footer
© 2023 GitHub, Inc.
Footer navigation
Terms
Privacy
Security
Status
Docs
Contact GitHub
Pricing
API
Training
Blog
About
compelete the littel lemon · soheilnikroo/little-lemon@76346bd
