#include <iostream>
using namespace std;

\\ Define structs

struct Student
{
	int MSSV;
	string FullName;
	int Age;
	Student *left;
	Student *right;
};

struct Class
{
	int MaLop;
	string Name;
	Student *st;
};


// Define functions

Student* AddStudent(Student* myst, Student* newst);
Class* CreateClass(int MaLop, string name);
Student* CreateStudent(int MSSV, string FullName, int Age);
Student* DeleteStudent(Student* myst, int MSSV);
Student* SearchStudent(Student *st, int MSSV);
void PrintListStudent(Student* st);


void DeleteClass(Class* myclass, Class* delclass);
Student* MergeClass(Student* myst1, Student* myst2);
Student* MinMSSV(Student* myst);


void DisplayMainMenu();
void DisplayClassMenu(int MaLop, string name);

void FunctionInClass(Class* listClass[], Class* currentClass, int sizeListClass);
void FunctionInMainMenu(Class* listClass[], int sizeListClass);


// Main function

int main()
{
	Class* listClass[100];
	int sizeListClass = 0;

	FunctionInMainMenu(listClass, sizeListClass);

	system("pause");
	return 0;

}


// Class Function

Class* CreateClass(int MaLop, string name)
{
	Class* newClass = new Class();
	newClass->MaLop = MaLop;
	newClass->Name = name;
	return newClass;
}

void DeleteClass(Class* myclass, Class* delclass)
{
	int key = delclass->st->MSSV;
	Student* current = myclass->st;

	while (current->left->MSSV != key && current->right->MSSV != key)
	{
		if (current->MSSV < key)
			current = current->right;
		else
			current = current->left;
	}

	if (current->left->MSSV == key)
		current->left = NULL;
	else if (current->right->MSSV == key)
		current->right = NULL;
	else
		cout << "Not Found DelClass";
}

Student* MergeClass(Student* myst1, Student* myst2)
{

	if (myst1 == NULL)
		myst1 = myst2;
	if (myst2->MSSV < myst1->MSSV)
		myst1->left = MergeClass(myst1->left, myst2);
	else if (myst2->MSSV > myst1->MSSV)
		myst1->right = MergeClass(myst1->right, myst2);
	return myst1;
}


// Student functions

Student* AddStudent(Student* firstSt, Student* newSt)
{
	if (firstSt == NULL)
	{
		firstSt = CreateStudent(newSt->MSSV, newSt->FullName, newSt->Age);
		return firstSt;
	}
	
	if (newSt->MSSV < firstSt->MSSV)
		firstSt->left = AddStudent(firstSt->left, newSt);
	else
		firstSt->right = AddStudent(firstSt->right, newSt);
		
	return firstSt;
}

Student* CreateStudent(int MSSV, string FullName, int Age)
{
	Student* newst = new Student();
	newst->MSSV = MSSV;
	newst->FullName = FullName;
	newst->Age = Age;
	return newst;
}

Student* DeleteStudent(Student* myst, int MSSV)
{
	if (myst == NULL) return myst;

	if (MSSV < myst->MSSV)
		myst->left = DeleteStudent(myst->left, MSSV);

	else if (MSSV > myst->MSSV)
		myst->right = DeleteStudent(myst->right, MSSV);

	else
	{
		if (myst->left == NULL && myst->right == NULL)
		{
			delete myst;
			return NULL;
		}
		else if (myst->left == NULL)
		{
			Student *temp = myst->right;
			delete myst;
			return temp;
		}
		else if (myst->right == NULL)
		{
			Student *temp = myst->left;
			delete myst;
			return temp;
		}

		Student* temp = MinMSSV(myst->right);

		myst->MSSV = temp->MSSV;
		myst->FullName = temp->FullName;
		myst->Age = temp->Age;

		myst->right = DeleteStudent(myst->right, temp->MSSV);
	}
	return myst;
}

void PrintListStudent(Student* st)
{
	if (st != NULL)
	{
		PrintListStudent(st->left);

		cout << "MSSV: " << st->MSSV << " | " << "FullName: " << st->FullName << " | " << "Age: " << st->Age << endl;

		PrintListStudent(st->right);
	}
}

Student* SearchStudent(Student *st, int MSSV)
{
	if (st == NULL || st->MSSV == MSSV)
		return st;

	if (st->MSSV < MSSV)
		return SearchStudent(st->right, MSSV);

	return SearchStudent(st->left, MSSV);

}


// Helper functions

Student* MinMSSV(Student* myst)
{
	Student* current = myst;

	while (current->left != NULL)
		current = current->left;
	return current;
}

