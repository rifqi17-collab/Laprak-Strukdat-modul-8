# <h1 align="center">Laporan Praktikum Modul 8 - QUEUE </h1>
<p align="center">Muhammad rifqi irwanto - 109082500016</p>

## Dasar Teori
Queue atau antrean adalah struktur data linear yang proses penambahan elemennya dilakukan pada satu ujung, yaitu belakang (tail), sedangkan penghapusan elemennya dilakukan pada ujung lainnya, yaitu depan (head). Karena itu, elemen yang pertama masuk akan menjadi elemen pertama yang dikeluarkan, atau yang dikenal dengan prinsip FIFO (First In First Out) [1]. Pada praktikum ini, queue direpresentasikan menggunakan array dan dipisahkan menjadi tiga bagian, yaitu file header (`queue.h`), file implementasi (`queue.cpp`), dan program utama (`main.cpp`).

### A. Konsep Dasar Queue<br/>
Queue bekerja seperti antrean dalam kehidupan sehari-hari.
#### 1. Pengertian dan Prinsip FIFO
Queue merupakan struktur data yang dapat diumpamakan seperti antrean pada loket pembelian tiket kereta api: orang yang pertama kali masuk antrean akan dilayani lebih dulu, dan yang terakhir masuk akan dilayani paling akhir [1]. Berbeda dengan stack yang hanya memakai satu ujung (LIFO), queue memakai dua ujung, yaitu head untuk pengambilan dan tail untuk penyisipan.
#### 2. Operasi Dasar Queue
Operasi utama pada queue meliputi:
- `enqueue` (insert): menambahkan elemen pada akhir antrean (tail).
- `dequeue` (delete): menghapus elemen pada awal antrean (head) dan mengembalikan nilainya.
- `isEmpty`: memeriksa apakah queue kosong.
- `isFull`: memeriksa apakah queue penuh [1].

Pada praktikum ini, operasi tersebut diwujudkan dalam prosedur `createQueue`, `isEmptyQueue`, `isFullQueue`, `enqueue`, `dequeue`, dan `printInfo`.
#### 3. Kondisi Batas Queue
Queue memiliki dua kondisi batas. Kondisi kosong terjadi ketika tidak ada elemen di dalam antrean, yang pada praktikum ini ditandai dengan `head = -1` dan `tail = -1`. Kondisi penuh terjadi ketika antrean sudah mencapai kapasitas maksimum array. Kedua kondisi ini harus diperiksa sebelum `enqueue` dan `dequeue` dijalankan agar tidak terjadi kesalahan akses data.

### B. ADT dan Modularisasi Program<br/>
Abstract Data Type (ADT) mengelompokkan tipe data beserta operasi-operasinya dalam satu kesatuan. Pada praktikum ini, ADT Queue dipisahkan menjadi tiga file:
- `queue.h`: berisi definisi tipe (`infotype`, `Queue`) dan prototype operasi.
- `queue.cpp`: berisi implementasi seluruh operasi.
- `main.cpp`: program utama yang memakai ADT tersebut.

Pemisahan ini membuat kode lebih rapi dan mudah dipakai ulang. Konsekuensinya, semua file `.cpp` harus dikompilasi bersamaan agar tidak muncul error `undefined reference` saat proses linking.

## Guided 
### 1. queue.h

```C++

#ifndef QUEUE_H_INCLUDE
#define QUEUE_H_INCLUDE

#include <iostream>

#define Nil NULL

typedef int infotype;
typedef bool boolean;
typedef struct elmQ *address;

struct elmQ {
    infotype info;
    address next;
};

struct queue {
    address head;
    address tail;
};

typedef struct queue queue;

boolean isEmpty(queue Q);
void CreateQueue(queue &Q);

address alokasi(infotype X);
void dealokasi(address P);

address findElm(queue Q, infotype X);
boolean fFindElm(queue Q, address P);

void insert(queue &Q, address P);
void deleteQueue(queue &Q, address &P);

void printInfo(queue Q);
int nbList(queue Q);
void delAll(queue &Q);

#endif

```
File queue.h merupakan file deklarasi yang berfungsi untuk mendefinisikan struktur data Queue serta mendeklarasikan fungsi-fungsi yang akan digunakan dalam program.

Pada file ini terdapat struktur elmQ yang menyimpan data berupa integer (info) dan pointer (next) untuk menunjuk ke elemen berikutnya. Selain itu, terdapat struktur queue yang memiliki dua pointer, yaitu head sebagai penunjuk elemen pertama dan tail sebagai penunjuk elemen terakhir.

File ini juga berisi prototype fungsi seperti isEmpty(), CreateQueue(), alokasi(), insert(), deleteQueue(), dan fungsi lainnya yang digunakan untuk mengelola Queue.
### 2. queue.cpp

