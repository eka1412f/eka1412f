
1. Near student management, key in details of students right. So, the details of the student need to connect to go to grade management, so when searching for student ID, come out the name of all of them and fill in the grade
2. Need to connect the course near the grade management, so that the grades filled in are only for the registered course
3. For the report, it is necessary to connect with all, as all the details need to be close to the report. *and must be included in the files*
4.COMBINED ALL CODE BELOW AND MAKE SURE IT'S FUNCTION WELL

import javax.swing.*;
import java.awt.*;

public class GradeModuleGUI extends JFrame {
    private GradeModule gradeModule = new GradeModule();
    private JTextField studentIdField;
    private JTextField courseField;
    private JTextField gradeField;
    private JTextArea displayArea;

    public GradeModuleGUI() {
        setTitle("Grade Module");
        setSize(800, 600);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        initComponents();
    }

    private void initComponents() {
        JPanel panel = new JPanel(new GridBagLayout());
        GridBagConstraints gbc = new GridBagConstraints();
        gbc.insets = new Insets(5, 5, 5, 5);
        gbc.anchor = GridBagConstraints.WEST;

        // Left Side Buttons
        JPanel leftPanel = new JPanel(new GridLayout(4, 1, 10, 10));
        JButton addGradeButton = new JButton("Add Grade");
        JButton viewGradesButton = new JButton("View Grades");
        JButton updateGradeButton = new JButton("Update Grade");
        JButton deleteGradeButton = new JButton("Delete Grade");

        leftPanel.add(addGradeButton);
        leftPanel.add(viewGradesButton);
        leftPanel.add(updateGradeButton);
        leftPanel.add(deleteGradeButton);

        gbc.gridx = 0;
        gbc.gridy = 0;
        gbc.gridheight = 6;
        panel.add(leftPanel, gbc);

        // Student Grade Details
        gbc.gridheight = 1;
        gbc.gridx = 1;
        gbc.gridy = 0;
        gbc.gridwidth = 1;
        panel.add(new JLabel("Student ID:"), gbc);
        studentIdField = new JTextField(20);
        gbc.gridx = 2;
        panel.add(studentIdField, gbc);

        gbc.gridx = 1;
        gbc.gridy = 1;
        panel.add(new JLabel("Course:"), gbc);
        courseField = new JTextField(20);
        gbc.gridx = 2;
        panel.add(courseField, gbc);

        gbc.gridx = 3;
        gbc.gridy = 1;
        panel.add(new JLabel("Grade:"), gbc);
        gradeField = new JTextField(20);
        gbc.gridx = 4;
        panel.add(gradeField, gbc);

        // Display Area
        gbc.gridx = 1;
        gbc.gridy = 2;
        gbc.gridwidth = 4;
        gbc.fill = GridBagConstraints.BOTH;
        displayArea = new JTextArea(10, 40);
        displayArea.setEditable(false);
        JScrollPane scrollPane = new JScrollPane(displayArea);
        panel.add(scrollPane, gbc);

        add(panel);

        // Button Actions
        addGradeButton.addActionListener(e -> addGrade());
        viewGradesButton.addActionListener(e -> viewGrades());
        updateGradeButton.addActionListener(e -> updateGrade());
        deleteGradeButton.addActionListener(e -> deleteGrade());
    }

    private void addGrade() {
        String studentId = studentIdField.getText().trim();
        String course = courseField.getText().trim();
        String grade = gradeField.getText().trim();

        gradeModule.addGrade(studentId, course, grade);
        displayArea.setText("Grade added successfully.");
    }

    private void viewGrades() {
        displayArea.setText(gradeModule.displayAllGrades());
    }

    private void updateGrade() {
        String studentId = studentIdField.getText().trim();
        String course = courseField.getText().trim();
        String grade = gradeField.getText().trim();

        gradeModule.updateGrade(studentId, course, grade);
        displayArea.setText("Grade updated successfully.");
    }

    private void deleteGrade() {
        String studentId = studentIdField.getText().trim();
        String course = courseField.getText().trim();

        gradeModule.deleteGrade(studentId, course);
        displayArea.setText("Grade deleted successfully.");
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(() -> {
            GradeModuleGUI gui = new GradeModuleGUI();
            gui.setVisible(true);
        });
    }
}

























import java.util.HashMap;
import java.util.Map;

public class GradeModule {

    private Map<String, Map<String, String>> gradeDatabase = new HashMap<>();

    public void addGrade(String studentId, String course, String grade) {
        gradeDatabase.computeIfAbsent(studentId, k -> new HashMap<>()).put(course, grade);
    }

    public void updateGrade(String studentId, String course, String grade) {
        if (gradeDatabase.containsKey(studentId) && gradeDatabase.get(studentId).containsKey(course)) {
            gradeDatabase.get(studentId).put(course, grade);
        }
    }

    public void deleteGrade(String studentId, String course) {
        if (gradeDatabase.containsKey(studentId)) {
            gradeDatabase.get(studentId).remove(course);
            if (gradeDatabase.get(studentId).isEmpty()) {
                gradeDatabase.remove(studentId);
            }
        }
    }

    public String displayAllGrades() {
        StringBuilder sb = new StringBuilder();
        for (Map.Entry<String, Map<String, String>> studentEntry : gradeDatabase.entrySet()) {
            String studentId = studentEntry.getKey();
            sb.append("Student ID: ").append(studentId).append("\n");
            for (Map.Entry<String, String> gradeEntry : studentEntry.getValue().entrySet()) {
                sb.append("Course: ").append(gradeEntry.getKey()).append(", Grade: ").append(gradeEntry.getValue()).append("\n");
            }
            sb.append("\n");
        }
        return sb.toString();
    }
}

















import javax.swing.*;
import java.awt.*;

