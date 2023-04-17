# assignment-2
The user provided an explanation and UML class diagram of a dental company's software system. The diagram shows classes such as DentalCompany, DentalBranch, Staff, Patient, Appointment, Service, and Bill, and their relationships to each other. The user also provided assumptions about attributes and a description of the relationships in the diagram.

This is the code with its test cases and documented with comments on each line.

import datetime  # import datetime module

class Staff:  # create a Staff class
    def __init__(self, id, name, service_type, salary):  # create constructor for the Staff class
        self.id = id  # set id attribute to id parameter
        self.name = name  # set name attribute to name parameter
        self.service_type = service_type  # set service_type attribute to service_type parameter
        self.salary = salary  # set salary attribute to salary parameter

class Manager(Staff):  # create a Manager class that inherits from Staff class
    pass

class Receptionist(Staff):  # create a Receptionist class that inherits from Staff class
    pass

class Hygienist(Staff):  # create a Hygienist class that inherits from Staff class
    pass

class Dentist(Staff):  # create a Dentist class that inherits from Staff class
    pass

class Patient:  # create a Patient class
    def __init__(self, id, name, phone_number, email):  # create constructor for the Patient class
        self.id = id  # set id attribute to id parameter
        self.name = name  # set name attribute to name parameter
        self.phone_number = phone_number  # set phone_number attribute to phone_number parameter
        self.email = email  # set email attribute to email parameter
        self.appointments = []  # create an empty list of appointments for the patient
        
    # Schedule a new appointment with a patient, staff and list of services
    def schedule_appointment(self, appointment_time, patient, staff, services):
        # Create a new appointment with an ID, date, time, patient, staff and list of services
        appointment = Appointment(len(self.appointments)+1, appointment_time.date(), appointment_time.time(), patient, staff, services)
        # Add the new appointment to the list of appointments
        self.appointments.append(appointment)
        # Print a message confirming the scheduling of the appointment
        print(f"Appointment {appointment.id} scheduled for {appointment.date} at {appointment.time}")
        
    # Return the list of appointments
    def get_appointments(self):
        return self.appointments

    # Cancel an appointment
    def cancel_appointment(self, appointment):
        self.appointments.remove(appointment)

class Service:  # create a Service class
    def __init__(self, name, cost):  # create constructor for the Service class
        self.name = name  # set name attribute to name parameter
        self.cost = cost  # set cost attribute to cost parameter

        
class Cleaning(Service):  # create a Cleaning class that inherits from Service class
    pass

class Implants(Service):  # create an Implants class that inherits from Service class
    pass

class Crowns(Service):  # create a Crowns class that inherits from Service class
    pass

class Fillings(Service):  # create a Fillings class that inherits from Service class
    pass

class Appointment:  # create an Appointment class
    def __init__(self, id, date, time, patient, staff, services):  # create constructor for the Appointment class
        self.id = id  # set id attribute to id parameter
        self.date = date  # set date attribute to date parameter
        self.time = time  # set time attribute to time parameter
        self.patient = patient  # set patient attribute to patient parameter
        self.staff = staff  # set staff attribute to staff parameter
        self.services = services  # set services attribute to services parameter
        self.total_cost = sum(service.cost for service in services)  # calculate total cost of services and set to total_cost attribute

    def add_service(self, service):  # create add_service method to add a service to the appointment
        self.services.append(service)  # append service to the services list

    def get_services(self):  # create get_services method to get the list of services for the appointment
        return self.services  # return the services list

    def get_id(self):  # create get_id method to get the id of the appointment
        return self.id  # return the id


# Define a class for dental services
class DentalService:
    # Initialize the list of services with Cleaning, Implants, Crowns and Fillings
    def __init__(self):
        self.services = [Cleaning("Cleaning", 50), Implants("Implants", 1500), Crowns("Crowns", 800), Fillings("Fillings", 200)]

# Define a class for the bill calculation
class Bill:
    # Define the VAT as a class variable
    VAT = 0.05

    # Define a class method to calculate VAT
    @classmethod
    def calculate_vat(cls, amount):
        return amount * cls.VAT