```C++

#include "queue.h"
#include <iostream>
#include <new>

using namespace std;

boolean isEmpty(queue Q) {
    return (Q.head == Nil);
}

void CreateQueue(queue &Q) {
    Q.head = Nil;
    Q.tail = Nil;
}

address alokasi(infotype X) {
    address P = new (nothrow) elmQ;

    if (P != Nil) {
        P->info = X;
        P->next = Nil;
    }

    return P;
}

void dealokasi(address P) {
    delete P;
}

address findElm(queue Q, infotype X) {
    address P = Q.head;

    while (P != Nil) {
        if (P->info == X) {
            return P;
        }

        P = P->next;
    }

    return Nil;
}

boolean fFindElm(queue Q, address P) {
    address R = Q.head;

    while (R != Nil) {
        if (R == P) {
            return true;
        }

        R = R->next;
    }

    return false;
}

void insert(queue &Q, address P) {
    if (P == Nil) {
        return;
    }

    if (isEmpty(Q)) {
        Q.head = P;
        Q.tail = P;
    } else {
        Q.tail->next = P;
        Q.tail = P;
    }
}

void deleteQueue(queue &Q, address &P) {
    if (isEmpty(Q)) {
        P = Nil;
        return;
    }

    P = Q.head;

    if (Q.head == Q.tail) {
        Q.head = Nil;
        Q.tail = Nil;
    } else {
        Q.head = Q.head->next;
    }

    P->next = Nil;
}

void printInfo(queue Q) {
    if (isEmpty(Q)) {
        cout << "Queue kosong" << endl;
    } else {
        address P = Q.head;

        while (P != Nil) {
            cout << P->info << " ";
            P = P->next;
        }

        cout << endl;
    }
}

int nbList(queue Q) {
    int jumlah = 0;
    address P = Q.head;

    while (P != Nil) {
        jumlah++;
        P = P->next;
    }

    return jumlah;
}

void delAll(queue &Q) {
    address P;

    while (!isEmpty(Q)) {
        deleteQueue(Q, P);
        dealokasi(P);
    }
}

```
File queue.cpp merupakan file implementasi yang berisi isi dari fungsi-fungsi yang telah dideklarasikan pada queue.h.
Pada file ini, setiap fungsi memiliki tugas masing-masing, seperti membuat Queue kosong, mengalokasikan dan membebaskan memori, mencari elemen, menambahkan elemen ke bagian belakang Queue, serta menghapus elemen dari bagian depan Queue.
Selain itu, terdapat fungsi printInfo() untuk menampilkan seluruh isi Queue, nbList() untuk menghitung jumlah elemen, dan delAll() untuk menghapus seluruh elemen yang tersimpan.

### 3. main.cpp

```C++

#include "queue.h"
#include <iostream>

using namespace std;

int main() {
    queue Q;
    address P;

    CreateQueue(Q);

    cout << "===== PROGRAM ADT QUEUE =====" << endl;

    cout << "\n1. Membuat Queue Kosong" << endl;
    if (isEmpty(Q)) {
        cout << "Queue berhasil dibuat dan masih kosong." << endl;
    }

    cout << "\n2. Menambahkan Elemen Queue" << endl;

    P = alokasi(10);
    insert(Q, P);

    P = alokasi(20);
    insert(Q, P);

    P = alokasi(30);
    insert(Q, P);

    cout << "Isi Queue: ";
    printInfo(Q);

    cout << "Jumlah Elemen: " << nbList(Q) << endl;

    cout << "\n3. Mencari Elemen Queue" << endl;

    P = findElm(Q, 20);

    if (P != Nil) {
        cout << "Elemen 20 ditemukan." << endl;
    } else {
        cout << "Elemen 20 tidak ditemukan." << endl;
    }

    cout << "\n4. Mengecek Address Elemen" << endl;

    if (fFindElm(Q, P)) {
        cout << "Address elemen terdapat dalam Queue." << endl;
    } else {
        cout << "Address elemen tidak terdapat dalam Queue." << endl;
    }

    cout << "\n5. Menghapus Elemen Pertama" << endl;

    deleteQueue(Q, P);

    if (P != Nil) {
        cout << "Elemen yang dihapus: " << P->info << endl;
        dealokasi(P);
    }

    cout << "Isi Queue sekarang: ";
    printInfo(Q);

    cout << "Jumlah Elemen: " << nbList(Q) << endl;

    cout << "\n6. Menghapus Semua Elemen Queue" << endl;

    delAll(Q);

    if (isEmpty(Q)) {
        cout << "Semua elemen berhasil dihapus." << endl;
    }

    cout << "Jumlah Elemen: " << nbList(Q) << endl;

    cout << "\n===== PROGRAM SELESAI =====" << endl;

    return 0;
}

```
Program diawali dengan membuat Queue kosong menggunakan CreateQueue(). Selanjutnya, program menambahkan beberapa elemen, menampilkan isi Queue, mencari elemen tertentu, mengecek alamat elemen, dan menghapus elemen pertama.
Pada bagian akhir, program menjalankan delAll() untuk menghapus seluruh elemen yang tersisa. Dengan adanya file ini, seluruh fungsi yang terdapat pada queue.cpp dapat diuji secara langsung.