public class StudentManagementGUI extends JFrame {
    private StudentManagement studentManagement = new StudentManagement();
    private JTextField idField;
    private JTextField firstNameField;
    private JTextField middleNameField;
    private JTextField lastNameField;
    private JTextField emailField;
    private JComboBox<String> degreeProgramComboBox;
    private JComboBox<String> majorComboBox;
    private JComboBox<String> semesterComboBox;
    private JTextArea displayArea;

    public StudentManagementGUI() {
        setTitle("Student Management System");
        setSize(800, 600);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        initComponents();
    }

    private void initComponents() {
        JPanel panel = new JPanel(new GridBagLayout());
        GridBagConstraints gbc = new GridBagConstraints();
        gbc.insets = new Insets(5, 5, 5, 5);
        gbc.anchor = GridBagConstraints.WEST;

        // Left Side Buttons
        JPanel leftPanel = new JPanel(new GridLayout(4, 1, 10, 10));
        JButton createButton = new JButton("Add Student");
        JButton readButton = new JButton("View Student");
        JButton updateButton = new JButton("Update Student");
        JButton deleteButton = new JButton("Delete Student");

        leftPanel.add(createButton);
        leftPanel.add(readButton);
        leftPanel.add(updateButton);
        leftPanel.add(deleteButton);

        gbc.gridx = 0;
        gbc.gridy = 0;
        gbc.gridheight = 6;
        panel.add(leftPanel, gbc);

        // Search bar
        gbc.gridheight = 1;
        gbc.gridx = 1;
        gbc.gridy = 0;
        gbc.gridwidth = 2;
        JTextField searchField = new JTextField(20);
        panel.add(searchField, gbc);

        JButton searchButton = new JButton("Search");
        gbc.gridx = 3;
        panel.add(searchButton, gbc);

        // Student Details
        gbc.gridwidth = 1;
        gbc.gridx = 1;
        gbc.gridy = 1;
        panel.add(new JLabel("ID:"), gbc);
        idField = new JTextField(20);
        gbc.gridx = 2;
        panel.add(idField, gbc);

        gbc.gridx = 1;
        gbc.gridy = 2;
        panel.add(new JLabel("First Name:"), gbc);
        firstNameField = new JTextField(20);
        gbc.gridx = 2;
        panel.add(firstNameField, gbc);

        gbc.gridx = 1;
        gbc.gridy = 3;
        panel.add(new JLabel("Middle Name:"), gbc);
        middleNameField = new JTextField(20);
        gbc.gridx = 2;
        panel.add(middleNameField, gbc);

        gbc.gridx = 1;
        gbc.gridy = 4;
        panel.add(new JLabel("Last Name:"), gbc);
        lastNameField = new JTextField(20);
        gbc.gridx = 2;
        panel.add(lastNameField, gbc);

        gbc.gridx = 1;
        gbc.gridy = 5;
        panel.add(new JLabel("Email:"), gbc);
        emailField = new JTextField(20);
        gbc.gridx = 2;
        panel.add(emailField, gbc);

        gbc.gridx = 1;
        gbc.gridy = 6;
        panel.add(new JLabel("Degree Program:"), gbc);
        degreeProgramComboBox = new JComboBox<>(new String[]{"Degree Program 1", "Degree Program 2"});
        gbc.gridx = 2;
        panel.add(degreeProgramComboBox, gbc);

        gbc.gridx = 1;
        gbc.gridy = 7;
        panel.add(new JLabel("Major:"), gbc);
        majorComboBox = new JComboBox<>(new String[]{"Major 1", "Major 2"});
        gbc.gridx = 2;
        panel.add(majorComboBox, gbc);

        gbc.gridx = 1;
        gbc.gridy = 8;
        panel.add(new JLabel("Semester:"), gbc);
        semesterComboBox = new JComboBox<>(new String[]{"Semester 1", "Semester 2"});
        gbc.gridx = 2;
        panel.add(semesterComboBox, gbc);

        // Display Area
        gbc.gridx = 1;
        gbc.gridy = 9;
        gbc.gridwidth = 3;
        gbc.fill = GridBagConstraints.BOTH;
        displayArea = new JTextArea(10, 40);
        displayArea.setEditable(false);
        JScrollPane scrollPane = new JScrollPane(displayArea);
        panel.add(scrollPane, gbc);

        add(panel);

        // Button Actions
        createButton.addActionListener(e -> {
            addStudent();
            displayArea.setText("Student added successfully.");
        });
        readButton.addActionListener(e -> displayStudents());
        updateButton.addActionListener(e -> updateStudent());
        deleteButton.addActionListener(e -> {
            deleteStudent();
            displayArea.setText("Student deleted successfully.");
        });
        searchButton.addActionListener(e -> searchStudent(searchField.getText().trim()));
    }

    private void addStudent() {
        String id = idField.getText().trim();
        String firstName = firstNameField.getText().trim();
        String middleName = middleNameField.getText().trim();
        String lastName = lastNameField.getText().trim();
        String email = emailField.getText().trim();
        String degreeProgram = (String) degreeProgramComboBox.getSelectedItem();
        String major = (String) majorComboBox.getSelectedItem();
        String semester = (String) semesterComboBox.getSelectedItem();

        Student student = new Student(id, firstName, middleName, lastName, email, degreeProgram, major, semester);
        studentManagement.addStudent(student);
    }

    private void displayStudents() {
        displayArea.setText(studentManagement.displayAllStudents());
    }

    private void updateStudent() {
        String id = idField.getText().trim();
        String firstName = firstNameField.getText().trim();
        String middleName = middleNameField.getText().trim();
        String lastName = lastNameField.getText().trim();
        String email = emailField.getText().trim();
        String degreeProgram = (String) degreeProgramComboBox.getSelectedItem();
        String major = (String) majorComboBox.getSelectedItem();
        String semester = (String) semesterComboBox.getSelectedItem();

        Student student = new Student(id, firstName, middleName, lastName, email, degreeProgram, major, semester);
        studentManagement.updateStudent(id, student);
        displayArea.setText("Student updated successfully.");
    }

