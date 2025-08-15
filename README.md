import pandas as pd
import random
from faker import Faker

fake = Faker()

# Parameters
num_records = 500_000
countries = ["USA", "Canada", "UK", "Australia"]
states = ["Texas", "California", "New York", "Florida", "Ontario", "Quebec", "London", "Sydney"]
genders = ["Male", "Female", "Other"]
specializations = ["Cardiology", "Neurology", "Orthopedics", "Pediatrics", "Oncology", "Dermatology"]
hospital_types = ["General", "Specialty", "Teaching", "Research"]
accreditations = ["JCI", "ISO", "NABH", "None"]
room_types = ["ICU", "General Ward", "Private Room", "Semi-Private"]
units = ["ICU Unit", "Surgical Unit", "Maternity Unit", "ER Unit"]
shift_names = ["Morning", "Evening", "Night"]
diagnosis_categories = ["Chronic", "Acute", "Infectious", "Genetic"]
insurance_plans = ["Gold Plan", "Silver Plan", "Platinum Plan", "Basic Plan"]

data = []
for i in range(1, num_records + 1):
    # Patient
    patient_id = f"P{i:06d}"
    patient_name = fake.name()
    gender = random.choice(genders)
    dob = fake.date_of_birth(minimum_age=1, maximum_age=100)
    age = 2025 - dob.year
    patient_address = fake.street_address()
    patient_city = random.choice(states)
    patient_state = random.choice(states)
    patient_country = random.choice(countries)
    postal_code = fake.postcode()
    admission_date = fake.date_between(start_date="-1y", end_date="today")
    discharge_date = fake.date_between(start_date=admission_date, end_date="+5d")
    wait_time = f"{random.randint(5, 120)} mins"

    # Doctor
    doctor_id = f"D{random.randint(1, 500):04d}"
    doctor_name = f"Dr. {fake.name()}"
    specialization = random.choice(specializations)
    doctor_contact = fake.phone_number()
    years_exp = random.randint(1, 40)
    doctor_email = fake.email()
    doctor_shift = random.choice(shift_names)

    # Department
    dept_id = f"DEP{random.randint(1, 50):03d}"
    dept_name = f"{specialization} Dept"
    dept_speciality = specialization
    floor_num = random.randint(1, 10)

    # Hospital
    hospital_id = f"H{random.randint(1, 50):03d}"
    hospital_name = fake.company() + " Hospital"
    hospital_type = random.choice(hospital_types)
    accreditation = random.choice(accreditations)
    bed_capacity = random.randint(50, 1000)
    hospital_address = fake.street_address()
    hospital_state = random.choice(states)
    hospital_city = random.choice(states)
    hospital_country = random.choice(countries)
    hospital_postal = fake.postcode()

    # Room
    room_id = f"R{random.randint(1, 200):04d}"
    room_number = str(random.randint(1, 500))
    room_type = random.choice(room_types)
    room_floor = random.randint(1, 10)

    # Unit
    unit_id = f"U{random.randint(1, 100):03d}"
    unit_name = random.choice(units)

    # Shift
    shift_id = f"S{random.randint(1, 20):03d}"
    shift_name = random.choice(shift_names)
    start_time = random.choice(["08:00", "14:00", "20:00"])
    end_time = "14:00" if start_time == "08:00" else ("20:00" if start_time == "14:00" else "08:00")

    # Billing
    total_billing = random.randint(500, 20000)
    insurance_covered = int(total_billing * random.uniform(0.5, 0.9))
    patient_covered = total_billing - insurance_covered

    # Date
    full_date = admission_date

    # Diagnosis
    diagnosis_id = f"DG{random.randint(1, 500):04d}"
    diagnosis_code = fake.lexify(text="???##")
    diagnosis_description = fake.sentence(nb_words=3)
    diagnosis_category = random.choice(diagnosis_categories)

    # Insurance
    insurance_id = f"IP{random.randint(1, 200):04d}"
    insurance_name = fake.company()
    plan_type = random.choice(insurance_plans)
    coverage_percentage = random.randint(50, 100)
    contact_number = fake.phone_number()
    insurance_email = fake.email()

    data.append([
        i, patient_id, patient_name, gender, age, dob, patient_address, patient_city, patient_state, patient_country, postal_code,
        admission_date, discharge_date, wait_time, doctor_id, doctor_name, specialization, doctor_contact, years_exp, doctor_email,
        doctor_shift, dept_id, dept_name, dept_speciality, floor_num, hospital_id, hospital_name, hospital_type, accreditation,
        bed_capacity, hospital_address, hospital_state, hospital_city, hospital_country, hospital_postal, room_id, room_number,
        room_type, room_floor, unit_id, unit_name, shift_id, shift_name, start_time, end_time, total_billing, insurance_covered,
        patient_covered, full_date, diagnosis_id, diagnosis_code, diagnosis_description, diagnosis_category, insurance_id,
        insurance_name, plan_type, coverage_percentage, contact_number, insurance_email
    ])

# Column names
columns = [
    "Visit_ID", "Patient_ID", "Patient_FullName", "Patient_Gender", "Age", "Patient_DOB", "Patient_Address", "Patient_City", "Patient_State",
    "Patient_Country", "Patient_PostalCode", "Patient_AdmissionDate", "Patient_DischargeDate", "Patient_WaitTime", "Doctor_ID", "Doctor_FullName",
    "Doctor_Specialization", "Doctor_ContactNumber", "Doctor_YearsExperience", "Doctor_Email", "Doctor_ShiftType", "Department_ID", "Department_Name",
    "Dept_Speciality", "Floor_Number", "Hospital_ID", "Hospital_Name", "Hospital_Type", "Accreditation_Level", "Bed_Capacity", "Hospital_Address",
    "Hospital_State", "Hospital_City", "Hospital_Country", "Hospital_PostalCode", "Room_ID", "Room_Number", "Room_Type", "Room_FloorNumber",
    "Unit_ID", "Unit_Name", "Shift_ID", "Shift_Name", "Shift_StartTime", "Shift_EndTime", "Total_Billing_Amount", "Insurance_Covered_Amount",
    "Patient_Covered_Amount", "Full_Date", "Diagnosis_ID", "Diagnosis_Code", "Diagnosis_Description", "Diagnosis_Category", "InsuranceProvider_ID",
    "InsuranceProvider_Name", "InsurancePlan_Type", "Coverage_Percentage", "Insurance_ContactNumber", "Insurance_Email"
]

# Create DataFrame
df = pd.DataFrame(data, columns=columns)

# Save to CSV
df.to_csv("fact_visits_500k.csv", index=False)

print("CSV file 'fact_visits_500k.csv' created successfully!")