## Unguided 

### 1. Buatlah ADT Queue menggunakan ARRAY sebagai berikut di dalam file “queue.h”:
### Unqueu.h
```c++
#ifndef QUEUE_H
#define QUEUE_H

#include <iostream>
using namespace std;

typedef int infotype;

const int MAX_QUEUE = 5;

struct Queue {
    infotype info[MAX_QUEUE];
    int head;
    int tail;
};

/* membuat queue kosong, head = -1 dan tail = -1 */
void createQueue(Queue &Q);

/* mengembalikan true jika queue kosong */
bool isEmptyQueue(Queue Q);

/* mengembalikan true jika queue penuh */
bool isFullQueue(Queue Q);

/* menambahkan elemen x pada akhir queue */
void enqueue(Queue &Q, infotype x);

/* mengambil elemen terdepan queue, mengembalikan nilainya */
infotype dequeue(Queue &Q);

/* menampilkan head, tail, dan seluruh isi queue */
void printInfo(Queue Q);

#endif

```

### Unmain.cpp
```C++
#include "Unqueue.h"

int main() {
    cout << "Hello World" << endl;
    Queue Q;
    createQueue(Q);

    cout << "----------------------" << endl;
    cout << " H - T \t | Queue info" << endl;
    cout << "----------------------" << endl;

    printInfo(Q);
    enqueue(Q, 5); printInfo(Q);
    enqueue(Q, 2); printInfo(Q);
    enqueue(Q, 7); printInfo(Q);
    dequeue(Q);    printInfo(Q);
    enqueue(Q, 4); printInfo(Q);
    dequeue(Q);    printInfo(Q);
    dequeue(Q);    printInfo(Q);

    return 0;
}
```
##### Output 1
Unqueue.h` adalah header header file . ADT Queue didefinisikan didefinisikanmenggunakan representasi array untuk bilangan bulat .menggunakan representasi array untuk bilangan bulat . Termasuk di dalamnya adalah perlindungan ` QUEUE_H``QUEUE_H` untuk mencegah pemuatan file berulang kali untuk mencegah file dimuat berulang kali

### 2. Buatlah implementasi ADT Queue pada file “queue.cpp” dengan menerapkan mekanisme queue Alternatif 2 (head bergerak, tail bergerak).
### Unqueue1.cpp
```C++
#include "Unqueue.h"

void createQueue(Queue &Q) {
    Q.head = -1;
    Q.tail = -1;
}

bool isEmptyQueue(Queue Q) {
    return Q.head == -1 && Q.tail == -1;
}

bool isFullQueue(Queue Q) {
    return Q.tail == MAX_QUEUE - 1;
}

void enqueue(Queue &Q, infotype x) {
    if (isFullQueue(Q)) {
        cout << "Queue penuh!" << endl;
        return;
    }
    if (isEmptyQueue(Q)) {
        Q.head = 0;
        Q.tail = 0;
    } else {
        Q.tail++;
    }
    Q.info[Q.tail] = x;
}

infotype dequeue(Queue &Q) {
    if (isEmptyQueue(Q)) {
        cout << "Queue kosong!" << endl;
        return -1;
    }
    infotype x = Q.info[Q.head];
    if (Q.head == Q.tail) {
        createQueue(Q);                 // elemen terakhir, queue jadi kosong
    } else {
        for (int i = Q.head; i < Q.tail; i++) {
            Q.info[i] = Q.info[i + 1];  // geser elemen ke depan
        }
        Q.tail--;
    }
    return x;
}

void printInfo(Queue Q) {
    cout << Q.head << " - " << Q.tail << "\t | ";
    if (isEmptyQueue(Q)) {
        cout << "empty queue";
    } else {
        for (int i = Q.head; i <= Q.tail; i++) {
            cout << Q.info[i] << " ";
        }
    }
    cout << endl;
}
```

### Unqueue2.cpp
```C++
#include "Unqueue.h"

void createQueue(Queue &Q) {
    Q.head = -1;
    Q.tail = -1;
}

bool isEmptyQueue(Queue Q) {
    return Q.head == -1 && Q.tail == -1;
}

/* penuh sungguhan: head di awal dan tail di akhir */
bool isFullQueue(Queue Q) {
    return Q.head == 0 && Q.tail == MAX_QUEUE - 1;
}

