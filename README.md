Sudoku 
#include <iostream>
#include <vector>

using namespace std;

typedef vector<vector<int>> Board;

bool isValid(const Board& board, int row, int col, int num) {
    // Check row
    for (int i = 0; i < 9; i++) {
        if (board[row][i] == num) {
            return false;
        }
    }

    // Check column
    for (int i = 0; i < 9; i++) {
        if (board[i][col] == num) {
            return false;
        }
    }

    // Check 3x3 subgrid
    int subGridRow = (row / 3) * 3;
    int subGridCol = (col / 3) * 3;
    for (int i = subGridRow; i < subGridRow + 3; i++) {
        for (int j = subGridCol; j < subGridCol + 3; j++) {
            if (board[i][j] == num) {
                return false;
            }
        }
    }

    return true;
}

bool solveSudoku(Board& board) {
    for (int row = 0; row < 9; row++) {
        for (int col = 0; col < 9; col++) {
            if (board[row][col] == 0) {
                for (int num = 1; num <= 9; num++) {
                    if (isValid(board, row, col, num)) {
                        board[row][col] = num;

                        if (solveSudoku(board)) {
                            return true;
                        }

                        board[row][col] = 0;
                    }
                }
                return false;
            }
        }
    }
    return true;
}

void printBoard(const Board& board) {
    for (int row = 0; row < 9; row++) {
        if (row % 3 == 0 && row != 0) {
            cout << "- - - - - - - - - - -" << endl;
        }
        for (int col = 0; col < 9; col++) {
            if (col % 3 == 0 && col != 0) {
                cout << "| ";
            }
            cout << board[row][col] << " ";
        }
        cout << endl;
    }
}

int main() {
    Board board = {
        {5, 3, 0, 0, 7, 0, 0, 0, 0},
        {6, 0, 0, 1, 9, 5, 0, 0, 0},
        {0, 9, 8, 0, 0, 0, 0, 6, 0},
        {8, 0, 0, 0, 6, 0, 0, 0, 3},
        {4, 0, 0, 8, 0, 3, 0, 0, 1},
        {7, 0, 0, 0, 2, 0, 0, 0, 6},
        {0, 6, 0, 0, 0, 0, 2, 8, 0},
        {0, 0, 0, 4, 1, 9, 0, 0, 5},
        {0, 0, 0, 0, 8, 0, 0, 7, 9}
    };

    cout << "Original Board:" << endl;
    printBoard(board);
    cout << endl;

    if (solveSudoku(board)) {
        cout << "Solved Board:" << endl;
        printBoard(board);
    } else {
        cout << "No solution exists." << endl;
    }

    return 0;
}

Login and Registration
#include <iostream>
#include <fstream>
#include <string>

using namespace std;

void registerUser() {
    string username, password;

    cout << "Enter a username: ";
    cin >> username;
    cout << "Enter a password: ";
    cin >> password;

    // Create a file for the user
    ofstream userFile(username + ".txt");
    if (userFile.is_open()) {
        userFile << username << endl;
        userFile << password << endl;
        userFile.close();
        cout << "Registration successful!" << endl;
    } else {
        cout << "Error creating user file." << endl;
    }
}

bool loginUser() {
    string username, password, storedUsername, storedPassword;

    cout << "Enter your username: ";
    cin >> username;
    cout << "Enter your password: ";
    cin >> password;

    // Open the user file
    ifstream userFile(username + ".txt");
    if (userFile.is_open()) {
        getline(userFile, storedUsername);
        getline(userFile, storedPassword);
        userFile.close();

        if (storedUsername == username && storedPassword == password) {
            cout << "Login successful!" << endl;
            return true;
        } else {
            cout << "Invalid username or password." << endl;
            return false;
        }
    } else {
        cout << "User not found." << endl;
        return false;
    }
}

int main() {
    int choice;

    do {
        cout << "Welcome to the Login and Registration System" << endl;
        cout << "1. Register" << endl;
        cout << "2. Login" << endl;
        cout << "3. Exit" << endl;
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                registerUser();
                break;
            case 2:
                if (loginUser()) {
                    cout << "You are now logged in." << endl;
                }
                break;
            case 3:
                cout << "Exiting the system." << endl;
                break;
            default:
                cout << "Invalid choice. Please try again." << endl;
        }
        cout << endl;
    } while (choice != 3);

    return 0;
}

GPA Calculator
#include <iostream>
#include <iomanip>
#include <vector>

using namespace std;

struct Course {
    string name;
    int credits;
    double grade;
};

double calculateGPA(const vector<Course>& courses) {
    double totalGradePoints = 0;
    int totalCredits = 0;

    for (const auto& course : courses) {
        totalGradePoints += course.credits * course.grade;
        totalCredits += course.credits;
    }

    return totalGradePoints / totalCredits;
}

int main() {
    int numCourses;
    cout << "Enter the number of courses: ";
    cin >> numCourses;

    vector<Course> courses(numCourses);

    for (int i = 0; i < numCourses; i++) {
        cout << "Course " << i + 1 << ":" << endl;
        cout << "Name: ";
        cin.ignore();
        getline(cin, courses[i].name);
        cout << "Credits: ";
        cin >> courses[i].credits;
        cout << "Grade: ";
        cin >> courses[i].grade;
        cout << endl;
    }

    double gpa = calculateGPA(courses);

    cout << fixed << setprecision(2);
    cout << "Courses:" << endl;
    for (const auto& course : courses) {
        cout << course.name << " (" << course.credits << " credits): " << course.grade << endl;
    }
    cout << endl;
    cout << "GPA: " << gpa << endl;

    return 0;
}
