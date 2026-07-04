# Student-record-management-
Student record management 
#include <iostream>
#include <fstream>
#include <vector>
#include <string>

using namespace std;

class Student {
public:
    int rollNo;
    string name;
    int age;
    string course;
    float marks;

    void input() {
        cout << "\nEnter Roll Number: ";
        cin >> rollNo;
        cin.ignore();

        cout << "Enter Name: ";
        getline(cin, name);

        cout << "Enter Age: ";
        cin >> age;
        cin.ignore();

        cout << "Enter Course: ";
        getline(cin, course);

        cout << "Enter Marks: ";
        cin >> marks;
    }

    void display() const {
        cout << "\n--------------------------------";
        cout << "\nRoll Number : " << rollNo;
        cout << "\nName        : " << name;
        cout << "\nAge         : " << age;
        cout << "\nCourse      : " << course;
        cout << "\nMarks       : " << marks;
        cout << "\n--------------------------------";
    }
};

vector<Student> students;

void saveToFile() {
    ofstream file("students.txt");

    for (Student s : students) {
        file << s.rollNo << endl;
        file << s.name << endl;
        file << s.age << endl;
        file << s.course << endl;
        file << s.marks << endl;
    }

    file.close();
}

void loadFromFile() {
    students.clear();

    ifstream file("students.txt");

    Student s;

    while (file >> s.rollNo) {
        file.ignore();

        getline(file, s.name);

        file >> s.age;
        file.ignore();

        getline(file, s.course);

        file >> s.marks;
        file.ignore();

        students.push_back(s);
    }

    file.close();
}

void addStudent() {
    Student s;

    cout << "\n===== Add Student =====";
    s.input();

    students.push_back(s);

    saveToFile();

    cout << "\nStudent Added Successfully!\n";
}

void displayStudents() {

    if (students.empty()) {
        cout << "\nNo Records Found.\n";
        return;
    }

    cout << "\n===== Student Records =====";

    for (Student s : students)
        s.display();
}

void searchStudent() {

    int roll;

    cout << "\nEnter Roll Number: ";
    cin >> roll;

    for (Student s : students) {
        if (s.rollNo == roll) {
            s.display();
            return;
        }
    }

    cout << "\nStudent Not Found.\n";
}

void updateStudent() {

    int roll;

    cout << "\nEnter Roll Number to Update: ";
    cin >> roll;
    cin.ignore();

    for (int i = 0; i < students.size(); i++) {

        if (students[i].rollNo == roll) {

            cout << "\nEnter New Details:\n";

            cout << "Name: ";
            getline(cin, students[i].name);

            cout << "Age: ";
            cin >> students[i].age;
            cin.ignore();

            cout << "Course: ";
            getline(cin, students[i].course);

            cout << "Marks: ";
            cin >> students[i].marks;

            saveToFile();

            cout << "\nRecord Updated Successfully!\n";
            return;
        }
    }

    cout << "\nStudent Not Found.\n";
}

void deleteStudent() {

    int roll;

    cout << "\nEnter Roll Number to Delete: ";
    cin >> roll;

    for (int i = 0; i < students.size(); i++) {

        if (students[i].rollNo == roll) {

            students.erase(students.begin() + i);

            saveToFile();

            cout << "\nRecord Deleted Successfully!\n";
            return;
        }
    }

    cout << "\nStudent Not Found.\n";
}

int main() {

    loadFromFile();

    int choice;

    do {

        cout << "\n====================================";
        cout << "\n STUDENT RECORD MANAGEMENT SYSTEM";
        cout << "\n====================================";
        cout << "\n1. Add Student";
        cout << "\n2. Display Students";
        cout << "\n3. Search Student";
        cout << "\n4. Update Student";
        cout << "\n5. Delete Student";
        cout << "\n6. Exit";
        cout << "\nEnter Choice: ";
        cin >> choice;

        switch (choice) {

        case 1:
            addStudent();
            break;

        case 2:
            displayStudents();
            break;

        case 3:
            searchStudent();
            break;

        case 4:
            updateStudent();
            break;

        case 5:
            deleteStudent();
            break;

        case 6:
            cout << "\nThank You!\n";
            break;

        default:
            cout << "\nInvalid Choice!\n";
        }

    } while (choice != 6);

    return 0;
}
