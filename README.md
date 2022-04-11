#include <iostream>
#include <vector>
#include <string>
#include <sstream>

#include "include/utils.hpp"
#include "include/save.hpp"
#include "include/data.hpp"

#include "include/user.hpp"
#include "include/departemen.hpp"
#include "include/matkul.hpp"
#include "include/dosen.hpp"
#include "include/tendik.hpp"
#include "include/mahasiswa.hpp"
#include "include/frs.hpp"

#define DATAPATH "data/data.bin"
#define USERPATH "data/user.bin"
#define DEPARTEMENPATH "data/departemen.bin"
#define MATKULPATH "data/matkul.bin"
#define DOSENPATH "data/dosen.bin"
#define TENDIKPATH "data/tendik.bin"
#define MAHASISWAPATH "data/mahasiswa.bin"
#define FRSPATH "data/frs.bin"

using namespace std;

database* dat = new database();

enum states{
    MAIN

};

int main ()
{
    states state = MAIN;

    while(1) {
        switch (state)
        {
            case MAIN:
            input::Startmenu(dat);
            break;

            default:
                break;
        }
    }

    return 0;
}

Data myData;
vector<User> recUser;
vector<Departemen> recDepartemen;
vector<Matkul> recMatkul;
vector<Dosen> recDosen;
vector<Tendik> recTendik;
vector<Mahasiswa> recMahasiswa;
vector<FRS> recFrs;

// ==================================================================

void adminPage(User *user);
void adminMenuPage();
void dosenPage(Dosen *dosen);
void mahasiswaPage(Mahasiswa *mahasiswa);

void showUserPage();
void showDepartemenPage();
void showMatkulPage(Departemen *departemen);
void showDosenPage(string deptId = "\0");
void showTendikPage();
void showMahasiswaPage(string deptId = "\0");
void showFRSPage(Mahasiswa *mahasiswa);

void addDosen(string deptId = "\0");
void addTendik();
void addMahasiswa(string deptId = "\0");

void editDosen(Dosen *dosen);
void editTendik(Tendik *tendik);
void editMahasiswa(Mahasiswa *mahasiswa);

void setNilaiMahasiswa(vector<Mahasiswa*> mahasiswas);
void setNilaiFRS(Mahasiswa *mahasiswa);

void insertData(string name, string departemenMhsId, string doswalId, int dd, int mm, int yy, int tahunMasuk);

// ==================================================================

void adminPage(User *user)
{
	char menu;
	while (1)
	{
		Utils::clearScreen();
		cout << "Selamat Datang!, admin!" << endl;
		cout  << endl;
		cout << "Data statistik:" << endl;
		cout << "  1. Jumlah User                  : " << recUser.size() << " User" << endl;
		cout << "  2. Jumlah Departemen            : " << recDepartemen.size() << " Departemen" << endl;
		cout << "  3. Jumlah Dosen                 : " << recDosen.size() << " Dosen" << endl;
		cout << "  4. Jumlah Tenaga Kependidikan   : " << recTendik.size() << " Tendik" << endl;
		cout << "  5. Jumlah Mahasiswa             : " << recMahasiswa.size() << " Mahasiswa" << endl;
		cout  << endl;
		cout << "Menu: " << endl;
		cout << "  1. Tampilkan User" << endl;
		cout << "  2. Tampilkan Departemen" << endl;
		cout << "  3. Tampilkan Dosen" << endl;
		cout << "  4. Tampilkan Tenaga Kependidikan" << endl;
		cout << "  5. Tampilkan Mahasiswa" << endl;
		cout << "  8. MENU ADMIN" << endl;
		cout << "  9. Ganti password" << endl;
		cout << "  0. Log out" << endl;
		cout << "-> Pilihan: ";
		cin >> menu;
		cin.ignore();
		switch (menu)
		{
		case '0':
			return;
		case '1':
			showUserPage();
			break;
		case '2':
			showDepartemenPage();
			break;
		case '3':
			showDosenPage();
			break;
		case '4':
			showTendikPage();
			break;
		case '5':
			showMahasiswaPage();
			break;
		case '8':
			adminMenuPage();
			break;
		case '9':
			{
				Utils::clearScreen();
				string oldPass, newPass, reNewPass;
				cout << "Password lama:\n-> ";
				cin >> oldPass;
				cout << "Password baru:\n-> ";
				cin >> newPass;
				cout << "Ketik ulang password baru:\n-> ";
				cin >> reNewPass;
				cin.ignore();

				if (user->getPassword() == oldPass)
				{
					if (newPass == reNewPass)
					{
						user->changePassword(newPass);
						Save::saveData(&recUser, USERPATH);
						cout << endl << "Password berhasil diubah!" << endl;
						cin.ignore();
						return;
					}
				}
				cout << endl << "Password berbeda!" << endl;
				cin.ignore();
			}
			break;
		default:
			break;
