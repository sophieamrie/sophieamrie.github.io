---
title: "Subset Sum Problem"
date: 2025-06-09
categories: [problem]
tags: [problem, subsetSum, knapsackproblem, dp]
---

**Subset Sum Problem (SSP)** adalah masalah klasik dalam teori himpunan dan teori kompleksitas komputasi. Tujuannya adalah **menentukan apakah terdapat subset dari suatu himpunan bilangan bulat yang jumlahnya sama dengan target tertentu**.

---

## 📘 Deskripsi Masalah

**Diberikan:**
- Himpunan bilangan bulat `S = {s₁, s₂, ..., sₙ}`
- Nilai target `T`

**Pertanyaan:**
> Apakah ada subset dari `S` yang jika dijumlahkan akan menghasilkan `T`?

---

## 🔐 Kategori Masalah

- Termasuk dalam **NP-Complete Problem**
- Sulit diselesaikan secara efisien untuk semua kasus
- Solusinya mudah **diverifikasi** jika diketahui

---

## 🧠 Pendekatan Penyelesaian

### 1. Brute Force (Rekursif)
- Coba semua kombinasi subset (dengan/atau tanpa elemen tertentu)
- Kompleksitas Waktu: **O(2ⁿ)**

### 2. Dynamic Programming (Tabulasi)
- Cocok untuk himpunan dan target yang **tidak terlalu besar**
- Buat tabel `dp[i][j]` yang bernilai `true` jika subset dari elemen ke-1 sampai ke-i dapat membentuk jumlah `j`
- Kompleksitas Waktu: **O(n × T)**

---

## 📊 Contoh Tabel DP

Misal:  
`S = {3, 34, 4, 12, 5, 2}`  
`Target T = 9`

| Elemen → / Target ↓ | 0   | 1   | 2   | 3   | 4   | 5   | 6   | 7   | 8   | 9   |
|---------------------|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
| (0 elemen)          | T   | F   | F   | F   | F   | F   | F   | F   | F   | F   |
| 3                   | T   | F   | F   | T   | F   | F   | F   | F   | F   | F   |
| 34                  | T   | F   | F   | T   | F   | F   | F   | F   | F   | F   |
| 4                   | T   | F   | F   | T   | T   | F   | F   | T   | F   | F   |
| 12                  | ... | ... | ... | ... | ... | ... | ... | ... | ... | ... |

---

## ✏️ Pseudocode C++: Dynamic Programming

```cpp
bool isSubsetSum(vector<int>& arr, int T) {
    int n = arr.size();
    vector<vector<bool>> dp(n+1, vector<bool>(T+1, false));

    // Target 0 selalu mungkin dengan subset kosong
    for (int i = 0; i <= n; i++)
        dp[i][0] = true;

    // Mengisi tabel DP
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= T; j++) {
            if (arr[i-1] <= j)
                dp[i][j] = dp[i-1][j] || dp[i-1][j - arr[i-1]];
            else
                dp[i][j] = dp[i-1][j];
        }
    }

    return dp[n][T];
}
