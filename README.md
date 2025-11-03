
## 🧩 Folder Structure

```
home-service/
│
├── Backend/
│   ├── models/
│   │   └── Service.js
│   ├── routes/
│   │   └── serviceRoutes.js
│   ├── server.js
│   ├── package.json
│
└── frontend/
    ├── public/
    │   └── index.html
    ├── src/
    │   ├── components/
    │   │   ├── Navbar.js
    │   │   ├── ServiceCard.js
    │   │   └── BookingPage.js
    │   ├── App.js
    │   ├── index.js
    │   ├── index.css
    ├── package.json
```

---

## ⚙️ BACKEND CODE

### 📁 `Backend/package.json`

```json
{
  "name": "homeease-backend",
  "version": "1.0.0",
  "main": "server.js",
  "type": "module",
  "scripts": {
    "start": "node server.js"
  },
  "dependencies": {
    "cors": "^2.8.5",
    "express": "^4.18.2",
    "mongoose": "^7.0.3"
  }
}
```

---

### 📄 `Backend/models/Service.js`

```js
import mongoose from "mongoose";

const serviceSchema = new mongoose.Schema({
  name: String,
  price: Number,
  description: String,
  category: String
});

const Service = mongoose.model("Service", serviceSchema);
export default Service;
```

---

### 📄 `Backend/routes/serviceRoutes.js`

```js
import express from "express";
const router = express.Router();

// Dummy data for now
const services = [
  { _id: 1, name: "Plumbing", price: 250, description: "Fix leaks and install bathroom fittings", category: "Home Repair" },
  { _id: 2, name: "Electrical", price: 300, description: "Wiring, switches, and fan repairs", category: "Maintenance" },
  { _id: 3, name: "Cleaning", price: 200, description: "Deep cleaning and regular maintenance", category: "House Care" },
  { _id: 4, name: "AC Service", price: 350, description: "AC installation and gas refill", category: "Cooling" },
  { _id: 5, name: "Carpentry", price: 400, description: "Furniture repair and fitting", category: "Woodwork" }
];

router.get("/", (req, res) => {
  res.json(services);
});

export default router;
```

---

### 📄 `Backend/server.js`

```js
import express from "express";
import cors from "cors";
import serviceRoutes from "./routes/serviceRoutes.js";

const app = express();
app.use(cors());
app.use(express.json());

app.use("/services", serviceRoutes);

const PORT = 5000;
app.listen(PORT, () => console.log(`✅ Server running on port ${PORT}`));
```

---

## 💻 FRONTEND CODE

### 📁 `frontend/package.json`

```json
{
  "name": "homeease-frontend",
  "version": "1.0.0",
  "private": true,
  "dependencies": {
    "axios": "^1.7.2",
    "react": "^18.3.1",
    "react-dom": "^18.3.1",
    "react-router-dom": "^6.23.1",
    "react-scripts": "5.0.1"
  },
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build"
  }
}
```

---

### 📄 `frontend/public/index.html`

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>HomeEase</title>
  </head>
  <body>
    <div id="root"></div>
  </body>
</html>
```

---

### 📄 `frontend/src/index.js`

```js
import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App";
import "./index.css";

const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(<App />);
```

---

### 📄 `frontend/src/index.css`

```css
body {
  font-family: Arial, sans-serif;
  margin: 0;
  background-color: #f8f9fa;
  color: #333;
}

h2 {
  color: #4a4ae0;
}

button {
  background-color: #4a4ae0;
  color: white;
  border: none;
  padding: 8px 12px;
  cursor: pointer;
  border-radius: 6px;
}

button:hover {
  background-color: #2c2ca0;
}
```

---

### 📄 `frontend/src/App.js`

```js
import React, { useEffect, useState } from "react";
import axios from "axios";
import Navbar from "./components/Navbar";
import ServiceCard from "./components/ServiceCard";
import BookingPage from "./components/BookingPage";
import { BrowserRouter as Router, Routes, Route } from "react-router-dom";

function App() {
  const [services, setServices] = useState([]);

  useEffect(() => {
    axios
      .get("http://localhost:5000/services")
      .then((res) => setServices(res.data))
      .catch((err) => console.error("Error fetching services:", err));
  }, []);

  return (
    <Router>
      <Navbar />
      <main style={{ padding: "20px" }}>
        <Routes>
          <Route
            path="/"
            element={
              <>
                <h2>Available Services</h2>
                <div style={{ display: "flex", flexWrap: "wrap", gap: "20px" }}>
                  {services.map((srv) => (
                    <ServiceCard key={srv._id} service={srv} />
                  ))}
                </div>
              </>
            }
          />
          <Route path="/book/:id" element={<BookingPage services={services} />} />
        </Routes>
      </main>
    </Router>
  );
}

export default App;
```

---

### 📄 `frontend/src/components/Navbar.js`

```js
import React from "react";

const Navbar = () => (
  <nav style={{
    display: "flex",
    justifyContent: "space-between",
    alignItems: "center",
    padding: "10px 20px",
    backgroundColor: "#4a4ae0",
    color: "white"
  }}>
    <h1>HomeEase</h1>
    <div>
      <input type="text" placeholder="Search services..." style={{ padding: "5px" }} />
      <button style={{ marginLeft: "10px", backgroundColor: "#fff", color: "#4a4ae0" }}>Sign In</button>
    </div>
  </nav>
);

export default Navbar;
```

---

### 📄 `frontend/src/components/ServiceCard.js`

```js
import React from "react";
import { useNavigate } from "react-router-dom";

const ServiceCard = ({ service }) => {
  const navigate = useNavigate();
  return (
    <div
      style={{
        border: "1px solid #ccc",
        borderRadius: "10px",
        padding: "15px",
        width: "250px",
        background: "#fff"
      }}
    >
      <h3>{service.name}</h3>
      <p><b>₹{service.price}</b></p>
      <p>{service.description}</p>
      <small>{service.category}</small>
      <br />
      <button onClick={() => navigate(`/book/${service._id}`)}>Book Now</button>
    </div>
  );
};

export default ServiceCard;
```

---

### 📄 `frontend/src/components/BookingPage.js`

```js
import React from "react";
import { useParams, useNavigate } from "react-router-dom";

const BookingPage = ({ services }) => {
  const { id } = useParams();
  const navigate = useNavigate();
  const service = services.find((s) => s._id === parseInt(id));

  if (!service) return <p>Service not found.</p>;

  const handleBooking = () => {
    alert(`✅ Booking confirmed for ${service.name}`);
    navigate("/");
  };

  return (
    <div style={{ padding: "30px" }}>
      <h2>Booking for {service.name}</h2>
      <p><b>Price:</b> ₹{service.price}</p>
      <p>{service.description}</p>
      <button onClick={handleBooking}>Confirm Booking</button>
    </div>
  );
};

export default BookingPage;
```

---

## 🧠 How to Run

### 🟢 Backend

```bash
cd Backend
npm install
node server.js
```

### 🔵 Frontend

```bash
cd frontend
npm install
npm start
```

Then open 👉 `http://localhost:3000`


