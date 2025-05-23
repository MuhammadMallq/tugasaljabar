# tugasaljabar
import numpy as np

# Fungsi untuk input matriks ukuran n x m dari user
def input_matrix(n=3, m=3, name="Matrix"):
    print(f"Input {name} ({n}x{m}):")
    matrix = []
    for i in range(n):
        while True:
            try:
                # Minta input satu baris, pisahkan dengan spasi lalu konversi ke float
                row = list(map(float, input(f"Row {i+1}: ").split()))
                if len(row) != m:
                    print(f"Please enter exactly {m} numbers separated by spaces.")
                    continue
                matrix.append(row)
                break
            except:
                print("Invalid input, please enter numbers separated by spaces.")
    # Kembalikan matrix dalam bentuk numpy array
    return np.array(matrix)

# Fungsi menjumlahkan dua matriks
def add_matrices(A, B):
    return A + B

# Fungsi mengurangkan dua matriks
def subtract_matrices(A, B):
    return A - B

# Fungsi mengalikan dua matriks menggunakan operator matriks numpy
def multiply_matrices(A, B):
    return A @ B

# Fungsi menghitung determinan matriks menggunakan numpy
def determinant(matrix):
    return round(np.linalg.det(matrix), 2)

# Fungsi menghitung invers matriks jika determinan != 0
def inverse_matrix(matrix):
    if np.linalg.det(matrix) == 0:
        # Jika determinan nol, matriks tidak dapat diinvers
        raise ValueError("Matrix is not invertible (det = 0)")
    # Menghitung invers dan membulatkan 2 desimal
    return np.round(np.linalg.inv(matrix), 2)

# Fungsi menghitung determinan matriks 3x3 secara manual menggunakan aturan Sarrus
def sarrus_rule(matrix):
    if matrix.shape != (3, 3):
        raise ValueError("Sarrus Rule only works for 3x3 matrices.")
    a = matrix
    det = (
        a[0][0]*a[1][1]*a[2][2] +
        a[0][1]*a[1][2]*a[2][0] +
        a[0][2]*a[1][0]*a[2][1] -
        a[0][2]*a[1][1]*a[2][0] -
        a[0][0]*a[1][2]*a[2][1] -
        a[0][1]*a[1][0]*a[2][2]
    )
    return det

# Fungsi menghitung matriks kofaktor 3x3
def cofactor(matrix):
    cof = np.zeros(matrix.shape)  # Matriks kosong untuk kofaktor
    for i in range(3):
        for j in range(3):
            # Minor matrix dengan menghapus baris i dan kolom j
            minor = np.delete(np.delete(matrix, i, axis=0), j, axis=1)
            # Cofaktor = (-1)^(i+j) * determinan minor
            cof[i][j] = ((-1) ** (i + j)) * np.linalg.det(minor)
    # Membulatkan hasil ke 2 desimal
    return np.round(cof, 2)

# Fungsi utama program - menampilkan menu dan menjalankan operasi berdasarkan input user
def main():
    print("\n=== Matrix Operations Menu ===")
    print("1. Penjumlahan Matriks (A + B)")
    print("2. Pengurangan Matriks (A - B)")
    print("3. Perkalian Matriks (A x B)")
    print("4. Determinan Matriks (A)")
    print("5. Invers Matriks (A)")
    print("6. Determinan Matriks dengan Metode Sarrus (A 3x3)")
    print("7. Matriks Kofaktor (A 3x3)")
    print("8. Perkalian Matriks dengan Invers (A x inv(A))")
    print("9. Perkalian Matriks dengan Kofaktor (A x Cofactor(A))")
    print("0. Keluar")

    choice = input("Pilih operasi (0-9): ")
    if choice == "0":
        print("Terima kasih! Program selesai.")
        return

    # Operasi yang memerlukan dua matriks: penjumlahan, pengurangan, perkalian
    if choice in {"1", "2", "3"}:
        A = input_matrix(3, 3, "Matrix A")
        B = input_matrix(3, 3, "Matrix B")

        if choice == "1":
            print("Hasil Penjumlahan A + B:")
            print(add_matrices(A, B))
        elif choice == "2":
            print("Hasil Pengurangan A - B:")
            print(subtract_matrices(A, B))
        elif choice == "3":
            print("Hasil Perkalian A x B:")
            print(multiply_matrices(A, B))

    # Operasi yang hanya butuh satu matriks
    elif choice in {"4", "5", "6", "7", "8", "9"}:
        A = input_matrix(3, 3, "Matrix A")

        try:
            if choice == "4":
                print("Determinan Matriks A:")
                print(determinant(A))

            elif choice == "5":
                print("Invers Matriks A:")
                print(inverse_matrix(A))

            elif choice == "6":
                print("Determinan Matriks A dengan Metode Sarrus:")
                print(sarrus_rule(A))

            elif choice == "7":
                print("Matriks Kofaktor A:")
                print(cofactor(A))

            elif choice == "8":
                invA = inverse_matrix(A)
                print("Perkalian Matriks A dengan Invers A:")
                print(multiply_matrices(A, invA))

            elif choice == "9":
                cofA = cofactor(A)
                print("Perkalian Matriks A dengan Kofaktor A:")
                print(multiply_matrices(A, cofA))

        except ValueError as e:
            # Menangani error misal matriks tidak invertible
            print(f"Error: {e}")

    print("Program selesai.")

# Menjalankan fungsi utama saat script dieksekusi
if __name__ == "__main__":
    main()
