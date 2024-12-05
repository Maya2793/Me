//ass1-1]audio

import java.util.Scanner;
interface MediaPlayer {
    void play(String audioType, String fileName);}
interface AdvancedMediaPlayer {
    void playMp4(String fileName);
    void playVlc(String fileName);}
class Mp4Player implements AdvancedMediaPlayer {
    public void playMp4(String fileName) {
        System.out.println("Playing MP4 file. Name: " + fileName); }
public void playVlc(String fileName) {}}
class VlcPlayer implements AdvancedMediaPlayer {
public void playMp4(String fileName) {    }
public void playVlc(String fileName) {
        System.out.println("Playing VLC file. Name: " + fileName);}}
class MediaAdapter implements MediaPlayer {
    private AdvancedMediaPlayer advancedMediaPlayer;
  public MediaAdapter(String audioType) {
        if (audioType.equalsIgnoreCase("mp4")) {
            advancedMediaPlayer = new Mp4Player();
        } else if (audioType.equalsIgnoreCase("vlc")) {
            advancedMediaPlayer = new VlcPlayer();  } }
  public void play(String audioType, String fileName) {
        if (audioType.equalsIgnoreCase("mp4")) {
            advancedMediaPlayer.playMp4(fileName);
        } else if (audioType.equalsIgnoreCase("vlc")) {
            advancedMediaPlayer.playVlc(fileName);} }}
class AudioPlayer implements MediaPlayer {
    private MediaAdapter mediaAdapter;
 public void play(String audioType, String fileName) {
        if (audioType.equalsIgnoreCase("mp3")) {
            System.out.println("Playing MP3 file. Name: " + fileName);
        } else if (audioType.equalsIgnoreCase("mp4") || audioType.equalsIgnoreCase("vlc")) {
            mediaAdapter = new MediaAdapter(audioType);
            mediaAdapter.play(audioType, fileName);
        } else {
            System.out.println("Invalid media type: " + audioType + ". Supported formats are MP3, MP4, VLC.");}}}
public class AdapterPattern {
    public static void main(String[] args) {
        AudioPlayer audioPlayer = new AudioPlayer();
        Scanner scanner = new Scanner(System.in);
System.out.println("Enter the file type (mp3, mp4, vlc):");
        String audioType = scanner.nextLine();
  System.out.println("Enter the file name:");
        String fileName = scanner.nextLine();
  audioPlayer.play(audioType, fileName);
        scanner.close();}}




        //ass1-2

        package Assignment1;
import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

// Component
interface Employee {
    void showDetails();
}

// Leaf - Individual Employee
class Worker implements Employee {
    private String name;
    private String position;

    public Worker(String name, String position) {
        this.name = name;
        this.position = position;
    }

    @Override
    public void showDetails() {
        System.out.println("Employee Name: " + name + ", Position: " + position);
    }
}

// Composite - Manager
class Manager implements Employee {
    private String name;
    private String position;
    private List<Employee> employees = new ArrayList<>();

    public Manager(String name, String position) {
        this.name = name;
        this.position = position;
    }

    public void addEmployee(Employee employee) {
        employees.add(employee);
    }

    public void removeEmployee(Employee employee) {
        employees.remove(employee);
    }

    @Override
    public void showDetails() {
        System.out.println("Manager Name: " + name + ", Position: " + position);
        System.out.println("Managing:");
        for (Employee employee : employees) {
            employee.showDetails();
        }
    }
}

// Main Class with User Input
public class CompositePatternDemo {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        List<Employee> allEmployees = new ArrayList<>();

        System.out.println("Welcome to the Employee Management System!");

        // Loop to enter employees and managers
        while (true) {
            System.out.print("Enter type of employee (worker/manager/exit): ");
            String type = scanner.nextLine().trim().toLowerCase();

            if (type.equals("exit")) {
                break;
            }

            System.out.print("Enter employee name: ");
            String name = scanner.nextLine();

            System.out.print("Enter employee position: ");
            String position = scanner.nextLine();

            if (type.equals("worker")) {
                Worker worker = new Worker(name, position);
                allEmployees.add(worker);
            } else if (type.equals("manager")) {
                Manager manager = new Manager(name, position);
                allEmployees.add(manager);

                // Manager can have subordinates (employees)
                while (true) {
                    System.out.print("Does the manager have a subordinate (yes/no)? ");
                    String hasSubordinate = scanner.nextLine().trim().toLowerCase();
                    if (hasSubordinate.equals("no")) {
                        break;
                    }

                    System.out.print("Enter subordinate name: ");
                    String subName = scanner.nextLine();

                    System.out.print("Enter subordinate position: ");
                    String subPosition = scanner.nextLine();

                    Worker subordinate = new Worker(subName, subPosition);
                    manager.addEmployee(subordinate);
                }
            } else {
                System.out.println("Invalid input. Please enter 'worker' or 'manager'.");
            }
        }

        // Display all employees
        System.out.println("\nAll Employees and Managers:");
        for (Employee employee : allEmployees) {
            employee.showDetails();
        }

        scanner.close();
    }
}

