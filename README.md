# Medical Appointment System — FastAPI

A fully functional REST API backend for managing doctors, scheduling appointments,
and tracking medical consultations — built as part of the **Innomatics Research Labs
FastAPI Internship (Feb 2026)**.

---

## Project Structure

```
medical-appointment-system/
├── main.py               ← All 20 API endpoints (single file)
├── requirements.txt      ← Project dependencies
├── README.md             ← This file
├── .gitignore            ← Ignore venv, cache, .env
└── screenshots/          ← Swagger UI test screenshots
    ├── q01_home_route.png
    ├── q02_all_doctors.png
    ├── q03_doctor_valid.png
    ├── q03_doctor_404.png
    ├── q04_appointments.png
    ├── q05_summary.png
    ├── q06_validation_422.png
    ├── q07_helper_functions.png
    ├── q08_book_appointment.png
    ├── q09_senior_discount.png
    ├── q10_filter_spec.png
    ├── q10_filter_fee.png
    ├── q11_add_doctor_201.png
    ├── q11_duplicate_400.png
    ├── q12_update_doctor.png
    ├── q12_update_404.png
    ├── q13_delete_success.png
    ├── q13_delete_blocked.png
    ├── q14_confirm.png
    ├── q14_cancel.png
    ├── q15_complete.png
    ├── q15_active.png
    ├── q15_by_doctor.png
    ├── q16_search_results.png
    ├── q16_search_noresult.png
    ├── q17_sort_fee_asc.png
    ├── q17_sort_exp_desc.png
    ├── q18_page1.png
    ├── q18_page2.png
    ├── q19_appt_search.png
    ├── q19_appt_sort.png
    ├── q19_appt_page.png
    ├── q20_browse_all.png
    └── q20_browse_keyword.png
```

---

## Getting Started

### Prerequisites
- Python 3.9 or higher
- pip

### Installation & Running

```bash
# 1. Clone the repository
git clone https://github.com/YOUR_USERNAME/medical-appointment-system.git
cd medical-appointment-system

# 2. Create a virtual environment
python -m venv venv

# 3. Activate the virtual environment
# On Windows:
venv\Scripts\activate
# On Mac/Linux:
source venv/bin/activate

# 4. Install dependencies
pip install -r requirements.txt

# 5. Start the server
uvicorn main:app --reload
```

### Open Swagger UI
```
http://127.0.0.1:8000/docs
```

---

## API Endpoints — All 20

### Doctors

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/` | Welcome message |
| GET | `/doctors` | All doctors with total and available count |
| GET | `/doctors/summary` | Stats: most experienced, cheapest fee, count by specialization |
| GET | `/doctors/filter` | Filter by specialization, max_fee, min_experience, is_available |
| GET | `/doctors/search` | Case-insensitive search across name and specialization |
| GET | `/doctors/sort` | Sort by fee, name, or experience_years (asc/desc) |
| GET | `/doctors/page` | Paginate doctors list with total_pages |
| GET | `/doctors/browse` | Combined: search + sort + paginate in one endpoint |
| GET | `/doctors/{doctor_id}` | Single doctor by ID — 404 if not found |
| POST | `/doctors` | Add new doctor — 201 created, reject duplicates |
| PUT | `/doctors/{doctor_id}` | Update fee and/or availability |
| DELETE | `/doctors/{doctor_id}` | Delete doctor — blocked if active appointments exist |

### Appointments

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/appointments` | All appointments with total count |
| POST | `/appointments` | Book appointment with fee calculation and senior discount |
| GET | `/appointments/active` | Only scheduled or confirmed appointments |
| GET | `/appointments/search` | Search by patient name (case-insensitive) |
| GET | `/appointments/sort` | Sort by fee or date |
| GET | `/appointments/page` | Paginate appointments with total_pages |
| GET | `/appointments/by-doctor/{doctor_id}` | All appointments for a specific doctor |
| POST | `/appointments/{id}/confirm` | Change status to confirmed |
| POST | `/appointments/{id}/cancel` | Cancel and mark doctor available again |
| POST | `/appointments/{id}/complete` | Mark appointment as completed |

---

## Key Features

### Fee Calculation (Helper Function)
```
in-person  → 100% of base fee
video      → 80% of base fee
emergency  → 150% of base fee
senior citizen → extra 15% discount applied after type calculation
```

**Example:** Doctor fee = ₹300, video appointment, senior citizen
```
300 × 0.80 = 240  (video discount)
240 × 0.85 = 204  (senior citizen 15% off)
→ original_fee: 240, final_fee: 204
```

### Multi-step Workflow (Day 5)
```
Book appointment → status: "scheduled"
     ↓
Confirm          → status: "confirmed"
     ↓
Complete         → status: "completed"  (doctor freed)
  OR
Cancel           → status: "cancelled"  (doctor freed)
```

### Business Rules
- Cannot delete a doctor with active (scheduled) appointments
- Cannot confirm an already confirmed/cancelled appointment
- Doctor availability updates automatically on booking and cancellation

---

## Screenshots

All 20 questions tested and verified in Swagger UI at `http://127.0.0.1:8000/docs`.  
See the `screenshots/` folder for all test results.

---

## Tech Stack

- **Python** 3.11
- **FastAPI** 0.115.0
- **Uvicorn** 0.30.6 (ASGI server)
- **Pydantic** v2 (data validation)

---

## Author

**Sujal Rajendra Shirude**  
GitHub: [@Sujal5941](https://github.com/Sujal5941)  