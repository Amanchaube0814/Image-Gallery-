HTML 

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Responsive Image Gallery</title>
  <link rel="stylesheet" href="style.css" />
</head>
<body>
  <h1>Image Gallery</h1>

  <!-- Filter Buttons -->
  <div class="filter-buttons">
    <button onclick="filterImages('all')">All</button>
    <button onclick="filterImages('nature')">Nature</button>
    <button onclick="filterImages('animals')">Animals</button>
    <button onclick="filterImages('cities')">Cities</button>
  </div>

  <!-- Image Grid -->
  <div class="gallery">
    <div class="image nature"><img src="nature1.jpg" alt="Nature 1" onclick="openLightbox(this)" /></div>
    <div class="image animals"><img src="animal1.jpg" alt="Animal 1" onclick="openLightbox(this)" /></div>
    <div class="image cities"><img src="city1.jpg" alt="City 1" onclick="openLightbox(this)" /></div>
    <div class="image nature"><img src="nature2.jpg" alt="Nature 2" onclick="openLightbox(this)" /></div>
    <div class="image animals"><img src="animal2.jpg" alt="Animal 2" onclick="openLightbox(this)" /></div>
    <div class="image cities"><img src="city2.jpg" alt="City 2" onclick="openLightbox(this)" /></div>
  </div>

  <!-- Lightbox -->
  <div id="lightbox" class="lightbox">
    <span class="close" onclick="closeLightbox()">&times;</span>
    <img class="lightbox-img" id="lightbox-img" />
    <div class="lightbox-nav">
      <span class="prev" onclick="changeImage(-1)">❮</span>
      <span class="next" onclick="changeImage(1)">❯</span>
    </div>
  </div>

  <script src="script.js"></script>
</body>
</html>

CSS

body {
  font-family: sans-serif;
  margin: 0;
  padding: 1rem;
  background-color: #f9f9f9;
}

h1 {
  text-align: center;
  margin-bottom: 1rem;
}

.filter-buttons {
  text-align: center;
  margin-bottom: 1rem;
}

.filter-buttons button {
  padding: 0.5rem 1rem;
  margin: 0 5px;
  border: none;
  background-color: #444;
  color: #fff;
  cursor: pointer;
  transition: background 0.3s;
}

.filter-buttons button:hover {
  background-color: #777;
}

.gallery {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 15px;
}

.image {
  overflow: hidden;
  position: relative;
  cursor: pointer;
}

.image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.4s ease;
}

.image:hover img {
  transform: scale(1.1);
  filter: brightness(80%);
}

/* Lightbox */
.lightbox {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0,0,0,0.9);
  display: none;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}

.lightbox-img {
  max-width: 90%;
  max-height: 80vh;
  margin-bottom: 10px;
}

.lightbox .close {
  position: absolute;
  top: 30px;
  right: 50px;
  font-size: 30px;
  color: #fff;
  cursor: pointer;
}

.lightbox-nav .prev,
.lightbox-nav .next {
  font-size: 2rem;
  color: white;
  cursor: pointer;
  margin: 0 2rem;
  user-select: none;
}

@media (max-width: 600px) {
  .lightbox-nav .prev,
  .lightbox-nav .next {
    font-size: 1.5rem;
    margin: 1rem;
  }
}


JS

let images = Array.from(document.querySelectorAll(".gallery .image img"));
let currentIndex = 0;

function openLightbox(img) {
  document.getElementById("lightbox").style.display = "flex";
  document.getElementById("lightbox-img").src = img.src;
  currentIndex = images.findIndex(i => i.src === img.src);
}

function closeLightbox() {
  document.getElementById("lightbox").style.display = "none";
}

function changeImage(step) {
  currentIndex += step;
  if (currentIndex < 0) currentIndex = images.length - 1;
  if (currentIndex >= images.length) currentIndex = 0;
  document.getElementById("lightbox-img").src = images[currentIndex].src;
}

function filterImages(category) {
  const allImages = document.querySelectorAll(".gallery .image");
  allImages.forEach(img => {
    img.style.display =
      category === "all" || img.classList.contains(category) ? "block" : "none";
  });
  // Update image array for lightbox
  images = Array.from(document.querySelectorAll(".gallery .image img")).filter(img =>
    category === "all" || img.parentElement.classList.contains(category)
  );
}