    private void deleteStudent() {
        String id = idField.getText().trim();
        studentManagement.deleteStudent(id);
    }

    private void searchStudent(String id) {
        Student student = studentManagement.getStudent(id);
        if (student != null) {
            displayArea.setText(student.toString());
        } else {
            displayArea.setText("Student not found.");
        }
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(() -> {
            StudentManagementGUI gui = new StudentManagementGUI();
            gui.setVisible(true);
        });
    }
}





import javax.swing.*;
import java.awt.*;
import java.util.HashMap;
import java.util.Map;

public class StudentGradeRecordSystem extends JFrame {

    private JPanel mainPanel;
    private CardLayout cardLayout;
    private Map<String, String> userDatabase;
    private String loggedInUser;

    public StudentGradeRecordSystem() {
        userDatabase = new HashMap<>();

        setTitle("Student Grade Record System");
        setSize(1000, 800);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLocationRelativeTo(null);

        cardLayout = new CardLayout();
        mainPanel = new JPanel(cardLayout);

        JPanel loginPanel = createLoginPanel();
        JPanel registrationPanel = createRegistrationPanel();
        JPanel mainAppPanel = createMainAppPanel();

        mainPanel.add(loginPanel, "Login");
        mainPanel.add(registrationPanel, "Register");
        mainPanel.add(mainAppPanel, "MainApp");

        add(mainPanel);
    }

    private JPanel createLoginPanel() {
        JPanel loginPanel = new JPanel(new GridBagLayout());
        GridBagConstraints gbc = new GridBagConstraints();
        gbc.insets = new Insets(5, 5, 5, 5);

        JLabel userLabel = new JLabel("Username:");
        JLabel passLabel = new JLabel("Password:");
        JTextField userField = new JTextField(15);
        JPasswordField passField = new JPasswordField(15);
        JButton loginButton = new JButton("Login");
        JButton registerButton = new JButton("Register");
        JLabel loginMessageLabel = new JLabel();

        gbc.gridx = 0;
        gbc.gridy = 0;
        loginPanel.add(userLabel, gbc);
        gbc.gridx = 1;
        loginPanel.add(userField, gbc);

        gbc.gridx = 0;
        gbc.gridy = 1;
        loginPanel.add(passLabel, gbc);
        gbc.gridx = 1;
        loginPanel.add(passField, gbc);

        gbc.gridx = 1;
        gbc.gridy = 2;
        loginPanel.add(loginButton, gbc);
        gbc.gridy = 3;
        loginPanel.add(registerButton, gbc);

        gbc.gridx = 0;
        gbc.gridy = 4;
        gbc.gridwidth = 2;
        loginPanel.add(loginMessageLabel, gbc);

        loginButton.addActionListener(e -> {
            String username = userField.getText().trim();
            String password = new String(passField.getPassword()).trim();
            System.out.println("Trying to login with username: " + username + " and password: " + password);

            if (userDatabase.containsKey(username)) {
                System.out.println("Username found in database");
                if (userDatabase.get(username).equals(password)) {
                    System.out.println("Password matched");
                    loginMessageLabel.setText("Login successful.");
                    loggedInUser = username;
                    cardLayout.show(mainPanel, "MainApp");
                } else {
                    System.out.println("Password did not match");
                    loginMessageLabel.setText("Invalid username or password.");
                }
            } else {
                System.out.println("Username not found in database");
                loginMessageLabel.setText("Invalid username or password.");
            }
        });

        registerButton.addActionListener(e -> cardLayout.show(mainPanel, "Register"));

        return loginPanel;
    }

    private JPanel createRegistrationPanel() {
        JPanel registrationPanel = new JPanel(new GridBagLayout());
        GridBagConstraints gbc = new GridBagConstraints();
        gbc.insets = new Insets(5, 5, 5, 5);

        JLabel userLabel = new JLabel("Username:");
        JLabel passLabel = new JLabel("Password:");
        JTextField userField = new JTextField(15);
        JPasswordField passField = new JPasswordField(15);
        JButton registerButton = new JButton("Register");
        JButton backButton = new JButton("Back to Login");
        JLabel registrationMessageLabel = new JLabel();

        gbc.gridx = 0;
        gbc.gridy = 0;
        registrationPanel.add(userLabel, gbc);
        gbc.gridx = 1;
        registrationPanel.add(userField, gbc);

        gbc.gridx = 0;
        gbc.gridy = 1;
        registrationPanel.add(passLabel, gbc);
        gbc.gridx = 1;
        registrationPanel.add(passField, gbc);

        gbc.gridx = 1;
        gbc.gridy = 2;
        registrationPanel.add(registerButton, gbc);
        gbc.gridy = 3;
        registrationPanel.add(backButton, gbc);

        gbc.gridx = 0;
        gbc.gridy = 4;
        gbc.gridwidth = 2;
        registrationPanel.add(registrationMessageLabel, gbc);

        registerButton.addActionListener(e -> {
            String username = userField.getText().trim();
            String password = new String(passField.getPassword()).trim();
            System.out.println("Trying to register with username: " + username + " and password: " + password);

            if (userDatabase.containsKey(username)) {
                System.out.println("Username already exists in database");
                registrationMessageLabel.setText("Username already exists.");
            } else {
                System.out.println("Registering new user");
                userDatabase.put(username, password);
                registrationMessageLabel.setText("User registered successfully.");
                cardLayout.show(mainPanel, "Login");
            }
        });

        backButton.addActionListener(e -> cardLayout.show(mainPanel, "Login"));

        return registrationPanel;
    }

