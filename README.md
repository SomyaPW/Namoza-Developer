/* ===========================================
   OrthoNow Landing Page
=========================================== */

// Initialize GTM Data Layer
window.dataLayer = window.dataLayer || [];

// DOM Elements
const form = document.getElementById("consultationForm");
const formBox = document.getElementById("formBox");
const thankYou = document.getElementById("thankyou");

const nameInput = document.getElementById("name");
const phoneInput = document.getElementById("phone");
const error = document.getElementById("error");

/* ===========================================
   Helper Functions
=========================================== */

function showError(message) {
    error.textContent = message;
}

function clearError() {
    error.textContent = "";
}

function validateName(name) {
    return name.trim().length >= 3;
}

function validatePhone(phone) {
    return /^[6-9]\d{9}$/.test(phone);
}

/* ===========================================
   Form Submit
=========================================== */

form.addEventListener("submit", function (e) {

    e.preventDefault();

    clearError();

    const name = nameInput.value.trim();
    const phone = phoneInput.value.trim();

    // Validate Name
    if (!validateName(name)) {
        showError("Please enter your full name.");
        nameInput.focus();
        return;
    }

    // Validate Phone
    if (!validatePhone(phone)) {
        showError("Please enter a valid 10-digit mobile number.");
        phoneInput.focus();
        return;
    }

    /* ===========================================
       GTM Data Layer Push
    =========================================== */

    window.dataLayer.push({

        event: "consultation_form_submitted",

        patient_name: name,

        phone_number: phone,

        form_name: "Consultation Landing Page",

        page_location: window.location.pathname,

        submission_time: new Date().toISOString()

    });

    console.group("GTM Event");
    console.table(window.dataLayer);
    console.groupEnd();

    // Show Thank You Message
    formBox.style.display = "none";
    thankYou.style.display = "block";

    // Reset Form
    form.reset();

});

/* ===========================================
   Clear Errors While Typing
=========================================== */

nameInput.addEventListener("input", clearError);
phoneInput.addEventListener("input", clearError);

/* ===========================================
   Page Loaded
=========================================== */

document.addEventListener("DOMContentLoaded", () => {
    console.log("OrthoNow Landing Page Loaded Successfully");
});