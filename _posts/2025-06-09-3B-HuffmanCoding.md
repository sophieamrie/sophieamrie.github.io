---
title: "Huffman Coding"
date: 2025-06-09
categories: [algorithm]
tags: [huffmanCoding, transmisiData]
---

**Huffman Coding** adalah algoritma kompresi data yang menggunakan **panjang bit yang bervariasi** untuk merepresentasikan karakter. Karakter yang sering muncul diberikan **kode yang lebih pendek**, sedangkan karakter yang jarang diberikan **kode yang lebih panjang**.

📌 **Tujuan**: Mengurangi ukuran data tanpa kehilangan informasi (lossless compression).

---

## Contoh Kasus

Misal teks: `ABBCCCDDDD`

| Karakter | Frekuensi |
|----------|-----------|
| A        | 1         |
| B        | 2         |
| C        | 3         |
| D        | 4         |

---

## Langkah-Langkah Huffman Coding

1. **Hitung Frekuensi**
   - Tentukan jumlah kemunculan setiap karakter dalam teks.

2. **Bangun Pohon Huffman**
   - Buat simpul (node) untuk setiap karakter.
   - Gabungkan dua simpul dengan frekuensi terendah menjadi satu simpul induk.
   - Ulangi hingga hanya ada satu simpul — akar dari pohon Huffman.

3. **Traversal dan Pemberian Kode**
   - Lakukan *traversal* dari akar:
     - Kiri = `0`
     - Kanan = `1`
   - Simpan hasil sebagai kode Huffman untuk masing-masing karakter.

4. **Kompresi**
   - Gantikan setiap karakter dalam teks asli dengan kode Huffman-nya.

5. **Dekompresi**
   - Gunakan pohon Huffman untuk membaca kembali urutan kode dan mengubahnya ke karakter asli.

---

## Contoh Hasil Kode Huffman

(Tergantung struktur pohon, contoh asumsi: D paling sering, A paling jarang)

| Karakter | Kode Huffman |
|----------|--------------|
| A        | 1100         |
| B        | 110          |
| C        | 10           |
| D        | 0            |

📌 Maka teks `ABBCCCDDDD` akan dikodekan menjadi:


---

## Pseudocode C++ (Penyederhanaan)

```cpp
#include <iostream>
#include <queue>
#include <unordered_map>
using namespace std;

struct Node {
    char ch;
    int freq;
    Node *left, *right;
    Node(char c, int f) : ch(c), freq(f), left(NULL), right(NULL) {}
};

struct Compare {
    bool operator()(Node* a, Node* b) {
        return a->freq > b->freq;
    }
};

void generateCodes(Node* root, string code, unordered_map<char, string>& huffMap) {
    if (!root) return;
    if (!root->left && !root->right)
        huffMap[root->ch] = code;
    generateCodes(root->left, code + "0", huffMap);
    generateCodes(root->right, code + "1", huffMap);
}

unordered_map<char, string> huffmanCoding(string text) {
    unordered_map<char, int> freq;
    for (char c : text) freq[c]++;

    priority_queue<Node*, vector<Node*>, Compare> pq;
    for (auto [c, f] : freq)
        pq.push(new Node(c, f));

    while (pq.size() > 1) {
        Node* l = pq.top(); pq.pop();
        Node* r = pq.top(); pq.pop();
        Node* parent = new Node('\0', l->freq + r->freq);
        parent->left = l;
        parent->right = r;
        pq.push(parent);
    }

    unordered_map<char, string> huffMap;
    generateCodes(pq.top(), "", huffMap);
    return huffMap;
}


