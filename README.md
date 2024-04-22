# Task-from-Schedula-Services
import java.util.*;

class Doctor {
    private int id;
    private String name;
    private String specialty;
    private List<String> availability;

    public Doctor(int id, String name, String specialty, List<String> availability) {
        this.id = id;
        this.name = name;
        this.specialty = specialty;
        this.availability = availability;
    }

    // Getters and setters
    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getSpecialty() {
        return specialty;
    }

    public void setSpecialty(String specialty) {
        this.specialty = specialty;
    }

    public List<String> getAvailability() {
        return availability;
    }

    public void setAvailability(List<String> availability) {
        this.availability = availability;
    }
}

class Appointment {
    private int id;
    private Doctor doctor;
    private String patientName;
    private String appointmentTime;

    public Appointment(int id, Doctor doctor, String patientName, String appointmentTime) {
        this.id = id;
        this.doctor = doctor;
        this.patientName = patientName;
        this.appointmentTime = appointmentTime;
    }

    // Getters and setters
    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public Doctor getDoctor() {
        return doctor;
    }

    public void setDoctor(Doctor doctor) {
        this.doctor = doctor;
    }

    public String getPatientName() {
        return patientName;
    }

    public void setPatientName(String patientName) {
        this.patientName = patientName;
    }

    public String getAppointmentTime() {
        return appointmentTime;
    }

    public void setAppointmentTime(String appointmentTime) {
        this.appointmentTime = appointmentTime;
    }
}

class AppointmentSystem {
    private List<Doctor> doctors;
    private List<Appointment> appointments;
    private int maxPatients;

    public AppointmentSystem(int maxPatients) {
        this.doctors = new ArrayList<>();
        this.appointments = new ArrayList<>();
        this.maxPatients = maxPatients;
        // Initialize doctors and their availability
        // For simplicity, let's assume one doctor practicing on evenings (Mon-Sat)
        List<String> availability = Arrays.asList("Monday 5:00 PM", "Tuesday 5:00 PM", "Wednesday 5:00 PM",
                "Thursday 5:00 PM", "Friday 5:00 PM", "Saturday 5:00 PM");
        Doctor doctor = new Doctor(1, "Dr. Smith", "General Physician", availability);
        this.doctors.add(doctor);
    }

    public List<Doctor> getDoctors() {
        return doctors;
    }

    public Doctor getDoctorById(int id) {
        for (Doctor doctor : doctors) {
            if (doctor.getId() == id) {
                return doctor;
            }
        }
        return null;
    }

    public List<String> getDoctorAvailability(int id) {
        Doctor doctor = getDoctorById(id);
        if (doctor != null) {
            return doctor.getAvailability();
        }
        return null;
    }

    public boolean bookAppointment(int doctorId, String patientName, String appointmentTime) {
        Doctor doctor = getDoctorById(doctorId);
        if (doctor != null && doctor.getAvailability().contains(appointmentTime)) {
            // Check if doctor has reached max patients for the day
            int count = (int) appointments.stream().filter(a -> a.getDoctor().getId() == doctorId).count();
            if (count < maxPatients) {
                Appointment appointment = new Appointment(appointments.size() + 1, doctor, patientName, appointmentTime);
                appointments.add(appointment);
                return true;
            }
        }
        return false;
    }
}

public class Main {
    public static void main(String[] args) {
        AppointmentSystem appointmentSystem = new AppointmentSystem(5); // Assuming maximum 5 patients per doctor per day

        // Sample usage
        System.out.println("Available Doctors:");
        List<Doctor> doctors = appointmentSystem.getDoctors();
        for (Doctor doctor : doctors) {
            System.out.println(doctor.getName() + " - " + doctor.getSpecialty());
        }

        System.out.println("\nDoctor Detail:");
        Doctor doctor = appointmentSystem.getDoctorById(1);
        if (doctor != null) {
            System.out.println("Name: " + doctor.getName());
            System.out.println("Specialty: " + doctor.getSpecialty());
            System.out.println("Availability: " + doctor.getAvailability());
        }

        System.out.println("\nDoctor Availability:");
        List<String> availability = appointmentSystem.getDoctorAvailability(1);
        if (availability != null) {
            for (String slot : availability) {
                System.out.println(slot);
            }
        }

        // Booking an appointment
        boolean booked = appointmentSystem.bookAppointment(1, "John Doe", "Monday 5:00 PM");
        if (booked) {
            System.out.println("\nAppointment booked successfully!");
        } else {
            System.out.println("\nFailed to book appointment. Please try again later.");
        }
    }
}