    private JPanel createMainAppPanel() {
        JPanel mainAppPanel = new JPanel(new BorderLayout());

        JTabbedPane tabbedPane = new JTabbedPane();
        tabbedPane.addTab("Student Management", new StudentManagementGUI().getContentPane());
        tabbedPane.addTab("Grade Management", new GradeModuleGUI().getContentPane());

        JButton logoutButton = new JButton("Logout");
        logoutButton.addActionListener(e -> {
            loggedInUser = null;
            cardLayout.show(mainPanel, "Login");
        });

        mainAppPanel.add(tabbedPane, BorderLayout.CENTER);
        mainAppPanel.add(logoutButton, BorderLayout.SOUTH);

        return mainAppPanel;
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(() -> {
            StudentGradeRecordSystem app = new StudentGradeRecordSystem();
            app.setVisible(true);
        });
    }
}

CourseManagement.java

import javax.swing.*;
import java.awt.*;
import java.util.HashMap;
import java.util.Map;

public class CourseManagement extends JFrame {

    private final CardLayout cardLayout;
    private final JPanel cardPanel;
    private final Map<String, Course> courses = new HashMap<>();

    private JTextField createCourseCodeField;
    private JTextField createCourseNameField;
    private JTextField createCreditHoursField;
    private JTextField createCourseInstructorsField;
    private JTextField createDescriptionField;

    private JTextField viewCourseCodeField;
    private JPanel viewCourseDetailsPanel;

    private JTextField editCourseCodeField;
    private JTextField editNewCourseCodeField;
    private JTextField editNewCourseNameField;
    private JTextField editNewCreditHoursField;
    private JTextField editNewCourseInstructorsField;
    private JTextField editNewDescriptionField;

    private JTextField removeCourseCodeField;

    public CourseManagement() {
        setTitle("Course Management");
        setSize(800, 500);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLocationRelativeTo(null);

        cardLayout = new CardLayout();
        cardPanel = new JPanel(cardLayout);

        cardPanel.add(createNewCoursePanel(), "Create");
        cardPanel.add(createViewCoursePanel(), "View");
        cardPanel.add(createEditCoursePanel(), "Edit");
        cardPanel.add(createRemoveCoursePanel(), "Remove");

        getContentPane().add(createMenuPanel(), BorderLayout.NORTH);
        getContentPane().add(cardPanel, BorderLayout.CENTER);
    }

    private JPanel createMenuPanel() {
        JPanel panel = new JPanel();
        panel.setLayout(new GridLayout(1, 4));

        JButton createButton = new JButton("CREATE");
        createButton.addActionListener(e -> {
            clearFields();
            cardLayout.show(cardPanel, "Create");
        });
        panel.add(createButton);

        JButton viewButton = new JButton("VIEW");
        viewButton.addActionListener(e -> {
            clearFields();
            clearViewCourseFields(); // Clear the view course field
            cardLayout.show(cardPanel, "View");
        });
        panel.add(viewButton);

        JButton editButton = new JButton("EDIT");
        editButton.addActionListener(e -> {
            clearFields();
            clearViewCourseFields(); // Clear the view course field
            cardLayout.show(cardPanel, "Edit");
        });
        panel.add(editButton);

        JButton removeButton = new JButton("REMOVE");
        removeButton.addActionListener(e -> {
            clearFields();
            clearViewCourseFields(); // Clear the view course field
            cardLayout.show(cardPanel, "Remove");
        });
        panel.add(removeButton);

        return panel;
    }

    private JPanel createNewCoursePanel() {
        JPanel panel = new JPanel(new BorderLayout());
        JPanel topPanel = new JPanel(new GridLayout(2, 1));

        JLabel newCourseLabel = new JLabel("NEW COURSE");
        newCourseLabel.setHorizontalAlignment(JLabel.CENTER); // Center align the label
        topPanel.add(newCourseLabel);
        topPanel.add(new JLabel("")); // Add empty label for spacing

        JPanel inputPanel = new JPanel(new GridLayout(6, 2));
        inputPanel.add(new JLabel("COURSE CODE:"));
        createCourseCodeField = new JTextField();
        inputPanel.add(createCourseCodeField);

        inputPanel.add(new JLabel("COURSE NAME:"));
        createCourseNameField = new JTextField();
        inputPanel.add(createCourseNameField);

        inputPanel.add(new JLabel("CREDIT HOURS:"));
        createCreditHoursField = new JTextField();
        inputPanel.add(createCreditHoursField);

        inputPanel.add(new JLabel("COURSE INSTRUCTORS:"));
        createCourseInstructorsField = new JTextField();
        inputPanel.add(createCourseInstructorsField);

        inputPanel.add(new JLabel("DESCRIPTION:"));
        createDescriptionField = new JTextField();
        inputPanel.add(createDescriptionField);

        // Add empty labels to create space between input fields and buttons
        inputPanel.add(new JLabel(""));
        inputPanel.add(new JLabel(""));

        JPanel buttonPanel = new JPanel(new FlowLayout(FlowLayout.CENTER)); // Use FlowLayout for button panel

        // Create SAVE button with custom background and foreground colors
        JButton saveButton = new JButton("SAVE");
        saveButton.setPreferredSize(new Dimension(85, 30)); // Set preferred size for SAVE button
        saveButton.setBackground(Color.GREEN); // Set background color
        saveButton.setForeground(Color.BLACK); // Set text color
        saveButton.addActionListener(e -> {
            if (isFieldsEmpty()) {
                JOptionPane.showMessageDialog(null, "COMPLETE ALL THE FIELDS");
            } else {
                Course course = new Course(
                        createCourseCodeField.getText(),
                        createCourseNameField.getText(),
                        createCreditHoursField.getText(),
                        createCourseInstructorsField.getText(),
                        createDescriptionField.getText()
                );
                courses.put(course.getCourseCode(), course);
                JOptionPane.showMessageDialog(null, "COURSE SAVED");
                clearFields(); // Clear fields after saving
                cardLayout.show(cardPanel, "Create"); // Show the Create panel again to refresh the fields
            }
        });
        buttonPanel.add(saveButton);

        // Create CANCEL button with custom background and foreground colors
        JButton cancelButton = new JButton("CANCEL");
        cancelButton.setPreferredSize(new Dimension(85, 30)); // Set preferred size for CANCEL button
        cancelButton.setBackground(Color.RED); // Set background color
        cancelButton.setForeground(Color.BLACK); // Set text color
        cancelButton.addActionListener(e -> clearFields());
        buttonPanel.add(cancelButton);

        // Add empty labels to create space between buttons and bottom border
        buttonPanel.add(new JLabel(""));
        buttonPanel.add(new JLabel(""));

        panel.add(topPanel, BorderLayout.NORTH);
        panel.add(inputPanel, BorderLayout.CENTER);
        panel.add(buttonPanel, BorderLayout.SOUTH); // Add the button panel to the bottom of the main panel

        return panel;
    }

