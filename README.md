/* ===========================================
   OrthoNow Consultation Form
=========================================== */

// Initialize Google Tag Manager dataLayer
window.dataLayer = window.dataLayer || [];

// DOM Elements
const form = document.getElementById("consultationForm");
const formBox = document.getElementById("formBox");
const thankYou = document.getElementById("thankyou");

const nameInput = document.getElementById("name");
const phoneInput = document.getElementById("phone");
const clinicInput = document.getElementById("clinic");

const error = document.getElementById("error");

/* ===========================================
   Helper Functions
=========================================== */

// Show error message
function showError(message) {
    error.textContent = message;
}

// Clear error message
function clearError() {
    error.textContent = "";
}

// Validate Name
function validateName(name) {
    return name.trim().length >= 3;
}

// Validate Phone Number (10 digits)
function validatePhone(phone) {
    return /^[6-9]\d{9}$/.test(phone);
}

// Validate Clinic
function validateClinic(clinic) {
    return clinic !== "";
}

/* ===========================================
   Form Submit
=========================================== */

form.addEventListener("submit", function (e) {

    e.preventDefault();

    clearError();

    const name = nameInput.value.trim();
    const phone = phoneInput.value.trim();
    const clinic = clinicInput.value;

    // Name Validation
    if (!validateName(name)) {
        showError("Please enter a valid full name.");
        nameInput.focus();
        return;
    }

    // Phone Validation
    if (!validatePhone(phone)) {
        showError("Please enter a valid 10-digit mobile number.");
        phoneInput.focus();
        return;
    }

    // Clinic Validation
    if (!validateClinic(clinic)) {
        showError("Please select your preferred clinic.");
        clinicInput.focus();
        return;
    }

    /* ===========================================
       GTM Data Layer Push
    =========================================== */

    window.dataLayer.push({

        event: "consultation_form_submitted",

        patient_name: name,

        phone_number: phone,

        clinic_preference: clinic,

        form_name: "Consultation Landing Page",

        page_location: window.location.pathname,

        submission_time: new Date().toISOString()

    });

    console.log("GTM Event Fired");
    console.log(window.dataLayer);

    /* ===========================================
       Thank You State
    =========================================== */

    formBox.style.display = "none";
    thankYou.style.display = "block";

    /* ===========================================
       Reset Form
    =========================================== */

    form.reset();

});

/* ===========================================
   Remove Error While Typing
=========================================== */

nameInput.addEventListener("input", clearError);

phoneInput.addEventListener("input", clearError);

clinicInput.addEventListener("change", clearError);

/* ===========================================
   Console Message
=========================================== */

console.log("OrthoNow Landing Page Loaded Successfully");