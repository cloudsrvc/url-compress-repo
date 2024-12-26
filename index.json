require('dotenv').config(); // This will load the .env file
const express = require('express');
const mongoose = require('mongoose');
const shortid = require('shortid');

// Initialize app
const app = express();
app.use(express.json());

// MongoDB connection
mongoose.connect(process.env.MONGO_URI, {
    useNewUrlParser: true,
    useUnifiedTopology: true,
  })
    .then(() => console.log("MongoDB connected"))
    .catch(err => console.log("MongoDB connection error:", err));
  

// Define the URL schema and model
const urlSchema = new mongoose.Schema({
  originalUrl: String,
  shortUrl: String,
});

const Url = mongoose.model('Url', urlSchema);

// Root route
app.get('/', (req, res) => {
  res.send('Welcome to the URL Shortener API');
});

// Route to create a short URL
app.post('/shorten', async (req, res) => {
  const { originalUrl } = req.body;

  // Check if the URL already exists
  let url = await Url.findOne({ originalUrl });

  if (url) {
    return res.json({ shortUrl: url.shortUrl });
  }

  // Generate a short URL
  const shortUrl = shortid.generate();

  // Save the new URL
  url = new Url({
    originalUrl,
    shortUrl,
  });

  await url.save();

  res.json({ shortUrl });
});

// Route to redirect to the original URL
app.get('/:shortUrl', async (req, res) => {
  const { shortUrl } = req.params;

  const url = await Url.findOne({ shortUrl });

  if (!url) {
    return res.status(404).send('URL not found');
  }

  res.redirect(url.originalUrl);
});

// Start the server
const port = process.env.PORT || 3000;
app.listen(port, () => {
  console.log(`Server running on port ${port}`);
});