    private JPanel createViewCoursePanel() {
        JPanel panel = new JPanel(new BorderLayout());

        // Create a label for the VIEW title and center it
        final JLabel[] viewLabel = {new JLabel("VIEW", JLabel.CENTER)};
        panel.add(viewLabel[0], BorderLayout.NORTH);

        // Create the input panel for entering course code
        JPanel inputPanel = new JPanel(new FlowLayout(FlowLayout.CENTER));
        JLabel enterLabel = new JLabel("Enter course code:");
        viewCourseCodeField = new JTextField(10);
        JButton enterButton = new JButton("ENTER");

        // Add the label, text field, and button to the input panel
        inputPanel.add(enterLabel);
        inputPanel.add(viewCourseCodeField);
        inputPanel.add(enterButton);

        // Add the input panel to the center of the main panel
        panel.add(inputPanel, BorderLayout.CENTER);

        enterButton.addActionListener(e -> {
            String courseCode = viewCourseCodeField.getText();
            Course course = courses.get(courseCode);
            if (course != null) {
                panel.removeAll();

                // Update the VIEW label to display course code and name
                viewLabel[0] = new JLabel(course.getCourseCode() + "  " + course.getCourseName(), JLabel.CENTER);
                panel.add(viewLabel[0], BorderLayout.NORTH);

                JPanel courseDetailsPanel = new JPanel(new GridLayout(5, 2));
                courseDetailsPanel.add(new JLabel("COURSE CODE:"));
                courseDetailsPanel.add(new JLabel(course.getCourseCode()));

                courseDetailsPanel.add(new JLabel("COURSE NAME:"));
                courseDetailsPanel.add(new JLabel(course.getCourseName()));

                courseDetailsPanel.add(new JLabel("CREDIT HOURS:"));
                courseDetailsPanel.add(new JLabel(course.getCreditHours()));

                courseDetailsPanel.add(new JLabel("COURSE INSTRUCTORS:"));
                courseDetailsPanel.add(new JLabel(course.getCourseInstructors()));

                courseDetailsPanel.add(new JLabel("DESCRIPTION:"));
                courseDetailsPanel.add(new JLabel(course.getDescription()));

                JPanel backPanel = new JPanel(new FlowLayout(FlowLayout.CENTER));
                JButton backButton = new JButton("BACK");
                backButton.setPreferredSize(new Dimension(80, 30)); // Set the preferred size of the BACK button
                backButton.addActionListener(ev -> {
                    clearViewCourseFields();
                    panel.removeAll();

                    // Create a new VIEW label and add it to the panel
                    viewLabel[0] = new JLabel("VIEW", JLabel.CENTER);
                    panel.add(viewLabel[0], BorderLayout.NORTH);

                    panel.add(inputPanel, BorderLayout.CENTER);
                    panel.revalidate();
                    panel.repaint();
                });

                backPanel.add(backButton);

                panel.add(viewLabel[0], BorderLayout.NORTH); // Keep VIEW label
                panel.add(courseDetailsPanel, BorderLayout.CENTER);
                panel.add(backPanel, BorderLayout.SOUTH);

                panel.revalidate();
                panel.repaint();
            } else {
                JOptionPane.showMessageDialog(null, "COURSE NOT FOUND");
            }
        });

        return panel;
    }

