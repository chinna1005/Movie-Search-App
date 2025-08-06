# Movie-Search-App
#html code<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Movie Search App</title>
  <link rel="stylesheet" href="style.css" />
</head>
<body>
    <div class="container">
        <h1>🎬 Movie Search App</h1>
        <input type="text" id="searchInput" placeholder="Search for a movie..." />
        <button onclick="searchMovie()">Search</button>
        <button onclick="refreshpage()">restart</button>
        <div id="movieContainer"></div>
    </div>

<script src="script.js"></script>
</body>
</html>

#CSS Code
body {
  font-family: Arial, sans-serif;
  background-image: url("https://fastly.restofworld.org/uploads/2024/04/IMG_0116-scaled.jpeg?width=800&dpr=2&crop=16:9"); /* <-- change to your image file name */
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

.movie img {
  width: 120px;
  height: auto;
  border-radius: 8px;
}

.movie-details {
  flex: 1;
}


#JavaScript code
async function searchMovie() {
  const query = document.getElementById("searchInput").value;
  const movieContainer = document.getElementById("movieContainer");
  movieContainer.innerHTML = "";

  if (!query) {
    movieContainer.innerHTML = "<p>Please enter a movie name.</p>";
    return;
  }

  const apiKey = "a238497"; // ✅ working API key

  try {
    const response = await fetch(`https://www.omdbapi.com/?s=${query}&apikey=${apiKey}`);
    const data = await response.json();

    if (data.Response === "True") {
      for (const movie of data.Search) {
        const movieDetailRes = await fetch(`https://www.omdbapi.com/?i=${movie.imdbID}&apikey=${apiKey}`);
        const movieDetail = await movieDetailRes.json();

        const movieEl = document.createElement("div");
        movieEl.classList.add("movie");

        movieEl.innerHTML = `
          <img src="${movieDetail.Poster !== "N/A" ? movieDetail.Poster : 'https://via.placeholder.com/120x180'}" alt="${movieDetail.Title}" />
          <div class="movie-details">
            <h2>${movieDetail.Title} (${movieDetail.Year})</h2>
            <p><strong>Genre:</strong> ${movieDetail.Genre}</p>
            <p><strong>IMDb Rating:</strong> ${movieDetail.imdbRating}</p>
            <p><strong>Plot:</strong> ${movieDetail.Plot}</p>
          </div>
        `;
        movieContainer.appendChild(movieEl);
      }
    } else {
      movieContainer.innerHTML = `<p>No results found for "${query}".</p>`;
    }
  } catch (error) {
    movieContainer.innerHTML = `<p>Error: ${error.message}</p>`;
    console.error("Error fetching movie data:", error);
  }
}
