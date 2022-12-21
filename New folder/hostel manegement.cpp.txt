#include<iostream>
#include<fstream>
#include<string.h>
#include<time.h>
#include<windows.h>
#include<stdlib.h>
#include<conio.h>
using namespace std;
int count = 0;
void payment_method ();
void display_price(int );
int upi_valid(string);

struct HSTL
{
    int NUMBER = 1;
    int fill = 0;
    char CUS_NAMES[3][10];
    HSTL *NXT;
    HSTL *PREVIOUS;
};
class hostel
{
    HSTL *TITLE[3];
    HSTL *WORDS;

public:
    hostel()
    {
        for (int x = 0; x < 3; x++)
            TITLE[x] = NULL;
    }
    void create()
    {
        for (int x = 0; x < 3; x++)
        {
            for (int y = 0; y < 9; y++)
            {
                HSTL *ss = new HSTL;
                ss->NXT = NULL;
                ss->PREVIOUS = NULL;
                if (TITLE[x] == NULL)
                {
                    TITLE[x] = ss;
                    ss->NUMBER = 1;
                }
                else
                {
                    HSTL *WORDS = TITLE[x];
                    while (WORDS->NXT != NULL)
                    {
                        WORDS = WORDS->NXT;
                    }
                    if (y == 3 || y == 5 || y == 7 || y == 8)
                    {
                        WORDS->NUMBER = 3;
                    }
                    if (y == 2 || y == 4 || y == 6)
                    {
                        WORDS->NUMBER = 2;
                    }
                    WORDS->NXT = ss;
                    ss->PREVIOUS = WORDS;
                }
            }
        }
    }
    void display()
    {
        int y = 0, z = 0, w = 0;
        for (int x = 0; x < 48; x++)
        {
            cout << "--";
        }
        cout << "\n "
        ;
        for (int x = 1; x < 4; x++)
        {
            cout << "| Floor number : "<< x << " \t\t";
        }
        cout << "|\n" ;
        for (int x = 0; x < 48; x++)
        {
            cout << "--";
        }
        WORDS = TITLE[y];
        HSTL *s = TITLE[y + 1];
        HSTL *t = TITLE[y + 2];
        cout << "\n ";
        while (WORDS != NULL)
        {
            if (WORDS->fill != WORDS->NUMBER && WORDS->NUMBER != 0)
            {
                y++;
                cout << "| room no : "<< y;
                cout << "->Vacant cots->"<< WORDS->NUMBER;
            }
            else
            {
                y++;
                cout << "| room no : "<< y;
                cout << "->Present ";
            }
            if (s->fill != s->NUMBER && s->NUMBER != 0)
            {
                z++;
                cout << "\t | room no : "<< y;
                cout << "->Vacant cots->"<< s->NUMBER;
            }
            else
            {
                z++;
                cout << " \t | room no : "<< y;
                cout << "->Present ";
            }
            if (t->fill != t->NUMBER && t->NUMBER != 0)
            {
                w++;
                cout << "\t | room no : "<< y;
                cout << "->Vacant cots->"<< t->NUMBER << "| ";
            }
            else
            {
                w++;
                cout << "\t | room no : "<< y;
                cout << "->Present "<< " | ";
            }
            cout << " \n ";
            for (int x = 0; x < 48; x++)
            {
                cout << "--" ;
            }
            cout << "\n ";
            WORDS = WORDS->NXT;
            s = s->NXT;
            t = t->NXT;
        }
    }
    void booking(int people)
    {
        int fl,rom;
        cout << "\nEnter the floor number : ";
        cin >> fl;
        try
        {
            if (fl < 0 || fl > 4)

            {
                throw(fl);
            }
            WORDS = TITLE[fl - 1];

            cout << "\nEnter the room number : ";
            cin >> rom;
            try
            {

                if (rom < 0 || rom > 10)
                {
                    throw(rom);
                }
                else
                {
                    int x = 1;
                    while (x < rom)
                    {
                        WORDS = WORDS->NXT;
                        x++;
                    }
                    if (WORDS->NUMBER >= people)
                    {
                        cout << "\nroom is vacant you can apply for room" ;

                        int count = 0;
                        while (WORDS->fill - 1 <= WORDS->NUMBER)
                        {
                            int reg_no;
                            cout << "\nEnter name "<< WORDS -> fill + 1 << " : ";
                            cin >> WORDS->CUS_NAMES[WORDS->fill];
                            cout<<"Enter your Registration number:";
                            cin>>reg_no;
                            cout<<"Your room is booked!";
                            count++;
                            WORDS->fill++;
                            if (count >= people)
                            {
                                break;
                            }
                        }
                        WORDS->NUMBER = WORDS->NUMBER - people;
                    }

                    else
                    {
                        cout << "\nroom is not vacant... SORRY !!!";
                    }
                }
            }
            catch (int r)
            {
                cout << "\ninvalid room number : "<< r;
            }
        }
        catch (int r)
        {
            cout << " \ninvalid floor number : " << r;
        }
    }
    void cancelled(int check)
    {
        char checking_namess[10];
        int fg = 0;
        int rms, x = 1;
        try
        {
            if (check < 0 || check > 4)

            {
                throw(check);
            }
            else
            {
                cout << " Enter the room no : ";
                cin >> rms;
                try
                {
                    if (rms < 0 || rms > 10)
                    {
                        throw(rms);
                    }
                    else
                    {
                        cout << " Enter the name to be delete :";

                        cin >> checking_namess;
                        WORDS = TITLE[check - 1];
                        while (x < rms)
                        {
                            WORDS = WORDS->NXT;
                            x++;
                        }
                        x = 0;
                        while (x < 3)
                        {

                            if (!strcmp(checking_namess, WORDS -> CUS_NAMES[x]))

                            {
                                fg = 1;
                                break;
                                x = 0;
                            }
                            else
                                x++;
                        }
                        if (fg == 1 && WORDS->fill != 0)
                        {
                            cout << "\nrecord deleted : "<< WORDS -> CUS_NAMES[x];

                            WORDS->CUS_NAMES[x][0] ='A';
                            WORDS->CUS_NAMES[x][1] ='\0';
                            WORDS->fill--;
                            WORDS->NUMBER++;
                        }
                        else

                            cout << "\nrecord not present ";
                    }
                }
                catch (int r)
                {
                    cout << "\ninvalid room number : " << r;
                }
            }
        }

        catch (int r)

        {
            cout << " \n floor dosn't exist : " << r;
        }
    }
    void modify(int check)
    {
        char checking_namess[10];
        int rms, x = 1;
        try
        {
            if (check < 0 || check > 4)

            {
                throw(check);
            }
            else
            {
                cout << " Enter the room no : ";
                cin >> rms;
                try
                {
                    if (rms < 0 || rms > 10)

                    {
                        throw(rms);
                    }
                    else
                    {
                        cout << "Enter the name to be updated :";

                        cin >> checking_namess;
                        WORDS = TITLE[check - 1];
                        while (x < rms)
                        {
                            WORDS = WORDS->NXT;
                            x++;
                        }
                        x = 0;
                        while (x < 3)
                        {
                            if (!strcmp(checking_namess, WORDS -> CUS_NAMES[x]))

                            {
                                cout << "\nenter updated name : " ;

                                cin >> WORDS->CUS_NAMES[x];
                                break;
                            }
                            else
                                x++;
                        }
                        if (x >= 3)
                            cout << "record not found ";
                        else
                        {
                            cout << "\nrecord updated\nprevious name : "<< checking_namess << "\nupdated name : "<< WORDS->CUS_NAMES[x];
                        }
                    }
                }
                catch (int r)
                {
                    cout << "\ninvalid room number : "<< r;
                }
            }
        }

        catch (int r)

        {
            cout << "\n floor dosn't exist : "<< r;
        }
    }
};
void payment_method ()
{   cout <<"************************************"<<endl;
    cout<< "\t\tPAYMENT STARTS"<<endl;
    cout<<"*************************************"<< endl;
  cout << "Enter your UPI ID" << endl;
  string upi;
  cin >> upi;
  if (!upi_valid (upi))
    {
      int pass;
      cout << "Enter the password- " << endl;
      cin >> pass;
      cout << "PAYMENT IS SUCCESSFULL !!!" << endl;
      cout << "*************" << endl;
    }
  else
      cout << "Not valid UPI ID" << endl;
}
int upi_valid (string upi)
{  // 10 digits@ibl
    int d = 0, i = 0, ext = 0;
    while (upi[i] != '\0')
    {  if (upi[i] >= 0 and upi[i] <= 9)
  d++;
       i++;
       int len = upi.length ();
       if (upi[10] == '@' && upi[11] == 'i' && upi[12] == 'b' && upi[13] == 'l')
    ext = 1;
    }
  if (d == 10 && ext == 1)
    return 1;
  return 0;
}

