import java.util.ArrayList;
import java.util.HashMap;
import java.util.Map;
import java.util.Scanner;

// Room class
class Room {
    private int roomNumber;
    private boolean isAvailable;

    public Room(int roomNumber) {
        this.roomNumber = roomNumber;
        this.isAvailable = true;
    }

    public int getRoomNumber() {
        return roomNumber;
    }

    public boolean isAvailable() {
        return isAvailable;
    }

    public void setAvailable(boolean available) {
        isAvailable = available;
    }
}

// Student class
class Student {
    private String name;
    private int id;

    public Student(String name, int id) {
        this.name = name;
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public int getId() {
        return id;
    }
}

// HostelManagement class
class HostelManagement {
    private ArrayList<Room> rooms = new ArrayList<>();
    private Map<Integer, Student> students = new HashMap<>();
    private Map<Integer, Integer> roomAssignments = new HashMap<>();

    // Add a new room
    public void addRoom(int roomNumber) {
        rooms.add(new Room(roomNumber));
        System.out.println("Room " + roomNumber + " added.");
    }

    // Register a new student
    public void addStudent(String name, int id) {
        students.put(id, new Student(name, id));
        System.out.println("Student " + name + " with ID " + id + " added.");
    }

    // Assign a room to a student
    public void assignRoom(int studentId, int roomNumber) {
        Room room = findRoom(roomNumber);
        if (room == null) {
            System.out.println("Room " + roomNumber + " does not exist.");
            return;
        }
        if (!room.isAvailable()) {
            System.out.println("Room " + roomNumber + " is not available.");
            return;
        }
        if (!students.containsKey(studentId)) {
            System.out.println("Student with ID " + studentId + " does not exist.");
            return;
        }
        room.setAvailable(false);
        roomAssignments.put(studentId, roomNumber);
        System.out.println("Room " + roomNumber + " assigned to student ID " + studentId + ".");
    }

    // Check out a student
    public void checkOut(int studentId) {
        if (!roomAssignments.containsKey(studentId)) {
            System.out.println("Student ID " + studentId + " is not checked in.");
            return;
        }
        int roomNumber = roomAssignments.remove(studentId);
        Room room = findRoom(roomNumber);
        if (room != null) {
            room.setAvailable(true);
        }
        System.out.println("Student ID " + studentId + " checked out of room " + roomNumber + ".");
    }

    // Find a room by number
    private Room findRoom(int roomNumber) {
        for (Room room : rooms) {
            if (room.getRoomNumber() == roomNumber) {
                return room;
            }
        }
        return null;
    }

    // List all rooms and their availability
    public void listRooms() {
        for (Room room : rooms) {
            System.out.println("Room Number: " + room.getRoomNumber() + ", Available: " + room.isAvailable());
        }
    }

    // List all students and their room assignments
    public void listStudents() {
        for (Map.Entry<Integer, Integer> entry : roomAssignments.entrySet()) {
            int studentId = entry.getKey();
            int roomNumber = entry.getValue();
            Student student = students.get(studentId);
            System.out.println("Student: " + student.getName() + ", ID: " + studentId + ", Room Number: " + roomNumber);
        }
    }
}

// Main class
public class HostelManagementSystem {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        HostelManagement management = new HostelManagement();

        while (true) {
            System.out.println("\nHostel Management System");
            System.out.println("1. Add Room");
            System.out.println("2. Add Student");
            System.out.println("3. Assign Room");
            System.out.println("4. Check Out");
            System.out.println("5. List Rooms");
            System.out.println("6. List Students");
            System.out.println("7. Exit");

            int choice = scanner.nextInt();
            scanner.nextLine();  // Consume newline

            switch (choice) {
                case 1:
                    System.out.print("Enter room number: ");
                    int roomNumber = scanner.nextInt();
                    management.addRoom(roomNumber);
                    break;

                case 2:
                    System.out.print("Enter student name: ");
                    String name = scanner.nextLine();
                    System.out.print("Enter student ID: ");
                    int id = scanner.nextInt();
                    management.addStudent(name, id);
                    break;

                case 3:
                    System.out.print("Enter student ID: ");
                    int studentId = scanner.nextInt();
                    System.out.print("Enter room number: ");
                    int assignedRoom = scanner.nextInt();
                    management.assignRoom(studentId, assignedRoom);
                    break;

                case 4:
                    System.out.print("Enter student ID: ");
                    int checkoutId = scanner.nextInt();
                    management.checkOut(checkoutId);
                    break;

                case 5:
                    management.listRooms();
                    break;

                case 6:
                    management.listStudents();
                    break;

                case 7:
                    System.out.println("Exiting...");
                    scanner.close();
                    return;

                default:
                    System.out.println("Invalid choice. Please try again.");
            }
        }
    }
}
