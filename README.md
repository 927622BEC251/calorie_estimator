# calorie_estimator
// html code for calorie_estimator using ML

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Food Calorie Estimator</title>
  <link rel="stylesheet" href="aitel.css" />
</head>
<body>

  <div class="container">
    <h1>🍱 Food Calorie Estimator</h1>
    <p>Upload an image of food to estimate its calories.</p>

    <input type="file" id="imageInput" accept="image/*" onchange="previewImage(event)" />
    <img id="imagePreview" class="hidden" alt="Image Preview" />

    <button onclick="submitImage()">Estimate Calories</button>

    <div id="result"></div>
  </div>

  <script src="voda.js"></script>
</body>
</html>


// css code

body {
    font-family: 'Segoe UI', sans-serif;
    background-image:url('https://as1.ftcdn.net/v2/jpg/02/52/38/80/1000_F_252388016_KjPnB9vglSCuUJAumCDNbmMzGdzPAucK.jpg');
    margin: 0;
    padding: 0;
  }
  
  .container {
    max-width: 500px;
    background: rgb(247, 245, 245);
    margin: 5% auto;
    padding: 30px;
    border-radius: 10px;
    text-align: center;
    box-shadow: 0 0 10px rgba(0,0,0,0.1);
  }
  
  input[type="file"] {
    padding: 10px;
    font-size: 16px;
    margin: 15px 0;
  }
  
  button {
    padding: 12px 20px;
    background-color: #2a9d8f;
    color: white;
    border: none;
    border-radius: 6px;
    font-size: 16px;
    cursor: pointer;
  }
  
  button:hover {
    background-color: #21867a;
  }
  
  .hidden {
    display: none;
  }
  
  #imagePreview {
    max-width: 100%;
    margin-top: 20px;
    border-radius: 6px;
    box-shadow: 0 4px 8px rgba(0,0,0,0.1);
  }
  
  #result {
    margin-top: 20px;
    font-weight: bold;
    font-size: 18px;
  }

  // js for calorie estimator

  let selectedImage = null;

function previewImage(event) {
  selectedImage = event.target.files[0];
  const imagePreview = document.getElementById("imagePreview");
  imagePreview.src = URL.createObjectURL(selectedImage);
  imagePreview.classList.remove("hidden");
}

function submitImage() {
  const resultDiv = document.getElementById("result");

  if (!selectedImage) {
    resultDiv.textContent = "⚠️ Please upload an image!";
    return;
  }

  // Mock food detection: use filename to simulate
  const fileName = selectedImage.name.toLowerCase();
  const isFood = fileName.includes("food") || fileName.includes("meal") || fileName.includes("rice");

  if (!isFood) {
    resultDiv.innerHTML = "❌ This does not look like a food image.";
    return;
  }

  // Estimate calories (simulate via random logic)
  const calories = Math.floor(Math.random() * 600) + 200;

  resultDiv.innerHTML = `🔥 Estimated Calories: <strong>${calories} kcal</strong>`;

  // Store in backend
  const formData = new FormData();
  formData.append("fileName", selectedImage.name);
  formData.append("calories", calories);

  fetch("/save", {
    method: "POST",
    body: formData
  }).then(response => response.json())
    .then(data => console.log("Saved to DB:", data))
    .catch(err => console.error("Error saving data:", err));
}

  
