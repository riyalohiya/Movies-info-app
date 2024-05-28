# Movies-info-app
//HTML STYLING 
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Movie App</title>
<link rel="stylesheet" href="style.css" />
<script src="script.js" defer></script>
</head>
<body>
<header>
<h1>Trending Movies</h1>
<form id="form">
<input
type="text"
id="search"
placeholder="Search"
class="search"
/>
</form>
</header>
<div id="content"></div>
</body>
</html>

//JAVASCRIPT LOGIC

const APIURL = "https://api.themoviedb.org/3/discover/movie?api_key=04c35731a5ee918f014970082a0088b1";
const IMGPATH = "https://image.tmdb.org/t/p/w1280";
const SEARCHAPI =
"https://api.themoviedb.org/3/search/movie?&api_key=04c35731a5ee918f014970082a0088b1&query=";
const main = document.getElementById("content");
const form = document.getElementById("form");
const search = document.getElementById("search");
// initially get fav movies
getMovies(APIURL);
async function getMovies(url) {
const resp = await fetch(url);
const respData = await resp.json();
console.log(respData);
showMovies(respData.results);
}
function showMovies(movies) {
// clear main
main.innerHTML = "";
movie.forEach((movie) => {
const { poster_path, title, vote_average, overview } = movie;
const movieEl = document.createElement("div");
movieEl.classList.add("movie");
movieEl.innerHTML = `
<img
src="${IMGPATH + poster_path}"
alt="${title}"
/>
<div class="movie-info">
<h3>${title}</h3>
<span class="${getClassByRate(
vote_average
)}">${vote_average}</span>
</div>
<div class="overview">
<h3>Overview:</h3>
${overview}
</div>
`;
main.appendChild(movieEl);
});
}
function getClassByRate(vote) {
if (vote >= 8) {
return "green";
} else if (vote >= 5) {
return "orange";
} else {
return "red";
}
}
form.addEventListener("submit", (e) => 
e.preventDefault();
const searchTerm = search.value;
if (searchTerm) {
getMovies(SEARCHAPI + searchTerm);
search.value = "";
}
});

//CSS STYLING

@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@200;400;600&display=swap");
* {
box-sizing: border-box;
}
body {
background-color: #030303;
font-family: "Poppins", sans-serif;
margin: 0;
}
header {
background-color: #363636;
display: flex;
}
h1{
color: whitesmoke;
align-self: center;
margin-left: 28rem;
}
.search {
margin: 3rem;
margin-left: 12rem;
background-color: transparent;
border: 2px solid #e9e9ec;
border-radius: 50px;
color: #fff;
font-family: inherit;
font-size: 1rem;
padding: 0.5rem 1rem;
}
.search::placeholder {
color: #7378c5;
}
.search:focus {
background-color: #050505;
outline: none;
}
div{
display: flex;
flex-wrap: wrap;
}
.movie {
background-color: #373b69;
border-radius: 3px;
box-shadow: 0 4px 5px rgba(0, 0, 0, 0.2);
overflow: hidden;
position: relative;
margin: 1rem;
justify-content: center;
align-content: center;
width: 265px;
}
.movie img {
width: 100%;
}
.movie-info {
color: #eee;
display: flex;
flex-direction: column;
align-items: center;
justify-content: center;
padding: 0.5rem 1rem 1rem;
letter-spacing: 0.5px;
}
.movie-info h3 {
margin: 0;
}
.movie-info span {
background-color: #22254b;
border-radius: 3px;
font-weight: bold;
padding: 0.25rem 0.8rem;
}
.movie-info span.green {
color: rgb(39, 189, 39);
}
.movie-info span.orange {
color: orange;
}
.movie-info span.red {
color: rgb(189, 42, 42);
}
.overview {
background-color: #fff;
padding: 2rem;
position: absolute;
max-height: 100%;
overflow: auto;
left: 0;
bottom: 0;
right: 0;
transform: translateY(101%);
transition: transform 0.3s ease-in;
}
.overview h3 {
margin-top: 0;
}
.movie:hover .overview {
transform: translateY(0);
}
