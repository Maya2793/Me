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
interface Employee {
    void showDetails();
}
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
public class CompositePatternDemo {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        List<Employee> allEmployees = new ArrayList<>();
        System.out.println("Welcome to the Employee Management System!");
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
        System.out.println("\nAll Employees and Managers:");
        for (Employee employee : allEmployees) {
            employee.showDetails();
        }
scanner.close();
    }
}



//ass1 bridge


package Assignment1;
import java.util.Scanner;
interface DrawingAPI { void drawCircle(double x, double y, double radius); void drawRectangle(double x, double y, double width, double height);}
class DrawingAPI1 implements DrawingAPI {
@Override
public void drawCircle(double x, double y, double radius) {
System.out.println("Drawing Circle [API1] at (" + x + ", " + y + ") with radius " + radius);
} @Override
public void drawRectangle(double x, double y, double width, double height) {
System.out.println("Drawing Rectangle [API1] at (" + x + ", " + y + ") with width " + width + " and height " + height); }
}
class DrawingAPI2 implements DrawingAPI {
 @Override   public void drawCircle(double x, double y, double radius) {
System.out.println("Drawing Circle [API2] at (" + x + ", " + y + ") with radius " + radius);
    }
   @Override
public void drawRectangle(double x, double y, double width, double height) {
 System.out.println("Drawing Rectangle [API2] at (" + x + ", " + y + ") with width " + width + " and height " + height);
    }}
abstract class Shape {
protected DrawingAPI drawingAPI;  protected Shape(DrawingAPI drawingAPI) {
this.drawingAPI = drawingAPI;
    }

 public abstract void draw();
 public abstract void resize(double factor);
}
class Circle extends Shape {
    private double x, y, radius;
    public Circle(double x, double y, double radius, DrawingAPI drawingAPI) {
        super(drawingAPI);
        this.x = x;
        this.y = y;
        this.radius = radius;
    }
    @Override
    public void draw() {
        drawingAPI.drawCircle(x, y, radius);
    }
    @Override
    public void resize(double factor) {
        radius *= factor;
    }
}

class Rectangle extends Shape {
    private double x, y, width, height;
    public Rectangle(double x, double y, double width, double height, DrawingAPI drawingAPI) {
        super(drawingAPI);
        this.x = x;
        this.y = y;
        this.width = width;
        this.height = height;
}@Override    public void draw() {
        drawingAPI.drawRectangle(x, y, width, height);
    }
   @Override
    public void resize(double factor) {
        width *= factor;
        height *= factor;
    }
}


public class BridgePattern {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.println("Choose a Drawing API (1 or 2):");
        int apiChoice = scanner.nextInt();
DrawingAPI drawingAPI = (apiChoice == 1) ? new DrawingAPI1() : new DrawingAPI2();        System.out.println("Choose a shape (circle or rectangle):");
        String shapeType = scanner.next();

Shape shape;
        if (shapeType.equalsIgnoreCase("circle")) {
            System.out.println("Enter x, y, and radius:");
            double x = scanner.nextDouble();
            double y = scanner.nextDouble();
            double radius = scanner.nextDouble();
            shape = new Circle(x, y, radius, drawingAPI);
        } else if (shapeType.equalsIgnoreCase("rectangle")) {
            System.out.println("Enter x, y, width, and height:");
            double x = scanner.nextDouble();
            double y = scanner.nextDouble();
            double width = scanner.nextDouble();
            double height = scanner.nextDouble();
            shape = new Rectangle(x, y, width, height, drawingAPI);
        } else {
            System.out.println("Invalid shape type.");
            scanner.close();            return;
}        shape.draw();

System.out.println("Enter a resize factor:");
        double factor = scanner.nextDouble();
        shape.resize(factor);        shape.draw();
scanner.close();}}
