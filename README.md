# 🎬 Movie Search App

A simple front-end app to search for movies using the OMDb API.  
Built with **HTML**, **CSS**, and **JavaScript**.

---

## 📄 index.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Movie Search App</title>
    <link rel="stylesheet" href="style.css" />
</head>
<body>
    <div class="container">
        <h1>🎬 Movie Search App</h1>
        <input type="text" id="searchInput" placeholder="Search for a movie..." />
        <button onclick="searchMovie()">Search</button>
        <button onclick="refreshpage()">Restart</button>
        <div id="movieContainer"></div>
    </div>
    <script src="script.js"></script>
</body>
</html>

# CSS Code
body {
    font-family: Arial, sans-serif;
    background-image: url("https://fastly.restofworld.org/uploads/2024/04/IMG_0116-scaled.jpeg?width=800&dpr=2&crop=16:9");
    background-size: cover;
    background-position: center;
    background-repeat: no-repeat;
    background-attachment: fixed;
    color: #f2efef;
    margin: 0;
    padding: 20px;
}

.container {
    max-width: 700px;
    margin: auto;
    text-align: center;
}

input[type="text"] {
    padding: 10px;
    width: 70%;
    margin: 10px 0;
    font-size: 16px;
    border-radius: 5px;
    border: none;
}

button {
    padding: 10px 20px;
    font-size: 16px;
    background-color: #03a9f4;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
}

#movieContainer {
    margin-top: 20px;
    text-align: left;
}

.movie {
    background-color: #2c2c2c;
    padding: 15px;
    margin-bottom: 20px;
    border-radius: 10px;
    display: flex;
    gap: 20px;
}


# JS Code
async function searchMovie() {
    const movieName = document.getElementById("searchInput").value;
    const response = await fetch(`https://www.omdbapi.com/?t=${movieName}&apikey=YOUR_API_KEY`);
    const data = await response.json();

    const container = document.getElementById("movieContainer");
    container.innerHTML = `
        <div class="movie">
            <img src="${data.Poster}" alt="${data.Title}" />
            <div>
                <h2>${data.Title} (${data.Year})</h2>
                <p>${data.Plot}</p>
            </div>
        </div>
    `;
}

function refreshpage() {
    window.location.reload();
}