# Define a class for dental branches
class DentalBranch:
    # Initialize the branch with an address, phone number and manager, and empty lists for staff, patients, appointments and services
    def __init__(self, address, phone_number, manager):
        self.address = address
        self.phone_number = phone_number
        self.manager = manager
        self.staff = []
        self.patients = []
        self.appointments = []
        self.services = DentalService().services

    # Add a new service to the list of services
    def add_service(self, service):
        self.services.append(service)

    # Remove a service from the list of services
    def remove_service(self, service):
        self.services.remove(service)

    # Add a new staff member to the list of staff
    def add_staff(self, staff):
        self.staff.append(staff)

    # Remove a staff member from the list of staff
    def remove_staff(self, staff):
        self.staff.remove(staff)

    # Add a new patient to the list of patients
    def add_patient(self, patient):
        self.patients.append(patient)

    # Remove a patient from the list of patients
    def remove_patient(self, patient):
        self.patients.remove(patient)

    # Schedule a new appointment with a patient, staff and list of services
    def schedule_appointment(self, appointment_time, patient, staff, services):
        # Create a new appointment with an ID, date, time, patient, staff and list of services
        appointment = Appointment(len(self.appointments)+1, appointment_time.date(), appointment_time.time(), patient, staff, services)
        # Add the new appointment to the list of appointments
        self.appointments.append(appointment)
        # Print a message confirming the scheduling of the appointment
        print(f"Appointment {appointment.id} scheduled for {appointment.date} at {appointment.time}")
        
    # Return the list of appointments
    def get_appointments(self):
        return self.appointments

    # Cancel an appointment
    def cancel_appointment(self, appointment):
        self.appointments.remove(appointment)

    # Update the details of an appointment
    def update_appointment(self, appointment, new_date=None, new_time=None, new_services=None):
        # If a new date is specified, update the appointment date
        if new_date:
            appointment.date = new_date
        # If a new time is specified, update the appointment time
        if new_time:
            appointment.time = new_time
        # If new services are specified, update the list of services for the appointment
        if new_services:
            appointment.services = new_services
        # Calculate the new total cost for the appointment
        appointment.total_cost = sum(service.cost for service in appointment.services)

    # Generate a bill for an appointment
    def generate_bill(self, appointment):
        # Calculate the cost of the services for the appointment
        services_cost = sum(service.cost for service in appointment.services)
        # Calculate the VAT on the services cost
        vat = Bill.calculate_vat(services_cost)
        # Calculate the total cost (including VAT)
        total_cost = services_cost + vat
        # Return the formatted bill string
        return f"Appointment ID: {appointment.id}\nPatient Name: {appointment.patient.name}\nServices: {', '.join(service.name for service in appointment.services)}\nServices Cost: ${services_cost:.2f}\nVAT: ${vat:.2f}\nTotal Cost: ${total_cost:.2f}"
    
    # Generate a appointment to book
    def book_appointment(self, patient, staff, date, time, services):
        # Check if patient exists in the list of patients
        if patient not in self.patients:
            raise ValueError("Patient does not exist.")
        # Check if staff exists in the list of staff
        if staff not in self.staff:
            raise ValueError("Staff does not exist.")
        # Check if the requested appointment time is already taken
        for appointment in self.appointments:
            if appointment.date == date and appointment.time == time:
                raise ValueError("The appointment time is not available.")
        # If all checks passed, create a new appointment and add it to the list of appointments
        appointment_id = len(self.appointments) + 1
        appointment = Appointment(appointment_id, date, time, patient, staff, services)
        self.appointments.append(appointment)    

    # generates a checkout for when the patient is done from booking an appointment    
    def checkout(self, appointment):
        # Generate the bill for the appointment
        bill = self.generate_bill(appointment)
        # Print the bill
        print(bill)

#creates a class for Dental comapny
class DentalCompany:
    # Define the class constructor and set the name and branches as instance variables
    def __init__(self, name):
        # Set the name of the dental company
        self.name = name
        # Initialize an empty list of branches
        self.branches = []

    def add_branch(self, branch): # Define a method to add a branch to the list of branches of the company
        self.branches.append(branch) # Add a new branch to the list of branches