    private JPanel createEditCoursePanel() {
        JPanel panel = new JPanel(new BorderLayout());

        // Create a label for the VIEW title and center it
        final JLabel[] viewLabel = {new JLabel("EDIT", JLabel.CENTER)};
        panel.add(viewLabel[0], BorderLayout.NORTH);

        // Create the input panel for entering course code
        JPanel inputPanel = new JPanel(new FlowLayout(FlowLayout.CENTER));
        JLabel enterLabel = new JLabel("Enter course code to EDIT:");
        editCourseCodeField = new JTextField(10);
        JButton enterButton = new JButton("ENTER");

        inputPanel.add(enterLabel);
        inputPanel.add(editCourseCodeField);
        inputPanel.add(enterButton);

// Add the input panel to the center of the main panel
        panel.add(inputPanel, BorderLayout.CENTER);

        enterButton.addActionListener(_ -> {
            var ref = new Object() {
                String courseCode = editCourseCodeField.getText();
            };
            Course course = courses.get(ref.courseCode);
            if (course != null) {
                panel.removeAll(); // Clear the panel for new components

                // Update the VIEW label to display course code and name
                JLabel newCourseLabel = new JLabel(course.getCourseCode() + "  " + course.getCourseName());
                newCourseLabel.setHorizontalAlignment(JLabel.CENTER);
                panel.add(newCourseLabel);
                panel.add(new JLabel(""));

                panel.setLayout(new GridLayout(12, 2));
                // Add the existing course details with new labels for editing
                panel.add(new JLabel("COURSE CODE:"));
                panel.add(new JLabel(course.getCourseCode()));

                panel.add(new JLabel("NEW COURSE CODE:"));
                editNewCourseCodeField = new JTextField();
                panel.add(editNewCourseCodeField);

                panel.add(new JLabel("COURSE NAME:"));
                panel.add(new JLabel(course.getCourseName()));

                panel.add(new JLabel("NEW COURSE NAME:"));
                editNewCourseNameField = new JTextField();
                panel.add(editNewCourseNameField);

                panel.add(new JLabel("CREDIT HOURS:"));
                panel.add(new JLabel(course.getCreditHours()));

                panel.add(new JLabel("NEW CREDIT HOURS:"));
                editNewCreditHoursField = new JTextField();
                panel.add(editNewCreditHoursField);

                panel.add(new JLabel("COURSE INSTRUCTORS:"));
                panel.add(new JLabel(course.getCourseInstructors()));

                panel.add(new JLabel("NEW COURSE INSTRUCTORS:"));
                editNewCourseInstructorsField = new JTextField();
                panel.add(editNewCourseInstructorsField);

                panel.add(new JLabel("DESCRIPTION:"));
                panel.add(new JLabel(course.getDescription()));

                panel.add(new JLabel("NEW DESCRIPTION:"));
                editNewDescriptionField = new JTextField();
                panel.add(editNewDescriptionField);

                JPanel backPanel = new JPanel(new FlowLayout(FlowLayout.CENTER));

                JButton saveButton = new JButton("SAVE");
                saveButton.setPreferredSize(new Dimension(85, 30)); // Set preferred size for SAVE button
                saveButton.setBackground(Color.GREEN); // Set background color
                saveButton.setForeground(Color.BLACK); // Set text color

                saveButton.addActionListener(ev -> {
                    if (isEditFieldsEmpty()) {
                        JOptionPane.showMessageDialog(null, "COMPLETE ALL THE FIELDS");
                    } else {
                        ref.courseCode = editCourseCodeField.getText();
                        Course editedCourse = new Course(
                                editNewCourseCodeField.getText(),
                                editNewCourseNameField.getText(),
                                editNewCreditHoursField.getText(),
                                editNewCourseInstructorsField.getText(),
                                editNewDescriptionField.getText()
                        );
                        // Remove the old course details
                        courses.remove(ref.courseCode);
                        // Add the edited course details
                        courses.put(editedCourse.getCourseCode(), editedCourse);
                        JOptionPane.showMessageDialog(null, "COURSE EDITED");
                        clearEditFields(); // Clear the edit fields after saving
                        // Show the initial input panel after saving
                        panel.removeAll();
                        panel.add(inputPanel, BorderLayout.CENTER); // Add the input panel back
                        panel.revalidate();
                        panel.repaint();

                        // Update the course details in the VIEW panel if the course is currently being viewed
                        Course viewedCourse = courses.get(viewCourseCodeField.getText());
                        if (viewedCourse != null && viewedCourse.getCourseCode().equals(ref.courseCode)) {
                            // Update the course details in the VIEW panel
                            // You can implement this logic based on your current VIEW panel implementation
                        }
                    }
                });

                panel.add(saveButton);

                JButton cancelButton = new JButton("CANCEL");
                cancelButton.setPreferredSize(new Dimension(85, 30)); // Set preferred size for CANCEL button
                cancelButton.setBackground(Color.RED); // Set background color
                cancelButton.setForeground(Color.BLACK); // Set text color
                cancelButton.addActionListener(ev -> {
                    clearFields(); // Clear all fields
                    // Show the initial input panel after canceling
                    panel.removeAll();
                    panel.add(inputPanel, BorderLayout.CENTER); // Add the input panel back
                    panel.revalidate();
                    panel.repaint();
                });
                panel.add(cancelButton);

                panel.revalidate(); // Revalidate and repaint the panel to update the UI
                panel.repaint();
            } else {
                JOptionPane.showMessageDialog(null, "COURSE NOT FOUND");
            }
        });
        inputPanel.add(enterButton);

        panel.add(inputPanel, BorderLayout.CENTER);

        return panel;
    }

    private JPanel createRemoveCoursePanel() {
        JPanel panel = new JPanel(new BorderLayout());

        // Create a label for the VIEW title and center it
        JLabel viewLabel = new JLabel("REMOVE", JLabel.CENTER);
        panel.add(viewLabel, BorderLayout.NORTH);

        // Create the input panel for entering course code
        JPanel inputPanel = new JPanel(new FlowLayout(FlowLayout.CENTER));
        JLabel enterLabel = new JLabel("Enter course code to REMOVE:");
        JTextField removeCourseCodeField = new JTextField(10);
        JButton removeButton = new JButton("REMOVE");

        inputPanel.add(enterLabel);
        inputPanel.add(removeCourseCodeField);
        inputPanel.add(removeButton);

        // Add the input panel to the center of the main panel
        panel.add(inputPanel, BorderLayout.CENTER);

        removeButton.addActionListener(e -> {
            String courseCode = removeCourseCodeField.getText();
            Course course = courses.get(courseCode);
            if (course != null) {
                // Construct message with course details and confirmation question
                StringBuilder message = new StringBuilder();
                message.append(course.getCourseCode() + course.getCourseName()).append("\n");
                message.append("COURSE CODE: ").append(course.getCourseCode()).append("\n");
                message.append("COURSE NAME").append(course.getCourseName()).append("\n");
                message.append("CREDIT HOURS: ").append(course.getCreditHours()).append("\n");
                message.append("COURSE INSTRUCTORS: ").append(course.getCourseInstructors()).append("\n");
                message.append("DESCRIPTION: ").append(course.getDescription()).append("\n\n");
                message.append("Are you sure you want to delete this course?");

                int confirm = JOptionPane.showConfirmDialog(null, message.toString(), "Confirmation", JOptionPane.YES_NO_OPTION);
                if (confirm == JOptionPane.YES_OPTION) {
                    courses.remove(courseCode);
                    JOptionPane.showMessageDialog(null, "Course Removed Successfully");
                } else {
                    JOptionPane.showMessageDialog(null, "Deletion Canceled");
                }
            } else {
                JOptionPane.showMessageDialog(null, "COURSE NOT FOUND");
            }
        });

        return panel;
    }


