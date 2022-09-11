#include <stdio.h>
#include <string.h>
#include <stdlib.h>

typedef struct Student
{
    char firstName[256];
    char lastName[256];
    char major[64];
    char degree[64];
    double gpa;
    int creditHours;
    char ta[4];
    char advisor[256];

} Student;

typedef struct StudentDataset
{
    Student students[100];
    int numberOfStudents;
} StudentDataset;

int isStringPresentInArray(char **degreeNames, int numberOfDegreeNames, char *degreeName)
{
    for (int i = 0; i < numberOfDegreeNames; i++)
    {
        if (strcmp(degreeNames[i], degreeName) == 0)
        {
            return 1;
        }
    }
    return 0;
}

int student_gpa_cmp(const void *p1, const void *p2)
{
    return ((Student *)p1)->gpa < ((Student *)p2)->gpa ? 1 : -1;
}

int student_major_cmp(const void *p1, const void *p2)
{
    return strcmp(((Student *)p1)->major, ((Student *)p2)->major);
}

int degree(StudentDataset studentDataset)
{
    char **degreeNames;
    degreeNames = malloc(studentDataset.numberOfStudents * sizeof(char *));
    int numberOfDegreeNames = 0;
    for (int i = 0; i < studentDataset.numberOfStudents; i++)
    {
        if (!isStringPresentInArray(degreeNames, numberOfDegreeNames, studentDataset.students[i].degree))
        {
            degreeNames[numberOfDegreeNames] = malloc(64 * sizeof(char));
            strcpy(degreeNames[numberOfDegreeNames], studentDataset.students[i].degree);
            numberOfDegreeNames++;
        }
    }
    printf("Number of degrees: %d\n", numberOfDegreeNames);
    for (int i = 0; i < numberOfDegreeNames; i++)
    {
        printf("%s\n", degreeNames[i]);
    }
    return 0;
}

int highest(StudentDataset studentDataset)
{
    qsort(studentDataset.students, studentDataset.numberOfStudents, sizeof(Student), student_gpa_cmp);

    int count = 0;
    double leastGPA = 0;
    while (1)
    {
        if (count > 2 && leastGPA > studentDataset.students[count].gpa)
        {
            break;
        }
        printf("%s %s\n", studentDataset.students[count].firstName, studentDataset.students[count].lastName);
        leastGPA = studentDataset.students[count].gpa;
        count++;
    }
    return 0;
}

int averageCredit(StudentDataset studentDataset)
{
    double sum = 0;
    for (int i = 0; i < studentDataset.numberOfStudents; i++)
    {
        sum += studentDataset.students[i].creditHours;
    }
    printf("Average Credit: %f\n", sum / studentDataset.numberOfStudents);
    return 0;
}

int averageGPA(StudentDataset studentDataset)
{
    double sum = 0;
    int count = 0;
    for (int i = 0; i < studentDataset.numberOfStudents; i++)
    {
        if (strcmp(studentDataset.students[i].major, "Computer Science") == 0)
        {
            sum += studentDataset.students[i].gpa;
            count++;
        }
    }
    printf("Average GPA: %f\n", sum / count);
    return 0;
}

int departments(StudentDataset studentDataset)
{
    qsort(studentDataset.students, studentDataset.numberOfStudents, sizeof(Student), student_major_cmp);
    char currentDept[64];
    strcpy(currentDept, studentDataset.students[0].major);
    int currentDeptCount = 1;
    int depts = 1;
   
    for (int i = 1; i < studentDataset.numberOfStudents; i++)
    {
        if (strcmp(currentDept, studentDataset.students[i].major) == 0)
        {
            currentDeptCount++;
        }
        else
        {
            depts++;
            printf("%s : %d\n", currentDept, currentDeptCount);
            currentDeptCount = 1;
            strcpy(currentDept, studentDataset.students[i].major);
        }
    }
    printf("Total: %d\n", depts);
    return 0;
}

Student parseStudent(char *line)
{
    char *split = strtok(line, ",");
    int column = 0;
    Student s;
    while (split != NULL)
    {
        switch (column)
        {
        case 0:
            strcpy(s.firstName, split);
            break;
        case 1:
            strcpy(s.lastName, split);
            break;
        case 2:
            strcpy(s.major, split);
            break;
        case 3:
            strcpy(s.degree, split);
            break;
        case 4:
            s.gpa = strtod(split, NULL);
            break;
        case 5:
            s.creditHours = atoi(split);
            break;
        case 6:
            strcpy(s.ta, split);
            break;
        case 7:
            strcpy(s.advisor, split);
            break;
        }
        column++;
        split = strtok(NULL, ",");
    }
    return s;
}

int main()
{
    int option;
    StudentDataset studentDataset;
    studentDataset.numberOfStudents = 0;

    FILE *students_dataset = fopen("students_dataset.csv", "r");
    if (!students_dataset)
    {
        printf("FATAL, Can't open file. Exiting...\n");
        return 1;
    }

    int row = 0;

    const size_t line_size = 1024;
    char *line = malloc(line_size);

    while (fgets(line, line_size, students_dataset) != NULL)
    {
        row++;
        if (row == 1)
        {
            continue;
        }
        Student s = parseStudent(line);
        studentDataset.students[studentDataset.numberOfStudents++] = s;
    }

    fclose(students_dataset);
    
    printf("How many different degrees do we have? Display their names. \n");
    degree(studentDataset);
    
    printf("\nWhat is the full name of the 3 students who have the highest GPA? If we have multiple, print them all. \n");
    highest(studentDataset);
    
    printf("\nWhat are the average credit hours of the college? \n");
    averageCredit(studentDataset);
    
    printf("\nWhat is the average GPA of the students whose department is Computer Science? \n");
    averageGPA(studentDataset);
    
    printf("\nDisplay the list of departments along with the total number of advisors.   \n");
    departments(studentDataset);

}
