# Tuần 2: Mảng & Con Trỏ — Bài tập
#include <iostream>
#include <fstream>

using namespace std;

// ==========================================
// HÀM HỖ TRỢ THAY THẾ STL (Dùng cho Bài 3 & 4)
// ==========================================

// Tính độ dài chuỗi
int strLength(const char* str) {
    int len = 0;
    while (str[len] != '\0') len++;
    return len;
}

// Sao chép chuỗi
void strCopy(char* dest, const char* src) {
    int i = 0;
    while (src[i] != '\0') {
        dest[i] = src[i];
        i++;
    }
    dest[i] = '\0';
}

// So sánh 2 chuỗi giống nhau hoàn toàn
bool strCompare(const char* str1, const char* str2) {
    int i = 0;
    while (str1[i] != '\0' && str2[i] != '\0') {
        if (str1[i] != str2[i]) return false;
        i++;
    }
    return str1[i] == str2[i];
}

// Tìm chuỗi con (Kiểm tra needle có nằm trong haystack không)
bool strContains(const char* haystack, const char* needle) {
    int hLen = strLength(haystack);
    int nLen = strLength(needle);
    if (nLen > hLen) return false;
    for (int i = 0; i <= hLen - nLen; i++) {
        bool found = true;
        for (int j = 0; j < nLen; j++) {
            if (haystack[i + j] != needle[j]) {
                found = false;
                break;
            }
        }
        if (found) return true;
    }
    return false;
}


// ============================================================================
// BÀI 1: MẢNG CƠ BẢN (Min, Max, Trung bình, Tổng)
// ============================================================================
void Bai1_MangCoBan() {
    cout << "\n--- KET QUA BAI 1: MANG CO BAN ---\n";
    int n;
    cout << "Nhap so phan tu n: ";
    cin >> n;
    if (n <= 0) {
        cout << "So phan tu khong hop le.\n";
        return;
    }

    int* arr = new int[n];
    for (int i = 0; i < n; i++) {
        cout << "arr[" << i << "] = ";
        cin >> arr[i];
    }

    int minVal = arr[0];
    int maxVal = arr[0];
    long long tong = 0;

    for (int i = 0; i < n; i++) {
        if (arr[i] < minVal) minVal = arr[i];
        if (arr[i] > maxVal) maxVal = arr[i];
        tong += arr[i];
    }
    double trungBinh = (double)tong / n;

    cout << "-> Min: " << minVal << "\n";
    cout << "-> Max: " << maxVal << "\n";
    cout << "-> Tong: " << tong << "\n";
    cout << "-> Trung binh: " << trungBinh << "\n";

    delete[] arr; // Giải phóng bộ nhớ
}


// ============================================================================
// BÀI 2: MẢNG 2D (Nhân ma trận NxN & Định thức 3x3)
// ============================================================================
void Bai2_Mang2D() {
    cout << "\n--- KET QUA BAI 2: MANG 2D ---\n";
    
    // 1. Nhân 2 ma trận NxN
    int n;
    cout << "Nhap kich thuoc ma tran vuong n x n: ";
    cin >> n;

    // Cấp phát động ma trận 2D
    int** A = new int*[n];
    int** B = new int*[n];
    int** C = new int*[n];
    for (int i = 0; i < n; i++) {
        A[i] = new int[n];
        B[i] = new int[n];
        C[i] = new int[n];
    }

    cout << "Nhap ma tran A:\n";
    for (int i = 0; i < n; i++)
        for (int j = 0; j < n; j++) cin >> A[i][j];

    cout << "Nhap ma tran B:\n";
    for (int i = 0; i < n; i++)
        for (int j = 0; j < n; j++) cin >> B[i][j];

    // Thuật toán nhân ma trận
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            C[i][j] = 0;
            for (int k = 0; k < n; k++) {
                C[i][j] += A[i][k] * B[k][j];
            }
        }
    }

    cout << "\nTich hai ma tran (C = A * B):\n";
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            cout << C[i][j] << "\t";
        }
        cout << "\n";
    }

    // Giải phóng bộ nhớ ma trận động
    for (int i = 0; i < n; i++) {
        delete[] A[i]; delete[] B[i]; delete[] C[i];
    }
    delete[] A; delete[] B; delete[] C;

    // 2. Tính định thức ma trận 3x3
    cout << "\nNhap ma tran 3x3 de tinh dinh thuc:\n";
    int M[3][3];
    for (int i = 0; i < 3; i++)
        for (int j = 0; j < 3; j++) cin >> M[i][j];

    // Công thức định thức Sarrus cho ma trận 3x3
    int det = M[0][0] * (M[1][1] * M[2][2] - M[1][2] * M[2][1])
            - M[0][1] * (M[1][0] * M[2][2] - M[1][2] * M[2][0])
            + M[0][2] * (M[1][0] * M[2][1] - M[1][1] * M[2][0]);

    cout << "-> Dinh thuc (Determinant) 3x3 la: " << det << "\n";
}