class Admission
{
private:
    string name, roll_no, course, address, semester, contact_no;
public:
    void show();
    void insert1();
    void modify();
    void search1();
};
void Admission::show()
{   start:
    int op;
    system("cls");
    cout << " ********************************" << endl;
    cout << "\tADMISSIONS " << endl;
    cout << " **********************************" << endl;
    cout << " 1. Enter Student details" << endl;
    cout << " 2. Update Student details" << endl;
    cout << " 3. Search Student details" << endl;
    cout << " 4. Exit" << endl;
     cout << " *******************************" << endl;
    cout << "Enter Your Choose: ";
    cin >> op;
    switch (op) {
    case 1:  insert1();
             break;
    case 2:  modify();
             break;
    case 3:  search1();
             break;
    case 4:  exit(0);
    default:
        cout << " Invalid choice... Please Try Again..";  }
    getch();
    goto start;}

void Admission::insert1() // add student details
{   system("cls");
    fstream file;
    cout <<"***************************************"<<endl;
    cout<< "\t\tSTUDENT DETAILS"<<endl;
    cout<<"***************************************"<< endl;
    cout << "Enter Name: ";
    cin >> name;
    cout << "Enter Roll No.: ";
    cin >> roll_no;
    cout << "Enter Course: ";
    cin >> course;
    cout << "Enter Semester: ";
    cin >> semester;
    cout << "Enter Contact No: ";
    cin >> contact_no;
    cout << "Enter Address: ";
    cin >> address;
    file.open("studentRecord.txt", ios::app | ios::out);
    file <<" " << name << " " << roll_no << " " << course << " " << semester<< " " << contact_no << " " << address << "\n";
    file.close();
}
void Admission::modify()
{   system("cls");
    fstream file, file1;
    string rollno;
    int found = 0;
    cout << "******* Modify Details ********" << endl;
    file.open("studentRecord.txt", ios::in);
    if (!file)
        cout << "\nNo Data is Present..";
    else
    {
        cout << "Enter Roll No. of Student which you want to Modify: ";
        cin >> rollno;
        file1.open("record.txt", ios::app | ios::out);
        file >> name >> roll_no >> course >> semester >> contact_no >> address;
        while (!file.eof())
        {
            if (rollno != roll_no)
                file1 << " " << name << " " << roll_no << " " << course << " " << semester << " " << contact_no << " " << address << "\n";
            else
            {   cout << "Enter Name: ";
                cin >> name;
                cout << "Enter Roll No.: ";
                cin >> roll_no;
                cout << "Enter Course: ";
                cin >> course;
                cout << "Enter Semester: ";
                cin >> semester;
                cout << "Enter Contact No.: ";
                cin >> contact_no;
                cout << "Enter Address: ";
                cin >> address;
                file1 << " " << name << " " << roll_no << " " << course << " " << semester << " " << contact_no << " " << address << "\n";
                found++;  }
                file >> name >> roll_no >> course >> semester>> contact_no >> address;
            if (found == 0)
                cout << "Student Roll No. Not Found....";
        }
        file1.close();
        file.close();
        remove("studentRecord.txt");
        rename("record.txt", "studentRecord.txt");
    }
}
void Admission::search1() // search data of student
{   system("cls");
    fstream file;
    int found = 0;
    file.open("studentRecord.txt", ios::in);
    if (!file) {
        cout <<"********************************************"<<endl;
        cout<< "\t\tStudent Search Data"<<endl;
        cout<<"***********************************************"<< endl;
        cout << "No Data Is Present...";  }
    else{
        string rollno;
        cout <<"************************************************"<<endl;
        cout<< "\t\tSearch Data"<<endl;
        cout<<"**************************************************"<< endl;
        cout <<" Enter Roll No. of Student Which You Want to Search: ";
        cin >> rollno;
        file >> name >> roll_no >> course >> semester >> contact_no >> address;
        while (!file.eof())
        {
            if (rollno == roll_no)
            {
                cout << " Name: " << name << endl;
                cout << " Roll No.: " << roll_no << endl;
                cout << " Course: " << course << endl;
                cout << " Semester: " << semester << endl;
                cout << " Address: " << address << endl;
                found++;
            }
            file >> name >> roll_no >> course >> semester >> contact_no >> address;
        }
        if (found == 0)
            cout << "Student Roll No. Not Found...";
        file.close();
    }
}