#Test cases
# create a new branch
branch1 = DentalBranch("22b St.", "050-456-7634", Manager("001", "Fatma Nasser", "Manager", 50000))

# create another new branch
branch2 = DentalBranch("456 Elm St.", "055-989-8765", Manager("002", "Khalifa Saif", "Manager", 55000))

# add the branches to the dental company
company = DentalCompany("My Dental Company")
company.add_branch(branch1)
company.add_branch(branch2)

# check if the branches have been added to the dental company
assert branch1 in company.branches
assert branch2 in company.branches

# check if the number of branches in the dental company is correct
assert len(company.branches) == 2

# create a new cleaning service
cleaning = Cleaning("Cleaning", 50)

# add the cleaning service to the services offered at the branch
branch1.add_service(cleaning)

# create a new receptionist
receptionist1 = Receptionist("003", "Meera Ali", "Receptionist", 35000)

# add the receptionist to the staff at the branch
branch1.add_staff(receptionist1)

# create a new patient
patient1 = Patient("010", "Hamdan Jaber", "056-987-7744", "7amdan1@gmail.com")

# add the patient to the patient list at the branch
branch1.add_patient(patient1)

# check if the cleaning service has been added to the services offered at the branch
assert cleaning in branch1.services

# check if the receptionist has been added to the staff at the branch
assert receptionist1 in branch1.staff

# check if the patient has been added to the patient list at the branch
assert patient1 in branch1.patients


# create a new staff member
dentist1 = Dentist("121", "Dr. Sara", "Dentist", 75000)

# create a new service
implant_service = Implants("Implants", 1500)
appointment1_time = datetime.datetime.strptime("2023-04-15 09:00", '%Y-%m-%d %H:%M')
appointment1 = Appointment(1, appointment1_time.date(), appointment1_time.time(), patient1, dentist1, [implant_service])
branch1.schedule_appointment(appointment1_time, patient1, dentist1, [implant_service])


# Check if the appointment was added to the branch's appointment list
appointment_ids = [appointment.get_id() for appointment in branch1.get_appointments()]
assert appointment1.get_id() in appointment_ids

# Schedule another appointment for a different patient with a different staff member and service
patient2 = Patient("011", "Kobchoi", "050-997-2344", "kobchoi9@gmail.com")
hygienist1 = Hygienist("034", "Zainab Rashed", "Hygienist", 50000)
cleaning_service = Cleaning("Teeth Cleaning", 50)
appointment_time = datetime.datetime.strptime("2023-04-16 10:00", '%Y-%m-%d %H:%M')
branch1.schedule_appointment(appointment_time, patient2, hygienist1, [cleaning_service])


# Check if both appointments were added to the branch's appointment list
print(branch1.get_appointments())

appointment = branch1.get_appointments()[0]
branch1.checkout(appointment)


#Another extra tests

# Defines a function called test_add_branch that tests adding a branch to a Dental Company
def test_add_branch():
    # Creates a Dental Company instance called company with a name "My Dental Company"
    company = DentalCompany("My Dental Company")
    # Creates a Dental Branch instance called branch1 with a name "18e St.", phone number "042375960", and a Manager instance with ID "121", name "Hessa Essa", title "Manager", and salary 50000
    branch1 = DentalBranch("18e St.", "042375960", Manager("121", "Hessa Essa", "Manager", 50000))
    # Adds the created branch1 to the branches list of the company
    company.add_branch(branch1)
    # Asserts that the branch1 is in the branches list of the company
    assert branch1 in company.branches
    # Asserts that the length of the branches list of the company is 1
    assert len(company.branches) == 1