// ============================================================================
// BÀI 3: CON TRỎ & CẤP PHÁT ĐỘNG (Tự chế Vector)
// ============================================================================
class MyVector {
private:
    int* data;
    int capacity;
    int size;

    void resize(int newCapacity) {
        int* temp = new int[newCapacity];
        for (int i = 0; i < size; i++) {
            temp[i] = data[i];
        }
        delete[] data;
        data = temp;
        capacity = newCapacity;
    }

public:
    MyVector() {
        capacity = 2;
        size = 0;
        data = new int[capacity];
    }

    ~MyVector() {
        delete[] data;
    }

    void push_back(int value) {
        if (size == capacity) {
            resize(capacity * 2);
        }
        data[size] = value;
        size++;
    }

    void pop_back() {
        if (size > 0) {
            size--;
        }
    }

    int at(int index) {
        if (index >= 0 && index < size) {
            return data[index];
        }
        return -1; 
    }

    int getSize() { return size; }
    int getCapacity() { return capacity; }
};

void Bai3_CapPhatDong() {
    cout << "\n--- KET QUA BAI 3: TU XAY DUNG VECTOR ---\n";
    MyVector v;
    cout << "Push_back cac so: 5, 15, 25, 35...\n";
    v.push_back(5);
    v.push_back(15);
    v.push_back(25);
    v.push_back(35);

    cout << "Size hien tai: " << v.getSize() << " | Capacity hien tai: " << v.getCapacity() << "\n";
    cout << "Cac phan tu trong Vector: ";
    for (int i = 0; i < v.getSize(); i++) cout << v.at(i) << " ";
    
    cout << "\nPop_back (Xoa phan tu cuoi)...\n";
    v.pop_back();
    cout << "Sau khi xoa: ";
    for (int i = 0; i < v.getSize(); i++) cout << v.at(i) << " ";
    cout << "\n";
}


// ============================================================================
// BÀI 4: DỰ ÁN MINI — STUDENT SCORE MANAGER (Mảng động Struct, không STL)
// ============================================================================
struct SinhVien {
    char mssv[20];
    char hoTen[50];
    double diem;
};

class StudentManager {
private:
    SinhVien* ds;
    int size;
    int capacity;

    void resize(int newCapacity) {
        SinhVien* temp = new SinhVien[newCapacity];
        for (int i = 0; i < size; i++) {
            temp[i] = ds[i];
        }
        delete[] ds;
        ds = temp;
        capacity = newCapacity;
    }

public:
    StudentManager() {
        capacity = 5;
        size = 0;
        ds = new SinhVien[capacity];
    }

    ~StudentManager() {
        delete[] ds;
    }

    // 1. Thêm Sinh Viên
    void themSinhVien() {
        if (size == capacity) {
            resize(capacity * 2);
        }
        cout << "Nhap MSSV: ";
        cin.getline(ds[size].mssv, 20);
        cout << "Nhap Ho Ten: ";
        cin.getline(ds[size].hoTen, 50);
        cout << "Nhap Diem: ";
        cin >> ds[size].diem;
        cin.ignore(); // Xử lý bộ đệm ngay sau khi cin số
        size++;
        cout << "=> Them sinh vien thanh cong!\n";
    }

    // 2. Xóa Sinh Viên Theo MSSV
    void xoaSinhVien() {
        char mssvXoa[20];
        cout << "Nhap MSSV can xoa: ";
        cin.getline(mssvXoa, 20);

        int index = -1;
        for (int i = 0; i < size; i++) {
            if (strCompare(ds[i].mssv, mssvXoa)) {
                index = i;
                break;
            }
        }

        if (index == -1) {
            cout << "=> Khong tim thay sinh vien co MSSV vua nhap.\n";
            return;
        }

        // Dịch chuyển mảng đè lên phần tử bị xóa
        for (int i = index; i < size - 1; i++) {
            ds[i] = ds[i + 1];
        }
        size--;
        cout << "=> Xoa sinh vien thanh cong!\n";
    }

    // 3. Tìm kiếm theo Tên hoặc MSSV (Linear Search)
    void timKiem() {
        char tuKhoa[50];
        cout << "Nhap ten hoac MSSV can tim: ";
        cin.getline(tuKhoa, 50);

        bool timThay = false;
        cout << "\n--- KET QUA TIM KIEM ---\n";
        for (int i = 0; i < size; i++) {
            if (strContains(ds[i].hoTen, tuKhoa) || strContains(ds[i].mssv, tuKhoa)) {
                cout << "[" << ds[i].mssv << "] - " << ds[i].hoTen << " - Diem: " << ds[i].diem << "\n";
                timThay = true;
            }
        }
        if (!timThay) cout << "Khong tim thay sinh vien nao.\n";
    }

