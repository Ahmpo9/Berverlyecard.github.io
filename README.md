<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>E-Invitation Maker</title>
  <link rel="stylesheet" href="styles.css">
</head>
<body>
  <header>
    <h1>Create Your E-Invitation</h1>
  </header>
  <main>
    <form id="invitationForm">
      <label for="eventName">Event Name:</label>
      <input type="text" id="eventName" name="eventName" required>

      <label for="eventDate">Event Date:</label>
      <input type="date" id="eventDate" name="eventDate" required>

      <label for="eventLocation">Event Location:</label>
      <input type="text" id="eventLocation" name="eventLocation" required>

      <label for="eventDescription">Event Description:</label>
      <textarea id="eventDescription" name="eventDescription" rows="4" required></textarea>

      <button type="submit">Create Invitation</button>
    </form>

    <div id="invitationPreview" class="hidden">
      <h2>Your E-Invitation</h2>
      <p><strong>Event Name:</strong> <span id="previewEventName"></span></p>
      <p><strong>Date:</strong> <span id="previewEventDate"></span></p>
      <p><strong>Location:</strong> <span id="previewEventLocation"></span></p>
      <p><strong>Description:</strong> <span id="previewEventDescription"></span></p>
    </div>
  </main>
  <script src="script.js"></script>
</body>
</html>
body {
  font-family: Arial, sans-serif;
  background-color: #f4f4f4;
  margin: 0;
  padding: 0;
}

header {
  background-color: #333;
  color: #fff;
  padding: 20px;
  text-align: center;
}

main {
  padding: 20px;
}

form {
  background-color: #fff;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

label {
  display: block;
  margin-bottom: 8px;
  font-weight: bold;
}

input, textarea {
  width: 100%;
  padding: 10px;
  margin-bottom: 20px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

button {
  background-color: #333;
  color: #fff;
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

button:hover {
  background-color: #555;
}

#invitationPreview {
  margin-top: 20px;
  background-color: #fff;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}
.hidden {
  display: none;
}
document.getElementById('invitationForm').addEventListener('submit', function (e) {
  e.preventDefault();

  // Get form values
  const eventName = document.getElementById('eventName').value;
  const eventDate = document.getElementById('eventDate').value;
  const eventLocation = document.getElementById('eventLocation').value;
  const eventDescription = document.getElementById('eventDescription').value;

  // Display preview
  document.getElementById('previewEventName').textContent = eventName;
  document.getElementById('previewEventDate').textContent = eventDate;
  document.getElementById('previewEventLocation').textContent = eventLocation;
  document.getElementById('previewEventDescription').textContent = eventDescription;

  // Show preview section
  document.getElementById('invitationPreview').classList.remove('hidden');
});
npm init -y
npm install express mongoose body-parser cors
const express = require('express');
const mongoose = require('mongoose');
const bodyParser = require('body-parser');
const cors = require('cors');

const app = express();
const PORT = 5000;

// Middleware
app.use(bodyParser.json());
app.use(cors());

// Connect to MongoDB
mongoose.connect('mongodb://localhost:27017/eventDB', {
  useNewUrlParser: true,
  useUnifiedTopology: true,
});

// Event Schema
const eventSchema = new mongoose.Schema({
  eventName: String,
  eventDate: String,
  eventLocation: String,
  eventDescription: String,
});
const Event = mongoose.model('Event', eventSchema);

// API to save event
app.post('/api/events', async (req, res) => {
  const { eventName, eventDate, eventLocation, eventDescription } = req.body;

  const newEvent = new Event({
    eventName,
    eventDate,
    eventLocation,
    eventDescription,
  });
  await newEvent.save();
  res.status(201).send('Event saved successfully!');
});

// Start server
app.listen(PORT, () => {
  console.log(`Server running on http://localhost:${PORT}`);
});

document.getElementById('invitationForm').addEventListener('submit', async function (e) {
  e.preventDefault();

  const eventName = document.getElementById('eventName').value;
  const eventDate = document.getElementById('eventDate').value;
  const eventLocation = document.getElementById('eventLocation').value;
  const eventDescription = document.getElementById('eventDescription').value;

  const response = await fetch('http://localhost:5000/api/events', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify({ eventName, eventDate, eventLocation, eventDescription }),
  });

  if (response.ok) {
    alert('Event saved successfully!');
  } else {
    alert('Failed to save event.');
  }
});


node server.js
