---
title: Counting Sort
author: Tristan Madden
categories: [Java, Processing]
tags: [sorting, counting sort]
date: 2017-07-18
---

```Java
//int [] A = {2, 5, 3, 0, 2, 3, 0, 3};
int [] A = {6, 0, 2, 0, 1, 3, 4, 6, 1, 3, 2};
int []B = new int[A.length];

void setup() {
  countingSort(A);
  noLoop();
}

void countingSort(int arr[]) {

  int k = arr.length;

  // The output character array that will have sorted arr
  int output[] = new int[k];

  // Create a count array to store count of inidividual
  // characters and initialize count array as 0
  int count[] = new int[256];
  for (int i=0; i<256; ++i)
    count[i] = 0;

  // store count of each character
  for (int i=0; i<k; ++i) {
    ++count[arr[i]];
    print(count[i]);
  }
  // Change count[i] so that count[i] now contains actual
  // position of this character in output array
  for (int i=1; i<=255; ++i) {
    count[i] += count[i-1];
  }
  println("");
  // Build the output character array
  for (int i = 0; i<k; ++i)
  {
    output[count[arr[i]]-1] = arr[i];
    print(output[i]);
    --count[arr[i]];
  }
  println("");
  // Copy the output array to arr, so that arr now
  // contains sorted characters
  for (int i = 0; i<k; ++i) {
    arr[i] = output[i];
    print(output[i]);
  }
}
```