void enqueue(Queue &Q, infotype x) {
    if (isFullQueue(Q)) {
        cout << "Queue penuh!" << endl;
        return;
    }
    if (isEmptyQueue(Q)) {
        Q.head = 0;
        Q.tail = 0;
    } else if (Q.tail == MAX_QUEUE - 1) {
        /* penuh semu: geser elemen ke awal array */
        int n = Q.tail - Q.head + 1;
        for (int i = 0; i < n; i++) {
            Q.info[i] = Q.info[Q.head + i];
        }
        Q.head = 0;
        Q.tail = n;
    } else {
        Q.tail++;
    }
    Q.info[Q.tail] = x;
}

infotype dequeue(Queue &Q) {
    if (isEmptyQueue(Q)) {
        cout << "Queue kosong!" << endl;
        return -1;
    }
    infotype x = Q.info[Q.head];
    if (Q.head == Q.tail) {
        createQueue(Q);                 // elemen terakhir, queue jadi kosong
    } else {
        Q.head++;
    }
    return x;
}

void printInfo(Queue Q) {
    cout << Q.head << " - " << Q.tail << "\t | ";
    if (isEmptyQueue(Q)) {
        cout << "empty queue";
    } else {
        for (int i = Q.head; i <= Q.tail; i++) {
            cout << Q.info[i] << " ";
        }
    }
    cout << endl;
}
```

### Unqueue3.cpp
```C++
#include "Unqueue.h"

void createQueue(Queue &Q) {
    Q.head = -1;
    Q.tail = -1;
}

bool isEmptyQueue(Queue Q) {
    return Q.head == -1 && Q.tail == -1;
}

/* penuh jika successor dari tail adalah head */
bool isFullQueue(Queue Q) {
    return !isEmptyQueue(Q) && (Q.tail + 1) % MAX_QUEUE == Q.head;
}

void enqueue(Queue &Q, infotype x) {
    if (isFullQueue(Q)) {
        cout << "Queue penuh!" << endl;
        return;
    }
    if (isEmptyQueue(Q)) {
        Q.head = 0;
        Q.tail = 0;
    } else {
        Q.tail = (Q.tail + 1) % MAX_QUEUE;
    }
    Q.info[Q.tail] = x;
}

infotype dequeue(Queue &Q) {
    if (isEmptyQueue(Q)) {
        cout << "Queue kosong!" << endl;
        return -1;
    }
    infotype x = Q.info[Q.head];
    if (Q.head == Q.tail) {
        createQueue(Q);                 // elemen terakhir, queue jadi kosong
    } else {
        Q.head = (Q.head + 1) % MAX_QUEUE;
    }
    return x;
}

void printInfo(Queue Q) {
    cout << Q.head << " - " << Q.tail << "\t | ";
    if (isEmptyQueue(Q)) {
        cout << "empty queue";
    } else {
        int i = Q.head;
        while (true) {
            cout << Q.info[i] << " ";
            if (i == Q.tail) break;
            i = (i + 1) % MAX_QUEUE;
        }
    }
    cout << endl;
}
```
### Output Unguided 2
<img width="191" height="165" alt="image" src="https://github.com/user-attachments/assets/f0f705a0-2fc2-449a-a1b0-08729d482524" />

### 3. Buatlah implementasi ADT Queue pada file “queue.cpp” dengan menerapkan mekanisme queue Alternatif 3 (head dan tail berputar).
```C++
#include "Unqueue.h"
int main() {
    Queue Q;
    createQueue(Q);

    cout << "----------------------" << endl;
    cout << " H - T \t | Queue info" << endl;
    cout << "----------------------" << endl;

    for (int i = 1; i <= 5; i++) {
        enqueue(Q, i); printInfo(Q);
    }
    enqueue(Q, 99);                 // antrean penuh
    dequeue(Q); printInfo(Q);
    dequeue(Q); printInfo(Q);
    enqueue(Q, 6); printInfo(Q);
    enqueue(Q, 7); printInfo(Q);
    enqueue(Q, 8);                  // penuh lagi
    while (!isEmptyQueue(Q)) {
        dequeue(Q); printInfo(Q);
    }
    dequeue(Q);                     // antrean kosong

    return 0;
}
```
##### Output 3
<img width="236" height="157" alt="Screenshot 2026-11-23 213750" src="https://github.com/user-attachments/assets/ef49fc28-54aa-4996-ab64-043eb4fad1e0" />

## Kesimpulan
Berdasarkan semua praktik ini , dapat disimpulkan bahwa Queue adalah struktur data linier .​​beroperasi sesuai dengan prinsip Masuk Pertama , Keluar Pertama ( FIFO) , yang berartidata pertama yang dimasukkan menjadiData pertama dirilis. Berbeda dengan ketumpukan yang hanya memiliki satu ujung ,tumpukan yang hanya memiliki satu ujung ,