void DisplayMainMenu()
{
	cout << "*************************************" << endl;
	cout << "         You are in Main Menu        " << endl;
	cout << "1.Create Class                       " << endl;
	cout << "2.Merge Class                        " << endl;
	cout << "3.PrintListClass                     " << endl;
	cout << "4.Select Class:                      " << endl;
	cout << "5.Back                               " << endl;
	cout << "6.Exit                               " << endl;
	cout << "*************************************" << endl;
}

void DisplayClassMenu(int MaLop, string Name)
{
	cout << "*************************************" << endl;
	cout << "You are in Class: " << MaLop << " " << Name << endl;
	cout << "1.Add Student" << endl;
	cout << "2.Search Student" << endl;
	cout << "3.Delete Student" << endl;
	cout << "4.Delete SubClass" << endl;
	cout << "5.PrintListStudent" << endl;
	cout << "6.Back" << endl;
	cout << "*************************************" << endl;
}

void FunctionInMainMenu(Class* listClass[], int sizeListClass)
{
	DisplayMainMenu();
	int key;

	do
	{
		cin >> key;
		switch (key)
		{
		case 1:
			{
				int MaLop; string name;
				cout << "Enter MaLop: ";
				cin >> MaLop;
				cout << "Enter Name: ";
				cin >> name;
				listClass[sizeListClass] = CreateClass(MaLop, name);
				sizeListClass++;
			}
			break;

		case 2:
			{
				cout << "Enter MaLop1: ";
				int MaLop1;
				cin >> MaLop1;

				cout << "Enter MaLop2: ";
				int MaLop2;
				cin >> MaLop2;

				Class *first = NULL;
				Class *second = NULL;

				for (int i = 0; i < sizeListClass; i++)
				{
					if (listClass[i]->MaLop == MaLop1)
						first = listClass[i];
					if (listClass[i]->MaLop == MaLop2)
						second = listClass[i];
				}
				if (first == NULL)
					cout << "Not Found Class: " << MaLop1 << endl;
				if (second == NULL)
					cout << "Not Found Class: " << MaLop2 << endl;
				if (first != NULL && second != NULL)
				{
					cout << "Enter new MaLop: ";
					int MaLop;
					cin >> MaLop;
					cout << "Enter new Name: ";
					string name;
					cin >> name;
					listClass[sizeListClass] = CreateClass(MaLop, name);

					listClass[sizeListClass]->st = MergeClass(first->st, second->st);
					sizeListClass++;
				}
			}
			break;

		case 3:
			for (int i = 0; i < sizeListClass; i++)
			{
				cout << listClass[i]->MaLop << listClass[i]->Name << endl;
			}
			break;

		case 4:
			{
				cout << "Enter MaLop: ";
				int MaLop;
				cin >> MaLop;

				Class* currentClass = NULL;

				for (int i = 0; i < sizeListClass; i++)
				{
					if (listClass[i]->MaLop == MaLop)
						currentClass = listClass[i];
				}
				if (currentClass == NULL)
					cout << "Not Found";
				else
					FunctionInClass(listClass, currentClass, sizeListClass);
			}
			break;
			

		case 5:
			break;

		default:
			break;
		}
	} while (key != 6);
}

void FunctionInClass(Class* listClass[], Class* currentClass, int sizeListClass)
{
	DisplayClassMenu(currentClass->MaLop, currentClass->Name);
	int key;
	
	do
	{
		cin >> key;
		switch (key)
		{
		case 1:
			{
				int MSSV; string FullName; int Age;
				cout << "Enter FullName: ";
				cin >> FullName;
				cout << "Enter MSSV: ";
				cin >> MSSV;
				cout << "Enter Age: ";
				cin >> Age;

				Student* newSt = CreateStudent(MSSV, FullName, Age);
				currentClass->st = AddStudent(currentClass->st, newSt);
			}
			break;

		case 2:
			{
				cout << "Enter MSSV: ";
				int MSSV;
				cin >> MSSV;
				Student* st = SearchStudent(currentClass->st, MSSV);
				cout << "MSSV: " << st->MSSV << " | " << "FullName: " << st->FullName << " | " << "Age: " << st->Age << endl;
			}
			break;

		case 3:
			{
				cout << "Enter MSSV: ";
				int MSSV;
				cin >> MSSV;
				currentClass->st = DeleteStudent(currentClass->st, MSSV);
			}
			break;

		case 4:
			{
				cout << "Enter MaLop: ";
				int MaLop;
				cin >> MaLop;
				Class* delClass = NULL;
				for (int i = 0; i < sizeListClass; i++)
					if (listClass[i]->MaLop == MaLop)
						delClass = listClass[i];
				DeleteClass(currentClass, delClass);
			}
			break;

		case 5:
			PrintListStudent(currentClass->st);
			break;

		case 6:
			DisplayMainMenu();
			break;
		}
	} while (key != 6);
}


