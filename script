document.addEventListener("DOMContentLoaded", () => {
  const form = document.getElementById("lendingForm");
  const registerForm = document.getElementById("registerForm");
  const logoutBtn = document.getElementById("logoutBtn");
  const tableBody = document.querySelector("#recordsTable tbody");

  const storageName = "ddlt-password";
  logoutBtn.addEventListener("click", (e) => displayScreen("login"));

  const displayScreen = (screenName) => {
    const landingScreeen = document.getElementById("landingContainer");
    const registerScreeen = document.getElementById("registerContainer");
    const loginScreeen = document.getElementById("loginContainer");

    landingScreeen.style.display = "none";
    registerScreeen.style.display = "none";
    loginScreeen.style.display = "none";
    logoutBtn.style.display = "none";

    switch (screenName) {
      case "login":
        return (loginScreeen.style.display = "block");
      case "landing":
        logoutBtn.style.display = "block";
        return (landingScreeen.style.display = "block");
      case "register":
      default:
        registerScreeen.style.display = "block";
    }
  };

  const setError = (errorType = "login", errorMsg = null) => {
    const errorPanel = errorType == "login" ? 1 : 0;
    const error = document.querySelectorAll(".error")[errorPanel];

    error.innerText = "";

    // Hide the error message when called without a msg param
    if (!errorMsg) return (error.style.display = "none");

    error.innerText = errorMsg;
    error.style.display = "block";
  };

  const password = localStorage.getItem(storageName);

  if (!password) {
    // Allow to create one
    displayScreen("register");
  }

  const loadRecords = () => {
    const records = JSON.parse(localStorage.getItem("records")) || [];
    tableBody.innerHTML = "";
    records.forEach((record, index) => {
      const row = `
          <tr>
            <td>${record.name}</td>
            <td>${record.phone}</td>
            <td>${record.recipient}</td>
            <td>${record.dataAmount}</td>
            <td>${record.amountDue}</td>
            <td>${record.status}</td>
            <td>
              <button onclick="markAsPaid(${index})">Mark Paid</button>
              <button onclick="deleteRecord(${index})">Delete</button>
            </td>
          </tr>`;
      tableBody.insertAdjacentHTML("beforeend", row);
    });
  };

  const saveRecord = (record) => {
    const records = JSON.parse(localStorage.getItem("records")) || [];
    records.push(record);
    localStorage.setItem("records", JSON.stringify(records));
  };

  const markAsPaid = (index) => {
    const records = JSON.parse(localStorage.getItem("records")) || [];
    records[index].status = "Paid";
    localStorage.setItem("records", JSON.stringify(records));
    loadRecords();
  };

  const deleteRecord = (index) => {
    const records = JSON.parse(localStorage.getItem("records")) || [];
    records.splice(index, 1);
    localStorage.setItem("records", JSON.stringify(records));
    loadRecords();
  };

  window.markAsPaid = markAsPaid;
  window.deleteRecord = deleteRecord;

  form.addEventListener("submit", (e) => {
    e.preventDefault();
    const record = {
      name: form.name.value,
      phone: form.phone.value,
      recipient: form.recipient.value,
      dataAmount: form.dataAmount.value,
      amountDue: form.amountDue.value,
      status: "Unpaid"
    };
    saveRecord(record);
    form.reset();
    loadRecords();
  });

  registerForm.addEventListener("submit", (e) => {
    e.preventDefault();

    const pass = registerForm.password.value;
    const passConfirm = registerForm.passwordConfirmation.value;

    if (pass !== passConfirm) {
      return setError("register", "Password does not match");
    }

    localStorage.setItem(storageName, pass);

    setError(); // Hide the error field;
    registerForm.reset(); // Reset the form
    displayScreen("login");
  });

  loginForm.addEventListener("submit", (e) => {
    e.preventDefault();

    const pass = loginForm.loginPassword.value;
    const currPass = localStorage.getItem("ddlt-password");

    if (pass !== currPass) {
      return setError("login", "Incorrect password!");
    }

    setError(); // Hide the error field;
    loginForm.reset(); // Reset the form
    displayScreen("landing");
  });

  loadRecords();
});
