# GTM Event Schema

## Project Overview

**Client:** OrthoNow

**Industry:** Healthcare (Orthopaedic Clinics)

**Objective**

Implement a complete Google Tag Manager (GTM) and Google Analytics 4 (GA4) event tracking strategy for OrthoNow's website to accurately measure user interactions, optimize marketing campaigns, improve appointment booking conversions, and support data-driven decision making.

---

# Tracking Goals

The implementation will track the following user interactions:

- Appointment Booking Funnel (3-Step Form)
- Call Now Button Clicks
- WhatsApp Widget Clicks
- Patient Guide Downloads
- Clinic Location Page Views
- Blog Engagement (Scroll Depth)

The collected events will be used in:

- Google Tag Manager (GTM)
- Google Analytics 4 (GA4)
- Google Ads Conversion Tracking
- GA4 Funnel Exploration
- Audience Building & Remarketing

---

# GTM Event Schema

| Event Name | Trigger Type | Key Parameters | GA4 Report / Audience |
|------------|--------------|----------------|-----------------------|
| appointment_booking_started | Custom Event | clinic_location, specialty, page_location | Booking Funnel |
| booking_step_complete | Custom Event | step_number, step_name, clinic_location, specialty | Funnel Exploration |
| appointment_booked | Custom Event | booking_id, clinic_location, appointment_date | Conversions |
| call_now_clicked | Click - Just Links | phone_number, clinic_location, page_location | CTA Performance |
| whatsapp_clicked | Click - Just Links | clinic_location, source_page, destination | Engagement Report |
| patient_guide_downloaded | Form Submission + File Download | guide_name, clinic_location, user_type | Lead Generation |
| clinic_page_view | Page View | clinic_name, city, page_location | Page Performance |
| blog_scroll | Scroll Depth (75%) | article_title, scroll_depth, category | Content Engagement |

---

# Booking Form Funnel Tracking

The appointment booking form consists of three steps.

## Step 1

- Select Clinic Location
- Select Specialty

### dataLayer Push

```json
{
  "event": "booking_step_complete",
  "step_number": 1,
  "step_name": "location_specialty_selected",
  "clinic_location": "Bengaluru - Indiranagar",
  "specialty": "Orthopaedics"
}
```

---

## Step 2

- Enter Name
- Enter Phone Number
- Select Preferred Date

### dataLayer Push

```json
{
  "event": "booking_step_complete",
  "step_number": 2,
  "step_name": "patient_details_entered",
  "patient_name": "John Doe",
  "phone_number": "9876543210",
  "preferred_date": "2026-07-10"
}
```

---

## Step 3

- Review Booking
- Confirm Appointment

### dataLayer Push

```json
{
  "event": "booking_step_complete",
  "step_number": 3,
  "step_name": "booking_confirmed",
  "booking_id": "BK123456",
  "clinic_location": "Bengaluru - Indiranagar",
  "appointment_status": "confirmed"
}
```

---

## Final Appointment Conversion Event

After the booking has been successfully saved in the backend, a separate conversion event should be fired.

### dataLayer Push

```json
{
  "event": "appointment_booked",
  "booking_id": "BK123456",
  "clinic_location": "Bengaluru - Indiranagar",
  "specialty": "Orthopaedics",
  "appointment_date": "2026-07-10"
}
```

---

# Funnel Drop-off Tracking

The booking funnel is tracked using the `booking_step_complete` event together with the `step_number` parameter.

## Funnel Steps

```text
Booking Started
        │
        ▼
Step 1
Location & Specialty Selected
        │
        ▼
Step 2
Patient Details Entered
        │
        ▼
Step 3
Booking Confirmed
        │
        ▼
Appointment Booked
```

## GA4 Funnel Exploration Configuration

| Funnel Step | Event | Filter |
|-------------|-------|--------|
| Booking Started | appointment_booking_started | event_name = appointment_booking_started |
| Step 1 | booking_step_complete | step_number = 1 |
| Step 2 | booking_step_complete | step_number = 2 |
| Step 3 | booking_step_complete | step_number = 3 |
| Completed Booking | appointment_booked | event_name = appointment_booked |

This funnel allows the marketing team to identify where users abandon the booking process and optimize the user experience to improve conversion rates.

---

# Google Ads Conversion

## Recommended Conversion Event

**Conversion Event**

```
appointment_booked
```

## Reason

The `appointment_booked` event represents a successfully completed consultation booking and directly reflects the primary business objective.

Earlier events such as booking starts, button clicks, or WhatsApp clicks indicate user interest but do not guarantee a qualified lead.

Using the completed booking event enables Google Ads Smart Bidding to optimize campaigns toward actual appointment bookings rather than intermediate interactions.

---

# Notes for Frontend Developer

The booking form is a custom multi-step component.

Google Tag Manager cannot automatically detect internal step transitions.

The frontend application must fire `window.dataLayer.push()` whenever a booking step is successfully completed.

Example implementation:

```javascript
window.dataLayer = window.dataLayer || [];

window.dataLayer.push({
    event: "booking_step_complete",
    step_number: 1,
    step_name: "location_specialty_selected",
    clinic_location: selectedClinic,
    specialty: selectedSpecialty
});
```

GTM listens for these custom events and forwards them to:

- Google Analytics 4
- Google Ads
- Other marketing platforms if required

This approach ensures accurate funnel tracking and conversion measurement.