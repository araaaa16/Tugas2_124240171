#include <iostream>
#include <string>
using namespace std;

struct Buku {
    string judul;
    int harga;
    Buku* prev;
    Buku* next;
};

Buku* head = nullptr;
Buku* tail = nullptr;

bool isEmpty() {
    return head == nullptr;
}

void tambahBuku(string judul, int harga) {
    Buku* baru = new Buku{judul, harga, nullptr, nullptr};
    if (isEmpty()) {
        head = tail = baru;
    } else {
        tail->next = baru;
        baru->prev = tail;
        tail = baru;
    }
    cout << "Buku berhasil ditambahkan.\n";
}

void tampilkanBuku() {
    if (isEmpty()) {
        cout << "Belum ada data buku.\n";
        return;
    }

    for (Buku* i = head; i != nullptr; i = i->next) {
        for (Buku* j = i->next; j != nullptr; j = j->next) {
            if (i->judul > j->judul) {
                swap(i->judul, j->judul);
                swap(i->harga, j->harga);
            }
        }
    }

    cout << "\n=== Data Buku ===\n";
    Buku* bantu = head;
    while (bantu != nullptr) {
        cout << "Judul: " << bantu->judul << " | Harga: " << bantu->harga << endl;
        bantu = bantu->next;
    }
    cout << endl;
}

void cariBuku(string judulCari) {
    Buku* bantu = head;
    while (bantu != nullptr) {
        if (bantu->judul == judulCari) {
            cout << "Buku ditemukan: " << bantu->judul << " | Harga: " << bantu->harga << endl;
            return;
        }
        bantu = bantu->next;
    }
    cout << "Buku dengan judul '" << judulCari << "' tidak ditemukan.\n";
}

void sisipDepan(string judul, int harga) {
    Buku* baru = new Buku{judul, harga, nullptr, nullptr};
    if (isEmpty()) {
        head = tail = baru;
    } else {
        baru->next = head;
        head->prev = baru;
        head = baru;
    }
}

void sisipTengah(string judul, int harga, string judulSetelah) {
    Buku* bantu = head;
    while (bantu != nullptr && bantu->judul != judulSetelah) {
        bantu = bantu->next;
    }

    if (bantu == nullptr) {
        cout << "Judul referensi tidak ditemukan.\n";
        return;
    }

    Buku* baru = new Buku{judul, harga, bantu, bantu->next};
    if (bantu->next != nullptr) {
        bantu->next->prev = baru;
    } else {
        tail = baru;
    }
    bantu->next = baru;
}

void sisipBelakang(string judul, int harga) {
    tambahBuku(judul, harga);
}

void hapusBuku(string judulHapus) {
    Buku* bantu = head;
    while (bantu != nullptr && bantu->judul != judulHapus) {
        bantu = bantu->next;
    }

    if (bantu == nullptr) {
        cout << "Buku tidak ditemukan.\n";
        return;
    }

    if (bantu == head && bantu == tail) {
        head = tail = nullptr;
    } else if (bantu == head) {
        head = head->next;
        head->prev = nullptr;
    } else if (bantu == tail) {
        tail = tail->prev;
        tail->next = nullptr;
    } else {
        bantu->prev->next = bantu->next;
        bantu->next->prev = bantu->prev;
    }

    delete bantu;
    cout << "Buku berhasil dihapus.\n";
}

void menuSisip() {
    int pilih;
    string judul, judulRef;
    int harga;

    cout << "\n== Menu Sisip Buku ==\n";
    cout << "1. Sisip Depan\n";
    cout << "2. Sisip Tengah (Setelah Judul Tertentu)\n";
    cout << "3. Sisip Belakang\n";
    cout << "Pilih: ";
    cin >> pilih;

    cin.ignore(); // flush newline
    cout << "Judul Buku: ";
    getline(cin, judul);
    cout << "Harga Buku: ";
    cin >> harga;

    switch (pilih) {
        case 1:
            sisipDepan(judul, harga);
            break;
        case 2:
            cin.ignore();
            cout << "Disisip setelah judul: ";
            getline(cin, judulRef);
            sisipTengah(judul, harga, judulRef);
            break;
        case 3:
            sisipBelakang(judul, harga);
            break;
        default:
            cout << "Pilihan tidak valid.\n";
    }
}

int main() {
    int pilihan;
    string judul;
    int harga;

    do {
        cout << "\n=== Menu ===\n";
        cout << "1. Tambahkan Buku\n";
        cout << "2. Tampilkan Seluruh Data Buku\n";
        cout << "3. Cari Data Buku (Sequential Search)\n";
        cout << "4. Sisip Buku (Depan, Tengah, Belakang)\n";
        cout << "5. Hapus Buku\n";
        cout << "6. Keluar\n";
        cout << "Pilih Menu: ";
        cin >> pilihan;
        cin.ignore();

        switch (pilihan) {
            case 1:
                cout << "Judul Buku: ";
                getline(cin, judul);
                cout << "Harga Buku: ";
                cin >> harga;
                tambahBuku(judul, harga);
                break;
            case 2:
                tampilkanBuku();
                break;
            case 3:
                cout << "Masukkan Judul Buku yang Dicari: ";
                getline(cin, judul);
                cariBuku(judul);
                break;
            case 4:
                menuSisip();
                break;
            case 5:
                cout << "Masukkan Judul Buku yang Akan Dihapus: ";
                getline(cin, judul);
                hapusBuku(judul);
                break;
            case 6:
                cout << "Terima kasih. Keluar dari program.\n";
                break;
            default:
                cout << "Menu tidak valid.\n";
        }
    } while (pilihan != 6);

    return 0;
}