class Mess
{
  int total_days = 31;
  int breakfast = 100;
  int lunch = 200;
  int dinner = 150;
  int dbreak = 30;
  int dlunch = 30;
  int ddinner = 30;
public:
  int total_cost ()
  {
    return dinner * get_dinner_mess () + lunch * get_lunch_mess () + breakfast * get_breakfast_mess ();
  }
  void set_breakfast_days (int a)
  {
    dbreak = a;
  }
  int get_breakfast_mess ()
  {
    return dbreak;
  }

  void set_lunch_days (int a)
  {
    dlunch = a;
  }
  int get_lunch_mess ()
  {
    return dlunch;
  }

  void set_dinner_days (int a)
  {
    ddinner = a;
  }
  int get_dinner_mess ()
  {
    return ddinner;
  }

  int dinner_cost ()
  {
    return dinner;
  }
  int lunch_cost ()
  {
    return lunch;
  }
  int breakfast_cost ()
  {
    return breakfast;
  }
  void payment ()
  {
    cout << " \n**   PAYMENT **\n" << endl;
    cout << "Cost of mess for one breakfast is Rs " << breakfast_cost () <<
      "/-" << endl;
    cout << "Cost of mess for one lunch is Rs " << lunch_cost () << "/-" <<
      endl;
    cout << "Cost of mess for one dinner is Rs " << dinner_cost () << "/-" <<
      endl;
    cout << "Your total monthly mess fee is Rs " << total_cost () << "/-" <<
      endl;
    cout << "\n ***\n" << endl;
    cout << "Do you want to pay now?(Y- For Yes and N- For No)\n" << endl;
    char o;
    cin >> o;
    if (o == 'N' or o == 'n')
      cout << "Payment should be made before 15th of following month" << endl;
    else
      payment_method ();
  }
};
int main()
{
    HANDLE h = GetStdHandle (STD_OUTPUT_HANDLE);
    string name,user,password;
    SetConsoleTextAttribute (h, 10);
        cout<<"\t\t\t\t*******************************************"<<endl;
        cout<<"\t\t\t\t SRM UNIVERSITY HOSTEL MANAGEMENT SYSTEM "<<endl;
        cout<<"\t\t\t\t*******************************************"<<endl;
        cout<<"\t\t\t\t\tEnter your Full name:";
        cin>>name;
         string u=name+"_srmap.edu";
         string p=name+"@srm";
        cout<<"\t\t\t\t\tEnter username:";
        cin>>user;
        cout<<"\t\t\t\t\tEnter password:";
        cin>>password;

char ch;
  srand (time (0));
  int Captcha = rand () % 10000;
  SetConsoleTextAttribute (h, 8);
  cout <<"\t\t\t\t\t--------------------"<<endl;
  cout << "\t\t\t\t\t\t" << Captcha << endl;
  cout <<"\t\t\t\t\t--------------------"<<endl;
  int a;
  SetConsoleTextAttribute (h, 12);
  cout <<"\t\t\t\t\tEnter the captcha code- ";
  SetConsoleTextAttribute (h, 7);
  cin >> a;
  system("cls");
    if (a == Captcha && user==u && password==p)
    {
      int op;
      SetConsoleTextAttribute (h, 10);
      cout <<"**********************************************"<<endl;
      cout<< "\t\tWELCOME TO HOSTEL BOOKING SYSTEM"<<endl;
      cout<<"************************************************"<< endl;
      cout << "\t\t\t1. Admission" << endl;
      cout << "\t\t\t2. Room Booking" << endl;
      cout << "\t\t\t3. Mess " << endl;
      cout<< "\t\t\tEnter your option- ";
      cin >> op;
      system ("cls");
      // MESS BOOKING
      if (op == 3)
{
 Mess bill;
     cout <<"**********************************************"<<endl;
     cout << "\tHow  many days did you had your breakfast? " << endl;
     int op;
     cin>>op;
     bill.set_breakfast_days (op); /// Days he/she had mess
     cout << "\tHow many days did you had your Lunch? " << endl;
     int op2;
     cin >> op2;
     bill.set_lunch_days (op2);
     cout << "\tHow many days did you had your Dinner? " << endl;
     int op3;
     cin >> op3;
     bill.set_dinner_days (op3);
     bill.payment ();

}

      // ADMISSION
      else if (op == 1)
{
 Admission project;
 project.show();
}
      //ROOM BOOKING
      else
{
 cout <<"****************************************"<<endl;
    cout<< "\t\tROOM BOOKING"<<endl;
    cout<<"***************************************"<< endl;
 hostel objectives;
 char channels;
 int key_rooms;
 int checking_floors;
 objectives.create();
 do
   {
       cout<<"**************************************"<<endl;
        cout<<"\nMain Menu"<<endl;
        cout<<"**************************************"<<endl;
        cout << "\n1.One sharing \n2.Two sharing\n3.Three sharing\n4.Display the current status of rooms\n5.cancel a booking\n6.modify"<< endl;
        cout << "\nEnter your choice : "   ;
        cin >> key_rooms;
        switch (key_rooms)
        {
        case 1:
        {
            objectives.booking(1);
            break;
        }
        case 2:
        {
            objectives.booking(2);
            break;
        }
        case 3:
        {
            objectives.booking(3);
            break;
        }
        case 4:
        {
            objectives.display();
            break;
        }
        case 5:
        {
            cout << "Enter floor number : ";
            cin >> checking_floors;
            objectives.cancelled(checking_floors);
            break;
        }
        case 6:
        {
            cout << "Enter floor number : ";
            cin >> checking_floors;
            objectives.modify(checking_floors);
            break;
        }

        default:
            cout << "\nInvalid choice ";
        }
        cout << "\nDo you want to continue(Y / N) ";
        cin >> channels;
    } while (channels =='Y'|| channels =='y');
  }
}
else{
    SetConsoleTextAttribute (h, 10);
    cout<<"WRONG DETAILS";
}

}