# Defines a function called test_add_service_staff_and_patient_to_branch that tests adding a service, staff, and patient to a Dental Branch
def test_add_service_staff_and_patient_to_branch():
    # Creates a Dental Branch instance called branch with a name "14b St.", phone number "04-5636628", and a Manager instance with ID "122", name "Meera Ali", title "Manager", and salary 50000
    branch = DentalBranch("14b St.", "04-5636628", Manager("122", "Meera Ali", "Manager", 50000))
    # Creates a Cleaning instance with name "Cleaning" and cost 50
    cleaning = Cleaning("Cleaning", 50)
    # Creates a Receptionist instance with ID "035", name "Maryam Nasser", title "Receptionist", and salary 35000
    receptionist = Receptionist("035", "Maryam Nasser", "Receptionist", 35000)
    # Creates a Patient instance with ID "01", name "Arrah Loma", phone number "050-435-6757", and email "ArrahLoma@gmail.com"
    patient = Patient("01", "Arrah Loma", "050-435-6757", "ArrahLoma@gmail.com")

    # Adds the created cleaning service to the services list of the branch
    branch.add_service(cleaning)
    # Adds the created receptionist staff to the staff list of the branch
    branch.add_staff(receptionist)
    # Adds the created patient to the patients list of the branch
    branch.add_patient(patient)

    # Asserts that the created cleaning service is in the services list of the branch
    assert cleaning in branch.services
    # Asserts that the created receptionist staff is in the staff list of the branch
    assert receptionist in branch.staff
    # Asserts that the created patient is in the patients list of the branch
    assert patient in branch.patients

# Define a function called test_schedule_appointment that tests scheduling an appointment to a Dental Branch
def test_schedule_appointment():
    # Create a DentalBranch object with an address, phone number, and Manager object
    branch = DentalBranch("14b St.", "04-5636628", Manager("122", "Meera Ali", "Manager", 50000))
    # Create a Patient object with an ID, name, phone number, and email
    patient = Patient("01", "Arrah Loma", "050-435-6757", "ArrahLoma@gmail.com")
    # Create a Dentist object with an ID, name, title, and salary
    dentist = Dentist("149", "Dr. Rachel", "Dentist", 75000)
    # Create an Implants object with a service name and price
    implant_service = Implants("Implants", 1500)
    # Create a datetime object for the appointment time using strptime to parse the string
    appointment_time = datetime.datetime.strptime("2023-04-15 09:00", '%Y-%m-%d %H:%M')
    # Create an Appointment object with an ID, date, time, patient, dentist, and services
    appointment = Appointment(1, appointment_time.date(), appointment_time.time(), patient, dentist, [implant_service])

    # Schedule the appointment to the DentalBranch using the appointment time, patient, dentist, and services
    branch.schedule_appointment(appointment_time, patient, dentist, [implant_service])

    # Assert that the appointment ID is in the list of appointment IDs returned by the DentalBranch's get_appointments method
    assert appointment.get_id() in [appointment.get_id() for appointment in branch.get_appointments()]

# Define a function called test_checkout that tests checking out an appointment at a Dental Branch
def test_checkout():
    # Create a DentalBranch object with an address, phone number, and Manager object
    branch = DentalBranch("14b St.", "04-5636628", Manager("122", "Meera Ali", "Manager", 50000))
    # Create a Patient object with an ID, name, phone number, and email
    patient = Patient("01", "Arrah Loma", "050-435-6757", "ArrahLoma@gmail.com")
    # Create a Dentist object with an ID, name, title, and salary
    dentist = Dentist("149", "Dr. Rachel", "Dentist", 75000)
    # Create an Implants object with a service name and price
    implant_service = Implants("Implants", 1500)
    # Create a datetime object for the appointment time using strptime to parse the string
    appointment_time = datetime.datetime.strptime("2023-04-15 09:00", '%Y-%m-%d %H:%M')
    # Create an Appointment object with an ID, date, time, patient, dentist, and services
    appointment = Appointment(1, appointment_time.date(), appointment_time.time(), patient, dentist, [implant_service])

    # Schedule the appointment to the DentalBranch using the appointment time, patient, dentist, and services
    branch.schedule_appointment(appointment_time, patient, dentist, [implant_service])
    # Check out the appointment by paying for it at the DentalBranch
    branch.checkout(appointment)

    # Assert that the appointment is paid by checking the is_paid attribute of the Appointment object
    assert appointment.is_paid() == True



This is what it prints:

Appointment 1 scheduled for 2023-04-15 at 09:00:00
Appointment 2 scheduled for 2023-04-16 at 10:00:00
Appointment ID: 1
Patient Name: Hamdan Jaber
Services: Implants
Services Cost: $1500.00
VAT: $75.00
Total Cost: $1575.00
