---
layout: post
title: Number Sort
---

<head>
  <title>Number Sorting</title>
  <style>
    table {
      border-collapse: collapse;
      margin-bottom: 20px;
    }
    table, th, td {
      border: 1px solid black;
      padding: 5px;
    }
  </style>
</head>
<body>
  <h2>Number Sorting</h2>
  <label for="numCount">Enter the number of elements:</label>
  <input type="number" id="numCount">
  <button onclick="generateNumbers()">Generate Numbers</button>
  <br><br>

  <div id="sortingOptions" style="display: none;">
    <label for="algorithm">Select sorting algorithm:</label>
    <select id="algorithm">
      <option value="bubble">Bubble Sort</option>
      <option value="merge">Merge Sort</option>
      <option value="insertion">Insertion Sort</option>
      <option value="selection">Selection Sort</option>
      <option value="shell">Shell Sort</option>
      <option value="bucket">Bucket Sort</option>
    </select>
    <button onclick="sortNumbers()">Sort Numbers</button>
  </div>
  <br><br>
  <div id="sortingAlgorithm"></div>
  <br>
  <div id="sortingTime"></div>
  <br>

  <div id="output" style="display: none;">
    <h3>Original List (Unsorted)</h3>
    <div id="unsortedNumbers"></div>
    <br>
    <h3>Sorted List</h3>
    <div id="sortedNumbers"></div>
    <br>
  </div>

  <script>
    let numbers = [];

    function generateNumbers() {
      const numCount = parseInt(document.getElementById("numCount").value, 10);

      if (isNaN(numCount) || numCount <= 0) {
        alert("Please enter a valid number of elements.");
        return;
      }

      numbers = [];
      for (let i = 0; i < numCount; i++) {
        numbers.push(Math.floor(Math.random() * 10000));
      }

      const unsortedNumbersDiv = document.getElementById("unsortedNumbers");
      unsortedNumbersDiv.textContent = numbers.join(", ");

      document.getElementById("sortingOptions").style.display = "block";
    }

    function sortNumbers() {
      const algorithm = document.getElementById("algorithm").value;
      let sortedNumbers;
      let sortingTime;

      if (algorithm === "bubble") {
        const startTime = new Date();
        sortedNumbers = bubbleSort(numbers);
        sortingTime = new Date() - startTime;
      } else if (algorithm === "merge") {
        const startTime = new Date();
        sortedNumbers = mergeSort(numbers);
        sortingTime = new Date() - startTime;
      } else if (algorithm === "insertion") {
        const startTime = new Date();
        sortedNumbers = insertionSort(numbers);
        sortingTime = new Date() - startTime;
      } else if (algorithm === "selection") {
        const startTime = new Date();
        sortedNumbers = selectionSort(numbers);
        sortingTime = new Date() - startTime;
      } else if (algorithm === "shell") {
        const startTime = new Date();
        sortedNumbers = shellSort(numbers);
        sortingTime = new Date() - startTime;
      } else if (algorithm === "bucket") {
        const startTime = new Date();
        sortedNumbers = bucketSort(numbers);
        sortingTime = new Date() - startTime;
      }

      const sortedNumbersDiv = document.getElementById("sortedNumbers");
      sortedNumbersDiv.textContent = sortedNumbers.join(", ");

      const sortingAlgorithmDiv = document.getElementById("sortingAlgorithm");
      sortingAlgorithmDiv.textContent = "Algorithm used: " + algorithm + " Sort";

      const sortingTimeDiv = document.getElementById("sortingTime");
      sortingTimeDiv.textContent = "Time taken: " + sortingTime + " ms";

      document.getElementById("output").style.display = "block";
    }

    function bubbleSort(arr) {
      // Bubble sort algorithm implementation
      const len = arr.length;
      for (let i = 0; i < len - 1; i++) {
        for (let j = 0; j < len - 1 - i; j++) {
          if (arr[j] > arr[j + 1]) {
            const temp = arr[j];
            arr[j] = arr[j + 1];
            arr[j + 1] = temp;
          }
        }
      }
      return arr;
    }

    function mergeSort(arr) {
  if (arr.length <= 1) {
    return arr;
  }

  const mid = Math.floor(arr.length / 2);
  const left = arr.slice(0, mid);
  const right = arr.slice(mid);

  return merge(mergeSort(left), mergeSort(right));
}

function merge(left, right) {
  let result = [];
  let leftIndex = 0;
  let rightIndex = 0;

  while (leftIndex < left.length && rightIndex < right.length) {
    if (left[leftIndex] < right[rightIndex]) {
      result.push(left[leftIndex]);
      leftIndex++;
    } else {
      result.push(right[rightIndex]);
      rightIndex++;
    }
  }

  return result.concat(left.slice(leftIndex)).concat(right.slice(rightIndex));
}

function insertionSort(arr) {
  const len = arr.length;
  for (let i = 1; i < len; i++) {
    let key = arr[i];
    let j = i - 1;

    while (j >= 0 && arr[j] > key) {
      arr[j + 1] = arr[j];
      j--;
    }

    arr[j + 1] = key;
  }

  return arr;
}

function selectionSort(arr) {
  const len = arr.length;
  for (let i = 0; i < len - 1; i++) {
    let minIndex = i;
    for (let j = i + 1; j < len; j++) {
      if (arr[j] < arr[minIndex]) {
        minIndex = j;
      }
    }

    if (minIndex !== i) {
      const temp = arr[i];
      arr[i] = arr[minIndex];
      arr[minIndex] = temp;
    }
  }

  return arr;
}

function bucketSort(arr, bucketSize = 5) {
  if (arr.length === 0) {
    return arr;
  }

  const minValue = Math.min(...arr);
  const maxValue = Math.max(...arr);

  const bucketCount = Math.floor((maxValue - minValue) / bucketSize) + 1;
  const buckets = new Array(bucketCount);

  for (let i = 0; i < buckets.length; i++) {
    buckets[i] = [];
  }

  for (let i = 0; i < arr.length; i++) {
    const bucketIndex = Math.floor((arr[i] - minValue) / bucketSize);
    buckets[bucketIndex].push(arr[i]);
  }

  arr.length = 0;

  for (let i = 0; i < buckets.length; i++) {
    insertionSort(buckets[i]);
    for (let j = 0; j < buckets[i].length; j++) {
      arr.push(buckets[i][j]);
    }
  }

  return arr;
}

function shellSort(arr) {
  const len = arr.length;
  let gap = Math.floor(len / 2);

  while (gap > 0) {
    for (let i = gap; i < len; i++) {
      let temp = arr[i];
      let j = i;

      while (j >= gap && arr[j - gap] > temp) {
        arr[j] = arr[j - gap];
        j -= gap;
      }

      arr[j] = temp;
    }

    gap = Math.floor(gap / 2);
  }

  return arr;
}

    // Implement mergeSort, insertionSort, and selectionSort functions similarly if needed.

  </script>
</body>