    private void clearFields() {
        createCourseCodeField.setText("");
        createCourseNameField.setText("");
        createCreditHoursField.setText("");
        createCourseInstructorsField.setText("");
        createDescriptionField.setText("");

    }
    private boolean isFieldsEmpty() {
        return createCourseCodeField.getText().isEmpty() ||
                createCourseNameField.getText().isEmpty() ||
                createCreditHoursField.getText().isEmpty() ||
                createCourseInstructorsField.getText().isEmpty() ||
                createDescriptionField.getText().isEmpty();
    }
    private void clearViewCourseFields() {
        viewCourseCodeField.setText("");
    }
    private boolean isEditFieldsEmpty() {
        return editNewCourseCodeField.getText().isEmpty() ||
                editNewCourseNameField.getText().isEmpty() ||
                editNewCreditHoursField.getText().isEmpty() ||
                editNewCourseInstructorsField.getText().isEmpty() ||
                editNewDescriptionField.getText().isEmpty();
    }

    private void clearEditFields() {
        editNewCourseCodeField.setText("");
        editNewCourseNameField.setText("");
        editNewCreditHoursField.setText("");
        editNewCourseInstructorsField.setText("");
        editNewDescriptionField.setText("");
    }

    // Method to clear the edit panel
    private void clearEditPanel(JPanel panel) {
        panel.removeAll();
        panel.setLayout(new BorderLayout()); // Reset the layout
        // Create a label for the VIEW title and center it
        final JLabel[] viewLabel = {new JLabel("EDIT", JLabel.CENTER)};
        panel.add(viewLabel[0], BorderLayout.NORTH);
        JPanel inputPanel = new JPanel(new FlowLayout(FlowLayout.CENTER));
        JLabel enterLabel = new JLabel("Enter course code to edit:");
        editCourseCodeField = new JTextField(10);
        inputPanel.add(enterLabel);
        inputPanel.add(editCourseCodeField);


        // Add the ENTER button to the input panel
        JButton enterButton = new JButton("ENTER");
        enterButton.addActionListener(e ->  {
            // Your enter button logic here
        });
        panel.revalidate();
        panel.repaint();
    }

    // Method to reset the edit panel
    private void resetEditPanel(JPanel panel, JButton enterButton) {
        clearEditPanel(panel);
        enterButton.addActionListener(e -> {
        });
    }


    public static void main(String[] args) {
        SwingUtilities.invokeLater(() -> {
            CourseManagement frame = new CourseManagement();
            frame.setVisible(true);
        });
    }
}

Course.java
public class Course {
    private String courseCode;
    private String courseName;
    private String creditHours;
    private String courseInstructors;
    private String description;

    public Course(String courseCode, String courseName, String creditHours, String courseInstructors, String description) {
        this.courseCode = courseCode;
        this.courseName = courseName;
        this.creditHours = creditHours;
        this.courseInstructors = courseInstructors;
        this.description = description;
    }

    public String getCourseCode() {
        return courseCode;
    }

    public void setCourseCode(String courseCode) {
        this.courseCode = courseCode;
    }

    public String getCourseName() {
        return courseName;
    }

    public void setCourseName(String courseName) {
        this.courseName = courseName;
    }

    public String getCreditHours() {
        return creditHours;
    }

    public void setCreditHours(String creditHours) {
        this.creditHours = creditHours;
    }

    public String getCourseInstructors() {
        return courseInstructors;
    }

    public void setCourseInstructors(String courseInstructors) {
        this.courseInstructors = courseInstructors;
    }

    public String getDescription() {
        return description;
    }

    public void setDescription(String description) {
        this.description = description;
    }
}
import javax.swing.*;
import java.awt.*;
import java.util.HashMap;
import java.util.Map;

public class StudentGradeRecordSystem extends JFrame {

    private final JPanel mainPanel;
    private final CardLayout cardLayout;
    private final Map<String, String> userDatabase;

    public StudentGradeRecordSystem() {
        userDatabase = new HashMap<>();

        setTitle("Student Grade Record System");
        setSize(1000, 800);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLocationRelativeTo(null);

        cardLayout = new CardLayout();
        mainPanel = new JPanel(cardLayout);

        JPanel loginPanel = createLoginPanel();
        JPanel registrationPanel = createRegistrationPanel();
        JPanel mainAppPanel = createMainAppPanel();

        mainPanel.add(loginPanel, "Login");
        mainPanel.add(registrationPanel, "Register");
        mainPanel.add(mainAppPanel, "MainApp");

        add(mainPanel);
    }

    private JPanel createLoginPanel() {
        JPanel loginPanel = new JPanel(new GridBagLayout());
        GridBagConstraints gbc = new GridBagConstraints();
        gbc.insets = new Insets(5, 5, 5, 5);

        JLabel userLabel = new JLabel("Username:");
        JLabel passLabel = new JLabel("Password:");
        JTextField userField = new JTextField(15);
        JPasswordField passField = new JPasswordField(15);
        JButton loginButton = new JButton("Login");
        JButton registerButton = new JButton("Register");
        JLabel loginMessageLabel = new JLabel();

        gbc.gridx = 0;
        gbc.gridy = 0;
        loginPanel.add(userLabel, gbc);
        gbc.gridx = 1;
        loginPanel.add(userField, gbc);

        gbc.gridx = 0;
        gbc.gridy = 1;
        loginPanel.add(passLabel, gbc);
        gbc.gridx = 1;
        loginPanel.add(passField, gbc);

        gbc.gridx = 1;
        gbc.gridy = 2;
        loginPanel.add(loginButton, gbc);
        gbc.gridy = 3;
        loginPanel.add(registerButton, gbc);

        gbc.gridx = 0;
        gbc.gridy = 4;
        gbc.gridwidth = 2;
        loginPanel.add(loginMessageLabel, gbc);

        loginButton.addActionListener(e -> {
            String username = userField.getText().trim();
            String password = new String(passField.getPassword()).trim();
            System.out.println("Trying to login with username: " + username + " and password: " + password);

            if (userDatabase.containsKey(username)) {
                System.out.println("Username found in database");
                if (userDatabase.get(username).equals(password)) {
                    System.out.println("Password matched");
                    loginMessageLabel.setText("Login successful.");
                    cardLayout.show(mainPanel, "MainApp");
                } else {
                    System.out.println("Password did not match");
                    loginMessageLabel.setText("Invalid username or password.");
                }
            } else {
                System.out.println("Username not found in database");
                loginMessageLabel.setText("Invalid username or password.");
            }
        });

        registerButton.addActionListener(e -> cardLayout.show(mainPanel, "Register"));

        return loginPanel;
    }

