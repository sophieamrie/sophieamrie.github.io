---
title: "Rat In Maze"
date: 2025-06-09
categories: [problem, algorithm]
tags: [problem, algorithm, backtracking, maze, recursion]
---

**Rat in a Maze** adalah algoritma klasik dalam pemrograman rekursif yang memanfaatkan teknik **backtracking** untuk mencari **jalur dari titik awal ke tujuan** dalam sebuah labirin.

---

## 🎯 Tujuan

Mencari jalur dari **titik awal** (biasanya `(0, 0)`) ke **titik tujuan** (biasanya `(N-1, N-1)`) pada sebuah **labirin** berbentuk matriks `N x N`, di mana:
- `1` menandakan jalan yang bisa dilewati
- `0` menandakan dinding atau penghalang

---

## 🧭 Langkah-langkah Algoritma

1. **Mulai dari titik awal `(0, 0)`**
2. **Periksa apakah posisi tersebut valid**:
   - Tidak keluar dari batas matriks
   - Tidak merupakan dinding (`0`)
   - Belum dikunjungi sebelumnya
3. **Tandai posisi tersebut sebagai bagian dari solusi**
4. **Lanjutkan ke arah yang mungkin**:
   - Biasanya: **kanan**, **bawah**, (bisa juga ke **kiri** dan **atas**)
5. **Jika tujuan tercapai, selesaikan**
6. **Jika tidak ada jalur yang valid, lakukan backtrack**:
   - Kembali ke langkah sebelumnya dan coba alternatif jalur

---

## 📦 Representasi Input (Contoh Maze)

```txt
Maze (5x5):

1 0 0 0 0  
1 1 0 1 1  
0 1 1 0 1  
0 0 1 1 1  
0 0 0 0 1

Solusi (tanda 1 menunjukkan jalur tikus):

1 0 0 0 0  
1 1 0 0 0  
0 1 1 0 0  
0 0 1 1 1  
0 0 0 0 1

#define N 5
bool isSafe(int maze[N][N], int x, int y) {
    return (x >= 0 && x < N && y >= 0 && y < N && maze[x][y] == 1);
}

bool solveMazeUtil(int maze[N][N], int x, int y, int sol[N][N]) {
    if (x == N-1 && y == N-1) {
        sol[x][y] = 1;
        return true;
    }

    if (isSafe(maze, x, y)) {
        sol[x][y] = 1;

        if (solveMazeUtil(maze, x+1, y, sol)) return true; // turun
        if (solveMazeUtil(maze, x, y+1, sol)) return true; // kanan

        sol[x][y] = 0; // backtrack
        return false;
    }

    return false;
}

bool solveMaze(int maze[N][N]) {
    int sol[N][N] = { {0} };

    if (!solveMazeUtil(maze, 0, 0, sol)) {
        cout << "Tidak ada solusi\n";
        return false;
    }

    printSolution(sol); // fungsi tambahan untuk mencetak
    return true;
}