    // 4. Sắp xếp điểm giảm dần (Bubble Sort) & Hiển thị Xếp Hạng
    void sapXepVaXepHang() {
        if (size == 0) {
            cout << "Danh sach sinh vien dang trong.\n";
            return;
        }

        // Tạo mảng tạm để xếp hạng, không làm thay đổi thứ tự mảng gốc
        SinhVien* tempDS = new SinhVien[size];
        for (int i = 0; i < size; i++) tempDS[i] = ds[i];

        // Bubble Sort
        for (int i = 0; i < size - 1; i++) {
            for (int j = 0; j < size - i - 1; j++) {
                if (tempDS[j].diem < tempDS[j + 1].diem) {
                    SinhVien temp = tempDS[j];
                    tempDS[j] = tempDS[j + 1];
                    tempDS[j + 1] = temp;
                }
            }
        }

        cout << "\n--- BANG XEP HANG LOP (DIEM GIAM DAN) ---\n";
        for (int i = 0; i < size; i++) {
            cout << "Hang " << i + 1 << ": [" << tempDS[i].mssv << "] " << tempDS[i].hoTen << " | Diem: " << tempDS[i].diem << "\n";
        }

        delete[] tempDS;
    }

    // 5. Thống kê & Xuất báo cáo ra File diem_sinhvien.txt
    void xuatBaoCaoFile() {
        if (size == 0) {
            cout << "Danh sach trong, khong execution xuat file.\n";
            return;
        }

        double maxDiem = ds[0].diem;
        double minDiem = ds[0].diem;
        double tongDiem = 0;

        for (int i = 0; i < size; i++) {
            if (ds[i].diem > maxDiem) maxDiem = ds[i].diem;
            if (ds[i].diem < minDiem) minDiem = ds[i].diem;
            tongDiem += ds[i].diem;
        }
        double trungBinh = tongDiem / size;

        ofstream outFile("diem_sinhvien.txt");
        if (!outFile) {
            cout << "=> Loi: Khong the tao hoac mo file để ghi!\n";
            return;
        }

        outFile << "=== BAO CAO QUAN LY DIEM SINH VIEN ===\n\n";
        outFile << "DANH SACH LOP:\n";
        for (int i = 0; i < size; i++) {
            outFile << i + 1 << ". MSSV: " << ds[i].mssv << " | Ten: " << ds[i].hoTen << " | Diem: " << ds[i].diem << "\n";
        }
        outFile << "\n-----------------------------------\n";
        outFile << "THONG KE LOP:\n";
        outFile << "- Diem cao nhat: " << maxDiem << "\n";
        outFile << "- Diem thap nhat: " << minDiem << "\n";
        outFile << "- Diem trung binh: " << trungBinh << "\n";

        outFile.close();
        cout << "=> Da xuat file 'diem_sinhvien.txt' thanh cong!\n";
    }
};

void Bai4_MiniProject_Menu() {
    StudentManager sm;
    int luaChon;

    do {
        cout << "\n=== QUAN LY DIEM SINH VIEN ===\n";
        cout << "1. Them sinh vien\n";
        cout << "2. Xoa sinh vien\n";
        cout << "3. Tim kiem\n";
        cout << "4. Xep hang lop\n";
        cout << "5. Xuat bao cao\n";
        cout << "0. Quay lai menu chinh\n";
        cout << "Nhap lua chon cua ban: ";
        cin >> luaChon;
        cin.ignore(); // Xóa bộ đệm ngay sau khi đọc số lựa chọn để các lệnh getline sau chạy đúng

        switch (luaChon) {
            case 1: sm.themSinhVien(); break;
            case 2: sm.xoaSinhVien(); break;
            case 3: sm.timKiem(); break;
            case 4: sm.sapXepVaXepHang(); break;
            case 5: sm.xuatBaoCaoFile(); break;
            case 0: cout << "Dang thoat khoi mini project...\n"; break;
            default: cout << "Lua chon loi! Chon lai.\n";
        }
    } while (luaChon != 0);
}


// ============================================================================
// HÀM MAIN: ĐIỀU HƯỚNG CHỌN BÀI TẬP ĐỂ CHẠY
// ============================================================================
int main() {
    int baiChon;
    do {
        cout << "\n============================================\n";
        cout << "CHON BAI TAP DE CHAY THU:\n";
        cout << "1. Chay Bai 1 (Mang co ban)\n";
        cout << "2. Chay Bai 2 (Mang 2D - Ma tran)\n";
        cout << "3. Chay Bai 3 (Con tro - Tu xay dung Vector)\n";
        cout << "4. Chay Bai 4 (Mini Project - Quan ly SV)\n";
        cout << "0. Thoat chuong trinh\n";
        cout << "============================================\n";
        cout << "Nhap so bai (0 -> 4): ";
        cin >> baiChon;
        cin.ignore(); // Xóa bộ đệm sau khi nhập số bài chọn

        switch (baiChon) {
            case 1: Bai1_MangCoBan(); break;
            case 2: Bai2_Mang2D(); break;
            case 3: Bai3_CapPhatDong(); break;
            case 4: Bai4_MiniProject_Menu(); break;
            case 0: cout << "Ket thuc chuong trinh. Tam biet!\n"; break;
            default: cout << "So nhap vao khong hop le!\n";
        }
    } while (baiChon != 0);

    return 0;
}