    private JPanel createRegistrationPanel() {
        JPanel registrationPanel = new JPanel(new GridBagLayout());
        GridBagConstraints gbc = new GridBagConstraints();
        gbc.insets = new Insets(5, 5, 5, 5);

        JLabel userLabel = new JLabel("Username:");
        JLabel passLabel = new JLabel("Password:");
        JTextField userField = new JTextField(15);
        JPasswordField passField = new JPasswordField(15);
        JButton registerButton = new JButton("Register");
        JButton backButton = new JButton("Back to Login");
        JLabel registrationMessageLabel = new JLabel();

        gbc.gridx = 0;
        gbc.gridy = 0;
        registrationPanel.add(userLabel, gbc);
        gbc.gridx = 1;
        registrationPanel.add(userField, gbc);

        gbc.gridx = 0;
        gbc.gridy = 1;
        registrationPanel.add(passLabel, gbc);
        gbc.gridx = 1;
        registrationPanel.add(passField, gbc);

        gbc.gridx = 1;
        gbc.gridy = 2;
        registrationPanel.add(registerButton, gbc);
        gbc.gridy = 3;
        registrationPanel.add(backButton, gbc);

        gbc.gridx = 0;
        gbc.gridy = 4;
        gbc.gridwidth = 2;
        registrationPanel.add(registrationMessageLabel, gbc);

        registerButton.addActionListener(e -> {
            String username = userField.getText().trim();
            String password = new String(passField.getPassword()).trim();
            System.out.println("Trying to register with username: " + username + " and password: " + password);

            if (userDatabase.containsKey(username)) {
                System.out.println("Username already exists in database");
                registrationMessageLabel.setText("Username already exists.");
            } else {
                System.out.println("Registering new user");
                userDatabase.put(username, password);
                registrationMessageLabel.setText("User registered successfully.");
                cardLayout.show(mainPanel, "Login");
            }
        });

        backButton.addActionListener(e -> cardLayout.show(mainPanel, "Login"));

        return registrationPanel;
    }

    private JPanel createMainAppPanel() {
        JPanel mainAppPanel = new JPanel(new BorderLayout());

        JTabbedPane tabbedPane = new JTabbedPane();
        tabbedPane.addTab("Student Management", new StudentManagementGUI().getContentPane());
        tabbedPane.addTab("Grade Management", new GradeModuleGUI().getRootPane());
        tabbedPane.addTab("Report Generation", new ReportGenerationGUI().getRootPane());

        JButton logoutButton = new JButton("Logout");
        logoutButton.addActionListener(e -> cardLayout.show(mainPanel, "Login"));

        mainAppPanel.add(tabbedPane, BorderLayout.CENTER);
        mainAppPanel.add(logoutButton, BorderLayout.SOUTH);

        return mainAppPanel;
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(() -> {
            StudentGradeRecordSystem app = new StudentGradeRecordSystem();
            app.setVisible(true);
        });
    }

    private record StudentManagementGUI() {
        public Component getContentPane() {
            return null;
        }
    }

    private static class GradeModuleGUI {
        public Component getRootPane() {
            return null;
        }
    }
}

class ReportGenerationGUI extends JPanel {
    private final GradeModule gradeModule = new GradeModule();
    private final JTextArea reportArea;

    public ReportGenerationGUI() {
        setLayout(new BorderLayout());

        JPanel panel = new JPanel(new GridBagLayout());
        GridBagConstraints gbc = new GridBagConstraints();
        gbc.insets = new Insets(5, 5, 5, 5);

        JLabel studentIdLabel = new JLabel("Student ID:");
        JTextField studentIdField = new JTextField(20);
        JButton generateReportButton = new JButton("Generate Report");

        gbc.gridx = 0;
        gbc.gridy = 0;
        panel.add(studentIdLabel, gbc);
        gbc.gridx = 1;
        panel.add(studentIdField, gbc);
        gbc.gridx = 2;
        panel.add(generateReportButton, gbc);

        reportArea = new JTextArea(20, 50);
        reportArea.setEditable(false);
        JScrollPane scrollPane = new JScrollPane(reportArea);

        add(panel, BorderLayout.NORTH);
        add(scrollPane, BorderLayout.CENTER);

        generateReportButton.addActionListener(e -> generateReport(studentIdField.getText().trim()));
    }

    private void generateReport(String studentId) {
        if (studentId.isEmpty()) {
            reportArea.setText("Please enter a Student ID.");
            return;
        }

        Map<String, String> grades = gradeModule.getGradesByStudentId(studentId);
        if (grades == null) {
            reportArea.setText("No records found for Student ID: " + studentId);
            return;
        }

        StringBuilder report = new StringBuilder();
        report.append("Report for Student ID: ").append(studentId).append("\n\n");
        for (Map.Entry<String, String> entry : grades.entrySet()) {
            report.append("Course: ").append(entry.getKey()).append(", Grade: ").append(entry.getValue()).append("\n");
        }

        reportArea.setText(report.toString());
    }
}

class GradeModule {
    public final Map<String, Map<String, String>> gradeDatabase = new HashMap<>();

    public Map<String, String> getGradesByStudentId(String studentId) {
        return gradeDatabase.get(studentId);
    }
}
