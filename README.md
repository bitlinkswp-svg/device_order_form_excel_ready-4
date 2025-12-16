# device-order-form 1
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Device Order Form</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<style>
body {
  font-family: Arial, sans-serif;
  background: #f4f4f5;
}
.container {
  max-width: 800px;
  margin: 30px auto;
  background: #fff;
  padding: 25px;
  border-radius: 10px;
}
label {
  font-weight: 600;
  display: block;
  margin-top: 12px;
}
input, select {
  width: 100%;
  padding: 8px;
  margin-top: 5px;
}
button {
  margin-top: 20px;
  padding: 12px 20px;
  background: #2563eb;
  color: #fff;
  border: none;
  border-radius: 20px;
  cursor: pointer;
}
.message {
  margin-top: 15px;
  font-weight: 600;
}
.success { color: green; }
.error { color: red; }
.hidden { display: none; }
</style>
</head>

<body>

<div class="container">
<h2>Device Order Form</h2>

<form id="orderForm">
<label>Name</label>
<input type="text" name="name" required>

<label>Vendor Address</label>
<select name="vendor" required>
<option value="">Select</option>
<option>350A Beverly Blvd, Upper Darby, PA 19082</option>
<option>34 Diamond St Elmont, NY 11003</option>
<option>309 Richfield Rd, Upper Darby, PA 19082</option>
<option>8006 Ramsburg Dr Charlotte, NC 28269</option>
</select>

<label>Quantity</label>
<select name="quantity" id="quantity" required>
<option value="">Select</option>
<option>1</option>
<option>2</option>
<option>3</option>
<option>4</option>
<option>5</option>
</select>

<div id="devices"></div>

<button type="submit">Submit</button>
<div class="message" id="msg"></div>
</form>
</div>

<script>
const webAppUrl = "https://script.google.com/macros/s/AKfycbyibMpKc8lc1-yy3eRqq05jZw5bTJ-DstdpDdB9NH0IrAy5JFVbUM_K-USVd0z-m3Py/exec";

const quantityEl = document.getElementById("quantity");
const devicesDiv = document.getElementById("devices");

quantityEl.addEventListener("change", () => {
  devicesDiv.innerHTML = "";
  let q = parseInt(quantityEl.value);
  for (let i = 1; i <= q; i++) {
    devicesDiv.innerHTML += `
      <label>Device ${i}</label>
      <select name="device${i}" required>
        <option value="">Select</option>
        <option>iPhone 17 Pro Max</option>
        <option>iPhone 16 Pro</option>
        <option>iPhone 15</option>
      </select>

      <label>Storage ${i}</label>
      <select name="gb${i}" required>
        <option value="">Select</option>
        <option>256GB</option>
        <option>512GB</option>
        <option>1TB</option>
      </select>
    `;
  }
});

document.getElementById("orderForm").addEventListener("submit", e => {
  e.preventDefault();
  const msg = document.getElementById("msg");
  msg.textContent = "Submitting...";
  msg.className = "message";

  const formData = new FormData(e.target);
  const data = Object.fromEntries(formData.entries());

  fetch(webAppUrl, {
    method: "POST",
    body: JSON.stringify(data)
  })
  .then(res => res.json())
  .then(() => {
    msg.textContent = "Order submitted successfully!";
    msg.classList.add("success");
    e.target.reset();
    devicesDiv.innerHTML = "";
  })
  .catch(() => {
    msg.textContent = "Submission failed. Try again.";
    msg.classList.add("error");
  });
});
</script>

</body>
</html>
